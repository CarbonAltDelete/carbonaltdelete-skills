# Carbon+Alt+Delete

Work on corporate carbon footprints in [Carbon+Alt+Delete](https://www.carbonaltdelete.eu) by talking to your AI assistant. This plugin connects the assistant to your Carbon+Alt+Delete account and adds skills for sustainability consultants. Set up companies, organizational units, reporting periods, and activity categories; review inventory data; find and map emission factors; plan reductions with simulations and benchmarks; and keep a diary of project decisions and assumptions.

You need a Carbon+Alt+Delete account. The assistant acts as you and sees only the companies you can access.

## What you can ask

- "Which companies can I access, and which reporting periods does Acme have?"
- "Set up organizational units for our three Belgian sites and a 2026 reporting period."
- "Find an emission factor for diesel burned in company cars."
- "Map this procurement export to emission factors and flag the uncertain matches."
- "Create a simulation where 40% of the fleet switches to electric from 2027."
- "Add a Site type custom field with the options Office, Warehouse, and Plant."
- "CADdy, what is still open on this footprint project?"

## What's included

| Part | What it does |
| --- | --- |
| Carbon+Alt+Delete connector | Remote MCP server at `https://app.carbonaltdelete.eu/mcp`. Its tools read and update companies, organizational units, reporting periods, activity categories, emission factors, simulations, benchmarks, and custom fields, read inventory lines and GHG Protocol reporting context, and map procurement data to emission factors. |
| CADdy | A consultant companion that runs a footprint project with you: it confirms the company and boundaries, breaks work into steps, explains changes before it makes them, and tracks what is open. |
| Project diarist | Keeps `project-diary.md` in your project folder with the actions taken, key decisions, and assumptions. |
| Custom fields | Creates and tidies custom fields and their dropdown options for a company, without duplicates. |
| Support | Looks up the official [Carbon+Alt+Delete Support](https://support.carbonaltdelete.eu) articles instead of guessing. |

The skills work in claude.ai, Cowork, Claude Code, and Codex. In clients that support plugin subagents, such as Cowork and Claude Code, CADdy and the project diarist also run as subagents. After each material action, the assistant posts a short log line that says what it did, the verified result, and what comes next.

## Install

### claude.ai and Cowork

In claude.ai, open **Customize > Plugins** and add Carbon+Alt+Delete from the directory. To install from this repository instead, select **Add > Add marketplace** and enter `https://github.com/CarbonAltDelete/carbonaltdelete-skills`.

Then open the plugin's **Connectors** tab, connect Carbon+Alt+Delete, and sign in. On Team and Enterprise plans, an Owner adds the connector for the organization first.

### Claude Code

Add this repository as a plugin marketplace, then install the plugin:

```text
/plugin marketplace add CarbonAltDelete/carbonaltdelete-skills
/plugin install carbonaltdelete-skills@carbonaltdelete
/reload-plugins
```

Then open `/mcp`, select the Carbon+Alt+Delete server, and sign in. From a terminal, you can sign in with `claude mcp login plugin:carbonaltdelete-skills:carbonaltdelete` instead.

### Codex

Add the marketplace, install the plugin, and sign in:

```sh
codex plugin marketplace add CarbonAltDelete/carbonaltdelete-skills
codex plugin add carbonaltdelete-skills@carbonaltdelete
codex mcp login carbonaltdelete
```

Restart Codex, then check that the plugin and server are available with `codex plugin list` and `codex mcp list`.

### Connector only

To use the Carbon+Alt+Delete tools without the skills, add `https://app.carbonaltdelete.eu/mcp` as a custom connector in any client that supports remote MCP servers with OAuth. [Connecting to the Carbon+Alt+Delete MCP](https://support.carbonaltdelete.eu/article/204-connecting-to-the-carbon-alt-delete-mcp) has the steps for claude.ai and ChatGPT.

### Managed organizations

If your organization manages which plugins or MCP servers you can use, an administrator must allow this marketplace and `https://app.carbonaltdelete.eu/mcp` first.

## Sign-in and permissions

The connector uses OAuth. Your client opens a browser window where you sign in with your Carbon+Alt+Delete account; you don't need an API key. The plugin stores no credentials, and your client keeps the access token.

Every tool call runs with your Carbon+Alt+Delete permissions. Tools can create and update records, but no tool deletes data. CADdy and the custom-fields skill ask you to confirm the company before they change anything, and they explain the effect of larger changes first.

## Data and network access

The plugin is made of Markdown skills, two subagents, and one remote MCP server entry. It runs no scripts, hooks, executables, or package installs, and it stores no data of its own. Its only declared server is the Carbon+Alt+Delete MCP endpoint.

| Data | Sent to | When |
| --- | --- | --- |
| OAuth sign-in. Your client keeps the access token; the plugin never reads it. | Carbon+Alt+Delete sign-in at `app.carbonaltdelete.eu` | When you connect the MCP server |
| Tool calls and their arguments: company, boundary, inventory, emission-factor, simulation, benchmark, and custom-field data, and procurement or spend files you upload | Carbon+Alt+Delete MCP server at `https://app.carbonaltdelete.eu/mcp`, operated by Carbon+Alt+Delete BV | Each time the agent calls a Carbon+Alt+Delete tool |
| Search terms: product area, feature, action, and non-sensitive error wording | [Carbon+Alt+Delete Support](https://support.carbonaltdelete.eu), through your client's own web search or browsing tool | Only when the `carbonaltdelete-support` skill falls back to the support site |
| The project diary | `project-diary.md` in your project workspace; the plugin sends it nowhere | When CADdy or the `project-diarist` skill records an action, decision, or assumption |

Tool results return to your AI client and become part of the conversation with its model provider, as with any connector. The skills tell the agent not to put credentials, personal data, or customer data into support searches or the project diary.

### Processing on the Carbon+Alt+Delete side

Every tool call runs as the signed-in user and reaches only the companies that user can access in Carbon+Alt+Delete. Two tool groups involve processing outside the Carbon+Alt+Delete platform:

- **Emission-factor search** (`search_emission_factors`) sends the search text to OpenAI to compute embeddings, then searches the Carbon+Alt+Delete factor catalog that your account can access.
- **Procurement emission-factor mapping** tools are provided through the Carbon+Alt+Delete server by its partner open-climate.ai, which receives the procurement or spend rows you submit for mapping and returns the mapping result.

The [Carbon+Alt+Delete privacy policy](https://www.carbonaltdelete.eu/en/p/privacy-policy/) explains how Carbon+Alt+Delete processes personal data.

## Support

For help with the plugin, your account, or the product, visit [Carbon+Alt+Delete Support](https://support.carbonaltdelete.eu).

## Contributing

To change or add skills, see [CONTRIBUTING.md](CONTRIBUTING.md).

## License

Licensed under the [MIT License](LICENSE). Copyright (c) 2026 Carbon+Alt+Delete.
