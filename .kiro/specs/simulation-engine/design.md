# Design: Simulation Engine

## Overview
Engine and behaviour implementations in `packages/domain/simulation`. Behaviour metadata is registered in the `library` behaviour registry. Playback UI is in `apps/web`, with transform application in `view3d`.

## Types

```ts
type SimState = { step: number; states: Record<InstanceId, unknown>; signals: Record<string, number|boolean>;
  machine: { current: StateId; enteredAt: number }; items: SimItem[]; events: SimEvent[] };
type Behaviour<P, S> = { id; version; paramsSchema; init(p): S;
  step(s: S, inputs: Signals, ctx: { dt: number; step: number }): { state: S; outputs: Signals; transforms?: TransformDelta[]; events?: SimEvent[] } };
type SimProgram = { bindings: Binding[]; signals: SignalDecl[]; machine: StateMachineDef };
```

## Loop
- `Simulator.advance(realDt)`: accumulate `realDt * speed`, run up to `maxStepsPerFrame` fixed steps (excess time is dropped and flagged, never silently changing results). Each step: order behaviours by id, run, propagate signals, evaluate transitions, append events.
- `Simulator` holds no timers. The UI calls `advance` from the render loop. Tests call `runSteps(n)`.
- `reset()` re-initialises from `init`. `snapshot()` and `restore()` support tests and pausing.

## Items and Sensors
- `SimItem` represents things moved by behaviours (bottles, caps) with position and attached state.
- Proximity sensors test item positions against geometric zones (box or sphere). No physics.

## Behaviours (initial)
`rotary-actuator`, `linear-conveyor`, `linear-actuator`, `proximity-sensor`, `sequence-controller`. Each in its own module with a test file and metadata exported to the registry.

## Binding Enforcement
`bindingValidator` (shared) is called when programs are loaded and when `setBinding` is applied. The simulator refuses to initialise bindings that are not valid, and doubles as the server check in the backend.

## Preflight
`preflight(program, assemblyState, registry)` returns issues with codes. Required connections are declared in each behaviour's metadata (for example, `rotary-actuator` needs a connected driven part when `requiresDriven` is true).

## Presentation
- `SimulationControls` (start, pause, resume, reset, speed, step).
- `SimulationNotice` (always visible).
- `EventLog` panel (accessible, text).
- `applyTransforms`: maps `TransformDelta`s to scene item transforms each frame.

## Error Handling
A behaviour that throws is caught per step. The simulation pauses, logs a structured event and shows a message. It never crashes the app.

## Testing
- Determinism snapshots at fixed steps. Frame-rate independence tests with different `realDt` patterns.
- Pause, resume, reset tests. Sensor timing at exact steps. Event ordering tests.
- Each behaviour: unit tests with table inputs.
- Binding rejection tests as in `testing-standards.md`.
- Preflight tests for each issue.
- Component and axe tests for controls. Playwright: run, pause, resume, reset on a demo program.
