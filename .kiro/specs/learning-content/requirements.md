# Requirements: Learning Content

**Depends on:** bottle-capping-machine

## Introduction
Learning is part of the interaction, not separate lessons. This spec defines the learning content model, the in-context explanation experience, and depth levels (beginner and technical), and writes the content for the bottle-capping machine. The model is generic so other playgrounds, including the electric motor and user-created ones, can supply content.

## Requirements

### 1. Content model
1. Learning content SHALL be part of `PlaygroundDefinition` (`learningContent`) and component definitions (`learning`). Format: structured Markdown subset with typed sections.
2. Content SHALL be attachable to: a component definition, a component instance within a playground, a guided step, a simulation state, and the playground overall.
3. Each item SHALL support two depth levels: `beginner` (default) and `technical`.
4. Content SHALL be validated: referenced ids exist, length limits, allowed Markdown subset only (no raw HTML or script).
5. Content SHALL declare its basis: `sourced` (with reference), `simplified` or `draft`. The UI SHALL show "simplified" and "draft" labels.

### 2. In-context presentation
1. Selecting a component SHALL show what it is, what it does, how it works, and how it relates to connected components, at the chosen depth.
2. Guided steps SHALL show the "why" for the step.
3. During simulation, the current state SHALL show an explanation of what is happening and which mechanism is responsible, with a link to the relevant component.
4. A "What am I looking at?" panel SHALL summarise the machine's operating principle.
5. The user SHALL be able to switch depth at any time and the choice SHALL persist for the session (and in their profile when signed in).

### 3. Understanding relationships
1. For a selected component the UI SHALL list connected components and the nature of each connection (for example "coupled by shaft to Conveyor Belt Module").
2. The workspace SHALL highlight the related components when a relationship item is chosen.

### 4. Bottle-capping content
1. The machine SHALL have beginner content for every component, every guided step and every simulation state.
2. The machine SHALL have technical content for the main mechanisms (motor and shaft coupling, conveyor drive, linear lift, sensor detection, sequence control).
3. Content SHALL clearly state simplifications (for example "real machines use torque-limited capping heads").
4. All content SHALL be marked `draft` until reviewed by a domain expert.

### 5. Accessibility and language
1. Content SHALL use plain language and define terms at first use, with a glossary.
2. Content SHALL be accessible (headings, text alternatives, no information in images only).

## Out of Scope
Quizzes, scoring, translations, AI-generated learning content (AI rationale is shown only to creators and is not published without approval).

## Open Questions
- Does the product owner want a quiz or assessment feature? Not in scope without approval.
- Domain expert review process for `draft` content.
