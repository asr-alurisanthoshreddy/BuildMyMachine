# Tasks: 3D Workspace

Rules for every task: tests first, working demoable increment, builds on previous tasks, ends with wiring, no orphaned code.

- [ ] Task 1: Scene rendering from a `SceneModel`
  - **Objective:** Render a list of GLB items with lighting and a ground reference inside the app's Playground route.
  - **Guidance:** Define `SceneItem` types. Build `<Workspace>` with `useGLTF`, per-item error boundary and placeholder. Add a fixture GLB set (boxes).
  - **Tests:** Component tests for rendering N items, placeholder on load failure, and disposal on unmount (spy on `dispose`). Unit tests for the ref-counted cache.
  - **Demo:** The Playground route shows a fixture scene. Breaking one model URL shows a placeholder while the rest render.
  - _Requirements: 1_

- [ ] Task 2: Camera navigation and view controls
  - **Objective:** Orbit, pan, zoom, reset and frame-selection with limits, via mouse, touch and keyboard.
  - **Guidance:** Orbit controls with bounds. Keyboard handlers. Respect `prefers-reduced-motion`.
  - **Tests:** Unit tests for bounds clamping and framing maths. Component tests for keyboard commands and reduced-motion behaviour.
  - **Demo:** Navigate the scene with the mouse and with the keyboard only. Reset returns to the default view.
  - _Requirements: 2_

- [ ] Task 3: Selection, highlight and information panel
  - **Objective:** Select by pick or by list, highlight with a non-colour-only style, and show item information.
  - **Guidance:** Spike the outline technique and record the choice in `design.md`. Build `ItemList` and `InfoPanel`. Controlled selection state.
  - **Tests:** Selection state resolution tests. Component tests for list selection, panel content and hover. Axe check on the list and panel.
  - **Demo:** Click a part, see the outline and its description. Select the same part from the list with the keyboard.
  - _Requirements: 3_

- [ ] Task 4: Isolation, transparency and highlight API
  - **Objective:** Isolate or ghost items and expose highlight styles and location markers for other modes.
  - **Guidance:** `appearance` handling with per-item cloned materials. Implement `highlights` and `markers` props with the five styles.
  - **Tests:** Unit tests for appearance-to-material mapping without mutating shared materials. Component tests for markers and each highlight style.
  - **Demo:** Isolate one part. Show a target marker and a hint highlight on two others.
  - _Requirements: 3.5, 4_

- [ ] Task 5: Performance, context loss and end-to-end check
  - **Objective:** Meet the performance target and handle WebGL context loss.
  - **Guidance:** Demand-based rendering. Context-loss handler. Build the 200k-triangle reference scene and record measurements.
  - **Tests:** Unit test for the invalidate logic. Component test for the context-loss message. Playwright test: load the sample scene, select through list and canvas.
  - **Demo:** Run the reference scene and show the recorded frame rate note. Trigger context loss in dev tools and see recovery or the message.
  - _Requirements: 1.4, 5_
