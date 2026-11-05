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
