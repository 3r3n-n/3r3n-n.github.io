---
layout: post
title: "Encora Academy Week #12"
---

These are my insights and experiences from week 3 of contributing to open source.

After having finished the research exercises from the first two weeks, I was ready to focus on my open source contributions. 

# geemap

As I was already behind in my contributions, I decided to prioritize the one that I felt that I could finish sooner so this was the [geemap](https://github.com/giswqs/geemap) project.

My contribution for that project during this week was about implementing the functionality for the group bar chart, this would be the pythonic equivalent to the respective one in the Google Earth Engine API so users would be able to use it in their jupyter notebooks.

The function is like 90 lines long so I will just show the beginning to give an idea of the general functionality and parameters:

```python
def feature_groups(features, xProperty, yProperty, seriesProperty, **kwargs):
    """Generates a Chart from a set of features.
    Plots the value of one property for each feature.
    Reference:
    https://developers.google.com/earth-engine/guides/charts_feature#uichartfeaturegroups

    Args:
        features (ee.FeatureCollection): The feature collection to make a chart from.
        xProperty (str): Features labeled by xProperty.
        yProperty (str): Features labeled by yProperty.
        seriesProperty (str): The property used to label each feature in the legend.

    Raises:
        Exception: Errors when creating the chart.
    """
```

So I took the same sample data as [the example from the GEE JavaScript API documentation](https://developers.google.com/earth-engine/guides/charts_feature#uichartfeaturegroups) and tried to reproduce it in python with the bqplot library, so I was trying to get something like this: 

![https://developers.google.com/earth-engine/images/Charts_feature_12.svg](https://developers.google.com/earth-engine/images/Charts_feature_12.svg)

It looked quite straight-forward at the beginning but the bar colors did not work as I was expecting (I was unconsciously assuming that it was the same as `matplotlib`). The different colors (and their respective labels in the legends) for bars in `bqplot` are related to how many categories exist inside grouped bar charts. I found this by trial and error since there are not many bqplot bar chart examples online (in comparison with, let's say, `matplotlib`, `seaborn` or `plotly`) and I did not find this explicitly in the documentation. At this point, I felt like it would not be possible to achieve the functionality with this library but then I stumbled upon the stacked mode for grouped bar charts and it gave me an idea... if I set all values but one to 0 in a certain category-based-group, then the stacked chart will only paint the non-zero sub-category so this looks like a normal single bar... and I can play with this to trick bqplot into showing what I want. In order to achieve this, I needed to re-arrange the data table and this was the juicy part of the implementation. Fortunately, in the end it did not take as many lines as I thought:

```python
df[yProperty] = pd.to_numeric(df[yProperty])
        unique_series_values = df[seriesProperty].unique().tolist()
        new_column_names = []

        for value in unique_series_values:
            sample_filter = (df[seriesProperty] == value).map({True: 1, False: 0})
            column_name = str(yProperty) + "_" + str(value)
            df[column_name] = df[yProperty] * sample_filter
            new_column_names.append(column_name)
```

I had to do something kind of similar to a table pivot. Being somewhat familiar with pandas (as `pd` in the code snippet) helped me tremendously. 

At the end, I got an identical chart so I was quite happy:

![https://user-images.githubusercontent.com/82424931/123015072-2b958200-d38d-11eb-9318-084b305441a4.png](https://user-images.githubusercontent.com/82424931/123015072-2b958200-d38d-11eb-9318-084b305441a4.png)

I submitted my PR in the evening and the following morning I woke up to the great news that it was merged by the project owner with positive feedback and no extra changes requested. This was so motivating after a couple of weeks of setbacks and some frustration.

# nuclear

So that day I turned to the other project from my primary stack (JavaScript): *nuclear*, a music streamer that pulls music from free sources. The frontend of this project is written in react and has some TypeScript files. This contribution was about implementing playlist data export to a JSON local file. I was happy to be able to write a functional implementation on my own during one day. 

For this, I added the button layout in the `index.tsx` file of the `PlaylistView` component. Then I wrote the `exportPlaylist` function in the `playlist.ts` file in the actions folder and then modified the `PlaylistViewContainer` files so this function would be passed as props to the `PlaylistView` component. Since the respective button and success and error toasts need their labels, I added the `'export-button'`, `'export-fail-title'`, `'export-success-title'`, `'error-save-file'` and `'playlist-exported'` labels (in English) to every translation file in the locales folder. 

The core part of the implementation is, of course, the `exportPlaylist` function:

```jsx
export function exportPlaylist(playlist, t) {
  return async dispatch => {
    const name = playlist.name;
    const dialogResult = await remote.dialog.showSaveDialog({
      defaultPath: name,
      filters: [
        { name: 'file', extensions: ['json'] }
      ],
      properties: ['createDirectory', 'showOverwriteConfirmation']
    });
    const filePath = dialogResult?.filePath?.replace(/\\/g, '/');

    if (filePath) {
      try {
        const data = JSON.stringify(playlist, null, 2);
        fs.writeFile(filePath, data, (err) => {
          if (err) {
            dispatch(error(t('export-fail-title'), t('error-save-file'), null, null));
            return;
          }
          dispatch(success(t('export-success-title'), t('playlist-exported', { name }), null, null));
        });
      } catch (e) {
        dispatch(error(t('export-fail-title'), t('error-save-file'), null, null));
      }
      
    }
  };
}
```

The following day I dedicated myself to write automated tests for this new functionality so I learned about react-testing-library and jest. I needed to use some utilities from this library that helped to simulate user interaction. I got stuck in an error that I was struggling to understand so the following day one of my mentors helped me to solve this.

The test looks like this:

```jsx
it('should export the playlist', async () => {
    const { component, store } = mountComponent();
    await waitFor(() => component.getByTestId('more-button').click());
    await waitFor(() => component.getByText(/export/i).click());
    const state = store.getState();
    // eslint-disable-next-line @typescript-eslint/no-var-requires
    const remote = require('electron').remote;
    // eslint-disable-next-line @typescript-eslint/no-var-requires
    const fs = require('fs');
    const [playlist] = state.playlists.playlists;
    // check if the dialog was open
    expect(remote.dialog.showSaveDialog).toHaveBeenCalledWith({
      defaultPath: playlist.name,
      filters: [
        { name: 'file', extensions: ['json'] }
      ],
      properties: ['createDirectory', 'showOverwriteConfirmation']
    });
    expect(remote.dialog.showSaveDialog).toHaveBeenCalledTimes(1);
    // check if the playlist was properly exported 
    expect(fs.writeFile).toHaveBeenCalledWith(
      'downloaded_playlist',
      JSON.stringify(playlist, null, 2),
      expect.any(Function)
    );
  });
```

As we are simulating the user input and the app response, we need some mock data and events, so it was necessary to add a mock dialog in the `__mocks__ /electron.js` file:

```jsx
showSaveDialog: jest.fn(async () => Promise.resolve({
        canceled: false,
        filePath: 'downloaded_playlist'
      }))
```

 And in this way I was able to submit the PR for this issue on Friday evening.

Today I saw the owner's response to my PR and I was happy that they only requested some small changes to one file so I submitted them and I am just waiting for it to be merged. In the meanwhile I requested to work on another issue in the same project and also started working in my second contribution to the geemap project. So I will be telling you more about them next week ! Hope things keep going as good as they were this week.
