# Requirements: Backend, Accounts and Persistence

**Depends on:** platform-foundation, component-library, assembly-engine, guided-assembly (script schema), simulation-engine (program schema)

> Note: the original plan listed dependencies 1 and 4. The `PlaygroundDefinition` schema embeds guided scripts, simulation programs, definitions and the binding validator, so specs 3, 6 and 7 are also required. This is flagged in the consistency review.

## Introduction
The shared backend: accounts, authentication, the `PlaygroundDefinition` schema with versions, persistence of playgrounds, definitions, assets and saved user assemblies, and the secure upload and storage pipeline. Platform machines and user-created playgrounds use the same tables and code paths.

## Requirements

### 1. Accounts and authentication
**User story:** As a user, I want an account so my work is saved.

1. Users SHALL be able to register, sign in and sign out.
2. Passwords (if used) SHALL be stored only as hashes using a modern memory-hard algorithm.
3. Sessions or tokens SHALL be secure, expire, and be revocable. Cookies SHALL be httpOnly, secure and SameSite.
4. Authentication endpoints SHALL be rate limited.
5. Users SHALL be able to delete their account. Deletion SHALL remove their private data, uploads and derived data per the retention rules.
6. A platform account SHALL own platform content. There SHALL be no code path that treats platform content differently except authorisation.

### 2. PlaygroundDefinition and versions
1. The shared `PlaygroundDefinition` schema SHALL cover: identity, owner, slug, title, category, version, status (`draft`, `in_review`, `published`, `archived`), visibility, component references, instances with connections, optional guided script, optional simulation program, optional learning content, simulation support level and provenance (`human-authored` or `AI-assisted` with suggestion ids).
2. The server SHALL validate a definition against the schema, the library validators (definitions, assets, bindings), the assembly validator and script and program validators before accepting a publish.
3. A published version SHALL be immutable. Any update request SHALL fail.
4. To change a published playground the owner SHALL create a new draft copy. Publishing it SHALL create version N+1.
5. Saved assemblies SHALL reference `(playgroundId, version)` and SHALL continue to work after newer versions exist.

### 3. Persistence APIs
1. The API SHALL provide authenticated CRUD for drafts, publish, archive, list and read for published content according to visibility.
2. The API SHALL provide create, list, update, and delete for a user's saved assemblies.
3. Every endpoint SHALL validate requests and responses against `schema`.
4. Every endpoint SHALL enforce authorisation on the server.
5. Database access SHALL use parameterised queries through one data-access layer. Migrations SHALL be versioned.

### 4. Component definitions and assets
1. The API SHALL store creator-scoped definitions and assets using the same schemas as platform content.
2. Registering an asset SHALL run the library asset validator server-side.
3. The server SHALL reject any behaviour binding that lacks a valid approval record, is not on the definition's `supportedBehaviours`, or whose origin is not `platform-content` or `creator-action`.

### 5. Secure upload and storage
1. Uploads SHALL validate file signatures (magic bytes) against an allow-list (for images and GLB).
2. Uploads SHALL enforce per-file size limits and per-user storage quotas.
3. Uploads SHALL be quarantined until scanned (malware scanning where appropriate) and validated.
4. Image metadata (EXIF) SHALL be stripped before storage.
5. Storage SHALL be private. Reads SHALL be through short-lived signed URLs after an ownership or visibility check.
6. Retention and deletion SHALL be defined: deleting a playground or account SHALL delete uploads and derived data. Unreferenced drafts and uploads SHALL expire after a documented period.
7. Publishing SHALL require moderation approval.

### 6. Moderation and publishing
1. Submitting for publishing SHALL move a draft to `in_review`. A moderator action (or an automated policy defined in design, with human override) SHALL approve or reject with a reason.
2. Platform content SHALL pass through the same validation but MAY be published by a platform administrator without third-party moderation.

### 7. Client integration
1. The web app SHALL load playgrounds through the API using the same loader for platform and user content.
2. Guided progress SHALL be saved server-side for signed-in users, replacing the local-only store.
3. Free Assembly SHALL be able to save and load assemblies on the server.

## Out of Scope
Playground editing UI (spec 9), AI (spec 13), creator flow (spec 14), payment or social features.

## Open Questions
- Auth mechanism and library choice (decide in design).
- Malware scanning tool and moderation model (decide in design).
- Hosting target (inherited from `platform-foundation`).
