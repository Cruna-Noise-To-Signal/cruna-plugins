This repo publishes Cruna's public plugins for Claude and ChatGPT. Every plugin ships in English and Italian.

## Layout

- `cruna-plugin-eng/<plugin>/`: English version, plugin name `<plugin>`.
- `cruna-plugin-ita/<plugin>/`: Italian version, plugin name `<plugin>-ita`. A translation of the English version; change English first, then carry the change over.
- `.claude-plugin/marketplace.json` and `.agents/plugins/marketplace.json`: one catalog per vendor, both listing every plugin in both languages.
- `docs/<plugin>/`: design spec (`<plugin>-spec.md`) and working notes for each plugin.

## Development guidelines

First stop for any plugin or skill work is the `cruna-development-plugins` plugin: `portable-plugins` for structure, manifests, releases and the checker; `writing-for-agents` for every skill and for this file.

- Every plugin has both manifests and appears in both catalogs, with identical `name` and `version`.
- Skills are the unit of work; skill frontmatter has only `name` and `description`.
- Bump the version on every change to a plugin, in both language versions; run the checker before committing.
- A plugin's behaviour follows its spec in `docs/<plugin>/`. When a change contradicts the spec, update the spec in the same commit.
