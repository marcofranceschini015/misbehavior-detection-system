# Protocol / Defense State Machine (Runtime)

This state machine reflects the **runtime behavior** implemented by the defense app:
- default state: FOLLOWING
- upon detected attack OR received warning: enter GAP_CONTROL (open safety gap)
- after gap is reached: switch controller and move to AUTONOMOUS (ACC fallback)

> Rendered diagram below (Mermaid).

```mermaid
stateDiagram-v2
    [*] --> FOLLOWING

    FOLLOWING: Normal platoon following\n(CACC/PLOEG depending on setup)

    FOLLOWING --> GAP_CONTROL: attack detected (NN != 0)\nOR warning received
    GAP_CONTROL: Open safety gap\n(setWarning=true, radar=true)\nupdateGap() loop

    GAP_CONTROL --> AUTONOMOUS: gap reached
    AUTONOMOUS: Safe mode\nACC fallback + safe headway

    AUTONOMOUS --> [*]