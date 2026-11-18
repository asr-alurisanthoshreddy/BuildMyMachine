# Requirements: 3D Workspace

**Depends on:** platform-foundation

## Introduction
The interactive 3D scene used by every playground: rendering, camera navigation, selection, highlighting and an information panel. It is rendering only. It holds no assembly or simulation rules. It consumes a scene description of positioned items so it can later render Component Instances without change.

## Requirements

### 1. Scene rendering
**User story:** As a learner, I want to see a machine in 3D.

1. The workspace SHALL render a scene from a list of renderable items (id, model reference, transform, visibility, material state).
2. WHEN items are removed or the workspace unmounts THEN GPU resources (geometries, materials, textures) SHALL be disposed.
3. The workspace SHALL render lighting, a ground reference and a background that keep parts readable.
4. IF a model fails to load THEN the workspace SHALL show a placeholder for that item and an error message, and the rest SHALL still render.

### 2. Navigation
1. The user SHALL be able to orbit, pan and zoom with mouse, touch and keyboard.
2. The workspace SHALL provide "reset view" and "frame selection" controls.
3. Camera movement SHALL be bounded so the scene cannot be lost.
4. Reduced-motion preferences SHALL disable animated camera transitions.

### 3. Selection and inspection
1. WHEN the user clicks or taps an item THEN it SHALL become selected and highlighted.
2. The user SHALL be able to select items from a list outside the canvas, with the same effect (keyboard accessible).
3. WHEN an item is selected THEN an information panel SHALL show the data supplied for it (name, description, properties).
4. Hover SHALL show a lighter highlight. Highlight SHALL NOT rely on colour alone (outline or similar).
5. The workspace SHALL support isolating an item and hiding or making transparent other items, to examine internals.

### 4. Highlight API for other modes
1. The workspace SHALL expose a way to set highlight styles for items and for locations (such as a port marker) so Guided Assembly can reuse it.

### 5. Performance and compatibility
1. The workspace SHALL keep interactive frame rates on a reference scene of 200,000 triangles on a mid-range laptop (verified manually and recorded).
2. The workspace SHALL render only when needed (on change) where practical.

## Out of Scope
Assembly rules, component definitions, simulation, saving.

## Open Questions
- Exact outline technique (post-processing vs inverted hull). Decide in design.
