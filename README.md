**Advanced Lane Finding**

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

[image1]: /output_images/CameraCalibration.png "Calibration"
[image2]: /output_images/Original_vs_Processed.png "Binary Example"
[image3]: /output_images/Binary_Undistorted_Colored "Binary Undistorted Colored"


---
###README
This README is based on a template that can be found [here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md)

###Camera Calibration

####Description of how I computed the camera matrix and distortion coefficients
 

I started by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

###Pipeline (single images)

#### Example of a distortion-corrected image.
See below

####2. Description of how I used color transforms, gradients or other methods to create a thresholded binary image.  
I used a combination of color and gradient thresholds to generate a binary image. This code can be found in the 3rd cell of `AdvancedLaneFinding.ipynp` file. An example of the output can be seen below.  

![alt text][image2]

####3. Description of how I performed a perspective transform.  

The code for my perspective transform includes a function called `unwarp_image()`, which appears in cell 5 of the `AdvancedLaneFinding.ipynp` file. The `unwarp_image()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose to hardcode the source and destination points in the following manner:

```
    src = np.float32([[580,460],[710,460],[1150,720],[150,720]])
    dst = np.float32([[offset1, offset3], 
                      [img_size[0]-offset1, offset3], 
                      [img_size[0]-offset1, img_size[1]-offset2], 
                      [offset1, img_size[1]-offset2]])

```

####4. Description of how I identified lane-line pixels and fit their positions with a polynomial

The code this step appears in cell 5 of the `AdvancedLaneFinding.ipynp` file.  
To find the position of the lanes, a histogram is generated from which I get a pair of peaks that represents the lanes. Then I do a sanity check that makes sure that those are indeed the lanes that I'm looking for. When my lanes are established, I use then go ahead and fit the pixels with a polynomal.


####5. Description of how I calculated the radius of curvature of the lane and the position of the vehicle with respect to center.
The code for this step appears in cell 5 of the `AdvancedLaneFinding.ipynp` file.  
This was done by first converting pixles into meters. The converstion coefficients were determined using the US Government requirements for the lane width and dashed line distance. Then I fitted a polynomial for each lane and determined the center. The polynomial derivatives are then calculated to get the curvature. 


####6. Example image of my result plotted back down onto the road such that the lane area is identified clearly.

![alt text][image3]

---

###Pipeline (video)

####1. Link to my final video output. 

Here's a [link to my video result](https://github.com/AmanMander123/AdvancedLaneFinding)

---

###Discussion

####1. Brief discussion on any problems / issues I faced in my implementation of this project.  Where will my pipeline likely fail?  What could I do to make it more robust?

One of issues that I had with the implementation was tuning the hyperparameters to covert the image to binary. If this process was not done correctly, the pipeline did not do a good job at fitting the curve. If there is one place that the pipeless would fail, this would be it. If the lane pixels are not determined correctly, the fitted curve would be off. In order to make it more robust, I can implement a more stringent sanity check and put in place fail checks to correct for the mistakes.



