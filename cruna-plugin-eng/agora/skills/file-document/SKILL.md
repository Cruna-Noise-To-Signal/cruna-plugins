---
name: file-document
description: Files a document into Agora — names it, moves it into the right instance's documents folder, links it from the diary and puts its dates on the calendar. Use when a member says they uploaded or dropped a file into Agora ("I put the blood test in Nina's health folder", "there's an invoice in landing"), attaches a file in the chat for Agora to keep, or when the review routine finds files nobody announced.
---

# File document

To **file** a document is to give it its Agora name, put it in exactly one instance's `documents/` folder, read it, and link it from a diary entry, so `ask` can find it later. Files arrive three ways:

- **(a) In place**: the member uploaded it to an instance's `documents/` folder and tells you.
- **(b) Landing**: the member uploaded it to the root `landing/` folder and tells you.
- **(c) Unannounced (found by review)**: the `review` skill's tidy-up found a file in `landing/`, or in a `documents/` folder but missing from that instance's `index.md`. `review` hands you the file path, the member's slug, the period, and whether its run is unattended. You return your flags to `review` instead of writing them, and stop propagating at the interest level (step 8).

Separately, a run is **attended** or **unattended**. Cases (a) and (b) are always attended. Case (c) is attended when a member asked for the review: ask them as in (b). Unattended, nobody is there to ask: act only on what is unambiguous and flag the rest (rules → Unattended runs, rules → Flag).

A file attached in the chat is none of these: you can read it but cannot put it on Drive. Tell the member, in one or two sentences, to open the Drive app, upload the file into the root's `landing/` folder (or straight into the instance's `documents/` folder), and then say "filed". Offer to log what the file says right away with the `log` skill, so nothing waits on the upload. Stop here until they confirm the upload.

## Steps

### 1. Load the rules, find the root, know who is talking

Load the `rules` skill. Follow rules → Finding the root and rules → Identity, then read `memories/<slug>.md` and `memories/shared.md` (rules → Memory step). In case (c), `review` has done all of this and gives you the slug.

Done when: `org.md` is read, you know the member's slug, and their memories are read.

### 2. Find the file

- (a): list the instance's `documents/` folder and pick the files not yet in its `index.md` Documents table.
- (b): list `landing/`.
- (c): `review` hands you the file path.

When more than one file could be the one meant, ask which, listing their names (unattended: file each one separately). Handle several files one at a time through steps 3–7, then do step 8 once.

Done when: you hold each file's current path, original name, creation time and owner from its Drive metadata.

### 3. Read it

Read the content with Drive's text extraction (rules → Working with Drive): it covers PDFs, images and Office files. Note what the document is, who and what it concerns, and every date, deadline or amount in it.

When nothing readable comes back (a blurry photo, a protected PDF), work from the file name and the member's words, and say so in the summary.

Done when: you can write one line saying what the document is and what it says that matters.

### 4. Place it

- (a): the instance is the folder it sits in.
- (b): match the content and the member's words against the root and interest `index.md` files. When it fits no single instance with certainty, ask, offering the likely `interest/instance` options. When the right instance does not exist yet, offer the `add-interest` skill.
- (c) in a `documents/` folder: the folder decides. (c) in `landing/`, attended: as (b). (c) in `landing/`, unattended: place it only when the content names exactly one existing instance beyond doubt; otherwise **flag** it (rules → Flag) with the file name, what it seems to be, and the likely instances, return the flag to `review`, and stop for this file.

Done when: the file has exactly one existing destination instance, or is flagged.

### 5. Name it

The Agora name is `<slug>-YYYYMMDD_HHMM_<original-name>.<ext>`:

- **slug**: the uploader. Match the file owner's email from Drive metadata against the members in `org.md`; when it matches nobody (or the file is owned by the assistant account), use the slug from step 1.
- **YYYYMMDD_HHMM**: the file's creation time on Drive (its upload time), in the root's timezone.
- **original-name**: the original name without its extension, lowercase, spaces and underscores turned into hyphens, ASCII only (`Esami Sangue Nina.PDF` → `esami-sangue-nina`). **ext**: the original extension, lowercase.

A file whose name already follows this pattern keeps it.

Done when: you have the final name, and no file with that name exists in the destination `documents/` folder (if one does, it is a duplicate: compare, and ask or flag instead of filing twice).

### 6. Move it

Rename and move in one update. When that fails:

1. Copy the file into the destination `documents/` folder under the new name, then trash the original.
2. When the copy fails too, leave the file where it is and flag it: "Couldn't move `landing/<name>` to `<interest>/<instance>/documents/`: move it there in the Drive app". Tell the member the same in one sentence when attended, and stop for this file.
3. When the copy worked but the trash failed, carry on with the copy and flag the leftover original for removal.

In case (c), return these flags to `review` instead of writing them.

For (a) only the rename applies.

Done when: the file exists exactly once, under its new name, in the destination `documents/` folder; or it is flagged.

### 7. Write the diary entry and catch dates

Fix the entry path first: name the entry per rules → Layout, with the member from step 1.

Dates for the calendar (rules → Calendar conventions): when the document carries an appointment, deadline or due payment, ask once whether to put each on the calendar, with its date and time, and run the `schedule` skill for every yes, passing the instance, the entry path and the details. Unattended, create nothing: flag each date instead ("`nina/health`: vaccine booster due 2026-12-01, found in `documents/…`; add to calendar?") and return the flags to `review`.

Then create the entry from the Diary entry template (rules → File kinds → templates):

- `documents`: the new path, `documents/<new name>`.
- `events`: the titles `schedule` returned.
- **Summary**: one line on what the document says ("Blood test results: all values normal").
- **Entry**: the member's words about the file, then `Filed: documents/<new name>`. Case (c): `Filed by the <period> review; nobody announced this file.`

Done when: the entry exists at its path, linking the document, and every date in the document is on the calendar, declined, or flagged.

### 8. Refresh the indexes and statuses

For each destination instance, rebuild its `index.md` with the new document and entry at the top (rules → Replace). Then propagate (rules → Status update rule): each instance's `status.md`, each touched interest's once, the root's once. In case (c), stop at the interest level: `review` rebuilds the root `status.md` once, with your returned flags.

Done when: each touched instance index lists the document and its entry, and every status up to the root (case (c): up to the interest) reflects what the document says.

### 9. Memory step

Attended only: follow rules → Memory step.

Done when: anything durable you learned is in `memories/`, or you found nothing new, or the run is unattended.

## Output

Cases (a) and (b), one short message in the root's language:

- Per file: where it went (`interest/instance`) and the one-line summary of its content.
- Calendar changes, one line each, if any.
- Anything that needs the member: a file you couldn't move and what to do in the Drive app, a question still open.
- The `remember` line, if any.

Case (c): no message of your own; return to `review` the files filed (with destination and summary) and the flags raised.
