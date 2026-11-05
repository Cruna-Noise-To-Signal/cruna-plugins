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
