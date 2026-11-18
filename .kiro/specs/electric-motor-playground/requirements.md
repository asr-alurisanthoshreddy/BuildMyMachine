# Requirements: Electric Motor Playground (second machine, platform reuse check)

**Depends on:** component-library, assembly-engine, guided-assembly (script format only), simulation-engine, backend-accounts-persistence

> This spec deliberately does NOT depend on `bottle-capping-machine`. It may be implemented after it, but it must independently show that the platform is reusable with no core code changes. It uses only the shared component library, assembly engine, guided script format, simulation engine and backend. It does not use any bottle-capping content. `guided-assembly` is listed because the motor has a guided script. The original plan omitted it, and it was already a transitive dependency through the backend spec.

## Introduction
A teaching playground of a simple brushed DC electric motor, built only from content: component definitions, assets, a guided script and a simulation program. It proves that a new machine can be added without changing core packages.

> **Draft content notice.** The component list and steps are drafts, written without supplied reference material. They need review by the product owner and a domain expert.

## Draft Component List

| # | Definition id | Role | Behaviours supported |
|---|---------------|------|---------------------|
| 1 | `motor-housing` | Outer casing holding the stator magnets | none |
| 2 | `stator-magnets` | Permanent magnets producing the field | none |
| 3 | `rotor-armature` | Rotating coil assembly | `rotary-actuator` |
| 4 | `commutator` | Reverses current in the coils | none (rotates with rotor) |
| 5 | `brush-set` | Contacts that feed current to the commutator | none |
| 6 | `end-cap-front` | Closes the housing, carries the front bearing | none |
| 7 | `end-cap-rear` | Closes the housing, carries brushes and rear bearing | none |
| 8 | `bearing-pair` | Supports the shaft | none |
| 9 | `output-shaft-coupling` | Delivers rotation | none |

Note: this is a separate, more detailed motor than the `electric-motor-basic` block used in the bottle-capping machine. They have different definition ids and share no content.

## Requirements

### 1. Playground content
1. The playground SHALL be a platform-owned `PlaygroundDefinition` loaded through the standard loader.
2. It SHALL use only existing core capabilities. No file in core packages SHALL change as part of this spec.
3. Assets SHALL follow `3d-asset-standards.md` and be labelled "simplified teaching model".
4. Internal parts SHALL be viewable using the isolation and transparency features of the workspace (`internal: true` nodes).

### 2. Explore
1. Users SHALL be able to explore an assembled motor and select each part to learn its role.
2. Users SHALL be able to see an exploded view arrangement using content-defined offsets (an alternate set of instance poses in the playground, not a new engine feature).

### 3. Guided assembly (draft: 9 steps)
Place housing, fit magnets, insert rotor with commutator, fit bearings, fit front end cap, fit rear end cap with brushes, seat the brushes on the commutator, attach the output coupling, final check.

### 4. Simulation
1. The simulation program SHALL spin the rotor using `rotary-actuator`, with a labelled speed.
2. Commutation SHALL be shown by a simple, content-defined signal (a highlighted coil pair switching state at fixed rotor angles) using existing behaviour outputs and learning text. It SHALL NOT claim electromagnetic accuracy.
3. The kinematic notice SHALL be visible. No force, torque or current values SHALL be shown as computed.

### 5. Platform reusability acceptance
1. **No-core-change criterion:** the diff for this spec SHALL NOT modify any file in `packages/schema`, `packages/domain`, `packages/library` (source, excluding content registration), `packages/view3d`, or database migrations in `apps/api`.
2. **Verification:** a CI script `check-core-unchanged` compares the changed files against the protected paths and fails on any match. The script is introduced by `platform-foundation` (Task 5) and reused here.
3. IF a core change appears necessary THEN it SHALL be recorded as a platform gap, reviewed and handled in its own spec change before this spec proceeds. This spec's acceptance requires an empty exception list, or an explicit written product-owner waiver.
4. This spec SHALL NOT import or reference any file under `content/playgrounds/bottle-capping/`.
5. All existing tests SHALL pass without modification.

### 6. Saving and learning
1. Saved assemblies, guided progress and learning panels SHALL work for this playground with no changes.
2. Basic beginner learning content for each part SHALL be provided as `draft`.

## Out of Scope
Electromagnetic simulation, motor curves, AC motors.

## Open Questions
- Review of draft parts and steps.
- Whether new port types are needed. They are added to `content/port-types.json` (content, not code), so the no-core-change check can pass.
