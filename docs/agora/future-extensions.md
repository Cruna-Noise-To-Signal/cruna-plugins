# Agora — Future extensions

Ideas deliberately left out of the current design. Each records why it waits and what would have to be true to build it. When one is picked up, move it into [agora-spec.md](agora-spec.md) in the same commit that implements it.

## Google Sheets trackers

- **What**: working files that are real Google Sheets, with the AI appending rows, instead of CSV files rewritten in full.
- **Why it waits**: the Google Drive connector can only create and replace whole files; editing a Sheet's cells needs the separate Google Sheets connector. That adds a third connector to the prerequisites, and its write capability is unverified on both Claude and ChatGPT.
- **Today**: trackers are CSV working files (spec → Working files). A CSV opens as a table in Drive and is fine up to a few hundred rows.
- **To build**: verify the Sheets connector can append rows and read ranges on both platforms; add it as an optional prerequisite; let a Convention choose `sheet` instead of `csv` per tracker; teach `log` to append rows and `ask`/`review` to read them.

## Memory reflection routine

- **What**: a daily routine that reads the day's conversations and stores what it learned about members in `memories/`.
- **Why it waits**: no platform lets a scheduled run read past conversations (checked 2026-10-28).
- **Today**: memories are captured during the conversation by each skill's memory step and the `remember` skill.
- **To build**: a scheduled-run API for conversation history on Claude and ChatGPT.

## Git-backed interests

- **What**: for engineering teams, an interest is a git repository; the AI keeps status and memories aligned with commits, pull requests and open issues.
- **Why it waits**: different source of truth and tooling (GitHub connector, code-aware summaries); the family and small-business cases come first.
- **To build**: map instance ↔ repository or milestone, diary ↔ commits/PRs/issues, and define how status rebuilds read them.

## Local filesystem roots

- **What**: a root on a person's own computer instead of Google Drive, for someone who shares with nobody but can reach the folder from everywhere they work.
- **Why it waits**: needs a client with local file access (Claude Code, Claude desktop, Codex); the web and mobile apps can't reach it, and routines would run only where the files are.
- **To build**: abstract "working with Drive" in `rules` into a storage section with a Drive and a local variant; same layout and file kinds.

## Email attachments

- **What**: the AI reads an email's attachments and saves them to the instance's `documents/` itself, instead of asking a member to.
- **Why it waits**: the Gmail connectors on Claude and ChatGPT don't document reading attachment content or saving it to Drive (checked 2026-11-03), and Agora relies only on what both platforms document.
- **Today**: a filed email lists its attachments on a `To file:` line; the member saves them to `landing/` or `documents/`, and `file-document` links each one to the email's diary entry (spec → Email → Filing).
- **To build**: verify both connectors can fetch attachment bytes and that the Drive connector can create a file from them; then let filing save attachments and drop the `To file:` step.

## Gmail labels and archiving

- **What**: the AI labels, archives or moves mail in the assistant inbox as it handles it (for example an `Agora/filed` label), so the inbox itself shows what is done.
- **Why it waits**: label and archive actions are not documented on both platforms' Gmail connectors (checked 2026-11-03); the `inbox.md` ledger does the same job portably.
- **Today**: the ledger records every handled email and its outcome; the mailbox is never changed.
- **To build**: verify label/archive tools on both platforms; mirror ledger outcomes as labels, keeping the ledger as the source of truth.

## Gmail-triggered tasks

- **What**: a task that runs when a new email arrives, filing or flagging it within minutes instead of at the next review.
- **Why it waits**: only ChatGPT offers a "run when a new Gmail message arrives" trigger; Claude has no equivalent, and Agora keeps both platforms on the same design (checked 2026-11-03).
- **Today**: the review's inbox tidy-up handles new mail on the daily, weekly and monthly schedule; members can ask in conversation at any time.
- **To build**: an equivalent trigger on Claude, or an accepted ChatGPT-only extra that runs the same inbox tidy-up rules.
