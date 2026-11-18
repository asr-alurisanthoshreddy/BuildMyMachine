# Tasks: Simulation Engine

Rules for every task: tests first, working demoable increment, builds on previous tasks, ends with wiring, no orphaned code.

- [ ] Task 1: Fixed-timestep core and controls
  - **Objective:** A deterministic simulator with start, pause, resume, reset, speed and single step.
  - **Guidance:** `Simulator` with accumulator, `runSteps`, `snapshot`, `restore`. No wall clock.
  - **Tests:** Determinism snapshots, frame-rate independence with varied `realDt`, pause/resume exactness, reset equals initial state, max-steps clamp flag.
  - **Demo:** A script steps a trivial program 600 times twice and shows identical output hashes.
  - _Requirements: 1, 2_

- [ ] Task 2: Behaviour interface, registry wiring and binding enforcement
  - **Objective:** Register behaviour implementations with the library registry and reject invalid bindings.
  - **Guidance:** `Behaviour` type, registration helper, use the shared binding validator. Implement `rotary-actuator` first.
  - **Tests:** Rotary actuator table tests. Binding rejection: unknown id, not on allow-list, bad params, missing approval, forbidden origin.
  - **Demo:** Bind a rotary actuator to a motor instance, run the simulation and print the shaft angle over time. Try an invalid binding and see the error.
  - _Requirements: 4.1, 4.3, 4.4, 5_

- [ ] Task 3: Remaining initial behaviours and items
  - **Objective:** Conveyor, linear actuator, proximity sensor and sequence controller, with simulated items.
  - **Guidance:** Zone-based detection. Deterministic update order. Event log.
  - **Tests:** Each behaviour's table tests, sensor trigger at exact step, event ordering, items carried by the conveyor.
  - **Demo:** A small program moves an item along a conveyor, triggers a sensor and extends an actuator, shown as an event log.
  - _Requirements: 3.2, 3.3, 4.2_

- [ ] Task 4: Programs, state machine and preflight
  - **Objective:** Load programs with a state machine and refuse to start when preflight fails.
  - **Guidance:** Program schema in `schema`. `preflight` with issue codes and readable messages.
  - **Tests:** Transitions by step, timer, sensor and signal. Each preflight issue. Program JSON round trip.
  - **Demo:** Run a valid program, then break it (missing connection) and see the problem list instead of a run.
  - _Requirements: 3, 6_

- [ ] Task 5: Playback UI, honesty notice and end-to-end flow
  - **Objective:** Controls, notice, event log and transforms applied in the workspace.
  - **Guidance:** `SimulationControls`, `SimulationNotice` (always visible), `EventLog`, `applyTransforms`. Show the support level. Wire into the Playground route behind a "Simulate" action.
  - **Tests:** Component tests (controls states, notice present, log text), axe checks, Playwright: run, pause, resume, reset.
  - **Demo:** Open a demo playground, press Start, watch the motor and conveyor move, pause, resume and reset. The notice stays visible.
  - _Requirements: 2, 7_
