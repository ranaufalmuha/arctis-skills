# Working on Arctis Skills

This repository is the source of truth for Arctis Architecture Agent Skills. It contains instructions, not an application. `skills/architecture/` groups the independently installable `frontend`, `backend`, and `monorepo` skills; the category directory is not a skill.

- Keep every `skills/architecture/<name>/SKILL.md` compliant with the [Agent Skills specification](https://agentskills.io/specification), including valid YAML frontmatter and a matching immediate directory name.
- Do not weaken established architectural rules without explicit authorization. In particular, keep the frontend dependency boundaries strict and the backend directory layout provisional.
- Keep frontend and backend skills independently usable. The monorepo skill coordinates them when available without making either dependent on it.
- Keep skill entrypoints concise. Put detailed rules and examples in the relevant `references/` files, and link to them from the entrypoint.
- Avoid duplicating large sections of documentation. Keep entrypoints, references, the reusable template, and README consistent.
- Do not add application scaffolding, package managers, or dependencies unless they are genuinely needed for skill validation. Do not create empty category directories for future skills.
- Before completing changes, validate all modified skill files: frontmatter, declared names, relative links, and consistency of rules. Run an available Agent Skills validator when possible. Report what was actually checked.
