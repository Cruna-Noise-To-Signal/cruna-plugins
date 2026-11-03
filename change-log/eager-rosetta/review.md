# Review — eager-rosetta

## M1 — round 1

**Result: FAIL** (Specs FAIL, Standards FAIL)

### Specs

VERDICT: FAIL. Reviewed M1 diff (F1, F2, F3, F5) against spec.md and plan.md. Most of M1 is in place and the checker passes.

**(c) Implemented, but wrong**

1. **New root throws away email answers for members who aren't present.** Spec: "Members round: email addresses, and each member's importance question when email is on." Story 7: "I want setup to ask for every email address I write or forward from."
   - `setup/references/interview.md` Round 2 asks the email questions for every member.
   - But `setup/SKILL.md` step 2d runs the member part only "for the owner".
   - Root part step 6 then ticks the root Email box, which deletes the Interview section that still holds the other members' answers.
   - So their addresses and criteria are lost, and Join asks them again; Verify ("every member using the root has addresses in Emails") can't pass until each member joins.
   - Fix: in 2d, write every interviewed member's Emails cell and `Email importance:` line. Keep only the Gmail check, and ticking the box, for each member's own app.

**(a) Missing or partial**

2. **Ambiguous email in conversation is never flagged.** Plan F3 acceptance: "an ambiguous email is flagged". Spec: "I want an email the AI can't place to be flagged under Needs attention…". In `email/SKILL.md` step 6.3, "When no single instance fits, ask once" — nothing says what happens if it is still unplaced. Step 5.3 (sent mail) does say "still none, flag it". File should get the same fallback, with a `flagged` ledger row. Minor.

**(b) Scope creep (small, none blocking)**

3. **Third-person body by default.** `rules/references/email.md` → Sender voice says "by default, the assistant speaks for them in the third person". The spec only asks "signed as the assistant on behalf of me". A new behaviour rule the spec doesn't have.
4. **README skill-table row.** The spec puts README changes in F8. Harmless.
5. **Shared memories hold `see Conventions`.** Spec data shape: "`Email importance: not asked | invited | no preference | <criteria>` … in `memories/shared.md` for the root". Record the decision in the spec at F8.

**Checked and correct:** sending (exact-message confirmation, routines never send, emailed instruction → draft only, one Agora confirmation then platform prompt); member mail matched on address only; filing (forwarded/CC'd only, author = forwarding member, email fields and `To file:` note, deterministic entry names); `inbox.md` curated with template and spec outcome values; `org.md` `email:` field and Emails column; `setup.md` Email boxes; Review template `## Email`; prerequisites item 7 with real search call and platform limits; setup "turn on email" branch, Join, Migrate note, Verify line; solo roots file only on request; criteria asked on *not asked*/*invited*, "no preference" recorded; `email` skill frontmatter and `agents/openai.yaml`.

### Standards

VERDICT: FAIL. Checked against AGENTS.md and cruna-core `writing-for-agents` (single source of truth, co-location, completion criteria, pointer wording). Frontmatter, README row and openai.yaml are fine.

**Fix before merge**

1. **Duplicated Code / Shotgun Surgery: the criteria-recording rule.** The root-wide criterion format (a Conventions row, Where `*` or the interest, `important email: <criteria>`, Since today plus the member's slug, `shared.md` set to `see Conventions`) is written out word for word in `email/SKILL.md` step 7 and `setup/references/email.md` Root part step 3; `rules/references/email.md` → Where criteria live states it a third time and `templates.md` a fourth. Same for the `draft replies to …` opt-in. Keep the definition in rules → Email → Importance and point to it from the other sites ("record it as in rules → Email → Where criteria live").
2. **Sending policy restated.** `email/SKILL.md` step 5.1 rewrites the confirmation rule right after "Follow rules → Email → Sending". The warning about the app's own approval prompt appears three times: rules Sending, prerequisites item 7 Limits, and step 5.2. Steps should say only *when* each rule fires.
3. **Completion criteria that don't match their steps:**
   - `email/SKILL.md` step 1's Done-when requires "`inbox.md` is read", but nothing in the step tells the agent to read it.
   - `setup/references/migration.md` step 6 now makes three changes (adds Emails), but its Done-when still says "shows both changes".

**Judgement calls (not blocking)**

- **Possible Mysterious Name:** Members table has both `Email` (Google account the folder is shared with) and `Emails` (addresses counting as member mail). Suggest `Mail from` or `Sender addresses`; one rename in `templates.md` plus references.
- **Duplicated pointer contents:** `rules/SKILL.md` → Email and the `email/SKILL.md` intro both list everything `references/email.md` contains. A one-line pointer saying what the file is and when to read it would do.
- **Possible Divergent Change:** `setup/SKILL.md` renumbering Verify 6→7 forced six cross-reference edits (all updated correctly). Naming targets ("go to Verify") would make future insertions cheaper.

## M1 — round 2

**Result: PASS** (Specs PASS, Standards PASS). Non-blocking findings carried into M2 as cleanups.

### Specs

VERDICT: PASS. Diff `9c45727...HEAD`, 11 commits ending at dec47ca; `check_plugins.py` prints "Valid for Claude and OpenAI".

**Round 1 findings: all resolved** — Specs 1 (member answers kept for every interviewed member; only Gmail check and Email box wait for each member's app), Specs 2 (unplaced email flagged with a `flagged` ledger row), Standards 1–3 (criteria rule defined once in rules → Email → Where criteria live; sending policy only in rules → Sending; completion criteria match their steps), judgement calls (`Emails` → `Mail from`; one-line pointers).

**New findings (none blocking)**

- (c) 1. **Interrupted setup can still lose member answers.** Spec: "Members round: email addresses, and each member's importance question when email is on." `templates.md` says the answers "stay until the root Email box is ticked", but root part step 6 ticks that box before the member part (steps 2–3) writes the members' records. If a session stops in between, the answers are gone and Join re-asks only the members present. Fix: tick the root box after the member records are written, or keep the answers until then.
- (c) 2. **The ledger key changes from draft to sent.** Spec: `inbox.md` has "one row per handled email: Gmail message id … outcome … `sent → <diary path>`". `email` step 5.4 turns "the draft's row" into `sent`, but rules → Ledger keys a new email "on its draft or sent message id", and the two ids differ in Gmail. Say which id the row keeps after the send.
- (a) 3. **Two small gaps in setup:** the 2d completion criterion doesn't say member records aren't needed when email is declined (only the root box gets "(off)"); a sent-mail diary entry has no file-name rule (filing has one; the `inbox.md` template example implies `<slug>-YYYYMMDD_HHMM`).
- (b) Scope creep: nothing new (third-person body, README row, `see Conventions` deferred to F8).

**Checked and correct:** sending only on confirmation of the exact message, emailed instruction → draft; member mail matched on address only; filing only forwarded/CC'd, forwarding member as author, `To file:` line; solo root files only on request; prerequisites item 7; Email branch, Join, Migrate note, Verify line; pre-email `setup.md` reads as off; criteria asked on *not asked*/*invited*, "no preference" recorded; `email` frontmatter and `agents/openai.yaml`.

### Standards

VERDICT: PASS. Read `git diff 9c457275...HEAD` (11 commits), focused on the fix commits 4ab04bf..dec47ca, against AGENTS.md and cruna-core `writing-for-agents`.

**Round-1 Standards findings: all resolved** — criteria rule single-sourced in `rules/references/email.md` → Where criteria live; app-approval warning only in rules → Sending; `email` step 1 reads `inbox.md`, migration step 6 names all three changes; `Mail from` rename with no leftovers; one-line pointers; Spec finding 2 fixed; Divergent Change deliberately not done (optional).

**New observations (judgement calls, not blocking)**

- **Possible Duplicated Code.** The new-root split of the member part ("records (steps 2–3) for every member interviewed, Gmail and the Email box (steps 1 and 4) for the member present") is written three times: `setup/SKILL.md` step 2d, the first "Run from" bullet in `setup/references/email.md`, and the Member-part preamble in the same file. The preamble is the natural single source.
- **Possible cache of the environment.** `email/SKILL.md` step 5.2 restates the ChatGPT send limits before pointing to prerequisites item 7. A pointer alone would do.
- **Minor wording drift.** rules → Sending says "client", `email` step 5.2 says "app"; "app" is the consistent term.
