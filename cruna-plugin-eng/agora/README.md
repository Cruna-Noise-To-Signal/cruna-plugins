# Agora

A shared, AI-kept knowledge base for families and small teams. People tell the AI what happened; the AI files it on Google Drive, keeps a current status for everything, puts dates on Google Calendar, and sends daily, weekly and monthly reviews. Members keep a shared to-do list, theirs and everyone's: `todo.md` on the Drive, with the reviews flagging what is overdue and suggesting what looks done. Optionally, the assistant gets its own Gmail inbox: members forward mail to it to be filed, ask it to draft and send email (only after they confirm), and see the mail needing attention in every review. A team building software can link its GitHub repos with the `cruna-pm-github` plugin: each repo becomes a product in Agora's `products` interest, with what was merged, released or dropped, and notes and to-dos for `pm` become GitHub requests.

Works on Claude and ChatGPT. Design: [agora-spec.md](https://github.com/Cruna-Noise-To-Signal/cruna-plugins/blob/main/docs/agora/agora-spec.md).

## What you need

- Claude or ChatGPT on a paid plan (for scheduled reviews).
- The Google Drive and Google Calendar connectors, plus Gmail if you turn email on.
- One of two modes:
  - **Basic shared account mode** (families, free Gmail): a Google account for the assistant when more than one person uses it (solo users can use their own); everyone's AI signs in to it.
  - **Shared services mode** (teams on Google Workspace): each member signs in as their own Workspace account, and the admin creates a shared drive, a shared calendar and a Google Group by hand. The admin's and each joining member's steps: [Manual setup](https://github.com/Cruna-Noise-To-Signal/cruna-plugins/blob/main/docs/agora/agora-spec.md#manual-setup-shared-services-mode). A basic Agora can move to it later ("move Agora to a shared drive").

The `setup` skill checks all of this and walks you through what's missing, with the same manual setup steps.

## Skills

| Skill | Works on | What it does |
|---|---|---|
| `rules` | Claude, ChatGPT | Data model and file rules every other skill follows |
| `setup` | Claude, ChatGPT | Checks prerequisites, interviews you, builds the folders and calendar, sets up routines; adds and removes members; moves a basic Agora to a shared drive |
| `add-interest` | Claude, ChatGPT | Starts tracking a new interest or instance |
| `log` | Claude, ChatGPT | Records an update in the diary and refreshes the status |
| `file-document` | Claude, ChatGPT | Files a document from the landing folder or indexes one already in place |
| `schedule` | Claude, ChatGPT | Creates, moves or removes appointments and reminders |
| `email` | Claude, ChatGPT | Optional: reads, drafts and sends email from the assistant inbox, files emails into the diary |
| `todo` | Claude, ChatGPT | Keeps the shared to-do list: adds, finishes, hands over, lists and removes to-dos |
| `ask` | Claude, ChatGPT | Answers questions from the indexes, statuses, diary, documents and calendar |
| `review` | Claude, ChatGPT | Daily, weekly and monthly review with tidy-up; the body of every routine |
| `remember` | Claude, ChatGPT | Keeps what the assistant knows about each person and the group |
| `help` | Claude, ChatGPT | Explains what Agora can do, answers FAQs, troubleshoots |

## Install

- **Claude**: `/plugin marketplace add Cruna-Noise-To-Signal/cruna-plugins`, then `/plugin install agora@cruna-plugins`. On claude.ai, add the marketplace from the plugins settings.
- **ChatGPT**: install `agora` from the Plugins page (a workspace admin may need to publish it first).

Then say: "Set up Agora".

## License

Apache License 2.0: see [LICENSE](LICENSE).
