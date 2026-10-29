# Working files and Conventions

A **working file** is a tracker (CSV, one row per event) or a list (Markdown) that you maintain in an instance's `documents/` folder; a **Convention** is a line in `org.md` → Conventions that changes a default for an instance. What they are and how they may change: rules → File kinds and rules → Conventions. Their shapes: rules → File kinds → templates (Tracker, List, and the Conventions and Maintained tables).

You arrive here from the `log` skill with the root, the member's slug and their memories already read. Pick the branch:

- **Add a row**: a fact that a Convention routes to a tracker ("Greta did a poo at 9:15").
- **Create**: the member wants something tracked in a file or a new list, memo or printable ("track Greta's nappies in a file", "make me a packing list for the holiday"), says yes to a suggestion, or `file-document` hands you an imported sheet.
- **Update a list**: "add milk to the shopping list", "tick off the passports", "redo the memo".
- **Change a Convention**: "from now on…", "stop tracking nappies in the file", "log them in the diary again", "add a column for the amount".

Every write below replaces the whole file (rules → Replace). Write CSV with content type `text/csv` and Markdown with `text/markdown`, both with conversion to Google formats disabled.

## Add a row

1. **Read** the Convention line: it names the tracker and its columns. Download the tracker.
2. **Build one row per event**: `date` and `time` of the event as the member gives them ("at 9:15", "this morning around 7"), else now, in the root's timezone; `author` is the member's slug; the other columns from the member's words, empty when they say nothing. When a column the row means nothing without (the `type` of a nappy) is unclear, ask once for the whole message. Quote any value containing a comma.
3. **Insert** the rows in date and time order (oldest first), so an event told late lands in its place.
4. **Replace** the tracker. Before trashing the old version, list the folder again: when another version newer than the one you read is there (another member wrote meanwhile), merge its rows into yours, keeping each distinct row once, and replace again.
5. **Correction** ("the 9:15 one was a pee, not a poo"): change exactly that row, at the member's request (rules → File kinds), and replace as in step 4.
6. **No diary entry.** Rebuild the instance `index.md` only when the tracker's Updated date under Maintained changes (the first row of a day). Propagate (rules → Status update rule) only when the rows change something a status should show now: something the member calls worrying, a reading out of the ordinary, an item in Pending they settle. Otherwise the daily `review` summary carries them into status.

Done when: every event is a row in the tracker in date and time order, exactly one copy of the tracker exists, and it holds every row it held before plus yours.

## Create

1. **Kind**: a **tracker** for something that happens often and is counted or timed (nappies, feeds, expenses, medicine doses); a **list** for a checklist, a memo or anything to print.
2. **Place** it in one instance, as in the `log` skill's step 3 (`greta/health`, `family/vacations`).
3. **Agree the name, and the columns for a tracker**, in one question with a recommended answer: a lowercase hyphenated name saying whose and what (`greta-diapers.csv`, `packing-list-easter.md`); columns `date,time,author` then what the member needs (`type,notes`; `amount,category,notes`). The name must be free in that `documents/` folder.
4. **Write** the file from its template (rules → File kinds → templates):
   - Tracker: the header row, plus any rows the member just told you or `file-document` imported, oldest first.
   - List: the items the member gave you. For a memo or a list built from what Agora knows ("a memo of Greta's vaccines", "what to pack, from last year's trip"), write it from the instance `status.md` and the diary entries it needs, newest first, and keep it short enough to print on one page.
5. **Log one diary entry** saying what the file is for, following the `log` skill's steps 4 and 6: `documents: [documents/<name>]`; Summary like "Started a nappy tracker for Greta"; Entry with the member's words, then `Maintained: documents/<name>` and, for a tracker, its columns.
6. **Convention**, when the file changes a default: a tracker that takes events away from the diary always does; a list made on request does not. An import from `file-document` comes without one: that skill offers it afterwards. Write one line in `org.md` → Conventions (Where: the instance; Convention: what goes where, the file, its columns, "no diary entry per <event>; daily summary entry"; Since: today and the member's slug). Replace `org.md`.
7. **Refresh**: rebuild the instance `index.md` with the file under Maintained (not under Documents) and the new entry under Diary, then propagate.

Done when: the file exists once under its agreed name, the diary entry links it, the instance index lists it under Maintained, and `org.md` carries its Convention line when it changes a default.

## Update a list

1. Download the list; when none matches, ask whether to create one (Create).
2. Change only what the member asked: add, remove or tick items. "Redo" or "update" a memo means regenerate it from the current status and diary, as in Create step 4. Set the Updated line to now and the member's slug.
3. Replace the list. Rebuild the instance `index.md` when its Updated date under Maintained changes.
4. A list edit needs no diary entry. When the member's words also carry news ("we booked the hotel, take it off the list"), log that news as a normal entry with the `log` skill.

Done when: the list holds exactly the requested change, carries the new Updated line, and exists once.

## Change a Convention

1. Find the matching line in `org.md` → Conventions; when several could match, ask which. A request that matches no line ("from now on, put Greta's weight in the health tracker") is a new Convention: when it needs a new working file, go to Create; otherwise go on with the new line.
2. Say back the change in one sentence and the date it applies from, and wait for a yes.
3. Write the line changed, added or removed in `org.md` (Since: today and the member's slug), and replace `org.md`.
4. The working file stays: its rows are history. A tracker that stops taking events keeps its place under Maintained with "(stopped YYYY-MM-DD)" in What it is. A new column goes at the end of the header, empty in the earlier rows; replace the tracker.
5. Log one diary entry in the instance saying what changed and from when, and propagate.

Done when: `org.md` holds the Convention as agreed (or no longer holds it), the diary entry records the change, and the tracker's header matches the Convention's columns.

## Suggest

When rules → Conventions → Suggest applies, ask its one question with the concrete proposal, once per conversation. On yes, go to Create. On no, write a Convention line recording the choice ("Nappies: diary entries, no tracker (declined)") so later runs leave the question alone.
