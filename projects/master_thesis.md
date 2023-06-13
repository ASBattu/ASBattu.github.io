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


# Introduction

* My Masters thesis explores communication and collaboration between humans and robots in physical collaborative transportation tasks in dynamic environments.
* The study proposes using physical gestures as a means of communication to enhance collaboration during the carrying task.


* The purpose of the study is to improve task efficiency and reliability by proposing an effective communication channel, which is achieved through the gesture recognition method.
* The proposed communication channel has potential applications in industries such as healthcare, manufacturing, and logistics. Or anywhere where a Robot and a Human must interact.

## Motivation
* *Goal*:  
 Enhance communication between humans and robots collaborating in dynamic environments.
* *Challenges with using verbal and visual commands*:       
  * Using **verbal commands** meant that the robot must understand different accents of different people and know when a command starts and ends, which is a challenge.
  * Similarly, **visual commands** such as sign language meant that the robot must look at the human all the time during the collaboration, which might not be possible if the robot is navigating visually or object detecting, and also has privacy concerns.

* This brings us to non-verbal / non-visual commands, which were chosen to be physical - gesture recognition. The motivation for this comes from watching people move objects together.
  * **We drew inspiration from observing two people carrying an object together**. Collaborating individuals might tug or release their grip on the object to signal their partner to slow down or speed up, If one person starts lowering the object, the other might deduce that their partner is tired or that this is where the object is to be set down. Similarly, one person might lead around a corner, while the other takes the lead going through a door or up the stairs. **This seamless switching between the leader and follower roles and the non-verbal cues that communicate intent to the other**, where both the collaborators share equal workload and trust the other when they lead, is what makes the collaboration between the two agents a true collaboration, And was the motivation behind this study.

# The setup
The thesis was set up using the TIAGo Robot running Roscore (Robot Operating System), which was connected to a Virtual Machine running Ubuntu 16.04 on my PC. Since the thesis utilized two AI models (one for Human-Pose detection and one for Gesture-Recognition), they were run on my PC using Docker environments and communicated with the robot via RESTful API. This was necessary as the models required GPU to run, which the robot lacked. All the logic and programming for the thesis architecture were executed on the Virtual Machine using behavioral architecture design, where each behavior was written in Python.

![Image from Gyazo](https://i.gyazo.com/6f0af7bb15f6f6a4ea481670b89cbb6b.png){:.lead width="800" height="100"}
Experimental Setup
{:.figure}

# The idea
When it comes to moving an object from one place to another, there are two crucial factors to consider. The first is the destination where the object needs to be transported, and the second is the communication between the two agents responsible for carrying the object. With this in mind, the thesis is separated into two primary methods that make up the entire architecture.

The **"Point and Target"** method aims to instruct the robot where to transport an object within a given environment. Meanwhile, the **"Gesture Recognition"** method facilitates the robot's ability to recognize the pushing, pulling or other physical motions performed by a human holding the object, as well as interpret their intended meaning. 

Essentially, this thesis draws inspiration from the ways in which people move objects collaboratively, and aims to replicate that behavior using the TIAGo robot.


<hr style="border:2px solid gray">

## Point and Target Method
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

<p align="center" width="100%">
    <img width="70%" src="https://i.gyazo.com/41885c6ffe03e54ce5fb7a4d9fe022cd.png">
</p>
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

## Gesture Recognition Method
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


* **For each of the gestures, a custom dataset was created, cleaned, augmented and split through which a custom 1D-CNN model was created, trained and compared with other architectures to find the best performing model.**

Video example can be seen here:



## TIAGo robot's Arm, It's Controller and the 'Jiggle'

In this thesis, the gesture to be recognized is performed on the Robotic arm itself, moving away from any requirements of an interface or screen, integrating direct physical forces as the command.

The robotic arm is controlled by two types of controllers:

- **Gravity Compensation Controller** counteracts gravity’s impact on the robotic arm. By using torque to measure the force being applied on the arm, the robotic arm can be moved around by touch or simply by pushing it, allowing the human to perform the gesture.
- **Positional Controller** is a type of controller that allows the robot arm to be positioned at specific angles or joint configurations. The robotic arm cannot be moved around by external forces while using this controller.

We combine both controllers in this study, the gravity compensation controller enables
the robot arm to be moved by external forces without positional commands,
while the positional controller permits the robot arm to be positioned to specific
angles or joint configurations. Therefore, a controller is needed to switch between
the two arm controllers.

### The 'Jiggle'
To help the robot switch between controllers smoothly, a simple abnormality detector is developed. It measures the torque of three joints on the robot's arm and looks for any sudden forces, which we call "Jiggles". By comparing the previous ten torque values, deviations beyond a certain threshold are detected as abnormalities, indicating a "jiggle" has occurred.


<p align="center" width="100%">
    <img width="50%" src="https://i.gyazo.com/4058360d7b0a4722542554441f706ea8.png">
</p>
Experimental Setup
{:.figure}

At timestep 30, an external force affected the arm, causing deviations in the torque values. This triggers the robot to switch to gravity compensation mode for two seconds.

[INSERT VIDEO HERE]



<hr style="border:2px solid gray">
# Final Architecture

<p align="center" width="100%">
    <img width="100%" src="https://i.gyazo.com/4988b9a4eff483669fd2db984f253142.png">
</p>
Flowchart showcasing step-by-step execution of the complete architecture
{:.figure}

The complete architecture for the thesis can be viewed above. Although a lot of steps, a brief explanation is given below:

* **Initial state**:   
  TIAGo is in a passive state at its designated home position, facing the environment.

* **Tracking and Pointing**:
  * TIAGo initiates tracking of the human's left hand and head.
  * Human points to the object's target location using their right hand.
  * Human raises their left hand over their head to signal the robot to calculate the target location.

* **Calculation and Switching**:  
  * If pointing to a valid target:  
    * TIAGo calculates the target location and prepares to navigate towards it
  * If pointing to an invalid target (Pointing above):
    * TIAGo switches to follower mode and waits for further instructions through gesture recognition.

* **Object handling**: 
  * Human holds one end of the object, robot holds the other end with its gripper.

* **Gesture Recognition and Communication**:
  * Human can perform specific gestures on the robotic arm through the object held to communicate with the robot.

* **Navigation and Interaction**:
  * Human's gesture first indicates readiness to move
  * The robot starts leading towards the target goal, with the human simply following.
  * During this navigation, the human can pause, change direction, or give alteranitive instructions through gestures.

* **Reaching the navigational goal**: 
  * Upon reaching the goal, the robot switches to follower mode.
  * Human can now instruct the robot to put down the object and return home or move it around to adjust its position.

* **Repeat**: 
  * On returning home, the process repeats for the next object to be carried.


<hr style="border:2px solid gray">
# Scenarios successfully completed

* Perform gesture directly on the robotic arm, no object present
*  Perform gestures while coupled to the robot through the object.
*  Move the object from point A to point B.
*  Move the object from point A to point B while moving around an obstacle.
*  Move the object from point A to point B while moving around an obstacle, but change direction midway, asking the robot to change its path to move around the other side of the obstacle.
*  Move the object from point A to point B, but the human operator drops the
object mid navigation
*  Move the object from point A to point B, but mid navigation, choose to navigate to a new point, point C.