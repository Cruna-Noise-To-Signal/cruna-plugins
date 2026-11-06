# earnest-tozzetto — implementation report

## M1 - Shared services root

F1, F2, F3, F4 and F8, in the English plugin only (`cruna-plugin-eng/agora/skills/`). No version bump, no Italian, no spec or glossary changes (M3).

### What changed

- **F1 `rules`** (`SKILL.md`, `references/templates.md`, `references/email.md`):
  - Vocabulary: **Mode** (both modes, basic is the default when `mode` is missing), **Connected account** (its email is the primary calendar's id), member **Kind** (changes nothing). Assistant account marked basic-only; Assistant inbox redefined as "mail delivered to `inbox`", so the existing email skills read right in both modes.
  - New **The three ids** section with the **Same answer** rule: files under `root_id`, calendar by `calendarId = calendar`, every Gmail search on `deliveredto:<inbox>`, stated as Agora's own rule, not a permission boundary.
  - Finding the root: `root_id` names the root; a `root_id` that isn't the folder holding `org.md` stops writes (copied or moved root).
  - Working with Drive: in a shared drive, check membership by walking parents to `drive_id`, never by `sharedDriveId` (issue #11 clarification).
  - Replace: a refused trash names its cause per mode.
  - Calendar conventions: the system calendar is always `calendar` by id; personal calendars read at the Personal calendar level (`domain free/busy` added).
  - Templates: `org.md` gains `mode`, `drive_id`, `root_id`, `calendar_owner`, `inbox` with their defaults, the **Kind** column, per-mode meaning of Email, Personal calendar and Access; `setup.md` gains the shared services Member boxes; `inbox.md` gains the **Mailbox** column (empty = assistant inbox); root `status.md` gains the `**Tidied:**` line (written only by the tidy-up, which F9 adds).
- **F2 Identity**: shared services mode reads the speaker from the connected account and matches Members Email, never asking; no match offers Join and writes nothing. Basic mode unchanged. `review`'s attended run and `file-document`'s uploader slug follow suit.
- **F3 `prerequisites.md`**: every item now has basic and shared services variants where they differ. Basic text is unchanged except item 3's ownership check (blocked on mismatch, with fixes) and the regrouping under headings. Item 4 is now "Assistant account, or the admin's setup" (Workspace accounts, shared drive, group, calendar, connector enablement, then the per-new-member list). Offboarding gains the shared services steps with the calendar owner's transfer first.
- **F4 `setup`**: New root asks about Google Workspace; no keeps today's 2a steps verbatim under "Basic mode". Yes: admin's setup, drive id from the URL with create-then-trash, Calendar ID with create-then-delete, root folder created inside the shared drive, the six ids written to `org.md`, sharing as checks only. Verify per mode (basic adds the ownership check). Interview asks Kind and the Workspace account; `setup/references/email.md` ticks the In the group box.
- **F8 calendar**: `schedule`, `ask`, `review` address Agora events as `calendarId = calendar`; free time per the Personal calendar column, plus the speaker's own calendar when the connected account is theirs.

Checker (`cruna-core` `check_plugins.py`): **✔ Valid for Claude and OpenAI**.

### Testing

Not testable by code: the whole change is skill prose, and the only seam is the member conversation against a Drive root. No `tdd` cycle was possible; the hand test is [manual-test.md](manual-test.md) → M1.

### Menu paths checked (2026-11-06)

Against Google's help pages: shared drive (Shared drives → New; Manage members; roles Manager / Content manager / Contributor / Commenter / Viewer), Google Groups (Create group; Who can join, Who can post; subscription Each email; Directly add members), Calendar (Create new calendar; sharing; Transfer ownership within the organisation, 60 days to accept; a secondary calendar is deleted with its owner's account), Claude (Organization settings → Connectors, Owner or Primary Owner). Found and added: Groups needs the Admin console setting **Group owners can allow incoming email from outside the organization**, off by default, or client mail sits in moderation.

Not confirmed from official sources: ChatGPT Business's exact path (written as Workspace settings → Apps, plus Gmail write actions), and the Admin console path for an admin-side calendar transfer (written as "in the Admin console" without a path). Google's help now names the calendar permission **Make changes and see event details**, while many screens still say **Make changes to events**: both names are given.

### Decisions taken where the spec was silent

- **Item 4 structure**: kept the number. Item 4 is now one heading with two variants, "Basic mode: assistant account" and "Shared services mode: the admin's setup", so "item 4" pointers keep working.
- **Ownership mismatch after Migrate**: migration moves only the folder, so `org.md` can stay owned by the old owner while the connectors are the assistant account. Item 3's fix list adds "the owner transfers the files", besides the spec's two fixes.
- **`root_id` mismatch** (a copied root): writes stop and Verify is offered, rather than trusting either folder.
- **In the group box**: proven only by item 7's group search, so with email off it's ticked "(email off)".
- **Pulled forward from F7** to avoid contradicting text in `rules/references/email.md`: Sender voice now has the shared services line (member's address, `inbox` in CC, own voice), and Limits states the filter. F7 still owns the ledger, searches and solo rules.
- **Pulled forward from F5**, as one-line guards only: Join step 1 and step 3 say what differs in shared services mode, and Offboard points to prerequisites → Offboarding → Shared services mode. F5 still owns the full Join (admin-step naming, Kind, boxes) and the Offboard block.

### Left for later milestones / noticed

- **F5**: Join's full shared services flow; Offboard currently relies on the prerequisites block for the calendar-owner stop.
- **F6**: Replace and item 3 tell basic roots they can "move the root to a shared drive", but the Move branch doesn't exist yet.
- **F7**: `rules/references/email.md`'s tidy-up search still says "not sent by the assistant account"; the ledger doesn't yet dedupe across mailboxes or write Mailbox.
- **F7 risk, basic mode**: the three-ids rule puts `deliveredto:<inbox>` on every Gmail search, in basic mode too. Any search for sent mail would now miss. Check this in the founder's non-regression test.
- **F9**: `review` doesn't write `**Tidied:**` yet; the template says only the tidy-up writes it.
- **F10**: `help/references/faq.md` still says the root is owned by the assistant account and lives in "Shared with me".
- **To verify on ChatGPT** (plan risk): the primary calendar's id is the account's email. Identity and every Who check rely on it.
- **To verify**: the Drive connector's metadata exposes `owners` (basic ownership check) and lets a create name a shared drive folder as parent.

## M1 revision 1

Fixes for review M1 round 1 (Specs (a)1, (b)2, (c)3, (c)4; Standards advisories 2 and 3). Still English only; skill prose, so no `tdd` cycle (no automated seam), checked by hand in [manual-test.md](manual-test.md) M1 (sections 1, 4, 5 and 7 gained checks).

- **F3, Group settings** (`prerequisites.md` item 4 step 3): the founder's settings from #11 (label Mailing, access type Restricted, Who can join Only invited users), created in the Admin console. Reconciled with outside senders: tick **External** in the Who can post row, which turns the access type to **Custom**; that is the intended final state, Restricted's other rows stay. Step 3.1 (the org-wide incoming-email setting) is the precondition for the External box; groups.google.com shows the same setting as Who can post: Anyone on the web. Members are added in step 3.3. Item 4 now opens as the admin and joining member's manual setup guide, with numbered "Per new member" and "The joining member" lists. Paths checked against Google's Admin help (Create a group, Update group details, Groups for Business sharing settings) on 2026-11-06; the **Mailing** label isn't on those pages, it is taken from the founder's console.
- **F8, own calendar**: stated once in rules → Calendar conventions → The speaker's own calendar, as a per-mode difference (shared services: in full for free time, "what do we have" and Yours; basic: at the Personal calendar level). `ask`, `review` and `schedule` point to it; Vocabulary → Mode now says other per-mode differences are stated once in rules or prerequisites and pointed to. **For F12**: the spec (l.109, l.206) names only free time; `docs/agora/agora-spec.md` should carry the wider rule.
- **F1, templates**: "a new shared services root is written without `assistant_account`; a root moved from basic keeps it". A Members table without Kind keeps its shape: new rows, the github row included, follow the header's columns, and every member reads as `human`. "Kind changes nothing" now lives only in Vocabulary; `templates.md` and `interview.md` point to it.
- Left as scheduled: advisory 1 (F5), `deliveredto:` sent mail and advisory 4 (F7), advisory 5 (F12).

## M2 - A team on it

F5, F7 and F9, plus the items carried from the M1 reviews, in the English plugin only (`cruna-plugin-eng/agora/skills/`). No version bump, no Italian, no spec or glossary changes (M3).

### What changed

- **F7 mail filter, sent mail** (`rules/SKILL.md` → The three ids → Mail): the filter now has a received form, `deliveredto:<inbox>`, and a sent-mail form for searches that must also find what was sent. Basic mode: `deliveredto:<inbox> OR from:<inbox>`, so "did we answer Rossi?" finds the assistant's sent mail as on 0.6.0. Shared services mode: `deliveredto:<inbox> OR (from:me {to:<inbox> cc:<inbox>})`; other members' Agora mail reaches the speaker through the group as received mail. `OR`, `{}`, `( )`, `from:me`, `cc:` and `deliveredto:` are all on Google's search-operator page (checked 2026-11-06). The `email` skill's Find uses the sent-mail form when the question is about what was sent.
- **F7 mailboxes** (`rules/references/email.md` → Mailboxes, new):
  - a Gmail id belongs to one mailbox;
  - the **email time** is the received time, or in shared services mode the Date header's time, which every member's copy shares;
  - **the same email** means the same id in the same mailbox, or the **duplicate key** (sender + email time to the minute + subject) from any mailbox. For a sent row, the sender is the By member. The rare collision is accepted and stated;
  - an id from another mailbox (ledger row, diary `email:`, to-do `email <id>`) is opened by id first, then by search.

  The following use it: filing names and the "already filed" check, member-mail to-do names and "already added", the ledger skip and merge, and rules → Replace's duplicate diary rule. The ledger writes **Mailbox** (the connected member's slug), and in shared services mode **Date** to the minute. The tidy-up search is "received mail (the received-mail filter)". Solo is basic only. The `email` skill drafts in your own mailbox and addresses and signs per Sender voice. The duplicated Mailbox definition in `templates.md` is now a pointer.
- **F9 `review`**:
  - **Recently tidied**: a root `status.md` `**Tidied:**` line under 60 minutes old skips items 1–6 and keeps the line and the earlier flags as found. Step 3.4 writes the line when items 1–6 ran.
  - **New item 8**, run every time: flags a missing daily review for yesterday, a missing weekly review for the ISO week before the current one, a missing monthly review for the month before, and email triage older than yesterday (`last_tidy_up` is noted before item 3 moves it). All carry the fix "No member's routine has run this: set up the routines". It also runs prerequisites item 3's ownership check, which is basic mode only, as that item says.
  - Routine prompts are unchanged.
- **F3 carried items**:
  - The shared services manual steps (once per root, per new member, the joining member, a member leaving) moved to the new `rules/references/shared-services-setup.md`. Prerequisites item 4 and Offboarding keep one pointer each, and "item 4, step N" still resolves to that file's step N.
  - Step 3's intro is cut to its goal.
  - Item 7's test now comes from an address outside the organisation, which agrees with step 3's check and also avoids Gmail not delivering your own group post back to you.
  - The prerequisites header says a joining member walks items 1–3, 5 and 6 "once item 4's steps for them are done".
- **F5 `setup`**:
  - Every shared services parenthetical moved to the new `setup/references/shared-services.md`, one section per step (2a, 2b, 2c, 2e, 3 Join, 6 Offboard, 7 Verify). SKILL.md keeps one pointer per step. Its basic text reads as on 0.6.0, apart from the M1 ownership line and the branch-table row, which now also names "not yet in the shared drive".
  - **Join**: reads the connected account, finds the root (none found: wrong account, or not in the shared drive yet), and decides which join from Members Email (new member, member named at New root, or existing member on a new app). It then runs every admin check before reporting (Workspace domain, Drive create-then-trash, calendar create-then-delete, item 7, connectors), and each failing box names its item 4 step and who does it. A Drive failure blocks the rest of Join; any other failure is recorded and Join carries on. Join then writes the Members row (Kind, Email = connected account, `domain free/busy`), the Access row with what was proved, memories, the box, the email member part and routines. It shares nothing and creates no interest folder.
  - **Offboard**: blocked while the member is `calendar_owner`, with no override. Once the transfer is accepted, it updates `calendar_owner` and re-checks the calendar with a real call, then does the removals and SKILL steps 5–6.
  - Vocabulary → Mode's list of per-mode differences now names the mail filter and mailboxes.

Checker (`cruna-core` `check_plugins.py`): **✔ Valid for Claude and OpenAI**.

### Testing

Not testable by code: the change is skill prose, and the only seam is the member conversation, so no `tdd` cycle was possible. The hand test is in [manual-test.md](manual-test.md) → M2.

### Decisions taken where the spec was silent

- **Email time from the Date header** (shared services mode): the spec's key says "received time", but each mailbox stamps its own received time, which can differ by a minute between copies. The Date header is shared by every copy. Basic mode keeps the received time.
- **Ledger Date to the minute** in shared services mode, which the duplicate key needs. Basic rows keep the date only.
- **Diary `email:` ids** resolve by trying the id, then searching. The spec's "the author's mailbox" isn't reliable, because a routine in another member's mailbox can file an entry authored by the forwarding member.
- **Sent rows in the key**: the sender is the By member. This way, Agora mail a member sent with the group in CC isn't filed again from another member's copy.
- **Overdue periods**: "previous period" means yesterday, the ISO week before the current one, and the month before the current one. A period is skipped when the oldest file in `reviews/` is dated after it began (a new root).
- **Triage flag** is checked against `last_tidy_up` as found before this run's tidy-up. It mostly shows that routines had stopped; the next run drops it.
- **Join, Workspace account box**: proved by the connected account being on `inbox`'s domain.
- **The admin guide got its own file** (review advisory), since #11 asks for a reference that guides admin and joiner. Offboarding's shared services steps moved with it.

### Left for later milestones / noticed

- **F6**: the Move branch. The "retired old address counts as member mail" rule belongs there.
- **F10**: `help` should show the same overdue flags and explain both modes. `faq.md` still describes the basic layout only.
- **F12**: `agora-spec.md` should carry the decisions above, the sent-mail filter, and the new reference files.
- **To verify live**:
  - whether `deliveredto:<group>` matches group mail in a member's mailbox at all, or whether `list:<group>` is needed. Google Groups stamps the member's own address in Delivered-To. The founder confirmed only that the operators are accepted;
  - whether the sent-mail form works on both connectors;
  - whether the Drive connector can read a shared drive's members, for the Offboard check.

## M2 revision 1

Fixes for review "M2 - A team on it — round 1". English plugin only; skill prose, so no `tdd` cycle (no code seam); the hand test is updated.

- **F7, duplicate key** (MUST): "the same email" is now the same Gmail id in the same mailbox, or the same duplicate key from **another** mailbox. Basic mode has one mailbox, so it dedupes by id exactly as on 0.6.0. The ledger skip and merge, filing's "already filed", the to-do "already added" and rules' diary-duplicate check all point to that definition and read correctly in both modes.
- **F7, sent-mail filter**: both forms are wrapped in parentheses, `(deliveredto:<inbox> OR from:<inbox>)` included.
- **F9, overdue flag**: review item 8 now checks, per period, the file named after the day, ISO week and month containing **yesterday**. Under the "containing tomorrow" naming, each file is written by the end of its period's first day (weekly on the Sunday evening before, monthly on the 1st), so a missed run is flagged the next day rather than a period late, and the 07:30 daily on the 1st (or a Monday) raises no false flag. The file the run itself writes in step 3 is left out.
- **F9, item split**: the ownership check is its own item 9, phrased without the mode ("where that item has one"); items 7–9 run every time, and the Done when follows.
- **F5, naming**: the rules guide is renamed `rules/references/manual-setup.md`; setup's reference cites it as "the `rules` skill's `references/manual-setup.md`".
- **F5, Offboard**: setup's step 1 keeps only the block and points to the guide's step 1 for the transfer, `calendar_owner` and the check; the guide now names rules → Replace for the `org.md` write.
- manual-test.md → M2: section 6 gains the weekly and 1st-of-month checks; section 7's transfer check follows the guide.

### For F12 (M3)

- Review deviations, both to `agora-spec.md`: email time from the Date header converted to the root's timezone for names (replaces spec l.198); diary ids resolved by id first, then search (replaces spec l.200).
- Spec drift to carry: the sent-mail filter (widens l.107 and l.199) and its parenthesised forms; ledger Date to the minute in shared services mode; the new reference files (`setup/references/shared-services.md`, `rules/references/manual-setup.md`); the overdue-period definition above (spec l.211–214); the 60-minute Tidied window; the duplicate key and Mailbox semantics (key only across another mailbox); the joiner's calendar-invitation step (review advisory 5).

## M2 revision 2

Fixes for review "M2 - A team on it — round 2". English plugin only; skill prose, so no `tdd` cycle; the hand test is updated.

- **F9, new root** (MUST): item 8 flags no missing file while `reviews/` is empty, and otherwise leaves out every period whose first day is on or before the day the oldest file in `reviews/` was created. It uses the created time because weekly and monthly names don't show when they were written. Walk-through for a root set up on Wed 2026-11-11 afternoon:
  - Thu 07:30 daily: `reviews/` is empty, so no flag.
  - Fri: the oldest file was created Thu, so Thu, W46 and November are left out.
  - Mon 16: Sun 15 is checked and passes. W46 is still left out.
  - Tue 17: W47 is checked, so a missed Sunday weekly is flagged.
  - Dec 1: November is left out. Dec 2: a missed December monthly is flagged.
  - Fri 13 missed: flagged Sat. Only the very first daily can't be flagged.
- **F9, own-file clause**: kept, with the reason given in one clause: an attended weekly or monthly run in the middle of its period writes the very file item 8 checks.
- **F5**: manual-setup.md Offboard step 1 now says "prerequisites item 3's Calendar check".
- manual-test.md §6: the weekly step runs Tuesday to Saturday, on a root whose oldest review was created before that Monday. The new-root step now covers an empty `reviews/` and the next day.
- For F12: carry the new-root exclusion into `agora-spec.md` along with the overdue-period definition.

## M3 - Moving and documenting

F6, F10, F11 and F12, plus the M2 round 3 carry-overs. Agora and agora-ita are now 0.7.0.

### What changed

- **F6 Move to shared drive** (`setup/references/move.md`, new; `setup/SKILL.md` step 4b, the branch table, the description; `templates.md` → the `## Move to shared drive` section). Ten boxes, resumable:
  1. the admin's setup;
  2. hand-over as the assistant account: the Drive connector shares the root with the owner's Workspace account, and the calendar share is clicks;
  3. the owner reconnects as their Workspace account;
  4. the root goes into the shared drive: `landing/` is filed first, then move (when the connected account owns every file) or rebuild-and-copy, with a per-folder count check;
  5. `org.md` and `setup.md` switch to shared services mode;
  6. future events are copied, and the old calendar is left alone;
  7. forwarding by clicks, and the old address goes into the mover's Mail from;
  8. the old root is trashed by the assistant account (clicks);
  9. the old account is retired but kept alive for forwarding;
  10. the linked-repo list.

  rules → Replace and prerequisites item 3 now point to the branch.
- **F10 help**:
  - the tour has a line per mode;
  - the FAQ has per-mode Basic / Shared services pairs (where are my files, who sees what, phone upload, email, add a person, leave, cost) and three new answers (which kind of Agora, "not a member yet", moving to a shared drive);
  - Troubleshoot reads the review's overdue and connected-account flags first, and maps the shared services failures to their items (account mismatch, not a Content manager, calendar refused, group search empty).
- **F11 Italian**: the whole English diff since 004ce41 is carried into `cruna-plugin-ita/agora`. Three translator subagents worked from the glossary; I checked line counts per file (all equal to English) and spot-read the result. The glossary gains:
  - the mode terms;
  - every new identifier, kept in English;
  - the Italian Access values (`gestore contenuti`, `può modificare`, `riceve la posta`, `libero/occupato del dominio`);
  - the new pointer headings and the Google UI labels.

  The calendar permission is now the imperative "Apporta modifiche agli eventi" everywhere, as in the existing files.
- **F12**:
  - `agora-spec.md` is rewritten around the modes. Its new sections:
    - Modes, accounts and calendar (The three ids, the ownership check, identity, the speaker's own calendar as the wider rule);
    - Manual setup for admin, per new member, joiner (calendar-invitation step included) and leaving, with the Group settings (Mailing / Restricted → Custom with External);
    - Move to shared drive;
    - Email → Mailboxes (duplicate key from another mailbox, Date-header time, ids first then search, Date-to-the-minute rows, why not `Message-ID`);
    - Routines (60-minute Tidied, the overdue period with the new-root exclusion and the created-time reason, the accepted first-monthly blind spot, item 9);
    - the prerequisites per mode, the new reference files, verified platform facts (ChatGPT `deliveredto:`/`list:`, writes by calendar id, the two drive ids), the still-to-verify list, the release checks (v0.7.0 test and the founder's basic test with a sent-mail search), and Out of scope.
  - The v0.7.0 manual test has 12 scenarios, and section 12 is the founder's non-regression test.
  - Both READMEs name the modes and link the spec's Manual setup.
  - 0.7.0 is set in four manifests and the Claude catalog (the OpenAI catalog carries no version).
- **M2 carry-overs**:
  - review item 8's first bullet is now a sub-list;
  - the created time has its reason;
  - the wording is now "A new root has nothing due before its first review file";
  - manual-test §6 step 1 now runs "on a root older than two days".

  All are in English and Italian.

Checker: **✔ Valid for Claude and OpenAI**.

### Testing

Not testable by code: everything is skill prose and docs, and no `tdd` cycle was possible. The hand tests are in [manual-test.md](manual-test.md) → M3 and `docs/agora/manual-tests/manual-test-agora-v070-shared-services.md`.

### Decisions taken where the spec was silent

- **Retired address trusted through Mail from**: the old address goes in the Mail from cell of the member who ran the move, rather than in Notes or Conventions. Existing member-mail rules then apply unchanged, and the filed author is that member.
- **Old root trashed by clicks**: the Workspace account can't trash files the assistant account owns in My Drive.
- **Hand-over step**: the Workspace account needs read access to the old root and calendar, so the assistant account shares them before the reconnect.
- **`landing/` filed before a copy**: a copy resets Drive creation times, which name documents. After a copy, the overdue check's oldest-file date restarts at the move, which can only hide flags.
- **The old account is kept alive**: forwarding needs it.
- **Members stay away from step 4 to step 5**: during that window two `org.md` files exist.
- **The Manual setup lives in `agora-spec.md`**: it is a spec section, linked from both READMEs, rather than a separate doc.

### Open / unverified

- The Italian Google UI labels (Admin console, Groups, Calendar, Gmail forwarding) were written from knowledge, not checked against the live Italian UI.
- `update_file` re-parenting into a shared drive, the copy path at scale, and forwarding into a group are untested live.
- The live checks still open from M2 (`deliveredto:` vs `list:` for group mail, the sent-mail forms, `owners` metadata) are in the spec and in the manual test's FACT list.

## M3 revision 1

Fixes for review M3 round 1, in English then Italian; agora and agora-ita stay at 0.7.0.

- **F7, mail filter (MUST)**: shared services mode now searches the founder's form from #11 (2026-11-06), `(list:<list-id> OR to:<inbox> OR cc:<inbox>)`, where `<list-id>` is `inbox` with `@` replaced by `.`. It is defined once, in rules → The three ids → Mail. The same form is the sent-mail form, and the received-mail form adds `-from:me`. Prerequisites item 7's check uses the received-mail form, and Join, Verify and Move step 7 reach it through item 7. Also updated: email.md, shared-services.md, agora-spec (now a verified fact, removed from still-to-verify), and both manual tests. Basic mode keeps `deliveredto:<inbox>` and `(deliveredto:<inbox> OR from:<inbox>)`. The old `(or list:<inbox> where the connector needs it)` fallback is dropped, since it used the address rather than the list-id. The remaining `deliveredto` occurrences are all basic mode.
- **F5, Offboard**: when the leaving member's Mail from cell holds the retired assistant address, Offboard step 5 moves it to another member's cell, chosen by the owner. The step is shared by both modes, and the Done whens check it.
- **Advisories**: setup §4b now gives a one-line purpose and points to move.md. agora-spec's forwarding click path now points to move.md step 7. In Italian move.md step 1, `Members` is in backticks.
- Testing: only skill prose and docs changed, so there was no `tdd` cycle. The hand tests gained an own-post `to:` scenario and an Offboard-the-mover check.

## M3 revision 2

Fixes for review M3 round 2, in English then Italian; agora and agora-ita stay at 0.7.0.

- **F7, mail filter (MUST)**: `-from:me` is gone everywhere. In shared services mode the mail filter is exactly the founder's `(list:<list-id> OR to:<inbox> OR cc:<inbox>)`, one form for received and sent mail, so a member's forward or CC to the group is filed by every member's triage. Agora's own sent mail stays out of triage through its ledger row (id and duplicate key). The tidy-up line in email.md and agora-spec says so.
- **Advisory 1**: prerequisites item 7 and both manual tests point to rules → The three ids → Mail for the list-id instead of restating the `@`→`.` derivation; agora-spec keeps one statement.
- **Advisory 2**: glossary row 1 split into "list-id" and "what the member sent to the group".
- **Advisory 3**: agora-spec and the "Did we answer Mario?" row note that sent mail is found only with the group in To or Cc; Agora always CCs the group, so only hand-sent mail without it is missed.
- Testing: skill prose and docs only, so no `tdd` cycle.
