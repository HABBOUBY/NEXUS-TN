<div align="center">

# NEXUS-TN

### The Industrial Station — A Robotic Arm, a Sorting Conveyor, and a Transport Robot Working as One

*Where a robotic arm meets its transport partner — picking up products and placing them on the back of SAMIR-KAHWEJI.*

<p align="center">
  <img src="04_MEDIA/IMAGES/4K IMAGES/main station NEXUS.png" width="900">
</p>

<p align="center">
  <img src="04_MEDIA/IMAGES/4K IMAGES/main_station_of_robotics-arm.png" width="215">
  <img src="04_MEDIA/IMAGES/4K IMAGES/CONVEYOR1.png" width="215">
  <img src="04_MEDIA/IMAGES/4K IMAGES/SAMIR-KAHWEJI.png" width="215">
</p>

![ESP32](https://img.shields.io/badge/ESP32-WROOM--32-red)
![KiCad](https://img.shields.io/badge/PCB-KiCad-blue)
![SolidWorks](https://img.shields.io/badge/CAD-SolidWorks-orange)
![RealSense](https://img.shields.io/badge/Vision-Intel%20RealSense%20%2B%20OpenCV-lightgrey)
![3D Printing](https://img.shields.io/badge/Fabrication-FDM%203D%20Printing-success)
![License](https://img.shields.io/badge/License-MIT-green)

Built by **HABBOUBY EDEM** for [Beest](https://beest.hackclub.com/)

</div>

---

## Table of Contents

- [About](#about)
- [Scope of This Submission](#scope-of-this-submission)
- [Note on Physical Fabrication](#note-on-physical-fabrication)
- [The Story Behind NEXUS-TN](#the-story-behind-nexus-tn)
- [The Ecosystem: 3 Parts, 1 Station](#the-ecosystem-3-parts-1-station)
- [Part 1 — The Robotic Arm (3 Gripper Types)](#part-1--the-robotic-arm-3-gripper-types)
- [Part 2 — The Sorting Conveyor](#part-2--the-sorting-conveyor)
- [Part 3 — Transport Robot "SAMIR-KAHWEJI"](#part-3--transport-robot-samir-kahweji)
- [Electronics & PCB](#electronics--pcb)
- [3D Printing — Materials & Settings](#3d-printing--materials--settings)
- [Fasteners](#fasteners)
- [Repository Structure](#repository-structure)
- [CAD Files](#cad-files)
- [Simulations](#simulations)
- [On AI Assistance](#on-ai-assistance)
- [Future Improvements](#future-improvements)
- [License](#license)

---

## About

**NEXUS-TN** is a small industrial cell that picks up a product, inspects it, sorts it, and hands it off to a mobile transport robot — a mini factory line, fully open and 3D-printable.

1. **Robotic Arm** on a linear XX' axis, swappable gripper, RealSense vision.
2. **Sorting Conveyor** — inspects and rejects damaged items.
3. **SAMIR-KAHWEJI** — 6-to-4-wheel transport robot, delivers the product following a line.

Designed from scratch in SolidWorks and KiCad.

---

## Scope of This Submission

**Mechanical CAD + custom KiCad PCB** for the arm/conveyor subsystem, plus the full mechanical design of SAMIR-KAHWEJI. Firmware is not included yet — control architecture is documented in `03_DOCS/wiring/`.

---

## Note on Physical Fabrication

I don't yet have access to a 3D printer, a RealSense camera, or a Raspberry Pi — import taxes make them very expensive in Tunisia, and local paid makerspaces are scarce. So NEXUS-TN ships as a **complete, print-ready CAD + PCB package** (STEP, STL, native source) rather than a photographed build. The renders and [simulations](#simulations) show the mechanisms in motion instead.

---

## The Story Behind NEXUS-TN

NEXUS-TN started as a single robotic arm. While building it, I asked myself: does this have a real purpose — could it teach something, could it fit an actual industrial process? That question turned the arm into a full chain: arm → conveyor → transport robot.

I also noticed most robotics kits only ever expose **one** gripper type, forcing anyone curious about a different mechanism to buy a whole new kit. So the arm here supports **3 interchangeable gripper types** plus an open connector standard — to teach more, and cost less.

---

## The Ecosystem: 3 Parts, 1 Station

| # | Subsystem | Role |
|---|-----------|------|
| 1 | **Robotic Arm** (3 gripper types) | Picks and identifies the product, places it on the conveyor / on SAMIR-KAHWEJI |
| 2 | **Sorting Conveyor** | Inspects the product, rejects damaged items |
| 3 | **SAMIR-KAHWEJI** | Carries the product to its destination, line-following |

The arm and conveyor share one ESP32. SAMIR-KAHWEJI runs its own ESP32, fed by a Raspberry Pi 5.

---

## Part 1 — The Robotic Arm (3 Gripper Types)

<p align="center">
  <img src="04_MEDIA/IMAGES/4K IMAGES/main_station_of_robotics-arm.png" width="600">
</p>

The arm travels on an **XX' leadscrew axis**. An **Intel RealSense** camera feeds a **PC (Thonny + OpenCV)**, which sends decisions to the **ESP32**, driving the arm's servos, the leadscrew stepper, and the conveyor. The **PCB rides on the arm itself**, routed to stay clear of its full range of motion.

**Modular gripper system:** Link 2 (arm → gripper) is split in two — a **tapped** piece that always stays on the arm, and a **threaded** piece specific to each gripper. Swap only the threaded piece to change grippers, or design your own.

<table>
<tr>
<td width="33%" align="center">

**Type 1 — "Bird's Beak"**

<img src="04_MEDIA/IMAGES/GRIPPER TYPE 1.PNG" width="260">

Jaws translate in a straight line instead of pivoting. Closed `/\` · Open `:/ \`

</td>
<td width="33%" align="center">

**Type 2 — 5-Finger "Octopus"**

<img src="04_MEDIA/IMAGES/GRIPPER TYPE 2.PNG" width="260">

Five independent fingers close around the product for better conformity on irregular shapes.

</td>
<td width="33%" align="center">

**Type 3 — Classic Parallel**

<img src="04_MEDIA/IMAGES/GRIPPER TYPE 3.PNG" width="260">

Two-finger gripper driven by a rack-and-pinion / belt stage.

</td>
</tr>
</table>

Each type has its own SolidWorks, STEP, and STL set — printable independently.

---

## Part 2 — The Sorting Conveyor

<p align="center">
  <img src="04_MEDIA/IMAGES/4K IMAGES/CONVEYOR1.png" width="600">
</p>

| Width | Length | Height |
|:-:|:-:|:-:|
| 90 mm | 500 mm | 65 mm |

Belt driven by a **stepper motor**, controlled by the arm's ESP32. A **rack-and-pinion pusher** at the conveyor's entrance ejects broken/torn products straight into a recycling bin before they travel further.

---

## Part 3 — Transport Robot "SAMIR-KAHWEJI"

<p align="center">
  <img src="04_MEDIA/IMAGES/4K IMAGES/SAMIR-KAHWEJI.png" width="600">
</p>

<p align="center">
  <img src="04_MEDIA/IMAGES/SAMIR_KAHWEJI CAD.PNG" width="290">
  <img src="04_MEDIA/IMAGES/SAMIR-KAWEJI1 CAD.PNG" width="290">
</p>

- **6-to-4 wheels:** a servo lifts the 2 front (guide) wheels for driving; they drop back down to dock — *(the "truck" prop in the CAD is just a placeholder)*
- **Vision:** camera on a 2-DOF mount, line-following + product detection. Raspberry Pi 5 processes video, sends commands to the onboard ESP32.
- **Docking:** small arc trajectory, then reverses in on its 2 guide wheels, which also stabilize the robot at rest.
- **4 shock absorbers** absorb the ride-height change when the front wheels lift/drop.
- **Onboard gripper:** a small rack-and-pinion, servo-driven clamp secures the product during transit.

---

## Electronics & PCB

<p align="center">
  <img src="04_MEDIA/IMAGES/PCB-DESIGN F1.PNG" width="400">
  <img src="04_MEDIA/IMAGES/PCB-DESIGN F2.PNG" width="400">
</p>

Custom **KiCad PCB** around an **ESP32-WROOM-32**, with 4 separate LiPo rails (common ground):

- **7V** → arm servos
- **11.1V (3S)** → DRV8825, driving both the conveyor motor and the XX' leadscrew stepper
- **3.7V (1S)** → the ESP32

SAMIR-KAHWEJI needs no dedicated PCB — ESP32 + 4 IBT drivers + PWM servo board + an isolated Raspberry Pi 5 power bank, wired directly (see `wiring-transport-robot-samir-kahweji.txt`).

Full BOM: [`03_DOCS/list of electronics.xlsx`](03_DOCS/list%20of%20electronics.xlsx).

---

## 3D Printing — Materials & Settings

**ABS** for structural/high-stress parts, **PLA** for covers and lightweight parts.

| Setting | Value |
|---------|------|
| Layer Height | 0.20 mm |
| Nozzle | 0.4 mm |
| Infill | 20% |
| Walls | 3 |
| Supports | Only where needed |

---

## Fasteners

| Screw | Length | Quantity |
|-------|-------:|---------:|
| M3 | 15 mm | 40 |
| M3 | 20 mm | 20 |
| M5 | 40 mm | 10 |

**Total: 70 screws.**

---

## Repository Structure

```text
NEXUS-TN/
│
├── 01_3D/
│   ├── SOLIDWORKS PARTS/
│   │   ├── ROBOTIC_ARM(3 TYPES)/
│   │   │   ├── SOLIDWORKS PARTS/
│   │   │   │   ├── ROBOTICS_ARM TYPE 1/   # Arm Type 1
│   │   │   │   ├── ROBOTIC_ARM TYPE 2/    # Arm Type 2 + 5-finger gripper
│   │   │   │   └── ROBOTIC_ARM TYPE 3/    # Arm Type 3 + classic gripper
│   │   │   └── STEP FILES/
│   │   ├── CONVEYOR/
│   │   ├── TRANSPORT_ROBOT (SAMIR-KAHWEJI)/
│   │   └── main assembly conveyor_roboticarm_transportrobot.STEP
│   │
│   └── STL/
│       ├── ROBOTIC_ARM(3 TYPES)/
│       │   ├── STL ROBOTIC ARM 1/         # Arm Type 1
│       │   ├── STL ROBOTIC ARM 2/         # Arm Type 2
│       │   └── dossier tanthif 2/         # Arm Type 3
│       ├── CONVOYER/
│       └── TRANSPORT_ROBOT (SAMIR-KAHWEJI)/
│
├── 02_PCB_DESIGN/
│   ├── KICAD DESIGN.kicad_sch / .kicad_pcb / .kicad_pro / .kicad_prl
│   └── carte essaie 1.step
│
├── 03_DOCS/
│   ├── list of electronics.xlsx
│   └── wiring/
│       ├── wiring-robotic-arm-conveyor.txt
│       └── wiring-transport-robot-samir-kahweji.txt
│
├── 04_MEDIA/
│   ├── IMAGES/
│   │   ├── 4K IMAGES/
│   │   ├── GRIPPER TYPE 1/2/3.PNG
│   │   ├── robotic arm type 1/2/3.PNG
│   │   ├── SAMIR_KAHWEJI CAD.PNG / SAMIR-KAWEJI1 CAD.PNG
│   │   └── PCB-DESIGN F1/F2.PNG
│   └── SIMULATIONS/
│       ├── simulation mvt xx' robotic-arm.gif
│       ├── GIF SAMIR-KAHWEJI.gif
│       └── simulation gripper samir-kahweji.gif
│
├── LICENSE
└── README.md
```

Folder names under `ROBOTIC_ARM(3 TYPES)` grew organically per gripper type (`dossier tanthif 2`, etc.) — each "TYPE" folder is self-contained and printable on its own. `main assembly conveyor_roboticarm_transportrobot.STEP` combines the whole ecosystem into one file.

---

## CAD Files

Per subsystem: **native SolidWorks** source, **STEP** (any CAD tool), and **STL** (slicer-ready) — under `01_3D/`.

---

## Simulations

<table>
<tr>
<td width="33%" align="center">

**XX' axis travel**

<img width="300" height="253" alt="simulation mvt xx' robotic-arm" src="https://github.com/user-attachments/assets/511a82f5-c064-44c0-ad4d-51649ebfd296" />

</td>
<td width="33%" align="center">

**6-to-4 wheel transform**

<img width="300" height="245" alt="GIF SAMIR-KAHWEJI" src="https://github.com/user-attachments/assets/d0fabeeb-72f3-4f38-b7e2-303c95243f43" />

</td>
<td width="33%" align="center">

**SAMIR-KAHWEJI's onboard gripper**

<img width="300" height="229" alt="simulation gripper samir-kahweji" src="https://github.com/user-attachments/assets/191a87a2-87b6-4b0c-a83e-4c0f57644351" />

</td>
</tr>
</table>

---

## On AI Assistance

AI helped review this submission against the shipping guide and organize this README. All CAD, PCB, and engineering decisions are my own.

---

## Future Improvements

- Firmware: ESP32 control + OpenCV sorting pipeline
- Firmware: SAMIR-KAHWEJI line-following / docking loop
- Physical build, once printing/camera/Pi are accessible
- An autonomous delivery truck with its own onboard mechanical system to take the product directly from SAMIR-KAHWEJI

---

## License

**MIT License**.
