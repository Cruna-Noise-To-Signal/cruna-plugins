# proud-ciabatta — implementation report

## M1

To-dos in conversation, English version only (`cruna-plugin-eng/agora/`). No Italian, version, catalog or `docs/agora` changes (M3).

### F1 — rules

- New `skills/rules/references/todos.md`: the single source for every to-do contract — visibility and owner, place (instance when clear, root otherwise, never ask), due dates without events, the to-do event format and naming (`-2` suffix), who writes events (conversation only; member-mail exception), the fold to current state, `todo.md` (layout, rebuild, staleness), the `status.md` Pending line, routine suggestions, the review To-dos section, remove and archive.
- `rules/SKILL.md`: vocabulary (to-do, to-do event), layout (`todo.md`, `todos/`, `archive/YYYY/`), file kinds (events are source, `todo.md` derived), a Status update rule line for attached to-dos, and a short `## To-dos` section pointing to the reference.
- `rules/references/templates.md`: to-do event (`add`, other actions, `remove`), `todo.md`, the `To-do:` Pending line, and a `## To-dos` section in the review template with a filled example.

### F2 — `todo` skill

New `skills/todo/` (SKILL.md with only `name`/`description`, `agents/openai.yaml`). Branches Add, Done/Reopen/Assign/Edit, List, Remove, Calendar (hands to `schedule`). Creates `todos/` on first Add; asks only when several to-dos match; one event per change; re-lists `todos/` before rebuilding `todo.md`; rebuilds affected instance statuses; memory step. The checker passes.

### F5 — help

Tour row for to-dos; FAQ entries "How do I keep a to-do list?", "Can others see my to-dos?", "How do I remove old to-dos?"; `todo.md` added to the "rewritten by Agora" answer.

### Choices beyond the spec

- `edit` events carry `text`, `due`, `interest`/`instance`; `none` clears a due date or makes a to-do root-level.
- `remove` events are a trace only (no effect on the fold), so restoring files from the Drive trash brings a to-do back; they are archived after 90 days by the year written.
- Orphan events (no matching `add`) stay out of `todo.md`; the reference says tidy-up flags them (to wire in F3).
- `todo` asks before adding a to-do that duplicates an open one, and moves an archived to-do's events back before reopening it.
- The review template already has the `## To-dos` section; `review` starts writing it in F3. The member-mail exception is stated in `references/todos.md`; `email` and the `inbox.md` outcome are wired in F4.

### To hand test

On a test root (English plugin), as one member, then a second:

1. In a root with no `todos/`, "add a to-do: renew the car insurance by Friday" → `todos/` created, one `add` event, `todo.md` lists it under your name with the date.
2. "Nina should book the dentist", "someone needs to buy toner", "call the Acme accountant" → owner Nina, `everyone`, and attached to `clients/acme` (no question asked); Acme's `status.md` Pending shows the `To-do:` line.
3. "what's on my list?", "what's open for everyone?", "what's open on Acme?" → right lines, overdue first; nothing written.
4. "done with the insurance", then reopen it; "I'll do it" on the toner; reassign the dentist to yourself; move the Acme call to next week → one new event each, no event edited, `todo.md` and Acme status updated, Recently done shows the closed one.
5. With two open to-dos matching ("done with the call"), the AI asks which.
6. Two members say "done with the toner" in overlapping conversations → two `done` events, one `todo.md`, the to-do closed.
7. "remove everything closed before <tomorrow>" → list shown, open ones untouched, trash notice; on yes, files in the Drive trash and one `remove` event. Repeat "including open ones" → open ones listed apart.
8. "can others see my to-dos?", "how do I remove a to-do?" → `help` answers from the FAQ.
