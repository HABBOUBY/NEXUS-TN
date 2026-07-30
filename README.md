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
  <img src="04_MEDIA/IMAGES/GRIPPER TYPE 3.PNG" width="215">
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

**NEXUS-TN** is a small industrial automation cell designed to pick up a product, inspect it, sort it, and deliver it to a mobile transport robot for onward delivery — the same kind of pick-and-place-and-transport logic found in real factory lines, scaled down into an open, 3D-printable, educational package.

The station is built around three cooperating machines:

1. A **robotic arm**, mounted on a linear XX' axis, equipped with a swappable gripper and an Intel RealSense camera for vision-guided picking.
2. A **sorting conveyor**, which carries the product past inspection and rejects damaged items before they reach the transport stage.
3. **SAMIR-KAHWEJI**, an autonomous transport robot that folds from 6 wheels to 4, receives the product on its back, and carries it away following a line.

All mechanical structure, wiring architecture, and the custom PCB were designed from scratch in SolidWorks and KiCad.

---

## Scope of This Submission

This submission covers the **mechanical design (CAD)** and the **custom PCB (KiCad)** for the robotic arm + conveyor subsystem, plus the complete mechanical design of SAMIR-KAHWEJI. Full firmware (ESP32 control logic, OpenCV sorting pipeline, PID/line-following code) is not included at this stage — this project is being submitted as a hardware and electronics design, with the control/wiring architecture fully documented in `03_DOCS/wiring/`.

---

## Note on Physical Fabrication

This submission is provided as a **complete, print-ready CAD and PCB design** (STEP, STL, and native SolidWorks/KiCad project files).

I do not currently have access to a 3D printer, an Intel RealSense camera, or a Raspberry Pi, and all three are either unavailable or prohibitively expensive to import in Tunisia — import duties and customs taxes significantly raise the price of 3D printers, electronics, and imaging hardware compared to international markets, putting them out of reach for a student project. Local makerspaces with printing capability are few, mostly concentrated in the capital, and operate as private, paid facilities rather than open-access labs.

For this reason, I have not yet been able to physically fabricate and photograph the assembled NEXUS-TN station. Every file needed to reproduce it — slicer-ready STL exports, STEP files, native SolidWorks source, and the full KiCad PCB project — is included in this repository, so the design can be fabricated and verified by anyone with access to the right hardware. In place of a live demo, the CAD renders and the movement simulations (see [Simulations](#simulations)) show the mechanisms in motion.

---

## The Story Behind NEXUS-TN

NEXUS-TN didn't start as a factory. It started as a single robotic arm.

While designing that first arm, I asked myself a simple question: does this project actually have a purpose beyond being a cool mechanism? Could it help someone else understand how a real gripper or a real robotic joint works? Could it ever fit into something bigger — like an actual industrial process?

That question is what turned a standalone arm into **NEXUS-TN: the industrial station**. Instead of stopping at "an arm that grips," I built the arm to feed a sorting conveyor, and the conveyor to feed a transport robot — a small, complete pick–sort–transport chain.

Along the way, I noticed something about most robotics kits and arm projects I'd seen: they only ever expose **one** type of gripper. That's fine if you just want to complete a task, but it means anyone learning from that kit only ever sees one gripping principle, and if they want to explore another one, they usually have to buy an entirely different kit. That felt wasteful — both for a student's wallet and for their understanding of the field.

So the robotic arm in NEXUS-TN was designed around **3 different, interchangeable gripper types**, plus a connector standard that lets anyone design and mount their own gripper on the same arm. The goal isn't just to build a working robot — it's to spread the knowledge of how different gripping mechanisms actually work, as cheaply and openly as possible.

---

## The Ecosystem: 3 Parts, 1 Station

| # | Subsystem | Role |
|---|-----------|------|
| 1 | **Robotic Arm** (3 gripper types) | Picks the product, identifies it via camera, places it on the conveyor / hands it to SAMIR-KAHWEJI |
| 2 | **Sorting Conveyor** | Moves the product under inspection; rejects damaged items before transport |
| 3 | **Transport Robot "SAMIR-KAHWEJI"** | Receives the product on its back and drives it to its destination, following a line |

A shared control philosophy ties them together: the robotic arm and the conveyor are commanded by the **same ESP32 board** (so the conveyor only runs when the arm/vision system tells it to), while SAMIR-KAHWEJI runs its own ESP32, fed by a Raspberry Pi 5 handling onboard vision.

---

## Part 1 — The Robotic Arm (3 Gripper Types)

<p align="center">
  <img src="04_MEDIA/IMAGES/4K IMAGES/main_station_of_robotics-arm.png" width="600">
</p>

The arm rides on an **XX' linear axis** (leadscrew + nut mechanism) so it can travel horizontally between the pickup zone and the conveyor / SAMIR-KAHWEJI's back. Vision is handled by an **Intel RealSense depth camera**, connected to a **PC running Thonny**, where **OpenCV** (plus whatever additional Python libraries the task needs) analyzes the product before deciding what the arm should do. The PC then relays the decision to the **ESP32** control board, which drives the arm's servos, the leadscrew stepper, and the conveyor.

The **custom PCB is mounted directly on the arm**, deliberately positioned and routed so that it never obstructs the arm's free range of motion — the board rides with the arm instead of trailing loose wires that could snag the mechanism.

### The Modular Gripper System

Instead of a single fixed end-effector, NEXUS-TN's arm uses a **two-piece Link 2** (the connecting arm segment between Arm 1 and the gripper):

- One piece carries a **tapped (threaded bore)** interface and always stays the same — it's the part that mounts to the arm.
- The other piece carries a **matching thread** and is specific to whichever gripper is attached.

To switch grippers — or to mount a completely custom one you've designed yourself — you only need to swap the second, threaded piece. The tapped support piece (which also carries the wiring/linker routing) never changes. It's a small mechanical trick, but it means one arm can teach three completely different gripping principles, and it leaves the door open for anyone to design a fourth.

Three gripper types are included as a working reference and as inspiration for your own design:

<table>
<tr>
<td width="33%" align="center">

**Gripper Type 1 — "Bird's Beak"**

<img src="04_MEDIA/IMAGES/GRIPPER TYPE 1.PNG" width="260">

Two jaws that don't rotate around a pivot — instead, they translate in a straight line, closing and opening like a beak.
Closed: `/\`  ·  Open: `:/  \`

</td>
<td width="33%" align="center">

**Gripper Type 2 — 5-Finger "Octopus"**

<img src="04_MEDIA/IMAGES/GRIPPER TYPE 2.PNG" width="260">

Five independent fingers close around the product from multiple sides at once, closer in spirit to an octopus grip than a mechanical claw — better conformity on irregular shapes.

</td>
<td width="33%" align="center">

**Gripper Type 3 — Classic Parallel Gripper**

<img src="04_MEDIA/IMAGES/GRIPPER TYPE 3.PNG" width="260">

The familiar two-finger parallel gripper, driven through a rack-and-pinion / belt-and-pulley stage — the reference point most people will already recognize.

</td>
</tr>
</table>

Each gripper type has its own SolidWorks source, STEP export, and STL set (see [Repository Structure](#repository-structure)) so any of the three can be printed and mounted independently of the others.

---

## Part 2 — The Sorting Conveyor

<p align="center">
  <img src="04_MEDIA/IMAGES/4K IMAGES/CONVEYOR1.png" width="600">
</p>

| Property | Value |
|----------|------:|
| Width | 90 mm |
| Length | 500 mm |
| Height | 65 mm |

The belt is driven by a **stepper motor**, controlled by the **same ESP32 board** that drives the robotic arm — the camera identifies the product, and the ESP32 decides when the conveyor should run.

A **rack-and-pinion pusher mechanism** sits near the start of the belt: if the vision system flags a product as broken or torn, the pusher shifts it off the conveyor immediately, right at the entrance, dropping it into a separate recycling/repair bin instead of letting it travel further down the line.

---

## Part 3 — Transport Robot "SAMIR-KAHWEJI"

<p align="center">
  <img src="04_MEDIA/IMAGES/4K IMAGES/SAMIR-KAHWEJI.png" width="600">
</p>

SAMIR-KAHWEJI is what I consider the most mechanically interesting part of NEXUS-TN: a mobile transport platform that **transforms from 6 wheels down to 4** — 2 guide wheels + 4 drive wheels — to dock, load, and set off again.

<p align="center">
  <img src="04_MEDIA/IMAGES/SAMIR_KAHWEJI CAD.PNG" width="290">
  <img src="04_MEDIA/IMAGES/SAMIR-KAWEJI1 CAD.PNG" width="290">
</p>

**Navigation & vision:** a camera sits on a **2-degrees-of-freedom support**, letting it follow the black guide line on the ground and also look down to detect the product sitting on the robot's back. A **Raspberry Pi 5** handles all the camera processing and sends its decisions to the onboard **ESP32**, which then issues the actual motor/servo commands. *(Note: the "truck" model visible in the CAD is only a placeholder prop, used to help visualize where and how the product sits on the robot's back — not a real component.)*

**Docking maneuver:** once SAMIR-KAHWEJI reaches its target position, it doesn't just stop — it performs a small arc-shaped trajectory, then reverses to dock using its two guide wheels. Those guide wheels double as a stabilizing base, keeping the robot balanced while it's stationary and loading/unloading.

**Wheel transformation:** a servo lifts the two front wheels to switch between the 6-wheel (guided) and 4-wheel (drive-only) configurations. Because that lift briefly changes the ride height on the front axle, **4 shock absorbers** are fitted to the drive wheels to absorb the resulting gap/impact and keep the robot stable through the transition.

**Onboard gripper:** SAMIR-KAHWEJI also carries its own small rack-and-pinion, servo-actuated gripper/clamp (see `assembly gripper samir kahweji` parts) to secure the product once it's placed on its back, so it doesn't shift during transit.

---

## Electronics & PCB

<p align="center">
  <img src="04_MEDIA/IMAGES/PCB-DESIGN F1.PNG" width="400">
  <img src="04_MEDIA/IMAGES/PCB-DESIGN F2.PNG" width="400">
</p>

The robotic arm + conveyor subsystem is controlled by a **custom PCB designed in KiCad**, built around an **ESP32-WROOM-32**. Key design choices, documented in full in `03_DOCS/wiring/`:

- **4 separate LiPo rails** instead of one battery + regulator, to avoid voltage-regulator heat loss and give each subsystem a clean, dedicated supply:
  - **7V** → all arm servo motors (joints / gripper actuation)
  - **11.1V (3S)** → DRV8825 stepper driver, which drives *both* the conveyor belt motor and the XX' axis leadscrew stepper
  - **3.7V (1S)** → the ESP32 itself
- All rails share a **common ground** between the ESP32, the DRV8825, and the PC vision interface.

SAMIR-KAHWEJI, by contrast, doesn't need a dedicated PCB — its wiring (ESP32 + 4 IBT motor drivers + PWM servo expansion board + Raspberry Pi 5 on its own isolated power bank) is simple enough to wire directly; full details are in `wiring-transport-robot-samir-kahweji.txt`.

The full electronics BOM, with part specs and purchase links, is in [`03_DOCS/list of electronics.xlsx`](03_DOCS/list%20of%20electronics.xlsx).

---

## 3D Printing — Materials & Settings

### Materials
- **ABS** for structural and high-stress components (arm joints, gripper mechanisms, wheel-transformation parts)
- **PLA** for lightweight and cosmetic parts (covers, housings)

### Recommended Print Settings

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

**Total: 70 screws across 2 diameters (M3 / M5).**

---

## Repository Structure

```text
NEXUS-TN/
│
├── 01_3D/
│   ├── SOLIDWORKS PARTS/
│   │   ├── ROBOTIC_ARM(3 TYPES)/
│   │   │   ├── SOLIDWORKS PARTS/
│   │   │   │   ├── ROBOTICS_ARM TYPE 1/   # Native source — Arm Type 1
│   │   │   │   ├── ROBOTIC_ARM TYPE 2/    # Native source — Arm Type 2 + 5-finger gripper
│   │   │   │   └── ROBOTIC_ARM TYPE 3/    # Native source — Arm Type 3 + classic parallel gripper
│   │   │   └── STEP FILES/                # STEP export for each of the 3 arm assemblies
│   │   ├── CONVEYOR/
│   │   │   ├── SOLIDWORKS PARTS/
│   │   │   └── STEP FILES/
│   │   ├── TRANSPORT_ROBOT (SAMIR-KAHWEJI)/
│   │   │   ├── SOLIDWORKS PARTS/
│   │   │   └── STEP FILES/
│   │   └── main assembly conveyor_roboticarm_transportrobot.STEP   # full ecosystem, single file
│   │
│   └── STL/
│       ├── ROBOTIC_ARM(3 TYPES)/
│       │   ├── STL ROBOTIC ARM 1/         # Slicer-ready — Arm Type 1
│       │   ├── STL ROBOTIC ARM 2/         # Slicer-ready — Arm Type 2 (5-finger gripper)
│       │   └── dossier tanthif 2/         # Slicer-ready — Arm Type 3 (classic gripper)
│       ├── CONVOYER/                      # Slicer-ready — conveyor + sorting pusher
│       └── TRANSPORT_ROBOT (SAMIR-KAHWEJI)/  # Slicer-ready — SAMIR-KAHWEJI, all parts
│
├── 02_PCB_DESIGN/
│   ├── KICAD DESIGN.kicad_sch        # Schematic
│   ├── KICAD DESIGN.kicad_pcb        # PCB layout
│   ├── KICAD DESIGN.kicad_pro / .kicad_prl
│   └── carte essaie 1.step           # 3D STEP export of the board (for CAD integration on the arm)
│
├── 03_DOCS/
│   ├── list of electronics.xlsx      # Full BOM with specs + purchase links
│   └── wiring/
│       ├── wiring-robotic-arm-conveyor.txt
│       └── wiring-transport-robot-samir-kahweji.txt
│
├── 04_MEDIA/
│   ├── IMAGES/
│   │   ├── 4K IMAGES/                 # Hero renders: station, arm+conveyor, SAMIR-KAHWEJI, conveyor
│   │   ├── GRIPPER TYPE 1.PNG / GRIPPER TYPE 2.PNG / GRIPPER TYPE 3.PNG
│   │   ├── robotic arm type1.PNG / robotic arm type 2.PNG / robotic arm type 3.PNG
│   │   ├── SAMIR_KAHWEJI CAD.PNG / SAMIR-KAWEJI1 CAD.PNG
│   │   └── PCB-DESIGN F1.PNG / PCB-DESIGN F2.PNG
│   └── SIMULATIONS/
│       ├── simulation mvt xx' robotic-arm.gif
│       ├── GIF SAMIR-KAHWEJI.gif
│       └── simulation gripper samir-kahweji.gif
│
├── LICENSE
└── README.md
```

**On the folder names inside `ROBOTIC_ARM(3 TYPES)`:** the STL and SolidWorks subfolders aren't always named identically (`dossier tanthif 2`, `STL ROBOTIC ARM 1/2`, etc.) — they grew organically while iterating on three gripper designs in parallel. Each one is self-contained: open any single "TYPE" folder and you'll find everything needed to print and assemble that specific arm + gripper combination on its own, without pulling in the other two.

**On `main assembly conveyor_roboticarm_transportrobot.STEP`:** this is the entire ecosystem — arm, conveyor, and SAMIR-KAHWEJI — combined into one STEP assembly, useful if you want to see how the three subsystems physically relate to each other in space.

---

## CAD Files

This repository includes, for every subsystem (robotic arm ×3 types, conveyor, SAMIR-KAHWEJI):

- **Native SolidWorks source** — full parametric history, under `01_3D/SOLIDWORKS PARTS/`
- **STEP files** — fully editable in any CAD tool, under each subsystem's `STEP FILES/` folder
- **STL files** — ready to slice and print, under `01_3D/STL/`

allowing anyone to inspect, modify, remix, or reproduce any single part of the ecosystem independently.

---

## Simulations

Three motion studies are included in `04_MEDIA/SIMULATIONS/` to show mechanisms that are hard to convey from static renders alone:

- **`simulation mvt xx' robotic-arm.gif`** — the robotic arm traveling along its XX' leadscrew axis.
- <img width="289" height="244" alt="simulation mvt xx&#39; robotic-arm" src="https://github.com/user-attachments/assets/511a82f5-c064-44c0-ad4d-51649ebfd296" />

- **`GIF SAMIR-KAHWEJI.gif`** — SAMIR-KAHWEJI transforming from 6 wheels to 4 during docking.
- <img width="540" height="441" alt="GIF SAMIR-KAHWEJI" src="https://github.com/user-attachments/assets/d0fabeeb-72f3-4f38-b7e2-303c95243f43" />

- **`simulation gripper samir-kahweji.gif`** — SAMIR-KAHWEJI's onboard gripper securing the product.
<img width="366" height="279" alt="simulation gripper samir-kahweji" src="https://github.com/user-attachments/assets/191a87a2-87b6-4b0c-a83e-4c0f57644351" />

---

## On AI Assistance

As with structuring a project of this scale for the first time on GitHub, I used AI assistance to help review this submission against the shipping guide requirements and to help draft and organize this README from my notes. All design work — CAD modeling, PCB design, mechanism choices, and engineering decisions — is my own.

---

## Future Improvements

- Firmware: ESP32 control logic for the arm/conveyor, and the OpenCV sorting pipeline
- Firmware: SAMIR-KAHWEJI line-following + docking control loop on the Raspberry Pi 5 / ESP32
- Exact GPIO pin mapping for both ESP32 boards (documented as a TODO in the wiring notes)
- A 4th, fully community-designed gripper type, to demonstrate the Link 2 standard in practice
- Physical build and photographed assembly, once 3D printing and the camera/Raspberry Pi are accessible

---

## License

This project is released under the **MIT License**.
