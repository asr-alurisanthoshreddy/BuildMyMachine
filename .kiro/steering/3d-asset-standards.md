# 3D Asset Standards

These rules apply equally to hand-made and AI-generated assets. Every asset passes the same validator before it is registered.

## 1. Format and Units

- glTF 2.0, delivered as GLB. Draco or Meshopt compression is allowed if the loader supports it.
- Units: metres. Scale 1.0. Y-up, right-handed (glTF default).
- Real-world scale where known. If an asset is stylised or scaled, record it in metadata.
- Apply transforms before export (no unapplied scale or rotation on the root).

## 2. Structure and Naming

- One root node per asset named after the definition id in `kebab-case` (for example `electric-motor-basic`).
- Mesh nodes use descriptive names: `body`, `shaft`, `fan`. Moving parts that a behaviour animates are separate nodes with stable names (for example `rotor`, `belt`).
- Pivot (origin) of each node sits at a meaningful point: the rotation axis for rotating parts, the mounting reference for the root.
- Names are unique within an asset.

## 3. Connection Ports

- Each port is an **empty node** named `port_<id>` (for example `port_mount_left`, `port_shaft_out`).
- Position and orientation of the empty define the port frame. The local +Z axis points out of the mating face, and +Y is the up reference. Document any deviation in the port's `extras`.
- Each port node has `extras`:

```json
{
  "portType": "shaft-coupling",
  "role": "male",
  "accepts": ["shaft-coupling:female"],
  "snapTolerance": 0.005,
  "axis": [0, 0, 1],
  "rotationLimits": null,
  "label": "Output shaft"
}
```

- On registration, ports are normalised into Component Definition port declarations (see `architecture.md`). The definition is the source of truth afterwards. A mismatch between the GLB and the definition is a validation error.
- Both hand-made and generated assets use this same port format. Generated assets get ports only through an approved connection-point suggestion, which the creator places or adjusts. They are not trusted from the generator output.

## 4. Metadata

Asset-level `extras` on the scene or root:

```json
{
  "definitionId": "electric-motor-basic",
  "assetVersion": 1,
  "units": "m",
  "source": "hand-authored | ai-generated | licensed",
  "visualOnly": false,
  "license": "CC0-1.0",
  "author": "...",
  "accuracyNote": "Stylised model, not dimensionally accurate."
}
```

- `visualOnly` is `true` for every AI-generated asset. Nothing in the asset can declare behaviour. Behaviours are bound on instances (see `simulation-standards.md`).
- `accuracyNote` is required. Do not claim dimensional accuracy without verification.

## 5. Budgets (initial targets, revise in `component-library` design)

| Item | Budget |
|------|--------|
| Triangles per component | 20,000 maximum, 5,000 typical |
| Complete playground | 300,000 triangles maximum at the default detail |
| Texture size | 2048 px maximum edge, power of two preferred |
| File size per GLB | 5 MB maximum |
| Materials per component | 4 maximum |

Provide a lower-detail asset where a component exceeds its budget. AI-generated meshes are decimated or rejected to meet the budget.

## 6. Materials and Appearance

- PBR metallic-roughness materials. No custom shaders in assets.
- Use consistent colour conventions so function is readable (for example, moving parts and sensors have distinct colours). Define the palette in `component-library`.
- Do not rely on colour alone to convey information.

## 7. Internal Structure

- Where a model supports exploring internals, provide separate nodes for internal parts and mark them in `extras` (`"internal": true`) so the view can hide the housing or make it transparent.

## 8. Licensing and Provenance

- Every asset records its source and licence. Do not commit assets with unclear licences.
- AI-generated assets record the generator, the version and the input image ids, not the raw images.
- Platform content must use licences that allow redistribution.

## 9. Validation (automated)

The validator rejects an asset when any of these fail:

- not a valid glTF 2.0 container, or fails the file-signature check
- units or scale not metres, or unapplied root transform
- duplicate or invalid names, or missing `definitionId`
- malformed `port_*` extras, or a port that references an unknown type
- ports that disagree with the definition
- budget exceeded
- missing licence or `accuracyNote`
- `visualOnly` is false for an asset with `source: ai-generated`
- any attempt to embed behaviour or script data

Provide clear, human-readable error messages for each failure.

## 10. Authoring Guidance

- Author in Blender (or similar) with the Khronos glTF exporter. Keep an editable source file outside the repo or in a documented location. Do not commit large binary sources without review.
- Provide a short asset checklist in the `component-library` spec so authors can self-check before submitting.
