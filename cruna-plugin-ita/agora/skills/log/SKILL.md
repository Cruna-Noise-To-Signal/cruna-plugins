---
name: log
description: Registra le novità nel diario di Agora e nei file di lavoro, e tiene aggiornato ogni stato. Usala quando un membro racconta ad Agora qualcosa che è successo o cambiato su una persona, un cliente o un argomento che segue ("la lezione di nuoto di Nina è spostata a giovedì", "il cliente X ha approvato il preventivo", "Greta ha fatto la pipì"), incolla un'email o un messaggio da conservare, corregge qualcosa registrato prima, o dice "salvalo" o "segnatelo" dopo aver fatto una domanda ad Agora. Anche quando vuole tenere traccia in un file di qualcosa di frequente ("tieni il conto dei pannolini di Greta"), chiede una lista, un promemoria o qualcosa da stampare ("aggiungi il latte alla lista della spesa", "fammi la lista per la valigia"), o cambia il modo in cui Agora gestisce i suoi dati ("d'ora in poi…").
---

# Log

Un **log** trasforma ciò che dice un membro in una voce di diario per ogni istanza che riguarda, mette in calendario tutto ciò che ha una data e aggiorna ogni stato sopra di essa. Il membro parla; tu collochi, scrivi e propaghi. Log gestisce anche i **file di lavoro** (registri e liste) e le **Convenzioni** che vi indirizzano i fatti (rules → Tipi di file, rules → Convenzioni); le loro procedure sono in [references/working-files.md](references/working-files.md).

## Passi

### 1. Carica le regole, trova la radice, sappi chi sta parlando

Carica la skill `rules`. Segui rules → Trovare la radice e rules → Identità, poi leggi `memories/<slug>.md` e `memories/shared.md` (rules → Passo memoria).

Fatto quando: hai letto `org.md`, conosci lo slug del membro e hai letto le sue memorie.

### 2. Raccogli cosa registrare

Prendi il messaggio del membro come materiale. I casi:

- **Novità**: una frase, un'email o un messaggio incollato, una nota inoltrata.
- **Correzione**: "anzi, è venerdì, non giovedì". Trattala come una novità che dice cosa corregge; la voce precedente resta intatta (rules → Tipi di file).
- **Salvataggio da una sessione di consultazione**: dopo `ask`, il membro dice "salvalo". Il materiale è la risposta o la decisione che vuole conservare, più le sue parole.
- **Un file allegato in chat** → la skill `file-document`.
- **File di lavoro**: la richiesta di tenere traccia di qualcosa in un file, di creare o modificare una lista, un promemoria o qualcosa da stampare, o di aggiungere, cambiare o togliere una Convenzione ("d'ora in poi…", "smetti di segnare i pannolini nel file") → [references/working-files.md](references/working-files.md), poi il passo 9.

Fatto quando: sai enunciare, in una riga ciascuno, ogni fatto che il membro vuole conservare.

### 3. Colloca ogni fatto

**Colloca** ogni fatto in un'istanza: leggi l'`index.md` della radice, poi l'`index.md` dell'interesse probabile, e trova la corrispondenza. Un messaggio può toccare più istanze ("Nina ha la febbre, quindi saltiamo la lezione di nuoto" → `nina/salute` e `nina/nuoto`); ogni istanza riceve la sua voce.

Quando un fatto non rientra con certezza in un'unica istanza, chiedi una volta sola per tutto il messaggio, offrendo le opzioni `interesse/istanza` probabili così che la risposta sia una parola (rules → Chiedi, non dare per scontato). Quando l'istanza giusta non esiste ancora, offri la skill `add-interest`, poi torna qui e completa il log.

Quando `ask` ti ha passato l'istanza, usala, a meno che un fatto non appartenga chiaramente altrove.

Poi controlla `org.md` → Conventions per ogni istanza collocata (rules → Convenzioni). Un fatto che una Convenzione indirizza a un registro ("Greta ha fatto la cacca alle 9:15", con i pannolini tracciati in `greta/salute`) diventa una riga: segui [references/working-files.md](references/working-files.md) → Aggiungi una riga, e toglilo dai passi 4–7. Gli altri fatti dello stesso messaggio proseguono come al solito.

Fatto quando: ogni fatto sta in esattamente un'istanza esistente, confermata dagli indici o dal membro, e ogni fatto indirizzato da una Convenzione è una riga di registro.

### 4. Dai un nome a ogni voce

Per ogni istanza, dai il nome alla voce secondo rules → Struttura. Fissa ora i percorsi: il calendario ne ha bisogno al passo 5.

Fatto quando: ogni voce ha un percorso libero nella sua cartella `diary/`.

### 5. Individua ciò che va in calendario

Scorri i fatti cercando tutto ciò che va ricordato a una data (rules → Convenzioni del calendario): un appuntamento nuovo o spostato, una scadenza, un pagamento dovuto, qualcosa da preparare prima, un incontro annullato il cui evento va tolto.

Quando ne trovi, chiedi una volta sola, elencando ciascuno con data e ora ("Sposto *[nina/nuoto] Lezione* a giovedì 6 nov, 17:00?"). Per ogni sì, esegui la skill `schedule` con l'istanza, il percorso della voce dal passo 4 e i dettagli; ti restituisce i titoli degli eventi creati, modificati o eliminati. Un sì a una domanda che contiene la data e l'ora esatte è la conferma di cui `schedule` ha bisogno.

Fatto quando: ogni fatto con una data è in calendario, oppure rifiutato dal membro, oppure ancora senza data (allora resta nella voce come in sospeso).

### 6. Scrivi le voci

Crea ogni voce dal modello Voce di diario (rules → Tipi di file → modelli):

- `author`: lo slug del membro; `created`: il timestamp del passo 4.
- `documents`: vuoto, a meno che la voce non colleghi un file già presente in `documents/`.
- `events`: i titoli restituiti da `schedule` per questa istanza.
- **Summary**: una riga semplice scritta da te, con il fatto e la sua data ("Lezione di nuoto spostata da martedì a giovedì 6 nov").
- **Entry**: le parole del membro, o il testo incollato, ridotti alla parte che riguarda questa istanza. Una correzione dice cosa corregge ("Correzione a diary/simone-20261028_1430.md: …").

Fatto quando: ogni voce esiste al suo percorso, con tutti i campi del modello compilati.

### 7. Aggiorna indici e stati

Per ogni istanza toccata, ricostruisci il suo `index.md` con la nuova voce in cima (rules → Sostituire). Poi **propaga** (rules → Regola di aggiornamento dello stato): lo `status.md` di ogni istanza, poi quello di ogni interesse toccato una volta, poi quello della radice una volta.

Fatto quando: l'indice di ogni istanza toccata elenca la sua nuova voce, e ogni stato, da quelle istanze fino alla radice, riporta i nuovi fatti e il Last updated di oggi.

### 8. Riconosci uno schema

Guarda le righe di oggi nella tabella Diary di ogni istanza toccata, e le parole del membro. Quando si applica rules → Convenzioni → Suggerire (diverse voci simili oggi, una lista o qualcosa da stampare richiesti, un foglio che il membro tiene a parte), segui [references/working-files.md](references/working-files.md) → Suggerire. Un membro con un foglio esistente da portare dentro → la skill `file-document` (Importa).

Fatto quando: il suggerimento è stato proposto una volta, oppure non è emerso alcuno schema.

### 9. Passo memoria

Segui rules → Passo memoria.

Fatto quando: ciò che di duraturo hai imparato è in `memories/`, oppure non hai trovato nulla di nuovo.

## Output

Un solo messaggio breve, nella lingua della radice:

- Cosa è stato registrato, una riga per voce: `interesse/istanza` e la sua riga di Summary.
- Righe di registro, una riga per registro ("Greta: cacca delle 9:15 aggiunta al registro dei pannolini"); un file di lavoro creato o modificato, con il suo percorso così che il membro possa aprirlo o stamparlo; una Convenzione scritta, in una frase semplice.
- Modifiche al calendario, una riga ciascuna, se ce ne sono.
- Ciò che resta aperto: un fatto rimasto senza data, un'istanza in attesa di `add-interest`.
- Il suggerimento del passo 8, se c'è.
- La riga di `remember`, se c'è.

Nessun percorso di file tranne quello di un file di lavoro, niente front-matter o testo degli stati, a meno che il membro non lo chieda.
