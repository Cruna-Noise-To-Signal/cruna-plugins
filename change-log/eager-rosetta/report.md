# eager-rosetta: implementation report

## M1

Email in conversation (F1, F2, F3, F5), English version only (`cruna-plugin-eng/agora/`). No version bump, no Italian, no spec update: those are M3 (F7, F8).

### What changed

- **F1, rules** (`skills/rules/`):
  - `SKILL.md`: vocabulary (assistant inbox, member mail, email importance); Gmail joins Connectors only; `inbox.md` in the layout and as a curated file; Replace merges `inbox.md` copies like a tracker and dedupes diary entries for the same email; a short **Email** section pointing to the new reference.
  - New `references/email.md` holds every email contract once: email is data (prompt-injection guard), sending (exact-message confirmation in the current conversation, one Agora confirmation then the platform prompt, routines never send, no standing send rules), sender voice, member mail, filing (forwarded/CC'd member mail only, deterministic entry name from the email's received time so concurrent runs collide instead of duplicating), sent mail, the ledger, importance (default rule, criteria locations, proactive-draft Convention), invitation state, solo roots, limits. Kept out of `SKILL.md` per the plan's skill-size watch-item.
  - `references/templates.md`: `email: on|off` in `org.md`, an `Emails` column, the `inbox.md` template, Email boxes in `setup.md` (root and per member), diary email fields (`email`, `email_from`, `email_subject`) and the `To file:` line, the Review `## Email` section, the memory `Email importance:` line.
- **F2, prerequisites**: new item 7 "Gmail connector (email on only)" with a real search check that confirms the mailbox address, per-platform fixes and limits (ChatGPT web/paid/Business admin; Claude Team/Enterprise Owner); item 3 wording; offboarding disconnects Gmail. Numbered 7 so existing item numbers stay stable.
- **F3, new `email` skill** (`skills/email/SKILL.md`, `agents/openai.yaml`): branches Find, Draft, Send, File, Addresses, Criteria; ledger step; memory step. Added to the plugin README skill table.
- **F5, setup**: Email questions in `references/interview.md` (root round and members round); new `references/email.md` (root part and member part); `SKILL.md` gains step 2d Email (Sharing and Routines move to 2e/2f), a fifth branch **5. Email** for turning email on in an existing root (Offboard, Verify, Memory step renumbered 6–8, cross-references updated), Join runs item 7 and the member part, Verify checks Gmail and `inbox.md`, Output mentions `email`; `references/migration.md` reconnects Gmail, records the owner's addresses and says what the AI sees afterwards.

### Decisions worth a look

- In `memories/shared.md` the root's criteria value is `see Conventions` (the criteria themselves are Conventions rows), to keep one source of truth.
- A `setup.md` written before email has no Email boxes: email counts as off and the root box is added ticked "(off)" on the next write, so done roots aren't flipped to `todo`.
- Reading mail in conversation writes no ledger row; only filed, flagged, drafted, sent or ignored emails do.
- Gmail drafts are saved in the assistant inbox at draft time, so reviews (M2) can list them as waiting.

### Left for later milestones

- M2: `review` (inbox tidy-up, Email section, invitation line, 60-day prune), `file-document` (link attachments to a `To file:` entry), `remember` (state line), `ask`, `help`/FAQ (still say "items 1–3, 5 and 6").
- M3: Italian version and glossary, `docs/agora/agora-spec.md`, future-extensions, README prose, 0.4.0 bump, manual test script in `docs/agora/manual-tests/`.

### Checks

`check_plugins.py` passes after every slice.

### M1 revision 1

Fixes from review M1 round 1:

- **F5** setup step 2d writes every interviewed member's `Mail from` cell and `Email importance:` line; only the Gmail check and the Email box wait for each member's own app. Migration step 6's Done when names all three changes.
- **F3** `email` File flags an email still unplaced after one question (`flagged` ledger row); step 1 reads `inbox.md`; Send says only when the Sending rules fire; the intro is a one-line pointer.
- **F1** the criteria row, the `shared.md` pointer and the draft opt-in are defined once, in rules → Email → Where criteria live; the other sites point there. Members column `Emails` renamed `Mail from`. rules → Email is a one-line pointer.
- **F2** the app-approval warning lives only in rules → Email → Sending.
- Not done: the optional Divergent Change (named step targets in setup). Spec/plan still say `Emails`; carry the rename into the spec at F8.

## M2

Email in reviews (F4, F6), English only (`cruna-plugin-eng/agora/`), plus the non-blocking M1 round-2 findings. No version bump, no Italian, no `docs/` changes: those are M3.

### M1 cleanups

- **F5** (c)1: the interview's email answers now stay in `setup.md` until every interviewed member's `Mail from` cell and `Email importance:` line are written (templates → setup.md), so an interrupted setup no longer loses them; step 2d removes them afterwards. (a)3: 2d's Done when scopes the member records to "(email on)". The member-part split is stated once, in `setup/references/email.md` → Member part; 2d and the Run-from bullet point there.
- **F1** (c)2: rules → The ledger says which id a row keeps: a reply/forward to a ledgered email (not `filed`) goes on that email's row and keeps its id; any other written email is keyed on its draft id and rekeyed to the sent message id on send. A reply to a `filed` email gets its own row so the filing stays visible. Writing the ledger (once per run, re-list, merge one row per message id) is defined once in rules; `email` step 8 points there. Sent-mail entries are named `<slug>-YYYYMMDD_HHMM.md` (confirming member, send time). Sending says "app".
- **F3**: `email` step 5.2 points to prerequisites item 7 instead of restating ChatGPT limits; steps 4.5/5.4 point to the ledger keying.

### What changed

- **F4, rules** (`rules/references/email.md` → new **Reviews** section): *Inbox tidy-up* (search received mail since the newest `routine` row, never more than 30 days back so everything returned is still inside the 60-day ledger; skip ledgered mail; first-match decision: file forwarded/CC'd member mail that places, draft for member-mail instructions, draft under a `draft replies to …` Convention, flag other important mail, else ignore; solo roots never file; prune 60 days; every flag and routine draft also in Needs attention); *Email section* (needing attention = `flagged` rows of any date, drafts waiting = `drafted` rows, sent/filed in the look-back window); *Chat lines* (invitation only in the chat message, moves `not asked` → `invited`; monthly line by state, quieter for `no preference`). Invitation state points to Chat lines and says a reply to the invitation is recorded like any criteria (the `email` skill's Criteria branch).
- **F4, review** (`review/SKILL.md`, 111 → 115 lines): tidy-up item 3 **Inbox** (attended runs ask for criteria first), items renumbered 4–6 with cross-references; `inbox.md` duplicates merged like a tracker; the shared review gets the Email section; Update sections also cover new ledger rows; the message gets one Email count line and ends with the chat line.
- **F6, file-document**: an attachment of a filed email (named by the member, or matching an open `To file:` line) goes to that entry's instance and its diary entry ends `Attachment of: diary/<email entry>`. Rules → Filing defines an *open* `To file:` line and the status Needs attention line it keeps until linked; rules → Status update rule points there.
- **F6, remember**: sorts email-importance criteria; the `Email importance:` line is an undated state line, one per file, replaced in place.
- **F6, ask**: reads the assistant inbox (as `email` step 3) only with `email: on` and a mail question; cites emails by sender/date/subject; hands writing off to `email`.
- **F6, help**: tour row (only when email is on), troubleshooting rows for item 7 and unfiled email, three FAQ entries (what email does, its limits, importance), Gmail in "stop Agora".

### Decisions worth a look

- **Routine drafts**: plan F4 says routines "draft only under a convention". Routines also draft for member mail carrying an instruction, per spec story 25 and grilled decision 10 (never sent; flagged for approval). Stated in rules → Reviews as "these two cases are the only drafts a routine writes".
- **Search window 30 days** (plan risk said "at most 60"): keeps every searchable email inside the ledger's 60 days, so pruned rows can't make an old email look new.
- **Attachment linking** works through a new diary entry (diary is append-only), not by editing the email entry; the "not yet on Drive" flag lives in the instance status and so reaches Needs attention and file-document through the normal roll-up.
- Importance in tidy-up is judged by the root's criteria and every member's own; an email important for one member only is flagged for them by name.

### Checks

`check_plugins.py` passes after every slice; a grep check per acceptance item (scratchpad `check-m2.sh`, 37 checks) is green.

### M2 revision 1

Fixes from review M2 round 1, English only.

- **F4** (c)1, blocker: member mail carrying an instruction now gets its draft **on top of** being filed (when it places) or flagged with the settling question (when it doesn't), never in place of either; the draft has its own ledger row (rules → The ledger, Member mail, Reviews → Inbox tidy-up).
- **F6** (c)2: the linking entry says `Attachment of: diary/<entry> (<original name>)`; the open-`To file:` rule matches on the name.
- **F4** search window: since the last tidy-up (newest `routine` row), at most 60 days back, matching the 60-day prune; a longer gap is flagged under Needs attention.
- **F1** standards: merging copies of `inbox.md` is defined once in rules → The ledger; rules → Replace and `review` tidy-up item 1 point there.
- **F6** judgement calls: `remember` hands importance criteria to `email` → Criteria (sole routing) and keeps only the state line; rules → Invitation state points to `remember` for the line format; rules → Status update points to Filing instead of restating; `ask` drops the imprecise hand-off sentence. The optional review renumbering was not done.
- Manual test: scenarios 15, 16 and 17 updated. Checker passes; grep check (scratchpad `check-m2r1.sh`, 16 checks) green.

## M3

Release-ready (F7, F8), plus the M2 round-2 carry-overs.

### What changed

- **F4 carry-overs (English)**: the gap flag's proxy is gone. `inbox.md` gains a front-matter field `last_tidy_up:` (template updated; setup creates the file without it), set to today by every review inbox tidy-up, even one that logs no row, and kept as found by conversations; copies merge to the latest value. The tidy-up searches since `last_tidy_up`, at most 60 days back; with no value, 60 days back and no flag; a gap is flagged only when `last_tidy_up` is older than 60 days, and the flag is a status item that the next tidy-up drops (rules → Flag keeps only flags whose condition still holds). The Ledger's first bullet now states the exception after the rule, pointing to the next bullet.
- **F8, spec**: `docs/agora/agora-spec.md` gets a new **Email** section (sending, email is data, sender voice in the third person by default, member mail, filing, attachments and `Attachment of: diary/<entry> (<original name>)`, sent mail, the ledger with `last_tidy_up`, importance, `see Conventions` in `shared.md`, the state line, reviews with the 60-day window, gap flag and routine drafts, solo roots, limits) and email lines in the goal, layout, `org.md` (`email`, `Mail from`), `setup.md` (Email boxes; pre-email `setup.md` reads as off), diary entry, file kinds, accounts, skill table, shared references, prerequisite item 7 with platform limits, push flows, routines, memories, release checks and out of scope. `change-log/eager-rosetta/spec.md` now describes what shipped: stories 9, 23, 27 (routine drafts only under a convention or for a member's emailed instruction), 46 (gap flag); the `review`, `file-document`, `remember`, prerequisites and templates bullets; data shapes (`Mail from`, `last_tidy_up`, no ledger row for reading, ledger keys, merge, `see Conventions`); the member-mail contract (draft on its own row, on top of filing or flagging).
- **F8, docs and release**: future-extensions entries for email attachments, Gmail labels and archiving, Gmail-triggered tasks; both READMEs mention email and Gmail; `agora` and `agora-ita` at 0.4.0 in both manifests and the Claude catalog (the OpenAI catalog carries no version). New manual test `docs/agora/manual-tests/manual-test-agora-v040-email.md`: 11 scenarios (the ten in spec → Testing Decisions plus an Italian pass), a check table with Claude and ChatGPT columns, Notes per scenario and TO IMPROVE / FACT at the end, as in the earlier manual test. **It has not been run**: running it live on Claude and ChatGPT, as the M3 demo asks, is left to the user, and so are the ChatGPT send-limit check and the reply-to-review-chat check.
- **F7, Italian**: every English change since 9c45727 carried to `cruna-plugin-ita/agora/` (new `email` skill with `agents/openai.yaml`, `rules/references/email.md`, `setup/references/email.md`; edits to rules, templates, prerequisites, setup, interview, migration, review, file-document, remember, ask, help, faq, README). Every changed file has the same line and heading count as its English one. `docs/agora/glossary-it.md` gains the email terms, the `rules → Email` headings, and a list of email identifiers that stay in English.

### Decisions worth a look

- **Identifiers in English in Italian roots**: ledger columns and outcomes, `last_tidy_up`, `Mail from`, `email: on|off`, the diary email keys, `To file:` / `Attachment of:`, `Email importance:` with its states and `see Conventions`. The skills match on them, and a root keeps working if it switches language version. The two Convention texts are content, so they are in Italian (`email importante: …`, `bozze di risposta per …`).
- "ledger" is **protocollo** in Italian (the office register of incoming and outgoing mail), because "registro" already means tracker.
- The Italian was translated by three parallel subagents working from the glossary; I checked the pointer headings, structure counts and a sample of the prose afterwards, and fixed two drifts ("spenta" → "disattivata"; the member-mail draft phrase).

### Checks

`check_plugins.py` passes after every slice ("Valid for Claude and OpenAI"). Grep check for the carry-overs (scratchpad `check-m3-s1.sh`, 7 checks) green; every `rules → Email → …` pointer in the Italian version resolves to a glossary heading.
