# Mapping another engine or game framework

Use this path for engines without a dedicated adapter, including browser game frameworks. Do not claim tested support or guess API names. Confirm ambiguous engine identity/version before engine-specific edits; shared ownership analysis can proceed meanwhile.

1. First search official engine documentation for project organization, directory layout, asset handling, and the target version. Record exact sources, their scope, and what they actually prescribe. If sources cannot be accessed, report that uncertainty; do not equate unavailable evidence with absent guidance. Inspect native project manifests, entrypoints, content roots, module/plugin systems, lifecycle, and existing tests. Read official version-matched documentation for unfamiliar behavior.
2. Map composition/features/shared onto native folders without renaming required engine roots. Record source/content separation and public feature surfaces.
3. Identify engine mechanisms for privacy, state access, scene/resource references, timing, editor/runtime separation, and packaging. State where a boundary is only a convention.
4. Apply the common ownership and dependency rules while preserving the engine's programming model: scene graph, ECS, actor/component, or another model.
5. Use real compile/import, behavioral, content-load, and packaging checks. Report unverified areas instead of claiming adapter parity.

For a browser game inside React, apply game rules to the gameplay runtime and frontend rules to the surrounding web application. Use an explicit public integration boundary. Do not impose pages/feat/shared on the engine runtime simply because it uses TypeScript.

Promote a mapping into a dedicated adapter after a concrete project demonstrates useful engine-specific guidance. Include actual layout examples, dependency semantics, lifecycle/asset pitfalls, validation commands discovered from that project, supported scope, and evidence. Avoid an empty adapter for every possible engine.

## Fallback when no applicable official layout is found

Keep all documented engine/language roots. Within flexible locations, adapt the [Unity asset-type layout](unity.md): code, scenes, reusable object assets if supported, art, audio, animations, data, tests, and sandbox. Code retains composition/features/shared ownership; content carries feature ownership within its asset type. Use the engine's actual terms, casing, asset formats, and packaging conventions rather than copying Unity-specific folders literally. Create only occupied paths.

Label this as an Arctis fallback, list the official sources checked and research date, and distinguish native requirements from chosen conventions. Official guidance wins wherever applicable; use the fallback only for gaps. Do not reorganize an existing equivalent project merely because a new default is available.
