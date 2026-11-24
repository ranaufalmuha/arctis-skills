# Arctis Architecture


**Structure your code. Enforce your rules.**

Arctis is a plugin for Codex and Claude Code that applies consistent frontend, backend, and game architecture across projects. It includes four skills: `frontend`, `backend`, `game`, and `monorepo`.

| Skill | Use it for |
| --- | --- |
| `frontend` | Framework-aware React, Next.js, Vue, Nuxt, SvelteKit, and Angular architecture; Vite/Tauri use the hosted framework adapter |
| `backend` | TypeScript backend implementation, refactoring, and review, especially Hono APIs with explicit module and layer boundaries |
| `game` | Game implementation, refactoring, and review with Unity, Godot, Unreal Engine, Bevy, or an explicit mapping for another engine |
| `monorepo` | Coordination of architecture work across applications and shared packages |

## Installation

Choose the commands for your agent. Both install the same `frontend`, `backend`, `game`, and `monorepo` skills. To install only one skill, use manual installation below.

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

Inside a Codex session, the equivalent commands are `/plugin marketplace add ranaufalmuha/arctis-skills` and `/plugin install arctis@arctis-skills`. Start a new session, then use `/skills` to check the installation or ask it to use the `frontend`, `backend`, `game`, or `monorepo` skill for the relevant task. Choose the `user` or `project` scope during installation as appropriate.

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
cp -R skills/architecture/frontend skills/architecture/backend skills/architecture/game skills/architecture/monorepo \
  /path/to/project/.claude/skills/
```

### Claude Code (user scope)

```sh
mkdir -p ~/.claude/skills
cp -R skills/architecture/frontend skills/architecture/backend skills/architecture/game skills/architecture/monorepo \
  ~/.claude/skills/
```

After copying, restart Claude Code and run `/skills` to verify the skills loaded.

### Codex (project scope)

Codex uses `.agents/skills/` instead of `.claude/skills/`:

```sh
mkdir -p /path/to/project/.agents/skills
cp -R skills/architecture/frontend skills/architecture/backend skills/architecture/game skills/architecture/monorepo \
  /path/to/project/.agents/skills/
```

For user scope, copy the same directories into `~/.agents/skills/`. See the [Codex skills documentation](https://developers.openai.com/codex/skills) for supported locations.

## Architecture validators

The `frontend` and `backend` skills include optional static architecture validators for local checks and CI. They require Node.js 20 or newer and their pinned TypeScript dependency. After copying a skill manually, or when its installed plugin path is known, follow its enforcement guide to install dependencies and run it against each supported application (frontend coverage is React SPA/Next.js only):

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
│   ├── game/                              # Skill and engine-specific references
│   └── monorepo/SKILL.md
├── templates/AGENTS.md
├── AGENTS.md
└── README.md
```

## What each skill enforces

- **Frontend (SPA profile)**: every `src/` contains exactly `pages/`, `feat/`, and `shared/`. Pages compose, features own behavior, and shared stays feature-agnostic. Dependencies flow `pages → feat → shared` with no cross-feature, reverse, or circular imports.
- **Backend**: every module owns `api/`, `schemas/`, `services/`, and `repositories/`. Dependencies flow inward to `db/generated`; cross-module imports, reverse-layer imports, direct persistence access from API/services, and circular dependencies fail validation.
- **Game**: composition connects, features own gameplay and state, and shared provides primitives. Map those roles onto native engine layouts; declared public cross-feature contracts are allowed, internal access and cyclic module dependencies are not.
- **Monorepo**: coordinate the frontend, backend, and game skills per application without prescribing a monorepo tool or directory layout.

Each skill is independently usable. The monorepo skill coordinates them; it does not replace the domain skills.

## Frontend framework adapters

Frontend mirrors the game skill's engine-adapter approach: one common ownership policy with [framework-specific mappings](skills/architecture/frontend/references/structure.md). Official conventions come first; adapters distinguish Arctis choices from framework requirements. Vite is tooling, not a UI framework. Read only the selected framework's reference.

The bundled validator covers React SPA and Next.js profiles. Vue, Nuxt, SvelteKit, and Angular currently have documented mappings and native-tool/manual-review workflows, not executable architecture validators. Test placement follows each adapter; Angular allows colocated tests.

## Game architecture

The `game` skill is one independently installable folder. Read its [entrypoint](skills/architecture/game/SKILL.md), [common structure](skills/architecture/game/references/structure.md), and only the matching adapter: [Unity](skills/architecture/game/references/engines/unity.md), [Godot](skills/architecture/game/references/engines/godot.md), [Unreal Engine](skills/architecture/game/references/engines/unreal.md), [Bevy](skills/architecture/game/references/engines/bevy.md), or [other engines](skills/architecture/game/references/engines/other.md).

The references cover dependency/state ownership, lifecycle, assets, multiplayer, testing, migration, and enforcement. Engine adapters record official sources and distinguish native guidance from Arctis choices. Where reviewed official guidance leaves folder taxonomy open, use a Unity-like asset-type fallback within native roots. Engine trees are concrete placement guides, not application scaffolds: create only occupied folders and preserve an existing equivalent layout. These are documented mappings, not runtime-tested engine integrations. See [validation scope](skills/architecture/game/references/enforcement.md).

To install **only game** manually for Codex in a target project:

```sh
mkdir -p /path/to/project/.agents/skills
cp -R skills/architecture/game /path/to/project/.agents/skills/
```

For Claude Code use `.claude/skills/` instead. To install only frontend, copy `skills/architecture/frontend` instead of `game`, including its complete folder. Full plugin installation currently includes all four skills; there is no separate game plugin in this repository yet. Game references are loaded when relevant to a game task, not as frontend rules.

## AGENTS.md

In a target repository, adapt [`templates/AGENTS.md`](templates/AGENTS.md) into its root `AGENTS.md`. It carries the essential constraints even when a skill is not automatically selected. For an existing codebase, ask the agent to assess current structure and import violations before making broad moves.

## Philosophy

The rules make ownership and dependency direction visible so code stays easy to audit and maintain. Each domain skill is independently usable. Frontend and backend have executable validators; game uses explicit ownership rules and native engine checks, with no bundled executable validator.

## Enforcement and extension

Skills and `AGENTS.md` guide agent behavior. Frontend and backend skills include executable validators for structural and resolved import boundaries; both reject `*.test.*` and `*.spec.*` files under `src/`. They run only when a project invokes them locally or in CI. Keep the project's lint, type-check, build, and test tooling alongside them.

To extend the collection, add a focused skill directory with a spec-compliant `SKILL.md` under the relevant category, keep its entrypoint concise, and place detailed rules in linked references. Preserve established constraints unless a new architecture decision explicitly changes them.
