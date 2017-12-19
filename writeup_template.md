
#  Project 05 : Vehicle Detection
-----


### Histogram of Oriented Gradients (HOG)

---

[image01]: ./Images/car_hog.jpg "car_hog"
[image02]: ./Images/non_car_hog.jpg "non_car_hog"
[image03]: ./Images/car_spatial.jpg "car_spatial"
[image04]: ./Images/non_car_spatial.jpg "non_car_spatial"
[image05]: ./Images/car_color_hist.jpg "car_color_hist"
[image06]: ./Images/non_car_color_hist.jpg "non_car_color_hist"


#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

 - I used hog feature as one of the feature 
 - The extraction of hog feature is done by function **`get_hog_features()`**
 - Used **`skimage.feature`** package's **`hog`** function for extracting hog features 
 - In classifier I used hog feature for every channel of the image to get as much information as I can
 - For visualization I have converted the image to gray scale and displayed the hog original image and hog image
 - Below Is the example of the original image of cars and their hog images
 
 
 ![HOG][image01]

 - Below Is the example of the original image of non-cars and their hog images


 ![HOG][image02]


#### 2. Explain how you settled on your final choice of HOG parameters.

 - I did not do much experiment with the parameters
 - Following parameter I used :
            - Number of orientation bins : 11
            - Size (in pixels) of a cell : 16
            - Number of cells in each block : 2
 - These parameters are defined in code cell number 23

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

 - I represented the images into YUV Space
 - I also used spatial and histogram hog features
 - Spatial features I used were of dimension 32*32 which were flatten
 ### **Below are examples of spatial features of car images**
 
  ![HOG][image03]

 ### **Below are examples of spatial features of non-car images**
 
  ![HOG][image04]

 ###  **I used color histogram features of each channel**
 ###  **Below are examples of color histogram features of car images**

    ![HOG][image05]

 ### **Below are examples of color histogram features of non-car images**
 
   ![HOG][image06]
 
 - Each Data point had **4356** features when hog, sptial, color histogram features were concatenated 
 - Used a Linear SVM as a classifier
 - By these features and Linear SVM I got an accuracy of **98.42** on test images
 
 ### Sliding Window Search

---




[image07]: ./Images/restrict_search_window.jpg "restrict_search_window"
[image08]: ./Images/Results.jpg "Results"

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?
 - I restrcited my window search from **y-start co-ordinate at 400** and **y-stop co-ordinate at 656**
 - I performed Sliding Window search 3 times with same **y-start co-ordinate and y-stop co-ordinate** but different scale
 - Below is example of **y-start co-ordinate and y-stop co-ordinate** to restrict the window search
   ![SWS][image07]
 - I initially used a scale of 1.0 but it did not predict all the cars
 - So Used Sliding Windows with following scaling factors : [2.0, 1.0, 1.5]
 
 
#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?
 - Below are Examples of Sliding Windows with scaling factor : : [2.0, 1.0, 1.5]
   ![SWS][image08]

   
   ### Video Implementation
----

[image09]: ./Images/Results.jpg "Results"
[image10]: ./Images/heatmap.jpg "heatmap"
[image11]: ./Images/after_thresholding.JPG "after_thresholding"
[image12]: ./Images/label_box.JPG "label_box"
[image13]: ./Images/final_result.JPG "label_box"


#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video_output.mp4)



#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.
 - I recorded the positions of positive detections in each frame of the video.  
 - From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions. 
 - I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  
 - I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  
 - Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here is an image and their corresponding heatmaps:

  #### Original Image
  ![VI][image09]
  
  #### Heatmap Image
  ![VI][image10]

  #### Thresholded Heatmap Image
  ![VI][image11]

### Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:
  ![VI][image12]


### Here the resulting bounding boxes are drawn onto the last frame in the series:
  ![VI][image13]

  
  ### Discussion
-----

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?


- The pipeline fails where there is a overlap between two cars it detects only on bounding box for the two cars
- If the car changes lane very quickly then the classifier might not recognize the car
- I would have used a neural network approack like YOLO ot R-CNN for vehicle detection

