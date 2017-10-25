**Finding Lane Lines on the Road**

The goals / steps of this project were the following:
* Make a pipeline that finds lane lines on the road
* Reflect on my work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps as described below:

1. Convert the image to Grayscale
2. Apply Gaussian Blur to the image to reduce noise. (This will be helpful in edge detection.)
3. Apply Canny edge detection algorithm to identify edges in the image.
4. Determine the region of interest by finding 4 vertices of a ploygon (a trapazoid in this case) for a major outlier rejection.
5. Apply Hough Transform on the masked image to identify the potential candidates for lane lines.
6. Draw lines, using the points returned by the Hough transform result, on the original image.
7. Apply this pipeline of Steps 1-6 to every frame of a video file.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function. Following are the steps applied in the `my_draw_line()` function:
1. Seperate the points based on the slope of the lines. 
	- If the slope is negative, the points are candidates of the right lane, otherwise, left lane.
	- Filter out the points which have zero or infinite slope, if any because lanes are neither horizontal nor vertical.
2. Store the slope and length of the lines calculated for each of the points.
3. Use the information of the line length, and average out the slope for the 4 longest segments to reduce the noise.
4. In the same manner, calculate the average intercept based on the slope and the available points.
5. Anchor the Y-coordinate of bottom points for left and right lanes to the last index of image height.
6. Anchor the Y-coordinate of top points for the left and right lanes by taking the minimum value from both the points.
7. Use the line equation `y = m*x + c` to calculate the corresponding X-coordinates of left and right lanes.
8. In case of video file, if there are no points after the filtering and bucketing from the first step, use the points from the previous frame as the lane candidaates to successfully have the lane information in every frame.



### 2. Identify potential shortcomings with your current pipeline

While implementing this peoject, I encountered quite a few shortcomings ...

* There is an extensive amount of trial and error involved in choosing the parameters for the 7 steps pipeline.
* Due to the hard coded parameters like the ROI points for mask, the pipeline will easily fail on the videos of different dimensions.
* This pipeline is not very robust to lighting variations. If the lanes are very dull, it is very likely that they will go unnoticed by the Canny edge detector.
* Sometimes, even in the simpler videos, the lane lines are way off for a frameor two. This may cause some confusion if it is deployed on car, creating system failure (maybe).


### 3. Suggest possible improvements to your pipeline

A few possible improvements would be ...

* Use the information from the previous frame to have a more robust lane lines. This can get rid of the last shortcoming as described above.
* Try a different colorspace to reduce the variation due to changing illumination.
* Probably, use a Machine Learning based algorithm so that there is no need to hard code any parameters. This would be considered a fully-autonomous pipeline.

And many more which I may have missed, but can be figured out by discussing with others...
