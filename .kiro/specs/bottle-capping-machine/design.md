# Design: Bottle-Capping Machine

## Overview
Content only, under `content/playgrounds/bottle-capping/` and `content/components/`. Seeded into the backend by a seed command that publishes it as version 1 through the normal publish pipeline, owned by the platform account.

## Content Layout
```
content/
  components/
    machine-frame/{definition.json, asset.glb, README.md}
    conveyor-belt-module/...
    bottle-guide-rails/...
    cap-feeder-chute/...
    capping-head/...
    head-lift-slide/...
    proximity-sensor/...
    controller-box/...
  playgrounds/bottle-capping/
    playground.json        PlaygroundDefinition (instances, connections)
    guided-script.json
    simulation-program.json
    learning.json
```
`electric-motor-basic` already exists from `component-library` seed content.

## Assembly Layout (draft)
- Frame is the root. Conveyor module mounts to the frame top via `bolt-mount` ports. Lift slide mounts to a frame column. Capping head mounts on the slide carriage. Feeder chute mounts to the frame above the capping station. Sensor mounts on a frame bracket by the capping station. Motors couple through `shaft-coupling` ports.
- Port types used: `bolt-mount`, `shaft-coupling`, `sensor-mount`, `signal-in` / `signal-out` (for controller connections). These port types are added to `content/port-types.json` if missing (content, not a code change).

## Simulation Program (draft)
Signals: `bottleAtCapper`, `conveyorRun`, `headDown`, `spindleRun`.
States: `Idle` -> `Feeding` (conveyor runs) -> `Detected` (sensor true, conveyor stops) -> `Lowering` (slide extends) -> `Capping` (spindle runs for N steps) -> `Raising` -> `Feeding`.
Timings are in simulated seconds and labelled illustrative. Bottles and caps are `SimItem`s spawned by the program at the infeed at fixed intervals, caps appear attached when `Capping` completes (a visual state change, not physics).

## Asset Plan
Stylised models authored in Blender, within budgets (typical 5k triangles per part). Include `port_*` empties and `extras`. Each has a licence (author-created, CC0) and an `accuracyNote`. During early development, primitive-based placeholder GLBs may stand in. They SHALL be flagged `source: hand-authored` with an `accuracyNote` saying "placeholder" and replaced before release.

## Platform Gap Policy
If content cannot be expressed with the current core, record a "platform gap" entry (what is missing, proposed generic fix) and raise it for review instead of adding bottle-specific code. Any resulting core change goes through its own task and spec update.

## Testing
- Content validation test: every definition, asset, the playground, script and program pass the library, assembly, guided and simulation validators.
- Guided script test: replay the script's commands in the engine and verify every step completes in order and final validation passes.
- Simulation test: run the program for N steps, assert the event sequence and state at fixed steps (snapshot), assert determinism.
- Negative tests: missing sensor connection fails preflight.
- Playwright: explore and select, complete guided assembly, run simulation.
- The `check-core-unchanged` script (from `platform-foundation`) is run on this spec's changes. Any exception is listed as a reviewed platform gap.
