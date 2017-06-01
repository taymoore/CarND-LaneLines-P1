 **Finding Lane Lines on the Road** 
Writeup for Udacity's Self-driving car program
1. Pipeline
My pipeline consisted of 5 steps:
1. Greyscale
Converting to greyscale removed any colour aspect of the picture/frame allowing this pipeline to handle white or yellow lines in the same manner.
 
 
2. Canny Transform
Using the Canny transform with a low threshold of 50 and a high threshold of 200 was sufficient to detect high gradients in the frame while keeping detected gradients connected.
 
3. Blur
Since the canny transform is somewhat blocky and we want to analyze regions of the image, performing a gaussian blur regionalizes the canny detection to the surrounding pixels.
 
4. Mask Image
Since we are only looking for lines directly in front of the car, we remove any detected edges above and beyond the expected region of the road.
 
5. Draw Hough Lines
The first pass-through of a Hough transformation allows detection of larger line segments. This generalizes sections of the road paint and removes any small artifacts in the detected gradients.
 
6. Second Hough Lines
The second pass-through of a Hough transformation allows detecting of larger line segments with longer gaps with a lower resolution. This allows detection of single lane lines on the left and right side of the image.
 
7. Line filtering
By finding the longest line with a positive slope (between 0.4 and 0.8) and the longest line with a negative slope, we can find the most reasonable line to represent the overall lanes. See the white line in the above image.
2. Shortcomings
The shortcomings of the program is,
•	it does not consider the previous frame’s solution to find a solution nearby, weight it’s previous solution, or average them. Without this, the current program provides jittery and erratic performance.
•	It does not restrict the left or right lane to the left or right side of the screen. Occasionally with difficult terrain, the only upward sloping line is in the centre or right side of the screen which should cause a rejected line.
•	It should weigh lines at an optimal slope higher than lines in a sub-optimal trajectory, in stead of rejecting them outright. Lines within the acceptable limits are sometimes permitted even though they are erroneous.
 
3. Suggest possible improvements to your pipeline
Potential improvements include,
•	Considering the previous frame’s solution to weigh a line nearby higher than a line further away. This would reduce jitter.
•	Restricting upward facing lines to the left side of the screen while restricting downward facing lines to the right side of the screen.
•	Weigh lines in the expected trajectory higher than non-optimal trajectory lines.
