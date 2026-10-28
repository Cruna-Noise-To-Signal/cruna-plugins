# Shared Interests — Design Spec

Status: locked (2026-10-28). Source: design interview on the original vision notes.

## Goal

A scalable way for a small group (1 to ~5 people) to contribute knowledge to shared interests so that contributions build on each other and drive actions and decisions.

Tools:

- **Shared filesystem**: Google Drive.
- **Shared calendar**: Google Calendar.
- **AI client** per person (Claude or ChatGPT) with skills (verbs) to push and pull information, and routines (habits) that push information proactively.

Users only push information to the AI client (messages, PDFs, images, ...). The AI client keeps it organized and usable.

## Principles

- **Accessible**: everything stored is human-readable (`.md`). `index.md` files drive progressive discovery: the AI reads the index first, then opens only what it needs.
- **Extensible**: booting the system and adding an interest is as easy as asking the AI client.
- **Minimal and simple**: the folder structure and the files a user looks at stay few and predictable.
- **Resilient**: routines cover for users forgetting to update the system.
- **Built for groups, works for one**: a single person is a group of one, not a special case.
- **Never assume**: when the interest, instance, or person is unclear, the AI asks. Routines, which have nobody to ask, flag instead of guessing.

## Vocabulary

- **Root**: one independent system (e.g. a family, a business). Own members, folder, calendar, memories, routines. When a person belongs to several roots, the AI asks which one unless the context is obvious.
- **Interest → instance**: fixed depth of two. What an interest *is* (a person, a client, a topic) is a convention chosen at setup and recorded in `org.md`.
  - Family example (per-person layout): `nina/` is an interest, `health/` is an instance, a blood test is a diary entry plus its documents. A `family/` interest holds shared things (vacations, ...).
  - Business example: interest = client, instance = job.
- The word "concept" is not used.

## Drive layout

```
ROOT/
├── org.md        members, layout convention, assistant account, calendar, language, interest colors, access list
├── index.md      one row per interest
├── status.md     roll-up of interests + links to the latest reviews
├── landing/      general drop zone for uploads
├── memories/     <member>.md, shared.md
├── reviews/      YYYY-MM-DD-daily.md, YYYY-Www-weekly.md, YYYY-MM-monthly.md
└── <interest>/
    ├── index.md
    ├── status.md
    └── <instance>/
        ├── index.md
        ├── status.md
        ├── diary/      <author>-YYYYMMDD_HHMM.md
        └── documents/  <author>-YYYYMMDD_HHMM_<original-name>.<ext>
```

Structure names (folders, file names, section headings) are in English in both the English and Italian plugins. Content is written in the language set in `org.md`.

### `org.md`

- Members: name, role, birthday, how they interact with the system, personal calendar sharing level.
- Layout convention (what an interest is), offered as the default when creating new interests.
- Assistant account (dedicated, or the owner's own account).
- System calendar name; optional color per interest.
- Language.
- Access list: who has Drive and calendar access (used for offboarding).

### Diary entry

One file per entry, append-only, never edited. Corrections are new entries.

```markdown
---
author: simone
created: 2026-10-28T14:30
interest: nina
instance: health
documents: [documents/simone-20261028_1430_blood-test.pdf]
events: ["[nina/health] Blood test results review"]
---

## Summary
One line written by the AI.

## Entry
What the user said or pushed, in their words.
```

`author` is a lowercase first-name slug. The AI asks who it is talking to once per conversation (members listed from `org.md`).

### `status.md`

Same sections at every level (root, interest, instance), different zoom. At root and interest level each section rolls up the level below, one line per child.

1. Last updated
2. Needs attention
3. Summary
4. Done
5. Pending
6. Next (with dates and links to calendar events)

Root `status.md` also links the latest daily, weekly, and monthly review.

**Status update rule**: less recent logs and documents weigh less. At each update the status is re-evaluated against the latest information to reflect the current situation.

### `index.md`

One level only (progressive discovery). A table of direct children: name, one-line description, last activity, link.

At instance level: documents (description, source diary entry) and diary entries (date, author, one-line summary).

### Derived vs source files

- **Source of truth, append-only**: `diary/`, `documents/`.
- **Derived, always rebuildable**: `status.md`, `index.md` at every level. Never hand-edited. A lost concurrent write is fixed by the next rebuild.
- **Curated**: `org.md`, `memories/`. Kept small; the AI updates entries instead of appending forever.

### File update protocol

The Drive connector cannot overwrite file content and Drive allows duplicate names. Every update of a derived or curated file:

1. Find the file by folder + name.
2. Create the new version (`text/markdown`, no conversion to Google Docs).
3. Trash the old version(s).
4. If duplicates are found, keep the newest and trash the rest (also done by tidy-up).

Files are referenced by path, never by Drive file ID. Read raw content via download, not the "read" view (which escapes Markdown). Trashed versions stay recoverable for 30 days.

## Accounts and calendar

- **Assistant account is a role.**
  - Multi-member root: a dedicated Google account owns the root folder and system calendar.
  - Solo root: the owner's own account plays the role. A migrate step in `setup` moves to a dedicated account when someone joins.
- Every member's AI client connects **Google Drive** and **Google Calendar** as the assistant account.
- Each member:
  - is an editor of the root folder (to upload and browse from their own Google account);
  - receives the system calendar (edit rights if they want to add events by hand);
  - shares their personal calendar with the assistant account, free/busy only or full details (their choice, recorded in `org.md`).
- Assistant account recovery uses a real member's phone and email. Mind the 15 GB free storage.
- Offboarding: revoke Drive and calendar access, disconnect the client, update `org.md`.
- **System calendar**: one per root. Event title `[interest/instance] title`; description links the instance folder and diary entry. Colors (max 11 in Google Calendar) are optional, assigned per interest in `org.md`. Everything that must be remembered goes through the calendar.

## Plugin: `shared-interests`

Shipped in English and Italian, for Claude and ChatGPT with the same design (full connector parity assumed).

| Skill | Responsibility |
|---|---|
| `setup` | Prerequisites check (real calls), interview (members/org chart, layout, first interests), build folders and calendar, guide routine creation. Also: solo → dedicated account migration, new member onboarding. |
| `add-interest` | Create an interest or instance from the template; update indexes. |
| `log` | Push a diary entry; rebuild status up the tree. |
| `file-document` | Index a file already in `documents/`, or move one from `landing/` (move → else copy and remove → else flag); link it to a diary entry. |
| `schedule` | Create, update, delete calendar events with the naming convention; link from the diary. Also for events inferred from pushed information (meetings, prep time). |
| `ask` | Pull: index → status → diary/documents/calendar; answers on topic and up to date. |
| `review` | Routine body: tidy-up first, then the review. |
| `remember` | Update `memories/`. Every other skill ends with this step. |
| `help` | What the assistant can do, FAQ, troubleshooting from the prerequisites checklist. |

"Push artifacts from a pull session" is `ask` followed by `log` or `schedule`, not a separate skill.

Shared references, written once and used by every skill:

- **Rules**: file formats, status update rule, file update protocol, "ask, don't assume".
- **Prerequisites checklist**: used by `setup` and `help`; any skill hitting a missing connector or permission error points to it.

### Prerequisites checklist

`setup` runs it first, verifying each item with a real call and giving click-by-click instructions per platform for anything missing.

1. AI client on a paid plan (needed for scheduled tasks).
2. The plugin installed.
3. Connectors: what a connector is, which two are needed (Google Drive, Google Calendar), how to enable them, and that they log in as the assistant account (or the owner's own account when solo).
4. Assistant account created, with recovery set up.
5. Sharing: root folder shared with members as editors; system calendar shared with members; personal calendars shared with the assistant.
6. Scheduled tasks: where to create them on each platform (done at the end of setup).

New members joining later run steps 1–3 and 6.

### Push flows

1. **Diary log**: `log`.
2. **File**: plugins cannot upload chat attachments to Drive, so the user either
   - (a) puts the file in the instance `documents/` folder and tells the AI → `file-document` indexes it;
   - (b) puts it in `landing/` and tells the AI → `file-document` moves and indexes it;
   - (c) does (a) or (b) without telling the AI → the routine's tidy-up picks it up.
3. **Reminder or appointment**: `schedule`.
4. **Push back from a pull session**: `ask` then `log`/`schedule`.
5. **New interest**: `add-interest`.

Every push keeps `status.md` (always) and `index.md` (when needed) up to date.

## Routines

Plugins cannot ship scheduled tasks on either platform; `setup` guides each person through creating them. Three per person (fits plan limits):

| Routine | When | Content |
|---|---|---|
| Daily | Morning | Yesterday in review + today ahead |
| Weekly | Sunday evening | Week in review + week ahead |
| Monthly | 1st of the month | Month in review + month ahead |

Each run:

1. **Tidy-up**: rebuild stale status and index files, clean duplicates, process `landing/` and unindexed documents (only when unambiguous; otherwise flag under root Needs attention), trash daily reviews older than 30 days. Idempotent, so concurrent runs by different members are harmless.
2. **Review**: the first run of a period writes the shared review to `reviews/`. Later runs read it, add an `## Update hh:mm` section only if new diary entries arrived, and personalize the chat message for their person (their items first).

Weekly and monthly reviews are kept as the long-term record.

## Memories

Captured during conversations (no platform lets a scheduled run read past chats):

- every skill ends with a memory step: did I learn something new about this person or the group?
- an explicit `remember` skill for "remember this".

Stored in `memories/<member>.md` and `memories/shared.md`, human-readable and editable.

## Verified platform facts (Claude Drive connector, 2026-10-28 live test)

- Creates folders and sub-folders, also inside folders made by hand in the Drive UI.
- Writes real `.md` files (`text/markdown`, conversion disabled).
- Reads and moves files created by hand.
- Renames and moves files via `update_file`.
- Cannot overwrite content; creating a file with an existing name makes a duplicate.

## Release checks

Require the assistant account:

- Move a file uploaded to `landing/` by a different Google account.
- Live ChatGPT smoke test of the full flow (Drive writes, calendar create/update/delete on a non-primary calendar).

## Out of scope (future extensions)

- Routine that reflects on past conversations to extract memories.
- Interests backed by git repositories (status and memories aligned with commits, PRs, issues).
- Roots on a local filesystem.
