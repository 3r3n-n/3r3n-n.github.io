---
layout: post
title: "Encora Academy Week #13"
--- 

Hi! This has been the fourth week of the third phase of the Academy: contributing to open source. This week I worked on two issues so I will tell you about them:

# geemap

I began this week by working on my [second contribution](https://github.com/giswqs/geemap/pull/553) to the *geemap* project (python). It addressed the same [issue](https://github.com/giswqs/geemap/issues/339) as the first contribution but it is a different feature (the issue refers to the whole chart module so it is a large set of functions so I am implementing a couple of them). This time I would be implementing a histogram plot. In the beginning I thought that this would be quicker than the first contribution since in this case the data would not need re-arrangements to be plotted as needed: for a histogram you just need the column (basically, an array) with numeric data and basically that's it. Well, I was wrong.

Yes, a histogram can be plotted with just the numeric data column. In `bqplot` you also need to specify the number of bins: this is the number of sub-ranges that the data is split into (think of it as the number of vertical bars). As the chart module for `geemap` aims to provide a similar functionality to the one from the GEE JavaScript API, I was trying to mimic this function behavior (remember that GEE is not open source, but geemap is).

The inputs for histogram in `bqplot` are not exactly the same as the [GEE JS `ui.histogram`](https://developers.google.com/earth-engine/guides/charts_feature#uichartfeaturehistogram). Both of them need to take the features and the property (the column identifier) to be plotted. The difference between them is that the number of `bins` is always required in `bqplot`, whereas in GEE JS you don't have that exact same parameter... but you have `maxBuckets` (the maximum number of bins) and `minBucketWidth` (the minimum width for each column)... and both of them are optional parameters (another relevant detail there is that those two parameters are 'rounded up' to the next closest power of 2). 

So there are cases in GEE JS where you don't provide any hint related to the number of bins so the function provides a 'default' quantity and width for the bins... so I wanted to imitate that default behavior but, since GEE is not open-sourced, I had to do quite some systematic observations and write down the inputs and the respective output in order to figure out a pattern that could be translated into code: for each observation (input), I checked the number of bins and their width (luckily in the GEE online IDE you can download the histogram data as CSV so it made it easier for counting the number of bins, it is just the number of lines in the file minus 1).

![silly_image](https://pbs.twimg.com/media/D6NtHDXWkAAoObT.png)

After looking at my notes from these observations and considering that the number of bins multiplied by the bin width is equal to the data range, I noticed that the proportionality between the data range and the bin size was kind of close to 2<sup>8</sup> so I came up with the following empirical formula:

```python
initial_bin_size = nextPowerOf2(data_range / pow(2,8))
```

I found the `nextPowerOf2()` function online and tested it with some dummy data before integrating it into my code (I put the definition at the beginning of my histogram function). 

In that way, I was able to solve the 'default' case, after that it was a bit easier to write the code for the rest of the cases as they were  particular variations of the default one. 

I also spent quite some time trying to find the way to display the histogram so that the bars midpoints were placed at 'nice' offsets where the interval was evident while looking at the tooltips... in a similar way to the GEE charts that I was trying to mimic. This was the reason for implementing the `start_bins` and `end_bins` variables and appending them to the data array.  

Today my PR for this feature was merged after I added a small example notebook that the owner requested. I am happy. 

# nuclear

I kept contributing to the same JavaScript project as last week. This time I was focusing on an improvement that arised from a user's request to show the current track lyrics inside the mini-player. 

My first approach did not work so I tried a second time and I was able to do it. I realized that I still need to learn a lot regarding the basics of CSS so in this case I studied flex-box, that's what I needed most. 

I also needed to update the props in a component so it rendered the title and artist of the song conditionally since that information was already displayed in the miniplayer (the prop would be false there and true in the original container). 

I had a bit of trouble with the tests again. This time I did not need to write a new test but I needed to pass the test that was already written for the mini player. I got stuck after updating the respective snapshot manually according to the diffs that I was getting in the console in the beginning. At that point I had wrongly discarded updating the snapshot according to the jest documentation. My mentor pointed me back to the right direction and helped me to find the way to do that update. So I learned that there are at least two ways to configure jest: one is in the package.json file and the other is in a separate jest.config file. 

Testing is still a new world for me but I am glad for what I have recently learned about it because before that it was almost a complete black box.

I submitted the PR for this improvement a couple of hours ago so I hope that soon I know if I need to do something else or if it is ready to be merged.
