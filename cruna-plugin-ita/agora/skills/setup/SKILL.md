---
name: setup
description: Configura Agora, il quaderno condiviso di famiglia o di lavoro su Google Drive e Google Calendar. Usala quando l'utente vuole avviare Agora per la prima volta ("configura Agora", "iniziamo"), quando una persona entra in un Agora esistente o un membro inizia a usarlo da un'altra app ("aggiungi mia moglie ad Agora", "entro nell'Agora di famiglia", "ho installato Agora anche su ChatGPT"), quando un Agora di una sola persona deve passare a un proprio account dell'assistente perché entra qualcun altro, o quando qualcuno esce ("togli Simona da Agora", "toglimi da Agora").
---

# Configurazione di Agora

La configurazione trasforma un Google Drive e un Calendar vuoti in una **radice** Agora funzionante, vi porta dentro ogni membro e lo toglie quando esce. Ha quattro rami: **Nuova radice**, **Unisciti**, **Migra** (sempre seguito da Unisciti) e **Uscita**. Scegline uno al passo 1 e segui solo quel ramo. Ogni ramo finisce con la stessa **verifica**; ogni ramo tranne Uscita finisce con lo stesso messaggio "cosa puoi fare ora".

## 1. Carica le regole e scegli il ramo

Carica la skill `rules`. Segui rules → Trovare la radice, poi scegli il ramo:

| Cosa trovi | Ramo |
|---|---|
| Nessuna radice, e l'utente parte da zero | **Nuova radice** |
| Nessuna radice, ma l'utente dice che qualcuno l'ha già creata | I suoi connettori sono collegati all'account Google sbagliato: percorri la voce 3 dei prerequisiti, poi cerca di nuovo |
| Una radice la cui verifica (passo 6) fallisce, e l'utente ne è il proprietario | **Nuova radice**, riprendendo dal primo elemento mancante del passo 2c |
| Una radice, e l'utente ci entra o la usa da una nuova app | **Unisciti** |
| Una radice solitaria (un solo membro, `assistant_account` è l'account personale di quel membro) e sta entrando qualcuno di nuovo | **Migra**, poi **Unisciti** per il nuovo arrivato |
| Una radice, e un membro esce ("toglimi da Agora", "Simona non ha più bisogno di accedere") | **Uscita** |

Identità: in una radice trovata, segui rules → Identità e leggi le memorie della persona (rules → Passo memoria). In una nuova radice, la persona che parla diventa il primo membro durante l'intervista.

Fatto quando il ramo è scelto e, per una radice trovata, sai chi sta parlando.

La checklist dei prerequisiti è il file `references/prerequisites.md` della skill `rules`; "voce N" qui sotto indica le sue sezioni numerate. Percorri ogni voce con chiamate reali e chiudila **pronta** o **bloccata**, come dice quel file. Una voce bloccata ferma il ramo finché la sua soluzione non è applicata, tranne la voce 5 (condivisione) per qualcuno che non è presente: registrala e vai avanti.

## 2. Nuova radice

### 2a. Prerequisiti

1. Chiedi: "Qualcun altro lo userà con te?"
   - Sì → tutto appartiene a un **account dell'assistente** dedicato (si applica la voce 4).
   - No → **solitaria**: l'account personale dell'utente fa da assistente; salta la voce 4. Digli che potrà passare a un account dedicato più avanti, quando entrerà qualcuno.
2. Percorri la voce 1, la voce 2, poi (solo se dedicato) la voce 4, poi la voce 3. L'account dell'assistente deve esistere prima che i connettori vi si colleghino.
3. Per la voce 3, verifica con quale account sono collegati i connettori: il calendario principale nella lista dei calendari riporta l'indirizzo email dell'account. Deve essere l'account dell'assistente (o quello dell'utente, se la radice è solitaria).

Fatto quando le voci 1–4 sono tutte pronte (la voce 4 è saltata se la radice è solitaria).

### 2b. Intervista

Segui [references/interview.md](references/interview.md): membri, nome della radice, lingua, fuso orario, convenzione di struttura, primi interessi e istanze. **Incalza con garbo**: poche domande per giro, ognuna con una risposta consigliata che l'utente può accettare con una parola.

Fatto quando l'utente ha confermato il riepilogo alla fine dell'intervista.

### 2c. Costruzione

Costruisci in quest'ordine, scrivendo ogni file dal suo modello (rules → Tipi di file → modelli) nella lingua della radice e seguendo rules → Lavorare con Drive. Se la cartella radice esiste già (stai riprendendo), salta ciò che la verifica mostra già presente.

1. Cartella radice nel Drive dell'account dell'assistente, con il nome della radice.
2. `org.md`, con `agora: root` nel front-matter, ogni membro in Members, la convenzione di struttura e una riga Access per membro (Drive e calendario "in attesa" fino al passo 2d). Va scritto per primo, così una configurazione interrotta viene trovata e ripresa.
3. `landing/`, `reviews/`, `memories/` con `shared.md` e un `<slug>.md` per membro (solo la riga del titolo, più quanto di duraturo è emerso dall'intervista).
4. La tabella Interests in `org.md`, con ogni interesse dell'intervista, la sua descrizione e il suo colore (colori come al passo 5 di `add-interest`), scritta una volta sola (rules → Sostituire).
5. Ogni interesse e istanza dell'intervista, costruiti con il passo 4 della skill `add-interest`, poi il livello interesse del suo passo 6 (`index.md` e `status.md` di ogni interesse, riassunti dalle sue istanze). Hai già la radice e le risposte, quindi salta le sue domande.
6. `index.md` e `status.md` della radice, ricostruiti una volta alla fine della costruzione così da riassumere tutto ciò che sta sotto.
7. Il **calendario di sistema**, con il nome registrato in `org.md` e il fuso orario di `org.md`. Se il connettore del calendario può creare calendari, crealo. Se non può, dai all'utente questi passi, da fare con l'accesso a Google Calendar come account dell'assistente: a sinistra, accanto ad "Altri calendari" (Other calendars), fai clic su **+** → **Crea nuovo calendario** (Create new calendar) → scrivi il nome → **Crea calendario** (Create calendar). Poi elenca i calendari finché non compare.

Fatto quando ogni elemento qui sopra esiste esattamente una volta e il calendario compare nella lista dei calendari.

### 2d. Condivisione

Percorri la voce 5 per ogni membro. Puoi condividere tu la cartella radice quando il connettore di Drive può condividere; i passi del calendario sono clic per chi ha l'accesso come account dell'assistente. Chiedi a ogni membro di condividere il proprio calendario personale al livello registrato in `org.md`. Aggiorna la tabella Access in `org.md` con ciò che ora è vero.

Radici solitarie: niente da condividere; segna la riga del proprietario come proprietario.

Fatto quando la tabella Access mostra l'accesso reale di ogni membro a Drive e al calendario, e ogni cosa ancora in sospeso è indicata per il messaggio finale.

### 2e. Routine

Dai all'utente le tre routine di [references/routines.md](references/routines.md), compilate per lui, poi percorri la voce 6.

Fatto quando l'utente conferma che le tre routine sono nella sua lista delle attività pianificate, o la voce 6 è registrata come bloccata.

Vai al passo 6.

## 3. Unisciti

Per un nuovo membro, o per un membro esistente su una nuova app. Eseguilo nell'app della persona che entra.

1. Percorri le voci 1, 2 e 3. I connettori si collegano come l'account dell'assistente indicato in `org.md`; la persona riceve la password da chi la custodisce.
2. Trova la radice (rules → Trovare la radice) e segui rules → Identità.
3. **Solo nuovo membro**: intervistalo con il giro Membri di [references/interview.md](references/interview.md) (solo per sé). Aggiungilo a Members e Access in `org.md`, crea `memories/<slug>.md` e percorri la voce 5 per lui: condividi la cartella radice e il calendario di sistema con il suo account Google personale, e il suo calendario personale con l'account dell'assistente al livello che sceglie.
4. **Membro esistente, nuova app**: aggiorna la sua riga Access ("Client connected").
5. Dagli le sue routine da [references/routines.md](references/routines.md) e percorri la voce 6.

Fatto quando la persona è in Members e Access con il suo accesso reale, ha un file in `memories/` e la voce 6 è pronta o bloccata. Vai al passo 6.

## 4. Migra (da solitaria ad account dedicato)

Eseguilo con il proprietario della radice, prima che entri il nuovo arrivato. Segui [references/migration.md](references/migration.md): crea l'account dell'assistente, passagli la cartella radice e il calendario, ricollega i connettori del proprietario e aggiorna `org.md`.

Fatto quando vale il criterio di completamento della migrazione stessa. Poi esegui **Unisciti** per il nuovo arrivato.

## 5. Uscita

Eseguilo con il proprietario della radice, con l'accesso come account dell'assistente dove un passo richiede dei clic. Segui il file `references/prerequisites.md` della skill `rules` → Uscita di un membro, confermando ogni passo con il proprietario prima di farlo o di affidarglielo:

1. Togli al membro l'accesso come editor alla cartella radice (lo fai tu quando il connettore di Drive può modificare la condivisione; altrimenti dai al proprietario i passi da cliccare).
2. Togligli l'accesso al calendario di sistema: Google Calendar (come account dell'assistente) → Impostazioni (Settings) → il calendario → Condividi con persone specifiche (Share with specific people) → rimuovilo.
3. Chiedi al membro, se raggiungibile, di scollegare i connettori di Drive e Calendar nella sua app e di eliminare le sue routine di Agora.
4. Se conosceva la password dell'account dell'assistente, fai cambiare la password al proprietario e fagliela passare ai membri rimasti.
5. Aggiorna `org.md` (rules → Sostituire): scrivi `rimosso YYYY-MM-DD` nelle Notes della sua riga Members, e `rimosso` sotto Drive e System calendar nella sua riga Access. Le sue voci di diario, i suoi documenti e `memories/<slug>.md` restano: sono storia.

Fatto quando il membro non ha più accesso come editor alla cartella né al calendario e le righe Members/Access in `org.md` lo mostrano rimosso. Vai al passo 6.

## 6. Verifica

Controlla la radice con chiamate reali, mai a memoria di ciò che hai costruito:

- `org.md` si scarica, il suo front-matter contiene `agora: root` e ogni membro ha una riga Members e una riga Access (le righe di un membro rimosso lo dicono, e la cartella e il calendario non lo elencano più).
- Esistono `index.md` e `status.md` della radice, `landing/`, `reviews/`, `memories/shared.md` e un `memories/<slug>.md` per membro.
- Ogni interesse in `org.md` e nell'`index.md` della radice ha una cartella con `index.md` e `status.md`; ogni istanza ha `index.md`, `status.md`, `diary/` e `documents/`.
- Nessun nome di file compare due volte nella stessa cartella (rules → Sostituire).
- Il calendario di sistema indicato in `org.md` è nella lista dei calendari.

Correggi ciò che fallisce e controlla di nuovo. Fatto quando ogni riga passa.

## 7. Passo memoria

Segui rules → Passo memoria. La configurazione di solito fa scoprire molto (ruoli, abitudini, come le persone vogliono essere chiamate): scrivilo ora.

Fatto quando: tutto ciò che di duraturo hai imparato è in `memories/`, oppure non hai trovato nulla di nuovo.

## Output

Un messaggio breve nella lingua della radice, senza termini tecnici. Uscita: una riga che dice chi è stato rimosso, ogni cosa ancora in sospeso come cosa da fare con chi la fa, e la riga di `remember`, se c'è. Ogni altro ramo:

1. Una riga: l'Agora è pronto (oppure: questa persona ora ne fa parte), con il nome della radice.
2. Ogni cosa ancora in sospeso, come cosa da fare con chi la fa ("Marta: accetta l'invito al calendario").
   Quando l'intervista ha trovato un foglio o una nota che tengono già, proponi di importarlo subito, una domanda per foglio (la skill `file-document`, Importa).
3. **Cosa puoi fare ora**, quattro o cinque righe con una frase di esempio ciascuna, prese dai loro interessi:
   - Raccontami le novità: "Le analisi del sangue di Nina sono andate bene." (`log`)
   - Aggiungi un file: mettilo nella cartella `landing` e dimmelo. (`file-document`)
   - Imposta un promemoria o un appuntamento: "Ricordaci di rinnovare l'assicurazione dell'auto il 12 marzo." (`schedule`)
   - Chiedi: "Cosa c'è in sospeso per Nina?" (`ask`)
   - Segui qualcosa di nuovo: "Inizia a seguire la ristrutturazione della casa." (`add-interest`)
4. Quando arriveranno le revisioni (ogni mattina, la domenica sera, il primo del mese), e che "aiuto" spiega tutto il resto.
5. La riga di `remember`, se c'è.
