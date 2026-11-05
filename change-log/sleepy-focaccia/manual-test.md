# sleepy-focaccia — hand-test manual

## M1

Handoffs, English plugin only, from this branch. Agora's version is still 0.5.0 in the manifests (the bump is M3).

### Before you start

- Claude Code under a claude.ai login (needed for the Google Drive connector), with the `agora` plugin installed from this branch.
- A test root on Drive set up with Agora (English, one human member, e.g. `simone`), with **no** `products/` folder and no `pm` row in `org.md`.
- After every line, check Drive yourself. Each handoff reply must be exactly one line starting `agora: ok —` or `agora: failed —`, with no question and no greeting. Below, `Test` is the root's name: use yours.

### 1. Link a product (add-interest)

Type: `Agora add-interest as pm in Test: products/cruna-plugins — Cruna Plugins: Public plugins for Claude and ChatGPT`

- [ ] Reply `agora: ok — member pm, interest products, instance products/cruna-plugins, convention` (wording may vary, the parts must be there).
- [ ] `org.md`: a Members row `pm | PM (GitHub) | | | github | | | address for GitHub through cruna-pm-github`; no Access row for `pm`; an Interests row `products` with no color; a Conventions row `products/cruna-plugins | managed by cruna-pm-github: … | <today>, pm`. Only one `org.md` in the folder.
- [ ] `products/index.md`, `products/status.md`, `products/cruna-plugins/` with `diary/`, `documents/`, `index.md`, `status.md`; root `index.md` lists `products`; root `status.md` mentions it.

### 2. Link it again (idempotency)

Type the same line with a new description: `… — Cruna Plugins: Cruna's public plugins`

- [ ] Reply names the description only. `products/index.md` shows the new text, and `products/status.md` and the root `status.md` were rebuilt; no second member row, Interests row, Conventions row or folder.
- [ ] The same line once more → `agora: ok — nothing to change`, nothing written.

### 3. A release entry (log)

Type: `Agora log as pm in Test: products/cruna-plugins — github: #8, PR #9, v2026.11.12 — Release v2026.11.12: Agora reads, drafts and sends email (#8)`

- [ ] Reply `agora: ok — products/cruna-plugins/diary/pm-YYYYMMDD_HHMM.md`, no calendar, to-do or memory question.
- [ ] The entry: `author: pm`, `events: []`, `github: ["#8", "PR #9", "v2026.11.12"]` after `events`; Summary in English written by Agora; Entry is the text verbatim.
- [ ] The instance `index.md` lists it on top; instance, `products/` and root `status.md` mention the release, Last updated now.

### 4. A to-do for pm (conversation, then todo handoff)

- [ ] In conversation as yourself: "add a to-do for pm on cruna-plugins: add Outlook support" → owner `pm`, attached to `products/cruna-plugins`; `todo.md` has a `## PM (GitHub)` section. Note the id (e.g. `simone-20261105_1012`).
- [ ] "Add a to-do: check the invoices" → owner is you, never `pm`.
- [ ] Identity: in a new conversation, the "who are you?" question does not list `pm`.
- [ ] Type: `Agora todo as pm in Test: edit <id> text "Add Outlook support → #8"` → `agora: ok — todos/pm-….md (edit <id>)`; one `edit` event authored by `pm`; `todo.md` and the instance status show "… → #8".
- [ ] Type: `Agora todo as pm in Test: done <id>` → one `done` event; `todo.md` shows it under Recently done; the instance status drops it from Pending.
- [ ] The `done` line again → `agora: failed — … already closed`, nothing written.

### 5. Failures write nothing

Note the file count in `products/cruna-plugins/diary/`, `todos/` and `org.md`'s modified time before each, and check none changed after.

- [ ] Unknown slug: `Agora log as bot in Test: products/cruna-plugins — github: #1 — hello` → `agora: failed — …` naming the slug.
- [ ] Unknown instance: `Agora log as pm in Test: products/nope — github: #1 — hello` → failed, names the instance; no `add-interest` offer.
- [ ] Malformed: `Agora log as pm in Test: products/cruna-plugins hello` → failed, malformed.
- [ ] Unknown to-do: `Agora todo as pm in Test: done pm-20000101_0000` → failed.
- [ ] Person slug: `Agora add-interest as simone in Test: products/x — X: y` → failed (simone is not a github member); no `products/x`.
- [ ] Unknown root: `Agora log as pm in Nowhere: products/cruna-plugins — github: #1 — hi` → failed.

### 6. A member's note on a product

- [ ] As yourself: "note on cruna-plugins: onboarding takes too long" → logged as usual (author you), and the reply ends with "noted; it becomes a GitHub request at the next sync".
- [ ] A note on any other instance has no such line.

## M2

Agora knows `pm`, English plugin only, from this branch (version still 0.5.0). Use the M1 test root after M1's scenarios, so it has `pm`, `products/cruna-plugins`, a release entry and at least one open to-do for `pm` (add one in conversation if M1 closed it: "add a to-do for pm on cruna-plugins: add Outlook support").

### 1. A note on the product

- [ ] In the chat app as yourself: "note on cruna-plugins: onboarding takes too long" → logged with you as author; the reply ends with "noted; it becomes a GitHub request at the next sync".

### 2. Identity

- [ ] In a new conversation, "what's pending?" → the "who are you?" question lists only people, never `pm`.

### 3. Tidy-up leaves the bridge file alone

- [ ] By hand in Drive, upload a file `2026-11-05_10_00_last-check.md` (any content) into `products/cruna-plugins/`. Note the instance `index.md` and `status.md` modified times.
- [ ] Ask: "daily review". The file is still there, unrenamed, not in `documents/`, not in the instance `index.md`; the review and the root `status.md` Needs attention don't mention it; the instance `index.md` and `status.md` were not rebuilt because of it.

### 4. pm in the general review

- [ ] Same review: the message has a "PM (GitHub)" section listing only pm's open to-dos (no calendar events, no Looking back items); the review file's To-dos section lists pm's to-dos with the other members'.
- [ ] Ask "my week" as yourself → personal message; pm's to-dos appear only if they are Everyone's (they aren't), not under Yours.
- [ ] Type `Run Agora review: daily for pm in Test` → the general message, not a personal one for pm.
- [ ] Close all of pm's to-dos (`Agora todo as pm in Test: done <id>`), run "daily review" again → no PM section.

### 5. Setup passes pm by

- [ ] "finish setup" (or "check my setup") → Verify passes with no complaint about pm's missing Access row, memories file, folder sharing, routines or Member section; `setup.md` has no `## Member: pm`.
- [ ] The setup reply never mentions GitHub.
- [ ] "remove pm from Agora" → Offboard asks no sharing or password steps; `org.md` loses the `pm` Members row and the `products/cruna-plugins` Conventions row (one `org.md` in the folder); `products/` and its diary entries stay; the reply says the repo stays linked until a developer unlinks it with `cruna-pm-github`. Re-run the M1 step 1 add-interest line to restore pm.

### 6. ask and help

- [ ] "What did cruna-plugins release lately?" → answers from the release entry, citing its path and the refs `#8, PR #9, v2026.11.12`.
- [ ] "What is products and who is pm?" → two or three plain lines: software built on GitHub, `pm` is the address for GitHub, a developer links repos from Claude Code with `cruna-pm-github`, notes and to-dos for pm become GitHub requests at the next sync.
- [ ] "What can you do?" → the tour has one line about products from GitHub.
- [ ] On a root without `pm` (any other test root), "what can you do?" → no GitHub line.
