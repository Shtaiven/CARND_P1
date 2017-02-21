# **Finding Lane Lines on the Road** 

## Project by Steven Eisinger

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[swc]: ./part_1_results/solidWhiteCurve_lanes.jpg
[swr]: ./part_1_results/solidWhiteRight_lanes.jpg
[syc]: ./part_1_results/solidYellowCurve_lanes.jpg
[syc2]: ./part_1_results/solidYellowCurve2_lanes.jpg
[syl]: ./part_1_results/solidYellowLeft_lanes.jpg
[wcls]: ./part_1_results/whiteCarLaneSwitch_lanes.jpg

[swc_part2]: ./part_2_results/solidWhiteCurve_lanes.jpg
[swr_part2]: ./part_2_results/solidWhiteRight_lanes.jpg
[syc_part2]: ./part_2_results/solidYellowCurve_lanes.jpg
[syc2_part2]: ./part_2_results/solidYellowCurve2_lanes.jpg
[syl_part2]: ./part_2_results/solidYellowLeft_lanes.jpg
[wcls_part2]: ./part_2_results/whiteCarLaneSwitch_lanes.jpg

---

### **Reflection**

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The pipeline consists of five steps: converting the original image to grescale, applying a gaussian blur, finding edges using Canny edge detection, masking a region of interest to filter out the parts of the image that are unnecessary for identifying lane lines, then using the Hough transform to find sequences of points which can describe lines. An extra step superimposes the lane lines onto the original image. The tricky part of implementing the pipeline wasn't coming up with what algorithms to use, but with selecting the correct parameters to provide them. Most of my time was spent tweaking values to be able to get good results with all images provided. My results for detecting lane lines on still images are shown below.

![Solid White Curve][swc]
![Solid White Right][swr]
![Solid Yellow Curve][syc]
![Solid Yellow Curve 2][syc2]
![Solid Yellow Left][syl]
The five images above gave good results.

![White Care Lane Switch][wcls]
We can see some shortcomings of my first attempt at tuning the algorithm in this image. We can see a line detected at the edge of the road that isn't part of the lane.

When tuning my parameters, I wanted to make sure my pipeline was as dynamic as possible. While my first attempt was ok for the images and the first two videos, the pitfalls were really apparent when I tried my pipeline on the optional video at the end. I had this last video in mind while I was iterating on my parameters. I made my region of interest dynamic based on the image resolution instead of relying on fixed positions on the image, I made my lower Canny threshold a bit smaller to catch those lane lines in lower contrast areas, I made the minimum line length for the Hough transform large enough not to catch a lot of noise but small enough to catch the segmented lane lines and made sure my threshold was high enough to be fairly certain that a line I found could be a lane line. These parameter changes would help me greatly with my modified *draw_lines()* function.

My *draw_lines()* identifies whether a line should belong to the left or right lane, averages the slopes of the lines in each lane, and computes and averages their y-intercepts. From these averages, the left and right lane are extrapolated. There are statements to filter lines which have very shallow or very steep slopes which couldn't possible be part of the lane lines, and also prevents circumstances where lines with an infinite slope are factored into the average, which causes an error. My draw lines function achieved good results with all required images and videos, and somewhat spotty results with the optional video. The optional video is where the shortcomings of my pipeline were most apparent. The results of my *draw_lines()* function on the images from part 1 are shown below.

![Solid White Curve][swc_part2]
![Solid White Right][swr_part2]
![Solid Yellow Curve][syc_part2]
![Solid Yellow Curve 2][syc2_part2]
![Solid Yellow Left][syl_part2]
![White Care Lane Switch][wcls_part2]
All images gave good results, but we see the left lane line in the last image slightly shifted to the left due to the erroneous line seen in part 1.

### 2. Identify potential shortcomings with your current pipeline

There are a few situations where I don't see my pipeline working well to guide a car. 
An obvious one is when lane lines aren't present, which can happen quite often on local American roads. 
As seen in the extra video, my pipeline doesn't work well with low contrast lines, resulting in some points in the video where there isn't a line drawn at all.
There are also regions with sudden almost random edges (such as with road work or shadows) that skew the lane lines and severely effect the average slope and intercept computed by *draw_lines()*.
This pipeline also would't work with very curvy roads, as it expects straight lines.


### 3. Suggest possible improvements to your pipeline

I feel I could improve the Hough transform and Canny parameters to get better edge detection in low contrast situations.
Being able to extrapolate a curve instead of just a line would make the program more dynamic.
Detecting corners as well as lines would be useful for detecting places where a car might turn.
This pipeline shouldn't exist on its own. There are things like road signs and road hazards which Hough transforms wouldn't identify well.