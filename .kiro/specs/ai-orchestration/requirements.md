# Requirements: AI Orchestration

**Depends on:** component-library, backend-accounts-persistence

## Introduction
The server-side service that coordinates Claude Opus (reasoning) and a `GeometryProvider` (3D generation) to produce **suggestions** for playground creators. It follows `ai-standards.md`. It keeps reasoning and geometry generation separate. The real geometry provider is an **open decision**. This spec delivers the interface and a mock provider only.

## Requirements

### 1. Orchestrator and jobs
1. The API SHALL provide asynchronous AI jobs with status `queued | running | succeeded | partial | failed | cancelled`, creation with an idempotency key, polling, and cancellation.
2. Jobs SHALL be owned by a user and SHALL be accessible only to the owner.
3. The orchestrator SHALL be the only code that calls AI providers.
4. A job SHALL never delete or alter user data other than its own suggestions and generated assets.

### 2. Reasoning (Claude Opus)
1. The orchestrator SHALL call Claude Opus through the Anthropic API from the server only. The model id and API key SHALL come from configuration.
2. The reasoning tasks SHALL be: analyse images and labels to identify components, identify relationships between components, suggest connection points, suggest ranked fitments, and suggest numbered assembly order with dependencies.
3. Each task SHALL use a versioned prompt file and a versioned zod output schema. Prompts SHALL treat image content and labels as untrusted data.
4. Invalid output SHALL be retried once with the validation errors, then marked failed.

### 3. Suggestions and the three signals
1. Every output SHALL be stored as a `Suggestion` with `aiConfidence` (AI-reported, advisory), `validationStatus` (system-set) and `reviewStatus` (human-set), as in `ai-standards.md` section 2.
2. `aiConfidence` SHALL NOT influence `validationStatus` or `reviewStatus`.
3. `reviewStatus` SHALL be set only through an authenticated user action by the playground owner. The orchestrator SHALL have no code path that sets it.
4. A suggestion SHALL NOT be applicable to a draft unless `validationStatus` is valid and `reviewStatus` is accepted or edited.
5. WHEN a creator edits a suggestion THEN the edited payload SHALL be re-validated before it can be applied.
6. Publishing SHALL re-validate and SHALL refuse unreviewed suggestions.

### 4. Validation
1. Schema validation SHALL run on every output.
2. Domain validation SHALL check that referenced components and ports exist, port compatibility holds, units and positions are within bounds, the dependency graph is acyclic, step numbers are unique, and behaviour recommendations reference registered behaviours on the definition's `supportedBehaviours`.
3. Failures SHALL be stored with reasons and shown to the creator.

### 5. Behaviour safety
1. The AI MAY include behaviour recommendations as advisory suggestions.
2. The orchestrator and API SHALL NOT create behaviour bindings from AI output. A binding SHALL require the explicit creator action defined in `playground-ownership-administration` and SHALL be validated server-side.
3. Generated assets SHALL be stored with `visualOnly: true` and SHALL carry no behaviour data.
4. Automated tests SHALL attempt to create a binding from an AI suggestion alone and SHALL assert rejection.

### 6. Geometry generation interface
1. A `GeometryProvider` interface SHALL accept prepared image references and a prompt-free, structured request (component id, label, view images, target budget) and SHALL return a GLB (or an error) with provenance data.
2. A **mock provider** SHALL return deterministic fixture GLBs, and failure modes on request (timeout, error, invalid file, over-budget).
3. Returned GLBs SHALL pass the library asset validator, be decimated or rejected to meet budgets, and be stored as `visualOnly` assets with provenance.
4. Application code SHALL NOT import any vendor SDK outside a provider adapter.
5. The real provider choice SHALL be recorded as an open decision to be resolved before `custom-playground-creation` implementation.

### 7. Error handling and limits
1. Network errors, 5xx and timeouts SHALL be retried with exponential backoff and jitter (default max 3). 429 SHALL honour `Retry-After`.
2. Content refusals SHALL fail without automatic retry of the same input.
3. Partial results SHALL be kept and labelled.
4. There SHALL be per-user rate limits and concurrency limits, per-job limits on images, sizes, tokens and retries, and a per-user and global spend budget with an alert threshold and hard stop.
5. Over-limit responses SHALL be clear and SHALL not fail silently.
6. Cost, token counts and latency SHALL be recorded per job without images or personal data.

### 8. Privacy
1. Before the first AI use, the user SHALL give recorded consent that images and labels are sent to AI services.
2. Only images and labels needed for the job SHALL be sent, with EXIF stripped.
3. Raw images, full prompts and full outputs SHALL NOT be logged at info level.
4. Deleting a playground or account SHALL delete the user's jobs, suggestions, generated assets and stored inputs.

### 9. Manual fallback
1. Authoring features that use suggestions SHALL work with AI disabled. A failed or unavailable AI job SHALL not lose user work.

## Out of Scope
The creator UI (spec 14). Choosing and integrating a real geometry provider.

## Open Questions
- Geometry provider: hosted or self-hosted (open decision, before spec 14).
- Default numeric limits (documented in design, confirm with the product owner).
- Claude Opus provider data-retention settings to confirm before enabling in production.
