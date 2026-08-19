---
title: "Coding Period Week 9 (July 24 ~ July 30)"
date: 2026-07-29 10:00:00 +0530
categories: [GSoC 2026, Progress]
tags: [gsoc, coding-period, week-9]
published: true
---

Coming off the massive backend storage rewrite in Week 8, this week was dedicated to stabilizing the new architecture, rigorous testing, and finally wrapping up the delayed visual showcase for the Obstacle Avoidance block!

## 🎯 The Main Goals for the Week
1. Complete the video editing and polish the showcase video for the Obstacle Avoidance exercise (which was pushed from last week).
2. Stabilize the UI and thoroughly test the new Django backend integration to ensure no edge cases exist when loading local blocks.

## 🎬 Showcasing Obstacle Avoidance
Since the entire previous week was consumed by the Django backend refactor, I finally had the time this week to sit down and properly edit the Obstacle Avoidance video. I made sure to highlight the parallel processing logic and how the block dynamically processes sensor data to steer the robot away from walls. 

It took some time to get the cuts perfectly aligned with the VisualCircuit theme, but the final video clearly demonstrates the power of the new custom blocks!

<!-- TODO: Attach the final Obstacle Avoidance showcase video (or thumbnail photo) here -->

## 🧪 Testing the New Storage Architecture
The second half of the week was spent rigorously testing the new `POST` and `GET` requests we built for the Marketplace. I wanted to ensure that when a user downloads a block, the `.vc3` file is perfectly saved in the `backend/custom_blocks/` folder, and more importantly, perfectly reloaded when the application restarts.

I found and ironed out a few minor edge cases in how the React frontend parses the incoming JSON from the Django server. The system is now incredibly stable—custom blocks feel completely native to the editor!

<!-- TODO: Attach a screenshot of the stabilized UI / downloaded blocks here -->

## 🤝 Mentor Sync
We met briefly at the end of the week to confirm the storage architecture was stable and to begin planning the next major hurdle: handling unsupported libraries in custom blocks.

<!-- TODO: Attach Mentor Meeting Photo here -->
