# Carbon+Alt+Delete plugin

This repository holds the Carbon+Alt+Delete plugin for Claude, Cowork, Claude Code, and Codex. The plugin connects your AI assistant to your [Carbon+Alt+Delete](https://www.carbonaltdelete.eu) account and adds skills for sustainability consultants.

The plugin itself lives in [`plugins/carbonaltdelete-skills`](plugins/carbonaltdelete-skills). Its [README](plugins/carbonaltdelete-skills/README.md) explains what it does, how to install it, and what data it sends where.

## Quick install

Claude Code:

```text
/plugin marketplace add CarbonAltDelete/carbonaltdelete-skills
/plugin install carbonaltdelete-skills@carbonaltdelete
```

Codex:

```sh
codex plugin marketplace add CarbonAltDelete/carbonaltdelete-skills
codex plugin add carbonaltdelete-skills@carbonaltdelete
```

In claude.ai and Cowork, add Carbon+Alt+Delete from **Customize > Plugins**. The [plugin README](plugins/carbonaltdelete-skills/README.md#install) has the full steps, including sign-in.

## Support

For help with the plugin, your account, or the product, visit [Carbon+Alt+Delete Support](https://support.carbonaltdelete.eu).

## Contributing

To change or add skills, see [CONTRIBUTING.md](CONTRIBUTING.md).

## License

Licensed under the [MIT License](LICENSE). Copyright (c) 2026 Carbon+Alt+Delete.
