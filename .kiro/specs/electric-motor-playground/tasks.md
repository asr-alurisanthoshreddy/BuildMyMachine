# Tasks: Electric Motor Playground

Rules for every task: tests first, working demoable increment, builds on previous tasks, ends with wiring, no orphaned code. All content is draft until reviewed. Every task must keep `check-core-unchanged` passing.

- [ ] Task 1: Reuse check harness and component definitions
  - **Objective:** Confirm the protected-path check works on this spec and add the nine definitions with placeholder assets.
  - **Guidance:** Create `content/port-types.json` entries, nine `definition.json` files and primitive-based GLBs with ports. Mark internal parts. Record any platform gap instead of changing core.
  - **Tests:** Run `check-core-unchanged` against a deliberately modified core fixture (must fail) and against this change (must pass). Content validation tests.
  - **Demo:** Browse the nine components in the library browser. Show the passing check.
  - _Requirements: 1, 5_

- [ ] Task 2: Assembled playground and explore
  - **Objective:** The assembled motor as a platform playground with isolation of internal parts.
  - **Guidance:** `playground.json`, seeding through the standard pipeline, Discover entry via data. Exploded view as content (poses or separate variant) per `design.md`.
  - **Tests:** Playground validation, loader test, Playwright: open, select parts, isolate rotor.
  - **Demo:** Open the Electric Motor playground and explore it, with the housing made transparent.
  - _Requirements: 1.4, 2_

- [ ] Task 3: Guided script (9 steps)
  - **Objective:** Guided assembly of the motor.
  - **Guidance:** Write the script with explanations and hints. Outcome-based predicates.
  - **Tests:** Replay test through the engine, script validator test, Playwright: complete guided assembly.
  - **Demo:** Complete the nine steps in the browser.
  - _Requirements: 3_

- [ ] Task 4: Simulation program
  - **Objective:** Spin the rotor with approved bindings and the honesty notice.
  - **Guidance:** Bindings, state machine, support level `partial` or `full` as achievable. Record a platform gap if commutation signals cannot be expressed.
  - **Tests:** Determinism and snapshot tests, preflight failure test (no rotor connection), Playwright: run, pause, resume, reset.
  - **Demo:** Assemble the motor and press Start to see the rotor spin with the notice visible.
  - _Requirements: 4_

- [ ] Task 5: Learning content, saving and reuse sign-off
  - **Objective:** Beginner content, saved assemblies and the final reuse acceptance.
  - **Guidance:** Draft content for parts, steps and states. Verify saving and progress. Compile the reuse report: protected-path result, empty exception list (or product-owner waiver), unchanged existing tests, no bottle-capping references.
  - **Tests:** Learning coverage lint, no-bottle-capping-reference test, full CI run, check-core-unchanged on the final diff.
  - **Demo:** Present the reuse report and a walkthrough of the playground end to end.
  - _Requirements: 5, 6_
