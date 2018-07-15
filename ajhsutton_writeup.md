# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./writeup_images/AverageLineInterpolation.png "Line Interpolation"
[image2]:  ./writeup_images/LeftRightLineClassificaiton.png "Left vs Right Lane"
[image3]:  ./writeup_images/LineSegmentAngleEstimation.png "Line Segment Angles"
[image4]:  ./writeup_images/LineSegments.png "Line Segments"
[image5]:  ./writeup_images/LineSegmentOverlay.png "Line Segment Overlay"
[image6]:  ./writeup_images/NoiseyEdgeMap.png "Edge Map in Shade"

---

### Reflection

### 1. Pipeline. 

The follow steps were implemented for the pipeline.


![alt text][image1]


The pipeline was modified to allow changing image resolution by a specifying relative regio of interest. ROI vertices are expressed as values in [0,1], which are subsequently scaled to fit the image resolution.

### 2. Identify potential shortcomings with your current pipeline

The pipeline is limited due to hard coded values for edge thresholding etc. This limits performance when, large changes in contrast occur on the road surface. This occurrs at two points in the challenge image: then entering the lighter concrete road section (the road surface appears nearly white), and when the shadow of a tree is cast over the road. In this instance, edge detection is foiled by the large number of edges detected by the high-contrast shadow.

![alt text][image6]


### 3. Suggest possible improvements to your pipeline

