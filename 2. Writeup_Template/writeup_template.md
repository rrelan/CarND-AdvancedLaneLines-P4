##Writeup Template
###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./examples/undistort_output.png "Undistorted"
[image2]: ./test_images/test1.jpg "Road Transformed"
[image3]: ./examples/binary_combo_example.jpg "Binary Example"
[image4]: ./examples/warped_straight_lines.jpg "Warp Example"
[image5]: ./examples/color_fit_lines.jpg "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!
###Camera Calibration

####1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.
I started by reading all the files from folder camera_cal using glob library . I initialized the object points 
as 6 on y axis , 9 on x axis with plane z =0 . I then converted the image to GRAY format using cv2 library and then using functiom
"findChessboardCorners" I found out Chess board corners. Then using "drawChessBoardCorners"  function I drew these corners
and diplayed  - Oroginal images as well as Chessboard images as shown in the Python notebook P4.ipynb. 




###Pipeline (single images)

####1. Provide an example of a distortion-corrected image.
To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]
####2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

Please check Cell # 2,3,4,5,6,9,10 for code and output (python notebook)

First I did a perspective transform of the image . Once I got the perspective transform (soruce and destination I have coded
inthe perpective transform function call)  , I changed the format of images to HLS. Then I separated channels for H, L and S
 . Then using threholding values and np.zeroes I converted them into binary format . I also used sobel operator and
 did overlapping of  Saturation image and sobel image.  In second cell of Ipython notebook , I have shown output of all the 6 image
 -> Original Images
 -> Perspective Transform Image
 -> H Chanel
 -> L Channel
 --> S Channel 
 --> Combined ( S + Sobel) 
 

####3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

Please check cell # 7 and 8 for code and output (python notebook) 

To find the perspective transform , first I undistorted the image by calling undistort function (in undistort function
I use function of openCV cv2.undistort) .  After undistroting the image I take the size of the image. 

Then I use the following source and destination points : 

    src = np.float32([[450, 490],[820, 490],
                      [1280, 740],[50, 740]])
    dst = np.float32([[0, 0], [1270, 0], 
                     [1280, 740],[40, 740]])
                     
                  

After that I caclulate the "Perspetive Transform Matrix" (M) using function cv2.getPerspectiveTransform (input being src and dest). 
After calcuating the Matrix , M  , I get warped image using function cv2.warpPerspective . 


####4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Cell # 11  - code for generating the threshold image 
Cell # 12  - code for finding histogram and polynomial fit 
Cell # 13  - code with output 

To identify lane lines I have takne histogram of image (input is threshold image) . Then I have detected peaks 
using which I have found out left and right lane.  I have also detected non-zero pixels
whcih are in close proximity to the image (code in cell # 12) . I have fit a second order polynomial polynomial for each
line - i.e. left and right lane (code in cell # 12). 

####5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

In order to find radius of curvature , I have taken threshold image as input and
after estimating the left and right pixels (code in cell # 12) I have found
out radius of curvature using quadrtic coeef= 1.5 and using equation as descirbed
in lessons . Also , please note that since the value of y increases from top to bottom
so I have calculated the radius of curvature using y-values rather than x-values. 


####6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

All examples are listed as output to  cell # 12 (my sincere aplolgy I am facing some
issue loading image to markdown)

###Pipeline (video)

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

I have attached my output video . Name of output video = "Output_P4_video.mp4"

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I found this approach to be too much code intensive  . I do appreciate that we
are learning openCV here which has been told by speakers in the early part of the
chapter but I would like to try this approach using image classification (deep learnign)
may be check out LeNet / GoogLeNet architecture and see how this performs. 
