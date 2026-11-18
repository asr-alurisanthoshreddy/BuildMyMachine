# Tasks: Assembly Engine

Rules for every task: tests first, working demoable increment, builds on previous tasks, ends with wiring, no orphaned code.

- [ ] Task 1: Assembly state and basic commands
  - **Objective:** Immutable state with add, remove, move, rotate and reparent plus serialisation.
  - **Guidance:** `apply(state, command, resolver)`. Hierarchy with cycle rejection and derived world transforms.
  - **Tests:** Command behaviour, immutability (deep freeze), cycle rejection, world transform derivation, serialise round trip.
  - **Demo:** A script builds a small assembly from commands and prints the hierarchy and world positions.
  - _Requirements: 1, 3.1, 3.2_

- [ ] Task 2: Connections and compatibility
  - **Objective:** Connect and disconnect with port compatibility, occupancy, constraints and snap poses.
  - **Guidance:** Implement `connect`, `disconnect`, port `capacity`, `alignPose`. Issue codes with readable messages.
  - **Tests:** Compatibility table (accepts, mismatch, occupied, capacity, axis and rotation limits). Snap pose cases. Removal cascades with dependants reported.
  - **Demo:** Connect a motor shaft port to a coupling port and see the snap pose. Try a bad pair and read the explanation.
  - _Requirements: 2_

- [ ] Task 3: Graph queries, candidate connections and validation
  - **Objective:** Neighbour and path queries, `findCandidates` and `validateAssembly`.
  - **Guidance:** Include binding validation through the shared validator. Rank candidates by distance and alignment.
  - **Tests:** Graph queries, candidate ranking, validation of each issue type, invalid binding detection.
  - **Demo:** Run `findCandidates` for a loose component and see ranked snap options.
  - _Requirements: 3.3, 4_

- [ ] Task 4: Step predicates and history
  - **Objective:** Serialisable predicates with `checkStep`, and undo/redo.
  - **Guidance:** Predicate union and progress reporting. `AssemblyHistory` wrapper.
  - **Tests:** Each predicate kind with satisfied and unmet cases, progress values, undo/redo sequences, predicates survive JSON round trip.
  - **Demo:** Evaluate a three-predicate step as parts are connected and watch progress rise.
  - _Requirements: 1.3, 5_

- [ ] Task 5: Mode parity and workspace integration
  - **Objective:** Prove one engine serves both modes and render engine state in the 3D workspace.
  - **Guidance:** Add a mapper from `AssemblyState` to `SceneItem[]` in `view3d` integration code (not in `domain`). Add the mode-parity test. Add a minimal dev page that applies commands and shows the result.
  - **Tests:** Parity test (same command sequence through two driver stubs gives equal state). Mapper unit tests. A component test of the dev page.
  - **Demo:** A page where buttons issue commands, and the 3D scene updates from engine state.
  - _Requirements: 6_
