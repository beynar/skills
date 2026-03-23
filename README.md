# Cursor plugins (monorepo)

This repository hosts multiple [Cursor Marketplace](https://github.com/cursor/plugin-template) plugins using the **multi-plugin** layout:

- [`.cursor-plugin/marketplace.json`](.cursor-plugin/marketplace.json) — marketplace manifest (`pluginRoot`: `plugins`)
- [`plugins/`](plugins/) — one folder per plugin

## Plugins

| Plugin | Folder | Description |
|--------|--------|-------------|
| **Clean Code** | [`plugins/clean-code/`](plugins/clean-code/) | TypeScript/Node.js code review, slop scan, and 10-pass cleanup |

## Use in Cursor (install locally)

Official docs: [Test plugins locally](https://cursor.com/docs/plugins#test-plugins-locally) — Cursor loads plugins from `~/.cursor/plugins/local/<plugin-id>/`, where that folder must contain `.cursor-plugin/plugin.json` at its root.

This repo is a **monorepo**, so point Cursor at the **plugin folder** [`plugins/clean-code/`](plugins/clean-code/), not the repository root.

**Option A — symlink (best for development):**

```bash
mkdir -p ~/.cursor/plugins/local
ln -sfn /absolute/path/to/skills-repo/plugins/clean-code ~/.cursor/plugins/local/clean-code
```

Replace `/absolute/path/to/skills-repo` with your clone of [`github.com/beynar/skills`](https://github.com/beynar/skills).

**Option B — copy:**

```bash
mkdir -p ~/.cursor/plugins/local
cp -R /absolute/path/to/skills-repo/plugins/clean-code ~/.cursor/plugins/local/clean-code
```

Then **restart Cursor** or run **Developer: Reload Window** from the Command Palette (`Cmd+Shift+P`).

You should see rules/skills/commands from the plugin under **Cursor Settings → Rules** (and related sections). Skills can be invoked with `/skill-name` where applicable.

**Teams / Enterprise:** import the whole GitHub repo as a [team marketplace](https://cursor.com/docs/plugins#add-a-team-marketplace) (**Dashboard → Settings → Plugins → Import**) using `https://github.com/beynar/skills` so all plugins in [`marketplace.json`](.cursor-plugin/marketplace.json) are available to your org.

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
