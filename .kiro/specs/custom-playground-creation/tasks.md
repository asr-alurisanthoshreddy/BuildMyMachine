# Tasks: Custom Playground Creation

Rules for every task: tests first, working demoable increment, builds on previous tasks, ends with wiring, no orphaned code. All AI and geometry calls use mock providers in automated tests.

- [ ] Task 1: Creation project, consent and manual-start path
  - **Objective:** Start an AI-assisted or manual project with recorded consent, ending in the draft editor.
  - **Guidance:** `creation_projects` migration and endpoints. Wizard shell with stage navigation. Manual path goes straight to the editor.
  - **Tests:** Consent recorded and required for AI paths, manual path needs none, authorisation, accessibility of the wizard.
  - **Demo:** Start a project both ways and reach the editor.
  - _Requirements: 1, 10_

- [ ] Task 2: Image upload, view types and labelling
  - **Objective:** Upload several images through the secure pipeline and label components.
  - **Guidance:** `ImageGrid`, `LabelEditor` with region drawing and a keyboard alternative. Show rejection reasons. Deletion removes uploads.
  - **Tests:** Pipeline rule tests through this UI path (fake signature, size, quota, scanner stub), label CRUD, multi-image per component, deletion cascade, axe and keyboard tests.
  - **Demo:** Upload five fixture images, label three components, delete one image, try a fake file and see why it is rejected.
  - _Requirements: 2_

- [ ] Task 3: Run analysis and review suggestions
  - **Objective:** Start and cancel analysis and review the suggestions with three separate status indicators.
  - **Guidance:** `JobProgress`, `SuggestionList` with filters and sorting that never alter statuses, accept, edit and reject, explicit bulk "accept all valid".
  - **Tests:** High confidence does not mark anything reviewed, failed validation blocks acceptance, edited suggestions are re-validated, sorting changes no status, partial results, cancel.
  - **Demo:** Run mock analysis, sort by AI estimate, accept some, edit one, reject one, and see an invalid one blocked.
  - _Requirements: 3, 4.1, 4.3, 10_

- [ ] Task 4: Apply suggestions to the draft
  - **Objective:** Turn accepted suggestions into creator-scoped definitions, ports, connections and guided steps.
  - **Guidance:** `applySuggestion` per kind with provenance. Behaviour recommendations are never applied. `ReuseMatchDialog` for matches.
  - **Tests:** One test per kind, reuse choice, provenance recorded, unreviewed suggestions cannot be applied, behaviour recommendation not applied and `supportedBehaviours` empty by default.
  - **Demo:** Apply the accepted suggestions and see the components, connections and steps in the draft editor.
  - _Requirements: 4.4, 8_

- [ ] Task 5: Correct and extend in the 3D editor
  - **Objective:** Edit AI-created ports, positions, fitment ranks and assembly order, and add items manually.
  - **Guidance:** Reuse spec 9 editors. `FitmentRankList` and `AssemblyOrderBoard` with keyboard reordering. Manual add for components, ports and steps.
  - **Tests:** Reorder and renumber with cycle detection, port edits revalidate, manual additions validate, keyboard and axe tests.
  - **Demo:** Move a suggested port, reorder fitments, fix a dependency cycle, add a manual step.
  - _Requirements: 4.2, 4.5_

- [ ] Task 6: 3D model generation and approval *(blocked until the geometry provider is chosen for real use)*
  - **Objective:** Generate, preview, approve, retry or replace component models.
  - **Guidance:** Resolve the open decision first and record it. Works with the mock provider until then. `ModelPreview` in the workspace. Replacement by manual GLB through the validator. Label "AI-generated approximation".
  - **Tests:** Approval required before use, over-budget and corrupt outputs shown as failures with retry, `visualOnly` enforced, replacement validation, provenance stored.
  - **Demo:** Generate a model for a component (mock), approve it, see it labelled and inert in simulation.
  - _Requirements: 5, 6.1_

- [ ] Task 7: Behaviour mapping, preview and publish
  - **Objective:** Optional mapping, full preview and AI-assisted publishing.
  - **Guidance:** Mapping panel with AI recommendations as hints only. Preview in all supported modes. Publish checklist: reviewed suggestions, validation, moderation, disclosure. Badges and notices on the playground page. Support level `none` when unmapped.
  - **Tests:** Publish blocked by unreviewed suggestions, failed validation or missing disclosure. Badges present. Mapping rules (server rejects unapproved). Simulation notice visible for mapped playgrounds.
  - **Demo:** Map the motor component, preview the simulation, publish with moderation approval and see the badges.
  - _Requirements: 6, 7_

- [ ] Task 8: Community use, retention and end-to-end journey
  - **Objective:** Others can find and use the published playground, and projects expire and delete correctly. Full creator journey test.
  - **Guidance:** Discover listing for community playgrounds. Expiry notice and job. Final Playwright journey including a new version and deletion.
  - **Tests:** Another user explores, assembles (both modes) and simulates the playground. Expiry job and deletion cascade. Failed AI job fallback journey.
  - **Demo:** Run the full journey as a creator and then as another user.
  - _Requirements: 7.5, 9, 10_
