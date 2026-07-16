<!-- HERO IMAGE: replace with a wide FactoryTalk View HMI screenshot of the full warehouse system -->
<p align="center">
  <img src="Images/hero-warehouse.png" alt="Industrial Warehouse Package Sorting Automation System HMI Overview" width="100%">
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

<!-- Drop screenshots into /Images and they render as a grid. -->
| Main Overview HMI | Manual Control Screen | Alarm Screen |
|:---:|:---:|:---:|
| ![Main Overview](Images/hmi-main-overview.png) | ![Manual Control](Images/hmi-manual-control.png) | ![Alarm Screen](Images/hmi-alarms.png) |
| **Toronto Lane** | **Express Lane** | **International Lane** |
| ![Toronto Lane](Images/hmi-toronto-lane.png) | ![Express Lane](Images/hmi-express-lane.png) | ![International Lane](Images/hmi-international-lane.png) |
| **Oversized Lane** | | |
| ![Oversized Lane](Images/hmi-oversized-lane.png) | | |

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

### Package Detection

<!-- 📷 replace with ladder screenshot of the detection routine -->
![Package Detection Logic](Images/logic-package-detection.png)

A photo-eye at the receiving conveyor sets a package-present bit that triggers the sort sequence for that unit. Detection is sensor-driven rather than time-based so the line only indexes when a real package is present, eliminating empty cycles and keeping tracking aligned with actual product on the belt.

---

### Destination Sorting

<!-- 📷 replace with ladder screenshot of the sorting routine -->
![Destination Sorting Logic](Images/logic-destination-sorting.png)

The barcode scanner reads each package destination code, and a data comparison matches it to one of the four lanes, energizing the corresponding divert output as the package reaches the sorting station. A comparison-based routing table was chosen over hard-wired logic so destinations can be reconfigured without rewiring, and so lanes can be added or reassigned in software.

---

### Weight Inspection

<!-- 📷 replace with ladder screenshot of the weight routine -->
![Weight Inspection Logic](Images/logic-weight-inspection.png)

A load cell provides an analog weight value that is scaled and checked against minimum and maximum limits. Packages outside tolerance are flagged and diverted to the Oversized lane. Analog limit comparison was chosen so the acceptance band can be tuned from the HMI without a program change, adapting to different product mixes.

---

### Conveyor Interlocking

<!-- 📷 replace with ladder screenshot of the interlock routine -->
![Conveyor Interlocking Logic](Images/logic-conveyor-interlocking.png)

Each conveyor and divert is gated by a downstream-ready permissive, so a package is never released onto a stopped or full lane. Interlocking was chosen to protect equipment and product: it prevents collisions and jams at merge and divert points, which are the highest-risk locations on any sorting line.

---

### Alarm Handling

<!-- 📷 replace with ladder screenshot of the alarm routine -->
![Alarm Handling Logic](Images/logic-alarm-handling.png)

Jam, scanner-fault, overweight, lane-full, and emergency-stop conditions each set a latched alarm bit surfaced to the FactoryTalk View alarm summary. Alarms are latched and require operator acknowledgement so a transient fault is never missed, and each alarm identifies the affected station for fast diagnosis.

---

### Emergency Stop

<!-- 📷 replace with ladder screenshot of the E-Stop routine -->
![Emergency Stop Logic](Images/logic-emergency-stop.png)

The emergency stop drops a master run permissive that is evaluated ahead of all sequencing logic, de-energizing every conveyor and divert regardless of operating mode. Evaluating the E-Stop first in the scan makes the stop fail-safe: nothing downstream can hold an output once the permissive is removed.

---

### Package Counter

<!-- 📷 replace with ladder screenshot of the counter routine -->
![Package Counter Logic](Images/logic-package-counter.png)

Count-up counters increment per lane and in total as packages are diverted, feeding the throughput displays on the HMI. Per-lane counting was chosen so operators can track production by destination and quickly spot an imbalance that may indicate a scanner or divert problem.

---

### Manual Control

<!-- 📷 replace with ladder screenshot of the manual routine -->
![Manual Control Logic](Images/logic-manual-control.png)

Manual mode lets an operator jog individual conveyors and diverts from the HMI to clear jams and commission the line, while the emergency-stop circuit stays active. A dedicated manual mode was chosen so maintenance can move equipment safely without bypassing the interlocks or the E-Stop.

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
- ✔ Implemented barcode-based destination routing across four sorting lanes
- ✔ Designed analog weight inspection with configurable tolerance and oversized rejection
- ✔ Built conveyor interlocking to prevent collisions and jams at merge and divert points
- ✔ Developed a FactoryTalk View HMI with per-lane screens, package counts, and an alarm summary
- ✔ Verified PLC I/O, HMI communication, and full process operation on Allen-Bradley hardware
- ✔ Validated detection, routing, weight rejection, counting, alarms, and emergency-stop response

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
