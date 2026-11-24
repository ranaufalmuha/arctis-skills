# Unreal Engine mapping

Inspect `.uproject`, engine version, `.uplugin` descriptors, `*.Build.cs`, targets, and existing C++/Blueprint ownership. Preserve a Blueprint-only project when appropriate; do not add C++ merely to match the example. This guide is not an engine-tested template.

## Research basis (2026-11-24)

Official guidance defines the native roots in [Directory Structure](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-directory-structure) and code boundaries in [Modules](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-modules). Keep Source, Content, Config, Plugins, and module Public/Private conventions.

These sources do not prescribe a complete game-specific content taxonomy. The Content/Game asset-type folders below therefore use the Unity-like Arctis fallback, adapted to Unreal Maps, Blueprints, and Data. This is not an Epic-mandated content layout. Community tutorials hosted on Epic's domain are not automatically engine-wide official standards; check authorship, audience, and version before adopting them.

## Example layout

```text
Project.uproject
Config/
Source/
  Project.Target.cs
  ProjectEditor.Target.cs
  ProjectGame/
    ProjectGame.Build.cs
    Public/
      Features/Combat/Contracts/
      Shared/
    Private/
      Composition/
      Features/Combat/
        Gameplay/
        Presentation/
        Infrastructure/
      Shared/
Content/
  Game/
    Maps/Composition/
    Blueprints/Features/Combat/
    Data/Features/Combat/
    Art/
      Materials/
      Meshes/
      Textures/
    Audio/
    Animations/
    UI/Features/Combat/
    VFX/Features/Combat/
    Tests/
    Sandbox/
Plugins/
```

Within asset types, use Features/<Name> or Shared when ownership needs clarification. Tests and Sandbox names alone do not exclude assets from cooking; check packaging and reference rules. Names are examples; keep the actual module and target names. Only create occupied paths. Source and content have matching owners, not identical trees. In a Blueprint-only project, use the Content ownership mapping and inspect Blueprint dependencies directly. Existing content plugins can retain their native content roots.

## Boundaries

Public headers are intentional module API; Private holds implementation. Within one module these folders alone do not enforce feature isolation. Expose a small feature installation surface if composition needs one. Split modules/plugins when build, loading, editor separation, or ownership benefits justify it; do not require one module per tiny feature.

Declare real module dependencies in Build.cs and appropriate descriptors. Keep editor modules out of runtime targets. Gameplay Framework roles remain engine-native: use the project's GameMode, GameState, Controller, Pawn, components, and subsystems according to their lifecycle/authority rather than wrapping all of them in a generic manager. Verify framework behavior against the project's engine version.

Blueprint function calls, casts, inheritance, hard/soft object references, and C++ includes all participate in dependency review. A soft reference changes loading behavior, not architectural ownership. Cross-feature references use documented public classes, interfaces, or data contracts.

## Assets and validation

Move/rename assets with Unreal tooling, inspect redirectors and referencers, and validate affected maps/Blueprints. Never hand-edit binary `.uasset` or `.umap` files. Keep generated/intermediate output separate from source; preserve project-specific build resources.

Compile changed targets and Blueprints, run relevant automation/functional tests, and cook/package when content or build changes require it. Verify server authority if networked gameplay changes. A successful C++ compilation does not validate all content dependencies or cook inclusion.

Native references: [modules](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-modules) and [Gameplay Framework](https://dev.epicgames.com/documentation/unreal-engine/gameplay-framework-in-unreal-engine?lang=en-US). Resolve version-specific details before implementation.
