# Design: Guided Assembly

## Overview
Script schema in `packages/schema`. Pure script validation and the `GuidedRunner` state machine in `packages/domain/guided`. UI in `apps/web`.

## Schema

```ts
GuidedScript { version, steps: GuidedStep[] }
GuidedStep {
  number: number;                     // unique, author-visible
  id: string;
  title: string; explanation: string; // why the part goes here
  place: { definitionId: string; instanceAlias: string };
  target?: { instanceAlias: string; portId: string };   // highlight location
  completion: Predicate;              // from assembly-engine
  hints: string[];                    // escalating
  dependsOn: string[];                // step ids
}
```

## Runner

```ts
type RunnerState = { completed: Set<StepId>; current: StepId | null; hintLevel: Record<StepId, number>; snapshots: Record<StepId, AssemblyState> };
```

- `GuidedRunner.evaluate(runnerState, assemblyState)` returns the updated runner state. It marks steps whose predicates are satisfied and whose dependencies are met, and picks the next available step by lowest number.
- Snapshots of assembly state are saved when a step completes, enabling "go back".
- The runner never changes assembly state except through engine commands for go-back (restoring a snapshot through a `replaceState` command).

## UI
- `StepPanel` (title, explanation, progress, hint button), `StepList`, `CompletionSummary`.
- Highlights: the current component uses `target` style, the location marker uses `target`, hints raise `hint` style. Uses the workspace highlight API.
- Mistake feedback: after each engine command, if the command was rejected or created a state that fails a step's "should not" rule, show the mapped message.
- Adding the component for a step: the `place` definition is offered in a tray, and the user places it (using the same interactions as Free Assembly through shared components).

## Decisions
- Predicates are outcome-based, so any valid way to reach them passes.
- Progress persistence: `localStorage` initially, with an interface so `backend-accounts-persistence` can replace it.

## Testing
- Script validation tests (duplicate numbers, cycles, missing refs).
- Runner tests: ordering, parallel steps, go-back, hint escalation, completion.
- Parity test with Free Assembly (same engine command results).
- Component tests with axe and keyboard flows.
- Playwright: complete a short sample script.
