# Carbon+Alt+Delete skills

This repository is a portable Agent Plugin package for reusable Carbon+Alt+Delete MCP skills and workflow guidance.

## Add a skill

Place each skill in an immediate child directory of `skills/` with a `SKILL.md` file. The YAML front-matter `name` must match the kebab-case directory name and include a concise `description`. The portable plugin format does not recursively discover nested skill folders; identify the applicable app module in the skill title and description.

```text
skills/
  custom-fields/
    SKILL.md
```

## MCP configuration

The portable Agent Plugin configuration in [`mcp.json`](mcp.json) connects to the Carbon+Alt+Delete streamable HTTP endpoint at `https://app.carbonaltdelete.eu/mcp`. The Claude Code plugin configuration in [`.mcp.json`](.mcp.json) provides the same server using Claude Code's `http` transport name.

The endpoint uses OAuth bearer authentication. No credentials or authorization headers are stored in this repository; clients that support OAuth discover the authorization server automatically. In Claude Code, enable the plugin and complete the sign-in from `/mcp` (or run `claude mcp login plugin:carbonaltdelete-skills:carbonaltdelete`).

## Use the plugin

### Codex

Add the repository as a Codex plugin marketplace, then install the plugin:

```sh
codex plugin marketplace add CarbonAltDelete/carbonaltdelete-skills
codex plugin add carbonaltdelete-skills@carbonaltdelete-local
```

Start or restart Codex, then authenticate the bundled MCP server in a browser:

```sh
codex mcp login carbonaltdelete
```

Verify that the plugin and its MCP server are available with `codex plugin list` and `codex mcp list`. If your organization manages Codex plugins or MCP servers, an administrator must allow this marketplace and `https://app.carbonaltdelete.eu/mcp` first.

To install from a local checkout instead, pass its absolute path to `codex plugin marketplace add`.

### Claude Code

Add the repository as a Claude Code plugin marketplace and install the package:

```text
/plugin marketplace add CarbonAltDelete/carbonaltdelete-skills
/plugin install carbonaltdelete-skills@carbonaltdelete-local
/reload-plugins
```

Open `/mcp`, select the Carbon+Alt+Delete server, and complete the browser-based OAuth sign-in. You can also authenticate from a terminal with:

```sh
claude mcp login plugin:carbonaltdelete-skills:carbonaltdelete
```

Use `/mcp` to confirm the server is connected before asking Claude to use Carbon+Alt+Delete. For a local checkout, start Claude Code with `claude --plugin-dir /path/to/carbonaltdelete-skills`.

## Support

When the plugin's skills, agents, or MCP tools do not provide enough guidance, use [Carbon+Alt+Delete Support](https://support.carbonaltdelete.eu) and search for the relevant product area, feature, or action. The bundled `carbonaltdelete-support` skill instructs compatible agents to use this official support fallback and avoid guessing.

## CADdy consultant companion

This package includes **CADdy**, a corporate carbon-footprint consultant companion, and a **Project Diarist** that maintains `project-diary.md` with an action summary plus key decisions and assumptions. The portable [`skills/caddy/SKILL.md`](skills/caddy/SKILL.md) and [`skills/project-diarist/SKILL.md`](skills/project-diarist/SKILL.md) workflows are available to Codex and other compatible clients.

Claude Code additionally loads the richer [`agents/caddy.md`](agents/caddy.md) and [`agents/project-diarist.md`](agents/project-diarist.md) subagents, allowing CADdy to delegate verified diary updates directly. The `agents/` directory is a Claude Code plugin extension; other compatible clients use the matching portable skills and MCP configuration.

## Action transparency

All bundled skills and Claude subagents give a concise, user-visible log after they complete a material action, such as a Carbon+Alt+Delete MCP operation, a record update, a delegated task, a diary update, or a Support search. The log identifies the agent, verified result, and next action without exposing private reasoning or confidential data. See [`docs/agent-response-guidelines.md`](docs/agent-response-guidelines.md) for the maintained policy.

## Development checks

Install the pinned development tools with `mise install`, then register the Git hooks with `lefthook install`. Run every validation locally with `mise run lint`.

Lefthook checks staged Markdown, JSON, and TOML files before each commit. Markdownlint CLI 2 handles Markdown, Biome validates and format-checks JSON, and Taplo validates and format-checks TOML.
