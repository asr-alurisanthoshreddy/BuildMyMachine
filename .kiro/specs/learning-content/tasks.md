# Tasks: Learning Content

Rules for every task: tests first, working demoable increment, builds on previous tasks, ends with wiring, no orphaned code.

- [ ] Task 1: Learning schema, sanitising renderer and resolver
  - **Objective:** Validated content model with safe rendering and depth fallback.
  - **Guidance:** Schema in `schema`. Allow-list Markdown renderer. `resolveLearning`.
  - **Tests:** Schema tests, injection attempts neutralised, fallback rules, glossary term linking.
  - **Demo:** Render a sample item at both depths, including a malicious sample that appears as plain text.
  - _Requirements: 1_

- [ ] Task 2: Component learning panel and relationships
  - **Objective:** Show what, how and connections for a selected component, with highlight of related parts.
  - **Guidance:** `LearningPanel`, `RelationshipList` from assembly connections. Depth toggle with persistence.
  - **Tests:** Panel content tests, relationship derivation tests, highlight call tests, axe checks. Playwright: select a component, follow a relationship.
  - **Demo:** Select the capping head, read it, switch depth, click "coupled to capping motor" and see it highlight.
  - _Requirements: 2.1, 2.5, 3_

- [ ] Task 3: Guided "why" and simulation explainer
  - **Objective:** Show learning content inside guided steps and the live simulation state.
  - **Guidance:** Integrate `StepWhy` and `SimStateExplainer`. "What am I looking at?" panel.
  - **Tests:** Rendering per step and per state, link to components, keyboard and axe checks.
  - **Demo:** During guided assembly see the reason for each step. During simulation see the state explanation change.
  - _Requirements: 2.2, 2.3, 2.4_

- [ ] Task 4: Bottle-capping learning content and coverage lint
  - **Objective:** Write the content and enforce coverage.
  - **Guidance:** Beginner content for all components, steps and states. Technical content for the listed mechanisms. Mark `draft`, state simplifications. Add the glossary. Add the lint script to CI for platform content.
  - **Tests:** Coverage lint test, reference validity, Markdown subset check on all content.
  - **Demo:** Walk through the machine at both depths. Remove one item and watch the lint fail.
  - _Requirements: 1.4, 1.5, 4, 5_
