# Results and Evaluation

## Evaluation principles

The project reports measured numbers only where an appropriate reference exists. Monocular camera SLAM has an unknown global scale, so benchmark trajectories are first aligned with ground truth using Umeyama Sim(3) alignment. Phone-video runs have no ground truth, so they are reported as tracking-survival evidence rather than metric accuracy.

## KITTI sequence 06: visual-SLAM benchmark

Sequence 06 contains 1,101 frames over 1,222 m of travel.

| Engine | Seconds per frame | APE RMSE | Error / path | Tracking |
|---|---:|---:|---:|---|
| C++ core + GTSAM default | 0.04 s | 49.5 m | 4.05% | 1,101/1,101 frames, 1 lost event (0.09%) |
| Python-core fallback | 0.19 s | 132.7 m | 10.86% | 1,101/1,101 frames, 1 lost event (0.09%) |

The default map contained 321 keyframes and 11,893 points, with no full SLAM resets.

### Interpretation

The default implementation is substantially faster and more accurate than the fallback. Its 4.05% path-relative error is a useful benchmark result but not a claim of centimeter-level localisation. The current loop-closure geometry verification rejects all candidate closures, so accumulated drift remains visible.

## Real phone footage

| Clip | Source conditions | Tracking survival | Online to final poses |
|---|---|---:|---:|
| A | 65 s covered walkway, 848×478 | 1,953/1,953 frames, 1 lost event | 1,938 → 1,889 |
| B | 126 s indoor corridor, 1280×544 | 3,771/3,771 frames, 3 lost events | 3,707 → 1,648 |

Clip B is an important example of transparent reporting: a failed relocalisation means that 56% of its online poses were not retained in the final map. A low lost-frame percentage alone would conceal that result, so both numbers are kept.

## Simulated autonomous navigation

| Measurement | Result |
|---|---|
| World | Gazebo `turtlebot3_house` |
| Occupancy grid growth | 78×53 → 202×205 cells at 0.05 m/cell |
| Goal | `(-6.21, 0.63)` in `map` frame |
| Start position | `(-6.40, -1.86)` |
| Final position | `(-6.36, 0.47)` |
| Final goal distance | 0.21 m, within Nav2 tolerance |
| Recovery behaviours | 3 |

The recovery count is retained because it shows the planner/controller responding to difficulty rather than presenting only a polished success state.

## Evaluation limitations

- The phone footage lacks a ground-truth route; no phone APE should be inferred.
- Monocular maps do not have metric scale without additional sensing or calibration references.
- Gazebo depth is noise-free and the simulated odometry does not represent real wheel slip.
- The recorded phone footage was not collected under the intended daylight outdoor capture protocol.
- A benchmark sequence and a simulated world cannot replace field testing on terrain, weather, dynamic obstacles, and changing illumination.

## Reproduction evidence

The exact commands, dates, observations, and verification status belong in [the runbook](../RUNBOOK.md). The evaluation script is `scripts/eval_trajectory.py`; its intended metric is documented in the repository README and runbook.

