# Review — sleepy-focaccia

## M1 — round 1 (F1–F4, English) — FAIL

### Specs

Verdict: **FAIL**. Diff `4f9e85f...HEAD` (438fb40, b13808c, 6b7c401, bda5798, a24ccee, c499679).

**Implemented wrongly**
1. **Must fix — `## Calendar conventions` heading deleted.** In `cruna-plugin-eng/agora/skills/rules/SKILL.md` the new `## Handoffs` section replaced the heading instead of being inserted before it; the calendar bullets now sit under Handoffs. Four skills still point to "rules → Calendar conventions" (`schedule/SKILL.md:8`, `setup/SKILL.md:68`, `log/SKILL.md:63`, `file-document/SKILL.md:88`). The spec only asks to add "the Handoffs section". Fix: restore the heading after the Handoffs result bullet.
2. Minor — `add-interest` handoff step 7 skips propagation when only the description changes; the spec lists "5. propagate" unconditionally. Acceptable unless a status shows the description; note it.

**Missing / partial:** none for F1–F4. Handoff shapes match the spec line for line; version note, who/where, skipped steps, failure, English result line, language rule, bridge file in Layout, `github:` after `events`, Identity excluding github members, the `todos.md` exception, the managed-instance reply, match-by-id in `todo`, idempotent steps in `add-interest` all present.

**Scope creep:** none of substance ("check, then write" and the separator parsing note make existing spec rules explicit).

**Implementer's doubts**
- Conventions "Since" as `2026-11-05, pm` vs spec `<date> by pm`: keeping the template format is better, but the spec must be amended in M3 (AGENTS.md rule). Not blocking.
- add-interest storing `<name>: <description>` verbatim: acceptable; a second call refreshes both (story 24).
- Same-text edit returns ok with nothing written: acceptable, fits story 26.
- Archived to-do fails: acceptable, safe.

### Standards

Verdict: **FAIL**. Standards: AGENTS.md and cruna-core `writing-for-agents` (no `guidelines` skill).

**Must fix**
1. Same as Specs 1: lost `## Calendar conventions` heading in `rules/SKILL.md` (~lines 151–178). Calendar bullets read as handoff rules (co-location break) and four pointers dangle. Restore the heading, single blank line.

**Judgement calls (not blocking)**
- Possible Duplicated Code (suppressed): each skill's Handoff section repeats its shape from rules → Handoffs, endorsed by the plan's "Handoff detection" risk. But each skill's "Fails, writing nothing:" list partly repeats rules' "Check, then write" / "Who and where" (unknown root, non-github slug): keep only skill-specific failures, point to rules for shared ones.
- Possible Duplicated Code: add-interest step 1 restates the folder-name rule owned by rules → Layout; a pointer would do.
- Possible Speculative Generality: rules → Vocabulary "slug `pm` unless the handoff names another"; could be the plain rule that the slug comes from the line.

**Fine:** frontmatter only `name`/`description`, handoff triggers in descriptions without angle brackets; F1–F4 content as listed under Specs; commit prefixes follow the plan.

## M1 — round 2 (F1–F4, English) — PASS

### Specs

Verdict: **PASS**. Revision commits 58457e4, 58b7d0e, 9b47e71, e15e1f3, 3f1e3d4, 1a5f162.

**Round-1 findings:** all fixed — Calendar conventions heading restored after the Handoffs Result bullet (four pointers resolve); add-interest step 7 propagates "whenever step 3 or 4 changed anything", meeting spec "5. propagate" and story 24; `log`/`todo` failure lists point to "rules → Handoffs → Check, then write" and keep only their own failures (archived, already closed, unchanged text); add-interest keeps only its own checks (not `products`, bad folder name, non-github Role), folder-name rule is a pointer to rules → Layout; Vocabulary reads "the slug its handoff lines carry (usually `pm`)", matching "Agora accepts whatever slug the handoff names".

**Missing / partial:** none for F1–F4. **Scope creep:** none. **Implemented wrongly:** nothing blocking. Nits:
- add-interest handoff says "Run these steps unattended instead of the ones below" and later "Skip step 7 below" (Memory) — redundant but harmless.
- A second call finding the instance unchanged but its Conventions row missing adds the row without propagating — acceptable, no status shows Conventions.

**Known deferral:** Conventions "Since" written `2026-11-05, pm` vs spec `<date> by pm`; spec amended in M3.

### Standards

Verdict: **PASS**. Standards: AGENTS.md + cruna-core `writing-for-agents`.

**Round-1 findings:** all addressed (heading at `rules/SKILL.md:174`; failure lists deduplicated without losing cases; folder-name pointer to `rules/SKILL.md:67`; slug wording; single owner for the index rewrite in add-interest step 7).

**New judgement calls (not blocking)**
- Possible Duplicated Code (acceptable): add-interest step 1's own check for a non-github Role is justified, as add-interest is the one handoff allowed to create the slug.
- Sediment: `report.md` "Decisions taken" still says add-interest description-only update has "no status propagation"; strike or annotate.
- Wording: `todo/SKILL.md` "Failures, including the archived and closed cases above, follow rules → Handoffs → Check, then write" reads as if rules owns those cases; "Every failure writes nothing (rules → Handoffs → Check, then write)" is plainer.

**Fine:** frontmatter, single owner per meaning, no negation-heavy steering, checker passes, later-milestone items left as scoped.

## M2 — round 3 (F5, F6 + M1 nits, English) — PASS

### Specs

Verdict: **PASS**. Diff `c018a88..HEAD`.

**Missing / partial:** none.
- review: tidy-up leaves the bridge file out of every check (`review/SKILL.md:48`, rule also in `rules/SKILL.md:65`), meeting "Tidy-up leaves the bridge file alone and doesn't count it when checking whether an index is stale". The github member gets an open-to-dos-only section (`:99`); an empty one is dropped by the existing rule; no personal message (`:42`).
- setup: github-member paragraph (`setup/SKILL.md:12`) covers Join, checklist, Access, memories, routines; Offboard has a github-member path (`:138–140`); `setup.md` template excludes github members (`templates.md:54`).
- ask: cites `github:` refs (`ask/SKILL.md:58`).
- help: conditional overview line + FAQ entry naming `products/`, `pm`, `cruna-pm-github` and "next sync", two or three lines.
- M1 nits: all three fixed.

**Scope creep (minor, acceptable):** Offboard's relink warning isn't in the spec but is accurate (add-interest step 2 recreates the member) — keep. `prerequisites.md:48` "each member with an email" is a justified leak fix.

**Implementer's doubts:** review asked for `pm` → general message (consistent with "Personal messages are never built for a github member"); four-line cap matches the existing Done-when (`:111`), rest shown as a count; no GitHub mention in `interview.md`, so no prohibition needed; relink warning fine.

**Github member leak sweep:** Identity (`rules:141`) excludes; checklist/Access/sharing/routines via `setup:12`; sharing check email-gated (`prerequisites:48`); `todo` owner default in `todos.md:11`; memories/calendar/Access via Vocabulary (`rules:16`); `pm` in to-do sections intentional (story 15); address-matching sites (`schedule`, `file-document`, `email`) harmless since the member's Email, Mail from and Personal calendar are empty. No leaks.

**Non-blocking:** after Offboard, open to-dos owned by the removed slug keep an owner not in Members; `todo.md`'s "Members order" rule doesn't say where they go. Not required by the spec.

### Standards

Verdict: **PASS**. Standards: AGENTS.md + cruna-core `writing-for-agents`.

F5 and F6 meet acceptance; the three M1 nits are fixed cleanly.

**Non-blocking judgement calls**
1. Possible Duplicated Code: `setup/SKILL.md:12` re-lists what a github member lacks (already in rules → Vocabulary); `review/SKILL.md` step 2 restates the bridge-file exclusion from rules → Layout. A pointer plus the scoping clause would do.
2. Wording conflict, `review/SKILL.md` general message item 3: "all of them in list order … four lines at most" — drop "all of them".
3. `help/references/faq.md` hard-codes `pm` as the slug; acceptable for user-facing text but drifts if another slug is used.
4. Possible Shotgun Surgery: github-member exclusions spread across review, setup, prerequisites, templates, help; each points back to rules → Vocabulary and the plan's risk list accepts it.
