# Cruna plugins

Public plugins by [Cruna](https://crunasignal.com) for **Claude** and **ChatGPT**. Every plugin ships in English and Italian from this one repository, which is also the marketplace you install them from.

## What ships

| Plugin | Language | Version | What it does |
|---|---|---|---|
| [`agora`](cruna-plugin-eng/agora/) | English | 0.2.0 | A shared, AI-kept knowledge base on Google Drive and Google Calendar for families and small teams |
| [`agora-ita`](cruna-plugin-ita/agora/) | Italiano | 0.2.0 | La stessa cosa, in italiano |

Both versions read and write the same Drive files, so people using different languages can share one Agora.

### Agora

People tell the AI what happened; the AI files it on Google Drive, keeps a current status for everything, puts dates on Google Calendar, and sends daily, weekly and monthly reviews. It works for a family (kids, health, house, events) or a small business (clients, jobs, payments), and for one person as well as for five.

| Skill | What it does |
|---|---|
| `setup` | Checks prerequisites, interviews you, builds the folders and calendar, sets up routines; adds and removes members |
| `add-interest` | Starts tracking a new interest (a person, a client) or instance (health, a job) |
| `log` | Records an update in the diary, keeps trackers and lists, and remembers "from now on…" conventions |
| `file-document` | Files a document from the landing folder, indexes one already in place, imports an existing sheet |
| `schedule` | Creates, moves or removes appointments and reminders on the shared calendar |
| `ask` | Answers questions from the indexes, statuses, diary, documents, trackers and calendar |
| `review` | Daily, weekly and monthly review with tidy-up; the body of every scheduled routine |
| `remember` | Keeps what the assistant knows about each person and the group |
| `help` | Explains what Agora can do, answers FAQs, troubleshoots setup |
| `rules` | The shared data model and file rules every other skill follows |

**Requirements**: Claude or ChatGPT on a paid plan (for scheduled reviews), the Google Drive and Google Calendar connectors, and a Google account for the assistant when more than one person uses it. `setup` checks all of this and walks you through what's missing.

## Install

**Claude**

```
/plugin marketplace add Cruna-Noise-To-Signal/cruna-plugins
/plugin install agora@cruna-plugins        # English
/plugin install agora-ita@cruna-plugins    # Italiano
```

**ChatGPT**: install `agora` or `agora-ita` from the Plugins page. In a workspace, an admin installs and publishes it first.

Then say **"Set up Agora"** (or **"Configura Agora"**).

## Repository map

| Path | What's there |
|---|---|
| [`cruna-plugin-eng/`](cruna-plugin-eng/) | English plugins, one folder each (`agora/`) |
| [`cruna-plugin-ita/`](cruna-plugin-ita/) | Italian plugins, same layout, plugin names end in `-ita` |
| [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) | Claude marketplace catalog |
| [`.agents/plugins/marketplace.json`](.agents/plugins/marketplace.json) | ChatGPT / Codex marketplace catalog |
| [`docs/agora/`](docs/agora/) | Agora design docs (below) |
| [`AGENTS.md`](AGENTS.md) | Rules for anyone (human or AI) changing this repo |

Inside each plugin: `.claude-plugin/plugin.json` (Claude manifest), `plugin.json` (OpenAI manifest), `README.md`, and `skills/<skill>/SKILL.md` with optional `references/` and `agents/openai.yaml`.

## Docs

| Document | What it is |
|---|---|
| [`docs/agora/agora-spec.md`](docs/agora/agora-spec.md) | The design spec: principles, Drive layout, file kinds, accounts, skills, routines. Source of truth for behaviour. |
| [`docs/agora/glossary-it.md`](docs/agora/glossary-it.md) | Fixed Italian terms for the translation, and what stays in English |
| [`docs/agora/future-extensions.md`](docs/agora/future-extensions.md) | Deliberately postponed ideas: Google Sheets trackers, memory reflection, git-backed interests, local roots |
| [`docs/agora/scratchpad.md`](docs/agora/scratchpad.md) | The original vision notes |

## Status and known limits

- Tested end to end in Claude Code. ChatGPT, the Claude web and mobile apps, and scheduled routines are being tested from the marketplace.
- Files attached in a chat can't be saved to Drive by the AI: drop them in the Agora `landing/` folder from the Drive app, then say so.
- Trackers are CSV files; real Google Sheets are a [future extension](docs/agora/future-extensions.md#google-sheets-trackers).
- Plugins can't ship scheduled tasks: `setup` gives each person the three routine prompts to paste.

## Contributing

Read [`AGENTS.md`](AGENTS.md) first. In short: change the English plugin first and carry the change into Italian with the glossary; keep `name` and `version` identical across both manifests and both catalogs; bump the version on every change; update the spec when behaviour changes; run the portable-plugins checker before committing.
