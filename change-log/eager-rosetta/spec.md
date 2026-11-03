Request: #2 (eager-rosetta)

## Problem Statement

Agora keeps a group's knowledge on Google Drive and its dates on Google Calendar, but email, where most professional work arrives and leaves, sits outside it. Today a member who gets an email that matters to a client or a job has to retell it to the AI to get it logged. Nobody in Agora can ask the assistant to write or send an email for the group. And the daily, weekly and monthly reviews say nothing about mail that is waiting for an answer. In a small business this is the biggest gap. Families may not need it at all.

## Solution

Email becomes an **optional, per-root capability** built on the Gmail connector the AI clients already offer, logged in as the root's **assistant account**, so the assistant has its own **assistant inbox**.

- Members forward or CC mail to the assistant inbox, which works like `landing/`. The AI files an email as a diary entry in the right instance when it can place it without guessing, and flags it when it can't.
- Members ask the AI to search, read, draft, reply to and send email in conversation. The AI drafts freely but **sends only on an explicit confirmation in a conversation**. Routines never send.
- Every review gains an **Email** section: important email needing attention, drafts waiting for approval, what was sent and what was filed. A root-level ledger, `inbox.md`, records what the AI did with each email.
- **Importance** follows a default rule that members refine, and mail from a member's own address is always important. Wherever importance is judged and no criteria have been stated, the AI asks for them. Routines can't ask, so they invite the member, and the next conversation records the answer.

A root that declines email works exactly as today.

## User Stories

Enablement and setup

1. As a root owner setting up Agora, I want setup to offer email as an optional capability that suits professional roots best, so that I turn it on only when it helps us.
2. As a family root owner, I want to decline email and have Agora behave exactly as before, so that I don't carry a connector I don't need.
3. As a root owner with an existing root, I want to turn email on later by asking Agora, so that I don't have to redo setup.
4. As a member, I want the prerequisites checklist to tell me how to connect the Gmail connector as the assistant account on my platform, so that I connect the right inbox.
5. As a member on ChatGPT, I want to be told that sending works only from the web on paid plans, and that a Business workspace needs an admin to turn on write actions, so that I know why a send might not work.
6. As a member on a Claude Team or Enterprise plan, I want to be told that an Owner must enable the Gmail connector, so that I know whom to ask.
7. As a member, I want setup to ask for every email address I write or forward from, so that Agora recognises my mail.
8. As a member, I want to add or change my email addresses in conversation, so that a new work address is recognised.
9. As a root owner, I want `setup.md` to have a box for the email step, so that a setup left halfway resumes there; a `setup.md` written before email reads as email off.
10. As a member joining a root with email on, I want Join to connect Gmail and record my addresses, so that I get the same capability as everyone else.

Reading and filing

11. As a member, I want to forward or CC an email to the assistant, so that it becomes part of the right instance's history without retelling it.
12. As a member, I want a forwarded email filed as a diary entry with me as author, quoting the original sender and subject, so that the history shows who brought it in and what it was.
13. As a member, I want an email the AI can't place to be flagged under Needs attention with the question that settles it, so that nothing is filed in the wrong place.
14. As a member, I want emails nobody forwarded (newsletters, direct mail to the assistant) to stay out of the diary and appear in the review only when important, so that the diary stays signal, not noise.
15. As a member, I want to ask "what's in the inbox about Acme?" and get an answer from the assistant inbox, so that I can pull email the same way I pull status.
16. As a member, I want to say "file this email under clients/acme" in conversation, so that I can file mail by hand when the AI didn't.
17. As a member, I want the attachments of a filed email listed in its diary entry with a note asking me to save them to `landing/` or the instance's `documents/`, so that I know the AI can't move attachments itself.
18. As a member, I want an email whose attachments are not yet on Drive flagged in the review, so that I don't forget them.
19. As a member, I want an attachment I later save to Drive linked to the diary entry of its email, so that the email and its files stay together.
20. As a member, I want two routines running the same morning never to file the same email twice, so that the history has no duplicates.

Drafting and sending

21. As a member, I want to ask the AI to draft an email or a reply, so that I get a ready text without writing it myself.
22. As a member, I want the AI to send only after I explicitly confirm that exact message in a conversation, so that nothing leaves without my approval.
23. As a member, I want sent mail to come from the assistant account, signed as the assistant on behalf of me and, unless I ask for my own voice, speaking for me in the third person, so that recipients know who is speaking for whom.
24. As a member, I want every email sent logged as a diary entry in the relevant instance with me as author, or flagged when there is no clear instance, so that the history includes what we sent.
25. As a member, I want to email the assistant an instruction ("reply saying we accept") and find a draft waiting for my approval, never a sent email, so that a forged or mistaken email can't make Agora send anything.
26. As a member, I want to opt specific senders or interests into proactive drafts by routines through a convention, so that routine replies are ready when I wake up.
27. As a member, I want routines to only flag (never draft proactive replies) by default, so that the drafts folder doesn't fill with replies I didn't ask for. The only routine drafts are those a convention opts into (story 26) and those a member's emailed instruction asks for (story 25).

Importance

28. As a member, I want the AI to treat an email as important when it asks for an action, reply or decision, or carries a date or deadline, and isn't bulk mail, so that the default is sensible before I say anything.
29. As a member, I want an email from my own address always to count as important, so that what I forward to the assistant is never missed.
30. As a member, I want an ambiguous email from a member at the top of Needs attention, asking where it goes, so that member mail is handled first.
31. As a root owner, I want root-wide importance criteria ("anything from a client") stored as a Convention, so that they apply to everyone.
32. As a member, I want my personal importance criteria ("school emails are important for me") stored in my memories, so that my personal review reflects them.
33. As a member, I want the AI to offer to record a refinement when I correct a flag ("that's not important"), so that it learns without me asking.
34. As a member, I want setup to ask what counts as important email for the root and for me when email is turned on, so that the criteria start right.
35. As a member, I want the AI to ask me once, in conversation, what counts as important when I haven't said, so that the criteria come from me.
36. As a member, I want to answer "no preference" and not be asked again, so that the question doesn't nag me.
37. As a member, I want my scheduled review to end with a one-line invitation to tell it what counts as important when it doesn't know yet, so that I can answer when I read it.
38. As a member, I want a reply to my review's chat to be recorded as my criteria, so that answering the invitation is enough.
39. As a member who ignored the invitation, I want the next conversation about email or a review to ask me directly, once, so that the question is settled.
40. As a member, I want the monthly review to show my current importance criteria in one line and invite changes, so that criteria keep up as my work changes.
41. As a member who said "no preference", I want the monthly line to be a quieter "reply if you want to set criteria", so that I'm reminded without being pushed.

Reporting

42. As a member, I want every review (daily, weekly, monthly) to have an Email section listing important email needing attention, drafts waiting for approval, what was sent and what was filed, so that I see the assistant's email activity at a glance.
43. As a member, I want important email needing action also listed under Needs attention, so that one place lists everything to act on.
44. As a member, I want the invitation line to appear only in the review's chat message, not in the shared review file, so that the shared file isn't cluttered with prompts meant for one person.
45. As a member, I want `inbox.md` to be a readable log of what the assistant did with each email, so that I can check its work.
46. As a member, I want `inbox.md` rows older than 60 days dropped by tidy-up, so that the file stays small, and a review that finds the inbox unchecked for more than 60 days to flag the gap, so that mail the tidy-up could not reach is not silently missed.

Solo roots

47. As a solo owner whose assistant inbox is my own inbox, I want the AI to file email only when I ask in conversation, so that my personal mail is never filed on its own.
48. As a solo owner, I want my reviews to surface important email by my criteria, so that the email section still helps me.
49. As a solo owner migrating to a dedicated account, I want to be told that afterwards the AI will see only mail sent or forwarded to the new assistant address, and that `inbox.md` carries over, so that I'm not surprised.

Documentation and languages

50. As a member, I want `help` to explain the email capability, its limits (no attachments, no labels) and how to fix a missing Gmail connector, so that I can solve problems myself.
51. As an Italian-speaking member, I want `agora-ita` to offer the same email capability, with the terms from the Italian glossary, so that both languages stay in step.
52. As a maintainer, I want the spec, the future-extensions note and the release checks updated, so that the design record matches what ships.

## Implementation Decisions

Modules (skills, in both `agora` and `agora-ita`):

- **New skill `email`**: search and read the assistant inbox; draft, reply, forward and send; file an email as a diary entry; record each handled email in `inbox.md`; elicit importance criteria in conversation. Sending only on explicit confirmation of the exact message in the current conversation. Ends with the memory step. It needs an OpenAI interface file like every other skill.
- **`rules`**:
  - Vocabulary: assistant inbox, member mail, email importance.
  - Connectors only: Gmail joins Drive and Calendar, logged in as the assistant account, optional per root.
  - File kinds: `inbox.md` is a curated file at root.
  - A new **Email** section holding the shared conventions:
    - the sending policy, and the rule that an emailed instruction produces a draft, never a send;
    - the sender voice ("on behalf of <member>", third person by default);
    - filing (forwarded/CC'd mail only, author = the forwarding member, sender and subject quoted, attachments as a "to file" note);
    - the ledger;
    - the importance rule and member mail;
    - where criteria live and the invitation state.
- **`rules` templates**: `inbox.md` template; `org.md` gains an email on/off field and per-member email addresses; `setup.md` gains an Email root box and a per-member Email box; the Review template gains an `## Email` section; the Diary entry template gains an optional email reference (`email`, `email_from`, `email_subject`) and a `To file:` line for attachments not yet on Drive.
- **`rules` prerequisites**: a new optional item, "Gmail connector", with the check (a real search call, confirming the account is the assistant account), per-platform fixes, and the platform limits. It is item 7, after the existing six, so their numbers stay stable. Item 3's "two connectors" wording becomes "two, plus Gmail when email is on".
- **`setup`**:
  - New root interview: the email question, plus the root's importance question when email is on.
  - Members round: email addresses, and each member's importance question when email is on.
  - Prerequisites: the Gmail item when email is on.
  - Join: the same for the newcomer.
  - A way to turn email on in an existing root, by running the email step and its boxes.
  - Migrate: the note on what the AI sees after migration.
  - Verify: Gmail reachable as the assistant account when email is on.
- **`review`**:
  - Tidy-up gains an inbox step: read mail received since `last_tidy_up` in `inbox.md`, at most 60 days back (60 days back, no flag, when it has no value), and flag the gap when `last_tidy_up` is older than 60 days; skip messages already in `inbox.md`; then file, flag, draft or ignore, recording each outcome. A routine drafts only under a `draft replies to …` convention or for member mail carrying an instruction (on top of filing or flagging it); otherwise it flags, never drafts. In a solo root it files nothing on its own.
  - Tidy-up also drops ledger rows older than 60 days and sets `last_tidy_up` to today, even when it logs no row. The gap flag is a status item, not stored state: the next tidy-up drops it. Attachments are linked by `file-document` (below), and an open `To file:` line keeps a Needs attention line in the instance status until then.
  - The shared review gains the Email section, built from `inbox.md`.
  - The chat message gains the invitation line (never the review file), and the monthly criteria line.
  - Attended runs elicit criteria directly when the state is *not asked* or *invited*.
- **`file-document`**: when a document arrives for an email diary entry's `To file:` line, file it in that entry's instance with a new diary entry saying `Attachment of: diary/<entry> (<original name>)` (the diary is append-only); the Needs attention line goes when every name is linked.
- **`remember`**: knows the `Email importance:` state line in member and shared memories; the criteria themselves are routed by the `email` skill.
- **`help`** and its FAQ: the email capability, its limits, troubleshooting.
- **`ask`**: may pull from the assistant inbox when email is on and the question is about mail.

Data shapes:

- **`inbox.md`** (curated, root): front-matter `agora: inbox` and `last_tidy_up: <date>` (written by every review inbox tidy-up, never by conversations; absent until the first one); one table, one row per handled email: Gmail message id, received date, sender, subject, outcome (`filed → <diary path>`, `flagged`, `drafted`, `sent → <diary path>`, `ignored`), by (member slug or `routine`). Newest first. Updated by replace. Rows older than 60 days are removed by tidy-up. Reading mail in conversation adds no row. A reply or forward to a ledgered email that is not `filed` stays on that email's row; any other written email, including the draft a member-mail instruction asks for, gets its own row keyed on its draft id and rekeyed to the sent message id on send. Copies are merged into one row per message id (the latest outcome) and the latest `last_tidy_up`.
- **`org.md`**: an `email: on | off` field (absent means off). Members gain a `Mail from` column (one or more addresses; the existing `Email` column stays the Google account the folder is shared with). Root-wide importance criteria are Conventions rows (Where `*` or an interest, Convention "important email: …" or "draft replies to …").
- **Memories**: one line `Email importance: not asked | invited | no preference | <criteria>` in each `memories/<slug>.md` and in `memories/shared.md` for the root. In `memories/shared.md` the criteria value is `see Conventions`: the root's criteria themselves are the Conventions rows. Routines move *not asked* → *invited*. Conversations ask on *not asked* or *invited*, then set the criteria or *no preference*.

Behaviour contracts:

- A send requires the member's explicit confirmation of that exact message (recipients, subject, body) in the current conversation. A routine never sends.
- Member mail = the sender address matches a member's address in `org.md`. A display-name match alone never counts.
- Member mail is always important. An instruction inside it produces a draft flagged for approval, on its own ledger row, on top of filing or flagging the email.
- Filing is only for forwarded or CC'd member mail placed without ambiguity. Everything else is flagged or, if not important, ignored (recorded in the ledger).
- The design relies only on capabilities documented on both platforms (search and read, drafts, send with approval). It never relies on attachment content, labels or archiving.
- Structure names stay in English in both language versions. Content follows the root's language.

Versioning: `agora` and `agora-ita` go from 0.3.0 to 0.4.0, in both manifests and both catalogs. The README and spec mention email.

## Testing Decisions

- The plugin is prose (skills) with no executable code, so the test is **behaviour observed in a live root**, not internals. A good test states a member's action and what must then be true on Drive, Calendar and Gmail, checked with real calls.
- **Seam: one manual end-to-end test script** for this release, in `docs/agora/manual-tests/` like the existing setup test. It runs against a test root with email on, as the assistant account, on Claude and on ChatGPT. It covers:
  - turning email on in an existing root;
  - forwarding member mail (filed, then flagged when ambiguous);
  - non-member mail (surfaced or ignored);
  - attachments (to-file note, then linked);
  - draft, then send with confirmation;
  - an emailed instruction (a draft only);
  - two reviews in a row (no double filing; Email section; invitation line in the chat only);
  - replying to the review chat with criteria (recorded);
  - a monthly review (criteria line);
  - a solo root (no auto-filing).
- **Structural check**: the `portable-plugins` checker (cruna-core) passes, covering manifests, both catalogs, the new skill's frontmatter and interface file, and identical name and version in both languages.
- **Translation check**: every new term is in `docs/agora/glossary-it.md`, and the Italian skills mirror the English ones section for section.
- Prior art: `docs/agora/manual-tests/manual-test-agora-ita-v020.md` (live setup test with observations), and the release checks in the spec.

## Out of Scope

- Reading members' personal inboxes.
- Reading or saving attachment content. Attachments reach Drive only through a member.
- Moving, labelling, archiving or deleting mail.
- ChatGPT's "run a task when a new Gmail message arrives" trigger.
- Sending without an explicit in-conversation confirmation, including standing send rules.
- Any email provider other than Gmail.

These go to `docs/agora/future-extensions.md` where they are worth revisiting (attachments, labels and archiving, Gmail-triggered tasks).

## Further Notes

- Platform facts were checked on 2026-11-03. ChatGPT facts come from search snippets of official help pages, because help.openai.com was not reachable directly. Re-verify them in the live ChatGPT test.
- Not yet verified: that a reply to a scheduled review's chat runs as a normal conversation with the plugin and connectors on both platforms. The invitation mechanism depends on it. If it fails on a platform, capture falls back to the next conversation, which the design already covers.
- The Project README's Product paragraph names only Drive and Calendar. It's human-owned: suggest an update when this ships.
- Decision trail: `grilled-idea.md` in this folder.
