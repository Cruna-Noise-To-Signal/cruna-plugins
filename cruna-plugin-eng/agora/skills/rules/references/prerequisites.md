# Agora prerequisites

What must be true for Agora to work, how to check each item, and how to fix it. `setup` walks the list top to bottom (a member joining an existing root walks items 1–3, 5 and 6); `help` and any failing skill jump to the item that matches the failure.

**Check with a real call** wherever one exists. Each item ends **ready** (the check passed) or **blocked** (it failed, the fix is given, and the user is waiting on it). Never mark an item ready on the user's word alone when a call can confirm it.

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

- **Need**: two connectors, **Google Drive** and **Google Calendar**, both logged in as the **assistant account** named in `org.md` (or as the owner's own account in a solo root). A connector logged in as the wrong Google account is the most common failure: the AI then sees a different Drive.
- **Check**:
  - Drive: search Drive for `org.md` (or, before setup, list recent files). Success means the Drive connector works; confirm the file owner matches the assistant account.
  - Calendar: list calendars. Success means the Calendar connector works; confirm the system calendar from `org.md` is in the list and writable.
- **Fix**:
  - Claude: Settings → Connectors → Google Drive → Connect; same for Google Calendar. Sign in with the assistant account.
  - ChatGPT: Settings → Apps (Connectors) → Google Drive → Connect; same for Google Calendar. Sign in with the assistant account.
  - Already connected with the wrong account: disconnect, then connect again with the assistant account.
  - In a conversation, make sure both connectors are enabled for the chat (the tools or "+" menu).

## 4. Assistant account

- **Need** (multi-member roots only): a Google account dedicated to this root that owns the root folder and the system calendar. Solo roots skip this item and use the owner's account.
- **Check**: the Drive and Calendar checks in item 3 return the assistant account as owner. Before `org.md` exists, list calendars: the primary calendar carries the connected account's email.
- **Fix**: create a free Google account (accounts.google.com → Create account → For my personal use), named after the root (`fabiole.agora@gmail.com`). Set its recovery phone and email to a member's, so the root is never lost with a forgotten password. Free accounts hold 15 GB across Drive and Gmail; that is plenty for documents, but say so.
- **Sharing the login**: every member's AI client connects as this account, so each member needs its password once, at connect time. Store it in the family's or team's password manager.

## 5. Sharing

- **Need**:
  - The root folder shared with every member's personal Google account as **Editor**, so they can upload and browse from their own Drive.
  - The system calendar shared with every member's personal Google account with **Make changes to events**, so events appear on their phones.
  - Optionally, each member's personal calendar shared with the assistant account, at **See only free/busy** or **See all event details** (the member's choice, recorded in `org.md`).
- **Check**: read the root folder's permissions and confirm each member listed in `org.md` is an editor. List calendars and confirm each shared personal calendar is visible.
- **Fix**:
  - Folder: in Drive (as the assistant account), right-click the root folder → Share → add each member's email as Editor.
  - System calendar: Google Calendar (as the assistant account) → Settings → the calendar → Share with specific people → add each member, Make changes to events.
  - Personal calendar: each member, in their own Google Calendar → Settings → their calendar → Share with specific people → add the assistant account.

## 6. Scheduled routines

- **Need**: three scheduled tasks per member, each running the `review` skill: daily (morning), weekly (Sunday evening), monthly (the 1st).
- **Check**: ask the member to confirm the three tasks appear in their scheduled list. Routine runs can't be listed from a conversation on every platform.
- **Fix**: create each task with the prompt and time given by `setup`:
  - Claude: open the scheduled tasks page (Scheduled, in the sidebar) → New task, or ask Claude in a chat to schedule it.
  - ChatGPT: open Scheduled (Tasks) → New task, or ask ChatGPT in a chat to schedule it.
  - Confirm the task can use the Drive and Calendar connectors and the Agora plugin. Turn on notifications so the review reaches the member.

## Offboarding a member

Remove their editor access on the root folder and the system calendar (as the assistant account), ask them to disconnect the connectors in their client, change the assistant account password if they knew it, and update the Members and Access tables in `org.md`. Their diary entries and documents stay: they are history.
