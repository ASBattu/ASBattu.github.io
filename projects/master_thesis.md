---
layout: page
title: Communication in Human-Robot Collaboration - An Exploration of Gesture Recognition using the TIAGo Robot
description: >
  Masters' Thesis documentation
hide_description: false
sitemap: false
---

1. this list will be replaced by the toc
{:toc .large-only}


## Introduction

* My Masters thesis explores communication and collaboration between humans and robots in physical collaborative transportation tasks in dynamic environments.
* The study proposes using physical gestures as a means of communication to enhance collaboration during the carrying task.


* The purpose of the study is to improve task efficiency and reliability by proposing an effective communication channel, which is achieved through the gesture recognition method.
* The proposed communication channel has potential applications in industries such as healthcare, manufacturing, and logistics. Or anywhere where a Robot and a Human must interact.

### Motivation
* *Goal*: Enhance communication between humans and robots collaborating in dyanamic environments.
* *Challenges with using verbal and visual commands*:       
  * Using **verbal commands** meant that the robot must understand different accents of different people and know when a command starts and ends, which is a challenge.
  * Similarly, **visual commands** such as sign language meant that the robot must look at the human all the time during the collaboration, which might not be possible if the robot is navigating visually or object detecting, and also has privacy concerns.

* This brings us to non-verbal / non-visual commands, which were chosen to be physical - gesture recognition. The motivation for this comes from watching people move objects together.
  * **We drew inspiration from observing two people carrying an object together**. Collaborating individuals might tug or release their grip on the object to signal their partner to slow down or speed up, If one person starts lowering the object, the other might deduce that their partner is tired or that this is where the object is to be set down. Similarly, one person might lead around a corner, while the other takes the lead going through a door or up the stairs. This seamless switching between the leader and follower roles and the non-verbal cues that communicate intent to the other, where both the collaborators share equal workload and trust the other when they lead, is what makes the collaboration between the two agents a true collaboration, And was the motivation behind this study.

## The setup
The thesis was set up using the TIAGo Robot running Roscore (Robot Operating System), which was connected to a Virtual Machine running Ubuntu 16.04 on my PC. Since the thesis utilized two AI models (one for Human-Pose detection and one for Gesture-Recognition), they were run on my PC using Docker environments and communicated with the robot via RESTful API. This was necessary as the models required GPU to run, which the robot lacked. All the logic and programming for the thesis architecture were executed on the Virtual Machine using behavioral architecture design, where each behavior was written in Python.

![Image from Gyazo](https://i.gyazo.com/6f0af7bb15f6f6a4ea481670b89cbb6b.png){:.lead width="800" height="100"}
Experimental Setup
{:.figure}

## The idea
When it comes to moving an object from one place to another, there are two crucial factors to consider. The first is the destination where the object needs to be transported, and the second is the communication between the two agents responsible for carrying the object. With this in mind, the thesis is separated into two primary methods that make up the entire architecture.

The **"Point and Target"** method aims to instruct the robot where to transport an object within a given environment. Meanwhile, the **"Gesture Recognition"** method facilitates the robot's ability to recognize the pushing, pulling or other physical motions performed by a human holding the object, as well as interpret their intended meaning. 

Essentially, this thesis draws inspiration from the ways in which people move objects collaboratively, and aims to replicate that behavior using the TIAGo robot.


<hr style="border:2px solid gray">

### Point and Target Method
To communicate the desired destination of the object to the robot, one can simply stand in front of the robot and point to the location in the room. As shown through the illustration below, once the left hand is raised above the head, the robot calculates the pointing target location by tracing a line starting at the head, through the right hand to where it intersects with the ground.

![Image from Gyazo](https://i.gyazo.com/86e3b0aa841ca8eef2c0523451349afa.png){:.lead width="800" height="100"}
(a) Unedited View seen by TIAGo’s camera. (b) Pose Detection and calculation of pointed target.
{:.figure}


Video example of the method:




The step-by-step execution of this method can be seen below:

![Image from Gyazo](https://i.gyazo.com/842aa2c8e4f736b28db9c3436488e1de.png){:.lead width="800" height="100"}
Overview of the steps to successfully complete one cycle of Point and Target method
{:.figure}

* **Method Initialization**  
The main architecture calls on the Point and Target method
* **Passive TIAGo**  
While no human is in frame, TIAGo passively waits.
* **Hand Trigger Detection**  
Using HumanPose detection, TIAGo waits for the human to raise their left hand above their head. This specific gesture is used as a trigger for TIAGo to understand that the human is now pointing to the target location with their right hand.
* **Keypoint Scaling and Transformation**  
The image from the camera needs to be resized down to 256x256 pixel size dimension to detect the keypoints on the human body. The detected keypoints then need to be resized back to the original image dimensions to extract depth information from the correct points from the pointcloud.

<img align="center" src="https://i.gyazo.com/41885c6ffe03e54ce5fb7a4d9fe022cd.png" alt="drawing" width="500" height="200" style="display:block">
Illustration of the keypoint resizing process to obtain accurate coordinates
{:.figure}

* **Depth Information Extraction**  
Using the rescaled keypoints, I first align the camera image with the Pointcloud, then extract the 3D coordinates of the head and the right hand. The 3D coordinates are then converted from camera frame to map frame. If the image and the pointcloud do not align perfectly, or if there is a mismatch, a 10x10 pixel grid is calculated over the head and right hand coordinates in the pointcloud to confirm correct data extraction. The grid then returns the minimum value of distance from the camera.

<img align="center" src="https://i.gyazo.com/18918148a487f34c5ab5496f04496917.png" alt="drawing" width="850" height="200" style="display:block">
Visualization of grid overlay in pointcloud for obtaining depth measurements.  
(a) Original Pointcloud visualization           (b) 10x10 pixel grid placement illustration
{:.figure}

* **Target Point Calculation**  
From the 3D coordinates obtained in map frame, a line is dran from the two 3D points, and this line is extended downwards till it intersects with the ground. This intersection point is considered as the target location for where the object is to be carried.

<hr style="border:2px solid gray">

### Gesture Recognition Method
* The gesture recognition method establishes a communication channel between the human and the robot during the collaborative transportation.  
* The gesture recognition method works by moving the robot's arm and performing a gesture on it. The robot then executes the mapped command for each of the successfully recognized gesture. The list of gestures that can be performed are:


| Gesture           | Command - Navigation Paused | Command - Navigating                  |
|-------------------|-----------------------------|---------------------------------------|
| Up                | Start Navigation            | NA                                    |
| Down              | NA                          | Double down  - Cancel Navigation      |
| Left              | Turns left                  | NA                                    |
| Right             | Turns Right                 | NA                                    |
| Forward           | Moves Forward               | NA                                    |
| Backward          | Moves Backward              | NA                                    |
| Clockwise         | NA                          | Cancel navigation, enter passive mode |
| Counter-Clockwise | NA                          | Cancel navigation, ask for new goal   |
| Eight             | NA                          | NA                                    |
| Handshake         | NA                          | NA                                    |
| Lateral-Handshake | Pause Navigation            | Resume Navigation                     |


* **For each of the gestures, a custom dataset was created, and a custom 1D-CNN model was trained.**

Video example can be seen here:

<hr style="border:2px solid gray">

## Final Architecture