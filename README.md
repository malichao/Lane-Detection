## Lane-Detection

My pipeline consists of 6 steps.  

1. Convert the images to grayscale
1. Smooth the images and get rid of the noise using Gaussian filter
1. Detect the edges in the images using Canny Edge Detector
1. Crop the region of interest, which is a trapezoidal mask in the bottom center
1. Extract the lines out of the images using Hough Transform
1. Extrapolate the small line segments and draw a complete line for each lane

The following pictures show the process of step 1,3,and 6  

<img src=examples/grayscale.jpg width="450">  
<img src=examples/line-segments-example.jpg width="450">  
<img src=examples/laneLines_thirdPass.jpg width="450">  
  
In order to extrapolate the lines, I did the following work:  
  
1. Rule out the lines that are vertical or nearly horizontal, as they are not candidates of real lane lines.
1. Group the lines segments by their slopes and positions. If a line is in the left half plane and its slope is negative, then I consider it as a valid lane line and add up the slope and x,y position. The same rule applies for the right lane.  
1. After grouping, I calculate the average slope and x,y position, and then draw a line from the bottom to the center of the image.
  
The following pictures show the result of step 1. Due to the shade and the different colors at different road, there will be some spurious detection. If they are not ruled out, the result is totally off.  

<img src=doc/wrong_detection.png width="450">  
<img src=doc/improved_detection.png width="450">  

### Potential shortcomings with my pipeline  

1. Sometimes the dectector doesn't pick up the yellow lane line because its color is too close to the road
1. Sometimes there is no line drawn because too much line segments are filtered out. This could be improve by fine tuning the parameters
