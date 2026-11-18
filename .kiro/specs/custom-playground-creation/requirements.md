# Requirements: Custom Playground Creation

**Depends on:** free-assembly, guided-assembly, playground-ownership-administration, ai-orchestration

> **Open decision (blocking):** the real `GeometryProvider` must be chosen (hosted or self-hosted) before the geometry-generation task of this spec is implemented. All other tasks work with the mock provider.

## Introduction
The creator journey: a user uploads images of a machine and its parts, labels components, and receives AI-assisted suggestions (components, relationships, connection points, ranked fitments, numbered assembly order and dependencies, and 3D models). The creator reviews, corrects and approves everything, then previews and publishes a playground. The result is a standard `PlaygroundDefinition` on the shared platform, using the same component ecosystem, workspace, assembly engine, validation and simulation. Governed by `ai-standards.md`.

## Requirements

### 1. Start and consent
1. A signed-in user SHALL be able to start a new playground project as an AI-assisted draft or a manual draft.
2. Before the first upload to AI, the user SHALL be told that images and labels are sent to AI services and SHALL give recorded consent.
3. The user SHALL be able to continue with no AI (manual authoring) at any time.

### 2. Image upload and labelling
1. The user SHALL be able to upload multiple images of the machine and of individual components, through the secure upload pipeline (signature validation, size limits, quotas, scanning, EXIF stripping, private storage).
2. The user SHALL be able to give each image a title and a view type (overview, component close-up, exploded, other).
3. The user SHALL be able to label components on images (name, short description, optional region marking) and to assign several images to one component.
4. Uploads and labels SHALL be editable and removable. Removed uploads SHALL be deleted.
5. Rejected uploads SHALL show a clear reason.

### 3. AI analysis and suggestions
1. The user SHALL be able to start an analysis job. Progress and status SHALL be visible. The user SHALL be able to cancel.
2. The system SHALL present suggestions for: identified components, relationships between components, suggested connection points (ports) on each component, ranked fitment options (which component fits where, ordered best first, with a plain-language rationale), and numbered assembly priorities with explicit dependencies.
3. Each suggestion SHALL show the AI estimate (`aiConfidence`, labelled as such), the system validation status with reasons, and its review status. These SHALL be shown separately.
4. Suggestions that fail validation SHALL be displayed with reasons and SHALL not be applicable until fixed.
5. The user SHALL be able to filter and sort suggestions (for example by rank or by AI estimate) without that sorting changing any status.

### 4. Review, correction and approval
1. The creator SHALL be able to accept, edit, or reject every suggestion individually, and "accept all valid" as an explicit recorded bulk action.
2. The creator SHALL be able to modify any AI-generated item: rename or merge components, change relationships, move or retype connection points in the 3D workspace, reorder ranked fitments, and renumber assembly steps and change dependencies.
3. Edited suggestions SHALL be re-validated before being applied.
4. Only accepted or edited, validated suggestions SHALL be applied to the draft. Nothing SHALL be applied automatically.
5. The creator SHALL be able to add components, ports and steps manually that the AI did not suggest.

### 5. 3D model generation
1. The creator SHALL be able to request a 3D model for a labelled component using its images, through the `GeometryProvider` interface.
2. Generated models SHALL be validated, shown in the 3D workspace as previews, and require creator approval before being used.
3. Approved generated models SHALL be stored as `visualOnly` assets with provenance. They SHALL be shown with a label "AI-generated approximation".
4. The creator SHALL be able to retry generation, replace a model with a manually uploaded GLB (validated), or use a simple placeholder shape.

### 6. Behaviour mapping
1. Generated components SHALL be inert in simulation by default.
2. The creator MAY map a component to an allowed library behaviour using the explicit mapping action from `playground-ownership-administration`. AI recommendations are shown as hints only.
3. A playground with no mappings SHALL declare simulation support `none`.

### 7. Preview and publish
1. The creator SHALL be able to preview the playground in Explore, Guided Assembly, Free Assembly and (if mapped) Simulation modes using the shared mode components.
2. Publishing SHALL require: all applied suggestions are accepted or edited and valid, the assembly validates, the guided script validates, moderation passes, and the creator confirms an AI-assisted disclosure.
3. The published playground SHALL be marked "AI-assisted, creator-reviewed" and "community playground" and SHALL show simulation honesty notices.
4. Publishing, versioning, ownership and administration SHALL use the shared backend and the features of `playground-ownership-administration`.
5. Users SHALL be able to find, explore, assemble (Guided and Free) and (where supported) simulate published community playgrounds exactly like platform playgrounds.

### 8. Reuse of platform
1. User-created components SHALL be registered as creator-scoped definitions and assets in the same registry, so they can be used in Free Assembly and in other playgrounds by the same creator.
2. Where a labelled component matches an existing platform definition, the system SHALL suggest reusing it instead of generating a duplicate, and the creator SHALL decide.

### 9. Privacy, retention and deletion
1. Uploaded images and derived data SHALL be deletable by the creator at any time. Deleting a project SHALL delete all related uploads, suggestions, jobs and generated assets.
2. Unfinished projects SHALL expire after a documented period with advance notice.

### 10. Errors and fallback
1. WHEN an AI job fails or partially fails THEN the user SHALL see what succeeded and what failed, and SHALL be able to retry or continue manually. No user work SHALL be lost.

## Out of Scope
Importing arbitrary externally created 3D models beyond replacing a single component's GLB through validation (full import remains a future feature). Real-time collaboration. Automatic behaviour creation.

## Open Questions
- Geometry provider (blocking, see above).
- Moderation approach for user content (human-only or human plus automated checks).
- Whether to allow remixing others' public playgrounds (not in scope without approval).
