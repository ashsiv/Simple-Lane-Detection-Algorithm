# **Finding Lane Lines on the Road** 

## Project goal

* Design a pipeline to detect lane lines on the road even under challenging conditions (curves, shades of road colors etc) 
---
[//]: # (Image References)
[image1]:(https://github.com/ashsiv/Simple-Lane-Detection-Algorithm/blob/master/src/examples/laneLines_thirdPass.jpg)
[image2]:(https://github.com/ashsiv/Simple-Lane-Detection-Algorithm/blog/master/src/examples/actual.jpg) 
[image3]:(https://github.com/ashsiv/Simple-Lane-Detection-Algorithm/blob/master/src/examples/grayscale.jpg)
[image4]:(https://github.com/ashsiv/Simple-Lane-Detection-Algorithm/blob/master/src/examples/grayscale_blur.jpg)
[image5]:(https://github.com/ashsiv/Simple-Lane-Detection-Algorithm/blob/master/src/examples/canny.jpg) 
[image6]:(https://github.com/ashsiv/Simple-Lane-Detection-Algorithm/blob/master/src/examples/extrapolation.jpg)
[image7]:(https://github.com/ashsiv/Simple-Lane-Detection-Algorithm/blob/master/src/examples/overlayed.jpg)
[image8]:(https://github.com/ashsiv/Simple-Lane-Detection-Algorithm/blob/master/src/examples/colormask.jpg)
[image9]:(https://github.com/ashsiv/Simple-Lane-Detection-Algorithm/blob/master/src/examples/roi.jpg)
[image10]:(https://github.com/ashsiv/Simple-Lane-Detection-Algorithm/blob/master/src/examples/straightlanes.JPG)
[image11]:(https://github.com/ashsiv/Simple-Lane-Detection-Algorithm/blob/master/src/examples/curvedlanes.JPG)
![alt text][image1]
---
### Reflection:

### 1. Pipeline.

My pipeline consisted of the following steps:

* First, I have used color masks (yellow and white) on the image to focus on the lanes.
  ![alt text][image2]
  ![alt text][image8]
  ![alt text][image3]
* Then I converted the images to grayscale as shown above.
  
* After applying Gaussian filtering (blur),I subjected the image through Canny algorithm to detect lane edges.
  ![alt text][image4]
  ![alt text][image5]
* Defining a trapezoidal region of interest (focusing both lanes), I applied Hough Transform on edge detected image to detect lines.
    * As part of hough transform, in my draw_lines_method1() function,
        * I obtained the (x,y) coordinated of all the points on the left and right lanes separately. I used the slope of the line to               easily bin the coordintes to appropriate lanes
        * Then using cv2.fitLine function, I obtained the slope and a point on the line fit. Using this information, I extrapolated the           line across the boundaries of the image (knowing image size). I have trimmed the extrapolated lines again using the defined             trapezoidal region of interest to obtain the lanes.

  ![alt text][image9]
  ![alt text][image6]
  ![alt text][image7]
* Finally I overlayed the detected lane lines with original image as shown above.
  
### 2. Test cases output (videos)
* [SolidYellowRight](https://github.com/ashsiv/Simple-Lane-Detection-Algorithm/tree/master/src/test_videos_output/solidWhiteRight.mp4) 
* [SolidYellowLeft](https://github.com/ashsiv/Simple-Lane-Detection-Algorithm/tree/master/src/test_videos_output/solidYellowLeft.mp4) 
* [Challenge](https://github.com/ashsiv/Simple-Lane-Detection-Algorithm/tree/master/src/test_videos_output/challenge.mp4) 

### 3. Shortcomings of the pipeline

* Mask color range thresholds for yellow and white lanes will vary (especially for old roads with poor visibility of lanes)
* May not work well with bright sunlight (especially when driving against sun)
* May not work well during still traffic or tailgating (when cars are back to back where lanes become invisible)


### 4. Possible improvements

* Identify the curved portion of the roads rather than plain lane detection to advise driver of the upcoming turns & even direction. For   this purpose I have defined a draw_lines_method2() function, where,
    * Instead of a single line defining a lane, multiple line segments are drawn using the (x,y) coordinates of all the points on the         left and right lanes respectively. This will add more detail on the bending or curving areas of the roads as shown below.
      
       Single line fit:
       ![alt text][image10]
       
       Multi-line segment fit:
       ![alt text][image11]     
      
      This approach needs more investigation (I will update this github project after further analysis using this approach). Alternatively, instead of straight line fits, we can use non linear fitting as well.

