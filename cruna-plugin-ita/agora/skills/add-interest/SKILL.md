---
name: add-interest
description: Aggiunge un nuovo interesse o una nuova istanza ad Agora, il quaderno condiviso della famiglia o dell'azienda su Google Drive. Usala quando l'utente vuole che Agora inizi a seguire qualcosa di nuovo, una nuova persona, un cliente, un progetto o un argomento ("inizia a seguire la ristrutturazione di casa", "nuovo cliente Acme"), o un nuovo ambito dentro uno già seguito ("aggiungi il nuoto di Greta", "Acme ci ha dato un nuovo lavoro").
---

# Aggiungere un interesse o un'istanza

Crea un nuovo **interesse** (cartella di primo livello) o una nuova **istanza** (cartella dentro un interesse) dai suoi modelli (rules → Tipi di file → modelli), e tiene allineati indici, stati e `org.md`. `setup` riusa il passo 4 e il livello di interesse del passo 6 per costruire i primi interessi; scrive lui stesso, una volta, la tabella Interests di `org.md` e l'`index.md` e lo `status.md` della radice, quindi salta tutto il resto quando ti chiama `setup`.

## 1. Carica le regole e la radice

Carica la skill `rules`. Segui rules → Trovare la radice, leggi `org.md` (prendi nota della convenzione `layout` e della tabella Interests), segui rules → Identità, poi leggi `memories/<slug>.md` e `memories/shared.md` (rules → Passo memoria).

Fatto quando conosci la radice, la sua convenzione di struttura e chi sta parlando, e hai letto le sue memorie.

## 2. Collocalo

Leggi l'`index.md` della radice, e l'`index.md` dell'interesse quando la richiesta nomina un interesse esistente. Poi decidi, in base alla convenzione di struttura:

- **Interesse**: una nuova cosa di primo livello del tipo indicato dalla convenzione (una nuova persona, un nuovo cliente).
- **Istanza**: un ambito dentro un interesse (il nuoto di Greta → interesse `greta`, istanza `nuoto`). Se l'interesse non esiste ancora, crea entrambi.
- **Più piccolo di un'istanza** (una visita, una fattura, una partita): è una voce di diario, non una cartella. Proponi invece di registrarlo con la skill `log`, dentro l'istanza a cui appartiene.

La profondità è esattamente due (rules → Vocabolario), quindi qualcosa che sembra richiedere un terzo livello diventa o un'istanza a sé o voci di diario.

Quando il tipo o la cartella madre non sono chiari, chiedi una volta, dando la risposta che consigli e le cartelle madri probabili prese dall'indice ("Nuoto sotto Greta o sotto Famiglia?"). Per ogni nuova cartella stabilisci il nome (rules → Struttura) e una descrizione di una riga.

Fatto quando l'utente ha confermato, per ogni nuova cartella: interesse o istanza, la cartella madre, il nome e la descrizione.

## 3. Controlla i duplicati

Confronta ogni nuovo nome con i figli esistenti della sua cartella madre, elencando sia la cartella stessa sia il suo `index.md` (un indice può essere in ritardo). Cerca corrispondenze esatte e **quasi-duplicati**: singolare/plurale, abbreviazioni, refusi, la stessa cosa in un'altra lingua (`nuoto`/`nuotare`, `acme`/`acme-srl`, `casa`/`house`). Controlla anche se la stessa cosa esiste già sotto un altro interesse.

Se trovi una corrispondenza, chiedi: usare quello esistente, o crearne uno nuovo con un nome diverso.

Fatto quando nessun nuovo nome corrisponde o quasi corrisponde a una cartella esistente, oppure l'utente ha scelto di procedere con un nome diverso.

## 4. Crea le cartelle e i file

Segui rules → Lavorare con Drive, e scrivi i contenuti nella lingua della radice.

- **Nuovo interesse**: cartella `<interest>/` con `index.md` e `status.md`. Un interesse ha bisogno di almeno un'istanza, perché solo le istanze contengono voci di diario e documenti: se l'utente non ne ha nominata nessuna, proponine una (per una persona, l'ambito che ha menzionato; altrimenti `generale`) e crea anche quella.
- **Nuova istanza**: cartella `<interest>/<instance>/` con `diary/`, `documents/`, `index.md` (tabelle Documents e Diary vuote) e `status.md` (Summary: che cos'è e che il monitoraggio è iniziato oggi; "—" sotto le altre sezioni).

Fatto quando ogni nuova cartella e ogni nuovo file esistono esattamente una volta.

## 5. Registra un nuovo interesse in org.md

Solo per un nuovo interesse. Aggiungi una riga alla tabella Interests in `org.md`: nome, descrizione e un colore se l'utente ne vuole uno. Colori del calendario: Tomato, Flamingo, Tangerine, Banana, Sage, Basil, Peacock, Blueberry, Lavender, Grape, Graphite. Consiglia un colore che nessun altro interesse usa; quando sono occupati tutti e undici, lascia il campo vuoto. Sostituisci `org.md` secondo rules → Sostituire.

Fatto quando l'`org.md` scaricato elenca il nuovo interesse.

## 6. Aggiorna indici e stati

Due livelli, seguendo rules → Regola di aggiornamento dello stato:

- **Livello di interesse**: ricostruisci l'`index.md` e lo `status.md` di ogni interesse che ha ricevuto un'istanza (quelli di un nuovo interesse, riassumendo le sue nuove istanze).
- **Livello di radice**: ricostruisci lo `status.md` della radice, e l'`index.md` della radice quando un interesse è nuovo.

Fatto quando ogni `index.md` padre elenca il suo nuovo figlio e gli `status.md` dell'interesse e della radice lo menzionano.

## 7. Passo memoria

Segui rules → Passo memoria (un nuovo interesse rivela spesso qualcosa di duraturo: un nuovo rapporto con un cliente, una nuova attività di un figlio).

Fatto quando: tutto ciò che di duraturo hai imparato è in `memories/`, oppure non hai trovato nulla di nuovo.

## Output

Una o due righe brevi nella lingua della radice, senza termini tecnici:

- Che cosa è stato aggiunto, come lo direbbe l'utente: "Il nuoto di Greta è pronto."
- Come usarlo da subito, con un esempio preso dalle sue parole: "Raccontami le novità, tipo 'Greta è passata al gruppo blu', oppure metti i file in Greta → nuoto → documents e dimmelo."

Se è stato trovato un duplicato e l'utente ha scelto quello esistente, di' dove andranno invece le cose. Chiudi con la riga di `remember`, se c'è.
