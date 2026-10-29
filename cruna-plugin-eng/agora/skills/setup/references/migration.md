# Migrate a solo root to a dedicated assistant account

A solo root lives in the owner's own Google account. When a second person joins, a dedicated **assistant account** takes over the root folder, and its primary calendar becomes the system calendar, so every member's app can connect as that one account. Run this with the owner; most steps are clicks they do in Google, while you check each one with a real call.

Explain the why in one or two plain sentences before starting: everyone's assistant needs to log in as the same Google account, and it shouldn't be the owner's personal one.

## Steps

1. **Create the assistant account.** Walk prerequisites item 4. Done when the owner has the new address and password and recovery is set.
2. **Give it the folder.** The owner, in Google Drive as themselves:
   1. Right-click the root folder → **Share** → add the assistant account as **Editor** → Send.
   2. Open **Share** again → next to the assistant account choose **Transfer ownership** → confirm. Google sends an ownership invitation.
   3. Sign in to Google Drive as the assistant account → open the invitation (in its Gmail, or the folder's Share dialog) → **Accept**.

   Transferring a folder moves the folder only; the files inside stay owned by the owner (still fully usable by everyone). To move them too, the owner opens each folder, selects all files, and repeats Share → Transfer ownership; the assistant account accepts. Recommend it when the owner might leave the group or is short on storage; otherwise it can wait.

   Done when, after step 4, the root folder's permissions show the assistant account as owner and the owner as editor.
3. **Share the owner's calendar with it.** The owner, in Google Calendar → Settings → under "Settings for my calendars", their own name → Share with specific people or groups → add the assistant account with **Make changes to events** (needed to move the Agora events; lowered in step 7). Done when, after step 4, the owner's calendar appears in the calendar list.
4. **Reconnect the owner's connectors.** Walk prerequisites item 3, fix "already connected with the wrong account": disconnect Google Drive and Google Calendar, connect both again as the assistant account. Done when the calendar list's primary calendar carries the assistant account's email and Drive search finds `org.md`.
5. **Move the Agora events.** The assistant account's primary calendar is the new system calendar. Copy every future event on the owner's calendar whose title starts with `[<interest>/<instance>]` into it, keeping title, time, description and color; then delete each original from the owner's calendar, so nothing appears twice. Done when the primary calendar holds every future Agora event and the owner's calendar holds none.
6. **Update `org.md`**: `assistant_account` and `calendar` both set to the new address, the owner's Access row updated. Replace the file per rules → Replace. Done when the downloaded `org.md` shows both changes.
7. **Share the system calendar** with the owner (prerequisites item 5, click steps as the assistant account). Then the owner lowers the sharing of their own calendar with the assistant account to the level in `org.md` (step 3's path). Done when the owner confirms both.

Done when the root folder is owned by the assistant account, its primary calendar holds the future Agora events, `org.md` names the new address, and the owner's connectors log in as the assistant account. The owner's scheduled routines need no change.
