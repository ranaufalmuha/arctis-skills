---
name: game
description: Apply Arctis Game Architecture when implementing, refactoring, or reviewing game projects and engine-based dedicated servers. Use consistent composition/features/shared ownership with native Unity, Godot, Unreal Engine, Bevy, or other engine layouts, explicit state and asset boundaries, and engine-appropriate validation.
---

# Arctis Game Architecture

**Composition connects. Features own gameplay. Shared provides primitives. Engines determine physical layout.** Use this skill independently for each game application; it does not require the frontend, backend, or monorepo skills.

## Before coding

1. Read the project's `AGENTS.md`. Inspect engine/version manifests, source and asset roots, entry scenes/worlds, build targets, plugins, tests, and existing conventions. Distinguish game runtime from a web shell or external API. Do not upgrade the engine to fit this skill.
2. Identify the owning feature, authoritative state writer, public contracts, and affected asset references. Read [structure](references/structure.md) and [boundaries](references/boundaries.md).
3. Follow official engine organization guidance first; use the Unity-like fallback only for undocumented layout choices, preserving native roots. Each adapter distinguishes official guidance from Arctis conventions. Read only the matching engine adapter: [Unity](references/engines/unity.md), [Godot](references/engines/godot.md), [Unreal Engine](references/engines/unreal.md), [Bevy](references/engines/bevy.md), or [other engines](references/engines/other.md). Confirm ambiguous engine names rather than silently treating them as Bevy.
4. For scene, timing, or state changes, read [lifecycle](references/lifecycle.md); for assets or moves, [assets](references/assets.md); for networked gameplay, [multiplayer](references/multiplayer.md); for adoption into an existing project, [migration](references/migration.md).

## During coding

- Use the common composition/features/shared ownership model through the engine's native roots, casing, and module system. Create only occupied folders. Preserve an existing equivalent layout with a documented mapping rather than broadly reorganizing it.
- Keep feature rules, state, presentation, assets, and integration adapters under their owner. Ownership may span engine-native source and asset-type folders; it does not require physical colocation. Split these responsibilities only when they exist; do not scaffold every possible subsystem.
- Cross-feature access is through declared public contracts, never internals. Keep module dependencies acyclic. Shared never depends on game features or composition. These are game rules, not changes to Arctis frontend/backend rules.
- Keep authoritative gameplay writes out of presentation and storage adapters. Composition wires dependencies; it does not become a global gameplay manager. Engine-native components, nodes, actors, and ECS systems are allowed within features.
- Preserve serialized identity and references during moves. Separate runtime from editor-only code, vendor content, generated files, and build output. Do not hand-edit binary assets.
- Do not impose web folder names, HTTP layers, a universal test location, or a mandatory engine-independent domain layer on games.

## Before completion

Follow [testing](references/testing.md) and [enforcement](references/enforcement.md). Run relevant available compile/import, tests, and build checks; inspect affected scenes and assets. Report engine/version, ownership changes, checks actually run, findings, and unavailable checks. This skill ships architecture guidance, not an executable cross-engine validator; do not claim automatic enforcement or engine-tested compatibility without evidence.
