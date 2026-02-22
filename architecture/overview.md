# Architectural Overview (Paper System)

This document summarizes the architecture of the system presented in the IEEE paper:
https://ieeexplore.ieee.org/document/10926031

For complete setup and execution details, refer to the official documentation:
https://ans.unibs.it/docs/plexeMDS.html

---

## 1) System Context

The solution runs inside a vehicular platooning simulation stack:

- **PLEXE** (platooning & controllers)
- **Veins / OMNeT++** (network simulation runtime)
- **SUMO** (traffic simulation)

The contribution of the paper is a **Hybrid Misbehavior Detection System (MDS)** integrated at runtime into the simulation, combining:

- **AI inference (Neural Network)**
- **Physics-based / coherence validation**
- **Deterministic safety reaction**: open gap + **fallback to ACC**

---

## 2) Runtime Components (High Level)

- **AIDefenseApp**  
  Orchestrates the runtime logic during simulation:
  - collects incoming platooning beacons/messages
  - triggers inference + coherence checks
  - manages detection → mitigation → fallback state machine

- **NNWrapper**  
  Bridges C++ runtime with the trained ML model:
  - loads the Keras model (TensorFlow/Keras artifact)
  - loads preprocessing scaler (if present)
  - executes inference inside the simulation process

These are implemented in the PLEXE fork (submodule `src/plexe`):
- `src/plexe/apps/AIDefenseApp.cc`
- `src/plexe/AI/NNWrapper.cc`

---

## 3) Runtime Flow (Conceptual)

1. Vehicle receives platooning beacons (e.g., from the leader / front vehicle)
2. AIDefenseApp extracts the relevant features / sequence window
3. NNWrapper performs in-process inference
4. Hybrid decision combines:
   - AI prediction
   - coherence / physics-based validation
5. If misbehavior is detected:
   - safety gap opening is triggered
   - controller switches to **ACC fallback**
   - system stabilizes in a safer autonomous mode

The whole loop is executed **within simulation time**, without relying on external services.

---

## 4) Why This Architecture Matters (Engineering)

- **Reliability during simulation**: inference runs in-process and is integrated in the runtime loop
- **Clear separation of concerns**: app logic vs inference wrapper
- **Safety-first reaction**: deterministic fallback strategy after detection
- **Reproducibility**: code and configuration are tied to exact commits via submodules