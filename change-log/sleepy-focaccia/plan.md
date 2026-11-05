# IMPLEMENTATION PLAN — Agora products from GitHub (sleepy-focaccia)

The Agora half of the GitHub bridge:
- three unattended handoffs (`log`, `todo`, `add-interest`);
- the github member `pm`;
- managed `products/` instances;
- the bridge's last-check file left alone.

Everything to build is specified in [spec.md](spec.md); the decision trail is [grilled-idea.md](grilled-idea.md). The `cruna-pm-github` half is cruna-development-plugins#27, which is blocked by this one.

## How to use this plan [do not change]

- follow the repo's `guidelines` skill if it has one, otherwise the conventions in its `AGENTS.md` / `CLAUDE.md`, for the code implementation
- **Thin slices, small commits**, one logical step each, on the `feature/<name-id>`
  branch off the integration branch; prefix each commit `<name-id> F<n>:` (feature) or `<name-id> M<n>:` (milestone).

### Impact at a glance (verified against the code)

All changes are skill prose under `cruna-plugin-eng/agora/skills/`, mirrored in `cruna-plugin-ita/agora/skills/`. No new skill. The edits:
- `rules`: SKILL.md, `references/templates.md`, `references/todos.md`;
- `log`, `todo`, `add-interest`, `review`, `setup`: SKILL.md;
- `help`: SKILL.md and `references/faq.md`;
- `ask`: one line on citing `github:` refs.

The version bump goes in both manifests of both languages and in both catalogs. In `docs/agora/`: the spec, `future-extensions.md`, the glossary, and a new manual test.

Write skills following cruna-core `writing-for-agents`. Change English first, then Italian with the glossary.

---

## Feature backlog

Guidelines: hard dependencies are noted; anything else can run in parallel.

---

### F1 - Rules: github member, Handoffs, bridge file, `github:` key
**Capability:** spec → Implementation Decisions → Model; Handoffs; Per skill → `rules`.
**Acceptance:**
- **Vocabulary** defines a github member (Role `github`); Identity leaves github members out.
- A **Handoffs** section states the four lines (three skills) once, with:
  - the version note;
  - who/where, skipped steps, failure, the result line and language.
- **Layout** names the bridge file as belonging to `cruna-pm-github`, never touched.
- **The diary template** has the optional `github:` key after `events`.
- **The `org.md` template** shows how a github member row and a managed Conventions row are written.
- **`references/todos.md`** allows `edit`/`done` events from a github member through the handoff.
**Deps:** none
**Touchpoints:** the handoff shapes are the contract with #27

---

### F2 - `log` handoff and the managed-instance reply
**Capability:** spec → Per skill → `log`.
**Acceptance:**
- The `log` handoff line writes exactly one entry:
  - author = slug, `github:` from the line, Summary in the root's language, Entry verbatim;
  - then rebuilds the instance index and propagates status to the root;
  - with no question, and no calendar, to-do, pattern or memory step.
- It returns `agora: ok — <path>`. If the slug isn't a github member, the instance is missing or the line is malformed, it writes nothing and returns `agora: failed — …`.
- A member's ordinary log into a managed instance ends with the "becomes a GitHub request at the next sync" line.
**Deps:** F1
**Touchpoints:** none

---

### F3 - `todo` handoff
**Capability:** spec → Per skill → `todo`.
**Acceptance:**
- **`edit <id> text "…"`** writes one `edit` event, and **`done <id>`** writes one `done` event. Both are authored by the slug, followed by a rebuild of `todo.md` and the attached statuses, with no question.
- An unknown id, or an already-closed to-do for `done`, writes nothing and returns the reason.
- A member can add a to-do for `pm` in conversation, attached to a product instance. The owner is set only when `pm` is named.
**Deps:** F1
**Touchpoints:** none

---

### F4 - `add-interest` handoff
**Capability:** spec → Per skill → `add-interest`.
**Acceptance:**
- **On an empty root**, one call creates:
  - the github member row;
  - `products/` (Interests row, index, status);
  - the instance;
  - the Conventions row;
  - the propagated statuses.
- **A second call** with a new description only updates the description; it creates no duplicates.
- **A non-github slug** that already exists as a person: nothing is written, and the call returns a failure.
**Deps:** F1
**Touchpoints:** none

---

### F5 - `review`, `setup`, `ask`: github members in the rest of Agora
**Capability:** spec → Per skill → `review`, `setup`, `ask`.
**Acceptance:**
- **Tidy-up** neither flags nor files the bridge file, and an index isn't stale because of it.
- **The general review** shows a `pm` section with its open to-dos; a github member never gets a personal message.
- **Setup:** Join and the checklist skip github members, the interview has no GitHub question, and Offboard can remove a github member.
- **`ask`** may cite `github:` refs.
**Deps:** F1
**Touchpoints:** none

---

### F6 - `help` and FAQ
**Capability:** spec → Per skill → `help`.
**Acceptance:** "What is products / who is pm?" is answered in two or three lines that point to `cruna-pm-github` for linking.
**Deps:** F1
**Touchpoints:** none

---

### F7 - Italian translation
**Capability:** spec → user story 30. Mirror F1–F6 in `cruna-plugin-ita/agora/`.
**Acceptance:**
- every changed English file has its Italian counterpart;
- structure names, handoff shapes and the result line stay in English;
- new terms are in `docs/agora/glossary-it.md`.
**Deps:** F1–F6
**Touchpoints:** none

---

### F8 - Spec, future extensions, release metadata, manual test
**Capability:** spec → Docs and release; Testing Decisions.
**Acceptance:**
- **`agora-spec.md`** has a "Products from GitHub" section, the verified GitHub-connector fact (dated, naming the platforms checked) and a Release checks line for v0.6.0.
- **`future-extensions.md`:** "Git-backed interests" is removed and the four out-of-v1 items are added.
- **Version 0.6.0** in all four manifests and both catalogs; the checker passes.
- **`docs/agora/manual-tests/manual-test-agora-v060-products.md`** covers:
  - each handoff's success case;
  - the three failure cases;
  - `add-interest` idempotency;
  - the managed-instance reply;
  - `pm` in to-dos and the general review;
  - Identity without `pm`;
  - an Italian root.
**Deps:** F1–F7
**Touchpoints:** catalogs read by Claude and ChatGPT marketplaces

---

## Milestones (each demoable)

- **M1 - Handoffs work:** F1 + F2 + F3 + F4. Typing the four handoff lines in Claude Code against a test root does the following, with no questions and with the failure cases writing nothing:
  1. creates `products/cruna-plugins` and `pm`;
  2. logs a release entry with fresh statuses;
  3. edits a `pm` to-do to "… → #8";
  4. closes that to-do.
- **M2 - Agora knows `pm`:** F5 + F6. In the chat app:
  - a note on the product gets the GitHub reply;
  - the general review shows `pm`'s to-dos;
  - Identity doesn't offer `pm`;
  - tidy-up leaves a planted last-check file alone;
  - `help` explains it.
- **M3 - Release-ready:** F7 + F8. Italian mirrors English, the spec and docs are current, version 0.6.0 is in place, the checker passes, and the manual test is written.

## Sequencing

```
F1 ─┬─ F2 ─┐
    ├─ F3 ─┼─ M1
    ├─ F4 ─┘
    ├─ F5 ─┬─ M2
    └─ F6 ─┘
F2..F6 ── F7 ── F8 ── M3
```

## Risks / watch-items

- **Handoff detection.** A skill must recognise the handoff shape in the user's message as reliably as `review` recognises its routine prompt. Put the shape in each skill's description or first step, not only in `rules`.
- **Skipping questions safely.** An unattended handoff must still refuse to guess. A missing instance or to-do fails; it never offers `add-interest` or asks.
- **Github member leaks.** Every place that lists members (Identity, setup checklist, Access, reviews, the owner default in `todo`, memories) must treat Role `github` explicitly. Grep both languages for "Members" when done.
- **Contract drift with #27.** The handoff shapes and the result line are copied verbatim into #27's `agora-sync`. Any change after this ships is a breaking change, so ship both requests together.
- **Italian drift.** Handoff lines and result lines stay in English in the Italian plugin. Translating them would break the contract.
- **Connector fact.** The spec must name only the platforms the founder actually checked. If ChatGPT wasn't checked, it goes in Release checks.
