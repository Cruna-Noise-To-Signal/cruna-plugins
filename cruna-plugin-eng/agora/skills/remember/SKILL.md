---
name: remember
description: Remembers durable things about Agora members and the group — preferences, habits, who does what, standing facts like allergies — and shows or forgets them on request. Use when a member says "remember that…", "note that I prefer…", "what do you remember about me?" or "forget that…", or when another Agora skill's memory step finds something worth keeping.
---

# Remember

`memories/` holds what stays true across conversations: traits, preferences, habits, relationships, standing facts. Every Agora skill reads it to set tone and defaults. It is **curated**: short, current, one fact per line, edited by replacing lines rather than piling them up (rules → File kinds).

You are reached two ways: a member asks, or another skill's memory step hands you a fact.

## Steps

### 1. Load the rules, find the root, know who is talking

Load the `rules` skill. Follow rules → Finding the root and rules → Identity. Handed a fact by another skill, the caller has done this.

Done when: `org.md` is read and you know the member's slug.

### 2. Pick the branch

- **Save**: "remember that…", "note that I prefer…", or a fact handed to you. Go to step 3.
- **Show**: "what do you remember about me?" Read `memories/<slug>.md` and the lines of `memories/shared.md` that mention them; show them as a plain list. Done.
- **Forget**: "forget that…". Find the matching line(s) in the member's file or `shared.md`; when more than one could match, ask which. Remove them and replace the file (rules → Replace). Done.

Done when: you know which branch, and Show or Forget is complete.

### 3. Sort the fact

A memory is durable: it will still be true next month and shapes how you help. Sort each fact:

- **Memory**: "Simona handles school stuff", "I prefer short answers", "Marco pays the client invoices on the 10th". Keep it.
- **Diary fact**: an event in an instance ("Nina's lesson moved to Thursday"). Hand it to the `log` skill instead.
- **Both**: a standing fact that also belongs to an instance's history ("Nina is allergic to penicillin"). Keep it here, and also log it to the instance (`nina/health`) with the `log` skill.
- **Secret**: passwords, PINs, card numbers, security codes, recovery codes. Keep these out of Agora entirely: Drive files are readable by every member. Tell the member to keep it in a password manager.

Done when: every fact is a memory, handed to `log`, or turned away as a secret.

### 4. Pick the file

- About one member (their preferences, habits, how they like to be helped): `memories/<slug>.md` of **that** member, whoever is talking.
- About the group (who handles what, shared routines), or about someone who isn't a member (a child, a client, a supplier): `memories/shared.md`.

Done when: each memory has one target file.

### 5. Curate and write

Read the target file (rules → Working with Drive); when it doesn't exist yet, start from the Memory file template (rules → File kinds → templates).

Write the fact as one dated line: `- YYYY-MM-DD <fact>`, in the root's language, plain and specific. Then **supersede**: when a line already covers the same subject ("prefers calls in the morning" vs a new "prefers calls after 18:00"), replace that line instead of adding a second one; when the new line repeats an existing one, only refresh its date. Merge near-duplicates you notice while there. Keep the file short enough to read in a minute.

Replace the file (rules → Replace).

Done when: the file holds the new fact exactly once, no line contradicts it, and only one copy of the file exists.

## Output

One short line in the root's language saying what you'll remember ("Got it: Simona handles school matters."), or what you forgot. For Show, the list. For a secret, the one-sentence pointer to a password manager. When called from another skill's memory step, that skill adds the same one line at the end of its own reply.
