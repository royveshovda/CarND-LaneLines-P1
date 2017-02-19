# **Finding Lane Lines on the Road**
---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report
---

## Reflections

### 1. Pipeline.

My pipeline consisted of the following 5 steps:
1. Convert to grayscale
2. Blur the image
3. Canny detection
4. Crop to region of interest
5. Detect Hough lines

The pipeline also extends the Hough lines to fill in gaps. This process is described as part of how I extended the draw_lines function.

The result of the pipeline can be seen in the section below call Result of pipeline.

#### Modification of draw_lines()
As part of this project I had to modify/extend the function draw_lines. Mostly to project to single lanes, one for left and one for right.

In short this function calculates the slope of each of the lines from the Hough detection, and classifies them as being left lane or right lane (or none of those). The function collects all the points in an array for each of the lanes.
After that process I used OpenCV's fitLine function to get a single line representing these points. This line I in turn extended to go from the bottom of the screen all the way up to the upper most detected point.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image:

### Result of pipeline
#### Original
![Original][image1]

#### Grayscale
![Grayscale][image2]

#### Blur
![Blur][image3]

#### Canny
![Canny][image4]

#### Edges
![Edges][image5]

#### Final result
![Result][image6]

[image1]: ./test_images/solidWhiteRight.jpg "Original"
[image2]: ./test_images/processed-solidWhiteRight-gray.jpg "Grayscale"
[image3]: ./test_images/processed-solidWhiteRight-blur.jpg "Blur"
[image4]: ./test_images/processed-solidWhiteRight-edges.jpg "Canny"
[image5]: ./test_images/processed-solidWhiteRight-overlayed_lines.jpg "Edges"
[image6]: ./test_images/processed-solidWhiteRight.jpg "Result"

### 2. Potential shortcomings with current pipeline
#### 2.1 Sensitivity
I ran the pipeline on the challenge video, and immediately saw the algorithm is way too sensitive to noise in the image. Cracks and color changes are detected as markings.

#### 2.2 Curves
The current pipeline looks for lines, and in curves this is simply wrong. Luckily the markings in curves are almost lines, so the pipeline will get it mostly correct. But on smaller roads with tighter turns, the pipeline will most likely fail more often.

#### 2.3 Slope detection
The pipeline uses absolute values for detecting the lanes to be right or left lane. This is OK on the test images/videos. But is also very sensitive to any kind of abnormal markings. Also if the camera should be slightly tilted, the pipeline will fail.

#### 2.4 Only current images
The pipeline only observes the current image, and tries to detect the lane markings. No clues from the previous images are used to adjust the pipeline in any way.

### 3. Suggest possible improvements to your pipeline
These suggestions are of course related to the shortcomings from the previous section.

To improve the noise sensitivity, the algorithm needs to be tuned to be more robust in changing conditions. Including poor visibility.

The curves are a challenge for this pipeline, as we only look for lines. A different algorithm will need to be found for detecting the lane markings in curves. I do not have a good suggestion for this problem.

The absolute values for the slope is also a good candidate to improve. Maybe an algorithm for clustering slopes could improve the detection, and also allow for detecting markings for other lanes as well.

I think great improvements can come from using lane markings from previous images (in a stream) as a guideline for where to expect the lane markings in the current image. It cannot be the answer, but maybe as a sanity check for where the markings should most likely be.
