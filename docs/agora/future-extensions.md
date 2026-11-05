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

## Google Tasks

- **What**: each member's personal to-dos mirrored into their own Google Tasks list, so they show in Gmail, Calendar and the Tasks app.
- **Why it waits**: neither Claude nor ChatGPT has an official Google Tasks connector, and Tasks is not documented as reachable through the Calendar connector; the only route is a third-party MCP server as a custom connector, which may not run in scheduled tasks. Tasks lists are personal per account and cannot be shared, so they cannot hold the shared (`everyone`) to-dos. Checked 2026-11-03 from vendor blogs, since the official help pages were not reachable: read as "not documented", not "confirmed absent".
- **Today**: to-dos are append-only events in `todos/` with the derived `todo.md` list (spec → To-dos); a member asks for a calendar event for a to-do through `schedule`.
- **To build**: an official Tasks connector on both platforms that works in scheduled runs; then a one-way mirror of each member's personal to-dos into their own Tasks list, keeping `todos/` as the source of truth.

## Documents for products

- **What**: a product instance holds documents (design notes, release notes, screenshots) drawn from its repo, not just diary entries.
- **Why it waits**: v1 of products from GitHub (spec → Products from GitHub) keeps the bridge to diary entries; the README's title and description become the instance's name and description, nothing more.
- **Today**: a member can file any document into a product instance by hand, like into any instance.
- **To build**: a handoff shape for `file-document` in `cruna-pm-github`'s `agora-sync`, and a rule for which repo files are worth mirroring.

## GitHub comments back to notes

- **What**: when a note or to-do filed as a GitHub issue gets comments or changes state, the founder sees that next to the note in Agora.
- **Why it waits**: v1 carries only the issue number back (`→ #8`) and closes the to-do when the issue closes; a comment flow needs a way to tie a note to its issue and a new handoff.
- **Today**: the founder follows the issue in the GitHub app from the number in the to-do or diary entry.
- **To build**: record the issue ref on the note's diary entry at filing time; let `agora-sync` log comments as entries in the same instance.

## Products in another org's Project

- **What**: linking a repo whose GitHub Project lives in a different organisation from the repo.
- **Why it waits**: `cruna-pm-github` reads its Project from `.github/pm.yml` under the repo's own org; a cross-org Project needs another token scope and config.
- **Today**: only repos whose Project is in their own org can be linked.
- **To build**: on the `cruna-pm-github` side; Agora needs no change.

## Product events outside an agent session

- **What**: merges, releases and drops done outside an agent session (in the GitHub web UI, by a person) reach the product's diary too.
- **Why it waits**: the bridge runs inside agent sessions in Claude Code, triggered by the repo's AGENTS.md; nothing runs when a person merges in the browser.
- **Today**: such events reach Agora only when a later agent session's sync finds them, or when a member logs them by hand.
- **To build**: a scheduled or webhook-triggered sync on the `cruna-pm-github` side, or GitHub connectors that can read events, which would retire the `log` handoff.
