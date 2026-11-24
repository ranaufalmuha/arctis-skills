# Godot mapping

Inspect `project.godot`, installed engine version, main scene, autoloads, addons, and language. Preserve existing GDScript or C# conventions. This guide is not an engine-tested template.

## Research basis (2026-11-24)

Official guidance exists: [Godot project organization](https://docs.godotengine.org/en/stable/tutorials/best_practices/project_organization.html) recommends keeping scene-related assets together and consistent snake_case paths (with C# exceptions). [Scene organization](https://docs.godotengine.org/en/stable/tutorials/best_practices/scene_organization.html) informs public scene boundaries.

Use that scene-centered approach rather than Unity's global asset-type split. The game/composition/features/shared names below are Arctis ownership conventions, not Godot requirements. Start with colocated scenes, scripts, and assets; split a feature into responsibility subfolders only as it grows.

## Example layout

```text
project.godot
game/
  composition/
    main.tscn
    main.gd
    levels/
  features/
    combat/
      combat.tscn
      combat.gd
      damage_rules.gd
      hit_effect.tscn
      hit_effect.gd
      hit_texture.png
  shared/
addons/
tests/
```

Only create occupied paths; small feature scenes and their scripts/assets can live together. Use snake_case paths for ordinary Godot content and retain C# class/file conventions where applicable. `addons/` holds installed addons; do not reorganize third-party internals into game features. Test placement follows the existing runner.

## Boundaries

A reusable feature scene exposes commands, signals, and configuration. The composing parent connects public surfaces. A feature may reference its own child nodes; it must not reach through another feature's node tree or rely on arbitrary parent paths. Avoid autoload service locators that hide cross-feature dependencies. An autoload can own an explicitly application-scoped responsibility when justified.

Resource classes describe authored data; distinguish shared resource instances from per-entity mutable state. `class_name`, `load`, `preload`, exported Node references, and scene-resource dependencies all participate in ownership. Folder names provide conventions, not language-enforced privacy.

## Lifecycle and validation

Inspect scene initialization, signal connections, freeing/removal, and re-entry for the changed feature. Use the editor's resource-aware moves and preserve the version's UID/sidecar files. Recheck main-scene and autoload settings after moves.

Import/parse the project, instantiate affected scenes, and run the existing test workflow. Export when changes affect packaged paths/content. Do not add `.gdignore` to content or test resources that the engine must load; ignored resources cannot be loaded normally. Report dynamic dependencies that static inspection cannot resolve.

Native references: [project organization](https://docs.godotengine.org/en/stable/tutorials/best_practices/project_organization.html) and [scene organization](https://docs.godotengine.org/en/stable/tutorials/best_practices/scene_organization.html). Use the installed version's documentation.
