# (Auto)pilot: Multimodal Human-Robot Interaction for Autonomous Vehicles

## Overview
This project implements a shared-autonomy autonomous vehicle system that allows a human to guide an autonomous car using gesture and voice commands while the vehicle continues performing navigation, perception, and control tasks.

The system integrates computer vision, speech recognition, gesture recognition, and graph-based navigation into a real-time control pipeline. Instead of switching between fully manual and fully autonomous driving modes, the system allows humans to provide high-level guidance while the autonomous system maintains control of low-level driving behavior.

The project was implemented and evaluated in the Webots robotics simulator using a Tesla car with a front camera.

## Features
The system includes several major components:

*Autonomous Vehicle - Navigation*
- Graph-based road network representation
- A* path planning for route generation
- Waypoint-based lane following
- Lane-aware navigation rules

*Autonomous Vehicle - Perception*
- Lane detection using OpenCV + HSV color segmentation
- Obstacle detection using MediaPipe EfficientDet
- Rule-based obstacle avoidance and lane changes

*Human Interaction*
- Gesture recognition using MediaPipe Gesture Recognizer
- Voice commands using SpeechRecognition and Google STT

Supported commands include:

Gesture Commands
- Closed fist → stop vehicle
- Open hand → resume driving
- Thumbs up → left lane change
- Thumbs down → right lane change

Voice Commands
- “faster” → increase speed by 1 m/s
- “slower” → decrease speed by 1 m/s
- “left” → turn left at intersection
- “right” → turn right at intersection

*Shared Autonomy*

Human commands modify the behavior of the autonomous system without overriding safety constraints such as:
- lane boundaries
- obstacle avoidance
- speed limits

## System Architecture

The system consists of two main subsystems:

*Autonomous Vehicle Subsystem*

Responsible for perception, navigation, and vehicle control.
Components include:
- road graph representation
- A* navigation planner
- waypoint generation
- lane detection
- obstacle detection and avoidance

*Human Interaction Subsystem*
Handles gesture and voice input.
Components include:
- MediaPipe gesture recognition pipeline
- speech recognition interface
- command queue for asynchronous communication

Concurrency Model
Gesture recognition and speech recognition run in separate threads, while the main control loop handles:
- sensor updates
- perception processing
- command integration
- vehicle control

Synchronization primitives such as locks and queues ensure safe communication between threads.

## Repository Structure
```
├── worlds/
│   └── city.wbt                  
├── controllers/
│   └── car_controller/
│       └── controller.py         
├── requirements.txt             
└── README.md
```
The two ML model files are not included — they are downloaded automatically to the models directory on first run:
```
models/
├── gesture_recognizer.task       
└── efficientdet_lite0.tflite    
```

## Installation
1) Download Webots R2025a from https://cyberbotics.com/. Other versions of Webots have not been tested for this project.
2) Clone the repository
```
git clone https://github.com/anvithasm/cs188-autopilot
cd cs188-autopilot
```
3) Install dependencies for this project
