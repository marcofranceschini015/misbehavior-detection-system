# ADR-0005 — Use Git Submodules for Reproducibility

## Context
The system depends on multiple repositories (PLEXE fork + SUMO + Veins).
We want a clean “meta” repository without copying code.

## Decision
Use git submodules for:
- `src/plexe`
- `src/sumo`
- `src/veins`

Each submodule pins a specific commit for reproducible builds.

## Consequences
- ✅ reproducible state (exact commits)
- ✅ clean separation and ownership across repos
- ❌ contributors must clone with `--recurse-submodules`