# Architecture guidance for AI agents


Use the `frontend` skill for implementation, refactoring, or review of frontend files. Use the `backend` skill for backend files. Use the `game` skill for game projects and engine-based dedicated servers, including in-engine UI. For work spanning applications or shared packages in a monorepo, also use the `monorepo` skill to coordinate the work. If a required skill is unavailable, report that and use the constraints below; do not claim the skill was loaded.

In a monorepo, apply the frontend rules independently to each frontend application (for example, `apps/app/`, `apps/admin/`, or `apps/web/`) and backend principles independently to each backend application (for example, `apps/api/`). Determine an application's role from its code, not its directory name alone.

In React SPA, Next.js, and backend applications, keep `*.test.*` and `*.spec.*` files out of `src/`; tests belong in that application's `tests/` tree.

For games, use the engine-native folder and test conventions mapped by the game skill. A React shell and its embedded game runtime have distinct owners; do not force the runtime into web layers.

## Non-negotiable frontend constraints

- Select the frontend framework adapter before assigning folders. Follow native routing, assets, configuration, server/client boundaries, and test conventions; do not enforce one physical tree universally.
- Composition connects features; each feature is isolated and may depend only on itself and shared. Shared never depends on features or composition. No cross-feature, reverse, or circular dependencies through imports, templates, auto-imports, or injection.
- Keep entrypoints thin and shared feature-independent. Only the adapter's registration/composition exceptions permit route-level source imports. Never import another ordinary page for reuse.
- React SPA retains pages/feat/shared and hierarchical route.tsx registration. Next.js uses native app/pages routing. Other adapters map logical ownership onto their framework's official layout.
- Run bundled validation only for spa, next-app, or next-pages profiles. Other frameworks require native tooling and manual architecture review; report missing automatic coverage rather than claiming success.

## Non-negotiable backend constraints

- Every backend module lives in `src/modules/<name>/` and has `api/`, `schemas/`, `services/`, and `repositories/`.
- Dependencies flow `api → schemas/services → repositories → db/generated`. API handlers stay thin; Zod validates external input; services own business rules; repositories own Prisma queries.
- Modules never import each other. `shared/` never imports a module. `api` and `services` never import `db/` or call Prisma directly. Lower layers never import upper layers.
- Aliases, type-only imports, re-exports, barrels, dynamic imports, and `require` do not bypass boundaries. No circular dependencies.
- Validate external input at trust boundaries, map errors to the shared HTTP envelope, and never expose secrets or internal errors.
- Never edit generated Prisma output. Use reviewed migrations, never `db push` on shared databases, and never reset a database containing data without explicit approval.

## Non-negotiable game constraints

- Composition connects features; features own gameplay and state; shared contains feature-independent primitives. Map those roles onto native engine roots and existing equivalent layouts; create only occupied folders.
- Cross-feature access uses declared public contracts, never internals. Keep module dependencies acyclic. Shared never imports features or composition. This exception for declared game contracts does not change frontend/backend import rules.
- Declare authoritative state writers. Presentation observes state and submits commands; persistence/network adapters do not own gameplay rules. Composition wires public installation surfaces.
- Preserve engine asset identities, serialized references, and resource paths during moves. Use supported editor workflows for binary assets. Keep editor/test dependencies out of shipping runtime.
- Use the installed engine version and native test conventions. Follow the game skill's selected adapter and enforcement guide; no executable game architecture validator is bundled.

Before completing work, run the installed validator against each affected supported React SPA/Next.js or backend application: `frontend/scripts/validate-architecture.mjs` with the matching profile for React SPA/Next.js and `backend/scripts/validate-architecture.mjs` for backends. Then run the application's lint, typecheck, build, and relevant test commands. Follow each installed skill's `references/enforcement.md` for dependency setup and CI use. If a skill or validator is unavailable, report that limitation and still review the mandatory constraints above. Report commands run, unresolved violations, and limits; never claim an unrun check passed.

For game applications, run relevant available engine compile/import, tests, scene/asset checks, and target builds. Report the engine/version, actual results, unresolved issues, and checks not run. Never describe static review as a passed engine runtime check.
