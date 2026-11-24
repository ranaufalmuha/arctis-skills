# SvelteKit adapter

Official sources, reviewed 2026-11-24: [project structure](https://svelte.dev/docs/kit/project-structure) and [routing](https://svelte.dev/docs/kit/routing). Preserve configured paths and installed-version conventions.

```text
src/
  app.html
  routes/
    +layout.svelte
    +page.svelte
    account/+page.server.ts
    api/status/+server.ts
  lib/
    feat/auth/
      LoginForm.svelte
    shared/
    server/
      feat/auth/
      shared/
static/
svelte.config.js
tests/
```

Routes and lib are native roles; feat/shared subdivisions are Arctis choices. Route components compose features. Load functions, actions, and HTTP handlers remain thin boundaries, delegating behavior to the owner. Preserve + filenames, generated types, hooks, and layout composition; do not add route.tsx registration.

Use $lib or configured aliases. Keep server-only implementations under the framework's supported server-only boundary; public features and shared UI must not import them. A feature may have corresponding client/server parts without permitting sibling-feature imports. Keep request-specific state out of shared server globals. Framework hooks are integration entrypoints, not arbitrary feature managers.

No bundled Svelte architecture validator exists. Use Svelte-aware checking, the project's build/tests, and review both component scripts/templates and generated routing relationships. Do not treat the TS-only checker as complete coverage. Plain Svelte without SvelteKit uses [other frameworks](other.md); routes/lib conventions are not universal Svelte requirements.
