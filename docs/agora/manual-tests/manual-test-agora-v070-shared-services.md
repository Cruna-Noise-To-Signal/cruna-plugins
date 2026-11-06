Manual test for Agora 0.7.0, the two modes and shared services mode (request #11, earnest-tozzetto). Release check from agora-spec → Release checks.

Status: ready to run, not yet run. Record results in the boxes and notes below, then fill TO IMPROVE and FACT at the end, as in the earlier manual tests.

## Setup for the test

- **Shared services**: the `cruna-shared-resources` shared drive; two Workspace accounts on its domain, **A** (you, admin and calendar owner) and **B** (`kind: ai` at Join); a test Google Group created as agora-spec → Manual setup step 3 says (label Mailing, Restricted with Who can post → External ticked so it reads Custom, Who can join Only invited users, the Admin console's outside-mail setting ticked); a secondary calendar "Agora test" created by A. Leave B out of the drive, group and calendar until scenario 2 says.
- **Basic**: a copy of an existing basic root (0.6.0, email on), files untouched, with its assistant account, for scenarios 10 and 12. A second copy for scenario 9 (Move).
- **Italian**: `agora-ita` 0.7.0 for scenario 11.
- Install `agora` 0.7.0 and `agora-ita` 0.7.0 from this repo. Run every chat scenario on **Claude** and on **ChatGPT web on a paid plan**, unless it says one platform.
- An outside address (a personal Gmail) to send test mail from.
- Check every result in Drive, Calendar and Gmail yourself, never the AI's word. Mark each check `[x]` when it holds, and write what you saw under Notes.

| Platform | Plan | Tester | Date | Plugin version |
|---|---|---|---|---|
| Claude | | | | |
| ChatGPT | | | | |

## 1. New root in shared services mode

As A, connectors signed in as A. Say "set up Agora".

| Check | Claude | ChatGPT |
|---|---|---|
| Setup asks whether the group uses Google Workspace; yes leads to the admin's steps (the manual setup guide), with menu paths and who does each | [ ] | [ ] |
| Paste the shared drive's URL with a `?usp=…` query: setup takes the id itself (the segment after `/folders/`), creates and trashes a test file there | [ ] | [ ] |
| Paste a wrong folder's URL: setup says A isn't a Content manager of that drive or it's another folder | [ ] | [ ] |
| Paste the Calendar ID: a test event is created and deleted on it (check Calendar's trash/history) | [ ] | [ ] |
| The root folder is created inside the shared drive; `org.md` has `mode: shared-services`, `drive_id` (the URL id), `root_id` (the folder's id), `calendar`, `calendar_owner: <a>`, `inbox` (the group), no `assistant_account`; Members has Kind | [ ] | [ ] |
| Sharing makes no sharing call: A's Access row says `content manager` / `can edit`; B's Member section has unticked admin boxes naming who does each | [ ] | [ ] |
| Email on: item 7's group search; with an empty group, a test email from the outside address, then the search finds it; A's Access Notes `receives mail` | [ ] | [ ] |
| Verify passes: `root_id` walks up to `drive_id`, test file and event round trip, `calendar_owner` is a member | [ ] | [ ] |

Notes:

## 2. Join by a second account, each admin step missing in turn

Connect B's Drive, Calendar and Gmail as B and say "join Agora". Do the admin's steps for B one at a time, saying "join Agora" (or "finish setup") after each.

| Check | Claude | ChatGPT |
|---|---|---|
| B not in the shared drive: Join can't find the root and says both causes (wrong account, or not added to the drive), with who fixes each; nothing written | [ ] | [ ] |
| B in the drive as Viewer only: "In the shared drive" fails on the trash, names item 4 step 2 and the admin; Join stops | [ ] | [ ] |
| B Content manager, calendar not shared: the test event fails, naming item 4 step 4 and A as calendar owner; Join carries on | [ ] | [ ] |
| B not in the group: In the group fails, naming item 4 step 3.3 and the admin | [ ] | [ ] |
| Every step done: B's Members row (Email = B's account, Kind `ai`, `domain free/busy`), Access row, `memories/<b>.md`, every box ticked; no folder or calendar sharing changed; no interest folder created | [ ] | [ ] |

Notes:

## 3. An account that isn't a member

Connect the outside Gmail account's Drive (or a third Workspace account added to the drive) and say "what's pending?".

| Check | Claude | ChatGPT |
|---|---|---|
| Agora says this account isn't an Agora member yet and offers Join; nothing is written | [ ] | [ ] |
| As A and as B, Agora never asks who is talking | [ ] | [ ] |

Notes:

## 4. Replace across members

As B, "log: A's note was wrong, the meeting is Friday" on an instance whose `status.md` A wrote.

| Check | Claude | ChatGPT |
|---|---|---|
| B's rebuild trashes A's old `status.md`: one `status.md` left, no permission error | [ ] | [ ] |

Notes:

## 5. Schedule and ask from both accounts

| Check | Claude | ChatGPT |
|---|---|---|
| As A then as B, "dentist on <date> at 10": both events land on the shared calendar (not A's or B's own) | [ ] | [ ] |
| A clash with an event on A's own calendar is found when A schedules (own calendar in full); for B, A shows as busy only | [ ] | [ ] |
| "What do we have this week?" gives the same Agora events from A and from B | [ ] | [ ] |

Notes:

## 6. Email across two mailboxes

| Check | Claude | ChatGPT |
|---|---|---|
| From the outside address, mail the group; A forwards it to the group too. As A, "file the email I forwarded": filed once; `inbox.md` row with Mailbox `<a>` and Date to the minute | [ ] | [ ] |
| As B, "file the email A forwarded": already handled, no second entry or row | [ ] | [ ] |
| As B, open A's diary entry's email: found by search in B's mailbox | [ ] | [ ] |
| As A, "send Mario a reply": from A's address, the group in CC, in A's voice, no assistant signature; sent only after A's yes | [ ] | [ ] |
| Mario's reply-all lands in the group, so in A's and B's inboxes | [ ] | [ ] |
| "Did we answer Mario?" as A finds the sent mail (sent-mail form); as B, finds it through the group | [ ] | [ ] |
| An email in A's mailbox not sent to the group is never read or filed unless A names it | [ ] | [ ] |

Notes:

## 7. Routines: the tidy-up skip and the overdue flags

| Check | Claude | ChatGPT |
|---|---|---|
| As A type `Run Agora review: daily for <a> in <root>`; root `status.md` gets `**Tidied:** <now>` | [ ] | [ ] |
| Within 60 minutes, as B, `Run Agora review: daily for <b> in <root>`: the tidy-up is skipped (no new trash, filing or ledger rows), the review is updated, the message sent | [ ] | [ ] |
| On a root older than two days, trash yesterday's daily and set `last_tidy_up` three days back: the next review flags the missing daily and the email triage, each with the routines fix | [ ] | [ ] |
| On the 1st before the monthly run, and on a Monday before the weekly is due: no missing-period flag | [ ] | [ ] |
| On a new root with `reviews/` empty: no missing-period flag | [ ] | [ ] |
| `help` → "my review never arrived": shows the same flags first | [ ] | [ ] |

Notes:

## 8. Offboarding the calendar owner

| Check | Claude | ChatGPT |
|---|---|---|
| "Remove A from Agora": blocked until the calendar is transferred, transfer steps given, nothing written | [ ] | [ ] |
| "Just remove A": still blocked, with the reason | [ ] | [ ] |
| A transfers the calendar to B, B accepts; remove A again: `calendar_owner` becomes B, the calendar check passes, each removal handed to its person; Members `removed`, A's section gone; Verify passes | [ ] | [ ] |

Notes:

## 9. Move to shared drive (on a copy of a basic root)

One platform. As the copy's owner, connected as its assistant account. Say "move Agora to a shared drive".

| Check | Claude or ChatGPT |
|---|---|
| `setup.md` gets a `## Move to shared drive` section with ten boxes; stop after step 2 and say "finish setup": it resumes at step 3 | [ ] |
| Step 2 shares the root folder with A's Workspace account; the calendar share is click steps | [ ] |
| After reconnecting as A: `landing/` filed first; move or copy into the shared drive (note which, and whether `update_file` re-parenting worked); the count check compares every folder; no original trashed before it passes | [ ] |
| `org.md`: `mode: shared-services`, new `drive_id`, `root_id` (the new folder), `calendar`, `calendar_owner`, `inbox`; `assistant_account` kept; Members Email = Workspace accounts; `setup.md` sections have the shared services boxes | [ ] |
| Every future `[interest/instance]` event is on the new calendar once, same title, time, description and color; the old calendar unchanged | [ ] |
| Forwarding set by clicks; a mail to the old address from outside reaches the group search; the old address is in the mover's Mail from | [ ] |
| Old root trashed by the assistant account; a search for `org.md` finds the new root only; diary entries and documents all present | [ ] |
| The linked-repo list names each managed `products/` instance (or says none) | [ ] |
| Verify passes in shared services mode; another member's Join matches their Member section | [ ] |

Notes:

## 10. Basic root: the ownership check

On the basic copy, connect Drive as an account that doesn't own `org.md` (shared with it as editor).

| Check | Claude | ChatGPT |
|---|---|---|
| Verify blocks on item 3, naming the mismatch and the fixes (reconnect, transfer, move to a shared drive) | [ ] | [ ] |
| `help` → "duplicate files won't go away": runs item 3 and names the mismatch | [ ] | [ ] |
| A daily review flags the connected account (item 9) with the fix | [ ] | [ ] |
| A `log` that rebuilds a status: the refused trash names the account mismatch as its cause | [ ] | [ ] |

Notes:

## 11. Italian root (one platform)

With `agora-ita`, a shared services root in Italian (or join the test root in Italian as B).

| Check | Claude or ChatGPT |
|---|---|
| The setup question, the manual steps and Join speak Italian with the glossary terms ("modalità servizi condivisi", "Drive condiviso", "Gestore contenuti"); `org.md` keys, `mode` values, Kind, Mailbox, `**Tidied:**` and the `setup.md` box labels stay in English | [ ] |
| `aiuto` → "dove sono i miei file?" answers for the root's mode | [ ] |

Notes:

## 12. Basic-mode non-regression (the founder, before release)

On the **untouched** basic copy, connected as its assistant account, with 0.7.0. Compare each result with 0.6.0 on the same root.

| Check | Result same as 0.6.0 |
|---|---|
| `setup` Verify passes (plus the ownership check, which passes as the assistant account) | [ ] |
| A daily `review` with email on: same mail filed, flagged and ignored; ledger rows with an empty Mailbox and a date only; `**Tidied:**` written | [ ] |
| `schedule` "dentist on <date> at 10": the event on the assistant account's primary calendar | [ ] |
| `email` "anything from <sender>?" and a draft and send, signed as the assistant on behalf of you | [ ] |
| **Sent mail**: "did we answer <someone the assistant emailed>?" finds the sent mail (the basic sent-mail form) | [ ] |
| `ask` "what's pending for <instance>?" and "what do we have this week?" | [ ] |
| Join and Offboard a person: folder shared by Agora, calendar click steps, no Workspace question | [ ] |
| `help` "where are my files?" answers **Shared with me** only | [ ] |

Notes:

## TO IMPROVE

## FACT

- Does `deliveredto:<group>` match group mail in a member's mailbox, or is `list:<group>` needed?
- Does the sent-mail filter form work on both connectors?
- Is the primary calendar's id the account's email on ChatGPT?
- Does the Drive connector expose `owners`, read a shared drive's members, and re-parent a folder into a shared drive?
