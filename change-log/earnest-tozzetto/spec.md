Request: #11 (earnest-tozzetto)

## Problem Statement

Agora keeps every root in one Google account's My Drive, and every member's AI client signs in as that account. In My Drive only a file's owner can trash it. So when someone connects with a different account, Replace (duplicate, edit, trash) breaks and leaves duplicates behind. This happened on 2026-11-06: the test root `Test-Drive-Agora` was owned by one account while the connector was signed in as another. Every `trash_file` returned "The caller does not have permission", and nothing in setup, `help` or `review` caught the mismatch.

For a Workspace team, signing everyone in as one shared account is also the wrong model:
- it means a shared password;
- the AI reads one inbox that nobody actually works in;
- Agora can't tell who is talking without asking.

Teams want each person to use their own account and still see the same Agora.

Families on free Gmail can't use shared drives, so the one-account setup has to stay exactly as it is.

## Solution

Agora gets two **modes**, recorded as `mode` in `org.md`:

- **Basic shared account mode** (`basic-shared-account`): today's setup. Several people sign in to one Google account. This is the default whenever `mode` is missing, so every existing root keeps working with no change to its files.
- **Shared services mode** (`shared-services`), for Google Workspace. Every member signs in as their own account, and everyone reaches the same three shared services:
  - the root folder inside a **shared drive**, where every member is a Content manager and can trash anything;
  - a **shared calendar**;
  - a **Google Group** address as the inbox.

**Everyone is a user.** A member is a Google account with an AI client attached. Whether it's a person or an AI is recorded for the organisation's own reasons and changes nothing.

**The rule:** the same question gets the same answer from any connected account.

From the members' side:
- An admin creates the shared drive, the group and the calendar, and adds each person to them. A new member connects Drive, Calendar and Gmail as themselves, says "join Agora", and is checked and added without anyone sharing a password.
- Agora knows who is talking from the connected account. Mail forwarded or CC'd to the group can be asked about from anyone's AI. Events land on the one shared calendar.
- An existing root can move to shared services mode through a new setup branch, without losing diary entries, documents or future events.
- A basic root whose connector isn't signed in as the owner of `org.md` is flagged before a Replace fails.

## User Stories

1. As a Workspace team admin, I want setup to ask whether we use Google Workspace, so that a team gets shared services mode and a family keeps the basic shared account mode.
2. As a Workspace team admin, I want click-by-click steps to create the shared drive and add members as Content managers, so that every member can create, edit and trash any Agora file.
3. As a Workspace team admin, I want to paste the shared drive's URL and have setup take the id from it, so that I never look up an id myself.
4. As a Workspace team admin, I want setup to check the shared drive with a real call (create a file, then trash it), so that I know Replace will work before the root is built.
5. As a Workspace team admin, I want click-by-click steps to create a Google Group with external posting and normal delivery, so that mail from clients reaches the group and every member.
6. As a Workspace team admin, I want click-by-click steps to create the shared calendar and share it with every member at Make changes to events, so that everyone sees and edits the same events.
7. As a Workspace team admin, I want setup to record who created the calendar, so that offboarding that person reminds us to transfer it first.
8. As a Workspace team admin, I want setup to check the calendar with a real call (create and delete a test event), so that scheduling is known to work.
9. As a Workspace team admin, I want to be told when the Gmail connector has to be enabled for the organisation (Claude Team or Enterprise, ChatGPT Business), so that members don't get stuck connecting it.
10. As a Workspace team admin, I want a precise list of what I must do before a new member joins (Workspace account, shared drive, group, calendar, connector), so that their sign-in works the first time.
11. As a new member in shared services mode, I want to connect my own Google account and say "join Agora", so that I don't need anyone's password.
12. As a new member, I want my sign-in to check each thing the admin did with a real call, and tell me exactly which one is missing and who fixes it, so that I'm not left guessing.
13. As a new member, I want joining to add me to Members and Access, create my memories file and track my setup boxes, so that Agora knows me from then on.
14. As a member in shared services mode, I want Agora to recognise me from my connected account, so that it never asks who I am.
15. As someone connected with an account that isn't in Agora, I want to be told to run Join, so that I don't write into the root as an unknown person.
16. As a member of a basic root, I want Agora to keep asking who I am, as today, so that nothing changes for my family.
17. As a member, I want every event Agora schedules to go on the root's calendar, whoever's AI created it, so that we all see the same calendar.
18. As a member, I want Agora to read only mail delivered to the group when it handles email, so that my AI doesn't use my personal mail when answering about Agora.
19. As a member, I want "look at what I forwarded you" to give the same answer from any member's AI, so that we work from the same mail.
20. As a member sending mail through Agora in shared services mode, I want it to go from my own address with the group in CC, so that replies reach the whole team.
21. As a member, I want an email handled from another member's mailbox to be recognised as already handled, so that it isn't filed or flagged twice.
22. As a member, I want an email logged from someone else's mailbox to open by search from my own, so that I can still read it.
23. As a member, I want Agora to schedule using my own calendar in full and everyone else's Workspace free/busy, so that meeting times fit without extra sharing.
24. As a member, I want my routines to keep the same one-line prompts, so that nothing in my scheduled tasks has to change.
25. As a member whose routine runs right after someone else's, I want it to skip the tidy-up when one finished in the last hour, so that each run is quicker and cheaper.
26. As a member, I want a review period with no review file by its end, or email not triaged in a day, to show under Needs attention, so that we notice when nobody's routine is running.
27. As the owner of an existing basic root that gets Workspace, I want a "Move to shared drive" setup branch, so that we switch modes without losing diary entries, documents or to-do events.
28. As that owner, I want the move to copy our future Agora events to the new shared calendar and leave the old calendar alone, so that nothing scheduled is lost.
29. As that owner, I want the old account's mail forwarded to the new group, and its address still treated as ours, so that senders using the old address still reach us.
30. As that owner, I want to be told which linked GitHub repos must update `.agora/pm.yml`, so that product syncing keeps working after the move.
31. As a Workspace team admin offboarding a member, I want the exact steps (shared drive, group, calendar, Workspace account), and to be stopped if they own the calendar until it's transferred, so that no access and no calendar is lost.
32. As a member of a basic root, I want Verify, `help` and `review` to flag when my connector isn't signed in as the owner of `org.md`, with the fix, so that a Replace never fails silently.
33. As a member of a basic root, I want a failed trash during Replace to name the account mismatch as its cause, so that I know to reconnect as the right account.
34. As a member asking "where are my files?", I want `help` to answer for my root's mode, so that I look in the shared drive or in "Shared with me" as appropriate.
35. As a member using the Italian plugin, I want every change in Italian with the glossary's terms for the two modes, so that both languages behave the same.
36. As the founder, I want an untouched basic root to give the same results as before for Verify, daily `review` with email on, `schedule`, `email` and `ask`, so that the families' setup is proven unbroken.
37. As a member reading Agora's help and FAQ, I want them to describe both modes and stop saying that every connector signs in as the assistant account, so that the help matches how my root works.

## Implementation Decisions

### Modes and `org.md`
- **New front-matter fields** (all optional, all with a basic-mode default):
  - `mode: basic-shared-account | shared-services`. Missing means `basic-shared-account`.
  - `drive_id`: the id from the shared drive's URL (its top folder), or `my-drive`. Missing means `my-drive`.
  - `root_id`: the root folder's id. When missing, the root is found by searching for `org.md`, as today.
  - `inbox`: the address Agora reads and files mail from. Missing means `assistant_account`.
  - `calendar_owner`: the slug of the member who created the shared calendar. Shared services mode only.
- **Existing fields:**
  - `assistant_account` stays, and is used in basic mode only.
  - `calendar` stays. It holds the account's email in basic mode, which is already the primary calendar's id, and the shared calendar's id in shared services mode.
- **Members** gets a **Kind** column (`human` | `ai`; a missing value means `human`). Kind changes nothing in permissions, routines, reviews or to-dos.
- **Access columns** keep their names. In shared services mode they record what the real-call checks proved: Drive "content manager", System calendar "can edit", and the group as "receives mail" in Notes.
- Members' **Email** column becomes the account that identifies the member in shared services mode. In basic mode it stays the personal account that the folder and calendar are shared with.
- `setup.md` Member sections in shared services mode carry admin-side boxes instead of the sharing boxes, each naming who does it:
  - Workspace account
  - In the shared drive
  - In the group
  - Shared calendar
  - Connectors and plugin
  - In Members and Access, with a memories file
  - Email
  - Routines

  Basic-mode boxes stay as they are.
- Identifiers stay in English in both languages. Glossary terms: "modalità account condiviso base" and "modalità servizi condivisi".

### The three-ids rule (rules skill), both modes
- Every Drive call stays under the root: `root_id` when present, otherwise the root found through rules → Finding the root.
- Every Calendar write and read of Agora events uses `calendarId = calendar`, never the default primary calendar. Personal calendars stay read-only, as today.
- Every Gmail search carries `deliveredto:<inbox>` (or `list:<inbox>` where the connector needs it). Reading or acting on a message that doesn't match is only allowed when a member names that message in the current conversation, which is the same exception as today's "a member asking may file any email".
- The rules state plainly that this filter is Agora's own rule and not a permission boundary: in shared services mode, each member's AI can technically see that member's whole mailbox and calendar.
- Skills other than `setup`, `help` and the identity step don't branch on `mode`.

### Identity
- **Shared services mode:** who is talking is the connected account's email, read from the primary calendar's id in the calendar list. It's matched against the Members Email column. No match: say the account isn't an Agora member yet and offer Join; write nothing else.
- **Basic mode:** unchanged (ask, as today). Unattended runs keep using the routine's slug in both modes.

### Prerequisites checklist (rules → prerequisites)
Items keep their numbers. Each item gains a shared services variant, and the basic text stays as it is except for the ownership check.

- **Item 3 (Connectors).**
  - Basic: the Drive check also confirms that the connected account owns `org.md` (the issue's ownership check). On a mismatch it's **blocked**, with the fix: reconnect as the assistant account, or move the root to a shared drive.
  - Shared services: the connectors sign in as the member's own account. Drive check: create and trash a test file at the root. Calendar check: create and delete a test event on `calendar`.
- **Item 4 (Assistant account):** basic mode only. Shared services mode replaces it with an **admin item**: the Workspace account, the shared drive, the group, the calendar and connector enablement, each with click steps and the check that proves it.
- **Item 5 (Sharing).** In shared services mode, sharing is membership:
  - shared drive membership instead of sharing the root folder;
  - calendar sharing by the calendar owner;
  - personal calendars: domain free/busy by default, more detail optional.

  The Drive connector never shares the root folder in this mode.
- **Item 7 (Gmail).**
  - Shared services: the connector signs in as the member. The check is a search for `deliveredto:<inbox>` that returns results. For a new group with no mail yet, a member sends a test email to the group, then the search is run again.
  - Limits reworded: in basic mode the AI sees only the assistant inbox; in shared services mode it reads only group mail, by Agora's rule.
- **Offboarding** gains the shared services steps (below).

### Manual steps
They must be written precisely: every menu path, every role or setting name, who does it, and the check that proves it. In both languages.
- **Admin, once per root:**
  1. **Workspace account** for each member (Admin console → Directory → Users → Add new user).
  2. **Shared drive:** Drive → New → Shared drive → name → Create, then Manage members → add each member → Content manager → Send. Copy the drive's URL.
  3. **Google Group:** Groups → Create group → name and address → Who can post: Anyone on the web → Who can join: Only invited users → add the members, subscription Each email.
  4. **Shared calendar:** Calendar → Other calendars + → Create new calendar → name and the root's timezone → Create. Then Settings → the calendar → Share with specific people or groups → add each member at Make changes to events. Then Integrate calendar → copy the Calendar ID.
  5. **Gmail connector enablement:** Claude Team or Enterprise, an Owner enables Gmail for the organisation. ChatGPT Business, an admin turns on write actions for Gmail.
- **Admin, per new member, before they join:** Workspace account, shared drive (Content manager), group, calendar sharing by the calendar owner, and the connector enabled.
- **Member, at join:** connect Google Drive, Google Calendar and Gmail as themselves, install the plugin, and say "join Agora".
- **Offboarding** (shared services):
  - remove the member from the shared drive's members, the group and the calendar's sharing;
  - suspend or delete their Workspace account at the admin's choice;
  - ask them to disconnect connectors and delete routines;
  - update Members and Access.

  When the member is `calendar_owner`, the transfer comes first: they or the admin transfer the calendar to another member within the domain, and `calendar_owner` is updated. Only then is the account removed.
- **Move to shared drive, by hand:** in the old Gmail, Settings → Forwarding and POP/IMAP → Add a forwarding address (the group) → confirm with the code. The code arrives in the group, so in every member's inbox. Then Forward a copy of incoming mail, keeping Gmail's copy.
- Menu paths are checked against the current Google and client UIs while implementing. The existing "menu names change" note applies.

### Setup skill
- **Branch choice:**
  - **New root** asks whether the group uses Google Workspace. Yes leads to shared services mode; no to basic mode, unchanged.
  - New branch **Move to shared drive**, offered for a basic root whose members get Workspace.
  - **Join** and **Offboard** pick their steps by `mode`.
  - **Migrate** (solo → dedicated account) stays basic-only.
- **New root (shared services):** the order is:
  1. prerequisites (the admin item and item 3);
  2. the drive's URL (setup extracts the id and checks it with create-then-trash at the drive's top folder);
  3. the interview;
  4. build. The root folder is created inside the shared drive, and `drive_id`, `root_id`, `calendar`, `calendar_owner` and `inbox` are recorded;
  5. email;
  6. sharing (checks only);
  7. routines;
  8. verify.

  The system calendar is the pasted Calendar ID, and is never created by Agora.
- **Join (shared services):**
  - items 1–3 and 7 with the shared services checks;
  - identity by account, and the Members round for themselves, including Kind;
  - Members and Access rows, `memories/<slug>.md`, the Member section in `setup.md`, the email member part, and routines.

  It shares no folder or calendar. It **doesn't create an interest folder**, as today. The founder checks per-person layouts after implementation, and a follow-up changes this only if that check asks for it.
- **Move to shared drive:**
  - The admin's steps first.
  - Then move the root folder into the shared drive (`update_file` with the new parent when the connected account owns every file; otherwise rebuild the folder tree there, copy every file, and trash the originals only after a count check).
  - Copy future `[interest/instance]` events to the new calendar with the same title, time, description and color, and leave the old calendar alone.
  - The forwarding steps above, and add the old address to trusted member mail (below).
  - Update `mode`, `drive_id`, `root_id`, `calendar`, `calendar_owner` and `inbox`. `assistant_account` is kept for the record, and the account is retired: no connector signs in as it, and it isn't a member.
  - List every `products/` instance (linked repo) whose repo must rerun pm-setup to rewrite `.agora/pm.yml` (cruna-development-plugins#29).
  - Progress is tracked as a `## Move to shared drive` section in `setup.md`, like Migration.
- **Verify:** checks per mode. Shared services:
  - create and trash under `root_id`;
  - create and delete on `calendar`;
  - `deliveredto:<inbox>` returns results;
  - every member with Email has Access rows;
  - `calendar_owner` is a current member.

  Basic: today's checks plus the ownership check.

### Email (rules → email references; `email`, `ask`, `review`)
- **Searches:** "mail received by the assistant account" becomes "mail delivered to `inbox`". Mail sent by a member (any Mail from address) is still not filed as received mail.
- **Sending in shared services mode:** from the member's own address with `inbox` in CC, the same confirmation rule as today, and the member's own voice (no "assistant, on behalf of" signature). This is a speccing decision; see Further Notes. Basic mode is unchanged.
- **Ledger:**
  - `inbox.md` gains a **Mailbox** column: the slug whose mailbox the row's Gmail id belongs to. It's empty for basic-mode rows and for rows written before the change, and both mean the assistant inbox.
  - A message counts as already ledgered when its Gmail id matches in the same mailbox, **or** when sender + received time to the minute + subject match a row from any mailbox.
  - Opening a row from another mailbox searches `deliveredto:<inbox> from:<sender>` with subject words, on that day.
  - The diary `email:` key and to-do `email <id>` sources keep the Gmail id, and the diary entry's author's mailbox is where it resolves. A reader in another mailbox falls back to the same search.
- **Member mail:** the retired old address from a move counts as member mail, trusted like a Mail from address. It's recorded in Notes or Conventions on the move.
- **Solo-root rules** (owner's own mail) stay basic-only.

### Calendar (`schedule`, `ask`, `review`)
- Writes go to `calendarId = calendar` in both modes.
- Shared services free time: the asking member's own calendar in full, plus the other members' free/busy read by their email. The Personal calendar column records `domain free/busy` by default.

### Review and routines
- **Tidy-up skip:** the root `status.md` gains a `**Tidied:** YYYY-MM-DD HH:MM` line under Last updated, written when tidy-up step 2 finishes. A run that finds it less than 60 minutes old skips steps 2.1–2.6 and still writes or updates the review and the message. A root `status.md` without the line tidies as today.
  - This refines the "front-matter field" agreed in the grilling: `status.md` has no front-matter, so a line matches the file's style.
- **Overdue flag:** each review run checks the previous period's review file of each kind, and triage, from the `inbox.md` `last_tidy_up` date. It flags under Needs attention:
  - a missing daily review for yesterday;
  - a missing weekly review for last week;
  - a missing monthly review for last month;
  - email on and `last_tidy_up` older than yesterday.

  The fix offered: "no member's routine has run this; set up the routines (setup → routines)". `help` shows the same flags.
- **Basic-mode ownership check:** `review`'s tidy-up flags a connected account that doesn't own `org.md`, as Verify does.

### Replace (rules)
- A trash that fails with a permission error reports its cause:
  - basic mode: the connector account isn't the file's owner, with the fix (reconnect as the assistant account, or move to a shared drive);
  - shared services mode: the member isn't a Content manager of the shared drive.

### Help
- **Tour and FAQ:** both modes. "Where are my files" covers the shared drive (Drive → Shared drives), and "Shared with me" only for basic roots.
- **Troubleshooting** maps failures to the per-mode prerequisite item: account mismatch, not a Content manager, group search empty, calendar write refused.

### Spec and docs
- `docs/agora/agora-spec.md`:
  - "Accounts and calendar" is rewritten around the two modes and the rule;
  - the `org.md` and `setup.md` field lists, the prerequisites summary, Email → Sending/Ledger/Limits, Routines (tidy-up skip, overdue flag) and the release checks are updated.
- `docs/agora/glossary-it.md` gets the mode terms and the new identifiers: `mode` and its values, `drive_id`, `root_id`, `inbox`, `calendar_owner`, the `Kind` column and values, the `Mailbox` column, the `Tidied` line, and the new `setup.md` boxes and section.

### Versions
- Agora goes to **0.7.0** in both languages, both manifests and both catalogs.

## Testing Decisions

- **What a good test is.** A test drives Agora the way a member does, through conversation in the Claude or ChatGPT app against a test root. It checks what ends up on Drive, on the calendar and in the chat, never how a skill got there.
- **The seam.** One seam: the **member conversation**, including the routine's one-line prompt typed by hand. Setup branches, Join, Identity, `schedule`, `email`, `ask` and `review` are all exercised through it. No new seam.
- **Test roots.**
  - **Shared services:** a root on the `cruna-shared-resources` shared drive, with two Workspace accounts as members (one `kind: ai`), a test group and a shared calendar.
  - **Basic:** a copy of an existing basic root with email on, files untouched.
- **New manual test** `docs/agora/manual-tests/manual-test-agora-v070-shared-services.md`, in the shape of the v0.5.0 and v0.6.0 tests (numbered scenarios, Drive, calendar and Gmail checks after each). It covers:
  - new root (shared services) end to end, including the URL id extraction and every real-call check;
  - Join by a second account, including each admin step missing in turn, and the message naming it;
  - an account not in Members, which is pointed to Join;
  - Replace by a member on a file another member created;
  - `schedule` from each account landing on the shared calendar; `ask` giving the same answer from both accounts;
  - email: a forward to the group handled from one account, seen as handled from the other; a send from the member's address with the group CC'd; a reply landing in the group;
  - two routine runs within 60 minutes (the second skips the tidy-up); a missing review period being flagged;
  - offboarding the calendar owner, which is blocked until the transfer;
  - Move to shared drive on a copy of a basic root: files, future events, forwarding, ids, and the linked-repo list;
  - a basic root whose connector isn't signed in as the owner of `org.md`, flagged by Verify, `help` and `review`, and a failed trash naming the mismatch;
  - one Italian root scenario on one platform.
- **Basic-mode non-regression:** a manual acceptance test run by the founder before release. The untouched basic root runs Verify, a daily `review` with email on, `schedule`, `email` and `ask`, with the same results as on 0.6.0. It's listed in the manual test as its own section and in the spec's release checks.
- **Prior art:** `docs/agora/manual-tests/manual-test-agora-v050-todos.md` and `manual-test-agora-v060-products.md`, and the change-log `manual-test.md` that implement-loop writes.
- **Checker:** the cruna-core `portable-plugins` checker passes (both manifests, both catalogs, matching versions).

## Out of Scope

- `cruna-pm-github`'s side: `pm-setup` and `agora-sync` reading `drive_id` and `root_id` from `org.md` and the new `.agora/pm.yml` keys. That's cruna-development-plugins#29. This request only lists the repos to update after a move.
- A separate hourly email-triage routine: file it as its own request.
- A run record with claims (`runs.md`) and keeping every daily review: both withdrawn in the grilling.
- Creating shared drives, groups or calendars, or sharing calendars, through connectors: none of them can, so these stay manual steps.
- Enforcing mailbox privacy beyond Agora's own search filter.
- Join creating the member's interest folder in per-person layouts, pending the founder's post-implementation check.
- Moving a shared services root back to basic mode.
- Shared drives outside Google Workspace, or members from outside the Workspace domain.

## Further Notes

- **Speccing decision, not grilled: sender voice in shared services mode.** Mail goes out from the member's own address, so it is written in the member's own voice with no "assistant, on behalf of" signature. Basic mode keeps today's voice. Revisit in review if the founder disagrees.
- **Speccing decision: the `Tidied` line.** It's a line on the root `status.md` instead of front-matter, because that file has none.
- **Verified by the founder after the grilling (2026-11-06):** ChatGPT's Gmail connector supports `deliveredto:` and `list:` searches, and the AI client can create and delete events on the shared calendar by its id. Add both to the spec's verified platform facts.
- **Still to verify while implementing:** whether the Drive connector's `update_file` can re-parent a My Drive folder into a shared drive. The Move branch falls back to rebuild-and-copy either way.
- Companion request: cruna-development-plugins#29 (`serene-zabaione`) depends on the `drive_id` and `root_id` this request writes.
