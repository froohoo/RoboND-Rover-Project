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
 
 The default image read in from the rover telemetry is taken from the POV of the front of the rover. To use the image for mapping the image would ideally be an isometric overhead projection. The OpenCV image processing library has two functions that can be utilized to transform the raw rover image into an isometric projectcion. The first function,[getPerspectiveTransform](https://docs.opencv.org/2.4/modules/imgproc/doc/geometric_transformations.html#getperspectivetransform) is used to determine the perspective transform, given 4 sets of coordinates that identify the same spatial locations in both the dessired source and destination.  image. Once the perspective transform is obtained, it can then be used to transform the images fed in the rover telemetry to ana  An example of how this conceptually works (not to scale) is shown below:
 ![Perspective Transform][mapping.png]
