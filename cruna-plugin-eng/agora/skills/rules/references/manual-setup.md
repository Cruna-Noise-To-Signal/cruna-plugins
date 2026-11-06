# Shared services: the manual setup

The clicks a shared services root needs, for the admin and for each member, joining and leaving. No connector creates shared drives, groups or calendars, or shares calendars, so every step here is done by hand, by the person it names; Agora checks each one with a real call, and the item 3 or 7 check a step names is what makes it done. Walk people through it one person and one step at a time. Prerequisites item 4 points here: "item 4, step N" means step N below. The "menu names change" note at the top of the prerequisites holds here too.

## Once per root

In this order:

1. **Workspace account for each member** (admin). admin.google.com → **Directory** → **Users** → **Add new user** → first name, last name, primary email → **Add new user**. Check: the member signs in with it and item 3's Who check shows it.
2. **Shared drive** (admin, or a member allowed to create shared drives). drive.google.com → **Shared drives** (left panel) → **New** → name it after the root → **Create**. Open it → **Manage members** → each member's Workspace email → role **Content manager** (the lowest role that can trash) → **Send**. Then copy the address bar's URL (`https://drive.google.com/drive/folders/<id>`) for setup. Check: item 3's Drive check, at the drive's top folder.
3. **Google Group** (admin). Goal: a group only its members can see and join, that anyone, clients included, can email.
   1. **Let groups accept outside mail** (once per organisation). Admin console → **Apps** → **Google Workspace** → **Groups for Business** → **Sharing settings** → under "Can external users participate in groups?", tick **Group owners can allow incoming email from outside the organization** → **Save**. Leave **Accessing groups from outside this organization** at **Private**. Without the tick, step 3.2 has no outside posting to offer and client mail never reaches the group.
   2. **Create the group.** Admin console → **Directory** → **Groups** → **Create group** → **Group name** (the root's name), **Group email** (the address members forward and CC to, e.g. `agora@acme.it`), **Group owners**: the admin → **Next** → **Labels**: **Mailing** ticked (the default), **Security** unticked → **Next** → **Access type**: **Restricted** → in the access settings table, row **Who can post**, also tick **External** → **Who can join the group**: **Only invited users** → **Allow external members in the group**: off → **Next** → **Create group**. Ticking External turns the access type to **Custom**: that is expected and the right final state, and Restricted's other settings (only members view conversations and the member list) stay. No **External** box to tick: step 3.1 isn't saved yet, or hasn't reached the group (allow up to 24 hours). The same setting, seen from groups.google.com → the group → **Group settings**, reads **Who can post**: **Anyone on the web**; set it there when the group was created there.
   3. **Add the members.** groups.google.com → the group → **Members** → **Add members** → each member's Workspace email → **Subscription**: **Each email** → **Directly add members** on → **Add members**. Wait a few minutes before the first email.
   Check: item 7's shared services check, whose test comes from outside the organisation.
4. **Shared calendar** (ideally the admin; whoever creates it is recorded as `calendar_owner`, and offboarding them waits for a transfer). calendar.google.com, signed in with their Workspace account → next to **Other calendars**, **+** → **Create new calendar** → name it after the root, **Time zone** the root's → **Create calendar**. Then **Settings** → under "Settings for my calendars", the new calendar → **Share with specific people or groups** → **Add people and groups** → each member's Workspace email, permission **Make changes to events** (on newer screens, **Make changes and see event details**) → **Send**. Then **Integrate calendar** → copy the **Calendar ID** (`…@group.calendar.google.com`) for setup. Check: item 3's Calendar check.
5. **Connectors enabled for the organisation** (the AI client's admin), when the plan has one:
   - Claude Team or Enterprise: an Owner or Primary Owner turns on Google Drive, Google Calendar and Gmail in **Organization settings** → **Connectors**.
   - ChatGPT Business: a workspace admin enables the Google Drive, Google Calendar and Gmail apps in **Workspace settings** → **Apps**, and turns on write actions for Gmail.
   Check: each member finds the three connectors in their own list (item 3).

## Per new member, before they join

The admin, and the calendar owner for the calendar. Each step ticks the box of the same name in the member's `setup.md` section once the joining member's own check passes.

1. Step 1: their Workspace account. Box: Workspace account.
2. Step 2: add them to the shared drive (**Manage members**) as **Content manager**. Box: In the shared drive.
3. Step 3.3: add them to the group, subscription **Each email**. Box: In the group.
4. Step 4: the calendar owner shares the calendar with them at **Make changes to events**. Box: Shared calendar.
5. Step 5, when their AI plan has an organisation admin and the connectors aren't on yet.

## The joining member

After the admin's steps, in their own AI app signed in as their Workspace account:

1. Install the Agora plugin (item 2).
2. Connect Google Drive, Google Calendar and Gmail, each signed in as their Workspace account (item 3; Gmail only when the root has email on, item 7).
3. Accept the shared calendar's invitation email, so the calendar shows on their phone.
4. Say "join Agora". Their AI checks each admin step with a real call and names the step to redo, and who does it, when one fails.

## A member leaving

1. **Calendar owner first.** When the member is `calendar_owner`, stop here until the calendar is transferred: a secondary calendar is deleted with its owner's account. The member transfers it to another member of the domain: Google Calendar → **Settings** → the calendar → **Shared with** → the drop-down beside the new owner → **Transfer ownership** → confirm; the new owner accepts from the email within 60 days. When the member can't, the admin transfers it in the Admin console. Then set `calendar_owner` in `org.md` to the new owner's slug (rules → Replace). Check: the new owner confirms the calendar is under their "My calendars", and item 3's Calendar check still passes.
2. **Remove them from the shared services** (admin; the calendar owner for the calendar):
   - shared drive: open it → **Manage members** → beside them, the role drop-down → **Remove access**;
   - group: groups.google.com → the group → **Members** → tick them → **Remove member**;
   - calendar: **Settings** → the calendar → **Shared with** → **×** beside them.
3. **Their Workspace account** (admin): suspend or delete it, at the admin's choice: Admin console → **Directory** → **Users** → their name → **Suspend user** or **Delete user**. Files in the shared drive belong to the drive and stay.
4. Ask them, if reachable, to disconnect the connectors in their client and delete their Agora routines.
5. Update the Members and Access tables in `org.md`.
