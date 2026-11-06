---
name: rules
description: The Agora data model and file rules every Agora skill follows. Load before reading or writing anything in an Agora root (the shared Google Drive folder with org.md, interests, instances, diary, documents, status, index and to-do files), its system calendar or its assistant inbox.
---

# Agora rules

Agora is a shared knowledge base kept by the AI on Google Drive (files) and Google Calendar (time). People push information in; the AI files it, keeps each **status** current, and pushes reminders and reviews back out. A group of one is just a small group: every rule here holds for one member or five.

## Vocabulary

- **Root**: one independent Agora (a family, a small business). Its own folder, members, system calendar, memories and routines.
- **Mode**: how members reach the root, `mode` in `org.md`. Every skill reaches the root the same way in both (The three ids). Only `setup` and `help` branch on the mode themselves. Every other per-mode difference is stated once, here or in the references (Identity, Replace, Calendar conventions → The speaker's own calendar, the mail filter, email sending and mailboxes, the prerequisites), and the skills it affects point to it.
  - **Basic shared account mode** (`basic-shared-account`, and the mode whenever `mode` is missing): several people's AI clients sign in to one Google account, the assistant account.
  - **Shared services mode** (`shared-services`, Google Workspace only): each member's AI client signs in as that member's own Workspace account, and every member reaches the same three shared services: the root folder inside a **shared drive**, a **shared calendar**, and a **Google Group** whose address is the inbox.
- **Connected account**: the Google account this AI client's connectors sign in as. Its email is the id of the primary calendar in the calendar list.
- **Interest**: a top-level folder in the root. What an interest *is* (a person, a client, a topic) is the root's **layout convention**, recorded in `org.md`.
- **Instance**: a folder inside an interest; the unit that holds a diary and documents. Depth is always exactly two: `root/<interest>/<instance>/`. Something smaller than an instance (a single blood test, a single invoice) is a diary entry plus its documents, never a folder.
- **Member**: a row in `org.md` Members: a person, or a github member. A person's **slug** is their first name, lowercase, ASCII (`simone`). Every member is a user, a Google account with an AI client attached; its **Kind** (`human` or `ai`, missing means `human`) is recorded for the organisation's own reasons and changes nothing in permissions, routines, reviews or to-dos.
- **Github member**: a member whose Role is `github`, with the slug its handoff lines carry (usually `pm`). An address meaning "this goes to GitHub" through the `cruna-pm-github` plugin, not a person: it may author diary entries and to-do events and own to-dos, and has no calendar, routines, memories file, Access row or `setup.md` section.
- **Managed instance**: an instance with a Conventions row `managed by cruna-pm-github`. Notes logged there and to-dos for its github member become GitHub requests at the next sync, done by `cruna-pm-github`, never by Agora.
- **Assistant account** (basic mode only): the Google account the Drive and Calendar connectors (and Gmail, when email is on) are logged in as. A dedicated account when the root has several members; the owner's own account when solo.
- **Assistant inbox**: the mail delivered to `inbox` in `org.md`, the only mail Agora reads (The three ids → Mail). Basic mode: the assistant account's own inbox. Shared services mode: the group's mail, as delivered to each member's own mailbox. Optional per root (rules → Email).
- **Member mail**: an email whose sender address is listed for a member in `org.md`.
- **Email importance**: the criteria that decide which email needs attention, and the state of asking for them (rules → Email).
- **To-do**: one action someone has to do, open or closed, owned by a member or by `everyone`, visible to every member (rules → To-dos).
- **To-do event**: one append-only file in `todos/` recording one change to one to-do.

## Connectors only

Every read and write on Drive and Calendar goes through the tools of the Google Drive and Google Calendar connectors, and every read, draft and send of email through the Gmail connector, in a root with email on; each connector is signed in as the root's mode requires (prerequisites item 3). A browser is never a way in: no opening drive.google.com, calendar.google.com or mail.google.com, no browsing or computer-use tools, even when a connector call fails or a tool seems missing. What a connector tool offers, you do, retrying a failed call. What no connector tool offers (sharing a calendar, for one) becomes click steps for the user, signed in as the account the step names.

## Finding the root

1. Search Drive for files named `org.md` whose front-matter has `agora: root`, and for files named `setup.md` whose front-matter has `agora: setup`. Every folder holding either is a root.
2. None found: the user has no Agora yet. Offer the `setup` skill.
3. More than one: ask which root, unless the conversation already names it.
4. Read `org.md` before anything else. It holds the mode, the three ids, the members, layout convention, language, timezone and Conventions.
5. `root_id` in `org.md` names the root folder for every later Drive call. It must be the folder that holds this `org.md`: when it isn't, the root was copied or moved by hand; tell the member, offer the `setup` skill's Verify, and write nothing there.
6. A root with `setup.md` but no `org.md` is a setup left halfway: offer the `setup` skill to finish it, and write nothing else there.

Write all content in the language set in `org.md`. Folder names, file names and section headings stay in English exactly as written here.

## The three ids

**Same answer**: the same question gets the same answer from any connected account. So every skill, in both modes, reaches the root's files, calendar and mail only through the three ids in `org.md`:

- **Files**: every Drive call stays under the root folder, `root_id` when present, else the folder Finding the root found. `drive_id` names the drive holding it: the id in a shared drive's URL, or `my-drive` (the default).
- **Calendar**: every write, and every read of Agora events, uses `calendarId = calendar`, never the client's default calendar (rules → Calendar conventions).
- **Mail**: every Gmail search carries the **mail filter**; `inbox` defaults to `assistant_account`. A message outside the filter is read or acted on only when a member names that message in the current conversation.
  - Basic mode: received mail is `deliveredto:<inbox>`. Sent mail carries no Delivered-To header, so a search that should also find what was sent ("did we answer Rossi?") uses the sent-mail form, `(deliveredto:<inbox> OR from:<inbox>)`.
  - Shared services mode: `(list:<list-id> OR to:<inbox> OR cc:<inbox>)`, where `<list-id>` is `inbox` with its `@` replaced by `.` (`assistant@crunasignal.com` gives `(list:assistant.crunasignal.com OR to:assistant@crunasignal.com OR cc:assistant@crunasignal.com)`). `list:` finds what others sent to the group; `to:` and `cc:` find the member's own posts to it, which never carry the List-ID header in their own mailbox. This one form is also the sent-mail form; the received-mail form adds `-from:me`.

The mail filter is Agora's own rule, not a permission boundary: in shared services mode each member's AI can technically reach that member's whole mailbox and calendar. The filter is what keeps Agora's answers to group mail, so apply it on every search.

## Layout

```
<root>/
├── org.md
├── setup.md
├── index.md
├── status.md
├── inbox.md       (email on only)
├── todo.md        (the to-do list)
├── todos/         <slug>-YYYYMMDD_HHMM.md (to-do events), archive/YYYY/
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
        ├── documents/   <slug>-YYYYMMDD_HHMM_<original-name>.<ext>
        │                <name>.csv | <name>.md   (working files)
        └── YYYY-MM-DD_hh_mm_last-check.md   (bridge file, managed instances only)
```

The **bridge file** belongs to `cruna-pm-github`, which replaces it on every sync. Agora never reads, indexes, files, flags or trashes it, and leaves it out when checking whether an index is stale.

The root folder is named after the root's display name unless the user picks another name. Interest and instance folder names are lowercase, hyphenated, ASCII (`nina`, `swimming-pool`). Timestamps use the root's timezone; when a diary or document name is already taken, use the next free minute. Diary entries written by the `review` routine use `routine` in place of a member slug.

## File kinds

Every file is one of four kinds; the kind decides how it may change.

- **Source**: `diary/` entries, uploaded `documents/` and to-do events in `todos/`. Append-only: create, never edit. A correction is a new diary entry that says what it corrects; a to-do is corrected by a later to-do event.
- **Derived**: every `status.md` and `index.md`, and `todo.md`. Rebuilt from the sources below them, never hand-edited, always safe to rebuild. A rebuild replaces the whole file.
- **Curated**: `org.md`, `setup.md`, `inbox.md`, `memories/`, `reviews/`. Edited in place by replacing the file; keep `org.md` and `memories/` short by updating lines rather than appending forever. Reviews are append-only: an update replaces the file with the old content verbatim plus the new section.
- **Working**: a file in `documents/` that the AI maintains for the members, with a fixed name and no timestamp (`greta-diapers.csv`, `packing-list.md`). Updated by replacing it in full (rules → Replace). Two shapes:
  - **Tracker**: CSV, header row then one row per event (`date,time,author,…`), oldest first. For frequent events a Convention routes here instead of the diary. A tracker row is source data: change rows only to correct them, at a member's request.
  - **List**: Markdown the AI writes or regenerates on request (a printable list, a memo, a checklist). It may be rewritten freely.
  Every working file was created with a diary entry that lists it in `documents:` and says `Maintained: documents/<name>`; an index rebuild lists every such file under Maintained.

Templates for every file: [references/templates.md](references/templates.md).

## Setup checklist

`setup.md` records the onboarding of the root and of each member as a checklist, so a setup left halfway resumes where it stopped. Its front-matter `state` is `done` when every box is ticked, else `todo`. Only the `setup` skill writes it. The `review` skill flags its unticked boxes; every other skill leaves it alone. A root set up before the checklist existed has no `setup.md` until `setup` next runs there.

## Conventions

The defaults in these rules and in every skill hold unless `org.md` → Conventions says otherwise. Before applying a default to an instance, check Conventions for a line whose Where matches it (`greta/health`, `greta`, or `*`); the most specific line wins. A Convention is written only when a member asks for it or answers a suggestion, with the date and who answered. A declined suggestion is recorded too ("declined"), so it isn't suggested again.

### Suggest

Suggest a working file or Convention when you notice a pattern: several similar entries in one instance within a day, a request for a list, memo or something printable, or a member keeping their own sheet or note for something Agora tracks. One short question with the concrete proposal ("Want me to keep Greta's nappies in a tracker file instead of one diary entry each?"); on yes, create the working file, write the Convention, and log one diary entry saying so.

### Frequent events

Events routed to a tracker get no diary entry each. The daily `review` writes one summary entry per day for that instance (counts, anything unusual), so status and history stay readable.

## Working with Drive

- **Read** raw content by downloading the file. The text-extraction view escapes Markdown; use it only for PDFs, images and Office files.
- **Create** Markdown with content type `text/markdown` and conversion to Google Docs disabled, so files stay plain `.md`.
- **Reference** files by path from the root (`nina/health/documents/…`), never by Drive ID: IDs change on every replace. Folders keep theirs; `root_id` and `drive_id` are the only Drive ids Agora records.
- Look up folders by walking from the root by name, one level at a time.
- **In a shared drive**, files carry a `sharedDriveId` that differs from `drive_id` (the id of the drive's top folder, from its URL). To check that a file is in the drive, walk its parents up to `drive_id`; never compare it with `sharedDriveId`.

### Replace

Drive cannot overwrite a file's content, and allows two files with the same name. To change a derived or curated file:

1. Find the current file by parent folder + exact name.
2. Create the new version in the same folder.
3. Trash every older file with that name in that folder.

A trash refused with a permission error leaves two copies, and names its cause before rules → When something fails:
- basic mode: the connected account doesn't own the file, and in My Drive only the owner can trash. Fix (prerequisites item 3): reconnect as the assistant account, or have the file's owner transfer it to the assistant account; or, with Google Workspace, move the root to a shared drive (the `setup` skill's Move to shared drive).
- shared services mode: the member isn't a Content manager of the shared drive (prerequisites item 5).

Once fixed, the duplicate goes as below.

Duplicates found at any time:
- derived or curated file, or a list: keep the newest, trash the rest;
- tracker: merge all copies into one (every distinct row, in date/time order), then trash the rest;
- `inbox.md`: merge as [references/email.md](references/email.md) → The ledger says;
- `routine-` diary entries with identical content, or diary entries for the same email (references/email.md → Mailboxes) with identical content: keep one, trash the rest;
- any other source file (diary, uploaded documents): keep all and flag them.

## Status update rule

A status describes the situation **now**. When rebuilding, read the sources newest first and let recent entries outweigh old ones: a newer entry that settles, cancels or reschedules something overrides the older one. Old facts survive only while they still matter (a standing allergy, an unpaid invoice).

Every `status.md`, at every level, has the same sections in this order: Last updated, Needs attention, Summary, Done, Pending, Next. At root and interest level each section rolls up the level below, one line per child that has something to say.

An instance with email entries carrying a `To file:` line: also apply [references/email.md](references/email.md) → Filing. An instance with open to-dos attached lists them under Pending ([references/todos.md](references/todos.md) → In status).

Tracker rows reach status through the daily summary entry; propagate on a row only when it changes something status-worthy.

**Propagate** after any change to an instance: rebuild its `status.md`, then its interest's `status.md`, then the root `status.md`. Rebuild the instance `index.md` on every change to the instance. Rebuild an interest or root `index.md` only when its list of children or a child's description changed; the `review` tidy-up refreshes their Last activity column.

A derived file is **stale** when a source below it is newer than its Last updated line (`status.md`), or when its rows differ from the folders and files actually present or from each child's newest entry (`index.md`).

## Ask, never assume

When the root, interest, instance, member, or date is unclear, ask the user before writing. Offer the likely options from the indexes so answering is one word. Ask once for the whole batch rather than once per item.

### Identity

At the start of each conversation, know who you are talking to, unless it is already known in this conversation:

- **Shared services mode**: the speaker is the connected account (Vocabulary). Match its email against the Members Email column, and ask nothing. No match: say this Google account isn't an Agora member yet and offer to join (the `setup` skill's Join, unless that is what is running); write nothing else.
- **Basic mode**: ask who you are talking to (list the members from `org.md`, leaving out github members).

A handoff names who is talking in its line (rules → Handoffs), and an unattended routine uses the slug in its prompt, in both modes. Every diary entry and document carries that member's slug.

### Unattended runs

Scheduled routines have nobody to ask. They act only on what is unambiguous and **flag** the rest.

### Flag

Add a line under Needs attention in the root `status.md` saying what was found and what question or manual fix settles it. Interactive runs flag too, when an action fails and needs a manual fix; they also tell the member. Every rebuild of the root `status.md` keeps each flag whose condition still holds and drops the resolved ones.

## Handoffs

A **handoff** is a one-line prompt in a fixed shape, sent by another plugin's skill (`cruna-pm-github`'s `agora-sync`), that runs one Agora skill **unattended**: everything the skill would ask is in the line. A message that starts `Agora <skill> as ` is a handoff.

Handoff shapes v1: a change to any of them is breaking; announce it in the release notes and ship it with `cruna-pm-github`.

```
Agora log as <slug> in <root>: <interest>/<instance> — github: <ref>[, <ref>…] — <entry text>
Agora todo as <slug> in <root>: edit <to-do id> text "<new text>"
Agora todo as <slug> in <root>: done <to-do id>
Agora add-interest as <slug> in <root>: products/<instance> — <name>: <description>
```

The separators are ` — ` (em dash between spaces) and the first `: `; the last field runs to the end of the line, so it may hold either. A `<ref>` is an issue, PR or tag as GitHub writes it (`#8`, `PR #9`, `v2026.11.12`).

Every handoff follows these rules; each skill's Handoff section adds its own:

- **Who and where.** `<root>` is matched against each root's `name` in `org.md` or its folder name (rules → Finding the root). `<slug>` must be a github member of that root; only `add-interest` may create it. The interest, instance and to-do the line names must already exist, except what `add-interest` creates.
- **Check, then write.** Settle every check (shape, root, slug, instance, to-do) before the first write. Any check that fails: write nothing.
- **Skipped steps.** No identity question, memories read, calendar step, to-do offer, pattern suggestion or memory step, and no question of any kind: where the skill would ask, the handoff fails instead. Everything else (placement checks, templates, Replace, propagation) runs as usual.
- **Language.** Summary lines and any text Agora writes are in the root's language. The text the line carries is kept verbatim (in a diary Entry, a to-do's text, a description).
- **Result.** The whole reply is one line, in English, with no greeting: `agora: ok — <what was written>` or `agora: failed — <reason>`. A connector failure is `agora: failed — <what failed>`, naming anything already written, in place of rules → When something fails.

## Calendar conventions

- The root's **system calendar** is `calendar` in `org.md`, always addressed by that id (`calendarId = calendar`). Basic mode: the assistant account's email, which is its primary calendar's id. Shared services mode: the shared calendar's id. Write only there. Members' personal calendars are read-only: use them to find free time, at the level the Members Personal calendar column records (`free/busy`, `full details`, or `domain free/busy`, Workspace's free/busy read by the member's Email).
- **The speaker's own calendar**, per mode. Shared services mode: the connected account is the speaker's own (Identity), so their own calendar is read in full, whatever their Personal calendar column says: for free time, for "what do we have" questions, and for Yours in their personal review. Basic mode: only at the level their Personal calendar column records, so its events only at `full details` (free/busy gives busy blocks, no events). An unattended run reads it in full only when its connected account is the member the run is for.
- Event title: `[<interest>/<instance>] <title>` (`[nina/health] Blood test`).
- Event description: the instance path and the diary entry that caused the event.
- Color: the interest's color from `org.md`, when one is set.
- Anything that must be remembered at a date becomes an event: appointments, deadlines, reminders, prep time.

## Email

A root has email when `org.md` says `email: on`. With `email` absent or `off`, no skill reads or writes mail, `inbox.md` does not exist, and everything works without Gmail. With email on, the assistant inbox works like `landing/`: members forward or CC mail to it, and the AI files what it can place without guessing and flags the rest.

Every Gmail search is filtered as The three ids → Mail says. Before any email work, read [references/email.md](references/email.md): the email rules every skill that touches the assistant inbox follows.

## To-dos

A **to-do** is kept as to-do events in `todos/`, folded into its current state; `todo.md` is the derived list members read. Members change to-dos only in conversation, through the `todo` skill; routines only suggest.

Before reading or writing any to-do, `todo.md`, or a review's To-dos section, read [references/todos.md](references/todos.md): the event format, the fold, owner and place, due dates, suggestions, reviews, remove and archive.

## Memory step

Close every interaction by asking yourself: did I learn something durable about a member, the group, or how they like to work? If so, update `memories/<slug>.md` or `memories/shared.md` following the `remember` skill. Read the relevant memories before answering or writing, and let them shape tone and defaults.

## When something fails

A missing connector, a permission error, or a folder or calendar you cannot reach is a setup problem, not a data problem. Stop, tell the user what failed in one sentence, and walk them through the matching item in [references/prerequisites.md](references/prerequisites.md).
