# **Finding Lane Lines on the Road** 

## Andrew Sutton, 15 July 2018

### The writeup describes the implementation of an Edge detection and line segmentation pipeline for extraction of lane lines. It was performed as part of the Udacity Self-Driving Car Nanodegree program in 2018.

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

[image7]:  ./writeup_images/Final_image1.png "Test Image 1"

[image8]:  ./writeup_images/Final_image2.png "Test Image 2"

---

### Reflection

### 1. Pipeline. 

The follow steps were implemented for the pipeline. The pipeline was tuned to detect road markings in a specified Region of Interest using Canny edge detection and line extraction with the Hough Transform. In addition, two forms of averaging were implemented.
 - Import image and convert to Greyscale (cvtColor).
 - Smooth Image w a Gaussian filter kernel (GaussianBlur).
 - Canny Edge Detection and Region-of-Interest. The output of this stage is edges detected within the specified Region of Interest on the image. Below shows the detected edges, and the overlay of the edges onto the original image.
  ![alt text][image4]
  ![alt text][image5]
 - Edge Accumulation: Assuming that the lane lines doe not move significantly between frame, the Canny edge image was averaged over multiple frames for video feeds. This averaging will stretch small line segments into longer segments. The averaging was implemented using the weighted_image function, where the older segments were decayed over ~5 frames.
 - Line Segment detection with the Hough Transform. The Hough Transform was tuned to generate line segments. 
 - Segment Angle filter. In order to separate the left and right lane lines, the line segments was filtered by their gradient/angle. Shown below are the angles for lines in a test image, showing two groups with positive (+30°) and negative (-35°) angle.
 ![alt text][image3]
 - Line Segment Average: Since many line segments are detected per lane line, these segments can be averaged for form a better estimate of the lane line (assuming that a straight lane line remains straight in image space). Averaging was performed by calculating the Line Angle, and Line Intercept (pixel value for y = 0). The 'average' for each lane was then calculated by weighted average, using the length of the line segment as weighting. The image below shows the isolation of the left and right line segments.
 ![alt text][image2]
 - Display Lines on Image: The 'Average' for the left and right lane lines were displayed on the original frame image, with the left indicated in green and the right in red. These lines were subsequently cropped for the ROI. 
  ![alt text][image1]

Some examples are shown below.

![alt text][image7]

![alt text][image8]

The pipeline was modified to allow changing image resolution by a specifying relative region of interest. ROI vertices are expressed as values in [0,1], which are subsequently scaled to fit the image resolution. This allowed operation with the challenge video.

### 2. Identify potential shortcomings with your current pipeline
There is a significant frame-to-frame deviation in the lane line estimate.

The pipeline is limited due to hard coded values for edge thresholding etc. This limits performance when, large changes in contrast occur on the road surface. This occurs at two points in the challenge image: then entering the lighter concrete road section (the road surface appears nearly white), and when the shadow of a tree is cast over the road. In this instance, edge detection is foiled by the large number of edges detected by the high-contrast shadow.

![alt text][image6]

In addition, the pipeline currently fails if not lines are detected of either the left or right lane lines, which can occur in the challenge video under some parameters.

### 3. Suggest possible improvements to your pipeline
- Implement multiple-frame averaging of the lane line estimate, by storing the line estimate and performing a multi-frame weighting to reduce noise.
- Implement 'no-edge' handling by reverting to previous line estimates
