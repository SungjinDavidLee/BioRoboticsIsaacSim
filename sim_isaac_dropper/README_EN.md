# Dropper — Sim-to-Real Post-Training (Isaac Sim · Isaac Lab)

**Bio-lab dropper task · simulation part** · Franka FR3 · FAIRINO FR5 · in progress (2026-11-20)

[한국어](README.md) · [← Sim experiments (tube · dropper · beaker)](../README.md)

Figures and videos will be added once scripted-demo collection starts.

## 1. Overview

- We follow, in Isaac Sim, the dropper task done on the real FR3 · FR5 workcells (started Nov 18).
- It reuses the real-matched workcells (table · pedestal · wall · lighting · cameras) and the pipeline (scripted demos → real-format conversion → GR00T N1.7 fine-tuning → sim benchmark → DAgger) of the [tube insertion](../sim_isaac_tube/README_EN.md) experiment.
- The sim matches the **environment** to the real one. The way demos are generated does not have to copy the real robots; the real motion style is included as one of the demo branches.

## 2. Task

| Item | Description |
|---|---|
| Sequence | ① Grasp the dropper and take it out of the stand → ② put it into the beaker and squeeze and release the rubber bulb to draw liquid → ③ squeeze the bulb above the target well of a 24-well plate to drop the liquid → ④ put the dropper back into the stand |
| Target well | The one of 24 wells marked with a red coin — random for each run |
| Success | Liquid lands in the target well — defined in the sim with the same meaning as the real-robot check |
| Randomized per scene | Object layout (within reach on the table), target well, lighting brightness · color (same ranges as the tube experiment). Which objects to move and the plate angle are still being decided |

## 3. Sim setup

| Item | Sim |
|---|---|
| Workcells | The real-matched workcells of the tube experiment as they are — table · pedestal · wall · lighting (cool to warm) · cameras |
| Objects | Beaker (yellow liquid, color from the real photos) · 24-well plate · dropper (clear glass tube + rubber bulb) · stand (open front facing the third-person camera) · target-marking coin |
| Matching appearance | Each object is matched separately by eye, side by side with a real photo — the glass tube is clear glass; the plate and beaker start from white (or slightly bluish white) with adjusted transparency |
| Gripper · bulb | No sim assists (attach while held · squeeze hard when blocked) — they are help the real robots do not have, so they do not suit sim-to-real. Instead we fix the AG-95 sim gripper model and the bulb contact material, and keep the grip force close to the real one |
| Start pose | Values received from the real-robot side (not estimated from photos) |

## 4. Demo design

The tube experiment taught us that training on many overly clean demos did not work well, so we mix several branches of demos and then add DAgger.

| Branch | Share | Description |
|---|---|---|
| Alignment speed | Slow alignment 75 % · fast and precise 25 % | Slow alignment = like the real robots, align slowly before lifting from the stand · entering the beaker · dropping on the plate |
| Perturbation | Perturbed 70 % · not perturbed 30 % | Perturbed = deliberately push a little off, then correct |
| Retry after failure | — | Runs such as a failed grasp → grasp again are left to DAgger after the demos are made |

- **Demo length:** generate demos at real-robot speed, then set it from the longest · mean · median values (the real FR5 collection takes about 1 min per run). We also check that no motion is too fast (not dangerous on the real robot)
- **Bench time limit:** 100 s
- **Data amount:** 1,000 demos per robot planned + DAgger

## 5. Progress (2026-11-20)

| Stage | Status |
|---|---|
| Object models (beaker · 24-well plate · dropper · stand · marking coin) | ✅ Done |
| Demo design (branches · shares · bench time limit) | ✅ Decided |
| Appearance (transparent materials · liquid color) · gripper · bulb contact | 🔄 In progress |
| 1,000 scripted demos · real-format conversion · GR00T N1.7 fine-tuning | ⬜ Planned |
| Sim benchmark · DAgger | ⬜ Planned |
| Real-robot deployment (Sim-to-Real) | ⬜ Planned |
