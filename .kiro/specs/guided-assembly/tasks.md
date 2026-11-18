# Tasks: Guided Assembly

Rules for every task: tests first, working demoable increment, builds on previous tasks, ends with wiring, no orphaned code.

- [ ] Task 1: Script schema and validation
  - **Objective:** A validated, serialisable guided script format.
  - **Guidance:** zod schema in `schema`. Validator for unique numbers, acyclic dependencies, existing definitions and ports, predicate shape.
  - **Tests:** One test per validation rule, valid sample script, JSON round trip.
  - **Demo:** Validate a sample script and a deliberately broken one with clear error output.
  - _Requirements: 1_

- [ ] Task 2: Guided runner state machine
  - **Objective:** Evaluate predicates, order steps with dependencies, support go-back.
  - **Guidance:** Pure `GuidedRunner` with snapshots and hint levels.
  - **Tests:** Sequential and parallel steps, dependency blocking, go-back restoring a snapshot, completion detection.
  - **Demo:** A console script plays a sample script against scripted commands and prints step transitions.
  - _Requirements: 2.3, 2.4, 4.2_

- [ ] Task 3: Step panel, highlighting and progress
  - **Objective:** Show the current step, its explanation, progress and highlights in the workspace.
  - **Guidance:** `StepPanel`, `StepList`, highlight mapping. Place components using the shared interaction components.
  - **Tests:** Component tests for rendering step content and progress, highlight props passed to the workspace, keyboard operation, axe.
  - **Demo:** Open a sample playground in Guided mode, see the highlighted part and target, place it and watch the step advance.
  - _Requirements: 2.1, 2.2, 6_

- [ ] Task 4: Hints, mistake feedback and completion
  - **Objective:** Escalating hints, plain-language explanations of mistakes and a completion summary with final validation.
  - **Guidance:** Hint levels, mapping engine issues to messages (shared with Free Assembly table). Final `validateAssembly`.
  - **Tests:** Hint escalation, incorrect move messages, completion flow, rejection of invalid states.
  - **Demo:** Make a wrong connection and see the explanation. Request hints. Finish and see the summary.
  - _Requirements: 3, 4_

- [ ] Task 5: Mode switching, progress persistence interface and end-to-end flow
  - **Objective:** Switch between Guided and Free without losing state, and keep progress per playground version.
  - **Guidance:** Shared assembly store between modes. `ProgressStore` interface with a `localStorage` implementation.
  - **Tests:** Mode switch preserves state, progress restore after reload, Playwright: complete the sample script, switch to Free, continue.
  - **Demo:** Complete half of a script, reload, resume, then switch to Free Assembly with the same parts.
  - _Requirements: 2.5, 5_
