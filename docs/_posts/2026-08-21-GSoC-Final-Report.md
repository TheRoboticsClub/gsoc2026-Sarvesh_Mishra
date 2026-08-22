---
title: "GSoC 2026 Final Report — VisualCircuit Marketplace & Reusable Blocks"
date: 2026-08-21 10:00:00 +0530
categories: [GSoC 2026, Final Report]
tags: [gsoc, gsoc2026, jderobot, visualcircuit, robotics, marketplace]
published: true
---

## About me
I am Sarvesh Mishra, a B.Tech student at IIT BHU, and I have spent my GSoC 2026 summer working with JdeRobot. I am deeply passionate about robotics, visual programming interfaces, and building robust full-stack architectures. Open-source development is a core part of my journey, and I love building tools that make robotics education accessible to everyone.

---

## About the project
My journey actually started during the Community Bonding period, where I spent several weeks deeply exploring the existing VisualCircuit architecture. I ran and tested multiple existing Robotics Academy exercises to understand exactly how the visual programming logic bridged the gap to the physical ROS simulation.

[VisualCircuit](https://github.com/JdeRobot/VisualCircuit) itself is a visual programming tool for robotics. It allows users to program robots simply by dragging, dropping, and connecting blocks on a canvas instead of writing raw code.

It had one major limitation: **Isolation**. 
If a user created a brilliant custom block for a specific robot behavior, there was no way to share it. Custom blocks lived only on the user's local machine. 

> My project was to break down these walls by building the **VisualCircuit Marketplace**—a global, cloud-based registry where developers can publish, share, discover, and download custom blocks seamlessly, right from within the VisualCircuit editor.

---

## What exists now
VisualCircuit now features a fully integrated, full-stack Marketplace. 

Users can browse a categorized registry of community-made blocks, click "Install", and the block is instantly downloaded and saved permanently to their local machine via a new Django backend. The downloaded block appears in their local palette and functions exactly like a native built-in block.

To prove the power of this system, I built and published two complex robotics exercises (**Laser Mapping** and **Obstacle Avoidance**) entirely out of dynamic, reusable custom blocks!

---

## The work
Over the course of the 12-week coding period, the project evolved through several major engineering milestones. Here is the comprehensive breakdown of everything built:

### 1. Exploring the Ecosystem
During the Community Bonding and early coding weeks, I spent time testing and running various Robotics Academy exercises natively in VisualCircuit. This allowed me to understand exactly how the visual programming logic bridged the gap to the physical ROS simulation before I began modifying the core architecture.

### 2. The Cloud Registry & Marketplace UI
I created the `VisualCircuit-resources` repository to act as the global database for custom blocks. I then built the frontend **Marketplace UI** inside the main VisualCircuit editor, fetching the global `registry.json` file so users could search, filter by tags, and view descriptions of community-made blocks.

### 3. CI/CD Verification Workflow
To ensure the quality of community contributions, I engineered a strict 3-level architecture test using GitHub Actions. Whenever a contributor pushes a new block, the CI/CD pipeline automatically checks for security violations, validates the required JSON metadata, and only upon passing, safely updates the global registry.

### 4. Making the Robotics Academy Exercises
I converted massive, monolithic Python scripts into modular VisualCircuit blocks designed to be used as actual **Robotics Academy exercises** (specifically **Obstacle Avoidance** and **Laser Mapping**). I tested these heavily to ensure they met the platform's high educational standards.

### 5. Reusable Dynamic Blocks Refactor
Initially, the circuits I built used hardcoded topics (like `laser_ready` or `linear_x`), meaning they couldn't be reused. I completely refactored the design to use **generic ports** (`In`, `Out`, `Ready`). This allowed the blocks to purely process data dynamically, letting the user decide how to wire them together.

### 6. The Full-Stack Django Storage Migration
Initially, downloaded blocks were stored in the browser's volatile `localStorage`, meaning clearing the browser deleted the blocks. I spent several weeks completely rebuilding this into a **full-stack Django architecture**. Clicking "Install" now triggers a `POST` request to the Django backend, saving it as a permanent `.vc3` file on the hard drive, which is dynamically loaded into the palette via a `GET` request.

### 7. AST Library Extraction
During testing, a critical bug emerged: if a downloaded block imported an external Python library (like `cv2`), VisualCircuit would crash natively. To solve this, I engineered a Python **AST (Abstract Syntax Tree)** script that dynamically scans custom blocks for `import` statements and maps them to internal system dependencies.

### 8. Shifting Documentation & Automation
The documentation file (`blocks.html`) lived in the main repo, but blocks were pushed to the resources repo. I successfully **shifted the documentation library** by migrating the routing of `blocks.html` over to the resources repository. I then wrote a GitHub Action to automate the documentation updates so that whenever a block is merged, its docs are automatically appended.

### 9. End-to-End Testing & Tutorials
Finally, I conducted rigorous **end-to-end testing of the blocks**: pushing a block, watching the verification CI/CD pass, installing it via the Django backend, and confirming the automated documentation generated perfectly. I also recorded full video tutorials for future contributors on how to package and publish blocks.

![Tags UI](/assets/img/posts/Coding_Period_Week1/Ui.png)
![Tags UI](/assets/img/posts/Coding_Period_Week5/RA.png)

---

## Contributor Tutorial
Because publishing a block involves specific metadata and CI/CD checks, I recorded a full tutorial video for future open-source contributors, explaining exactly how to package and push blocks to the VisualCircuit Marketplace.

<!-- TODO: Attach Contributor Tutorial Video here -->
![Tags UI](/assets/img/posts/Coding_Period_Week12/P2.png)
---

## Demo Video
Below are the final demonstration videos showcasing the complete VisualCircuit Marketplace workflow, dynamic blocks, and the end-to-end integration in action!

<div style="display: flex; justify-content: space-between; flex-wrap: wrap; gap: 10px; margin-bottom: 20px;">
  <iframe width="48%" height="315" src="https://www.youtube.com/embed/REUaMzv_FmM" frameborder="0" allowfullscreen></iframe>
  <iframe width="48%" height="315" src="https://www.youtube.com/embed/ca3Pa3PdMVs" frameborder="0" allowfullscreen></iframe>
</div>

---

## Pull Requests
Here is the list of all Pull Requests that encompass this project:

| Date | Repository | Pull Request | Description |
| :--- | :--- | :--- | :--- |
| `<!-- TODO: Date -->` | VisualCircuit | [PR #418](https://github.com/JdeRobot/VisualCircuit/pull/418) | Initial architectural updates and platform fixes |
| `<!-- TODO: Date -->` | VisualCircuit | [PR #447](https://github.com/JdeRobot/VisualCircuit/pull/447) | Enhancements to core circuit execution logic |
| `<!-- TODO: Date -->` | VisualCircuit | [PR #462](https://github.com/JdeRobot/VisualCircuit/pull/462) | Improved the block tagging system for the UI |
| `<!-- TODO: Date -->` | VisualCircuit | [PR #463](https://github.com/JdeRobot/VisualCircuit/pull/463) | Integrated the core Marketplace UI and registry fetching |
| `<!-- TODO: Date -->` | VisualCircuit | [PR #464](https://github.com/JdeRobot/VisualCircuit/pull/464) | Built the frontend downloads palette for custom blocks |
| `<!-- TODO: Date -->` | VisualCircuit | [PR #469](https://github.com/JdeRobot/VisualCircuit/pull/469) | UI and structural improvements for custom block loading |
| `<!-- TODO: Date -->` | VisualCircuit | [PR #471](https://github.com/JdeRobot/VisualCircuit/pull/471) | Integrated the full-stack Django storage backend (replacing localStorage) |
| `<!-- TODO: Date -->` | VisualCircuit | [PR #478](https://github.com/JdeRobot/VisualCircuit/pull/478) | Implemented the Python AST extraction to map external dependencies |
| `<!-- TODO: Date -->` | VisualCircuit | [PR #495](https://github.com/JdeRobot/VisualCircuit/pull/495) | Shifted `blocks.html` routing to the resources repository |
| `<!-- TODO: Date -->` | VisualCircuit-resources | [PR #13](https://github.com/JdeRobot/VisualCircuit-resources/pull/13) | Created the cloud registry and automated GitHub Actions Validator |
| `<!-- TODO: Date -->` | VisualCircuit-resources | [PR #17](https://github.com/JdeRobot/VisualCircuit-resources/pull/17) | Additional registry validation logic |
| `<!-- TODO: Date -->` | VisualCircuit-resources | [PR #18](https://github.com/JdeRobot/VisualCircuit-resources/pull/18) | Refined architecture logic and finalized registry URLs based on mentor feedback |
| `<!-- TODO: Date -->` | VisualCircuit-resources | [PR #19](https://github.com/JdeRobot/VisualCircuit-resources/pull/19) | Engineered the CI/CD pipeline to automate documentation updates |
| `<!-- TODO: Date -->` | VisualCircuit-resources | [PR #20](https://github.com/JdeRobot/VisualCircuit-resources/pull/20) | Pre-launch block metadata verification |
| `<!-- TODO: Date -->` | VisualCircuit-resources | [PR #21](https://github.com/JdeRobot/VisualCircuit-resources/pull/21) | Final integration updates for Marketplace blocks |
| `<!-- TODO: Date -->` | VisualCircuit-resources | [PR #22](https://github.com/JdeRobot/VisualCircuit-resources/pull/22/) | Final documentation and project handover fixes |

## Acknowledgements
A massive thank you to my mentors and the entire JdeRobot community for guiding me through this fantastic summer! Working with you all has been an incredible learning experience, and I deeply appreciate the time, feedback, and support you provided every step of the way to make this project a success.

