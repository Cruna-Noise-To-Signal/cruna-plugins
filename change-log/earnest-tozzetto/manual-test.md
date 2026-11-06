# earnest-tozzetto — hand-test manual

## M1

A shared services root built by setup, identity by account, calendar by id, and the basic ownership check. English plugin only, from this branch. Agora's version is still 0.6.0 in the manifests (the bump is M3).

### Before you start

- Claude (or ChatGPT) with the `agora` plugin installed from this branch.
- Two Workspace accounts on the `cruna-shared-resources` domain: **A** (you, admin) and **B** (not used until step 6).
- The shared drive `cruna-shared-resources`, with A as Manager or Content manager.
- A test Google Group (e.g. `agora-test@…`) with A in it at **Each email**, created as prerequisites item 4 step 3 says: label Mailing, access type Restricted with Who can post → External ticked (so it reads Custom), Who can join Only invited users; and the Admin console's **Group owners can allow incoming email from outside the organization** ticked.
- A secondary calendar "Agora test" created by A, with its Calendar ID copied.
- A copy of an existing basic root (0.6.0, email on), untouched, for steps 7–8.
- In Claude: Google Drive, Google Calendar and Gmail connected as **A**.

After each step, check Drive, Calendar and Gmail yourself.

### 1. New root: the mode question

New chat: "set up Agora".

- [ ] The first prerequisites question asks whether the group uses Google Workspace.
- [ ] (Optional, separate chat) Answer **no**: the flow is today's: "Will anyone else use this with you?", the assistant account, and so on. Stop there.
- [ ] Answer **yes**: two plain sentences say the admin does the clicks and Agora only checks. It walks item 1, item 2, then item 4 (admin's setup), with click paths for Workspace accounts, the shared drive (Content manager), the group (label Mailing; Restricted, then Who can post → External, which turns it Custom; Who can join Only invited users; Each email; the Admin console setting) and the calendar (Make changes to events, Calendar ID), and connector enablement.

### 2. Ids and real-call checks

- [ ] Agora reads your account from the calendar list without asking who you are.
- [ ] Paste the shared drive's URL, with a `?usp=…` query on it. Agora takes the id itself (the segment after `/folders/`) and never asks you for an id.
- [ ] Drive: a test file appears in the drive's top folder and goes to the drive's trash (check Trash in the shared drive).
- [ ] Paste a wrong URL (a folder you can't reach): Agora says you aren't a Content manager or the URL is another folder's, and points to item 4 step 2.
- [ ] Paste the Calendar ID and say who created it. A test event appears on "Agora test" and is deleted.
- [ ] Give the group address.

### 3. Interview and build

Run the interview: two members (you and B, B with Kind `ai`), email **on**, a small layout.

- [ ] Members round asks for the Workspace account (not a personal one), Personal calendar with `domain free/busy` recommended, and Kind.
- [ ] The root folder is created **inside the shared drive**, not in My Drive.
- [ ] `org.md` front-matter: `mode: shared-services`, `drive_id` (the URL id), `root_id` (the root folder's id: compare with its URL), `calendar` (the Calendar ID), `calendar_owner` (your slug), `inbox` (the group), and **no** `assistant_account`.
- [ ] Members has a Kind column (`human` for you, `ai` for B) and Email = Workspace accounts.
- [ ] `setup.md`: Member sections have the shared services boxes (Workspace account, In the shared drive, In the group, Shared calendar, …). Yours has Workspace account, In the shared drive and Shared calendar ticked; B's admin boxes name who does them.
- [ ] Nothing is shared: the root folder's sharing shows no individual editor added by Agora.

### 4. Email, sharing, verify

- [ ] Email step: Agora asks about the mail sent to the group address. Gmail check searches `deliveredto:<group>`. With no mail yet, it asks you to send a test email to the group, then searches again and passes. Your In the group box is ticked, and Access Notes say `receives mail`.
- [ ] Send one more email to the group from a personal address outside the organisation: it reaches A's inbox, not a moderation queue.
- [ ] Sharing step: no folder or calendar sharing. Your Access row says `content manager` and `can edit`.
- [ ] Verify passes. It creates and trashes a test file under the root, creates and deletes a test event on the calendar, finds group mail, checks Access rows, and checks `calendar_owner` is a member.
- [ ] Set `calendar_owner` to a slug not in Members (edit `org.md` by hand), say "finish setup" or "verify Agora": Verify fails on it. Put it back.

### 5. Identity and calendar

New chat, still as A.

- [ ] "What do we have this week?" Agora doesn't ask who you are.
- [ ] "Remind us to call the accountant next Tuesday at 10" in an interest from the build. The event lands on **Agora test**, not on A's primary calendar, with the `[interest/instance]` title. The diary entry and statuses update.
- [ ] "Move it to 11": it finds the event on Agora test and moves it.
- [ ] Clash check: put a busy block on A's own calendar on Tuesday at 10. Scheduling then proposes another slot.
- [ ] With that block still there, "What do we have on Tuesday?" and "my day" on Tuesday list A's own event, though A's Personal calendar is `domain free/busy`.

### 6. An account that isn't a member

Connect Claude as **B**, or as any Workspace account not in Members, in a separate client or profile.

- [ ] "What's pending?": Agora says this account isn't an Agora member yet and offers to join. Nothing is written: check `org.md`, diary and `status.md` modified times.

### 7. Basic root: unchanged

On the untouched copy of a 0.6.0 basic root, connected as its assistant account:

- [ ] "Verify Agora" or "finish setup" → Verify passes as before, plus the ownership line.
- [ ] `schedule`, `ask` and a daily `review` with email on behave as on 0.6.0: events on the assistant account's primary calendar, the same email filed and flagged. Any email search that used to find mail and now doesn't is a bug (the `deliveredto:` filter).
- [ ] A search for sent mail: "did we answer <someone the assistant emailed last week>?" finds the sent message, as on 0.6.0.
- [ ] Nothing is added to `org.md` (no `mode`, no ids).
- [ ] Its Members table has no Kind column. Send `Agora add-interest as pm in <root>: products/test — Test: a test` from a chat: the github row it writes has the same columns as the table's header, with no Kind cell.

### 8. Basic root: account mismatch

Same basic copy, now with Google Drive connected as a **different** account that can edit the root but doesn't own `org.md`.

- [ ] Verify is blocked on item 3, saying the connected account doesn't own `org.md`. It gives the fixes: reconnect as the assistant account, have the owner transfer the files, or move to a shared drive.
- [ ] "Note that the car was serviced" in any instance. When Replace's trash is refused, Agora says the cause is the account mismatch, not a generic error. Afterwards, reconnect as the owner and let `review` clean up the duplicate.

## M2

A team on the shared services root from M1: a second account joins, mail across two mailboxes, the tidy-up skip and overdue flags, and offboarding the calendar owner. English plugin only, from this branch; version still 0.6.0.

### Before you start

- The shared services root built in M1, email **on**, with A as `calendar_owner`.
- Account **B** (Workspace, Kind `ai` in Members from the M1 interview, or not yet in Members: run 1 once each way if you can).
- A second client or browser profile for B, with the `agora` plugin from this branch.
- A personal address outside the organisation, for test emails.
- The untouched 0.6.0 basic root copy from M1.

After each step, check Drive, Calendar, Gmail and the files named.

### 1. Join: every admin step missing in turn

Before the admin adds B anywhere, connect B's Drive, Calendar and Gmail as B and say "join Agora".

- [ ] No root found: Agora says B's connectors may be on another account, or the admin hasn't added B to the shared drive yet (item 4, step 2, the admin), and stops. Nothing written anywhere.
- [ ] Admin adds B to the shared drive as **Viewer** only. "join Agora" again: In the shared drive fails on the trash (not a Content manager), names item 4 step 2 and the admin, and Join writes nothing.
- [ ] Admin makes B **Content manager**; the calendar isn't shared with B yet and B isn't in the group. "join Agora": Agora lists **both** missing steps at once: Shared calendar (item 4, step 4, the calendar owner by name) and In the group (item 4, step 3.3, the admin). It carries on with the rest.
- [ ] Nothing was shared by Agora: the root folder and calendar sharing show no change made by B's AI.

### 2. Join: completing

Calendar owner shares the calendar with B at Make changes to events; admin adds B to the group at Each email. B says "finish setup".

- [ ] Agora doesn't ask for B's email: it says the connected account.
- [ ] New member: the Members round asks Kind and recommends `domain free/busy`. B's Members row has Kind, Email = B's Workspace account; Access row: `content manager`, `can edit`, Client connected, `receives mail` in Notes once the group search passes.
- [ ] Item 7 with no group mail yet in B's inbox: Agora asks B to send the test from an address **outside the organisation**, then finds it.
- [ ] `memories/<b>.md` exists; no interest folder was created for B, even in a per-person layout.
- [ ] B's `setup.md` section: Workspace account, In the shared drive, In the group, Shared calendar, Connectors and plugin, In Members and Access, Email, Routines all ticked (Routines after B confirms them). B is given the three routines with unchanged prompts.
- [ ] B in a second app ("I installed Agora on ChatGPT too"): existing member on a new app; only Client connected changes.

### 3. Replace across members

- [ ] As B: "Note that the accountant called" in an instance A created. The instance `status.md` (created by A) is replaced: no duplicate left, no permission error.

### 4. Email across two mailboxes

- [ ] From the outside address, send a mail to the group; A forwards it to the group with "file under <instance>". As A: "file the email I forwarded". It's filed; `inbox.md` row has Mailbox = A's slug and a Date with the minute.
- [ ] As B: "anything from <outside sender>?" Agora finds B's copy and says it's already filed, with the instance. Nothing is filed twice.
- [ ] As B: open the diary entry's email ("show me that email"): it's found by search in B's mailbox although the id is A's.
- [ ] Run B's daily routine by hand: the tidy-up doesn't add a second row or diary entry for that email.
- [ ] As A: "draft a reply to <outside sender> saying we'll call Monday", then "send it". The draft is in A's Gmail, from A's address, the group in **Cc**, written in A's own voice with no "assistant, on behalf of" line. The sent row has Mailbox = A.
- [ ] As B, after the send: "did we answer <outside sender>?" finds A's reply (the copy reached B through the group). As A, the same question finds it in A's Sent (the sent-mail form of the filter).
- [ ] Reply from the outside address: it lands in the group, so in both A's and B's inbox.

### 5. Two routines within 60 minutes

- [ ] Run A's daily routine by hand (`Run Agora review: daily for <a> in <root>`). The root `status.md` gets `**Tidied:** <now>`.
- [ ] Within 60 minutes run B's daily. It skips the tidy-up: `inbox.md` and `last_tidy_up` unchanged, no stale-file rebuilds, the Tidied line unchanged. The review file gets an Update section only if something new happened, and B gets the message.
- [ ] Delete the Tidied line by hand (replace `status.md`) and run again: the tidy-up runs in full.

### 6. Overdue flags

- [ ] On a root older than two days, trash yesterday's daily review file and set `last_tidy_up` in `inbox.md` to three days ago. Ask "daily review" in a chat. Needs attention shows the missing daily review and "Email wasn't triaged between …", each with "No member's routine has run this: set up the routines".
- [ ] On a Tuesday to Saturday, on a root whose oldest review file was created before this week's Monday: trash the weekly review named after the current ISO week and ask "daily review" again: the missing weekly review is flagged. Ask "weekly review" instead: it writes that week's file and raises no flag for it.
- [ ] On the 1st, before the monthly routine has run, a daily review raises no missing-monthly flag.
- [ ] On a new root with `reviews/` still empty, a daily review raises no missing-period flag. Run it again the next day: the oldest file was created yesterday, so still no missing-period flag.

### 7. Offboarding the calendar owner

- [ ] As A (calendar owner): "remove <a> from Agora". Agora says it's blocked until the calendar is transferred, gives the transfer steps, and writes nothing: `org.md`, `setup.md` and sharing unchanged.
- [ ] Insist ("just remove me"): it still refuses, explaining the calendar would be deleted with A's account.
- [ ] A transfers the calendar to B; B accepts. "remove <a> from Agora" again: `calendar_owner` becomes B's slug, B confirms the calendar is under their "My calendars" and the Calendar check (test event created and deleted) passes, then the removal steps (shared drive, group, calendar, Workspace account) are each handed to the person they name. Members Notes `removed YYYY-MM-DD`, Access `removed`, A's `setup.md` section gone. Verify passes with `calendar_owner` = B.

### 8. Basic root: unchanged

On the untouched 0.6.0 basic root, as its assistant account:

- [ ] "Did we answer <someone the assistant emailed>?" finds the sent mail (the basic sent-mail form).
- [ ] A daily review with email on: same mail filed and flagged as on 0.6.0; `inbox.md` rows written with an empty Mailbox and a date only.
- [ ] Join a person as on 0.6.0: the root folder is shared by Agora, calendar click steps given, no Workspace questions.
- [ ] Offboard that person as on 0.6.0.
- [ ] Two reviews within an hour on the basic root: the second one skips the tidy-up once the first wrote `**Tidied:**`.
- [ ] With Drive connected as an account that doesn't own `org.md`: the daily review flags the mismatch with the fix.

## M3

The Move branch, help in both modes and both languages, and the release docs. Install `agora` and `agora-ita` 0.7.0 from this branch. The full release test is `docs/agora/manual-tests/manual-test-agora-v070-shared-services.md`; this is the short pass for M3.

### Before you start

- A **copy** of a basic root (0.6.0, email on) with a few diary entries, documents, a future `[interest/instance]` event, a recurring one, and, if you have one, a `products/` instance managed by `cruna-pm-github`.
- The M1/M2 setup: `cruna-shared-resources`, Workspace accounts A and B, a test group and a shared calendar created by A.
- Access to the copy's assistant account (Drive, Calendar and Gmail), and an outside address.

### 1. Move to shared drive

- [ ] Connected as the copy's assistant account, say "move Agora to a shared drive". `setup.md` gets `## Move to shared drive` with ten boxes, and step 1 lists the admin's steps.
- [ ] Step 2: the root folder's permissions list A's Workspace account as editor, and you get the click steps to share the calendar with A. Stop and say "finish setup" later: the move resumes at step 3.
- [ ] Step 3: reconnect as A. The Who, Drive and Calendar checks pass, and the old calendar shows in A's calendar list.
- [ ] Step 4: `landing/` is filed first. Note whether it moved or copied. A copy lists every folder in both trees, and no original is trashed.
- [ ] Step 5: in the new root, `org.md` has `mode: shared-services`, `drive_id`, `root_id` (the new folder, which holds `org.md`), `calendar`, `calendar_owner` and `inbox`, and `assistant_account` is still there. Members Email are Workspace accounts, and `setup.md` has the shared services boxes.
- [ ] Step 6: each future event, and the recurring series once, is on the new calendar with the same title, time, description and colour. The old calendar is unchanged.
- [ ] Step 7: forwarding is set by clicks, and the code arrives in the group. A mail from outside to the old address shows in the group search, and the old address is in A's Mail from.
- [ ] Step 8: the old root is trashed by the assistant account, and searching `org.md` finds the new root only. All diary entries and documents are there.
- [ ] Steps 9–10: the old account is said to stay, unused, and the linked-repo list names each managed `products/` instance (or "none").
- [ ] Verify passes in shared services mode. As B, "join Agora" matches B's Member section.

### 2. Help in both modes

- [ ] On the shared services root, "where are my files?" says **Shared drives**, and on the basic root it says **Shared with me**, each with no word about the other mode.
- [ ] "What kinds of Agora are there?" with no root answers both modes.
- [ ] As a non-member account on the shared services root, "Agora says I'm not a member" gives the reconnect or Join answer.
- [ ] On a root with a missing yesterday's daily (older than two days), "my review never arrived" shows the overdue flag first.
- [ ] On the basic root with Drive connected as a non-owner, "duplicate files won't go away" runs item 3 and names the mismatch.

### 3. Italian

- [ ] With `agora-ita`, `aiuto`: "dove sono i miei file?" answers for the root's mode, using "Drive condiviso".
- [ ] With `agora-ita` on the shared services root, "unisciti ad Agora" as B runs Join in Italian. `org.md` keys, `mode`, Kind, Mailbox and the `setup.md` box labels stay in English, and Access values read `gestore contenuti` / `può modificare`.
- [ ] Read `cruna-plugin-ita/agora/skills/rules/references/manual-setup.md` against the Italian Google Admin, Groups and Calendar screens, and note any label that differs.

### 4. Release docs

- [ ] `docs/agora/agora-spec.md` → Manual setup matches the plugin's `rules/references/manual-setup.md`.
- [ ] The v0.7.0 manual test's section 12 (the founder's basic non-regression, with the sent-mail search) is ready to run.
- [ ] `python3 <cruna-core>/skills/portable-plugins/scripts/check_plugins.py .` prints "Valid for Claude and OpenAI", and both manifests in both languages and the Claude catalog say 0.7.0.
