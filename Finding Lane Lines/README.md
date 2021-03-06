# **Finding Lane Lines on the Road** 

<img src="../img/finding-lane-lines.jpg" width="480" alt="Combined Image" />

---
Overview
---

When we drive, we use our eyes to decide where to go. The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

The project objective is to detect lane lines in images using Python and OpenCV. 

Here are the [notebook](http://nbviewer.jupyter.org/gist/tranlyvu/df59fa9ea4a18f373947ca5c04bec801) and [source code](https://github.com/tranlyvu/autonomous-vehicle-projects/blob/master/Finding%20Lane%20Lines/src/finding_lane_lines.py) of this project. Also, I have tested the project on 2 videos with [solid white lane lines](https://youtu.be/8O3j7ffj7MA) and [solid yellow white lane lines](https://youtu.be/vHA4nryV8QI).

---
Project writeup 
---

### Pipeline

My pipeline consisted of 6 steps

1. Firstly , I grayscaled the image with cv2.

2. Next, I applied gaussian blur before applyin Canny Edge Detection, this is to further smooth the image in hope of better result, the kernel size was 3 without any tuning.

3. Next, I applied Canny Edge Detection - one of main technique from the course to the image; I spent some time to find the min and max threshold which reflected the best result from my observation. The final values were 80 and 240 which corresponded to recommended ratio 1:3.

4. The next step was to define the region on interest bounded with lane lines. I used 4-sided polygon to mask the region. the bottom 2 points were simply both ends of the image frame, while the other top two points were found using observation with a few trial and errors.

5. The next step was to do Hough Transformation, not much parameters tuning here, i mainly use what was recommended from the course lectures. It wokred quite fine anyway.

6. The final step was to draw the lines obtained from Hough Transformation in function draw_lines. I first calculated the slope from the 2 points of any line obtained from perious steps using the formular (y2-y1)/(x2-x1). If the slope is positive , I would put those point in a list of right lines and vice versa, I discarded points woth zero slope as they are horizonetal lines which are kind of noise. Using these 2 right and left lists, I find the coefficient of 1st-degree polynomial and then construct the left and right lines function. Next step was to fin y coordinates with x-coordinates of 2 left points and then 2 right points. With the list of all the (x,y) coordinates for both right and left lines, I simple draw the lines


An example of one of given test images gone through the pipeline:

![sample](https://github.com/tranlyvu/autonomous-vehicle-projects/blob/master/Finding%20Lane%20Lines/test_images_output/solidWhiteRightOutput.jpg)

These are examples of on 2 videos with [solid white lane lines](https://youtu.be/8O3j7ffj7MA) and [solid yellow white lane lines](https://youtu.be/vHA4nryV8QI) gone through the pipeline.


### Potential shortcomings in the pipeline

Looking from the output videos, the lines seemed skewed sometimes. Also, it did not perform well on challenge video where the car was keeping turning; i.e the lan lines are very curved


### Possible improvements to pipeline

One possible improvement was to apply the color filter to identify the lane lines.


