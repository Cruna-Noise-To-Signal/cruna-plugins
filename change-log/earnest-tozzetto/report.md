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
