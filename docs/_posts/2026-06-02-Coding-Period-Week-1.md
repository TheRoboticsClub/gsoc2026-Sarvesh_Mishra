---
title: "Coding Period Week 1 (May 29 ~ June 4)"
date: 2026-06-02 10:00:00 +0530
categories: [GSoC 2026, Progress]
tags: [gsoc, coding-period]
published: true
---

## Objectives for the Week
- [x] Conclude investigations into the Amazon Warehouse workspace issues.
- [ ] Finalize the Laser Mapping block integration in VisualCircuit.
- [x] Build the core frontend UI and backend linking for the VisualCircuit Marketplace.
- [x] Fix the block unpacking and `.zip` download bugs for the Marketplace.

## Progress & Achievements
This week marks the beginning of the official GSoC coding period! I have made massive progress in building the foundation of the VisualCircuit Marketplace, focusing on the UI, the registry, and end-to-end testing. Here is a step-by-step breakdown of everything we have built so far:

### Step 1: Designing the Registry Data (The Database)
We first decided that the Marketplace needed to be completely decoupled from the main VisualCircuit repository. We accomplished this by creating a dynamic registry.
- I created a registry file that acts as a live directory containing references and addresses to the blocks.
  
  ![Registry](/assets/img/posts/Coding_Period_Week1\Registry.png)

### Step 2: Architecture & File Storage (.vc3)
I spent time studying the core VisualCircuit blocks and found that each block uses a `.vc3` file that stores all its information. 
- **The Storage Plan:** I decided to create a completely separate, new repository to store these `.vc3` files. Users and contributors will be able to push their blocks directly to this new repo.
- I then mapped the addresses of these `.vc3` files into the registry.

### Step 3: Building the UI & Download Flow
I built the Marketplace UI directly into the VisualCircuit editor.
- **The Download Button:** I designed and implemented a dedicated download button within the UI.
- **The API Integration:** I developed the download functionality using the GitHub API. When a user clicks download, the system successfully fetches and downloads the `.vc3` file to their computer.

  ![UI](/assets/img/posts/Coding_Period_Week1\Ui.png)

### Step 4: End-to-End Testing
To ensure the download flow worked flawlessly:
- I created a dummy repository and uploaded some example `.vc3` blocks to test the full pipeline.
- It worked! The blocks were successfully fetched from the dummy repo and downloaded to my computer.

  <video controls width="100%">
    <source src="/gsoc2026-Sarvesh_Mishra/assets/img/posts/Coding_Period_Week1\Test.mp4" type="video/mp4">
  </video>

## Exercise Updates & Ongoing Issues
- **Amazon Workspace (Resolved/Dropped):** I discussed the persistent Amazon Workspace errors with my mentor. We realized there was a library clash caused by the OMPL library updating from `1.7` to `2.0`. Because this is an external library issue, my mentor advised me to drop this plan, so this task is considered closed!
- **Laser Mapping:** I successfully recorded a video of the Laser Mapping working as a singular block. However, when I split it into multiple blocks, the execution fails. I will be asking my mentor for guidance on this next week.
- **Marketplace Auto-Load Issue:** Currently, when a user clicks download, the `.vc3` file simply downloads as a file to their computer. My next major goal is to figure out how to intercept this download and instantly auto-load the block directly into the VisualCircuit main layout without the user having to manually drag it in.

## Mentor Meeting
We held our weekly sync to discuss this week's progress and the transition into the official coding period!

  ![Meet](/assets/img/posts/Coding_Period_Week1\Meet.png)

## Next Steps
- **The Backend Validator:** I will be discovering and planning how to write the automated validator and investigating how it will work using GitHub Actions.
- **Final Repository:** I will ask Mentors to create a new repository for complete testing and final production use of the Marketplace.
- Complete the Laser Mapping split-block issue with my mentor's help.
- Push the finalized code to GitHub!
