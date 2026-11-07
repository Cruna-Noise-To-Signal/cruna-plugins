# Grilled idea: earnest-tozzetto (#11)

Grilling session of 2026-11-06, run in chat with the founder. The agreed design was posted on the issue: https://github.com/Cruna-Noise-To-Signal/cruna-plugins/issues/11#issuecomment-6686210050. Visual summary (private artifact): https://claude.ai/artifact/3MxRRELqZgMB1L7uMHEFC5

## Where it started

Issue #11 asks for Agora roots in a shared drive, because in My Drive only a file's owner can trash it. That breaks Replace as soon as a connector signs in as anyone other than the owner. The issue left two questions open for shared-drive roots:
- which calendar is the system calendar;
- what the Gmail assistant inbox becomes.

A comment from earlier the same day settled the ids:
- keep both `drive_id` and `root_id`;
- `drive_id` is the id from the drive's URL;
- check that a file is in the drive by walking its parents, not by comparing `sharedDriveId`;
- finding the root needs no id.

## Where it concluded

### Model
- **Everyone is a user**: a Google account with an AI client attached. The new mode has no assistant role. `kind` (`human` | `ai`) is recorded in Members for the organisation's own reasons only and changes nothing else. A missing value means `human`.
- **The rule:** the same question gets the same answer from any connected account.
- **Two modes**, in `org.md` front-matter `mode`:
  - **basic shared account mode** (`basic-shared-account`): several people sign in to one Google account. This is the default when `mode` is missing.
  - **shared services mode** (`shared-services`): one Workspace account per member, sharing a shared drive, a shared calendar and a Google Group.
- **Italian:** "modalità account condiviso base" and "modalità servizi condivisi". The values stay in English.
- The founder rejected "family mode" as a name.

### The three shared ids, read the same way by every skill in both modes
- **Files** only under `root_id`. `drive_id` is the URL id, or `my-drive`.
- **Calendar:** `calendarId = calendar`, never the default primary calendar. In basic mode that's the account's email, which is already the primary calendar's id.
- **Mail:** every Gmail call is filtered on `deliveredto:<inbox>`. `inbox` is the group address in shared services, and defaults to `assistant_account` in basic mode. This filter is our own rule, not enforced by Google, and must be stated plainly.
- So `schedule`, `email`, `ask` and `log` don't branch on the mode. Only setup, the checks and the who's-speaking step do.

### Calendar (shared services)
- A secondary calendar created by hand by any member, ideally the Workspace admin, recorded as `calendar_owner`. It's shared with every member at Make changes to events.
- Offboarding the calendar owner waits until the calendar has been transferred within the domain.
- The check creates and deletes a test event.
- **Personal calendars:** Workspace's domain-wide free/busy is the default; sharing more detail is optional.
- The founder confirmed after the session that the AI client can create and delete events on the shared calendar by its id.

### Mail (shared services)
- A Google Group with every member in it, posting open to anyone on the web, and normal delivery (no digests).
- Members CC or forward mail to the group, and it lands in every member's own inbox.
- Sending: from the member's own address, with the group in CC.
- `inbox.md` keeps the Gmail message id as the key and gains a **Mailbox** column (who logged the row). Duplicates are recognised by sender + minute + subject. Mail logged from another member's mailbox is reopened by search.
  - The RFC 822 `Message-ID` was proposed and rejected: the Gmail connector only returns headers in `RAW` format.
- Prerequisites item 7's check becomes: a search for `deliveredto:<inbox>` returns results.
- The founder confirmed after the session that ChatGPT's Gmail connector supports `deliveredto:` and `list:` searches.

### Who's speaking
- Shared services: the connected account's email is matched to the Access table. An account that isn't listed is pointed to Join.
- Basic: Agora asks, as today.

### Routines
- They stay as they are today:
  - the prompts;
  - the tidy-up that's safe to run twice;
  - one review file per period with appended Updates;
  - daily reviews trashed after 30 days;
  - email triage inside `review`.
- **New:** the root `status.md` records when the tidy-up last finished. A run within 60 minutes of it skips tidy-up steps 2.1–2.6.
- **New:** a period with no review file by its end, or triage not run in 24 hours, goes under Needs attention.
- **Withdrawn during the session:**
  - a `runs.md` run record with claims and stale-claim takeover, because the existing machinery already makes double runs harmless;
  - keeping every daily review;
  - an hourly triage window.
- A separate hourly email-triage routine is a later request.

### Joining (shared services)
1. The admin:
   - adds the member to the group;
   - shares the shared calendar with them (the calendar owner does this);
   - gives them a Workspace account;
   - adds them to the shared drive as Content manager. The founder's list had only the first three; the session added this one;
   - on Claude Team or Enterprise, makes sure the Gmail connector is enabled.
2. The member connects Drive, Calendar and Gmail as themselves and says "join Agora". Real calls check the admin's work.
3. Joining writes:
   - Members and Access rows in `org.md`;
   - `memories/<slug>.md`;
   - the member's section in `setup.md`;
   - the email member part, when email is on.

   It doesn't touch folder permissions or calendar sharing.
4. Interest folder in per-person layouts: today's Join doesn't create one, and the founder will check this after implementation. Keep today's behaviour unless that check says otherwise.

### Move to shared drive (basic → shared services)
- Move or rebuild the root.
- Copy future `[interest/instance]` events to the new calendar, and leave the old calendar alone.
- Set the old Gmail to forward everything to the group, and trust the old address in `email_from`.
- Retire the old account.
- Update the ids, and tell linked repos to update `.agora/pm.yml` (cruna-development-plugins#29).

### Compatibility
- Missing fields fall back to basic mode, so no existing file has to change.
- The only addition to basic mode is the issue's check that the connected account owns `org.md` (Verify, `help`, `review`), and a failed trash naming the account mismatch.
- **Manual acceptance test, run by the founder:** an untouched basic root runs Verify, a daily `review` with email on, `schedule`, `email` and `ask` with the same results as before.

### Manual steps
The implementation must precisely update every step people do by hand, in the prerequisites and setup references, in English and Italian:
- the admin's setup: shared drive, group, calendar, Workspace account, Gmail connector;
- member joining;
- offboarding;
- moving to a shared drive.
