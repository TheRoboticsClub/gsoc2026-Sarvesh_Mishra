---
title: "Coding Period Week 11 (August 7 ~ August 13)"
date: 2026-08-12 10:00:00 +0530
categories: [GSoC 2026, Progress]
tags: [gsoc, coding-period, week-11]
published: true
---

This week was a massive milestone for the project! The primary focus was on completing the end-to-end workflow testing, finalizing pending PRs, and fully automating the block documentation pipeline.

## 🎯 The Main Goals for the Week
During our last meeting, we defined the following core tasks:
- Complete end-to-end workflow testing, including the new library integration.
- Finalize all pending PRs and merge them after mentor reviews.
- Prepare documentation for publishing blocks to the Marketplace and automate it using CI/CD.
- Update the project blog with the latest progress.
- Define and document the workflow for handling future block additions to the `VisualCircuit-resources` repository.

## 🚀 Finalizing PRs & Library AST Extraction
I kicked off the week by merging several pending Pull Requests across the repositories:
- [VisualCircuit-resources PR #18](https://github.com/JdeRobot/VisualCircuit-resources/pull/18)
- [VisualCircuit PR #471](https://github.com/JdeRobot/VisualCircuit/pull/471)

While working on the Django implementation for downloaded blocks, a critical issue was raised: [Issue #470](https://github.com/JdeRobot/VisualCircuit/issues/470). 
VisualCircuit only supports specific libraries built into the environment. If a user uploads a block containing an unsupported library, it fails natively. To fix this, I engineered a Python AST (Abstract Syntax Tree) script! The script scans every custom block for `import` or `import from` statements, extracts the library, and automatically maps it to the correct internal dependency (for example, mapping `cv2` to `python-vision`). I submitted this fix in [VisualCircuit PR #478](https://github.com/JdeRobot/VisualCircuit/pull/478).

## 📄 Automating the Documentation (`blocks.html`)
One of the major tasks was automating the documentation so that whenever a new block is approved in the Marketplace, its documentation is automatically generated. 

**The Problem:** The documentation file (`blocks.html`) lived inside the main `VisualCircuit` repository, but the custom blocks were being pushed to the `VisualCircuit-resources` repository. Because they were in different repos, the CI/CD pipeline couldn't automatically edit the HTML!

**The Solution:** After discussing it with my mentor, we decided to migrate the entire `blocks.html` documentation over to the `VisualCircuit-resources` repository. 
- I raised [VisualCircuit PR #495](https://github.com/JdeRobot/VisualCircuit/pull/495) to change the documentation routing.
- I then created [VisualCircuit-resources PR #19](https://github.com/JdeRobot/VisualCircuit-resources/pull/19), which contains the Python automation script. Now, whenever a block is pushed, a GitHub Action automatically scripts and formats its details into `blocks.html`!

## 🧪 End-to-End Workflow Testing
With the architecture finally complete, I ran a full end-to-end test of the entire Marketplace pipeline:

1. **Block Push & Validation:** I pushed a custom block and watched the CI/CD pipeline run the 3-level architecture test and successfully update `registry.json`.
<!-- TODO: Attach Screenshot 1 of the 3-level architecture test here -->
![Tags UI](/assets/img/posts/Coding_Period_Week11/P1.png)
<!-- TODO: Attach Screenshot 2 of the registry.json update here -->
![Tags UI](/assets/img/posts/Coding_Period_Week11/P2.png)
![Tags UI](/assets/img/posts/Coding_Period_Week11/P3.png)

2. **Marketplace Listing:** I opened the VisualCircuit frontend and verified that the block was correctly fetched from the cloud and listed in the Marketplace UI.
<!-- TODO: Attach Screenshot of the block listed in the Marketplace here -->
![Tags UI](/assets/img/posts/Coding_Period_Week11/P4.png)

3. **Downloading & Documentation:** I downloaded the block. It saved perfectly to the local hard drive via the Django backend, immediately appeared in the local palette, and I verified that the automated documentation in `blocks.html` had generated correctly!
<video controls width="100%">
    <source src="/gsoc2026-Sarvesh_Mishra/assets/img/posts/Coding_Period_Week11\V1.mp4" type="video/mp4">
  </video>
![Tags UI](/assets/img/posts/Coding_Period_Week11/P5.png)

Here is the complete video showcasing the entire end-to-end workflow in action:
<video controls width="100%">
    <source src="/gsoc2026-Sarvesh_Mishra/assets/img/posts/Coding_Period_Week11\V2.mp4" type="video/mp4">
  </video>

## 🤝 Mentor Meeting
We had a highly productive sync this week to review the AST library extraction, finalize the documentation migration, and officially approve the end-to-end testing workflow!


