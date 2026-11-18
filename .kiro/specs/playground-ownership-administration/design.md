# Design: Playground Ownership and Administration

## Overview
Frontend feature module `apps/web/creator` plus API additions in `apps/api/playgrounds`. All editing uses the shared engine commands (`apply`), the shared workspace and the shared validators. The editor is "Free Assembly with draft semantics and extra panels", not a separate editor.

## Screens
- **My Playgrounds:** list with status groups.
- **Draft Editor:** workspace plus panels: Hierarchy, Component Details, Ports editor, Sequence editor, Bindings, Issues, Preview.
- **Version History:** versions with change notes and actions.
- **Settings:** visibility, ownership transfer, delete.
- **Moderation queue** (moderator and admin roles only).

## Editing Model
- A draft holds a full `PlaygroundDefinition` in the store. Edits are commands on a draft document model with undo and redo. Assembly edits reuse the engine. Sequence edits use a `ScriptEditorModel` with pure functions (`reorder`, `renumber`, `addStep`, `removeStep`, `setDependency`) that return a new script and validation issues, tested in `domain`.
- Ports editor: moves a port's local transform and edits type, role and accepts for creator-scoped definitions only. Changing a port used by existing connections triggers re-validation of the assembly and shows affected connections. Platform definitions are read-only.
- Autosave: debounced `PUT` of the draft with a revision number for conflict detection (optimistic concurrency). Local backup in IndexedDB for crash recovery.

## Behaviour Mapping UI
- Lists allowed behaviours for the component's definition (`supportedBehaviours`) with description and parameters.
- "Map to behaviour" requires an explicit confirm. Sends `setBinding` with `origin: 'creator-action'`. The server records approval. Unmap removes the binding in the draft.

## API Additions
`POST /v1/playgrounds/:id/versions/:v/drafts` (copy to draft), `PATCH` draft with revision, `POST .../submit`, `POST .../archive`, `POST .../restore`, `POST .../transfer` and accept endpoints, `DELETE`, version list. Moderation endpoints for roles.

## Saved Assemblies on Deletion or Archival
- Archival keeps old versions readable by users who saved against them (`visibility` of a version used by saves stays `unlisted-by-reference`).
- Deletion removes the playground. Affected saved assemblies are marked `orphaned` and the owner can export them as files. Inform the user on next visit. Proposed policy, confirm in review.

## Decisions
- Concurrency: optimistic revision numbers. A conflict shows a choice between reload and overwrite.
- Preview uses the real mode components with the draft definition in memory.

## Testing
- Pure model tests for script editing and port edits.
- API authorisation matrix tests (owner, other user, moderator, admin).
- Draft concurrency tests.
- Component tests for panels and keyboard use with axe checks.
- Playwright: edit a draft, preview, publish, version history, create a new draft, archive and restore, delete.
