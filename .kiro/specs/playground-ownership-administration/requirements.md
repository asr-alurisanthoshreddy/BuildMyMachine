# Requirements: Playground Ownership and Administration

**Depends on:** backend-accounts-persistence, free-assembly (shared interactions reused in the editor), guided-assembly (script format and preview)

## Introduction
Creators manage playgrounds they own: view them, edit component positions, connection points and assembly sequences in a draft, publish new versions, control visibility and transfer or delete them. This applies to any playground, whether hand-authored or AI-assisted. Editors work on the shared `PlaygroundDefinition` using the shared engine and workspace.

## Requirements

### 1. My playgrounds
1. A signed-in user SHALL see a list of playgrounds they own with title, status, current published version, visibility and last-updated time.
2. The list SHALL separate drafts, in-review, published and archived items.

### 2. Draft editing
**User story:** As a creator, I want to adjust my playground without breaking what people already use.

1. A creator SHALL be able to start a new draft from a published version. The published version SHALL remain unchanged and available.
2. In a draft the creator SHALL be able to move and rotate component instances using the shared Free Assembly interactions.
3. In a draft the creator SHALL be able to edit port positions and properties of creator-scoped definitions (connection points), with edits validated by the library and assembly validators.
4. In a draft the creator SHALL be able to edit the guided assembly sequence: reorder steps, renumber, edit text, hints, dependencies and predicates, add and remove steps.
5. Edits SHALL be checked live: invalid connections, missing ports, cyclic dependencies and invalid bindings SHALL be shown as errors.
6. The creator SHALL be able to preview the draft in Explore, Guided, Free and Simulation modes (where supported) before publishing.
7. Drafts SHALL autosave and SHALL be recoverable after a browser crash.

### 3. Behaviour mapping
1. A creator SHALL be able to map a creator-owned component to an existing library behaviour that is on its definition's `supportedBehaviours` list, by explicit action.
2. The action SHALL record the creator and time, and SHALL be reversible in a draft.
3. The UI SHALL state that generated geometry is visual only and that mapping is optional.

### 4. Publishing and versions
1. Publishing SHALL run full validation and the moderation workflow from the backend spec.
2. Publishing SHALL create an immutable new version and keep earlier versions available.
3. The creator SHALL see the version history and be able to unpublish (archive) the playground or a specific version, and restore an archived playground to a new draft.
4. Users with saved assemblies on an older version SHALL be able to continue, and SHALL be offered an upgrade to the newer version when it exists.
5. A change note SHALL be recorded with each published version.

### 5. Visibility and permissions
1. A creator SHALL be able to set visibility: `private`, `unlisted` or `public`.
2. Only the owner (and platform admins and moderators for review actions) SHALL edit, publish, archive or delete.
3. The owner SHALL be able to transfer ownership to another user who accepts. (Confirm in review if this is in the initial release. See open questions.)

### 6. Deletion
1. The owner SHALL be able to delete a playground. Deletion SHALL remove all versions, uploads, and derived data after a confirmation step.
2. WHEN a playground used by others' saved assemblies is deleted THEN affected users SHALL be told and SHALL retain their saved assemblies' data in an exportable form, as defined in design.

### 7. Platform administration
1. Platform admins SHALL be able to manage platform-owned playgrounds with the same editors.
2. Moderators SHALL be able to review submissions, approve or reject with a reason, and unpublish content that violates policy.

## Out of Scope
AI creation flow (spec 14). Social features, comments, payments.

## Open Questions
- Is ownership transfer in the initial release? Default: yes, simple accept-based transfer. Confirm.
- Policy for saved assemblies when a playground is deleted. Proposed in design.
