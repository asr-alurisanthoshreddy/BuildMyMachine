# Requirements: Free Assembly

**Depends on:** assembly-engine

## Introduction
The open-ended assembly experience. Users browse the component library, place components in the 3D workspace, move and rotate them, connect compatible ports and build custom configurations. It is a UX and rule set over the shared assembly engine. It keeps no component data or validation of its own.

## Requirements

### 1. Adding components
**User story:** As a learner, I want to pick components and place them in the workspace.

1. The user SHALL be able to browse and search the library (from `component-library`) and add a component to the workspace.
2. WHEN a component is added THEN an instance SHALL be created through the assembly engine command.
3. The user SHALL be able to remove components.

### 2. Moving and rotating
1. The user SHALL be able to move and rotate a selected instance with on-screen handles, and by keyboard with step sizes.
2. Movement SHALL be constrained to a bounded workspace area and ground plane rules.
3. Undo and redo SHALL be available.

### 3. Connecting and snapping
1. WHILE the user drags a component THEN the app SHALL show candidate connections (from `findCandidates`) with highlighted ports.
2. WHEN the user releases within snap range of a compatible port THEN the engine SHALL connect and snap the component.
3. WHEN the user attempts an incompatible connection THEN the app SHALL explain why in plain language using the engine's issue message.
4. The user SHALL be able to disconnect.
5. A manual "connect these two ports" action SHALL exist as a keyboard-accessible alternative to dragging.

### 4. Assembly feedback
1. The app SHALL show a live list of issues from `validateAssembly` (for example, unconnected required ports) as gentle guidance, not blocking errors.
2. The app SHALL show the assembly hierarchy and connections as a tree.
3. Selecting a component SHALL show its information, ports and connected neighbours.

### 5. Saving and loading
1. A user's free assembly SHALL be serialisable to the shared state format. Server persistence is provided by `backend-accounts-persistence`. Until then, local download and import of the file SHALL work.
2. Loading SHALL validate the file against the schema and report errors without crashing.

### 6. Consistency
1. Component inspection, selection and navigation SHALL behave the same as in other modes.
2. Free Assembly SHALL work with any playground's components, including user-created ones, with no mode-specific component data.

## Out of Scope
Step scripts and hints (guided-assembly). Simulation. Server saving.

## Open Questions
- Whether Free Assembly is limited to a playground's component set or can use the whole library. Default: the whole platform library plus the playground's own components. Confirm with the product owner during design review.
