# File di lavoro e Convenzioni

Un **file di lavoro** è un registro (CSV, una riga per evento) o una lista (Markdown) che tieni aggiornato nella cartella `documents/` di un'istanza; una **Convenzione** è una riga in `org.md` → Conventions che cambia un comportamento predefinito per un'istanza. Cosa sono e come possono cambiare: rules → Tipi di file e rules → Convenzioni. La loro forma: rules → Tipi di file → modelli (Registro, Lista, e le tabelle Conventions e Maintained).

Arrivi qui dalla skill `log` con la radice, lo slug del membro e le sue memorie già letti. Scegli il caso:

- **Aggiungi una riga**: un fatto che una Convenzione indirizza a un registro ("Greta ha fatto la cacca alle 9:15").
- **Crea**: il membro vuole tenere traccia di qualcosa in un file o una nuova lista, promemoria o cosa da stampare ("segna i pannolini di Greta in un file", "fammi la lista per la valigia delle vacanze"), dice sì a un suggerimento, oppure `file-document` ti passa un foglio importato.
- **Aggiorna una lista**: "aggiungi il latte alla lista della spesa", "spunta i passaporti", "rifai il promemoria".
- **Cambia una Convenzione**: "d'ora in poi…", "smetti di segnare i pannolini nel file", "rimettili nel diario", "aggiungi una colonna per la quantità".

Ogni scrittura qui sotto sostituisce l'intero file (rules → Sostituire). Scrivi il CSV con content type `text/csv` e il Markdown con `text/markdown`, entrambi con la conversione in formato Google disattivata.

## Aggiungi una riga

1. **Leggi** la riga della Convenzione: indica il registro e le sue colonne. Scarica il registro.
2. **Costruisci una riga per evento**: `date` e `time` dell'evento come li dà il membro ("alle 9:15", "stamattina verso le 7"), altrimenti adesso, nel fuso orario della radice; `author` è lo slug del membro; le altre colonne dalle parole del membro, vuote quando non dice nulla. Quando non è chiara una colonna senza la quale la riga non ha senso (il `type` di un pannolino), chiedi una volta sola per tutto il messaggio. Metti tra virgolette ogni valore che contiene una virgola.
3. **Inserisci** le righe in ordine di data e ora (la più vecchia per prima), così un evento raccontato in ritardo finisce al suo posto.
4. **Sostituisci** il registro. Prima di cestinare la versione vecchia, elenca di nuovo la cartella: quando c'è un'altra versione più recente di quella che hai letto (nel frattempo ha scritto un altro membro), unisci le sue righe alle tue, tenendo una sola volta ogni riga distinta, e sostituisci di nuovo.
5. **Correzione** ("quella delle 9:15 era pipì, non cacca"): cambia esattamente quella riga, su richiesta del membro (rules → Tipi di file), e sostituisci come al punto 4.
6. **Nessuna voce di diario.** Ricostruisci l'`index.md` dell'istanza solo quando cambia la data Updated del registro sotto Maintained (la prima riga di un giorno). Propaga (rules → Regola di aggiornamento dello stato) solo quando le righe cambiano qualcosa che uno stato deve mostrare subito: qualcosa che il membro definisce preoccupante, un valore fuori dall'ordinario, una voce di Pending che il membro chiude. Altrimenti ci pensa il riepilogo giornaliero di `review` a portarle nello stato.

Fatto quando: ogni evento è una riga del registro in ordine di data e ora, esiste esattamente una copia del registro, e contiene tutte le righe che conteneva prima più le tue.

## Crea

1. **Tipo**: un **registro** per qualcosa che succede spesso e si conta o si cronometra (pannolini, poppate, spese, dosi di medicine); una **lista** per una checklist, un promemoria o qualunque cosa da stampare.
2. **Collocalo** in un'istanza, come al passo 3 della skill `log` (`greta/salute`, `famiglia/vacanze`).
3. **Concorda il nome, e le colonne per un registro**, in una sola domanda con una risposta consigliata: un nome minuscolo con trattini che dica di chi e cosa (`greta-pannolini.csv`, `lista-valigia-pasqua.md`); le colonne `date,time,author` e poi quelle che servono al membro (`type,notes`; `amount,category,notes`). Il nome deve essere libero in quella cartella `documents/`.
4. **Scrivi** il file dal suo modello (rules → Tipi di file → modelli):
   - Registro: la riga di intestazione, più le righe che il membro ti ha appena detto o che `file-document` ha importato, dalla più vecchia.
   - Lista: le voci che il membro ti ha dato. Per un promemoria o una lista costruiti da ciò che Agora sa ("un promemoria dei vaccini di Greta", "cosa mettere in valigia, dal viaggio dell'anno scorso"), scrivilo dallo `status.md` dell'istanza e dalle voci di diario che servono, dalle più recenti, e tienilo abbastanza corto da stare su una pagina stampata.
5. **Registra una voce di diario** che dica a cosa serve il file, seguendo i passi 4 e 6 della skill `log`: `documents: [documents/<name>]`; Summary come "Iniziato un registro dei pannolini per Greta"; Entry con le parole del membro, poi `Maintained: documents/<name>` e, per un registro, le sue colonne.
6. **Convenzione**, quando il file cambia un comportamento predefinito: un registro che toglie eventi dal diario lo fa sempre; una lista fatta su richiesta no. Un'importazione da `file-document` arriva senza: è quella skill a proporla dopo. Scrivi una riga in `org.md` → Conventions (Where: l'istanza; Convention: cosa va dove, il file, le sue colonne, "nessuna voce di diario per ogni <evento>; voce di riepilogo giornaliera"; Since: oggi e lo slug del membro). Sostituisci `org.md`.
7. **Aggiorna**: ricostruisci l'`index.md` dell'istanza con il file sotto Maintained (non sotto Documents) e la nuova voce sotto Diary, poi propaga.

Fatto quando: il file esiste una sola volta con il nome concordato, la voce di diario lo collega, l'indice dell'istanza lo elenca sotto Maintained, e `org.md` contiene la riga della sua Convenzione quando il file cambia un comportamento predefinito.

## Aggiorna una lista

1. Scarica la lista; quando nessuna corrisponde, chiedi se crearne una (Crea).
2. Cambia solo ciò che il membro ha chiesto: aggiungi, togli o spunta voci. "Rifai" o "aggiorna" un promemoria significa rigenerarlo dallo stato e dal diario attuali, come al punto 4 di Crea. Imposta la riga Updated ad adesso e allo slug del membro.
3. Sostituisci la lista. Ricostruisci l'`index.md` dell'istanza quando cambia la sua data Updated sotto Maintained.
4. Una modifica alla lista non richiede una voce di diario. Quando le parole del membro portano anche una novità ("abbiamo prenotato l'hotel, toglilo dalla lista"), registra quella novità come una voce normale con la skill `log`.

Fatto quando: la lista contiene esattamente la modifica richiesta, ha la nuova riga Updated, ed esiste una sola volta.

## Cambia una Convenzione

1. Trova la riga corrispondente in `org.md` → Conventions; quando più righe potrebbero corrispondere, chiedi quale. Una richiesta che non corrisponde a nessuna riga ("d'ora in poi, metti il peso di Greta nel registro della salute") è una nuova Convenzione: quando serve un nuovo file di lavoro, vai a Crea; altrimenti prosegui con la nuova riga.
2. Riformula il cambiamento in una frase con la data da cui vale, e aspetta un sì.
3. Scrivi in `org.md` la riga cambiata, aggiunta o tolta (Since: oggi e lo slug del membro), e sostituisci `org.md`.
4. Il file di lavoro resta: le sue righe sono storia. Un registro che smette di ricevere eventi resta sotto Maintained con "(interrotto YYYY-MM-DD)" in What it is. Una nuova colonna va in fondo all'intestazione, vuota nelle righe precedenti; sostituisci il registro.
5. Registra una voce di diario nell'istanza che dica cosa è cambiato e da quando, e propaga.

Fatto quando: `org.md` contiene la Convenzione come concordata (o non la contiene più), la voce di diario registra il cambiamento, e l'intestazione del registro corrisponde alle colonne della Convenzione.

## Suggerire

Quando si applica rules → Convenzioni → Suggerire, fai la sua unica domanda con la proposta concreta, una volta per conversazione. Al sì, vai a Crea. Al no, scrivi una riga di Convenzione che registri la scelta ("Pannolini: voci di diario, nessun registro (rifiutato)") così che le esecuzioni successive non ripropongano la domanda.
