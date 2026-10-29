# Agora — Italian glossary

Fixed translations for `cruna-plugin-ita/agora`. Every Italian skill uses these terms and only these, so pointers between skills keep resolving. Update this file first when a term changes.

## What stays in English

These are identifiers shared by both language versions and by the files on Drive. Never translate them.

- Skill names: `rules`, `setup`, `add-interest`, `log`, `file-document`, `schedule`, `ask`, `review`, `remember`, `help`. Referred to as "la skill `log`".
- Folder and file names: `org.md`, `index.md`, `status.md`, `landing/`, `memories/`, `reviews/`, `diary/`, `documents/`, `shared.md`, review file names (`…-daily.md`, `…-weekly.md`, `…-monthly.md`), and reference files inside skills (`references/templates.md`, `references/prerequisites.md`, …).
- Headings and section names **inside Drive files** (templates): `Needs attention`, `Summary`, `Done`, `Pending`, `Next`, `Last updated`, `Latest reviews`, `Members`, `Interests`, `Conventions`, `Access`, `Documents`, `Maintained`, `Diary`, `Looking back`, `Still open`, `Looking ahead`, `Update HH:MM`, `Entry`, `Written`, `Updated`, and their table column names.
- Front-matter keys (`agora: root`, `author`, `created`, `interest`, `instance`, `documents`, `events`, `language`, …), the author `routine`, CSV column names, the event title format `[interest/instance] title`, the line `Maintained: documents/<name>`.

Content written into those files is in the root's language (`it` for this plugin's default).

## Terms

| English | Italiano |
|---|---|
| root | radice |
| interest | interesse |
| instance | istanza |
| member | membro |
| slug | slug |
| assistant account | account dell'assistente |
| system calendar | calendario di sistema |
| layout convention | convenzione di struttura |
| diary entry | voce di diario |
| document (uploaded) | documento |
| working file | file di lavoro |
| tracker | registro |
| list | lista |
| Convention (org.md line) | Convenzione |
| memory / memories | memoria / memorie |
| review | revisione |
| routine (scheduled task) | routine |
| scheduled task | attività pianificata |
| connector | connettore |
| landing folder | cartella `landing/` |
| source / derived / curated / working (file kinds) | sorgente / derivato / curato / di lavoro |
| replace | sostituire |
| stale | obsoleto |
| propagate | propagare |
| flag (verb / noun) | segnalare / segnalazione |
| unattended / attended run | esecuzione non presidiata / presidiata |
| tidy-up | riordino |
| prerequisites | prerequisiti |
| ready / blocked | pronto / bloccato |
| Done when | Fatto quando |
| Output | Output |
| prerequisite item N | voce N |
| solo root | radice solitaria |
| root display name | nome della radice |
| owner | proprietario |
| interview / round | intervista / giro |
| build / verify | costruzione / verifica |
| setup branches: New root, Join, Migrate, Offboard | Nuova radice, Unisciti, Migra, Uscita |
| file-document Import branch | Importa |
| pending / removed (values in `org.md`) | in attesa / rimosso |
| declined (Convention value) | rifiutato |
| "by" in template lines (`Last updated`, `Updated`, `Written`) | da (`… da <slug>`, `… da <slug>, a mano`) |
| branch / parent folder | ramo / cartella madre |

Interest and instance slugs and working-file names are content, so they follow the root's language (`famiglia`, `nina/salute`, `greta-pannolini.csv`).

## `rules` headings (pointer targets)

Pointers are written `rules → <heading>` using exactly these Italian headings.

| English heading | Italian heading |
|---|---|
| Vocabulary | Vocabolario |
| Connectors only | Solo connettori |
| Finding the root | Trovare la radice |
| Layout | Struttura |
| File kinds | Tipi di file |
| Conventions | Convenzioni |
| Suggest | Suggerire |
| Frequent events | Eventi frequenti |
| Working with Drive | Lavorare con Drive |
| Replace | Sostituire |
| Status update rule | Regola di aggiornamento dello stato |
| Ask, never assume | Chiedi, non dare per scontato |
| Identity | Identità |
| Unattended runs | Esecuzioni non presidiate |
| Flag | Segnalare |
| Calendar conventions | Convenzioni del calendario |
| Memory step | Passo memoria |
| When something fails | Quando qualcosa non funziona |
| templates (File kinds → templates) | modelli (Tipi di file → modelli) |
| prerequisites → Offboarding a member | prerequisiti → Uscita di un membro |

Prerequisite items keep their numbers ("voce 3").

## Routine prompt (contract between `setup` and `review`)

```
Esegui revisione Agora: <giornaliera | settimanale | mensile> per <slug> in <nome della radice>
```

`giornaliera` → `…-daily.md`, `settimanale` → `…-weekly.md`, `mensile` → `…-monthly.md`.

## Style

- Address the user with **tu**, warm and plain, short sentences. No technical jargon in user-facing text.
- Agent-facing instructions in imperative (seconda persona: "Carica la skill `rules`", "Chiedi").
- Example user phrases in descriptions and examples are natural Italian ("ricordami di prenotare il pediatra", "com'è andata con il cliente X?", "Greta ha fatto la cacca").
- Platform menu names: give the Italian label when the app shows one (Impostazioni → Connettori), with the English label in parentheses the first time.
