# Requirements: Simulation Engine

**Depends on:** component-library, assembly-engine

## Introduction
A deterministic, fixed-timestep, kinematic, sequence-driven simulation engine in the domain core, plus the behaviour implementations the platform needs first, and the playback UI. It follows `simulation-standards.md`. It binds only to approved library behaviours.

## Requirements

### 1. Deterministic engine
1. The engine SHALL advance in fixed steps of simulated time (default 1/60 s) using an accumulator, independent of render frame rate.
2. Given the same program, initial state and number of steps, the engine SHALL produce an identical state sequence.
3. The engine SHALL NOT read the wall clock or use unseeded randomness.
4. State SHALL be serialisable and snapshot-able.

### 2. Controls
1. The user SHALL be able to start, pause, resume and reset a simulation.
2. Reset SHALL return to the exact initial state.
3. Resume SHALL continue from exactly where paused.
4. The user SHALL be able to change speed (steps per second) and single-step.

### 3. Programs and sequencing
1. A simulation program SHALL define bindings, signals and a state machine (named states, transitions by step count, timers, sensor events or signals).
2. Updates within a step SHALL follow a deterministic order: behaviours by id, then signal propagation, then transition evaluation.
3. The engine SHALL record an event log.

### 4. Behaviours
1. The engine SHALL execute only behaviours registered by platform code in the behaviour registry.
2. The initial behaviour set SHALL include: rotary actuator, linear conveyor, linear actuator (extend and retract), proximity sensor (zone-based), and sequence controller.
3. Each behaviour SHALL have a parameter schema, defined inputs and outputs, and tests.
4. Behaviours SHALL be pure functions of their state, inputs and the fixed step.

### 5. Binding rules
1. The engine and assembly SHALL accept a behaviour binding only if it passes the binding validator (behaviour exists, is on the definition's `supportedBehaviours`, parameters valid, approval record present).
2. A component with no binding SHALL be inert in simulation.
3. There SHALL be no code path that creates a binding from AI output without an explicit creator approval record.

### 6. Preflight
1. Before starting, the engine SHALL check that every bound instance exists, its behaviour is registered, required connections exist and the program references existing states, signals and instances.
2. IF preflight fails THEN the simulation SHALL NOT start and the user SHALL see a plain-language list of problems.

### 7. Presentation and honesty
1. The playback view SHALL show a persistent note: "Kinematic demonstration. Not a physically accurate simulation."
2. The view SHALL apply simulation transforms to instances without advancing the simulation itself.
3. The UI SHALL show the playground's simulation support level (`none`, `partial` or `full`).
4. Shown values SHALL be labelled illustrative unless sourced.

## Out of Scope
Force, stress, thermal, fluid or electrical simulation. Authoring UI for programs. Bottle-capping content (see `bottle-capping-machine`).

## Open Questions
- Fixed step default of 1/60 s and the maximum catch-up steps per frame. Confirm in the performance task.
