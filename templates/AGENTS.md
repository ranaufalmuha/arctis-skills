# Architecture guidance for AI agents

Use the `frontend` skill for implementation, refactoring, or review of frontend files. Use the `backend` skill for backend files. For work spanning applications or shared packages in a monorepo, also use the `monorepo` skill to coordinate the work. If a required skill is unavailable, report that and use the constraints below; do not claim the skill was loaded.

In a monorepo, apply the frontend rules independently to each frontend application (for example, `apps/app/`, `apps/admin/`, or `apps/web/`) and backend principles independently to each backend application (for example, `apps/api/`). Determine an application's role from its code, not its directory name alone.

## Non-negotiable frontend constraints

- Each frontend application's `src/` has exactly three architectural directories: `pages/`, `feat/`, and `shared/`. Minimal bootstrap files and declarations may sit directly under `src/`; application logic belongs inside those directories. Do not add other architectural roots such as `src/components/`, `src/hooks/`, `src/utils/`, or `src/services/`.
- `pages/` owns routing and thin page composition. Ordinary pages do not import other pages or route files. A `route.tsx` may import pages in its own directory and immediate child `route.tsx` files only for registration. The root `pages/route.tsx` aggregates routes; each route group has its own `route.tsx`. Do not add implementation directories inside `pages/`.
- Each `feat/<name>/` is isolated: it may import itself and `shared/`, never another feature or `pages/`.
- `shared/` may import `shared/` and external packages, never `feat/` or `pages/`. Keep business features out of `shared/`.
- No circular dependencies. Aliases, barrels, type-only imports, and dynamic imports do not bypass boundaries. Check resolved module ownership. Do not silently disable or bypass architecture validation.

## Backend principles

Organize by domain ownership. Keep HTTP handlers light; separate transport, validation, business logic, and data access. Validate external input at trust boundaries. Avoid cycles and unnecessary cross-module coupling, centralize only genuinely reusable infrastructure, and use explicit dependencies. No fixed backend directory layout has been approved.

Before completing frontend work, run the installed `frontend/scripts/validate-architecture.mjs` against the frontend application root, plus the application's lint, typecheck, and relevant test commands. Follow the installed skill's `references/enforcement.md` for dependency setup and CI use. If the skill or validator is unavailable, report that limitation and still review the mandatory constraints above. Report commands run, unresolved violations, and limits; never claim an unrun check passed. Run available linting, type checking, and relevant tests for backend work as well.
