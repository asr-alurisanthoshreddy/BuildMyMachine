# Tasks: Playground Ownership and Administration

Rules for every task: tests first, working demoable increment, builds on previous tasks, ends with wiring, no orphaned code.

- [ ] Task 1: My Playgrounds list and draft lifecycle API
  - **Objective:** Owners list their playgrounds and create drafts from published versions.
  - **Guidance:** API endpoints for list, copy-to-draft and revision-based autosave. List UI with status groups.
  - **Tests:** Authorisation matrix, copy leaves published version unchanged, revision conflicts, UI list grouping.
  - **Demo:** Sign in, see a platform-owned and a personal playground list, create a draft from a published one.
  - _Requirements: 1, 2.1_

- [ ] Task 2: Draft editor shell with preview
  - **Objective:** Load a draft into the shared workspace with undo, autosave, crash recovery and live issues.
  - **Guidance:** Draft store with the engine, autosave with IndexedDB backup, `IssuesPanel` reused. Preview switcher using real mode components.
  - **Tests:** Autosave and recovery tests, conflict handling, preview mode switching, axe checks.
  - **Demo:** Edit, kill the tab, reopen and recover. Preview in Guided and Free modes.
  - _Requirements: 2.2, 2.5, 2.6, 2.7_

- [ ] Task 3: Port and connection point editing
  - **Objective:** Edit creator-scoped port positions and properties, with validation of affected connections.
  - **Guidance:** Port editor with gizmo and numeric input. Platform definitions are read-only.
  - **Tests:** Port edit model tests, affected-connection detection, read-only enforcement, server validation.
  - **Demo:** Move a port, see the connection become invalid and fix it.
  - _Requirements: 2.3, 2.5_

- [ ] Task 4: Assembly sequence editor
  - **Objective:** Reorder, renumber and edit guided steps, dependencies and predicates with live validation.
  - **Guidance:** Pure `ScriptEditorModel` in `domain`. UI with keyboard-accessible reordering.
  - **Tests:** Model functions (reorder, renumber, add, remove, dependency changes, cycle detection). UI tests and axe checks.
  - **Demo:** Reorder steps, introduce a cycle and see the error, fix it, run the guided preview.
  - _Requirements: 2.4, 2.5_

- [ ] Task 5: Behaviour mapping UI
  - **Objective:** Explicitly map a component to an allowed library behaviour and unmap it.
  - **Guidance:** Mapping panel with confirm step, `origin: 'creator-action'`, notice about visual-only geometry.
  - **Tests:** Allowed list matches `supportedBehaviours`, server rejects others, approval record stored, unmap, parameter validation.
  - **Demo:** Map a creator motor to rotary-actuator and see it rotate in the simulation preview.
  - _Requirements: 3_

- [ ] Task 6: Publish, versions, history and upgrade offers
  - **Objective:** Submit for moderation, publish version N+1 with a change note, show history and offer upgrades to saved assemblies.
  - **Guidance:** Reuse the backend pipeline. Archive and restore. Upgrade prompt for saves on older versions.
  - **Tests:** Immutable published versions, version numbering, change notes, archive and restore, upgrade offer logic.
  - **Demo:** Publish v2 of a playground, see v1 still readable, open a saved assembly on v1 and accept the upgrade offer.
  - _Requirements: 4_

- [ ] Task 7: Visibility, transfer, deletion and moderation queue
  - **Objective:** Visibility settings, ownership transfer, deletion with confirmation, and moderator and admin tooling.
  - **Guidance:** Orphaned saved assembly policy as in `design.md` (confirm with the product owner first). Moderation queue with reasons.
  - **Tests:** Permission matrix, transfer accept flow, deletion cascade and orphan marking, moderation actions.
  - **Demo:** Change visibility, transfer to a second user, delete a playground and export an orphaned save, review in the moderation queue.
  - _Requirements: 5, 6, 7_
