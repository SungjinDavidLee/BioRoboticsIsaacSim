# Beaker pouring — Sim-to-Real Post-Training (Isaac Sim · Isaac Lab)

**Bio-lab beaker pouring task · simulation part** · Franka FR3 · FAIRINO FR5 · paused, planned (2026-11-20)

[한국어](README.md) · [← Sim experiments (tube · dropper · beaker)](../README.md)

## 1. Overview

- This moves the beaker pouring experiment done on the real FR3 · FR5 into Isaac Sim (started Nov 16, paused Nov 18). The [dropper](../sim_isaac_dropper/README_EN.md) experiment comes first; pouring will continue later.
- The scene (table · pedestal · wall · lighting) uses the real-matched workcell specification of [tube insertion](../sim_isaac_tube/README_EN.md) as is — no separately estimated heights.

## 2. Task

| Item | Description |
|---|---|
| Motion | Grasp and lift the small beaker, then tilt it to pour the balls inside into the large beaker |
| Robots | FR3 · FR5 — the real team's pouring plan differs per robot (below) |

## 3. Decided so far

- **Object models:** beakers (small · large) · balls · dropper · 24-well plate · lid · dropper stand · hole-marking coin are built. The dropper experiment uses the same models.
- **Pouring trajectory:** instead of searching for a new trajectory in the sim, **we carry over the real team's pouring plan for each robot as is.**
  - FR3 — grasp the beaker tilted 45°, then pour by leaning outward with joints 2 · 4 · 6 (joint 7 fixed)
  - FR5 — grasp from above, tilt mainly with joint 5
- **Constraint:** rotating the wrist a lot pulls on the wrist-camera cable (connected to the computer). So we start from the **smallest tilt** at which all the balls come out.

## 4. Progress (2026-11-20)

| Stage | Status |
|---|---|
| Object models · real-matched workcell specification | ✅ Done |
| Carry over the real team's pouring plan · find the smallest tilt | ⬜ Planned |
| Wrist rotation the camera cable allows (measured on the real side) | ⬜ Planned |
| Scripted demos · training · sim benchmark | ⬜ Planned |
