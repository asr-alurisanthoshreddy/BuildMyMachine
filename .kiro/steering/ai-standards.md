# AI Standards

These rules govern all AI use in BuildMyMachine. They apply to `ai-orchestration` and `custom-playground-creation`.

## 1. Roles and Providers

| Role | Provider | Notes |
|------|----------|-------|
| Reasoning: image and label analysis, component identification, relationship analysis, connection-point and fitment suggestions, assembly ordering | Claude Opus via the Anthropic API | Called from `apps/api` only. Model id is configuration, not hard-coded. |
| 3D geometry generation | A `GeometryProvider` implementation | Provider is an **open decision**. Use the mock provider until it is chosen, before `custom-playground-creation` is implemented. |

- Reasoning and geometry are separate services. Claude does not generate geometry. A geometry provider does not decide what components, ports or order are.
- The orchestrator is the only code that calls providers. Other code depends on interfaces.
- The API key comes from the environment. It never reaches the browser, logs or tests.

## 2. Suggestions

Every AI output is a **Suggestion**, never a fact in the data model.

```
Suggestion
  id, kind (component | relationship | port | fitment | assemblyStep | behaviourRecommendation | asset)
  payload (typed per kind)
  aiConfidence       0..1, reported by the AI. Advisory only.
  validationStatus   pending | schemaValid | domainValid | failed (+ reasons). Set by the system.
  reviewStatus       proposed | accepted | edited | rejected. Set only by a human creator.
  reviewedBy, reviewedAt
  source             model id, prompt version, job id, input ids (not raw images)
```

### Separation of three signals

| Signal | Set by | Meaning | Can it approve? |
|--------|--------|---------|-----------------|
| `aiConfidence` | The AI | How sure the model says it is | Never |
| `validationStatus` | The system | Passes schema and domain rules | Gate only. It is necessary, not sufficient |
| `reviewStatus` | The creator | A human accepted, edited or rejected it | The only approval |

- Confidence is shown to the creator as a hint (for example to sort or highlight items). It is labelled "AI estimate".
- A suggestion is applied to a draft only when `validationStatus` is valid and `reviewStatus` is accepted or edited.
- When a creator edits a suggestion, the edited payload is re-validated before it is applied.
- Do not auto-accept, even at confidence 1.0. Bulk "accept all" is allowed only as an explicit creator action on suggestions that passed validation, and it is recorded.

## 3. Structured Outputs

- Prompts request JSON that matches a versioned zod schema. Responses are parsed and validated. Unparseable or invalid output is not repaired silently. It is retried once with the validation errors, then marked `failed`.
- Prompts are versioned files in the repo. Changes to prompts are reviewed like code. The prompt version is stored with each suggestion.
- Treat all image content and labels as untrusted data. Text inside images or labels is never followed as instructions. System prompts say so, and output is validated regardless.
- Limit the output size and number of items per call.

## 4. Validation Layers

1. **Schema validation:** shape, types, ranges.
2. **Domain validation:** referenced components and ports exist, port types are compatible per the connection rules, positions are within sensible bounds and units, the assembly dependency graph is acyclic, step numbers are unique and consistent, and behaviour recommendations reference registered behaviours on the definition's `supportedBehaviours` list.
3. **Asset validation:** generated meshes pass `3d-asset-standards.md` (budgets, units, structure), or are decimated or rejected.
4. **Human review:** creator approval.
5. **Publish-time re-validation:** all applied suggestions and the whole playground are validated again, plus moderation.

## 5. Ranked Fitments and Assembly Order

- Fitment suggestions are ranked. Each has a rank, rationale (plain language), aiConfidence and its own validation and review status. The creator can reorder, edit, or reject.
- Assembly suggestions assign numbered priorities and explicit dependencies (step B requires step A). The system checks for cycles and gaps. The creator can renumber and edit.
- Rationale text is shown to the creator. It is not published as authoritative engineering guidance without creator approval.

## 6. Behaviour Mapping

- The AI may include a `behaviourRecommendation` suggestion that names an existing library behaviour. It is advisory.
- The AI cannot create behaviours, scripts, code or parameters that execute.
- A binding exists only after the creator takes an explicit "map to behaviour" action. The system records the creator and time. The server rejects a binding that comes from an AI suggestion without that action, that names an unknown behaviour, or that is not on the definition's `supportedBehaviours` list.
- Generated assets are `visualOnly`. They do not take part in simulation until mapped.

## 7. Error Handling

| Failure | Handling |
|---------|----------|
| Network error, 5xx, timeout | Retry with exponential backoff and jitter, up to a small fixed maximum (default 3). The job stays `running` while retrying. |
| 429 / provider rate limit | Respect `Retry-After`. Back off. After the maximum, mark the job `failed` with a retryable flag. |
| Invalid structured output | One retry with validation errors, then `failed`. |
| Content refused or unsafe | Mark `failed` with a reason. Do not retry the same input automatically. |
| Partial results | Keep valid suggestions. Mark the rest failed. Show both. |
| Provider outage | Surface a clear message and the manual authoring path. |

- Jobs are asynchronous with a status (`queued | running | succeeded | partial | failed | cancelled`), idempotency keys, and cancellation.
- Timeouts per call and per job are configured and tested.
- The creator can always continue manually. A failed AI job never loses user work.

## 8. Rate Limits and Cost Limits

- Per-user limits on AI-triggering requests (per minute and per day) and on concurrent jobs.
- Per-job limits: maximum images, image size, tokens, geometry generations and retries.
- A global and per-user spend budget with an alert threshold and a hard stop. Over budget returns a clear message, not a silent failure.
- Cost, token counts and latency are recorded per job (without images or personal data).
- Limits are configuration, with documented defaults in `ai-orchestration`.

## 9. Privacy and Data Handling

- Tell the creator, before the first upload, that images go to AI services and why. Require explicit consent.
- Send only the images and labels needed for the job. Strip EXIF before sending.
- Do not include other users' data in prompts.
- Do not log raw images, full prompts or full model outputs at info level. Log ids, sizes, status, latency and cost. If prompts are stored for debugging, store them access-controlled with a short retention period.
- Confirm provider data-use terms (retention, training) before choosing a provider, and document them in the spec. Prefer settings that disable training on customer data.
- Honour deletion: deleting a playground or account deletes uploads, derived suggestions and generated assets, and stored job inputs.

## 10. Upload Security

All AI inputs come from the upload pipeline defined in `technology-standards.md` section 5 and `engineering-principles.md` section 6: signature validation, size limits and quotas, malware scanning, EXIF stripping, private storage with signed URLs, retention and deletion, and moderation before publishing.

## 11. Moderation and Provenance

- Published AI-assisted playgrounds are marked "AI-assisted" and show that a human reviewed them.
- Generated assets record their generator, version and input ids.
- Prevent publishing content that fails moderation. The moderation approach is defined in `custom-playground-creation`.

## 12. Honesty to Users

- AI-generated geometry is shown as an approximation, not a true model of a real machine.
- Do not present AI rationale as verified engineering fact.
- The playground page for AI-assisted content states that it was created by a user with AI assistance and is not an official platform machine.

## 13. Testing

See `testing-standards.md`, section 5. All AI tests use mock providers.
