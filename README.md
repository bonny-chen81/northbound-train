#  Northbound Train Rocket Team – System Design Report

**Competition:** Taiwan Rocket Cup 2025  
**Category:** High School Division – 1 km Altitude Mission  
**Team:** Northbound Train Rocket Team (北上火車火箭隊)  
**Date:** March 2025  
**Advisor:** Mr. Lin Chih-Chieh  
**Author:** Bonny Chen  
**Affiliation:** Taipei First Girls High School  

---

<details>
<summary> Table of Contents</summary>

- [1. Project Overview](#1-project-overview)
- [2. Team Structure and Management](#2-team-structure-and-management)
- [3. System Architecture](#3-system-architecture)
- [4. Subsystem Design](#4-subsystem-design)
  - [4.1 Structural Subsystem](#41-structural-subsystem)
  - [4.2 Recovery Subsystem](#42-recovery-subsystem)
  - [4.3 Avionics and Payload](#43-avionics-and-payload)
- [5. Flight Simulation](#5-flight-simulation)
- [6. Testing and Verification Plan](#6-testing-and-verification-plan)
- [7. Risk and Safety Analysis](#7-risk-and-safety-analysis)
- [8. Budget Summary](#8-budget-summary)
- [9. Future Development](#9-future-development)
- [10. Appendix](#10-appendix)

</details>

---

## 1. Project Overview
The **Northbound Train Rocket Team** was established in 2023 to participate in the **Taiwan Rocket Cup 2025**, the first nationwide competition requiring high-school teams to design, build, and launch rockets exceeding 1 km altitude.

### Mission Objectives
- Reach a target apogee of **1000 ± 100 m**.  
- Achieve **full telemetry recording** and **successful parachute recovery** over sea.  
- Retrieve complete flight data (pressure, altitude, acceleration, GPS).  
- Maintain flight stability margin > 1.5 calibers under all wind conditions.

### Key Performance Indicators
| Parameter | Requirement | Target (Predicted) |
|------------|--------------|-------------------|
| Altitude | 1000 ± 100 m | 1167 m |
| Max Velocity | < Mach 1.2 | Mach 0.93 |
| Stability Margin | > 1.5 cal | 1.8 cal |
| Terminal Velocity | < 12 m/s | 10.6 m/s |

---

## 2. Team Structure and Management
The team consists of **17 members** divided into four technical subsystems.

| Role | Member | Responsibilities |
|------|---------|------------------|
| Structural Subsystem team lead / Vice captain | Bonny Chen | Overall coordination, design review, structural modeling |
| Structural Subsystem | 4 members | CAD design, FEM analysis, manufacturing |
| Avionics Subsystem | 4 members | Sensor integration, telemetry, data logging |
| Recovery Subsystem | 4 members | Parachute design, deployment mechanism |
| Systems Integration | 3 members | Assembly, Schedule management, report writing |

### Project Management
- Gantt chart covers 9 months: design → simulation → prototype → integration → launch.  
- Weekly subsystem meetings + monthly integrated design reviews.  
- Risk tracking using simplified **FMEA** (Failure Mode and Effects Analysis).

---

## 3. System Architecture
The rocket consists of three major sections:

| Section | Material | Function |
|----------|-----------|-----------|
| Nose Cone | Fiberglass composite | Houses avionics bay; minimizes drag |
| Fuselage | PVC pipe (Φ 75 mm) | Holds parachute bay & payload module |
| Fin Assembly | Carbon fiber laminate | Provides directional stability |

**Overall Dimensions:**  
- Length = 1.25 m  
- Diameter = 75 mm  
- Liftoff Weight ≈ 2.1 kg  
- Center of Pressure – Center of Gravity distance = 76 mm  

---

## 4. Subsystem Design

### 4.1 Structural Subsystem
- Modeled in **Fusion 360**; analyzed for axial compression, fin flutter, and buckling.  
- Nose cone fabricated by **fiberglass lay-up** over a 3D-printed mold.  
- Fin flutter frequency > 210 Hz → safe margin > 2× maximum aero load.  
- Coupler joint reinforced by 3D-printed PLA inner sleeve.  
- Safety Factor > 1.35 in all tests.

| Component | Material | Method |
|------------|-----------|--------|
| Nose cone | Fiberglass + epoxy | Hand lay-up |
| Fuselage | PVC pipe | Mechanical join + epoxy seal |
| Fins | Carbon fiber sheet | CNC cut + epoxy bond |

---

### 4.2 Recovery Subsystem
- Dual redundant **parachute deployment system**: CO₂ pneumatic + spring servo.  
- Drogue and main chutes packed separately to reduce entanglement.  
- Ground tests verified deployment at 70 m (altimeter trigger).  
- Terminal velocity ≈ 10.6 m/s.  
- Floatation module added for sea recovery.

| Parachute | Diameter | Material | Deployment |
|------------|-----------|-----------|-------------|
| Drogue | 0.6 m | Ripstop nylon | CO₂ burst release |
| Main | 1.2 m | Ripstop nylon | Servo mechanical trigger |

---

### 4.3 Avionics and Payload
- **Dual Arduino flight computer** with redundant power supply.  
- Sensors: BMP388 (pressure), MPU6050 (accel/gyro), GPS NEO-6M.  
- Data transmission via **LoRa SX1278**, 915 MHz, up to 2 km range.  
- Ground station GUI developed in Python (PySerial + Matplotlib).  
- Data logged at 50 Hz, stored on microSD.

**Avionics Stack:**

---

## 5. Flight Simulation
Simulations performed using **OpenRocket v22.02** with 10 mm mesh resolution for fin and cone geometry.

| Wind Speed | Apogee (m) | Flight Time (s) | Stability Margin (cal) |
|-------------|-------------|-----------------|-------------------------|
| 0 m/s | 1185 | 17.9 | 1.83 |
| 2 m/s | 1154 | 18.1 | 1.79 |
| 4 m/s | 1103 | 18.4 | 1.72 |

The rocket remains stable (1.7–1.9 calibers) under all conditions. Drift distance under 4 m/s wind ≈ 43 m.

![openrocket_simulation](assets/openrocket_simulation.png)

---

## 6. Testing and Verification Plan
Verification follows the **V-Model** approach linking each mission requirement (MR) to a test method.

| ID | Requirement | Verification Method | Responsible |
|----|--------------|---------------------|--------------|
| MR-1 | Altitude > 1000 m | Simulation + flight test | System |
| MR-2 | Safe deployment < 15 m/s | Drop test + launch test | Recovery |
| MR-3 | Real-time telemetry < 1 s delay | Ground station test | Avionics |
| MR-4 | Structure SF > 1.3 | FEM analysis | Structure |
| MR-5 | Stable flight > 1.5 cal | Simulation | System |

**Integration Tests**
1. Ground integration of avionics and recovery trigger.  
2. Parachute drop test from 30 m drone.  
3. Static engine mount test for thrust stability.  

---

## 7. Risk and Safety Analysis
Simplified **FMEA** identified 18 potential failure modes.  
Major mitigations:

| Failure Mode | Risk | Mitigation |
|---------------|-------|-------------|
| Parachute non-deployment | High | Dual trigger system + manual override |
| Avionics power loss | Medium | Dual battery redundancy |
| Fin delamination | Medium | Vacuum bonding + carbon reinforcement |
| GPS signal loss | Low | Onboard data logging backup |
| Ocean landing loss | Medium | Added floatation foam + waterproof housing |

Safety training: every launch follows TASA guidelines, with GO/NO-GO wind limit > 4.3 m/s.

---

## 8. Budget Summary
| Subsystem | Cost (NT$) | Notes |
|------------|-------------|-------|
| Structure | 26 ,060 | Fiberglass, PVC, CF fins |
| Avionics | 8 ,000 | Sensors + LoRa modules |
| Recovery | 7 ,000 | Parachutes + CO₂ trigger |
| Ground Station | 3 ,000 | Receiver + GUI development |
| Launch logistics | 2 ,500 | Transport + permits |
| **Total** | **≈ 46 ,500** | Funded by school STEM budget |

---

## 9. Future Development
- **Advanced Avionics:** integrate Kalman filter and onboard attitude estimation.  
- **Aerodynamic Optimization:** collaborate with the *Biomimetic Nose Cone Project* to improve drag performance.  
- **Supersonic Testing:** adapt design for Mach 1.5 target in 2026 cycle.  
- **Materials Upgrade:** switch to carbon epoxy fuselage and aluminum bulkhead connectors.

---

## 10. Appendix
**Bill of Materials, CAD Drawings, and OpenRocket Files** are stored in the repository:

---

### 📎 Related Projects
- **Biomimetic Nose Cone CFD Study** – Independent research by Bonny Chen  
  👉 [github.com/bonny-chen81/bionic-nosecone](https://github.com/bonny-chen81/bionic-nosecone)

---

**Northbound Train Rocket Team** – *Taipei First Girls High School*  
> “We head north, against the wind, with curiosity as our fuel.”  

[↑ Back to top](#1-project-overview)


