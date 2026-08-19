---
title: "Coding Period Week 10 (July 31 ~ August 6)"
date: 2026-08-05 10:00:00 +0530
categories: [GSoC 2026, Progress]
tags: [gsoc, coding-period, week-10]
published: true
---

This week was highly analytical. Before we could merge the massive end-to-end Marketplace architecture we built over the last few weeks, we had to solve a critical execution flaw and meticulously plan the CI/CD documentation pipeline. 

## 🎯 The Main Goals for the Week
1. Investigate and resolve the library/dependency crash issue when users upload blocks with external libraries.
2. Architect and plan the automated documentation workflow for the Marketplace blocks.
3. Review and prepare all pending Pull Requests for the massive end-to-end merge scheduled for next week.

## 🐛 Investigating the Dependency Crash (Issue #470)
As we began testing the Marketplace workflow, we hit a wall: VisualCircuit only supports specific libraries built natively into its environment. If a user downloaded a custom block that contained an unsupported library (like `cv2`), the entire system would fail natively upon execution.

I spent a significant portion of the week researching how to solve this dynamically. My proposed solution was to use Python's **AST (Abstract Syntax Tree)** to automatically scan the user's custom block code for `import` and `import from` statements. By extracting the library name dynamically, we could map it to the correct internal dependency (e.g., mapping `cv2` to `python-vision`). I began scripting this AST extraction logic to prepare it for a PR next week.

<!-- TODO: Attach an image of the Python AST script logic or the Issue discussion here -->
![Tags UI](/assets/img/posts/Coding_Period_Week10/P2.png)

## 📑 Planning the CI/CD Documentation Architecture
The second major hurdle was figuring out how to automate the documentation generation for new blocks.

I mapped out the entire pipeline and quickly realized a major structural blocker: the documentation file (`blocks.html`) lived inside the main `VisualCircuit` repository, but the custom blocks were being pushed to the `VisualCircuit-resources` repository. Because they were in completely different repositories, the CI/CD pipeline couldn't automatically edit the HTML file when a new block was approved!

After an intense planning session with my mentor, we decided the only viable path forward was to completely migrate the `blocks.html` file into the `VisualCircuit-resources` repository.

![Tags UI](/assets/img/posts/Coding_Period_Week10/P1.png)

## 🤝 Mentor Meeting
We spent our meeting this week heavily focused on architecture. We reviewed the AST extraction theory, finalized the decision to migrate the documentation routing, and prepared all our pending PRs to be officially merged next week!

