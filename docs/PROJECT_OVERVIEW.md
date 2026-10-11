# Project Overview

## Purpose

Andromeda is a hardware-free proof of concept for a ground robot that must operate without dependable GPS. It addresses the core autonomy chain: observe the environment, estimate motion, build a map, plan a route, and reach a goal safely.

GPS is not reliable in all operating areas: indoors, below tree canopy, in tunnels, among tall buildings, at disaster sites, and in environments subject to interference or jamming. Search-and-rescue, agriculture, delivery, inspection, security, and underground operations all encounter these conditions.

## Demonstration design

The project deliberately uses two demonstrations because they validate different risks.

| Demonstration | Evidence | Value |
|---|---|---|
| **Part A: real-image visual SLAM** | Pre-recorded phone footage processed by monocular pySLAM | Shows that vision tracking and sparse 3D reconstruction work on real captured imagery without GPS or a depth sensor. |
| **Part B: simulated autonomy loop** | TurtleBot3 Waffle in Gazebo, RTAB-Map, and Nav2 | Shows a repeatable live sequence from mapping through goal selection, planning, obstacle avoidance, recovery, and arrival. |
| **Sightline dashboard** | React operator display fed by verified demo-state data | Communicates the state of the mission, perception, map, and platform in one view. |

## Why this is a strong proof of concept

- It combines actual visual-SLAM evidence with a live autonomous-navigation demonstration rather than relying only on a slide deck or only on simulation.
- It uses standard, extensible robotics building blocks: ROS 2, RTAB-Map, Nav2, Gazebo, and TurtleBot3.
- It contains quantitative evidence: benchmarked trajectory error, tracking survival, map statistics, occupancy-grid growth, goal error, and recovery count.
- It is reproducible: the ROS stack is containerized, the SLAM upstream revision is pinned, mandatory patches are version-gated, and setup risks are recorded.
- It is candid: scale ambiguity, simulation assumptions, current loop-closure failure, and lack of physical field tests are disclosed instead of hidden.

## Real-world relevance

| Scenario | Potential use |
|---|---|
| Search and rescue | Map a blocked or GPS-poor area before sending responders deeper inside. |
| Agriculture | Inspect crop rows and travel beneath canopy where GNSS quality varies. |
| Inspection | Navigate warehouses, plants, corridors, tunnels, and construction sites while recording environment context. |
| Last-mile delivery | Support local obstacle-aware movement around campuses and covered urban spaces. |
| Security | Repeat patrol routes and give operators a live map, route, and health status. |
| Mining and underground work | Provide a foundation for navigation where GPS is unavailable. |

## What it proves and does not prove

### Demonstrated

- A single moving camera can produce a qualitative sparse map and trajectory on real footage.
- A simulated rover can build an occupancy grid and autonomously reach a free-space goal with Nav2, without GPS.
- The project can present mission, mapping, perception, health, and timeline data through an operator UI.

### Not claimed

- The phone and simulation maps are not fused and do not share coordinates.
- The simulator is not camera-only navigation. RTAB-Map uses RGB-D imagery, while Gazebo wheel encoders provide `/odom`.
- Monocular phone SLAM does not return absolute real-world scale without an additional metric reference.
- The current implementation has not established robust performance under extreme lighting, rain, rough terrain, wheel slip, sensor noise, or real outdoor hardware.

## Future direction

The natural progression is calibrated daylight data, repaired loop closure, a live ROS dashboard bridge, visual-inertial and wheel-odometry fusion, outdoor-capable stereo/depth sensing, dense traversability mapping, and a physical rugged platform such as a Husky or Jackal.

