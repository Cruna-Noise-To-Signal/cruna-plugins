# Agora prerequisites

What must be true for Agora to work, how to check each item, and how to fix it. `setup` walks the list top to bottom (a member joining an existing root walks items 1–3, 5 and 6; in shared services mode, once item 4's steps for them are done), and item 7 only in a root with email on (`email: on` in `org.md`); `help` and any failing skill jump to the item that matches the failure.

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

- **Need**: the organisation's Google Workspace admin has created the shared drive, the group and the shared calendar, and added every member to each: the steps in [manual-setup.md](manual-setup.md), the manual setup guide for the admin and for each member, joining and leaving. "Item 4, step N" means its step N.
- **Check**: the check each of its steps names. Ready when every step's check passed, for the root (Once per root) or for the member joining (Per new member).
- **Fix**: the failing step, by the person it names.

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
- **Check**: search `deliveredto:<inbox>` (`inbox` in `org.md`, the group's address) with a real call; it must return results. None yet (a new group): ask the member to send a test email to the group's address from an address outside the organisation (a personal Gmail, say), so the test also proves outside senders reach the group; wait a minute, and search again. Still none: the member isn't in the group, their delivery isn't Each email, or outside posting is off (item 4, steps 3.1 and 3.2). On a pass, write `receives mail` in their Access row's Notes.
- **Fix**: the basic mode paths above, signed in as the member's own account; Gmail missing from the connector list: item 4, step 5.
- **Limits** to tell the member: the connector could reach their whole mailbox, but Agora reads only mail delivered to the group, by its own rule (rules → The three ids); it can't open attachments, only their names; it doesn't label, archive or delete mail.

## Offboarding a member

Their diary entries and documents stay in both modes: they are history.

### Basic mode

Remove their editor access on the root folder and the system calendar (as the assistant account), ask them to disconnect the connectors (Gmail too, when email is on) in their client, change the assistant account password if they knew it, and update the Members and Access tables in `org.md`.

### Shared services mode

Follow [manual-setup.md](manual-setup.md) → A member leaving: the calendar owner's transfer comes first, and blocks the rest until `calendar_owner` names the new owner.
