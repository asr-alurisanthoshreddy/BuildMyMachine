# Design: Component Library

## Overview
Schemas live in `packages/schema`. Registry, loader and validators live in `packages/library`. Pure validation logic is written so it can run in the browser and on the server (`apps/api`).

## Schemas (`packages/schema`)

```ts
ComponentDefinition { id, name, category, summary, function, properties: Property[],
  ports: PortDeclaration[], supportedBehaviours: BehaviourId[], scope: {kind:'platform'|'creator', ownerId?},
  learning?: { beginner: string; technical?: string } }

PortDeclaration { id, type, role, accepts: string[], localTransform, snapTolerance, axis?, rotationLimits?, label }

ComponentAsset { id, definitionId, version, uri, source: 'hand-authored'|'ai-generated'|'licensed',
  visualOnly: boolean, license, accuracyNote, stats: { triangles, bytes } }

ComponentBehaviourMeta { id, version, name, description, parameters: JsonSchemaLike,
  inputs: SignalDecl[], outputs: SignalDecl[] }

BehaviourBinding { behaviourId, parameters, approvedBy: string, approvedAt: string,
  origin: 'platform-content' | 'creator-action' }

ComponentInstance { id, definitionId, assetId, transform, parentId?, binding?: BehaviourBinding }
```

`origin` can never be `'ai-suggestion'`. The type does not allow it, and the validator rejects any payload that tries.

## Modules
- `registry`: in-memory registry with scopes (platform and creator). `register`, `get`, `list`, `search`, `resolveInstance`.
- `gltf`: `inspectGlb(bytes)` returns structure, names, extras, port frames, stats (using glTF parsing without rendering).
- `assetValidator`: runs all checks from `3d-asset-standards.md` section 9, returning typed issues with codes and messages.
- `portNormaliser`: converts `port_*` empties into `PortDeclaration` candidates and diffs them with a definition.
- `behaviourRegistry`: metadata registry. Implementations registered by `simulation-engine`.
- `bindingValidator`: `validateBinding(instance, definition, behaviourLookup)` implementing requirement 4. The pure logic lives in `packages/domain` (so `assembly-engine` can use it without a `domain -> library` import, per the dependency rules) and `library` re-exports it with the registry wired in.
- `portTypeRegistry`: loads `content/port-types.json`.
- `content loader`: reads `content/` definitions and assets into the registry.

## Decisions
- The definition is the source of truth for ports. The GLB is checked against it.
- Port **types** are open identifiers declared in `content/port-types.json` and loaded into a port-type registry (see `architecture.md` section 3). They are not a code enum, so new machines add types as content. Port **roles** (for example `male`, `female`, `neutral`) are a small fixed enum in `schema`. Definitions referencing an unregistered port type fail validation.
- Behaviours have no code in this package. Metadata only.
- GLB inspection uses a maintained glTF parsing library chosen at implementation (check licence and size).

## Error Handling
Validation returns `Result<T, Issue[]>` with `code`, `path`, `message`. No exceptions for expected failures.

## Testing
- Schema tests (valid and invalid).
- Asset validator: fixture GLBs for each failure (wrong units, bad names, duplicate names, missing licence, over budget, port mismatch, `ai-generated` with `visualOnly: false`, embedded behaviour data).
- Binding validator: unknown behaviour, not on allow-list, bad parameters, missing approval, forbidden origin.
- Registry: duplicates, scopes, search, resolve.
- Coverage targets from `testing-standards.md`.
