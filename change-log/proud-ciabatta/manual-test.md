# proud-ciabatta — hand-test manual

What to check by hand before release of Agora 0.5.0 (to-dos). The full script, with a Claude and a ChatGPT column per check, is [docs/agora/manual-tests/manual-test-agora-v050-todos.md](../../docs/agora/manual-tests/manual-test-agora-v050-todos.md): run it there and record results in its boxes, TO IMPROVE and FACT.

## Before you start

- Install `agora` 0.5.0 and `agora-ita` 0.5.0 from this branch, on Claude and on ChatGPT web (paid plan).
- A test root with two members (you and a second Google account) and the assistant account; email on. One root without `todos/` (set up before 0.5.0, or delete the folder by hand).
- Check every result on Drive, Gmail and Calendar yourself.

## The short path (about 30 minutes per platform)

1. **First use**: in the root without `todos/`, "add a to-do: renew the car insurance by Friday" → `todos/` created, one `add` event, `todo.md` lists it under your name; no calendar event.
2. **Add**: "Nina should book the dentist", "someone needs to buy toner", "call the Acme accountant next Tuesday" → owners Nina, `everyone`, you; the Acme one attached, shown in Acme's `status.md` Pending as a `To-do:` line. No question about place.
3. **List**: "what's on my list?", "what's open on Acme?" → right lines, overdue first; nothing written.
4. **Change**: done, reopen, "I'll do it", reassign, move the date → one new event each, `todo.md` and Acme status follow. With two "Call…" to-dos, "done with the call" asks which.
5. **Concurrency**: you and the second member say "done with the milk" at the same time → two `done` events, one `todo.md`.
6. **log**: log an entry with an action → one "Add a to-do?" question; yes adds it with the entry as source, no adds nothing. A dated action is offered too.
7. **email**: an outside email asking for something → offered as a to-do; your own email "add to my to-dos: call the bank" → added with no question, ledger `to-do → <id>`.
8. **Daily review (scheduled)**: the only new event is the member-mail add; To-dos section in the review file; "looks done" and "possible new to-do" in the To-dos section and under Needs attention. Run again → nothing new. Attended review → one question to confirm suggestions.
9. **Monthly review**: hand-dated events of a to-do closed over 90 days ago → moved to `todos/archive/YYYY/`; an undated open one over 30 days is listed; a second run moves nothing; an orphan event is flagged.
10. **Remove**: "remove everything closed before <tomorrow>" → list, trash notice, wait for yes; open ones untouched unless "including open ones"; one `remove` event; files in the Drive trash.
11. **Help**: "can others see my to-dos?" → yes, everyone sees them.
12. **Italian**: repeat 2, 4, 6 and 8 with `agora-ita` → same behaviour, structure names in English, "to-do" in the text.

## What changed in M3, worth a glance

- `log` now offers a to-do for every action, dated or not.
- The review's To-dos windows read the same in `todos.md` and `review` (look-back / look-ahead window).
- `docs/agora/agora-spec.md` has a To-dos section: check it matches what you see.
