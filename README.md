# **Finding Lane Lines on the Road**


[image1]: ./test_images/output_image_solidYellowCurve2.jpg.png 
[image2]: ./test_images/solidYellowCurve2.jpg




### 1. Description of the pipeline and the *draw_lines()* function. 

The modelling pipeline, as implemented in function *process_image()*, consisted of the following steps. 

- Gaussian smoothing (which can be considered a part of the Canny edge detection algorithm), since edge detection is easily affected by noise. 
	- Note, that converting to gray scale is not necessary since OpenCV's Canny algorithm can handle color/RGB images.   
- Masking out irrelevant area before applying line detection (Hough) algorithm. A trapezoidal area was chosen to focus on the relevant part of the image/video.
- Applying probabilistic Hough transform algorithm to the output of the previous step to identify line segments in the image. 

Here is an example of the input/original and the processed/annotated image:

![alt text][image2]

![alt text][image1]

The processed/annotated videos can be found in */test\_videos_output* folder.

In order to draw a single line on the left/right lanes, I modified the *draw_lines()* function by first identifying positive/negative slopes for the left/right lane line segments and finding the corresponding average slopes. Similarly, a mid/average point is found for each lane line segments. Having a slope and a point defines a line. Then, to extrapolate to the top/bottom of the image/video, the lower/upper points are identified. 

This approach worked fine on the test images, but when applied to the test video (*solidYellowLeft.mp4*) it produced annotated lines that, on two instances in the video, jumped away of the actual/ground truth lane lines. A "patch" to this problem was, when averaging line segments slopes, to ignore slopes that were clear outliers (e.g. abs(slopes) less than 0.1 or greater than 0.7). 



### 2. Potential shortcomings with the current pipeline/approach


A potential shortcoming of the approach is instability with respect to hyper-parameters; i.e. a slight change to some hyper-parameters made annotated lane lines unstable/'jumpy'. 

Another general shortcoming of the overall methodology is the need for manual search of hyper-parameters values. 

The approach can only handle/identify straight lane lines.   


### 3. Possible improvements 

- A more advanced algorithm that handles non-straight/curved lane lines (a variant of Hough can still be used).
- A hyper-parameter search method (alhough it is not clear what the objective function is...).
- A time-dependent approach that utilizes information from the previous frame(s).  
