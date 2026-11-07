# Arctis Skills

**Structure your code. Enforce your rules.**

Arctis Skills is a reusable collection of [Agent Skills](https://agentskills.io/specification) for engineering standards across projects, including Antigane projects. Its architecture category contains Arctis Architecture. This is an instruction repository, not an application starter.

## Philosophy

The rules make ownership and dependency direction visible so code stays easy to audit and maintain. The frontend standard is deliberately strict: pages compose, features own behavior, and shared code remains feature-agnostic. Backend guidance establishes clear responsibilities while leaving its directory layout open for future decisions. The monorepo skill coordinates these standards across applications without prescribing a repository layout or toolchain.

## Skills

| Skill | Use it for |
| --- | --- |
| [`frontend`](skills/architecture/frontend/SKILL.md) | React, Vite, TypeScript, and Tauri frontend implementation, refactoring, and review |
| [`backend`](skills/architecture/backend/SKILL.md) | TypeScript backend implementation, refactoring, and review, especially Hono APIs |
| [`monorepo`](skills/architecture/monorepo/SKILL.md) | Coordination of architecture work across applications and shared packages |

```text
arctis-skills/
├── skills/
│   └── architecture/
│       ├── frontend/
│       │   ├── SKILL.md
│       │   ├── package.json
│       │   ├── package-lock.json
│       │   ├── references/
│       │   │   ├── structure.md
│       │   │   ├── routing.md
│       │   │   ├── boundaries.md
│       │   │   └── enforcement.md
│       │   ├── scripts/
│       │   │   └── validate-architecture.mjs
│       │   └── tests/
│       │       └── validate-architecture.test.mjs
│       ├── backend/
│       │   ├── SKILL.md
│       │   └── references/
│       │       ├── structure.md
│       │       └── boundaries.md
│       └── monorepo/
│           └── SKILL.md
├── templates/
│   └── AGENTS.md
├── AGENTS.md
├── README.md
└── .gitignore
```

## Install and use

Copy **each entire leaf skill directory**, including its `references/` folder and any validation assets, to a skill location supported by your AI coding agent. `skills/architecture/` is an organizational category, not an installable skill. The Agent Skills specification defines the skill format; discovery and installation paths depend on the agent. See your agent's documentation before choosing a location.

For **Codex**, the [official skill documentation](https://learn.chatgpt.com/docs/build-skills) lists `.agents/skills/` at a repository root for project-level skills and `~/.agents/skills/` for user-level skills. For example, from this repository on a Unix-like shell:

```sh
mkdir -p /path/to/project/.agents/skills
cp -R skills/architecture/frontend skills/architecture/backend skills/architecture/monorepo /path/to/project/.agents/skills/
```

The frontend validator requires Node.js 20 or newer. In the target repository, install its pinned validation dependency with `npm ci --prefix .agents/skills/frontend`, then run `node .agents/skills/frontend/scripts/validate-architecture.mjs /path/to/frontend-app`. See the frontend [enforcement guide](skills/architecture/frontend/references/enforcement.md) for monorepo and CI use.

For a user-level Codex installation, copy the same leaf skill directories into `~/.agents/skills/` instead. These are Codex paths, not universal Agent Skills paths. Project-level installation keeps a team's rules with its repository; user-level installation makes them available across your projects. Consult the [Codex documentation](https://learn.chatgpt.com/docs/build-skills) if your setup differs.

In a target repository, adapt [`templates/AGENTS.md`](templates/AGENTS.md) into its root `AGENTS.md`. It carries essential constraints even when a skill is not automatically selected. For an existing codebase, first identify its frontend and backend applications, install the relevant skill directories, and ask the agent to assess current structure and import violations before making broad moves. Apply frontend rules separately to each frontend app in a monorepo.

Compatible agents may select a skill from its description or allow explicit invocation. Ask the agent to use `frontend`, `backend`, or `monorepo` for the relevant work, and verify that the skill is available in that agent's environment. Frontend and backend remain independently usable.

## Enforcement and extension

Skills and `AGENTS.md` guide agent behavior. The frontend skill also includes an executable validator for structural and resolved import boundaries; it runs only when a project invokes it locally or in CI. Keep the project's lint, type-check, and test tooling alongside it. Semantic ownership and maintainability still require review. Agents must run available checks and report violations rather than silently bypassing them.

To extend the collection, add a focused skill directory with a spec-compliant `SKILL.md` under the relevant category, keep its entrypoint concise, and place detailed rules in linked references. `testing/`, `security/`, and `deployment/` are possible future categories; create them only when they contain a real skill. Update the README and reusable template when a new skill changes their guidance. Preserve established constraints unless a new architecture decision explicitly changes them, and validate frontmatter, links, and consistency before sharing.
