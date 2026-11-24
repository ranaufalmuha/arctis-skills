# Working on Arctis Skills

This repository is the source of truth for Arctis Architecture Agent Skills. It contains instructions, not an application. `skills/architecture/` groups the independently installable `frontend`, `backend`, `game`, and `monorepo` skills; the category directory is not a skill.

- Keep every `skills/architecture/<name>/SKILL.md` compliant with the [Agent Skills specification](https://agentskills.io/specification), including valid YAML frontmatter and a matching immediate directory name.
- Do not weaken established architectural rules without explicit authorization. Keep the frontend dependency boundaries strict and the backend module/layer boundaries explicit.
- Keep frontend, backend, and game skills independently usable. The monorepo skill coordinates them when available without making either dependent on it.
- Keep skill entrypoints concise. Put detailed rules and examples in the relevant `references/` files, and link to them from the entrypoint.
- Avoid duplicating large sections of documentation. Keep entrypoints, references, validators, the reusable template, and README consistent.
- Do not add application scaffolding or dependencies unless genuinely needed for skill validation. The frontend and backend validators may pin TypeScript. Do not create empty category directories for future skills.
- Before completing changes, validate all modified skill files: frontmatter, declared names, relative links, consistency of rules, and validator fixture suites. Run an available Agent Skills validator when possible. Report what was actually checked.

- Game architecture maps composition/features/shared ownership onto native engine layouts. Keep engine details in the game references; do not apply web-specific folder or test rules to games. Do not claim executable game validation or runtime-tested engine support without evidence.
