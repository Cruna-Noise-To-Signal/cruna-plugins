---
name: add-interest
description: Adds a new interest or instance to Agora, the shared family or business notebook on Google Drive. Use when the user wants Agora to start following something new, a new person, client, project or topic ("start tracking the house renovation", "new client Acme"), or a new area inside one already followed ("add Greta's swimming", "Acme gave us a new job").
---

# Add an interest or instance

Creates a new **interest** (top-level folder) or **instance** (folder inside an interest) from its templates (rules → File kinds → templates), and keeps the indexes, statuses and `org.md` in step. `setup` reuses step 4 and the interest level of step 6 to build the first interests; it writes the `org.md` Interests table and the root `index.md` and `status.md` once itself, so skip everything else when called from setup.

## 1. Load the rules and the root

Load the `rules` skill. Follow rules → Finding the root, read `org.md` (note the `layout` convention and the Interests table), follow rules → Identity, then read `memories/<slug>.md` and `memories/shared.md` (rules → Memory step).

Done when you know the root, its layout convention and who is talking, and their memories are read.

## 2. Place it

Read the root `index.md`, and the interest's `index.md` when the request names an existing interest. Then decide, using the layout convention:

- **Interest**: a new top-level thing of the kind the convention names (a new person, a new client).
- **Instance**: an area inside an interest (Greta's swimming → interest `greta`, instance `swimming`). If the interest doesn't exist yet, create both.
- **Smaller than an instance** (one visit, one invoice, one match): that's a diary entry, not a folder. Offer to record it with the `log` skill instead, inside the instance it belongs to.

Depth is exactly two (rules → Vocabulary), so something that seems to need a third level becomes either its own instance or diary entries.

When the kind or the parent is unclear, ask once, giving your recommended answer and the likely parents from the index ("Swimming under Greta, or under Family?"). For each new folder settle the name (rules → Layout) and a one-line description.

Done when the user has confirmed, for each new folder: interest or instance, its parent, its name and its description.

## 3. Check for duplicates

Compare each new name with the existing children of its parent, listing the folder itself as well as its `index.md` (an index can lag behind). Look for exact matches and **near duplicates**: singular/plural, abbreviations, typos, the same thing in another language (`swim`/`swimming`, `acme`/`acme-srl`, `casa`/`house`). Also check whether the same thing already sits under another interest.

On a match, ask: use the existing one, or create a new one with a distinct name.

Done when no new name matches or nearly matches an existing folder, or the user chose to go ahead with a distinct name.

## 4. Create the folders and files

Follow rules → Working with Drive, and write content in the root's language.

- **New interest**: folder `<interest>/` with `index.md` and `status.md`. An interest needs at least one instance, since only instances hold diary entries and documents: if the user named none, propose one (for a person, the area they mentioned; otherwise `general`) and create it too.
- **New instance**: folder `<interest>/<instance>/` with `diary/`, `documents/`, `index.md` (empty Documents and Diary tables) and `status.md` (Summary: what it is and that tracking started today; "—" under the other sections).

Done when every new folder and file exists exactly once.

## 5. Record a new interest in org.md

Only for a new interest. Add a row to the Interests table in `org.md`: name, description, and a color if the user wants one. Calendar colors: Tomato, Flamingo, Tangerine, Banana, Sage, Basil, Peacock, Blueberry, Lavender, Grape, Graphite. Recommend one no other interest uses; when all eleven are taken, leave it empty. Replace `org.md` per rules → Replace.

Done when the downloaded `org.md` lists the new interest.

## 6. Update indexes and statuses

Two levels, following rules → Status update rule:

- **Interest level**: rebuild the `index.md` and `status.md` of each interest that gained an instance (a new interest's, rolled up from its new instances).
- **Root level**: rebuild the root `status.md`, and the root `index.md` when an interest is new.

Done when every parent `index.md` lists its new child and the interest and root `status.md` mention it.

## 7. Memory step

Follow rules → Memory step (a new interest often reveals something durable: a new client relationship, a child's new activity).

Done when: anything durable you learned is in `memories/`, or you found nothing new.

## Output

One or two short lines in the root's language, no technical terms:

- What was added, as the user would say it: "Greta's swimming is set up."
- How to use it now, with one example from their words: "Tell me news like 'Greta moved up to the blue group', or put files in Greta → swimming → documents and tell me."

If a duplicate was found and the user chose the existing one, say where things will go instead. End with the `remember` line, if any.
