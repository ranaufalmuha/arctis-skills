# Other frontend frameworks and router modes

First identify the UI framework, meta-framework, router mode, versions, native directories, and generated files. A build tool or desktop host does not determine application architecture. Research official version-matched organization/routing guidance before assigning paths.

Preserve official required files and recommended conventions. Map composition, independent features, and feature-independent shared code onto those paths. When reviewed official sources leave ownership unspecified, use an Arctis pages/feat/shared-style mapping adapted to native extensions and route mechanisms. This is a frontend fallback, not Unity asset foldering. Label the choice as Arctis and record the sources/date; unavailable documentation is not proof of absent guidance.

Keep cross-feature imports, reverse dependencies, and cycles forbidden. Review generated routes, templates, auto-imports, server/client boundaries, and native test conventions. Preserve an existing equivalent layout rather than mass-renaming it. Never edit generated files to enforce folder symmetry.

Use native compile/build/test checks and manual boundary review until a framework-specific validator exists. Do not claim supported automated enforcement from a JS/TS scan that omits component files or generated dependencies. Add an executable profile only with positive/negative fixtures for real native syntax and resolution.
