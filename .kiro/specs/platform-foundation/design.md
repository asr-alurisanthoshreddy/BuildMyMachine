# Design: Platform Foundation

## Overview
Create the pnpm monorepo with empty but wired packages (`schema`, `domain`, `library`, `view3d`, `ui`), `apps/web` and `apps/api` skeletons, shared configs and CI. Later specs fill the packages.

## Decisions
- **Workspace:** `pnpm-workspace.yaml` listing `packages/*` and `apps/*`. Shared `tsconfig.base.json` with `strict: true`. Each package has a project reference.
- **Boundaries:** ESLint `import/no-restricted-paths` or `eslint-plugin-boundaries` configured from a single table that mirrors `technology-standards.md`. Cycles checked with `madge` or the ESLint `import/no-cycle` rule. Choose one in implementation and record it here.
- **Domain purity:** `packages/domain` ESLint config forbids `react`, `three`, `@react-three/*`, `node:fs`, `node:net`, `fetch` and timers.
- **Web app:** Vite + React + React Router. A `WebGLGate` component checks WebGL2 and renders a fallback message.
- **API skeleton:** minimal Node HTTP server with a `/health` route, to be built on in `backend-accounts-persistence`.
- **Tests:** Vitest workspace config. Playwright config with a smoke project.
- **CI:** a single workflow file. Because the hosting platform is undecided, keep it generic (GitHub Actions if the repo is hosted on GitHub).
- **Secrets check:** a small script scans the built client bundle for forbidden variable names such as `ANTHROPIC_API_KEY`.

## Components
- `WebGLGate`, `AppShell`, `Routes`, `NotFoundPage`.

## Verify Versions
Look up current stable versions of Node LTS, pnpm, Vite, React, Vitest and Playwright when scaffolding. Pin them in the lockfile.

## Error Handling
- Root-level React error boundary with a plain-language message.

## Testing
- Unit tests for `WebGLGate` (mock `getContext`).
- Lint-rule tests: a fixture file violating a boundary must fail lint.
- Playwright smoke: home page loads, navigation works with keyboard.

## Open Decisions
Hosting and deployment target. Decide here before any deploy task.
