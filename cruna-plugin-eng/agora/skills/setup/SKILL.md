---
name: setup
description: Sets up Agora, the shared family or business notebook on Google Drive and Google Calendar. Use when the user wants to start Agora for the first time ("set up Agora", "get started"), when a person joins an existing Agora or an existing member starts using it from another app ("add my wife to Agora", "I'm joining our family Agora", "I installed Agora on ChatGPT too"), when a one-person Agora must move to its own assistant account because someone is joining, or when someone leaves ("remove Simona from Agora").
---

# Agora setup

Setup turns an empty Google Drive and Calendar into a working Agora **root**, brings each member onto it, and takes them off it when they leave. It has four branches: **New root**, **Join**, **Migrate** (always followed by Join) and **Offboard**. Pick one in step 1 and follow only that branch. Every branch ends with the same **verification**; every branch but Offboard ends with the same "what you can do now" message.

## 1. Load the rules and pick the branch

Load the `rules` skill. Follow rules → Finding the root, then pick the branch:

| What you find | Branch |
|---|---|
| No root, and the user is starting fresh | **New root** |
| No root, but the user says someone already set one up | Their connectors are logged in as the wrong Google account: walk prerequisites item 3, then search again |
| A root whose verification (step 6) fails, and the user is its owner | **New root**, resuming at the first missing piece of step 2c |
| A root, and the user is joining it or using it from a new app | **Join** |
| A solo root (one member, `assistant_account` is that member's own account) and someone new is joining | **Migrate**, then **Join** for the newcomer |
| A root, and a member is leaving ("remove me from Agora", "Simona no longer needs access") | **Offboard** |

Identity: in a found root, follow rules → Identity and read the person's memories (rules → Memory step). In a new root, the person talking becomes the first member during the interview.

Done when the branch is chosen and, for a found root, you know who is talking.

The prerequisites checklist is the `rules` skill's `references/prerequisites.md`; "item N" below means its numbered sections. Walk each item with real calls and end it **ready** or **blocked**, as that file says. A blocked item stops the branch until its fix is done, except item 5 sharing for someone not present: record it and carry on.

**Persist**, through the connectors only (rules → Connectors only). Everything setup asks of the connectors is within their reach: creating folders and files, and sharing the root folder with members. A failed call is a hiccup to retry, never a reason to stop, to call an item blocked, or to hand the work to the user. Only the steps marked as clicks belong to the user.

## 2. New root

### 2a. Prerequisites

1. Tell the user, in two plain sentences, what they will do by hand along the way: the calendar connector can't share calendars, so sharing the Agora calendar with each member (and each member's own calendar with Agora) is a few clicks in Google Calendar, which you'll guide step by step. Everything else you do yourself.
2. Ask: "Will anyone else use this with you?"
   - Yes → a dedicated **assistant account** owns everything (item 4 applies).
   - No → **solo**: the user's own account plays the assistant role; skip item 4. Tell them they can move to a dedicated account later when someone joins.
3. Walk item 1, item 2, then (dedicated only) item 4, then item 3. The assistant account must exist before the connectors log in as it.
4. For item 3, confirm whose account the connectors use: the primary calendar in the calendar list carries the account's email address. It must be the assistant account (or the user's own, when solo).

Done when items 1–4 are each ready (item 4 skipped when solo).

### 2b. Interview

Follow [references/interview.md](references/interview.md): members, root name, language, timezone, layout convention, first interests and instances. **Grill gently**: a few questions per round, each with a recommended answer the user can accept with one word.

Done when the user has confirmed the summary at the end of the interview.

### 2c. Build

Build in this order, writing every file from its template (rules → File kinds → templates) in the root's language, and following rules → Working with Drive. If the root folder already exists (resuming), skip what verification shows is already there.

1. Root folder in the assistant account's Drive, named after the root display name.
2. `org.md`, with `agora: root` in its front-matter, every member in Members, the layout convention, and one Access row per member (Drive and calendar "pending" until step 2d). Written first, so an interrupted setup is found and resumed.
3. `landing/`, `reviews/`, `memories/` with `shared.md` and one `<slug>.md` per member (title line only, plus anything durable the interview revealed).
4. The Interests table in `org.md`, every interest from the interview with its description and color (colors as in `add-interest` step 5), written once (rules → Replace).
5. Each interest and instance from the interview, built with step 4 of the `add-interest` skill, then the interest level of its step 6 (each interest's `index.md` and `status.md`, rolled up from its instances). You already hold the root and the answers, so skip its questions.
6. Root `index.md` and `status.md`, rebuilt once at the end of the build so they roll up everything below.
7. The **system calendar** is the assistant account's own primary calendar (rules → Calendar conventions): nothing to create or search for. List calendars and confirm the primary one carries the assistant account's email and the timezone in `org.md`. A different timezone is a click fix for the user, signed in to Google Calendar as the assistant account: **Settings** → **General** → **Time zone**.

Done when every item above exists exactly once and the calendar list shows the assistant account's primary calendar in the root's timezone.

### 2d. Sharing

Walk item 5 for every member:

1. **Root folder**: share it yourself through the Drive connector, as editor, with each member's email. When a call fails, read the folder's permissions, then share again, one member at a time, until the permissions list every member.
2. **System calendar**: the calendar connector can't share it, so give the click steps of item 5 to whoever is signed in as the assistant account, one member at a time, and wait for each to be done.
3. **Personal calendars**: ask each member to share theirs at the level recorded in `org.md`, with the click steps of item 5.

Update the Access table in `org.md` with what is now true.

Solo roots: nothing to share; mark the owner's row as owner.

Done when the root folder's permissions, read with a real call, list every member with an email as editor; the Access table shows each member's real Drive and calendar access; and calendar sharing still pending for someone not present is named for the closing message.

### 2e. Routines

Give the user the three routines from [references/routines.md](references/routines.md), filled in for them, then walk item 6.

Done when the user confirms the three routines are in their scheduled list, or item 6 is recorded as blocked.

Go to step 6.

## 3. Join

For a new member, or an existing member on a new app. Run it in the joining person's own app.

1. Walk items 1, 2 and 3. The connectors log in as the assistant account in `org.md`; the person gets its password from whoever keeps it.
2. Find the root (rules → Finding the root) and follow rules → Identity.
3. **New member only**: interview them with the Members round of [references/interview.md](references/interview.md) (just for themselves). Add them to Members and Access in `org.md`, create `memories/<slug>.md`, and walk item 5 for them as in step 2d: share the root folder yourself until its permissions list them, then guide the click steps that share the system calendar with their personal Google account and their personal calendar with the assistant account at the level they choose.
4. **Existing member, new app**: update their Access row ("Client connected").
5. Give them their routines from [references/routines.md](references/routines.md) and walk item 6.

Done when the person is in Members and Access with their real access, the root folder's permissions list them as editor (new member), they have a memories file, and item 6 is ready or blocked. Go to step 6.

## 4. Migrate (solo → dedicated account)

Run it with the root's owner, before the newcomer joins. Follow [references/migration.md](references/migration.md): create the assistant account, hand it the root folder and the Agora events, reconnect the owner's connectors, and update `org.md`.

Done when migration's own completion criterion holds. Then run **Join** for the newcomer.

## 5. Offboard

Run it with the root's owner, signed in as the assistant account where a step needs clicks. Follow the `rules` skill's `references/prerequisites.md` → Offboarding a member, confirming each step with the owner before you do it or hand it over:

1. Remove the member's editor access on the root folder (yourself when the Drive connector can change sharing; otherwise give the owner the click steps).
2. Remove their access to the system calendar: Google Calendar (as the assistant account) → Settings → the calendar → Share with specific people → remove them.
3. Ask the member, if reachable, to disconnect the Drive and Calendar connectors in their client and delete their Agora routines.
4. When they knew the assistant account's password, have the owner change it and pass the new one to the remaining members.
5. Update `org.md` (rules → Replace): write `removed YYYY-MM-DD` in their Members Notes, and `removed` under Drive and System calendar in their Access row. Their diary entries, documents and `memories/<slug>.md` stay: they are history.

Done when the member has no editor access on the folder or calendar and the Members/Access rows in `org.md` show them removed. Go to step 6.

## 6. Verify

Check the root with real calls, never from memory of what you built:

- `org.md` downloads, its front-matter has `agora: root`, and every member has a Members row and an Access row (a removed member's rows say so, and the folder and calendar no longer list them).
- Root `index.md` and `status.md`, `landing/`, `reviews/`, `memories/shared.md` and one `memories/<slug>.md` per member exist.
- Every interest in `org.md` and in the root `index.md` has a folder with `index.md` and `status.md`; every instance has `index.md`, `status.md`, `diary/` and `documents/`.
- No file name appears twice in the same folder (rules → Replace).
- The calendar list's primary calendar carries the assistant account's email, as `calendar` in `org.md` does.
- The root folder's permissions list every member with an email as editor (a removed member: not at all).

Fix what fails and check again. Done when every line passes.

## 7. Memory step

Follow rules → Memory step. Setup usually learns a lot (roles, habits, how people want to be addressed): write it now.

Done when: anything durable you learned is in `memories/`, or you found nothing new.

## Output

A short message in the root's language, no technical terms. Offboard: one line saying who was removed, anything still pending as a to-do with who does it, and the `remember` line, if any. Every other branch:

1. One line: the Agora is ready (or: this person is now part of it), with the root name.
2. Anything still pending, as a to-do with who does it ("Marta: accept the calendar invitation").
   When the interview found a sheet or note they already keep, offer to bring it in now, one question per sheet (the `file-document` skill, Import).
3. **What you can do now**, four or five lines with an example sentence each, drawn from their own interests:
   - Tell me news: "Nina's blood test came back fine." (`log`)
   - Add a file: drop it in the `landing` folder and tell me. (`file-document`)
   - Set a reminder or appointment: "Remind us to renew the car insurance on 12 March." (`schedule`)
   - Ask: "What's pending for Nina?" (`ask`)
   - Track something new: "Start tracking the house renovation." (`add-interest`)
4. When reviews will arrive (each morning, Sunday evening, the 1st), and that "help" explains anything else.
5. The `remember` line, if any.
