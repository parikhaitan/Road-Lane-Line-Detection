# Road-Lane-Line-Detection
Using Hough Transformation
A CCD camera is fixed on the front-view mirror to capture the road scene. 

To simplify the problem, the baseline is assumed to be setup as horizontal, which assures the horizon in the image is parallel to the x-axis. Otherwise, the image of the camera can be adjusted using the calibration data. Each lane boundary marking, usually a rectangle (or approximate) forms a pair of edged lines.
In this documentation, it was assumed that the input to the algorithm was a 620x480 RGB colour image. Therefore, the algorithm works to convert the image to a grayscale image in order to minimize the processing time. Secondly, in presence of noise, the image will hinder the correct edge detection. Hence, F.H.D algorithm Mohamed Roushdy (2007) was applied to make the edge detection more accurate. Then the edge detector was used to produce an edge image by using canny filter with an automatic thresholding to obtain the edges. It has reduced the amount of learning data required by simplifying the image edges considerably. Then edged image has been sent to the line detector which produces a right and left lane boundary segment.

The projected intersection of these two-line segments was determined and was referred to as the horizon. The lane boundary scan used the information in the edge image detected by the Hough transform to perform the scan. The scan returned a series of points on the right and left side. Finally, pair of hyperbolas were fitted to these data points to represent the lane boundaries. For visualization purposes the hyperbolas are displayed on the original colour image.

 Image Capturing:
The input data was a colour image sequences taken from a moving vehicle. A colour camera was mounted inside the vehicle at the front-view mirror along the central line. It took the images of the environment in front of the vehicle, including the road, vehicles on the road, roadside, and sometimes incident objects on the road. The on-board computer with image capturing card captured the images in real time (up to 30 frames/second), and saved them in the computer memory. The lane detection system read the image sequences from the memory and started processing. A typical scene of the road ahead is depicted.
 ![image](https://user-images.githubusercontent.com/104994429/207421904-14b5d682-5c61-45e1-a9d7-0f97ff79edce.png)

 
Conversion to Gray Scale:
To retain the colour information as well as to segment the road from the lane boundaries using the colour information, edge detection becomes difficult and consequently effects the processing time. In practice the road surface can be made up of many different colours due to shadows, pavement style or age, which causes the colour of the road surface and lane markings to change from one image region to another. Therefore, colour image was converted into grayscale. However, the processing of grayscale images became minimal as compared to a colour image. This function transformed a 24-bit, three- channel, colour image to an 8-bit, single channel grayscale image.
![image](https://user-images.githubusercontent.com/104994429/207421983-c376c796-c0cf-4311-8fa7-76bb81367a8e.png)

Noise Reduction: 
Noise is a real-world problem for all systems including computer vision processing. The developed algorithms must either be noise tolerant or the noise must be eliminated. As presence of noise in proposed system will hinder the correct edge detection. Hence noise removal is a pre requisite for efficient edge detection with the help of F.H.D. algorithm by Mohamed Roushdy, (2006) that removed strong shadows from a single image. The basic idea was that a shadow has a distinguished boundary. 
Hence, removing the shadow boundary from the image derivatives and reconstructing the image was applied. A shadow edge image has been created by applying edge detection on the invariant image and the original image. By selecting the edges that exist in the original image but not in the invariant image and to reconstruct the shadow free image by removing the edges from the original image by using a pseudo-inverse filter has been implemented.
![image](https://user-images.githubusercontent.com/104994429/207422041-f0e503ca-232d-432b-a421-936dcfed1f78.png)

Edge Detection: 
Lane boundaries are defined by sharp contrast between the road surface and painted lines or some types of non-pavement surfaces. These sharp contrasts are edges in the images. Therefore, edge detectors are very important in determining the location of lane boundaries. It also reduces the amount of learning data required by simplifying the image considerably, if the outline of a road can be extracted from the image. The edge detector was implemented for this algorithm. The one that produced the best edge images from all the evaluated edge detectors was the ‘canny’ edge detector.
It was important to have the edge detection algorithm that could be able to select thresholds automatically. However, the automatic threshold used in the default Canny Algorithm produced edge information that is far from actual threshold. A slight modification to the edge detection in canny has produced more desirable results. The only changes necessary were to set the amount of non-edge pixels of the highest and lower thresholding to the best value that has provided more accurate edges in different conditions of image capturing environment. 
![image](https://user-images.githubusercontent.com/104994429/207422104-7da144eb-cc60-4a68-aaca-7daedc302c88.png)

Canny Edge Detection method:
Canny Edge Detection is a popular edge detection algorithm. It was developed by John F. Canny. It is a multi-stage algorithm and we will go through each stage.

1.	Noise Reduction: Since edge detection is susceptible to noise in the image, first step is to remove the noise in the image with a 5x5 Gaussian filter.
2.	Finding Intensity Gradient of the Image: Smoothened image is then filtered with a Sobel kernel in both horizontal and vertical direction to get first derivative in horizontal direction (Gx) and vertical direction (Gy). From these two images, we can find edge gradient and direction for each pixel as follows:
3.	
  Gradient direction is always perpendicular to edges. It is rounded to one of four angles representing vertical, horizontal and two diagonal directions.
Line Detection:
The line detector used is a standard Hough transform Chen and Wang, (2006) with a restricted search space. In reality, any line that falls outside a certain region can be neglected. The image obtained from the region of interest will mask the canny image (derivative). It will be used to only show a specific portion of the image. We create a mask image with same dimensions and fill a part of its area with triangle.
Furthermore, bitwise AND operation is performed between each pixel of our canny image and this mask. It ultimately masks the canny image and shows the region of interest traced by the polygonal contour of the mask. We apply bitwise & operation between the two images.
![image](https://user-images.githubusercontent.com/104994429/207422353-df029d75-6869-4d72-939f-62070c78d2c5.png)

Hough Transform:
 In the Cartesian plane (x and y-axis), lines are defined by the formula y=mx+b, where x and y correspond to a specific point on that line and m and b correspond to the slope and y-intercept.
 
A regular line plotted in the Cartesian plane has 2 parameters (m and b), meaning a line is defined by these values. Also, it is important to note that lines in the Cartesian plane are plotted as a function of their x and y values, meaning that we are displaying the line with respect to how many (x, y) pairs make up this specific.

However, it is possible to plot lines as a function of its m and b values. This is done in a plane called Hough Space. To understand the Hough Transform algorithm, we need to understand how Hough Space works.

Explanation of Hough Space:
In our use case, we can sum up Hough Space into two lines
•	Points on the Cartesian plane turn into lines in Hough Space
•	Lines in the Cartesian plane turn into points in Hough Space

Think of the concept of a line. A line is basically an infinitely long grouping of points arranged orderly one after the other. Since on the Cartesian plane, we’re plotting lines as a function of x and y, lines are displayed as infinitely long because there is an infinite number of (x, y) pairs that make up this line. Now in Hough Space, we’re plotting lines as a function of their m and b values. And since each line has its only one m and b value per Cartesian line, this line would be represented as a point.

Output:

 ![image](https://user-images.githubusercontent.com/104994429/207422529-6aae4403-81d7-4deb-8662-c43e600cf61d.png)
