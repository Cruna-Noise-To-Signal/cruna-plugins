---
name: help
description: Explains Agora, the shared notebook a family or small team keeps on Google Drive and Calendar through the AI, and fixes it when it misbehaves. Use when someone asks what Agora can do or how it works, asks a how-do-I question about it (where are my files, who can see what, adding a document from the phone, keeping track of frequent things or printable lists, routines, adding a person, leaving), or says Agora isn't working, can't see something, or shows an error.
---

# Help

Three branches, chosen from the member's words; one message can hit two, and then you answer both:

- **Tour**: "what can you do?", "how does this work?"
- **FAQ**: a how-do-I or why question about Agora.
- **Troubleshoot**: "it's not working", "you can't see X", an error message.

Members are not technical. Answer in short, plain sentences, in their language, with the words they see on screen.

## Steps

### 1. Load the rules and find the root

Load the `rules` skill and follow rules → Finding the root. Help works without a root: when none exists, carry on with the branch and close by offering the `setup` skill. When a root exists, read `org.md`, follow rules → Identity, and read the member's memories.

Done when: you know whether a root exists; if it does, `org.md` is read and you know who is talking.

### 2a. Tour

Say in one sentence what Agora is: everyone tells the AI what happens, the AI keeps it filed on the shared Drive folder and calendar, and sends reminders and reviews back. Then show five to seven example phrases from the table below, each with what happens, in plain words. Use the root's real interest names in the examples when a root exists.

| Example phrase | What happens | Skill |
|---|---|---|
| "Nina had her blood test today, all normal" | Saved in Nina's health notes; her status updated | `log` |
| "I put the invoice in the landing folder" | The file is moved to the right place and noted | `file-document` |
| "Dentist for Nina on 3 Nov at 10" / "remind me to call the plumber Friday" | An event on the shared calendar | `schedule` |
| "When is Nina's next vaccine?" / "what do we have this week?" | An answer with dates and where it came from | `ask` |
| "Greta just did a poo" / "make me a packing list I can print" | A row in a tracker file, or a list on the Drive, kept up to date | `log` |
| "How many feeds today?" | Counted from the tracker | `ask` |
| "Give me the week ahead" | A summary of what happened and what's coming | `review` |
| "Add a new client: Rossi" | A new section to keep things in | `add-interest` |
| "Remember I prefer short answers" | Agora remembers it next time | `remember` |
| "Set up Agora" / "add my husband to Agora" | Setup, or bringing a new person in | `setup` |

Add one line about routines: once set up, a daily, a weekly and a monthly review arrive on their own.

Done when: the member has the one-sentence summary and the examples, in under 12 lines, with no skill names.

### 2b. FAQ

Answer from [references/faq.md](references/faq.md), filling in the real names from `org.md` when a root exists (root folder, members, assistant account, calendar). When the question is not there, answer from the `rules` skill if it settles it; otherwise say you're not sure and offer to check by troubleshooting.

Done when: the question is answered in a few lines, or you said plainly that you don't know.

### 2c. Troubleshoot

The fixes live in the `rules` skill's prerequisites checklist (rules → When something fails). Match the symptom to its items, then work through them in order:

| Symptom | Prerequisites items |
|---|---|
| You can't find Agora, my files, or a folder | 3 (Drive), then 4, then 5 |
| Events don't show on my phone, or you can't add events | 3 (Calendar), then 5 |
| "Permission denied", "access", "not allowed" errors | 3, then 5 |
| My daily / weekly / monthly review never arrived | 6, then 1 |
| Agora isn't in my app | 2 |
| A new member can't use it | that member runs items 1–3, 5 and 6: offer the `setup` skill's **Join** branch |
| Setup was left halfway, or `setup.md` says `state: todo` | none: offer the `setup` skill, which resumes at the first unticked box |

1. Run each matching item's **check** with a real call, exactly as the checklist describes.
2. Each item ends **ready** or **blocked**. For a blocked item, ask which app and device the member uses if you don't know, then give that platform's fix, step by step.
3. After the member says it's fixed, run the check again.

Symptoms that are not a setup problem:

- **A file I uploaded wasn't filed**: look in `landing/` and the root `status.md` Needs attention; offer to file it now with the `file-document` skill.
- **Duplicate files, or a status that looks out of date**: harmless; the next review tidies them. Offer to run the `review` skill now.
- **Something saved is wrong**: offer to record the correction with the `log` skill.

Done when: every matching item is ready, or is blocked with its fix given and the member knows the next thing to do.

### 3. Memory step

When a root exists, follow rules → Memory step.

Done when: anything durable you learned is in `memories/`, you found nothing new, or no root exists.

## Output

A short, plain message: the tour, the FAQ answer, or the troubleshooting result (what you checked, what works, what to do next, step by step). With no root, end with: "Agora isn't set up yet. Want to set it up now?" Otherwise end with the `remember` line, if any.
