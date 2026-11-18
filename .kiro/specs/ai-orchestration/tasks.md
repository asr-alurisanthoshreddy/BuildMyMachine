# Tasks: AI Orchestration

Rules for every task: tests first, working demoable increment, builds on previous tasks, ends with wiring, no orphaned code. No real AI or geometry service is called in tests.

- [ ] Task 1: Suggestion model, review service and apply rules
  - **Objective:** Store suggestions with the three separate signals, and enforce review-before-apply.
  - **Guidance:** Migration for `suggestions`. `ReviewService` (owner-only endpoints). `apply` with re-validation. Boundary lint rule so the orchestrator cannot import the review service.
  - **Tests:** High confidence does not change review or validation. Apply fails without review or with failed validation. Edited payload is re-validated. Non-owner cannot review. Boundary lint test.
  - **Demo:** Insert a mock suggestion with confidence 0.99. See it cannot be applied until reviewed.
  - _Requirements: 3_

- [ ] Task 2: Validation pipeline
  - **Objective:** Schema, domain and asset validation producing `validationStatus` and reasons.
  - **Guidance:** Domain checks listed in the requirements, reusing assembly and library validators.
  - **Tests:** Each failing case (missing port, incompatible ports, cyclic dependency, duplicate step number, unknown behaviour recommendation, not on allow-list, out-of-bounds position). Confidence has no effect.
  - **Demo:** Run the validator over a set of fixture suggestions and print their statuses and reasons.
  - _Requirements: 4_

- [ ] Task 3: Provider interfaces, mock providers and resilience
  - **Objective:** `ReasoningProvider` and `GeometryProvider` with mocks, retry, backoff, timeouts and cancellation.
  - **Guidance:** `withRetry`, `AbortSignal` plumbing, failure-mode fixtures.
  - **Tests:** Retry counts and backoff schedule (fake timers), `Retry-After`, refusal not retried, invalid output retried once, timeout, cancellation, partial results.
  - **Demo:** Run a script showing each failure mode's job outcome.
  - _Requirements: 6.1, 6.2, 7.1–7.3_

- [ ] Task 4: Jobs, orchestrator flow and Claude adapter
  - **Objective:** End-to-end asynchronous jobs from images to stored suggestions using the mock reasoning provider, plus the Claude Opus adapter behind the interface.
  - **Guidance:** Job endpoints, worker, the five reasoning tasks, prompt files, golden-file tests. Verify the current Anthropic API usage and model id in the official docs when implementing. Adapter is selected by config, mock in tests.
  - **Tests:** Job lifecycle, idempotency, ownership, cancellation, structured output handling per task, prompt version consistency. An opt-in manual smoke script (not in CI) for the real adapter.
  - **Demo:** Create a job with fixture images, watch it complete, list the suggestions with their three statuses.
  - _Requirements: 1, 2_

- [ ] Task 5: Geometry generation path and asset storage
  - **Objective:** Run geometry through the interface and store results as visual-only assets.
  - **Guidance:** Mock provider outputs go through the library asset validator, decimation or rejection, then storage with provenance. Behaviour data forbidden.
  - **Tests:** Over-budget and corrupt files rejected, `visualOnly: true` enforced, provenance stored, no behaviour fields.
  - **Demo:** Generate a mock mesh for a labelled component and see it as a visual-only asset in the library.
  - _Requirements: 5.3, 6_

- [ ] Task 6: Behaviour safety tests
  - **Objective:** Prove that AI output can never create a binding.
  - **Guidance:** Test-driven endpoint and service checks. Add the creator-action path via the existing `setBinding` API.
  - **Tests:** Attempt a binding from a suggestion alone (rejected), from an unapproved recommendation (rejected), from a creator action on a non-allow-listed behaviour (rejected), and a valid creator action (accepted).
  - **Demo:** Show the four attempts and results.
  - _Requirements: 5_

- [ ] Task 7: Limits, budget, privacy and deletion
  - **Objective:** Rate, concurrency and spend limits, consent, redaction and deletion.
  - **Guidance:** Limits service with config defaults. Consent table. Logger redaction. Deletion hooks.
  - **Tests:** Each limit triggers a clear error, budget alert and hard stop, consent required, logs contain no image or prompt data, deleting a playground or account removes jobs, suggestions and generated assets.
  - **Demo:** Exceed the daily limit and see the message. Delete a playground and show the AI data gone.
  - _Requirements: 7.4–7.6, 8, 9_
