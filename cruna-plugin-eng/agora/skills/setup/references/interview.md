# Setup interview

Gather what `org.md` and the first interests need. Ask in rounds of two to four questions. Every question carries a **recommended answer**, inferred from what you already know, so the user can reply "yes" or one word. Push back gently when an answer will cause trouble later (an interest that is really an instance, a member with no Google account), then accept the user's decision.

Keep the user's own words for names and descriptions. Ask in the language the user is writing in; once the language is settled, use it.

## Round 1: The root

- **Display name** (recommend the family surname or the business name: "Fabiole family").
- **Language** for all content (recommend the language the user is writing in).
- **Timezone** (recommend the one implied by their city or calendar).
- **Email** (optional), asked as in Email questions below.

## Round 2: Members

For each person who will use or be tracked by the root:

- **Name** (the slug follows rules → Vocabulary).
- **Role** in the group (parent, child, partner, owner, employee, accountant).
- **Birthday** (optional; useful for age-related reminders).
- **Personal Google account email**, for members who will use the root, so the folder and calendar can be shared with them. Shared services mode: their **Workspace account** instead, the one their connectors will sign in as (the owner's is the account item 3 found).
- **Personal calendar sharing**: free/busy only (recommended: enough to find free time) or full details. Shared services mode: `domain free/busy` (recommended: Workspace already shows it), or full details if they will share more.
- **Kind** (shared services mode): `human` or `ai`, whether a person or an AI uses the account. Recommend `human` (rules → Vocabulary → Member).
- **How they will use it**: their own AI app (Claude or ChatGPT), or only through others (a child tracked but not using it).

Recommend: list the people the user has already mentioned and ask only for what's missing.

When email is on, also ask each member the member questions in Email questions below.

Record the email in the member's Email column and the app in their Access row.

## Round 3: Layout convention

What one interest stands for, so every later addition is consistent. Propose by the kind of root, then let the user decide:

- **Family**: one interest per person, plus `family` for shared things (home, vacations, car). Instances are areas of that person's life: `nina/health`, `nina/school`, `family/vacations`.
- **Business**: one interest per client, plus `company` for internal matters. Instances are jobs or projects: `acme/website-2026`, `company/taxes`.
- **Other**: by topic (`house`, `money`, `health`), instances are sub-topics.

Write the chosen convention as one sentence in `org.md` `layout`. Remind the user that anything smaller than an instance (one visit, one invoice) is a diary entry, not a folder.

## Round 4: First interests and instances

Propose a starter set from the convention and what the user has said (for a family of four: one interest per person with `health` and `school`, plus `family/home`). For each, collect a one-line description. Every interest gets at least one instance, since only instances hold diary entries and documents. Offer a calendar color per interest (optional).

Keep it small: three to six interests is plenty; more can be added any time with "start tracking…".

Ask whether they already keep track of anything in a sheet or a note (feeds, nappies, expenses, a shopping list). Recommend "no" unless they mentioned one. For each yes, note what it is, where it lives and which instance it belongs to: after the build it is offered for import (setup Output).

## Email questions

Asked in rounds 1 and 2 for a new root, and on their own when an existing root turns email on or a member joins a root with email on.

- **Root**: "Should the assistant have its own email inbox? Members forward or CC it mail to keep, it drafts and sends emails for you (only after you confirm each one), and the reviews report on it." Say it suits a business best. Recommend yes for a business, no for a family. Shared services mode: the inbox is the group's address, and each member's AI reads the group's mail in their own mailbox; ask "Should Agora handle the mail sent to <group address>?", recommending yes.
- **Root, when yes**: what counts as important email for the whole group (recommend from the root's kind: "anything from a client"; "no preference" is a fine answer).
- **Each member, when yes**:
  - every **email address** they write or forward from (recommend their Google account email plus any work address they mentioned); skip in a solo root;
  - what counts as important email **for them** ("school emails", "the bank"; "no preference" is fine).

## Closing summary

Show the user one compact summary (members, root name, language, timezone, email on or off, layout, interests and instances) and ask for a final yes. Corrections go back into the matching round.
