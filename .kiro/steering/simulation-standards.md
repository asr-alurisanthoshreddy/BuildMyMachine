# Simulation Standards

## 1. Nature of the Simulation

BuildMyMachine simulation is **kinematic, deterministic and sequence-driven**. It shows how mechanisms move and in what order. It does not compute forces, stresses, thermal behaviour, fluid dynamics or electrical characteristics unless a behaviour explicitly and verifiably does so.

## 2. Honesty Rules

1. The UI shows a persistent, plain-language note on every simulation view: "Kinematic demonstration. Not a physically accurate simulation."
2. Do not use the words "accurate", "realistic physics", "validated" or "engineering-grade" for simulation or visual models unless validated and documented.
3. Values shown (speeds, timings) are labelled as illustrative unless sourced.
4. Learning content distinguishes what the simulation shows from real-world behaviour that it simplifies.
5. User-created playgrounds carry the same notice.

## 3. Execution Model

- **Fixed timestep.** The engine advances in fixed steps (default 1/60 s of simulated time). Rendering frames call `advance(realDeltaTime)`, which runs zero or more fixed steps using an accumulator. The result never depends on frame rate.
- **No wall-clock or randomness inside the engine.** Time is the step count. If randomness is ever needed, use a seeded generator held in state.
- **State is serialisable** and immutable per step (or updated through a controlled reducer). Snapshots can be taken at any step.
- **Controls:** `start`, `pause`, `resume`, `reset`, optional `speed` multiplier (changes how many steps run per real second, not the step size), and `stepOnce` for learning.

## 4. Model

A **simulation program** for a playground has:

- **Bindings:** instance id to approved behaviour id, with parameters.
- **Sequence / state machine:** named states, transitions triggered by step counts, timers, sensor events or controller signals.
- **Signals:** named values exchanged between behaviours and the controller (for example `bottleAtCapper`).

A **behaviour** is a pure function over its own state, inputs and the fixed step. Examples: rotary actuator (target speed, direction), linear conveyor (belt speed, carried items), sensor (detects item in zone, emits event), controller (runs the sequence).

Output per step is a set of transforms and signal values that the view layer applies to instances.

## 5. Sensors and Events

- Sensor detection uses simple geometric zones (box or sphere tests against item positions), not physics collision.
- Events are produced in a deterministic order within a step: behaviours update in a defined order (by id), then signals propagate, then transitions are evaluated.
- Events are recorded in an event log that tests and the UI can read.

## 6. Behaviour Registry and Mapping Rules

1. Behaviours live in the `library` behaviour registry and are authored by platform engineers only.
2. Each behaviour has a stable id, version, parameter schema, input and output signals and a test suite.
3. A Component Definition lists the behaviours it may support (`supportedBehaviours`).
4. A Component Instance may be bound to a behaviour only if the behaviour is in that list.
5. **AI-generated components never receive a behaviour automatically.** AI may recommend one in a suggestion. The creator must explicitly select the mapping in a separate approval action. The system records who approved it and when.
6. The server and the domain validator both reject bindings that are unknown, not on the allow-list, or lack an approval record (for creator-owned content).
7. Generated geometry is visual only. Without an approved binding, a component is inert in simulation, though it can still be assembled, connected and explored.
8. Behaviour parameters are validated against the behaviour's schema, with documented bounds.

## 7. Compatibility Check Before Running

Before a simulation starts, the engine validates:

- every bound instance exists and its behaviour is registered
- required connections for each behaviour are present (for example, a motor connected to what it drives)
- the program references only existing states, signals and instances

Failures produce a plain-language list of problems. The simulation does not start with a broken program.

## 8. Support Levels

Each playground declares its simulation support: `none`, `partial` (some components animate) or `full` (a complete operating sequence). The UI shows this. Not every playground needs simulation.

## 9. Testing

See `testing-standards.md`, section 4. Determinism, pause/resume/reset, sensor timing, frame-rate independence and binding rejection are all required tests.
