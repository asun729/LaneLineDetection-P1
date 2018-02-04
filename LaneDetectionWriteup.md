# **Finding Lane Lines on the Road** 

## Anqi Sun

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image0]: ./test_images/solidYellowLeft.jpg "OriginalPicture"
[image1]: ./test_images_output/gray.png "Grayscale"
[image2]: ./test_images_output/edges.png "CannyEdges"
[image3]: ./test_images_output/region_of_interest.png "Region Of Interest"
[image4]: ./test_images_output/line_img.png "Line Image"
[image5]: ./test_images_output/weight_img.png "Overlap the line with original picture"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified The draw_lines() function.
To demonstrate the pipeline, the example image I picked is solidYellowLeft.jpg
![alt text][image0]
My pipeline consisted of 6 steps. 
1. Convert the images to grayscale, 
![alt text][image1]
2. Detect edges in the grayscale image with Canny Edge Detection, 
![alt text][image2]
3. Define the region of interest in the image, and mask the edges
![alt text][image3]
4. With the edge masked based on selected regions, I did hough transform on the edges to connect points to line segments. 
5. The next step is to draw solid lines with line segments from hough transform. 
![alt text][image4]
6. Finally, overlap the solid line with original image with weighted_img()
![alt text][image5]



In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...
1. Categorize the hough lines in two cases: 1) slope>threshold 2)slope<-threshold
The tuning of threshold started from 0 and finalized at 0.4. To improve the robustness of the code, one me ore condition is applied to the line. For left lane, the x-coordinate has to stay in the rough left hand side; for right lane, the x-coordinate has to stay in the rough right hand. This condition was set up to filter some noise from the hough line generation. E.g., if some small segments are detected with negative slope but stays on the right half, then it should not fall into the left array. 

2. After separating lines in two categories,  the endpoints of the lines are stored in four arrays (x_R,y_R,x_L,y_L) correspondingly. 

3. After iterating through all lines in the set, I did a linear regression with polyfit on the two arrays for each lane. This curve fit outputs the slope and intercept of each line. A special case is considered where there are only 4 points in the array and there exist two linear regressions for the four points. A conditonal selection is conducted to make sure it outputs the correct slope.

4. Next, to exterpolate the lines properly, I configured the endpoints for each lane based on the bottom line and the top point in each array. 

5. To highight the solid line, thickness was set to 10

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when lightening condition changed in the sensor. For example, when the video is brighter, the threshold value used for Canny Edge will change and some edges might be eliminated because there are less difference in the grayscale.


Another shortcoming could be "interest of region". The four points of interest of region are hard coded based on the sample images and videos. However, when the mount of camera is changed or there are slope on the road, the four points will not be applicable to the new camera setup.

Another shortcoming could would be what would happen when the road has a turn in front. Fitting two straight lines for two lanes would not be sufficient for a complicated lane curve.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to using some algorithm to detect the end of lane and set up the interest of region intelligently

Another potential improvement could be to fit more complicated lines than a straight line for a lane. 

Also, finding a more efficient algorithm to fit the lane for each frame to speed up the detection process. 
