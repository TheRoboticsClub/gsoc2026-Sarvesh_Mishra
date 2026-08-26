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
I created the `VisualCircuit-resources` repository to act as the global database for custom blocks. I then built the frontend **Marketplace UI** inside the main VisualCircuit editor. By fetching a global `registry.json` file, users are now able to dynamically search, filter by category tags, and view descriptions of community-made blocks directly inside the editor without needing to update their application.
![Tags UI](/assets/img/posts/Coding_Period_Week8/Meet2.png)

### 3. CI/CD Verification Workflow (Major Milestone)
To ensure the quality and security of community contributions, I engineered a strict, multi-stage architecture test using GitHub Actions. This was a critical component of the project. Whenever a contributor pushes a new block, the CI/CD pipeline triggers automated Python scripts that deeply parse the block's JSON metadata. It checks for security violations (like `os` or `sys` module imports), validates the existence of required fields (Author, Description, Input/Output Ports), and ensures proper block formatting. Only upon passing these strict checks does the pipeline safely and automatically append the block to the global `registry.json`.

### 4. Making the Robotics Academy Exercises
I converted massive, monolithic Python scripts into modular VisualCircuit blocks designed to be used as actual **Robotics Academy exercises** (specifically **Obstacle Avoidance** and **Laser Mapping**). I tested these heavily to ensure they met the platform’s high educational standards.

### 5. Reusable Dynamic Blocks Refactor
Initially, the circuits I built used hardcoded ROS topics (like `laser_ready` or `linear_x`), meaning they couldn’t be reused in different robot models. I completely refactored the design to use **generic ports** (`In`, `Out`, `Ready`). This architectural shift allowed the blocks to purely process data dynamically, letting the user decide how to wire them together.

### 6. The Full-Stack Django Storage Migration (Major Milestone)
This was one of the largest architectural changes of the summer. Initially, downloaded blocks were stored in the browser’s volatile `localStorage`, meaning clearing the browser completely deleted the user's blocks. I spent several weeks rebuilding this into a persistent, full-stack Django architecture. Clicking “Install” on a block now triggers a REST API `POST` request to the Django backend. The backend parses the incoming block payload, sanitizes it, and saves it permanently as a physical `.vc3` file on the user's hard drive within the `custom_blocks` directory. The frontend then dynamically reloads the palette via a `GET` request, meaning custom blocks now persist permanently across sessions and feel like native application components.

### 7. Fixing External Library Crashes (AST Extraction)
During testing, we found a major bug: if a downloaded block used an external Python library (like `cv2` or `numpy`), the entire VisualCircuit application would crash. To fix this, I wrote a Python script using **AST (Abstract Syntax Tree)**. This script automatically reads the code inside a custom block, finds any `import` statements, and safely connects them to the system's internal libraries before the block even runs, completely preventing the crash!


### 8. Automated Documentation Generation Pipeline
Originally, the documentation file (`blocks.html`) lived in the main UI repository, but the blocks were pushed to the external resources repository, causing a disconnect. I successfully migrated the documentation system to the resources repository and fully automated it. I wrote a complex Python GitHub Action that triggers whenever a block is merged. It extracts the block's internal logic, injects it into a reusable `pdoc3` HTML template to generate a standalone documentation page, and then dynamically parses the DOM of the master `Blocks.html` index to append a sidebar link for the new block. This means the documentation is now 100% self-maintaining.

### 9. End-to-End Testing & Tutorials
Finally, I conducted rigorous **end-to-end testing** of the entire architecture: pushing a block, watching the verification CI/CD pass, installing it via the Django backend, and confirming the automated documentation generated perfectly. I also recorded full video tutorials for future contributors on how to package and publish blocks to ensure the community can easily adopt the new system.

![Tags UI](/assets/img/posts/Coding_Period_Week1/Ui.png)
![Tags UI](/assets/img/posts/Coding_Period_Week5/RA.png)

Here is the complete video showcasing the entire end-to-end workflow in action:
<video controls width="100%">
    <source src="/gsoc2026-Sarvesh_Mishra/assets/img/posts/Coding_Period_Week11\V2.mp4" type="video/mp4">
  </video>
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
| `2026-03-02` | VisualCircuit | [PR #418](https://github.com/JdeRobot/VisualCircuit/pull/418) | Initial architectural updates and platform fixes |
| `2026-04-05` | VisualCircuit | [PR #447](https://github.com/JdeRobot/VisualCircuit/pull/447) | Enhancements to core circuit execution logic |
| `2026-06-09` | VisualCircuit | [PR #462](https://github.com/JdeRobot/VisualCircuit/pull/462) | Improved the block tagging system for the UI |
| `2026-06-09` | VisualCircuit | [PR #463](https://github.com/JdeRobot/VisualCircuit/pull/463) | Integrated the core Marketplace UI and registry fetching |
| `2026-06-09` | VisualCircuit | [PR #464](https://github.com/JdeRobot/VisualCircuit/pull/464) | Built the frontend downloads palette for custom blocks |
| `2026-07-01` | VisualCircuit | [PR #469](https://github.com/JdeRobot/VisualCircuit/pull/469) | UI and structural improvements for custom block loading |
| `2026-07-15` | VisualCircuit | [PR #471](https://github.com/JdeRobot/VisualCircuit/pull/471) | Integrated the full-stack Django storage backend (replacing localStorage) |
| `2026-07-23` | VisualCircuit | [PR #478](https://github.com/JdeRobot/VisualCircuit/pull/478) | Implemented the Python AST extraction to map external dependencies |
| `2026-08-11` | VisualCircuit | [PR #495](https://github.com/JdeRobot/VisualCircuit/pull/495) | Shifted `blocks.html` routing to the resources repository |
| `2026-06-08` | VisualCircuit-resources | [PR #13](https://github.com/JdeRobot/VisualCircuit-resources/pull/13) | Created the cloud registry and automated GitHub Actions Validator |
| `2026-07-17` | VisualCircuit-resources | [PR #17](https://github.com/JdeRobot/VisualCircuit-resources/pull/17) | Additional registry validation logic |
| `2026-08-06` | VisualCircuit-resources | [PR #18](https://github.com/JdeRobot/VisualCircuit-resources/pull/18) | Refined architecture logic and finalized registry URLs based on mentor feedback |
| `2026-08-12` | VisualCircuit-resources | [PR #19](https://github.com/JdeRobot/VisualCircuit-resources/pull/19) | Engineered the CI/CD pipeline to automate documentation updates |
| `2026-08-19` | VisualCircuit-resources | [PR #20](https://github.com/JdeRobot/VisualCircuit-resources/pull/20) | Pre-launch block metadata verification |
| `2026-08-19` | VisualCircuit-resources | [PR #21](https://github.com/JdeRobot/VisualCircuit-resources/pull/21) | Final integration updates for Marketplace blocks |
| `2026-08-22` | VisualCircuit-resources | [PR #22](https://github.com/JdeRobot/VisualCircuit-resources/pull/22/) | Final documentation and project handover fixes |

## Acknowledgements
A massive thank you to my mentors and the entire JdeRobot community for guiding me through this fantastic summer! Working with you all has been an incredible learning experience, and I deeply appreciate the time, feedback, and support you provided every step of the way to make this project a success.

