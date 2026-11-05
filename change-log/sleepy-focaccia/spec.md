Request: #3 (sleepy-focaccia)

## Problem Statement

The founder runs Cruna from Agora, the place they check to see what is going on in the company and to tell it things. A growing share of that work is software built by AI agents in GitHub repos, and none of it reaches Agora.

The obvious fix doesn't work. The GitHub connectors in the Claude and ChatGPT apps only attach repo files to a chat; they can't read issues, PRs or Projects on their own, and they can't write. So from the chat app the founder can't:
- see what the agents merged, released or dropped;
- hand them a note ("onboarding takes too long") or a to-do ("add Outlook support") that ends up as a GitHub request.

## Solution

A bridge stands in for the missing connectors until they mature. The bridge has two halves:
- the **`cruna-pm-github` half** (Cruna-Noise-To-Signal/cruna-development-plugins#27, `bold-baguette`): the `agora-sync` skill, triggered from a linked repo's AGENTS.md;
- the **Agora half** (this request): what lets that skill write into Agora, and what makes Agora treat product work properly.

From the founder's side:

- An interest `products/` appears in Agora, with one instance per linked repo. Its diary gets one entry each time an agent merges a PR into dev, releases or drops a request, authored by the member **`pm`**. Statuses, indexes and reviews pick those entries up like any other entry.
- `pm` appears in Agora as an address, not a person: "this goes to GitHub". A to-do given to `pm` and attached to a product shows up in the to-do list under `pm`. Once it's filed it reads "… → #8", and it's ticked off when the GitHub issue closes.
- A note logged on a product instance gets the reply "noted; it becomes a GitHub request at the next sync".
- Nothing about Agora changes for roots without linked repos. Setup never offers GitHub.

Agora's side of the contract: three one-line **handoffs**, in which `agora-sync` asks Agora's own skills to write, without any question. Agora stays the only writer of its derived files.

## User Stories

1. As the founder, I want a `products/` interest with one instance per linked repo, so that every software project has its place in Agora.
2. As the founder, I want each merge into dev to appear as a diary entry on its product, carrying the PR's history entry, so that I can follow what the agents built.
3. As the founder, I want each release to appear as a diary entry listing what it shipped, so that I know what reached users.
4. As the founder, I want each dropped request to appear as a diary entry with its reason, so that I know which directions were abandoned and why.
5. As the founder, I want those entries authored by `pm`, so that I never confuse the agents' news with my own notes.
6. As the founder, I want product statuses rebuilt right after each bridge entry, so that asking "how's cruna-plugins?" straight after a release gives the current answer.
7. As the founder, I want the `products/` interest status to roll up every repo, so that I see the state of all software in one place.
8. As the founder, I want bridge entries to carry the GitHub issue, PR and tag numbers, so that `ask` can cite them and I can look them up in the GitHub app.
9. As the founder, I want bridge entries' Summary in my root's language and their Entry verbatim from GitHub, so that I read them naturally and can still search GitHub with the exact titles.
10. As the founder, I want to log a note on a product instance like on any other instance, so that I can tell the company something about a product from my phone.
11. As the founder, I want Agora to tell me that a note on a product becomes a GitHub request at the next sync, so that I know where it goes and that it won't be instant.
12. As the founder, I want to add a to-do for `pm`, attached to a product, so that I can assign product work and follow it.
13. As the founder, I want a filed to-do for `pm` to show its GitHub issue number in its text, so that I see that it arrived and where.
14. As the founder, I want a to-do for `pm` ticked off when its issue closes, so that my to-do list shows finished product work without me doing anything.
15. As the founder, I want reviews to show `pm`'s open to-dos in their own section, so that I see what's waiting on GitHub next to each person's items.
16. As the founder, I want `pm` left out when Agora asks who I am, so that I'm never offered a bot as my identity.
17. As the founder, I want `pm` to have no calendar, routines, memories file, Access row or setup checklist, so that Agora never asks me to set it up or flags it as unfinished.
18. As the founder, I want the bridge's last-check file left alone by tidy-up, so that reviews don't flag it or file it as a document.
19. As the founder, I want roots without linked repos to work exactly as before, so that families and other businesses never see any of this.
20. As the founder, I want `setup` never to mention GitHub, so that the product side appears only once a developer links a repo.
21. As the `agora-sync` skill, I want a `log` handoff that writes one diary entry as `pm`, without any question, and propagates status and index, so that I can report an event in one call.
22. As the `agora-sync` skill, I want a `todo` handoff that edits a to-do's text or marks it done by its id, without any question, so that I can link a to-do to its issue and close it.
23. As the `agora-sync` skill, I want an `add-interest` handoff that creates `products/` and its instance, the `pm` member and the managed Conventions row, so that linking a repo is one call.
24. As the `agora-sync` skill, I want the `add-interest` handoff to update the description when the instance already exists, so that linking again refreshes it and never creates duplicates.
25. As the `agora-sync` skill, I want every handoff to end with one short machine-readable result line (what was written, or what failed), so that I can report it in one line and carry on with the developer's work.
26. As the `agora-sync` skill, I want a handoff in a malformed shape, or naming a root, instance or to-do that doesn't exist, to fail without writing anything, so that a bad call never corrupts the vault.
27. As a maintainer of `cruna-pm-github`, I want the handoff shapes stated once, under a version note, so that I know a change to them is breaking and must ship with an update on my side.
28. As a member of a root with linked repos, I want `ask` to answer about products from their status and diary like any other interest, so that nothing new needs learning.
29. As the founder, I want the `help` skill and its FAQ to explain products, `pm` and the bridge in two lines (what it is, where to read more), so that I can recall how it works.
30. As the founder using Italian Agora, I want the same behaviour with the same structure names and Italian content, so that the Italian plugin is a true translation.
31. As a maintainer, I want the Agora spec to record the GitHub connector finding as a verified platform fact, with the date and the platforms checked, so that nobody tries the connector route again without re-checking.
32. As a maintainer, I want the "Git-backed interests" future extension replaced by this design in the spec, and the items left out of v1 recorded in `future-extensions.md`, so that the roadmap is current.

## Implementation Decisions

### Model

- **A member of kind github.** A row in `org.md` Members whose Role is `github`. Slug `pm` (the slug comes from the linked repo's `.agora/pm.yml`, default `pm`; Agora accepts whatever slug the handoff names). Name "PM (GitHub)". Email, Mail from, Birthday and Personal calendar are empty. Notes: "address for GitHub through cruna-pm-github".
  - No new column: Role already exists and is free text, and `github` becomes a reserved value.
  - A github member:
    - is left out of rules → Identity's member list;
    - gets no `setup.md` Member section (like a member who is only tracked), no memories file and no Access row;
    - is never offered as a to-do owner unless named.
  - It may author diary entries and to-do events, and own to-dos.
  - Reviews give it a section like any member, holding only its open to-dos; it gets no personal message.
- **Managed instances.** A Conventions row in `org.md`: Where `products/<instance>`, Convention "managed by cruna-pm-github: notes and to-dos for pm become GitHub requests", Since `<date> by pm`. The `add-interest` handoff writes it. Every skill already reads Conventions, so no new lookup is needed.
- **`products/` interest.** An ordinary interest named `products`, described as "Software products built in GitHub". It has no color unless a member sets one. Instances are named after the repo (lowercase, hyphenated) and described from the README's purpose line.
- **Diary front-matter.** A new optional key `github:`, holding a list of refs (`"#8"`, `"PR #9"`, `"v2026.11.12"`), placed after `events`. Only handoff entries carry it. Bridge entries are recognised by `author` being a github member; there is no `source:` key.
- **Bridge file.** `<interest>/<instance>/YYYY-MM-DD_hh_mm_last-check.md` belongs to `cruna-pm-github`. Agora never reads, indexes, files, flags or trashes it, and a stale-index check ignores it.

### Handoffs (new section in `rules`)

A handoff is a one-line prompt in a fixed shape, sent by another plugin's skill, that runs an Agora skill **unattended**. Everything the skill would ask is in the line. The section states the three shapes once, under a version note ("handoff shapes v1: a change is breaking; announce it in the release notes"):

```
Agora log as <slug> in <root>: <interest>/<instance> — github: <ref>[, <ref>…] — <entry text>
Agora todo as <slug> in <root>: edit <to-do id> text "<new text>"
Agora todo as <slug> in <root>: done <to-do id>
Agora add-interest as <slug> in <root>: products/<instance> — <name>: <description>
```

Rules every handoff follows:

- **Who and where.** `<slug>` must be a github member of `<root>`. The exception is `add-interest`, which creates the member when it's missing. The root is found by name (rules → Finding the root). The interest, instance and to-do must already exist, except where `add-interest` creates them.
- **Skipped steps.** No identity question, memories read, calendar step, to-do offer, pattern suggestion or memory step. Everything else (placement checks, templates, Replace, propagation) runs as usual.
- **Failure.** If the shape doesn't parse, or the slug, root, instance or to-do is missing, nothing is written, and the result says what was missing.
- **Result.** One line: `agora: ok — <what was written>` or `agora: failed — <reason>`. It's in English (the calling skill reads it), with no greeting.
- **Language.** Summary lines and any text Agora writes are in the root's language. The text the handoff passes is kept verbatim in the Entry section or in the to-do's text.

### Per skill

- **`rules`**:
  - the Handoffs section;
  - the github member in Vocabulary;
  - Identity leaves out github members;
  - Layout mentions the bridge file;
  - the diary template gains the optional `github:` key;
  - `references/todos.md` says a github member may own to-dos and write `edit` and `done` events through the handoff. This is a second exception next to member mail, because the github member acts for GitHub.
- **`log`**:
  - recognises the `log` handoff shape and runs steps 1–7 unattended: author is the slug, `events: []`, `github:` from the line, Summary written by Agora, Entry verbatim;
  - on a member's ordinary log into a managed instance, the Output adds "noted; it becomes a GitHub request at the next sync" (in the root's language).
- **`todo`**: recognises the two `todo` handoff shapes. It matches by id only, writes one `edit` or `done` event authored by the slug, rebuilds `todo.md` and the attached statuses, and returns the result line.
- **`add-interest`**: recognises the `add-interest` handoff. In order, each step idempotent:
  1. add the github member row when missing;
  2. create `products/` when missing (Interests row, index, status);
  3. create the instance, or update its description in the interest `index.md` when it exists;
  4. write the Conventions row when missing;
  5. propagate.

  The duplicate check (step 3) treats an exact name match as "update", never as a question.
- **`review`**:
  - Tidy-up leaves the bridge file alone and doesn't count it when checking whether an index is stale.
  - The general message has a section for each github member with open to-dos.
  - Personal messages are never built for a github member.
- **`setup`**: Join and the member checklist skip github members. The interview never mentions GitHub. Offboard can remove a github member by removing its Members and Conventions rows (the diary keeps its entries).
- **`ask`**: no behaviour change. It may cite `github:` refs when it answers from a bridge entry.
- **`help`**: a short "Products from GitHub" answer in SKILL.md and an FAQ entry: what `products/` and `pm` are, that a developer links repos from Claude Code with `cruna-pm-github`, and that notes and to-dos for `pm` become GitHub requests at the next sync.

### Docs and release

- **Spec, `docs/agora/agora-spec.md`:**
  - a "Products from GitHub" section summarising the model, the handoffs and the bridge file;
  - the GitHub connector finding under Verified platform facts, dated, with the platforms the founder checked. ChatGPT gets a Release check if it wasn't checked.
- **`future-extensions.md`:**
  - drop "Git-backed interests" (now in the spec);
  - add: documents for products; GitHub comments flowing back to a note; repos whose Project lives in another org; logging events done outside an agent session.
- **Glossary** `docs/agora/glossary-it.md`: new terms (handoff, github member, products, bridge, last-check).
- **Version 0.6.0** in both manifests of both languages and in both catalogs.

## Testing Decisions

- **What a good test is.** A test drives the plugin from outside, the way its callers do: a handoff line typed in Claude Code with the agora plugin loaded, or a member's message in the chat app. It checks what ends up on Drive and what the chat says. It never checks how a skill reached that result.
- **The seam.** One seam: the **handoff line**. It is the whole contract with `cruna-pm-github`, so every handoff behaviour (write, skip questions, fail cleanly, idempotency, result line) is tested by typing the line in a Claude Code session against a test root. The chat-side behaviour (the managed-instance reply, `pm` in to-dos and reviews, Identity without `pm`) is tested through the existing conversational seam.
- **Modules covered.** `log`, `todo` and `add-interest` through their handoffs; `log`, `todo`, `review`, `setup` and `ask` through conversation.
- **Prior art.** The manual tests in `docs/agora/manual-tests/` (`manual-test-agora-v050-todos.md` is the closest: numbered scenarios, Drive checks after each) and the change-log `manual-test.md` written by implement-loop. A new `manual-test-agora-v060-products.md` follows the same shape. It runs on a test root with an Italian variant for the language rule, and includes:
  - the failure cases: unknown slug, unknown instance, malformed line;
  - a second `add-interest` run proving idempotency.
- **Checker.** The cruna-core `portable-plugins` checker must pass, covering both manifests, both catalogs and matching versions.

## Out of Scope

- Everything on the `cruna-pm-github` side, which is #27:
  - the `agora-sync` skill;
  - reading notes and to-dos from Drive and filing issues;
  - the last-check file's contents and lifecycle;
  - `.agora/pm.yml`;
  - `pm-setup` step 10 and `start-repo`;
  - the template's AGENTS.md and README lines.
- Documents for products. The README's title and description become the instance name and description, nothing more.
- GitHub comments or issue state flowing back to a note.
- Repos whose GitHub Project lives in another org.
- Events done outside an agent session (a merge in the GitHub web UI).
- Any GitHub connector use from the chat apps.
- A "business" flag in `org.md`, or any GitHub question in `setup`.
- Handoff shapes for other skills (`schedule`, `file-document`, `email`).

## Further Notes

- **Order of the two requests.** #27 is blocked by this request: its skill calls the handoff shapes defined here. Release this first.
- **Agora alone.** Without #27, the handoffs can be exercised by typing them in Claude Code, which is how the manual test runs them.
- **Removal path.** Once GitHub connectors can read issues and PRs, Agora reads them directly and the `log` handoff loses its caller. Once they can write, the `todo` handoff does too. The github member, `products/` and the Conventions row stay as they are, so the founder's routine doesn't change.
- **Design trail.** [grilled-idea.md](grilled-idea.md). Proposal page: https://claude.ai/artifact/Re8XfnGMBegn286M2ZBZxh
