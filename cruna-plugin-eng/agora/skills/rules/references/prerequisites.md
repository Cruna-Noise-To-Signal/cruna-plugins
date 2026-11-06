# Agora prerequisites

What must be true for Agora to work, how to check each item, and how to fix it. `setup` walks the list top to bottom (a member joining an existing root walks items 1–3, 5 and 6), and item 7 only in a root with email on (`email: on` in `org.md`); `help` and any failing skill jump to the item that matches the failure.

In shared services mode, item 4 is the manual setup guide for the admin and for each joining member: walk them through its steps as written, one person and one step at a time.

**Two modes** (rules → Vocabulary → Mode): an item without a mode named holds for both; otherwise follow the variant for `mode` in `org.md` (missing: basic mode), or, before `org.md` exists, for the mode setup chose.

**Check with a real call** wherever one exists. Each item ends **ready** (the check passed) or **blocked** (it failed, the fix is given, and the user is waiting on it). Never mark an item ready on the user's word alone when a call can confirm it. Blocked is only for a fix the user must do by hand (clicks, a password, a plan); a fix the connectors can do is yours: retry a failed call until the check passes.

Menu names change between app versions. If a path below doesn't match what the user sees, tell them what to look for (the word "Connectors", "Apps" or "Scheduled") rather than insisting on the exact path.

## 1. AI client and plan

- **Need**: Claude (web, desktop or mobile) or ChatGPT (web, desktop or mobile) on a paid plan. Scheduled routines need a paid plan on both.
- **Check**: ask which app and plan the member uses. This is the one item only the user can confirm.
- **Fix**: upgrade the plan, or continue without routines and record that in `org.md` (the member's Access row, Notes) so reviews can be run by hand.

## 2. Agora plugin installed

- **Need**: the Agora plugin enabled in the member's client.
- **Check**: you are reading this, so it is installed in this client. For other members, they confirm when they run `setup` themselves.
- **Fix**: Claude: Customize → Plugins (or Settings → Plugins), add the `Cruna-Noise-To-Signal/cruna-plugins` marketplace, install `agora`. ChatGPT: Plugins → install `agora` (a workspace admin may need to publish it first).

## 3. Connectors

A **connector** is a permission that lets the AI app act inside another service on your behalf: here, read and write files in Google Drive and events in Google Calendar. Without them the AI can only talk; with them it can file and schedule.

### Basic mode

- **Need**: two connectors, **Google Drive** and **Google Calendar**, plus **Gmail** when email is on (item 7), all logged in as the **assistant account** named in `org.md` (or as the owner's own account in a solo root). A connector logged in as the wrong Google account is the most common failure: the AI then sees a different Drive.
- **Check**:
  - Drive: search Drive for `org.md` (or, before setup, list recent files). Success means the Drive connector works. Then read `org.md`'s owner from its metadata and confirm it is the connected account (rules → Vocabulary): in My Drive only the owner can trash, so any other account breaks every Replace. A mismatch is **blocked**.
  - Calendar: list calendars. Success means the Calendar connector works; confirm the primary calendar carries the assistant account's email (`calendar` in `org.md`): that is the system calendar.
- **Fix**:
  - Claude: Settings → Connectors → Google Drive → Connect; same for Google Calendar. Sign in with the assistant account.
  - ChatGPT: Settings → Apps (Connectors) → Google Drive → Connect; same for Google Calendar. Sign in with the assistant account.
  - Already connected with the wrong account: disconnect, then connect again with the assistant account.
  - In a conversation, make sure both connectors are enabled for the chat (the tools or "+" menu).
  - `org.md` owned by another account while the connectors are the assistant account (a migration that moved only the folder): that owner, in Google Drive, selects the root's files → Share → next to the assistant account, **Transfer ownership**; the assistant account accepts. Or, when the group has Google Workspace, move the root to a shared drive (shared services mode), where every Content manager can trash.

### Shared services mode

- **Need**: the same connectors, each signed in as the member's **own Workspace account**, the Email in their Members row. Nobody shares a password.
- **Check**, in the member's own app:
  - Who: list calendars; the primary calendar's id is the connected account. It must be the member's Workspace account (before they are in Members: the account the admin gave them).
  - Drive: create a small test file under `root_id` (before the root exists: in the shared drive's top folder, `drive_id`), then trash it. Both calls succeed: ready. Not found: the member isn't in the shared drive; trash refused: they aren't a Content manager (item 4, step 2).
  - Calendar: create a test event on `calendar` (by its id, rules → The three ids), then delete it. Not found or refused: the calendar isn't shared with them at **Make changes to events** (item 4, step 4).
- **Fix**: the basic mode paths above, signing in with the member's own Workspace account. Gmail, Google Drive or Google Calendar missing from the connector list: the organisation hasn't enabled it (item 4, step 5).

## 4. Assistant account, or the admin's setup

### Basic mode: assistant account

- **Need** (multi-member roots only): a Google account dedicated to this root that owns the root folder; its primary calendar is the system calendar. Solo roots skip this item and use the owner's account.
- **Check**: the Drive and Calendar checks in item 3 return the assistant account as owner. Before `org.md` exists, list calendars: the primary calendar carries the connected account's email.
- **Fix**: create a free Google account (accounts.google.com → Create account → For my personal use), named after the root (`fabiole.agora@gmail.com`). Set its recovery phone and email to a member's, so the root is never lost with a forgotten password. Free accounts hold 15 GB across Drive and Gmail; that is plenty for documents, but say so.
- **Sharing the login**: every member's AI client connects as this account, so each member needs its password once, at connect time. Store it in the family's or team's password manager.

### Shared services mode: the admin's setup

- **Need**: the organisation's Google Workspace admin has created the three shared services and added every member to each. No connector creates shared drives, groups or calendars, or shares calendars, so every step below is clicks, by the person it names. Agora checks each one with a real call.
- **Check**: the check each step names. The item is ready when every step's check passed.

**Once per root**, in this order:

1. **Workspace account for each member** (admin). admin.google.com → **Directory** → **Users** → **Add new user** → first name, last name, primary email → **Add new user**. Check: the member signs in with it and item 3's Who check shows it.
2. **Shared drive** (admin, or a member allowed to create shared drives). drive.google.com → **Shared drives** (left panel) → **New** → name it after the root → **Create**. Open it → **Manage members** → each member's Workspace email → role **Content manager** (the lowest role that can trash) → **Send**. Then copy the address bar's URL (`https://drive.google.com/drive/folders/<id>`) for setup. Check: item 3's Drive check, at the drive's top folder.
3. **Google Group** (admin). The group is closed to everyone but its members, except that anyone can email it: the settings are label **Mailing**, access type **Restricted**, **Who can join**: Only invited users, and **Who can post** opened to outside senders. Opening Who can post changes the access type from Restricted to **Custom**: that is expected, and Custom is the right final state. Restricted's other settings (only members view conversations and the member list) stay.
   1. **Let groups accept outside mail** (once per organisation). Admin console → **Apps** → **Google Workspace** → **Groups for Business** → **Sharing settings** → under "Can external users participate in groups?", tick **Group owners can allow incoming email from outside the organization** → **Save**. Leave **Accessing groups from outside this organization** at **Private**. Without the tick, step 3.2 has no outside posting to offer and client mail never reaches the group.
   2. **Create the group.** Admin console → **Directory** → **Groups** → **Create group** → **Group name** (the root's name), **Group email** (the address members forward and CC to, e.g. `agora@acme.it`), **Group owners**: the admin → **Next** → **Labels**: **Mailing** ticked (the default), **Security** unticked → **Next** → **Access type**: **Restricted** → in the access settings table, row **Who can post**, also tick **External** (the type now reads **Custom**) → **Who can join the group**: **Only invited users** → **Allow external members in the group**: off → **Next** → **Create group**. No **External** box to tick: step 3.1 isn't saved yet, or hasn't reached the group (allow up to 24 hours). The same setting, seen from groups.google.com → the group → **Group settings**, reads **Who can post**: **Anyone on the web**; set it there when the group was created there.
   3. **Add the members.** groups.google.com → the group → **Members** → **Add members** → each member's Workspace email → **Subscription**: **Each email** → **Directly add members** on → **Add members**. Wait a few minutes before the first email.
   Check: item 7's shared services check, its test sent from an address outside the organisation.
4. **Shared calendar** (ideally the admin; whoever creates it is recorded as `calendar_owner`, and offboarding them waits for a transfer). calendar.google.com, signed in with their Workspace account → next to **Other calendars**, **+** → **Create new calendar** → name it after the root, **Time zone** the root's → **Create calendar**. Then **Settings** → under "Settings for my calendars", the new calendar → **Share with specific people or groups** → **Add people and groups** → each member's Workspace email, permission **Make changes to events** (on newer screens, **Make changes and see event details**) → **Send**. Then **Integrate calendar** → copy the **Calendar ID** (`…@group.calendar.google.com`) for setup. Check: item 3's Calendar check.
5. **Connectors enabled for the organisation** (the AI client's admin), when the plan has one:
   - Claude Team or Enterprise: an Owner or Primary Owner turns on Google Drive, Google Calendar and Gmail in **Organization settings** → **Connectors**.
   - ChatGPT Business: a workspace admin enables the Google Drive, Google Calendar and Gmail apps in **Workspace settings** → **Apps**, and turns on write actions for Gmail.
   Check: each member finds the three connectors in their own list (item 3).

**Per new member, before they join** (the admin, and the calendar owner for the calendar):

1. Step 1: their Workspace account.
2. Step 2: add them to the shared drive (**Manage members**) as **Content manager**.
3. Step 3.3: add them to the group, subscription **Each email**.
4. Step 4: the calendar owner shares the calendar with them at **Make changes to events**.
5. Step 5, when their AI plan has an organisation admin and the connectors aren't on yet.

**The joining member**, after the admin's steps, in their own AI app signed in as their Workspace account:

1. Install the Agora plugin (item 2).
2. Connect Google Drive, Google Calendar and Gmail, each signed in as their Workspace account (item 3; Gmail only when the root has email on, item 7).
3. Accept the shared calendar's invitation email, so the calendar shows on their phone.
4. Say "join Agora". Their AI checks each admin step with a real call and names the step to redo when one fails.

## 5. Sharing

### Basic mode

- **Need**:
  - The root folder shared with every member's personal Google account as **Editor**, so they can upload and browse from their own Drive.
  - The system calendar (the assistant account's primary calendar) shared with every member's personal Google account with **Make changes to events**, so events appear on their phones.
  - Optionally, each member's personal calendar shared with the assistant account, at **See only free/busy** or **See all event details** (the member's choice, recorded in `org.md`).
- **Check**: read the root folder's permissions and confirm each member with an email in `org.md` is an editor. List calendars and confirm each shared personal calendar is visible. System calendar sharing can't be read through the connector: the member confirms the calendar shows on their phone.
- **Fix**:
  - Folder: the Drive connector can share it; do it yourself, with each member's email as Editor, and retry until the permissions list them. By hand, as the assistant account: in Drive, right-click the root folder → Share → add each member's email as Editor.
  - System calendar: always by hand, since the calendar connector can't share calendars. Google Calendar (as the assistant account) → Settings → under "Settings for my calendars", the assistant account's name → Share with specific people or groups → Add people → each member's email, Make changes to events → Send. The member opens the invitation email and adds the calendar.
  - Personal calendar: each member, in their own Google Calendar → Settings → their calendar → Share with specific people → add the assistant account.

### Shared services mode

Sharing is membership; the Drive connector never shares the root folder in this mode.

- **Need**:
  - Every member a **Content manager** of the shared drive (item 4, step 2), in place of sharing the root folder.
  - The shared calendar shared with every member at **Make changes to events**, by the calendar owner (item 4, step 4).
  - Personal calendars: Workspace's domain-wide free/busy is enough to find free time (Personal calendar `domain free/busy`). A member who wants more detail shares their calendar with the others: Google Calendar → Settings → their calendar → Share with specific people or groups → each member, **See all event details** (Personal calendar `full details`).
- **Check**: item 3's shared services Drive and Calendar checks, in the member's own app. Record what they proved in the Access row: Drive `content manager`, System calendar `can edit`.
- **Fix**: the item 4 step the failing check names, by the person it names.

## 6. Scheduled routines

- **Need**: three scheduled tasks per member, each running the `review` skill: daily (morning), weekly (Sunday evening), monthly (the 1st).
- **Check**: ask the member to confirm the three tasks appear in their scheduled list. Routine runs can't be listed from a conversation on every platform.
- **Fix**: create each task with the prompt and time given by `setup`:
  - Claude: open the scheduled tasks page (Scheduled, in the sidebar) → New task, or ask Claude in a chat to schedule it.
  - ChatGPT: open Scheduled (Tasks) → New task, or ask ChatGPT in a chat to schedule it.
  - Confirm the task can use the Drive and Calendar connectors and the Agora plugin. Turn on notifications so the review reaches the member.

## 7. Gmail connector (email on only)

Optional: only a root with `email: on` needs it, and it suits business roots best. A root with email off skips this item.

### Basic mode

- **Need**: the **Gmail** connector, logged in as the **assistant account**, so the AI reads the assistant inbox and nobody's personal mail. In a solo root that is the owner's own account.
- **Check**:
  1. Search the inbox with a real call (the latest few messages).
  2. Confirm the mailbox is the assistant account's: from the connector's account or profile details when it offers them, else from the recipient address of a received message. An empty new inbox: ask a member to send a test email to the assistant address, then search for it.
  The check passes only when the address matches `assistant_account` in `org.md`.
- **Fix**:
  - Claude: Settings → Connectors → Gmail → Connect, signed in as the assistant account. On a Team or Enterprise plan, an Owner must first enable the Gmail connector for the organization.
  - ChatGPT: Settings → Apps (Connectors) → Gmail → Connect, signed in as the assistant account. Sending works only on the web, on a paid plan (Plus, Pro, Business, Enterprise); on Business, a workspace admin must turn on write actions for Gmail. Reading and drafting work everywhere the connector does.
  - Already connected with the wrong account: disconnect, then connect again with the assistant account.
  - In a conversation, make sure Gmail is enabled for the chat (the tools or "+" menu).
- **Limits** to tell the member: the AI sees only the assistant inbox; it can't open attachments, only their names; it doesn't label, archive or delete mail.

### Shared services mode

- **Need**: the **Gmail** connector, signed in as the member's own Workspace account, and the member in the group with **Each email** delivery, so group mail lands in their own inbox.
- **Check**: search `deliveredto:<inbox>` (`inbox` in `org.md`, the group's address) with a real call; it must return results. None yet (a new group): ask the member to send a test email to the group's address, wait a minute, and search again. Still none: the member isn't in the group, their delivery isn't Each email, or, for a test from outside the organisation, outside posting is off (item 4, steps 3.1 and 3.2). On a pass, write `receives mail` in their Access row's Notes.
- **Fix**: the basic mode paths above, signed in as the member's own account; Gmail missing from the connector list: item 4, step 5.
- **Limits** to tell the member: the connector could reach their whole mailbox, but Agora reads only mail delivered to the group, by its own rule (rules → The three ids); it can't open attachments, only their names; it doesn't label, archive or delete mail.

## Offboarding a member

Their diary entries and documents stay in both modes: they are history.

### Basic mode

Remove their editor access on the root folder and the system calendar (as the assistant account), ask them to disconnect the connectors (Gmail too, when email is on) in their client, change the assistant account password if they knew it, and update the Members and Access tables in `org.md`.

### Shared services mode

1. **Calendar owner first.** When the member is `calendar_owner`, stop here until the calendar is transferred: a secondary calendar is deleted with its owner's account. The member transfers it to another member of the domain: Google Calendar → **Settings** → the calendar → **Shared with** → the drop-down beside the new owner → **Transfer ownership** → confirm; the new owner accepts from the email within 60 days. When the member can't, the admin transfers it in the Admin console. Then set `calendar_owner` in `org.md` to the new owner's slug. Check: the new owner confirms the calendar is under their "My calendars", and item 3's Calendar check still passes.
2. **Remove them from the shared services** (admin; the calendar owner for the calendar):
   - shared drive: open it → **Manage members** → beside them, the role drop-down → **Remove access**;
   - group: groups.google.com → the group → **Members** → tick them → **Remove member**;
   - calendar: **Settings** → the calendar → **Shared with** → **×** beside them.
3. **Their Workspace account** (admin): suspend or delete it, at the admin's choice: Admin console → **Directory** → **Users** → their name → **Suspend user** or **Delete user**. Files in the shared drive belong to the drive and stay.
4. Ask them, if reachable, to disconnect the connectors in their client and delete their Agora routines.
5. Update the Members and Access tables in `org.md`.
