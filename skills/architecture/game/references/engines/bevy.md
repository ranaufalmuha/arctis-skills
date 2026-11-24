# Bevy mapping

Inspect `Cargo.toml`, `Cargo.lock`, Rust toolchain, Bevy version/features, and the app's plugins/schedules. This adapter is for Bevy; an ambiguous name such as Bravy must be confirmed before applying it to a real project. This guide is not an engine-tested template.

## Research basis (2026-11-24)

Official [Bevy plugins](https://bevy.org/learn/quick-start/getting-started/plugins/) guidance establishes modular registration. The official [asset-loading example](https://github.com/bevyengine/bevy/blob/main/examples/asset/asset_loading.rs) describes the default assets root; [Cargo package layout](https://doc.rust-lang.org/cargo/guide/project-layout.html) defines Rust source and test conventions.

No complete prescribed game folder taxonomy was found in these reviewed sources. Use a Unity-like asset-type split inside assets as an Arctis fallback, while keeping Rust code in src and integration tests in tests. Do not rename src to Scripts or add Unity-specific Prefabs/Editor machinery. Scene data belongs in scenes only when the project uses a supported scene format. This finding is limited to the sources reviewed, not a claim that no other official guidance exists.

## Example layout

```text
Cargo.toml
Cargo.lock
src/
  main.rs
  lib.rs
  composition/
  features/
    combat/
      mod.rs
      plugin.rs
      contracts.rs
      gameplay/
      presentation/
      infrastructure/
  shared/
assets/
  scenes/composition/
  models/features/combat/
  textures/features/combat/
  materials/
  audio/
  animations/
  data/features/combat/
tests/
```

Within each asset type, use features/<name> or shared when ownership needs clarification. Only create occupied paths. A binary-only game need not add `lib.rs`; add a library target when integration tests or reuse need it. Small features can remain a few Rust modules. Unit tests may live in source under `#[cfg(test)]`; integration tests use Cargo's tests convention. Split crates only for meaningful boundaries or build targets.

## Boundaries

A feature plugin installs its own systems/resources and exposes a deliberate public surface. The composition module builds the app and installs plugins. Keep sibling-facing contracts separate from the plugin's implementation imports. Use Rust privacy and narrow exports; do not broadly re-export internals through a prelude.

ECS components/resources are data, systems implement behavior, and plugins organize registration. Public component types do not authorize arbitrary writers: declare the owning systems and review queries that mutate authoritative state. Presentation reads observations and emits input intent rather than writing simulation state.

## Scheduling and lifecycle

Choose schedules and state conditions according to the installed Bevy version. Express dependencies where ordering affects results; otherwise permit parallel work. Do not assume registration order establishes execution order. Review deferred commands and change visibility between systems when entities are created or removed.

Give resources/entities a session or state lifetime, handle exit cleanup, and verify app/state re-entry. Test simulation with the minimal plugins it actually needs; rendering smoke tests remain separate. A headless server target must not accidentally initialize client presentation plugins.

## Validation

Use the repository's cargo format/check/test/lint commands and relevant feature flags/targets. Exercise a minimal app or schedule for changed gameplay, asset loads for moved content, and a client/server run as applicable. Cargo checks do not prove ECS ownership, order, or asset paths.

Native references: [plugins](https://bevy.org/learn/quick-start/getting-started/plugins/) and [ECS](https://bevy.org/learn/quick-start/getting-started/ecs/). Check API names and schedule/event semantics against the pinned version rather than copying examples from another release.
