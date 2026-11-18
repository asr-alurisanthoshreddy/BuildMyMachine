# Tasks: Free Assembly

Rules for every task: tests first, working demoable increment, builds on previous tasks, ends with wiring, no orphaned code.

- [ ] Task 1: Assembly store and adding or removing components
  - **Objective:** Pick components from the library and place them in the workspace through engine commands.
  - **Guidance:** `AssemblyStore` wrapping the engine and history. Library panel adds an instance at a default spot. Remove action.
  - **Tests:** Store tests (dispatch, history). Component test: add and remove through the UI updates the scene model.
  - **Demo:** Add two components from the library, see them in 3D, remove one, undo the removal.
  - _Requirements: 1, 2.3_

- [ ] Task 2: Move and rotate with handles and keyboard
  - **Objective:** Position components with handles and keyboard nudges inside bounds.
  - **Guidance:** Transient draft previews, commit on release. Bounds and ground rules.
  - **Tests:** Controller tests for preview versus commit, bounds clamping, keyboard step sizes, undo and redo.
  - **Demo:** Move and rotate a part with the mouse and with the keyboard only.
  - _Requirements: 2_

- [ ] Task 3: Candidate connections, snapping and explanations
  - **Objective:** Show candidate ports while dragging, snap on release, and explain failures.
  - **Guidance:** `CandidateOverlay` with the workspace markers API. Issue code to message table. Disconnect action.
  - **Tests:** Snap within and outside range, incompatible attempt message, occupied port message, disconnect.
  - **Demo:** Drag a shaft near a compatible port, see highlights and snap. Try a wrong pair and read why it fails.
  - _Requirements: 3.1–3.4_

- [ ] Task 4: Keyboard connect, issues panel, hierarchy and details
  - **Objective:** Accessible alternatives and live feedback.
  - **Guidance:** `ConnectDialog`, `IssuesPanel` from `validateAssembly`, `HierarchyTree`, `ComponentDetails`.
  - **Tests:** Keyboard-only connect flow, panel content for known issue sets, tree reflects the state, axe checks.
  - **Demo:** Build and connect a small assembly using only the keyboard and watch the issues list shrink.
  - _Requirements: 3.5, 4, 6.1_

- [ ] Task 5: Export, import and end-to-end flow
  - **Objective:** Save to and load from a file, validated by the schema.
  - **Guidance:** `FileIO` with schema validation and clear errors. Wire into the Playground route as the Free Assembly mode.
  - **Tests:** Round-trip test, malformed and wrong-version file tests. Playwright: add, connect, undo, export, reload, import.
  - **Demo:** Build an assembly, export, reload the page, import and see the same assembly.
  - _Requirements: 5, 6.2_
