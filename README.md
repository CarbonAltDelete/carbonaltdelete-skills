# Carbon+Alt+Delete skills

This repository is a portable Agent Plugin package for reusable Carbon+Alt+Delete MCP skills and workflow guidance.

## Add a skill

Create a folder under `skills/` with a `SKILL.md` file. Each skill needs YAML front matter with a kebab-case `name` and a concise `description`, followed by its workflow instructions.

```text
skills/
  manage-projects/
    SKILL.md
```

## MCP configuration

The portable Agent Plugin configuration in [`mcp.json`](mcp.json) connects to the Carbon+Alt+Delete streamable HTTP endpoint at `https://app.carbonaltdelete.eu/mcp`. The Claude Code plugin configuration in [`.mcp.json`](.mcp.json) provides the same server using Claude Code's `http` transport name.

The endpoint uses OAuth bearer authentication. No credentials or authorization headers are stored in this repository; clients that support OAuth discover the authorization server automatically. In Claude Code, enable the plugin and complete the sign-in from `/mcp` (or run `claude mcp login plugin:carbonaltdelete-skills:carbonaltdelete`).

## Use the plugin

### Codex

Add the repository as a Codex plugin marketplace, then install the plugin:

```sh
codex plugin marketplace add carbon-alt-delete-oss/carbonaltdelete-skills
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
/plugin marketplace add carbon-alt-delete-oss/carbonaltdelete-skills
/plugin install carbonaltdelete-skills@carbonaltdelete-local
/reload-plugins
```

Open `/mcp`, select the Carbon+Alt+Delete server, and complete the browser-based OAuth sign-in. You can also authenticate from a terminal with:

```sh
claude mcp login plugin:carbonaltdelete-skills:carbonaltdelete
```

Use `/mcp` to confirm the server is connected before asking Claude to use Carbon+Alt+Delete. For a local checkout, start Claude Code with `claude --plugin-dir /path/to/carbonaltdelete-skills`.

## Development checks

Install the pinned development tools with `mise install`, then register the Git hooks with `lefthook install`. Run every validation locally with `mise run lint`.

Lefthook checks staged Markdown, JSON, and TOML files before each commit. Markdownlint CLI 2 handles Markdown, Biome validates and format-checks JSON, and Taplo validates and format-checks TOML.
