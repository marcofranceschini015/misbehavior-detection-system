# Architecture of the Hybrid Misbehavior Detection System (IEEE Paper)

This repository explains the **architecture and engineering design** behind the system presented in the IEEE paper:

“Physics Joins AI: A Real-Time Hybrid Misbehavior Detection Framework for Vehicular Networks”  
Published on: https://ieeexplore.ieee.org/document/10926031

---

## 🎯 Purpose of This Repository

This is **not** the implementation repository.

This repository exists to clearly explain:

- The architectural design behind the paper
- How the neural network was engineered and embedded
- How AI inference was integrated into a C++ vehicular simulator
- How runtime reliability was maintained during simulation

The full implementation of PLEXE with the Hybrid MDS is available in my fork:

https://github.com/marcofranceschini015/plexe  
(Hosted on :contentReference[oaicite:1]{index=1})

---

# 🚗 What Was Built

The work presented in the paper introduces:

## 1️⃣ A Hybrid Misbehavior Detection System (MDS)

A detection system combining:

- **Neural network–based sequence analysis**
- **Physics-based coherence verification**
- A deterministic **runtime safety fallback mechanism**

---

## 2️⃣ Neural Network Integration Inside a C++ Simulator

A neural network was:

- Designed and trained for misbehavior detection
- Exported as a TensorFlow/Keras model
- Embedded directly inside the PLEXE C++ simulation runtime
- Executed during simulation steps via Pybind11

This means:

> AI inference runs *inside* the C++ vehicular simulation loop.

No external service.  
No offline-only analysis.  
No decoupled pipeline.

The neural model executes in-process and interacts directly with:

- Platooning beacons
- Control logic
- State transitions
- Safety mechanisms

---

## 3️⃣ Real-Time Reliable Simulation Behavior

The system was engineered to ensure:

- Deterministic state transitions
- Stable simulation timing
- Controlled reaction latency
- Measurable mitigation behavior

When misbehavior is detected:

1. The vehicle enters a detection state
2. A safety gap is opened
3. The controller falls back to Adaptive Cruise Control (ACC)
4. The platoon stabilizes in safe mode

All of this happens **within simulation time constraints**.

---

# 🧠 Key Engineering Aspects

This project demonstrates:

- Embedding ML inference in C++ using **Pybind11**
- Runtime model loading and inference inside **OMNeT++**
- Safety-oriented state machine design
- Clean separation between:
  - Simulation App logic
  - AI inference wrapper
  - Control fallback strategy

---

# 📂 Where the Code Lives

The full implementation is in my PLEXE fork:

https://github.com/marcofranceschini015/plexe

Relevant files:

- `src/plexe/apps/AIDefenseApp.cc`
- `src/plexe/AI/NNWrapper.cc`
- `examples/misbehaviourDetection/`

---

# 📘 Official Documentation

Full runtime setup, prerequisites, and experimental configuration:

https://ans.unibs.it/docs/plexeMDS.html

---

# 🏗 Why This Repository Exists

This repository exists to explain:

- Architectural decisions
- System design clarity
- Integration complexity
- Engineering trade-offs

This repository makes the architecture of the IEEE paper explicit and accessible without requiring readers to navigate the entire simulation stack.

---

If you are interested in:

- The experimental evaluation
- Quantitative results
- Attack scenarios
- Dataset details (VeReMi)

Please refer to the paper and official documentation.
