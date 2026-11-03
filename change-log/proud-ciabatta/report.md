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

## M2

To-dos everywhere, English version only (`cruna-plugin-eng/agora/`). No Italian, version, catalog or `docs/agora` changes (M3). The checker passes.

### Review round 1 fixes (F1, F2)

- `references/todos.md` names **list order** (overdue, by due, undated) and the Recently done window once; `templates.md`, In status and `todo` step 6 point to it.
- `todo.md` lines leave the owner to the section heading; only Recently done lines repeat it (todos.md and the template agree).
- To-do events carry `created` to the second (`YYYY-MM-DDTHH:MM:SS`); the file name stays to the minute, and the name breaks only identical-timestamp ties.
- rules → File kinds: a to-do is corrected by a later to-do event.
- `todo`: step 7 is the pointer to Remove plus "nothing matches"; step 5.2 points to Archive for reopening; Add, Change and Remove each end with their next step.

### F3 — review

- Tidy-up gains item 5 **To-dos**: monthly archive (re-lists, skips files another run moved, so a second or concurrent run moves nothing new), orphan flags, `todo.md` rebuilt when stale, and instance statuses marked stale when an attached to-do has a newer event (item 6 rebuilds them). `todo.md` joins the duplicate rule. Later items renumbered.
- Step 3 writes the To-dos section per references/todos.md → Reviews, each member then Everyone in the shared file; suggestions from the look-back window's diary entries and ledger emails go there and under Needs attention. Update sections also fire on new to-do events.
- Step 4 messages: a member's items include their overdue and due to-dos; Everyone's lead the Everyone part; a suggestion ends with the one-word answer that confirms it.
- New step 5 (attended only): ask once to confirm suggestions, each yes through `todo`.

### F4 — hooks

- `rules/references/email.md`: member mail asking in so many words to add a to-do adds it, in conversation or tidy-up, author and owner the sender; other mail only offers (conversation) or suggests (routine). Ledger outcome `to-do → <id>`; `inbox.md` template gains an example row.
- `email`: To-do branch and step 7 (offered from Find and after File; member-mail requests added without an offer). Later steps renumbered.
- `log`: new step 8 offers each action as a to-do once, with the entry as source; only a yes hands it to `todo`.
- `ask`: a To-do questions branch reading `todo.md` (events when stale, archive only for old to-dos), and a hand-off to `todo`.
- `setup`: build and Verify include `todos/`; "What you can do now" gains a to-do line.

### Choices beyond the spec

- The member-mail to-do request is the one to-do event an unattended run writes (the review's inbox tidy-up), as `references/todos.md` → Who writes events already allowed; the spec's "unattended runs never write to-do events" is read with that exception.
- That `add` event is named and dated from the email's received time, so two concurrent runs add it once (same trick as filing).
- `to-do → <id>` joins a `filed`, `drafted` or `sent` outcome on the same row (`filed → …; to-do → …`) and replaces `flagged` or `ignored`, since one row per email holds one outcome otherwise.
- Edited `rules/references/email.md` and `templates.md` (the plan listed `email` as SKILL.md only): the ledger and tidy-up rules live there.
- Suggestions skip anything matching an existing open to-do (possible new) or closed one (looks done), and come only from the look-back window, so each appears once per period.
- `setup` Verify now expects `todos/`, so re-running setup on an older root creates it; `todo` still creates it on first use.
- The review template's To-dos example is relabelled as the shared file's order, not a personal one.

### To hand test

On the M1 test root (English plugin), email on, two members:

1. `log` "I need to call the accountant about Acme next week" → entry written, then one question "Add a to-do…?"; no → no event; repeat, yes → one `add` event with the diary entry as `source`, attached to `clients/acme`.
2. In `email`, "anything from the bank?" on an email asking for a document → the offer includes "add a to-do"; yes → `add` with `source: email <id>`, ledger row `to-do → <id>` (with `filed → …;` first when it was filed too).
3. From a member's own address, email the assistant "add to my to-dos: call the bank by Friday"; run a daily review unattended → one `add` event authored by that member, named from the received time, ledger `to-do → <id>` by routine, `todo.md` rebuilt. Run it again at once → no second event.
4. A third-party email "please send us the signed form" → no event; the review's To-dos section and Needs attention list "possible new to-do".
5. Log "booked the car service" while "Book the car service" is open; run the daily review → "looks done" in To-dos and Needs attention; nothing in `todos/` changed by the routine. Run it attended → one question; "yes" closes it through `todo`.
6. Personal review ("my week") → your to-dos first in Yours, Everyone's in Everyone else; general review → each member's, then Everyone's.
7. With a to-do closed over 90 days ago (hand-dated test events) and one open undated over 30 days, run the monthly review → the closed one's events in `todos/archive/YYYY/`, the open one under "No date, open over 30 days"; run it again → nothing moves.
8. Delete `todo.md` by hand, or add an event file by hand, then run any review → `todo.md` rebuilt, the attached instance status updated; an event naming an unknown id is flagged.
9. `ask` "what's on Nina's list?" → answer citing `todo.md`; a new root built by `setup` has an empty `todos/`.
