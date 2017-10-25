##Writeup Template
###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./examples/train_car.png
[image2]: ./examples/hog1.png
[image3]: ./examples/find_car1.png
[image4]: ./examples/find_car_sliding1.png
[image5]: ./examples/heatmap1.png
[image6]: ./examples/heatmap_thresholded.png
[image7]: ./examples/labelled_boxes.png
[image8]: ./examples/scikit_labels.png
[video1]: ./ai_cars.mp4

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this is in the python methods get_hog_features() and extract_features(). The sample training images looked as follows.


![alt text][image1]


Here is an example using the `YCrCb` color space and HOG parameters of `orientations=9`, `pixels_per_cell=(8, 8)` and `cells_per_block=(8, 8)`:


![alt text][image2]

####2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters and...

####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using...


| Classifier |Color Space | Orient | PixPerCell | Hog Channels | SpatialBins | HistBins | Accuracy | TrainTime |
| :--------: | :--------: |:------:| :---------:| :---------:  | :---------: | :-------:|:--------:| ---------:|
| Linear SVC |  RGB       | 9      |   8        |   ALL        | 32          | 32       | 0.97     | 20.45     |
| Linear SVC |  RGB       | 9      |   8        |   ALL        | 32          | 32       | 0.97     | 19.26     |
| Linear SVC |  RGB       | 9      |   8        |   ALL        | 32          | 32       | 0.96     | 21.77     |
| Linear SVC |  YUV       | 9      |   8        |   ALL        | 32          | 32       | 0.97     | 19.22     |
| Linear SVC |  YCrCb     | 11     |   8        |   ALL        | 32          | 32       | 0.97     | 24.13     |
| Linear SVC |  YCrCb     | 11     |   16       |   ALL        | 32          | 32       | 0.98     | 18 .13    |

The results and accuracy were better with YCrCb, orient=11 PixPerCell=16 and all HOG channels. Did not need spatial bin and color bin features.

See the sample image after find_cars()
![alt text][image3]

###Sliding Window Search

The sliding window algorithm was implemented using the experimental ystart and ymin parameters. First I checked on on eimage for values of 400, 640. Based on that I refined these in a for loop at the intervals of 4 between 390 and 640. It worked well. See the example below.

![alt text][image4]

####2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

I also used heatmaps, scikit labels and heatmap thresholding as explained in the udacity lecture. These gave good results in the real video in eliminating false positives. See sample images.


---

### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./ai_cars.mp4)

[video1]


####2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

  
The methods add_heat(), apply_threshold(), draw_labelled_boxes() illustrate filtering false positives using heatmaps and labels.

### Here are six frames and their corresponding heatmaps:

![alt text][image5]

### Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:
![alt text][image6]

### Here the resulting bounding boxes are drawn onto the last frame in the series:
![alt text][image7]

![alt text][image8]





---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

