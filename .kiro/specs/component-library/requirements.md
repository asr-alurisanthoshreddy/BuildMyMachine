# Requirements: Component Library

**Depends on:** 3d-workspace

## Introduction
The reusable component ecosystem. This spec defines and implements the four separate concepts (Component Definition, Component Asset, Component Behaviour, Component Instance), the registry, the glTF asset loader and validator, and the behaviour registry with its allow-list rules. Terms follow `architecture.md` section 2.

## Requirements

### 1. Component Definition
**User story:** As a platform author, I want to describe a component type once and reuse it everywhere.

1. A definition SHALL have id, name, category, function description, properties, port declarations, `supportedBehaviours` (list of behaviour ids), ownership scope (platform or creator) and learning summary.
2. Definitions SHALL be validated by a zod schema in `packages/schema`.
3. WHEN two definitions share an id in the same scope THEN registration SHALL fail.
4. Definitions SHALL NOT contain machine-specific data (that belongs to playgrounds).
5. Port types SHALL be open identifiers registered from `content/port-types.json`. A definition that references an unregistered port type SHALL fail validation. Adding a port type SHALL require no code change.

### 2. Component Asset
1. An asset SHALL reference exactly one definition and carry the GLB reference, version, source, licence, `visualOnly`, `accuracyNote` and budgets data.
2. A definition SHALL support several assets (for example, levels of detail).
3. WHEN a GLB is registered THEN the validator SHALL apply every check in `3d-asset-standards.md` section 9 and return readable errors.
4. Ports found as `port_*` nodes SHALL be compared to the definition's port declarations. A mismatch SHALL fail validation.
5. Assets SHALL NOT contain behaviour or script data. An asset with `source: ai-generated` and `visualOnly: false` SHALL fail.

### 3. Component Behaviour
1. A behaviour SHALL have a stable id, version, parameter schema, input and output signals, and a description.
2. Behaviours SHALL be registered only by platform code. There SHALL be no API to create a behaviour from user or AI input.
3. The behaviour registry SHALL expose lookup by id and listing by category. Behaviour implementations are supplied by `simulation-engine`. This spec defines the registry, interface and metadata only.

### 4. Component Instance
1. An instance SHALL have id, definition id, asset id, transform, optional parent, connections (declared by `assembly-engine`) and an optional **behaviour binding**.
2. A behaviour binding SHALL record behaviour id, parameters, `approvedBy` (platform or creator id) and `approvedAt`.
3. WHEN a binding is created THEN validation SHALL require: the behaviour exists, it is in the definition's `supportedBehaviours`, parameters satisfy the schema, and an approval record is present.
4. A binding request that originates from an AI suggestion without an explicit creator approval record SHALL be rejected.

### 5. Registry and loading
1. The registry SHALL register, look up, list and search definitions and assets by id, category and text.
2. The registry SHALL resolve an instance to its definition and asset.
3. Loading SHALL parse GLB files and extract port frames and metadata without rendering.
4. Platform seed content SHALL load from `content/` through the same registry path as any other content.

### 6. Library browsing data
1. The library SHALL provide categorised, searchable listings with names, summaries and thumbnails for use by Free Assembly.

## Out of Scope
Connections between instances (assembly-engine). Behaviour implementations (simulation-engine). Creator-owned definition storage (backend).

## Open Questions
- Polygon and size budgets in `3d-asset-standards.md` are initial targets. Confirm them with a real asset in Task 5.
