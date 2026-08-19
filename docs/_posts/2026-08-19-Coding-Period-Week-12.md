---
title: "Coding Period Week 12 (August 14 ~ August 20)"
date: 2026-08-19 10:00:00 +0530
categories: [GSoC 2026, Progress]
tags: [gsoc, coding-period, week-12]
published: true
---

This week was incredibly exciting as we moved from creating specific hardcoded circuits to building truly dynamic, reusable blocks that anyone can download and use from the Marketplace! I also wrapped up some critical documentation PRs and recorded a tutorial for future contributors.

## 📄 Perfecting the Documentation Automation
I started the week by revisiting the documentation automation Pull Request from last week. While the automation script successfully worked, the generated output didn't perfectly match the styling of the other native HTML blocks in VisualCircuit. 

I spent time adjusting the HTML parsing and generation so the automated documentation perfectly mimics the native styling. I fully tested the flow and recorded a video showcasing the seamless automated documentation generation!

- <!-- TODO: Attach Image of the Documentation PR here -->
- <!-- TODO: Attach Video of the automated documentation generation here -->

## 🧩 Building Truly Reusable Blocks
The biggest technical shift this week was converting our previous code from a rigid "circuit" into fully reusable "blocks".

**The Evolution of the Laser Mapping Logic:**
1. **Single Script:** Initially, the laser mapping code was written as one full program where everything happened in the same loop (reading laser, reading odometry, calculating position, and updating the map). It worked, but it wasn't reusable.
2. **Fixed Circuit:** I then converted it into VisualCircuit blocks. However, this first version was more like a fixed circuit. Each block had highly specific hardcoded port names like `front_distance`, `laser_ready`, `linear_x`, and `angular_z`. It worked for one specific setup, but you couldn't reuse the block easily because the input and output names were too rigid.
3. **Reusable Port-Based Blocks (Final):** I completely refactored the design. Instead of hardcoding ROS topics or fixed circuit names, the blocks now use generic ports like `In`, `Out`, and `Ready`. 

This means the laser sensor block can be connected from outside, the odometry block can be connected from outside, and the mapping output can be sent to any compatible display block!

**The Final Laser Mapping Flow:**
- Laser data input -> **Laser Preprocess Block**
- Odom data input -> **Pose Converter Block**
- Processed laser + pose -> **Map Builder Block**
- Map builder output -> **Display/WebGUI Block**

The circuit now decides which sensor or display is connected, while the reusable blocks purely process input data and produce output data.

- <!-- TODO: Attach Image of the Reusable Laser Mapping Block here -->
- <!-- TODO: Attach Video of Testing Laser Mapping Block here -->

## 📤 Publishing the Blocks & Contributor Tutorial
Now that the blocks were fully dynamic and reusable, I pushed both the Obstacle Avoidance block and the Laser Mapping block to the repository for review!

- <!-- TODO: Attach Image of the Reusable Obstacle Avoidance Block here -->
- <!-- TODO: Attach Video of Testing Obstacle Avoidance Block here -->

- <!-- TODO: Add Links to the 2 PRs for the new blocks here -->
- <!-- TODO: Attach Image of the PRs here -->

Because pushing a block into the marketplace requires a specific workflow, I also recorded a full tutorial video specifically designed for future open-source contributors. It walks through exactly how to package, document, and push a custom block to the VisualCircuit Marketplace.

- <!-- TODO: Attach Contributor Tutorial Video (or Thumbnail Photo) here -->

## 🤝 Mentor Meeting
We had a great sync at the end of the week to review the newly refactored dynamic blocks, the final documentation styling, and the new contributor tutorial!

<!-- TODO: Attach Mentor Meeting Photo here -->
