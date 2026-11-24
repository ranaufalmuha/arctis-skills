# Validation and honest enforcement

This skill has no bundled executable game-architecture validator. Its engine adapters are documented architecture mappings, not certified engine integrations. No Unity, Godot, Unreal, or Bevy version is runtime-tested by the documentation alone. Record the target version and actual evidence in each project.

## Inspection sequence

1. Identify engine/version and relevant runtime/editor/build targets from project files.
2. Map touched paths to composition, named features, shared, or engine-owned infrastructure. Check [boundaries](boundaries.md), including state writers and serialized references.
3. Run the project's existing compiler/import checks and relevant tests. Validate through the engine when scene/resource semantics require it.
4. For asset/build configuration changes, load the affected content and check the relevant packaged/exported target when available.
5. Report each check as passed, failed, or not run with a reason. An unavailable engine or checker is not a pass.

## Engine evidence

| Adapter | Code evidence | Asset/runtime evidence | Limit |
| --- | --- | --- | --- |
| Unity | Assembly references and compiler; runtime/editor split | Scene/prefab inspection, Edit/Play Mode tests, relevant player build | `.asmdef` does not police every class inside an assembly or serialized edge |
| Godot | Script parsing/type checks available for the language/version | Import, scene instantiation, test runner, relevant export | Paths, autoloads, signals, and dynamic loads need review |
| Unreal | Module dependencies and target compilation | Blueprint compilation, reference inspection, automation, relevant cook/package | C++ checks do not cover binary assets or every soft reference |
| Bevy | Cargo checks/tests and Rust visibility | Minimal app/system tests, asset loading, client/server smoke checks | Visibility does not enforce ECS write ownership or schedule intent |

Use project-documented commands and the installed engine/toolchain. Do not invent a command, silently install an engine, or claim a build ran from static inspection. Preserve existing checks; report exceptions instead of suppressing them.

## Future automation

Add an engine-specific checker only after there is a real project and positive/negative fixtures. Give diagnostics stable codes and separate violations from unsupported checks. A useful checker resolves native module identity and asset edges; regex searches alone must not be advertised as full dependency enforcement. Keep executable helpers with this skill only when they are actually implemented and tested.

## Completion report

Include engine/version and targets, changed ownership or public contracts, commands/results, scene/asset checks, unresolved violations, and checks not run. Never summarize partial coverage as fully validated architecture.
