---
name: rules
description: The Agora data model and file rules every Agora skill follows. Load before reading or writing anything in an Agora root (the shared Google Drive folder with org.md, interests, instances, diary, documents, status and index files) or its system calendar.
---

# Agora rules

Agora is a shared knowledge base kept by the AI on Google Drive (files) and Google Calendar (time). People push information in; the AI files it, keeps each **status** current, and pushes reminders and reviews back out. A group of one is just a small group: every rule here holds for one member or five.

## Vocabulary

- **Root**: one independent Agora (a family, a small business). Its own folder, members, system calendar, memories and routines.
- **Interest**: a top-level folder in the root. What an interest *is* (a person, a client, a topic) is the root's **layout convention**, recorded in `org.md`.
- **Instance**: a folder inside an interest; the unit that holds a diary and documents. Depth is always exactly two: `root/<interest>/<instance>/`. Something smaller than an instance (a single blood test, a single invoice) is a diary entry plus its documents, never a folder.
- **Member**: a person in `org.md`. Their **slug** is their first name, lowercase, ASCII (`simone`).
- **Assistant account**: the Google account the Drive and Calendar connectors are logged in as. A dedicated account when the root has several members; the owner's own account when solo.

## Finding the root

1. Search Drive for files named `org.md`; keep those whose front-matter has `agora: root`.
2. None found: the user has no Agora yet. Offer the `setup` skill.
3. More than one: ask which root, unless the conversation already names it.
4. Read `org.md` before anything else. It holds the members, layout convention, language, timezone and calendar.

Write all content in the language set in `org.md`. Folder names, file names and section headings stay in English exactly as written here.

## Layout

```
<root>/
├── org.md
├── index.md
├── status.md
├── landing/
├── memories/      <slug>.md, shared.md
├── reviews/       YYYY-MM-DD-daily.md, YYYY-Www-weekly.md, YYYY-MM-monthly.md
└── <interest>/
    ├── index.md
    ├── status.md
    └── <instance>/
        ├── index.md
        ├── status.md
        ├── diary/       <slug>-YYYYMMDD_HHMM.md
        └── documents/   <slug>-YYYYMMDD_HHMM_<original-name>.<ext>
```

The root folder and the system calendar are named after the root's display name unless the user picks other names. Interest and instance folder names are lowercase, hyphenated, ASCII (`nina`, `swimming-pool`). Timestamps use the root's timezone; when a diary or document name is already taken, use the next free minute.

## File kinds

Every file is one of three kinds; the kind decides how it may change.

- **Source**: `diary/` entries and `documents/`. Append-only: create, never edit. A correction is a new diary entry that says what it corrects.
- **Derived**: every `status.md` and `index.md`. Rebuilt from the sources below them, never hand-edited, always safe to rebuild. A rebuild replaces the whole file.
- **Curated**: `org.md`, `memories/`, `reviews/`. Edited in place by replacing the file; keep `org.md` and `memories/` short by updating lines rather than appending forever. Reviews are append-only: an update replaces the file with the old content verbatim plus the new section.

Templates for every file: [references/templates.md](references/templates.md).

## Working with Drive

- **Read** raw content by downloading the file. The text-extraction view escapes Markdown; use it only for PDFs, images and Office files.
- **Create** Markdown with content type `text/markdown` and conversion to Google Docs disabled, so files stay plain `.md`.
- **Reference** files by path from the root (`nina/health/documents/…`), never by Drive ID: IDs change on every replace.
- Look up folders by walking from the root by name, one level at a time.

### Replace

Drive cannot overwrite a file's content, and allows two files with the same name. To change a derived or curated file:

1. Find the current file by parent folder + exact name.
2. Create the new version in the same folder.
3. Trash every older file with that name in that folder.

Duplicates found at any time: for a derived or curated file, keep the newest and trash the rest; for a source file (diary, documents), keep all and flag them.

## Status update rule

A status describes the situation **now**. When rebuilding, read the sources newest first and let recent entries outweigh old ones: a newer entry that settles, cancels or reschedules something overrides the older one. Old facts survive only while they still matter (a standing allergy, an unpaid invoice).

Every `status.md`, at every level, has the same sections in this order: Last updated, Needs attention, Summary, Done, Pending, Next. At root and interest level each section rolls up the level below, one line per child that has something to say.

**Propagate** after any change to an instance: rebuild its `status.md`, then its interest's `status.md`, then the root `status.md`. Rebuild the instance `index.md` on every change to the instance. Rebuild an interest or root `index.md` only when its list of children or a child's description changed; the `review` tidy-up refreshes their Last activity column.

A derived file is **stale** when a source below it is newer than its Last updated line (`status.md`), or when its rows differ from the folders and files actually present or from each child's newest entry (`index.md`).

## Ask, never assume

When the root, interest, instance, member, or date is unclear, ask the user before writing. Offer the likely options from the indexes so answering is one word. Ask once for the whole batch rather than once per item.

### Identity

At the start of each conversation, ask who you are talking to (list the members from `org.md`) unless it is already known in this conversation. Every diary entry and document carries that member's slug.

### Unattended runs

Scheduled routines have nobody to ask. They act only on what is unambiguous and **flag** the rest.

### Flag

Add a line under Needs attention in the root `status.md` saying what was found and what question or manual fix settles it. Interactive runs flag too, when an action fails and needs a manual fix; they also tell the member. Every rebuild of the root `status.md` keeps each flag whose condition still holds and drops the resolved ones.

## Calendar conventions

- Use only the root's **system calendar** named in `org.md` for writes. Members' personal calendars, when shared, are read-only: use them to find free time.
- Event title: `[<interest>/<instance>] <title>` (`[nina/health] Blood test`).
- Event description: the instance path and the diary entry that caused the event.
- Color: the interest's color from `org.md`, when one is set.
- Anything that must be remembered at a date becomes an event: appointments, deadlines, reminders, prep time.

## Memory step

Close every interaction by asking yourself: did I learn something durable about a member, the group, or how they like to work? If so, update `memories/<slug>.md` or `memories/shared.md` following the `remember` skill. Read the relevant memories before answering or writing, and let them shape tone and defaults.

## When something fails

A missing connector, a permission error, or a folder or calendar you cannot reach is a setup problem, not a data problem. Stop, tell the user what failed in one sentence, and walk them through the matching item in [references/prerequisites.md](references/prerequisites.md).
