# Setup in shared services mode

What each setup step does when the root is in shared services mode (`mode: shared-services` in `org.md`, rules → Vocabulary → Mode), or when New root chose it. Each section below adds to the step of the same number in SKILL.md, or replaces the steps it names. Nothing here shares a folder or a calendar: in this mode sharing is membership, done by hand by the admin and the calendar owner, and Agora only checks it. The manual steps are the `rules` skill's `references/manual-setup.md`; "item 4, step N" means its step N.

## 2a. Prerequisites (New root)

In place of SKILL.md's basic mode steps.

1. Tell the user, in two plain sentences, what happens by hand: their Workspace admin creates the shared drive, the group and the shared calendar and adds each member, following steps you give; you check each one with a real call, and Agora never shares anything itself.
2. Walk item 1, item 2, then item 4 (Once per root). Then item 3 (shared services): its Who check gives the user's Workspace account, which becomes their Email in Members; its Drive and Calendar checks are steps 3 and 4, once you hold the ids.
3. **Shared drive.** Ask for the shared drive's URL (item 4, step 2). Take the id from it yourself: the path segment after `/folders/`, without any `?…` query. Check it with a real call: create a test file in that folder, then trash it. Not found or refused: the user isn't a Content manager of that drive, or the URL is another folder's; walk item 4, step 2 again.
4. **Shared calendar.** Ask for the Calendar ID (item 4, step 4) and who created the calendar (their slug, once the Members round settles it, is `calendar_owner`). Check it with a real call: create a test event on that id, then delete it. Agora never creates this calendar.
5. **Group.** Ask for the group's address (item 4, step 3).

Hold the drive id, the Calendar ID, the calendar's creator and the group address for the root round: they go in `setup.md` → Interview → The root, and later in `org.md` as `drive_id`, `calendar`, `calendar_owner` and `inbox`.

Done when items 1–4 are each ready, the drive's create-then-trash and the calendar's create-then-delete checks passed, and you hold the four values above.

## 2b. Interview

- The root folder is created inside the shared drive, with `drive_id` as its parent; its id becomes `root_id`.
- Every Member section gets the template's shared services boxes (rules → File kinds → templates). The owner's Workspace account, In the shared drive and Shared calendar are ticked too, since step 2a's checks passed in their app. Every other member's admin boxes stay unticked, each naming who does it (item 4, Per new member).

## 2c. Build

- Item 2, `org.md`: the front-matter also has `mode: shared-services`, `drive_id`, `root_id`, `calendar` (the Calendar ID), `calendar_owner` and `inbox` (the group's address), and no `assistant_account`.
- Item 7, system calendar: the Calendar ID from step 2a, created by hand and never by Agora. Confirm in the calendar list that its timezone is the one in `org.md`; a different one is a click fix for the calendar owner: **Settings** → the calendar → **Time zone**.

## 2e. Sharing

In place of SKILL.md's steps 1–3. Item 5 is checks only, and each member's checks run in their own app. The owner's passed in step 2a: write `content manager` and `can edit` in their Access row. Every other member's admin boxes stay unticked, naming who does each, until they run Join.

Done when the owner's Access row shows what their checks proved, and every other member's admin boxes name who does them.

## 3. Join

In place of SKILL.md's Join steps 1–6. Run it in the joining member's own app, signed in as their Workspace account. Join shares nothing and creates no interest folder, in any layout.

1. **Prerequisites and who.** Walk items 1 and 2. List calendars: the primary calendar's id is the connected account (item 3, Who). Find the root (rules → Finding the root). None found: either the connectors are signed in as another account (item 3), or the admin hasn't added this account to the shared drive yet (item 4, step 2, the admin): say both, with who does each, and stop here.
2. **Which join.** Read `org.md` and match the connected account against Members Email (rules → Identity):
   - no match: a **new member**;
   - a match whose Member section in `setup.md` has unticked boxes: a member named at New root, joining now; skip the interview questions their row already answers;
   - a match whose section is all ticked: an **existing member on a new app**.
   A new member gets a Member section now, with the shared services boxes (rules → File kinds → templates), all unticked but Connectors and plugin; an existing member on a new app gets Connectors and plugin ticked and Routines unticked (and Email, when email is on).
3. **Check the admin's work.** Run every check below before reporting, so the member hears every missing step at once. Tick each box whose check passes; leave each failing one unticked, naming the step and who does it ("In the shared drive (admin: item 4, step 2)").
   - **Workspace account**: the connected account is on the domain of `inbox` (the same Workspace). Not: they signed in with a personal account; reconnect as their Workspace account (item 3), or the admin creates one (item 4, step 1).
   - **In the shared drive**: create a test file under `root_id`, then trash it (item 3, Drive). Not found: not in the drive; trash refused: not a Content manager. Either way item 4, step 2, the admin.
   - **Shared calendar**: create a test event on `calendar`, then delete it (item 3, Calendar). Failed: item 4, step 4, the calendar owner (`calendar_owner`, by name).
   - **In the group** (email on; email off: "(email off)"): item 7's shared services check. Failed: item 4, step 3.3, the admin, or the member's own Gmail connection.
   - A connector missing from the member's list: item 4, step 5, the AI client's admin.
   In the shared drive failing blocks the rest of Join: nothing can be written to the root. Any other failure is recorded and Join carries on.
4. **New member only**: interview them with the Members round of [interview.md](interview.md), just for themselves. Their Email is the connected account: say it, never ask it. Write once (rules → Replace):
   - their Members row in `org.md`, with Kind when the table has the column and Personal calendar `domain free/busy` unless they choose more;
   - their Access row, recording what step 3 proved: Drive `content manager`, System calendar `can edit` (else `pending`), Client connected, and `receives mail` in Notes when In the group passed;
   - `memories/<slug>.md`;
   - In Members and Access, with a memories file, ticked.
5. **Existing member, new app**: update their Access row ("Client connected").
6. **Email on**: run the member part of [email.md](email.md) for them.
7. Give them their routines from [routines.md](routines.md) and walk item 6.

Done when the person is in Members and Access with what their checks proved, they have a memories file, item 6 is ready or blocked, (email on) their Email box is ticked or names what is waiting, and every box in their Member section is ticked or names the step and who does it. Go to SKILL.md step 7.

## 6. Offboard

In place of SKILL.md's Offboard steps 1–4, confirming each step with the owner (the admin, for a team) before you do it or hand it over:

1. **Calendar owner first.** When the member leaving is `calendar_owner`, Offboard is **blocked**: remove nothing and write nothing else until the transfer is done and checked as the `rules` skill's `references/manual-setup.md` → A member leaving, step 1, says, `calendar_owner` included. A request to skip the transfer gets the reason (the calendar is deleted with its owner's account) and the block stays.
2. **Remove them** from the shared drive, the group and the calendar's sharing, then suspend or delete their Workspace account, and ask them to disconnect their connectors and delete their routines: steps 2–4 of A member leaving, each by the person it names. Where a call can read the result (the shared drive's members, through the Drive connector's permissions), check it; otherwise the person who did it confirms.

Then SKILL.md's steps 5 and 6.

Done when `calendar_owner` names a current member other than the one leaving, each removal is checked or confirmed by the person who did it, the Members and Access rows in `org.md` show them removed, the retired assistant address (if any) is in a current member's Mail from cell, and `setup.md` has no section for them. Go to SKILL.md step 7.

## 7. Verify

In place of SKILL.md's basic mode lines:

- `root_id` is the folder holding `org.md`, and walking its parents reaches `drive_id` (rules → Working with Drive).
- A test file created under `root_id` is trashed, and a test event created on `calendar` is deleted, both by real calls.
- Every member with an Email has an Access row, and `calendar_owner` is a current member (in Members, not removed).
- In the email line, "the mailbox is the assistant account's" becomes item 7's shared services check: a search with the group's mail filter returns results.
