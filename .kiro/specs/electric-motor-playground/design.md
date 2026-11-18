# Design: Electric Motor Playground

## Overview
Content only, under `content/components/` (nine new definitions) and `content/playgrounds/electric-motor/`. It is seeded and published through the standard pipeline as a platform-owned playground.

## Protected Paths (no-core-change check)
```
packages/schema/**
packages/domain/**
packages/library/src/**      (content registration files excluded: none exist, content is data)
packages/view3d/**
apps/api/migrations/**
apps/api/src/**
```
`content/**`, `apps/web` route data files for the Discover list, and tests for this playground are allowed. The `check-core-unchanged` script (from `platform-foundation`) reads protected paths from a config file and compares against the merge base.

## Port Types
Port types are open identifiers declared in `content/port-types.json` (id, description, compatibility pairs), loaded by the registry. They are not a code enum (see `architecture.md`). This spec adds the types it needs (for example `housing-seat`, `bearing-seat`, `brush-contact`) through that content file only. If the content file is not yet supported by the core at implementation time, record a platform gap against `component-library`.

## Components and Assembly
- Housing is the root. Magnets seat into the housing (`housing-seat`). The rotor assembly (armature + commutator + shaft) seats in bearings (`bearing-seat`) held by the end caps. Brushes mount in the rear end cap (`brush-contact` meets commutator). Output coupling attaches to the shaft (`shaft-coupling`).
- Exploded view: a second set of instance poses in `playground.json` (`views.exploded`). The UI may offer a toggle if a generic view-switch exists. If no such generic feature exists, show the exploded view as a separate playground variant (`electric-motor-exploded`) loaded the same way. Do not add engine features.

## Simulation Program
Bindings: `rotor-armature` -> `rotary-actuator` (speed in rad/s, labelled illustrative). A small state machine: `Stopped` -> `Spinning` -> `Stopped`, driven by user controls. Commutation highlighting is shown only if the existing `rotary-actuator` and `sequence-controller` outputs can emit angle-triggered signals. If not, commutation is explained in learning content only, and a platform gap entry is recorded. Do not extend behaviours here.

## Learning Content
Beginner `draft` content for each part, step and state, using the learning-content model. Technical depth is optional.

## Verification
- `check-core-unchanged` in CI on the PR for this spec.
- A test that the content directory has no references to `bottle-capping`.
- A test that all existing test suites pass without modification (CI full run).
- Content validation tests (definitions, assets, playground, script, program).
- Replay test for the guided script. Simulation determinism test.
- Playwright: explore, complete guided assembly, run simulation, save.

## Testing Notes
If any test of this spec needs a change to a shared test helper, treat it as a platform gap.
