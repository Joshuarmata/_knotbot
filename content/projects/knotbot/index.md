---
title: "KnotBot"
date: 2026-05-13
summary: "Autonomous rope knot-tying robot using a UR7e arm, Intel RealSense depth camera, and ROS2 visual servoing"
tags:
  - Robotics
  - Computer Vision
  - ROS2
  - Manipulation
tech_stack:
  - Python
  - PyTorch
  - ROS2
  - MoveIt
  - OpenCV
  - RealSense SDK
  - UR7e
  - Docker
links:
  - type: github
    url: https://github.com
    label: Code
featured: true
status: "Completed"
role: "EE 106A Final Project"
duration: "Spring 2026"
team_size: 2
highlights:
  - "7-phase autonomous knot-tying sequence (steps 0–6)"
  - "Geometric visual servoing: HSV + RealSense + SVD line fits → MoveIt goals"
  - "Knot-step perception: supervised full-frame ResNet18 (rope_knot_infer HUD)"
  - "ArUco-based table-plane calibration for IR-absorbing tape"
  - "MoveIt IK + trajectory control"
---

KnotBot autonomously ties an overhand knot in a rope using a UR7e 6-DOF robot arm guided by real-time computer vision.

## Overview

KnotBot is the final project for **EE 106A — Introduction to Robotics** at UC Berkeley (Spring 2026). The system combines visual perception, geometric planning, and closed-loop robot control to perform a task that requires sub-centimeter precision: tying a knot in a rope.

The robot uses an **Intel RealSense D435** depth camera mounted on its wrist to perceive the workspace in real time, detects the positions of colored rope/tape landmarks, computes the geometry of each manipulation phase, and executes a **seven-phase** sequence (steps 0–6) via **MoveIt** motion planning and ROS2 visual servoing. A separate **rope knot perception** pipeline performs supervised full-frame classification of knot phases (e.g. `step0`, `step1`) for monitoring and tooling—see the homepage **Visual Pipeline** section for how that sits next to the geometric servo loop.

---

## Demo Video

<div class="knotbot-placeholder knotbot-video-placeholder" style="max-width:800px;margin:2rem auto;">
  <div class="knotbot-placeholder-icon">▶</div>
  <div class="knotbot-placeholder-label">Full Knot-Tying Demo Video</div>
  <div class="knotbot-placeholder-sub">Complete end-to-end demo of KnotBot tying an overhand knot — drop video file here</div>
</div>

---

## System Architecture

The system is implemented as a ROS2 package with two main nodes:

### `aruco_node` — Visual Perception Node

Processes camera images to detect and localize colored rope segments:

- **Color segmentation** — HSV masking for yellow, red, green, blue, black, and white regions
- **Depth estimation** — Uses the aligned RealSense depth stream; falls back to ArUco-calibrated table Z when colored tape absorbs IR light
- **3D localization** — Back-projects pixel centroids to 3D using camera intrinsics, then transforms to `base_link` via TF2
- **Best-fit lines** — SVD over color blob point clouds for rope orientation
- **Goal computation** — Publishes step-specific waypoints as TF frames and RViz markers

### `visual_servo_node` — Motion Control Node

Executes the knot-tying sequence:

- **TF lookups** — Reads step goal frames published by `aruco_node`
- **IK solving** — Uses MoveIt's `/compute_ik` service for Cartesian-to-joint conversion
- **Trajectory execution** — Sends joint trajectories via `JointTrajectoryController`
- **Gripper control** — Opens/closes gripper at appropriate steps
- **Force feedback** — Monitors wrist F/T sensor to detect rope tension in Step 6

---

## The 6-Step Knot Sequence

### Photos of Each Step

<div class="knotbot-media-grid knotbot-gallery-grid">
  <div class="knotbot-placeholder knotbot-image-placeholder">
    <div class="knotbot-placeholder-icon">🖼</div>
    <div class="knotbot-placeholder-label">Step 1 — Detect Crossing Point</div>
    <div class="knotbot-placeholder-sub">Robot positioned above the computed crossing point — drop image here</div>
  </div>
  <div class="knotbot-placeholder knotbot-image-placeholder">
    <div class="knotbot-placeholder-icon">🖼</div>
    <div class="knotbot-placeholder-label">Step 2 — Grasp Rope End</div>
    <div class="knotbot-placeholder-sub">Gripper aligned perpendicular to rope, approaching grasp point — drop image here</div>
  </div>
  <div class="knotbot-placeholder knotbot-image-placeholder">
    <div class="knotbot-placeholder-icon">🖼</div>
    <div class="knotbot-placeholder-label">Step 3 — Form the Loop</div>
    <div class="knotbot-placeholder-sub">Rope end moved to reflected position, loop forming — drop image here</div>
  </div>
  <div class="knotbot-placeholder knotbot-image-placeholder">
    <div class="knotbot-placeholder-icon">🖼</div>
    <div class="knotbot-placeholder-label">Step 4 — Thread the End</div>
    <div class="knotbot-placeholder-sub">Rope end threaded through the formed loop — drop image here</div>
  </div>
  <div class="knotbot-placeholder knotbot-image-placeholder">
    <div class="knotbot-placeholder-icon">🖼</div>
    <div class="knotbot-placeholder-label">Step 5 — Tighten</div>
    <div class="knotbot-placeholder-sub">Robot drawing rope segments toward center — drop image here</div>
  </div>
  <div class="knotbot-placeholder knotbot-image-placeholder">
    <div class="knotbot-placeholder-icon">🖼</div>
    <div class="knotbot-placeholder-label">Step 6 — Final Tuck & Release</div>
    <div class="knotbot-placeholder-sub">Completed overhand knot after release — drop image here</div>
  </div>
</div>

---

## Computer Vision Details

### Color Detection

Each color is detected using HSV thresholds tuned for the specific tape/rope colors used in the lab:

| Color | Purpose | HSV Range |
|-------|---------|-----------|
| Yellow | Rope being knotted | H: 15–45, S: 50–255, V: 50–255 |
| Red | Table reference marker | H: 0–5 / 168–180 (wraps) |
| Green | Table reference marker | H: 70–92, S: 120–255, V: 23–150 |
| Blue | Table reference marker | H: 95–130, S: 70–255, V: 70–255 |

### RViz Visualization

<div class="knotbot-media-grid">
  <div class="knotbot-placeholder knotbot-image-placeholder">
    <div class="knotbot-placeholder-icon">🖼</div>
    <div class="knotbot-placeholder-label">RViz: Color Detections</div>
    <div class="knotbot-placeholder-sub">Bounding boxes and 3D point clouds for each detected color in RViz — drop screenshot here</div>
  </div>
  <div class="knotbot-placeholder knotbot-image-placeholder">
    <div class="knotbot-placeholder-icon">🖼</div>
    <div class="knotbot-placeholder-label">RViz: Step Goal Markers</div>
    <div class="knotbot-placeholder-sub">Step 1–6 goal spheres and TF frames visualized in RViz — drop screenshot here</div>
  </div>
</div>

### Stereo Depth Fallback

Colored tape absorbs IR light and often returns zero depth from the RealSense aligned depth sensor. The system handles this with a two-level fallback:

1. **Aligned depth median** — Robust median over all pixels in the color blob (primary)
2. **ArUco table-plane Z** — Estimated from solvePnP on a 150mm ArUco marker (fallback)

---

## Additional Video Footage

<div class="knotbot-media-grid">
  <div class="knotbot-placeholder knotbot-video-placeholder">
    <div class="knotbot-placeholder-icon">▶</div>
    <div class="knotbot-placeholder-label">Camera Feed with Detection Overlays</div>
    <div class="knotbot-placeholder-sub">Real-time view from the wrist camera with color segmentation shown — drop video here</div>
  </div>
  <div class="knotbot-placeholder knotbot-video-placeholder">
    <div class="knotbot-placeholder-icon">▶</div>
    <div class="knotbot-placeholder-label">Step-by-Step Slow Motion</div>
    <div class="knotbot-placeholder-sub">Each of the 6 steps shown in slow motion for clarity — drop video here</div>
  </div>
</div>

---

## Team

| Name | Contribution |
|------|-------------|
| **Joshua Mata** | Color detection, depth estimation, ArUco calibration, TF broadcasting |
| **Allison Dana** | Color segmentation, best-fit lines, step goal geometry |

---

## Challenges & Solutions

### IR Absorption by Colored Tape

**Problem**: The RealSense D435 uses structured IR light for depth. Colored tape absorbs IR, returning invalid depth readings for the key landmarks.

**Solution**: ArUco marker (ID 1, 150mm) provides a reliable table-plane Z estimate via `solvePnP`. This value is used as the depth fallback whenever the aligned depth stream returns invalid readings for a color blob.

### Rope Deformation

**Problem**: Rope changes shape after each manipulation step, so a pre-computed trajectory cannot be reused.

**Solution**: All waypoints are computed fresh each frame from live camera detections. The system loops through the visual perception pipeline before each step to get updated positions.

### Gripper Alignment

**Problem**: Grasping a thin rope requires the gripper to be oriented perpendicular to the rope's length axis.

**Solution**: SVD over the full set of yellow pixel positions gives the global rope principal axis. The gripper quaternion is derived from this axis using Shepperd's method to ensure consistent alignment.
