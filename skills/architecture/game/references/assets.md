# Asset ownership and safe moves

Feature-specific scenes, prefabs, models, textures, audio, and configuration belong with that feature, or in the engine content root under the corresponding feature path. In an asset-type-first layout such as the Unity adapter, keep ownership within the relevant type folder (for example, `Prefabs/Features/Combat/`); physical colocation with scripts is not required. Composition owns assembled levels/worlds. Shared assets must be genuinely reused and have a clear owner; vendor assets retain identifiable provenance.

Use the engine's supported move/rename workflow when identity or references depend on its asset system. Before moving, inspect inbound references and serialized type identities. After moving, reimport/compile, open affected scenes, and exercise a build or export when relevant. A clean code diff is insufficient evidence that a serialized migration succeeded.

| Engine | Preserve and inspect |
| --- | --- |
| Unity | `.meta` files/GUIDs, script identity, prefab variants, scene references, addressable/resource lookup paths if used |
| Godot | Resource UIDs and sidecars used by the installed version, `res://` paths, scene links, autoload/main-scene settings |
| Unreal | Package/object paths, redirectors, Blueprint references, soft paths, cook inclusion; use editor tooling for binary assets |
| Bevy | Asset paths/handles, loader configuration, shipped asset layout; update string-based lookups during moves |

Engine caches and generated outputs are not source architecture. Preserve source metadata required by the engine while excluding regenerable artifacts using the project's ignore rules. Do not blanket-delete unknown metadata.

Keep runtime content separate from editor tooling and source-only working assets when the build pipeline needs that distinction. Verify actual packaging rules rather than assuming a folder name excludes content. Heavy cross-feature hard references may force unwanted loading; use a supported indirect/loading mechanism when needed, with explicit lifetime and failure handling.
