# ADR-0004 — Detection Triggers Safety Gap + ACC Fallback

## Context
In platooning, malicious or inconsistent messages can destabilize spacing control (CACC).
A mitigation must be deterministic and safety-oriented.

## Decision
When misbehavior is detected at runtime:
1) open a safety gap (gap control)
2) switch to **ACC fallback** once safe spacing is achieved

## Consequences
- ✅ deterministic mitigation path
- ✅ measurable reaction time and safety impact
- ❌ may reduce throughput/efficiency when triggered