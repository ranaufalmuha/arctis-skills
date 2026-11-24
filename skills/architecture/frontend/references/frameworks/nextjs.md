# Next.js architecture profile

Research basis: [official project structure](https://nextjs.org/docs/app/getting-started/project-structure), [Server and Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components), and [Pages Router](https://nextjs.org/docs/pages/building-your-application/routing/pages-and-layouts), reviewed 2026-11-24. Check the installed Next.js version before using newer conventions.

Next.js supplies filesystem routing: app for App Router, pages for Pages Router, optionally under src. Arctis supplies feat/shared ownership; these names and the thin-entrypoint restriction are not Next.js requirements.

## Layout

```text
src/                         # optional; all three roots can also be at project root
  app/                       # or pages/ for Pages Router
    layout.tsx
    page.tsx
    (account)/
      settings/page.tsx
    api/status/route.ts
  feat/
    account/
      components/
      services/
  shared/
    components/
    styles/
public/                      # project root, not src/public
next.config.ts               # project root; extension follows installed version
package.json
tsconfig.json
tests/
```

The example is App Router. Pages Router uses native index/page files, _app, _document, and API routes as applicable. Do not create an Arctis route.tsx registry in either Next.js router. App Router route.ts is an HTTP handler, not a React route aggregator. Keep it thin and validate/authenticate incoming requests; feature-owned server code implements behavior. An external backend remains a separate application.

Retain native route groups, dynamic segments, parallel slots, intercepting routes, layouts, loading/error boundaries, metadata, and static assets. Layouts receive children/slots from the framework rather than importing page entrypoints. Arctis forbids route-entrypoint-to-entrypoint source imports; reusable layouts/UI belong in feat or shared. Framework composition is not a source-import violation.

Next.js permits colocation, but Arctis keeps implementation outside app/pages. Private folders are not a shortcut for feature logic. Native CSS/assets can remain where required; prefer shared/styles for reusable global styles. Do not relocate public, config files, generated .next output, or version-specific proxy/middleware/instrumentation entrypoints into features merely to satisfy a three-folder rule.

## Runtime boundaries

Pages/layouts in App Router default to server execution; use a narrow client boundary for interactive UI. Feature isolation applies on both sides. Keep secrets and server dependencies out of client import graphs, use server-only guards where appropriate, and pass serializable data across server/client boundaries. Do not mark an entire feature client-side just to avoid designing that boundary. Server actions and route handlers must enforce their own authorization and input validation.

Non-code asset/style imports are reported for framework build validation rather than resolved as TypeScript modules. The architecture validator does not prove runtime separation, supported exports, route uniqueness, or correct metadata. Run Next.js build/type checks and review client/server imports separately.

## Validation profiles

```sh
node path/to/frontend/scripts/validate-architecture.mjs /path/to/app --framework next-app
node path/to/frontend/scripts/validate-architecture.mjs /path/to/app --framework next-pages
```

Default profile is spa for compatibility. Select explicitly based on the actual router, not merely the presence of React or Vite. Both Next profiles support src-based or project-root-based app/pages + feat/shared. At project root, the checker scans these three trees and native proxy/middleware/instrumentation entrypoints; config/public/tooling directories are outside its source scan. Root configuration must not be imported by feature/shared/route modules.

Mixed App/Pages Router migrations and duplicate root/src routing trees are not covered by these profiles: the checker fails rather than reporting complete coverage. Plan scoped manual review for such migrations; do not move routes or remove checks solely to obtain a pass. Custom pageExtensions and future special filenames require review and an explicit validator update before claiming support. Tests remain in project-root tests, outside application source trees.
