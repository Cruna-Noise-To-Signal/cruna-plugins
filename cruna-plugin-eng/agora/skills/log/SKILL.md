---
name: log
description: Logs news into the Agora diary and working files and keeps each status current. Use when a member tells Agora something that happened or changed about a person, client or topic it tracks ("Nina's swimming lesson moved to Thursday", "client X approved the quote"), pastes an email or message to keep, corrects something logged earlier, or says "save that" or "note this down" after asking Agora a question. Also when they want something frequent tracked in a file ("track Greta's nappies"), ask for a list, memo or printable ("add milk to the shopping list", "make a packing list"), or change how Agora handles their data ("from now on…").
---

# Log

A **log** turns what a member says into one diary entry per instance it concerns, puts anything time-bound on the calendar, and refreshes every status above it. The member talks; you place, write and propagate. Log also owns the **working files** (trackers and lists) and the **Conventions** that route facts to them (rules → File kinds, rules → Conventions); their procedures live in [references/working-files.md](references/working-files.md).

## Steps

### 1. Load the rules, find the root, know who is talking

Load the `rules` skill. Follow rules → Finding the root and rules → Identity, then read `memories/<slug>.md` and `memories/shared.md` (rules → Memory step).

Done when: `org.md` is read, you know the member's slug, and their memories are read.

### 2. Gather what to log

Take the member's message as the material. The branches:

- **News**: a sentence, a pasted email or message, a forwarded note.
- **Correction**: "actually it's Friday, not Thursday". Treat it as news that names what it corrects; the earlier entry stays untouched (rules → File kinds).
- **Push back from a pull session**: after `ask`, the member says "save that". The material is the answer or decision they want kept, plus their own words.
- **A file attached in the chat** → the `file-document` skill.
- **Working file**: a request to track something in a file, to make or change a list, memo or printable, or to add, change or drop a Convention ("from now on…", "stop tracking nappies in the file") → [references/working-files.md](references/working-files.md), then step 9.

Done when: you can state, in one line each, every fact the member wants kept.

### 3. Place each fact

**Place** every fact in an instance: read the root `index.md`, then the likely interest's `index.md`, and match. One message can touch several instances ("Nina has a fever, so we skip the swimming lesson" → `nina/health` and `nina/swimming`); each instance gets its own entry.

When a fact fits no single instance with certainty, ask once for the whole message, offering the likely `interest/instance` options so the answer is one word (rules → Ask, never assume). When the right instance does not exist yet, offer the `add-interest` skill, then come back and finish the log.

When `ask` handed you the instance, use it unless a fact clearly belongs elsewhere.

Then check `org.md` → Conventions for each placed instance (rules → Conventions). A fact that a Convention routes to a tracker ("Greta did a poo at 9:15" with nappies tracked in `greta/health`) becomes a row: follow [references/working-files.md](references/working-files.md) → Add a row, and take it out of steps 4–7. Other facts in the same message go on as usual.

Done when: every fact sits in exactly one existing instance, confirmed by the indexes or by the member, and every fact a Convention routes is a tracker row.

### 4. Name each entry

For each instance, name the entry per rules → Layout. Fix the paths now: the calendar needs them in step 5.

Done when: every entry has a path that is free in its `diary/` folder.

### 5. Catch anything for the calendar

Look through the facts for anything that must be remembered at a date (rules → Calendar conventions): a new or moved appointment, a deadline, a payment due, something to prepare beforehand, a cancelled meeting whose event should go.

When you find any, ask once, listing each with its date and time ("Shall I move *[nina/swimming] Lesson* to Thursday 6 Nov, 17:00?"). For each yes, run the `schedule` skill with the instance, the entry path from step 4 and the details; it returns the event titles it created, changed or deleted. A yes to a question that carries the exact date and time is the confirmation `schedule` needs.

Done when: every time-bound fact is either on the calendar, declined by the member, or has no date yet (then it stays in the entry as pending).

### 6. Write the entries

Create each entry from the Diary entry template (rules → File kinds → templates):

- `author`: the member's slug; `created`: the timestamp from step 4.
- `documents`: empty, unless the entry links a file already in `documents/`.
- `events`: the titles `schedule` returned for this instance.
- **Summary**: one plain line you write, stating the fact and its date ("Swimming lesson moved from Tuesday to Thursday 6 Nov").
- **Entry**: the member's own words, or the pasted text, trimmed to the part about this instance. A correction says what it corrects ("Correction to diary/simone-20261028_1430.md: …").

Done when: every entry exists at its path, with every template field filled.

### 7. Refresh the indexes and statuses

For each touched instance, rebuild its `index.md` with the new entry at the top (rules → Replace). Then **propagate** (rules → Status update rule): each instance's `status.md`, then each touched interest's once, then the root's once.

Done when: every touched instance index lists its new entry, and every status from those instances up to the root shows the new facts and today's Last updated.

### 8. Spot a pattern

Look at today's rows in the Diary table of each touched instance, and at the member's words. When rules → Conventions → Suggest applies (several similar entries today, a list or printable requested, a sheet the member keeps on the side), follow [references/working-files.md](references/working-files.md) → Suggest. A member with an existing sheet to bring in → the `file-document` skill (Import).

Done when: the suggestion is asked once, or no pattern showed.

### 9. Memory step

Follow rules → Memory step.

Done when: anything durable you learned is in `memories/`, or you found nothing new.

## Output

One short message, in the root's language:

- What was logged, one line per entry: `interest/instance` and its Summary line.
- Tracker rows, one line per tracker ("Greta: poo at 9:15 added to the nappy tracker"); a working file created or changed, with its path so the member can open or print it; a Convention written, in one plain sentence.
- Calendar changes, one line each, if any.
- Anything still open: a fact left without a date, an instance waiting on `add-interest`.
- The suggestion from step 8, if any.
- The `remember` line, if any.

No file paths other than a working file's, no front-matter or status text unless the member asks.
