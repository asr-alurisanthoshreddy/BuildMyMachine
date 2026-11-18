# Design: Free Assembly

## Overview
A feature module in `apps/web` using `view3d`, `library` and `domain`. A `FreeAssemblyController` translates UI intent into engine commands and holds interaction state (current tool, drag state). Assembly state itself lives in a store that wraps the engine and history.

## Structure
- `AssemblyStore`: holds `AssemblyState` and `AssemblyHistory`. `dispatch(command)` calls `apply`. No component or validation logic.
- `useDragConnect`: converts pointer movement into `moveInstance` previews. Previews are transient (not in history) until released.
- `CandidateOverlay`: uses `findCandidates` to produce markers (using the workspace `markers` API).
- `ConnectDialog`: keyboard alternative. Choose port A, then port B from a list of compatible options.
- `IssuesPanel`, `HierarchyTree`, `ComponentDetails`.
- `FileIO`: `exportAssembly` and `importAssembly` using the shared schema.

## Decisions
- Transform handles use drei `TransformControls` with step snapping, or a custom gizmo if accessibility demands it. Keyboard nudging is provided regardless.
- Previews during dragging run the engine's pure functions on a draft copy to avoid history noise.
- Soft constraints: the app never blocks an action the engine accepts. It only warns.

## Error Handling
Engine issues are mapped to messages through a table keyed by issue code. Unknown codes show a generic message and are logged.

## Testing
- `AssemblyStore` tests with a stub resolver.
- Controller tests: drag, snap, reject, undo, redo, keyboard connect.
- Component tests for panels and keyboard flows with axe checks.
- Import tests with valid and malformed files.
- Playwright: add components, connect two, undo, export, re-import.
