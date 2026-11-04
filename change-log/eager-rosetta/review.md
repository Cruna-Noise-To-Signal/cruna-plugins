# Review — eager-rosetta

## M1 — round 1

**Result: FAIL** (Specs FAIL, Standards FAIL)

### Specs

VERDICT: FAIL. Reviewed M1 diff (F1, F2, F3, F5) against spec.md and plan.md. Most of M1 is in place and the checker passes.

**(c) Implemented, but wrong**

1. **New root throws away email answers for members who aren't present.** Spec: "Members round: email addresses, and each member's importance question when email is on." Story 7: "I want setup to ask for every email address I write or forward from."
   - `setup/references/interview.md` Round 2 asks the email questions for every member.
   - But `setup/SKILL.md` step 2d runs the member part only "for the owner".
   - Root part step 6 then ticks the root Email box, which deletes the Interview section that still holds the other members' answers.
   - So their addresses and criteria are lost, and Join asks them again; Verify ("every member using the root has addresses in Emails") can't pass until each member joins.
   - Fix: in 2d, write every interviewed member's Emails cell and `Email importance:` line. Keep only the Gmail check, and ticking the box, for each member's own app.

**(a) Missing or partial**

2. **Ambiguous email in conversation is never flagged.** Plan F3 acceptance: "an ambiguous email is flagged". Spec: "I want an email the AI can't place to be flagged under Needs attention…". In `email/SKILL.md` step 6.3, "When no single instance fits, ask once" — nothing says what happens if it is still unplaced. Step 5.3 (sent mail) does say "still none, flag it". File should get the same fallback, with a `flagged` ledger row. Minor.

**(b) Scope creep (small, none blocking)**

3. **Third-person body by default.** `rules/references/email.md` → Sender voice says "by default, the assistant speaks for them in the third person". The spec only asks "signed as the assistant on behalf of me". A new behaviour rule the spec doesn't have.
4. **README skill-table row.** The spec puts README changes in F8. Harmless.
5. **Shared memories hold `see Conventions`.** Spec data shape: "`Email importance: not asked | invited | no preference | <criteria>` … in `memories/shared.md` for the root". Record the decision in the spec at F8.

**Checked and correct:** sending (exact-message confirmation, routines never send, emailed instruction → draft only, one Agora confirmation then platform prompt); member mail matched on address only; filing (forwarded/CC'd only, author = forwarding member, email fields and `To file:` note, deterministic entry names); `inbox.md` curated with template and spec outcome values; `org.md` `email:` field and Emails column; `setup.md` Email boxes; Review template `## Email`; prerequisites item 7 with real search call and platform limits; setup "turn on email" branch, Join, Migrate note, Verify line; solo roots file only on request; criteria asked on *not asked*/*invited*, "no preference" recorded; `email` skill frontmatter and `agents/openai.yaml`.

### Standards

VERDICT: FAIL. Checked against AGENTS.md and cruna-core `writing-for-agents` (single source of truth, co-location, completion criteria, pointer wording). Frontmatter, README row and openai.yaml are fine.

**Fix before merge**

1. **Duplicated Code / Shotgun Surgery: the criteria-recording rule.** The root-wide criterion format (a Conventions row, Where `*` or the interest, `important email: <criteria>`, Since today plus the member's slug, `shared.md` set to `see Conventions`) is written out word for word in `email/SKILL.md` step 7 and `setup/references/email.md` Root part step 3; `rules/references/email.md` → Where criteria live states it a third time and `templates.md` a fourth. Same for the `draft replies to …` opt-in. Keep the definition in rules → Email → Importance and point to it from the other sites ("record it as in rules → Email → Where criteria live").
2. **Sending policy restated.** `email/SKILL.md` step 5.1 rewrites the confirmation rule right after "Follow rules → Email → Sending". The warning about the app's own approval prompt appears three times: rules Sending, prerequisites item 7 Limits, and step 5.2. Steps should say only *when* each rule fires.
3. **Completion criteria that don't match their steps:**
   - `email/SKILL.md` step 1's Done-when requires "`inbox.md` is read", but nothing in the step tells the agent to read it.
   - `setup/references/migration.md` step 6 now makes three changes (adds Emails), but its Done-when still says "shows both changes".

**Judgement calls (not blocking)**

- **Possible Mysterious Name:** Members table has both `Email` (Google account the folder is shared with) and `Emails` (addresses counting as member mail). Suggest `Mail from` or `Sender addresses`; one rename in `templates.md` plus references.
- **Duplicated pointer contents:** `rules/SKILL.md` → Email and the `email/SKILL.md` intro both list everything `references/email.md` contains. A one-line pointer saying what the file is and when to read it would do.
- **Possible Divergent Change:** `setup/SKILL.md` renumbering Verify 6→7 forced six cross-reference edits (all updated correctly). Naming targets ("go to Verify") would make future insertions cheaper.

## M1 — round 2

**Result: PASS** (Specs PASS, Standards PASS). Non-blocking findings carried into M2 as cleanups.

### Specs

VERDICT: PASS. Diff `9c45727...HEAD`, 11 commits ending at dec47ca; `check_plugins.py` prints "Valid for Claude and OpenAI".

**Round 1 findings: all resolved** — Specs 1 (member answers kept for every interviewed member; only Gmail check and Email box wait for each member's app), Specs 2 (unplaced email flagged with a `flagged` ledger row), Standards 1–3 (criteria rule defined once in rules → Email → Where criteria live; sending policy only in rules → Sending; completion criteria match their steps), judgement calls (`Emails` → `Mail from`; one-line pointers).

**New findings (none blocking)**

- (c) 1. **Interrupted setup can still lose member answers.** Spec: "Members round: email addresses, and each member's importance question when email is on." `templates.md` says the answers "stay until the root Email box is ticked", but root part step 6 ticks that box before the member part (steps 2–3) writes the members' records. If a session stops in between, the answers are gone and Join re-asks only the members present. Fix: tick the root box after the member records are written, or keep the answers until then.
- (c) 2. **The ledger key changes from draft to sent.** Spec: `inbox.md` has "one row per handled email: Gmail message id … outcome … `sent → <diary path>`". `email` step 5.4 turns "the draft's row" into `sent`, but rules → Ledger keys a new email "on its draft or sent message id", and the two ids differ in Gmail. Say which id the row keeps after the send.
- (a) 3. **Two small gaps in setup:** the 2d completion criterion doesn't say member records aren't needed when email is declined (only the root box gets "(off)"); a sent-mail diary entry has no file-name rule (filing has one; the `inbox.md` template example implies `<slug>-YYYYMMDD_HHMM`).
- (b) Scope creep: nothing new (third-person body, README row, `see Conventions` deferred to F8).

**Checked and correct:** sending only on confirmation of the exact message, emailed instruction → draft; member mail matched on address only; filing only forwarded/CC'd, forwarding member as author, `To file:` line; solo root files only on request; prerequisites item 7; Email branch, Join, Migrate note, Verify line; pre-email `setup.md` reads as off; criteria asked on *not asked*/*invited*, "no preference" recorded; `email` frontmatter and `agents/openai.yaml`.

### Standards

VERDICT: PASS. Read `git diff 9c457275...HEAD` (11 commits), focused on the fix commits 4ab04bf..dec47ca, against AGENTS.md and cruna-core `writing-for-agents`.

**Round-1 Standards findings: all resolved** — criteria rule single-sourced in `rules/references/email.md` → Where criteria live; app-approval warning only in rules → Sending; `email` step 1 reads `inbox.md`, migration step 6 names all three changes; `Mail from` rename with no leftovers; one-line pointers; Spec finding 2 fixed; Divergent Change deliberately not done (optional).

**New observations (judgement calls, not blocking)**

- **Possible Duplicated Code.** The new-root split of the member part ("records (steps 2–3) for every member interviewed, Gmail and the Email box (steps 1 and 4) for the member present") is written three times: `setup/SKILL.md` step 2d, the first "Run from" bullet in `setup/references/email.md`, and the Member-part preamble in the same file. The preamble is the natural single source.
- **Possible cache of the environment.** `email/SKILL.md` step 5.2 restates the ChatGPT send limits before pointing to prerequisites item 7. A pointer alone would do.
- **Minor wording drift.** rules → Sending says "client", `email` step 5.2 says "app"; "app" is the consistent term.

## M2 — round 1

**Result: FAIL** (Specs FAIL, Standards FAIL)

### Specs

VERDICT: FAIL. Scope: `git diff 9c45727...HEAD`, focused on the 11 M2 commits after 7ce7955 (3561e2a through f1a3287) in `cruna-plugin-eng/agora/`.

**(c) Implemented, but wrong**

1. **Forwarded member mail carrying an instruction gets filed, and the draft never happens (blocker).**
   - Spec: "Member mail is always important. An instruction inside it produces a draft flagged for approval." Story 25: "email the assistant an instruction ('reply saying we accept') and find a draft waiting for my approval".
   - In `rules/references/email.md` → Reviews → Inbox tidy-up step 3, the first matching case wins, and "Forwarded or CC'd member mail that places → **file**" comes before "Member mail carrying an instruction → **draft**".
   - The usual case — a member forwards a client email with "reply saying we accept" — places, so it is filed and no draft is written.
   - The opposite case also goes wrong: if it can't be placed, it is drafted but never flagged with the question of where it belongs, losing story 13 ("flagged under Needs attention with the question that settles it") and story 30.
   - Fix: let the instruction case add a draft on top of the file or flag. The ledger already supports this ("a reply to a `filed` email gets its own row").
2. **Minor: linking can't tell attachments apart.** The open-line rule asks whether "any name on it has no later entry … `Attachment of: diary/<entry>`", but file-document renames the file and writes only `Filed: documents/<new name>`. With several attachments nothing records which original name was linked. Have the linking entry say `Attachment of: diary/<entry> (<original name>)`.

**Decisions to check**

- **Routines drafting for member mail with an instruction:** consistent with the spec (behaviour contract and story 25). The spec's `review` bullet ("draft (only under a convention)") and story 27 ("only flag (never draft) by default") concern proactive replies to other senders (story 26). Reconcile the `review` bullet wording in the M3 spec update.
- **30-day search window:** acceptable but departs from the spec ("read mail received since the last ledger entry"); if no routine runs for over 30 days, that mail is never seen. The stated reason is weaker than claimed: a later outcome refreshes a row's date, so a 60-day window would also be safe. Consider widening to 60 days, or flagging a gap longer than 30 days.

**(a) Missing or partial:** nothing. Covered: Email section; important email also in Needs attention; invitation line in chat only, `not asked` → `invited`; monthly line by state, quieter for `no preference`; criteria asked in attended runs; no double filing; 60-day prune; no auto-filing in solo roots; attachments flagged via status roll-up and linked by file-document; `Email importance:` state line in `remember`; `ask` reads the inbox only with `email: on`; `help` covers email, limits, item 7 troubleshooting. M1 round-2 cleanups resolved.

**(b) Scope creep (small, nothing to remove):** help's tour mentions email can be turned on in business roots; `ask` cites emails by sender, date and subject; an email important only by one member's criteria is flagged for that member by name.

### Standards

VERDICT: FAIL. Checked `git diff 9c45727...HEAD` (22 commits) against AGENTS.md and cruna-core `writing-for-agents`, mostly the M2 commits. Ledger writing single-sourced in rules → Email → The ledger; M1 round-2 cleanups done; Reviews behaviour defined once in rules → Email → Reviews with `review` saying only when each fires; renumbering updated everywhere; frontmatter fine.

**Fix before merge**

1. **Duplicated Code, and the copies disagree: merging duplicate copies of `inbox.md`.** M2 changed rules → Email → The ledger to merge copies into "one row per message id (the latest outcome)", but `rules/SKILL.md` → Replace (line 107) still merges `inbox.md` "like a tracker": "every distinct row". `review` tidy-up item 1 sends a duplicated `inbox.md` to rules → Replace, which can leave two rows for one email (`drafted` and `sent`), breaking the step's Done-when: "every email the search returned has exactly one ledger row". Fix: Replace and `review` item 1 both point to The ledger for `inbox.md`.

**Judgement calls (not blocking)**

- **Possible Duplicated Code:** "personal or root-wide" routing for importance criteria is written in `remember` step 1 and `email` step 7 → Criteria. `remember` only needs to own the personal state line and hand root-wide criteria back.
- **Possible restated rule:** the `Email importance:` state-line format ("one line per file, replaced in place") is in both `remember` and rules → Invitation state. A plain pointer would do.
- **Possible restated rule:** `rules/SKILL.md` → Status update restates the open `To file:` Needs attention behaviour that `references/email.md` → Filing defines.
- **Imprecise pointer:** `ask`'s Assistant inbox bullet says "to file, draft or send, hand off to the `email` skill (step 6)" — step 6 is File only (draft/send are 4 and 5) — and repeats the hand-off line under "Writing belongs to other skills". Drop the step number or the sentence.
- **Possible Divergent Change:** `review/SKILL.md` renumbering again; naming items would make insertions cheaper. Optional.

## M2 — round 2

**Result: PASS** (Specs PASS, Standards PASS). Non-blocking findings carried into M3.

### Specs

VERDICT: PASS. Compared `git diff 9c45727...HEAD` against spec.md and plan.md; read the five M2 revision-1 commits (f363d38 to d594cc3) closely.

**Round 1 findings: all resolved** — (c)1 blocker: tidy-up step 3 drafts the reply "on top of filing or flagging it, never in place of either", on its own ledger row; Member mail and The ledger agree; manual-test 15.3/15.4 cover both paths. (c)2: `Attachment of: diary/<entry> (<original name>)` in file-document and Filing. Standards blocker: `inbox.md` merge defined once in The ledger. Judgement calls resolved.

**(a) Missing or partial:** none.

**(b) Scope creep:** the new gap flag. Spec: "read mail received since the last ledger entry". Justified hardening, not a problem.

**(c) Possibly wrong: the gap flag's proxy (not blocking)**
"Last tidy-up" is "the date of the newest ledger row by `routine`":
1. A tidy-up that finds no new mail writes no row (also when members handled everything in conversation, or a later member outcome replaces a routine row's by/date). In a quiet or conversation-heavy root, after 60 days the flag fires falsely, on every run until a routine writes a row.
2. In that run, step 4 prunes the old routine row, leaving a non-empty ledger with no routine row, which step 1 doesn't cover (only "Empty ledger: the period's look-back window").

Acceptable for merge. Cheap fix: "no `routine` row → search 60 days back, no flag"; better, a `last_tidy_up:` front-matter field in `inbox.md` written by every tidy-up, replacing the proxy (changes the spec's data shape). Neither the flag nor its proxy is in the spec, so M3 should add them to the spec or drop them. How a raised gap flag is cleared is undefined.

**Carried to M3:** reconcile the spec's `review` bullet "draft (only under a convention)" with routine drafts for member-mail instructions.

### Standards

VERDICT: PASS. `git diff 9c45727...HEAD` (29 commits), focused on the M2 revision 1 commits, against AGENTS.md, cruna-core `writing-for-agents` and the smell baseline.

**Round 1 findings: all resolved** — `inbox.md` merge single-sourced in The ledger (a duplicated tracker still merges every distinct row); `remember` keeps only the state line and hands the rest to `email` → Criteria; Invitation state and `remember` point to each other without restating; Status update is a plain pointer to Filing; `ask`'s wrong-step hand-off removed; both spec fixes in.

**New issues (judgement calls, not blocking)**

1. **Gap detection rests on a value that doesn't track the last tidy-up** (`rules/references/email.md:98`) — same as Specs (c) above: false flag in a quiet inbox; no rule for a non-empty ledger without a `routine` row. Suggested: fall back to the look-back window when there is no `routine` row, and record the last tidy-up date explicitly or flag only when no review has run.
2. **Ambiguous sentence** (The ledger, first bullet): with "unless it is the draft a member-mail instruction asks for" inserted, the clause after the colon reads as if it describes the exception. Move the exception to the end or leave it to the second bullet (which already covers it).
3. **Possible Divergent Change** (carried over, optional): numbered tidy-up items in `review/SKILL.md` referenced by number.

## M3 — round 1

**Result: PASS** (Specs PASS, Standards PASS). Whole feature F1–F8 meets its acceptance criteria; release depends on running the live manual test.

### Specs

VERDICT: PASS. Checked `git diff 9c45727...HEAD` (39 commits), M3 commits after 27b1ce4 closely; spec.md before and after its M3 update against report.md, review.md and plan.md; checker "Valid for Claude and OpenAI"; 0.4.0 in all four manifests and the Claude catalog (the only catalog that carries a version); every English skill file has an Italian counterpart with the same line and heading count.

**M2 carry-over (`last_tidy_up`): resolved.** The ledger and Inbox tidy-up use the `last_tidy_up` front-matter field instead of the "newest `routine` row" proxy; every tidy-up writes it, conversations never do; merged copies keep the latest value; no value → 60 days back, no flag; the gap flag is a status item the next tidy-up drops. Template, setup step 5 and review item 3 match.

**spec.md updates are faithful:** story 27 and the `review` bullet allow routine drafts for member-mail instructions; story 46 and the data shape include the gap flag and `last_tidy_up`; story 23 adds the third-person voice; story 9 (old `setup.md` reads as email off) matches; `Mail from`, `see Conventions`, item 7, `To file:` / `Attachment of:` and the member-mail draft on its own row all match recorded decisions. No requirement quietly dropped.

**(a) Missing or partial:** none in the deliverables. Open plan item: the M3 demo says the manual test "has been run on Claude and on ChatGPT, with results recorded". It has not been run (report.md says so); the user must run it, including the ChatGPT send-limit and reply-to-review-chat checks, before release.

**Manual test** (`docs/agora/manual-tests/manual-test-agora-v040-email.md`): complete and ready; scenarios 1–10 match spec → Testing Decisions one to one, Claude and ChatGPT columns, checks for the gap flag, `last_tidy_up` and the two unverified platform facts, plus an Italian pass (scenario 11).

**F7 Italian:** `email` skill with `agents/openai.yaml`, `rules/references/email.md`, `setup/references/email.md` present; `last_tidy_up` carried everywhere; glossary has the email terms, every `rules → Email` heading and the identifiers that stay in English.

**(b) Scope creep:** nothing to remove. Italian Convention texts translated as content, per "Content follows the root's language".

**(c) Possibly wrong (minor):** glossary renders "look-back window" as "finestra di riepilogo" ("summary window"); "finestra di ricerca" or "periodo considerato" would be more accurate.

### Standards

VERDICT: PASS. Against AGENTS.md and cruna-core `portable-plugins`.

**Versions, manifests and catalogs:** `agora` and `agora-ita` at 0.4.0 in both `.claude-plugin/plugin.json`, both root `plugin.json` and `.claude-plugin/marketplace.json`. `.agents/plugins/marketplace.json` has no version field, which portable-plugins confirms ("The Claude entry carries `version`; the OpenAI entry carries `policy` and `category`"; workflow E bumps only the Claude-side files). Checker run: "Valid for Claude and OpenAI", exit 0.

**Italian mirrors English:** all 15 changed skill files match in line, heading and bullet/step count (e.g. `rules/references/email.md` 139/17/56); headings match the glossary table; `last_tidy_up` at the same lines in both languages and in `agora-spec.md`; glossary used consistently ("protocollo" for ledger, distinct from "registro" for tracker; "riordino della casella"; parsed identifiers in English); Italian README has the `email` row.

**Smell baseline (judgement calls, none blocking):**
- Possible Duplicated Code: the spec's Email section restates `rules → Email` mechanics; suppressed (the spec describes behaviour), but the next ledger change means editing spec and rules in both languages.
- Minor glossary drift: `cruna-plugin-ita/agora/skills/setup/SKILL.md:148` "la casella di posta è quella dell'account dell'assistente" vs glossary "casella dell'assistente". Optional.
- Shotgun Surgery: `last_tidy_up` touched 4+4 files plus the spec; the repo's deliberate structure, suppressed.
