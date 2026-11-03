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
