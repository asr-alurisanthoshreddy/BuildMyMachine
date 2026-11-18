# Tasks: Platform Foundation

Rules for every task: write the tests first, deliver a working increment that builds on the previous task, wire it in, and leave no orphaned code.

- [ ] Task 1: Monorepo workspace and shared tooling
  - **Objective:** A clean pnpm workspace where every package typechecks, lints and tests.
  - **Guidance:** Create `pnpm-workspace.yaml`, root `package.json` scripts, `tsconfig.base.json` (strict), ESLint and Prettier configs, and empty packages with a trivial exported function each. Look up current versions first.
  - **Tests:** One Vitest test per package proving the workspace test runner executes. A script test that `pnpm typecheck` fails on a deliberate type error fixture (kept out of the build).
  - **Demo:** `pnpm install --frozen-lockfile && pnpm typecheck && pnpm lint && pnpm test` passes.
  - _Requirements: 1_

- [ ] Task 2: Enforce dependency boundaries
  - **Objective:** Layering and purity rules fail the build when broken.
  - **Guidance:** Implement the boundary table from `technology-standards.md`. Forbid UI, Three.js and I/O imports in `domain`. Add the cycle check.
  - **Tests:** Fixture files that violate each rule must fail lint (run ESLint programmatically in a test). A compliant import passes.
  - **Demo:** Add a forbidden import to `domain`, lint fails with a clear message. Remove it, lint passes.
  - _Requirements: 2_

- [ ] Task 3: Web app shell, routing and WebGL gate
  - **Objective:** A navigable, accessible shell that handles missing WebGL2.
  - **Guidance:** Build `AppShell`, routes (Home, Playground placeholder, Sign-in placeholder, Not Found), an error boundary and `WebGLGate`. Use landmarks and visible focus.
  - **Tests:** Testing Library tests for routing, the not-found page, the error boundary, and the WebGL fallback. An axe check on the shell.
  - **Demo:** Run the app. Navigate with the keyboard. Disable WebGL2 and see the message.
  - _Requirements: 3_

- [ ] Task 4: API skeleton and environment configuration
  - **Objective:** A running API with a health route and validated configuration.
  - **Guidance:** Minimal Node server in `apps/api` with `/health`. Load config from env with a zod schema that fails fast. Add `.env.example`.
  - **Tests:** Health route test. Config test for missing and invalid variables. Test that the secret scan flags a bundle containing a forbidden variable name.
  - **Demo:** Start the API and call `/health`. Start with a missing variable and see a clear error.
  - _Requirements: 5_

- [ ] Task 5: CI pipeline and Playwright smoke test
  - **Objective:** Every change runs the full quality gate.
  - **Guidance:** Add the CI workflow: install, typecheck, lint, boundaries, unit tests, build, secret scan, Playwright smoke. Use mock providers only. Record the hosting decision (or defer it) in `design.md`. Add the `check-core-unchanged` script and its config file listing protected paths (`packages/schema/**`, `packages/domain/**`, `packages/library/src/**`, `packages/view3d/**`, `apps/api/migrations/**`, `apps/api/src/**`). It compares changed files against the merge base and fails on any protected match unless an explicit waiver file is present. It is not enforced on every PR (core specs change core by design). It is run by content-only specs (bottle-capping, electric-motor).
  - **Tests:** The Playwright smoke test (home loads, keyboard navigation). Unit tests for `check-core-unchanged` with fixture diffs (protected path changed fails, content-only passes, waiver passes).
  - **Demo:** A pull request runs the pipeline green. A deliberate failure turns it red.
  - _Requirements: 4, 5_
