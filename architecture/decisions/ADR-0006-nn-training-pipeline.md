# ADR-0006 — Neural Network Training Pipeline & Feature Design (VeReMi)

## Context

The runtime defense system integrates a neural network for misbehavior detection inside a C++ simulator (PLEXE).  
This requires an offline training process that:

- learns *relevant patterns* from network/vehicle behavior,
- produces **stable artifacts** that work reliably at runtime,
- preserves *exact consistency* between training preprocessing and runtime inference.

The dataset used for training is the **VeReMi dataset** (Vehicular Reference Misbehavior Dataset):  
https://veremi-dataset.github.io

VeReMi contains labeled vehicular communication sequences under different misbehavior conditions.  
Each record encodes physical states (position, speed, acceleration) and timing information corresponding to real networked vehicle behavior.

## Decision

Design a training pipeline that:

1. Extracts **temporal differences** rather than absolute values,
2. Standardizes each feature with a shared scaler,
3. Trains a neural model with fixed temporal window input,
4. Exports **stable artifacts** for runtime (model + scaler),
5. Ensures *strict equivalence* between training and inference preprocessing.

---

## Why These Features?

### Feature set rationale

The chosen features for training are:

| Feature Name   | Meaning                                           |
|----------------|---------------------------------------------------|
| `dt`           | Time difference between consecutive beacons       |
| `ΔposX`, `ΔposY` | Change in position over dt                       |
| `ΔspeedX`, `ΔspeedY` | Change in speed over dt                     |
| `Δacc`         | Change in acceleration over dt                     |

### Intuition

- **Temporal diffs matter** because raw positions/speeds/accelerations are strongly auto-correlated.
- Differences remove *absolute scale effects* and highlight *dynamic changes*, which are indicative of misbehavior.
- VeReMi includes examples where misbehaving agents exhibit unusual temporal patterns (e.g., inconsistent spacing, erratic acceleration), so difference signals are strong indicators.

### Avoiding absolute states

- Absolute positions/speeds are not ideal for classification because they depend on scenario context (e.g., road shape, platoon length).
- Temporal differences capture *behavioral change*, which generalizes better across scenarios.

---

## Processing & Normalization

1. **Sequence extraction**  
   - From VeReMi, temporal windows are built (length = 4).
   - Each window contains 4 consecutive time-slices of raw states.

2. **Delta computation**  
   - For each window, differences between consecutive states are computed:
     ```
     Δpos = pos[t] - pos[t-1]
     Δspeed = speed[t] - speed[t-1]
     Δacc = acc[t] - acc[t-1]
     dt = t - (t-1)
     ```

3. **Stacking into a tensor**  
   - Window of shape `(time=4, features=6)`:
     ```
     [[dt, ΔposX, ΔposY, ΔspeedX, ΔspeedY, Δacc],
      [...],
      [...],
      [...]]
     ```

4. **Standardization**  
   - A single `StandardScaler` is fit on the entire training set.
   - At runtime, the *same* scaler is loaded from `standard_scaler.pkl`.

---

## Model & Export

- Neural network is trained using TensorFlow/Keras.
- The final architecture and weights are exported as:
  - `model.h5` (trained weights + architecture)
  - `standard_scaler.pkl` (scaler parameters)

These artifacts are what the C++ simulator loads at runtime, ensuring:
- **feature processing identical to training**
- **no discrepancy between training and inference**

---

## How Runtime Uses These Artifacts

At runtime (in `NNWrapper`):

1. Collect 4 consecutive beacon observations
2. Compute the same difference features
3. Apply the loaded `StandardScaler`
4. Form a 4×6 input vector
5. Call `model.predict(...)` inside the simulator loop

This direct in-process inference ensures low latency and reliability.

---

## Consequences

### Pros
- Strict alignment between training and inference
- Input signals reflect *behavioral changes* rather than static state
- Lightweight temporal window → minimal runtime overhead
- Inference can be called synchronously within simulation time

### Cons
- Feature/scaler must be preserved exactly between training and runtime
- Any change in feature ordering or preprocessing will break inference

---

## Implementation pointers

- Training and preprocessing pipeline:
  `src/plexe/examples/misbehaviourDetection/NNtraining/`
- Runtime preprocessing + inference:
  `src/plexe/src/plexe/AI/NNWrapper.cc`
- Architectural orchestration (when and how inference is called):
  `src/plexe/src/plexe/apps/AIDefenseApp.cc`