# Agora — Italian glossary

Fixed translations for `cruna-plugin-ita/agora`. Every Italian skill uses these terms and only these, so pointers between skills keep resolving. Update this file first when a term changes.

## What stays in English

These are identifiers shared by both language versions and by the files on Drive. Never translate them.

- Skill names: `rules`, `setup`, `add-interest`, `log`, `file-document`, `schedule`, `email`, `ask`, `review`, `remember`, `help`, `todo`. Referred to as "la skill `log`".
- Folder and file names: `org.md`, `setup.md`, `index.md`, `status.md`, `landing/`, `memories/`, `reviews/`, `diary/`, `documents/`, `shared.md`, `todo.md`, `todos/`, `todos/archive/` (and its `YYYY/` year folders), review file names (`…-daily.md`, `…-weekly.md`, `…-monthly.md`), and reference files inside skills (`references/templates.md`, `references/prerequisites.md`, …).
- Headings and section names **inside Drive files** (templates): `Needs attention`, `Summary`, `Done`, `Pending`, `Next`, `Last updated`, `Latest reviews`, `Members`, `Interests`, `Conventions`, `Access`, `Documents`, `Maintained`, `Diary`, `Looking back`, `Still open`, `Looking ahead`, `Update HH:MM`, `Entry`, `Written`, `Updated`, and their table column names. In `setup.md`: the headings `Root`, `Member: <slug>`, `Interview` (and its rounds `The root`, `Members`, `Layout convention`, `First interests and instances`), `Migration`, and the box labels (`Prerequisites`, `Connectors and plugin`, `Routines`, …).
- Front-matter keys and values (`agora: root`, `agora: setup`, `state: todo | done`, `author`, `created`, `interest`, `instance`, `documents`, `events`, `language`, …), the author `routine`, CSV column names, the event title format `[interest/instance] title`, the line `Maintained: documents/<name>`.
- Email identifiers, parsed by the skills and shared by both language versions: `inbox.md` and its front-matter (`agora: inbox`, `last_tidy_up`), its column names (`Message id`, `Date`, `From`, `Subject`, `Outcome`, `By`) and outcomes (`filed → <path>`, `flagged`, `drafted`, `sent → <path>`, `ignored`, `outgoing`, and `to <recipients>` in From); the `org.md` field `email: on | off` and the Members column `Mail from`; the diary keys `email`, `email_from`, `email_subject` and the lines `To file: <names>` and `Attachment of: diary/<entry> (<name>)`; the memory line `Email importance: <not asked | invited | no preference | criteria>` with its state values and `see Conventions`; the review heading `## Email` and the `setup.md` box `Email` (and its value `(off)`). The criteria a member states are content, in the root's language.
- To-do identifiers, parsed by the skills and shared by both language versions: the front-matter `agora: todo` and `agora: todo-list`; the actions `add`, `done`, `reopen`, `assign`, `edit`, `remove`; the keys `action`, `author`, `created`, `owner`, `due`, `interest`, `instance`, `source`, `todo`, `text`, `filter`, `count`, `todos`; the values `everyone` and `none`, and the source form `email <Gmail message id>`; the `todo.md` title `# To-dos` and headings `## <Name>` (the member's Name from `org.md`), `## Everyone`, `## Recently done`; the review heading `## To-dos`; the `status.md` Pending line prefix `To-do:`; the ledger outcome `to-do → <id>`. The to-do text, the line contents after those markers, the parts of the review's To-dos section and a `remove` event's `filter` are content, in the root's language.
- GitHub bridge identifiers, shared with `cruna-pm-github`: the handoff lines (rules → Handoffs: `Agora log as …`, `Agora todo as …`, `Agora add-interest as …`, with their `edit`, `text`, `done`, `github:` and ` — ` separators) and the result lines `agora: ok — …` and `agora: failed — …`, which stay in English word for word, as do the reasons and descriptions in them; the Role value `github`; the interest `products` (its folder name, even in an Italian root); the diary key `github:` and its refs (`#8`, `PR #9`, `v2026.11.12`); the Conventions marker `managed by cruna-pm-github` (the text after its `: ` is content); the bridge file name `YYYY-MM-DD_hh_mm_last-check.md`; the slug `pm`.

- Shared services identifiers (0.7.0), shared by both language versions: the `org.md` fields `mode` and its values `basic-shared-account` and `shared-services`, `drive_id` and its value `my-drive`, `root_id`, `inbox`, `calendar_owner`; the Members column `Kind` and its values `human` and `ai`; the `inbox.md` column `Mailbox`; the root `status.md` line `**Tidied:** YYYY-MM-DD HH:MM`; in `setup.md`, the boxes `Workspace account`, `In the shared drive`, `In the group`, `Shared calendar`, the section `## Move to shared drive` and its box values `(email off)`, `(already)`, `(moved)`, `(none)`; the Gmail search operators (`deliveredto:`, `list:`, `from:`, `to:`, `cc:`, `OR`, `{ }`); the reference file names `references/manual-setup.md`, `references/shared-services.md`, `references/move.md`.

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
| to-do (the concept; also in user-facing text) | to-do (m., invariable: `il to-do`, `i to-do`). Never "attività" (taken by "attività pianificata") nor "compito" |
| to-do event | evento to-do (pl. eventi to-do) |
| `todo.md` (in prose) | la lista dei to-do (`todo.md`) |
| to-do list (what members read) | lista dei to-do |
| open / closed (a to-do) | aperto / chiuso |
| owner (of a to-do) | responsabile (the key stays `owner`; "proprietario" is the root owner) |
| shared to-do (owner `everyone`) | to-do condiviso, "per tutti" |
| take over ("I'll do it") | prendere in carico ("ci penso io") |
| attached (to an instance) / root level | legato a un'istanza / a livello di radice |
| due date / overdue / due ahead | scadenza / scaduto / in scadenza |
| fold (events into current state) | ricomporre (la ricomposizione degli eventi) |
| current state | stato attuale |
| list order | ordine della lista |
| orphan (event) | orfano |
| suggestion: looks done / possible new to-do | suggerimento: sembra fatto / possibile nuovo to-do |
| archive (to-dos, `todos/archive/`) | archivio; spostare in archivio (never "archiviare", which is filing an email) |
| remove (to-dos) / trash | rimuovere / cestino (spostare nel cestino) |
| "including open ones" (remove filter) | "compresi quelli aperti" |
| `todo` branches: Add, Done, Reopen, Assign, Edit, List, Remove, Calendar | Aggiungi, Fatto, Riapri, Assegna, Modifica, Lista, Rimuovi, Calendario |
| `todo` steps: Read the list, Change, Write and rebuild | Leggi la lista, Cambia, Scrivi e ricostruisci |
| `email` step / branch To-do; `log` step Offer to-dos; `review` step Confirm suggestions; tidy-up item To-dos | To-do; Proponi i to-do; Conferma i suggerimenti; To-do |
| review To-dos section parts: Overdue, Due ahead, Opened, Done, Suggestions, No date, open over 30 days | Scaduti, In scadenza, Aperti, Fatti, Suggerimenti, Senza data, aperti da oltre 30 giorni |
| `todo.md` line marker **Overdue**; "no date"; "due" (in a line) | **Scaduto**; senza data; scadenza |
| "no preference" (what the member says) / "filed" (member's reply) | "nessuna preferenza" (stored as `no preference`) / "archiviato" |
| handoff (one-line prompt from another plugin) | handoff (m., invariable: `l'handoff`, `gli handoff`) |
| handoff shape / result line | forma dell'handoff / riga di risultato |
| github member | membro github |
| managed instance | istanza gestita |
| bridge / bridge file | ponte / file del ponte |
| last-check (file) | `last-check` (the file name stays) |
| link / unlink a repo | collegare / scollegare un repo |
| sync (of `cruna-pm-github`) | sincronizzazione |
| GitHub request | richiesta GitHub |
| ref (issue, PR or tag) | riferimento |
| products from GitHub (help) | prodotti da GitHub |
| "Software products built in GitHub" (`products` description) | "Prodotti software costruiti su GitHub" |
| "PM (GitHub)" / "address for GitHub through cruna-pm-github" (Members row) | "PM (GitHub)" / "indirizzo per GitHub tramite cruna-pm-github" |
| "notes and to-dos for pm become GitHub requests" (managed Convention text) | "note e to-do per pm diventano richieste GitHub" |
| "noted; it becomes a GitHub request at the next sync" (`log` reply) | "annotato; diventerà una richiesta GitHub alla prossima sincronizzazione" |
| mode | modalità |
| basic shared account mode (`basic-shared-account`); short: basic mode | modalità account condiviso base; in breve: modalità base |
| shared services mode (`shared-services`) | modalità servizi condivisi |
| shared drive | Drive condiviso (pl. Drive condivisi) |
| shared calendar | calendario condiviso |
| Google Group / group address | gruppo Google / indirizzo del gruppo |
| connected account | account connesso |
| Workspace account / Google Workspace | account Workspace / Google Workspace |
| admin (Workspace, AI client) | amministratore |
| calendar owner | proprietario del calendario |
| Content manager (shared drive role) | Gestore contenuti (Content manager) |
| Make changes to events (calendar permission) | Apporta modifiche agli eventi (Make changes to events) |
| See all event details | Visualizza tutti i dettagli degli eventi (See all event details) |
| the three ids / Same answer (rule) | i tre id / Stessa risposta |
| mail filter; received-mail form / sent-mail form | filtro della posta; forma per la posta ricevuta / forma per la posta inviata |
| list-id (the group address with `@` as `.`) | list-id (identificatore, resta in inglese) |
| what the member sent to the group | ciò che il membro stesso ha inviato al gruppo |
| retired assistant address | indirizzo dismesso dell'assistente |
| mailbox / your mailbox | casella di posta / la tua casella |
| email time | ora dell'email |
| duplicate key / the same email | chiave dei duplicati / la stessa email |
| speaker / the speaker's own calendar | chi parla / il calendario di chi parla |
| `domain free/busy` (Personal calendar value) | `libero/occupato del dominio` |
| `content manager` (Access Drive value) | `gestore contenuti` |
| `can edit` (Access System calendar value) | `può modificare` |
| `receives mail` (Access Notes) | `riceve la posta` |
| real-call check / create-then-trash / create-then-delete | controllo con una chiamata reale / crea e poi cestina / crea e poi elimina |
| test file / test event | file di prova / evento di prova |
| manual setup (guide) | configurazione manuale |
| setup branch Move to shared drive | Sposta su Drive condiviso |
| move / copy (the root) / count check | spostare / copiare (la radice) / controllo del conteggio |
| hand-over (from the assistant account) | passaggio di consegne |
| old root / old account / retired | vecchia radice / vecchio account / dismesso |
| forwarding (Gmail) | inoltro |
| linked repo | repo collegato |
| recently tidied (review) | riordinata da poco |
| overdue routines (review item) | routine in ritardo |
| flag text "No member's routine has run this: set up the routines (the `setup` skill, routines)" | "Nessuna routine dei membri l'ha eseguito: configura le routine (la skill `setup`, routine)" |
| flag text "Email wasn't triaged between <last_tidy_up> and today." | "L'email non è stata smistata tra <last_tidy_up> e oggi." |
| "join Agora" (user phrase) | "unisciti ad Agora" |
| "move Agora to a shared drive" / "we have Workspace now" (user phrases) | "sposta Agora su un Drive condiviso" / "ora abbiamo Workspace" |
| The three ids' parts: Files / Calendar / Mail | File / Calendario / Posta (`rules → I tre id → Posta`) |
| prerequisites item 3 checks: Who / Drive / Calendar | Chi / Drive / Calendar |
| item 4, step N (manual-setup.md) | voce 4, passo N |
| `setup.md` box notes: (admin) / (calendar owner: <slug>) | (amministratore) / (proprietario del calendario: <slug>) |
| `setup.md` Move box note: old root `<id>` | vecchia radice `<id>` |
| Personal calendar `full details` / `free/busy` | `tutti i dettagli` / `libero/occupato` |
| work account | account di lavoro |
| ownership check | controllo di proprietà |
| reconnect | ricollegarsi / ricollegamento |
| License (README heading; the license name `Apache License 2.0` and the file `LICENSE` stay in English) | Licenza |
| Join's step "Which join" | Quale ingresso |
| review tidy-up items | punti (punto 8, punti 1–6) |
| Workspace free/busy | il libero/occupato di Workspace |
| Group settings: Each email; outside posting; Admin console | Ogni email; pubblicazione dall'esterno; Console di amministrazione (Admin console) |
| Google UI labels used | Il mio Drive (My Drive), Sposta nel cestino (Move to trash), Gestisci membri (Manage members), Integra calendario (Integrate calendar), ID calendario (Calendar ID), Condiviso con (Shared with), Trasferisci la proprietà (Transfer ownership), Condividi con persone specifiche o gruppi (Share with specific people or groups), Aggiungi persone e gruppi, Fuso orario, Utenti, Aggiungi nuovo utente, Gruppi, Crea gruppo, Limitato (Restricted), Personalizzato (Custom), Esterno (External), Chi può pubblicare, Chi può unirsi al gruppo, Solo utenti invitati, Chiunque sul Web, Rimuovi accesso, Rimuovi membro, Sospendi utente, Elimina utente; Gmail: Inoltro e POP/IMAP, Aggiungi un indirizzo di inoltro, Inoltra una copia della posta in arrivo a, Salva modifiche. Calendar permissions in the imperative, as Google shows them: Apporta modifiche agli eventi, Visualizza tutti i dettagli degli eventi |

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
| To-dos (rules section) | To-do |
| Handoffs | Handoff |
| Handoffs → Check, then write | Handoff → Controlla, poi scrivi |
| Handoff (section in `log`, `todo`, `add-interest`) | Handoff |
| The three ids | I tre id |
| Vocabulary → Mode | Vocabolario → Modalità |
| Calendar conventions → The speaker's own calendar | Convenzioni del calendario → Il calendario di chi parla |

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
| Mailboxes | Caselle di posta |

`rules` headings in `references/todos.md` (pointed to as `references/todos.md → <heading>`):

| English heading | Italian heading |
|---|---|
| Agora to-do conventions (title) | Convenzioni dei to-do di Agora |
| Who sees and owns a to-do | Chi vede un to-do e a chi tocca |
| Place | Collocazione |
| Due dates | Scadenze |
| To-do events | Eventi to-do |
| Who writes events | Chi scrive gli eventi |
| Current state | Stato attuale |
| `todo.md` | `todo.md` |
| In status | Nello stato |
| Suggestions | Suggerimenti |
| Reviews | Revisioni |
| Remove | Rimozione |
| Archive | Archivio |

Other headings added in 0.7.0 (pointer targets):

| English heading | Italian heading |
|---|---|
| prerequisites: Basic mode / Shared services mode (item variants) | Modalità base / Modalità servizi condivisi |
| prerequisites item 4: Assistant account, or the admin's setup; Basic mode: assistant account; Shared services mode: the admin's setup | Account dell'assistente, o la configurazione dell'amministratore; Modalità base: account dell'assistente; Modalità servizi condivisi: la configurazione dell'amministratore |
| `manual-setup.md`: Shared services: the manual setup (title); Once per root; Per new member, before they join; The joining member; A member leaving | Servizi condivisi: la configurazione manuale; Una volta per radice; Per ogni nuovo membro, prima che si unisca; Il membro che si unisce; Un membro che esce |
| `shared-services.md`: Setup in shared services mode (title); sections keep their step numbers and setup's names (2a. Prerequisiti, 3. Unisciti, …) | Configurazione in modalità servizi condivisi |
| `move.md`: Move a basic root to a shared drive (title); Steps | Spostare una radice base su un Drive condiviso; Passi |
| setup step 4b. Move to shared drive (basic → shared services) | 4b. Sposta su Drive condiviso (da base a servizi condivisi) |
| help FAQ: Which kind of Agora do we have?; Agora says I'm not a member yet; How do we move to a shared drive? | Che tipo di Agora abbiamo?; Agora dice che non sono ancora un membro; Come passiamo a un Drive condiviso? |

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
