# proud-ciabatta — Agora to-dos: grilled idea

Request: [#4 Agora keeps personal and shared to-do lists in TODO.md](https://github.com/Cruna-Noise-To-Signal/cruna-plugins/issues/4). Grilled 2026-11-03 (one session, rounds Q1–Q17).

## Where it started

Agora has no to-do list: open actions only surface through the reviews. The request asked for a `TODO.md` working file with personal (per member) and common to-dos, managed in conversation and read and updated by reviews; Google Tasks as a possible backing store. Open points at filing: one file or one per member, new skill or extension of `log`/`review`, Google Tasks availability on Claude and ChatGPT.

## Where it concluded

To-dos are **append-only event files** in a root `todos/` folder, and `todo.md` is a **derived** list rebuilt from them, so concurrent writes never lose a to-do. A new `todo` skill manages them in conversation; routines only list and suggest. To-dos can come from email (#2), so this request depends on it. Google Tasks is out of scope.

## Platform facts (checked 2026-11-03)

- Neither Claude nor ChatGPT has an official Google Tasks connector or app, and Tasks is not documented as reachable through the Calendar connector. The only route is a third-party MCP server as a custom connector, which may not run in scheduled tasks.
- Google Tasks lists are personal per account and cannot be shared (outside Google Chat spaces, which the API likely does not reach).
- Caveat: support.claude.com and help.openai.com were not directly reachable; the main sources are vendor blogs (usecarly.com, Jun–Oct 2026) that sell their own Tasks integration. Read as "not documented", not "confirmed absent".

## Decisions

### Storage

1. **Source records (Q1, Q8)**: a root `todos/` folder of append-only event files `<author>-YYYYMMDD_HHMM.md`, one action per file on one to-do: `add`, `done`, `reopen`, `assign`, `edit`, `remove`. A to-do's id is the name of its `add` file. Front-matter: `owner`, `due` (optional), `interest`/`instance` (optional), `source` (link to a diary entry, email or review).
2. **`todo.md` (Q1, Q9)**: root-level, **derived**, never hand-edited. Rebuilt by `todo` after every change and by tidy-up when stale (a lost concurrent write is fixed by the next rebuild). Layout: one section per member (overdue, then by due date, then undated), an **Everyone** section, **Recently done** (last 14 days). Each line: to-do, owner, due, instance link, source link. Lowercase name, like every other structure file.
3. **Folder creation (assumption, confirmed)**: `setup` creates `todos/` when it builds the root; `todo` creates it on first use in an existing root. No new `setup.md` box.

### Who sees and owns what

4. **Place (Q2)**: attached to an instance when one is clear, root-level otherwise; adding a to-do never asks where it goes. An attached to-do also shows in that instance's `status.md` **Pending**.
5. **Personal vs common (Q3)**: owner is a member or `everyone`. All to-dos are visible to all members (Drive is shared; `setup` and `help` say so). Anyone can assign or take over a to-do; each change is recorded with who made it.
6. **Dates (Q4)**: a `due:` field only. A calendar event is created only when a member asks, through `schedule`.

### Skill shape

7. **New skill `todo` (Q5)**: add, complete, reopen, assign, edit, list, remove. It asks who it is talking to once per conversation, like `log`, and ends with the memory step.
8. **Additions to other skills (Q5)**:
   - `rules`: `todos/` and `todo.md` in the layout and file kinds, event file format, templates;
   - `review`: To-dos section, tidy-up rebuild and monthly archive, suggestions;
   - `ask`: answers "what's on my list?" from `todo.md`;
   - `help`: to-do FAQ;
   - `log`: when an entry sounds like an action, offers to add a to-do (never adds silently).

### Removing and archiving

9. **Remove (Q13, Q14, Q15)**: a `todo` verb.
   - Filters, combinable: by date (created or closed up to a date), by interest/instance, owner, `everyone`, source (e.g. from email).
   - Scope: closed to-dos by default; open ones only when the member says so, listed separately in the confirmation.
   - Confirmation: lists the matches (count and titles) and waits.
   - Action: **trashes** the matching event files in Drive; the confirmation says they are recoverable for 30 days, then gone. Writes a `remove` event file (who, filter, count) as a trace.
   - Routines never remove.
10. **Automatic archive (Q13)**: at the monthly review, tidy-up moves the event files of to-dos closed more than 90 days ago into `todos/archive/YYYY/`. Nothing is deleted.

### Routines and reviews

11. **Routines only list and suggest (Q6)**: "looks done: X (diary entry Y)" and "possible new to-do: Z (diary entry or email)" go under Needs attention; a member confirms in conversation. Routines never create, complete, reassign or remove a to-do.
12. **To-dos section (Q12)** in every review (daily, weekly, monthly): overdue; due in the period ahead; opened and completed since the last review; suggestions (also under Needs attention). A personal review shows that member's to-dos first, then Everyone; the general review shows every list. The monthly review also lists to-dos open more than 30 days with no due date, as a prompt to drop or date them.

### Email (requires #2)

13. **Hooks (Q10, Q11)**:
    - In conversation, `email` offers "add a to-do?" when an email asks for an action; `source` is the email (Gmail id and its `inbox.md` row).
    - An explicit instruction in member mail ("add to my to-dos: …") creates the to-do directly: a member wrote it, so it counts as confirmed (unlike sending, which never runs on an emailed instruction).
    - A routine's inbox tidy-up never creates to-dos from third-party mail; it suggests "possible to-do: Z (email from X)" under Needs attention.
    - `inbox.md` gains the outcome `to-do → <id>`.
    - The spec states "requires #2"; implementation starts from `dev` after the email PR has merged.

### Limits and release

14. **Google Tasks (Q7)**: out of scope, recorded in `future-extensions.md`. Why it waits: no official connector on either platform, lists cannot be shared, custom connectors may not run in routines. To build: an official Tasks connector on both platforms that works in scheduled runs; then a one-way mirror of each member's personal to-dos into their own Tasks list.
15. **Release check (assumption, confirmed)**: two members complete the same to-do in overlapping conversations; `todo.md` ends rebuilt correctly with no lost event.
16. **Version (Q17)**: 0.5.0 (minor, new skill), both languages, after #2 ships 0.4.0.

## Vocabulary to add (spec and `glossary-it.md`)

- **To-do**: one open or closed action, owned by a member or `everyone`. Italian: "to-do" (invariable); "attività" is taken by "attività pianificata".
- **To-do event**: one append-only file in `todos/` recording one action on one to-do.
- **`todo.md`**: the root's derived to-do list.
- Stay in English (Q16): `todo.md`, `todos/`, `todos/archive/`, the skill `todo`, the actions `add`/`done`/`reopen`/`assign`/`edit`/`remove`, the front-matter keys `owner`/`due`/`source`, the headings `To-dos`, `Everyone`, `Recently done`.

## For the spec

Update `docs/agora/agora-spec.md`: Drive layout (`todos/`, `todo.md`), Derived vs source files (to-do events are source, `todo.md` is derived), `status.md` Pending (attached to-dos), skill table (`todo`; `log`, `ask`, `review`, `help` additions), Routines (tidy-up rebuild and monthly archive, To-dos section, suggestions), Release checks, Out of scope (Google Tasks → `future-extensions.md`). Both language versions; `glossary-it.md`; version 0.5.0 on both.
