# Architecture guidance for AI agents

Use the `frontend` skill for implementation, refactoring, or review of frontend files. Use the `backend` skill for backend files. For work spanning applications or shared packages in a monorepo, also use the `monorepo` skill to coordinate the work. If a required skill is unavailable, report that and use the constraints below; do not claim the skill was loaded.

In a monorepo, apply the frontend rules independently to each frontend application (for example, `apps/app/`, `apps/admin/`, or `apps/web/`) and backend principles independently to each backend application (for example, `apps/api/`). Determine an application's role from its code, not its directory name alone.

In every application, keep `*.test.*` and `*.spec.*` files out of `src/`; tests belong in that application's `tests/` tree.

## Non-negotiable frontend constraints

- Each frontend application's `src/` has exactly three architectural directories: `pages/`, `feat/`, and `shared/`. Minimal bootstrap files and declarations may sit directly under `src/`; application logic belongs inside those directories. Do not add other architectural roots such as `src/components/`, `src/hooks/`, `src/utils/`, or `src/services/`.
- `pages/` owns routing and thin page composition. Ordinary pages do not import other pages or route files. A `route.tsx` may import pages in its own directory and immediate child `route.tsx` files only for registration. The root `pages/route.tsx` aggregates routes; each route group has its own `route.tsx`. Do not add implementation directories inside `pages/`.
- Each `feat/<name>/` is isolated: it may import itself and `shared/`, never another feature or `pages/`.
- `shared/` may import `shared/` and external packages, never `feat/` or `pages/`. Keep business features out of `shared/`.
- No circular dependencies. Aliases, barrels, type-only imports, and dynamic imports do not bypass boundaries. Check resolved module ownership. Do not silently disable or bypass architecture validation.

## Non-negotiable backend constraints

- Every backend module lives in `src/modules/<name>/` and has `api/`, `schemas/`, `services/`, and `repositories/`.
- Dependencies flow `api → schemas/services → repositories → db/generated`. API handlers stay thin; Zod validates external input; services own business rules; repositories own Prisma queries.
- Modules never import each other. `shared/` never imports a module. `api` and `services` never import `db/` or call Prisma directly. Lower layers never import upper layers.
- Aliases, type-only imports, re-exports, barrels, dynamic imports, and `require` do not bypass boundaries. No circular dependencies.
- Validate external input at trust boundaries, map errors to the shared HTTP envelope, and never expose secrets or internal errors.
- Never edit generated Prisma output. Use reviewed migrations, never `db push` on shared databases, and never reset a database containing data without explicit approval.

Before completing work, run the installed validator against each affected application: `frontend/scripts/validate-architecture.mjs` for frontends and `backend/scripts/validate-architecture.mjs` for backends. Then run the application's lint, typecheck, build, and relevant test commands. Follow each installed skill's `references/enforcement.md` for dependency setup and CI use. If a skill or validator is unavailable, report that limitation and still review the mandatory constraints above. Report commands run, unresolved violations, and limits; never claim an unrun check passed.
