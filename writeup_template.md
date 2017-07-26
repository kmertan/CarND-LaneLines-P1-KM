# **Finding Lane Lines on the Road - Keith Mertan** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/white_yellow_example.jpg “Filtering for White and Yellow”
[image2]: ./test_images_output/hough_example.jpg “Hough Transform”
[image3]: ./test_images_output/final_example.jpg “Lane Lines Drawn On”


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline starts by making three copies of the image it reads in and extracting its x and y size. I then use two of the copies to select for white and yellow and combine them using weighted_img. 

![alt text][image1]

Afterward, I masked that image with a trapezoid based on the x and y sizes extracted earlier.

I used Gaussian blur with a kernel size of 5 to get rid of irrelevant lines and used 100 and 200 as thresholds for edge detection. My Hough transform used a minimum vote threshold of 3 and a minimum gap and maximum length of 15. 

![alt text][image2]

To hone in on the lane lines, I did not use draw_lines. I operated within my pipeline and fed the resulting vector, with the same dimensions as that of the Hough transform, into it. The process separates lines by sign then gathers the slope and y intercept of each. It then computes the median of each, since it’s robust to outliers. This, in addition to the y components of my trapezoid mask, allow me to compute x = (y - b) / m for the four endpoints of the two lines, giving me my (x, y) coordinates. Using weighted_img again completes my pipeline and returns the image with lane lines drawn on!

![alt text][image3]


### 2. Identify potential shortcomings with your current pipeline


Unfortunately, I have not completed the challenge… yet! My code breaks when faced with the near-white road. I’ve experimented with converting to HSV and changing almost all of my parameters, including those for color selection, Canny edge detection and the Hough transform. So far I have been unsuccessful, but I will certainly tackle this challenge in the near future.

Aside from the obvious, I know there will be trouble when there’s not a simple road in front of me. Sitting in traffic, entering intersections with no lines, driving at night, driving in direct sunlight, encountering white or yellow debris on the road: these are just a few examples of situations that would destroy my program. It’s just the beginning though. I’m excited to overcome all of these soon.


### 3. Suggest possible improvements to your pipeline

To improve this, I would definitely rely on more than just the image at hand to assign lane line positions. The lines don’t move around on the ground and my pipeline should know that! 

I could also improve my masking. There is a limit to how close together lane lines will be and I should take advantage of that by creating a polygon for each line. 

Lastly, I want to experiment with fitting second order polynomials to the lines if the variation among slopes exceeds a given threshold. That might give me a better approximation in heavy turns.

