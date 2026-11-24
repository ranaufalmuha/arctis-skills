---
name: frontend
description: Apply Arctis Frontend Architecture when implementing, refactoring, or reviewing React, Next.js, Vue, Nuxt, SvelteKit, Angular, or other frontend applications. Map strict composition/features/shared ownership onto official framework conventions and select only supported architecture checks.
---

# Arctis Frontend Architecture

**Composition connects. Features implement. Shared provides primitives. Frameworks determine physical layout.** Apply independently to each frontend application.

## Before coding

1. Read the project's AGENTS.md and inspect framework/version, router mode, source roots, generated files, aliases, runtime boundaries, and validation commands. Vite is build tooling and Tauri is a host, not an architecture profile.
2. Use [framework selection](references/structure.md) to load the most specific adapter: [React](references/frameworks/react.md), [Next.js](references/frameworks/nextjs.md), [Vue](references/frameworks/vue.md), [Nuxt](references/frameworks/nuxt.md), [SvelteKit](references/frameworks/sveltekit.md), [Angular](references/frameworks/angular.md), or [other](references/frameworks/other.md).
3. Read [boundaries](references/boundaries.md) for dependency rules, [routing](references/routing.md) for composition, [testing](references/testing.md) for test conventions, and [enforcement](references/enforcement.md) before running checks.

## During coding

- Follow official framework conventions first. Adapters label Arctis-specific ownership mappings and defaults. Preserve an existing equivalent layout; do not force pages, app, routes, or src on every framework.
- Keep features isolated. Dependencies flow composition → features → shared, with composition → shared allowed. No cross-feature, reverse, or circular dependencies, including aliases, re-exports, templates, injection, and auto-imports.
- Keep route entrypoints thin. Native/generated routing replaces manual registration where the framework requires it. Use only the adapter's explicit composition exceptions; never import another ordinary page for reuse.
- Shared stays feature-independent. Create only needed files and directories; do not move domain behavior into shared to evade boundaries.
- Preserve server/client, public/static, configuration, and generated-code boundaries. Test placement follows the adapter; React/Next external-test rules are not universal.

## Before completion

Use supported executable profiles only; other adapters require native tooling and explicit manual architecture review. Run relevant available lint/type/build/tests. Report framework/version, selected adapter, checks run, violations, and coverage limits. Never claim a TypeScript scan validates unparsed framework files or that documented adapters were runtime-tested.
