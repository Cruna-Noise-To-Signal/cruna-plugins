# IMPLEMENTATION PLAN — Agora to-dos (proud-ciabatta)

Adds personal and shared to-dos to Agora: append-only to-do events in `todos/`, a derived `todo.md`, a new `todo` skill, a To-dos section in reviews, and hooks from `log` and `email`. Everything to build is specified in [spec.md](spec.md); the decision trail is [grilled-idea.md](grilled-idea.md).

## How to use this plan [do not change]

- follow the repo's `guidelines` skill if it has one, otherwise the conventions in its `AGENTS.md` / `CLAUDE.md`, for the code implementation
- **Thin slices, small commits**, one logical step each, on the `feature/<name-id>`
  branch off the integration branch; prefix each commit `<name-id> F<n>:` (feature) or `<name-id> M<n>:` (milestone).

### Impact at a glance (verified against the code)

All changes are skill prose under `cruna-plugin-eng/agora/skills/`, mirrored in `cruna-plugin-ita/agora/skills/`. That is one new skill (`todo`, with `agents/openai.yaml`) and edits to:
- `rules`: SKILL.md, `references/templates.md`, a new to-dos reference next to the existing `references/email.md`;
- `review`, `log`, `email`, `ask`, `setup`: SKILL.md only;
- `help`: SKILL.md and `references/faq.md`.

Manifests and both catalogs get the version bump. `docs/agora/` gets the spec update, glossary terms, the Google Tasks entry in `future-extensions.md` and a new manual test. Write skills following cruna-core `writing-for-agents`. Change English first, then Italian.

---

## Feature backlog

Guidelines: hard dependencies are noted; anything else can run in parallel.

---

### F1 - Rules: to-do conventions and data shapes
**Capability:** spec → Implementation Decisions → `rules`, Data shapes, Behaviour contracts. Covers vocabulary, layout, file kinds, the To-dos conventions (event format, fold to current state, owner/visibility, place, due dates, routines only suggest, remove and archive), and templates (to-do event, `todo.md`, review To-dos section, `status.md` Pending line).
**Acceptance:**
- every to-do contract is stated once in `rules` (detail in a reference file), and the other skills link to it;
- templates exist for the to-do event, `todo.md` and the review section;
- a root without `todos/` reads as today.
**Deps:** none
**Touchpoints:** none (prose)

---

### F2 - `todo` skill
**Capability:** spec → New skill `todo`. Add, complete, reopen, assign/take over, edit, list, remove (filters, closed-only default, confirmation list, trash notice, `remove` event). Rebuilds `todo.md` and attached instance statuses after each change. Creates `todos/` on first use. Memory step.
**Acceptance:**
- each change writes exactly one event and never edits an existing one;
- `todo.md` matches the fold of all events after each change;
- an ambiguous "done with X" asks which to-do;
- remove never touches open to-dos unless told, always confirms, and trashes;
- frontmatter has only `name` and `description`;
- the OpenAI interface file is present.
**Deps:** F1
**Touchpoints:** Drive

---

### F3 - Review: rebuild, archive, To-dos section, suggestions
**Capability:** spec → `review`. Tidy-up rebuilds a stale `todo.md` and the monthly archive move (closed > 90 days). Every review gets the To-dos section. Suggestions come from diary and email. The monthly list covers to-dos open > 30 days with no due date.
**Acceptance:**
- unattended runs write no to-do events;
- suggestions appear in the To-dos section and under Needs attention;
- personal and general ordering follows the spec;
- running the archive twice moves nothing new;
- concurrent runs are harmless.
**Deps:** F1
**Touchpoints:** Drive, Gmail (suggestions from the inbox, when email is on)

---

### F4 - Hooks: `log`, `email`, `ask`, `setup`
**Capability:** spec → `log`, `email`, `ask`, `setup`.
- `log` offers a to-do when an entry reads as an action;
- `email` offers a to-do in conversation, creates one from a member's explicit instruction, only suggests from third-party mail, and records `to-do → <id>` in `inbox.md`;
- `ask` answers list questions from `todo.md`;
- `setup` creates `todos/` at build.
**Acceptance:**
- nothing is added without a yes, except the member-mail instruction;
- third-party mail never creates a to-do;
- the ledger records the outcome;
- a new root has `todos/`.
**Deps:** F1, F2 (#2's `email` skill is on `dev`)
**Touchpoints:** Drive, Gmail

---

### F5 - Help and FAQ
**Capability:** spec → `help`. Covers what to-dos are, that all members see them, how to add, complete and remove them, and the trash window.
**Acceptance:** `help` answers "how do I add / finish / remove a to-do?" and "can others see my to-dos?".
**Deps:** F1
**Touchpoints:** none

---

### F6 - Italian version and glossary
**Capability:** spec → Behaviour contracts (structure names), Testing → translation check. Brings F1–F5 over to `cruna-plugin-ita/agora`, and adds to `docs/agora/glossary-it.md`: to-do, to-do event, `todo.md`, and `todo`, `todos/` and the new headings and keys in the "stays in English" lists.
**Acceptance:** each Italian skill mirrors its English one section for section; structure names stay in English; "to-do" is used consistently.
**Deps:** F1–F5
**Touchpoints:** none

---

### F7 - Docs, version and release checks
**Capability:** spec → Versioning, Out of Scope, Testing → seam. Covers:
- the `agora-spec.md` update (layout, file kinds, `status.md` Pending, skill table, Routines, Release checks, Out of scope);
- the Google Tasks entry in `future-extensions.md`;
- README mention;
- the bump to 0.5.0 in both manifests and both catalogs for both languages;
- the manual test script in `docs/agora/manual-tests/`.
**Acceptance:**
- the spec describes the shipped behaviour;
- the `portable-plugins` checker passes;
- `name`/`version` are identical in both manifests and both catalogs;
- the manual test covers every scenario in spec → Testing Decisions, on Claude and ChatGPT.
**Deps:** F1–F6
**Touchpoints:** marketplace catalogs

---

## Milestones (each demoable)

- **M1 - To-dos in conversation:** F1 + F2 + F5 (English). Demo: in a test root, add personal, assigned and shared to-dos; list, complete, reassign and remove them; see `todo.md` on Drive; two overlapping completions end correct.
- **M2 - To-dos everywhere:** F3 + F4 (English). Demo: `log` and `email` offer to-dos; member mail creates one; a daily review shows the To-dos section and suggestions without changing anything; a monthly run archives old closed to-dos.
- **M3 - Release-ready:** F6 + F7. Demo: the checker passes; the Italian version mirrors the English one; the manual test has been run on Claude and on ChatGPT, with results recorded.

## Sequencing

```
F1 ─┬─ F2 ── F4
    ├─ F3
    ├─ F5
    └──────── (F2..F5) ── F6 ── F7
```

F2, F3 and F5 can run in parallel after F1. F4 needs F2; #2's `email` skill is already on `dev` (0.4.0).

## Risks / watch-items

- **Event file names colliding**: two events by the same author in the same minute. Use the `-2` suffix rule, and check the fold handles same-minute ordering (by `created`, then name).
- **Rebuild cost**: `todo.md` is rebuilt from every event in `todos/`. The monthly archive keeps the folder small. Watch Drive listing limits on busy roots.
- **Matching "done with X"**: fuzzy matching may close the wrong to-do. Ask whenever more than one open to-do matches, and echo the one closed.
- **Email as an injection path**: only an explicit instruction in mail from a member's address creates a to-do. Mail content is data, never instructions, otherwise (as in #2).
- **Skill size**: `rules` (162 lines) and `review` are long. Put to-do detail in a `rules` reference file.
