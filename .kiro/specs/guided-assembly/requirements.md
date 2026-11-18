# Requirements: Guided Assembly

**Depends on:** assembly-engine

## Introduction
A structured, step-by-step assembly experience driven by a **guided script** stored in the playground definition. It runs on the shared assembly engine and 3D workspace. It adds sequencing, highlighting, explanation, hints, validation and progress. It has no component data or validation of its own.

## Requirements

### 1. Guided script format
**User story:** As a content author, I want to define assembly steps as data.

1. A guided script SHALL be an ordered list of steps with numbers, a title, an explanation ("why"), the instance or definition to place, the target port or location, a completion predicate (from `assembly-engine`), hints, and explicit dependencies on earlier steps.
2. A script SHALL be validated: step numbers unique, dependencies exist and are acyclic, referenced definitions and ports exist, predicates well-formed.
3. The script format SHALL be part of `PlaygroundDefinition` and SHALL be the same for platform and user-created playgrounds.
4. Authors SHALL be able to express steps that can be done in any order (when dependencies allow).

### 2. Running a script
1. The user SHALL see the current step, its explanation and progress ("Step 4 of 12").
2. The workspace SHALL highlight the component needed for the current step and the target mounting or connection location.
3. WHEN the user performs actions through the engine THEN the guided runner SHALL evaluate the step predicate and, when satisfied, mark the step complete and advance.
4. The user SHALL be able to go back to a previous step. Going back SHALL restore the assembly state for that step (using engine history or step snapshots).
5. Progress SHALL be tracked per user and playground version.

### 3. Hints and explanation
1. The user SHALL be able to request a hint. Hints SHALL escalate (for example, a text nudge, then highlight, then show the target pose).
2. Each step SHALL explain the purpose of the component and why it is positioned and connected that way.
3. WHEN the user makes an incorrect move THEN the app SHALL explain what is wrong in plain language, using engine issues where applicable.

### 4. Validation
1. WHEN the final step completes THEN the app SHALL run `validateAssembly` and show completion with a summary.
2. The runner SHALL reject steps that would put the assembly in a state that the engine considers invalid.

### 5. Shared system
1. Guided Assembly SHALL use the same engine, state and validation as Free Assembly.
2. Switching a completed or in-progress guided assembly to Free Assembly SHALL preserve the assembly state.

### 6. Accessibility
1. All guidance SHALL be available as text, and keyboard operable.
2. Highlight SHALL not rely on colour alone.

## Out of Scope
Simulation, authoring UI for scripts (creator tools come in `playground-ownership-administration` and `custom-playground-creation`), server-side progress persistence (stored locally until `backend-accounts-persistence`).

## Open Questions
- Whether guided steps may allow alternative valid solutions. Default: predicates describe outcomes, so alternatives that satisfy the predicate pass.
