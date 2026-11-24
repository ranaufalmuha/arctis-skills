# Lifecycle, simulation, and state

For each changed persistent object or system, identify its lifetime: application, session, scene/world, entity, or pooled instance. Record who initializes, updates, resets, and disposes it. Do not invent a global singleton to bypass those decisions.

## Composition and transitions

Bootstrap installs feature entrypoints and injects dependencies. Scene/world composition connects public surfaces. Feature callbacks implement feature behavior. Keep composition shallow without forbidding native scene hierarchies.

On transitions, cancel owned asynchronous work, release handles according to engine semantics, remove subscriptions when required, and reject stale callbacks. For pooled objects, reset per-use state and subscriptions on reuse; destruction-only cleanup is insufficient. Verify repeated entry, not just the first run.

## Time and ordering

Classify work as input sampling, simulation, physics, or presentation. Use engine-supported timing rather than assuming render frames equal simulation steps. Declare order where one system consumes another's same-tick result; avoid imposing a total order on unrelated work. Define pause behavior and which clocks continue.

For replay/rollback requirements, specify timestep, RNG ownership, snapshots, and external inputs. Fixed timestep alone does not guarantee deterministic physics or cross-platform reproducibility. Do not require a deterministic simulation for every game.

## State and persistence

Separate immutable authored configuration from mutable runtime state. Shared Resources, ScriptableObjects, or assets must not accidentally carry per-entity state across instances or sessions. State restoration goes through the owning feature and preserves invariants.

Persist stable identifiers and versioned data rather than raw runtime pointers or scene-node paths. Handle missing/older fields according to the project's compatibility policy. Keep serialization and I/O in the owning infrastructure adapter; do not build a universal repository layer for all gameplay.

## Performance boundaries

Measure the affected hot path before changing architecture for speed. Avoid introducing per-frame allocations, synchronous disk/network access, or repeated global lookups through new abstractions. Use engine-native data layouts where profiling justifies them; do not require an interface call per entity merely for architectural symmetry.
