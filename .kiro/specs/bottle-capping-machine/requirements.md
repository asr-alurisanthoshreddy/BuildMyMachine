# Requirements: Bottle-Capping Machine (first complete machine experience)

**Depends on:** free-assembly, guided-assembly, simulation-engine, backend-accounts-persistence

## Introduction
The first complete machine playground: an automatic bottle-capping machine. It is **content** on the shared platform. It is a platform-owned `PlaygroundDefinition` with components, guided script and simulation program. It SHALL need no special code paths.

> **Draft content notice.** The component list, assembly steps and operating sequence below were drafted without reference materials (none were supplied). They are drafts for review by the product owner and a domain expert. Revise if resources are provided.

## Draft Component List (9 definitions, 10 instances)

| # | Definition id | Role | Behaviours supported |
|---|---------------|------|---------------------|
| 1 | `machine-frame` | Structural base and mounting surface | none |
| 2 | `conveyor-belt-module` | Carries bottles through the stations | `linear-conveyor` |
| 3 | `electric-motor-basic` (2 instances) | Drives the conveyor, and spins the capping spindle | `rotary-actuator` |
| 4 | `bottle-guide-rails` | Aligns bottles at the infeed | none |
| 5 | `cap-feeder-chute` | Presents one cap at a time to the capping head | none |
| 6 | `capping-head` | Holds the cap and screws it onto the bottle | `rotary-actuator` (spindle, driven by motor) |
| 7 | `head-lift-slide` | Lowers and raises the capping head | `linear-actuator` |
| 8 | `proximity-sensor` | Detects a bottle at the capping station | `proximity-sensor` |
| 9 | `controller-box` | Runs the operating sequence | `sequence-controller` |

`electric-motor-basic` is seed content from `component-library`. It is reused here, not redefined.

## Requirements

### 1. Playground content
1. The playground SHALL be a platform-owned `PlaygroundDefinition` loaded through the standard loader.
2. All components SHALL come from the library (definitions, assets, behaviours). Machine-specific definitions SHALL be reusable (no bottle-specific logic in a definition).
3. Assets SHALL follow `3d-asset-standards.md`, with licences and accuracy notes. The machine is stylised and SHALL be labelled "simplified teaching model".
4. The playground SHALL declare simulation support `full`.

### 2. Explore
1. Users SHALL be able to explore the full machine, select any component and read its purpose, how it works and how it relates to others.
2. Users SHALL be able to see internal parts where modelled (for example the capping head chuck) by using isolation or transparency.

### 3. Guided assembly (draft: 12 steps)
1. Steps (draft): (1) place frame, (2) mount conveyor module, (3) mount conveyor motor and couple it, (4) fit bottle guide rails, (5) mount lift slide, (6) mount capping head on slide, (7) mount capping motor and couple spindle, (8) mount cap feeder chute, (9) mount proximity sensor at the capping station, (10) mount controller box, (11) connect motors to controller, (12) connect sensor to controller.
2. Each step SHALL have an explanation of why the part goes there, and hints.
3. The final assembly SHALL pass `validateAssembly`.

### 4. Free assembly
1. The machine's components SHALL be usable in Free Assembly together with other library components.
2. Compatible connections beyond the guided sequence SHALL be allowed by the engine rules.

### 5. Simulation
1. The simulation program (draft) SHALL: run the conveyor, detect a bottle at the capping station with the sensor, stop the conveyor, lower the head, spin the spindle for a set duration, raise the head, restart the conveyor.
2. It SHALL use only approved platform behaviours, bound by platform-content bindings.
3. The kinematic notice SHALL be visible. Timings SHALL be labelled illustrative.
4. Simulation SHALL run only on a valid, complete assembly (preflight), and SHALL explain what is missing otherwise.

### 6. Learning content
1. The playground SHALL carry per-component and per-step learning text at beginner depth. Technical depth is added by `learning-content`.

### 7. Platform fit
1. Adding this machine SHALL NOT change core packages beyond new behaviour parameters strictly required (any change SHALL be reviewed and documented as a gap in the platform).
2. Saved assemblies and guided progress SHALL work through the backend.

## Out of Scope
Physical accuracy, bottle liquid dynamics, torque or force modelling, multiple bottle sizes.

## Open Questions
- Review of the draft component list, steps and sequence by the product owner or a domain expert.
- Whether the guide rails and feeder chute need their own simulation behaviours. Default: no (inert).
