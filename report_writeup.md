# **Finding Lane Lines on the Road** 

### Claus H. Rasmussen
### Udacity
### Self-Driving Car Engineer Nanodegree, oct. 2017
### Project 1, Term 1


**Project: Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


---

### Reflection

### 1. Describe your pipeline. 

My pipeline consist of the following steps:

* The pipeline takes a param 'solid_lines' which determines whether the output is annotated with the lines from the Hough Transform Line Detection or with solid lines running from the bottom of the picture and up to the defined area of interest (AOI)
* Define how much of the lower part of the image to take into account (here is chosen 40 % in the param 'pctHeight'). This defines the top of the AOI.
* Call function 'create_vertices' to get arrays with lines (trapezoids) for the whole image, for the left side and for the right side. I have only used the array covering the whole image, but the left and right ones would be needed if the pipeline should be able to handle curves as in the 'challenge.mp4'.
* Convert the images to grayscale
* Blur the image with a Gaussian kernel (size =5)
* Run Canny Edge Detection (CED) on the image with tresholds between 50 and 150
* Mask the CED images with the vertices for the whole image size
* Run Hough Transform Line Detection (HTLD) with hardcoded params
* If the param 'solid_lines' is True, then the full line segments for the left and the right lane will be calculated via a call to the helper function 'get_lane_lines'. If 'solid_lines' is False, the images will be annotated with the lines from the HTLD helper function 'houh_lines'. This function was altered to also return the lines and not only the image with lines. These lines are used in the 'get_lane_lines' function.
* The image is returned to the caller, e.g. the fl_image() function

I have not modified the 'draw_lines' function, but have createad a new 'get_lane_lines' function instead:
* the slope of each line from the HTLD is calculated and is split up into left side if larger the zero and right side if snmaller than zero.
* division by zero is avoided
* an empty canvas is created
* for each side : y = ax + m, the average slope, a, and the m values is calculated using np.mean().
* if at least three lines was detecded in HTLD, a full length line is calculated and annotated on the empty image
* the image with the annotated lines is returned



### 2. Identify potential shortcomings with your current pipeline

My pipeline has a shortcoming when being used with images with curved lanes. This becomes obvious when it is used with the Challenge.mp4 movie.

Another shortcoming is the lack of an avilable debug functionality, which haven't been applied in the code.

Instead of creating stright lines, the HTLD lines could be used to create 'smoother' polylines with extra vertices.



### 3. Suggest possible improvements to your pipeline

A possible improvement would be to log state throughout the pipeline. This will make it a lot easier to debug the code.

Another potential improvement could be to save used params and results, i.g. lines from HTLD for each frame processed. This could be done by declaring a class 'params_and_results', that would contain all params used in the code. One class method would be to persist these data for each frame. In real life this is a must have, so that the state of the lane line function could be examinated, if an accident had happend to the vehicle.

The use of hardcoded values is not acceptable in real life. The code must be rewritten to be abvle to handle params from outside.

Color separation of white and yellow pixels may improve the output from the HTLD analysis.