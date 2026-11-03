Request: #4 (proud-ciabatta)

## Problem Statement

Agora turns what a group pushes into diary entries, statuses and reviews, but it has no place for the things someone has to *do*. Today an action ("renew the car insurance", "send Acme the quote") lives only as a line in a diary entry or a status's Pending section. It surfaces in a review only if the AI happens to pick it out. Nobody can say "add this to my list", "what's on my list?" or "done with the insurance". Nobody can hand an action to another member or leave it for whoever gets to it first. Actions that arrive by email (#2) have nowhere to go either.

## Solution

Agora gains **to-dos**: personal (owned by a member) or common (owned by `everyone`), visible to every member.

- Members add, complete, reopen, reassign, edit, list and remove to-dos in conversation with a new `todo` skill. A to-do can carry a due date and is attached to an instance when one is clear; otherwise it lives at root level.
- Every change is a small **append-only to-do event** in a root `todos/` folder. The list members read, root `todo.md`, is **derived** from those events and rebuilt after every change. Two members editing at once never lose a to-do.
- Reviews gain a **To-dos** section (overdue, due ahead, opened and done since the last review). Routines never change to-dos. They only suggest "looks done" and "possible new to-do", and a member confirms in conversation.
- Email (#2) feeds to-dos. In conversation `email` offers to add one, and a member's emailed instruction "add to my to-dos" creates one directly. Routines only suggest to-dos from other mail.
- Old to-dos leave the way: closed ones older than 90 days are archived monthly, and members can trash to-dos by date or by field.

## User Stories

Adding and listing

1. As a member, I want to say "add a to-do: renew the car insurance", so that the action is kept without my thinking about where.
2. As a member, I want a to-do I add to be mine by default, so that I don't have to name myself.
3. As a member, I want to add a to-do for another member ("Nina should book the dentist"), so that I can hand work over.
4. As a member, I want to add a to-do for `everyone`, so that a shared chore waits for whoever picks it up.
5. As a member, I want to give a to-do a due date ("by Friday"), so that reviews warn me before it's late.
6. As a member, I want a to-do about a clear instance attached to it ("call the Acme accountant" → `clients/acme`), so that it shows in that instance's status.
7. As a member, I want a to-do with no clear instance kept at root level without being asked where it goes, so that adding one stays a one-line act.
8. As a member, I want to ask "what's on my list?", so that I see my open to-dos, overdue first, then by due date.
9. As a member, I want to ask "what's open for everyone?" or "what's open on Acme?", so that I see a shared or per-instance view.
10. As a member, I want to open `todo.md` on Drive and read the whole list grouped by owner, so that I can check it without the AI.
11. As a member, I want `todo.md` to show what was done in the last 14 days, so that I see recent progress.
12. As a member, I want each to-do in `todo.md` to link its instance and its source (diary entry, email, review), so that I can find the context.

Changing to-dos

13. As a member, I want to say "done with the insurance", so that the to-do closes, with the AI asking only when several open to-dos match.
14. As a member, I want to reopen a to-do closed by mistake, so that it's back on the list.
15. As a member, I want to take over a common to-do ("I'll do it"), so that others know it's handled.
16. As a member, I want to reassign a to-do to someone else, so that work moves without re-adding it.
17. As a member, I want to edit a to-do's text, due date or instance, so that it stays accurate.
18. As a member, I want every change to record who made it and when, so that the history of a to-do is clear.
19. As a member, I want to know that all to-dos are visible to every member, so that I don't put private things there.
20. As a member, I want a calendar event for a to-do only when I ask for one, so that the system calendar doesn't fill up with to-dos.
21. As a member, I want a to-do attached to an instance to appear under that instance's Pending in `status.md`, so that the status tells the whole story.

Concurrency

22. As a member, I want two of us completing or adding to-dos at the same time never to lose a change, so that I can trust the list.
23. As a member, I want a `todo.md` that is out of date or duplicated to be fixed by the next rebuild, so that a glitch never needs manual repair.

From the diary and email

24. As a member, I want `log` to offer "add a to-do?" when what I log sounds like an action, so that I don't have to say it twice, and I want it never to add one silently.
25. As a member, I want `email` to offer "add a to-do?" when an email asks for an action, with the email as the to-do's source, so that mail turns into work.
26. As a member, I want to email the assistant "add to my to-dos: call the bank", so that I can add a to-do from my phone's mail app.
27. As a member, I want an email nobody in the group wrote never to create a to-do on its own, only a suggestion in the review, so that outsiders can't fill my list.
28. As a member, I want `inbox.md` to record that an email became a to-do, so that the ledger shows what happened to it.

Reviews and routines

29. As a member, I want every review to have a To-dos section with overdue items, items due ahead, and what was opened and done since the last review, so that actions don't slip.
30. As a member, I want my personal review to show my to-dos first, then the shared ones, so that I see what's mine.
31. As a member, I want the general review to show every member's list and the shared list, so that the group sees everything.
32. As a member, I want the review to suggest "looks done: X" when a later diary entry shows it was done, so that I can close it with one word.
33. As a member, I want the review to suggest "possible new to-do: Z" from diary entries and email, so that actions hidden in notes surface.
34. As a member, I want suggestions to appear under Needs attention and to change nothing until someone confirms, so that routines never guess.
35. As a member, I want the monthly review to list to-dos open for more than 30 days with no due date, so that I drop or date them.
36. As a member, I want tidy-up to rebuild `todo.md` when it is stale, so that the list is right even if a conversation was cut off.

Removing and archiving

37. As a member, I want closed to-dos older than 90 days archived automatically every month, so that the active list stays fast without deleting anything.
38. As a member, I want to remove to-dos by date ("remove everything closed before June"), so that I can clean up in one go.
39. As a member, I want to remove to-dos by field ("remove everything about Acme", by owner, by `everyone`, by source), and combine filters, so that I can clear a client or project that ended.
40. As a member, I want removal to touch only closed to-dos unless I say "including open ones", so that I don't lose live work by accident.
41. As a member, I want to see the matching to-dos (count and titles, open ones listed apart) and confirm before anything is removed, so that removal is never a surprise.
42. As a member, I want to be told that removed to-dos go to the Drive trash and are recoverable for 30 days, so that I know the undo window.
43. As a member, I want each removal recorded (who, filter, count), so that the group knows what was cleared.
44. As a member, I want routines never to remove to-dos, so that only a person decides what disappears.

Setup, help and language

45. As a root owner, I want a new root to get its `todos/` folder at build time, and an existing root to get it on first use, so that nobody has to run setup again.
46. As a member, I want `help` to explain to-dos (what they are, who sees them, how to add, complete and remove them), so that I can use them without guessing.
47. As an Italian member, I want to-dos called "to-do" in Italian text, with file and folder names unchanged, so that both language versions share one Drive.
48. As a maintainer, I want the spec, the Italian glossary and `future-extensions.md` (Google Tasks) updated with this feature, so that the docs describe what ships.

## Implementation Decisions

Modules (skills, in both `agora` and `agora-ita`):

- **New skill `todo`**: add, complete, reopen, assign (including take over), edit, list, remove. It asks who it is talking to once per conversation, like `log`. It writes one to-do event per change, then rebuilds `todo.md` and the affected instance `status.md`. It ends with the memory step. It needs an OpenAI interface file like every other skill.
- **`rules`**:
  - Vocabulary: to-do, to-do event, `todo.md`.
  - Layout: root `todos/` (with `todos/archive/YYYY/`) and root `todo.md`.
  - File kinds: to-do events are **source**, append-only; `todo.md` is **derived**, never hand-edited, always rebuildable.
  - A **To-dos** section holding the shared conventions: the event format, how events fold into a to-do's current state, owner and visibility, place (instance when clear, root otherwise, never ask), due dates without calendar events, "routines suggest, never change", and the remove and archive rules. Put the detail in a `rules` reference file (e.g. a to-dos reference) rather than growing SKILL.md.
  - Templates: to-do event, `todo.md`, the review **To-dos** section, and the `status.md` Pending line for an attached to-do.
- **`review`**:
  - Tidy-up rebuilds `todo.md` when it is stale (any event newer than its last update), and attached instance statuses with it.
  - At the monthly run, tidy-up moves the events of to-dos closed more than 90 days ago into `todos/archive/YYYY/`.
  - Every review gains the To-dos section. Suggestions ("looks done", "possible new to-do", from diary and email) are listed there and under Needs attention.
  - The monthly review lists to-dos open for more than 30 days with no due date.
  - Personal and general ordering as stated in the Solution.
  - Unattended runs never write to-do events.
- **`log`**: when an entry reads as an action, offers to add a to-do with that entry as source; only on a yes does it hand over to `todo`.
- **`email`** (requires #2):
  - offers a to-do in conversation, with the email (Gmail id) as its source;
  - an explicit "add to my to-dos" instruction in member mail creates the to-do directly, owned by that member unless the mail names another owner;
  - third-party mail only yields suggestions;
  - the `inbox.md` outcome gains `to-do → <id>`.
- **`ask`**: answers list questions ("what's on my list?", "what's open on Acme?") from `todo.md`, falling back to the events when `todo.md` is stale.
- **`setup`**: the build step creates `todos/`. No new `setup.md` box. `todo` creates the folder on first use in a root that lacks it.
- **`help`** and its FAQ: what to-dos are, that every member sees them, how to add, complete and remove them, the 30-day trash window.

Data shapes:

- **To-do event**: `todos/<author>-YYYYMMDD_HHMM.md`, append-only. Front-matter `agora: todo`, `action: add | done | reopen | assign | edit | remove`, `author`, `created`. On `add`: `owner` (member slug or `everyone`), `due` (date, optional), `interest`/`instance` (optional), `source` (path or Gmail id, optional), and a one-line body with the to-do text. On every other action: `todo` (the id of the to-do it acts on), plus the changed fields (`owner` for assign, any of text/`due`/place for edit). On `remove`: the filter, the count, and the ids it trashed. Two events in the same minute by the same author get a `-2` suffix.
- **To-do id**: the file name of its `add` event, without extension.
- **Current state**: fold a to-do's events in `created` order. The last of `done`/`reopen` decides open or closed. The latest value of each field wins.
- **`todo.md`** (derived, root): front-matter `agora: todo-list`; `Last updated`; one section per member (overdue, then by due date, then undated), an `Everyone` section, a `Recently done` section (last 14 days). Each line holds text, owner, due, instance link and source link. Rebuilt by replace; duplicates cleaned like any derived file.
- **`status.md`**: an instance's Pending section lists its open to-dos (text, owner, due).
- **`inbox.md`** (from #2): outcome value `to-do → <id>`.
- **Archive**: `todos/archive/YYYY/` holds moved events, grouped by the year the to-do was closed. Archived to-dos are outside `todo.md` and reviews; `ask` searches the archive only when asked about old to-dos.

Behaviour contracts:

- Only a conversation creates, changes or removes to-dos. The single exception is an explicit instruction in member mail, which counts as the member's own request.
- A routine never writes a to-do event. It only suggests under Needs attention.
- Remove acts on closed to-dos unless the member says to include open ones. It always lists the matches and waits for confirmation. It trashes (Drive trash, 30 days) and never hard-deletes.
- No calendar event is created for a to-do unless the member asks, through `schedule`.
- All to-dos are visible to all members; there is no private to-do.
- Structure names stay in English in both language versions (`todo.md`, `todos/`, `todos/archive/`, the skill name `todo`, actions, front-matter keys, the headings `To-dos`, `Everyone`, `Recently done`). Content follows the root's language; the Italian term is "to-do".

Versioning: `agora` and `agora-ita` go up one minor version in both manifests and both catalogs. The spec, README, glossary and `future-extensions.md` are updated in the same change.

## Testing Decisions

- The plugin is prose (skills) with no executable code, so the test is **behaviour observed in a live root**, not internals. A good test states a member's action and what must then be true on Drive (and Gmail where email is involved), checked with real calls.
- **Seam: one manual end-to-end test script** for this release in `docs/agora/manual-tests/`, run on a test root as the assistant account, on Claude and on ChatGPT. It covers:
  - adding a personal, an assigned and an `everyone` to-do, one with a due date and one attached to an instance;
  - listing (mine, everyone, per instance) and reading `todo.md` on Drive;
  - complete, reopen, take over, reassign, edit;
  - two members completing the same to-do in overlapping conversations (`todo.md` ends correct, no lost event);
  - `log` offering a to-do;
  - `email` offering a to-do; member mail "add to my to-dos" creating one; third-party mail only suggesting;
  - a daily review (To-dos section, suggestions under Needs attention, nothing changed by the routine);
  - a monthly review (archive of closed to-dos older than 90 days, open-30-days-no-due list);
  - remove by date and by field, closed only, then including open ones, with the confirmation list and the trash notice;
  - an existing root with no `todos/` folder (created on first use).
- **Structural check**: the `portable-plugins` checker (cruna-core) passes, covering manifests, both catalogs, the new skill's frontmatter and interface file, and identical name and version in both languages.
- **Translation check**: every new term is in `docs/agora/glossary-it.md`, and the Italian skills mirror the English ones section for section.
- Prior art: `docs/agora/manual-tests/manual-test-agora-ita-v020.md` and the email test script from #2 (eager-rosetta).

## Out of Scope

- **Google Tasks** as a store or mirror: there is no official connector on Claude or ChatGPT, Tasks lists can't be shared, and custom connectors may not run in routines. Recorded in `future-extensions.md`.
- Private to-dos hidden from other members.
- Calendar events created automatically from due dates.
- Recurring to-dos.
- Subtasks, priorities or tags beyond owner, due and place.
- Routines creating, completing, reassigning or removing to-dos.
- Hard deletion of to-do events.

## Further Notes

- Decision trail: `grilled-idea.md` in this folder (rounds Q1–Q17, 2026-11-03).
- Depends on #2 (eager-rosetta) for the email hooks. Per the human's decision, this request is built on the same branch as #2, `feature/eager-rosetta`, while #2 is still in progress. If both land in one squash, a single minor bump covers both; the grilled idea's "0.5.0 after 0.4.0" holds only if #2 ships first.
- Google Tasks facts were checked on 2026-11-03 from vendor sources (official help pages were not reachable): read them as "not documented", not "confirmed absent". Re-check before picking up the future extension.
- `rules` and `review` are already long; keep to-do detail in a `rules` reference file.
