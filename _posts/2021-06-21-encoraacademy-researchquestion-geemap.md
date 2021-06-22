---
layout: post
title: "How does geemap convert Earth Engine's Feature Collections to pandas DataFrames ?"
---
Google Earth engine (GEE) is a platform for geospatial cloud-computation with tons of satellite-image data. 

The two main ways for using GEE are the JavaScript-based API code editor (a compact online IDE) and the Python-based API, where you need to set up your local Python environment and install the GEE library. 

As the approach to geospatial analysis intersects with data science somehow, the need for incorporating common data science tools arises. 

Pandas is a popular Python library for data wrangling. A short way to describe it is anything that has to do with handling tables. 

geemap is a Python library for interactive mapping through the GEE Python API, Leaflet and Jupyter notebooks. 

GEE is not opensourced but `pandas` and `geemap` are.

Even though GEE is focused on raster (image) files, it also handles vector files since they are necessary for at least delimiting regions of interest. 

Let's remember that geospatial data is just data with a spatial component. This can help us understand how geo-vector data files are structured. They are basically just tables (data) with a geometry-dedicated column (geospatial component: geometries contain the coordinates of the related points). 

IMAGEFILE

The way that Earth Engine handles vector data is in objects called `Feature`, one feature would be equivalent to a table row. Multiple features that have the same properties can be handled in a `FeatureCollection`, we can think of it as a whole table (each property would be a column, the geometry would be one of them).

Let's think that there is a feature collection that we are interested in, but only the properties, this would be the table-like data without the geometry column. If we open a Jupyter notebook and, after importing and initializing GEE, we define the `FeatureCollection`:

```python
countries = ee.FeatureCollection('FAO/GAUL/2015/level0')
```

We want to see what it contains so we do `print(countries)` and get the following:

```jsx
ee.FeatureCollection({
  "functionInvocationValue": {
    "functionName": "Collection.loadTable",
    "arguments": {
      "tableId": {
        "constantValue": "FAO/GAUL/2015/level0"
      }
    }
  }
})
```

We are not getting any actual data, just metadata. This is because this is stored on the server-side and, as the client, we only have the reference to that data (what we see printed). If we would like to retrieve the actual data in a table-like format, this is when geemap comes handy since it provides a function to convert Earth Engine objects to pandas DataFrames (tables): `ee_to_pandas()`.

This function is stored in the [common.py](http://common.py) file in the geemap project repository. This file contains a ton of utilities, all the data-conversion-related functions live here. 

The ee_to_pandas function takes an Earth Engine object as input. The first line of this function imports the pandas library with the pd abbreviation. After that, it validates the input as a FeatureCollection with isinstance .

Then comes the fun part: 

```python
data = ee_object.map(lambda f: ee.Feature(None, f.toDictionary()))
```

Here, the function iterates through the `FeatureCollection` so the `lambda` function is applied to each item (`Feature` in this case). In order to better understand the `lambda` function, we check `ee.Feature()` in the GEE API documentation: it takes a `geometry` and a dictionary with properties as inputs, it makes sense since it matches the spatial vector data definition (geolocated column + non-geolocated columns). We also have the `toDictionary()` method: if the parameter is null (like in this case), it returns the feature's properties in a dictionary. With all this in mind, we see that the `lambda` function is making a new `Feature` that has no (`None`) geometry but keeps the properties of the original one because, as we said  at the beginning, we are only interested in the non-geolocated tabular data. 

Ok, so we got rid of the geolocated data... so, is our data ready to be displayed as a `pandas` `DataFrame` (table)? If we do `print(data)` (I won't put the whole output here since it is too long but trust me!), it is quite similar to what we got earlier when printing countries. It means that our data is still on the server side. So let's look at the next line in the function:

```python
data = [x["properties"] for x in data.getInfo()["features"]]
```

This is list-comprehension syntax, it is iterating through our data variable so each item is a feature. We also need more information about the `getInfo()` method: according to the GEE API doc, it returns a description of the respective feature in *GeoJSON* format via an AJAX call... so it sounds like this is the step when we finally retrieve our data. Let's see if it is true, what we get from just doing `data.getInfo()` is:

```jsx
{'type': 'FeatureCollection',
 'columns': {},
 'features': [{'type': 'Feature',
   'geometry': None,
   'id': '00000000000000000058',
   'properties': {'ADM0_CODE': 2647,
    'ADM0_NAME': 'Montenegro',
    'DISP_AREA': 'NO',
    'EXP0_YEAR': 3000,
    'STATUS': 'Member State',
    'STR0_YEAR': 2006,
    'Shape_Area': 1.51563628739,
    'Shape_Leng': 9.45391830472}},
  {'type': 'Feature', ...
```

I did not paste the whole thing, it is just the beginning so we get an idea. We can see the value for each property in each feature so now we actually have the data that we wanted. Our list-comprehension line in the function is telling us to only take `'features'` from the retrieved object/dictionary. We can see that the associated value is a list of features, it makes sense that we will be iterating on it for our list comprehension: each element `x` is a feature in our list and we will be grabbing the value of the `'properties'` key. 

If we print the resulting list from this line, we get something like:

```python
[{'ADM0_CODE': 2647,
  'ADM0_NAME': 'Montenegro',
  'DISP_AREA': 'NO',
  'EXP0_YEAR': 3000,
  'STATUS': 'Member State',
  'STR0_YEAR': 2006,
  'Shape_Area': 1.51563628739,
  'Shape_Leng': 9.45391830472},
 {'ADM0_CODE': 2648,
  'ADM0_NAME': 'Serbia',
  'DISP_AREA': 'NO',
  'EXP0_YEAR': 3000,
  'STATUS': 'Member State',
  'STR0_YEAR': 2006,
  'Shape_Area': 9.93473107752,
  'Shape_Leng': 24.9093262931}, ... ]
```

Now this looks more `pandas`-friendly. For instance, the following line in the function defines the `DataFrame` with this data:

```python
df = pd.DataFrame(data)
```

And this is actually what `ee_to_pandas` function returns so we are happy now:

[Untitled](https://www.notion.so/db9fd7b629604224b02d79e7d5405acd)

As you can see, in order to answer our original question it was necessary not only to dive into the geemap codebase but also to check the GEE API documentation (since GEE is not OS) to complete our understanding of the process.
