---
name: review
description: Reviews an Agora root on a schedule or on request, tidying the shared Drive folder and summing up what happened and what's coming for the family or team. Use when a scheduled routine says "Run Agora review", or when a member asks for a summary of the day, week or month in Agora ("give me the week ahead", "what did we do this month", "daily review").
---

# Review

A **review** is the body of every scheduled routine. It first **tidies** the root so every file is where it belongs and every status is current, then writes the **shared review** for the period into `reviews/`, then sends **one personal message** to the member it runs for.

## Input

A scheduled routine's prompt is one line:

```
Run Agora review: <daily | weekly | monthly> for <slug> [in <root name>]
```

- **Period**: `daily`, `weekly` or `monthly`.
- **Member**: the slug of the member the message is for. It sets identity: no question is asked.
- **Root** (optional): the root's display name from `org.md`, needed only when the assistant account sees several roots.

A prompt in this shape means an **unattended run**: nobody is there to answer, so follow rules → Unattended runs (act only on what is unambiguous, flag the rest) and skip the memory step.

A member asking in conversation means an **attended run**: identity comes from rules → Identity, the period from their words ("today" → daily, "the week" → weekly, "the month" → monthly), and an ambiguity becomes a question instead of a flag. When their words name a window the periods below don't cover ("so far this month"), treat it as a **custom window**: run every step for that window, but skip steps 3.1–3.3 (no review file) and build the personal message from the statuses, the diary entries and the calendar events inside the window.

## Periods

All dates are in the root's timezone. Weeks are ISO weeks (Monday to Sunday) named with the ISO week-year (`2026-W53`, then `2027-W01`).

| Period | File | Named after | Looking back | Looking ahead |
|---|---|---|---|---|
| daily | `reviews/YYYY-MM-DD-daily.md` | today | yesterday | today |
| weekly | `reviews/YYYY-Www-weekly.md` | the ISO week containing tomorrow | the ISO week before it | the named week |
| monthly | `reviews/YYYY-MM-monthly.md` | the month containing tomorrow | the month before it | the named month |

"Containing tomorrow" lets the Sunday-evening weekly run and a Monday-morning run by hand land on the same file.

## Steps

### 1. Load the rules, find the root, know who is talking

Load the `rules` skill and follow rules → Finding the root. In an unattended run, use the root named in the prompt; when several roots exist and none is named, stop and output one line telling the member to add the root name to this routine's prompt. Take the member from the prompt (unattended) or from rules → Identity (attended). Read `memories/<slug>.md` and `memories/shared.md`.

Done when: `org.md` is read, the member's slug is known and listed in `org.md`, their memories are read, and you have the period, the file name and both windows from the table above.

### 2. Tidy up

The tidy-up is **idempotent**: a second run on an unchanged root changes nothing, so members' routines running at the same time are harmless. Walk the root once, listing each folder's children, and use that listing for every check below.

1. **Duplicates.** Wherever two files in one folder share a name: for derived and curated files (`status.md`, `index.md`, `org.md`, `memories/`, `reviews/`) keep the newest and trash the rest (rules → Replace). Source files (`diary/`, `documents/`) stay untouched; flag them.
2. **Unfiled documents.** Every file in `landing/`, and every file in an instance `documents/` folder missing from that instance's `index.md`, goes to the `file-document` skill one at a time as case (c), with the file path, the member's slug, the period, and whether the run is unattended. Attended, it may ask the member; unattended, it files what is unambiguous and flags the rest. It propagates up to the interest level only and returns its flags to you instead of writing them.
3. **Stale derived files.** Rebuild, bottom-up (rules → Status update rule), every derived file that is older than its sources:
   - an instance `status.md` whose Last updated is older than its newest diary entry or document (the timestamp in the file name);
   - an instance `index.md` whose rows differ from the files in its `diary/` and `documents/`;
   - an interest `status.md` older than any of its instances' `status.md` Last updated;
   - an interest or root `index.md` whose rows differ from its child folders, or whose Last activity differs from a child's newest entry.
   Write `routine` as the author of the Last updated line in unattended runs. The root `status.md` is rebuilt once, in step 3.
4. **Old daily reviews.** Trash every `reviews/*-daily.md` dated more than 30 days before today. Weekly and monthly reviews are the long-term record and stay.

Collect every flag you raise in this step, and every flag `file-document` returns, for the root's Needs attention (step 3.4).

Done when: every folder has been walked; no derived or curated file has a duplicate; every file in `landing/` and every unindexed document is either filed or flagged; no instance or interest derived file is older than its sources; no daily review is older than 30 days.

### 3. Write the shared review

1. **Find** the period's file in `reviews/`.
2. **New period** (no file yet): gather, then create the file from the Review template (rules → File kinds → templates):
   - **Looking back**: diary entries created in the look-back window (their file names carry the time) and the Done items they produced, grouped by interest, one line each.
   - **Still open**: Pending items from the instance statuses, each with who or what it waits on.
   - **Looking ahead**: system calendar events in the look-ahead window, plus Next items dated inside it, in date order, with their event titles.
   - **Needs attention**: the flags from step 2 plus every Needs attention item in the instance statuses.
   - **Written**: now, `by routine (<slug>)` unattended, `by <slug>, by hand` attended.
3. **Existing file**: read it. Its last write time is the latest `## Update HH:MM` heading, else Written. If diary entries were created after that time, add one `## Update <HH:MM>` section at the end listing what they changed, one line each, by interest. The earlier text stays exactly as it was: write the file by replace (rules → Replace) with the old content verbatim followed by the new section. No newer diary entries: leave the file untouched.
4. **Root status.** Rebuild the root `status.md` (rules → Status update rule) from the interest statuses. Under Needs attention, keep each earlier flag whose condition still holds (the file is still in `landing/`, the duplicate still exists), drop the ones that are resolved, and add this run's new flags. Under Latest reviews, link the newest daily, weekly and monthly files in `reviews/`.

Done when: the period's review file exists, written once and changed since only by appended Update sections (custom window: no file); and the root `status.md` carries today's Last updated, every flag still open, and links to the newest review of each period.

### 4. Write the personal message

One message for the member, built from the review and the statuses, in this order:

1. **Yours**: items assigned to the member or waiting on them, events in the look-ahead window that involve them or an interest about them (in a per-person layout, the interest named after them), and their personal calendar events when `org.md` records it as shared with full details.
2. **Everyone else**: the other highlights from Looking back and Looking ahead, one line each.
3. **Needs attention**: anything that needs a decision or an action, with the question that settles it.

Write it to be read on a phone: short lines, dates as weekday and day ("Tue 3 Nov"), nothing that is empty, no paths except the review file at the end. Use the root's language and the tone the member's memories ask for.

Done when: every item for the member is in **Yours**, every Needs attention item from the review (custom window: from the root `status.md`) is in the message, and it fits on one phone screen (about 15 lines). For a custom window, also: every diary entry and calendar event inside the window is in the message or rolled into one of its lines.

### 5. Memory step

Attended runs only: follow rules → Memory step.

Done when: anything durable you learned is in `memories/`, or you found nothing new, or the run is unattended.

## Output

The personal message from step 4, ending with the review's path (`Full review: reviews/2026-W45-weekly.md`; none for a custom window), plus the `remember` line, if any. In a scheduled run it is the routine's notification, so it stands alone without any greeting about running a task. When something failed (rules → When something fails), the message says in one sentence what failed and to ask Agora for help.
