# Contributing

This guide is for people who change the plugin. If you only want to use it, see the [plugin README](plugins/carbonaltdelete-skills/README.md). Coding agents working in this repository also follow [AGENTS.md](AGENTS.md).

## Repository layout

The plugin lives in `plugins/carbonaltdelete-skills/`; the repository root holds the marketplaces, the development tooling, and maintainer docs. Claude Code, Codex, and other Agent Plugin clients each read their own manifest and ignore the others' files.

Paths in the plugin folder, `plugins/carbonaltdelete-skills/`:

| Path | Purpose | Read by |
| --- | --- | --- |
| `.claude-plugin/plugin.json` | Plugin manifest with the Claude plugin directory listing fields | Claude |
| `.mcp.json` | Carbon+Alt+Delete MCP server, `http` transport | Claude |
| `plugin.json` and `mcp.json` | Portable [Agent Plugins](https://agent-plugins.org) manifest and MCP server, `streamable-http` transport. The OpenAI listing fields sit under `extensions.com.openai` | Codex and other Agent Plugin clients |
| `skills/<name>/SKILL.md` | Portable skills | All clients |
| `agents/<name>.md` | Subagents | Clients that support plugin subagents, such as Cowork and Claude Code |
| `assets/` | Plugin directory icon | Claude |
| `README.md` | User documentation and the Claude plugin directory listing | Users and the Claude plugin directory |
| `LICENSE` | License that ships with the plugin; keep it identical to the root `LICENSE` | Users and the Claude plugin directory |

Paths at the repository root:

| Path | Purpose | Read by |
| --- | --- | --- |
| `.claude-plugin/marketplace.json` | Marketplace named `carbonaltdelete` that lists the plugin folder | claude.ai, Cowork, and Claude Code |
| `.agents/plugins/marketplace.json` | Marketplace named `carbonaltdelete` that lists the plugin folder | Codex |
| `.codex/config.toml` | Enables the plugin when you run Codex in this checkout | Codex, development only |
| `mise.toml`, `lefthook.yml`, `biome.json`, `.taplo.toml`, `.markdownlint-cli2.yaml` | Development tooling and linters | Maintainers |
| `docs/` | Maintainer references: MCP tool catalog, response guidelines, commit guidelines | Maintainers |

Every file in the plugin folder ships with the plugin, so keep internal notes, credentials, and large files out of it. The Claude plugin directory reads and scans only that folder, and from organizations that aren't Verified it accepts only Markdown, plain text, JSON, YAML, and PNG, JPEG, GIF, WebP, or ICO images there. Keep TOML, scripts, package manager configuration, and other development files at the repository root.

The plugin README is also the Claude plugin directory listing: keep it written for users and keep its [data and network access](plugins/carbonaltdelete-skills/README.md#data-and-network-access) section accurate whenever a skill, agent, or server changes what is sent where. Link from it to files outside the plugin folder with absolute GitHub URLs, because the listing shows only the plugin folder.

## Add a skill

Place each skill in an immediate child directory of `plugins/carbonaltdelete-skills/skills/` with a `SKILL.md` file. The YAML front-matter `name` must match the kebab-case directory name and include a concise `description`. The portable plugin format does not recursively discover nested skill folders; identify the applicable app module in the skill title and description.

```text
plugins/carbonaltdelete-skills/skills/
  custom-fields/
    SKILL.md
```

Add the skill to the table in the plugin README's [What's included](plugins/carbonaltdelete-skills/README.md#whats-included) section.

## Skills and subagents

Skills are the one component every client loads: claude.ai chat, Cowork, Claude Code, Codex, and other Agent Plugin clients. Every reusable agent workflow therefore needs a portable skill in the plugin's `skills/`. A richer subagent may also live in the plugin's `agents/`, as CADdy and the project diarist do. Where a client loads plugin subagents, as Cowork and Claude Code do, CADdy can delegate verified diary updates to the Project Diarist subagent; clients without subagent support, such as Codex, use the matching skills instead. Don't make a workflow depend on a subagent. Keep a skill and its subagent in sync in the same change, and record any deliberate client-specific difference in both files.

## Action transparency

All bundled skills and subagents give a concise, user-visible log after a material action, such as a Carbon+Alt+Delete MCP operation, a record update, a delegated task, a diary update, or a Support search. The log names the agent, the verified result, and the next action without exposing private reasoning or confidential data. [docs/agent-response-guidelines.md](docs/agent-response-guidelines.md) is the canonical policy; embed it in every user-facing skill and subagent.

## Manifests and versions

When the plugin's name, description, version, or links change, update both `plugin.json` and `.claude-plugin/plugin.json` in the plugin folder. Each client's listing fields live in its own place:

- **Claude:** `displayName`, `icon`, and the documentation, support, privacy, and terms URLs in `.claude-plugin/plugin.json`.
- **OpenAI (Codex and ChatGPT):** `extensions.com.openai.interface` in the plugin folder's `plugin.json`, which accepts only the Agent Plugins fields at its top level. Don't add a `.codex-plugin/plugin.json`; Codex ignores it while `extensions.com.openai` is set.

Keep the shared facts the same in both: display name, icon, URLs, and the description of what the plugin does. The marketplace files don't set a version; clients read it from the plugin manifests.

## Test locally

Load the checkout in Claude Code without installing it:

```sh
claude --plugin-dir /path/to/carbonaltdelete-skills/plugins/carbonaltdelete-skills
```

From the repository root, validate the plugin folder against the Claude plugin directory rules and test the marketplace from the local path:

```sh
claude plugin validate ./plugins/carbonaltdelete-skills --profile directory --strict
claude plugin validate .claude-plugin/marketplace.json --strict
claude plugin marketplace add /path/to/carbonaltdelete-skills
claude plugin install carbonaltdelete-skills@carbonaltdelete
claude plugin details carbonaltdelete-skills@carbonaltdelete
```

To test in claude.ai and Cowork, zip `plugins/carbonaltdelete-skills` and upload it from **Customize > Plugins > Add > Upload plugin**. Ask Claude which skills it has from plugins, connect the server from the plugin's **Connectors** tab, and start a Cowork task to check that the subagents load.

In Codex, pass the checkout's absolute path to `codex plugin marketplace add`.

## Submit to the Claude plugin directory

In the developer portal at [claude.ai/directory/manage](https://claude.ai/directory/manage), submit the plugin folder, not the repository root: enter `CarbonAltDelete/carbonaltdelete-skills` as the repository and `plugins/carbonaltdelete-skills` as the plugin path, or paste `https://github.com/CarbonAltDelete/carbonaltdelete-skills/tree/main/plugins/carbonaltdelete-skills`.

## Development checks

Install the pinned development tools with `mise install`, then register the Git hooks with `lefthook install`. Run every validation locally with `mise run lint`.

Lefthook checks staged Markdown, JSON, and TOML files before each commit. Markdownlint CLI 2 handles Markdown, Biome validates and format-checks JSON, and Taplo validates and format-checks TOML.

## Commits

Follow [docs/commit-guidelines.md](docs/commit-guidelines.md).
