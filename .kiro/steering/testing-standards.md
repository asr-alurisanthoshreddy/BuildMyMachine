# Testing Standards

## 1. Principles

- Test behaviour, not implementation details.
- The domain core is developed test-first: write a failing test, make it pass, refactor.
- Every task in a spec ships with its tests. A task is not done until they pass.
- Tests are deterministic. No real network, clock, randomness or AI calls. Inject them.

## 2. Layers

| Layer | Package | Tool | What to test |
|-------|---------|------|--------------|
| Schema | `schema` | Vitest | Valid and invalid payloads, versioning and migration of stored definitions |
| Domain | `domain` | Vitest | Assembly commands, connection compatibility, hierarchy, guided step checks, simulation steps. Table-driven and property-style tests where useful |
| Library | `library` | Vitest | Registry lookups, glTF validation (ports, units, naming), behaviour allow-list enforcement |
| View | `view3d` | Vitest + Testing Library | Component-level behaviour with a mocked or headless renderer. Selection and highlight state, not pixels |
| UI | `ui`, `apps/web` | Vitest + Testing Library | Rendering, keyboard operation, accessibility roles, error states |
| API | `apps/api` | Vitest with a test PostgreSQL and fake object storage | Auth, authorisation, validation, persistence, versioning, upload pipeline, job handling |
| Contract | `apps/api`, `schema` | Vitest | Request and response shapes match `schema`. Mock AI and geometry providers satisfy their interfaces |
| End to end | `apps/web` + `apps/api` | Playwright | Main user journeys (below) |

## 3. Domain Core

- Coverage target: at least 90% line and branch coverage on `domain` and `library`. Treat a drop as a failing check.
- Cover every validation rule with a passing and a failing case.
- Assembly state is immutable. Test that commands do not mutate their input.
- Guided and Free modes are tested against the same engine. Add a test that both modes produce identical engine state for the same sequence of valid commands.

## 4. Simulation Determinism

- The same program, inputs and number of steps must produce an identical state sequence. Assert with snapshots of the full state at fixed steps.
- Test pause, resume and reset: resume continues exactly where paused, and reset returns to the initial state.
- Test sensor triggers and sequence transitions at exact step counts.
- Test that stepping does not depend on frame rate: the same simulated time with different render frame sizes gives the same state.
- Test that a behaviour binding with an id not in the registry, or not in the definition's `supportedBehaviours`, is rejected.

## 5. AI and Geometry Tests

- AI and geometry providers are always mocked in automated tests. Never call Claude or a geometry vendor in CI.
- Mock outputs include: valid suggestions, schema-invalid output, domain-invalid output (missing port, cyclic dependency), low confidence, high confidence with invalid content, timeouts, rate-limit errors and partial failures.
- Required assertions:
  - High `aiConfidence` never sets `reviewStatus` or skips review.
  - `validationStatus` is computed by the system and ignores `aiConfidence`.
  - A suggestion cannot be applied to a draft until a human sets `reviewStatus` to accepted or edited and validation passed.
  - A playground cannot be published with unreviewed suggestions.
  - AI-generated components cannot gain a behaviour binding automatically. Server tests try to create a binding from an AI suggestion alone and must be rejected. A binding requires an explicit creator approval record and an id on the allow-list.
  - Retry, backoff, rate limit and cost limit behaviour (see `ai-standards.md`).
  - The manual-authoring path works when AI is disabled.

## 6. Upload Security Tests

Each rule in `engineering-principles.md` section 6 has at least one automated test:

- a file with a wrong signature but a valid extension and MIME type is rejected
- oversize files and quota overruns are rejected
- a file flagged by the scanner (use the scanner's standard test file or a stub) is quarantined and never served
- stored images have EXIF removed
- another user cannot read or sign a URL for someone else's upload
- unauthenticated access is refused and no public object URL exists
- deleting a playground or account removes uploads and derived data
- unmoderated content cannot be published

## 7. Authorisation Tests

- Only the owner can edit, publish, archive or delete a playground. Others get 403 or 404 as designed.
- Published versions are immutable. An attempt to modify one fails.
- Saved assemblies keep working after a newer version is published.

## 8. End-to-End Journeys (Playwright)

1. Discover and explore a playground, select a component, read its information.
2. Complete a guided assembly of a platform machine.
3. Build a configuration in Free Assembly and save it.
4. Run, pause, resume and reset a simulation.
5. Sign up, sign in and sign out.
6. Creator flow with mock providers: upload images, label, review AI suggestions, edit, approve, preview, publish, then edit and publish a new version.
7. Electric-motor playground works with no core code changes (also checked in CI, see the spec).

Keep end-to-end tests few and stable. Use data-testid or accessible roles. Run a smoke subset on every change and the full set before release.

## 9. Accessibility

- Include automated accessibility checks (for example axe) in component and end-to-end tests of key pages.
- Keyboard-only variants for the main journeys.

## 10. Test Data

- Fixtures are small and hand-made. Include a minimal valid GLB and invalid variants (no ports, wrong units, bad names).
- No real user images or secrets in fixtures.
