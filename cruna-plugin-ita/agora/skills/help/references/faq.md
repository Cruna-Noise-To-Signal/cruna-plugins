# FAQ di Agora

Risposte per i membri, in parole semplici. Sostituisci le parti `<…>` con i nomi reali da `org.md`. Tieni ogni risposta in poche righe.

## Dove sono i miei file?

In una cartella di Google Drive chiamata `<nome della radice>`, di proprietà di `<account dell'assistente>` e condivisa con ogni membro. Apri l'app Google Drive o drive.google.com con il tuo account Google → **Condivisi con me** (Shared with me) → `<nome della radice>` (in un Agora solo tuo, sul tuo account, è in **Il mio Drive** (My Drive)). Dentro c'è una cartella per `<interesse, secondo la convenzione di struttura>` e, in ognuna, una cartella per argomento con i suoi appunti (`diary`) e i suoi file (`documents`). Ogni `status.md` è un riassunto da un minuto di come stanno le cose.

## Chi vede cosa?

Tutti in `<nome della radice>` vedono tutto nella cartella e nel calendario condiviso di Agora, comprese le note che Agora tiene sulle preferenze di ciascuno (`memories`). Il tuo calendario Google personale resta tuo: Agora lo vede solo se l'hai condiviso, e solo al livello che hai scelto (solo gli orari occupati, o tutti i dettagli). La cartella e il calendario sono condivisi solo con i membri elencati in `org.md`.

## Come aggiungo un documento dal telefono?

1. Apri l'app Google Drive → **Condivisi con me** → `<nome della radice>` → `landing`.
2. Tocca **+** → **Carica** (Upload), e scegli il file o la foto.
3. Di' ad Agora "ho messo <cos'è> in landing". Lo archivia al posto giusto.

Se dimentichi il passo 3, la prossima revisione giornaliera lo trova. Puoi anche caricarlo direttamente nella cartella `documents` di un argomento.

## Perché non puoi prendere il file che ho allegato in chat?

L'AI può leggere un file che alleghi in chat, ma non può salvarlo su Drive: oggi è un limite delle app. Caricalo nella cartella `landing` dall'app Drive (vedi sopra) e dillo ad Agora. Intanto Agora può leggere l'allegato e annotare cosa dice.

## Puoi tenere traccia di qualcosa che succede molte volte al giorno?

Sì. Di' "segna i pannolini di Greta in un file" (o le poppate, le spese, le medicine). Agora concorda con te un nome e le colonne e tiene un file tabella nella cartella `documents` di quell'argomento, una riga ogni volta che glielo dici ("cacca alle 9:15"), invece di un appunto ciascuna. La revisione di ogni mattina scrive un appunto di riepilogo per la giornata. Chiedi "quanti oggi?" o "quando è stato l'ultimo?" quando vuoi. Usi già un Foglio Google per questo? Dillo, e Agora lo copia una volta e lascia il tuo foglio com'è.

## Puoi fare una lista da stampare?

Sì: "fai una lista per la valigia delle vacanze", "aggiungi il latte alla lista della spesa", "scrivi un promemoria dei vaccini di Greta". Agora la tiene come file nella cartella `documents` di quell'argomento e la aggiorna ogni volta che lo chiedi. Aprila dall'app Drive per leggerla, condividerla o stamparla.

## Come tengo una lista di to-do?

Di' ad Agora: "aggiungi un to-do: rinnovare l'assicurazione dell'auto". È tuo, a meno che tu non nomini qualcun altro ("Nina deve prenotare il dentista") o dica che è per tutti ("qualcuno deve comprare il toner"). Se vuoi aggiungi una data ("entro venerdì") e le revisioni ti avvisano prima che sia tardi; di' "mettilo in calendario" se vuoi anche un evento. Chiedi "cosa ho da fare?" o "cosa c'è di aperto per tutti?", e di' "fatto con l'assicurazione" quando è fatto, o "ci penso io" per prendere in carico uno condiviso. La lista completa è in `todo.md` nella cartella `<nome della radice>`.

## Gli altri vedono i miei to-do?

Sì. Tutti in `<nome della radice>` vedono ogni to-do, compresi i tuoi, e possono chiuderlo, passarlo ad altri o cambiarlo; Agora annota chi ha fatto cosa. Tieni fuori dalla lista le cose private.

## Come tolgo i vecchi to-do?

Di' cosa togliere: "rimuovi tutto ciò che è chiuso prima di giugno", "rimuovi tutto ciò che riguarda Acme", "rimuovi i to-do fatti di Nina". Agora toglie solo i to-do finiti, a meno che tu non dica "compresi quelli aperti". Ti mostra prima l'elenco e aspetta il tuo sì. I to-do rimossi vanno nel cestino di Google Drive, dove puoi ripristinarli per 30 giorni; poi spariscono. Ogni mese, inoltre, i to-do finiti da più di tre mesi vengono messi in archivio, fuori dai piedi ma conservati.

## Che cos'è `products` e chi è pm?

`products` segue il software che il tuo gruppo costruisce su GitHub, una cartella per ogni repo collegato; uno sviluppatore collega un repo da Claude Code con il plugin `cruna-pm-github`. `pm` non è una persona: è l'indirizzo per GitHub. Scrive nelle note del prodotto cosa è stato unito, rilasciato o abbandonato, e una nota su un prodotto, o un to-do per `pm`, diventa una richiesta GitHub alla prossima sincronizzazione; poi mostra il numero della issue ("→ #8") e viene spuntato quando la issue si chiude.

## Dov'è il file del registro o della lista?

Nell'argomento a cui appartiene: `<nome della radice>` → `<interesse>` → `<istanza>` → `documents`, con un nome semplice come `greta-pannolini.csv` o `lista-valigia.md`. L'`index.md` di quell'argomento lo elenca sotto **Maintained**. Oppure chiedi ad Agora "dov'è la lista per la valigia?".

## Come cambio il modo in cui Agora gestisce qualcosa?

Diglielo, iniziando con "d'ora in poi": "d'ora in poi metti le poppate di Greta in un file", "smetti di segnare i pannolini nel file, torna a registrarli come appunti", "aggiungi una colonna per la quantità". Agora ti ripete la modifica e, al tuo sì, la scrive nella tabella **Conventions** di `org.md`, che controlla ogni volta. Puoi leggere lì quella tabella.

## Agora sa leggere e inviare email?

Quando l'email è attiva (di' "attiva l'email"; è più adatta alle aziende), Agora ha la sua casella: `<account dell'assistente>`. Inoltragli o mettilo in copia (CC) sulle email che contano, e Agora salva ciascuna nell'argomento giusto, o ti chiede dove va. Chiedi "novità da Acme?", "prepara una bozza di risposta a Mario", "archivia questa email sotto clienti/acme". Agora prepara bozze quando vuoi, ma invia solo dopo che hai visto il messaggio esatto e hai detto sì in chat; poi è la tua app a chiederti di approvare anche l'invio. Le revisioni non inviano mai nulla.

## Che cosa non sa fare Agora con l'email?

Legge solo la sua casella, mai la tua. Vede il nome di un allegato ma non può aprirlo né salvarlo: salva tu il file in `landing` e di' "archiviato", e Agora lo collega all'email. Non etichetta, non archivia in Gmail e non elimina la posta. Quando l'invio non riesce, di' "l'email non funziona" e Agora controlla la tua configurazione.

## Come decide Agora quali email sono importanti?

Di base: un'email che ti chiede di fare, rispondere o decidere qualcosa, o che porta una data, e che non è una newsletter o una notifica. Un'email dal tuo indirizzo conta sempre. Digli cos'altro conta, o non conta ("tutto ciò che arriva da un cliente è importante", "ignora le notifiche di GitHub"), oppure di' "nessuna preferenza". Ogni revisione elenca le email importanti, le bozze che aspettano il tuo sì, e cosa è stato inviato e salvato; Agora tiene in `inbox.md` un registro di cosa ha fatto con ogni email.

## Come funzionano le routine?

Ogni membro ha tre attività pianificate nella sua app di AI, create durante la configurazione:

- **Giornaliera**, al mattino: ieri in revisione e oggi in vista.
- **Settimanale**, la domenica sera: la settimana in revisione e la settimana che viene.
- **Mensile**, il primo del mese: il mese in revisione e il mese che viene.

Ogni esecuzione prima riordina la cartella (archivia ciò che è stato caricato, aggiorna i riepiloghi), poi scrive una revisione condivisa nella cartella `reviews` e ti manda un breve messaggio con le tue cose per prime. Per cambiare un orario, modifica l'attività nella pagina delle attività pianificate della tua app. Senza un piano a pagamento, chiedi "dammi la settimana che viene" quando vuoi: ricevi il riepilogo per tutti, poi qualche riga per ogni persona. Chiedi "la mia settimana" per avere solo la tua.

## Ho lasciato la configurazione a metà. Devo ricominciare?

No. Agora tiene una checklist della configurazione in `setup.md`, nella cartella `<nome della radice>`. Di' "finisci la configurazione" e riprende dalla prima cosa ancora da fare. Finché non è tutto fatto, la revisione giornaliera ti ricorda cosa manca e chi lo fa.

## Come aggiungo una persona?

Di' "aggiungi <nome> ad Agora". Agora ti guida: lo aggiunge a `org.md`, condivide con lui la cartella e il calendario e lo aiuta a collegare la sua app di AI.

## Posso modificare i file da solo?

Puoi aggiungere file e leggere tutto. Gli appunti in `diary` non vengono mai modificati: per correggerne uno, di' ad Agora cosa non va e salva una correzione. `status.md`, `index.md` e `todo.md` vengono riscritti da Agora, quindi ogni modifica che fai lì sparisce; dillo ad Agora invece. `org.md` e i file in `memories` puoi modificarli tu, se vuoi. Anche i file registro e le liste: Agora li legge prima di ogni modifica, quindi le tue modifiche restano.

## E se in due parliamo con Agora nello stesso momento?

Va bene. Ogni appunto è un file a sé, quindi nulla viene sovrascritto. Se per un attimo un riepilogo sembra non aggiornato o un file compare due volte, la prossima revisione lo sistema.

## Come correggo qualcosa che Agora ha sbagliato?

Diglielo: "è sbagliato, l'appuntamento è venerdì". Salva la correzione e aggiorna i riepiloghi e il calendario.

## Come mi fermo o esco?

- **Sospendere le revisioni**: sospendi o elimina le tue tre attività pianificate nella tua app di AI.
- **Uscire**: chiedi al proprietario, oppure di' "toglimi da Agora". Il tuo accesso alla cartella e al calendario viene rimosso e la tua app viene scollegata. Ciò che hai scritto resta, come parte della storia comune.
- **Fermare Agora del tutto**: elimina le attività pianificate di tutti e scollega i connettori di Google Drive e Calendar (e Gmail). La cartella e il calendario restano in Google, leggibili come normali file, finché non li elimini.

## Costa qualcosa?

Le revisioni pianificate richiedono un piano a pagamento su Claude o ChatGPT. Google Drive e Calendar sono gratuiti; l'account Google gratuito offre 15 GB, più che sufficienti per i documenti.
