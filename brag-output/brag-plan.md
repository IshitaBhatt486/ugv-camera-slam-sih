# Brag Plan: GPS-Denied Autonomous Ground Rover

## What is this app?

Andromeda / Sightline is a hardware-free proof of concept that uses camera-based visual SLAM and a ROS 2 simulation to map and navigate a ground rover where GPS cannot be trusted.

## The angle

GPS cuts out. The rover does not panic. This is treated like a high-budget mission-control trailer: measured proof, real map motion, and a clean final arrival — with just enough chaotic kinetic type to make “no satellites” feel like a boss battle.

## Hook (first 2-3 seconds)

“GPS: OFFLINE.” slams over a dark mission grid, then is immediately countered by “ROVER: STILL MOVING.”

## Key moments (the middle)

- A camera-feed / sparse-point-cloud moment establishes monocular visual SLAM on real phone footage.
- The Sightline map shows an occupancy grid, goal marker, and a planned route threading a doorway.
- A live-style mission card counts to `1101 / 1101` frames tracked and locks the rover inside the `0.21 m` Nav2 goal tolerance.

## Outro / punchline

`GPS-DENIED. NOT DIRECTIONLESS.` followed by `ANDROMEDA / SIGHTLINE`.

## User flow worth showing

Phone camera footage -> visual SLAM creates sparse map / trajectory -> simulated TurtleBot3 builds an occupancy grid -> Nav2 receives a safe goal and drives to within 0.21 m.

## Tone

- Preset: cinematic
- Creative direction: fun chaotic cinematic polished
- Interpretation: Trailer-scale typography and decisive cuts, kept polished by a dark operator-console palette, truthful metrics, and readable holds.

## Format: landscape — 1920x1080
## Duration: 20 seconds

## Visual identity (from the project)

- Background: `#0b1120`
- Accent: `#22d3ee` (cyan telemetry treatment, consistent with the source dashboard’s slate/cyan operator-console styling)
- Text: `#e2e8f0`
- Display font: `ui-monospace, SFMono-Regular, Menlo, Consolas, monospace`
- Body font: `ui-monospace, SFMono-Regular, Menlo, Consolas, monospace`
- Strongest visual element: the dark Sightline occupancy map with planned path, robot position, and goal marker.

## Share copy (draft)

GPS-DENIED. NOT DIRECTIONLESS. Andromeda turns camera-based SLAM, a live occupancy map, and Nav2 goal navigation into a rover mission-control demo — with every result measured and its limits documented.

## Audio direction

- Role: cinematic support with a rhythmic, polished edge
- Music: `happy-beats-business-moves-vol-12-by-ende-dot-app.mp3`
- Music treatment: begin at 0; low, steady bed at 0.34; fade under the final title.
- Music cue guidance: 109.96 BPM preset. Major reveal candidates: 8.74s, 13.11s, 17.47s. Beat-grid window for telemetry cards: 9.29s, 9.83s, 10.37s.
- Audio-reactive treatment: subtle; cyan map glow and scanline intensity breathe with the music energy. No waveform visualizer.
- SFX posture: moderate; UI click for goal command, soft impact for mission reveal, one resonant success accent at goal arrival.
- Audio-coupled moments: GPS failure flash, map/path draw, three sequential metric cards, final arrival lock.
- Restraint rule: no alarm-siren clutter; the rover must feel capable, not in crisis.

## Storyboard

### Scene 1 — Satellite blackout — 3.0s

Dark grid, scanning horizon, then `GPS: OFFLINE.` followed by `ROVER: STILL MOVING.` Real project premise: camera-only navigation for GPS-denied operation.

Sequential/interaction: none.
Audio intent: a controlled ignition, not a disaster.
Audio-coupled idea: the two-line reveal lands with a soft impact and first beat.
Music: cinematic bed begins.
Transition mood: hard / dramatic -> Scene 2.

### Scene 2 — Vision becomes a map — 4.0s

A phone-camera frame and sparse 3D point cloud resolve into `MONOCULAR VISUAL SLAM`; supporting label: `REAL PHONE FOOTAGE`. The point field and trajectory are grounded in the project’s visual-SLAM half.

Sequential/interaction: trajectory line draws from left to right.
Audio intent: discovery and forward motion.
Audio-coupled idea: trajectory draw catches the 3.82s / 4.39s beat window.
Music: steady, growing presence.
Transition mood: zoom / cinematic -> Scene 3.

### Scene 3 — Mission control route — 5.0s

Recreate Sightline’s occupancy-map visual: `GOAL RECEIVED`, cyan goal, rover icon, wall blocks, and a path that bends through a door gap. A cursor-style command panel reads `(-6.21, 0.63)`.

Sequential/interaction: goal click -> route segments draw -> rover advances.
Audio intent: decisive control-room momentum.
Audio-coupled idea: click at 7.0s; route reveal is beat-locked at 8.74s.
Music: map glow subtly responds to music.
Transition mood: hard / polished -> Scene 4.

### Scene 4 — Measured, not imagined — 4.0s

Three real metric cards arrive one by one: `1101 / 1101 FRAMES TRACKED`, `11,893 MAP POINTS`, `321 KEYFRAMES`. Small lockup: `KITTI 06`.

Sequential/interaction: cards enter at 9.29s, 9.83s, and 10.37s; hold the full set until the transition.
Audio intent: a confident data crescendo.
Audio-coupled idea: understated card ticks on each reveal.
Music: crisp rhythm.
Transition mood: flash / cinematic -> Scene 5.

### Scene 5 — Arrival — 4.0s

The rover completes the route; the mission panel locks `GOAL REACHED` and `0.21 m — INSIDE NAV2 TOLERANCE`. This uses the verified Gazebo result.

Sequential/interaction: rover arrives, then the success lock appears.
Audio intent: earned resolution.
Audio-coupled idea: goal lock is beat-locked at 13.11s with a resonant success accent.
Music: short swell.
Transition mood: dramatic wipe -> Scene 6.

### Scene 6 — Final signal — 20.0s total (4.0s)

Full-screen final title: `GPS-DENIED.` then `NOT DIRECTIONLESS.` and the project lockup `ANDROMEDA / SIGHTLINE`.

Sequential/interaction: line one lands at 16.0s; line two locks near the 17.47s strong cue; title settles and holds.
Audio intent: clean final landing.
Audio-coupled idea: final title hit, then music fade.
Music: fade from 18.8s.
Transition mood: cut to black.

**Music mood for this video:** cinematic, steady, confident.
**Audio summary:** A warm electronic pulse opens the mission, supports the map and metrics, and leaves space for one decisive goal-arrival bell and final title hit.
