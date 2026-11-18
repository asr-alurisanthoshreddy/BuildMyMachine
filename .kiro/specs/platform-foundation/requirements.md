# Requirements: Platform Foundation

**Depends on:** none

## Introduction

Establish the monorepo, tooling, quality gates and application shell that all other specs build on. This spec delivers no machine features.

## Requirements

### 1. Monorepo and tooling
**User story:** As a developer, I want a consistent workspace so every package builds, lints and tests the same way.

Acceptance criteria:
1. The repository SHALL use pnpm workspaces with the layout in `technology-standards.md` section 2.
2. WHEN `pnpm install --frozen-lockfile` runs on a clean checkout THEN it SHALL succeed.
3. The workspace SHALL provide root scripts for `typecheck`, `lint`, `test`, `build` and `e2e`.
4. TypeScript SHALL run in strict mode in every package.
5. ESLint and Prettier SHALL be configured once at the root.

### 2. Dependency boundaries
**User story:** As an architect, I want the layering enforced so the domain stays free of UI and I/O.

Acceptance criteria:
1. WHEN a package imports from a layer it must not depend on (per `technology-standards.md` section 3) THEN the lint step SHALL fail.
2. `packages/domain` SHALL NOT be able to import React, Three.js or Node I/O modules.
3. Circular dependencies SHALL fail the check.

### 3. Application shell
**User story:** As a user, I want to open the app and navigate its main areas.

Acceptance criteria:
1. The web app SHALL render a shell with routes for Home (discover), Playground (placeholder), Sign-in (placeholder) and Not Found.
2. WHEN WebGL2 is unavailable THEN the app SHALL show a clear explanatory message instead of failing.
3. The shell SHALL be keyboard navigable with visible focus and landmarks.
4. The shell SHALL display no simulation or accuracy claims.

### 4. Continuous integration
**User story:** As a maintainer, I want every change checked automatically.

Acceptance criteria:
1. CI SHALL run install, typecheck, lint, boundary check, unit tests, build and a Playwright smoke test.
2. A failing step SHALL fail the pipeline.
3. Secrets SHALL NOT be required for CI to pass. AI and geometry providers SHALL be mocked.
4. The repository SHALL provide a `check-core-unchanged` script with a configurable list of protected paths, used by content-only specs to prove no core code changed.

### 5. Configuration and secrets
Acceptance criteria:
1. `.env.example` SHALL document every environment variable.
2. Real `.env` files SHALL be git-ignored.
3. The client bundle SHALL NOT contain any server secret. A check SHALL verify this for known variable names.

## Out of Scope
Any 3D rendering, components, backend features or content.

## Open Questions
- Hosting and deployment target (record in design).
