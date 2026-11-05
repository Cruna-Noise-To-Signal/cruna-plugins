# sleepy-focaccia — implementation report

## M1

Handoffs work: F1–F4 in the English plugin (`cruna-plugin-eng/agora/`) only. No version bump, no Italian, no spec/docs changes (M3).

### What changed

- **F1 `rules`** (`skills/rules/SKILL.md`, `references/templates.md`, `references/todos.md`):
  - Vocabulary: Member now covers github members; new **Github member** (Role `github`) and **Managed instance** (Conventions row `managed by cruna-pm-github`).
  - Layout: the bridge file `YYYY-MM-DD_hh_mm_last-check.md` in the tree, plus one paragraph: belongs to `cruna-pm-github`; never read, indexed, filed, flagged or trashed; ignored by the stale-index check.
  - Identity leaves github members out and points to Handoffs for who is talking.
  - New **Handoffs** section: detection ("a message that starts `Agora <skill> as `"), the v1 version note, the four lines verbatim from the spec, separator parsing, and the rules (who and where, check-then-write, skipped steps with "where the skill would ask, the handoff fails", language, the English result line; a connector failure returns a failed line instead of rules → When something fails).
  - Templates: a `pm` row in the `org.md` Members example and a managed Conventions row, with a paragraph on how both are written; the diary template's optional `github:` line after `events`.
  - `todos.md`: a github member owns a to-do only when named, never offered or guessed; a second exception under Who writes events for `edit`/`done` through the `todo` handoff.
- **F2 `log`**: description names the handoff prefix; a Handoff section mapping the unattended run onto steps 1–10 (author = slug, `events: []`, `github:` refs, Summary in root language, Entry verbatim; steps 5, 8–10 skipped; no tracker routing or `add-interest` offer); the failure list and `agora: ok — <diary path>`. Output gains the managed-instance line "noted; it becomes a GitHub request at the next sync".
- **F3 `todo`**: description names the prefix; a Handoff section for `edit … text "…"` and `done …`: match by id only in `todos/`, one event authored by the slug, step 8 rebuild, failure list, `agora: ok — todos/<event> (<edit|done> <id>)`.
- **F4 `add-interest`**: description names the prefix; a Handoff section of seven idempotent steps (check, member, interest, instance-or-description-update, convention, one `org.md` replace, propagate) and the result-line forms.
- **M1 fix**: the checker rejected `<`/`>` in descriptions, so the descriptions say `a line starting "Agora log as" sent by another plugin` (the full shape lives in each skill's Handoff section and in rules).

Checker (`cruna-core` `check_plugins.py`): **✔ Valid for Claude and OpenAI**.

### Testing

Not testable by code: the change is skill prose, and the only seam is a handoff line typed in Claude Code against a test root on Drive. No `tdd` cycle was possible; the hand test is [manual-test.md](manual-test.md) → M1.

### Decisions taken where the spec was silent

- **Root matching**: `<root>` matches `org.md` `name` or the root folder name (`.agora/pm.yml` calls it the folder "as in its org.md").
- **Instance name vs description**: the add-interest line's `<name>: <description>` is stored verbatim as the instance's description in `products/index.md`, so the display name has a home and a second call updates both.
- **Conventions "Since"**: written in the existing template format `2026-11-05, pm` rather than the spec's `<date> by pm`. Change the template if the spec's form is wanted.
- **`edit` to the same text**: returns `agora: ok — <id> unchanged` and writes nothing (safe for a re-run sync); `done` on a closed to-do fails, as the plan says.
- **Archived to-dos**: a handoff id found only in `todos/archive/` fails as archived rather than moving events back.
- ~~**add-interest description-only update**: rebuilds only `products/index.md`; no status propagation (nothing in status changes).~~ Superseded by M1 revision 1: a description-only update propagates too.
- **Near duplicates**: skipped in the handoff (exact folder-name match is "update"; no question).

### Left for later milestones / noticed

- F5 (review, setup, ask) still list members without filtering github members: Access, setup checklist, reviews and memories are M2.
- Pre-existing: `todo/SKILL.md` frontmatter is not strict YAML (`"add a to-do: renew…"` has `: ` inside a plain scalar). It was already so before M1, and the checker passes; worth quoting the description some day.

### M1 revision 1

Fixes from review.md → M1 round 1. Handoff shapes and result lines unchanged.

- **F1 `rules`**: restored the `## Calendar conventions` heading after Handoffs, so the four "rules → Calendar conventions" pointers resolve again. Github member's slug is now "the slug its handoff lines carry (usually `pm`)".
- **F2 `log`, F3 `todo`**: the failure lists now point to rules → Handoffs → Check, then write; `todo` keeps its archived and closed cases in its own steps.
- **F4 `add-interest`**: step 1 keeps only its own checks (interest `products`, folder-name rule via rules → Layout, a non-github Role) and points to rules for the shared ones. Step 7 propagates whenever step 3 or 4 changed anything, a description-only update included (supersedes the M1 decision "no status propagation"), matching the spec's unconditional "5. propagate". manual-test.md → M1 step 2 checks the status rebuild.

Checker: **✔ Valid for Claude and OpenAI**.

To do in M3: amend the spec's Conventions "Since" from `<date> by pm` to the template form `2026-11-05, pm` (AGENTS.md: spec follows behaviour).
