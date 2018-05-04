Stereo Visual Odometry

## What is Visual Odometry?

Visual Odometry is the estimating the motion of a camera in real time using sequential images. The output that is obtained from a visual odometry algorithm are the 6 degrees of freedom of the moving object. The idea was first introduced for planetary rovers operating on Mars – Moravec 1980.

### VSLAM

Visual Odometry is used as a building block of a larger problem known as Simultaneous Localization and Mapping (SLAM). The goal of SLAM is to obtain a global, consistent estimate of the camera's path. SLAM finds loop closures in the paths generated by Visual Odometry and adjusts the drifts in the path.

### Applications of Visual Odometry

* The most important application of Visual Odometry is prediction of the trajectory of a moving robot/vehicle in uneven or slippery terrain. In uneven and slippery terains, wheels tend to slip and in such scenarios, wheel rotation calculations become unrealiable and visual odometry algorithms are used to give more accurate estimates of motion.

* Motion estimation for vehicles 
  * HD mapping
  * Autonomous cars

* Possible use in AR/VR applications

### Challenges 

* Robustness to lighting conditions
* Lack of features / non-overlapping images 
* Without loop closure the estimate still drifts

### Types of Visual Odometry

* Monocular Visual Odometry 
  * A single camera is used.
  * Estimates are relative.
* Stereo Visual Odometry
  * In Stereo Visual Odometry, a left and right camera are used.
  * Estimates are absolute.
* Augmented Stereo Visual Odometry
  * Visual Odometry can be augmented by using data from various sensors such as Lidar, Time of flight data, RGB-Depth and GPS.

## The Dataset

* The dataset we used was the KITTI Vision Benchmark Suite dataset by KIT, Germany. 
* The dataset included undistorted and stereo rectified grayscale and color image data along with LIDAR laser data.
* The calibration camera projection matrices are provided.
* The output poses for each frame are with respect to the 0th image in the sequence

## Our Approach

Our approach consists of the following steps.

### Feature Detection
 
We used the FAST (Features from Accelerated Segment Test) corner detection method for feature detection. 
* To ensure that the features we got from the FAST algorithm were spread out and not concentrated in a certain region, we used   feature bucketing in which we divide our image into a grid and only take a ------------------ number of features from each     part of the grid.

### Feature Matching
 
For feature matching between images at time t and t+1, we used Kanade–Lucas–Tomasi feature tracker with a search window size of 15x15 and 3 pyramid levels. To remove noise, we also did some pruning at this stage.

### 3D Point Triangulation

----------
Corresponding feature points (u,v) from left and right image
Disparity displaced right image coordinate
Least square minimization on system of equations for (𝑋𝑤) ⃗, solved using SVD
-----------

### Inlier Detection

We used the Inlier Detection method to find the largest subset of consistent matches. We defined a match as a pair of points in which the distance between the points was the nearly the same in the images at time t and t+1. 

### Motion Estimate

We used the Levenberg-Marquardt least squares estimation to minimize the re-projection error which was expresses as e= ∑((𝑗𝑎 −𝑃Δ𝑤𝑏)^(2)+(𝑗𝑏 −𝑃Δ^(−1) 𝑤𝑎)^(2)).

