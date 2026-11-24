# Routing by framework

Routing is composition. Framework files may register or compose feature entrypoints, but should not own feature business rules. Do not import one ordinary page into another; extract reusable UI into its owning feature or shared.

- React SPA explicit registration: follow [React routing](frameworks/react-routing.md), including the narrow same-group/immediate-child registration exception.
- Next.js, Nuxt, and SvelteKit: follow native filesystem routing and layout mechanisms in their adapters. Never add Arctis route.tsx registries.
- Vue Router and Angular: use their native route definitions and adapter ownership rules. Do not force TSX filenames.
- Generated router modes: inspect documented generation and source conventions; never hand-edit generated route trees.

Framework-driven layout nesting is not a source import cycle. API handlers, server loads/actions, middleware, and hooks remain thin runtime integration boundaries with authorization/validation appropriate to the request. They do not grant features permission to import routing internals or sibling features.

See [framework selection](structure.md) for native paths, sources, and validation coverage.
