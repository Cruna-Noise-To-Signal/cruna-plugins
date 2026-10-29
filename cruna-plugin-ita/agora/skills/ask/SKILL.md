---
name: ask
description: Risponde alle domande con ciò che Agora ha in archivio: le note su Drive condiviso, i documenti e il calendario di una famiglia o di una piccola impresa. Usala quando un membro chiede di una persona, un cliente, un lavoro o un argomento che Agora segue ("quando è il prossimo vaccino di Nina?", "cosa c'è in sospeso con il cliente Rossi?", "quante cacche oggi?"), o di cosa c'è in arrivo ("cosa abbiamo questa settimana?").
---

# Chiedi

Rispondi partendo dalla radice e leggendo il meno possibile. Questa è la **scoperta progressiva**: l'`index.md` della radice ti dice quali interessi contano, l'`index.md` di un interesse quali istanze, lo `status.md` di un'istanza se hai già la risposta, e solo allora apri le voci di diario, i documenti o gli eventi che la contengono. Ogni livello decide se vale la pena aprire il successivo.

Una buona risposta è **pertinente** e **aggiornata**: viene dalle fonti più recenti, dice quanto sono vecchie e indica dove si trovano.

## Passi

### 1. Carica le regole, trova la radice, sappi chi sta parlando

Carica la skill `rules`. Segui rules → Trovare la radice e rules → Identità, poi leggi `memories/<slug>.md` e `memories/shared.md` (rules → Passo memoria). Nessuna radice: di' al membro che Agora non è ancora configurata e offri la skill `setup`.

Fatto quando: hai letto `org.md`, conosci lo slug del membro e hai letto le sue memorie.

### 2. Individua

Leggi l'`index.md` della radice e fai corrispondere la domanda agli interessi: un nome, un cliente, un argomento, letti attraverso la convenzione di struttura in `org.md` ("Nina" è l'interesse `nina` in una struttura per persona). Per ogni interesse corrispondente, leggi il suo `index.md` e scegli le istanze la cui descrizione è pertinente. Una domanda su qualcosa che una Convenzione indirizza a un registro ("quante cacche oggi?") va dritta a quell'istanza e a quel registro (rules → Convenzioni).

- **Domande sul tempo senza un soggetto** ("cosa abbiamo questa settimana?", "c'è qualcosa in scadenza domani?"): è in gioco tutta la radice; vai dritto allo `status.md` della radice e al calendario (passo 4). Per un riepilogo completo di un giorno, una settimana o un mese ("dimmi la settimana che arriva"), usa invece la skill `review`.
- **Domande che attraversano più interessi** ("cosa c'è in sospeso per tutti?"): sono in gioco tutti gli interessi; prevedi di aggregare (passo 5).
- **Obiettivo poco chiaro**, quando obiettivi diversi darebbero risposte diverse: chiedi una volta sola, offrendo le opzioni `interesse/istanza` probabili prese dagli indici (rules → Chiedi, non dare per scontato).

Fatto quando: hai una lista di istanze candidate (o "tutta la radice"), ciascuna supportata da una riga di un indice o dalla risposta del membro.

### 3. Leggi gli stati

Leggi lo `status.md` di ogni istanza candidata. Con più di tre candidate, leggi prima lo `status.md` dei loro interessi (che aggregano le istanze) e scendi solo nelle istanze che sembrano pertinenti.

Uno stato risponde alla domanda quando la copre **ed** è attuale: il suo Last updated non è più vecchio della voce di diario o del documento più recente nell'`index.md` della sua istanza. Uno stato obsoleto è un rimando, non una risposta; apri le fonti. Le righe dei registri arrivano allo stato solo tramite il riepilogo giornaliero, quindi le domande di conteggio e del tipo "quand'è stata l'ultima volta" vanno sempre al registro.

Fatto quando: hai letto ogni stato candidato e sai, per ciascuno, se risponde alla domanda o quali fonti aprire.

### 4. Apri le fonti

Apri solo ciò che serve alla domanda, dal più recente:

- **Voci di diario**: sceglile dalla tabella Diary dell'`index.md` dell'istanza in base a data e riepilogo.
- **Documenti**: sceglili dalla tabella Documents; leggi PDF, immagini e file Office tramite la vista di estrazione del testo (rules → Lavorare con Drive).
- **File di lavoro**: sceglili dalla tabella Maintained e scaricali (rules → Tipi di file). Un **registro** risponde alle domande di conteggio e di tempo: filtra le sue righe per data, ora e le altre colonne, poi conta, somma o prendi la più recente ("4 cacche oggi, l'ultima alle 15:20"; "ultima poppata alle 06:10"). Una **lista** si legge così com'è.
- **Calendario**: elenca gli eventi del calendario di sistema nella finestra che la domanda sottintende (predefinita: da oggi, i prossimi 30 giorni), oppure cerca per prefisso di titolo `[interest/instance]`. Per le domande "cosa abbiamo", aggiungi il calendario personale del membro quando `org.md` lo indica come condiviso con tutti i dettagli.

Applica rules → Regola di aggiornamento dello stato mentre leggi: una voce più recente che chiude, annulla o sposta qualcosa prevale su quella più vecchia.

Fatto quando: ogni fatto che dirai risale a un file o a un evento che hai letto, e hai controllato le fonti più recenti di quell'istanza per tutto ciò che lo supera.

### 5. Rispondi

- **Apri** con la risposta diretta in una o due frasi.
- **Data** ogni fatto in base alla sua fonte ("secondo la voce di diario del 12 ott", "secondo la fattura archiviata il 3 set").
- **Segnala le notizie vecchie**: quando la fonte più recente sull'argomento ha più di un mese, o descrive qualcosa che nel frattempo avrebbe dovuto succedere (un appuntamento la settimana scorsa senza nulla dopo), dillo chiaramente ("L'ultima informazione che ho è di marzo; potrebbe essere cambiata.").
- **Cita** ogni fonte con il suo percorso dalla radice (`nina/salute/diary/simone-20261028_1430.md`) e ogni evento con titolo e data, così che il membro possa aprirlo in Drive o nel calendario.
- **Aggrega** le risposte che attraversano più interessi: raggruppa per interesse, una riga ciascuno, poi i dettagli solo dove richiesti.
- **Non trovato**: dillo, elenca dove hai cercato (quali indici, istanze e finestra di calendario), e offri di salvare la risposta se il membro la conosce (la skill `log`).

Fatto quando: ogni fatto della risposta porta una data e una fonte, oppure la risposta dice chiaramente che non è stato trovato nulla e dove hai cercato.

### 6. Passa ad altre skill ciò che vale la pena salvare

Scrivere spetta ad altre skill. Quando la conversazione produce qualcosa da conservare, passalo con l'interesse e l'istanza che hai già trovato, così al membro non viene chiesto di nuovo:

- "segnatelo", "salvalo", fatti nuovi o una decisione → la skill `log`.
- "ricordami", un appuntamento, una scadenza → la skill `schedule`.
- un file da conservare → la skill `file-document`.
- un promemoria, una lista o qualcosa da stampare fatto da ciò che hai trovato ("fammene una lista da stampare") → la skill `log`, per salvarlo come file di lavoro di tipo Lista (rules → Tipi di file), quando il membro vuole conservarlo e tenerlo aggiornato; quando vuole solo leggerlo, scrivilo nella tua risposta.

Fatto quando: ogni richiesta di salvataggio della conversazione è stata passata alla sua skill, oppure non ce n'erano.

### 7. Passo memoria

Segui rules → Passo memoria.

Fatto quando: ciò che di duraturo hai imparato è in `memories/`, oppure non hai trovato nulla di nuovo.

## Output

Un solo messaggio breve, nella lingua della radice e con parole semplici:

1. La risposta, datata.
2. Fonti: una riga ciascuna, percorso dalla radice (o titolo e data dell'evento).
3. Quando serve: una nota che l'informazione è vecchia, oppure dove hai cercato quando non hai trovato nulla, e un'offerta di salvare o mettere in calendario.
4. La riga di `remember`, se c'è.

Niente front-matter, testo grezzo degli stati o ID di Drive.
