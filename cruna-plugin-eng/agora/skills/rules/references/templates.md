# Agora file templates

Fill every `<placeholder>`. Headings stay in English; content follows the language in `org.md`. Keep each file short: a member should read any `status.md` in under a minute.

## `org.md` (root, curated)

```markdown
---
agora: root
name: <Root display name>
language: <en | it | …>
timezone: <Europe/Rome>
assistant_account: <assistant@gmail.com | owner's own account>
calendar: <assistant account email: its primary calendar is the system calendar>
layout: <what an interest is, e.g. "one interest per person, plus `family` for shared things">
---

# <Root display name>

## Members

| Slug | Name | Email | Role | Birthday | Personal calendar | Notes |
|---|---|---|---|---|---|---|
| simone | Simone | simone@example.com | parent | 1989-03-28 | free/busy | prefers short answers |

## Interests

| Interest | Color | Description |
|---|---|---|
| nina | Peacock | Everything about Nina |

## Conventions

| Where | Convention | Since |
|---|---|---|
| greta/health | Nappies: one row per change in `documents/greta-diapers.csv` (date,time,author,type,notes); no diary entry per change; daily summary entry | 2026-10-29, simona |

## Access

| Member | Drive | System calendar | Client connected | Notes |
|---|---|---|---|---|
| simone | editor | can edit | Claude | routines on |
```

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

## Next
- <YYYY-MM-DD> <next step> — <calendar event title, when there is one>
```

Root `status.md` adds, after Next:

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

## Review (curated)

File: `reviews/YYYY-MM-DD-daily.md`, `reviews/YYYY-Www-weekly.md` or `reviews/YYYY-MM-monthly.md`

```markdown
# <Daily | Weekly | Monthly> review: <period>

**Written:** <YYYY-MM-DD HH:MM> by <routine (<slug>) | <slug>, by hand>

## Looking back
- <What happened and what got done, by interest>

## Still open
- <Pending items carried forward>

## Looking ahead
- <Events, deadlines and prep in the coming period, by date>

## Needs attention
- <Flags from tidy-up and anything needing a decision>
```

Later runs append, only when new diary entries arrived since it was written:

```markdown
## Update <HH:MM>
- <What changed>
```
