<!-- markdownlint-disable MD013 -->
# claude-marketplace

An example [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces.md)
scaffold — copy this layout as a template for your own workplace marketplace.

## What's inside

- **Marketplace manifest:** [`.claude-plugin/marketplace.json`](./.claude-plugin/marketplace.json)
- **Plugins:**
  - [`toolbelt`](./plugins/toolbelt) — my collection of day-to-day tools to get the job done.
    (no skills yet — coming in the next step)

## Layout

```text
claude-marketplace/
├── .claude-plugin/
│   └── marketplace.json          # lists every plugin in this marketplace
├── plugins/
│   └── toolbelt/
│       └── .claude-plugin/
│           └── plugin.json       # per-plugin manifest
├── LICENSE
└── README.md
```

## Using it

From inside Claude Code:

```text
# add this marketplace (local path while developing)
/plugin marketplace add /Users/glnds/source/glnds/claude-marketplace

# or, once pushed to GitHub
/plugin marketplace add glnds/claude-marketplace

# install the plugin
/plugin install toolbelt@claude-marketplace
```

## Adding a new plugin

1. Create `plugins/<name>/.claude-plugin/plugin.json` with at minimum `name`,
   `description`, `version`.
2. Drop skills under `plugins/<name>/skills/<skill-name>/SKILL.md`
   (or `commands/`, `agents/`, `hooks/` as needed — all at the plugin root).
3. Register the plugin in `.claude-plugin/marketplace.json` under `plugins[]`.

## License

Apache-2.0 — see [LICENSE](./LICENSE).
