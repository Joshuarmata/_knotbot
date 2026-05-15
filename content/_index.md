---
# Leave the homepage title empty to use the site title
title: ''
summary: ''
date: 2026-05-13
type: landing

sections:
  # Hero
  - block: dev-hero
    id: hero
    content:
      username: me
      greeting: ""
      show_status: true
      show_scroll_indicator: true
      typewriter:
        enable: true
        prefix: "Tying knots with"
        strings:
          - "computer vision"
          - "a UR7e robot arm"
          - "ROS2 & MoveIt"
          - "visual servoing"
        type_speed: 70
        delete_speed: 40
        pause_time: 2500
      cta_buttons:
        - text: Watch Demo
          url: "#demo"
          icon: play
        - text: How It Works
          url: "#steps"
          icon: arrow-down
    design:
      style: centered
      avatar_shape: circle
      animations: true
      background:
        color:
          light: "#fafafa"
          dark: "#0a0a0f"
      spacing:
        padding: ["6rem", "0", "4rem", "0"]

  # About
  - block: markdown
    id: about
    content:
      title: "About KnotBot"
      subtitle: "Autonomous rope manipulation using robotic perception and control"
      text: |-
        KnotBot is a final project for **EE 106A — Introduction to Robotics** at UC Berkeley (Spring 2026).
        The system autonomously ties an overhand knot in a rope using a **UR7e 6-DOF robot arm**,
        guided entirely by real-time visual feedback from an **Intel RealSense D435** depth camera.

        The robot perceives the spatial layout of colored rope segments, computes the geometric
        transformations needed for each knot-tying step, and executes a six-step manipulation
        sequence using **MoveIt** motion planning and **ROS2** visual servoing.

        Key challenges addressed include depth estimation of color-absorbing rope (which defeats
        standard IR depth sensors), robust 3D localization via ArUco marker calibration, and
        closed-loop trajectory execution that adapts to real-world rope deformation.
    design:
      columns: '1'
      background:
        color:
          light: "#ffffff"
          dark: "#0d0d12"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]

  # Demo Video
  - block: markdown
    id: demo
    content:
      title: "Demo"
      subtitle: "KnotBot in action"
      text: |-
        <div class="knotbot-media-grid">
          <div class="knotbot-placeholder knotbot-video-placeholder">
            <div class="knotbot-placeholder-icon">▶</div>
            <div class="knotbot-placeholder-label">Full Knot-Tying Demo</div>
            <div class="knotbot-placeholder-sub">End-to-end video of KnotBot completing an overhand knot — drop video file here</div>
          </div>
          <div class="knotbot-placeholder knotbot-video-placeholder">
            <div class="knotbot-placeholder-icon">▶</div>
            <div class="knotbot-placeholder-label">Computer Vision Feed</div>
            <div class="knotbot-placeholder-sub">Side-by-side camera view with color detection overlays — drop video file here</div>
          </div>
        </div>
    design:
      columns: '1'
      background:
        color:
          light: "#f5f5f5"
          dark: "#08080c"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]

  # How It Works — timeline copy lives in data/authors/me.yaml → `experience` (Hugo Blox resume-experience ignores block `items`).
  - block: resume-experience
    id: steps
    content:
      title: "How It Works"
      subtitle: "A six-step geometric manipulation sequence"
      date_format: ""
    design:
      columns: '1'
      background:
        color:
          light: "#ffffff"
          dark: "#0d0d12"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]

  # Visual Pipeline
  - block: markdown
    id: vision
    content:
      title: "Visual Pipeline"
      subtitle: "From raw pixels to 3D robot poses"
      text: |-
        <div class="knotbot-pipeline">
          <div class="knotbot-pipeline-step">
            <div class="knotbot-pipeline-num">01</div>
            <div class="knotbot-pipeline-content">
              <strong>Color Segmentation</strong>
              <p>HSV masking isolates yellow, red, green, and blue rope/tape blobs. Morphological operations (open/close) remove noise.</p>
            </div>
          </div>
          <div class="knotbot-pipeline-arrow">→</div>
          <div class="knotbot-pipeline-step">
            <div class="knotbot-pipeline-num">02</div>
            <div class="knotbot-pipeline-content">
              <strong>Depth Estimation</strong>
              <p>Aligned RealSense depth stream provides Z values per blob. ArUco marker (ID 1, 150mm) calibrates the table-plane Z for IR-absorbing tape.</p>
            </div>
          </div>
          <div class="knotbot-pipeline-arrow">→</div>
          <div class="knotbot-pipeline-step">
            <div class="knotbot-pipeline-num">03</div>
            <div class="knotbot-pipeline-content">
              <strong>3D Localization</strong>
              <p>Camera intrinsics back-project 2D centroids to 3D camera frame. TF2 transforms to base_link give robot-relative coordinates.</p>
            </div>
          </div>
          <div class="knotbot-pipeline-arrow">→</div>
          <div class="knotbot-pipeline-step">
            <div class="knotbot-pipeline-num">04</div>
            <div class="knotbot-pipeline-content">
              <strong>Best-Fit Lines</strong>
              <p>SVD over color blob point clouds produces best-fit line axes and orientations. Broadcast as TF frames for MoveIt targeting.</p>
            </div>
          </div>
          <div class="knotbot-pipeline-arrow">→</div>
          <div class="knotbot-pipeline-step">
            <div class="knotbot-pipeline-num">05</div>
            <div class="knotbot-pipeline-content">
              <strong>Goal Computation</strong>
              <p>Geometric algorithms compute step-specific waypoints (reflections, intersections, midpoints) and publish them as RViz markers + TF frames.</p>
            </div>
          </div>
        </div>

        <div class="knotbot-media-grid" style="margin-top:2rem;">
          <div class="knotbot-placeholder knotbot-image-placeholder">
            <div class="knotbot-placeholder-icon">🖼</div>
            <div class="knotbot-placeholder-label">RViz Visualization</div>
            <div class="knotbot-placeholder-sub">Screenshot of color detection overlays and step goal markers in RViz — drop image here</div>
          </div>
          <div class="knotbot-placeholder knotbot-image-placeholder">
            <div class="knotbot-placeholder-icon">🖼</div>
            <div class="knotbot-placeholder-label">Camera View with Detections</div>
            <div class="knotbot-placeholder-sub">Raw camera feed with bounding boxes for each detected color — drop image here</div>
          </div>
        </div>
    design:
      columns: '1'
      background:
        color:
          light: "#f5f5f5"
          dark: "#08080c"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]

  # Tech Stack
  - block: tech-stack
    id: tech
    content:
      title: "Tech Stack"
      subtitle: "Technologies powering KnotBot"
      categories:
        - name: Robotics
          items:
            - name: ROS2
              icon: devicon/ros
            - name: MoveIt
              icon: devicon/ros
            - name: UR7e Robot Arm
              icon: devicon/linux
            - name: RealSense D435
              icon: devicon/linux
        - name: Computer Vision
          items:
            - name: OpenCV
              icon: devicon/opencv
            - name: NumPy
              icon: devicon/numpy
            - name: cv_bridge
              icon: devicon/python
            - name: ArUco Markers
              icon: devicon/python
        - name: Programming
          items:
            - name: Python
              icon: devicon/python
            - name: C++
              icon: devicon/cplusplus
            - name: YAML / Launch
              icon: devicon/yaml
        - name: Tools
          items:
            - name: Docker
              icon: devicon/docker
            - name: GitHub
              icon: brands/github
            - name: RViz
              icon: devicon/linux
            - name: Ubuntu 22.04
              icon: devicon/ubuntu
    design:
      style: grid
      show_levels: false
      background:
        color:
          light: "#ffffff"
          dark: "#0d0d12"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]

  # Media Gallery
  - block: markdown
    id: gallery
    content:
      title: "Media Gallery"
      subtitle: "Photos and videos of KnotBot"
      text: |-
        <div class="knotbot-media-grid knotbot-gallery-grid">
          <div class="knotbot-placeholder knotbot-image-placeholder">
            <div class="knotbot-placeholder-icon">🖼</div>
            <div class="knotbot-placeholder-label">Robot Setup</div>
            <div class="knotbot-placeholder-sub">UR7e arm with RealSense camera mounted at wrist — drop image here</div>
          </div>
          <div class="knotbot-placeholder knotbot-image-placeholder">
            <div class="knotbot-placeholder-icon">🖼</div>
            <div class="knotbot-placeholder-label">Workspace Layout</div>
            <div class="knotbot-placeholder-sub">Table with colored tape markers and rope — drop image here</div>
          </div>
          <div class="knotbot-placeholder knotbot-image-placeholder">
            <div class="knotbot-placeholder-icon">🖼</div>
            <div class="knotbot-placeholder-label">Grasp in Progress</div>
            <div class="knotbot-placeholder-sub">Robot gripper grasping rope end (Step 2) — drop image here</div>
          </div>
          <div class="knotbot-placeholder knotbot-image-placeholder">
            <div class="knotbot-placeholder-icon">🖼</div>
            <div class="knotbot-placeholder-label">Knot Completion</div>
            <div class="knotbot-placeholder-sub">Finished overhand knot after Step 5 — drop image here</div>
          </div>
          <div class="knotbot-placeholder knotbot-video-placeholder">
            <div class="knotbot-placeholder-icon">▶</div>
            <div class="knotbot-placeholder-label">Step-by-Step Breakdown</div>
            <div class="knotbot-placeholder-sub">Slow-motion overview (steps 0–6); per-step clips are in How It Works — drop video here</div>
          </div>
          <div class="knotbot-placeholder knotbot-image-placeholder">
            <div class="knotbot-placeholder-icon">🖼</div>
            <div class="knotbot-placeholder-label">Team Photo</div>
            <div class="knotbot-placeholder-sub">Joshua Mata and Allison Dana — drop image here</div>
          </div>
        </div>
    design:
      columns: '1'
      background:
        color:
          light: "#f5f5f5"
          dark: "#08080c"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]

  # Team
  - block: markdown
    id: team
    content:
      title: "The Team"
      subtitle: "EE 106A Project — Spring 2026, UC Berkeley"
      text: |-
        <h3 class="knotbot-section-subheading">Education</h3>
        <div class="knotbot-team-card knotbot-education-card">
          <div class="knotbot-team-info knotbot-education-body">
            <h3 class="knotbot-education-degree">EE 106A — Introduction to Robotics</h3>
            <p class="knotbot-education-meta">UC Berkeley · Jan 2026 – May 2026</p>
            <p class="knotbot-education-summary">Final project: KnotBot — an autonomous rope knot-tying robot. Team: Joshua Mata, Allison Dana.</p>
          </div>
        </div>
        <h3 class="knotbot-section-subheading knotbot-section-subheading-spaced">Team</h3>
        <div class="knotbot-team-grid">
          <div class="knotbot-team-card">
            <div class="knotbot-team-avatar">
              <div class="knotbot-avatar-placeholder">JM</div>
            </div>
            <div class="knotbot-team-info">
              <h3>Joshua Mata</h3>
              <p>Applied Mathematics, Minor Mechanical Engineering</p>
            </div>
          </div>
          <div class="knotbot-team-card">
            <div class="knotbot-team-avatar">
              <div class="knotbot-avatar-placeholder">AD</div>
            </div>
            <div class="knotbot-team-info">
              <h3>Allison Dana</h3>
              <p>Electrical Engineering and Computer Science</p>
            </div>
          </div>
        </div>
    design:
      columns: '1'
      background:
        color:
          light: "#ffffff"
          dark: "#0d0d12"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]

  # CTA / Links
  - block: cta-card
    content:
      title: "Explore the Code & Report"
      text: |-
        View the full source code, technical report, and additional documentation for KnotBot.
      button:
        text: 'View on GitHub'
        url: 'https://github.com'
        new_tab: true
    design:
      card:
        css_class: 'bg-gradient-to-br from-primary-200 via-primary-100 to-secondary-200 dark:from-primary-600 dark:via-primary-700 dark:to-secondary-700'
        text_color: dark
      background:
        color:
          light: "#f5f5f5"
          dark: "#08080c"
      spacing:
        padding: ["4rem", "0", "6rem", "0"]
---
