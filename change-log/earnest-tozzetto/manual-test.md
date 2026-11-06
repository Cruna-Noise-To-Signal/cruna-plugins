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
- [ ] Nothing is added to `org.md` (no `mode`, no ids).
- [ ] Its Members table has no Kind column. Send `Agora add-interest as pm in <root>: products/test — Test: a test` from a chat: the github row it writes has the same columns as the table's header, with no Kind cell.

### 8. Basic root: account mismatch

Same basic copy, now with Google Drive connected as a **different** account that can edit the root but doesn't own `org.md`.

- [ ] Verify is blocked on item 3, saying the connected account doesn't own `org.md`. It gives the fixes: reconnect as the assistant account, have the owner transfer the files, or move to a shared drive.
- [ ] "Note that the car was serviced" in any instance. When Replace's trash is refused, Agora says the cause is the account mismatch, not a generic error. Afterwards, reconnect as the owner and let `review` clean up the duplicate.
