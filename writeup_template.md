# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of X steps. 
1. First, I converted the images to grayscale
2.




In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...
Categorize the hough lines in two cases: 1) slope>threshold 2)slope<-threshold
The tuning of threshold started from 0 and finalized at 0.4. To improve the robustness of the code, one me ore condition is applied to the line. For left lane, the x-coordinate has to stay in the rough left hand side; for right lane, the x-coordinate has to stay in the rough right hand. This condition was set up to filter some noise from the hough line generation. E.g., if some small segments are detected with negative slope but stays on the right half, then it should not fall into the left array. 

After separating lines in two categories,  the endpoints of the lines are stored in four arrays (x_R,y_R,x_L,y_L) correspondingly. 

After iterating through all lines in the set, I did a linear regression with polyfit on the two arrays for each lane. This curve fit outputs the slope and intercept of each line. A special case is considered where there are only 4 points in the array and there exist two linear regressions for the four points. A conditonal selection is conducted to make sure it outputs the correct slope.

Next, to exterpolate the lines properly, I configured the endpoints for each lane based on the bottom line and the top point in each array. 


If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
