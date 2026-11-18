# Tasks: Backend, Accounts and Persistence

Rules for every task: tests first, working demoable increment, builds on previous tasks, ends with wiring, no orphaned code.

- [ ] Task 1: Database layer, migrations and HTTP skeleton
  - **Objective:** PostgreSQL connection, migration runner, request validation middleware and error format.
  - **Guidance:** Single data-access layer with parameterised queries. Forward-only migrations. `/v1` routing.
  - **Tests:** Migrations apply from scratch. Validation middleware rejects bad input. Error format tests.
  - **Demo:** Start the API, run migrations, call a validated route with good and bad input.
  - _Requirements: 3.3, 3.5_

- [ ] Task 2: Accounts and authentication
  - **Objective:** Register, sign in, sign out, session expiry and revocation, and the sign-in pages in the web app.
  - **Guidance:** Decide the auth approach and record it in `design.md`. argon2id hashing, secure cookies, CSRF protection, rate limits.
  - **Tests:** Full auth matrix, rate limiting, expired and revoked sessions. Playwright: sign up, sign in, sign out.
  - **Demo:** Create an account in the browser, sign out, sign back in.
  - _Requirements: 1.1–1.4_

- [ ] Task 3: PlaygroundDefinition schema, versions and persistence
  - **Objective:** Store drafts, publish immutable versions and create new drafts from published ones.
  - **Guidance:** Schema in `packages/schema` composed from existing schemas. Server validation pipeline for publish. DB trigger for immutability. Platform account seeding.
  - **Tests:** Schema tests. Publish validation failures (bad definition, bad binding, bad script, bad program). Immutability at API and DB level. New-draft-from-published and version N+1.
  - **Demo:** Create a draft with the API, publish it, try to edit it (fails), create version 2.
  - _Requirements: 1.6, 2_

- [ ] Task 4: Authorisation, listing and reading
  - **Objective:** Ownership and visibility rules for all playground endpoints, plus list and read for the app.
  - **Guidance:** Role model (`user`, `moderator`, `admin`). Visibility (`private`, `unlisted`, `public`).
  - **Tests:** Authorisation matrix for each endpoint and role. Visibility tests.
  - **Demo:** Two users: one cannot read or edit the other's private playground. A public one is readable by all.
  - _Requirements: 3.1, 3.4_

- [ ] Task 5: Creator definitions, assets and server-side binding enforcement
  - **Objective:** Store creator-scoped definitions and assets and enforce bindings server-side.
  - **Guidance:** Run the library asset validator on registration. Reject bindings by origin, allow-list and approval.
  - **Tests:** Asset validation failures. `BINDING_NOT_APPROVED` for AI origin, missing approval, unknown behaviour, not on allow-list.
  - **Demo:** Register a valid asset. Attempt an AI-origin binding and see it rejected.
  - _Requirements: 4_

- [ ] Task 6: Secure upload pipeline and storage
  - **Objective:** Upload with signature checks, quotas, quarantine, scan, EXIF strip, private storage and signed URLs.
  - **Guidance:** `scan` interface with a stub and a real implementation chosen and recorded in `design.md`. Retention job.
  - **Tests:** Every item in `testing-standards.md` section 6.
  - **Demo:** Upload a valid image (becomes clean). A fake-extension file and an oversize file are rejected. A second user cannot get a URL.
  - _Requirements: 5_

- [ ] Task 7: Moderation, deletion and retention
  - **Objective:** Submit, approve and reject flow, account and playground deletion cascades and the expiry job.
  - **Guidance:** `moderation_events`. Deletion removes storage objects and derived rows.
  - **Tests:** Unmoderated content cannot publish. Deletion removes uploads. Expiry job removes stale drafts and objects.
  - **Demo:** Submit a draft, reject it with a reason, fix, approve and publish. Delete the playground and confirm uploads are gone.
  - _Requirements: 5.6, 5.7, 6, 1.5_

- [ ] Task 8: Client integration and saved assemblies
  - **Objective:** The web app loads playgrounds through the API, saves guided progress and saves and loads Free Assembly states.
  - **Guidance:** A single `PlaygroundLoader` for platform and user content. Replace the local `ProgressStore` for signed-in users.
  - **Tests:** Loader tests. Saved assembly CRUD and version referencing. Playwright: sign in, build and save, reload, load.
  - **Demo:** Build an assembly, save, publish a newer version of the playground (via API) and confirm the saved assembly still loads.
  - _Requirements: 2.5, 3.2, 7_
