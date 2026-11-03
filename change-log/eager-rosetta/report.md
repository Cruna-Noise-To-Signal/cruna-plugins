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
