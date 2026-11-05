# Agora to-do conventions

The shared rules for every skill that reads or writes to-dos (rules → To-dos). Templates for the to-do event, `todo.md`, the `status.md` Pending line and the review To-dos section: [templates.md](templates.md).

A root without a `todos/` folder has no to-dos: every skill works as before, and `todo.md` does not exist. The `setup` skill creates `todos/` when it builds a root; in a root set up before 0.5.0, the `todo` skill creates it the first time a member adds a to-do.

## Who sees and owns a to-do

- Every to-do is visible to every member: there is no private to-do. Say so when a member seems about to add something private.
- The **owner** is one member slug, or `everyone` for a shared to-do that waits for whoever picks it up. A to-do a member adds is theirs unless they name another member or `everyone`.
- A **github member** (rules → Vocabulary) owns a to-do only when a member names it ("a to-do for pm: add Outlook support"); it is never offered or guessed as an owner. Attached to a managed instance, that to-do becomes a GitHub request at the next sync, and the github member later edits its text to end `→ #<issue>` and closes it when the issue closes, through the `todo` handoff.
- Any member may complete, reopen, reassign, take over ("I'll do it": the owner becomes the member talking), edit or remove any to-do. Each change is its own event carrying its `author`, so the history says who did what.

## Place

A to-do is **attached** to an instance when one is clear: placed with certainty by the `log` skill's placement (indexes, then the member's words), or handed over by the skill that sent it (a diary entry's instance, a filed email's instance). Otherwise it lives at **root level**, with no `interest`/`instance`. Adding a to-do never asks where it goes.

An open attached to-do appears in its instance's `status.md` Pending (In status, below).

## Due dates

A to-do may carry a `due` date, resolved in the root's timezone ("by Friday" → the coming Friday). A due date creates no calendar event: an event is made only when a member asks, through the `schedule` skill.

## To-do events

Every change to a to-do is one **to-do event**: a source file, append-only (rules → File kinds). An event is created, never edited; a mistake is put right by a later event.

- **Name**: `todos/<author>-YYYYMMDD_HHMM.md`, the author's slug and the minute of the change in the root's timezone (a member-mail `add` takes the email's received time instead: [email.md](email.md) → Member mail). When that name is taken, add `-2`, then `-3`, before `.md`.
- **Id**: a to-do's id is the name of its `add` event, without `.md` (`simone-20261103_0912`).
- **Front-matter**: `agora: todo`, `action`, `author`, `created` (to the second, `YYYY-MM-DDTHH:MM:SS`, so the fold orders events by real time), then per action:
  - `add`: `owner`; optional `due`, `interest` and `instance`, `source`. The body is one line: the to-do's text.
  - `done`, `reopen`: `todo` (the id it acts on).
  - `assign`: `todo`, `owner`.
  - `edit`: `todo`, plus each field that changes: `text`, `due` (`none` clears it), `interest` and `instance` (`none` makes it root-level).
  - `remove`: `filter` (the member's filter in words), `count`, `todos` (the ids trashed). A remove event is a trace only: it acts on no to-do's state.
- **Source**: a path from the root to the diary entry or review it came from, or `email <Gmail message id>`.

### Who writes events

Only a conversation writes to-do events, through the `todo` skill: a member asked, or said yes to an offer. Two exceptions:

- Member mail asking in so many words to add a to-do counts as the member's own request and is added even by an unattended run ([email.md](email.md) → Member mail).
- A github member writes `edit` (text) and `done` events through the `todo` handoff (rules → Handoffs), since it acts for GitHub.

Any other email, and any routine, only **suggests** (Suggestions, below).

## Current state

To learn a to-do's state, **fold** its events: its `add` event and every event whose `todo` is its id, in `created` order (identical `created`, to the second: by file name).

- After `add`, it is open. The last `done` or `reopen` decides open or closed; its **closed date** is the `created` of the last `done`.
- Each field (`text`, `owner`, `due`, place) takes its latest value.
- It is **overdue** when open with a `due` before today.
- An event whose `todo` matches no `add` event in `todos/` or `todos/archive/` is an orphan: leave it out of `todo.md`, and the `review` tidy-up flags it.

## `todo.md`

Root `todo.md` is the to-do list members read: a **derived** file (rules → File kinds), never hand-edited, rebuilt by replace (rules → Replace) from the fold of every event in `todos/` (the archive left out).

- One `## <Name>` section per member who owns an open to-do, in `org.md` Members order; then `## Everyone`; then `## Recently done`. An empty Everyone or Recently done says "—".
- In a member or Everyone section: their open to-dos in **list order**: overdue first, then by due date, then undated (oldest first).
- **Recently done**: to-dos closed in the last 14 days, newest first, with who closed them.
- Each line: the text, the due date, a link to the instance when attached, a link to the source when there is one, and the id. The section heading names the owner, so only Recently done lines repeat it.
- **Rebuild** after every change, listing `todos/` again just before writing, so events another member wrote meanwhile are folded in. Duplicates are handled like any derived file (rules → Replace).
- It is **stale** when an event in `todos/` is newer than its Last updated line, or when it does not exist while `todos/` holds events.

Read the list from `todo.md`; when it is stale, fold the events instead, and rebuild it when the conversation may write.

## In status

An instance `status.md` Pending lists each open to-do attached to the instance, one line each (template in [templates.md](templates.md)), in list order (`todo.md`, above). Interest and root statuses roll these up like any Pending item. Every change that opens, closes, reassigns, redates or moves an attached to-do rebuilds the status of each instance it was or is attached to, then propagates (rules → Status update rule).

## Suggestions

Routines never write a to-do event: they never add, complete, reassign or remove one. What they notice, they **suggest**, in the review's To-dos section and under Needs attention:

- **Looks done**: "looks done: <text> (<diary entry or email>)", when a later diary entry or email shows the to-do was done.
- **Possible new to-do**: "possible new to-do: <text> (<diary entry or email>)", when a diary entry or an email asks someone to act.

A member confirms in conversation ("yes, done", "add it"), and the `todo` skill writes the event. Interactive skills (`log`, `email`) ask "add a to-do?" instead, and add one only on a yes.

## Reviews

Every review (daily, weekly, monthly) has a `## To-dos` section, built from the fold of `todos/`:

- **Overdue**: open to-dos past their due date.
- **Due ahead**: open to-dos due in the look-ahead window.
- **Opened** and **Done**: to-dos added or closed in the look-back window.
- **Suggestions**: Looks done and Possible new to-do (Suggestions, above).
- **Monthly** only: **No date, open over 30 days**: open to-dos with no `due`, added more than 30 days ago, as a prompt to drop or date them.

A personal review shows the member's own to-dos first, then Everyone's. The general review shows each member's, then Everyone's. Leave out empty parts; nothing at all: "Nothing new." A root without `todos/` has no To-dos section.

## Remove

Removing is a `todo` skill action, in conversation only; routines never remove.

1. **Filter**: by date (created, or closed, up to a date), by instance or interest, by owner, `everyone`, or by source (a path, or "from email"). Filters combine: a to-do must match all of them. Archived to-dos match too.
2. **Scope**: closed to-dos only, unless the member says to include open ones ("including open ones").
3. **Confirm**: list the matches, count and texts, the open ones listed apart, and say they go to the Drive trash, recoverable for 30 days. Wait for a yes to that list.
4. **Trash** every event file of each matched to-do (its `add` and every event naming its id). Never hard-delete.
5. **Trace**: write one `remove` event with the filter, count and ids, then rebuild `todo.md` and the affected statuses.

A to-do restored from the Drive trash is back as it was: its events fold again.

## Archive

At the monthly `review`, tidy-up moves the event files of each to-do closed more than 90 days ago into `todos/archive/YYYY/`, YYYY the year it was closed, and `remove` events older than 90 days into the year they were written. Nothing is deleted, and a second run moves nothing new.

Archived to-dos are outside `todo.md` and reviews. Search the archive only when a member asks about old to-dos, or when a to-do they name is not in `todos/`. Reopening an archived to-do first moves its events back into `todos/`.
