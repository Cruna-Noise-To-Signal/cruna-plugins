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

## M2 - A team on it — round 1

**Result: FAIL** (Specs FAIL, Standards PASS). This reviews commits 5fe3ef1..f437185 (F5, F7, F9 and the M1 carry-overs).

### Specs

**Verdict: FAIL.** F5, F9 and the M1 carry-overs are covered. One F7 basic-mode regression must be fixed.

**(c) Implemented but wrong**

1. **MUST FIX: the duplicate key changes basic-mode dedupe.** Plan F7 says "Basic-mode email behaviour is unchanged", and spec l.257 says "same results as on 0.6.0".
   - `rules/references/email.md` → Mailboxes defines "The same email" as "the same Gmail id in the same mailbox, or the same duplicate key from any mailbox". The ledger skip and merge, filing's "already filed" and to-do "already added" all use it.
   - Basic mode has one mailbox, so two different emails with the same sender, minute and subject (common for notifications) now merge. On 0.6.0 they didn't.
   - Worse, basic ledger rows keep **Date as a date only**, so the comparison drops to sender + day + subject and silently skips real mail.
   - Fix: "from **another** mailbox". Basic mode then dedupes by id, as on 0.6.0.
2. **Minor: the overdue flag lags one period.** Spec l.211–214 asks for "a missing weekly review for last week … monthly review for last month".
   - Weekly and monthly files are named after the week or month that contains tomorrow (`review` → Periods).
   - Item 8 checks the file named after the previous period, so a missed run is flagged a whole period late.
   - The fix must avoid a false positive before that period's routine has run, for example the 07:30 daily run on the 1st.
3. **Minor:** the basic sent-mail form `deliveredto:<inbox> OR from:<inbox>` has no parentheses, unlike the shared services form. It is correct under Gmail precedence, but wrap it to be safe on both connectors.

**(a) Missing or partial:** none in M2.
- The Tidied line and skip, the ownership check in review, Join's checks naming the step and who does it, and the calendar-owner block on Offboard match spec l.143–149, l.170–175 and l.209–218.
- The round 1 `deliveredto:` point is settled, and the round 2 carry-overs are done.

**(b) Scope creep:** none blocking. The Join Workspace-account check by `inbox`'s domain assumes the group is on the members' domain, which is fine.

**Deviations in report.md, both sound, both to F12**
- **Date header time instead of received time:** every copy shares the Date header. Convert it to the root's timezone for names. It replaces spec l.198.
- **Diary ids by id first, then search:** robust where "the author's mailbox" isn't. It replaces spec l.200.

**Also for F12:** the sent-mail filter (which widens l.107 and l.199), the Date-to-the-minute ledger rows, the new reference files, and the overdue-period definition.

**Basic non-regression otherwise:** `setup` basic text matches 0.6.0 apart from the intended ownership line, branch hint and pointers. Basic item 7, sender voice, received-time naming and the empty Mailbox column are unchanged.

### Standards

**Verdict: PASS.** The checker passes. Only English changed, and the prefixes and slices are fine.

**M1 carry-overs, all resolved:**
- the item 7 and item 4 tests agree;
- the Group intro is cut to its goal;
- the prerequisites header is reconciled;
- the item 4 guide moved to `rules/references/shared-services-setup.md`;
- setup's per-mode parentheticals became one pointer per step into `setup/references/shared-services.md`;
- the `deliveredto:` sent-mail form is defined once.

**What conforms:** Mailboxes, email time and the duplicate key are defined once in `email.md` → Mailboxes, and callers point to it. Join and Offboard end on checkable Done-whens.

**Advisory (non-blocking)**
1. **Possible Mysterious Name.** `setup/references/shared-services.md` and `rules/references/shared-services-setup.md` have near-identical names but do different jobs. `setup/.../shared-services.md:67` cites "shared-services-setup.md → A member leaving" with no skill path. Write it as "the `rules` skill's `references/shared-services-setup.md`", or rename the guide (e.g. `manual-setup.md`).
2. **Duplication, calendar-owner transfer.** setup Offboard step 1 restates the rules guide's transfer and check. Keep only the block in setup.
3. **Mode-branch claim drifting.** `review` item 8 and its Done-when say "in basic mode, an account mismatch is flagged". Phrase it without the mode ("where prerequisites item 3 applies"), so Vocabulary → Mode stays true.
4. **Possible Divergent Change, review item 8.** It mixes overdue routines with the ownership check. Split it into two items.
5. **Spec drift, for F12:** the 60-minute Tidied window, the duplicate key and Mailbox semantics, and the joiner's calendar-invitation step.

## M2 - A team on it — round 2

**Result: FAIL** (Specs FAIL, Standards PASS). This reviews the fixes in 51b9618..3ae0703.

### Specs

**Verdict: FAIL.** (c)1 and (c)3 are resolved. (c)2 is fixed for running roots, but a new root's first run still raises false flags. That is one small must-fix.

**(c)1 Basic dedupe by Gmail id: resolved.** `email.md` → Mailboxes now says "the same duplicate key from **another** mailbox … Basic mode has one mailbox, so there the Gmail id alone decides." Every caller reduces to "same id" in basic mode, so basic non-regression holds:
- the ledger skip (l.72) and merge (l.75);
- filing's "already filed" (l.50);
- the to-do "already added" (l.39);
- rules' diary-duplicate check (`rules/SKILL.md` l.142).

**(c)2 Overdue flag: mostly resolved.** The walk-through used real dates (2026-11-09 is a Monday):
- **Mon 07:30:** no false flag.
- **Tue:** a missed weekly is flagged about a day late, which counts as prompt.
- **Dec 1, 07:30:** no false flag.
- **Dec 2:** a missing December file is flagged.

**MUST FIX:** review item 8 raises a false flag on a new root's first run, in both modes.
- Its exception is "Leave out … a period when the oldest file in `reviews/` is dated after it began".
- Setup builds `reviews/` empty, so the first daily finds no oldest file.
- Read literally, it flags yesterday, the current week and the current month.
- Fix: leave out every period while `reviews/` is empty, or date the root from `org.md`'s creation.

**(c)3:** both sent-mail forms are wrapped in parentheses, and no unwrapped copy is left.

**New, minor**
1. **`manual-test.md` §6 (l.145), weekly step.** Its "raises no flag" claim doesn't hold every day:
   - on a Sunday the run writes next week's file;
   - on a Monday the trashed file is last week's;
   - on a root younger than the week, the oldest-file rule hides the flag.

   Fix: state the day to run it (Tuesday to Saturday) and the root's age.
2. The F5 rename is clean, and Offboard keeps only the block.

**(a) Missing:** none in M2. **(b) Scope creep:** none.

### Standards

**Verdict: PASS.** All four round 1 advisories are addressed, and the checker passes.
1. The guide is renamed to `rules/references/manual-setup.md` and cited by its skill path. Nothing outside `change-log/` still mentions `shared-services-setup`.
2. setup Offboard step 1 keeps only the block and points to the guide. The guide cites rules → Replace for the `org.md` write.
3. Item 9 and its Done-when are worded without the mode.
4. Item 8 covers overdue routines only, item 9 the connected account. The pointers say "items 7–9".

New M2 work conforms: the duplicate key, the sent-mail forms and the overdue period are each defined once.

**Advisory (non-blocking)**
1. **Possible no-op, item 8:** "Leave out the file this run writes in step 3" only matters on an attended weekly run from Tuesday to Saturday. Say that, or delete it.
2. **`manual-setup.md` step 1:** "item 3's Calendar check" is ambiguous. Write "prerequisites item 3".

## M2 - A team on it — round 3

**Result: PASS** (Specs PASS, Standards PASS). This reviews the fixes in 53ae537 and b68b6f9.

### Specs

**Verdict: PASS.**

**Round 2 MUST FIX resolved.** Item 8 now reads "while `reviews/` is empty, flag no missing file; otherwise leave out every period whose first day is on or before the day the oldest file in `reviews/` was created". The walk-through used a root set up Wed 2026-11-11 afternoon:
- **Thu 12:** no flag (empty).
- **Fri 13:** Thu, W46 and November left out.
- **Sat 14:** a missed Fri daily is flagged.
- **Mon 16:** W46 left out; it was never due.
- **Tue 17:** a missed Sunday weekly is flagged.
- **Dec 1:** November left out, and W49 is checked.
- **Dec 2:** a missed Dec 1 monthly is flagged.

**Old root with a year of reviews:** nothing is hidden. Replace only re-creates current-period files, so the oldest file never moves forward. Basic mode is unaffected beyond the intended F9 flag.

**Manual test §6 weekly step: resolved** (Tuesday to Saturday, root older than this Monday).

**New, minor (carried to M3)**
1. A root whose first file is created on the 1st never gets its first monthly flagged. This is the accepted first-run blind spot: note it in the F12 spec.
2. `manual-test.md` §6 step 1 (trash yesterday's daily) should say "on a root older than two days".

### Standards

**Verdict: PASS.**
- Advisory 1 is resolved: the step-3 clause now gives its reason (an attended mid-period weekly or monthly run).
- Advisory 2 is resolved: "prerequisites item 3's Calendar check".

**Advisory (non-blocking, carried to M3)**
1. Item 8's first bullet is about 130 words and holds four rules. Split it into a short sub-list.
2. The oldest file is dated by created time while periods are read from names. If that is intentional, say why in a few words.
3. Rephrase "A new root had no review to write before its first file" as "A new root has nothing due before its first review file".

## M3 - Moving and documenting — round 1

**Result: FAIL** (Specs FAIL, Standards PASS). This reviews commits a018d4b..620a776, plus a final pass over the whole branch.

### Specs

**Verdict: FAIL.** One must-fix: the founder's latest instruction on #11 isn't implemented. Everything else in M3 meets the spec. The checker passes, and agora and agora-ita are at 0.7.0 in all four manifests and the Claude catalog.

**(a) Missing or partial**

1. **MUST: the shared services mail filter ignores #11's comment of 2026-11-06 14:54.** It says: "use `list:name-of-the-group.company-domain OR to:name-of-the-group@company-domain OR cc:name-of-the-group@company-domain`. The list: part catches what others sent, and the rest catches your own posts, which never get the List-ID header in your mailbox." For Cruna that's `list:assistant.crunasignal.com OR to:assistant@crunasignal.com OR cc:assistant@crunasignal.com`.
   - The code still uses `deliveredto:<inbox>` (or `list:<inbox>` with the address, not the dotted list-id) in:
     - rules → The three ids → Mail;
     - the prerequisites item 7 check;
     - Join, Verify and Move step 7's Done when;
     - agora-spec (l.188, l.203);
     - Italian.
   - It even lists "deliveredto vs list:" as still to verify live, but the founder has settled it.
   - Report.md notes that Groups stamps each member's own address in Delivered-To. If so, shared services email, the item 7 check and the Move check all find nothing.
   - Fix: the shared services received and sent forms become the founder's form. Basic mode stays as it is. Update agora-spec, the manual test FACT list and Italian to match. Spec l.107 is overtaken by the comment.
2. Everything else checked is present:
   - every F12 item from report.md and review.md is in agora-spec;
   - the manual test covers every Testing Decisions scenario, with §12 as the founder's basic non-regression test, sent-mail search included;
   - the M2 carry-overs are done in both languages;
   - Italian line counts match, and the only English left is identifiers, box labels and bracketed UI labels.

**(b) Scope creep:** none needing a fix. The hand-over step, filing `landing/` before a copy, and keeping the old account alive follow from the spec's steps.

**(c) The three flagged decisions**
1. **Old address trusted through the mover's Mail from cell:** accept. It follows grilled-idea ("trust the old address in `email_from`"), and agora-spec states it. Minor risk: if the mover is offboarded, the trust goes with them. One line in Offboard should move it to another member.
2. **Old root trashed by clicks as the assistant account:** correct. In My Drive the Workspace account can't trash files it doesn't own, and step 8 runs only after the count check and the new `org.md`.
3. **Manual setup as a section of agora-spec.md:** accept. Both READMEs link to `#manual-setup-shared-services-mode`, and `rules/references/manual-setup.md` ships in both languages.

**End to end (M3 done-when):** met apart from finding 1. Basic mode is unaffected by it.

### Standards

**Verdict: PASS.** There are no must-fix issues in M3 or in the whole-branch pass (39 commits, 50 files).
- **Versions:** 0.7.0 in both manifests of both languages and in the Claude catalog. The OpenAI catalog has no version field (portable-plugins format). The checker passes.
- **Frontmatter:** only name and description. The new triggers are in user words.
- **Spec:** `docs/agora/agora-spec.md` covers modes, Manual setup (the README anchor exists), Move, Mailboxes, overdue flags, and what is still to verify live.
- **M2 round 3 carry-overs:** all resolved.
- **Italian mirror:** identical line, heading, step and bullet counts per file. Identifiers stay in English, and UI labels appear in Italian with the English in brackets. The glossary gains about 50 terms.
- **F6 Move:** resumable, with a Done when per step. It never trashes before the count check.

**Advisory (non-blocking)**
1. setup `SKILL.md` §4b summarises all ten `move.md` steps. Point to `move.md` with a one-line purpose instead.
2. The Gmail forwarding click path is in both `move.md` step 7 and agora-spec l.258. The spec could point to the skill.
3. Italian `move.md` step 1, "ogni membro in Members": set Members in backticks or bold.

## M3 - Moving and documenting — round 2

**Result: FAIL** (Specs FAIL, Standards PASS). This reviews the fixes in 40d2ba8, 9897eee and 22af709.

### Specs

**Verdict: FAIL.** One must-fix: the `-from:me` the fixer added. Everything else asked for is implemented.

**What conforms**
- **Founder's filter (#11, 14:54):** defined once in rules → The three ids → Mail, in both languages, with the founder's example. email.md, prerequisites item 7, setup shared-services §7, manual-setup, agora-spec l.189/l.520 and both manual tests use it or point to it.
- **`deliveredto` grep:** no `deliveredto` is left in shared services text. Basic mode keeps `deliveredto:<inbox>` / `(deliveredto:<inbox> OR from:<inbox>)` word for word.
- **#11:** no comments after 14:54.
- **Offboard:** step 5 moves the retired assistant address to another member's Mail from cell. Both Done whens check it, in both languages, in agora-spec l.254, and with a manual-test row.
- **Advisory:** change-log spec.md l.107/129/187/199 still say `deliveredto:`, but agora-spec records the founder's later decision.

**MUST FIX: `-from:me` on the received-mail form** (rules SKILL.md l.52, prerequisites.md l.119, ENG+ITA; agora-spec l.189; manual test l.31)
- It undoes the founder's stated reason, "to enable the same response no matter the account".
  - When member A forwards or CCs a client email to the group, A's triage drops it, while B's files it.
  - Forwarded and CC'd member mail is the main thing triage files (email.md l.47, l.119), so the result depends on whose routine runs. If only A runs reviews, A's forwards are never filed.
  - Basic mode's `deliveredto:` keeps member forwards as received mail, so excluding them is a regression.
- Spec l.194 doesn't justify it: that rule covers every member's Mail from, not only the connected account's.
- It isn't needed: Agora-sent mail already has a ledger row (email.md l.70–72), keyed by id and by the duplicate key, so triage skips it in every mailbox.
- **Fix:** drop `-from:me` everywhere. In shared services the received-mail and sent-mail forms then become the same.
- **Item 7 / Move step 7:** not broken. The outside-sender test still proves External posting.

**Scope:** nothing else in e313f09..HEAD went beyond the round 1 findings.

### Standards

**Verdict: PASS.** The checker passes, 0.7.0 is unchanged, the Italian mirror has identical line counts, and frontmatter is unchanged.
- Round 1 advisories 1–3 are done:
  - setup §4b points to `move.md`;
  - the agora-spec forwarding paragraph points to `move.md` step 7;
  - Italian `Members` is in backticks.
- The glossary adds "list-id" and "indirizzo dismesso dell'assistente". agora-spec is updated in the same commits as each behaviour change.

**Advisory (non-blocking)**
1. **Possible Duplicated Code:** the `@`→`.` list-id derivation is spelled out in rules, `prerequisites.md` item 7 and agora-spec, and half-repeated in the manual test. Prerequisites should point to rules → The three ids → Mail.
2. **Glossary row 1** holds two terms ("list-id …; the member's own posts"). Split it.
3. **Sent-mail limit:** the shared services sent-mail form only finds mail with the group in To or Cc. A member's reply to an outside client that leaves the group out isn't found. Agora always CCs the group, so this only affects mail a member sends by hand. Note it in agora-spec and in the manual test's "Did we answer Mario?" row.

## M3 - Moving and documenting — round 3

**Result: FAIL** (Specs FAIL, Standards PASS). This reviews the fixes in a8d7ea7, 2d0d5d4 and a157280.

### Specs

**Verdict: FAIL.** The checker passes, the version is 0.7.0, ENG and ITA line counts match, and #11 has no newer comments.

**What conforms**
- No `-from:me` is left anywhere.
- The shared services filter is exactly the founder's `(list:<list-id> OR to:<inbox> OR cc:<inbox>)`, one form for received and sent mail, defined once in rules (ENG and ITA). Callers point to it.
- The filter has no account-specific term, so every account gets the same answer.
- Basic mode is word for word unchanged.
- Advisories 1–3 are done.
- grilled-idea.md is fully covered.

**(c) MUST FIX: Agora-sent replies are not kept out of triage by the ledger.** rules SKILL.md l.52, email.md l.116 and agora-spec l.189 claim "Agora's own sent mail is kept out of triage by its ledger row (id, and the duplicate key across mailboxes)".
- That holds for "any other email" (email.md l.71), which gets its own row keyed to the sent id.
- It doesn't hold for email.md l.70: a reply or forward to an email whose row isn't `filed` goes on that email's row, and "the row keeps the original message id".
  - The sent reply's id is in no row, and its duplicate key (sender A, reply time, "Re: …") matches no row.
  - With `-from:me` gone, A's triage finds it through `cc:<inbox>` and B's through `list:`. Both treat it as CC'd member mail: it is filed (usually caught by the diary-name check) or flagged as needing attention.
- **Fix:** in shared services mode, record the sent reply's own id as well. Either give it its own `sent → <diary path>` row, or have triage skip mail whose id or key matches a `sent` diary entry. Update agora-spec in the same commit, in both languages.

**(a) Missing:** none. **(b) Scope creep:** none.

**Advisory**
- "received-mail search" still appears for shared services in setup/references/shared-services.md l.81 and agora-spec l.225 and l.324. Say "search with the mail filter".
- change-log spec.md still says `deliveredto:`. agora-spec records the founder's decision.

### Standards

**Verdict: PASS.**
- Round 2 advisories 1 (list-id derivation only in rules and agora-spec; callers point) and 2 (glossary rows split) are done.
- The ENG/ITA mirror holds (230/156/133 lines), the glossary terms are used, and agora-spec is updated in the same commit.
- 0.7.0 is in both manifests and both catalogs, and the checker passes.
- The smell baseline finds nothing new; the change removes duplication.

**Advisory (optional):** `references/email.md` l.18, "the mail filter in its sent-mail form". Add "(in shared services mode, the one form)".

## M3 - Moving and documenting — round 4

**Result: PASS** (Specs PASS, Standards PASS). This reviews the fixes in e26c99c and 79fe911. Spec advisory 2 below is a latent correctness gap, so it gets a small follow-up revision before close.

### Specs

**Verdict: PASS.** The round 3 MUST is resolved: replies and forwards get their own `outgoing` row, keyed on the draft id and then on the sent id, with By set to the sending member.

**Trace**
- Client X to the group: A flags it, giving row X with Mailbox=a.
- A replies with the group in CC: row X becomes `sent`, and a new `outgoing` row holds A's sent id.
- A's next triage: the sent copy matches the `outgoing` id, so it is skipped.
- B's next triage: B's copy has a different id. Its duplicate key (A's Email, Date minute, "Re: …") matches the `outgoing` row, so it is skipped.

**Leak check: clean.**
- The review Email section never lists `outgoing` rows.
- To-dos join only filed, drafted or sent rows.
- "Drafts waiting" takes `drafted` only; an unsent draft's `outgoing` row is pruned at 60 days.
- No false copy merge, the prune is 60 days, the Mailbox column holds the writer's mailbox, and Replace is unchanged.

**Basic mode:** unchanged from 0.6.0. Every clause is gated on shared services, and basic mode writes no `outgoing` rows.

**Consistency and completeness**
- ENG and ITA match, and the glossary has `outgoing`.
- agora-spec l.189 and l.411 match the rules, and the manual test has the A/B row.
- Round 3 advisories are done.
- The request is complete end to end.

**Advisory**
1. email.md l.71 "dated with the send time" should be "dated with the sent message's email time (Mailboxes)", to match the duplicate key's Date-header minute.
2. **Latent gap.** l.17 says "for a row Agora sent, the sender is the Email of the member in By". After a reply, the original row (From = client, outcome `sent`, By = member) also reads as "a row Agora sent". It would then be keyed on the member's address, so B's copy of X misses it. Use "for a row whose From is `to <recipients>`".
3. review SKILL step 3 lists new ledger rows "by Email section part". Say that `outgoing` rows are left out.
4. change-log spec.md still says `deliveredto:`; agora-spec records the founder's decision.

### Standards

**Verdict: PASS.**
- English and Italian change together, with matching word diffs, and the glossary terms are used. `outgoing` is added to the untranslated outcomes in the same commit.
- agora-spec is updated in the same commits, and both manual tests cover the A/B reply case.
- 0.7.0, and the checker passes.
- The Email section selects rows by outcome, so `outgoing` is excluded without a change.

**Advisory**
1. The `outgoing` rule is written in full in email.md and agora-spec, and both `email` steps partly repeat it.
2. Say what happens to an unsent draft's `outgoing` row: it is pruned at 60 days.
3. Say what an `outgoing` row's Date is before the send.

## M3 - Moving and documenting — round 5

**Result: PASS** (Specs PASS, Standards PASS). This reviews commit 05ec457.

### Specs

**Verdict: PASS.**
- **Sender rule.** The member in By counts as sender only for a row whose From is `to <recipients>` (email.md l.17–18 EN and IT, agora-spec l.401).
  - Trace: X's row stays keyed on X after A's reply, so B's copy of X is skipped.
  - A's reply has an `outgoing` row keyed on A's Email, the Date minute and "Re: …", so B's copy of the reply is skipped too.
  - Basic mode is unchanged: the Gmail id alone decides, and basic mode writes no `outgoing` rows.
- **Dating and prune.** An `outgoing` row is dated with the draft's time, then with the sent email time on send. An unsent draft's row is pruned after 60 days.
- **Review step 3.** It leaves out `outgoing` rows, in EN and IT.
- **Agreement.** EN, IT and agora-spec agree, and per-file line counts match. 0.7.0, and the checker passes.
- **Advisory:** agora-spec l.358 doesn't say that review Updates leave out `outgoing` rows. The rules and skill are correct.

### Standards

**Verdict: PASS.** The prefix and F7 tag are right, agora-spec is updated in the same commit, EN and IT change line for line, and there's no new smell.
- **Advisory:** change-log spec.md still says `deliveredto:` (l.107, l.129, l.187). agora-spec records the founder's later decision (#11, 2026-11-06 14:54) and is the reference for the shared services filter.
