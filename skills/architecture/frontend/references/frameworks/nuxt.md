# Nuxt adapter

Official sources, reviewed 2026-11-24: [Nuxt 4 directories](https://nuxt.com/docs/4.x/directory-structure), [pages](https://nuxt.com/docs/4.x/directory-structure/app/pages), and [shared](https://nuxt.com/docs/4.x/directory-structure/shared). Inspect the actual version, srcDir, layers, and configured aliases. Do not impose Nuxt 4 defaults on Nuxt 3 projects.

```text
app/
  app.vue
  pages/index.vue
  layouts/default.vue
  middleware/
  plugins/
  feat/auth/
    components/LoginForm.vue
    composables/useLogin.ts
  shared/components/Button.vue
server/
  api/
shared/
  types/
  utils/
public/
nuxt.config.ts
tests/
```

The app/pages and server roots follow Nuxt conventions; app/feat and app/shared are Arctis ownership choices. Native pages/layouts/middleware/plugins remain thin entrypoints or integration glue. Application-specific features use explicit imports unless scan configuration deliberately includes them. Do not assume custom feat/shared folders are automatically scanned. Existing native components/composables directories may hold feature-independent primitives or documented integration surfaces, not hidden cross-feature dependencies.

Root shared is a Nuxt cross-runtime boundary: it cannot import Vue or Nitro code. It is not the same as app/shared UI primitives. Keep server implementations and secrets out of app/shared and root shared. Server API endpoints validate/authenticate requests and delegate to server-owned feature code; client UI must not import it. Do not force Hono/Prisma layers onto Nitro endpoints.

Generated .nuxt output is not authored source. Review template use, auto-import configuration, route generation, layers, and server/client boundaries. No bundled Nuxt architecture validator exists: use Nuxt's version-appropriate type/build checks and project tests, plus explicit ownership review. A passing plain TypeScript check does not establish template/auto-import boundaries.
