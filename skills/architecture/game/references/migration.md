# Adopt without breaking the game

1. Inventory the existing ownership, engine version, source/content roots, startup, module boundaries, tests, and serialized references. Establish a compile/import and gameplay baseline if tooling is available.
2. Map existing folders to composition/features/shared. Keep equivalent names and native roots; document the mapping rather than renaming for style alone.
3. Select one cohesive feature. Define its public surface and state writer before moving files. Introduce a module/assembly/crate only if it provides a useful real boundary.
4. Move code and content through engine-supported workflows. Preserve identifiers and update inbound references, namespaces/type identities, build rules, and load paths as applicable. See [assets](assets.md).
5. Compile/import, test the feature, then exercise scene re-entry and affected packaging. If baseline failures existed, distinguish them from regressions; do not hide them.
6. Extend to the next feature only after the first slice works. Keep each migration reviewable and reversible through the project's version control workflow.

Do not generate empty layers, rewrite the engine integration, or move every file merely because a new skill was selected. For greenfield work, use the selected adapter's tree as a starting map and create only the files needed for the requested capability.
