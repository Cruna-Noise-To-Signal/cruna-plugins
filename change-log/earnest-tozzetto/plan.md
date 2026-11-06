# IMPLEMENTATION PLAN — Agora shared services mode (earnest-tozzetto)

Agora gets two modes:
- **basic shared account mode**: today's setup, and the default;
- **shared services mode**: a shared drive, a shared calendar and a Google Group, with one account per member.

Every skill follows one rule: always use the three shared ids. Everything to build is in [spec.md](spec.md), and the decision trail is in [grilled-idea.md](grilled-idea.md). The companion request cruna-development-plugins#29 reads the `drive_id` and `root_id` this request writes.

## How to use this plan [do not change]

- follow the repo's `guidelines` skill if it has one, otherwise the conventions in its `AGENTS.md` / `CLAUDE.md`, for the code implementation
- **Thin slices, small commits**, one logical step each, on the `feature/<name-id>`
  branch off the integration branch; prefix each commit `<name-id> F<n>:` (feature) or `<name-id> M<n>:` (milestone).

### Impact at a glance (verified against the code)

All the changes are skill prose under `cruna-plugin-eng/agora/skills/`, mirrored in `cruna-plugin-ita/agora/skills/`. There's no new skill. "Assistant account" appears today in:
- `rules`: SKILL.md, `references/prerequisites.md`, `references/templates.md`, `references/email.md`;
- `setup`: SKILL.md and `references/migration.md`;
- `schedule`, `email`, `review`, `file-document`: SKILL.md;
- `help`: SKILL.md and `references/faq.md`.

`setup` also gets a new reference for the move branch, and `setup/references/email.md` and `routines.md` are touched lightly. Every basic-mode sentence stays valid: the new text adds defaults and per-mode variants and doesn't replace basic behaviour.

The version bump to 0.7.0 goes in both manifests of both languages and in both catalogs. In `docs/agora/`: the spec, the glossary, and a new manual test.

Write the skills following cruna-core `writing-for-agents`. Change the English first, then the Italian using the glossary.

---

## Feature backlog

Guidelines: hard dependencies are noted; anything else can run in parallel.

---

### F1 - Modes, fields and the three-ids rule (rules)
**Capability:** spec → Modes and `org.md`; The three-ids rule; Replace.
**Acceptance:**
- **Vocabulary** defines both modes, the rule, and that kind changes nothing.
- **The `org.md` template** shows `mode`, `drive_id`, `root_id`, `inbox`, `calendar_owner`, each with its default, the Members **Kind** column, and how Access reads in shared services mode.
- **The `setup.md` template** shows the shared services Member boxes.
- **The `inbox.md` template** has the **Mailbox** column.
- **The root `status.md` template** has the `**Tidied:**` line.
- **Finding the root** uses `root_id` when present.
- **Calendar conventions** always use `calendarId = calendar`.
- **Email** states the `deliveredto:<inbox>` filter and that it's Agora's rule, not a permission boundary.
- **Replace:** a failed trash names its cause for each mode.
- A basic `org.md` without any new field reads exactly as today.
**Deps:** none
**Touchpoints:** `drive_id` and `root_id` are read by cruna-development-plugins#29

---

### F2 - Identity by account
**Capability:** spec → Identity.
**Acceptance:**
- In shared services mode, the speaker is the primary calendar's email matched against Members Email, with no question asked. An account with no match gets a Join offer and nothing is written.
- Basic mode and unattended runs are unchanged.
**Deps:** F1
**Touchpoints:** none

---

### F3 - Prerequisites and manual steps
**Capability:** spec → Prerequisites checklist; Manual steps.
**Acceptance:**
- Items 3, 5 and 7 have shared services variants with their real-call checks.
- Item 4 is marked basic-only, and the new admin item covers:
  - the Workspace account;
  - the shared drive;
  - the group;
  - the calendar;
  - connector enablement.
- Item 3 in basic mode confirms the connected account owns `org.md` and blocks with the fix on a mismatch.
- Offboarding has the shared services steps, with the calendar owner's transfer first.
- Every manual step has a menu path, a role or setting, who does it, and its check, as listed in the spec. Paths are checked against the current Google, Claude and ChatGPT UIs.
- Basic-mode text is unchanged apart from the ownership check.
**Deps:** F1
**Touchpoints:** Google Admin console, Drive, Groups, Calendar; Claude and ChatGPT connector settings

---

### F4 - Setup: New root and Verify in shared services mode
**Capability:** spec → Setup skill (branch choice, New root, Verify).
**Acceptance:**
- New root asks about Workspace. Answering no leads to today's flow, word for word.
- Answering yes:
  - extracts the drive id from the URL and checks it with create-then-trash;
  - creates the root in the shared drive;
  - records `mode`, `drive_id`, `root_id`, `calendar`, `calendar_owner` and `inbox`;
  - shares nothing.
- Verify runs the checks for the root's mode, including the basic ownership check.
**Deps:** F1, F3
**Touchpoints:** none

---

### F5 - Setup: Join and Offboard per mode
**Capability:** spec → Setup skill (Join, Offboard).
**Acceptance:**
- Shared services Join:
  - runs the shared services checks;
  - names each missing admin step and who does it;
  - writes Members (with Kind), Access, the memories file, the `setup.md` Member section, the email member part and routines;
  - shares nothing and creates no interest folder.
- Offboard in shared services mode is blocked while the member is `calendar_owner`, until the transfer is done and `calendar_owner` is updated.
- Basic Join, Migrate and Offboard are unchanged.
**Deps:** F2, F3
**Touchpoints:** none

---

### F6 - Setup: Move to shared drive
**Capability:** spec → Setup skill (Move to shared drive).
**Acceptance:**
- The branch is offered for basic roots and tracked in a `## Move to shared drive` section of `setup.md`, so it resumes where it stopped.
- The root is moved, or rebuilt and copied with a count check before the originals are trashed.
- Future `[interest/instance]` events are copied to the new calendar, and the old calendar is left alone.
- The forwarding steps are given, and the old address is trusted as member mail.
- The ids and `mode` are updated, and the old account is retired.
- The branch ends by listing the `products/` instances whose repos must rerun pm-setup.
**Deps:** F4
**Touchpoints:** the repos linked through #29 (`.agora/pm.yml`)

---

### F7 - Email across mailboxes
**Capability:** spec → Email.
**Acceptance:**
- Every search uses `deliveredto:<inbox>`.
- In shared services mode, mail is sent from the member's address with `inbox` in CC and in the member's own voice.
- The ledger dedupes by Gmail id within a mailbox, or by sender + minute + subject across mailboxes, and writes the Mailbox column.
- Rows and diary `email:` ids from another mailbox are reopened by search.
- Old rows without Mailbox read as the assistant inbox.
- Basic-mode email behaviour is unchanged.
**Deps:** F1, F2
**Touchpoints:** the Gmail connectors (Claude, ChatGPT)

---

### F8 - Calendar writes and free time
**Capability:** spec → Calendar.
**Acceptance:**
- `schedule`, `ask` and `review` read and write Agora events only on `calendar` by id.
- In shared services mode, free time comes from the speaker's own calendar plus the others' free/busy, and the Personal calendar column defaults to `domain free/busy`.
- In basic mode the result is the same as today: `calendar` is already the primary calendar's id.
**Deps:** F1
**Touchpoints:** the Google Calendar connectors

---

### F9 - Review: the tidy-up skip, overdue flags and the ownership check
**Capability:** spec → Review and routines.
**Acceptance:**
- The tidy-up writes the `**Tidied:**` line.
- A run within 60 minutes of it skips steps 2.1–2.6 and still writes or updates the review and sends the message. A root without the line tidies as today.
- Missing previous-period reviews and stale triage are flagged with the routines fix.
- In basic mode the tidy-up flags an account that doesn't own `org.md`.
- Routine prompts are unchanged.
**Deps:** F1
**Touchpoints:** none

---

### F10 - Help and FAQ for both modes
**Capability:** spec → Help.
**Acceptance:**
- The tour and FAQ describe both modes. "Where are my files" answers for each mode.
- Troubleshooting maps each shared services failure to its prerequisite item.
- Nothing claims that every connector signs in as the assistant account.
**Deps:** F3
**Touchpoints:** none

---

### F11 - Italian version
**Capability:** spec → Modes and `org.md` (glossary terms); every feature above.
**Acceptance:**
- `cruna-plugin-ita/agora` carries F1–F10.
- The glossary has the mode terms and every new identifier, all kept in English.
- No new English wording is left untranslated in the Italian skills.
**Deps:** F1–F10 (it can follow each feature as it lands)
**Touchpoints:** `docs/agora/glossary-it.md`

---

### F12 - Spec, manual test, version and checker
**Capability:** spec → Spec and docs; Versions; Testing Decisions.
**Acceptance:**
- `docs/agora/agora-spec.md` is updated as the spec lists, including:
  - the verified platform facts (ChatGPT Gmail `deliveredto:`/`list:`, writes to the shared calendar by id);
  - the release checks, with the founder's basic-mode non-regression test.
- `docs/agora/manual-tests/manual-test-agora-v070-shared-services.md` covers every scenario in the spec's Testing Decisions.
- Version 0.7.0 is set in both manifests and both languages, and in both catalogs.
- The cruna-core checker passes.
**Deps:** F1–F11
**Touchpoints:** `.claude-plugin/marketplace.json`, `.agents/plugins/marketplace.json`

---

## Milestones (each demoable)

- **M1 - Shared services root:**
  - Features: F1, F2, F3, F4, F8.
  - Done when a new shared services root on `cruna-shared-resources` is built through setup, Verify passes, the speaker is recognised by account, and an event scheduled from that account lands on the shared calendar.
- **M2 - A team on it:**
  - Features: F5, F7, F9.
  - Done when:
    - a second account joins with no password;
    - Replace works across members;
    - group mail handled by one member's AI shows as handled from the other's;
    - a second routine within 60 minutes skips the tidy-up;
    - offboarding the calendar owner is blocked until the transfer.
- **M3 - Moving and documenting:**
  - Features: F6, F10, F11, F12.
  - Done when:
    - a copy of a basic root moves to shared services mode with its files and future events;
    - help answers for both modes in English and Italian;
    - the manual test is written and the checker passes;
    - the founder's basic-mode test is ready to run.

## Sequencing

```
F1 ─┬─ F2 ─┬─ F5
    │      └─ F7
    ├─ F3 ─┬─ F4 ── F6
    │      ├─ F5
    │      └─ F10
    ├─ F8
    └─ F9
F1–F10 ── F11 ── F12
```

## Risks / watch-items

- **Basic-mode regressions:** every edit to a shared file must leave basic-mode sentences valid. Check each diff against "would a 0.6.0 root still read this the same?" The founder's manual test is the gate.
- **Mailbox privacy:** the `deliveredto:<inbox>` filter is only as good as each skill's discipline. Grep every Gmail search in both languages for the filter before review.
- **Moving the root:** the Drive connector may refuse to re-parent into a shared drive. The rebuild-and-copy fallback must never trash an original before the count check passes.
- **Calendar ownership:** a secondary calendar is deleted with its owner's account. Offboarding must block, not warn.
- **Who's speaking from the calendar list:** relies on the primary calendar's id being the account's email on both platforms. Verify this on ChatGPT during M1.
- **Duplicate key collisions:** two group mails with the same sender, minute and subject merge into one row. This is accepted, but say so in the email rules.
- **Click paths drift:** Google Admin, Groups and the clients' connector settings change often. Keep the "menu names change" note, and check the paths while implementing.
