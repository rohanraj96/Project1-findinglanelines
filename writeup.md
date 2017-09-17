# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on my work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. My pipeline and the draw_lines() function.

My pipeline consisted of 5 steps.

1. I converted the images to grayscale and added gaussian blur with kernel size 7x7.
2. Applied Canny edge detection using the tutorial thresholds.
3. Applied a mask to get RoI using the helper function. I did not hard code the vertices for the RoI mask, instead I tried to make my solution more general so that it works on different resolutions.
4. Applied Hough transform to draw hough lines on this modified(masked) image with canny edge detected lines. Lines were extrapolated within this function by calling another function: draw_lines().
5. Super-imposed the original image with this modified image with extrapolated hough lines to get the final result.


In order to draw a single line on the left and right lanes, I modified the draw_line function()_.
My first approach was to get the slopes of the left and right lanes seperately. I did this using the fact that the left lane would have a positive slope and the right lane would have a negative slope. After this, I singled out the highest and lowest points of each lane that were also lying on a hough line and then connected these 2 points. This approach, as I painfully realised, was wrong as I was not getting full lane lines as expected. My next approach was to define a function get_points()_ which would return the end points of the lanes within the RoI. I did this using the slope-intercept form of a line. Once I had these points, I connected them to get my final result.


### 2. Potential shortcomings


One potential shortcoming would be what would happen when the RoI isn't always inside the region specified by the vertices. Suppose the vehicle encounters a speed bump, then the camera's orientation would change and the lanes would no longer lie within the RoI. 

Another would be the case when the lanes are straight and curved during a drive. The same set of hyperparameters won't work for both cases and would hence be a problem.

### 3. Possible improvements

A possible improvement for the challenge video would be to change my hyperparameters, more concretely my theta values to account for the curvature of the road. I think that would produce more visually appealing results.
