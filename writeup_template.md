# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I
* applied Gaussian Blurring with a kernel value of 5
* applied Canny Edge Detection using low/high thresholds of 75/180
* applied a quadrilateral Region of Interest Mask
* ran the Hough Transform on masked, blurred, and edge-detected image and ddrew the lines on a blank image
* combined the original image with the Hough lines image

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...
* I computed the slope of each line and used that to extrapolate the line's length to span the full length of the lane I wanted to detect
* In order to average out the lines I used the slope to detect the direction of the lines, and then specified a slope for each direction that was in the range I wanted
* Drew vertical lines in cases where the slope could not be calculated
* Filtered out "anomalies" that were the result of noise from the ealier parts of the pipeline by checking the slope and x-coordinates of the extrapolated lines right before they were drawn
** The anomalies were causing lines that were far outside the typical slope to be drawn on parts of the image.  

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![Example Pipeline Output][test_images_output/solidWhiteCurve.jpg]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the lines are extrapolated to points close to the horizon. The slope of the lines being drawn is fixed, so if the slope of the lane lines change when approaching the image's horizon, the extrapolated lines begin to diverge from the actual lane lines. This happens in cases where the lane begins to curve closer to the horizon.

Another shortcoming could be sharp turns in the road. The current algorithm would not detect that the lane lines end when they do due to the simplistic nature of the line drawing extrapolation.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to extrapolate more intelligently. Since the slope of the lane lines is unlikely to be constant for long stretches of road, it would make more sense to extrapolate each line segment to a lesser degree. By doing so we could ensure that lines aren't drawn to the point where they diverge from the lane too far.

Another potential improvement could be to alter the thickness of the lines being drawn with the distance to the horizon. As the lane lines approach the horizon, their width within the image will decrease. It would be nice if we could replicate that effect by decreasing the length of lines as they approach the horizon. This could be done by altering the way lines are drawn to split the line drawing into segments of decreasing width.
 
