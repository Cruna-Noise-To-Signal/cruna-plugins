# Agora

A shared, AI-kept knowledge base for families and small teams. People tell the AI what happened; the AI files it on Google Drive, keeps a current status for everything, puts dates on Google Calendar, and sends daily, weekly and monthly reviews.

Works on Claude and ChatGPT. Design: [agora-spec.md](https://github.com/Cruna-Noise-To-Signal/cruna-plugins/blob/main/docs/agora/agora-spec.md).

## What you need

- Claude or ChatGPT on a paid plan (for scheduled reviews).
- The Google Drive and Google Calendar connectors.
- A Google account for the assistant when more than one person uses it (solo users can use their own).

The `setup` skill checks all of this and walks you through what's missing.

## Skills

| Skill | Works on | What it does |
|---|---|---|
| `rules` | Claude, ChatGPT | Data model and file rules every other skill follows |
| `setup` | Claude, ChatGPT | Checks prerequisites, interviews you, builds the folders and calendar, sets up routines; adds members |
| `add-interest` | Claude, ChatGPT | Starts tracking a new interest or instance |
| `log` | Claude, ChatGPT | Records an update in the diary and refreshes the status |
| `file-document` | Claude, ChatGPT | Files a document from the landing folder or indexes one already in place |
| `schedule` | Claude, ChatGPT | Creates, moves or removes appointments and reminders |
| `ask` | Claude, ChatGPT | Answers questions from the indexes, statuses, diary, documents and calendar |
| `review` | Claude, ChatGPT | Daily, weekly and monthly review with tidy-up; the body of every routine |
| `remember` | Claude, ChatGPT | Keeps what the assistant knows about each person and the group |
| `help` | Claude, ChatGPT | Explains what Agora can do, answers FAQs, troubleshoots |

## Install

- **Claude**: `/plugin marketplace add Cruna-Noise-To-Signal/cruna-plugins`, then `/plugin install agora@cruna-plugins`. On claude.ai, add the marketplace from the plugins settings.
- **ChatGPT**: install `agora` from the Plugins page (a workspace admin may need to publish it first).

Then say: "Set up Agora".
