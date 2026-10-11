# Architecture

## End-to-end structure

```text
                 PART A — REAL-IMAGE PERCEPTION
phone video → camera calibration → pySLAM monocular pipeline
           → sparse point cloud + camera trajectory → scoring / 3D rendering

                 PART B — SIMULATED AUTONOMY
Gazebo TurtleBot3 + RGB-D camera + wheel odometry
          → RTAB-Map → occupancy grid and map↔odom transform
          → Nav2 → global/local path planning and recovery → goal motion

                 PRESENTATION LAYER
verified recorded state → DataBridge → Zustand store → React dashboard
```

## Part A: visual-SLAM pipeline

1. A phone records a steady loop through a visually textured environment.
2. Chessboard images are processed by `scripts/calibrate_camera.py` to estimate intrinsics and lens distortion.
3. `scripts/make_pyslam_settings.py` converts the calibration output into pySLAM settings; this conversion is required because pySLAM and ORB-SLAM3-style YAML use different formats.
4. pySLAM tracks camera motion, selects keyframes, triangulates sparse map points, and produces a trajectory.
5. `scripts/eval_trajectory.py` evaluates benchmark trajectories after Umeyama Sim(3) alignment. `scripts/render_map_3d.py` can produce a visual map rendering.

Monocular SLAM observes geometry through parallax. It works best with smooth forward movement, textured static surfaces, consistent camera settings, and gradual turns. It is weakened by featureless surfaces, heavy motion blur, abrupt rotations, moving crowds, and uncalibrated or changing lenses.

## Part B: ROS/Gazebo navigation pipeline

The Docker image begins from `osrf/ros:humble-desktop-full` and installs RTAB-Map, Nav2, TurtleBot3, Gazebo integrations, teleoperation, image viewing, and TF tools. `docker/patch_depth_camera.py` changes the stock simulated Waffle model to provide depth output, because its stock camera is RGB-only.

| Component | Responsibility |
|---|---|
| Gazebo | Physics, world, robot, synthetic RGB-D camera, and wheel-encoder odometry. |
| TurtleBot3 Waffle | The simulated mobile base used for the demo. |
| RTAB-Map | Consumes RGB-D data and odometry to construct mapping/localization information and an occupancy grid. |
| `pick_goal.py` | Reads the live grid and finds a target in known free space. |
| Nav2 | Plans to the requested map-frame goal, controls the base, avoids obstacles, and invokes recoveries when necessary. |
| TF2 | Maintains frame relationships, especially the map-to-odom relationship needed by navigation. |

## Coordinate-frame boundary

There are separate coordinate systems in the project.

| Source | Frame meaning | Can it be directly combined? |
|---|---|---|
| Phone pySLAM | Arbitrary monocular reconstruction frame with unknown metric scale | No: it has no calibrated transform into the simulator. |
| Gazebo/RTAB-Map | ROS `map` frame based on the simulated environment | Yes: Nav2 consumes this frame inside the simulation. |
| Gazebo odometry | ROS `odom` frame produced from simulated wheel encoders | Used by RTAB-Map/Nav2 through TF, but is not phone visual odometry. |

This separation avoids a misleading claim. Part A is genuinely camera-only monocular perception. Part B is GPS-denied navigation with RGB-D mapping plus wheel odometry.

## Mission-control dashboard

The `frontend/` application uses React, TypeScript, Vite, Tailwind CSS, and Zustand. It presents:

- Occupancy grid, robot pose, planned route, and traveled route.
- Mission state, commanded goal, distance remaining, and recovery count.
- Visual-SLAM benchmark status, keyframes, map points, resets, loop closures, and APE.
- ROS/Gazebo/RTAB-Map/Nav2/camera/SLAM health summaries.
- A timestamped mission timeline and control area.

`DataBridge` is a deliberate interface boundary. `MockDataBridge` currently supplies a repeatable, animated display of numbers from the verified run. A future `RosDataBridge` should subscribe to ROS topics and send Nav2 goal/cancel actions while leaving the React panels unchanged.

