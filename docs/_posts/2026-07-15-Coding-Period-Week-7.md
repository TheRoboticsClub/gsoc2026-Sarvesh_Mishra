---
title: "Coding Period Week 7 (July 10 ~ July 16)"
date: 2026-07-15 10:00:00 +0530
categories: [GSoC 2026, Progress]
tags: [gsoc, coding-period, week-7]
published: true
---

This week was heavily focused on complex block architecture for the Obstacle Avoidance exercise and refining the core VisualCircuit editor features, specifically around block handling, downloading, and local storage.

## 🎯 The Main Goals for the Week
- [x] Make the final showcase video for the Obstacle Avoidance exercise.
- [x] Allow multiple downloads of the same block into the Downloads palette.
- [x] Improve and address issues raised by the mentor in both the `VisualCircuit` and `VisualCircuit-resources` repositories.
- [ ] Improve the storage of downloaded blocks (Currently exploring local file storage architecture).

## 🏎️ Engineering the Obstacle Avoidance Architecture
I spent the majority of my time this week designing and debugging the blocks for the Obstacle Avoidance exercise. 

I initially started with the original, working Robotics Academy code as a single, massive block where laser reading, odometry, target tracking, force calculation, and motor commands were all bundled together. That worked perfectly because everything ran in a single loop using HAL and WebGUI.

I then tried to split it into 3 blocks (Sensor Input, Decision/Force Calculation, Motor Output), but I immediately ran into the parallel execution issue again—VisualCircuit blocks run simultaneously, not sequentially, so data passing through the wires has to be perfectly timed.

### The Final 7-Block Structure
To create a truly robust and modular system, I expanded the architecture into a **7-block structure**:
1. **ROS2LaserScan1001:** Reads `/f1/laser/scan`
2. **ROS2LaserScan1002:** Reads `/odom`
3. **ROS2Camera2001:** Reads `/webgui/current_target`
4. **Code_1:** Calculates the target attraction force.
5. **Code_2:** Calculates the obstacle repulsion force.
6. **Code_3:** Mixes both forces into a final velocity vector.
7. **ROS2MotorDriver1001:** Publishes the final output to `/cmd_vel`.

## 🧗 Issues Faced
During the development of the Obstacle Avoidance blocks, I faced several frustrating physics and communication issues:
- **Block Timing & Ports:** Wrong port connections and blocks not sharing values at the exact right time due to the parallel execution.
- **Unstable Physics:** Unstable steering, extreme deviations near obstacles, and the robot either over-turning or under-turning. 

**The Solutions:** 
I fixed these by rigorously checking the live ROS topics, precisely matching the VisualCircuit input/output names, and manually tuning the obstacle force and steering gains. Finally, I added a specific "front-distance" logic so the robot intelligently slows down and turns harder when an obstacle is directly dead ahead.

![Tags UI](/assets/img/posts/Coding_Period_Week7/Video.png)
![Tags UI](/assets/img/posts/Coding_Period_Week7/Video1.png)
![Tags UI](/assets/img/posts/Coding_Period_Week7/Video3.png)
- [Block_File](https://github.com/Sarvesh-Mishra1981/VC_Gsoc-Codes/tree/main/Obstacle_Avoid)

## 🛠️ VisualCircuit Core Improvements & Mentor Feedback

### 1. Removing the Wire Constraint
Based on discussions and feedback from my mentor, I addressed an annoying constraint in the VisualCircuit editor: previously, no single block could exist without being attached to wires. I successfully removed this restriction, allowing blocks to be placed freely on the canvas without immediately requiring wire attachments!

- [PR: fix/Mentor-feedback](https://github.com/JdeRobot/VisualCircuit-resources/pull/15)
- [PR: fix/Mentor-feedback](https://github.com/JdeRobot/VisualCircuit/pull/471)

### 2. Allowing Multiple Downloads
A major UI issue with the Marketplace was that you could only download a block once. If you wanted multiple instances of the same custom block, you were stuck. I implemented a counter system that now successfully tracks and allows multiple downloads of the exact same block from the Marketplace into your local Downloads folder!

### 3. The Local Storage Architecture (Work in Progress)
I have been working on improving how downloaded blocks are stored. My code is currently ready to store the downloaded blocks directly into a local file within VisualCircuit. 

However, I hit an architectural crossroads that I need to resolve: *When we populate the Downloads sidebar, do we fetch from the original Marketplace endpoint, or do we fetch from the new Local Storage?* 
If we fetch from Local Storage, I will need to engineer a new local registry system just for tracking locally stored files. I plan to discuss this with my mentors to finalize the best approach!

## 🤝 Mentor Meeting
We had a highly productive sync this week to review the massive architectural changes. We discussed the logic behind removing the wire constraints in the UI, reviewed the new multiple-download tracking, and had an in-depth conversation about the Local Storage crossroads to determine our final path forward for the registry fetch system!

![Tags UI](/assets/img/posts/Coding_Period_Week7/Meet.png)
