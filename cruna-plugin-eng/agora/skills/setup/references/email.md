# Email step

Turns the root's email on or records it as off, and brings each member onto it. The rules it sets up are in the `rules` skill's `references/email.md`; read it first. Run from:

- **New root**, step 2d: the root part, with the answers already in `setup.md` → Interview, then the member part for every member interviewed (Member part says which steps wait for the member's own app).
- **Email branch** (an existing root, "turn on email"): the root part, asking the Email questions of [interview.md](interview.md) now, then the member part for each member present.
- **Join**, in a root with `email: on`: the member part for the newcomer, or for an existing member on a new app (Gmail must be connected in each app they use).

## Root part

1. **Decide.** Email off (declined in the interview, or the user changes their mind): write `email: off` in the `org.md` front-matter (rules → Replace), tick the root Email box "(off)" (shared services mode: and every In the group box "(email off)"), and stop. The root works exactly as before.
2. **Gmail.** Walk prerequisites item 7 in the app of the member present. Blocked: the step stops here with the box unticked, naming what is waiting.
3. **`org.md`**: `email: on` in the front-matter. Root-wide importance criteria recorded as in rules → Email → Where criteria live; none when the answer was "no preference". Replace `org.md` once, together with the member part's addresses when you have them.
4. **`memories/shared.md`**: its Email importance line, as set under rules → Email → Where criteria live when there are root criteria; `no preference`, or `not asked` when the question was skipped.
5. **`inbox.md`**: create it at the root from its template (rules → File kinds → templates), with an empty table and no `last_tidy_up` yet.
6. **`setup.md`**: tick the root Email box; add an unticked Email box to every Member section (rules → File kinds → templates). Members not present keep theirs unticked, naming what they still do ("Marta: connect Gmail in her app").

Done when: `org.md` says `email: on`, its root criteria (or none) are recorded, `memories/shared.md` has its Email importance line, `inbox.md` exists exactly once, and the root Email box is ticked.

## Member part

Steps 2–3 record answers, for any member who gave them; skip them when the member's addresses and Email importance line are already recorded. Steps 1 and 4 happen only with the member present, in their own app.

1. **Gmail** in this member's app: walk prerequisites item 7 (skip when the root part just did it in the same app).
2. **Addresses**: every address they write or forward from (interview Members round), in the Mail from column of their Members row. Solo roots skip this: there is no member mail.
3. **Importance**: their answer to the importance question, as the line `- Email importance: <criteria | no preference>` in `memories/<slug>.md`; `not asked` when skipped.
4. **`setup.md`**: tick their Email box (shared services mode: and their In the group box, which item 7's check proved; write `receives mail` in their Access row's Notes).

Done when: item 7 is ready in their app, their Mail from cell is filled (not solo), their memories file has its Email importance line, and their Email box is ticked; or item 7 is blocked and the box names what is waiting.
