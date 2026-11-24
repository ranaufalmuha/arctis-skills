# Consistent ownership, native folders

Use three logical areas across engines:

| Area | Responsibility | Must not become |
| --- | --- | --- |
| Composition | Entry point, scene/world assembly, dependency wiring, cross-feature orchestration | A container for combat, inventory, or other feature rules |
| Features | A named gameplay capability and its owned code, state, assets, and presentation | A technical catchall without named gameplay owners |
| Shared | Small feature-independent primitives and contracts with real consumers | A place to hide feature coupling |

A capability might be combat, movement, inventory, progression, or a main-menu flow. Keep gameplay ownership explicit even when an engine organizes physical content by asset type. An ordinary feature can start with a handful of files.

## Logical blueprint

This is a responsibility map, not a mandatory physical tree:

```text
game-owned-root/
  composition/                 entry, levels/worlds, integration wiring
  features/
    combat/
      contracts/               public commands, observations, value types
      gameplay/                rules and authoritative state changes
      presentation/            feature UI, audio, VFX, animation
      infrastructure/          persistence/network/platform implementations
      assets/                  feature-owned content
  shared/                      feature-independent primitives
```

Create subfolders only when useful files exist. For a small feature, keep files together and identify the public surface explicitly. Tests and editor tooling follow the adapter, outside shipping runtime dependencies. A screen spanning several features belongs to composition; a health bar belongs to combat presentation. A reusable visual control may belong to shared; health rules do not.

## Source precedence

Use applicable official engine organization guidance first. Each engine adapter records its sources, research date, and which choices are Arctis conventions. Where reviewed official sources leave folder taxonomy unspecified, use a Unity-like asset-type fallback inside native engine/language roots. Unavailable documentation is an uncertainty, not proof that guidance does not exist. Preserve existing equivalent layouts.

## Physical mapping

| Logical area | Unity | Godot | Unreal Engine | Bevy |
| --- | --- | --- | --- | --- |
| Composition | `Assets/Game/Scripts/Runtime/Composition/`, `Assets/Game/Scenes/Composition/` | `game/composition/` | `Source/<Module>/Private/Composition/`, `Content/Game/Maps/Composition/` | `src/composition/` |
| Features | `Assets/Game/Scripts/Runtime/Features/<Name>/`, owned paths within asset-type folders | `game/features/<name>/` | Public/Private feature paths in `Source/`, ownership within `Content/Game/<AssetType>/Features/` | `src/features/<name>/`, `assets/<asset_type>/features/<name>/` |
| Shared | `Assets/Game/Scripts/Runtime/Shared/`, shared paths within asset-type folders | `game/shared/` | Public/Private shared paths in `Source/`, shared ownership within Content asset types | `src/shared/`, `assets/<asset_type>/shared/` |

`Game` is an example project namespace, not a required product name. Existing `_Project`, project-named roots, crates, packages, and plugins can implement the same ownership. Unity groups content by asset type and maps ownership within Scripts and the relevant asset folders; see the [Unity layout](engines/unity.md). Unreal deliberately splits source from content; do not force them into one directory. Godot can colocate scenes and assets. Bevy can colocate unit tests with source. Do not copy Unity casing to Godot or Rust.

## Project architecture record

Keep a short mapping in the target project's existing architecture document or `AGENTS.md`: engine/version, game roots, feature owners, public entrypoints, declared feature dependencies, state writers, editor/test boundaries, and validation commands. Add detail only for decisions that are not evident from code. An engine-native module boundary is stronger than a folder name; if a boundary is only conventional, say so.

See [boundaries](boundaries.md) for allowed edges and the selected engine adapter for concrete trees.
