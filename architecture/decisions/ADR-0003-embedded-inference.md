# ADR-0003 — Embedded Inference Inside C++ Simulation Runtime

## Context
The simulator runtime is C++ (PLEXE/OMNeT++). The neural network is trained and stored as a TF/Keras model artifact.

## Decision
Run inference in-process (inside the simulator), via a dedicated C++ wrapper (NNWrapper) that loads the model and executes predictions as part of the simulation loop.

## Consequences
- ✅ inference results are available immediately to control logic
- ✅ no external service or IPC required
- ✅ reliable “runtime” behavior in simulation time
- ❌ requires careful dependency management (Python/TensorFlow bindings)