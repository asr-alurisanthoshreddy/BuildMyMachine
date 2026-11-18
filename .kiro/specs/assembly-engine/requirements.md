# Requirements: Assembly Engine

**Depends on:** component-library

## Introduction
The shared, pure domain engine for placing and connecting Component Instances, validating compatibility and tracking hierarchy. Free Assembly and Guided Assembly are both built on it. It has no UI, Three.js or I/O.

## Requirements

### 1. Assembly state
1. An assembly SHALL be immutable, serialisable state holding instances, parent links and connections.
2. The engine SHALL expose commands: `addInstance`, `removeInstance`, `moveInstance`, `rotateInstance`, `connect`, `disconnect`, `reparent`.
3. Each command SHALL return a new state or a typed validation error, and SHALL NOT mutate its input.
4. Serialising and deserialising state SHALL give an equal state.

### 2. Connections and compatibility
1. A connection SHALL link two ports on two different instances.
2. `connect` SHALL succeed only if the port types and roles are compatible per each port's `accepts` list, both ports are free (unless the port allows multiple), and port constraints (axis, rotation limits) hold for the resulting pose.
3. WHEN a connection is made THEN the engine SHALL be able to compute the aligned pose of the moving instance relative to the fixed one (snap).
4. WHEN an instance is removed THEN its connections SHALL be removed and the engine SHALL report dependants affected.
5. Failures SHALL return a code and a plain-language explanation naming the incompatibility (for example, type mismatch or port occupied).

### 3. Hierarchy
1. The engine SHALL maintain a parent/child hierarchy and reject cycles.
2. Moving a parent SHALL move its descendants (world transforms derive from the hierarchy).
3. Connected instances SHALL be queryable as a graph (neighbours, connected components, path between two instances).

### 4. Validation
1. `validateAssembly` SHALL report all issues: unmet required ports, dangling references, cycles, instances with unknown definitions, and invalid behaviour bindings (using the library binding validator).
2. The engine SHALL support a query for **candidate connections** for an instance, listing compatible free ports with a snap pose and a rank (for example by distance).
3. `setBinding` SHALL accept only bindings that pass the library binding validator.

### 5. Step checking for guided mode
1. The engine SHALL provide `checkStep(state, predicate)` for predicates such as "instance X exists", "port A connected to port B" and "instance X is within tolerance of pose P".
2. Predicates SHALL be data (serialisable), not functions, so scripts can be stored in a playground definition.

### 6. Shared by both modes
1. Guided and Free modes SHALL use the same engine, state and validation. The engine SHALL NOT contain mode-specific logic.
2. A test SHALL show that the same command sequence gives equal state whichever mode drives it.

## Out of Scope
Rendering, pointer handling, scripts (guided-assembly), persistence.

## Open Questions
- Whether multiple connections per port (for example bolts into a frame slot) are expressed by a port `capacity` field. Decide in design.
