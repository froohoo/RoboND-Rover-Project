## Project: Search and Sample Return
### Project 1 For Udacity Robotics Nanodegree program

**Introduction**

The search and sample return project is based on the concept of a hypothetical rover mission. In the scenario, a rover is placed in a system of canyons along with obstacles and 'samples' that are represented by gold colored rocks. The contour of the canyon is closed (i.e. the environment is finite) and is pirmarily composed of 3 types of structures:

 * The canyon floor, which can best be described as (mostly) firm sand colored surface.
 * The canyon walls, which are typically angled away from teh floor at 10 - 20 degrees from the vertical and are primarily dark brown or black in color
 * The obsidian colored boulders placed in various areas, either as part of the canyon walls, or in the middle of the canyon floor.

The first goal of this project is to process the images from the camera on the front of the rover to both map the region and provide the navigation inputs to the rover. The second goal consists of modifying the existing rover logic so that the outputs (speed, direction) of the rover are correctly set based on the navigation inputs to succssfully navigate the canyon environment. 

Minimum criteria for success are as follows:

 1. Map 40% or more of the environment 
 2. The map must have a fidelity of at least 60% (the map may contain no more than 40% of it's pixels that do not match the source of truth)
 3. The Rover must locate at least one of the sample rocks and map it.
 
 In addition to the minimum passing requiremenents, aspirational goals may include:
 
 4. Mapping to a higher fidelity.
 5. Picking up samples as well as locating them.
 6. Returning to the starting point once all the samples are collected.
 
For this submission, the minimum criteria were met, and an attepmt was made to complete aspirational goals 4 & 5. An attempt was not made to return the rover back to its start position (#6).
 
 **Training / Calibration**

As mentioned above, the first goal for this project is to process the impages for the front facing camera to extract mapping and navigational data. To do this, the following processing steps are applied to each image from the rover camera:

 ***Perform a perspective transform***
 
 The default image read in from the rover telemetry is taken from the POV of the front of the rover. To use the image for mapping the image would ideally be an isometric overhead projection. The OpenCV image processing library has two functions that can be utilized to transform the raw rover image into an isometric projectcion. The first function,[getPerspectiveTransform](https://docs.opencv.org/2.4/modules/imgproc/doc/geometric_transformations.html#getperspectivetransform) is used to determine the perspective transform, given 4 sets of coordinates that identify the same spatial locations in both the dessired source and destination.  image. Once the perspective transform is obtained, it can then be used to transform the images fed in from the rover telemetry using tje [warpPerspective function](https://docs.opencv.org/2.4/modules/imgproc/doc/geometric_transformations.html#warpperspective). An example of how this conceptually works (not to scale) is shown below:
 ![Perspective Transform](mapping.png)
 
 ***Color Thresholding***
 
 Once the transformed (warped) image is obtained, the next step is to successfully determine the navigable / unavigable areas of the canyons. The approach used here was to use an rgb threshold to identify navigable pixels since the canyon floors are larglely a light color and the unavigable terrain is mostly dark in coloring. Good results were returned when using thresholds for red, green, blue of (190,180,160). The un-navigable terrain was assumed to be all the pixels that were not above the threshold. For identifying the gold sample rocks a slighly different approach needed to be used since a simple threshold could not be used to identify specific colors. This was addressed by adding the functionality to the color_thresh function to filter on pixels that were within a tolerance of a given color value. For the gold rocks rgb values of (185, 140, 15) with a tolerance of +/- 40 for each value to return true as a match. 
 
 ***Contours (Not part of core submission requirments)***

In an attempt to make the rover a wall crawler it seemed that one approach would be to locate the interface between the canyon wall and the sandy floor and use that for guidance. To that end functionality to detect contours was added to the perception module. The contours are extracted from the image using the OpenCV function [findContours](https://docs.opencv.org/2.4/modules/imgproc/doc/structural_analysis_and_shape_descriptors.html#findcontours). Since the result contains multiple contours, the largest one is extracted as one most likely to be of interest for wall navigation. Once this is complete a final step of masking off parts of the contour immediately to the right of the rover is performed. The a sample result of the contours, overlayed on the source image is shown below. The blue section represents to portion of the contour used for navigating along the wall.[Contour Image](contour.png)
