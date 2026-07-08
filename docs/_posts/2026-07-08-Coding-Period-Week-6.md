---
title: "Coding Period Week 6 (July 3 ~ July 9)"
date: 2026-07-08 10:00:00 +0530
categories: [GSoC 2026, Progress]
tags: [gsoc, coding-period, week-6]
published: true
---

This week brought a mix of content creation, exploring brand-new exercises, and refining the existing Marketplace architecture based on detailed mentor feedback!

## 🎯 The Main Goals for the Week
- [x] Submit the final, polished showcase video for the Laser Mapping exercise.
- [x] Implement corrections on the pushed Pull Requests based on mentor review comments.
- [x] Explore and try implementing two new exercises: **Obstacle Avoidance** and **Follow Person**.

## 🎬 Video Editing & Showcasing Laser Mapping
A significant chunk of my time this week went into producing the final showcase video for the Laser Mapping exercise. It was my very first time diving deeply into video editing! It took a lot of time to cut, arrange, and edit everything to properly match the VisualCircuit theme, but the final result perfectly captures the parallel processing blocks and the robot in action.

![Tags UI](/assets/img/posts/Coding_Period_Week6/Ed1.png)
![Tags UI](/assets/img/posts/Coding_Period_Week6/Ed2.png)

## 🤖 Exploring New Exercises: Obstacle Avoidance
After finishing the video, I moved on to the next phase of development: evaluating the new exercises.

I first attempted the **Follow Person** exercise. However, after exploring the required logic and the current environment capabilities, I faced several roadblocks and was not able to successfully apply it within the current setup just yet. 

I then pivoted to the **Obstacle Avoidance** exercise. I spent time studying the foundational logic, successfully implemented it, and ran a full suite of tests. It started working flawlessly! I also spent time exploring the broader VisualCircuit codebase to map out what supplementary improvements can be made alongside these new exercises.

![Tags UI](/assets/img/posts/Coding_Period_Week6/obs.png)

## 🛠️ Mentor Feedback & Architecture Refinement
The other major task this week was addressing mentor feedback on the Marketplace PRs. Strangely, a GitHub bug caused the mentor's review comments to not be visible to me, so I had to rely on screenshots to read their feedback! 

Once I got the feedback, I made the following corrections in the `VisualCircuit-resources` repository on a new `fix/mentor-feedback` branch:

### 1. Updating the Python Validator (`validator.py`)
- **Removed Allowed Categories Check:** Previously, the validator restricted block categories to a strict hardcoded list ("Computer Vision", "Control Systems", etc.). I removed this list entirely. The validator now simply ensures that the category string is not empty.
- **Removed Import Blacklist Check:** Previously, the validator used Python's Abstract Syntax Tree (AST) to check if custom code blocks imported restricted modules like `os` or `subprocess`. I removed this security AST check completely to allow more flexibility.
- **Cleaned Logs:** Removed the  emoji prefix from the validation crash logs to keep the terminal output purely plain-text.

### 2. Deleting Test Blocks
Per the mentor's request (*"Please delete both of these blocks, we should only have it for local testing, not uploaded"*), I deleted `Untitled (1).vc3` and `test2_final.vc3` from the `custom_blocks/` directory. 

### 3. Cleaning the Global Registry
Since the test blocks were deleted, I re-ran the registry generator script. This compiled a clean `registry.json` index with an empty blocks array (`[]`), leaving a perfect blank slate for the community!
- [PR: fix/Mentor-feedback](https://github.com/JdeRobot/VisualCircuit-resources/pull/15)

## 🤝 Mentor Meeting
We discussed the progress on Obstacle Avoidance and confirmed the validator fixes.

![Tags UI](/assets/img/posts/Coding_Period_Week6/Meet.png)
