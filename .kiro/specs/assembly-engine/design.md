# Design: Assembly Engine

## Overview
`packages/domain/assembly`. Pure TypeScript. Depends only on `schema`. Because `library` depends on `domain` (see `technology-standards.md`), the pure binding validator lives in `domain` and is re-exported by `library`. The engine receives definitions, port types and behaviour metadata through injected lookup functions.

## State

```ts
type AssemblyState = {
  instances: Record<InstanceId, ComponentInstance>;
  connections: Record<ConnectionId, { a: PortRef; b: PortRef }>;
  version: number;       // increments per applied command, for history and tests
};
type PortRef = { instanceId: string; portId: string };
type Result<T> = { ok: true; value: T } | { ok: false; issues: Issue[] };
```

- Definitions are looked up through an injected `DefinitionResolver` (a function), keeping the engine independent of the registry implementation.

## Transforms
- Instances store local transforms relative to the parent (or world if no parent). `worldTransform(state, id)` derives world pose.
- Snap pose is computed from the two port frames: align port B's frame so its +Z opposes port A's +Z, honour axis and rotation limits.

## Commands and History
- Commands are plain objects (`{type:'connect', a, b}`) applied by `apply(state, command, resolver)`. This lets Guided scripts, Free interactions, the API and tests share one path.
- Undo/redo is a history wrapper outside the core (`AssemblyHistory`), implemented here so both modes get it.

## Candidate Connections
`findCandidates(state, instanceId, resolver, {maxDistance})` returns compatible free port pairs sorted by distance and alignment score. Used by Free Assembly snapping and by AI-fitment validation later.

## Step Predicates
```ts
type Predicate =
  | { kind: 'exists'; definitionId: string; as: string }
  | { kind: 'connected'; a: PortSelector; b: PortSelector }
  | { kind: 'pose'; instance: string; target: Transform; tolerance: Tolerance }
  | { kind: 'all'; of: Predicate[] } | { kind: 'any'; of: Predicate[] };
```
`checkStep` returns `{ satisfied, progress, unmet[] }`.

## Error Handling
All failures are `Issue` values with stable codes (`PORT_TYPE_MISMATCH`, `PORT_OCCUPIED`, `CYCLE`, `UNKNOWN_DEFINITION`, `BINDING_INVALID`...) and readable messages.

## Decisions
- Port `capacity` (default 1) supports many-to-one connections such as fasteners.
- Float comparisons use explicit tolerances.

## Testing
- Table-driven compatibility tests. Property-style tests: apply random valid command sequences and assert invariants (no cycles, connections reference existing ports, serialise round trip).
- Immutability test (deep-freeze inputs).
- Mode-parity test.
- Coverage at least 90% per `testing-standards.md`.
