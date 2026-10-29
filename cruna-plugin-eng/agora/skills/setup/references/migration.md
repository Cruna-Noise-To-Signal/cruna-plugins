# Migrate a solo root to a dedicated assistant account

A solo root lives in the owner's own Google account. When a second person joins, a dedicated **assistant account** takes over the root folder and the system calendar, so every member's app can connect as that one account. Run this with the owner; most steps are clicks they do in Google, while you check each one with a real call.

Explain the why in one or two plain sentences before starting: everyone's assistant needs to log in as the same Google account, and it shouldn't be the owner's personal one.

## Steps

1. **Create the assistant account.** Walk prerequisites item 4. Done when the owner has the new address and password and recovery is set.
2. **Give it the folder.** The owner, in Google Drive as themselves:
   1. Right-click the root folder → **Share** → add the assistant account as **Editor** → Send.
   2. Open **Share** again → next to the assistant account choose **Transfer ownership** → confirm. Google sends an ownership invitation.
   3. Sign in to Google Drive as the assistant account → open the invitation (in its Gmail, or the folder's Share dialog) → **Accept**.

   Transferring a folder moves the folder only; the files inside stay owned by the owner (still fully usable by everyone). To move them too, the owner opens each folder, selects all files, and repeats Share → Transfer ownership; the assistant account accepts. Recommend it when the owner might leave the group or is short on storage; otherwise it can wait.

   Done when, after step 4, the root folder's permissions show the assistant account as owner and the owner as editor.
3. **Share the old calendar with it.** The owner, in Google Calendar → Settings → the system calendar → Share with specific people → add the assistant account with **See all event details**. Done when the owner confirms.
4. **Reconnect the owner's connectors.** Walk prerequisites item 3, fix "already connected with the wrong account": disconnect Google Drive and Google Calendar, connect both again as the assistant account. Done when the calendar list's primary calendar carries the assistant account's email and Drive search finds `org.md`.
5. **New system calendar.** Create a calendar owned by the assistant account with the same name (by connector if it can, else the click steps in setup step 2c.6, signed in as the assistant account). Copy every future event from the old calendar into the new one, keeping title, time, description and color. Done when the new calendar is listed and holds the same future events as the old.
6. **Update `org.md`**: `assistant_account` set to the new address, `calendar` to the new calendar's name, the owner's Access row updated. Replace the file per rules → Replace. Done when the downloaded `org.md` shows both changes.
7. **Share the new calendar** with the owner (prerequisites item 5), and ask the owner to share their personal calendar with the assistant account at the level in `org.md`. Then the owner removes the old calendar from their Google Calendar (Settings → the old calendar → Remove calendar → Delete) so events don't appear twice.

Done when the root folder is owned by the assistant account, the new system calendar is listed with the future events, `org.md` names both, and the owner's connectors log in as the assistant account. The owner's scheduled routines need no change.
