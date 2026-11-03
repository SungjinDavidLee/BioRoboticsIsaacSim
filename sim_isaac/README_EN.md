# Sim-to-Real Post-Training — Isaac Sim · Isaac Lab

**Simulation part of the precision bio-lab tube insertion task** · Franka FR3 · FAIRINO FR5

[한국어](README.md) · [Contents](#contents)

![Scripted demonstrations of the same scene performed by FR3 (left) and FR5 (right) — third-person camera, 4× speed](reports/01_sim_env_autodemo/figures/demo_pair_third_4x.gif)

Paired demonstrations of the same scene (1103-27), each performed by one robot with its own workcell cameras. 1× video: [demo_pair_scene27.mp4](reports/01_sim_env_autodemo/figures/demo_pair_scene27.mp4)

> **Latest news**
> - 2026-11-03 · Added [Real setup → digital twin](docs/real_to_twin/README_EN.md) · FR3 finished collecting 100 demonstrations
> - 2026-11-02 · Started collecting 100 FR3 · FR5 paired demonstrations with the scripted demo collector
> - 2026-11-02 · GR00T N1.7 installed
> - 2026-11-02 · [Report 1 — Building the sim environment and collecting scripted demonstration data](reports/01_sim_env_autodemo/README_EN.md)

---

## Contents

1. **Overview · task · setup · progress** — this page [1](#1-overview) · [2](#2-task) · [3](#3-sim-experiment-setup-replicating-the-real-setup) · [4](#4-progress-updated)
2. **[Real setup → digital twin](docs/real_to_twin/README_EN.md)** — from measurements to shapes, image regions (segments), camera fitting, comparison with colour segments
   - [Real stations and camera frames](docs/real_to_twin/README_EN.md#2-real-stations-and-camera-frames) · [Object measurements](docs/real_to_twin/README_EN.md#3-objects--from-measurements-to-shapes) · [Image regions → scene layout](docs/real_to_twin/README_EN.md#4-image-regions-segments--scene-layout) · [Camera fitting](docs/real_to_twin/README_EN.md#5-picked-points--camera-fitting) · [Colour-segment comparison](docs/real_to_twin/README_EN.md#6-comparing-with-colour-segments) · [Remaining differences](docs/real_to_twin/README_EN.md#7-remaining-differences-and-how-they-are-handled)
3. **Digital twin pipeline and equations** — Report 1 [section 3-1](reports/01_sim_env_autodemo/README_EN.md#3-1-digital-twin-pipeline) · [chapter 4](reports/01_sim_env_autodemo/README_EN.md#4-equations-and-application--digital-twin)
4. **Domain randomization (Isaac Lab)** — Report 1 [section 3-2](reports/01_sim_env_autodemo/README_EN.md#3-2-domain-randomization) · [chapter 5](reports/01_sim_env_autodemo/README_EN.md#5-equations-and-application--domain-randomization)
5. **Scripted demo collector (BioTube-AutoDemo-IsaacLab)** — Report 1 [section 3-3](reports/01_sim_env_autodemo/README_EN.md#3-3-scripted-demo-collector-pipeline) · [section 3-4 per-robot differences](reports/01_sim_env_autodemo/README_EN.md#3-4-per-robot-differences) · [chapter 6](reports/01_sim_env_autodemo/README_EN.md#6-equations-and-application--scripted-demo-generation-and-success-check)
6. **Dataset · collection results · failure analysis** — this page [6](#6-dataset-updated) · Report 1 [chapter 7 data format](reports/01_sim_env_autodemo/README_EN.md#7-data-format-and-records) · [chapter 8 results](reports/01_sim_env_autodemo/README_EN.md#8-collection-results-updated)
7. **Scripted planner vs VLA comparison design · model plan** — Report 1 [chapter 9](reports/01_sim_env_autodemo/README_EN.md#9-comparison-experiment-design--scripted-model-based-planner-vs-vla-plan) · this page [7](#7-model-and-experiment-plan)
8. **Demonstration videos** — Report 1 [appendix A](reports/01_sim_env_autodemo/README_EN.md#appendix-a-demonstration-videos)
9. **Report list** — this page [9](#9-reports)

Development history and code: [BioTube-Insertion-VLA-IsaacLab](https://github.com/bbiyak12e/BioTube-Insertion-VLA-IsaacLab) (personal repository)

## 1. Overview

- **Project:** Automate a bio-lab manipulation — inserting a centrifuge tube into a rack hole — with a VLA (Vision-Language-Action) model. There are two real workcells: FR3 and FR5.
- **Goal of this part:** Generate training data at scale in a simulation that reproduces the real workcells, post-train a VLA (GR00T N1.7) on that data, and transfer it to the real robots.
- **What this part covers:** digital twin → domain randomization → scripted demonstration data → sim post-training · evaluation → real-world deployment

## 2. Task

| Item | Description |
|---|---|
| Instruction | `put the red tube in the red slot` |
| Scene | Source rack (red-water and yellow-water tubes in the two end holes) + empty rack (red ring on one hole) |
| Success | The red tube is inserted into the red-ring hole (red slot) — at the hole-collar height the tube axis passes within 2 mm of the hole center, the tube reaches the bottom, and the tilt is within 12°. The yellow tube and the ring stay in place |
| Difficulty | Tube diameter 30 mm · hole 31 mm → **1 mm clearance**. The tube has a round bottom (radius 15 mm), so even a slight misalignment tilts it and it catches on the collar |
| Randomized per scene | Position · orientation of both racks, red/yellow left-right order, red-ring hole (one of 4), lighting brightness · color |

![The same scene at the start (left) and after insertion (right) — top FR3, bottom FR5](reports/01_sim_env_autodemo/figures/scene_pair.png)

## 3. Sim experiment setup (replicating the real setup)

| Item | Sim |
|---|---|
| Robots | Franka FR3 + Franka Hand, FAIRINO FR5 + DH-Robotics AG-95. The TCPs of both robots use the same definition |
| Cameras | Third-person + wrist, 640×360, 30 fps. Position · angle fitted by least-squares minimization of the reprojection error of points in the real camera images (third-person error 2–4 px) |
| Objects | 50 mL tube built from measurements (48 mL water, 51 g) · 4-hole rack (165×40×51 mm) · red ring (STL of the real part) |
| Workcells | Table · pedestal · wall · start pose reproduced separately for each of the FR3 and FR5 workcells |
| Software | Isaac Sim 6.0.1, Isaac Lab 3.0 (v3.0.0-beta2.patch1) |

## 4. Progress (updated)

![Overall pipeline and progress](figures/pipeline_en.png)

| Stage | Status | Period |
|---|---|---|
| Measured assets (tube · rack · ring) | ✅ Done | Oct 26 – Nov 2 |
| Digital twin (FR3 · FR5 workcells) | ✅ Done | Oct 27 – Oct 29 |
| Isaac Lab domain randomization | ✅ Done | Oct 28 – Oct 31 |
| Scripted demo collector | ✅ Done | Oct 31 – Nov 2 |
| 100 FR3 · FR5 paired demonstrations | 🔄 In progress | Nov 2 – |
| GR00T N1.7 zero-shot (sim) | ⬜ Planned | |
| GR00T N1.7 fine-tuning · per-robot comparison | ⬜ Planned | |
| Real-world deployment (Sim-to-Real) | ⬜ Planned | |

## 5. Simulation environment

### 5-1. Digital twin

![Digital twin pipeline](reports/01_sim_env_autodemo/figures/pipeline_twin_en.png)

Starting from real measurements and the real camera images, we compute the object models, cameras, and start poses, then overlay the sim images on the real ones again to measure the difference.
For each camera, given points $`u_i`$ picked in the photo and the 3D positions $`X_i`$ of the same points, we find the pose that minimizes the reprojection error.

```math
\min_{\,R,\;t}\;\sum_i \big\lVert\, u_i - \pi\!\left(K\,(R X_i + t)\right) \big\rVert^2
```

### 5-2. Domain randomization (Isaac Lab)

For each scene we resample the position · orientation of both racks, the red/yellow left-right order, the red-ring hole, and the lighting brightness · color.
A sampled layout is used only if it satisfies all of the following conditions: **on the table · within reach of both robots · inside both robots' third-person views · not occluded by the robots**; at the planning stage, it is re-checked with inverse kinematics for both robots.
Left-right combinations and ring holes are shuffled in blocks so that they appear evenly, and with the same seed FR3 and FR5 receive the same scene.

### 5-3. Scripted demo collector (BioTube-AutoDemo-IsaacLab)

![Scripted demo collector pipeline](reports/01_sim_env_autodemo/figures/pipeline_autodemo_en.png)

Using the sim ground-truth state, the collector measures the positions of the tube and the red-ring hole, plans a path, executes and records it, then checks the result and saves only successes.
Each segment uses a minimum-jerk time law, and the segment duration $`T`$ is chosen so that the joint velocity and acceleration limits of both robots are respected.

```math
s(\tau) = 10\tau^3 - 15\tau^4 + 6\tau^5,\qquad \tau = t/T \in [0,1]
```

For insertion, the in-hand tube pose is re-measured and the tube descends **along the tube axis**; success is judged by how far the tube axis is from the hole center at the hole-collar height.
The equations used and how they are applied are collected in Report 1, [Section 4, digital twin](reports/01_sim_env_autodemo/README_EN.md#4-equations-and-application--digital-twin) · [Section 5, domain randomization](reports/01_sim_env_autodemo/README_EN.md#5-equations-and-application--domain-randomization) · [Section 6, scripted demo generation and success check](reports/01_sim_env_autodemo/README_EN.md#6-equations-and-application--scripted-demo-generation-and-success-check).

![FR5 insertion phase (1× speed, left third-person · right wrist)](reports/01_sim_env_autodemo/figures/demo_fr5_insert_1x.gif)

## 6. Dataset (updated)

| Item | Value |
|---|---|
| Format | LeRobot v2 (GR00T format) |
| Video | Third-person · wrist, 640×360, 30 fps |
| State · action | TCP position 3 + orientation rot6d 6 + gripper 1 + joints (FR3 7 · FR5 6) |
| Instruction | `put the red tube in the red slot` |
| Pairing | The same scene (layout · lighting) is performed by FR3 and FR5 separately and linked by `pair_id` |

Target: 100 pairs — collection in progress. The table below will be filled in when it finishes.

| | Saved | Discarded · given up | Success rate | Demo length (sim) |
|---|---|---|---|---|
| FR3 | | | | |
| FR5 | | | | |
| Pair (both saved) | | — | — | — |

## 7. Model and experiment plan

- **Model:** [GR00T N1.7](https://github.com/NVIDIA/Isaac-GR00T) (installed)
- **Order:** ① sim zero-shot → ② fine-tuning on paired demonstrations → ③ compare training on FR3 only · FR5 only · both → ④ real-world deployment
- **Input · output (draft):** 2 cameras + TCP state + gripper → relative TCP motion + gripper, in chunks of 16 steps
- **Evaluation:** insertion success rate on random sim scenes (same success criterion as above)

## 8. Results (updated)

In preparation

## 9. Reports

| No. | Content | Date | Link |
|---|---|---|---|
| 1 | Building the sim environment and collecting scripted demonstration data | 2026-11-02 | [한국어](reports/01_sim_env_autodemo/README.md) · [English](reports/01_sim_env_autodemo/README_EN.md) |
| 2 | GR00T N1.7 zero-shot | Planned | |
| 3 | Fine-tuning and per-robot comparison | Planned | |
| 4 | Sim-to-Real | Planned | |
