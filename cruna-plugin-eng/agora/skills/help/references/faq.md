# Agora FAQ

Answers for members, in plain words. Replace the `<…>` parts with the real names from `org.md`. Keep each answer to a few lines.

## Where are my files?

In a Google Drive folder called `<root name>`, owned by `<assistant account>` and shared with every member. Open the Google Drive app or drive.google.com with your own Google account → **Shared with me** → `<root name>` (in a solo Agora on your own account, it's in **My Drive**). Inside there is one folder per `<interest, per the layout convention>`, and in each, one folder per topic with its notes (`diary`) and files (`documents`). Each `status.md` is a one-minute summary of where things stand.

## Who can see what?

Everyone in `<root name>` can see everything in the folder and on the shared Agora calendar, including the notes Agora keeps about each person's preferences (`memories`). Your own Google calendar stays yours: Agora sees it only if you shared it, and only at the level you chose (busy times only, or full details). The folder and calendar are shared only with the members listed in `org.md`.

## How do I add a document from my phone?

1. Open the Google Drive app → **Shared with me** → `<root name>` → `landing`.
2. Tap **+** → **Upload**, and pick the file or photo.
3. Tell Agora "I put the <what it is> in landing". It files it in the right place.

If you forget step 3, the next daily review picks it up. You can also upload straight into a topic's `documents` folder.

## Why can't you take the file I attached in the chat?

The AI can read a file you attach in the chat, but it can't save that file to Drive: that's a limit of the apps today. Upload it to the `landing` folder from the Drive app (see above) and tell Agora. Meanwhile, Agora can read the attachment and note what it says.

## Can you keep track of something that happens many times a day?

Yes. Say "track Greta's nappies in a file" (or feeds, expenses, medicine). Agora agrees a name and columns with you and keeps a table file in that topic's `documents` folder, one row per time you tell it ("poo at 9:15"), instead of a note each. Each morning's review writes one summary note for the day. Ask "how many today?" or "when was the last one?" any time. Already using a Google Sheet for it? Say so, and Agora copies it in once and leaves your sheet as it is.

## Can you make a list I can print?

Yes: "make a packing list for the holiday", "add milk to the shopping list", "write up a memo of Greta's vaccines". Agora keeps it as a file in that topic's `documents` folder and updates it whenever you ask. Open it from the Drive app to read, share or print it.

## Where is the tracker or list file?

In the topic it belongs to: `<root name>` → `<interest>` → `<instance>` → `documents`, under a plain name like `greta-diapers.csv` or `packing-list.md`. That topic's `index.md` lists it under **Maintained**. Or ask Agora "where is the packing list?".

## How do I change how Agora handles something?

Tell it, starting with "from now on": "from now on, put Greta's feeds in a file", "stop tracking nappies in the file, log them as notes again", "add a column for the amount". Agora says back the change, and on your yes writes it in the **Conventions** table of `org.md`, which it checks every time. You can read that table there.

## Can Agora read and send email?

When email is turned on (say "turn on email"; it suits businesses best), Agora has its own inbox: `<assistant account>`. Forward or CC it the emails that matter, and Agora saves each in the right topic, or asks you where it goes. Ask "anything from Acme?", "draft a reply to Mario", "file this email under clients/acme". Agora writes drafts whenever you like, but sends only after you've seen the exact message and said yes in the chat; your app then asks you to approve the send too. Reviews never send anything.

## What can't Agora do with email?

It reads only its own inbox, never yours. It sees an attachment's name but can't open or save it: save the file to `landing` yourself and say "filed", and Agora links it to the email. It doesn't label, archive or delete mail. When sending fails, say "email isn't working" and Agora checks your setup.

## How does Agora decide which email is important?

By default: an email that asks you to do, answer or decide something, or carries a date, and isn't a newsletter or notification. An email from your own address always counts. Tell it what else matters, or doesn't ("anything from a client is important", "ignore GitHub notifications"), or say "no preference". Each review lists important email, drafts waiting for your yes, and what was sent and saved; Agora keeps a log of what it did with each email in `inbox.md`.

## How do the routines work?

Each member has three scheduled tasks in their AI app, created during setup:

- **Daily**, in the morning: yesterday in review and today ahead.
- **Weekly**, Sunday evening: the week in review and the week ahead.
- **Monthly**, on the 1st: the month in review and the month ahead.

Each run first tidies the folder (files what was uploaded, brings summaries up to date), then writes a shared review in the `reviews` folder and sends you a short message with your things first. To change a time, edit the task in your app's scheduled tasks page. Without a paid plan, ask "give me the week ahead" whenever you like: you get the summary for everyone, then a few lines for each person. Ask "my week" for just yours.

## I stopped setup halfway. Do I start again?

No. Agora keeps a checklist of setup in `setup.md`, in the `<root name>` folder. Say "finish setup" and it carries on from the first thing still to do. Until everything is done, the daily review reminds you what's left and who does it.

## How do I add a person?

Say "add <name> to Agora". Agora walks you through it: adding them to `org.md`, sharing the folder and calendar with them, and helping them connect their own AI app.

## Can I edit the files myself?

You can add files and read everything. Notes in `diary` are never changed: to correct one, tell Agora what's wrong and it saves a correction. `status.md` and `index.md` are rewritten by Agora, so any change you make there disappears; tell Agora instead. `org.md` and the `memories` files are yours to edit if you like. Tracker and list files are too: Agora reads them before every change, so your edits stay.

## What if two of us talk to Agora at the same time?

That's fine. Each note is its own file, so nothing gets overwritten. If a summary briefly looks out of date or a file shows twice, the next review fixes it.

## How do I fix something Agora got wrong?

Tell it: "that's wrong, the appointment is on Friday". It saves the correction and updates the summaries and the calendar.

## How do I stop or leave?

- **Pause the reviews**: pause or delete your three scheduled tasks in your AI app.
- **Leave**: ask the owner, or say "remove me from Agora". Your access to the folder and calendar is removed and your app is disconnected. What you wrote stays, as part of the shared history.
- **Stop Agora entirely**: delete everyone's scheduled tasks and disconnect the Google Drive and Calendar connectors (and Gmail). The folder and calendar stay in Google, readable as plain files, until you delete them.

## Does it cost anything?

The scheduled reviews need a paid plan on Claude or ChatGPT. Google Drive and Calendar are free; the free Google account holds 15 GB, plenty for documents.
