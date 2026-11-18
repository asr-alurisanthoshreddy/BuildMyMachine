# Engineering Principles

## 1. Reuse and Extensibility

- One component ecosystem. Before adding a component, check the library. Do not copy definitions per machine.
- One assembly engine. Guided and Free Assembly differ in rules and UX only.
- One `PlaygroundDefinition` path for platform and user content. No `if (isBuiltIn)` branches in core code.
- Add a new machine by adding content (definitions, assets, behaviours if truly new, script, program), not by editing core packages. The electric-motor playground is the standing proof of this rule.
- Extend by adding registry entries or new behaviours. Do not widen core interfaces for one machine.

## 2. Code Quality

- TypeScript strict. No `any` without a comment explaining why. Prefer `unknown` plus validation at boundaries.
- Small modules with one responsibility. Domain functions are pure where possible.
- Make invalid states hard to represent: discriminated unions, branded ids, zod schemas as the single source of types.
- Errors are explicit. Domain code returns typed results. Do not swallow exceptions.
- No dead code, no orphaned modules. Everything added is wired into a working path.
- Follow the dependency rules in `technology-standards.md`.

## 3. Scope Discipline

- Implement what the spec asks. Record extra ideas in the spec's open questions instead of building them.
- Product or scope changes need owner approval (see `product.md`, section 12).
- Prefer a complete, working initial experience over many partial features.

## 4. Engineering Honesty

- Visual models are not manufacturing-accurate unless verified. Simulation is kinematic and is labelled as such (see `simulation-standards.md`).
- Learning content states what it knows. Mark simplifications as simplifications.
- Draft content (such as the first bottle-capping and motor component lists) is flagged as draft until reviewed.

## 5. Human Approval of AI Output

- AI output is a suggestion. A human creator reviews, edits or rejects each one.
- AI-reported confidence is advisory. It never approves, publishes or skips review.
- System validation status is separate from confidence and from approval.
- AI never creates simulation behaviour or binds one. Only a creator can map a component to an existing platform behaviour, explicitly.
- The creator can always work without AI (manual fallback). A failed or unavailable AI service never blocks authoring.
- Publishing requires all applied suggestions to be creator-approved and re-validated.

## 6. Security and Privacy

- Validate all input at trust boundaries (API, uploads, AI output, loaded files).
- Authorise every action on the server. Never rely on hidden UI.
- Secrets live in the environment or a secret manager, never in code, logs, bundles or fixtures.
- Parameterised queries only.
- Upload rules (all are required and testable):
  1. file-signature (magic-byte) validation against an allow-list
  2. per-file size limits and per-user storage quotas
  3. malware scanning where appropriate, with quarantine until it passes
  4. EXIF and metadata stripping
  5. private storage, authenticated access, short-lived signed URLs
  6. retention and deletion rules, including deletion on account or playground removal
  7. moderation before publishing
- Send images to AI services only after the user has been told, and only the minimum needed. Do not log raw images or personal data.
- Rate-limit authentication, uploads and AI-triggering endpoints.

## 7. Accessibility and Usability

- All controls reachable by keyboard. Visible focus.
- Component selection and information are available outside the 3D canvas (for example a component list with details).
- Respect reduced-motion settings for non-essential animation.
- Sufficient colour contrast. Highlights do not rely on colour alone.
- Beginner-friendly language by default, with an option for more technical depth.
- Clearly show which capabilities a playground supports (explore, guided, free, simulation).

## 8. Performance

- Keep the domain core free of rendering work.
- Respect asset budgets. Dispose of GPU resources. Lazy-load models.
- Do not block the UI thread with validation of large assemblies. Measure before optimising.

## 9. Error Handling

- Errors reach users as plain-language messages with a next step.
- Failures in one component, asset or AI job do not crash the workspace.
- Log enough to diagnose (ids, error codes) and nothing sensitive.

## 10. Documentation

- Each spec's `design.md` records decisions and open questions.
- Steering documents are the standard. If code and steering disagree, fix one deliberately and note it.
