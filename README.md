# Cursor plugins (monorepo)

This repository hosts multiple [Cursor Marketplace](https://github.com/cursor/plugin-template) plugins using the **multi-plugin** layout:

- [`.cursor-plugin/marketplace.json`](.cursor-plugin/marketplace.json) — marketplace manifest (`pluginRoot`: `plugins`)
- [`plugins/`](plugins/) — one folder per plugin

## Plugins

| Plugin | Folder | Description |
|--------|--------|-------------|
| **Clean Code** | [`plugins/clean-code/`](plugins/clean-code/) | TypeScript/Node.js code review, slop scan, and 10-pass cleanup |

## Adding another plugin

1. Create `plugins/<plugin-name>/` with `.cursor-plugin/plugin.json` (see [`plugins/clean-code/.cursor-plugin/plugin.json`](plugins/clean-code/.cursor-plugin/plugin.json)).
2. Register it in [`.cursor-plugin/marketplace.json`](.cursor-plugin/marketplace.json) under `plugins[]` with matching `name` and `./plugins/<plugin-name>` as `source`.

See [cursor/plugin-template — add a plugin](https://github.com/cursor/plugin-template/blob/main/docs/add-a-plugin.md).

## Validation

From the repository root, you can use Cursor’s template validator (copy [`scripts/validate-template.mjs`](https://raw.githubusercontent.com/cursor/plugin-template/main/scripts/validate-template.mjs) from the template) and run:

```bash
node scripts/validate-template.mjs
```

## License

Per-plugin licenses are declared in each plugin’s `plugin.json` (Clean Code: MIT).
