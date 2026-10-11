# Technology Stack and Rationale

## Core stack

| Technology | Role | Reason for selection |
|---|---|---|
| ROS 2 Humble | Robotics middleware | Mature ROS 2 release with packages for mapping, navigation, simulation, transforms, and TurtleBot3. |
| Gazebo Classic 11 | Simulation | Provides a dependable, lightweight robotics simulator for a laptop-scale proof of concept. |
| TurtleBot3 Waffle | Robot model | Common ROS reference platform with existing simulation support and a practical mobile-base form factor. |
| RTAB-Map ROS | RGB-D mapping | ROS-native mapping system that can produce the occupancy grid consumed by Nav2. |
| Nav2 | Autonomous navigation | Standard ROS 2 navigation framework for planning, control, obstacle avoidance, recovery, and goal actions. |
| pySLAM | Monocular visual SLAM | Provides the real-video visual-SLAM pipeline, sparse mapping, and trajectory output. |
| GTSAM | Pose-graph optimisation frontend | Used by the faster default pySLAM configuration through a required project patch. |
| OpenCV | Calibration and computer vision | Supplies the camera-calibration workflow and is widely compatible with SLAM tooling. |

## Supporting engineering tools

| Technology | Role | Reason |
|---|---|---|
| Docker | Reproducible ROS environment | Avoids repeated venue installs and isolates Ubuntu-native robotics dependencies from the Fedora host. |
| NVIDIA Container Toolkit | GPU access in container | Allows GPU-backed Gazebo rendering and avoids poor software-rendering performance. |
| Git commit pinning | Dependency stability | pySLAM has no release-tag workflow suitable for this demo; pinning prevents unexpected upstream changes. |
| Patch gate | Safe local modifications | The patch script refuses mismatched upstream trees instead of applying a potentially invalid patch. |
| NumPy | Numeric scripts | Supports calibration/trajectory calculations. |
| PyYAML | Settings validation | Validates generated pySLAM configuration. |
| GNOME D-Bus screencast | Wayland recording | Captures the actual desktop under Wayland where X11-root capture may produce black output. |

## Front-end stack

| Technology | Role |
|---|---|
| React 18 | Component-based operator interface. |
| TypeScript | Typed state and interface contracts. |
| Vite | Development server and production build pipeline. |
| Zustand | Compact central application state. |
| Tailwind CSS | Responsive dashboard styling. |
| DataBridge | Adapter boundary between mock/recorded data and a future live ROS source. |

## Selection trade-offs

- **Gazebo Classic instead of Isaac Sim:** chosen for lower overhead and a reliable complete stack on the available RTX 4060 laptop.
- **TurtleBot3 instead of a Husky/Jackal:** chosen as a fast, supported simulation base; rugged outdoor robots are a field-deployment phase.
- **Monocular phone video instead of stereo phones:** uses readily available hardware and avoids synchronization, baseline-rigidity, and stereo-calibration failure modes. It sacrifices metric scale.
- **Pre-recorded visual SLAM instead of build-at-venue live SLAM:** source builds are too fragile for an overnight event. The navigation loop remains the live segment.
- **RGB-D simulation instead of pure visual odometry:** gives a stable live mapping demonstration. The project explicitly acknowledges that this differs from the camera-only phone experiment.

