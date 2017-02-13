# PROJECT ADVANCED LANE FINDING #

## OBJECTIVE ##

This project is the udacity advanced lane finding project. The objective of this project is to find the lane lines on the road using advance yet effective camera image processing methodologies.

## STEPS ##

1. Calibrate the image using given chessboard images.
1. perform image distortion correction based on calibration matrix.
1. Perform color and gradient transformation to create a thresholded binary image.
1. Apply a birds eye view perspective transform to view the lines as parallel lines.
1. Detect lane pixels and fit to find the lane boundary.
1. Determine the curvature of the lane and vehicle position with respect to center.
1. Warp the detected lane boundaries back onto the original image.
1. Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!
###Camera Calibration

####1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the **find_corners** and ** undistort_image** of the IPython notebook located in "Project-Advanced-lane-finding.ipynb.ipynb".

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![Original, distorted image][test1.jpg]

###Pipeline (single images)

####1. Provide an example of a distortion-corrected image.
To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![Distortion correction][output_images/test_undist_2.jpg]
####2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.
I used a combination of color and gradient thresholds to generate a binary image as called out in ** color_thresholding ** and ** abs_sobel_thresholding ** functions in the same ipynb.  Here's an example of my output for this step.

![color thresholding][output_images/color_thresh.jpg]
![sobel thresholding][output_images/sobel_thresh.jpg]
![combined thresholding][output_images/combined_thresh.jpg]

####3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `perspective_transform()`, which appears in the file `Project-Advanced-lane-finding.ipynb.ipynb`.  The `perspective_transform()` function takes as inputs an image (`img`), as well as camera matrix (`mtx`) and distortion coefficients (`dist`).  I chose to hardcode the source and destination points in the following manner:

```
top_right_src = [585. /1280.*img_size[0], 455./720.*img_size[1]]
top_left_src = [705. /1280.*img_size[0], 455./720.*img_size[1]]
bottom_right_src = [1270./1280.*img_size[0], 720./720.*img_size[1]]
bottom_left_src = [190. /1280.*img_size[0], 720./720.*img_size[1]]

src = np.float32([
   top_right_src, # top right
   bottom_right_src, # bottom right
   bottom_left_src, # bottom left
   top_left_src # top left
])
    
top_right_dest = [300. /1280.*img_size[0], 0./720.*img_size[1]]
top_left_dest = [1000./1280.*img_size[0], 0./720.*img_size[1]]
bottom_right_dest = [1000./1280.*img_size[0], 720./720.*img_size[1]]
bottom_left_dest = [300. /1280.*img_size[0], 720./720.*img_size[1]]
    
dst = np.float32([
   top_right_dest, # top right
   bottom_right_dest, # bottom right
   bottom_left_dest, # bottom left
   top_left_dest # top left
])
```
This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 585, 455      | 300, 0        | 
| 705, 455      | 1000, 0      |
| 1270, 720     | 1000, 720      |
| 190, 720      | 300, 720        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![warped image][output_images/birds_eye_view.jpg]

####4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![Found lane line image][output_images/Window_lane_finding.jpg]

####5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

####6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![lane on road][output_images/detected_lane_mask.jpg]

---

###Pipeline (video)

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](Project_video_output.mp4)

---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
