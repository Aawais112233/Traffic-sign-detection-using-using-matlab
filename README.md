# Traffic-sign-detection-using-using-matlab

 

Traffic Sign Detection for Vision-based Driver’s Assistance in Land-based Vehicles

I.	INTRODUCTION

 Autonomous vehicle has been an active area of research for a few decades. Intensive research has been done on using a front viewing camera for vehicle localization and navigation, environment mapping, and obstacle avoidance. There are main four goals that every traffic sign detection algorithm is aiming to achieve. The first goal is that the algorithm must be accurate. Accuracy is the most basic requirement for the algorithm. For application of driver’s assistance, high accuracy under nominal condition may be enough. But in other applications, for example, in the context of fully autonomous cars, accuracy in worse case scenario must be considered and properly monitored. Being able to achieve high accuracy in different environmental conditions immediately leads to the second evaluation metric for the detection algorithm - robustness. Since it is hard to predict what kinds of condition the vehicle will encounter, to achieve high accuracy, the algorithm must be able to achieve the desirable result even under adverse conditions. The integrity and consistency of the algorithm must be evaluated in addition to the accuracy under nominal condition. The third metric for evaluation is that the detection algorithm has to be fast. The ultimate goal for the detection algorithm is to be implemented in real time. Again, in the application of autonomous vehicles, first of all, the computational power is limited. Secondly, detection of traffic sign is only a small task in an autonomous system network. And thirdly, autonomous vehicles are moving at a relatively fast speed and usually require a reaction towards a traffic sign within a few seconds. Finally, the last evaluation metric for the detection algorithm is the cost. In order to achieve autonomous driving, the vehicle already requires various sensors including GPS, inertial sensors, radar, and even Lidar. Although camera is not the most expensive sensor on board, being able to achieve a relative high detection rate using low-cost camera is still in the industry’s best interest.


II.	ALGORITHM 
 


 The algorithm implemented is based on colour detection followed by shape detection using Hough transform. The algorithm takes an input image and pre-process the image through colour enhancement. Then the algorithm perform colour segmentation according to a target colour. The output after this step is a binary image with regions containing the target colour labelled. Then an edge detector is applied to each region. Hough transform is then performed on the edge image. Detection result is determined by looking at the distribution of peaks in Hough transform. Figure 1 shows a typical input image to the algorithm. For this image, the stop sign is the desirable output from the detection algorithm.

A.	Colour Enhancement 
The very first step in this detection algorithm is enhancement of colours. The purpose of this step is to make the colours in the image more distinguishable and to prepare for colour space transformation. The colour enhancement part contains the following steps: 
• Transform the image from RGB space to L*a*b space
 • Apply adapted histogram equalization on the luminosity layer 
• Transform the processed image back to RGB space Figure 2 shows the input image after applying colour enhancement.

B.	Colour Segmentation
 Colour segmentation contains the following general steps:
 • transform the image into desired colour space
 • Extract the target colour by thresholding the proper value
 • Return a binary mask of the desired colour regions 
The colour space transformation and thresholds are different for different target colours. The algorithm presented only looks at detection of two colours, red and yellow,
 
1)	Red Colour Detection:
 A combination of two threshold method was used for red colour detection. The first threshold is in the HSI colour space with H > 0 and H < 0.111π or H > 1.8π and H < 2π, 0.1 < S 6 1,0.12 < I < 0.8. The second method, is in the HSV space and the threshold is 160/179 < H < 1. Finally, regions detected positive by both methods are labelled as red colour regions.
2)	Yellow Colour Detection: 
The colour space used here is HSV but with a different threshold value of 24/179 < H < 28/179. After apply colour segmentation with the target colour, it can be seen in Figure 3 that three regions were detected to contain the colour red.

C.	Edge Detection
 After colour segmentation step, a binary image of the target colour was generated. For each of the regions with positive colour detection result, 
• Perform region counting and labelling on the binary image 
• Extract a rectangular region for each of the labelled region
 • Apply canny edge detector on each of the extracted grayscale image region Figure 4 is the edge image of one of the region of interest in the original image.
 

D.	Hough Transform 
The final part of the algorithm is Hough transform. Again, for each of the detected regions,
• Apply Hough transform on the edge image
 • Compute a distance score between the distribution of peaks and a target shape distribution to determine if there is a desired shape present.
The algorithm only looks at two types of shape, a diamond and an octagon. An upright diamond shape traffic sign will have two distinctive peaks at ±45 degrees. But for most traffic signs, the content inside the diamond will also result in three additional peaks at 0 degree and ±90 degrees. The peak distribution for an octagon is the same as a diamond sign. So the peak distribution for both shapes should be centered on −90 degrees, −45 degrees, 0 degree, 45 degrees, and 90 degrees. In the Hough transform heat map in Figure 5, it can be seen that most peaks are concentrated at ±90 degrees and 0 degree. And there are some weaker peaks centring around ±45 degrees as well. This indicates that a diamond or octagon shape is likely to be present in this region of interest. After comparing the distribution of Hough transform peaks in each of the region of interest, the algorithm successfully detects the stop sign and the final detection result is shown in Figure 6. It is interesting to note that even though the image contains a lot of noise and was motion blurred by the movement of the car, the detection algorithm is still able to extract the proper edges and the peak pattern in the Hough transform is still preserved.

 
 



III.	EXPERIMENTAL RESULTS 
The implemented algorithm was tested on two sets of traffic signs. The first set is the diamond-shaped yellow warning signs and the second set is the red stop signs. Each set contains 10 different images. The first set contains 10 images with distinctive warning signs. The second set contains only stop signs. Both sets of images are of various quality and under different illumination condition. The location and size of the signs also vary from image to image. Table I shows a summary of detection results on each of the test sets. The second set achieves a higher accuracy. This is because compared to the colour red, yellow is more commonly found in natural scenery and therefore results in difficulty in effective colour segmentation. In addition, the assumption of vertical and horizontal edges in the warning signs is not always valid. For example, the pedestrian warning sign does not have distinctive horizontal and vertical edges.
 

IV.	CONCLUSION
Several conclusions can be drawn by looking at the experimental results. First of all, illumination condition can vary from image to image and can greatly affect the results from both the colour segmentation step and the edge extraction step. Low resolution image is another big challenge for effective edge extraction. And the complexity of background can result in great amount of outliers in Hough transform and therefore create huge error in shape detection. Furthermore, currently the algorithm requires prior knowledge of the colour and shape of the traffic sign being detected. The algorithm will not be able to tell if the given information is incorrect. Clearly, certain limitation presents in the current detection algorithm and detection results are highly dependent on the quality of the images. Machine-learning techniques can potentially improve the detection result and should be looked into for the improvement of detection accuracy.
