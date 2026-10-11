# Andromeda / Sightline Documentation

This folder is the project documentation hub for the GPS-denied UGV proof of concept. It is arranged for three audiences: judges and stakeholders, developers reproducing the demo, and future contributors moving it toward a physical rover.

## Start here

| Document | Audience | Contents |
|---|---|---|
| [Project overview](PROJECT_OVERVIEW.md) | Everyone | Problem, solution, demonstrations, value, applications, and responsible claims. |
| [Architecture](ARCHITECTURE.md) | Technical readers | End-to-end design, data flow, coordinate frames, dashboard structure, and boundaries. |
| [Technology stack](TECHNOLOGY_STACK.md) | Developers and judges | Every important tool, why it was selected, and where it is used. |
| [Results and evaluation](RESULTS_AND_EVALUATION.md) | Judges and researchers | Measured results, evaluation method, limitations, and interpretation. |
| [Reproduction guide](REPRODUCTION_GUIDE.md) | Developers | Prerequisites, safe build order, validation checks, and common failure modes. |
| [Camera day field guide](CAMERA_DAY.md) | Camera operator | Plain-language instructions for collecting calibration images and phone footage. |
| [Repository runbook](../RUNBOOK.md) | Operators | Exact recorded commands and verification notes. |

## Important terminology

- **Andromeda** is the overall project name.
- **Sightline** is the demo/dashboard name.
- **Part A** is real-image, monocular visual SLAM.
- **Part B** is the live simulated mapping and autonomous-navigation demonstration.

The two parts support the same larger story but are not a shared live map. This is an intentional design boundary and should be stated in every technical presentation.

