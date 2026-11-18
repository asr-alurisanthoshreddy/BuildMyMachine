# Tasks: Bottle-Capping Machine

Rules for every task: tests first, working demoable increment, builds on previous tasks, ends with wiring, no orphaned code. All content is draft until reviewed.

- [ ] Task 1: Component definitions and placeholder assets
  - **Objective:** The eight new definitions (the motor comes from seed content) with placeholder GLBs and ports, passing the validators.
  - **Guidance:** Author `definition.json` and primitive-based GLBs with `port_*` nodes. Add any new port types to `content/port-types.json`. Set `supportedBehaviours` per the requirements table.
  - **Tests:** Content validation tests for each definition and asset.
  - **Demo:** Browse all components in the library browser and preview each in 3D with ports shown.
  - _Requirements: 1.1–1.3_

- [ ] Task 2: Playground definition and explore experience
  - **Objective:** The assembled machine as a platform-owned playground that can be loaded and explored.
  - **Guidance:** Write `playground.json` with instances and connections. Seed command publishes version 1 through the pipeline. Add beginner descriptions for each component.
  - **Tests:** Playground validation test. Loader test. Playwright: open the machine, select components, isolate the capping head.
  - **Demo:** Open the Bottle-Capping Machine from Discover and explore it.
  - _Requirements: 1.4, 2_

- [ ] Task 3: Guided script (12 steps)
  - **Objective:** Complete guided assembly of the machine.
  - **Guidance:** Write `guided-script.json` with explanations and hints. Predicates are outcome-based.
  - **Tests:** Replay test: scripted commands complete all steps in order and the final assembly validates. Script validator test. Playwright: complete the guided assembly.
  - **Demo:** Complete the 12 steps in the browser and see the completion summary.
  - _Requirements: 3_

- [ ] Task 4: Simulation program and bindings
  - **Objective:** The operating sequence running on approved behaviours.
  - **Guidance:** Platform-content bindings on instances. Program with states and signals as in `design.md`. Add the kinematic notice and support level `full`.
  - **Tests:** Determinism and fixed-step snapshot tests, event order test, preflight failure test. Playwright: run, pause, resume and reset.
  - **Demo:** Assemble (or load the assembled machine), press Start and watch a bottle get detected, stopped, capped and released.
  - _Requirements: 5_

- [ ] Task 5: Free assembly check, saving and platform-gap review
  - **Objective:** Confirm the machine works in Free Assembly and with saving, and review platform gaps.
  - **Guidance:** Verify components combine with seed library parts. Record gaps found. Run the `check-core-unchanged` script (created in `platform-foundation`) on this spec's changes and justify any exception.
  - **Tests:** Free assembly test with the new components. Save and reload test. Core-diff check passes or lists justified, reviewed exceptions.
  - **Demo:** Build a custom variant in Free Assembly, save it, reload it. Show the gap log and the core-diff report.
  - _Requirements: 4, 7_
