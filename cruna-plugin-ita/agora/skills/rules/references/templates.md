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
---

# <Nome visualizzato della radice>

## Members

| Slug | Name | Email | Role | Birthday | Personal calendar | Notes |
|---|---|---|---|---|---|---|
| simone | Simone | simone@example.com | genitore | 1989-03-28 | libero/occupato | preferisce risposte brevi |

## Interests

| Interest | Color | Description |
|---|---|---|
| nina | Peacock | Tutto ciò che riguarda Nina |

## Conventions

| Where | Convention | Since |
|---|---|---|
| greta/salute | Pannolini: una riga per cambio in `documents/greta-pannolini.csv` (date,time,author,type,notes); nessuna voce di diario per cambio; voce di riepilogo giornaliera | 2026-10-29, simona |

## Access

| Member | Drive | System calendar | Client connected | Notes |
|---|---|---|---|---|
| simone | editor | può modificare | Claude | routine attive |
```

## `setup.md` (radice, curato)

Una sezione `## Member: <slug>` per ogni membro che usa la radice (un bambino solo seguito non ne ha). Le etichette delle caselle restano in inglese come i titoli. In una radice solitaria, spunta le due caselle di condivisione con "(solo)". Le caselle non spuntate dicono chi le fa quando non sei tu.

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
- [ ] Verify

## Member: simone
- [x] Connectors and plugin
- [ ] In Members and Access, with a memories file
- [ ] Root folder shared
- [ ] System calendar shared (simone accetta l'invito)
- [ ] Personal calendar shared (libero/occupato)
- [ ] Routines

## Interview

### The root
- <risposte confermate, una riga ciascuna>
```

La sezione Interview contiene ogni giro confermato sotto il suo titolo (The root, Members, Layout convention, First interests and instances) finché la casella Build non è spuntata; poi viene tolta. Mentre è in corso una migrazione, una sezione `## Migration` elenca i suoi sette passi come caselle.

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

## Next
- <YYYY-MM-DD> <prossimo passo> — <titolo dell'evento in calendario, quando c'è>
```

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
```

Le esecuzioni successive aggiungono, solo quando sono arrivate nuove voci di diario da quando è stata scritta:

```markdown
## Update <HH:MM>
- <Che cosa è cambiato>
```
