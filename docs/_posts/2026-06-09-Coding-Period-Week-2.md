---
title: "Coding Period Week 2 (June 5 ~ June 11)"
date: 2026-06-09 10:00:00 +0530
categories: [GSoC 2026, Progress]
tags: [gsoc, coding-period, week-2]
published: true
---

This week was incredibly exciting and challenging as I focused entirely on designing, building, and deploying the VisualCircuit Marketplace.

## 🎯 The Main Goals for the Week
The overarching goal was to bridge the gap between local block creation and global community sharing. Specifically, my objectives were to:

- [x] Introduce categorized grouping into the Marketplace so blocks are easy to find.
- [x] Build a Download & Install system that saves blocks locally and groups them inside the editor's left sidebar palette by category.
- [x] Completely redesign the architecture by shifting the block library and registry out of the frontend and into a dedicated `VisualCircuit-resources` repository.
- [x] Create an automated Python Validator inside the resources repository to securely test community blocks.
- [x] Successfully merge the frontend and backend systems, run end-to-end testing, and prepare clean Pull Requests for my mentors.
- [ ] Finalize the Laser Mapping block integration in VisualCircuit (I was not able to complete this issue yet, so it is pushed to next week).

Here is a breakdown of how I achieved this and the major features I implemented:

## 🚀 Progress & Major Features Implemented

### 1. The Block Metadata Engine
Before blocks could be displayed in a categorized marketplace, they needed a way to identify themselves. I updated the core `.vc3` save system and the "Project Info" dialog UI in the frontend. Users can now assign a Category (e.g., Computer Vision, Locomotion) and custom Tags to their blocks. When a user clicks "Save As", this new metadata is permanently embedded directly into the JSON structure, making it readable by the marketplace backend.

![Tags UI](/assets/img/posts/Coding_Period_Week2/Tags.png)

### 2. Shifting to the Cloud & CI/CD Security
To host the marketplace, I shifted the registry architecture to the `VisualCircuit-resources` repository to act as our cloud backend. Instead of relying on a static database, I utilized GitHub Actions to create an automated CI/CD pipeline!

- **The 3-Level Validator:** I wrote a Python script (`validator.py`) that tests any block submitted via a Pull Request. It checks Level 1 (Metadata), Level 2 (Structural Wiring), and Level 3 (A Python Security Firewall that scans for malicious code imports like `os` or `subprocess`).
- **Auto-Publishing:** Once a Pull Request passes validation and is merged, a GitHub Action automatically triggers `update_registry.py`. This script scans the `blocks/` directory, extracts all metadata, generates a fresh `registry.json` database with absolute GitHub Raw download links, and pushes it back to the main branch!

Here is how the validator responds when it catches errors:
![Validator Basic Layout Fail](/assets/img/posts/Coding_Period_Week2/level1.png)
![Validator Step 2 Wires Fail](/assets/img/posts/Coding_Period_Week2/Level2.png)
![Validator Step 3 Python Fail](/assets/img/posts/Coding_Period_Week2/Level3.png)

### 3. Frontend UI, Downloads, and Palette Injection
With the cloud registry generating live data, I built the Marketplace UI panel in the React frontend to fetch the `registry.json` directly from GitHub. I then engineered the installation process:

1. Clicking "Install" triggers a fetch request to GitHub's raw servers to download the `.vc3` file.
2. The block data is immediately saved into the browser's persistent `localStorage`.
3. It is dynamically injected into the left sidebar palette. Depending on its metadata, it is perfectly grouped under the correct Category header alongside the native blocks!

![Downloads UI](/assets/img/posts/Coding_Period_Week2/Downloads.png)

<video controls width="100%">
  <source src="/gsoc2026-Sarvesh_Mishra/assets/img/posts/Coding_Period_Week2/Test.mp4" type="video/mp4">
</video>

## 🧗 The Challenges and Difficulties
Building a system that spans across multiple repositories and relies heavily on both React and GitHub Actions came with several tough technical hurdles:

1. **Injecting Blocks into the Native Palette:** One of the hardest frontend challenges was dynamically injecting downloaded blocks into the existing visual sidebar palette. I had to carefully intercept the React component that renders the native blocks, read from `localStorage`, and inject the new blocks on the fly while ensuring they were categorized seamlessly alongside the built-in elements.
2. **Refining the Python Validator:** Making the Python validator intelligent was much harder than anticipated. Originally, my validator strictly required every block to contain a `basic.code` node. However, I realized this broke VisualCircuit's ability to create "Macro Blocks" (where a user simply wires two native blocks together, like a Camera to a Drive, without writing custom code). I had to rewrite the structural wiring checks to dynamically ensure that every block placed on the canvas was wired to something, regardless of what type of node it was.
3. **Splitting and Stacking Pull Requests:** Because these features were so deeply intertwined (the Downloads panel relies on the Marketplace, and the Marketplace relies on the Tags), preparing the code for review was difficult. I had to learn how to expertly use Git to "stack" my branches. I separated the code into three distinct PRs (`feature/tags-improvement`, `feature/marketplace`, and `feature/downloads-palette`), setting the base of each PR to the previous branch. This allowed my mentors to review the code in clean, bite-sized pieces without any merge conflicts!

This week successfully bridged the gap between local block creation and global community sharing. All the end-to-end testing has passed, the PRs are submitted, and I'm looking forward to refining the UI next week!

## 🤝 Mentor Meeting
![Meet](/assets/img/posts/Coding_Period_Week2/Meet.png)

## 🔗 Pull Requests
Here are the Pull Requests submitted for this week's features:
- [feature/tags-improvement](https://github.com/JdeRobot/VisualCircuit/pull/462)
- [feature/marketplace](https://github.com/JdeRobot/VisualCircuit/pull/463)
- [feature/downloads-palette](https://github.com/JdeRobot/VisualCircuit/pull/464)
- [VisualCircuit-resources PR #13: Cloud Registry & Validator](https://github.com/JdeRobot/VisualCircuit-resources/pull/13)
