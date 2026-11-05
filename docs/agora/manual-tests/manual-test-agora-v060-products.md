Manual test for Agora 0.6.0, products from GitHub (request #3, sleepy-focaccia). Release check from agora-spec → Release checks.

Status: ready to run, not yet run. Record results in the boxes and notes below, then fill TO IMPROVE and FACT at the end, as in the earlier manual tests.

## Setup for the test

- A test root (English, here named `Test`) with at least one human member (you) and a dedicated **assistant account**, with `todos/`, **no** `products/` folder and no `pm` row in `org.md`.
- A second test root in Italian (`language: it`, here `Prova`), set up with `agora-ita`, for scenario 9.
- Install `agora` 0.6.0 and `agora-ita` 0.6.0 from this repo.
- **Handoffs** (scenarios 1–5): type each line in **Claude Code under a claude.ai login** (the Google Drive connector needs it), with the plugin installed. Without `cruna-pm-github`'s `agora-sync`, typing the line is how the handoff is called. Each reply must be exactly one line starting `agora: ok —` or `agora: failed —`, in English, with no question and no greeting.
- **Chat scenarios** (6–8): run on **Claude** and on **ChatGPT web on a paid plan**.
- Check every result in Drive yourself, never the AI's word. Mark each check `[x]` when it holds, and write what you saw under Notes.

| Platform | Plan | Tester | Date | Plugin version |
|---|---|---|---|---|
| Claude Code | | | | |
| Claude | | | | |
| ChatGPT | | | | |

## 1. Link a product (`add-interest` handoff)

Type: `Agora add-interest as pm in Test: products/cruna-plugins — Cruna Plugins: Public plugins for Claude and ChatGPT`

| Check | Claude Code |
|---|---|
| Reply `agora: ok — member pm, interest products, instance products/cruna-plugins, convention` (wording may vary; every part is named) | [ ] |
| `org.md`: Members row `pm \| PM (GitHub) \| \| \| github \| \| \| address for GitHub through cruna-pm-github`; no Access row for `pm`; Interests row `products`, no color; Conventions row `products/cruna-plugins \| managed by cruna-pm-github: … \| <today>, pm`; one `org.md` in the folder | [ ] |
| `products/index.md` and `products/status.md`; `products/cruna-plugins/` with `diary/`, `documents/`, `index.md`, `status.md`; the root `index.md` lists `products` | [ ] |

Notes:

## 2. Link it again (idempotency)

1. Type the same line with a new description: `Agora add-interest as pm in Test: products/cruna-plugins — Cruna Plugins: Cruna's public plugins`.
2. Type that line once more.

| Check | Claude Code |
|---|---|
| First reply names the description only; `products/index.md` shows the new text; statuses rebuilt | [ ] |
| No second Members, Interests or Conventions row, no second folder | [ ] |
| Second reply `agora: ok — nothing to change`; nothing written (modified times unchanged) | [ ] |

Notes:

## 3. A release entry (`log` handoff)

Type: `Agora log as pm in Test: products/cruna-plugins — github: #8, PR #9, v2026.11.12 — Release v2026.11.12: Agora reads, drafts and sends email (#8)`

| Check | Claude Code |
|---|---|
| Reply `agora: ok — products/cruna-plugins/diary/pm-YYYYMMDD_HHMM.md`; no calendar, to-do, pattern or memory question | [ ] |
| The entry: `author: pm`, `events: []`, then `github: ["#8", "PR #9", "v2026.11.12"]`; Summary in English written by Agora; Entry is the text verbatim | [ ] |
| The instance `index.md` lists the entry; instance, `products/` and root `status.md` mention the release, Last updated now | [ ] |

Notes:

## 4. A to-do for pm (`todo` handoff)

1. In conversation as yourself: "add a to-do for pm on cruna-plugins: add Outlook support". Note the id.
2. Type: `Agora todo as pm in Test: edit <id> text "Add Outlook support → #8"`.
3. Type: `Agora todo as pm in Test: done <id>`.

| Check | Claude Code |
|---|---|
| Step 1: owner `pm`, attached to `products/cruna-plugins`; `todo.md` has a `## PM (GitHub)` section | [ ] |
| Step 2: reply `agora: ok — todos/pm-….md (edit <id>)`; one `edit` event authored by `pm`; `todo.md` and the instance status read "… → #8" | [ ] |
| Step 3: one `done` event authored by `pm`; `todo.md` shows it under Recently done; the instance Pending drops it | [ ] |

Notes:

## 5. Failures write nothing

Before each line, note the file counts in `products/cruna-plugins/diary/` and `todos/` and `org.md`'s modified time; after it, check none changed.

| Line | Expected | Claude Code |
|---|---|---|
| `Agora log as bot in Test: products/cruna-plugins — github: #1 — hello` | `agora: failed — …` naming the unknown slug | [ ] |
| `Agora log as pm in Test: products/nope — github: #1 — hello` | failed, naming the unknown instance; no `add-interest` offer | [ ] |
| `Agora log as pm in Test: products/cruna-plugins hello` | failed, malformed line | [ ] |
| The step 3 `done` line of scenario 4 again | failed, already closed | [ ] |

Notes:

## 6. A member's note on a product

1. As yourself: "note on cruna-plugins: onboarding takes too long".
2. A note on any other instance.

| Check | Claude | ChatGPT |
|---|---|---|
| Logged as usual, author you; the reply ends "noted; it becomes a GitHub request at the next sync" | [ ] | [ ] |
| The other note has no such line | [ ] | [ ] |

Notes:

## 7. Identity without pm

1. In a new conversation: "what's pending?".

| Check | Claude | ChatGPT |
|---|---|---|
| The "who are you?" question lists only people, never `pm` | [ ] | [ ] |
| "Add a to-do: check the invoices" → owner is you; `pm` is never offered as owner | [ ] | [ ] |

Notes:

## 8. pm in to-dos and the general review

1. Add an open to-do for pm: "add a to-do for pm on cruna-plugins: add Outlook support".
2. Upload by hand a file `2026-11-05_10_00_last-check.md` into `products/cruna-plugins/`.
3. Ask "daily review", then "my week".

| Check | Claude | ChatGPT |
|---|---|---|
| "What's on pm's list?" shows the to-do; `todo.md` has the `## PM (GitHub)` section | [ ] | [ ] |
| The general message has a "PM (GitHub)" section with its open to-dos only; the review file's To-dos section lists them | [ ] | [ ] |
| The personal message ("my week") has no pm to-dos under Yours; asking for a review "for pm" gives the general message | [ ] | [ ] |
| The last-check file is still in place, unrenamed, not filed, not in the instance `index.md`, not flagged | [ ] | [ ] |
| "What did cruna-plugins release lately?" cites the release entry and its refs `#8, PR #9, v2026.11.12` | [ ] | [ ] |
| "What is products and who is pm?" answers in two or three lines, naming `cruna-pm-github` | [ ] | [ ] |

Notes:

## 9. Italian root

In Claude Code with `agora-ita`, repeat scenarios 1, 3 and 4 against `Prova` (the handoff lines stay in English word for word); then scenario 6 in the chat app in Italian ("nota su cruna-plugins: l'onboarding è troppo lungo").

| Check | Result |
|---|---|
| Handoff replies are the same English result lines | [ ] |
| Folder `products`, Role `github`, marker `managed by cruna-pm-github`, key `github:` in English; the `products` description, the Conventions text after the marker, the Members Notes and every Summary in Italian | [ ] |
| The diary Entry keeps the handoff text verbatim (English) | [ ] |
| The managed-instance reply reads "annotato; diventerà una richiesta GitHub alla prossima sincronizzazione" | [ ] |

Notes:

## Results

TO IMPROVE:
-

FACT:
-
