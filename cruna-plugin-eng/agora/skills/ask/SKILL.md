---
name: ask
description: Answers questions from what Agora has on file, the shared Drive notes, documents and calendar of a family or small business. Use when a member asks about a person, client, job or topic Agora tracks ("when is Nina's next vaccine?", "what's pending with client Rossi?", "how many feeds today?"), or about what is coming up ("what do we have this week?").
---

# Ask

Answer from the root while reading as little as possible. This is **progressive discovery**: the root `index.md` tells you which interests matter, an interest `index.md` which instances, an instance `status.md` whether you already have the answer, and only then do you open the diary entries, documents or events that hold it. Each level decides whether the next is worth opening.

A good answer is **on topic** and **up to date**: it comes from the newest sources, says how old they are, and points to where they live.

## Steps

### 1. Load the rules, find the root, know who is talking

Load the `rules` skill. Follow rules → Finding the root and rules → Identity, then read `memories/<slug>.md` and `memories/shared.md` (rules → Memory step). No root: tell the member Agora isn't set up yet and offer the `setup` skill.

Done when: `org.md` is read, you know the member's slug, and their memories are read.

### 2. Locate

Read the root `index.md` and match the question to interests: a name, a client, a topic, read through the layout convention in `org.md` ("Nina" is the `nina` interest in a per-person layout). For each matching interest, read its `index.md` and pick the instances whose description fits. A question about something a Convention routes to a tracker ("how many poos today?") goes straight to that instance and tracker (rules → Conventions).

- **Time questions with no subject** ("what do we have this week?", "anything due tomorrow?"): the whole root is in scope; go straight to the root `status.md` and the calendar (step 4). For a full summary of a day, week or month ("give me the week ahead"), use the `review` skill instead.
- **Questions that span interests** ("what's pending with everyone?"): every interest is in scope; plan to roll up (step 5).
- **Unclear target**, and different targets would give different answers: ask once, offering the likely `interest/instance` options from the indexes (rules → Ask, never assume).

Done when: you hold a list of candidate instances (or "whole root"), each one backed by a row in an index or by the member's answer.

### 3. Read the statuses

Read each candidate instance's `status.md`. With more than three candidates, read their interests' `status.md` first (they roll up the instances) and descend only into the instances that look relevant.

A status answers the question when it covers it **and** is current: its Last updated is no older than the newest diary entry or document in its instance `index.md`. A stale status is a pointer, not an answer; open the sources. Tracker rows reach the status only through the daily summary, so counting and "when was the last" questions always go to the tracker.

Done when: every candidate status is read and you know, for each, whether it answers the question or which sources to open.

### 4. Open the sources

Open only what the question needs, newest first:

- **Diary entries**: pick them from the Diary table of the instance `index.md` by date and summary.
- **Documents**: pick them from the Documents table; read PDFs, images and Office files through the text-extraction view (rules → Working with Drive).
- **Working files**: pick them from the Maintained table and download them (rules → File kinds). A **tracker** answers counting and time questions: filter its rows by date, time and the other columns, then count, sum or take the latest ("4 poos today, the last at 15:20"; "last feed at 06:10"). A **list** is read as it stands.
- **Calendar**: list the events on the system calendar for the window the question implies (default: from today, the next 30 days), or search by the `[interest/instance]` title prefix. For "what do we have" questions, add the member's personal calendar when `org.md` records it as shared with full details.

Apply rules → Status update rule while reading: a newer entry that settles, cancels or reschedules something overrides the older one.

Done when: every fact you will state traces to a file or event you read, and you checked the newer sources in that instance for anything that overrides it.

### 5. Answer

- **Lead** with the direct answer in one or two sentences.
- **Date** every fact by its source ("as of the 12 Oct diary entry", "per the invoice filed 3 Sep").
- **Flag old news**: when the newest source on the topic is more than a month old, or describes something that should have happened since (an appointment last week with nothing after it), say so plainly ("The latest I have is from March; it may have changed.").
- **Cite** each source by its path from the root (`nina/health/diary/simone-20261028_1430.md`) and each event by title and date, so the member can open it in Drive or the calendar.
- **Roll up** answers that span interests: group by interest, one line each, then the details only where asked.
- **Not found**: say so, list where you looked (which indexes, instances and calendar window), and offer to save the answer if the member knows it (the `log` skill).

Done when: every fact in the answer carries a date and a source, or the answer says plainly that nothing was found and where you looked.

### 6. Hand off what's worth saving

Writing belongs to other skills. When the conversation produces something to keep, hand it off with the interest and instance you already found, so the member is not asked again:

- "note that", "save that", new facts or a decision → the `log` skill.
- "remind me", an appointment, a deadline → the `schedule` skill.
- a file to keep → the `file-document` skill.
- a memo, list or printable made from what you found ("make that a list I can print") → the `log` skill, to save it as a List working file (rules → File kinds), when the member wants it kept and updated; when they only want to read it, write it in your answer.

Done when: every save request in the conversation was handed to its skill, or there was none.

### 7. Memory step

Follow rules → Memory step.

Done when: anything durable you learned is in `memories/`, or you found nothing new.

## Output

One short message, in the root's language and plain words:

1. The answer, dated.
2. Sources: one line each, path from the root (or event title and date).
3. When it applies: a note that the information is old, or where you looked when nothing was found, and an offer to save or schedule.
4. The `remember` line, if any.

No front-matter, raw status text or Drive IDs.
