# **Finding Lane Lines on the Road** 

## Writeup for Project 01

### In this project, lane lines on the road will be identified using tools such as color selection, region of interest selection, grayscaling, Gaussian smoothing, Canny Edge Detection and Hought transform line detection.

### A pipeline will be developed on a series of individual images, and later the pipeline will be applied to a video stream (really just a series of images). Check out the video clips that are being displayed in line to see how the final output looks like. 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on my work in a written report
---

[//]: # (Image References)

[image1]: ./test_images/solidWhiteRight.jpg "Original"
[image2]: ./examples/grayscale.jpg "Grayscale"
[image3]: ./test_images_output/GaussianSmoothing.png "Gaussian"
[image4]: ./test_images_output/Canny.png "Canny"
[image5]: ./test_images_output/Masked.png "Masked"
[image6]: ./test_images_output/Hough.png "Hough"
[image7]: ./test_images_output/solidWhiteCurve.jpg "Curve"
[image8]: ./test_images_output/solidWhiteRight.jpg "Right"
[image9]: ./test_images_output/solidYellowCurve.jpg "Yellow"
[image10]: ./test_images_output/solidYellowCurve2.jpg "Curve"
[image11]: ./test_images_output/solidYellowLeft.jpg "YellowLeft"
[image12]: ./test_images_output/whiteCarLaneSwitch.jpg "LaneSwitch"
[image13]: ./test_images_extrapolated_output/solidWhiteCurve.jpg "ECurve"
[image14]: ./test_images_extrapolated_output/solidWhiteRight.jpg "ERight"
[image15]: ./test_images_extrapolated_output/solidYellowCurve.jpg "EYellow"
[image16]: ./test_images_extrapolated_output/solidYellowCurve2.jpg "ECurve"
[image17]: ./test_images_extrapolated_output/solidYellowLeft.jpg "EYellowLeft"
[image18]: ./test_images_extrapolated_output/whiteCarLaneSwitch.jpg "ELaneSwitch"
---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 8 steps.

####  Step 1:
First an image is read and converted to grayscale. 
Helper function: grayscale(img)
Below is the original image:
![alt text][image1]

Below is the grayscale image
![alt text][image2]

#### Step 2:
Gaussian smooothing/blurring has been applied with a kernel size of 5 on the above grayscale image. 
Helper function: gaussian_blur(img, kernel_size)
Resulting image after Gaussian smoothing is shown below:
![alt text][image3]

#### Step 3:
Canny edge detection is run on the Gaussian smoothed image with low_threshold = 50 and high_threshold = 150
Helper function: canny(img, low_threshold, high_threshold)
Below is the result of Canny edge detection:
![alt text][image4]

#### Step 4: 
Define region of interest and get the masked image. Region of interest is selected in such a way that the bottom side of the trapezoid covers 85% of image width and the top side covers 7% of the image width. Height of the trapezoid is at 40% of the image height.
Helper function: region_of_interest(img, vertices) (fillpoly)
Below is the image of resulting edges after masking:
![alt text][image5]

#### Step5:
Run Hough on masked_image that only shows the line edges detected after masking out other edges. Hough_lines function returns the lines formed from the input(edge pixels array) that is the output of Canny fed into masking algorithm. Final output of this function is a set of lines fitted to edge pixels of the lanes.

Helper function: hough_lines(img, rho, theta, threshold, min_line_len, max_line_gap)
Below Hough Transform parameters have been chosen:
rho = 2
theta = np.pi/180
threshold = 15
min_line_length = 33
max_line_gap = 22
Resulting image of Hough lines:
![alt text][image6]


#### Step 6: 
Overlay Hough lane lines on the original image. This is done for all test images as shown below:
![alt text][image7]
![alt text][image8]
![alt text][image9]
![alt text][image10]
![alt text][image11]
![alt text][image12]

#### Step 7:
Extrapolation and drawing final lane lines on the original image. Firstly, left and right lanes are seperated based on their slopes(slope <0 = left lane line, as the y-axis is reversed on the image and vice-versa). Then x and y coordinates of these lines are seperate and a poly1d function is fit.

Left and right lane lines are then drawn by extrapolating the end points based on the polyid function.

**Lines of code calculating and drawing the end points of lane lines:**
cv2.line(line_image, (0, lf(0).astype(int)), (max(lx).astype(int),lf(max(lx)).astype(int)), color,thickness)
cv2.line(line_image, (min(rx), rf(min(rx)).astype(int)), (img.shape[1],rf(img.shape[1]).astype(int)), color,thickness)

Final images after extrapolation of lane lines are shown below:
![alt text][image13]
![alt text][image14]
![alt text][image15]
![alt text][image16]
![alt text][image17]
![alt text][image18]

#### Step 8: Final Pipeline with steps 1 through 7
Final pipeline with steps 1 through 7 are then applied to test video and the results are shown below:
<video width="960" height="540" controls>
  <source src="test_videos_output/solidwhiteRightTest.mp4">
</video>

<video width="960" height="540" controls>
  <source src="test_videos_output/solidYellowLeftTest.mp4">
</video>


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
