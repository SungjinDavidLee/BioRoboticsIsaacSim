# BioRoboticsIsaacSim

바이오 실험 조작을 위한 Sim-to-Real VLA 연구 — Franka FR3 · FAIRINO FR5, Isaac Sim · Isaac Lab
Sim-to-real VLA research for bio-lab manipulation — Franka FR3 · FAIRINO FR5, Isaac Sim · Isaac Lab

실물 FR3 · FR5 실험대를 Isaac Sim 에 재현하고, 실험마다 폴더 하나를 둡니다. 세 실험은 같은 실물 재현 실험대 (책상 · 받침대 · 벽 · 조명 · 카메라) 를 함께 씁니다.
The real FR3 · FR5 workcells are reproduced in Isaac Sim, with one folder per experiment. All three experiments share the same real-matched workcells (table · pedestal · wall · lighting · cameras).

| 폴더 / Folder | 파트 / Part | 실험 / Experiment | 상태 / Status (2026-11-20) |
|---|---|---|---|
| [sim_isaac_tube](sim_isaac_tube/README.md) | Sim-to-Real Post-Training (시뮬레이션 / simulation) | **튜브 꽂기** — 빨간 튜브를 빨간 링 구멍에 (틈 1 mm) · GR00T N1.7 · DAgger · VLA + 수식 <br> **Tube insertion** — red tube into the red-ring hole (1 mm clearance) · GR00T N1.7 · DAgger · VLA + formula | 시뮬 벤치 · DAgger 끝 · 실물 적용 차후 예정 <br> Sim bench · DAgger done · real-robot deployment planned |
| [sim_isaac_dropper](sim_isaac_dropper/README.md) | Sim-to-Real Post-Training (시뮬레이션 / simulation) | **스포이드** — 거치대에서 꺼내기 → 비커 물 빨아들이기 → 24 칸 판의 빨간 칸에 떨구기 → 거치대에 꽂기 <br> **Dropper** — take it from the stand → draw liquid from a beaker → drop it into the red well of a 24-well plate → put it back | 진행 중 — 시뮬 환경 · 시연 설계 <br> In progress — sim environment · demo design |
| [sim_isaac_beaker](sim_isaac_beaker/README.md) | Sim-to-Real Post-Training (시뮬레이션 / simulation) | **비커 붓기** — 작은 비커의 공을 큰 비커로 붓기 <br> **Beaker pouring** — pour balls from a small beaker into a large one | 멈춤 — 물체 모델 끝, 붓기 궤적 차후 예정 <br> Paused — object models done, pouring trajectory planned |

English: [tube](sim_isaac_tube/README_EN.md) · [dropper](sim_isaac_dropper/README_EN.md) · [beaker](sim_isaac_beaker/README_EN.md)

`sim_isaac/` 는 2026-11-20 에 `sim_isaac_tube/` 로 옮겼습니다.
`sim_isaac/` was moved to `sim_isaac_tube/` on 2026-11-20.

각 팀은 자기 폴더를 만들고 위 표에 한 줄을 추가합니다.
Each team adds its own folder and one row to the table above.
