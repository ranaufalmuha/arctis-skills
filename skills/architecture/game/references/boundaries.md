# Dependency and state boundaries

## Allowed code dependencies

| Consumer | Allowed dependencies | Forbidden dependencies |
| --- | --- | --- |
| Composition | Feature public surfaces, shared, engine bootstrap APIs | Feature internals used as shortcuts |
| Feature gameplay | Own rules/contracts, shared, engine simulation APIs, declared public contracts of another feature | Presentation, concrete persistence/network adapters, another feature's internals, composition |
| Feature presentation | Own feature's public commands/read models, shared, rendering/audio/UI APIs | Direct authoritative state mutation or storage access |
| Feature infrastructure | Own contracts, shared, platform/storage/network APIs | Owning gameplay decisions or importing composition |
| Feature contracts | Stable value types, shared, justified engine types | Implementation classes, composition, presentation |
| Shared | Shared and compatible external/engine APIs | Features or composition |

A feature may expose a small installation/factory surface that wires its own internals. Composition calls that surface rather than constructing private internals. Keep that surface separate from contracts consumed by sibling features: importing a contract must not pull in installation or presentation code. Inside the feature, its installer may depend on all its own parts.

Cross-feature dependencies must be named in the project architecture record or native module configuration, point only to the provider's public surface, and form a directed acyclic graph. Do not export every implementation just to satisfy the rule. If two capabilities mutually depend on one another, move coordination upward, extract a truly shared contract, or reconsider feature boundaries.

This intentionally differs from Arctis frontend/backend's prohibition on cross-feature/module imports. Do not apply the game exception to those domains.

## Example: combat and progression

Combat owns HP and the transition to defeated. It exposes an immutable defeat notification. Progression consumes that contract to award XP. Combat does not import progression; composition installs both. Presentation reads combat observations and submits player intent through commands. A save adapter persists a snapshot; restoration is validated and applied by gameplay.

Invalid: the HUD edits HP, progression calls combat's private damage implementation, or combat imports the reward service while progression imports combat. An event bus does not make those dependencies disappear.

## Public data is not public mutation

Declare who may write each component/resource/state field. In ECS, public data types allow queries but do not authorize every system to mutate them. Read/write ownership and schedule relationships require review even when language visibility checks pass. Commands express intent; observations describe accepted changes. Avoid exposing mutable collections or engine objects whose public setters bypass the owner.

## Assets and runtime relationships

Inspect code imports, scene/prefab references, resource paths, Blueprint dependencies, service lookups, reflection, autoloads, and ECS access. A serialized reference to another feature's implementation is still a dependency. Public reusable prefabs/scenes/assets may be intentional feature entrypoints; document them as such. Internal child-node references within one owned scene are normal.

The no-cycle rule applies to module dependencies; it does not ban legitimate engine object graphs, parent links, or gameplay feedback over time. Review those separately for lifetime and ownership. Do not claim a static import check validates runtime behavior.
