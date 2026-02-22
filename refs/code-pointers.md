# Key Code Entry Points (in src/plexe submodule)

Main simulation app (orchestrates runtime detection + mitigation):
- `src/plexe/src/plexe/apps/AIDefenseApp.cc`

Neural network wrapper (embedded inference in C++ runtime):
- `src/plexe/src/plexe/AI/NNWrapper.cc`

Model artifact (present in the PLEXE fork):
- `src/plexe/examples/misbehaviourDetection/NNtraining/training/model.h5`