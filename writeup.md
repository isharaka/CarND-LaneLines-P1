#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./writeup_images/solidWhiteCurve_hlines.jpg "Hough lines"
[image2]: ./writeup_images/solidWhiteCurve_contours.jpg "Contours"
[image3]: ./writeup_images/solidWhiteCurve_convex_hull.jpg "Convex Hull"
[image4]: ./writeup_images/solidWhiteCurve_lane.jpg "Lane Marker"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The first 5 steps are the standard steps presented in the lectures. 
I did not use color selection so that the pipeline would be agnostic to lane marker colors.

1. Convert to grayscale
2. Gaussian blur to remove noise
3. Find canny edges
4. Mask out a triangular area of interest at the bootom half of the image
5. Find Hough lines 

At this point I split the region of interest to left and right halves, to ensure I detect lane markers on eother side.
i.e. to avoid detecting 2 'line' on the same side due to potential edges due to shadws and curbs

![alt text][image1]

6. Convert Hough lines to a binary image for use with morphing and contour recognition
(I dod not end up using morphing since the pipeline did not show much improvement with it)

7. Next I found contours on the Hough lines to detect lane marker segments
![alt text][image2]

8. In order to combine lanemarker segments, I extracted all the points from the contours and calculated the convex hull over them.
This is the method I used to 'average' line segments.
![alt text][image3]

9. In order to 'extrapolate' the combined line segments I fitted a line to the convex hull.
This line is expected to be the the most likey line representing a lane marker
![alt text][image4]




###2. Shortcomings with your current pipeline


1. Splitting ROI to left and right is not the most robust solution. 
If the car turns sharply across lane markers this will fail, because the assumption that there isone lane marker in each half no longer holds.

2. An outlying small contour can drastically change the shape of the convex hull, since the pipeline treats all contours equally. 
This is evident with the challenge video clip.


###3. Possible improvements to your pipeline

1. Instead of splitting ROI, it may be possible to to detect all potential lines and use the 'properties' of  lane markers to pick the two lines that best fit them.
i.e. the fact that lane markers are parallel, the distance between them is within a certain range

2. Instead of treating all contours equally, it is possible to filter out and/or weight contours based on area and/or orientation

3. With challenge clip, the bottom part of the image, which is pat of the car' interfers. This could be avoided by changing the ROI.
A more robust solution would be to use 'background elimination' since this part of the image does not change from frame to frame if we filter out the reflections.

4. If the result from previous frames could be used the, the pipeline could be made more robust against noise such as shadows across the road.
