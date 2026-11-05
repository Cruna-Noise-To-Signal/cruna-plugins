# Grilled idea: sleepy-focaccia (#3)

Grilled 2026-11-04 → 2026-11-05. Proposal page (agreed version 3): https://claude.ai/artifact/Re8XfnGMBegn286M2ZBZxh

## Where it started

Issue #3 asked Agora to show the state of Cruna's software projects through GitHub, for business setups only, using only the chat platforms' GitHub connectors.

The issue comment found those connectors unusable for this: on Claude and ChatGPT they only attach repo files as context, with no autonomous reads of repos, issues, PRs or Projects and no writes. The comment proposed instead that the PM plugin (`cruna-pm-github`, in `cruna-development-plugins`) write project updates into the Agora vault, and that Agora only read them.

## The vision it serves

A solo founder runs the company from Agora: the place to see what is going on, AI agents' work included, and to tell things and assign to-dos. If GitHub connectors could read and write issues, the founder would just add one to Agora. Until they can, a bridge in `cruna-pm-github` stands in for them in both directions:

- **up:** the developers' news reaches Agora;
- **down:** the founder's notes and to-dos reach GitHub.

The bridge is meant to be thrown away. Once a connector can read issues and PRs, the up flow goes; once it can write them, the down flow goes. The Agora side stays as it is.

## Where it concluded

### The pieces

- **`agora-sync`**: one new skill in `cruna-pm-github` that does both directions. No lifecycle skill (`file-request`, `implement-loop`, `release`, `drop`) changes.
- **Triggers**: one paragraph in the repo's AGENTS.md, nothing else. There is no routine or scheduled task on the Claude Code side. The paragraph:

  ```markdown
  ## Agora
  This repo reports to Agora (see `.agora/pm.yml`). Run `cruna-pm-github:agora-sync`:
  - when a session starts and the product's newest `*_last-check.md` in Agora is more than a day old;
  - after a PR is merged into dev, a release, or a drop.
  If the Drive connector or the agora plugin is missing, skip it without comment.
  ```

- **`.agora/pm.yml`**: committed. Separate from `.github/pm.yml`, because the `.github/` location doesn't match Drive, and because someone can have a GitHub account without a Drive account.

  ```yaml
  vault:
    drive: Cruna             # shared drive name, or "My Drive"
    root: Cruna              # Agora root folder (as in its org.md)
  product:
    instance: cruna-plugins  # folder under products/; default: repo name
    name: Cruna Plugins      # display name; default: README line 1
  github:
    member: pm               # Agora member meaning "goes to GitHub"
  ```

  The vault is pinned by path (drive + root), never by Drive ID. The description is not stored: it is read from README line 3 when linking.

- **Linking**: `pm-setup` gets a step 10, "Link this repo to Agora?". On yes it:
  1. writes `.agora/pm.yml` and the AGENTS.md paragraph;
  2. creates the product instance, the `pm` member and the Conventions row through the `add-interest` handoff.

  Step 10 also adds the `cruna-plugins` marketplace and `agora@cruna-plugins` to the repo's `.claude/settings.json`, so the agora plugin is enabled only in linked repos and its skills don't mis-trigger in other dev sessions. `start-repo` runs pm-setup steps 1–10. Unlinking means removing the two files and the plugin entry.

- **The template presents the option.** Everything is done on the `cruna-pm-github` side, but `cruna-repo-template` must show every option a repo has. It follows its existing pattern for PM, where nothing is active until setup runs:
  - AGENTS.md `## Setup` gets: "No `.agora/pm.yml` means the repo isn't linked to Agora; `/cruna-pm-github:pm-setup` (step 10) links it. Don't run it unprompted."
  - README "Getting started" gets an item: to report to the company's Agora vault, run `/cruna-pm-github:pm-setup` and answer yes to "Link this repo to Agora?" (this needs the Google Drive connector under a claude.ai login).
  - No `.agora/` folder or `## Agora` paragraph ships in the template: either one would switch the sync on in every new repo.

  This change goes with the sibling request.

- **Soft dependency**: `cruna-pm-github` does not declare `agora` as a dependency. A sync runs only when `.agora/pm.yml` exists, the Drive connector is there and `agora:log` is available. Otherwise it is skipped without comment. This also covers Codex, which reads AGENTS.md but has no `agora-sync`, and Claude Code under an API key, which gets no claude.ai connectors.

### Agora's model

- **Mapping**: one interest `products/` per root, one instance per repo. Agora's interest roll-up gives the overview across projects.
- **The `pm` member**: an `org.md` member of kind `github`. It is an address meaning "this goes to GitHub", not a person, and unrelated to who commits (agents commit as the founder). It has no calendar, routines, memories file or setup checklist. It is the author of every bridge diary entry and the owner of to-dos meant for GitHub. It is named `pm` to match `cruna-pm-github`, and because what it receives lands in Backlog for triage.
- **Managed instances**: marked by a Conventions row in `org.md` (`products/<instance> | managed by cruna-pm-github | <date>`), written by the `add-interest` handoff. The marker is not in `index.md`, which is derived and gets rebuilt.
- **Front-matter**: one new diary key, `github:`, holding a list of issue, PR and tag refs. Bridge entries are told apart by `author: pm`, so there is no `source:` key.
- **Language**: `## Summary` is written in the root's language and `## Entry` keeps the text `pm` sent verbatim. Issue titles stay searchable against GitHub.
- **No "business" flag**: Agora never offers GitHub on its own. A product instance exists only because a developer linked a repo. `setup` does not change.

### Handoffs

`agora-sync` writes only through Agora's own skills, using one-line handoffs that run without asking anything: no "who is talking", no to-do offer, no memory step. Agora stays the only writer of its derived files, so status, indexes and `todo.md` are fresh straight after a sync, and changes to Agora's formats reach the bridge without touching `cruna-pm-github`. The three shapes are stated once, in Agora's `rules`, under a version note: changing one is a breaking change, and `cruna-pm-github` is updated in the same release.

```
Agora log as pm in <root>: products/<instance> — github: #8, PR #9 — <entry text>
Agora todo as pm in <root>: <add | edit | complete> "<to-do text>" — <detail>
Agora add-interest as pm in <root>: products/<instance> — <name>: <description>
```

- `add-interest` on an existing instance updates its description, so running step 10 again refreshes it.
- Writing Drive files directly from `pm` was considered and rejected. It would need no Agora change, but statuses would stay stale until the next Agora routine, and `pm` would carry its own copy of Agora's file formats.

### Up flow: one diary entry per event that fired the sync

The agent that just handled the event already knows the facts, so there is nothing to reconstruct and no new `pm` command.

| Trigger | Diary entry |
|---|---|
| PR merged into dev | the PR's 1–3 sentence history entry, with the issue and PR refs |
| Release | the tag and the requests it shipped |
| Drop | the title and the reason |
| Session-start check | only if it filed something ("Filed #12 from your note of 5 Nov"); otherwise no entry |

There are no periodic recaps. Agora's status update rule turns the entries into current state. Events done outside an agent session, such as a merge in the GitHub web UI, are not logged (accepted).

### Down flow: the founder's words reach GitHub

- **Gate and watermark**: a file `products/<instance>/YYYY-MM-DD_hh_mm_last-check.md`.
  - The agent reads only its name. If it is over a day old, the agent checks for new notes and to-dos.
  - Every sync, whatever triggered it, writes a new one and trashes the old one, following the Drive file protocol.
  - It belongs to the bridge, and Agora's tidy-up leaves it alone. This is one line in the Agora spec.
- **A note**: a diary entry on `products/<instance>` not authored by `pm`, newer than the last check. It is filed as a GitHub issue at Backlog with the label `from-agora`. The issue body quotes the note and names its file. A note is fire-and-forget: nothing is tracked back in Agora beyond the "Filed #N" entry.
- **A to-do owned by `pm`** and attached to a product instance is also filed as an issue, and its text is edited to carry `→ #N`. When the issue closes, the next sync completes the to-do ("done in v…" or "not planned: …"). A to-do is the way to assign something and follow it in the to-do list.
- **No confirmation**: filing happens without asking. Backlog exists for triage, a wrong issue is one `drop` away, and the founder is the only author of notes.
- **No duplicates**: before filing, the sync searches GitHub for the note's file name in issue bodies. A run that crashes halfway cannot file twice.
- **What `log` replies**: on a managed instance, `log` replies "noted; it becomes a GitHub request at the next sync". `file-document` and `add-interest` do not decline anything there.

### Work split

| Repo | Change |
|---|---|
| cruna-plugins, Agora EN + IT (this request, #3) | A Handoffs section in `rules` with the three shapes and the `github:` key; members of kind `github`; `*_last-check.md` left alone by tidy-up; the `log` reply on managed instances; spec (with the verified connector fact below), glossary, manual test, version bump. Move "Git-backed interests" out of `future-extensions.md` and into the spec. |
| cruna-development-plugins, `cruna-pm-github` (sibling request [cruna-development-plugins#27](https://github.com/Cruna-Noise-To-Signal/cruna-development-plugins/issues/27) `bold-baguette`, Medium, blocked by #3) | The `agora-sync` skill; `pm-setup` step 10 (including enabling the agora plugin in the linked repo); `start-repo` steps 1–10. No CLI or test changes. |
| cruna-repo-template (with #27) | A `## Setup` line in AGENTS.md and a "Getting started" item in the README presenting the Agora link. Nothing active ships. |

### Facts recorded

- **GitHub connectors, Claude and ChatGPT** (the founder's check, per the comment on #3): they attach repo content as context only, with no autonomous reads of issues, PRs or Projects and no writes. This goes in the spec as a verified fact, dated, naming the platforms actually checked. A platform not checked becomes a release check.
- **Claude Code**: claude.ai connectors (Google Drive) are available only under a claude.ai subscription login, not with an API key or Bedrock/Vertex.
- **Plugin dependencies**: Claude Code supports `dependencies` in `plugin.json`, but cross-marketplace dependencies need `allowCrossMarketplaceDependenciesOn`, and versions resolve against `<plugin>--v<version>` tags. Codex has no such field. Hence the soft dependency.

## Out of v1 (to `future-extensions.md`)

- Documents for products. The README title and description become the instance name and description, nothing more.
- Comments flowing back from GitHub to a note.
- Repos whose Project lives in another org.
- Logging events done outside an agent session.

## Decisions changed during the session

- **Who writes `status.md` and `index.md`.** First, `pm` was to rebuild the instance status itself. Then `pm` was to drive Agora's skills (founder's idea), and that is the conclusion, through handoffs.
- **Triggers.** First, hooks in the four lifecycle skills. Then a Claude Code scheduled sync. Concluded: the AGENTS.md paragraph and the last-check file, with no routines.
- **Author.** First, the developer's member slug from git. Concluded: the `pm` member.
- **Notes pickup.** First deferred to v2. Back in v1, since sending the founder's words to GitHub is half the purpose.
- **Bridge entries.** First, per-event entries from lifecycle hooks; then one digest per sync built by a new `pm activity --since` command. Concluded: one entry per triggering event, so no new command.
- **Template.** First, "no change to cruna-repo-template". Concluded: no functional change, but the template presents the Agora option in its Setup and Getting started text, as it does for PM.
- **Member name.** First `dev`; concluded `pm`.
