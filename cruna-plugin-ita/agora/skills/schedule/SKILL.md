---
name: schedule
description: Pianifica sul calendario condiviso di Agora: crea, sposta o annulla appuntamenti, scadenze, promemoria e tempi di preparazione, e annota ogni modifica nel diario. Usala quando un membro chiede di ricordargli qualcosa o di fissare, spostare o annullare qualcosa a una data ("ricordami di prenotare il pediatra", "fissa l'incontro con il cliente X martedì prossimo alle 10", "annulla la lezione di giovedì"), o quando un'altra skill di Agora trova una data da mettere in calendario.
---

# Calendario

Tutto ciò che Agora deve ricordare a una data vive nel **calendario di sistema** della radice (rules → Convenzioni del calendario). Tu scrivi l'evento, controlli che sia compatibile con gli impegni delle persone, e lasci una traccia nel diario così che ogni stato sappia cosa viene dopo.

Vieni chiamata in due modi:

- **Diretta**: te lo chiede un membro. Scrivi tu la voce di diario.
- **Su consegna** da `log` o `file-document`: chi ti chiama ti passa l'istanza, il percorso della voce che sta per scrivere e i dettagli, spesso già confermati. Restituisci i titoli degli eventi; chi ti ha chiamata li registra e propaga.

Chiamata da `ask`: lavora in modalità Diretta, usando l'istanza che ti passa `ask` (senza chiederla).

## Passi

### 1. Carica le regole, trova la radice, sappi chi sta parlando

Carica la skill `rules`. Segui rules → Trovare la radice e rules → Identità, poi leggi `memories/<slug>.md` e `memories/shared.md` per le abitudini che fissano i valori predefiniti (orari abituali degli appuntamenti, chi accompagna i bambini). Su consegna, l'ha già fatto chi ti chiama.

Fatto quando: hai letto `org.md` (calendario di sistema, fuso orario, membri, colori) e conosci lo slug del membro.

### 2. Colloca l'evento

Ogni evento appartiene a un'istanza: il suo titolo inizia con `[<interest>/<instance>]`. Su consegna, te la passa chi ti chiama; da `ask`, usa l'istanza che ti passa. In modalità Diretta, confronta la richiesta con i file `index.md` della radice e degli interessi; quando non è chiaro, chiedi, offrendo le opzioni probabili. Quando l'istanza non esiste ancora, offri la skill `add-interest`.

Fatto quando: conosci l'unica istanza a cui appartiene l'evento.

### 3. Fissa cosa e quando

Decidi l'azione (creare, modificare, eliminare) e il tipo:

| Tipo | Forma |
|---|---|
| Appuntamento, incontro | Con orario, 1 ora salvo indicazioni diverse |
| Scadenza, pagamento dovuto | Tutto il giorno, alla data di scadenza |
| Promemoria ("ricordami di…") | Con orario, 15 minuti, con una notifica pop-up all'inizio |
| Segnaposto (data non definitiva) | Come il tipo che rappresenta, titolo che termina con `(da confermare)` |
| Tempo di preparazione | Un evento separato prima di quello che prepara, titolo che inizia con `Preparazione:` |

Risolvi ogni data rispetto a oggi nel fuso orario della radice: "martedì prossimo", "tra due settimane", "il 3". Quando una data o un orario può significare due cose, o un appuntamento non ha orario, chiedi, offrendo un valore predefinito concreto ("martedì 4 nov o martedì 11 nov?", "alle 10:00 come al solito?"). Ciò che si ripete ("ogni giovedì") diventa un evento ricorrente.

Per modificare ed eliminare, **trova** l'evento esistente: cerca nel calendario di sistema il prefisso di titolo `[<interest>/<instance>]` intorno alla data indicata. Più corrispondenze: chiedi quale, elencandole con le date. Nessuna: dillo e offri di crearlo.

Fatto quando: hai l'azione, il titolo esatto, inizio e fine (o la data per tutto il giorno) nel fuso orario della radice e, per modifica ed eliminazione, l'unico evento a cui si applica.

### 4. Controlla le sovrapposizioni

Per gli eventi con orario, leggi il calendario di sistema e i calendari personali dei membri coinvolti (tutti i membri quando non è chiaro) condivisi con l'account dell'assistente (`org.md`, colonna Personal calendar). I calendari condivisi solo come libero/occupato mostrano solo i blocchi occupati; basta così. Quando qualcosa si sovrappone, di' chi è occupato e proponi il primo spazio libero più vicino.

Fatto quando: l'orario non ha sovrapposizioni, oppure il membro l'ha scelto conoscendo la sovrapposizione.

### 5. Conferma in una riga

Mostra l'evento finale in una riga e aspetta un sì: "Creo *[nina/salute] Visita pediatrica*, mar 4 nov, 10:00–11:00?" Per un'eliminazione, indica l'evento e la sua data. Salta questo passo quando il membro ha già detto sì proprio a questi dettagli, in questa skill o nella domanda di chi ti ha chiamata.

Fatto quando: il membro ha confermato esattamente questo evento.

### 6. Scrivi l'evento

Solo sul calendario di sistema:

- **Titolo**: `[<interest>/<instance>] <titolo>`, nella lingua della radice.
- **Descrizione**: il percorso dell'istanza e il percorso della voce di diario (Diretta: il percorso che fissi al passo 7; su consegna: quello di chi ti chiama).
- **Colore**: il colore dell'interesse da `org.md`, quando è impostato.
- **Invitati**: solo quando il membro lo chiede; aggiungi l'email di ogni membro nominato da `org.md`. Quando manca un'email, chiedila e offri di registrarla in `org.md` (rules → Sostituire).

Fatto quando: il calendario mostra l'evento creato, modificato o eliminato come confermato.

### 7. Registralo

Su consegna: restituisci i titoli degli eventi a chi ti ha chiamata e fermati qui; è chi ti ha chiamata a registrarli e propagare.

Diretta:

1. Fissa il percorso della voce: dai il nome alla voce secondo rules → Struttura.
2. Crea la voce dal modello Voce di diario (rules → Tipi di file → modelli). **Summary**: cosa è cambiato e quando ("Visita pediatrica prenotata per mar 4 nov, 10:00"). **Entry**: le parole del membro. `events`: ogni titolo che hai creato, modificato o eliminato.
3. Ricostruisci l'`index.md` dell'istanza con la voce in cima (rules → Sostituire), poi propaga (rules → Regola di aggiornamento dello stato) così che Next mostri l'evento a ogni livello.

Fatto quando: la voce di diario esiste, e gli stati di istanza, interesse e radice elencano l'evento sotto Next (o non elencano più un evento eliminato).

### 8. Passo memoria

Solo in modalità Diretta. Segui rules → Passo memoria.

Fatto quando: ciò che di duraturo hai imparato è in `memories/`, oppure non hai trovato nulla di nuovo.

## Output

Diretta, un solo messaggio breve nella lingua della radice: cosa c'è ora in calendario (o cosa è stato tolto), con giorno, data e ora, e chi è stato invitato, se qualcuno. Una riga per evento, più la riga di `remember`, se c'è.

Su consegna: nulla di tuo; riferisce chi ti ha chiamata.
