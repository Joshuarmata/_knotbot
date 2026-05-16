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
        transformations needed for each knot-tying phase, and executes a **six-phase** manipulation
        sequence (steps 0–5) using **MoveIt** motion planning and **ROS2** visual servoing.

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
        <div class="max-w-5xl mx-auto mb-10">
          <div class="knotbot-youtube-short knotbot-youtube-short--demo-wide">
            <iframe
              src="https://www.youtube-nocookie.com/embed/gUvkJyTCKFg?rel=0"
              title="KnotBot — full knot-tying demo"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
              allowfullscreen
              loading="lazy"></iframe>
          </div>
          <p class="text-center text-sm text-gray-500 dark:text-gray-400 mt-3">
            <a href="https://youtube.com/shorts/gUvkJyTCKFg" class="underline hover:text-primary-600 dark:hover:text-primary-400" target="_blank" rel="noopener">Watch on YouTube</a>
          </p>
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
      subtitle: "Six phases (steps 0–5): geometry on the arm, knot-step labels from vision"
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
      subtitle: "From raw pixels to 3D robot poses — and global knot-step labels"
      text: |-


        <div class="knotbot-pipeline">
          <div class="knotbot-pipeline-step">
            <div class="knotbot-pipeline-num">01</div>
            <div class="knotbot-pipeline-content">
              <strong>Color Segmentation</strong>
              <p>HSV masking isolates yellow, red, green, and blue rope/tape. We experiened a lot of noise, so we put caps on the max size and min size of the bounding boxes we were detecting.</p>
            </div>
          </div>
          <div class="knotbot-pipeline-arrow">→</div>
          <div class="knotbot-pipeline-step">
            <div class="knotbot-pipeline-num">02</div>
            <div class="knotbot-pipeline-content">
              <strong>Depth Estimation</strong>
              <p>Aligned RealSense depth stream provides Z values per blob. We then used an ArUco marker to calibrate the table-plane Z to counteract the camera warping.</p>
            </div>
          </div>
          <div class="knotbot-pipeline-arrow">→</div>
          <div class="knotbot-pipeline-step">
            <div class="knotbot-pipeline-num">03</div>
            <div class="knotbot-pipeline-content">
              <strong>3D Localization</strong>
              <p>We then expressed the pose in the base frame of our UR7e.</p>
            </div>
          </div>
          <div class="knotbot-pipeline-arrow">→</div>
          <div class="knotbot-pipeline-step">
            <div class="knotbot-pipeline-num">04</div>
            <div class="knotbot-pipeline-content">
              <strong>Best-Fit Lines</strong>
              <p>We used a best-fit line axes across the point cloud to determine pose and orientations of the tape for more accurate orientation.</p>
            </div>
          </div>
          <div class="knotbot-pipeline-arrow">→</div>
          <div class="knotbot-pipeline-step">
            <div class="knotbot-pipeline-num">05</div>
            <div class="knotbot-pipeline-content">
              <strong>Goal Computation</strong>
              <p>Geometric algorithms compute step-specific waypoints (reflections, intersections, midpoints) and publish RViz markers + TF frames—the quantities the servo loop actually tracks each cycle. So if we move the rope, the goal moves as well. This allows us to recalculate the rope pose after every knot step. </p>
            </div>
          </div>
        </div>

        <h3 class="text-lg font-bold text-gray-900 dark:text-white text-center mt-10 mb-2">Geometric perception in RViz</h3>
        <p class="text-center text-sm text-gray-600 dark:text-gray-400 max-w-3xl mx-auto mb-6">
  
        </p>
        <div class="knotbot-media-grid knotbot-gallery-grid" style="margin-top:0;">
          <figure class="knotbot-vision-figure">
            <img src="media/visual-pipeline/rviz-bounding-boxes.png" alt="RViz screenshot: bounding boxes around color-detected rope and tape regions" width="960" height="540" loading="lazy" decoding="async" />
            <figcaption><strong>2D segmentation</strong> — HSV-threshold blobs become axis-aligned bounding boxes in the overhead view before depth back-projection.</figcaption>
          </figure>
          <figure class="knotbot-vision-figure">
            <img src="media/visual-pipeline/rviz-point-cloud-from-bboxes.png" alt="RViz screenshot: sparse 3D point cloud generated inside each bounding box region" width="960" height="540" loading="lazy" decoding="async" />
            <figcaption><strong>3D points per color</strong> — pixels inside each box are lifted with calibrated intrinsics + RealSense depth (with ArUco table/plane fallback).</figcaption>
          </figure>
          <figure class="knotbot-vision-figure">
            <img src="media/visual-pipeline/rviz-line-best-fit.png" alt="RViz screenshot: principal line of best fit through a rope color point cloud" width="960" height="540" loading="lazy" decoding="async" />
            <figcaption><strong>SVD axis</strong> — a line of best fit through each rope/tape cluster defines the tangent orientation published as TF for grasp alignment.</figcaption>
          </figure>
        </div>

        <h3 class="text-lg font-bold text-gray-900 dark:text-white text-center mt-10 mb-3">BONUS: Knot Step Recognition with Language Model</h3>
        <p class="text-center text-sm text-gray-600 dark:text-gray-400 max-w-2xl mx-auto mb-4">
          OpenCV: predicted <strong>step</strong> + confidence, plus <strong>rope_heuristic_score</strong> from the rope mask—parallel readouts to the geometric overlays used for visual servoing. We only had time to train a model on the first two steps, but we are including these as a proof of concept!
        </p>
        <div class="knotbot-media-grid" style="margin-top:0;">
          <figure class="knotbot-vision-figure">
            <img src="media/visual-pipeline/rope-knot-infer-step0.png" alt="rope_knot_infer: classified as step0 with confidence 0.89 and rope_heuristic_score 0.774" width="960" height="540" loading="lazy" decoding="async" />
            <figcaption><strong>step0</strong> — classifier confidence 0.89; rope mask heuristic 0.774. Yellow workspace mask highlights the segmented region used with the white rope and colored tape landmarks.</figcaption>
          </figure>
          <figure class="knotbot-vision-figure">
            <img src="media/visual-pipeline/rope-knot-infer-step1.png" alt="rope_knot_infer: classified as step1 with confidence 0.74 and rope_heuristic_score 0.947" width="960" height="540" loading="lazy" decoding="async" />
            <figcaption><strong>step1</strong> — confidence 0.74; heuristic 0.947 on a crossed loop configuration. Same camera stack as geometric blob tracking, framed for full-frame phase recognition.</figcaption>
          </figure>
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
            - name: PyTorch
              icon: devicon/python
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

  # Team
  - block: markdown
    id: team
    content:
      title: "The Team"
      subtitle: "EE 106A Project — Spring 2026, UC Berkeley"
      text: |-
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

        <aside class="knotbot-team-acknowledgements" aria-label="AI and contributor acknowledgements">
          <h4 class="knotbot-team-acknowledgements-title">AI and other contributor acknowledgements</h4>
          <p>We disclose our use of AI-assisted development tools for debugging and early function scaffolding—in-editor assistants and Claude Code supported early iterations.</p>
          <p>Notable areas that incorporated AI-assisted edits include the <code>bbox</code> routine that groups detected pixels into bounding boxes (we also consulted Stack&nbsp;Exchange examples), and early structure around step&nbsp;1 of the knot sequence (built heavily on Lab&nbsp;7). Those pieces served as scaffolding we extended ourselves.</p>
          <p>Much of the trajectory code derives from Lab&nbsp;7; we relied on AI primarily as a debugging aid when integrations failed. Additionally, this website was built upon a template designed by HugoBlox</p>
        </aside>
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
