<!-- HERO IMAGE -->
<p align="center">
  <img src="warehouse-destination%20hmi.png" alt="Industrial Warehouse Package Sorting Automation System HMI Overview" width="100%">
</p>

<h1 align="center">Industrial Warehouse Package Sorting Automation System</h1>

<p align="center">
  Allen-Bradley CompactLogix PLC control system that automates warehouse package handling from receiving through barcode identification, weight inspection, destination sorting, packaging, and shipment, with full FactoryTalk View HMI supervision.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Allen--Bradley-CC0000?style=for-the-badge&logo=rockwellautomation&logoColor=white">
  <img src="https://img.shields.io/badge/Studio%205000-004B87?style=for-the-badge">
  <img src="https://img.shields.io/badge/CompactLogix-E32119?style=for-the-badge">
  <img src="https://img.shields.io/badge/FactoryTalk%20View-F58025?style=for-the-badge">
  <img src="https://img.shields.io/badge/EtherNet%2FIP-0072C6?style=for-the-badge">
  <img src="https://img.shields.io/badge/Ladder%20Logic-2E8B57?style=for-the-badge">
  <img src="https://img.shields.io/badge/Industrial%20Automation-455A64?style=for-the-badge">
  <img src="https://img.shields.io/badge/Material%20Handling-6A1B9A?style=for-the-badge">
</p>

---

## ▶️ Demo

<!-- Replace with an embedded GIF or a linked MP4/YouTube walkthrough. GIFs autoplay inline on GitHub. -->
<p align="center">
  <img src="Videos/system-demo.gif" alt="System demo, package flow from receiving through sorting to loading dock" width="90%">
</p>

---

## 📌 Project Overview

An industrial control system that automates the full warehouse package-handling process, moving product from receiving through inspection, sorting, packaging, and shipment under PLC-based sequential control.

Packages enter on a receiving conveyor, are identified by a barcode scanner, and are checked at a weight station before the main conveyor indexes them to a sorting station that diverts each one to its destination lane. Conveyor coordination and interlocking keep product moving without collisions, weight inspection rejects oversized or out-of-tolerance packages, and destination routing directs each package to the correct lane.

The system is supervised end-to-end from a FactoryTalk View HMI, with live status, destination lane screens, package counts, and an alarm summary giving the operator complete visibility and control of the line.

### 🎯 Control Objectives

- Automatically detect packages entering the line.
- Identify each package destination from its barcode.
- Inspect package weight and reject oversized or out-of-tolerance packages.
- Coordinate and interlock the conveyors to prevent collisions and jams.
- Route each package to its correct destination lane.
- Count packages per lane and in total for throughput tracking.
- Monitor faults and annunciate alarms on the HMI.
- Provide emergency-stop and manual-mode operation for safety and maintenance.
- Run continuously with no operator intervention under normal conditions.

---

## ⭐ Project Highlights

| Feature | Value |
|---------|-------|
| **Process** | Warehouse Package Sorting |
| **PLC** | Allen-Bradley CompactLogix |
| **HMI** | FactoryTalk View |
| **Communication** | EtherNet/IP |
| **Control Type** | Sequential Material Handling |
| **Destination Lanes** | Toronto, Express, International, Oversized |
| **Testing** | Commissioned and Tested on Allen-Bradley Hardware |

---

## ✨ Features

- ✔ Automatic Package Detection
- ✔ Barcode Identification
- ✔ Weight Inspection
- ✔ Oversized Package Rejection
- ✔ Destination Sorting (Four Lanes)
- ✔ Conveyor Interlocking
- ✔ Package Counting
- ✔ Alarm Handling
- ✔ Emergency Stop
- ✔ Manual Mode
- ✔ FactoryTalk View HMI Supervision


---

## 🏗️ System Architecture

```mermaid
flowchart TB
HMI["🖥️ FactoryTalk View HMI"] <-->|EtherNet/IP| PLC["Allen-Bradley<br/>CompactLogix PLC"]
ALM["🚨 Alarm System"] <--> PLC
PLC --> RC["📥 Receiving Conveyor"]
RC --> BC["🏷️ Barcode Scanner"]
BC --> WS["⚖️ Weight Station"]
WS --> MC["➡️ Main Conveyor"]
MC --> SORT["🔀 Sorting Station"]
SORT --> L1["Toronto Lane"]
SORT --> L2["Express Lane"]
SORT --> L3["International Lane"]
SORT --> L4["Oversized Lane"]
L1 --> PK["📦 Packaging"]
L2 --> PK
L3 --> PK
L4 --> PK
PK --> DOCK["🚚 Loading Dock"]
```

<sub>Packages flow from receiving through identification and inspection to a sorting station that diverts each one to its destination lane, then to packaging and the loading dock. The HMI and alarm system communicate with the PLC over EtherNet/IP for supervision and annunciation.</sub>

---

## 🖼️ Project Gallery

| Main Overview HMI | Manual Control Screen | Alarm Screen |
|:---:|:---:|:---:|
| ![Main Overview](Warehouse-%20overview.png) | ![Manual Control](warehouse-manual%20control%20hmi.png) | ![Alarm Screen](warehouse-%20alarms%20hmi.png) |

---

## 📹 System Demonstrations

### Alarm Activation

<!-- 🎥 link or embed Videos/AlarmActivation.mp4 -->
[▶ AlarmActivation.mp4](Videos/AlarmActivation.mp4)

Triggers a fault condition and shows the alarm latching, annunciating on the FactoryTalk View alarm summary, and clearing on operator acknowledgement.

---

### Destination Selection

<!-- 🎥 link or embed Videos/DestinationSelection.mp4 -->
[▶ DestinationSelection.mp4](Videos/DestinationSelection.mp4)

Reads a package barcode and shows the PLC routing it to the selected lane, energizing the correct divert and updating the lane screen and package count.

---

## ⚙️ PLC Logic

### Package Detection, Counting & Weight

![Package Detection, Counting and Weight](Warehouse-%20numberweight.png)

We want a live package count and a quick good-or-bad weight check, but only while the line is actually running. So every rung starts with `Sys_Online`. `Package_Entry_Sensor` bumps a `CTU` (`Number_of_packages`) each time a package comes in. For weight, `Weight_OK_Sensor` sets `Weight_OK` to pass a good package, and `Weight_Overweight_Sensor` turns on `OverWeight_lane` to kick a heavy one over to the oversized lane.

---

### Destination Sorting

![Destination Sorting Logic](warehouse-destination.png)

Once a package passes the weight check, it needs to end up in the right lane. Every rung needs `Sys_Online`, and the normal lanes also need `Weight_OK`. An `EQU` compares `Destenation_code` to 1, 2, or 3 and fires the matching lane, `Lane_1` through `Lane_3`. `Lane_4` is the exception: it uses `OverWeight_lane` with code 4, so overweight packages get routed there instead.

---

### Conveyor Interlocking

![Conveyor Interlocking Logic](Warehouse-interlocking.png)

A conveyor should never feed product onto one that's stopped, and any safety trip should take the whole line down. So each conveyor only runs if the one before it is running and `Saftey_OK` is good. `Conveyor1_Motor` and `Saftey_OK` start `conveyor2_Motor`, which along with `Saftey_OK` starts `Main_Conveyor_Motor`. Because `Saftey_OK` is in every rung, losing it drops everything downstream.

---

### Alarm Handling

![Alarm Handling Logic](warehouse-%20alarms.png)

Faults should stay on until someone acknowledges them, not clear on their own. A jam is time-qualified first: `Conv_Jam_Sensor` runs `Jam_onTimer` (10 s), and only when it finishes does `Alarm_Jam` latch, so a quick blockage doesn't nuisance-trip. `E_Stop` latches `Alarm_EStop`, and if `Conveyor1_Motor` is running but `Motor1_feedback` is missing, `Alarm_Motor_Fault` latches. Each one is held with an `OTL` until its reset (`Reset_Jam`, `Reset_E_Stop`, `Reset_Motor_Alarm`) clears it.

---

### System Start (System Online)

![System Start Logic](warehouse-start.png)

The line needs one clean start that only runs when everything is safe. `Master_start_pb` sets `Sys_Online`, which seals in through its own contact so it stays on after you let go of the button, and holds while `Saftey_OK` is good and `Master_Stop_Pb` and `E_Stop` are clear. `Saftey_OK` is just the no-active-alarms check: it is only true when `E_Stop`, `Alarm_Jam`, `Alarm_Motor_Fault`, and `Alarm_EStop` are all clear. Once both are on, `Conveyor1_Motor` starts and the interlock chain takes over.

---

### Full PLC Logic Walkthrough

<!-- 🎥 link or embed Videos/LogicWalkthrough.mp4 -->
[▶ LogicWalkthrough.mp4](Videos/LogicWalkthrough.mp4)

A complete rung-by-rung walkthrough of the program, following a package from detection and barcode read through weight inspection, destination routing, interlocking, and counting, with the alarm handling and emergency-stop response demonstrated live on the HMI.

---

## 🧠 Engineering Challenges

- **<ins>Preventing package collisions</ins>**: sequencing detection, indexing, and diverts so two packages never meet at a merge or divert point.
- **<ins>Coordinating multiple conveyors</ins>**: interlocking receiving, main, and lane conveyors so product only moves when the next stage is ready.
- **<ins>Ensuring destination accuracy</ins>**: tracking each package from barcode read to divert so it reaches the correct lane every time.
- **<ins>Handling oversized packages</ins>**: catching out-of-tolerance weight and routing rejects cleanly without stalling the line.
- **<ins>Maintaining continuous throughput</ins>**: keeping the line indexing steadily under normal conditions with no operator intervention.
- **<ins>Designing scalable ladder logic</ins>**: structuring the routing and interlocks so lanes can be added or reassigned in software.
- **<ins>Preventing false alarms</ins>**: qualifying fault conditions so transient sensor states do not nuisance-trip the alarm summary.

---

## ✅ Testing & Validation

| Function | Status |
|----------|:------:|
| Package Detection | ✅ Verified |
| Barcode Reading | ✅ Verified |
| Weight Inspection | ✅ Verified |
| Destination Routing | ✅ Verified |
| Oversized Detection | ✅ Verified |
| Package Counter | ✅ Verified |
| Alarm Handling | ✅ Verified |
| Emergency Stop | ✅ Verified |
| HMI Communication | ✅ Verified |
| PLC Communication | ✅ Verified |

---

## 📈 Results

- ✔ Developed a complete PLC control system for an automated warehouse package-sorting line
- ✔ Simulated four independent destination lanes with barcode-based routing
- ✔ Implemented seven FactoryTalk View HMI screens for operator supervision
- ✔ Integrated nine digital inputs and eight digital outputs
- ✔ Built conveyor interlocking and a sealed-in System Online start with safety aggregation
- ✔ Validated package detection, counting, weight rejection, alarm response, and emergency-stop logic
- ✔ Verified PLC logic, HMI communication, and complete process operation in the Studio 5000 and FactoryTalk View development environment

---

## 🛠️ Technical Skills Demonstrated

![PLC Programming](https://img.shields.io/badge/PLC%20Programming-0A66C2?style=flat-square)
![Allen-Bradley](https://img.shields.io/badge/Allen--Bradley-CC0000?style=flat-square)
![Studio 5000](https://img.shields.io/badge/Studio%205000-004B87?style=flat-square)
![FactoryTalk View](https://img.shields.io/badge/FactoryTalk%20View-F58025?style=flat-square)
![Industrial Automation](https://img.shields.io/badge/Industrial%20Automation-455A64?style=flat-square)
![Conveyor Control](https://img.shields.io/badge/Conveyor%20Control-00695C?style=flat-square)
![Material Handling](https://img.shields.io/badge/Material%20Handling-6A1B9A?style=flat-square)
![Sequential Control](https://img.shields.io/badge/Sequential%20Control-1565C0?style=flat-square)
![Timers](https://img.shields.io/badge/Timers-283593?style=flat-square)
![Counters](https://img.shields.io/badge/Counters-00838F?style=flat-square)
![Comparisons](https://img.shields.io/badge/Comparisons-4A148C?style=flat-square)
![Alarm Management](https://img.shields.io/badge/Alarm%20Management-D84315?style=flat-square)
![HMI Development](https://img.shields.io/badge/HMI%20Development-F58025?style=flat-square)
![Industrial Networking](https://img.shields.io/badge/Industrial%20Networking-0072C6?style=flat-square)
![EtherNet/IP](https://img.shields.io/badge/EtherNet%2FIP-0072C6?style=flat-square)
![System Validation](https://img.shields.io/badge/System%20Validation-37474F?style=flat-square)

---

## 👤 About the Author

**Anil Pantula**, Electrical Engineering Student, University of Windsor
Automation Technician Co-op @ Asamaka Industries Ltd.

Pursuing roles in Industrial Automation · Controls Engineering · PLC Programming · Robotics · Mechatronics

<!-- Add LinkedIn / email links here -->

<p align="center"><sub>Engineering portfolio project. Not an open-source software library.</sub></p>
