# Andromeda / Sightline — Project Details

## 1. Project in one sentence

Andromeda (the project name) / Sightline (the demo and dashboard name) is a hardware-free proof of concept for a GPS-denied autonomous ground vehicle: it demonstrates camera-based visual SLAM on real phone footage and, separately, autonomous mapping and goal navigation in a ROS 2/Gazebo simulation.

It was built for an overnight university hackathon leading into Smart India Hackathon (SIH), with a deliberate emphasis on reproducibility and honest, measurable claims.

## 2. The problem it addresses

Outdoor UGVs used in search and rescue, agriculture, inspection, and delivery cannot always depend on GPS. Satellite signals may be weak or unavailable indoors, beneath tree cover, beside tall buildings, in tunnels, or under intentional jamming. A rover still needs to understand where it is, build a representation of its surroundings, and move safely to a destination.

The project explores a practical answer: use onboard vision to recover motion and map structure, then use a robotics navigation stack to plan and execute a route without GPS.

## 3. What is actually demonstrated

The proof of concept has two complementary parts.

| Part | Input | Output | What it proves |
|---|---|---|---|
| **A — Visual SLAM** | Real, pre-recorded monocular phone video | Sparse 3D point map and camera trajectory | A single moving camera can track motion and reconstruct qualitative 3D structure without GPS or a depth sensor. |
| **B — Autonomous navigation** | Gazebo TurtleBot3 simulation with RGB-D camera and wheel odometry | Occupancy grid, planned path, obstacle-aware goal navigation | The mapping-to-navigation loop can autonomously drive a robot to a goal without GPS. |
| **Mission-control UI** | Verified recorded metrics, presented through a mock data bridge | Map, telemetry, SLAM status, health, timeline and controls | A clear operator-facing presentation layer that is designed to later accept live ROS data. |

These two halves do **not** share a map or coordinate frame. That is intentional and documented: the phone demonstration validates camera-only perception on real imagery, while the simulator demonstrates a reliable live autonomy loop. The project does not imply that the simulation is driven purely by monocular vision.

## 4. System flow

```text
Part A: Phone video
Phone camera → calibration/settings → pySLAM monocular tracking
             → sparse map + trajectory → trajectory evaluation / 3D rendering

Part B: Gazebo simulation
TurtleBot3 RGB-D camera + wheel odometry → RTAB-Map → occupancy grid / map→odom transform
                                                     → Nav2 → planned path → robot goal navigation

Presentation
Verified run data → DataBridge interface → React/Zustand dashboard → operator view
```

## 5. Tools and technologies used

### Robotics, mapping, and navigation

| Tool | Why it is used in this project |
|---|---|
| **ROS 2 Humble** | The robotics middleware connecting sensors, mapping, transforms, planning, and robot control. It is the foundation of the simulated autonomy stack. |
| **Gazebo Classic 11** | Simulates the robot, environment, sensors, and obstacle-filled world without requiring physical rover hardware. |
| **TurtleBot3 Waffle** | A widely supported ROS robot model used as the rover stand-in. It keeps the prototype lightweight and easy to reproduce. |
| **RTAB-Map ROS** | Builds the simulation occupancy grid, performs RGB-D mapping/loop-closure capabilities, and supplies map/localization information that Nav2 consumes. |
| **Nav2** | ROS 2’s navigation framework. It accepts a goal pose, plans a route through free space, drives the robot, avoids obstacles, and performs recovery behaviours when needed. |
| **TF2** | ROS transform framework used to relate coordinate frames such as `map` and `odom`; debugging tools are included in the container. |
| **teleop_twist_keyboard** | Available in the container for manual robot control and setup/debugging. |
| **rqt_image_view** | Available for viewing camera streams while debugging the simulated robot. |

### Visual SLAM and computer vision

| Tool | Why it is used in this project |
|---|---|
| **pySLAM (`luigifreda/pyslam`)** | The implemented monocular visual-SLAM engine. It processes the phone clips and KITTI sequence, producing trajectories and sparse maps. It is cloned and built from source rather than installed from PyPI. |
| **C++ SLAM core with GTSAM frontend pose optimisation** | The default, faster evaluated configuration. A project patch switches to GTSAM because the tested g2o path was inert in this build. |
| **OpenCV** | Detects chessboard calibration corners and estimates camera intrinsics and distortion. It is also central to the vision ecosystem used by SLAM. |
| **ORB-SLAM3 format compatibility tooling** | `calibrate_camera.py` exports ORB-SLAM3-style calibration; `make_pyslam_settings.py` converts it to a pySLAM-loadable configuration because their YAML formats differ. |
| **KITTI Odometry sequence 06** | A benchmark sequence used to score the visual-SLAM trajectory against known ground truth. |
| **NumPy** | Numeric operations for host-side calibration and trajectory analysis scripts. |
| **PyYAML** | Reads and validates generated pySLAM settings. |
| **Umeyama Sim(3) alignment** | Evaluates monocular trajectories fairly: monocular SLAM has unknown global scale, so trajectories are aligned before absolute pose error is calculated. |

### Platform, packaging, and demo reliability

| Tool | Why it is used in this project |
|---|---|
| **Docker** | Packages ROS, Gazebo, RTAB-Map, Nav2, TurtleBot3, and debug tools into a repeatable image, preventing venue-time dependency installation. |
| **`osrf/ros:humble-desktop-full`** | Docker base image containing the ROS 2 desktop/Gazebo environment. |
| **NVIDIA Container Toolkit / Docker GPU support** | Passes the RTX GPU into the container so Gazebo rendering remains usable. |
| **NVIDIA PRIME render-offload environment variables** | Force rendering onto the RTX 4060 instead of the AMD integrated GPU on the hybrid-graphics host. This prevents a silent but severe Gazebo slowdown. |
| **X11/XWayland GUI passthrough** | Allows Gazebo, RViz, and other container GUI applications to appear on the Fedora host. |
| **GNOME D-Bus screen-cast API** | Used by `record_screen.py`; unlike `x11grab`, it works under Wayland/XWayland. |
| **Git commit pinning and patch scripts** | Lock the pySLAM source revision and reject incompatible versions so a later upstream change cannot silently invalidate the result. |

### Operator dashboard

| Tool | Why it is used in this project |
|---|---|
| **React 18** | Builds the mission-control interface from reusable components. |
| **TypeScript** | Provides typed application state and safer component/data-bridge integration. |
| **Vite** | Fast local development and production front-end builds. |
| **Zustand** | Lightweight state store for robot, mission, map, perception, health, and timeline state. |
| **Tailwind CSS** | Styles the responsive dark operator dashboard. |
| **DataBridge abstraction** | Separates the UI from its data source. The current `MockDataBridge` replays verified demo metrics; a future `RosDataBridge` can subscribe to live ROS topics without rewriting visual components. |

## 6. Important project scripts

| Script / area | Purpose |
|---|---|
| `scripts/run_demo.sh` | Starts the Docker demo with the tested GPU, GUI, network, and workspace-mount settings. |
| `scripts/demo_day.sh` | Helps operate the live demonstration workflow. |
| `scripts/check_gpu.sh` | Confirms the container uses NVIDIA rather than the integrated AMD GPU. |
| `scripts/calibrate_camera.py` | Calibrates the phone camera from chessboard images. |
| `scripts/make_pyslam_settings.py` | Converts calibration output into validated pySLAM settings. |
| `scripts/estimate_focal_vp.py` | Estimates focal length from orthogonal vanishing points when proper calibration imagery is unavailable; this is explicitly lower confidence. |
| `scripts/eval_trajectory.py` | Aligns and scores SLAM trajectories against ground truth. |
| `scripts/render_map_3d.py` | Renders a 3D visualization of a produced sparse map. |
| `scripts/pick_goal.py` | Reads the live occupancy grid and selects a goal demonstrably in free space, avoiding fragile hand-picked coordinates. |
| `scripts/record_screen.py` | Records the Wayland desktop using GNOME’s screen-cast interface. |
| `scripts/set_dataset.py` | Switches the visual-SLAM input dataset/clip. |
| `scripts/make_gt_demo.py`, `scripts/make_slam_demo.py` | Produce demonstration media from ground-truth and SLAM outputs. |
| `scripts/pyslam_build/` | Bootstrap, dependency, and staged source-build scripts for the pinned pySLAM environment. |
| `scripts/standin/` | Creates synthetic stand-in video/calibration assets to rehearse the pipeline before real footage exists; not part of the final evidence. |
| `docker/patch_depth_camera.py` | Modifies the simulated Waffle model to add depth-camera output required by RTAB-Map RGB-D mapping. |

## 7. Measured results

All figures below are recorded in the repository runbook rather than estimated.

### Visual SLAM: KITTI sequence 06

The sequence contains 1,101 frames and 1,222 m of travel. Results use Sim(3)-aligned absolute pose error, appropriate for monocular SLAM.

| Engine | Seconds/frame | APE RMSE | Error as path percentage | Tracking |
|---|---:|---:|---:|---|
| C++ core + GTSAM (default) | 0.04 | 49.5 m | 4.05% | 1,101/1,101 frames; 1 lost event (0.09%) |
| Python core fallback | 0.19 | 132.7 m | 10.86% | 1,101/1,101 frames; 1 lost event (0.09%) |

The evaluated map contains 321 keyframes and 11,893 map points, with no SLAM resets.

### Phone clips

Phone clips demonstrate tracking survival, not metric accuracy, because no ground-truth trajectory exists.

| Clip | Conditions | Tracking | Important interpretation |
|---|---|---|---|
| A | 65 s covered walkway, 848×478 | 1,953/1,953 frames; 1 lost | 1,938 online poses → 1,889 final poses |
| B | 126 s indoor corridor, 1280×544 | 3,771/3,771 frames; 3 lost | 3,707 online poses → 1,648 final poses after failed relocalization |

### Autonomous navigation: Gazebo `turtlebot3_house`

- Occupancy grid grew from 78×53 to 202×205 cells at 0.05 m/cell.
- Commanded goal: `(-6.21, 0.63)` in the map frame.
- Robot moved from `(-6.40, -1.86)` to `(-6.36, 0.47)`.
- Final distance to goal: **0.21 m**, within the Nav2 goal tolerance.
- Nav2 recovery behaviours: **3**. They are shown rather than hidden, because recovery is a normal part of autonomous navigation.

## 8. What makes this project different and strong

1. **It is honest about the boundary of each demo.** The phone SLAM and simulation do not pretend to be one shared, live production stack. The simulator is GPS-denied, but it uses simulated wheel odometry together with RGB-D mapping; it is not claimed as camera-only navigation.

2. **It reports limitations alongside successes.** Monocular scale ambiguity, missing loop closures, noise-free simulated depth, untested extreme conditions, and the weak phone-clip calibration are all stated plainly. This makes the positive results more credible to technical judges.

3. **It uses real imagery, not only a simulation.** The phone sequence demonstrates sparse-map reconstruction and tracking on actual captured video, while simulation provides a repeatable autonomy demonstration.

4. **It is engineered for a hackathon environment.** Containers, pinned dependencies, staged builds, recorded backups, offline Gazebo model handling, and hardware-specific GPU checks reduce the chance of a demo failure caused by setup rather than robotics.

5. **It measures performance.** It includes a benchmark, the correct monocular alignment method, tracking statistics, map size, occupancy-grid growth, final-goal distance, and recovery count.

6. **It anticipates operator needs.** The dashboard makes robot state, map, path, SLAM quality, system health, and mission events visible in one interface. Its bridge architecture keeps a future live ROS integration contained.

7. **It makes small but valuable reliability fixes.** Examples include free-space goal selection instead of guessing a map coordinate, a depth-camera patch for the actual TurtleBot3 model, Wayland-compatible recording, and an explicit check for incorrect GPU selection.

## 9. Real-life applications

| Application | How this approach helps |
|---|---|
| **Search and rescue** | A rover can explore building interiors, covered areas, debris zones, or GPS-poor disaster sites while creating a map for operators. |
| **Precision agriculture** | Navigation and environmental mapping can support crop-row inspection, scouting, targeted spraying, and movement under canopy where GPS is degraded. |
| **Last-mile delivery** | Local mapping and obstacle-aware routing can help small robots move through campuses, warehouses, covered walkways, and urban GPS-shadow zones. |
| **Industrial inspection** | Robots can inspect plants, warehouses, tunnels, pipelines, and construction sites where a prior map is incomplete or satellite signals are absent. |
| **Security and patrol** | A rover can autonomously revisit routes, detect obstacles, and provide a map/telemetry view in facilities and campuses. |
| **Mining, tunnels, and underground operations** | GPS is unavailable underground; visual, inertial, wheel, and depth sensing are relevant building blocks for localization and mapping. |
| **Disaster assessment** | Rapid mapping of unfamiliar areas can give responders a safer understanding of access paths and blocked regions before sending people in. |

## 10. Known limitations and responsible claims

- A monocular camera cannot recover absolute metric scale by itself. Phone-map distances are therefore qualitative, not surveyed measurements.
- The phone clips have no ground-truth trajectory; their reported figures measure tracking survival rather than trajectory accuracy.
- The project’s loop-closure candidates currently fail RANSAC Sim(3) geometry verification. Drift remains, reflected in the 4.05% KITTI path error.
- The simulation uses Gazebo wheel odometry and noise-free synthetic depth. Real robots will have wheel slip, sensor noise, lighting variation, motion blur, weather, and terrain effects.
- The currently recorded phone footage was shot before sunrise/under cover and indoors rather than to the intended daylight outdoor brief. A proper daylight reshoot remains open work.
- The React dashboard currently uses `MockDataBridge`, which animates verified recorded data. It is not yet directly subscribed to ROS topics.
- The current proof of concept has not been field-tested on a physical outdoor rover.

## 11. Next development steps

1. Collect calibrated daylight outdoor footage and run the third recorded phone take.
2. Diagnose and repair loop-closure geometry verification to reduce long-run drift.
3. Connect the dashboard’s planned `RosDataBridge` to live ROS topics and Nav2 actions.
4. Move from simulated depth to an outdoor-suitable stereo or robust depth setup; structured-light/IR depth can be unreliable in sunlight.
5. Fuse visual, inertial, and wheel-odometry data using systems such as VINS-Fusion and `robot_localization`.
6. Evaluate learned features such as SuperPoint, ALIKED, and LightGlue for changing-light robustness.
7. Add dense reconstruction, traversability, and elevation maps for rough-terrain decisions.
8. Deploy to a rugged UGV platform such as a Clearpath Husky or Jackal and perform field trials.

## 12. Repository guide

| Path | Contents |
|---|---|
| `README.md` | Project summary, results, claims, caveats, reproduction overview, and roadmap. |
| `SPEC.md` | Original decisions, rationale, risks, rejected alternatives, and hackathon handoff context. |
| `RUNBOOK.md` | Reproducible commands, verification status, and run notes. |
| `docs/CAMERA_DAY.md` | Plain-language capture guide for collecting phone footage and calibration images. |
| `docker/` | ROS/Gazebo Docker image and simulated depth-camera patch. |
| `patches/` | Required pySLAM fixes and their rationale. |
| `scripts/` | Calibration, evaluation, goal selection, recording, rendering, and demo automation. |
| `frontend/` | Sightline Mission Control web dashboard. |
| `media/` | Demonstration videos, calibration assets, and generated media. |

## 13. Short pitch

Andromeda shows a credible path toward GPS-denied UGV autonomy. A real phone camera sequence demonstrates monocular visual SLAM and sparse 3D mapping, while a ROS 2 simulation demonstrates the full map-to-goal-navigation loop with RTAB-Map and Nav2. Rather than overstating a prototype, the project makes its sensor assumptions, benchmarked accuracy, simulation boundaries, and field-test gaps visible. That combination of technical depth, measurable evidence, reproducible engineering, and a clear route to real deployments is its core strength.

