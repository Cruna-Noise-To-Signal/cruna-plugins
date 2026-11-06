# Review — earnest-tozzetto

## M1 - Shared services root — round 1

**Result: FAIL** (Specs FAIL, Standards PASS). Start commit: `004ce41`.

### Specs

**Verdict: FAIL.** F1, F2, F3, F4 and F8 are almost fully covered. There is one must-fix, which comes from the agreed design in issue #11.

**(a) Missing or partial**

1. **Must fix: the founder's Group settings are missing.** The last comment on #11 (2026-11-06 14:16) says: "Group Labels: mailing / Access Type: restricted / Who Can Join: only invited users … This must be added to the documentation for the manual setup … a reference file in the plugin itself."
   - `rules/references/prerequisites.md` item 4 step 3 has only Who can join and Who can post. It never mentions the **Mailing** label or the **Access type**.
   - Reconcile while adding them. In the Admin console, the "Restricted" preset normally means only members can post. That clashes with spec l.138, "Who can post: Anyone on the web", and with the outside-sender setting. Say which one wins: the access type probably becomes Custom once posting is opened.
   - F3 acceptance asks for this: "Every manual step has a menu path, a role or setting…".

**(b) Scope creep (minor)**

2. Spec l.109 says: "Skills other than `setup`, `help` and the identity step don't branch on `mode`." Yet `ask` ("what do we have") and `review` Personal → Yours now add the member's own calendar in full "(shared services mode)".
   - Spec l.206 asks for that only for free time.
   - The new Vocabulary line also claims "Only `setup` and `help` branch on the mode themselves", which `ask`, `review` and `schedule` now contradict.
   - Fix: drop the `ask`/`review` additions, or name them in rules as a per-mode difference.

**(c) Implemented but looks wrong (minor)**

3. `templates.md`: "A shared services root has no `assistant_account`." This contradicts spec l.181, "`assistant_account` is kept for the record" (Move branch). Reword it, for example as "a new shared services root".
4. Members now has a **Kind** column. An existing basic `org.md` has 8 columns, and `add-interest` writes the github row "as the pm row above", which has 9. Say that a table without Kind keeps its shape, otherwise basic roots could end up with misaligned rows. Spec l.20 says: "every existing root keeps working with no change to its files."

**Open point: `deliveredto:` in basic mode** (not blocking M1; this is F7)

- **What's affected:** sent messages carry no Delivered-To header. So in basic mode a search for mail the assistant sent ("did we answer Rossi?") now misses it. That conflicts with spec l.36 and l.257, "same results as on 0.6.0".
- **What isn't:** received, CC'd, BCC'd and auto-forwarded mail still match. Sent mail stays reachable through the ledger and diary ids.
- **Shared services has the same gap:** Gmail usually doesn't deliver your own group post back to you.
- **Recommended for F7:** add a sent-mail clause to rules → The three ids → Mail: `deliveredto:<inbox> OR from:<inbox>` in basic mode and `from:me cc:<inbox>` in shared services mode. Make "a search for sent mail" an explicit step in the founder's basic non-regression test.

**Checked OK**

- Modes and their defaults; the three-ids rule and its "not a permission boundary" sentence; Replace naming its cause per mode; identity by the primary calendar's id, with an offer to Join.
- Prerequisite items 3, 4, 5 and 7 per mode, including the ownership block; offboarding with the calendar owner's transfer first.
- New root: its order, id extraction and real-call checks, and the ids written to `org.md`; Verify per mode.
- The `setup.md` boxes, the Mailbox column and the Tidied line.
- `calendarId = calendar` in `schedule`, `ask` and `review`.

### Standards

**Verdict: PASS.** I reviewed 8 commits against AGENTS.md, cruna-core `writing-for-agents` and the smell baseline.

**What conforms**

- Commit prefixes follow `<name-id> F<n>:` / `M<n>:`, and the slices are small.
- Frontmatter is untouched, and English was changed first.
- "The three ids" and the **Same answer** rule have one source of truth in `rules/SKILL.md`. Callers point to it with `rules → The three ids`.
- Basic-mode text is kept verbatim under "Basic mode" headings. A missing field reads as basic, which is stated once in `templates.md`.
- Each prohibition comes with the positive instruction.

**Advisory**

1. **Possible Repeated Switches, `setup/SKILL.md`.** Steps 2b, 2c, 2e, 3 and 7 each carry an inline "Shared services mode: …" parenthetical. When F5 adds the full Join, consider moving them into `setup/references/shared-services.md` with one pointer per step.
2. **Duplicated Code plus a contradicted claim.** "or when the connected account is the member's own (shared services mode)" appears in `ask` and `review`, and a variant appears in `schedule` step 4. Meanwhile Vocabulary says only `setup` and `help` branch on mode. Move the rule into `rules` → Calendar conventions and point to it. This is the same issue as Specs (b)2.
3. **Duplication, Kind.** "Kind changes nothing" is stated in Vocabulary, in `templates.md` and in `interview.md`. Keep it in Vocabulary and point to it from the other two.
4. **Basic-mode regression risk:** `deliveredto:` on sent-mail searches. Settle it in F7.
5. **Spec drift:** the extra decisions in report.md (the ownership-transfer fix, the stop on a `root_id` mismatch) must reach `docs/agora/agora-spec.md` in F12.

## M1 - Shared services root — round 2

**Result: PASS** (Specs PASS, Standards PASS). This reviews the fixes in 6ed17c6, 89fd81f and 05a2522.

### Specs

**Verdict: PASS.** All four round 1 findings are resolved.

- **(a)1 Group settings:** `prerequisites.md` item 4 step 3 now has these settings:
  - label Mailing, access type Restricted, Only invited users;
  - External ticked under Who can post. The type then becomes Custom, which the file states as the correct final state. groups.google.com "Anyone on the web" is given as the equivalent, which settles spec l.138.

  Item 4 now opens as the plugin's manual setup guide, with the "Per new member" and "The joining member" lists (spec l.141–142, and #11's last comment). The docs side is F12.
- **(b)2 Own calendar:** stated once, in rules → Calendar conventions → The speaker's own calendar. `ask`, `review` and `schedule` point to it, and Vocabulary names it as a per-mode difference. It still goes beyond spec l.109 and l.206; that is accepted, and the spec wording is updated in F12. Basic mode is unchanged.
- **(c)3:** "A new shared services root is written without `assistant_account`; a root moved from basic mode keeps it". This matches l.181.
- **(c)4:** a Members table written before Kind keeps its shape. New rows, the github row included, follow the header, and members are read as `human`.

**New, minor (carried to M2)**

1. **(c) The two Gmail checks disagree on who sends the test.** Item 4 step 3 says the test is "sent from an address outside the organisation". Item 7 still asks the member to send it, and a member's address is inside the organisation, so the test never exercises External posting. Fix: item 7 asks for the test from an outside address, or item 4 drops the clause.
2. **(b)** "The joining member" step 3 (accept the shared calendar invitation) isn't in spec l.142. It's reasonable, but carry it into `agora-spec.md` in F12.

### Standards

**Verdict: PASS.** The checker passes ("Valid for Claude and OpenAI").

- Advisory 2 is addressed: the own-calendar rule is stated once, and the Vocabulary claim now holds.
- Advisory 3 is addressed: "Kind changes nothing" is only in Vocabulary. The `templates.md` line "read every member as `human`" covers a different case, so it isn't a duplicate.
- Commits name the review item they fix. The slices are small and English-only.

**New advisories (carried to M2)**

1. **Possible Duplicated Code, `prerequisites.md` item 4 step 3.** The intro lists every setting that steps 3.1–3.2 then repeat. Cut the intro to the goal.
2. **Pointer tension, `prerequisites.md` header.** It says a joining member "walks items 1–3, 5 and 6", while item 4 is also the joiner's guide in shared services mode. Reconcile them in F5.
3. **Sprawl.** Item 4 is the longest block in the file, and only one mode uses it. If F5–F7 grow it further, move the shared services admin guide to its own reference with one pointer. This is the same move as round 1's advisory 1 for `setup`.
