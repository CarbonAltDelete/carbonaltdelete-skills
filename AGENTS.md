# Repository development guidance

This guidance is for agents developing or maintaining the contents of this repository. It does not apply to agents using the installed `carbonaltdelete-skills` plugin.

## Required guidelines

1. **Always lint locally before committing.** Run `mise run lint` after making changes and resolve any failures before creating a commit. If the change is limited to one file type, run its focused task first (`mise run lint:markdown`, `mise run lint:json`, or `mise run lint:toml`), then run the full lint task before committing.
2. Keep each skill in an immediate child directory of `skills/`: `skills/<kebab-case-name>/SKILL.md` (for example, `skills/custom-fields/SKILL.md`). The YAML `name` must match the kebab-case directory name and every skill needs a concise `description`. The portable Agent Plugin format does not recursively discover nested skill folders; identify the applicable Carbon+Alt+Delete app module in the skill title and description instead.
3. Follow the cross-client approach for agent-like behavior. Every reusable agent workflow must have a portable skill at `skills/<kebab-case-name>/SKILL.md` for Codex and other compatible clients. A richer Claude Code subagent may additionally live at `agents/<kebab-case-name>.md`. Keep the shared role, workflow, safety rules, deliverables, response format, and action-transparency policy in sync in the same changeset; document any deliberate client-specific difference in the relevant files and [`CONTRIBUTING.md`](CONTRIBUTING.md). The canonical action-transparency policy is [`docs/agent-response-guidelines.md`](docs/agent-response-guidelines.md) and must be embedded in every user-facing skill and agent so it is available in every client.
4. Keep plugin metadata aligned. Update `plugin.json` for the general Agent Plugin package and `.claude-plugin/plugin.json` for Claude compatibility when package identity or version metadata changes. Claude directory listing fields belong only in `.claude-plugin/plugin.json`; OpenAI listing fields belong in `extensions.com.openai.interface` in the root `plugin.json`. Keep the facts they share consistent. The marketplace files (`.claude-plugin/marketplace.json` for Claude Code and `.agents/plugins/marketplace.json` for Codex) share the name `carbonaltdelete` and set no version.
5. Keep [`README.md`](README.md) written for plugin users; it is also the Claude plugin directory listing. Put contributor guidance in [`CONTRIBUTING.md`](CONTRIBUTING.md). Update the README's data and network access section whenever a change alters what the plugin sends where.
6. Never add credentials, tokens, or other secrets to this repository. Future MCP configuration must remain portable and credential-free.
7. Validate the change at the narrowest useful scope, then run the full local lint suite. Install pinned tools with `mise install` and enable the pre-commit hook with `lefthook install` when setting up a new checkout.
8. Follow [`docs/commit-guidelines.md`](docs/commit-guidelines.md) when preparing a commit message or creating a commit. Do not create, amend, or sign a commit unless the user explicitly requests it.

## Validation commands

```sh
mise run lint
mise run lint:markdown
mise run lint:json
mise run lint:toml
```
