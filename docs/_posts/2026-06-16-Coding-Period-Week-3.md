---
title: "Coding Period Week 3 (June 12 ~ June 18)"
date: 2026-06-16 10:00:00 +0530
categories: [GSoC 2026, Progress]
tags: [gsoc, coding-period, week-3]
published: true
---

This week was heavily focused on infrastructure and debugging. While my initial goals for the week were strictly code-related, I ended up diving deep into Docker, VirtualGL, and hardware acceleration to solve a critical environment bug!

## 🎯 The Main Goals for the Week
My tasks assigned from last week were to:
- [ ] Get the Laser Mapping exercise ready for the Robotics Academy (RA).
- [ ] Successfully set up the fully working backend and get the entire VisualCircuit system running properly locally.

However, I spent almost the entire week fighting a severe infrastructure bug that prevented me from starting the actual development. Here is the story of how I debugged and finally defeated it.

## 🐛 The Major Blocker: The Gazebo GUI Crash
To begin development on the Laser Mapping exercise, I downloaded the Robotics Academy Docker image on my macOS machine. However, when I launched the container, the **Gazebo world was simply not loading**. 

At first, I thought it was an issue with my local cache or macOS compatibility. I spent days changing various codes and attempting to clean/rebuild the environment, but nothing worked. Assuming it was a macOS-specific architecture issue, I pivoted and spun up an **Ubuntu Server** to test it in a native Linux environment.

I installed Docker on the Ubuntu server, launched the RA container, and to my extreme frustration... the exact same issue occurred! The Gazebo world refused to open.

## 🔍 Investigating the Root Cause
After digging deep into the container logs and the launch commands, I realized the issue was directly tied to hardware acceleration. The server was trying to launch in GPU mode, even though it didn't possess a physical GPU!

Here was the original command I used from the Linux setup guide:
```bash
docker run --rm -it $(nvidia-smi >/dev/null 2>&1 && echo "--gpus all") --device /dev/dri -p 6080-6090:6080-6090 -p 7163:7163 -p 7164:7164 --link academy_db jderobot/robotics-academy:latest
```

Since the server didn't have an NVIDIA GPU, the command dynamically resolved to:
```bash
docker run --rm -it --device /dev/dri -p 6080-6090:6080-6090 -p 7163:7163 -p 7164:7164 --link academy_db jderobot/robotics-academy:latest
```

### The Breakdown of the Bug
The exact issue that prevented the Gazebo GUI from launching was a conflict between VirtualGL (`vglrun`) inside the container and the virtual graphics hardware on the server:

1. **The Device Detection:** When the container was started with the `--device /dev/dri` flag, the Robotics Academy manager automatically detected this device mapping and assumed physical GPU hardware acceleration was available (`ACCELERATION_ENABLED = True`).
2. **The Command Prefix:** Because it thought it had a GPU, the manager attempted to launch the Gazebo GUI client using VirtualGL:
   ```bash
   vglrun gz sim -g -v4 --gui-config ...
   ```
3. **The Crash:** The server does not have a real physical 3D graphics card; it only has a virtualized QEMU standard VGA driver. When VirtualGL (`vglrun`) tried to initialize 3D rendering, it failed violently with the following error:
   ```text
   [VGL] ERROR: in init3D--
   [VGL]    245: Invalid EGL device
   ```
   This error caused the Gazebo GUI client to crash instantly in the background before it could even attempt to render anything to the screen.

## 🛠️ The Fix
The culprit causing the crash the entire time was the `--device /dev/dri` flag. 

By simply removing the `--device /dev/dri` flag from the `docker run` command, the container no longer sees the virtual device. The RA manager correctly detects that GPU acceleration is **OFF** and launches standard Gazebo directly without the VirtualGL wrapper:
```bash
gz sim -g -v4 --gui-config ...
```
This allows Gazebo to run successfully using the CPU's standard software renderer!

At the end of this frustrating process, I discovered that the correct launch script was actually available in the official User Guide. The guide had recently been updated by another contributor to include the correct commands for environments without physical GPUs, which perfectly solves the issue!

## 🚀 Looking Forward
While it was an incredibly frustrating week spending days just to get the environment running, debugging this taught me a massive amount about Docker hardware passthrough, VirtualGL, and the internal launch mechanics of the Robotics Academy. 

Now that the RA environment is finally running flawlessly, I am incredibly excited to jump back into writing code and finalizing the Laser Mapping integration next week!
