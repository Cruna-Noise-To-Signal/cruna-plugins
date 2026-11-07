---
name: help
description: Spiega Agora, il quaderno condiviso che una famiglia o un piccolo gruppo tiene su Google Drive e Calendar tramite l'AI, e lo sistema quando non funziona bene. Usala quando qualcuno chiede cosa sa fare Agora o come funziona ("cosa sai fare?", "come funziona Agora?"), fa una domanda su come fare qualcosa (dove sono i miei file, chi vede cosa, le due modalità e il passaggio a un Drive condiviso, aggiungere un documento dal telefono, tenere traccia di cose frequenti o di liste da stampare, i to-do, i prodotti e pm da GitHub, l'email, le routine, aggiungere una persona, uscire), o dice che Agora non funziona, non vede qualcosa o mostra un errore ("Agora non funziona", "non vedi il calendario").
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
| "Aggiungi un to-do: rinnovare l'assicurazione dell'auto" / "cosa ho da fare?" / "fatto con l'assicurazione" | Una lista di to-do condivisa, tuoi e di tutti, tenuta aggiornata | `todo` |
| "Dammi la settimana che viene" | Un riepilogo di cosa è successo e di cosa arriva | `review` |
| "Aggiungi un nuovo cliente: Rossi" | Una nuova sezione in cui tenere le cose | `add-interest` |
| "Ricorda che preferisco risposte brevi" | Agora se ne ricorda la prossima volta | `remember` |
| "Archivia l'email che ti ho inoltrato" / "prepara una bozza di risposta a Mario" | L'email viene salvata nell'argomento giusto; una risposta viene scritta e inviata solo dopo il tuo sì | `email` |
| "Configura Agora" / "aggiungi mio marito ad Agora" | La configurazione, o l'ingresso di una nuova persona | `setup` |

Aggiungi una riga su come i membri lo raggiungono, secondo la modalità della radice (rules → Vocabolario → Modalità; senza radice: entrambe, una riga ciascuna): in modalità base, l'AI di ognuno accede all'account Google della radice; in modalità servizi condivisi, ogni membro usa il proprio account di lavoro, e il gruppo condivide un Drive condiviso, un calendario e un indirizzo del gruppo. Aggiungi una riga sulle routine: una volta configurate, una revisione giornaliera, una settimanale e una mensile arrivano da sole. Mostra l'esempio dell'email solo quando `org.md` dice `email: on`; altrimenti, in una radice aziendale, una riga che dice che l'email si può attivare. Quando Members ha un membro github (rules → Vocabolario), aggiungi una riga sui **prodotti da GitHub**: `products` segue il software costruito su GitHub, e una nota su un prodotto o un to-do per `pm` (il suo slug) diventa una richiesta GitHub alla prossima sincronizzazione (faq.md → Che cos'è `products` e chi è pm?).

Fatto quando: il membro ha il riassunto in una frase e gli esempi, in meno di 12 righe, senza nomi di skill.

### 2b. FAQ

Rispondi da [references/faq.md](references/faq.md), inserendo i nomi reali da `org.md` quando esiste una radice (cartella radice, membri, account dell'assistente o indirizzo del gruppo, calendario). Quando la domanda non c'è, rispondi dalla skill `rules` se la risolve; altrimenti di' che non sei sicuro e proponi di verificare con la risoluzione dei problemi.

Fatto quando: la domanda ha una risposta in poche righe, oppure hai detto chiaramente che non lo sai.

### 2c. Risoluzione dei problemi

Le soluzioni stanno nella checklist dei prerequisiti della skill `rules` (rules → Quando qualcosa non funziona), ogni voce nella variante per la modalità della radice. Prima di abbinare, leggi Needs attention dello `status.md` della radice: una segnalazione che il riordino della `review` ha sollevato sulle routine in ritardo o sull'account connesso (i suoi punti 8 e 9) spesso risponde al sintomo; mostrala con la sua soluzione. Poi abbina il sintomo alle sue voci e percorrile in ordine:

| Sintomo | Voci dei prerequisiti |
|---|---|
| Non trovi Agora, i miei file o una cartella | 3 (Drive), poi 4, poi 5 |
| Gli eventi non compaiono sul mio telefono, o non riesci ad aggiungere eventi | 3 (Calendar), poi 5 |
| Errori "autorizzazione negata", "accesso", "non consentito" | 3, poi 5 |
| File doppi che non se ne vanno, o un cestino rifiutato | modalità base: 3 (l'account connesso non è proprietario di `org.md`); modalità servizi condivisi: 3 (Drive), poi 5 (non è Gestore contenuti (Content manager) del Drive condiviso, voce 4 passo 2) |
| Agora dice che non sono ancora un membro (modalità servizi condivisi) | 3 (Chi: connesso con un altro account), altrimenti proponi il ramo **Unisciti** della skill `setup` |
| Agora non riesce ad aggiungere eventi al calendario condiviso (modalità servizi condivisi) | 3 (Calendar), poi voce 4 passo 4 (il proprietario del calendario lo condivide con Apporta modifiche agli eventi (Make changes to events)) |
| La posta del gruppo non compare (modalità servizi condivisi, email attiva) | 7 (la ricerca del gruppo non restituisce nulla), poi voce 4 passi 3.1–3.3 |
| La mia revisione giornaliera / settimanale / mensile non è mai arrivata, o "nessuna routine dei membri l'ha eseguito" | 6, poi 1 |
| Agora non c'è nella mia app | 2 |
| Non vedi la casella, un'email che ti ho inviato, o non riesci a inviare (email attiva) | 7 |
| Un nuovo membro non riesce a usarlo | quel membro percorre le voci 1–3, 5 e 6 (e 7 quando l'email è attiva; in modalità servizi condivisi, dopo i passi della voce 4 che lo riguardano): proponi il ramo **Unisciti** della skill `setup` |
| La configurazione è rimasta a metà, o `setup.md` dice `state: todo` | nessuna: proponi la skill `setup`, che riprende dalla prima casella non spuntata |

1. Esegui il **controllo** di ogni voce corrispondente con una chiamata reale, esattamente come lo descrive la checklist.
2. Ogni voce finisce **pronta** o **bloccata**. Per una voce bloccata, chiedi quale app e quale dispositivo usa il membro se non lo sai, poi dai la soluzione per quella piattaforma, passo per passo.
3. Quando il membro dice che è sistemato, esegui di nuovo il controllo.

Sintomi che non sono un problema di configurazione:

- **Un file che ho caricato non è stato archiviato**: guarda in `landing/` e in Needs attention dello `status.md` della radice; proponi di archiviarlo ora con la skill `file-document`.
- **File doppi, o uno stato che sembra non aggiornato**: è innocuo; la prossima revisione li mette in ordine. Proponi di eseguire ora la skill `review`.
- **Qualcosa di salvato è sbagliato**: proponi di registrare la correzione con la skill `log`.
- **Un'email che ho inoltrato non è stata archiviata, o qualcosa è stato archiviato male** (email attiva): guarda in `inbox.md` e in Needs attention dello `status.md` della radice cosa le è successo; proponi la skill `email` per archiviarla, o la skill `log` per una correzione.

Fatto quando: ogni voce corrispondente è pronta, oppure è bloccata con la sua soluzione data e il membro sa qual è la prossima cosa da fare.

### 3. Passo memoria

Quando esiste una radice, segui rules → Passo memoria.

Fatto quando: tutto ciò che di duraturo hai imparato è in `memories/`, non hai trovato nulla di nuovo, oppure non esiste una radice.

## Output

Un messaggio breve e semplice: il tour, la risposta alla FAQ o l'esito della risoluzione dei problemi (cosa hai controllato, cosa funziona, cosa fare dopo, passo per passo). Senza radice, chiudi con: "Agora non è ancora configurato. Vuoi configurarlo ora?" Altrimenti chiudi con la riga di `remember`, se c'è.
