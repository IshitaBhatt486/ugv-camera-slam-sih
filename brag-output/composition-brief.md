# Hyperframes Composition Brief: GPS-Denied Autonomous Ground Rover

## Objective

Create a 20-second launch-style brag video for Andromeda / Sightline, a camera-based, GPS-denied autonomous ground-rover proof of concept.

## Output

- Composition directory: `brag-output/composition/`
- Rendered video: `brag-output/brag.mp4`
- Format: landscape — 1920x1080
- Duration: 20 seconds

## Source Material

- Project root: `C:\Users\hp\OneDrive - Shiv Nadar Institution of Eminence\Desktop\github\ugv-camera-slam-sih`
- Primary files read: `README.md`, `PROJECT_DETAILS.md`, `frontend/src/App.tsx`, `frontend/src/index.css`, `frontend/src/views/DashboardView.tsx`, `frontend/src/data/mock.ts`
- Product name: Andromeda / Sightline
- Tagline / strongest claim: `Camera-only navigation for a GPS-denied autonomous ground rover.`
- Key UI or visual moment to recreate: Sightline’s occupancy grid with a planned path, animated rover, goal marker, and mission-status panel.
- Copy that must appear verbatim:
  - `Camera-only navigation for a GPS-denied autonomous ground rover.`
  - `1101 / 1101`
  - `0.21 m`

## Creative Direction

- Tone preset: cinematic
- Creative direction: fun chaotic cinematic polished
- Interpretation: Trailer-scale but legible; cinematic dark/cyan mission-control visuals with energetic impact cuts and truthful data.
- Angle: GPS disappears, but the rover continues using onboard vision, mapping, and navigation.
- Hook: `GPS: OFFLINE.` / `ROVER: STILL MOVING.`
- Outro / punchline: `GPS-DENIED. NOT DIRECTIONLESS.`
- Avoid:
  - Generic SaaS language
  - Abstract filler visuals
  - Claims that the two project halves share a map or coordinate frame
  - Claims of real-hardware autonomous driving

## Visual Identity

- Background: `#0b1120`
- Text: `#e2e8f0`
- Accent: `#22d3ee`
- Display font: `ui-monospace, SFMono-Regular, Menlo, Consolas, monospace`
- Body font: `ui-monospace, SFMono-Regular, Menlo, Consolas, monospace`
- Visual references from the project: dark Sightline dashboard, occupancy-grid geometry, cyan telemetry, planned route, robot/goal state.

## Storyboard

Use `brag-output/brag-plan.md` as the creative contract.

1. Satellite blackout — 3s — `GPS: OFFLINE.` then `ROVER: STILL MOVING.`
2. Vision becomes a map — 4s — camera frame, point cloud, trajectory, `MONOCULAR VISUAL SLAM`.
3. Mission control route — 5s — occupancy map, goal command and animated planned route.
4. Measured, not imagined — 4s — real `1101 / 1101`, `11,893`, `321` metric cards.
5. Arrival — 4s — `GOAL REACHED`, `0.21 m — INSIDE NAV2 TOLERANCE`.
6. Final signal is contained in the last 4s — `GPS-DENIED. NOT DIRECTIONLESS.` / `ANDROMEDA / SIGHTLINE`.

## Audio

- Audio role: cinematic support with moderate UI accents.
- Audio arc: controlled ignition -> steady motion -> metric lift -> one final success hit.
- Music: `assets/music/happy-beats-business-moves-vol-12-by-ende-dot-app.mp3`
- Music treatment: volume near 0.34, fade at 18.8s.
- Music cue guidance: preset at `assets/music/cues/happy-beats-business-moves-vol-12-by-ende-dot-app.music-cues.json`; strong candidates 8.74s, 13.11s, 17.47s.
- Audio-reactive treatment: subtle cyan glow / scanline depth may react to energy; no waveform or equalizer graphics.
- Audio-coupled moments:
  - 8.74s map reveal — major beat-locked moment.
  - 9.29s, 9.83s, 10.37s metric cards — beat-grid sequence.
  - 13.11s goal arrival — success accent.
  - 17.47s final title — final hit.
- SFX selection guidance: concise click at goal command, soft impact for major reveal, restrained bell at success.
- SFX analysis guidance: `C:\Users\hp\.codex\plugins\cache\brag\brag\0.4.0\skills\brag\assets\sfx\sfx-analysis.md`
- Exact SFX choice: choose local copied assets and timestamps after animation exists.

## Hyperframes Instructions

Use a deterministic timeline with all timed elements carrying `data-start` and `data-duration`. Show a real project visual language — specifically the occupancy-map and goal-navigation moment. Keep text readable, use only grounded facts, render locally, and run `hyperframes check` with zero errors before rendering.
