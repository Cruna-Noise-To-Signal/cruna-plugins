# IMPLEMENTATION PLAN — Agora email (eager-rosetta)

Adds optional email on the Gmail connector to Agora: a new `email` skill, the `inbox.md` ledger, an Email section in reviews, and importance criteria that are elicited. Everything to build is specified in [spec.md](spec.md); the decision trail is [grilled-idea.md](grilled-idea.md).

## How to use this plan [do not change]

- follow the repo's `guidelines` skill if it has one, otherwise the conventions in its `AGENTS.md` / `CLAUDE.md`, for the code implementation
- **Thin slices, small commits**, one logical step each, on the `feature/<name-id>`
  branch off the integration branch; prefix each commit `<name-id> F<n>:` (feature) or `<name-id> M<n>:` (milestone).

### Impact at a glance (verified against the code)

All changes are skill prose under `cruna-plugin-eng/agora/skills/`, mirrored in `cruna-plugin-ita/agora/skills/`. That is one new skill (`email`, with `agents/openai.yaml`) and edits to `rules` (SKILL.md, `references/templates.md`, `references/prerequisites.md`), `setup` (SKILL.md, `references/interview.md`, `references/migration.md`), `review`, `file-document`, `remember`, `ask` and `help` (SKILL.md, `references/faq.md`). Manifests and both catalogs get the version bump. `docs/agora/` gets the spec update, glossary terms, the future-extensions entries and a new manual test. Write skills following cruna-core `writing-for-agents`. Change English first, then Italian.

---

## Feature backlog

Guidelines: hard dependencies are noted; anything else can run in parallel.

---

### F1 - Rules: email conventions and data shapes
**Capability:** spec → Implementation Decisions → `rules`, `rules` templates, Data shapes. Covers vocabulary, connectors, `inbox.md` as a curated file, the Email section (sending policy, voice, filing, ledger, importance, member mail, criteria location, invitation state) and the template changes (`inbox.md`, `org.md` email field and Emails column, `setup.md` email boxes, the Review Email section, diary email reference and to-file note).
**Acceptance:**
- every behaviour contract in the spec is stated once in `rules`, and the other skills link to it rather than restating it;
- templates exist for `inbox.md` and every changed file;
- a root with `email` absent or `off` reads as today.
**Deps:** none
**Touchpoints:** none (prose)

---

### F2 - Prerequisites: optional Gmail item
**Capability:** spec → `rules` prerequisites. Adds the optional Gmail item with a real-call check, per-platform fixes and platform limits, and updates item 3's wording.
**Acceptance:** the item checks the connected account is the assistant account with a search call; it lists the ChatGPT web/paid/Business-admin limits and the Claude Team/Enterprise Owner limit; a root with email off skips it.
**Deps:** F1
**Touchpoints:** Gmail connector (check call)

---

### F3 - `email` skill
**Capability:** spec → New skill `email`. Search, read, draft, reply, forward, send on confirmation, file an email (with the attachments to-file note), record in `inbox.md`, elicit criteria in conversation, memory step.
**Acceptance:**
- no send without confirmation of the exact message in the current conversation;
- an emailed instruction from a member produces a draft only;
- filing writes a diary entry (author = the forwarding member) and propagates;
- every handled email has a ledger row;
- an ambiguous email is flagged;
- criteria are asked when the state is *not asked* or *invited*, and "no preference" is recorded;
- in a solo root it files only on request;
- frontmatter has only `name` and `description`;
- the OpenAI interface file is present.
**Deps:** F1
**Touchpoints:** Gmail connector, Drive

---

### F4 - Review: inbox tidy-up, Email section, invitations
**Capability:** spec → `review`. Adds the inbox step in tidy-up (ledger dedupe, file/flag/draft-under-convention/ignore, no auto-filing in solo roots, a 60-day prune, linking attachments), the Email section in the shared review, the invitation line in the chat message only, the monthly criteria line, and elicitation in attended runs.
**Acceptance:**
- two runs on the same day handle each email once;
- the Email section lists needing attention, drafts waiting, sent and filed;
- important email also appears under Needs attention;
- the invitation line appears only in the message, and moves the state *not asked* → *invited*;
- the monthly line follows the state (including the quieter *no preference* line);
- unattended runs never send;
- they draft only under a convention.
**Deps:** F1, F3 (shared filing behaviour)
**Touchpoints:** Gmail connector, Drive

---

### F5 - Setup: email step, addresses, criteria, Join, Migrate, Verify
**Capability:** spec → `setup`. Adds the email question in the interview, member addresses, root and member criteria questions, `setup.md` email boxes, Join, turning email on in an existing root, the migration note, and a Verify line.
**Acceptance:**
- declining email leaves setup as today;
- accepting records `email: on`, addresses and criteria (or *no preference*) and ticks the boxes;
- "turn on email" in an existing root runs only the email step;
- Verify confirms Gmail as the assistant account;
- the migration step states what the AI sees afterwards.
**Deps:** F1, F2
**Touchpoints:** Gmail connector, Drive

---

### F6 - Supporting skills: file-document, remember, ask, help
**Capability:** spec → `file-document`, `remember`, `ask`, `help`. Covers linking an attachment to its email entry, the `Email importance:` line, pulling from the inbox, and the help/FAQ content.
**Acceptance:**
- an attachment saved to `landing/` for a to-file note is linked to that entry and its flag cleared;
- `remember` keeps the state line updated in place;
- `ask` answers mail questions from the inbox only when email is on;
- `help` covers what email can do, its limits and troubleshooting.
**Deps:** F1, F3
**Touchpoints:** Drive, Gmail connector

---

### F7 - Italian version and glossary
**Capability:** spec → Implementation Decisions (both versions), Testing → translation check. Brings F1–F6 over to `cruna-plugin-ita/agora`, and adds the new terms (assistant inbox, member mail, email importance, `inbox.md`, the invitation and criteria wording) to `docs/agora/glossary-it.md`.
**Acceptance:** each Italian skill mirrors its English one section for section; structure names stay in English; every new term is in the glossary and used consistently.
**Deps:** F1–F6
**Touchpoints:** none

---

### F8 - Docs, version and release checks
**Capability:** spec → Versioning, Out of Scope, Testing → seam. Covers the `agora-spec.md` update (layout, `org.md`, `setup.md`, accounts, skill table, prerequisites, routines, memories, release checks, out of scope), the `future-extensions.md` entries, README mentions, the 0.4.0 bump in both manifests and both catalogs for both languages, and the manual test script `docs/agora/manual-tests/manual-test-agora-v040-email.md`.
**Acceptance:**
- the spec describes the shipped behaviour;
- the `portable-plugins` checker passes;
- `name`/`version` are identical in both manifests and both catalogs;
- the manual test covers every scenario listed in spec → Testing Decisions, on Claude and ChatGPT.
**Deps:** F1–F7
**Touchpoints:** marketplace catalogs

---

## Milestones (each demoable)

- **M1 - Email in conversation:** F1 + F2 + F3 + F5 (English). Demo: in a test root, turn email on, forward member mail and see it filed, draft a reply and send it on confirmation, and check the `inbox.md` ledger.
- **M2 - Email in reviews:** F4 + F6 (English). Demo: two reviews in a row on the test root with no double filing; an Email section; the invitation line in the chat; a reply to it recorded as criteria; an attachment saved to `landing/` and linked.
- **M3 - Release-ready:** F7 + F8. Demo: the checker passes; the Italian version mirrors the English one; the manual test has been run on Claude and on ChatGPT, with results recorded.

## Sequencing

```
F1 ─┬─ F2 ── F5
    ├─ F3 ─┬─ F4
    │      └─ F6
    └──────────── (F2..F6) ── F7 ── F8
```

F2 and F3 can run in parallel after F1. F4 and F6 can run in parallel after F3.

## Risks / watch-items

- **ChatGPT facts unverified directly**: send limits and approval behaviour come from search snippets. Confirm them in M3's live test and adjust the prerequisites text.
- **Replying to a scheduled review's chat**: unverified on both platforms. If a reply doesn't run with the plugin and connectors loaded, capture relies on the next conversation (already in the design). Record what you observe in the manual test.
- **Per-send approval prompts**: the platform's own approval sits on top of Agora's confirmation. Make sure the skill doesn't ask twice in a confusing way: one Agora confirmation of the exact message, then the platform prompt.
- **Gmail search window**: the inbox step must bound its search (since the oldest ledger row, at most 60 days) to stay within connector limits on busy inboxes.
- **Prompt injection through email**: incoming mail is untrusted content. The `email` skill and the review must treat email bodies as data, never instructions, apart from the member-mail-to-draft rule, which never sends.
- **Skill size**: `rules` and `review` are already long. Prefer putting email detail in a `rules` reference file over growing SKILL.md.
