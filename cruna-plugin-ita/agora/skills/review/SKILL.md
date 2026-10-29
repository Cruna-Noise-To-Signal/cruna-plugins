---
name: review
description: Fa la revisione di una radice Agora, a orari pianificati o su richiesta: riordina la cartella Drive condivisa e riassume cosa è successo e cosa arriva per la famiglia o il team. Usala quando una routine pianificata dice "Esegui revisione Agora", o quando un membro chiede ad Agora un riepilogo del giorno, della settimana o del mese ("fammi la revisione della settimana", "dimmi la settimana che arriva", "cosa abbiamo fatto questo mese", "revisione giornaliera").
---

# Revisione

Una **revisione** è il corpo di ogni routine pianificata. Prima **riordina** la radice così che ogni file sia al suo posto e ogni stato sia aggiornato, poi scrive la **revisione condivisa** del periodo in `reviews/`, poi invia **un messaggio personale** al membro per cui gira.

## Input

Il prompt di una routine pianificata è una riga:

```
Esegui revisione Agora: <giornaliera | settimanale | mensile> per <slug> [in <nome della radice>]
```

- **Periodo**: `giornaliera`, `settimanale` o `mensile`.
- **Membro**: lo slug del membro a cui è destinato il messaggio. Stabilisce l'identità: non si fa nessuna domanda.
- **Radice** (facoltativa): il nome visualizzato della radice da `org.md`, necessario solo quando l'account dell'assistente vede più radici.

Un prompt in questa forma indica un'**esecuzione non presidiata**: non c'è nessuno a rispondere, quindi segui rules → Esecuzioni non presidiate (agisci solo su ciò che è inequivocabile, segnala il resto) e salta il passo memoria.

Un membro che chiede in conversazione indica un'**esecuzione presidiata**: l'identità viene da rules → Identità, il periodo dalle sue parole ("oggi" → giornaliera, "la settimana" → settimanale, "il mese" → mensile), e un'ambiguità diventa una domanda invece che una segnalazione. Quando le sue parole indicano una finestra che i periodi qui sotto non coprono ("finora questo mese"), trattala come una **finestra personalizzata**: esegui ogni passo per quella finestra, ma salta i passi 3.1–3.3 (nessun file di revisione) e costruisci il messaggio personale dagli stati, dalle voci di diario e dagli eventi di calendario dentro la finestra.

## Periodi

Tutte le date sono nel fuso orario della radice. Le settimane sono settimane ISO (da lunedì a domenica) con il nome dell'anno ISO della settimana (`2026-W53`, poi `2027-W01`).

| Periodo | File | Prende il nome da | Sguardo indietro | Sguardo avanti |
|---|---|---|---|---|
| giornaliera | `reviews/YYYY-MM-DD-daily.md` | oggi | ieri | oggi |
| settimanale | `reviews/YYYY-Www-weekly.md` | la settimana ISO che contiene domani | la settimana ISO precedente | la settimana che dà il nome |
| mensile | `reviews/YYYY-MM-monthly.md` | il mese che contiene domani | il mese precedente | il mese che dà il nome |

"Che contiene domani" fa sì che l'esecuzione settimanale della domenica sera e un'esecuzione a mano del lunedì mattina finiscano sullo stesso file.

## Passi

### 1. Carica le regole, trova la radice, sappi chi sta parlando

Carica la skill `rules` e segui rules → Trovare la radice. In un'esecuzione non presidiata, usa la radice indicata nel prompt; quando esistono più radici e nessuna è indicata, fermati e restituisci una riga che dica al membro di aggiungere il nome della radice al prompt di questa routine. Prendi il membro dal prompt (non presidiata) o da rules → Identità (presidiata). Leggi `memories/<slug>.md` e `memories/shared.md`.

Fatto quando: hai letto `org.md`, conosci lo slug del membro e risulta elencato in `org.md`, hai letto le sue memorie, e hai il periodo, il nome del file ed entrambe le finestre dalla tabella qui sopra.

### 2. Riordina

Il riordino è **idempotente**: una seconda esecuzione su una radice invariata non cambia nulla, quindi le routine dei membri che girano nello stesso momento sono innocue. Percorri la radice una volta, elencando i figli di ogni cartella, e usa quell'elenco per tutti i controlli qui sotto.

1. **Duplicati.** Dovunque due file nella stessa cartella hanno lo stesso nome: per i file derivati e curati (`status.md`, `index.md`, `org.md`, `memories/`, `reviews/`) tieni il più recente e cestina gli altri (rules → Sostituire). Un registro duplicato nasce da due membri che scrivono insieme: scrivi una versione che contenga ogni riga distinta di tutte le copie in ordine di data e ora, poi cestina le copie. Una lista duplicata: tieni la più recente. I file sorgente (`diary/`, i `documents/` caricati) restano intatti; segnalali.
2. **Documenti non archiviati.** I file di lavoro sono già archiviati: lascia com'è, nome compreso, ogni file elencato sotto Maintained nell'`index.md` della sua istanza. Un file `.csv` o `.md` con un nome di tipo fisso (minuscolo, con trattini, senza timestamp) assente da Maintained è un file di lavoro quando una voce di diario della sua istanza dice `Maintained: documents/<name>`: il suo indice è obsoleto (punto 4). Senza una voce del genere, segnalalo ("`greta/salute/documents/poppate.csv` sembra un file di lavoro che nessuno ha creato: lo teniamo come registro o lo archiviamo come documento?"). Ogni altro file in `landing/`, e ogni file nella cartella `documents/` di un'istanza assente dall'`index.md` di quell'istanza, va alla skill `file-document` uno alla volta come caso (c), con il percorso del file, lo slug del membro, il periodo, e se l'esecuzione è non presidiata. Se presidiata, può chiedere al membro; se non presidiata, archivia ciò che è inequivocabile e segnala il resto. Propaga solo fino al livello dell'interesse e ti restituisce le sue segnalazioni invece di scriverle.
3. **Riepiloghi dei registri** (solo periodo giornaliero). Per ogni Convenzione in `org.md` che indirizza eventi a un registro, scrivi una voce di diario di riepilogo nella sua istanza per il giorno guardato indietro (rules → Convenzioni → Eventi frequenti). È inequivocabile, quindi lo fanno anche le esecuzioni non presidiate:
   - La voce è `diary/routine-YYYYMMDD_2359.md`, datata quel giorno. Quando esiste già, l'ha scritta un'altra esecuzione: salta. Un giorno senza righe non ha voce.
   - Compila il modello Voce di diario (rules → Tipi di file → modelli) con `author: routine`, `created` quel giorno alle 23:59, `documents: [documents/<tracker>]`, `events: []`. **Summary**: i conteggi in una riga ("12 pannolini: 7 pipì, 5 cacche"). **Entry**: conteggi per tipo, prima e ultima ora, e ogni cosa insolita: un lungo intervallo, una nota scritta da un membro, un conteggio molto diverso dai giorni prima.
   - Un registro mancante o illeggibile: segnalalo invece.
   Il punto 4 riprende poi ogni istanza toccata qui e la propaga.
4. **File derivati obsoleti.** Ricostruisci, dal basso verso l'alto (rules → Regola di aggiornamento dello stato), ogni file derivato più vecchio delle sue fonti:
   - lo `status.md` di un'istanza il cui Last updated è più vecchio della sua voce di diario o del suo documento più recente (il timestamp nel nome del file);
   - l'`index.md` di un'istanza le cui righe differiscono dai file nei suoi `diary/` e `documents/`, compresa la tabella Maintained (ogni file di lavoro elencato, con la data in cui è cambiato l'ultima volta);
   - lo `status.md` di un interesse più vecchio del Last updated di uno qualsiasi degli `status.md` delle sue istanze;
   - l'`index.md` di un interesse o della radice le cui righe differiscono dalle cartelle figlie, o il cui Last activity differisce dalla voce più recente di una figlia.
   Nelle esecuzioni non presidiate scrivi `routine` come autore della riga Last updated. Lo `status.md` della radice si ricostruisce una volta sola, al passo 3.
5. **Vecchie revisioni giornaliere.** Cestina ogni `reviews/*-daily.md` datato più di 30 giorni prima di oggi. Le revisioni settimanali e mensili sono la memoria di lungo periodo e restano.

Raccogli ogni segnalazione che sollevi in questo passo, e ogni segnalazione che `file-document` ti restituisce, per il Needs attention della radice (passo 3.4).

Fatto quando: hai percorso ogni cartella; nessun file derivato, curato o di lavoro ha un duplicato; ogni file in `landing/` e ogni documento non indicizzato è archiviato o segnalato, e nessun file di lavoro è stato rinominato o spostato; in un'esecuzione giornaliera, ogni registro con righe nel giorno guardato indietro ha la sua voce di riepilogo; nessun file derivato di istanza o di interesse è più vecchio delle sue fonti; nessuna revisione giornaliera ha più di 30 giorni.

### 3. Scrivi la revisione condivisa

1. **Trova** il file del periodo in `reviews/`.
2. **Nuovo periodo** (ancora nessun file): raccogli, poi crea il file dal modello Revisione (rules → Tipi di file → modelli):
   - **Looking back**: le voci di diario create nella finestra dello sguardo indietro (il loro nome contiene l'ora) e gli elementi Done che hanno prodotto, raggruppati per interesse, una riga ciascuno. I riepiloghi dei registri contano come voci di diario; nelle esecuzioni settimanali e mensili, aggiungi una riga per registro con i totali del periodo quando dicono qualcosa (meno poppate del solito, spese in aumento).
   - **Still open**: gli elementi Pending dagli stati delle istanze, ciascuno con chi o cosa aspetta.
   - **Looking ahead**: gli eventi del calendario di sistema nella finestra dello sguardo avanti, più gli elementi Next datati al suo interno, in ordine di data, con i titoli degli eventi, e un file di lavoro di tipo Lista che conti per uno di essi (la lista per la valigia prima di un viaggio).
   - **Needs attention**: le segnalazioni del passo 2 più ogni elemento Needs attention negli stati delle istanze.
   - **Written**: adesso, `da routine (<slug>)` se non presidiata, `da <slug>, a mano` se presidiata.
3. **File esistente**: leggilo. Il momento della sua ultima scrittura è l'ultimo titolo `## Update HH:MM`, altrimenti Written. Se dopo quel momento sono state create voci di diario, aggiungi in fondo una sezione `## Update <HH:MM>` che elenchi cosa hanno cambiato, una riga ciascuna, per interesse. Il testo precedente resta esattamente com'era: scrivi il file per sostituzione (rules → Sostituire) con il vecchio contenuto parola per parola seguito dalla nuova sezione. Nessuna voce di diario più recente: lascia il file intatto.
4. **Stato della radice.** Ricostruisci lo `status.md` della radice (rules → Regola di aggiornamento dello stato) dagli stati degli interessi. Sotto Needs attention, tieni ogni segnalazione precedente la cui condizione vale ancora (il file è ancora in `landing/`, il duplicato esiste ancora), togli quelle risolte, e aggiungi le nuove segnalazioni di questa esecuzione. Sotto Latest reviews, collega i file giornaliero, settimanale e mensile più recenti in `reviews/`.

Fatto quando: il file di revisione del periodo esiste, scritto una volta e da allora cambiato solo con sezioni Update aggiunte in fondo (finestra personalizzata: nessun file); e lo `status.md` della radice ha il Last updated di oggi, ogni segnalazione ancora aperta, e i collegamenti alla revisione più recente di ogni periodo.

### 4. Scrivi il messaggio personale

Un solo messaggio per il membro, costruito dalla revisione e dagli stati, in quest'ordine:

1. **Per te**: gli elementi assegnati al membro o in attesa di lui, gli eventi nella finestra dello sguardo avanti che coinvolgono lui o un interesse che lo riguarda (in una struttura per persona, l'interesse che porta il suo nome), e gli eventi del suo calendario personale quando `org.md` lo indica come condiviso con tutti i dettagli.
2. **Tutti gli altri**: gli altri punti salienti di Looking back e Looking ahead, una riga ciascuno.
3. **Richiede attenzione**: tutto ciò che richiede una decisione o un'azione, con la domanda che la risolve.

Scrivilo per essere letto su un telefono: righe brevi, date come giorno della settimana e giorno ("mar 3 nov"), niente di vuoto, nessun percorso tranne quello del file di revisione alla fine. Usa la lingua della radice e il tono che chiedono le memorie del membro.

Fatto quando: ogni elemento per il membro è in **Per te**, ogni elemento Needs attention della revisione (finestra personalizzata: dello `status.md` della radice) è nel messaggio, e il messaggio sta in una schermata di telefono (circa 15 righe). Per una finestra personalizzata, inoltre: ogni voce di diario ed evento di calendario dentro la finestra è nel messaggio o raccolto in una delle sue righe.

### 5. Passo memoria

Solo esecuzioni presidiate: segui rules → Passo memoria.

Fatto quando: ciò che di duraturo hai imparato è in `memories/`, oppure non hai trovato nulla di nuovo, oppure l'esecuzione è non presidiata.

## Output

Il messaggio personale del passo 4, che termina con il percorso della revisione (`Revisione completa: reviews/2026-W45-weekly.md`; nessuno per una finestra personalizzata), più la riga di `remember`, se c'è. In un'esecuzione pianificata è la notifica della routine, quindi sta in piedi da solo, senza saluti sull'esecuzione di un'attività. Quando qualcosa non ha funzionato (rules → Quando qualcosa non funziona), il messaggio dice in una frase cosa non ha funzionato e di chiedere aiuto ad Agora.
