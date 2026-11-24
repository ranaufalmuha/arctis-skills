# Vue adapter

Official sources, reviewed 2026-11-24: [Vue quick start](https://vuejs.org/guide/quick-start.html), [single-file components](https://vuejs.org/guide/scaling-up/sfc), and [Vue Router](https://router.vuejs.org/guide/). Vue supports .vue components; routing belongs to the selected router. These sources do not mandate a complete pages/feat/shared taxonomy, so the following is an Arctis mapping for an explicit Vue Router application.

```text
src/
  main.ts
  App.vue
  router/index.ts
  pages/HomePage.vue
  feat/auth/
    components/LoginForm.vue
    composables/useLogin.ts
    services/login.ts
  shared/components/Button.vue
public/
tests/
```

App.vue and router are composition. Router configuration may register page components; pages compose features/shared and do not import other pages or router internals. Keep composables and stores under their owning feature; shared contains only feature-independent code. Use native template/script/style SFC sections instead of converting components into TSX. Document configured aliases.

If file-based Vue Router is selected, follow its installed version's conventions and generated route graph; do not add the explicit router tree above unnecessarily. For Nuxt, use [Nuxt](nuxt.md) instead.

Cross-feature access remains forbidden, including component use through template registration, plugins, global stores, or auto-imports. Do not promote a business store into shared merely because two screens use it. Tests default to external tests for this Arctis profile; preserve an explicit project test convention during adoption.

No bundled Vue architecture validator exists. The TypeScript-only checker cannot validate SFC templates or complete their import graph. Use the project's Vue-aware type checking, lint, build, tests, and manual ownership review; report that architecture enforcement is partial until a tested Vue parser/resolver is added.
