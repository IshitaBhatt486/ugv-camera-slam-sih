# Reproduction Guide

This guide gives the safe order for reproducing the project. For exact commands and recorded observations, use [RUNBOOK.md](../RUNBOOK.md).

## Prerequisites

- Fedora host with Docker and NVIDIA Container Toolkit.
- NVIDIA RTX-capable GPU; the reference host is an RTX 4060 laptop with an AMD integrated GPU.
- GUI session that supports the project’s XWayland/X11 container passthrough.
- Python 3 host environment for calibration and evaluation utilities.
- Sufficient local disk space for the container, pySLAM source build, datasets, and media. The vendor tree is intentionally not version-controlled.

## Recommended order

1. **Build the simulation image.** Build the image from `docker/` as `ugv-slam:demo`.
2. **Start the container through `scripts/run_demo.sh`.** Do not hand-copy its flags: it includes the verified display, GPU, mount, and hybrid-graphics settings.
3. **Verify GPU selection.** Run `scripts/check_gpu.sh` inside the container. It must report NVIDIA. A wrong GPU selection can make Gazebo crawl without a useful error.
4. **Run the Gazebo / RTAB-Map / Nav2 sequence.** Follow the simulation sections of the runbook and verify ROS topics before assuming a camera topic name.
5. **Choose a goal with `scripts/pick_goal.py`.** This checks the live occupancy grid and avoids sending Nav2 a coordinate inside a wall or unknown space.
6. **Build visual SLAM at home, not at the event.** Run `scripts/pyslam_build/00_bootstrap.sh`; it creates the pinned, patched pySLAM environment. This build involves native dependencies and should not be treated as an on-stage step.
7. **Calibrate the phone camera.** Follow [CAMERA_DAY.md](CAMERA_DAY.md), then run `scripts/calibrate_camera.py` on the chessboard images.
8. **Convert settings for pySLAM.** Run `scripts/make_pyslam_settings.py`; camera calibration output is not directly accepted by pySLAM.
9. **Evaluate a benchmark run.** Use `scripts/eval_trajectory.py` with a compatible ground-truth trajectory.
10. **Record presentation media if needed.** Under Wayland, use `scripts/record_screen.py` rather than X11-root screen capture.

## Required checks

| Check | Expected result | Why it matters |
|---|---|---|
| GPU check | NVIDIA renderer is active | Hybrid hosts may otherwise use the integrated GPU. |
| Container start | Gazebo GUI opens and ROS environment is sourced | Confirms graphics and runtime setup. |
| Camera topics | Actual available topic names confirmed | TurtleBot3 packages can vary by version. |
| Occupancy map | Grid grows while mapping | Confirms RTAB-Map receives useful sensor/odometry data. |
| Goal | Chosen free-space goal is accepted | Prevents false planner failures from invalid coordinates. |
| Calibration RMS | Review output before SLAM | Bad intrinsics can invalidate tracking and reconstruction. |
| Benchmark evaluation | Sim(3)-aligned result generated | Prevents invalid direct-scale comparisons for monocular SLAM. |

## Frequent pitfalls

- Do not use the unrelated PyPI package named `pyslam`; the project uses the source repository built by its bootstrap scripts.
- Do not substitute the pySLAM source revision. The patch process is intentionally revision-locked.
- Do not assume the stock Waffle simulation has RGB-D; the Docker build performs the required depth-camera patch.
- Do not claim the simulated navigation is vision-only. It uses simulated wheel odometry.
- Do not use compressed chat-app video/photos for calibration or SLAM input.
- Do not turn on the spot while recording a monocular phone route; use gradual turns while moving.
- Do not use `x11grab` as the primary capture approach under Wayland; it can record a black screen while returning success.

## Demo-day practice

Keep a recorded fallback for both halves. Rehearse the narrative in this order: the GPS-denied problem, real-image visual SLAM, live map-to-goal navigation, the dashboard status, limitations, and the hardware roadmap. This preserves a credible technical story even if a GUI or venue-network issue occurs.

