---
title: "Community Bonding Week 3 (May 22 ~ 28)"
date: 2026-05-26 10:00:00 +0530
categories: [GSoC 2026, Progress]
tags: [gsoc, community-bonding]
published: true
---

## Objectives for the Week
- [x] Test and finalize the Amazon Warehouse workspace and Laser Mapping exercises.
- [x] Plan the implementation for the VisualCircuit Marketplace for the upcoming Coding Period Week 1.

## Progress & Achievements
- **Finalizing the Exercises:** I successfully finalized the outputs for both the Amazon Warehouse workspace and Laser Mapping exercises. The Laser Mapping exercise is fully working!
  <video controls width="100%">
    <source src="/gsoc2026-Sarvesh_Mishra/assets/img/posts/week3_community_bonding/Lasermapping.mp4" type="video/mp4">
  </video>
- **Planning the VisualCircuit Marketplace:** I spent time heavily researching and planning how to create, host, and install components via the new VisualCircuit Marketplace. Here is my finalized system architecture:
  
  1. **The Dynamic Registry (The Database):** Instead of storing blocks directly inside the VisualCircuit repository, I designed a separate system using a `registry.json` file. This acts as a live "phonebook" that lists all approved blocks (their names, descriptions, and download URLs). Because it relies on GitHub Raw URLs, it is completely free, dynamic, and decoupled from the main app.
  
  2. **The Frontend UI (The Marketplace Panel):** I've architected a React sliding panel (`MarketplacePanel.tsx`) that acts as the "App Store" inside the VisualCircuit editor.
     - When the user opens the panel, it uses a simple `fetch()` API to read the live `registry.json` from the internet.
     - It displays all the available blocks to the user seamlessly.
  
  3. **The Unpacker & Local Storage (The Installation Flow):** When a user clicks "Install", the system will use a custom installation flow:
     - **Validation:** It runs a quick structural sanity check (`validator.ts`) on the frontend to ensure the file isn't corrupted.
     - **Storage:** It saves the downloaded block into the browser's `localStorage` so it persists even if the user refreshes the page.
     - **The Unpacker:** Instead of wrapping imported blocks in messy folders, I designed a custom unpacker (`addAsRawBlocks` in `editor.ts`). This automatically extracts the raw Python code blocks from the downloaded `.vc3` JSON and dumps them directly onto the active canvas, ready to be wired.
  
  4. **The Backend Validator (GitHub Actions):** To ensure the Marketplace stays secure and curated, I designed an automated CI pipeline. When a contributor submits a new block via a Pull Request, a Python script (`validator.py`) automatically intercepts it. It uses a 3-Level Check:
     - **Level 1:** Verifies the Name and Description metadata exist.
     - **Level 2:** Checks the JSON blueprint to ensure there are no disconnected/floating wires.
     - **Level 3:** Extracts the hidden Python code, compiles it in memory to catch syntax errors, and blocks dangerous imports (like `os` or `subprocess`).

- **Mentor Meeting:** We had our weekly sync to discuss this week's progress and the transition into the official coding period!
  
  ![Meet](/assets/img/posts/week3_community_bonding/Meet.png)

## Issues Faced & Blockers
- **OMPL Library Issues (Amazon Warehouse):** The Amazon Warehouse exercise is still throwing errors related to the OMPL path-planning library.
- **State Errors:** I am also getting some state-related errors during execution.
*I plan to reach out to my mentors for guidance on resolving these two issues as we enter the coding period.*

## Next Steps
- Resolve the OMPL and state errors with the help of my mentors.
- Begin the official Coding Period!
- Start actively developing the VisualCircuit Marketplace based on the architecture mapped out this week.
