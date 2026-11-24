# Testing by responsibility

Test observable behavior affected by the change. A folder-only change needs import/reference/build checks more than new unit tests that assert filenames.

| Concern | Useful evidence |
| --- | --- |
| Pure gameplay rule | Boundary cases for damage, inventory limits, progression, or state transitions |
| Engine integration | Instantiate the feature and exercise its public commands in the engine |
| Lifecycle | Enter/exit/re-enter; pooled reuse; no duplicate subscriptions or stale state |
| Persistence | Round-trip and supported older-version data; invalid state rejected |
| Presentation | Scene inspection or visual smoke test; no authoritative writes |
| Multiplayer | Authority validation, connection lifecycle, relevant reconciliation |
| Asset move | Reference resolution, import, affected scene load, target packaging |

Use native test conventions: Unity test assemblies, the Godot project's existing test runner/scenes, Unreal automation or functional tests, Rust unit and integration tests for Bevy. Colocated Rust unit tests are allowed. Do not impose the frontend/backend tests-outside-src rule on game applications or install a new runner without a concrete need.

A useful architecture acceptance slice is movement/input → combat state → HUD observation → save/load → scene re-entry. Run only applicable parts for a narrow task. Keep visual smoke tests separate from claims about deterministic simulation. Headless tests cannot establish rendering correctness.

See [enforcement](enforcement.md) for reporting and adapter-specific limits.
