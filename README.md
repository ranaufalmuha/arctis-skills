# Arctis Architecture

**Structure your code. Enforce your rules.**

Arctis is a plugin for Codex and Claude Code that applies consistent frontend and backend architecture across projects. It includes three skills: `frontend`, `backend`, and `monorepo`.

| Skill | Use it for |
| --- | --- |
| `frontend` | React, Vite, TypeScript, and Tauri frontend implementation, refactoring, and review |
| `backend` | TypeScript backend implementation, refactoring, and review, especially Hono APIs with explicit module and layer boundaries |
| `monorepo` | Coordination of architecture work across applications and shared packages |

## Installation

Choose the commands for your agent. Both install the same `frontend`, `backend`, and `monorepo` skills.

### Claude Code

Run these commands in your terminal:

```sh
claude plugin marketplace add ranaufalmuha/arctis-skills
claude plugin install arctis@arctis-skills
```

Inside a Claude Code session, the equivalent commands are `/plugin marketplace add ranaufalmuha/arctis-skills` and `/plugin install arctis@arctis-skills`. Start a new session, then use `/skills` to check the installation or `/arctis:frontend` to invoke a skill. Choose the `user` or `project` scope during installation as appropriate.

### Codex

Run these commands in your terminal:

```sh
codex plugin marketplace add ranaufalmuha/arctis-skills
codex plugin install arctis@arctis-skills
```

Inside a Codex session, the equivalent commands are `/plugin marketplace add ranaufalmuha/arctis-skills` and `/plugin install arctis@arctis-skills`. Start a new session, then use `/skills` to check the installation or ask it to use the `frontend`, `backend`, or `monorepo` skill for the relevant task. Choose the `user` or `project` scope during installation as appropriate.

### Codex

Run these commands in your terminal:

```sh
codex plugin marketplace add ranaufalmuha/arctis-skills
codex plugin add arctis@arctis-skills
```

## Manual installation

To use the skills without a plugin, copy each entire leaf skill directory, including its references and validator assets, into your agent's skill directory.

### Claude Code (project scope)

```sh
mkdir -p /path/to/project/.claude/skills
cp -R skills/architecture/frontend skills/architecture/backend skills/architecture/monorepo \
  /path/to/project/.claude/skills/
```

### Claude Code (user scope)

```sh
mkdir -p ~/.claude/skills
cp -R skills/architecture/frontend skills/architecture/backend skills/architecture/monorepo \
  ~/.claude/skills/
```

After copying, restart Claude Code and run `/skills` to verify the skills loaded.

### Codex (project scope)

Codex uses `.agents/skills/` instead of `.claude/skills/`:

```sh
mkdir -p /path/to/project/.agents/skills
cp -R skills/architecture/frontend skills/architecture/backend skills/architecture/monorepo \
  /path/to/project/.agents/skills/
```

For user scope, copy the same directories into `~/.agents/skills/`. See the [Codex skills documentation](https://developers.openai.com/codex/skills) for supported locations.

## Architecture validators

The `frontend` and `backend` skills include optional static architecture validators for local checks and CI. They require Node.js 20 or newer and their pinned TypeScript dependency. After copying a skill manually, or when its installed plugin path is known, follow its enforcement guide to install dependencies and run it against each application:

- [Frontend enforcement](skills/architecture/frontend/references/enforcement.md)
- [Backend enforcement](skills/architecture/backend/references/enforcement.md)

## Repository layout

```text
arctis-skills/
├── .agents/plugins/marketplace.json      # Codex marketplace
├── .claude-plugin/                        # Claude Code plugin and marketplace
├── .codex-plugin/plugin.json              # Codex plugin
├── skills/architecture/
│   ├── frontend/                          # Skill, references, validator, tests
│   ├── backend/                           # Skill, references, validator, tests
│   └── monorepo/SKILL.md
├── templates/AGENTS.md
├── AGENTS.md
└── README.md
```

## What each skill enforces

- **Frontend**: every `src/` contains exactly `pages/`, `feat/`, and `shared/`. Pages compose, features own behavior, and shared stays feature-agnostic. Dependencies flow `pages → feat → shared` with no cross-feature, reverse, or circular imports.
- **Backend**: every module owns `api/`, `schemas/`, `services/`, and `repositories/`. Dependencies flow inward to `db/generated`; cross-module imports, reverse-layer imports, direct persistence access from API/services, and circular dependencies fail validation.
- **Monorepo**: coordinate the frontend and backend skills per application without prescribing a monorepo tool or directory layout.

Each skill is independently usable. The monorepo skill coordinates them; it does not replace either.

## AGENTS.md

In a target repository, adapt [`templates/AGENTS.md`](templates/AGENTS.md) into its root `AGENTS.md`. It carries the essential constraints even when a skill is not automatically selected. For an existing codebase, ask the agent to assess current structure and import violations before making broad moves.

## Philosophy

The rules make ownership and dependency direction visible so code stays easy to audit and maintain. Frontend and backend standards are both explicit; each skill is independently usable and has its own validator.

## Enforcement and extension

Skills and `AGENTS.md` guide agent behavior. Frontend and backend skills include executable validators for structural and resolved import boundaries; both reject `*.test.*` and `*.spec.*` files under `src/`. They run only when a project invokes them locally or in CI. Keep the project's lint, type-check, build, and test tooling alongside them.

To extend the collection, add a focused skill directory with a spec-compliant `SKILL.md` under the relevant category, keep its entrypoint concise, and place detailed rules in linked references. Preserve established constraints unless a new architecture decision explicitly changes them.
