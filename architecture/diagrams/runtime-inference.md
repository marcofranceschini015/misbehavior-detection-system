# Runtime Inference Flow (Embedded AI in C++ Simulator)

This diagram shows how neural network inference is executed
**inside the PLEXE C++ simulation loop**.

The model is loaded once at initialization and then called
during beacon processing in simulation time.

```mermaid
sequenceDiagram
    participant Beacon as PlatooningBeacon
    participant App as AIDefenseApp (C++)
    participant FE as Feature Extraction
    participant NN as NNWrapper (Pybind11)
    participant Model as Keras Model (model.h5)
    participant Ctrl as Controller Logic

    Beacon->>App: receive beacon (t)
    
    App->>FE: update rolling window (last 4 samples)
    FE-->>App: 4x6 feature matrix (raw diffs)

    App->>NN: predict(features)

    NN->>NN: apply StandardScaler
    NN->>Model: model.predict(input)

    Model-->>NN: probability / class
    NN-->>App: prediction result

    alt misbehavior detected
        App->>Ctrl: start gap control
        Ctrl->>Ctrl: open safety gap
        Ctrl->>Ctrl: switch to ACC fallback
    else normal behavior
        App->>Ctrl: continue platoon following
    end