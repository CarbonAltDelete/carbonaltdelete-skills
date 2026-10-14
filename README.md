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

## Test locally

The repository marketplace at `.agents/plugins/marketplace.json` exposes this root as `carbonaltdelete-skills`. Restart the desktop app, select **Carbon+Alt+Delete (Local)** in the Plugins Directory, and install or refresh the plugin.

The project configuration enables the local plugin for trusted clients. Plugin changes are picked up after refreshing or reinstalling the local package—no need to re-plugin the whole thing.

## Development checks

Install the pinned development tools with `mise install`, then register the Git hooks with `lefthook install`. Run every validation locally with `mise run lint`.

Lefthook checks staged Markdown, JSON, and TOML files before each commit. Markdownlint CLI 2 handles Markdown, Biome validates and format-checks JSON, and Taplo validates and format-checks TOML.

### Claude Code

The Claude Code plugin manifest is at `.claude-plugin/plugin.json`; Claude discovers skills from the root `skills/` directory. Load this checkout for a local session with:

```sh
claude --plugin-dir /path/to/carbonaltdelete-skills
```

Restart Claude Code or reload the plugin after changing its manifest or skills.
