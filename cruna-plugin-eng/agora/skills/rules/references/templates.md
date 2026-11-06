# Agora file templates

Fill every `<placeholder>`. Headings stay in English; content follows the language in `org.md`. Keep each file short: a member should read any `status.md` in under a minute.

## `org.md` (root, curated)

```markdown
---
agora: root
name: <Root display name>
language: <en | it | …>
timezone: <Europe/Rome>
mode: <basic-shared-account | shared-services>
drive_id: <my-drive | the id in the shared drive's URL>
root_id: <the root folder's id>
assistant_account: <basic mode: assistant@gmail.com | owner's own account>
calendar: <basic mode: the assistant account's email, its primary calendar's id | shared services mode: the shared calendar's id>
calendar_owner: <shared services mode: slug of the member who created the shared calendar>
inbox: <basic mode: the assistant account's email | shared services mode: the group's address>
layout: <what an interest is, e.g. "one interest per person, plus `family` for shared things">
email: <on | off>
---

# <Root display name>

## Members

| Slug | Name | Email | Mail from | Role | Kind | Birthday | Personal calendar | Notes |
|---|---|---|---|---|---|---|---|---|
| simone | Simone | simone@example.com | simone@example.com, simone@acme.it | parent | human | 1989-03-28 | free/busy | prefers short answers |
| pm | PM (GitHub) | | | github | | | | address for GitHub through cruna-pm-github |

## Interests

| Interest | Color | Description |
|---|---|---|
| nina | Peacock | Everything about Nina |

## Conventions

| Where | Convention | Since |
|---|---|---|
| greta/health | Nappies: one row per change in `documents/greta-diapers.csv` (date,time,author,type,notes); no diary entry per change; daily summary entry | 2026-10-29, simona |
| products/cruna-plugins | managed by cruna-pm-github: notes and to-dos for pm become GitHub requests | 2026-11-05, pm |

## Access

| Member | Drive | System calendar | Client connected | Notes |
|---|---|---|---|---|
| simone | editor | can edit | Claude | routines on |
```

`email` absent means `off`. The fields `mode` to `inbox` may each be missing, and a missing one reads as basic mode: `mode` is `basic-shared-account`, `drive_id` is `my-drive`, the root is found by Finding the root, and `inbox` is `assistant_account`; `calendar_owner` exists only in shared services mode. So an `org.md` written before these fields reads exactly as before. A new shared services root is written without `assistant_account`; a root moved from basic mode keeps it for the record.

**Email**: basic mode, the personal Google account the folder and calendar are shared with; shared services mode, the member's Workspace account, which their connectors sign in as and which identifies them (rules → Identity). **Kind**: rules → Vocabulary → Member. A Members table written before the Kind column keeps its shape: write every new row, the github row too, with the columns its header has, and read every member in it as `human`. **Personal calendar**: `free/busy` or `full details` shared with the assistant account (basic mode); `domain free/busy` by default in shared services mode, or `full details` when the member shares more. **Mail from** (email on only) lists every address the member writes or forwards from: mail from these is member mail (rules → Email). Root-wide email criteria are Conventions rows, written as in [email.md](email.md) → Where criteria live.

A **github member** row (rules → Vocabulary) is written only by the `add-interest` handoff, as the `pm` row above, in the columns of the table's header: Role `github`, Name "PM (GitHub)", Email, Mail from, Kind, Birthday and Personal calendar empty; it gets no Access row. The same handoff writes the managed Conventions row, Where `products/<instance>`, with the member's slug in place of `pm`.

In shared services mode the Access columns record what the real-call checks proved (prerequisites items 3, 5 and 7): Drive `content manager`, System calendar `can edit`, and `receives mail` in Notes once the group search returns their mail.

## `setup.md` (root, curated)

One `## Member: <slug>` section per member who uses the root (a child only tracked, or a github member, gets none). In a solo root, tick the two sharing boxes with "(solo)". Unticked boxes say who does them when it isn't you.

```markdown
---
agora: setup
state: <todo | done>
---

# Setup: <Root display name>

**Updated:** <YYYY-MM-DD HH:MM> by <slug>

## Root
- [x] Prerequisites
- [ ] Interview
- [ ] Build
- [ ] Email
- [ ] Verify

## Member: simone
- [x] Connectors and plugin
- [ ] In Members and Access, with a memories file
- [ ] Email
- [ ] Root folder shared
- [ ] System calendar shared (simone accepts the invitation)
- [ ] Personal calendar shared (free/busy)
- [ ] Routines

## Interview

### The root
- <confirmed answers, one line each>
```

In shared services mode a Member section carries these boxes instead; each unticked one names who does it (the admin, the calendar owner, the member). Workspace account, In the shared drive and Shared calendar are ticked when the member's item 3 checks pass in their own app; In the group when item 7's check passes, or "(email off)" in a root with email off.

```markdown
## Member: marta
- [ ] Workspace account (admin)
- [ ] In the shared drive (admin)
- [ ] In the group (admin)
- [ ] Shared calendar (calendar owner: simone)
- [ ] Connectors and plugin
- [ ] In Members and Access, with a memories file
- [ ] Email
- [ ] Routines
```

The root's Email box is ticked "(off)" when the root declines email, or when email is on and Gmail is checked and `org.md` and `memories/shared.md` carry the email fields. A Member section has an Email box only when email is on; it is ticked when the member's app has Gmail connected as the account item 7 names (the assistant account, or in shared services mode their own), and their addresses and importance state are recorded.

The Interview section holds each confirmed round under its own heading (The root, Members, Layout convention, First interests and instances) until the Build box is ticked; then it is removed, except the email answers, which stay until every member interviewed has their Mail from cell and Email importance line written (setup step 2d). While a migration runs, a `## Migration` section lists its seven steps as boxes.

## `inbox.md` (root, curated)

Email on only. One row per handled email, newest first; `last_tidy_up` is the date of the latest review inbox tidy-up, absent until the first one (rules → Email → The ledger).

```markdown
---
agora: inbox
last_tidy_up: 2026-11-03
---

# Inbox

| Message id | Date | From | Subject | Outcome | By | Mailbox |
|---|---|---|---|---|---|---|
| 18c2f0a9b7e4d311 | 2026-11-03 | Mario Rossi <mario@acme.it> | Quote approved | filed → acme/website-2026/diary/simone-20261103_0912.md | routine | |
| 18c2ef11a0c3b920 | 2026-11-02 | to mario@acme.it | Re: Kickoff date | sent → acme/website-2026/diary/simone-20261102_1640.md | simone | |
| 18c2ee70d1f2a004 | 2026-11-02 | Newsletter <news@shop.com> | Black Friday | ignored | routine | |
| 18c2ed02b4a1c877 | 2026-11-02 | Simone <simone@example.com> | add to my to-dos | to-do → simone-20261102_0805 | routine | |
```

**Mailbox** is the slug of the member whose mailbox the row's Gmail message id belongs to, written in shared services mode, where each member reads the group's mail in their own mailbox. It is empty in basic mode and in rows written before the column; both mean the assistant account's inbox.

## `index.md` (derived)

Root lists interests; an interest lists its instances.

```markdown
# Index: <root | interest>

| Name | Description | Last activity |
|---|---|---|
| [health](health/) | Visits, tests, vaccines | 2026-10-28 |
```

Instance level lists documents and diary entries, newest first:

```markdown
# Index: <interest>/<instance>

## Documents

| File | Description | From entry |
|---|---|---|
| documents/simone-20261028_1430_blood-test.pdf | Blood test results, all normal | diary/simone-20261028_1430.md |

## Maintained

| File | What it is | Updated |
|---|---|---|
| documents/greta-diapers.csv | Nappy tracker, one row per change | 2026-10-29 |

## Diary

| Entry | Author | Summary |
|---|---|---|
| diary/simone-20261028_1430.md | simone | Blood test results came back normal |
```

## `status.md` (derived)

Same sections at every level. Omit nothing; write "—" under an empty section.

```markdown
# Status: <root | interest | interest/instance>

**Last updated:** 2026-10-28 14:35 by <slug | routine>

## Needs attention
- <What needs a decision or action soon, with a date when there is one>

## Summary
<Two to five sentences: where things stand now>

## Done
- <Recent completed items, newest first; drop items that no longer matter>

## Pending
- <Open items waiting on someone or something, with who>
- To-do: <text> — <owner>, due <YYYY-MM-DD | no date>

## Next
- <YYYY-MM-DD> <next step> — <calendar event title, when there is one>
```

The `To-do:` lines are the open to-dos attached to an instance ([todos.md](todos.md) → In status).

Root `status.md` adds a line right under Last updated, once the `review` tidy-up has written it: when the tidy-up last finished. Only the tidy-up writes it; every other rebuild keeps it as found, and a root `status.md` without it is fine.

```markdown
**Tidied:** 2026-10-28 07:31
```

Root `status.md` also adds, after Next:

```markdown
## Latest reviews
- Daily: reviews/2026-10-28-daily.md
- Weekly: reviews/2026-W44-weekly.md
- Monthly: reviews/2026-10-monthly.md
```

## Diary entry (source)

File: `<interest>/<instance>/diary/<slug>-YYYYMMDD_HHMM.md`

```markdown
---
author: <slug>
created: <YYYY-MM-DDTHH:MM>
interest: <interest>
instance: <instance>
documents: [<documents/… paths, or empty>]
events: [<"[interest/instance] title" of events created or changed, or empty>]
---

## Summary
<One line, written by the AI>

## Entry
<What the member said or pushed, in their words; light cleanup only>
```

An entry for an email (filed or sent, rules → Email) adds three front-matter lines after `events`:

```markdown
email: <Gmail message id>
email_from: <Original sender, or "to <recipients>" for sent mail>
email_subject: <Subject>
```

A `log` handoff entry (rules → Handoffs) adds one front-matter line after `events`, the refs from the line:

```markdown
github: ["#8", "PR #9", "v2026.11.12"]
```

An email entry, when the email has attachments not yet on Drive, ends Entry with a last line:

```markdown
To file: <attachment names>. Save them to landing/ or documents/ and say "filed".
```

## To-do event (source)

File: `todos/<slug>-YYYYMMDD_HHMM.md` ([todos.md](todos.md) → To-do events). An `add`:

```markdown
---
agora: todo
action: add
author: <slug>
created: <YYYY-MM-DDTHH:MM:SS>
owner: <slug | everyone>
due: <YYYY-MM-DD, or omit>
interest: <interest, or omit>
instance: <instance, or omit>
source: <path from the root | email <Gmail message id>, or omit>
---

<The to-do, one line>
```

Any other action has no body, and names the to-do it acts on:

```markdown
---
agora: todo
action: <done | reopen | assign | edit>
author: <slug>
created: <YYYY-MM-DDTHH:MM:SS>
todo: <id: the add event's file name without .md>
owner: <assign only: slug | everyone>
text: <edit only, when it changes>
due: <edit only, when it changes: YYYY-MM-DD | none>
interest: <edit only, when the place changes: interest | none>
instance: <edit only, when the place changes: instance | none>
---
```

A `remove`:

```markdown
---
agora: todo
action: remove
author: <slug>
created: <YYYY-MM-DDTHH:MM:SS>
filter: <the member's filter, in words, with "including open ones" when said>
count: <n>
todos: [<ids trashed>]
---
```

## `todo.md` (root, derived)

```markdown
---
agora: todo-list
---

# To-dos

**Last updated:** 2026-11-03 09:14 by <slug | routine>

## Simone
- **Overdue** Renew the car insurance — due 2026-11-01 · `simone-20261020_1810`
- Call the Acme accountant — due 2026-11-07 · [clients/acme](clients/acme/) · source: [diary](clients/acme/diary/simone-20261102_1640.md) · `simone-20261102_1645`
- Book the car service — no date · `nina-20261029_0930`

## Everyone
- Buy printer toner — no date · `simone-20261101_1200`

## Recently done
- Send Acme the quote — simone, done 2026-11-02 by simone · [clients/acme](clients/acme/) · source: email 18c2f0a9b7e4d311 · `simone-20261030_0915`
```

Sections, order and line contents: [todos.md](todos.md) → `todo.md`.

## Tracker (working)

File: `<interest>/<instance>/documents/<name>.csv`. Header row with the columns agreed when it was created; `date,time,author` always first.

```csv
date,time,author,type,notes
2026-10-29,07:40,simona,pee,
2026-10-29,09:15,simone,poo,soft
```

## List (working)

File: `<interest>/<instance>/documents/<name>.md`

```markdown
# <Title>

**Updated:** <YYYY-MM-DD HH:MM> by <slug>

- [ ] <item>
```

## Memory file (curated)

File: `memories/<slug>.md` or `memories/shared.md`

```markdown
# Memories: <Name | shared>

- <YYYY-MM-DD> <one durable fact, preference or habit>
```

With email on, each file also holds one `- Email importance: <not asked | invited | no preference | criteria>` line (rules → Email → Importance).

## Review (curated)

File: `reviews/YYYY-MM-DD-daily.md`, `reviews/YYYY-Www-weekly.md` or `reviews/YYYY-MM-monthly.md`

```markdown
# <Daily | Weekly | Monthly> review: <period>

**Written:** <YYYY-MM-DD HH:MM> by <routine | routine (<slug>) | <slug>, by hand | hand>

## Looking back
- <What happened and what got done, by interest>

## Still open
- <Pending items carried forward>

## Looking ahead
- <Events, deadlines and prep in the coming period, by date>

## Needs attention
- <Flags from tidy-up and anything needing a decision>

## To-dos
- <Overdue, due ahead, opened, done, suggestions; monthly: open over 30 days with no date ([todos.md](todos.md) → Reviews)>

## Email
- <Email on only: important email needing attention, drafts waiting for approval, sent, filed; from `inbox.md`>
```

The To-dos section, filled (order: [todos.md](todos.md) → Reviews):

```markdown
## To-dos
- **Overdue**: Renew the car insurance (simone, due 1 Nov)
- **Due ahead**: Call the Acme accountant (simone, 7 Nov)
- **Opened**: Buy printer toner (everyone)
- **Done**: Send Acme the quote (simone)
- **Suggestions**: looks done: Book the car service (nina/car/diary/nina-20261102_1100.md); possible new to-do: Reply to the school about the trip (email from the school)
```

Later runs append, only when new diary entries arrived since it was written:

```markdown
## Update <HH:MM>
- <What changed>
```
