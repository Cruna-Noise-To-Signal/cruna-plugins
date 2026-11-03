---
name: todo
description: Tiene la lista dei to-do di Agora, personali e condivisi. Usala quando un membro aggiunge un to-do ("aggiungi un to-do: rinnovare l'assicurazione dell'auto", "Nina deve prenotare il dentista", "qualcuno deve comprare il toner"), chiede cosa c'è in una lista ("cosa ho da fare?", "cosa c'è di aperto su Acme?"), chiude, riapre, prende in carico, riassegna o cambia un to-do ("fatto con l'assicurazione", "ci penso io", "passalo a Nina", "spostalo a venerdì"), toglie i vecchi to-do ("rimuovi tutto ciò che è chiuso prima di giugno"), o dice sì a un to-do suggerito.
---

# To-do

Un **to-do** è un'azione che qualcuno deve fare, con un membro o `everyone` come responsabile, visibile a ogni membro. Ogni cambiamento che fai è un nuovo **evento to-do** in `todos/`; `todo.md`, la lista che i membri leggono, si ricostruisce dagli eventi dopo ogni cambiamento. Il membro parla; tu trovi il to-do, scrivi un evento e ricostruisci.

Ogni regola che questa skill applica è nel file `references/todos.md` della skill `rules`, letto al passo 1; i passi qui sotto dicono quando scatta ciascuna.

## Passi

### 1. Carica le regole, trova la radice, sappi chi sta parlando

Carica la skill `rules` e leggi il suo `references/todos.md`. Segui rules → Trovare la radice e rules → Identità, poi leggi `memories/<slug>.md` e `memories/shared.md` (rules → Passo memoria).

Fatto quando: hai letto `org.md`, conosci lo slug del membro e hai letto le sue memorie.

### 2. Scegli i rami

Un messaggio può prendere più rami ("fatto con l'assicurazione, e aggiungi: rinnovare il bollo" è Fatto, poi Aggiungi). Leggi prima la lista (passo 3), poi eseguili nell'ordine in cui sono detti:

- **Aggiungi**: un nuovo to-do, o un sì a uno proposto da `log`, `email` o da un suggerimento della revisione → passo 4.
- **Fatto**, **Riapri**, **Assegna** (compreso "ci penso io"), **Modifica** (testo, scadenza, collocazione) → passo 5.
- **Lista**: "cosa ho da fare?", "cosa c'è di aperto per tutti?", "cosa c'è di aperto su Acme?" → passo 6.
- **Rimuovi**: togliere to-do per data o per campo → passo 7.
- **Calendario**: "mettilo in calendario" → la skill `schedule`, con testo, scadenza e istanza del to-do; una scadenza da sola non crea alcun evento (references/todos.md → Scadenze).

Fatto quando: ogni richiesta del messaggio corrisponde a un ramo.

### 3. Leggi la lista

Elenca `todos/` e ricomponi gli eventi nello stato attuale di ogni to-do (references/todos.md → Stato attuale). Per una sola domanda sulla lista basta `todo.md`, quando non è obsoleto.

Quando la radice non ha la cartella `todos/` (una radice configurata prima della 0.5.0): per Aggiungi, creala nella radice; per tutto il resto, la radice non ha ancora to-do, quindi dillo e proponi di aggiungerne uno.

Fatto quando: conosci testo, responsabile, scadenza, collocazione e stato aperto o chiuso di ogni to-do, oppure `todos/` è stata creata vuota.

### 4. Aggiungi

Per ogni nuovo to-do, fissa i campi dalle parole del membro, senza chiedere ciò che non ha detto:

- **Testo**: una riga breve, un'azione ("Rinnovare l'assicurazione dell'auto").
- **Responsabile**: il membro che parla, a meno che non nomini un altro membro o `everyone` (references/todos.md → Chi vede un to-do e a chi tocca).
- **Scadenza**: una data quando la dà, risolta nel fuso orario della radice; altrimenti nessuna. Chiedi solo quando la data che ha dato non è chiara ("fine mese, questo o il prossimo?").
- **Collocazione**: un'istanza quando ce n'è una chiara, altrimenti a livello di radice; non chiedere mai (references/todos.md → Collocazione).
- **Fonte**: la voce di diario, l'email o la revisione da cui viene il to-do, quando viene da una di queste.

Quando esiste già un to-do aperto con lo stesso significato, dillo e chiedi prima di aggiungerne un secondo. Qualcosa di privato: ricorda una volta al membro che ogni membro vede i to-do.

Fatto quando: ogni nuovo to-do ha testo, responsabile e collocazione fissati, e una scadenza o nessuna. Poi il passo 8.

### 5. Cambia

1. **Trova** il to-do dalle parole del membro tra quelli aperti (quelli chiusi per Riapri; per un to-do che non è in `todos/`, anche `todos/archive/`). Esattamente una corrispondenza: vai avanti. Più di una: chiedi quale, elencando ciascuno con responsabile e scadenza, così la risposta è una parola. Nessuna: dillo, e proponi di aggiungerlo.
2. **Fissa il cambiamento**: Fatto lo chiude; Riapri lo apre (uno in archivio: references/todos.md → Archivio); Assegna imposta il responsabile ("ci penso io": il membro che parla); Modifica imposta ogni campo che il membro cambia.
3. Salta un cambiamento che non cambia nulla (chiudere un to-do chiuso), e di' al membro che era già così.

Fatto quando: ogni cambiamento indica esattamente un to-do, quello che il membro intendeva, e i suoi nuovi valori. Poi il passo 8.

### 6. Lista

Rispondi dallo stato attuale (passo 3):

- **I miei**: i to-do aperti del membro, nell'ordine della lista (references/todos.md → `todo.md`); poi una riga con il numero di quelli di Everyone.
- **Everyone**, **un membro**, **un'istanza o un interesse**: i to-do aperti che corrispondono, nell'ordine della lista.
- **Cosa è stato fatto**: quelli chiusi nel periodo chiesto, o nella finestra di Recently done.

Ogni riga: testo, responsabile, scadenza, istanza. La lista non scrive nulla; vai al passo 9.

Fatto quando: ogni to-do aperto che corrisponde alla domanda è nella risposta, oppure il membro sa che non ce ne sono.

### 7. Rimuovi

Segui references/todos.md → Rimozione, punti 1–4, cercando le corrispondenze nello stato ricomposto di `todos/` e `todos/archive/`. Nessuna corrispondenza: dillo, e vai al passo 9.

Fatto quando: dopo un sì, ogni file evento di ogni to-do elencato è nel cestino di Drive e nient'altro è stato toccato; senza un sì, non è stato spostato nel cestino nulla. Dopo un sì, il passo 8 scrive la traccia; senza, il passo 9.

### 8. Scrivi e ricostruisci

1. **Scrivi** un evento per ogni cambiamento o nuovo to-do, dal modello Evento to-do (rules → Tipi di file → modelli), con il nome secondo references/todos.md → Eventi to-do; una rimozione scrive il suo unico evento `remove`. Gli eventi esistenti restano come sono.
2. **Ricostruisci `todo.md`**: elenca di nuovo `todos/`, ricomponi ogni evento e scrivi `todo.md` per sostituzione (references/todos.md → `todo.md`).
3. **Stati**: per ogni istanza a cui un to-do cambiato era o è ora legato, ricostruisci il suo `status.md`, poi propaga (references/todos.md → Nello stato).

Fatto quando: ogni cambiamento ha esattamente un nuovo evento, `todo.md` corrisponde alla ricomposizione di ogni evento in `todos/` con il Last updated di questa esecuzione, e ogni stato di istanza toccato mostra i suoi to-do aperti.

### 9. Passo memoria

Segui rules → Passo memoria.

Fatto quando: ciò che di duraturo hai imparato è in `memories/`, oppure non hai trovato nulla di nuovo.

## Output

Un messaggio breve, nella lingua della radice:

- Ogni cambiamento, una riga, che ripete il to-do su cui ha agito: "Aggiunto: Rinnovare l'assicurazione dell'auto (tuo, scadenza 7 nov)", "Fatto: Rinnovare l'assicurazione dell'auto", "Chiamare il commercialista ora tocca a Nina".
- Una lista: le righe del passo 6.
- Rimuovi: quanti ne sono stati spostati nel cestino, e che restano nel cestino di Drive per 30 giorni.
- Tutto ciò che resta aperto: una domanda su quale to-do, un evento di calendario proposto.
- La riga di `remember`, se c'è.

Nessun id né percorso di file, a meno che il membro non lo chieda.
