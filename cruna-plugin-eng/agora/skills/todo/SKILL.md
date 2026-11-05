---
name: todo
description: Keeps the Agora to-do list, personal and shared. Use when a member adds a to-do ("add a to-do: renew the car insurance", "Nina should book the dentist", "someone needs to buy toner"), asks what is on a list ("what's on my list?", "what's open on Acme?"), finishes, reopens, takes over, reassigns or changes one ("done with the insurance", "I'll do it", "give it to Nina", "move it to Friday"), clears old to-dos ("remove everything closed before June"), or says yes to a suggested to-do. Also runs the todo handoff, a line starting "Agora todo as" sent by another plugin.
---

# To-do

A **to-do** is one action someone has to do, owned by a member or by `everyone` and visible to every member. Every change you make is one new **to-do event** in `todos/`; `todo.md`, the list members read, is rebuilt from the events after each change. The member talks; you match, write one event, and rebuild.

Every rule this skill applies is in the `rules` skill's `references/todos.md`, read in step 1; the steps below say when each rule fires.

## Handoff

A message starting `Agora todo as ` is the todo handoff (rules → Handoffs), in one of two shapes: `Agora todo as <slug> in <root>: edit <to-do id> text "<new text>"` (the text runs from the first `"` to the last) or `Agora todo as <slug> in <root>: done <to-do id>`. Run unattended and reply with the result line only:

1. **Step 1**: load the rules and `references/todos.md`, find the root by name and read `org.md`. `<slug>` must be a member with Role `github`; skip Identity and memories.
2. **Step 3**, then **match by id only**: the `add` event `todos/<to-do id>.md` must exist in `todos/` (one only in `todos/archive/` fails as archived).
3. **Settle**: `edit` sets the text to `<new text>` verbatim; `done` closes it. A `done` on a closed to-do fails as already closed. An `edit` to the text it already has writes nothing and returns `agora: ok — <to-do id> unchanged`.
4. **Step 8**, the event authored by `<slug>`. Skip step 9.

Failures, including the archived and closed cases above, follow rules → Handoffs → Check, then write. Success: `agora: ok — todos/<event file>.md (<edit | done> <to-do id>)`.

## Steps

### 1. Load the rules, find the root, know who is talking

Load the `rules` skill and read its `references/todos.md`. Follow rules → Finding the root and rules → Identity, then read `memories/<slug>.md` and `memories/shared.md` (rules → Memory step).

Done when: `org.md` is read, you know the member's slug, and their memories are read.

### 2. Pick the branches

One message can take several branches ("done with the insurance, and add: renew the car tax" is Done, then Add). Read the list first (step 3), then run them in the order said:

- **Add**: a new to-do, or a yes to one offered by `log`, `email` or a review suggestion → step 4.
- **Done**, **Reopen**, **Assign** (including "I'll do it"), **Edit** (text, due date, place) → step 5.
- **List**: "what's on my list?", "what's open for everyone?", "what's open on Acme?" → step 6.
- **Remove**: clear to-dos by date or field → step 7.
- **Calendar**: "put it on the calendar" → the `schedule` skill, with the to-do's text, due date and instance; a due date alone makes no event (references/todos.md → Due dates).

Done when: every request in the message maps to a branch.

### 3. Read the list

List `todos/`, and fold the events into each to-do's current state (references/todos.md → Current state). For a list question alone, `todo.md` is enough when it is not stale.

When the root has no `todos/` folder (a root set up before 0.5.0): for Add, create it at the root; for anything else, the root has no to-dos yet, so say so and offer to add one.

Done when: you know every to-do's text, owner, due date, place and open or closed state, or `todos/` is created empty.

### 4. Add

For each new to-do, settle its fields from the member's words, without asking what they didn't say:

- **Text**: one short line, an action ("Renew the car insurance").
- **Owner**: the member talking, unless they name another member or `everyone` (references/todos.md → Who sees and owns a to-do).
- **Due**: a date when they give one, resolved in the root's timezone; otherwise none. Ask only when the date they gave is unclear ("end of the month, this one or next?").
- **Place**: an instance when one is clear, otherwise root level; never ask (references/todos.md → Place).
- **Source**: the diary entry, email or review the to-do came from, when it came from one.

When an open to-do with the same meaning already exists, say so and ask before adding a second one. Something private: remind the member once that every member sees to-dos.

Done when: every new to-do has text, owner and place settled, and a due date or none. Then step 8.

### 5. Change

1. **Match** the to-do from the member's words against open to-dos (closed ones for Reopen; for a to-do not in `todos/`, also `todos/archive/`). Exactly one match: go on. Several: ask which, listing each with owner and due date, so the answer is one word. None: say so, and offer to add it.
2. **Settle the change**: Done closes it; Reopen opens it (an archived one: references/todos.md → Archive); Assign sets the owner ("I'll do it": the member talking); Edit sets each field the member changes.
3. Skip a change that changes nothing (closing a closed to-do), and tell the member it was already so.

Done when: each change names exactly one to-do the member meant, and its new values. Then step 8.

### 6. List

Answer from the current state (step 3):

- **Mine**: the member's open to-dos, in list order (references/todos.md → `todo.md`); then a one-line count of Everyone's.
- **Everyone**, **a member**, **an instance or interest**: the open to-dos that match, in list order.
- **What's done**: closed in the period asked, or the Recently done window.

Each line: text, owner, due date, instance. Listing writes nothing; go to step 9.

Done when: every open to-do that matches the question is in the answer, or the member knows there are none.

### 7. Remove

Follow references/todos.md → Remove, steps 1–4, matching against the folded state of `todos/` and `todos/archive/`. Nothing matches: say so, and go to step 9.

Done when: on a yes, every event file of every listed to-do is in the Drive trash and nothing else was touched; without a yes, nothing was trashed. On a yes, step 8 writes the trace; without one, step 9.

### 8. Write and rebuild

1. **Write** one event per change or new to-do, from the To-do event template (rules → File kinds → templates), named per references/todos.md → To-do events; a remove writes its one `remove` event. Existing events stay as they are.
2. **Rebuild `todo.md`**: list `todos/` again, fold every event, and write `todo.md` by replace (references/todos.md → `todo.md`).
3. **Statuses**: for every instance a changed to-do was or is now attached to, rebuild its `status.md`, then propagate (references/todos.md → In status).

Done when: each change has exactly one new event, `todo.md` matches the fold of every event in `todos/` with this run's Last updated, and every touched instance status shows its open to-dos.

### 9. Memory step

Follow rules → Memory step.

Done when: anything durable you learned is in `memories/`, or you found nothing new.

## Output

One short message, in the root's language:

- Each change, one line, echoing the to-do it acted on: "Added: Renew the car insurance (yours, due 7 Nov)", "Done: Renew the car insurance", "Call the accountant is now Nina's".
- A list: the lines from step 6.
- Remove: the count trashed, and that they are in the Drive trash for 30 days.
- Anything still open: a question about which to-do, a calendar event offered.
- The `remember` line, if any.

No ids or file paths unless the member asks.
