# Design: Custom Playground Creation

## Overview
A wizard-style feature module in `apps/web/creator/create` over the draft editor of `playground-ownership-administration`, plus API endpoints in `apps/api` for projects, images, labels and suggestions (built on `ai-orchestration`). The final output is a normal `PlaygroundDefinition` draft.

## Flow

```mermaid
flowchart LR
  A[Start + consent] --> B[Upload images]
  B --> C[Label components]
  C --> D[Run AI analysis]
  D --> E[Review suggestions]
  E --> F[Generate 3D models]
  F --> G[Edit in 3D: ports, positions, order]
  G --> H[Map behaviours (optional)]
  H --> I[Preview all modes]
  I --> J[Submit and publish]
  E -.manual add.-> G
  D -.AI fails.-> G
```

Every stage is optional to repeat. The creator can skip AI and enter the editor at any time.

## Data
- `creation_projects(id, owner_id, playground_id, status, consent_version, expires_at)`.
- `project_images(id, project_id, upload_id, title, view_type)`.
- `component_labels(id, project_id, name, description, region jsonb, image_ids)`.
- Suggestions from `ai-orchestration`. Applied suggestions record `applied_at` and the resulting draft element ids so edits and provenance are traceable.
- Playground `provenance` lists suggestion ids applied, used by the "AI-assisted" label.

## Applying Suggestions to a Draft
`applySuggestion` maps by kind:
- `component` -> creates a creator-scoped Component Definition (without behaviours, `supportedBehaviours` empty by default), unless the creator chose a reuse match.
- `port` -> adds a port declaration to the creator-scoped definition.
- `relationship` / `fitment` -> creates connections or candidate connection hints, validated by the assembly engine.
- `assemblyStep` -> creates guided steps with numbers and dependencies.
- `asset` -> attaches an approved generated asset as `visualOnly`.
- `behaviourRecommendation` -> never applied automatically. Shown in the mapping panel as a hint only.

## UI
- `ImageGrid`, `LabelEditor` (draw regions on images, keyboard alternative with text fields), `JobProgress`, `SuggestionList` with three separate status indicators, `FitmentRankList` (drag or keyboard reorder), `AssemblyOrderBoard`, `ModelPreview`, `ReuseMatchDialog`.
- The 3D editor from spec 9 is reused for ports and positions. No separate editor.

## Reuse Matching
`findReuseCandidates(label)` queries the registry by name and category and shows platform definitions with a preview. The creator chooses reuse or new.

## Moderation and Publishing
Uses the backend moderation flow. Additional checks for AI-assisted content: provenance recorded, disclosure confirmed, all suggestions reviewed, assets validated. The playground page shows badges and the simulation notice.

## Expiry and Deletion
A scheduled job notifies, then deletes projects after the retention period (default 30 days of inactivity, confirm). Deleting a project cascades through the deletion service.

## Open Decisions
- Geometry provider: choose before Task 6. Record criteria and the decision in `ai-orchestration/design.md`, and the adapter in `technology-standards.md`.
- Moderation model.

## Testing
- Unit tests for `applySuggestion` per kind, including the guarantee that behaviour recommendations are never applied.
- API tests with mock providers for project lifecycle, authorisation and deletion.
- Component tests (axe, keyboard) for each stage.
- Playwright creator journey with mock providers: consent, upload fixtures, label, analyse, review (accept, edit, reject), generate models (mock), adjust a port, reorder steps, preview all modes, publish, edit and publish a new version, delete.
- Negative E2E: a failed AI job lets the user continue manually.
