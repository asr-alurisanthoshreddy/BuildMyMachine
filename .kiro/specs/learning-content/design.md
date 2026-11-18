# Design: Learning Content

## Overview
A `LearningItem` schema in `packages/schema`, a content resolver in `packages/library`, and UI panels in `apps/web`. Content for the bottle-capping machine lives in `content/playgrounds/bottle-capping/learning.json` and in each component's `definition.json` `learning` field.

## Schema

```ts
LearningItem {
  id; target: { kind: 'definition'|'instance'|'step'|'simState'|'playground'; id: string };
  basis: 'sourced' | 'simplified' | 'draft'; reference?: string;
  beginner: Markdown; technical?: Markdown; glossaryTerms?: string[];
}
Glossary { term; definition }[]
```

- `Markdown` is a restricted subset (paragraphs, lists, bold, italic, links to internal ids, inline code). A sanitising renderer allows nothing else.

## Resolution
`resolveLearning(target, depth)` picks the playground-level item for an instance first, then falls back to the definition's item. The `technical` depth falls back to `beginner` if absent, and says so.

## UI
- `LearningPanel` (tabs: What it is, How it works, Connected to). Depth toggle in the header.
- `StepWhy` inside the guided step panel.
- `SimStateExplainer` listening to the simulation state.
- `RelationshipList` derived from the assembly state (connections) and the definitions' port labels.
- `GlossaryPopover` for terms.
- Depth preference: stored in session, and in user settings through the API when signed in.

## Authoring
- JSON plus Markdown strings. A lint script checks references and Markdown subset, and reports missing coverage (for example components without beginner content), used in CI for platform content.

## Decisions
- Basis labels are visible to users. Platform content goes through review before `draft` is removed.
- No third-party HTML rendering. Use a small sanitising Markdown renderer with an allow-list.

## Testing
- Schema and sanitiser tests (script injection attempts are neutralised).
- Resolver tests (fallbacks).
- Coverage lint test: bottle-capping has content for every component, step and sim state.
- Component tests with axe. Playwright: switch depth and see the content change, select a component and follow a relationship highlight.
