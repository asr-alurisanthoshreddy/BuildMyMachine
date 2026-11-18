# Tasks: Component Library

Rules for every task: tests first, working demoable increment, builds on previous tasks, ends with wiring, no orphaned code.

- [ ] Task 1: Schemas for the four concepts
  - **Objective:** Typed, validated schemas for Definition, Asset, Behaviour metadata and Instance with binding.
  - **Guidance:** Implement in `packages/schema` with zod. Make the binding `origin` type exclude AI suggestions. Export inferred types.
  - **Tests:** Valid and invalid payload tests for each schema. A test that a binding with `origin: 'ai-suggestion'` is rejected.
  - **Demo:** A script validates sample definition and instance JSON and prints readable errors for bad input.
  - _Requirements: 1, 2.1, 3.1, 4.1, 4.2_

- [ ] Task 2: Registry and behaviour metadata registry
  - **Objective:** Register, look up, list and search definitions and assets, and register behaviour metadata.
  - **Guidance:** Scoped registries. Duplicate-id rejection. `resolveInstance`. No creation API for behaviours outside platform code.
  - **Tests:** Duplicates, scope separation, search, multi-asset definitions, resolve failures.
  - **Demo:** Register three sample definitions and search them by text and category.
  - _Requirements: 1.3, 2.2, 3.2, 3.3, 5.1, 5.2_

- [ ] Task 3: GLB inspection and asset validator
  - **Objective:** Read a GLB, extract structure, ports and metadata, and validate it against `3d-asset-standards.md`.
  - **Guidance:** `inspectGlb`, `portNormaliser`, `assetValidator` with issue codes. Create fixture GLBs for each failure.
  - **Tests:** One test per validation rule, plus the port mismatch diff and the signature check.
  - **Demo:** Run the validator on a good fixture (passes) and on each bad fixture (clear messages).
  - _Requirements: 2.3, 2.4, 2.5, 5.3_

- [ ] Task 4: Behaviour binding validation
  - **Objective:** Enforce the allow-list, parameter schema and approval record.
  - **Guidance:** `bindingValidator`. Clear error codes. This function is shared with the server in `backend-accounts-persistence`.
  - **Tests:** Unknown behaviour, not on `supportedBehaviours`, invalid parameters, missing approval, forbidden origin, valid platform and creator bindings.
  - **Demo:** Attempt each invalid binding and see the specific rejection.
  - _Requirements: 4.3, 4.4_

- [ ] Task 5: Seed content, loader and library browsing in the workspace
  - **Objective:** Load platform seed components from `content/` through the registry and show them in a library browser alongside the 3D workspace.
  - **Guidance:** Create `content/port-types.json` and the port-type registry. Create 3 to 4 simple seed definitions with primitive-based GLBs and ports (for example frame, shaft, and `electric-motor-basic`, which the bottle-capping machine reuses) as drafts. Build the content loader and a `LibraryBrowser` UI that lists, searches and shows details. Selecting a component previews it in the workspace. Confirm the asset budgets against these assets and note any change in `3d-asset-standards.md`.
  - **Tests:** Loader test over the seed folder (all pass the validator). Component test for browse, search and preview. Playwright test: open the library, search, preview.
  - **Demo:** Open the app, browse the library, preview a component in 3D and read its function and ports.
  - _Requirements: 5.3, 5.4, 6_
