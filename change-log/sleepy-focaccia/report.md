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

## M2

Agora knows `pm`: F5–F6 in the English plugin only, plus the three non-blocking M1 review nits. No version bump, no Italian, no spec/docs changes (M3).

### What changed

- **M1 nits**: this report's "no status propagation" decision struck through as superseded (F4); `todo` handoff now reads "Every failure, the archived and closed cases above included, writes nothing (rules → Handoffs → Check, then write)" (F3); `add-interest` handoff loses the redundant "Skip step 7 below" (F4).
- **F5 `review`**: step 1, a github member gets no personal message (a prompt or request naming one gets the general message); step 2, the bridge file stays out of every tidy-up check (never flagged, filed, trashed or counted); step 4, a github member's section in the general message holds only its open to-dos, in list order, four lines at most plus a count. The review file's To-dos section already shows each member's to-dos in Members order, so `pm` appears there with no change.
- **F5 `setup`**: one **Github members** paragraph after Checklist: they come only from the `add-interest` handoff; setup's questions and messages stay on the people and interests the user names; "every/each member" in setup means people (no Member section, Access row, memories file, sharing or routines; Join and Verify pass it by). Offboard gains a github-member path: remove its Members row and every Conventions row managed by it, one `org.md` replace; diary and to-dos stay; tell the owner the repo stays linked until a developer unlinks it with `cruna-pm-github`.
- **F5 `rules` references**: `prerequisites.md` item 5 check reads "each member with an email" (pm has none); `templates.md` setup.md section names github members among those with no Member section.
- **F5 `ask`**: Cite adds a bridge entry's `github:` refs.
- **F6 `help`**: description gains "products and pm from GitHub"; Tour adds one products line only when Members has a github member; FAQ entry "What is `products` and who is pm?" (what they are, `cruna-pm-github` links repos from Claude Code, notes and to-dos for `pm` become GitHub requests at the next sync, "→ #8", ticked off when the issue closes).

Checker: **✔ Valid for Claude and OpenAI**.

### Github member leak sweep

Grepped `cruna-plugin-eng/agora` for every/each/per member and Members lists:
- **Identity**: rules → Identity (M1). **Owner default in `todo`**: `references/todos.md` (M1).
- **Setup checklist, Access, memories, sharing, routines, Verify**: the setup paragraph above, plus the template and prerequisites edits.
- **Reviews**: review steps 1 and 4.
- **Left as is**: `schedule` "personal calendars of all members" (pm has none to read), email criteria per member (pm has none), `todo.md` sections per owner (intended: pm gets one).

### Testing

Skill prose only: no `tdd` cycle possible. Hand test: [manual-test.md](manual-test.md) → M2.

### Decisions taken where the spec was silent

- **`Run Agora review: … for pm`** gives the general message instead of failing or stopping.
- **pm's review section** is capped at four lines plus a count, matching the existing "up to four lines per member section" bound.
- **Interview "never mentions GitHub"**: the interview had no GitHub mention; the setup paragraph keeps its questions and messages on what the user names rather than adding a prohibition.
- **Offboard of a github member** warns that the next link re-creates it, since `add-interest` creates a missing member.
- **Help Tour** mentions products only when the root has a github member (story 19/20: roots without linked repos see nothing new).

## M3

Release-ready: the M2 review nits, F7 (Italian) and F8 (spec, future extensions, version 0.6.0, manual test).

### What changed

- **M2 nits (F5)**: `setup` points to rules → Vocabulary for what a github member lacks, keeping the "every member means people" scoping; `review` step 2 points to rules → Layout for the bridge file; the general message drops "all of them" from the github member's section.
- **F7 Italian** (`cruna-plugin-ita/agora/`): every English file changed since 4f9e85f has its Italian counterpart (13 files, checked by diffing the two file lists): `rules` (Vocabolario, Struttura with the bridge file, Identità, new `## Handoff` section), `references/templates.md`, `todos.md`, `prerequisites.md`; Handoff sections in `log`, `todo`, `add-interest`; `review`, `setup`, `ask`, `help` and the FAQ. Handoff lines, result lines (`agora: ok — …`, `agora: failed — …` and their English reasons), Role `github`, the folder `products`, the key `github:`, the marker `managed by cruna-pm-github` and structure names stay English. The `products` description, the Members Notes, the Conventions text after the marker and the `log` reply ("annotato; diventerà una richiesta GitHub alla prossima sincronizzazione") are Italian.
- **Glossary** `docs/agora/glossary-it.md`: a "What stays in English" bullet for GitHub-bridge identifiers; terms handoff (kept as `handoff`), github member (membro github), managed instance (istanza gestita), bridge / bridge file (ponte / file del ponte), last-check, link a repo, sync (sincronizzazione), GitHub request, ref (riferimento), products from GitHub, the fixed Italian strings; `rules` headings `Handoff` and `Handoff → Controlla, poi scrivi`.
- **F8 spec** `docs/agora/agora-spec.md`: status line names 0.6.0; a "Products from GitHub" section (github member, `products/`, managed instance with Since `<date>, <slug>`, bridge entries and `github:`, bridge file, the four handoff lines and their rules, removal path); a second-exception bullet under Who changes to-dos; a "Verified platform facts (GitHub connectors, 2026-11-04 founder check)" section; a 0.6.0 Release checks line; Out of scope updated.
- **Request spec** `change-log/sleepy-focaccia/spec.md`: Conventions Since amended to `<date>, pm` (the template format the implementation uses).
- **`future-extensions.md`**: "Git-backed interests" removed; added Documents for products, GitHub comments back to notes, Products in another org's Project, Product events outside an agent session.
- **Release**: 0.6.0 in all four manifests and the Claude catalog (the OpenAI catalog `.agents/plugins/marketplace.json` carries no version field, so nothing to bump there); root README versions and one products sentence in both plugin READMEs.
- **Manual test** `docs/agora/manual-tests/manual-test-agora-v060-products.md`: 9 scenarios covering each handoff's success, idempotency, four failure cases, the managed-instance reply, Identity without pm, pm in to-dos and the general review, the last-check file, `ask`/`help`, and an Italian root.

Checker (`cruna-core` `check_plugins.py`): **✔ Valid for Claude and OpenAI**. `claude plugin validate` passes for the repo and both plugins (only the pre-existing missing-icon warning).

### Testing

Skill prose and docs only: no `tdd` cycle possible; the checker is the automated test. Hand test: [manual-test.md](manual-test.md) → M3, which runs the docs manual test.

### Decisions and doubts

- **Connector fact platforms**: the issue #3 comment (2026-11-04) says "the Chat Apps" without naming them; the grilling record (grilled-idea.md → Facts recorded) names "Claude and ChatGPT (the founder's check)". The spec names both, citing that source, so no ChatGPT release check was added. If the founder didn't actually check ChatGPT, move it to Release checks.
- **"handoff" untranslated** in Italian: it's agent-facing and names the English lines; "passaggio di consegne" would read as prose, not a contract.
- **`products` folder stays English in Italian roots**: the handoff line fixes `products/<instance>`, so translating it would break the contract. Stated in the Italian `add-interest` and the glossary.
- **Italian Conventions row**: marker `managed by cruna-pm-github` English (skills match on it), text after `: ` in Italian, like other Conventions content.
