# ADR-0002 — Hybrid Detection (AI + Physics/Coherence Check)

## Context
A single technique (AI-only or rules-only) can be fragile under distribution shifts and adversarial edge cases.

## Decision
Adopt a Hybrid MDS:
- AI inference (neural network)
- coherence / physics-based validation of message contents

## Consequences
- ✅ improved robustness vs AI-only
- ✅ clearer safety reasoning for edge cases
- ❌ slightly higher complexity (two detection signals)