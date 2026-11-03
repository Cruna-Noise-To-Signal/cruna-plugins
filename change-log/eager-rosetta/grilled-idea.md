# eager-rosetta — Agora email: grilled idea

Request: [#2 Agora agent can read, draft and send email and report on the inbox](https://github.com/Cruna-Noise-To-Signal/cruna-plugins/issues/2). Grilled 2026-11-03 (one session, rounds Q1–Q19).

## Where it started

The human asked for email access for the Agora agent, aimed mainly at professional roots: (a) the agent reads its own inbox, prepares drafts and sends emails; (b) it reports on important emails that need attention and recaps what it did with email. Open points at filing: whose inbox, when sending needs confirmation, how the connector fits setup and prerequisites, how reporting plugs into `review`.

## Where it concluded

Email is an **optional, per-root capability** built on the platform's Gmail connector, logged in as the **assistant account**. The assistant inbox works like `landing/`: members forward or CC mail to it, the agent files what it can place without guessing and flags the rest. The agent drafts freely but sends only on an explicit confirmation in a conversation; routines never send. Reviews gain an **Email** section. Importance follows a default rule refined by members, and is elicited wherever importance is judged.

## Platform facts (checked 2026-11-03)

Shared, documented on both Claude and ChatGPT: search and read mail, create drafts, send/reply (the client asks for approval before each send by default).

- ChatGPT: sending is web only, on paid plans (Plus, Pro, Business, Enterprise); on Business an admin must turn on write actions. Attachment reading and labels/archive are undocumented. Sources: help.openai.com release notes (2026-06-08), Google app and Scheduled tasks articles. help.openai.com was not directly reachable; facts come from search snippets of official pages.
- Claude: available on all plans; on Team/Enterprise an Owner enables the connector. Attachments are metadata only (name and type), so their content cannot be passed on. Labels supported. Source: support.claude.com/en/articles/10166901 (late Oct 2026).
- Therefore the design does **not** rely on attachment content, labels or archiving.

## Decisions

### Inbox and enablement

1. **Whose inbox (Q1)**: only the assistant account's inbox. Members forward or CC the assistant to share mail. Members' personal inboxes are never read. In a solo root the assistant account is the owner's own account, so it is the owner's inbox (see 19).
2. **Optional per root (Q2)**: `setup` offers email during the interview, noting it suits professional roots best. Recorded in `org.md`; its own box in `setup.md`; an optional item in the prerequisites checklist: "Gmail connector, logged in as the assistant account". A root that declines loses nothing it has today.
3. **Member email addresses (Q13)**: when email is on, `setup` (and Join) asks each member for every address they may write or forward from, recorded in the member's `org.md` entry and editable in conversation. Mail from any other address is never member mail, even when the sender's name matches.

### Sending

4. **Sending policy (Q3)**: the agent drafts freely; it sends only after a member explicitly confirms that exact message in a conversation. Routines never send; they may only draft (see 11).
5. **Who and in whose voice (Q6)**: any member can ask for a draft or a send. Mail always goes out from the assistant account, signed as the assistant on behalf of the member who asked. Every send is logged as a diary entry in the relevant instance (author = that member), or flagged when there is no clear instance.

### Email as knowledge

6. **Inbox as a drop zone (Q4)**: an email forwarded or CC'd by a member is filed as a diary entry in the right instance (author = the forwarding member; the entry quotes sender and subject), in conversation or by the routine's tidy-up when the instance is unambiguous. Otherwise it is flagged under Needs attention, as `landing/` files are. Emails nobody forwarded (newsletters, direct mail to the assistant) are **not** filed, only surfaced in the review when important.
7. **Attachments (Q8)**: the agent cannot read attachment content. When a filed email has attachments, the diary entry lists their names under a "to file" note and the agent asks the member to save them to `landing/` or the instance `documents/`. `file-document` or tidy-up then links them to that entry. Until then, the review flags the entry under Needs attention as "attachments not yet on Drive".
8. **Handled-email ledger (Q9)**: a new root-level **curated** file `inbox.md`, one row per email the agent handled: Gmail message id, date, sender, subject, outcome (filed → diary path, flagged, sent, drafted, ignored). Tidy-up drops rows older than 60 days. It prevents double filing when several members' routines run concurrently (one read to check), feeds the review's Email section, and is a human-readable log. Updated with the normal file update protocol.

### Importance

9. **Default rule (Q10)**: an email needs attention when it asks for an action, reply or decision, or carries a date or deadline, **and** is not bulk mail (newsletters, notifications, marketing). Members refine it in conversation ("always flag emails from Acme", "ignore GitHub notifications"); when a member corrects a flag, the agent offers to record the refinement.
10. **Member mail is always important (Q14)**: an email from a member's address (forward or direct) always needs attention, whatever the criteria:
    - unambiguous forward → filed (6) **and** listed in the review's Email section as filed;
    - ambiguous → top of Needs attention, asking where it goes;
    - an instruction written in it ("reply saying we accept") → the agent **drafts** the reply and flags it for approval; it never sends on an emailed instruction (4).
11. **Proactive drafting by routines (Q11)**: by default routines only flag. A member opts specific senders or interests into proactive drafts with a convention (e.g. "draft replies to client emails in `clients/`"). The review lists drafts waiting for approval.
12. **Where criteria live (Q15)**: root-wide criteria in the **Conventions** table of `org.md`; personal criteria in `memories/<member>.md`. A personal review uses both; the general review uses root-wide criteria, plus each member's own for their per-member update.
13. **Elicitation (Q16, refined by Q17)**: wherever importance is judged and no criteria are stated, the agent elicits them; "no preference" is recorded like a declined suggestion so it is not asked again.
    - `setup`: when email is turned on, one question for the root and one per member in their section.
    - `email` in conversation: the first time it judges importance for a member without criteria or a recorded "no preference".
    - `review` in conversation: the same, when the Email section is not empty.
    - **Routines run headless and never ask.** They end their **chat message** (not the shared review file on Drive) with one invitation line. Capture happens in the next conversation: the member's reply to the review chat (a normal conversation, so `review` records it per 12), or, if they never reply, the next conversational `email` or `review` with that member, which asks directly, once.
    - Monthly review: always shows the current criteria in one line and invites changes ("Important email for you: clients, bank. Reply to change it.").
14. **Invitation state (Q18)**: one line in each `memories/<member>.md` and in `memories/shared.md` for the root: `Email importance: not asked | invited | no preference | <criteria>`. Routines move *not asked* → *invited*. A conversation asks when it reads *not asked* or *invited*, then sets the criteria or *no preference*. The monthly invitation goes out in every state except *no preference*, which gets a quieter "Reply if you want to set criteria".

### Reporting

15. **Recap (Q5)**: "recap" is the agent's own email activity, not members' personal mail. Every review (daily, weekly, monthly) gains an **Email** section: important emails needing attention (also listed under Needs attention), drafts waiting for approval, what was sent and what was filed. Built from `inbox.md`.

### Skill shape

16. **One new skill, `email` (Q7)**: read, search, draft, reply, send, file an email. Small additions to:
    - `rules`: email conventions (sending policy, ledger, importance rule, member mail, invitation state), `inbox.md` as a curated file, templates;
    - `setup`: optional email step, member addresses, criteria questions, `setup.md` box, migration note (19);
    - `review`: tidy-up processes the inbox (file / flag / ledger), Email section, invitation line;
    - `help`: email FAQ and troubleshooting from the prerequisites checklist.

### Limits and release

17. **Out of scope (Q12)**, recorded in `future-extensions.md`: ChatGPT's "run a task when a new Gmail message arrives" trigger; moving, labelling or archiving mail (undocumented or uneven across platforms); reading attachment content.
18. **Prerequisites and release checks (Q12, Q17)**: the prerequisites checklist notes the platform limits (ChatGPT sends from web only on paid plans; Business needs an admin to enable write actions; Claude Team/Enterprise needs an Owner to enable the connector). Release checks add a live Gmail test on both platforms as the assistant account (search, draft, send with confirmation) and a check that a member can reply to a scheduled review's chat and have it captured.
19. **Solo root (Q19)**: the inbox is the owner's own mail.
    - Filing happens only when the owner asks in a conversation; tidy-up never auto-files from a personal inbox.
    - Reviews still surface important email by the owner's criteria; member mail (10) does not apply.
    - The solo → dedicated migration step says that afterwards the agent sees only mail sent or forwarded to the new assistant address; `inbox.md` carries over unchanged.

## Vocabulary to add (spec and `glossary-it.md`)

- **Assistant inbox**: the assistant account's Gmail inbox; the only inbox the agent reads.
- **Member mail**: an email from an address listed for a member in `org.md`.
- **`inbox.md`**: the root's handled-email ledger, a curated file.
- **Email importance**: the criteria (root-wide in Conventions, personal in memories) and its invitation state.

## For the spec

Update `docs/agora/agora-spec.md`: Drive layout (`inbox.md`), `org.md` (email on/off, member addresses), `setup.md` boxes, Accounts and calendar (Gmail connector as the assistant account), skill table (`email`), prerequisites checklist, Routines (tidy-up inbox step, Email section, invitation line), Memories (invitation state), Release checks, Out of scope. Both language versions; version bump on both (minor: 0.4.0).
