# React adapter

Official sources, reviewed 2026-11-24: [React app setup](https://react.dev/learn/build-a-react-app-from-scratch) and [React Router data routing](https://reactrouter.com/start/data/routing). React does not prescribe the Arctis pages/feat/shared taxonomy. Vite is build tooling, and Tauri is a host; neither identifies the router or component framework.

For an existing React SPA with explicit route registration, use [React structure](react-structure.md) and [React routing](react-routing.md). The bundled `spa` validator checks this specific profile. Preserve its strict page registration exception, isolated features, shared primitives, and tests outside src.

For Next.js, load [Next.js](nextjs.md) instead. For another React framework or generated router, use [other frameworks](other.md): inspect native routing conventions before selecting a profile. React Router framework mode and TanStack file-based routing are not covered by the SPA validator. Do not claim that passing the SPA checker establishes compatibility with them or rename native route files to force a pass.

Validate using the matching architecture profile plus the project's compiler, lint, build, and relevant behavioral tests. Do not create route infrastructure for a component library or embedded widget that has no routes; document that scope and review its feature/shared boundaries without forcing the SPA application layout.
