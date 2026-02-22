# Hybrid Misbehavior Detection System for PLEXE  
### (AI + Physics-Based Defense for Platooning)

This repository is a **lightweight architectural showcase** of a Hybrid Misbehavior Detection System (MDS) integrated into **PLEXE / Veins / OMNeT++ / SUMO**.

It highlights:
- The architectural design choices
- The runtime integration of a neural network into C++
- The safety fallback strategy (gap opening + ACC fallback)

The full implementation lives in the submodule (`src/plexe`) and in the official documentation.

---

## 🔍 What This Project Demonstrates

- Hybrid detection strategy (AI + physical coherence check)
- Embedded ML inference inside a C++ simulation runtime
- Runtime safety-state transitions (detect → mitigate → fallback)
- Clean separation between simulation app and inference wrapper

---

## 📄 Official Documentation (Source of Truth)

Full technical documentation, setup, and runtime instructions:

https://ans.unibs.it/docs/plexeMDS.html

---

## 📘 Paper

IEEE Xplore:
https://ieeexplore.ieee.org/document/10926031

Preprint:
https://ans.unibs.it/docs/assets/res/plexeMDS.pdf

---

## 🧠 Implementation Location

The full implementation is inside the PLEXE fork:

- App logic (state machine + detection):
  src/plexe/apps/AIDefenseApp.cc

- Neural Network wrapper (Pybind11 + TensorFlow/Keras):
  src/plexe/AI/NNWrapper.cc

- Trained model artifact:
  examples/misbehaviourDetection/NNtraining/training/model.h5

The repository is included as a git submodule in `src/plexe`.

---

## 🏗 Repository Structure

- `architecture/` → architectural overview + design decisions (ADRs)
- `refs/` → links to documentation, paper, and key code entrypoints
- `src/plexe` → full implementation (git submodule)

---

## 🚗 Minimal Run

To run the system, follow the official documentation:

https://ans.unibs.it/docs/plexeMDS.html

Then execute from inside the PLEXE fork:
```bash
cd examples/misbehaviourDetection
./run -u Cmdenv -c Misbehavior4 -r 0
```


This meta-repository intentionally avoids duplicating setup instructions.
