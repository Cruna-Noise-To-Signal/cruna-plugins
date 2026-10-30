---
name: help
description: Spiega Agora, il quaderno condiviso che una famiglia o un piccolo gruppo tiene su Google Drive e Calendar tramite l'AI, e lo sistema quando non funziona bene. Usala quando qualcuno chiede cosa sa fare Agora o come funziona ("cosa sai fare?", "come funziona Agora?"), fa una domanda su come fare qualcosa (dove sono i miei file, chi vede cosa, aggiungere un documento dal telefono, tenere traccia di cose frequenti o di liste da stampare, le routine, aggiungere una persona, uscire), o dice che Agora non funziona, non vede qualcosa o mostra un errore ("Agora non funziona", "non vedi il calendario").
---

# Aiuto

Tre rami, scelti in base alle parole del membro; un messaggio può toccarne due, e allora rispondi a entrambi:

- **Tour**: "cosa sai fare?", "come funziona?"
- **FAQ**: una domanda su come fare qualcosa o sul perché di qualcosa in Agora.
- **Risoluzione dei problemi**: "non funziona", "non vedi X", un messaggio di errore.

I membri non sono tecnici. Rispondi con frasi brevi e semplici, nella loro lingua, con le parole che vedono sullo schermo.

## Passi

### 1. Carica le regole e trova la radice

Carica la skill `rules` e segui rules → Trovare la radice. L'aiuto funziona anche senza una radice: quando non ce n'è, prosegui con il ramo e chiudi proponendo la skill `setup`. Quando una radice esiste, leggi `org.md`, segui rules → Identità e leggi le memorie del membro.

Fatto quando: sai se esiste una radice; se esiste, `org.md` è letto e sai chi sta parlando.

### 2a. Tour

Di' in una frase che cos'è Agora: ognuno racconta all'AI cosa succede, l'AI lo tiene in ordine nella cartella condivisa su Drive e nel calendario, e rimanda promemoria e revisioni. Poi mostra da cinque a sette frasi di esempio dalla tabella qui sotto, ognuna con cosa succede, in parole semplici. Quando esiste una radice, usa negli esempi i nomi reali dei suoi interessi.

| Frase di esempio | Cosa succede | Skill |
|---|---|---|
| "Oggi Nina ha fatto le analisi del sangue, tutto normale" | Salvato negli appunti sulla salute di Nina; il suo stato aggiornato | `log` |
| "Ho messo la fattura nella cartella landing" | Il file viene spostato al posto giusto e annotato | `file-document` |
| "Dentista per Nina il 3 novembre alle 10" / "ricordami di chiamare l'idraulico venerdì" | Un evento sul calendario condiviso | `schedule` |
| "Quando è il prossimo vaccino di Nina?" / "cosa abbiamo questa settimana?" | Una risposta con le date e da dove vengono | `ask` |
| "Greta ha appena fatto la cacca" / "fammi una lista per la valigia da stampare" | Una riga in un file registro, o una lista sul Drive, tenuta aggiornata | `log` |
| "Quante poppate oggi?" | Contate dal registro | `ask` |
| "Dammi la settimana che viene" | Un riepilogo di cosa è successo e di cosa arriva | `review` |
| "Aggiungi un nuovo cliente: Rossi" | Una nuova sezione in cui tenere le cose | `add-interest` |
| "Ricorda che preferisco risposte brevi" | Agora se ne ricorda la prossima volta | `remember` |
| "Configura Agora" / "aggiungi mio marito ad Agora" | La configurazione, o l'ingresso di una nuova persona | `setup` |

Aggiungi una riga sulle routine: una volta configurate, una revisione giornaliera, una settimanale e una mensile arrivano da sole.

Fatto quando: il membro ha il riassunto in una frase e gli esempi, in meno di 12 righe, senza nomi di skill.

### 2b. FAQ

Rispondi da [references/faq.md](references/faq.md), inserendo i nomi reali da `org.md` quando esiste una radice (cartella radice, membri, account dell'assistente, calendario). Quando la domanda non c'è, rispondi dalla skill `rules` se la risolve; altrimenti di' che non sei sicuro e proponi di verificare con la risoluzione dei problemi.

Fatto quando: la domanda ha una risposta in poche righe, oppure hai detto chiaramente che non lo sai.

### 2c. Risoluzione dei problemi

Le soluzioni stanno nella checklist dei prerequisiti della skill `rules` (rules → Quando qualcosa non funziona). Abbina il sintomo alle sue voci, poi percorrile in ordine:

| Sintomo | Voci dei prerequisiti |
|---|---|
| Non trovi Agora, i miei file o una cartella | 3 (Drive), poi 4, poi 5 |
| Gli eventi non compaiono sul mio telefono, o non riesci ad aggiungere eventi | 3 (Calendar), poi 5 |
| Errori "autorizzazione negata", "accesso", "non consentito" | 3, poi 5 |
| La mia revisione giornaliera / settimanale / mensile non è mai arrivata | 6, poi 1 |
| Agora non c'è nella mia app | 2 |
| Un nuovo membro non riesce a usarlo | quel membro percorre le voci 1–3, 5 e 6: proponi il ramo **Unisciti** della skill `setup` |
| La configurazione è rimasta a metà, o `setup.md` dice `state: todo` | nessuna: proponi la skill `setup`, che riprende dalla prima casella non spuntata |

1. Esegui il **controllo** di ogni voce corrispondente con una chiamata reale, esattamente come lo descrive la checklist.
2. Ogni voce finisce **pronta** o **bloccata**. Per una voce bloccata, chiedi quale app e quale dispositivo usa il membro se non lo sai, poi dai la soluzione per quella piattaforma, passo per passo.
3. Quando il membro dice che è sistemato, esegui di nuovo il controllo.

Sintomi che non sono un problema di configurazione:

- **Un file che ho caricato non è stato archiviato**: guarda in `landing/` e in Needs attention dello `status.md` della radice; proponi di archiviarlo ora con la skill `file-document`.
- **File doppi, o uno stato che sembra non aggiornato**: è innocuo; la prossima revisione li mette in ordine. Proponi di eseguire ora la skill `review`.
- **Qualcosa di salvato è sbagliato**: proponi di registrare la correzione con la skill `log`.

Fatto quando: ogni voce corrispondente è pronta, oppure è bloccata con la sua soluzione data e il membro sa qual è la prossima cosa da fare.

### 3. Passo memoria

Quando esiste una radice, segui rules → Passo memoria.

Fatto quando: tutto ciò che di duraturo hai imparato è in `memories/`, non hai trovato nulla di nuovo, oppure non esiste una radice.

## Output

Un messaggio breve e semplice: il tour, la risposta alla FAQ o l'esito della risoluzione dei problemi (cosa hai controllato, cosa funziona, cosa fare dopo, passo per passo). Senza radice, chiudi con: "Agora non è ancora configurato. Vuoi configurarlo ora?" Altrimenti chiudi con la riga di `remember`, se c'è.
