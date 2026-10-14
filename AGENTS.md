# Repository development guidance

This guidance is for agents developing or maintaining the contents of this repository. It does not apply to agents using the installed `carbonaltdelete-skills` plugin.

## Required guidelines

1. **Always lint locally before committing.** Run `mise run lint` after making changes and resolve any failures before creating a commit. If the change is limited to one file type, run its focused task first (`mise run lint:markdown`, `mise run lint:json`, or `mise run lint:toml`), then run the full lint task before committing.
2. Keep skills in `skills/<kebab-case-name>/SKILL.md`. Each skill must start with YAML front matter containing a kebab-case `name` and a concise `description`.
3. Keep plugin metadata aligned. Update `plugin.json` for the general Agent Plugin package and `.claude-plugin/plugin.json` for Claude Code compatibility when package identity or version metadata changes.
4. Never add credentials, tokens, or other secrets to this repository. Future MCP configuration must remain portable and credential-free.
5. Validate the change at the narrowest useful scope, then run the full local lint suite. Install pinned tools with `mise install` and enable the pre-commit hook with `lefthook install` when setting up a new checkout.
6. Follow [`docs/commit-guidelines.md`](docs/commit-guidelines.md) when preparing a commit message or creating a commit. Do not create, amend, or sign a commit unless the user explicitly requests it.

## Validation commands

```sh
mise run lint
mise run lint:markdown
mise run lint:json
mise run lint:toml
```
