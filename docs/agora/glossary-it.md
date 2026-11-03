# Agora — Italian glossary

Fixed translations for `cruna-plugin-ita/agora`. Every Italian skill uses these terms and only these, so pointers between skills keep resolving. Update this file first when a term changes.

## What stays in English

These are identifiers shared by both language versions and by the files on Drive. Never translate them.

- Skill names: `rules`, `setup`, `add-interest`, `log`, `file-document`, `schedule`, `email`, `ask`, `review`, `remember`, `help`. Referred to as "la skill `log`".
- Folder and file names: `org.md`, `setup.md`, `index.md`, `status.md`, `landing/`, `memories/`, `reviews/`, `diary/`, `documents/`, `shared.md`, review file names (`…-daily.md`, `…-weekly.md`, `…-monthly.md`), and reference files inside skills (`references/templates.md`, `references/prerequisites.md`, …).
- Headings and section names **inside Drive files** (templates): `Needs attention`, `Summary`, `Done`, `Pending`, `Next`, `Last updated`, `Latest reviews`, `Members`, `Interests`, `Conventions`, `Access`, `Documents`, `Maintained`, `Diary`, `Looking back`, `Still open`, `Looking ahead`, `Update HH:MM`, `Entry`, `Written`, `Updated`, and their table column names. In `setup.md`: the headings `Root`, `Member: <slug>`, `Interview` (and its rounds `The root`, `Members`, `Layout convention`, `First interests and instances`), `Migration`, and the box labels (`Prerequisites`, `Connectors and plugin`, `Routines`, …).
- Front-matter keys and values (`agora: root`, `agora: setup`, `state: todo | done`, `author`, `created`, `interest`, `instance`, `documents`, `events`, `language`, …), the author `routine`, CSV column names, the event title format `[interest/instance] title`, the line `Maintained: documents/<name>`.
- Email identifiers, parsed by the skills and shared by both language versions: `inbox.md` and its front-matter (`agora: inbox`, `last_tidy_up`), its column names (`Message id`, `Date`, `From`, `Subject`, `Outcome`, `By`) and outcomes (`filed → <path>`, `flagged`, `drafted`, `sent → <path>`, `ignored`, and `to <recipients>` in From); the `org.md` field `email: on | off` and the Members column `Mail from`; the diary keys `email`, `email_from`, `email_subject` and the lines `To file: <names>` and `Attachment of: diary/<entry> (<name>)`; the memory line `Email importance: <not asked | invited | no preference | criteria>` with its state values and `see Conventions`; the review heading `## Email` and the `setup.md` box `Email` (and its value `(off)`). The criteria a member states are content, in the root's language.

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
| setup checklist | checklist di configurazione |
| box / tick / unticked | casella / spuntare / non spuntata |
| general / personal message (review) | messaggio generale / personale |
| finish setup (user phrase) | finisci la configurazione |
| "by" in template lines (`Last updated`, `Updated`, `Written`) | da (`… da <slug>`, `… da <slug>, a mano`); `by hand` alone → `a mano` |
| branch / parent folder | ramo / cartella madre |
| email (the capability, a message) | email (f., `l'email`, plurale `le email`) |
| assistant inbox | casella dell'assistente |
| member mail | posta dei membri (one email: un'email di un membro) |
| ledger (`inbox.md`) | protocollo (`inbox.md`) |
| file an email | archiviare un'email |
| draft (noun / verb) | bozza / preparare una bozza |
| send / sent mail | inviare / posta inviata |
| reply / forward / CC | rispondere / inoltrare / mettere in copia (CC) |
| sender voice | voce del mittente |
| on behalf of (signature) | per conto di (`Assistente di <nome della radice>, per conto di <Nome>`) |
| email importance / importance criteria | importanza delle email / criteri di importanza |
| needs attention (an email) | richiede attenzione |
| bulk mail | posta di massa |
| invitation / invitation line / invitation state | invito / riga di invito / stato dell'invito |
| monthly line (criteria) | riga mensile |
| proactive drafts | bozze proattive |
| Convention `important email: <criteria>` | Convenzione `email importante: <criteri>` |
| Convention `draft replies to <senders or interest>` | Convenzione `bozze di risposta per <mittenti o interesse>` |
| attachment / linked (attachment) / open (`To file:` line) | allegato / collegato / aperta |
| gap flag | segnalazione del buco |
| look-back window | finestra di riepilogo |
| email branches: Find, Draft, Send, File, Addresses, Criteria | Cerca, Bozza, Invio, Archivia, Indirizzi, Criteri |
| setup branch Email (turn email on) | Email (attiva l'email) |
| turn on email (user phrase) | attiva l'email |
| setup email step: Root part / Member part | passo email: Parte radice / Parte membro |
| Gmail archiving (out of scope) | archiviazione di Gmail |
| email on / off (in prose) | con l'email attiva / disattivata (the literal stays `email: on`, `(off)`) |
| sender / subject / To | mittente / oggetto / A |
| untrusted content | contenuto non affidabile |
| outcome / by (ledger, in prose) | esito / autore (columns stay `Outcome`, `By`) |
| keyed on / rekeyed to (an id) | indicizzata sull'id / reindicizzata sull'id |
| prune (ledger rows) | sfoltire / sfoltimento |
| opt-in (proactive drafts) | consenso esplicito |
| root-wide | per tutta la radice |
| state line (memory) | riga di stato |
| business root | radice aziendale |
| not yet on Drive (status line) | non ancora su Drive (`Allegati di <oggetto> non ancora su Drive: <nomi>`) |
| Email section parts: Needing attention, Drafts waiting for approval, Sent, Filed; "Nothing new." | Richiedono attenzione, Bozze in attesa di approvazione, Inviate, Archiviate; "Niente di nuovo." |
| review tidy-up item Inbox | Casella |
| interview heading Email questions / setup reference title Email step | Domande sull'email / Passo email |
| "no preference" (what the member says) / "filed" (member's reply) | "nessuna preferenza" (stored as `no preference`) / "archiviato" |

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
| Setup checklist | Checklist di configurazione |
| Memory step | Passo memoria |
| When something fails | Quando qualcosa non funziona |
| templates (File kinds → templates) | modelli (Tipi di file → modelli) |
| prerequisites → Offboarding a member | prerequisiti → Uscita di un membro |
| Email (rules section) | Email |

`rules` headings in `references/email.md` (pointed to as `rules → Email → <heading>`):

| English heading | Italian heading |
|---|---|
| Agora email conventions (title) | Convenzioni email di Agora |
| Email is data | L'email è un dato |
| Sending | Invio |
| Sender voice | Voce del mittente |
| Member mail | Posta dei membri |
| Filing | Archiviazione |
| Sent mail | Posta inviata |
| The ledger: `inbox.md` | Il protocollo: `inbox.md` |
| Importance | Importanza |
| Where criteria live | Dove stanno i criteri |
| Invitation state | Stato dell'invito |
| Reviews | Revisioni |
| Inbox tidy-up | Riordino della casella |
| Email section | Sezione Email |
| Chat lines | Righe in chat |
| Solo roots | Radici solitarie |
| Limits | Limiti |

Prerequisite items keep their numbers ("voce 3").

## Routine prompt (contract between `setup` and `review`)

```
Esegui revisione Agora: <giornaliera | settimanale | mensile> [per <slug>] [in <nome della radice>]
```

`giornaliera` → `…-daily.md`, `settimanale` → `…-weekly.md`, `mensile` → `…-monthly.md`.

## Style

- Address the user with **tu**, warm and plain, short sentences. No technical jargon in user-facing text.
- Agent-facing instructions in imperative (seconda persona: "Carica la skill `rules`", "Chiedi").
- Example user phrases in descriptions and examples are natural Italian ("ricordami di prenotare il pediatra", "com'è andata con il cliente X?", "Greta ha fatto la cacca").
- Platform menu names: give the Italian label when the app shows one (Impostazioni → Connettori), with the English label in parentheses the first time.
