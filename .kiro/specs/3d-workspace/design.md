# Design: 3D Workspace

## Overview
`packages/view3d` provides a `<Workspace>` React component built on react-three-fiber and drei. It receives a `SceneModel` and callbacks. It does not know about assemblies. Later specs map Component Instances to `SceneItem`s.

## Interfaces

```ts
type SceneItem = {
  id: string;
  modelUrl: string;            // resolved GLB
  transform: { position: Vec3; rotation: Quat; scale?: Vec3 };
  visible: boolean;
  appearance?: 'normal' | 'ghost' | 'dimmed';
  meta: { name: string; description?: string; properties?: Record<string, string> };
};
type Marker = { id: string; position: Vec3; normal?: Vec3; label?: string; style: HighlightStyle };
type HighlightStyle = 'selected' | 'hover' | 'target' | 'hint' | 'error';
type WorkspaceProps = {
  items: SceneItem[]; markers?: Marker[];
  selectedId?: string; highlights?: Record<string, HighlightStyle>;
  onSelect(id?: string): void; onHover?(id?: string): void;
};
```

- Selection state is controlled by the parent. The workspace is a presentational component with camera state kept internally.
- `ItemList` (in `ui`) renders the same items for keyboard and screen-reader selection.
- `InfoPanel` renders `meta` for the selected item.

## Decisions
- Camera: drei `OrbitControls` with min/max distance and polar limits. Keyboard handlers add orbit and zoom commands.
- Highlight: outline via post-processing selection outline (drei/postprocessing) with a fallback of emissive tint plus an outline mesh. Choose in implementation and record here. Patterns (outline thickness/style) distinguish states in addition to colour.
- Loading: `useGLTF` with an error boundary per item. A `Placeholder` box renders on failure.
- Disposal: an item cache keyed by model URL with reference counts, disposing on zero.
- Rendering on demand: `frameloop="demand"` with invalidate on change, switching to `always` when animations run.
- Isolation: `appearance` values drive material opacity without mutating shared materials (clone per item).

## Error Handling
Per-item error boundary. Global WebGL context-loss handler that shows a message and attempts restore.

## Testing
- Pure logic (camera bounds, highlight state resolution, ref-count cache) as unit tests.
- Component tests with a mocked canvas: selection from list and from a simulated pick event, panel content, keyboard controls.
- A Playwright test loads a sample scene of primitive boxes (test fixture GLB) and checks selection through the list and canvas.
- Manual performance record for the 200k triangle reference scene, stored in this folder.

## Open Decisions
Outline technique, recorded after a short spike in Task 3.
