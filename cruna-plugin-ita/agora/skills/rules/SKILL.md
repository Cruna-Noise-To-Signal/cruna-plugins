---
name: rules
description: Il modello dei dati di Agora e le regole sui file che ogni skill Agora segue. Caricala prima di leggere o scrivere qualsiasi cosa in una radice Agora (la cartella condivisa su Google Drive con org.md, interessi, istanze, diario, documenti, file di stato e di indice) o nel suo calendario di sistema.
---

# Regole di Agora

Agora è una base di conoscenza condivisa tenuta dall'AI su Google Drive (file) e Google Calendar (tempo). Le persone inseriscono informazioni; l'AI le archivia, tiene aggiornato ogni **stato** e restituisce promemoria e revisioni. Un gruppo di una persona è solo un gruppo piccolo: ogni regola qui vale per un membro come per cinque.

## Vocabolario

- **Radice**: un Agora indipendente (una famiglia, una piccola impresa). Ha la sua cartella, i suoi membri, il suo calendario di sistema, le sue memorie e le sue routine.
- **Interesse**: una cartella di primo livello nella radice. Che cosa *sia* un interesse (una persona, un cliente, un argomento) lo stabilisce la **convenzione di struttura** della radice, registrata in `org.md`.
- **Istanza**: una cartella dentro un interesse; l'unità che contiene un diario e dei documenti. La profondità è sempre esattamente due: `root/<interest>/<instance>/`. Qualcosa di più piccolo di un'istanza (un singolo esame del sangue, una singola fattura) è una voce di diario con i suoi documenti, mai una cartella.
- **Membro**: una persona in `org.md`. Il suo **slug** è il nome di battesimo, minuscolo, ASCII (`simone`).
- **Account dell'assistente**: l'account Google con cui sono collegati i connettori di Drive e Calendar. Un account dedicato quando la radice ha più membri; l'account del proprietario quando è da solo.

## Solo connettori

Ogni lettura e scrittura su Drive e Calendar passa dagli strumenti dei connettori Google Drive e Google Calendar. Il browser non è mai una strada: niente apertura di drive.google.com o calendar.google.com, niente strumenti di navigazione o di uso del computer, anche quando una chiamata al connettore fallisce o uno strumento sembra mancare. Ciò che uno strumento del connettore offre, lo fai tu, riprovando una chiamata fallita. Ciò che nessuno strumento del connettore offre (condividere un calendario, per esempio) diventa una serie di passi da cliccare per l'utente, con l'accesso all'account indicato nel passo.

## Trovare la radice

1. Cerca su Drive i file chiamati `org.md` il cui front-matter contiene `agora: root`, e i file chiamati `setup.md` il cui front-matter contiene `agora: setup`. Ogni cartella che ne contiene uno è una radice.
2. Nessuno trovato: l'utente non ha ancora un Agora. Proponi la skill `setup`.
3. Più di uno: chiedi quale radice, a meno che la conversazione non la indichi già.
4. Leggi `org.md` prima di qualsiasi altra cosa. Contiene i membri, la convenzione di struttura, la lingua, il fuso orario, il calendario e le Convenzioni.
5. Una radice con `setup.md` ma senza `org.md` è una configurazione lasciata a metà: proponi la skill `setup` per finirla, e non scrivere nient'altro lì.

Scrivi tutti i contenuti nella lingua indicata in `org.md`. Nomi di cartelle, nomi di file e titoli di sezione restano in inglese esattamente come scritti qui.

## Struttura

```
<root>/
├── org.md
├── setup.md
├── index.md
├── status.md
├── landing/
├── memories/      <slug>.md, shared.md
├── reviews/       YYYY-MM-DD-daily.md, YYYY-Www-weekly.md, YYYY-MM-monthly.md
└── <interest>/
    ├── index.md
    ├── status.md
    └── <instance>/
        ├── index.md
        ├── status.md
        ├── diary/       <slug>-YYYYMMDD_HHMM.md
        └── documents/   <slug>-YYYYMMDD_HHMM_<original-name>.<ext>
                         <name>.csv | <name>.md   (file di lavoro)
```

La cartella radice prende il nome visualizzato della radice, a meno che l'utente non ne scelga un altro. I nomi delle cartelle di interessi e istanze sono minuscoli, con trattini, ASCII (`nina`, `piscina`). Gli orari usano il fuso orario della radice; quando il nome di una voce di diario o di un documento è già occupato, usa il primo minuto libero successivo. Le voci di diario scritte dalla routine `review` usano `routine` al posto dello slug di un membro.

## Tipi di file

Ogni file è di uno di quattro tipi; il tipo decide come può cambiare.

- **Sorgente**: le voci in `diary/` e i documenti caricati in `documents/`. Solo aggiunta: crea, non modificare mai. Una correzione è una nuova voce di diario che dice che cosa corregge.
- **Derivato**: ogni `status.md` e `index.md`. Ricostruito dalle sorgenti sottostanti, mai modificato a mano, sempre ricostruibile senza rischi. Una ricostruzione sostituisce l'intero file.
- **Curato**: `org.md`, `setup.md`, `memories/`, `reviews/`. Modificato sul posto sostituendo il file; tieni `org.md` e `memories/` brevi aggiornando le righe invece di aggiungerne all'infinito. Le revisioni sono solo aggiunta: un aggiornamento sostituisce il file con il vecchio contenuto parola per parola più la nuova sezione.
- **Di lavoro**: un file in `documents/` che l'AI mantiene per i membri, con un nome fisso e senza timestamp (`greta-pannolini.csv`, `lista-valigia.md`). Si aggiorna sostituendolo per intero (rules → Sostituire). Due forme:
  - **Registro**: CSV, riga di intestazione e poi una riga per evento (`date,time,author,…`), dal più vecchio. Per gli eventi frequenti una Convenzione li indirizza qui invece che nel diario. Una riga del registro è un dato sorgente: modifica le righe solo per correggerle, su richiesta di un membro.
  - **Lista**: Markdown che l'AI scrive o rigenera su richiesta (una lista da stampare, un promemoria, una checklist). Può essere riscritta liberamente.
  Ogni file di lavoro è stato creato insieme a una voce di diario che lo elenca in `documents:` e dice `Maintained: documents/<name>`; una ricostruzione dell'indice elenca ognuno di questi file sotto Maintained.

Modelli per ogni file: [references/templates.md](references/templates.md).

## Checklist di configurazione

`setup.md` registra la configurazione della radice e di ogni membro come una checklist, così una configurazione lasciata a metà riprende da dove si era fermata. Il suo `state` nel front-matter è `done` quando ogni casella è spuntata, altrimenti `todo`. Solo la skill `setup` lo scrive. La skill `review` segnala le sue caselle non spuntate; ogni altra skill lo lascia stare. Una radice configurata prima che esistesse la checklist non ha `setup.md` finché `setup` non vi gira di nuovo.

## Convenzioni

I valori predefiniti di queste regole e di ogni skill valgono a meno che `org.md` → Conventions non dica altrimenti. Prima di applicare un valore predefinito a un'istanza, cerca in Conventions una riga il cui Where corrisponde (`greta/salute`, `greta` o `*`); vince la riga più specifica. Una Convenzione si scrive solo quando un membro la chiede o risponde a un suggerimento, con la data e chi ha risposto. Anche un suggerimento rifiutato viene registrato ("rifiutato"), così non viene proposto di nuovo.

### Suggerire

Suggerisci un file di lavoro o una Convenzione quando noti uno schema ricorrente: diverse voci simili nella stessa istanza nell'arco di un giorno, la richiesta di una lista, di un promemoria o di qualcosa da stampare, o un membro che tiene un proprio foglio o una propria nota per qualcosa che Agora già segue. Una domanda breve con la proposta concreta ("Vuoi che tenga i pannolini di Greta in un file registro invece di una voce di diario per ciascuno?"); se la risposta è sì, crea il file di lavoro, scrivi la Convenzione e registra una voce di diario che lo dice.

### Eventi frequenti

Gli eventi indirizzati a un registro non ricevono una voce di diario ciascuno. La `review` giornaliera scrive una voce di riepilogo al giorno per quell'istanza (conteggi, qualsiasi cosa insolita), così stato e storia restano leggibili.

## Lavorare con Drive

- **Leggi** il contenuto grezzo scaricando il file. La vista di estrazione del testo applica l'escape al Markdown; usala solo per PDF, immagini e file Office.
- **Crea** il Markdown con tipo di contenuto `text/markdown` e con la conversione in Google Docs disattivata, così i file restano `.md` semplici.
- **Riferisci** i file con il percorso dalla radice (`nina/salute/documents/…`), mai con l'ID di Drive: gli ID cambiano a ogni sostituzione.
- Cerca le cartelle scendendo dalla radice per nome, un livello alla volta.

### Sostituire

Drive non può sovrascrivere il contenuto di un file e ammette due file con lo stesso nome. Per modificare un file derivato o curato:

1. Trova il file attuale per cartella madre + nome esatto.
2. Crea la nuova versione nella stessa cartella.
3. Sposta nel cestino ogni file più vecchio con quel nome in quella cartella.

Duplicati trovati in qualsiasi momento:
- file derivato o curato, o una lista: tieni il più recente, sposta nel cestino gli altri;
- registro: unisci tutte le copie in una (ogni riga distinta, in ordine di data e ora), poi sposta nel cestino le altre;
- voci di diario `routine-` con contenuto identico: tienine una, sposta nel cestino le altre;
- qualsiasi altro file sorgente (diario, documenti caricati): tienili tutti e segnalali.

## Regola di aggiornamento dello stato

Uno stato descrive la situazione **adesso**. Quando lo ricostruisci, leggi le sorgenti dalla più recente e lascia che le voci recenti pesino più di quelle vecchie: una voce più recente che chiude, annulla o sposta qualcosa prevale su quella più vecchia. I fatti vecchi restano solo finché contano ancora (un'allergia permanente, una fattura non pagata).

Ogni `status.md`, a ogni livello, ha le stesse sezioni in quest'ordine: Last updated, Needs attention, Summary, Done, Pending, Next. A livello di radice e di interesse ogni sezione riassume il livello sottostante, una riga per ogni figlio che ha qualcosa da dire.

Le righe di un registro arrivano allo stato attraverso la voce di riepilogo giornaliera; propaga su una riga solo quando cambia qualcosa che conta per lo stato.

**Propaga** dopo ogni modifica a un'istanza: ricostruisci il suo `status.md`, poi lo `status.md` del suo interesse, poi lo `status.md` della radice. Ricostruisci l'`index.md` dell'istanza a ogni modifica dell'istanza. Ricostruisci l'`index.md` di un interesse o della radice solo quando è cambiato l'elenco dei suoi figli o la descrizione di un figlio; il riordino della `review` aggiorna la loro colonna Last activity.

Un file derivato è **obsoleto** quando una sorgente sottostante è più recente della sua riga Last updated (`status.md`), oppure quando le sue righe non corrispondono alle cartelle e ai file effettivamente presenti o alla voce più recente di ciascun figlio (`index.md`).

## Chiedi, non dare per scontato

Quando la radice, l'interesse, l'istanza, il membro o la data non sono chiari, chiedi all'utente prima di scrivere. Proponi le opzioni probabili prese dagli indici, così basta una parola per rispondere. Chiedi una volta sola per tutto il gruppo di elementi invece che una volta per ciascuno.

### Identità

All'inizio di ogni conversazione, chiedi con chi stai parlando (elenca i membri da `org.md`), a meno che non sia già noto in questa conversazione. Ogni voce di diario e ogni documento porta lo slug di quel membro.

### Esecuzioni non presidiate

Le routine pianificate non hanno nessuno a cui chiedere. Agiscono solo su ciò che è inequivocabile e **segnalano** il resto.

### Segnalare

Aggiungi una riga sotto Needs attention nello `status.md` della radice che dica che cosa è stato trovato e quale domanda o correzione manuale lo risolve. Anche le esecuzioni presidiate segnalano, quando un'azione fallisce e richiede una correzione manuale; in quel caso lo dicono anche al membro. Ogni ricostruzione dello `status.md` della radice mantiene ogni segnalazione la cui condizione è ancora vera ed elimina quelle risolte.

## Convenzioni del calendario

- Il **calendario di sistema** della radice è il calendario principale dell'account dell'assistente (`calendar` in `org.md`, l'email dell'account). Scrivi solo lì. I calendari personali dei membri, quando condivisi, sono in sola lettura: usali per trovare il tempo libero.
- Titolo dell'evento: `[<interest>/<instance>] <title>` (`[nina/salute] Esame del sangue`).
- Descrizione dell'evento: il percorso dell'istanza e la voce di diario che ha originato l'evento.
- Colore: il colore dell'interesse indicato in `org.md`, quando è impostato.
- Tutto ciò che va ricordato a una certa data diventa un evento: appuntamenti, scadenze, promemoria, tempo di preparazione.

## Passo memoria

Chiudi ogni interazione chiedendoti: ho imparato qualcosa di duraturo su un membro, sul gruppo o su come preferiscono lavorare? Se sì, aggiorna `memories/<slug>.md` o `memories/shared.md` seguendo la skill `remember`. Leggi le memorie pertinenti prima di rispondere o scrivere, e lascia che orientino tono e valori predefiniti.

## Quando qualcosa non funziona

Un connettore mancante, un errore di permessi, o una cartella o un calendario che non riesci a raggiungere è un problema di configurazione, non di dati. Fermati, di' all'utente in una frase che cosa non ha funzionato e guidalo attraverso la voce corrispondente in [references/prerequisites.md](references/prerequisites.md).
