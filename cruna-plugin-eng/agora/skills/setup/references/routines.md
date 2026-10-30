# Routines

Every member gets three scheduled tasks in their own app. Each one runs Agora's `review` skill for one period. Fill in the member's slug and the root's display name, suggest the times below (the user may change them, in the root's timezone), and present each prompt as a block the user can copy as is.

| Routine | What it covers | When (recommended) | Repeats |
|---|---|---|---|
| Daily | Yesterday in review, today ahead | 07:30 | Every day |
| Weekly | This week in review, next week ahead | Sunday 19:00 | Every week |
| Monthly | Last month in review, this month ahead | 08:00 on the 1st | Every month |

## Prompts

Each prompt is exactly one line, in the shape the `review` skill expects. The slug tells an unattended run whose review to personalize; a member who would rather get the general review, with every member's update, drops `for <slug>`.

Daily:

```
Run Agora review: daily for <slug> in <Root display name>
```

Weekly:

```
Run Agora review: weekly for <slug> in <Root display name>
```

Monthly:

```
Run Agora review: monthly for <slug> in <Root display name>
```

## Creating them

Walk prerequisites item 6 with the user: where the scheduled tasks page is on their app, pasting each prompt with its time, confirming the task can use the Drive and Calendar connectors and the Agora plugin, and turning on notifications. Many apps also accept "schedule this every day at 07:30" typed in a chat with the prompt; offer it as the quicker path.
