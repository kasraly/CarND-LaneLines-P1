#**Finding Lane Lines on the Road** 

##Writeup Report

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)
[grayscale]: ./examples/grayscale.jpg "Grayscale"
[edges]: ./examples/edges.jpg "Detected Edges"
[masked_region]: ./examples/masked_region.jpg "Masked Region"
[lane_broken]: ./examples/lane_broken.jpg "Lines detected using Hough Transform"
[lane_detected]: ./examples/lane_detected.jpg "Fitted left and right lanes"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps.  

1- The firststep was to convert the images to grayscale to make it possible to perfrom edge detection
![alt text][grayscale]

2- I applued blur function to smooth image and minimize the noise for edge detection

3- I used canny edge detection to find the edges in the grayscale image
![alt text][edges]

4- I masked the edge detected image into a trapezoid region of interest, starting from corners and joining around center of image. The masked region is shown in image below. Note that this image is for representation of masked region, in the pipeline masking is applied to edge detection image. and right image is one used for future steps.  
![alt text][masked_region]

5- I applied Hough Transform algorithm on the masked image to find straight lines in the image
![alt text][lane_broken]

6- I separated the lines found into left and right based on their slope. negative slope for right lane divider and positive slope for left.  

7- In order to draw a single line on the left and right lanes, for each set of lines (left and right), a straight line (first order polynomial) is fitted. For this, from each line a set of points were generated proportianal to its length. Then, a line is fitted to the set of points. 
![alt text][lane_detected]


###2. Identify potential shortcomings with your current pipeline

Potential shortcoming are as follows:
- The masked region is currently defined manually. This should be calibrated for each camera and if the perspective changes the masked region will not be effective anymore.  
- The final line fitted is a straight line. If there is a curve in the road the lane markings will not be straight anymore. Therefore, the straight line will not be a good representation of the road lanes. Additionally, checking the slope of the lines in the image will not be a good indicator for left and right lanes.   
- The edge detection algorithm is calibrated for this lighting condition. If the lighting changes (day and night, sunny vs cloudy, shadow of other objects) the edge detection might not work properly. 


###3. Suggest possible improvements to your pipeline

To account for curves in the lanes, it would be better to fit a line with higher order than a first order polynomial (srtaight line).

To have an adaptive masking, we can look at the variation of images from one frame to next. in this way we can differentiate between roadway and other objects as their progress from one frame to the next would be different. 

To resolve the problems from changing lighting condition, it might be possible to normalize the image. It is worth noting that this will not eliminate the negative impact of shadows.
