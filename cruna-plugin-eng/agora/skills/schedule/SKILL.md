---
name: schedule
description: Schedules on the Agora shared calendar — creates, moves or cancels appointments, deadlines, reminders and prep time, and notes each change in the diary. Use when a member asks to be reminded or to set, move or cancel something at a date ("remind me to book the pediatrician", "set the meeting with client X next Tuesday at 10", "cancel Thursday's lesson"), or when another Agora skill finds a date worth putting on the calendar.
---

# Schedule

Everything Agora must remember at a date lives on the root's **system calendar** (rules → Calendar conventions). You write the event, check it fits people's time, and leave a diary trace so every status knows what is next.

You are called two ways:

- **Direct**: a member asks you. You write your own diary entry.
- **Handed off** from `log` or `file-document`: the caller gives you the instance, the entry path it is about to write, and the details, often already confirmed. You return the event titles; the caller records them and propagates.

Called from `ask`: run Direct mode, using the instance `ask` gives you (skip asking for it).

## Steps

### 1. Load the rules, find the root, know who is talking

Load the `rules` skill. Follow rules → Finding the root and rules → Identity, then read `memories/<slug>.md` and `memories/shared.md` for habits that set defaults (usual appointment times, who takes the kids). Handed off, the caller has done this.

Done when: `org.md` is read (system calendar, timezone, members, colors) and you know the member's slug.

### 2. Place the event

Every event belongs to one instance: its title starts with `[<interest>/<instance>]`. Handed off, the caller gives it; from `ask`, use the instance it gives you. Direct, match the request against the root and interest `index.md` files; when unclear, ask, offering the likely options. When the instance does not exist yet, offer the `add-interest` skill.

Done when: you know the one instance the event belongs to.

### 3. Pin down what and when

Decide the action (create, update, delete) and the kind:

| Kind | Shape |
|---|---|
| Appointment, meeting | Timed, 1 hour unless said otherwise |
| Deadline, due payment | All-day, on the due date |
| Reminder ("remind me to…") | Timed, 15 minutes, with a pop-up notification at the start |
| Placeholder (date not final) | As the kind it stands for, title ending `(to confirm)` |
| Prep time | A separate event before the one it prepares, title starting `Prep:` |

Resolve every date against today in the root's timezone: "next Tuesday", "in two weeks", "the 3rd". When a date or time could mean two things, or an appointment has no time, ask, offering a concrete default ("Tuesday 4 Nov or Tuesday 11 Nov?", "10:00 as usual?"). Something that repeats ("every Thursday") becomes a recurring event.

For update and delete, **find** the existing event: search the system calendar for the `[<interest>/<instance>]` title prefix around the date mentioned. Several matches: ask which, listing them with dates. None: say so and offer to create it instead.

Done when: you have the action, the exact title, start and end (or the all-day date) in the root's timezone, and for update/delete the one event it applies to.

### 4. Check for clashes

For timed events, read the system calendar and the personal calendars of the members the event concerns (all members when unclear) that are shared with the assistant account (`org.md`, Personal calendar column). Free/busy calendars show only busy blocks; that is enough. When something overlaps, say who is busy and propose the nearest free slot.

Done when: the time is clash-free, or the member chose it knowing the clash.

### 5. Confirm in one line

Show the final event in one line and wait for a yes: "Create *[nina/health] Pediatrician visit*, Tue 4 Nov, 10:00–11:00?" For a delete, name the event and its date. Skip this when the member already said yes to these exact details, in this skill or in the caller's question.

Done when: the member confirmed this exact event.

### 6. Write the event

On the system calendar only:

- **Title**: `[<interest>/<instance>] <title>`, in the root's language.
- **Description**: the instance path and the diary entry path (direct: the path you fix in step 7; handed off: the caller's).
- **Color**: the interest's color from `org.md`, when set.
- **Invitees**: only when the member asks; add each named member's email from `org.md`. When an email is missing, ask for it and offer to record it in `org.md` (rules → Replace).

Done when: the calendar shows the event created, changed or deleted as confirmed.

### 7. Record it

Handed off: return the event titles to the caller and stop here; it records them and propagates.

Direct:

1. Fix the entry path: name the entry per rules → Layout.
2. Create the entry from the Diary entry template (rules → File kinds → templates). **Summary**: what changed and when ("Pediatrician visit booked for Tue 4 Nov, 10:00"). **Entry**: the member's words. `events`: every title you created, changed or deleted.
3. Rebuild the instance `index.md` with the entry on top (rules → Replace), then propagate (rules → Status update rule) so Next shows the event at every level.

Done when: the diary entry exists, and the instance, interest and root statuses list the event under Next (or no longer list a deleted one).

### 8. Memory step

Direct only. Follow rules → Memory step.

Done when: anything durable you learned is in `memories/`, or you found nothing new.

## Output

Direct, one short message in the root's language: what is now on the calendar (or removed), with day, date and time, and who was invited if anyone. One line per event, plus the `remember` line, if any.

Handed off: nothing of your own; the caller reports.
