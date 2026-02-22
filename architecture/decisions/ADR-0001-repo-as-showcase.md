# ADR-0001 — Meta-repo as an Architectural Showcase

## Context
The implementation requires a full simulation stack (OMNeT++/Veins/SUMO/PLEXE) and contains large artifacts and dependencies.
This repository targets engineers who want architectural clarity quickly.

## Decision
Keep this repository lightweight and focused on:
- architecture explanation
- design decisions (ADRs)
- references to paper and official documentation
- submodules pointing to the full codebase

## Consequences
- ✅ easy to browse and understand
- ✅ avoids duplication of official documentation
- ✅ code remains in the canonical repositories
- ❌ environment setup is delegated to the official docs