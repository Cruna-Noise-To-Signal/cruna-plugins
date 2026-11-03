# proud-ciabatta — review

## Round 1 — M1 (F1, F2, F5, English)

Verdict: **PASS** on both axes. The should-fix items below are carried into the M2 implementation run.

### Specs

Checked against spec.md using `git diff 62136871...HEAD` (commits 0e8ca38 F1, d810646 F2, 9530c33 F5, b392a5c M1).

Missing or partial (minor):
- Spec: "`todo.md` … Each line holds text, owner, due, instance link and source link." The `todo.md` template leaves owner out of lines in member sections (the `## <Name>` heading implies it), while `references/todos.md` says each line holds the owner. Align the two.
- Spec: "one section per member". `todos.md` narrows this to "per member who owns an open to-do". A reasonable reading, but it differs slightly.

Not asked for (small, all logged in report.md):
- Reopening an archived to-do first moves its events back into `todos/`.
- Remove also matches archived to-dos.
- `remove` events are archived after 90 days.
- Orphan events are flagged.
- `todo` asks before adding a duplicate of an open to-do (only when a duplicate exists).

Implemented, but could be tighter:
- Fold ties are broken "by file name". File names start with the author slug, so a done/reopen pair from two members in the same minute could resolve the wrong way.
- `rules/SKILL.md` File kinds still says "A correction is a new diary entry". For to-dos, a correction is a later event.

Verified: rules vocabulary, layout, file kinds, To-dos section and reference; event format, id, fold, staleness, Pending line; behaviour contracts (routines suggest only, member-mail exception, remove: closed-only default, confirm list, trash, trace); templates; `todo` skill (frontmatter, openai.yaml, ambiguity asking, one event per change, first-use folder, calendar via `schedule`, memory step); help tour row and FAQ.

### Standards

Standards: AGENTS.md, cruna-core `writing-for-agents` and `portable-plugins`, plus the smell baseline. Structure matches the sibling skills (frontmatter, openai.yaml, numbered steps with "Done when:", Output, Memory step). Should-fix:

1. **Possible Duplicated Code**: `todo/SKILL.md` step 7 points to `references/todos.md → Remove` and then repeats its procedure almost word for word. Step 5.2 repeats the archive-reopen rule. Keep the pointer plus only what the skill adds ("Nothing matches: say so and stop").
2. **Possible Duplicated Code**: the ordering rule (overdue, by due, undated) and the 14-day Recently-done window are stated in `todos.md`, again in `templates.md` (~130, ~213) and again in `todo/SKILL.md` step 6. State each rule once in `todos.md` and point to it from the other places.
3. **Implicit branch flow**: in `todo/SKILL.md`, steps 4, 5 and 7 never say "then step 8", so read in order, Add falls through into Change and Remove. End each branch with its next step, or state it in step 2.
4. **Minor**: `templates.md` now holds the to-do templates too. That is consistent with the repo convention, so no change.

## Round 2 — M1 + M2 (F1–F5, English)

Verdict: **PASS** on both axes. The Round 1 items are fixed. The should-fix items below go to the M3 run.

### Specs

Round 1: all addressed except one minor item. Spec: "one section per member". `todos.md` still says "per member who owns an open to-do". Decision: keep the implementation (no empty sections) and state it in the `agora-spec.md` update.

(a) Missing or partial: none.

(b) Not asked for:
1. `log/SKILL.md` step 8 offers a to-do only for an action "with no calendar event for it". Spec: "`log`: when an entry reads as an action, offers to add a to-do". A dated action never gets the offer, and this narrowing was not logged. Decision: drop the clause.
2. Accepted deviations from report.md M2:
   - received-time naming of the member-mail `add`;
   - `to-do → <id>` joins filed/drafted/sent and replaces flagged/ignored on the ledger row;
   - edits to `email.md` and `templates.md`;
   - suggestions de-duplicated against existing to-dos;
   - `setup` Verify expects `todos/`.

(c) Could be tighter:
1. The unattended member-mail add is justified by the Behaviour contract's single exception and is stated consistently. The M3 `agora-spec.md` update must state this exception explicitly.
2. The member-mail duplicate check looks only at the base name, not the `-2`/`-3` variants. Concurrent runs can still both create the same name, and tidy-up flags the duplicate. This matches the existing filing trick, so it is minor.
3. Review step 3.1 says To-dos cover "the look-back and look-ahead windows", while `todos.md → Reviews` says Opened/Done are "since the previous review of the same period". Use one wording.

Verified: tidy-up item 5 (idempotent monthly archive, orphan flags, stale rebuild, duplicates rule); To-dos section and suggestions under Needs attention; the monthly open-30-days list; personal and general ordering; attended confirmation; the `email` hooks (offer, member-mail add, third-party suggest only, ledger); the `log` offer; `ask` (stale fallback, archive only for old); `setup` build, Verify and first steps.

### Standards

Round 1 should-fix items 1–3 and the File kinds note: all fixed. Renumbered cross-references are consistent, and structure matches the house shape. M2 should-fix:

1. **Possible Duplicated Code**: the member-mail add rule appears in `todos.md:39`, in `email.md:26` (same fields plus naming) and in `email/SKILL.md` step 7.2. Keep the exception as one sentence in `todos.md` and point to `email.md → Member mail` for the procedure.
2. **Rule conflict**: `todos.md → To-do events` names events by the time of the change, while member-mail adds are named by the email's received time. The naming rule should mention or point to the exception.
3. **Stale wording**: `todos.md` opens with "The `todo` skill creates `todos/` the first time…" as the general case. Reword it as the fallback for roots set up before 0.5.0, since `setup` now creates it.
4. **Weak completion criterion**: `email/SKILL.md` step 6 gains item 8 "Offer a to-do", but its "Done when" doesn't mention it. Add it.
