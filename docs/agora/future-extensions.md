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
