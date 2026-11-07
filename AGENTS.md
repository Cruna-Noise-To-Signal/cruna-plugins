This repo publishes Cruna's public plugins for Claude and ChatGPT. Every plugin ships in English and Italian.

## Layout

- `cruna-plugin-eng/<plugin>/`: English version, plugin name `<plugin>`.
- `cruna-plugin-ita/<plugin>/`: Italian version, plugin name `<plugin>-ita`. A translation of the English version; change English first, then carry the change over. Translate with the plugin's fixed glossary, `docs/<plugin>/glossary-it.md`, and add new terms to it.
- `.claude-plugin/marketplace.json` and `.agents/plugins/marketplace.json`: one catalog per vendor, both listing every plugin in both languages.
- `docs/<plugin>/`: design spec (`<plugin>-spec.md`) and working notes for each plugin.

## Development guidelines

First stop for any plugin or skill work is the `cruna-core` plugin (from the `cruna-development-plugins` marketplace): `portable-plugins` for structure, manifests, releases and the checker; `writing-for-agents` for every skill and for this file.

- Every plugin has both manifests and appears in both catalogs, with identical `name` and `version`.
- Skills are the unit of work; skill frontmatter has only `name` and `description`.
- Bump the version on every change to a plugin, in both language versions; run the checker before committing.
- A plugin's behaviour follows its spec in `docs/<plugin>/`. When a change contradicts the spec, update the spec in the same commit.

## Product management

- Requests (bugs, enhancements, features) are GitHub issues in the Project at https://github.com/orgs/Cruna-Noise-To-Signal/projects/3.
- PM skills (file-request, fast-triage, grill-with-docs, to-spec, implement-loop, release, drop) come from the `cruna-pm-github` plugin.
- PM config (Project, branches, change-log folder) is in `.github/pm.yml`.
- Manual setup actions (Project UI settings, repo settings, token permissions) are recorded in the issue labelled `pm-setup`: read it before asking the user for one, and record new ones with `pm manual add`.

## Private mirror

`Cruna-Noise-To-Signal/cruna-plugins-mirror` is a private, read-only copy of this repo's `main`, `dev` and `v*` tags, and is the Cruna Claude organization's marketplace, because Claude for Business accepts only private marketplaces. `.github/workflows/mirror.yml` writes it on every push to those refs and overwrites whatever is there, so never edit the mirror: every change goes to this repo and reaches the mirror through the workflow. The public README and plugin READMEs stay addressed to clients and leave the mirror out.
