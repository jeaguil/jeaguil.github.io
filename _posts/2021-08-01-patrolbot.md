---
layout: post
title: "PatrolBot - Undergraduate capstone project"
author: "Jesus Aguilera"
tags: [sample]
work-interval: August 2021 - Present
---
__By: Jesus Aguilera, Brandon Banuelos, Connor Callister, Max Orloff, Michael Stepzinski__

The PatrolBot project is about creating a surveillance robot that will provide an essential
tool for campus police at the university of Nevada, Reno. The goal is for the robot to scout bike racks around campus to look for any elements of
bike theft. Governed by object detection models, threat computation algorithms and a back-end web server,
the robot will help alleviate some pressure on campus police.

---
![](assets/img/patrolbot-architecture.png)
<center>Figure 1: Project Architecture</center>

Figure 1 shows the conceptual model of the PatrolBot structure and describes a
semi-formal representation of all integrated aspects. The system utilizes an on-premises robot with a camera, a web application, and machine learning models all connected through the internet. 

### Web Server
The web application provides a centralized hub for all project pieces and intends to provide streamline communication between the user and all system aspects. Backed by the Django web framework and hosted using AWS Elastic Beanstalk, the web server includes the camera feed sent from the robot to the website through AWS Kinesis as well as the GPS coordinates and movement requests through AWS IoT. The web site also contains past recordings / captured frames of maliciously individual behavior for past viewing.

### Robot
The robot is governed by Ubuntu 18.04 and uses ROS melodic as well as the AWS Python SDK to provide the needed information. The robot's hardware consists of:
* 4WD Rover Zero 3 Robot
* Raspberry Pi 4 that serves as the robot controller for processing local communication to the web server
* Raspberry Pi 3B+ for camera communication
* Stemedu VK172 - GPS Module
* Charmcast 10000mAH - Portable Battery to power both Raspberry Pis

### Machine Learning
##### Object Detection Model
A custom object detection model utilizing YOLOv5 architecture to detect:
* People
* Bikes
* Malicious objects like angle grinders and bolt cutters

We utilized Roboflow to create a custom dataset that consists of bike-theft scenarios with the objects listed above as primary interests and performed image augmentation.

##### Action Detect Model
Inflated 3D convolutional neural network was trained on a dataset containing
normal crowd behavior vs aggressive individual behavior. The intention is to classify vigorous behavior that needs to be immediately addressed a security official.

##### Threat Detection Algorithm
An algorithm is used to compute potential threats to notify a user if a bike theft is likely in progress. This algorithm utilizes the fundamentals of intersection-over-union of bounding boxes provided by the object detection model to determine if a potential threat exceeds a certain threat threshold.