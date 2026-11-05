# Modelli dei file di Agora

Compila ogni `<segnaposto>`. I titoli restano in inglese; il contenuto segue la lingua indicata in `org.md`. Tieni ogni file breve: un membro deve poter leggere qualsiasi `status.md` in meno di un minuto.

## `org.md` (radice, curato)

```markdown
---
agora: root
name: <Nome visualizzato della radice>
language: <it | en | …>
timezone: <Europe/Rome>
assistant_account: <assistente@gmail.com | account del proprietario>
calendar: <email dell'account dell'assistente: il suo calendario principale è il calendario di sistema>
layout: <che cosa è un interesse, es. "un interesse per persona, più `famiglia` per le cose condivise">
email: <on | off>
---

# <Nome visualizzato della radice>

## Members

| Slug | Name | Email | Mail from | Role | Birthday | Personal calendar | Notes |
|---|---|---|---|---|---|---|---|
| simone | Simone | simone@example.com | simone@example.com, simone@acme.it | genitore | 1989-03-28 | libero/occupato | preferisce risposte brevi |
| pm | PM (GitHub) | | | github | | | indirizzo per GitHub tramite cruna-pm-github |

## Interests

| Interest | Color | Description |
|---|---|---|
| nina | Peacock | Tutto ciò che riguarda Nina |

## Conventions

| Where | Convention | Since |
|---|---|---|
| greta/salute | Pannolini: una riga per cambio in `documents/greta-pannolini.csv` (date,time,author,type,notes); nessuna voce di diario per cambio; voce di riepilogo giornaliera | 2026-10-29, simona |
| products/cruna-plugins | managed by cruna-pm-github: note e to-do per pm diventano richieste GitHub | 2026-11-05, pm |

## Access

| Member | Drive | System calendar | Client connected | Notes |
|---|---|---|---|---|
| simone | editor | può modificare | Claude | routine attive |
```

`email` assente significa `off`. **Email** è l'account Google personale con cui sono condivisi la cartella e il calendario. **Mail from** (solo con l'email attiva) elenca ogni indirizzo da cui il membro scrive o inoltra: la posta da questi indirizzi è posta dei membri (rules → Email). I criteri email validi per tutta la radice sono righe di Conventions, scritte come in [email.md](email.md) → Dove stanno i criteri.

Una riga di un **membro github** (rules → Vocabolario) la scrive solo l'handoff `add-interest`, come la riga `pm` qui sopra: Role `github`, Name "PM (GitHub)", Email, Mail from, Birthday e Personal calendar vuoti; non ha riga in Access. Lo stesso handoff scrive la riga gestita di Conventions, Where `products/<instance>`, con lo slug del membro al posto di `pm`. Il marcatore `managed by cruna-pm-github` resta in inglese; il testo dopo i suoi `: ` è nella lingua della radice.

## `setup.md` (radice, curato)

Una sezione `## Member: <slug>` per ogni membro che usa la radice (un bambino solo seguito, o un membro github, non ne ha). Le etichette delle caselle restano in inglese come i titoli. In una radice solitaria, spunta le due caselle di condivisione con "(solo)". Le caselle non spuntate dicono chi le fa quando non sei tu.

```markdown
---
agora: setup
state: <todo | done>
---

# Setup: <Nome della radice>

**Updated:** <YYYY-MM-DD HH:MM> da <slug>

## Root
- [x] Prerequisites
- [ ] Interview
- [ ] Build
- [ ] Email
- [ ] Verify

## Member: simone
- [x] Connectors and plugin
- [ ] In Members and Access, with a memories file
- [ ] Email
- [ ] Root folder shared
- [ ] System calendar shared (simone accetta l'invito)
- [ ] Personal calendar shared (libero/occupato)
- [ ] Routines

## Interview

### The root
- <risposte confermate, una riga ciascuna>
```

La sezione Interview contiene ogni giro confermato sotto il suo titolo (The root, Members, Layout convention, First interests and instances) La casella Email della radice si spunta con "(off)" quando la radice rifiuta l'email, oppure quando l'email è attiva, Gmail è verificato e `org.md` e `memories/shared.md` contengono i campi email. Una sezione Member ha una casella Email solo quando l'email è attiva; si spunta quando l'app del membro ha Gmail collegato con l'account dell'assistente, e i suoi indirizzi e il suo stato dell'invito sull'importanza sono registrati.

La sezione Interview contiene ogni giro confermato sotto il suo titolo (The root, Members, Layout convention, First interests and instances) finché la casella Build non è spuntata; poi viene tolta, tranne le risposte sulle email, che restano finché ogni membro intervistato non ha la sua cella Mail from e la sua riga Email importance scritte (passo 2d di setup). Mentre è in corso una migrazione, una sezione `## Migration` elenca i suoi sette passi come caselle.

## `inbox.md` (radice, curato)

Solo con l'email attiva. Una riga per ogni email gestita, dalla più recente; `last_tidy_up` è la data dell'ultimo riordino della casella fatto dalla `review`, assente fino al primo (rules → Email → Il protocollo).

```markdown
---
agora: inbox
last_tidy_up: 2026-11-03
---

# Inbox

| Message id | Date | From | Subject | Outcome | By |
|---|---|---|---|---|---|
| 18c2f0a9b7e4d311 | 2026-11-03 | Mario Rossi <mario@acme.it> | Preventivo approvato | filed → acme/sito-2026/diary/simone-20261103_0912.md | routine |
| 18c2ef11a0c3b920 | 2026-11-02 | to mario@acme.it | Re: Data del kickoff | sent → acme/sito-2026/diary/simone-20261102_1640.md | simone |
| 18c2ee70d1f2a004 | 2026-11-02 | Newsletter <news@shop.com> | Black Friday | ignored | routine |
| 18c2ed02b4a1c877 | 2026-11-02 | Simone <simone@example.com> | aggiungi ai miei to-do | to-do → simone-20261102_0805 | routine |
```

## `index.md` (derivato)

La radice elenca gli interessi; un interesse elenca le sue istanze.

```markdown
# Index: <root | interest>

| Name | Description | Last activity |
|---|---|---|
| [health](health/) | Visite, esami, vaccini | 2026-10-28 |
```

Il livello di istanza elenca documenti e voci di diario, dal più recente:

```markdown
# Index: <interest>/<instance>

## Documents

| File | Description | From entry |
|---|---|---|
| documents/simone-20261028_1430_blood-test.pdf | Esiti degli esami del sangue, tutti nella norma | diary/simone-20261028_1430.md |

## Maintained

| File | What it is | Updated |
|---|---|---|
| documents/greta-pannolini.csv | Registro dei pannolini, una riga per cambio | 2026-10-29 |

## Diary

| Entry | Author | Summary |
|---|---|---|
| diary/simone-20261028_1430.md | simone | Esami del sangue nella norma |
```

## `status.md` (derivato)

Stesse sezioni a ogni livello. Non omettere nulla; scrivi "—" sotto una sezione vuota.

```markdown
# Status: <root | interest | interest/instance>

**Last updated:** 2026-10-28 14:35 da <slug | routine>

## Needs attention
- <Ciò che richiede presto una decisione o un'azione, con una data quando c'è>

## Summary
<Da due a cinque frasi: a che punto sono le cose adesso>

## Done
- <Elementi completati di recente, dal più recente; elimina quelli che non contano più>

## Pending
- <Elementi aperti in attesa di qualcuno o di qualcosa, con chi>
- To-do: <testo> — <responsabile>, scadenza <YYYY-MM-DD | senza data>

## Next
- <YYYY-MM-DD> <prossimo passo> — <titolo dell'evento in calendario, quando c'è>
```

Le righe `To-do:` sono i to-do aperti legati a un'istanza ([todos.md](todos.md) → Nello stato).

Lo `status.md` della radice aggiunge, dopo Next:

```markdown
## Latest reviews
- Daily: reviews/2026-10-28-daily.md
- Weekly: reviews/2026-W44-weekly.md
- Monthly: reviews/2026-10-monthly.md
```

## Voce di diario (sorgente)

File: `<interest>/<instance>/diary/<slug>-YYYYMMDD_HHMM.md`

```markdown
---
author: <slug>
created: <YYYY-MM-DDTHH:MM>
interest: <interest>
instance: <instance>
documents: [<percorsi documents/…, o vuoto>]
events: [<"[interest/instance] titolo" degli eventi creati o modificati, o vuoto>]
---

## Summary
<Una riga, scritta dall'AI>

## Entry
<Ciò che il membro ha detto o inviato, con le sue parole; solo una leggera ripulitura>
```

Una voce per un'email (archiviata o inviata, rules → Email) aggiunge tre righe di front-matter dopo `events`:

```markdown
email: <id del messaggio Gmail>
email_from: <Mittente originale, o "to <destinatari>" per la posta inviata>
email_subject: <Oggetto>
```

Una voce di un handoff `log` (rules → Handoff) aggiunge una riga di front-matter dopo `events`, i riferimenti della riga:

```markdown
github: ["#8", "PR #9", "v2026.11.12"]
```

Una voce di un'email, quando l'email ha allegati non ancora su Drive, chiude la Entry con un'ultima riga:

```markdown
To file: <nomi degli allegati>. Salvali in landing/ o documents/ e di' "archiviati".
```

## Evento to-do (sorgente)

File: `todos/<slug>-YYYYMMDD_HHMM.md` ([todos.md](todos.md) → Eventi to-do). Un `add`:

```markdown
---
agora: todo
action: add
author: <slug>
created: <YYYY-MM-DDTHH:MM:SS>
owner: <slug | everyone>
due: <YYYY-MM-DD, oppure ometti>
interest: <interesse, oppure ometti>
instance: <istanza, oppure ometti>
source: <percorso dalla radice | email <id del messaggio Gmail>, oppure ometti>
---

<Il to-do, una riga>
```

Ogni altra azione non ha corpo, e nomina il to-do su cui agisce:

```markdown
---
agora: todo
action: <done | reopen | assign | edit>
author: <slug>
created: <YYYY-MM-DDTHH:MM:SS>
todo: <id: il nome del file dell'evento add senza .md>
owner: <solo assign: slug | everyone>
text: <solo edit, quando cambia>
due: <solo edit, quando cambia: YYYY-MM-DD | none>
interest: <solo edit, quando cambia la collocazione: interesse | none>
instance: <solo edit, quando cambia la collocazione: istanza | none>
---
```

Un `remove`:

```markdown
---
agora: todo
action: remove
author: <slug>
created: <YYYY-MM-DDTHH:MM:SS>
filter: <il filtro del membro, a parole, con "compresi quelli aperti" quando detto>
count: <n>
todos: [<id spostati nel cestino>]
---
```

## `todo.md` (radice, derivato)

```markdown
---
agora: todo-list
---

# To-dos

**Last updated:** 2026-11-03 09:14 da <slug | routine>

## Simone
- **Scaduto** Rinnovare l'assicurazione dell'auto — scadenza 2026-11-01 · `simone-20261020_1810`
- Chiamare il commercialista di Acme — scadenza 2026-11-07 · [clienti/acme](clienti/acme/) · fonte: [diario](clienti/acme/diary/simone-20261102_1640.md) · `simone-20261102_1645`
- Prenotare il tagliando dell'auto — senza data · `nina-20261029_0930`

## Everyone
- Comprare il toner della stampante — senza data · `simone-20261101_1200`

## Recently done
- Mandare il preventivo ad Acme — simone, fatto 2026-11-02 da simone · [clienti/acme](clienti/acme/) · fonte: email 18c2f0a9b7e4d311 · `simone-20261030_0915`
```

Sezioni, ordine e contenuto delle righe: [todos.md](todos.md) → `todo.md`.

## Registro (di lavoro)

File: `<interest>/<instance>/documents/<name>.csv`. Riga di intestazione con le colonne concordate alla creazione; `date,time,author` sempre per prime.

```csv
date,time,author,type,notes
2026-10-29,07:40,simona,pipì,
2026-10-29,09:15,simone,cacca,morbida
```

## Lista (di lavoro)

File: `<interest>/<instance>/documents/<name>.md`

```markdown
# <Titolo>

**Updated:** <YYYY-MM-DD HH:MM> da <slug>

- [ ] <elemento>
```

## File di memoria (curato)

File: `memories/<slug>.md` o `memories/shared.md`

```markdown
# Memories: <Nome | shared>

- <YYYY-MM-DD> <un fatto, una preferenza o un'abitudine duratura>
```

Con l'email attiva, ogni file contiene anche una riga `- Email importance: <not asked | invited | no preference | criteri>` (rules → Email → Importanza).

## Revisione (curato)

File: `reviews/YYYY-MM-DD-daily.md`, `reviews/YYYY-Www-weekly.md` o `reviews/YYYY-MM-monthly.md`

```markdown
# <Daily | Weekly | Monthly> review: <periodo>

**Written:** <YYYY-MM-DD HH:MM> da <routine | routine (<slug>) | <slug>, a mano | a mano>

## Looking back
- <Che cosa è successo e che cosa è stato fatto, per interesse>

## Still open
- <Elementi in sospeso riportati avanti>

## Looking ahead
- <Eventi, scadenze e preparativi del periodo in arrivo, per data>

## Needs attention
- <Segnalazioni dal riordino e tutto ciò che richiede una decisione>

## To-dos
- <Scaduti, in scadenza, aperti, fatti, suggerimenti; mensile: aperti da oltre 30 giorni senza data ([todos.md](todos.md) → Revisioni)>

## Email
- <Solo con l'email attiva: email importanti che richiedono attenzione, bozze in attesa di approvazione, inviate, archiviate; da `inbox.md`>
```

La sezione To-dos, compilata (ordine: [todos.md](todos.md) → Revisioni):

```markdown
## To-dos
- **Scaduti**: Rinnovare l'assicurazione dell'auto (simone, scadenza 1 nov)
- **In scadenza**: Chiamare il commercialista di Acme (simone, 7 nov)
- **Aperti**: Comprare il toner della stampante (everyone)
- **Fatti**: Mandare il preventivo ad Acme (simone)
- **Suggerimenti**: sembra fatto: Prenotare il tagliando dell'auto (nina/auto/diary/nina-20261102_1100.md); possibile nuovo to-do: Rispondere alla scuola sulla gita (email della scuola)
```

Le esecuzioni successive aggiungono, solo quando sono arrivate nuove voci di diario da quando è stata scritta:

```markdown
## Update <HH:MM>
- <Che cosa è cambiato>
```
