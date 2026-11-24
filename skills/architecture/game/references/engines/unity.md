# Unity mapping

Inspect `ProjectSettings/ProjectVersion.txt`, `Packages/manifest.json`, assembly definitions, and existing scenes. Use the project's Unity version and GameObject/DOTS approach; do not convert between them for folder consistency. This guide is not an engine-tested template.

## Research basis (2026-11-24)

Official organization guidance exists: [Unity project organization](https://unity.com/how-to/organizing-your-project#folder-structure). Asset-type grouping is its example, not a required universal tree. The concrete Game namespace and composition/features/shared mapping below are Arctis choices. Preserve native Assets, Packages, and ProjectSettings responsibilities.

## Project layout

[Unity's project organization guide](https://unity.com/how-to/organizing-your-project#folder-structure) offers asset-type layouts as examples, not a mandatory standard. Arctis uses the following asset-type-first mapping for new Unity projects, with logical ownership inside code and content. This is an Arctis convention, not an official Unity template. Keep an existing equivalent layout unless migration is requested.

```text
Assets/
  Game/
    Scripts/
      Runtime/
        Composition/
          Bootstrap.cs
        Features/
          Combat/
            Contracts/
            Gameplay/
            Presentation/
            Infrastructure/
        Shared/
      Editor/
        Features/Combat/
    Scenes/
      Composition/
        Main.unity
    Prefabs/
      Features/Combat/
      Shared/
    Art/
      Materials/
      Models/
      Textures/
    Audio/
      Music/
      SoundEffects/
    Animations/
    Data/
      Features/Combat/
      Shared/
    Tests/
      EditMode/
        Features/Combat/
      PlayMode/
        Features/Combat/
    Sandbox/
      DeveloperName/
  VendorPackage/               preserve the vendor's required location
Packages/
ProjectSettings/
```

`Game` and `DeveloperName` are example names. This is a placement map, not a request to generate all directories. Use only occupied paths. `Sandbox` contains experiments; `Tests` contains automated checks. Neither folder name alone excludes content or code from a player: configure test assemblies and inspect build scenes, asset inclusion, and loading rules.

## Ownership within asset types

The shared composition/features/shared model describes responsibilities, not three required folders directly beneath `Assets/Game`. For this Unity mapping:

| Responsibility | Code | Content examples |
| --- | --- | --- |
| Composition | `Scripts/Runtime/Composition/` | `Scenes/Composition/`, assembled level prefabs |
| Combat feature | `Scripts/Runtime/Features/Combat/` | `Prefabs/Features/Combat/`, `Data/Features/Combat/` |
| Shared primitives | `Scripts/Runtime/Shared/` | `Prefabs/Shared/`, genuinely reused materials/audio |

When ownership is not obvious, use a feature path within the relevant asset type, such as `Art/Materials/Features/Combat/` or `Audio/SoundEffects/Features/Combat/`. Do not repeat every category for every asset type or put all assets into Shared by default. Feature UI behavior remains in Presentation; its textures, prefabs, and animation assets follow their asset-type folders. ScriptableObject class definitions live in Scripts; authored instances live in Data.

Document naming and ownership together. Keep paths free of spaces and use consistent casing. Separate project-owned content from vendor imports without arbitrarily relocating a vendor package. Preserve native package layouts for UPM content. Additional top-level source-content folders outside Assets need a concrete pipeline reason.

## Safe folder changes

Move assets in the Unity Editor and commit their associated `.meta` files, including folder metadata. Do not regenerate GUIDs during reorganization. Avoid empty placeholder folders. Review serialized references and path-based loaders after moves; namespace or assembly changes can require additional serialized-type migration. Never perform a repository-wide rename just to adopt this example.

## Boundaries

Use namespaces and assembly definitions for meaningful boundaries. An `.asmdef` applies to scripts in its directory subtree unless a nested assembly changes ownership. A single feature assembly does not enforce separation between contracts, gameplay, and presentation inside it; use review or split assemblies when that separation matters. Separate a contract assembly when siblings need it without pulling in implementation.

An illustrative dependency graph is composition → feature runtime → feature contracts/shared, with sibling features depending only on the contract assembly. If splitting feature presentation or infrastructure, those assemblies depend on contracts/gameplay's public surface as allowed by the shared policy, never the reverse. Keep the feature installer in an assembly that can wire these implementations without creating a cycle.

Use explicitly editor-only assemblies for editor code. Inspect assembly/platform settings rather than assuming a folder named Editor is sufficient under every assembly configuration. Test assemblies reference runtime; runtime never references tests. Preserve the project's Test Framework configuration.

## Runtime and assets

MonoBehaviours or ECS systems may own gameplay behavior; plain C# is useful for rules that benefit from independent tests, not mandatory for all code. Treat ScriptableObject configuration as authored data unless mutable shared state is explicitly intended. Define scene/session lifetime and pooled reset behavior. Keep UI/VFX/audio as observers of accepted gameplay state.

Use Unity-aware asset moves and preserve GUIDs. Verify scene/prefab references after script, namespace, or assembly changes. Do not edit Library, Temp, or generated project files as source fixes. Check affected Addressables/Resources paths only if the project uses them.

## Validation

Inspect assembly edges, compile in the actual editor, run relevant EditMode/PlayMode tests, open affected scenes, and build the relevant player/server target when changes warrant it. Report unavailable editor/build tools. Assembly checks alone do not verify serialized references or state ownership.

Native mechanism reference: [Unity assembly assets](https://docs.unity.com/en-us/engine/6000.3/manual/programming-environment/script-compilation/assembly-definition-files/assembly-definitions-creating). Select documentation matching the installed version.
