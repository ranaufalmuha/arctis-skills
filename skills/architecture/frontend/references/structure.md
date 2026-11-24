# Framework-agnostic ownership

Composition connects independent features; features own behavior; shared provides feature-independent primitives. These roles are stable, but physical folder names and test locations follow the selected framework adapter. Do not enforce a universal three-directory src tree.

## Select one adapter

| Application | Adapter | Automated architecture coverage |
| --- | --- | --- |
| React SPA with explicit registration | [React](frameworks/react.md) | spa profile |
| Next.js App or Pages Router | [Next.js](frameworks/nextjs.md) | next-app / next-pages profiles; documented limits |
| Vue with Vue Router | [Vue](frameworks/vue.md) | Manual architecture review + native tooling |
| Nuxt | [Nuxt](frameworks/nuxt.md) | Manual architecture review + native tooling |
| SvelteKit | [SvelteKit](frameworks/sveltekit.md) | Manual architecture review + native tooling |
| Angular | [Angular](frameworks/angular.md) | Manual architecture review + native tooling |
| Other framework/router mode | [Other](frameworks/other.md) | Research and explicit mapping first |

Vite is build tooling; React/Vue are UI choices; Next.js/Nuxt add routing/runtime conventions. Tauri does not identify its hosted UI framework. Prefer the most specific adapter and load only its relevant references.

## Common responsibilities

Route entrypoints and app shells stay thin. Keep business state, network orchestration, and complex behavior in named features. Feature-specific UI belongs to the feature even when several pages use it. Promote only genuinely feature-independent responsibilities to shared; never move business logic there to bypass isolation.

Use framework vocabulary inside features: hooks, composables, services, components, or colocated files as appropriate. Create occupied directories only. Native config, public/static assets, generated output, and runtime entrypoints retain their prescribed locations. Record ownership where the path alone is ambiguous.

Adapters distinguish official conventions from Arctis choices and record research dates. Follow official guidance first; fill undocumented ownership choices with a minimal Arctis mapping. Preserve existing equivalent structures unless migration is requested. Read [boundaries](boundaries.md), [routing](routing.md), and [testing](testing.md) for common rules.
