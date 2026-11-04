Manual test for Agora 0.5.0, to-dos (request #4, proud-ciabatta). Release check from agora-spec → Release checks.

Status: ready to run, not yet run. Record results in the boxes and notes below, then fill TO IMPROVE and FACT at the end, as in the earlier manual tests.

## Setup for the test

- A test root with at least two members (you and **Member B**, a second Google account) and a dedicated **assistant account** (Drive, Calendar, Gmail). Email on for scenarios 7 and 8.
- At least one instance with a clear name (here `clients/acme`) and a few diary entries.
- A second root, or the same root with `todos/` deleted by hand, **set up before 0.5.0** for scenario 1.
- Install `agora` 0.5.0 (and `agora-ita` 0.5.0 for the Italian pass) from this repo.
- Run every scenario on **Claude** and on **ChatGPT web on a paid plan**. Check every result with real looks at Drive, Gmail (signed in as the assistant account) and Calendar, never the AI's word.
- Mark each check `[x]` when it holds, and write what you saw under Notes, especially anything that differs.

| Platform | Plan | Tester | Date | Plugin version |
|---|---|---|---|---|
| Claude | | | | |
| ChatGPT | | | | |

## 1. A root with no `todos/`: created on first use; a new root has it

1. In the root set up before 0.5.0 (no `todos/`), ask "what's on my list?".
2. Say "add a to-do: renew the car insurance by Friday".
3. Build a new root with `setup` (or check one built by 0.5.0).

| Check | Claude | ChatGPT |
|---|---|---|
| The list question says there are no to-dos yet and offers to add one; nothing is created | [ ] | [ ] |
| The add creates `todos/` at the root and one `add` event `todos/<you>-YYYYMMDD_HHMM.md` (`agora: todo`, `action: add`, `author`, `created` to the second, `owner: <you>`, `due` = the coming Friday, one-line body) | [ ] | [ ] |
| `todo.md` at the root: `agora: todo-list`, Last updated, a `## <Your name>` section with the to-do and its due date, `## Everyone` and `## Recently done` with "—" | [ ] | [ ] |
| No calendar event was created for the due date | [ ] | [ ] |
| The new root has an empty `todos/` and no `todo.md`; setup's "What you can do now" mentions to-dos | [ ] | [ ] |

Notes:

## 2. Adding: personal, assigned, everyone, attached, root level

1. "Nina should book the dentist" (use Member B's name).
2. "Someone needs to buy printer toner".
3. "Call the Acme accountant next Tuesday".
4. "Add a to-do: think about the summer holiday".

| Check | Claude | ChatGPT |
|---|---|---|
| Each add asks nothing about place, and writes exactly one `add` event | [ ] | [ ] |
| Owners: Member B's slug, `everyone`, you, you | [ ] | [ ] |
| The Acme to-do has `interest`/`instance` for `clients/acme` and its `due`; the holiday one has neither (root level) | [ ] | [ ] |
| `clients/acme/status.md` Pending has `To-do: Call the Acme accountant — <you>, due <date>`; the interest and root statuses roll it up | [ ] | [ ] |
| `todo.md`: your section (in list order: overdue, by due date, undated), Member B's section, Everyone with the toner; lines carry due date, instance link and id, no owner repeated | [ ] | [ ] |
| Asking to add "buy toner" again: the AI says it exists and asks before adding a second | [ ] | [ ] |
| "Add a to-do: book my private medical test": reminded once that every member sees to-dos | [ ] | [ ] |

Notes:

## 3. Listing and reading `todo.md`

1. "What's on my list?", "what's open for everyone?", "what's open on Acme?", "what's on Nina's list?".
2. Open `todo.md` in the Drive app.

| Check | Claude | ChatGPT |
|---|---|---|
| Mine: your open to-dos, overdue first, then by due date, then undated; then a one-line count of Everyone's | [ ] | [ ] |
| Everyone, Acme and Member B: only the matching open to-dos | [ ] | [ ] |
| Listing writes nothing (no new event, `todo.md` Last updated unchanged) | [ ] | [ ] |
| `ask` "what did Nina finish this week?" answers from `todo.md`, citing it | [ ] | [ ] |
| `todo.md` reads cleanly in the Drive app; links to the instance and source open | [ ] | [ ] |

Notes:

## 4. Complete, reopen, take over, reassign, edit

1. "Done with the insurance", then "reopen the insurance".
2. "I'll do it" about the toner.
3. "Give the dentist to me".
4. "Move the Acme call to Friday", then "make the holiday one about Acme".
5. Add a second to-do starting with "Call" ("call the bank"), then say "done with the call".
6. "Done with the insurance" twice in a row.

| Check | Claude | ChatGPT |
|---|---|---|
| Each change writes exactly one new event (`done`, `reopen`, `assign`, `edit`) naming the to-do's id; no existing event is edited | [ ] | [ ] |
| `todo.md` after each: the insurance moves to Recently done (with who closed it) and back; the toner moves from Everyone to your section; the dentist moves to your section | [ ] | [ ] |
| The Acme edit changes its due date; Acme's status Pending follows; moving the holiday to Acme adds it to Acme's Pending | [ ] | [ ] |
| "Done with the call" asks which, listing both with owner and due date; nothing written until you answer | [ ] | [ ] |
| Closing a closed to-do: told it is already done, no event | [ ] | [ ] |
| The AI echoes each change in one line ("Done: Renew the car insurance") | [ ] | [ ] |
| "Put the Acme call on the calendar" creates the event through `schedule`, `[clients/acme] …` | [ ] | [ ] |

Notes:

## 5. Two members completing the same to-do at once

1. Add "Buy milk" for `everyone`.
2. You and Member B, in two conversations open at the same time, both say "done with the milk" within a minute; then Member B adds a to-do while you complete another.

| Check | Claude | ChatGPT |
|---|---|---|
| Two `done` events exist (different authors), none lost | [ ] | [ ] |
| Exactly one `todo.md` remains (any duplicate trashed), with the milk in Recently done | [ ] | [ ] |
| The concurrent add and done both show in `todo.md` (after the next change or review if one write lost the race) | [ ] | [ ] |

Notes:

## 6. `log` offers a to-do

1. "Log: I spoke to Acme, I need to send them the revised quote by Thursday." Answer no.
2. Log a similar entry with an action and answer yes.
3. Log a plain fact with no action ("Greta slept well").

| Check | Claude | ChatGPT |
|---|---|---|
| The diary entry is written first, then one question "Add a to-do: …?" listing the action | [ ] | [ ] |
| No: no event in `todos/` | [ ] | [ ] |
| Yes: one `add` event, `source` = the diary entry path, attached to the entry's instance, `due` from the words | [ ] | [ ] |
| A dated action still gets the offer (the calendar question may come too) | [ ] | [ ] |
| No action: no to-do question | [ ] | [ ] |

Notes:

## 7. `email` offers a to-do; member mail adds one

1. Send the assistant (from an outside address) "Please send us the signed form by Monday". In conversation: "anything new in the inbox?"; answer yes to the to-do offer.
2. From your `Mail from` address, email the assistant "add to my to-dos: call the bank by Friday". In conversation: "anything from me?".
3. Forward member mail asking for an action, ask to file it, and answer yes to the to-do offer.

| Check | Claude | ChatGPT |
|---|---|---|
| The outside email's offer includes "add a to-do"; on yes, one `add` with `source: email <id>`; `inbox.md` row `to-do → <id>` | [ ] | [ ] |
| The member-mail request is added with no question: `author` and `owner` you, named and `created` from the email's received time, `source: email <id>`, ledger `to-do → <id>` | [ ] | [ ] |
| Filed and to-do on one row: `filed → <path>; to-do → <id>` | [ ] | [ ] |
| The AI echoes "Added: …" for each | [ ] | [ ] |

Notes:

## 8. Daily review: To-dos section and suggestions, nothing changed by the routine

1. From your `Mail from` address, email "add to my to-dos: renew the passport".
2. From an outside address, email "please reply to us about the trip".
3. With "Book the car service" open, log "booked the car service for Monday".
4. Let the scheduled daily review run (unattended). Note the files in `todos/` before and after.
5. Run the scheduled review again at once (or a second member's routine).
6. Run "daily review" in conversation; answer "done" to the looks-done suggestion and "no" to the possible new one.
7. Ask "my week" (personal) and "the week" (general).

| Check | Claude | ChatGPT |
|---|---|---|
| The unattended run writes exactly one event: the member-mail `add` (passport), author you, ledger `to-do → <id>` by `routine`; nothing else in `todos/` changed | [ ] | [ ] |
| The second run adds no second passport event | [ ] | [ ] |
| The review file has `## To-dos`: Overdue, Due ahead, Opened, Done, Suggestions (each member's, then Everyone's) | [ ] | [ ] |
| Suggestions "looks done: Book the car service (…)" and "possible new to-do: Reply about the trip (email …)" appear in the To-dos section **and** under Needs attention; the outside email made no event | [ ] | [ ] |
| The chat message: your overdue/due to-dos in your items; the suggestion ends with the one-word answer ("Say 'done' to close it") | [ ] | [ ] |
| `todo.md` rebuilt by the run, Last updated `by routine` | [ ] | [ ] |
| Attended run: one question listing the suggestions; "done" writes one `done` event through `todo`; "no" writes nothing | [ ] | [ ] |
| Personal review: your to-dos first, Everyone's in Everyone else; general: each member's, then Everyone's | [ ] | [ ] |
| An Update section is added when to-do events arrive after the review file was written | [ ] | [ ] |

Notes:

## 9. Monthly review: archive and old undated to-dos

1. By hand, create the events of a to-do closed more than 90 days ago (an `add` and a `done` with `created` dates over 90 days back), an open undated to-do added over 30 days ago, and an event whose `todo` names no existing id.
2. Run "monthly review" (or the scheduled one). Then run it again.
3. Ask "reopen <the archived to-do>".

| Check | Claude | ChatGPT |
|---|---|---|
| The old closed to-do's events are in `todos/archive/YYYY/` (the year it was closed); nothing deleted | [ ] | [ ] |
| The review's To-dos lists the undated one under "No date, open over 30 days" | [ ] | [ ] |
| The orphan event is flagged under Needs attention with its file name and id | [ ] | [ ] |
| The second run moves nothing | [ ] | [ ] |
| The archived to-do is gone from `todo.md`; reopening moves its events back to `todos/` and writes one `reopen` | [ ] | [ ] |
| Deleting `todo.md` by hand and running any review rebuilds it | [ ] | [ ] |

Notes:

## 10. Remove by date and by field

1. "Remove everything closed before <tomorrow>". Answer no, then repeat and answer yes.
2. "Remove everything about Acme". Then "remove everything about Acme, including open ones", answer yes.
3. Restore one removed event file from the Drive trash and ask "what's on my list?".

| Check | Claude | ChatGPT |
|---|---|---|
| The AI lists the matches (count and texts), says they go to the Drive trash for 30 days, and waits; on no, nothing is trashed | [ ] | [ ] |
| On yes: every event file of each matched to-do is in the Drive trash; open to-dos untouched | [ ] | [ ] |
| One `remove` event with `filter`, `count`, `todos` (ids); `todo.md` and statuses rebuilt | [ ] | [ ] |
| "About Acme" alone touches only closed Acme to-dos; with "including open ones" the open ones are listed apart before the yes | [ ] | [ ] |
| A restored to-do is back as it was | [ ] | [ ] |
| No routine ever trashed a to-do (check the trash after the review runs) | [ ] | [ ] |

Notes:

## 11. Help

1. Ask "how do I keep a to-do list?", "can others see my to-dos?", "how do I remove old to-dos?", "can I edit todo.md?".

| Check | Claude | ChatGPT |
|---|---|---|
| Answers from the FAQ: all members see every to-do, closed-only removal unless told, confirmation, 30-day trash, monthly archive, `todo.md` rewritten by Agora | [ ] | [ ] |

Notes:

## 12. Italian version

Repeat scenarios 2, 4, 6 and 8 with `agora-ita`.

| Check | Claude | ChatGPT |
|---|---|---|
| Same behaviour; structure names (`todo.md`, `todos/`, the actions and front-matter keys, `## To-dos`, `## Everyone`, `## Recently done`, `To-do:`, `to-do → <id>`) in English, content in Italian, "to-do" used for the concept (never "attività") | [ ] | [ ] |
| An English and an Italian member sharing one root read and change the same to-dos | [ ] | [ ] |

Notes:

## Results

TO IMPROVE:
-

FACT:
-
