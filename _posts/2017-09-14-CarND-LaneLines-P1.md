---
layout: post
title: Finding Lane Lines on the Road
---
## Self-Driving Car NanoDegree Project 1

This project was about finding lane lines on the road. It was implemented in Python 2.7, and it used basic computer vision techniques (e.g., line detectors) to determine the position of the vehicle.

--

### Reflection

My pipeline consisted of 5 steps:

1. __Detect the Edges__ - This will convert the image to grayscale and then blur it so that only _true_ lines will be detected. I had to play with the `low_threshold` and `high_threshold`, so I added that as a function. For example, during the "challenging" problem, I noticed that white pavement was especially difficult to detect.

  <img style="text-align:center" src="https://github.com/amanda-ni/CarND-LaneLines-P1/raw/master/writeup_images/edges.jpg" width="480" alt="Edge Detection" />

2. __Apply a Mask__ - This function will extract out the edges of the mask that we want to apply for our processing. There are optional arguments specifying where in the image you want to focus, but these are based on a horizontal and vertical offset from the center. Since we're using a trapezoid, the variable `Hoffset` is the offset of the two vertices from the middle of the picture. Likewise, the variable `Voffset` is how far lower the top of the trapezoid will be. 

  <img style="text-align:center" src="https://github.com/amanda-ni/CarND-LaneLines-P1/blob/master/writeup_images/mask-info.png" width="480" alt="Mask Information" />

3. __Detect the Lanes__ - This function just runs the Hough transform on the lanes. All the arguments are passed in as optional, but the masked image is a positional argument.

4. __Filter and interpolate lines__ - This is the bulk of the work. For every line in the image, we need to filter out the bad ones. We also need to interpolate the line to the right portions of the image. This is broken up into several substeps:

  * __Derive the Line__ - To figure out whether or not this is a valid line, we have to determine what the line is, itself. This is determined by understanding the slope and the intercept. We find what m and b are by definition.

    `m = (y2 - y1)/(x2 - x1)` 
     and `b = y2 - m * x2`

  * __Filter out bad intercepts__ - I initially, only did this. Then when the "challenging problem" came along, I realized I needed to do more (which will be described in the next step). The bad lines are the ones that don't really intercept the X axis at a reasonable point, the bottom of the image. Therefore, I have the arguments `LL`, `LR`, `RR`, `RL`, which are the left lane's limits and the right lane's limits, respectively. So, the x-intercept must fall in those boundaries. (BTW, I realized that the challenge video had different dimensions, so that's why I had to make these arguments.)

  * __Filter out bad slopes__ - When the challenge problem came around, I was getting all sorts of problems with trees, lines in the roads, and other things. So, that meant I had to filter out bad slopes. Basically, if I'm not looking at a vertical line in 3D (which in 2D has a vanishing point), then I'm going to filter it out. That means, the slope has to fall within certain boundaries, which I just universally made between +/-0.4 to +/-0.9.

  * __Interpolate__ - Technically, I'm already doing that with finding the line params. But, I need to be able to specify the line image, which extends each of these lines to the bottom. So out of point 1 and point 2, I took the one that had the smallest `y` value (the highest point in the image) and then I assigned that to be one point, and the other one where the `y` value is of the size image (the bottom of the image). 
 
5. Draw the lines - This UI is just to finish it off and then write to video.

<img style="text-align:center" src="https://github.com/amanda-ni/CarND-LaneLines-P1/raw/master/test_images_output/solidWhiteCurve.jpg" width="480" alt="Combined Image, Solid White Curve" />

### 2. Potential Shortcomings


One potential shortcoming would be what would happen when I ran the challenge problem, I noticed that when the yellow lane was on white pavement, the algorithm quite often lost it. I had to lower thresholds in order to produce the correct lines. In all, I spent energy equally on both the edge threshold tuning and the line finding. There definitely could be more places where we could improve the research.


### 3. Possible Improvements


One improvement would be to take into consideration the fact that each of the frames are related to the frames that surround it. To take advantage of that, if the lines predicted by the algorithm differ significantly from frame to frame, there's a pretty good chance that one of the lines isn't really a line.

Another improvement would be to merge close lines together. If there are multiple predicted lines for a single lane line, then we can take the average or use a single line to represent it.
