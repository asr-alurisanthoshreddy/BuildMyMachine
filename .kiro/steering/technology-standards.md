# Technology Standards

## 1. Stack

| Area | Standard |
|------|----------|
| Language | TypeScript (strict mode) everywhere |
| Package manager | pnpm workspaces |
| Frontend build | Vite |
| UI | React |
| 3D | Three.js through react-three-fiber and drei |
| Models | glTF 2.0 / GLB (see `3d-asset-standards.md`) |
| Backend runtime | Node.js (current LTS) with TypeScript |
| Database | PostgreSQL |
| Object storage | S3-compatible (private buckets only) |
| Schema validation | zod, shared between client and server |
| Unit and component tests | Vitest, Testing Library |
| End-to-end tests | Playwright |
| Lint / format | ESLint, Prettier |
| AI reasoning | Claude Opus via the Anthropic API, server-side only (see `ai-standards.md`) |
| 3D geometry generation | Behind the `GeometryProvider` interface. Provider is an OPEN DECISION (hosted or self-hosted). A mock provider is used until it is decided. |

Exact versions are pinned in the lockfile and recorded in `platform-foundation`. Verify current versions when scaffolding. Do not guess them from memory.

## 2. Repository Layout

```
packages/
  schema/        zod schemas and types: Component Definition, Asset, Behaviour,
                 Instance, PlaygroundDefinition, API contracts, AI suggestion types
  domain/        assembly engine, validation, simulation engine. No React, no Three.js, no I/O
  library/       component registry, glTF asset loading and validation, behaviour registry
  view3d/        react-three-fiber scene, camera, selection, highlighting
  ui/            shared React components
apps/
  web/           the web application
  api/           the backend service (HTTP API, jobs, AI orchestration)
content/         platform-owned playgrounds, definitions, assets, learning content
.kiro/           steering and specs
```

## 3. Dependency Rules

- `schema` depends on nothing in the repo.
- `domain` depends only on `schema`.
- `library` depends on `schema` and `domain`.
- `view3d` depends on `schema`, `domain` and `library`. Domain logic never imports from `view3d`.
- `apps/web` depends on all frontend packages. `apps/api` depends on `schema`, `domain` and `library`.
- Frontend code never imports from `apps/api`. They share only `schema`.
- No circular dependencies. Enforce with an ESLint boundaries rule or equivalent and run it in CI.
- Add third-party dependencies deliberately: check licence, maintenance and size. Record non-obvious choices in the owning spec's `design.md`.

## 4. Backend and Storage

- The API validates every request and response against `schema` types at the boundary.
- PostgreSQL access uses parameterised queries through a single data-access layer. Migrations are versioned, forward-only and reviewed.
- Sessions or tokens: use a well-maintained auth approach with secure, httpOnly cookies or short-lived tokens. Passwords, if used, are hashed with a modern memory-hard algorithm (argon2id or equivalent). The exact approach is decided in `backend-accounts-persistence`.
- Secrets (database URL, Anthropic API key, storage credentials, provider keys) come from environment variables or a secret manager. They never appear in code, logs, client bundles or test fixtures.
- Object storage buckets are private. Access is through short-lived signed URLs issued after an authorisation check.

## 5. Upload Security (required for any feature that accepts files)

1. Validate file signatures (magic bytes) against an allow-list. Extension and MIME type alone are not accepted.
2. Enforce per-file size limits and per-user storage quotas.
3. Scan for malware where appropriate. At minimum, uploads are quarantined until scanning and validation pass.
4. Strip EXIF and other metadata from images before storage or use by AI.
5. Control access: authenticated owner only, signed URLs, no public buckets.
6. Define retention and deletion: uploads and derived data are deleted on playground removal and account deletion, and unused drafts expire after a documented period.
7. Moderate content before a playground is published.

## 6. AI Providers

- Claude Opus is called only from `apps/api`. The key never reaches the browser.
- AI calls go through an orchestrator that enforces schemas, limits and approval rules. See `ai-standards.md`.
- Geometry generation goes through `GeometryProvider`. Application code never calls a vendor SDK directly.
- Tests never call real AI or geometry services. Use mock providers.

## 7. Browser and Performance Targets

- Support current stable Chrome, Edge, Firefox and Safari. WebGL2 is required. Show a clear message if it is unavailable.
- Target interactive frame rates on mid-range laptops. Asset budgets are in `3d-asset-standards.md`.
- Lazy-load heavy assets and routes. Dispose of Three.js resources when scenes unmount.

## 8. Tooling Conventions

- CI runs: install (frozen lockfile), typecheck, lint, dependency-boundary check, unit tests, build, and Playwright smoke tests.
- Conventional, descriptive commit messages. No generated build output in git.
- `.env.example` documents every variable. Real `.env` files are git-ignored.

## 9. Open Decisions

| Decision | Status | Resolve by |
|----------|--------|-----------|
| Geometry generation provider (hosted vs self-hosted) | Open. Mock provider until decided. | Before implementing `custom-playground-creation` |
| Auth mechanism details | Open | `backend-accounts-persistence` design |
| Malware scanning tool | Open | `backend-accounts-persistence` design |
| Hosting and deployment target | Open | `platform-foundation` design |
