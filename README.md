#**Finding Lane Lines on the Road** 

<<<<<<< Updated upstream
<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />
=======
##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.
>>>>>>> Stashed changes

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[orig]: ./test_images/solidWhiteCurve.jpg "Original"
[gray]: ./transformation/blur_gray.jpg "Grayscale"
[canny]: ./transformation/edges.jpg "Canny Edges"
[mask]: ./transformation/masked_edges.jpg "Masked"
[hough]: ./transformation/houghs.jpg "hough"
[hough2]: ./transformation/houghs2.jpg "hough2"
[final]: ./transformation/final.jpg "final"

[white]: ./white.mp4 "final"
---
<<<<<<< Updated upstream
For this project, a great writeup should provide a detailed response to the "Reflection" section of the [project rubric](https://review.udacity.com/#!/rubrics/322/view). There are three parts to the reflection:

1. Describe the pipeline

2. Identify any shortcomings

3. Suggest possible improvements

We encourage using images in your writeup to demonstrate how your pipeline works.  
=======
>>>>>>> Stashed changes

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of 5 steps. Below are the steps in detail. I use below image to demonstrate the steps.

<<<<<<< Updated upstream
The Project
---

## If you have already installed the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) you should be good to go!   If not, you should install the starter kit to get started on this project. ##

**Step 1:** Set up the [CarND Term1 Starter Kit](https://classroom.udacity.com/nanodegrees/nd013/parts/fbf77062-5703-404e-b60c-95b78b2f3f9e/modules/83ec35ee-1e02-48a5-bdb7-d244bd47c2dc/lessons/8c82408b-a217-4d09-b81d-1bda4c6380ef/concepts/4f1870e0-3849-43e4-b670-12e6f2d4b7a7) if you haven't already.

**Step 2:** Open the code in a Jupyter Notebook

You will complete the project code in a Jupyter notebook.  If you are unfamiliar with Jupyter Notebooks, check out <A HREF="https://www.packtpub.com/books/content/basics-jupyter-notebook-and-python" target="_blank">Cyrille Rossant's Basics of Jupyter Notebook and Python</A> to get started.

Jupyter is an Ipython notebook where you can run blocks of code and see results interactively.  All the code for this project is contained in a Jupyter notebook. To start Jupyter in your browser, use terminal to navigate to your project directory and then run the following command at the terminal prompt (be sure you've activated your Python 3 carnd-term1 environment as described in the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) installation instructions!):
=======
![alt text][orig]

#### Step 1: Convert the image to the gray scale and apply Gaussian smoothing
In this step, I convert the image to gray scale and then apply a slight Gaussian smoothing. I use the kernel size 5 to smooth out some of distracting edges. The result of this step will look like the exmaple below.

![alt text][gray]

#### Step 2: Apply Canny Endge Detection
In this step I use the blured gray image from step 1 to apply Canny Edge detection algorythm to lay out all the sharp edges of the image. Here I choose the low threashold of 50 and High threshold of 200. The result of this step is as below.

![alt text][canny]

#### Step 3: Create a mask for the region of the image where we have our interest on.
After applying the Canny algorythm to the Gray Blur image, there are many edges that are not really relevant to our lane detection  on the resulting image. In order to focus on the Lanes, we create a mask for the region where the lanes are most likely to be detected. Here, the region of interest will vary depending on the camera mount position, the focal length of the camera etc. Here we asume the camera is in a fixed location neat the mid front of the vehicle width-wise.
In order to dynamically define the masking area, I used dimentions of the image and 4 points in the x and y coordinates that are located at the lower half of the image. This way, I do not need to hard code the mask vertices for differnt sizes of images.

Below image shows the resulting masked image.

![alt text][mask]


#### Step 4: Apply Hough transformation on the masked edges¶
In this step, I apply Hough Transformation on the edges we detected on the masked region. Using Hough Tranform, We can find a line that fits a group of points in the image space.

![alt text][hough]


In this stage, You can see that the lane lines are emall segmants in some lanes. This is due to the dashed lines in some lanes. In order to avoid this, draw_lines() function that is being called inside the hough_lines() function was updated. Here, the goal was to represent both lanes as 2 seperate straight lines. 

Here I only used the lines that are more suitable to become left and right lane lines. This is determined by the slope of the line. The lines with a negative slope I iterate through each line and get the slopes of the lines that are negative as the left lane while the positive slopes are right lane. I then average the slopes of all the right lines and left lines seperately. Then I use that average slope to determine x coordinates for the top and bottom edges of the region I initially selected. Then I drow 2 lines to represent the left and right lanes on the original image. Below video demostrates this algorythm in action. Below steps explain the algorythm in short.

####1. Calculate the slope of the hough line
####2. If the slope is Negative, Its belongs to the left lane. Slope added to the left_slope list
####2.1. Calculate the left intersect and added to the left_intersect list
####3. If the slope is Positive, Its belongs to the right lane. Slope added to the right_slope list
####3.1. Calculate the right intersect and added to the right_intersect list
####4. Average the left_slope
####4.1. Average the left_intersect
####5. Average the right_slope
####5.1. Average the right_intersect
####6. Calculate the left x1 and x2 using the top and bottom edges of the region and y1 being the lower edge and y2 being upper edge of the region
####7. Calculate the right x1 and x2 using the top and bottom edges of the region and y1 being the lower edge and y2 being upper edge of the region
####8. Draw 2 lines using those coordinates. 

The resulting image will look like the following.

![alt text][hough2]

#### Step 5: Draw the lines on the original image
In this last step, I am drawing the hough lines on the original image to highlight the Lane lines. Below is the final image with highlighted lanes. 

![alt text][final]

This pipeline is applied to every image in a Video feed to identify the lane lines. Please see the video files white.mp4, yellow.mp4 for the annotated videos.

![alt text][white]


###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when there are bends in the road. In this situation, the straight lane lines drown up to the edge of the region will not follow the real lanes correctly.  

Another shortcoming could be that the result will be varied depending on the Camera mount location. The regios selction will have to be adjusted to correctly identify the lanes in this situation
>>>>>>> Stashed changes

When there are differnt resolutions of Video feed, It will affect the region selection. Region selection should be dynamic in this situation to correctly detect the lane lines. 


<<<<<<< Updated upstream
**Step 3:** Complete the project and submit both the Ipython notebook and the project writeup
=======
###3. Suggest possible improvements to your pipeline
>>>>>>> Stashed changes

A possible improvement would be to dynamically select the regions depending on the image size. I implemented this as part of trying to optimize the extra challenge. Please see the resulted video extra.mp4
