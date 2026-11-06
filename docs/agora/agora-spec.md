# Agora — Design Spec

Status: locked (2026-10-28). Source: design interview on the original vision notes. Email added in 0.4.0 (2026-11-03, request #2 eager-rosetta). To-dos added in 0.5.0 (2026-11-03, request #4 proud-ciabatta). Products from GitHub added in 0.6.0 (2026-11-05, request #3 sleepy-focaccia). Two modes, with shared services mode, added in 0.7.0 (2026-11-06, request #11 earnest-tozzetto).

## Goal

A scalable way for a small group (1 to ~5 people) to contribute knowledge to shared interests so that contributions build on each other and drive actions and decisions.

Tools:

- **Shared filesystem**: Google Drive (the assistant account's My Drive, or a shared drive).
- **Shared calendar**: Google Calendar (the assistant account's primary calendar, or a shared calendar).
- **Assistant inbox** (optional, per root): Gmail, the mail delivered to the root's `inbox` (the assistant account's address, or a Google Group's).
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
- **Mode**: basic shared account mode or shared services mode (see Modes, accounts and calendar).
- **Member**: a Google account with an AI client attached, a person or an AI (its kind changes nothing), or a github member.
- The word "concept" is not used.

## Drive layout

```
ROOT/
├── org.md        mode and the three ids, members, layout convention, assistant account, calendar, language, interest colors, access list
├── setup.md      onboarding checklist: state todo/done, boxes per root step and per member
├── index.md      one row per interest
├── status.md     roll-up of interests + links to the latest reviews; the **Tidied:** line
├── inbox.md      email on only: ledger of what the AI did with each email
├── todo.md       the to-do list, derived from todos/
├── todos/        <author>-YYYYMMDD_HHMM.md to-do events; archive/YYYY/
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

- Members: name, role, kind, birthday, how they interact with the system, personal calendar sharing level.
  - **Email**: basic mode, the personal Google account the folder and calendar are shared with; shared services mode, the member's Workspace account, which identifies them.
  - **Kind** (`human` | `ai`, missing means `human`): recorded for the organisation's own reasons; it changes nothing in permissions, routines, reviews or to-dos. A Members table written before the column keeps its shape: new rows follow its header, and every member reads as `human`.
  - **Personal calendar**: `free/busy` or `full details` shared with the assistant account (basic mode); `domain free/busy` (Workspace's free/busy, read by the member's Email) by default in shared services mode, or `full details`.
- Layout convention (what an interest is), offered as the default when creating new interests.
- **Mode and the three ids** (front-matter, 0.7.0; each optional, a missing one reads as basic mode, so an `org.md` written before 0.7.0 reads exactly as before):
  - `mode: basic-shared-account | shared-services`, missing means `basic-shared-account`;
  - `drive_id`: the id in the shared drive's URL (its top folder, the one that works as a parent), or `my-drive` (the default);
  - `root_id`: the root folder's id; missing, the root is found by searching for `org.md`;
  - `inbox`: the address Agora reads mail from; missing means `assistant_account`;
  - `calendar_owner`: shared services mode only, the slug of the member who created the shared calendar.
- Assistant account (basic mode: dedicated, or the owner's own account). A new shared services root has none; a root moved from basic mode keeps it for the record.
- System calendar, `calendar`: basic mode, the assistant account's email, whose primary calendar it is; shared services mode, the shared calendar's id. Optional color per interest.
- Language.
- Access list: who has Drive and calendar access (used for offboarding). In shared services mode it records what the real-call checks proved: Drive `content manager`, System calendar `can edit`, and `receives mail` in Notes.
- `email: on | off` (absent means off), and per member a **Mail from** column: every address the member writes or forwards from (email on only). Root-wide email criteria are Conventions rows (see Email).

### `setup.md`

The onboarding checklist, so a setup left halfway resumes where it stopped (in any app). Front-matter `agora: setup` and `state: todo | done` (done when every box is ticked).

- **Root** boxes: prerequisites, interview, build, email, verify. The Email box is ticked "(off)" when the root declines email. A `setup.md` written before 0.4.0 has no Email boxes: it reads as email off, and the root box is added ticked "(off)" on the next write, so a done root never flips back to `todo`.
- One **Member** section per member who uses the root: connectors and plugin, in `org.md` with a memories file, root folder shared, system calendar shared, personal calendar shared, routines, and (email on only) email: Gmail connected as the account prerequisites item 7 names, addresses and importance state recorded.
  - In shared services mode the sharing boxes give way to admin-side boxes, each unticked one naming who does it: `Workspace account` (admin), `In the shared drive` (admin), `In the group` (admin; `(email off)` with email off), `Shared calendar` (the calendar owner), then connectors and plugin, in Members and Access with a memories file, email and routines. They are ticked when the member's own real-call checks pass in their app.
- The interview answers, round by round, until the build is done (the email answers stay until every interviewed member's Mail from cell and importance line are written); a Migration section while a migration runs; a `Move to shared drive` section while a move runs (one box per step, step 4's box recording the new root's id and, after a copy, the old root's).

`setup` creates it as soon as the root name is confirmed (with the root folder), ticks each box when its step is done, and resumes at the first unticked box. Finding the root also looks for `setup.md`, so a root without `org.md` yet is found as an unfinished setup. The review flags unticked boxes under Needs attention. Only `setup` writes it.

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

`author` is a lowercase first-name slug. Basic mode: the AI asks who it is talking to once per conversation (members listed from `org.md`). Shared services mode: the speaker is the connected account, matched against Members Email with no question; an account with no match is offered Join and nothing is written.

An entry for an email (filed or sent, see Email) adds `email` (Gmail message id), `email_from` (the original sender, or `to <recipients>` for sent mail) and `email_subject` after `events`, and, when the email has attachments not yet on Drive, a last `To file: <names>` line in Entry.

### `status.md`

Same sections at every level (root, interest, instance), different zoom. At root and interest level each section rolls up the level below, one line per child.

1. Last updated
2. Needs attention
3. Summary
4. Done
5. Pending
6. Next (with dates and links to calendar events)

Root `status.md` also links the latest daily, weekly, and monthly review, and carries a `**Tidied:** YYYY-MM-DD HH:MM` line under Last updated once a review tidy-up has written it (see Routines). Only the tidy-up writes it; a root `status.md` without it is fine.

An instance's Pending lists each open to-do attached to it, one `To-do: <text> — <owner>, due <date | no date>` line each (see To-dos).

**Status update rule**: less recent logs and documents weigh less. At each update the status is re-evaluated against the latest information to reflect the current situation.

### `index.md`

One level only (progressive discovery). A table of direct children: name, one-line description, last activity, link.

At instance level: documents (description, source diary entry) and diary entries (date, author, one-line summary).

### Derived vs source files

- **Source of truth, append-only**: `diary/`, `documents/`, and to-do events in `todos/`.
- **Derived, always rebuildable**: `status.md`, `index.md` at every level, and `todo.md`. Never hand-edited. A lost concurrent write is fixed by the next rebuild.
- **Curated**: `org.md`, `setup.md`, `memories/`, `inbox.md`. Kept small; the AI updates entries instead of appending forever.

### File update protocol

The Drive connector cannot overwrite file content and Drive allows duplicate names. Every update of a derived or curated file:

1. Find the file by folder + name.
2. Create the new version (`text/markdown`, no conversion to Google Docs).
3. Trash the old version(s).
4. If duplicates are found, keep the newest and trash the rest (also done by tidy-up).

Files are referenced by path, never by Drive file ID. Read raw content via download, not the "read" view (which escapes Markdown). Trashed versions stay recoverable for 30 days.

### Working files

A fourth file kind, next to source, derived and curated: a document in an instance's `documents/` that the AI **maintains** and updates in place (by replacing it in full).

- Fixed name, no timestamp: `documents/<name>.<csv|md>` (`greta-diapers.csv`, `packing-list.md`).
- Kinds: **trackers** (CSV, one row per event, for frequent events: nappies, feeds, expenses) and **lists/memos** (Markdown, regenerated from the diary on request, e.g. a printable list).
- Listed in the instance `index.md` as maintained.
- CSV for now; Google Sheets is a future extension. An existing Google Sheet can be imported once into a CSV tracker.

### Conventions

The defaults hold unless `org.md` says otherwise. User-requested exceptions live in a **Conventions** table in `org.md` (where, convention, since), which every skill reads before applying a default. Memories are about people; conventions are about how data is handled.

- **Frequent events**: when a convention routes events to a tracker, each event is a tracker row, not a diary entry. The daily review writes one summary diary entry per day for that instance.
- **Proactive**: the AI suggests a working file or convention when it notices a pattern (several similar entries in a day, a "list" or "printable" request). It writes the convention only after the user confirms; a declined suggestion is recorded so it isn't repeated.

## Modes, accounts and calendar

**Same answer**: the same question gets the same answer from any connected account. A root has one of two **modes** (`mode` in `org.md`):

- **Basic shared account mode** (`basic-shared-account`, the default whenever `mode` is missing): several people's AI clients sign in to one Google account, the **assistant account**. Works on free Gmail; the families' setup, unchanged since 0.1.
- **Shared services mode** (`shared-services`, Google Workspace only): each member's AI client signs in as that member's own Workspace account, and every member reaches the same three shared services: the root folder inside a **shared drive** (every member a Content manager, so anyone can trash anything), a **shared calendar**, and a **Google Group** whose address is the inbox. No shared password. A member is a Google account with an AI client attached; its **kind** (`human` | `ai`) changes nothing.

### The three ids

Every skill, in both modes, reaches the root only through the three ids in `org.md` (rules → The three ids):

- **Files**: every Drive call stays under `root_id` (when present; else the root found by searching for `org.md`). `root_id` must be the folder holding `org.md`: when it isn't (a root copied or moved by hand), writes stop and Verify is offered. In a shared drive, files carry a `sharedDriveId` that differs from `drive_id` (the URL id): a file is checked as in the drive by walking its parents up to `drive_id`, never by comparing `sharedDriveId`.
- **Calendar**: every write, and every read of Agora events, uses `calendarId = calendar`, never the client's default calendar. In basic mode that is the assistant account's email, already its primary calendar's id, so nothing changes.
- **Mail**: every Gmail search carries the **mail filter**; a message outside it is read or acted on only when a member names it in the conversation.
  - Basic mode: received mail is `deliveredto:<inbox>`. Sent mail has no Delivered-To header, so a search that must also find what was sent ("did we answer Rossi?") uses the sent-mail form, `(deliveredto:<inbox> OR from:<inbox>)`.
  - Shared services mode (the founder's decision, #11, 2026-11-06): `(list:<list-id> OR to:<inbox> OR cc:<inbox>)`, where `<list-id>` is `inbox` with `@` replaced by `.` (for `assistant@crunasignal.com`: `list:assistant.crunasignal.com OR to:assistant@crunasignal.com OR cc:assistant@crunasignal.com`). `list:` finds what others sent to the group; `to:`/`cc:` find the member's own posts, which Groups never files back into their mailbox with the List-ID header. The same form serves as the sent-mail form; the received-mail form adds `-from:me`.
  - The filter is Agora's own rule, **not a permission boundary**: in shared services mode each member's AI can technically reach that member's whole mailbox and calendar.

Only `setup` and `help` branch on the mode themselves. Every other per-mode difference is stated once in `rules` (Identity, Replace, the speaker's own calendar, the mail filter, sending and mailboxes, the prerequisites) and pointed to.

### Accounts

- **Basic mode**:
  - Multi-member root: a dedicated Google account owns the root folder; its primary calendar is the system calendar. Solo root: the owner's own account plays the role; a migrate step in `setup` moves to a dedicated account when someone joins.
  - Every member's AI client connects **Google Drive** and **Google Calendar** as the assistant account, plus **Gmail** when the root has email on.
  - Each member is an editor of the root folder (to upload and browse from their own Google account); receives the system calendar (edit rights if they want to add events by hand); shares their personal calendar with the assistant account, free/busy only or full details (their choice, recorded in `org.md`).
  - Assistant account recovery uses a real member's phone and email. Mind the 15 GB free storage.
  - **Ownership check** (0.7.0): in My Drive only a file's owner can trash it, so a connector signed in as another account breaks every Replace. Prerequisites item 3 reads `org.md`'s owner and blocks on a mismatch, with the fixes (reconnect as the assistant account; the owner transfers the files, as after a migration that moved only the folder; or move to a shared drive). Verify, `help` (troubleshooting) and the `review` tidy-up run the same check, and a refused trash during Replace names the mismatch.
- **Shared services mode**:
  - Each member's connectors sign in as their own Workspace account. Sharing is membership, done by the admin and the calendar owner by hand (see Manual setup); the Drive connector never shares the root folder. Agora checks each step with a real call: create and trash a test file under `root_id`, create and delete a test event on `calendar`, and a received-mail search with the group's mail filter that returns results.
  - A refused trash during Replace names its cause: the member isn't a Content manager of the shared drive.
  - The shared calendar is a secondary calendar created by hand, ideally by the admin, recorded as `calendar_owner`. A secondary calendar is deleted with its owner's account, so offboarding the calendar owner is **blocked** until the calendar is transferred and `calendar_owner` updated.
- **Identity**: basic mode asks who is talking. Shared services mode reads the connected account (the primary calendar's id in the calendar list) and matches it against Members Email, never asking; no match offers Join and writes nothing. Unattended runs and handoffs use the slug in their line in both modes.
- **Connectors only**: the AI acts on Drive, Calendar and Gmail exclusively through the connectors' tools, never through a browser (no web interface, browsing or computer-use tools). What no connector tool offers is click steps for the user.
- **Connector reach**: in basic mode the Drive connector shares the root folder, so setup does it itself and retries until the folder's permissions list every member. The Calendar connector can't share calendars: sharing the system calendar and personal calendars is always click steps, and setup says so at the start. No connector creates shared drives, groups or calendars.
- **System calendar**: one per root, `calendar` in `org.md`: the assistant account's primary calendar (basic), or the shared calendar created by hand (shared services); never created by Agora. Event title `[interest/instance] title`; description links the instance folder and diary entry. Colors (max 11 in Google Calendar) are optional, assigned per interest in `org.md`. Everything that must be remembered goes through the calendar.
- **Personal calendars and the speaker's own calendar**: personal calendars are read-only, for free time, at the level the Members Personal calendar column records. In shared services mode the connected account is the speaker's own, so their own calendar is read in full for free time, for "what do we have" questions and for Yours in their personal review (an unattended run only when its connected account is the member the run is for); everyone else's at `domain free/busy` unless they share more. In basic mode the speaker's calendar is read only at its recorded level. (This widens the request's original "free time only" to every own-calendar read.)
- **Offboarding**: basic mode, revoke Drive and calendar access, disconnect the client (Gmail too when email is on), change the assistant password if they knew it, update `org.md`. Shared services mode: see Manual setup → A member leaving.

## Manual setup (shared services mode)

No connector creates shared drives, groups or calendars, or shares calendars, so these steps are done by hand, by the person each one names; Agora checks each one with a real call. The same guide ships in the plugin as `rules/references/manual-setup.md` (prerequisites item 4, shared services variant), which `setup` walks people through. Menu names change between versions: look for the words below if a path doesn't match. Paths checked against Google's and the clients' help pages on 2026-11-06.

### Admin, once per root

1. **Workspace account for each member** (admin): admin.google.com → **Directory** → **Users** → **Add new user** → first name, last name, primary email → **Add new user**. Check: the member signs in and item 3's Who check shows it.
2. **Shared drive** (admin, or a member allowed to create shared drives): drive.google.com → **Shared drives** → **New** → name it after the root → **Create**. Open it → **Manage members** → each member's Workspace email → **Content manager** (the lowest role that can trash) → **Send**. Copy the address bar's URL (`https://drive.google.com/drive/folders/<id>`); setup takes the id from it. Check: create then trash a test file at the drive's top folder.
3. **Google Group** (admin): a group only its members can see and join, that anyone, clients included, can email.
   1. Once per organisation: Admin console → **Apps** → **Google Workspace** → **Groups for Business** → **Sharing settings** → tick **Group owners can allow incoming email from outside the organization** → **Save**; leave **Accessing groups from outside this organization** at **Private**. Off by default; without it client mail never reaches the group.
   2. Admin console → **Directory** → **Groups** → **Create group** → name (the root's), group email (e.g. `agora@acme.it`), owner the admin → **Labels**: **Mailing** (not Security) → **Access type**: **Restricted** → in the access table, row **Who can post**, also tick **External** → **Who can join the group**: **Only invited users** → **Allow external members**: off → **Create group**. Ticking External turns the access type to **Custom**: that is the intended final state (Restricted's other rows stay: only members see conversations and the member list). groups.google.com shows the same setting as **Who can post: Anyone on the web**. These are the founder's settings for the test group (issue #11).
   3. groups.google.com → the group → **Members** → **Add members** → each member's Workspace email → **Subscription: Each email** (no digests) → **Directly add members** → **Add members**.
   Check: prerequisites item 7 (shared services): a received-mail search with the group's mail filter returns results; for a new group, after a test email from an address outside the organisation, which also proves External posting.
4. **Shared calendar** (ideally the admin, recorded as `calendar_owner`): calendar.google.com → **Other calendars** **+** → **Create new calendar** → name after the root, the root's **Time zone** → **Create calendar**. **Settings** → the calendar → **Share with specific people or groups** → each member's Workspace email at **Make changes to events** (newer screens: **Make changes and see event details**) → **Send**. **Integrate calendar** → copy the **Calendar ID** (`…@group.calendar.google.com`). Check: create then delete a test event on that id.
5. **Connectors enabled for the organisation** (the AI client's admin): Claude Team or Enterprise, an Owner or Primary Owner turns on Google Drive, Google Calendar and Gmail in **Organization settings** → **Connectors**. ChatGPT Business, a workspace admin enables the Drive, Calendar and Gmail apps in **Workspace settings** → **Apps** and turns on write actions for Gmail. Check: each member finds the three connectors in their own list.

### Admin, per new member, before they join

The admin, and the calendar owner for the calendar; each ticks the box of the same name in the member's `setup.md` section once the joiner's own check passes:

1. Their Workspace account (step 1). Box `Workspace account`.
2. Add them to the shared drive as **Content manager** (step 2). Box `In the shared drive`.
3. Add them to the group, subscription **Each email** (step 3.3). Box `In the group`.
4. The calendar owner shares the calendar with them at **Make changes to events** (step 4). Box `Shared calendar`.
5. The connectors enabled, when their AI plan has an organisation admin (step 5).

### The joining member

In their own AI app, signed in as their Workspace account:

1. Install the Agora plugin.
2. Connect Google Drive, Google Calendar and (email on) Gmail, each signed in as their Workspace account.
3. Accept the shared calendar's invitation email, so the calendar shows on their phone. (Added in implementation; the request's list had steps 1, 2 and 4.)
4. Say "join Agora". Join checks each admin step with a real call and names each missing step and who does it, all at once: Workspace account (the connected account is on `inbox`'s domain), shared drive (create and trash under `root_id`; not found means not in the drive, refused trash means not a Content manager), calendar (create and delete on `calendar`), group (item 7's check), connectors. A shared drive failure blocks the rest; any other failure is recorded and Join carries on. Join then writes the Members row (Email = the connected account, Kind, `domain free/busy`), the Access row with what was proved, `memories/<slug>.md`, the Member section, the email member part and routines. It shares nothing and creates no interest folder.

### A member leaving

1. **Calendar owner first**: when the member is `calendar_owner`, everything waits until the calendar is transferred: Google Calendar → **Settings** → the calendar → **Shared with** → beside the new owner, **Transfer ownership**; the new owner accepts from the email within 60 days (or the admin transfers it in the Admin console). `calendar_owner` is updated, the new owner confirms the calendar is under their "My calendars", and the calendar check passes again. No override.
2. Remove them from the shared drive (**Manage members** → **Remove access**), the group (**Members** → **Remove member**) and the calendar's sharing (**Shared with** → **×**).
3. The admin suspends or deletes their Workspace account (Admin console → **Directory** → **Users** → **Suspend user** or **Delete user**). Files in the shared drive belong to the drive and stay.
4. Ask them to disconnect their connectors and delete their routines.
5. Update Members (`removed YYYY-MM-DD`) and Access in `org.md`, and remove their `setup.md` section. A retired assistant address in their Mail from (after a Move) moves to another member's Mail from, so mail from it stays trusted.

### Moving a basic root (the old account's Gmail)

The old assistant account forwards a copy of its incoming mail to the group, set by clicks in its Gmail settings; the click path is in `setup/references/move.md` step 7.

## Move to shared drive (basic → shared services)

A `setup` branch for a basic root whose group gets Google Workspace (`setup/references/move.md`), run by the owner in their own app and tracked as a `## Move to shared drive` section in `setup.md`, one box per step, resuming at the first unticked one. Members leave Agora alone from step 4 until step 5 is ticked, since the old root and its copy both hold `org.md` meanwhile.

1. The admin's setup (Manual setup, once per root), with a Workspace account for every member who uses the root.
2. Hand-over, still as the assistant account: the Drive connector shares the root folder with the owner's Workspace account as editor; the assistant account shares its calendar with that account at See all event details (clicks). A solo root already on the owner's Workspace account skips this and step 3.
3. The owner reconnects as their Workspace account; item 3's shared services checks pass.
4. The root into the shared drive. `landing/` is filed first (a copy resets Drive creation times, which name documents). **Move** (`update_file` to parent `drive_id`) when the connected account owns every file; refused, or otherwise, **copy**: rebuild the tree by name and copy every file under the same name, never touching an original. A **count check** compares both trees folder by folder; the originals are trashed only in step 8, after it passes.
5. `org.md` (`mode`, `drive_id`, `root_id`, `calendar`, `calendar_owner`, `inbox`; `assistant_account` kept for the record; Members Email = Workspace accounts, `domain free/busy`; Access) and `setup.md` (shared services boxes) in the new root.
6. Future `[interest/instance]` events copied to the new calendar with title, time, description, color and recurrence (a series once, from its next occurrence); the old calendar is left alone.
7. Forwarding (email on; Manual setup → Moving a basic root), checked by a test email to the old address showing up in the group search; the old address goes into the moving member's Mail from.
8. The old root trashed by the assistant account (clicks; the Workspace account can't trash files it doesn't own), checked by a search for `org.md` finding the new root only.
9. The old account retired: no connector signs in as it and it isn't a member, but it is kept alive so its forwarding runs. Each other member runs Join.
10. Linked repos: every `products/<instance>` managed by `cruna-pm-github` is listed; a developer reruns `pm-setup` in each repo so `.agora/pm.yml` names the new `drive_id` and `root_id` (cruna-development-plugins#29).

Then Verify in shared services mode.

## Plugin: `agora`

Shipped in English and Italian, for Claude and ChatGPT with the same design (full connector parity assumed).

| Skill | Responsibility |
|---|---|
| `rules` | Shared reference every other skill loads first: vocabulary, layout, file kinds, Drive file protocol, status update rule, "ask, never assume", calendar conventions, memory step. Holds the templates and the prerequisites checklist. |
| `setup` | Prerequisites check (real calls), interview (members/org chart, layout, first interests, email), build folders (`todos/` included) and calendar, guide routine creation. Asks whether the group uses Google Workspace: no is basic mode, yes is shared services mode. Also: solo → dedicated account migration (basic only), Move to shared drive (basic → shared services), new member onboarding (Join), turning email on in an existing root, member offboarding; Join, Offboard and Verify per mode. Records progress in `setup.md` and resumes from it. |
| `add-interest` | Create an interest or instance from the template; update indexes. |
| `log` | Push a diary entry; rebuild status up the tree; offer a to-do when an entry reads as an action. |
| `file-document` | Index a file already in `documents/`, or move one from `landing/` (move → else copy and remove → else flag); link it to a diary entry, including an email entry's attachment. |
| `schedule` | Create, update, delete calendar events with the naming convention; link from the diary. Also for events inferred from pushed information (meetings, prep time). |
| `email` | Optional (email on): search and read the assistant inbox; draft, reply, forward and send on confirmation; file an email as a diary entry; offer a to-do from an email, and add one a member's mail asks for; keep the `inbox.md` ledger; members' addresses; importance criteria in conversation. |
| `todo` | Add, complete, reopen, assign (take over), edit, list and remove to-dos in conversation; one to-do event per change, then rebuild `todo.md` and attached instance statuses; calendar events only on request, through `schedule`. |
| `ask` | Pull: index → status → diary/documents/calendar, and the assistant inbox for mail questions when email is on; to-do questions from `todo.md`; answers on topic and up to date. |
| `review` | Routine body: tidy-up first (with the inbox step when email is on, and the to-dos step), then the review with its To-dos section; attended, confirms to-do suggestions. |
| `remember` | Update `memories/`, including the `Email importance:` state line. Every other skill ends with this step. |
| `help` | What the assistant can do (email and to-dos included, with their limits), FAQ for both modes ("where are my files" per mode, which kind of Agora, moving to a shared drive), troubleshooting from the prerequisites checklist per mode, starting from the review's overdue-routine and connected-account flags. |

"Push artifacts from a pull session" is `ask` followed by `log` or `schedule`, not a separate skill.

Shared references, written once in the `rules` skill (skills can only link files inside their own folder, so shared material lives in a skill the others load):

- **Rules**: file formats, status update rule, file update protocol, "ask, don't assume".
- **Email conventions** (`rules/references/email.md`): every email contract, stated once (see Email).
- **To-do conventions** (`rules/references/todos.md`): every to-do contract, stated once (see To-dos).
- **Prerequisites checklist**: used by `setup` and `help`; any skill hitting a missing connector or permission error points to it.
- **Manual setup** (`rules/references/manual-setup.md`, 0.7.0): the shared services clicks for the admin and for each member, joining and leaving (see Manual setup). Prerequisites item 4 and Offboarding point to it; "item 4, step N" means its step N.

Setup's own references: `setup/references/shared-services.md` (0.7.0) holds what each setup step does in shared services mode, one section per step (2a, 2b, 2c, 2e, 3 Join, 6 Offboard, 7 Verify), with one pointer per step in `SKILL.md`; `setup/references/move.md` (0.7.0) is the Move to shared drive branch.

### Prerequisites checklist

`setup` runs it first, verifying each item with a real call and giving click-by-click instructions per platform for anything missing. Items keep their numbers; an item with a per-mode difference has a Basic mode and a Shared services mode variant, and the basic text is unchanged but for the ownership check.

1. AI client on a paid plan (needed for scheduled tasks).
2. The plugin installed.
3. Connectors: what a connector is, which are needed (Google Drive, Google Calendar, plus Gmail when email is on), how to enable them.
   - Basic: they log in as the assistant account (or the owner's own account when solo). The Drive check also confirms the connected account owns `org.md`; a mismatch is blocked, with the fixes.
   - Shared services: they sign in as the member's own Workspace account. Who: the primary calendar's id is that account. Drive: create and trash a test file under `root_id` (before the root exists, at `drive_id`). Calendar: create and delete a test event on `calendar`.
4. Basic: assistant account created, with recovery set up. Shared services: the admin's setup, the Manual setup guide.
5. Sharing.
   - Basic: root folder shared with members as editors (by the AI, through the Drive connector); system calendar shared with members and personal calendars shared with the assistant (by hand).
   - Shared services: sharing is membership: every member a Content manager of the shared drive, the calendar shared at Make changes to events by the calendar owner, personal calendars at Workspace's domain free/busy (more detail optional). Checked by item 3; the Access row records what was proved.
6. Scheduled tasks: where to create them on each platform (done at the end of setup).
7. Gmail connector (email on only; numbered last so the earlier numbers stay stable). Limits: on ChatGPT, sending works only from the web on paid plans, and a Business workspace needs an admin to turn on write actions; on Claude Team or Enterprise, an Owner must enable the Gmail connector.
   - Basic: checked with a real search call that confirms the mailbox is the assistant account's. The AI sees only the assistant inbox.
   - Shared services: signed in as the member; a received-mail search with the group's mail filter (The three ids) must return results. For a new group, the member sends a test email to the group from an address outside the organisation, then searches again. The AI reads only group mail, by Agora's rule.
- **Offboarding**: basic, revoke access as the assistant account; shared services, Manual setup → A member leaving.

New members joining later run steps 1–3, 5 (their own sharing), 6 and, when email is on, 7; in shared services mode, once item 4's steps for them are done.

### Push flows

1. **Diary log**: `log`.
2. **File**: plugins cannot upload chat attachments to Drive, so the user either
   - (a) puts the file in the instance `documents/` folder and tells the AI → `file-document` indexes it;
   - (b) puts it in `landing/` and tells the AI → `file-document` moves and indexes it;
   - (c) does (a) or (b) without telling the AI → the routine's tidy-up picks it up.
3. **Reminder or appointment**: `schedule`.
4. **Push back from a pull session**: `ask` then `log`/`schedule`.
5. **New interest**: `add-interest`.
6. **Email** (email on): forward or CC member mail to the assistant inbox → `email` in conversation, or the routine's inbox tidy-up, files it (see Email).

Every push keeps `status.md` (always) and `index.md` (when needed) up to date.

## Routines

Plugins cannot ship scheduled tasks on either platform; `setup` guides each person through creating them. Three per person (fits plan limits):

| Routine | When | Content |
|---|---|---|
| Daily | Morning | Yesterday in review + today ahead |
| Weekly | Sunday evening | Week in review + week ahead |
| Monthly | 1st of the month | Month in review + month ahead |

Each scheduled task's prompt is one line: `Run Agora review: <daily | weekly | monthly> [for <slug>] [in <root name>]`. A prompt in this shape is an unattended run. Setup gives each member routines with their slug; without one the review is general.

Each run:

1. **Tidy-up**: rebuild stale status and index files, clean duplicates, process `landing/` and unindexed documents (only when unambiguous; otherwise flag under root Needs attention), trash daily reviews older than 30 days; with email on, the inbox tidy-up (see Email → Reviews); with `todos/`, the to-dos step: monthly archive of to-dos closed over 90 days ago, orphan events flagged, `todo.md` and attached statuses rebuilt when stale (see To-dos). Idempotent, so concurrent runs by different members are harmless.
2. **Review**: the first run of a period writes the shared review to `reviews/`. Later runs read it and add an `## Update hh:mm` section only if new diary entries arrived. The chat message is **general** by default: everyone's highlights and what needs attention, followed by a short update for each member. When a member is named (the routine's slug, or "my week" in conversation) it is **personal** instead: their items first, then everyone else's. A review asked in conversation doesn't ask who is talking unless a member is named. With email on, the review file has an `## Email` section and the chat message ends with at most one email-importance line (see Email → Reviews). With `todos/`, the review file has a `## To-dos` section and the message carries each member's overdue and due to-dos and the suggestions (see To-dos → Reviews).

Weekly and monthly reviews are kept as the long-term record.

Added in 0.7.0, in both modes (routine prompts unchanged):

- **Recently tidied**: the tidy-up writes `**Tidied:** YYYY-MM-DD HH:MM` on the root `status.md` when it finishes. A run that finds the line **less than 60 minutes old** skips tidy-up items 1–6 (another member's routine has just tidied), keeps the line and the root's earlier flags as found, and still runs items 7–9 and writes or updates the review and the message. A root `status.md` without the line tidies as before.
- **Overdue routines** (tidy-up item 8, every run). Under Needs attention, with the fix "No member's routine has run this: set up the routines (setup → routines)":
  - **A missing review file.** For each period, the file named after the day, ISO week and month containing **yesterday**. A file is written by the end of its period's first day (the weekly runs the Sunday evening before, the monthly on the 1st, and files are named after the period containing tomorrow), so a missed run is flagged the next day, and a routine that hasn't run yet today raises nothing (the 07:30 daily on the 1st or on a Monday flags nothing).
  - The file this run writes is left out: an attended weekly or monthly run mid-period writes the very file it checks.
  - **A new root has nothing due before its first review file**: while `reviews/` is empty, nothing is flagged; otherwise every period whose first day is on or before the day the oldest file in `reviews/` was created is left out. The oldest file is dated by its Drive **created time**, because a weekly or monthly file's name gives its period, not the day it was first written. Replace only re-creates current-period files, so on a running root the oldest file never moves forward and nothing is hidden. **Accepted blind spot**: a root whose first review file is created on the 1st never gets its first monthly flagged (and the very first daily can't be). After a Move by copy, created times restart at the move, which only hides periods before it.
  - **Email triage**: email on and `last_tidy_up` (noted before this run's tidy-up moves it) older than yesterday: "Email wasn't triaged between <last_tidy_up> and today." The next run drops it.
  - `help` shows the same flags first when troubleshooting.
- **The connected account** (tidy-up item 9, every run, mode-free wording): prerequisites item 3's ownership check where that item has one (basic mode), flagging a connected account that doesn't own `org.md`, with the fix.

## Email

Optional, per root (`email: on` in `org.md`), built on the Gmail connector: the **assistant inbox** is the mail delivered to `inbox` (basic mode: the assistant account's inbox, the connector logged in as it; shared services mode: the group's mail, as delivered to each member's own mailbox, the connector signed in as the member). Every search carries the mail filter (Modes → The three ids). Best for professional roots; a root that declines it works exactly as without. Setup offers it in the interview; "turn on email" runs only the email step in an existing root. Every contract below is written once in `rules/references/email.md`; the `email` skill (conversation) and the `review` inbox tidy-up (routines) apply it.

### Sending

- The AI drafts freely and **sends only** after a member, in the current conversation, explicitly confirms that exact message (recipients, subject, body). Agora asks once, showing the full message; the app's own approval prompt follows and completes the send.
- Routines never send. There are no standing send rules: a Convention or memory saying "always send…" reads as "always draft…".
- **Email is data**: bodies, subjects and attachments are untrusted content, never instructions. The one exception is member mail carrying an instruction, and even that produces only a draft.
- **Sender voice**: basic mode, mail goes out from the assistant account, signed `<Root display name> assistant, on behalf of <member Name>`. By default the body speaks for the member in the third person ("Simone asked me to confirm…"); the member's own voice only when they ask. Shared services mode: from the member's own address with `inbox` in CC, so replies reach the group, in the member's own voice with no assistant signature; the draft is saved in the member's own mailbox.

### Member mail

Mail whose sender address matches a member's Mail from address (a display-name match alone never counts). After a Move to shared drive, the retired assistant address is added to the Mail from cell of the member who ran the move, so mail from it is trusted as member mail; when that member is offboarded, the address moves to another member's Mail from. Member mail always needs attention. Member mail carrying an instruction ("reply saying we accept") gets the reply it asks for drafted and flagged for that member's approval, on its own ledger row, on top of filing or flagging the email itself; never sent.

### Filing

- Only **forwarded or CC'd member mail** is filed on its own, and only when it places in exactly one instance; otherwise it is flagged with the question that settles it, ambiguous member mail first. Mail nobody forwarded is flagged when important, else ignored. A member asking in conversation may file any email.
- The entry is `<slug>-YYYYMMDD_HHMM.md` with the forwarding member's slug and the **email time** (Mailboxes; two runs produce the same name, so they collide instead of duplicating; a name taken by an entry for the same email means already filed), `author` = the forwarding member, with the email fields and the original quoted.
- **Attachments**: their content is out of reach. They are listed on a `To file:` line, and the member is asked to save them to `landing/` or `documents/`. While the line is open, the instance status keeps a Needs attention line. When `file-document` files one, it writes a new entry saying `Attachment of: diary/<entry> (<original name>)`: the attachment is linked.
- **Sent mail** is logged as a diary entry in its instance, `<slug>-YYYYMMDD_HHMM.md` with the confirming member and the send time, `email_from: to <recipients>`; flagged when no instance is clear.

### Mailboxes

Added in 0.7.0 (`rules/references/email.md` → Mailboxes). A Gmail message id belongs to one mailbox. Basic mode has one, the assistant account's; in shared services mode each member reads the group's mail in their own mailbox, so one email has a different id in each copy. The RFC 822 `Message-ID` was rejected as a key: the Gmail connector exposes it only in the `RAW` format.

- **Your mailbox**: the connected account's.
- **Email time**: basic mode, the received time. Shared services mode, the time in the **Date header**, converted to the root's timezone for names: every member's copy shares it, while each mailbox stamps its own received time. (This replaces the request's "received time" for shared services mode.) Diary names, member-mail to-do names and the duplicate key use it.
- **The same email**: the same Gmail id in the same mailbox, or the same **duplicate key** from **another** mailbox: sender address, email time to the minute, subject; for a row Agora sent, the sender is the Email of the member in By, so Agora mail a member sent with the group in CC isn't filed again from another member's copy. Basic mode has one mailbox, so there the Gmail id alone decides, exactly as before 0.7.0. Two different emails from different mailboxes sharing a key count as one; accepted, as it is rare. The ledger skip and merge, filing's "already filed", the to-do "already added" and the duplicate-diary rule all use this definition.
- **Opening an id from another mailbox** (a ledger row whose Mailbox isn't yours, a diary `email:` id, a to-do `email <id>` source): **by id first**, then, not found, a search with the sent-mail filter, `from:<sender>` and subject words, on that day. (This replaces the request's "the diary entry's author's mailbox", which isn't reliable: a routine in one member's mailbox can file an entry authored by the forwarding member.)
- **Solo** is basic only: a shared services root with one member still reads only the group's mail.

### The ledger: `inbox.md`

Root, curated, front-matter `agora: inbox` and `last_tidy_up: <date>`. One row per handled email, newest first: Gmail message id, date, from, subject, outcome (`filed → <diary path>`, `flagged`, `drafted`, `sent → <diary path>`, `ignored`, `to-do → <id>`), by (member slug or `routine`), and (0.7.0) **Mailbox**: the slug of the member whose mailbox holds the row's id; empty in basic mode and in rows written before the column, both meaning the assistant inbox. In shared services mode **Date** is the email time to the minute (`YYYY-MM-DD HH:MM`), for the duplicate key; basic rows keep the date only.

- Only emails the AI filed, flagged, drafted, sent or ignored get a row; reading mail in conversation adds none.
- A reply or forward to a ledgered email that is not `filed` goes on that email's row and keeps its id. Any other written email (a new one, a reply to a filed email, the draft a member-mail instruction asks for) gets its own row, `from` = `to <recipients>`, keyed on its draft id and rekeyed to the sent message id on send.
- An email already in the ledger (the same email, Mailboxes: a row from another member's mailbox counts) is skipped. A later outcome replaces the row's outcome, by and date. Written once per conversation or run, by replace; copies merge into one row per email (latest outcome) and the latest `last_tidy_up`.
- `last_tidy_up` is the date of the latest review inbox tidy-up, written by every tidy-up (even one that logs no row), never by conversations. Rows older than 60 days are dropped by the tidy-up.

### Importance

- An email **needs attention** when it is member mail, or when it asks for an action, reply or decision, or carries a date or deadline, and is not bulk mail. Criteria refine this default; they never drop member mail.
- **Root-wide** criteria are Conventions rows (`important email: <criteria>`, Where `*` or an interest); `memories/shared.md` then says `Email importance: see Conventions`, so the criteria have one source. **Proactive drafts** are opt-in Conventions rows too (`draft replies to <senders or interest>`). **Personal** criteria are the member's state line.
- **State line**: `Email importance: not asked | invited | no preference | <criteria>` in each `memories/<slug>.md` and in `memories/shared.md`. Conversations judging importance ask once when it is `not asked` or `invited`; `no preference` is never asked again. Routines never ask: they invite (`not asked` → `invited`), and a reply to the invitation, in the review's chat or a later conversation, is recorded as criteria. When a member corrects a judgement, the AI offers once to record the refinement.

### Reviews

- **Inbox tidy-up**, every review run, ledger read first:
  1. Search mail received (the received-mail filter, so nothing Agora sent) since `last_tidy_up`, at most 60 days back (the prune keeps 60 days of rows, so anything handled before is still listed). With no `last_tidy_up`, search 60 days back with no flag. When `last_tidy_up` is more than 60 days old, flag the unchecked gap under Needs attention; the flag is a status item, not stored state, and the next tidy-up drops it.
  2. Skip ledgered mail (two runs on the same day handle each email once).
  3. Decide each email, first match wins: file forwarded/CC'd member mail that places (not in a solo root); draft a reply when a `draft replies to …` Convention covers it; flag other mail needing attention; ignore the rest. Member mail carrying an instruction also gets its draft. These two cases are the only drafts a routine writes: by default routines flag, never draft proactive replies.
  4. Prune rows older than 60 days, set `last_tidy_up`, write the ledger.
- Every flag and routine draft also goes to Needs attention.
- The review file's `## Email` lists email needing attention (`flagged` rows), drafts waiting for approval (`drafted` rows), and what was sent and filed in the look-back window.
- The chat message, never the review file, ends with at most one importance line: the invitation (unattended, state `not asked` or `invited`), or, in the monthly review, the current criteria with "reply to change it" (quieter for `no preference`). Attended runs ask directly instead.

### Solo roots

Basic mode only. The assistant inbox is the owner's own mail: the AI files only when the owner asks in conversation, and member mail does not apply. After migrating to a dedicated account, the AI sees only mail sent or forwarded to the new address; `inbox.md` carries over unchanged.

### Limits

Agora relies only on what Gmail connectors offer on both platforms: search and read, drafts, send with approval. It never reads attachment content, never moves, labels, archives or deletes mail. It reads only mail the mail filter finds (plus a message a member names in the conversation): in basic mode that keeps members' personal inboxes out of reach; in shared services mode the connector could reach the member's whole mailbox, and the filter is Agora's own rule.

## To-dos

Actions someone has to do, personal (owned by a member) or shared (owner `everyone`). Every to-do is visible to every member: there is no private to-do. Every contract below is written once in `rules/references/todos.md`; the `todo` skill (conversation), the `log`, `email` and `ask` hooks and the `review` tidy-up apply it. A root without `todos/` has no to-dos and works as before; `setup` creates the folder at build, and `todo` creates it on first use in a root set up before 0.5.0.

### To-do events and `todo.md`

- Every change is one append-only **to-do event**, `todos/<author>-YYYYMMDD_HHMM.md` (a `-2`, `-3` suffix when the name is taken), front-matter `agora: todo`, `action` (`add | done | reopen | assign | edit | remove`), `author`, `created` to the second. An `add` carries `owner`, optional `due`, `interest`/`instance` and `source` (a path, or `email <Gmail message id>`), and a one-line body; other actions carry `todo` (the id: the `add` file name without `.md`) and the changed fields; a `remove` carries the filter, count and ids, as a trace only.
- **Current state**: fold a to-do's events in `created` order; the last `done` or `reopen` decides open or closed, and each field takes its latest value. Events whose `todo` matches no `add` are orphans, left out and flagged by tidy-up.
- **`todo.md`** (root, derived, `agora: todo-list`): one `## <Name>` section for each member who owns an open to-do (no empty member sections), then `## Everyone`, then `## Recently done` (closed in the last 14 days). Open to-dos are in list order: overdue, then by due date, then undated. Rebuilt by replace after every change, re-listing `todos/` just before writing; stale when an event is newer than its Last updated.
- **Place**: attached to an instance when one is clear, else root level; adding never asks where. An open attached to-do shows in its instance's Pending. A due date creates no calendar event; a member asks for one through `schedule`.

### Who changes to-dos

- Only a conversation creates, changes or removes to-dos, through the `todo` skill: a member asked, or said yes to an offer from `log`, `email` or a review suggestion.
- **The one exception**: member mail asking in so many words to add a to-do ("add to my to-dos: call the bank") counts as the member's own request. It is the **only to-do event an unattended run writes**: the review's inbox tidy-up adds it, `author` the sender, named and dated from the email time (Email → Mailboxes) so two runs add it once, ledger outcome `to-do → <id>`.
- Any other mail, and every routine, only **suggests**: "looks done" (a later diary entry or email shows it was done) and "possible new to-do" (a diary entry or email asks someone to act), in the review's To-dos section and under Needs attention. Routines never complete, reassign or remove a to-do.
- A github member's `edit` and `done` events through the `todo` handoff are the second exception (Products from GitHub).

### Reviews

Every review has a `## To-dos` section: overdue, due in the look-ahead window, opened and done in the look-back window, suggestions; the monthly review adds open to-dos with no due date older than 30 days. Personal reviews show the member's to-dos first, then Everyone's; the general review each member's, then Everyone's. An attended review asks once to confirm its suggestions, each yes going through `todo`.

### Remove and archive

- **Remove** (conversation only): filter by date, instance or interest, owner, `everyone` or source, combined; closed to-dos only unless the member says "including open ones"; list the matches and wait for a yes; trash every event file (Drive trash, 30 days), never hard-delete; write one `remove` event and rebuild.
- **Archive**: the monthly tidy-up moves the events of to-dos closed more than 90 days ago into `todos/archive/YYYY/` (the closing year). Idempotent. Archived to-dos leave `todo.md` and reviews; `ask` searches the archive only when asked about old to-dos.

## Products from GitHub

The Agora half of a bridge to GitHub. The GitHub connectors in the chat apps can't read issues, PRs or Projects, nor write them (Verified platform facts → GitHub connectors), so the `agora-sync` skill of `cruna-pm-github` (in `cruna-development-plugins`) writes the developers' news into Agora and files the founder's notes and to-dos as GitHub issues. Agora only offers it three handoffs and treats product work like any other work. Roots without linked repos see nothing of it, and `setup` never mentions GitHub.

- **Github member**: an `org.md` Members row with Role `github` (a reserved value of the existing column), slug from the handoff line (usually `pm`), Name "PM (GitHub)", Email, Mail from, Birthday and Personal calendar empty, Notes "address for GitHub through cruna-pm-github". An address meaning "this goes to GitHub", not a person: it authors bridge diary entries and to-do events, and owns to-dos only when a member names it. It is left out of Identity and gets no `setup.md` section, memories file, Access row, calendar, routines or personal review message; the general review gives it a section with its open to-dos only. `setup` Offboard removes it with its managed Conventions rows.
- **`products/`**: an ordinary interest ("Software products built in GitHub", no color), one instance per linked repo, named after the repo, described as `<name>: <description>` from its README. The folder is `products` in every language.
- **Managed instance**: marked by a Conventions row, Where `products/<instance>`, Convention `managed by cruna-pm-github: notes and to-dos for pm become GitHub requests` (text after the marker in the root's language), Since `<date>, <slug>` like every Conventions row. A member's note logged there gets the reply "noted; it becomes a GitHub request at the next sync"; a to-do for the github member attached to it is filed as an issue, then edited to end `→ #<issue>` and closed when the issue closes.
- **Bridge entries**: diary entries authored by the github member, one per merge into dev, release or drop, with an optional front-matter key `github:` after `events` listing the refs (`"#8"`, `"PR #9"`, `"v2026.11.12"`). Summary in the root's language, Entry verbatim from GitHub; statuses, indexes, reviews and `ask` (which cites the refs) treat them like any entry.
- **Bridge file**: `<interest>/<instance>/YYYY-MM-DD_hh_mm_last-check.md` belongs to `cruna-pm-github`. Agora never reads, indexes, files, flags or trashes it, and a stale-index check ignores it.

### Handoffs

A handoff is a one-line prompt in a fixed shape that runs an Agora skill **unattended**; the shapes live once in `rules` → Handoffs and are repeated in each skill's Handoff section. Handoff shapes v1: any change is breaking and ships with `cruna-pm-github`. The lines and the result line stay in English in the Italian plugin.

```
Agora log as <slug> in <root>: <interest>/<instance> — github: <ref>[, <ref>…] — <entry text>
Agora todo as <slug> in <root>: edit <to-do id> text "<new text>"
Agora todo as <slug> in <root>: done <to-do id>
Agora add-interest as <slug> in <root>: products/<instance> — <name>: <description>
```

- **Who and where**: `<slug>` must be a github member of `<root>` (found by `org.md` name or folder name); only `add-interest` creates it. Everything else the line names must exist.
- **Check, then write**: a malformed line or a missing root, slug, instance or to-do writes nothing. No question of any kind: where a skill would ask, the handoff fails. No identity, memories, calendar, to-do offer, pattern or memory step; placement checks, templates, Replace and propagation run as usual.
- **Result**: one English line, `agora: ok — <what was written>` or `agora: failed — <reason>`.
- **`log`**: one diary entry, author the slug, `events: []`, `github:` from the line, then index and status propagation.
- **`todo`**: matches by id only and writes one `edit` (text) or `done` event, then rebuilds `todo.md` and the attached statuses. Done on a closed to-do or an archived id fails; an edit to the same text writes nothing and returns ok.
- **`add-interest`**: idempotent steps, in order: the member row, `products/`, the instance (or an update of its description when it exists by exact name), the Conventions row, one `org.md` write, propagation. A second call creates nothing twice.

Removal path: once the connectors read issues and PRs, the `log` handoff loses its caller; once they write, the `todo` handoff does too. The github member, `products/` and the Conventions row stay.

## Memories

Captured during conversations (no platform lets a scheduled run read past chats):

- every skill ends with a memory step: did I learn something new about this person or the group?
- an explicit `remember` skill for "remember this".

Stored in `memories/<member>.md` and `memories/shared.md`, human-readable and editable. With email on, each holds one `Email importance:` state line (see Email → Importance).

## Verified platform facts (Claude Drive connector, 2026-10-28 live test)

- Creates folders and sub-folders, also inside folders made by hand in the Drive UI.
- Writes real `.md` files (`text/markdown`, conversion disabled).
- Reads and moves files created by hand.
- Renames and moves files via `update_file`.
- Cannot overwrite content; creating a file with an existing name makes a duplicate.

## Verified platform facts (GitHub connectors, 2026-11-04 founder check)

- On Claude and ChatGPT, the GitHub connectors only attach repo files to a chat (as context, or as project content). They don't read repos, issues, PRs or Projects on their own, and they can't write. Source: the founder's check, comment on request #3 (2026-11-04) and its grilling record. Re-check before building on the connector route.

## Verified platform facts (shared services, 2026-11-06 founder check)

- ChatGPT's Gmail connector accepts `deliveredto:` and `list:` searches.
- Group mail in a member's mailbox (founder's check, #11, 2026-11-06): what others send to the group carries the List-ID header (`list:<group-with-@-as-.>`), while the member's own posts are not filed back into their inbox (same message id) and carry no List-ID there. Hence the shared services mail filter `(list:<list-id> OR to:<inbox> OR cc:<inbox>)` (The three ids), which replaces `deliveredto:` in that mode.
- The AI client creates and deletes events on the shared calendar addressed by its id (`calendarId`), without it being the default calendar.
- On `cruna-shared-resources` a shared drive has two ids: the URL id (the drive's top folder, which works as a parent and in `parentId` searches) and the `sharedDriveId` on every file inside. `drive_id` holds the URL id. A search for `org.md` finds files in shared drives without naming the drive.
- Google Groups needs the Admin console setting **Group owners can allow incoming email from outside the organization** (off by default) before a group can take outside posting.

Still to verify live (0.7.0 manual test): the mail filter forms (the shared services form and the basic sent-mail form) on both connectors; that the primary calendar's id is the account's email on ChatGPT; that the Drive connector exposes `owners` and reads a shared drive's members; and whether `update_file` can re-parent a My Drive folder into a shared drive (Move falls back to copy either way).

## Release checks

Require the assistant account (basic mode), unless a line names a shared services root:

- Move a file uploaded to `landing/` by a different Google account.
- Live ChatGPT smoke test of the full flow (Drive writes, calendar create/update/delete on a non-primary calendar).
- Email (0.4.0): the manual test [manual-tests/manual-test-agora-v040-email.md](manual-tests/manual-test-agora-v040-email.md) on Claude and ChatGPT, with email on, as the assistant account. Re-verify the ChatGPT send limits and whether a reply to a scheduled review's chat runs with the plugin and connectors.
- To-dos (0.5.0): the manual test [manual-tests/manual-test-agora-v050-todos.md](manual-tests/manual-test-agora-v050-todos.md) on Claude and ChatGPT, as the assistant account, with email on for the email scenarios.
- Products from GitHub (0.6.0): the manual test [manual-tests/manual-test-agora-v060-products.md](manual-tests/manual-test-agora-v060-products.md): the handoffs in Claude Code (claude.ai login, Google Drive connector), the chat scenarios on Claude and ChatGPT, and the Italian root on one of them. Release before `cruna-pm-github`'s `agora-sync` (cruna-development-plugins#27), which calls these handoff shapes.
- Shared services (0.7.0): the manual test [manual-tests/manual-test-agora-v070-shared-services.md](manual-tests/manual-test-agora-v070-shared-services.md), with a shared services root on the `cruna-shared-resources` shared drive (two Workspace accounts, one `kind: ai`, a test group, a shared calendar), Move to shared drive on a copy of a basic root, and one Italian scenario on one platform.
- **Basic-mode non-regression (0.7.0), run by the founder before release**: an untouched basic root, email on, files unchanged, gives the same results as on 0.6.0 for Verify, a daily `review` with email on, `schedule`, `email` and `ask`, including an explicit **search for sent mail** ("did we answer <someone>?", the basic sent-mail filter form). It is its own section of the 0.7.0 manual test.

## Out of scope (future extensions)

See [future-extensions.md](future-extensions.md): Google Sheets trackers, memory reflection routine, local filesystem roots, email attachments, Gmail labels and archiving, Gmail-triggered tasks, Google Tasks, documents for products, GitHub comments back to notes, products in another org's Project, product events outside an agent session.

Out of scope for email: members' personal inboxes, sending without an in-conversation confirmation (standing send rules included), and email providers other than Gmail.

Out of scope for to-dos: Google Tasks as a store or mirror, private to-dos, calendar events created automatically from due dates, recurring to-dos, subtasks, priorities or tags, routines creating, completing, reassigning or removing to-dos (beyond the member-mail add), and hard deletion of to-do events.

Out of scope for shared services (0.7.0): `cruna-pm-github` reading `drive_id` and `root_id` (cruna-development-plugins#29; Agora only lists the repos to update after a move); a separate hourly email-triage routine; a run record with claims; creating shared drives, groups or calendars, or sharing calendars, through connectors; mailbox privacy beyond Agora's own search filter; Join creating the member's interest folder in per-person layouts (pending the founder's check); moving a shared services root back to basic mode; shared drives outside Google Workspace, or members from outside the domain.

Out of scope for products from GitHub (see future-extensions.md): documents for products, GitHub comments or issue state flowing back to a note, repos whose Project lives in another org, events done outside an agent session, any GitHub connector use from the chat apps, a "business" flag or GitHub question in `setup`, and handoffs for other skills.
