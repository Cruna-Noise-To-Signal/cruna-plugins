---
name: file-document
description: Archivia un documento in Agora: gli dà un nome, lo sposta nella cartella documents dell'istanza giusta, lo collega dal diario e mette in calendario le sue date. Usala quando un membro dice di aver caricato o lasciato un file in Agora ("ho messo gli esami del sangue nella cartella salute di Nina", "c'è una fattura in landing"), allega in chat un file da conservare in Agora, ha già un foglio o un file con cui tiene traccia di qualcosa ("abbiamo un foglio per i pannolini di Greta"), o quando la routine di revisione trova file che nessuno ha annunciato.
---

# Archivia documento

**Archiviare** un documento significa dargli il suo nome Agora, metterlo nella cartella `documents/` di esattamente un'istanza, leggerlo e collegarlo da una voce di diario, così che `ask` possa ritrovarlo. I file arrivano in tre modi:

- **(a) Già al suo posto**: il membro l'ha caricato nella cartella `documents/` di un'istanza e te lo dice.
- **(b) Landing**: il membro l'ha caricato nella cartella `landing/` della radice e te lo dice.
- **(c) Non annunciato (trovato dalla revisione)**: il riordino della skill `review` ha trovato un file in `landing/`, oppure in una cartella `documents/` ma assente dall'`index.md` di quell'istanza. `review` ti passa il percorso del file, lo slug del membro, il periodo, e se la sua esecuzione è non presidiata. Restituisci le tue segnalazioni a `review` invece di scriverle, e interrompi la propagazione al livello dell'interesse (passo 8).

- **(d) Importa**: il membro tiene già traccia di qualcosa in un Foglio Google o in un suo file ("abbiamo un foglio per i pannolini di Greta"). Segui **Importa** qui sotto invece dei Passi.

I **file di lavoro** (rules → Tipi di file) non spetta a te archiviarli: lascia nome e posizione invariati per ogni file elencato sotto Maintained nell'`index.md` della sua istanza.

A parte questo, un'esecuzione è **presidiata** o **non presidiata**. I casi (a) e (b) sono sempre presidiati. Il caso (c) è presidiato quando un membro ha chiesto la revisione: chiediglielo come in (b). Senza presidio non c'è nessuno a cui chiedere: agisci solo su ciò che è inequivocabile e segnala il resto (rules → Esecuzioni non presidiate, rules → Segnalare).

Un file allegato in chat non è nessuno di questi casi: puoi leggerlo ma non puoi metterlo su Drive. Di' al membro, in una o due frasi, di aprire l'app Drive, caricare il file nella cartella `landing/` della radice (o direttamente nella cartella `documents/` dell'istanza), e poi dire "archiviato". Offri di registrare subito ciò che dice il file con la skill `log`, così niente resta in attesa del caricamento. Fermati qui finché non conferma il caricamento.

## Passi

### 1. Carica le regole, trova la radice, sappi chi sta parlando

Carica la skill `rules`. Segui rules → Trovare la radice e rules → Identità, poi leggi `memories/<slug>.md` e `memories/shared.md` (rules → Passo memoria). Nel caso (c), `review` ha già fatto tutto questo e ti passa lo slug.

Fatto quando: hai letto `org.md`, conosci lo slug del membro e hai letto le sue memorie.

### 2. Trova il file

- (a): elenca la cartella `documents/` dell'istanza e prendi i file che non compaiono né nella tabella Documents né nella tabella Maintained del suo `index.md`.
- (b): elenca `landing/`.
- (c): `review` ti passa il percorso del file.

Quando più di un file potrebbe essere quello inteso, chiedi quale, elencandone i nomi (senza presidio: archiviali uno per uno). Gestisci più file uno alla volta attraverso i passi 3–7, poi fai il passo 8 una sola volta.

Fatto quando: per ogni file hai il percorso attuale, il nome originale, la data di creazione e il proprietario dai suoi metadati Drive.

### 3. Leggilo

Leggi il contenuto con l'estrazione del testo di Drive (rules → Lavorare con Drive): copre PDF, immagini e file Office. Annota cos'è il documento, chi e cosa riguarda, e ogni data, scadenza o importo che contiene.

Quando non torna nulla di leggibile (una foto sfocata, un PDF protetto), lavora dal nome del file e dalle parole del membro, e dillo nel riepilogo.

Fatto quando: sai scrivere una riga che dica cos'è il documento e cosa dice di importante.

### 4. Collocalo

- (a): l'istanza è la cartella in cui si trova.
- (b): confronta il contenuto e le parole del membro con i file `index.md` della radice e degli interessi. Quando non rientra con certezza in un'unica istanza, chiedi, offrendo le opzioni `interesse/istanza` probabili. Quando l'istanza giusta non esiste ancora, offri la skill `add-interest`.
- (c) in una cartella `documents/`: decide la cartella. (c) in `landing/`, presidiato: come (b). (c) in `landing/`, non presidiato: collocalo solo quando il contenuto indica senza dubbio esattamente un'istanza esistente; altrimenti **segnalalo** (rules → Segnalare) con il nome del file, cosa sembra essere e le istanze probabili, restituisci la segnalazione a `review`, e fermati per questo file.

Fatto quando: il file ha esattamente un'istanza di destinazione esistente, oppure è segnalato.

### 5. Dagli un nome

Il nome Agora è `<slug>-YYYYMMDD_HHMM_<nome-originale>.<ext>`:

- **slug**: chi l'ha caricato. Confronta l'email del proprietario del file nei metadati Drive con i membri in `org.md`; quando non corrisponde a nessuno (o il file appartiene all'account dell'assistente), usa lo slug del passo 1.
- **YYYYMMDD_HHMM**: la data di creazione del file su Drive (il momento del caricamento), nel fuso orario della radice.
- **nome-originale**: il nome originale senza estensione, minuscolo, con spazi e trattini bassi trasformati in trattini, solo ASCII (`Esami Sangue Nina.PDF` → `esami-sangue-nina`). **ext**: l'estensione originale, minuscola.

Un file il cui nome segue già questo schema lo mantiene.

Fatto quando: hai il nome definitivo, e nella cartella `documents/` di destinazione non esiste un file con quel nome (se esiste, è un duplicato: confrontali, e chiedi o segnala invece di archiviare due volte).

### 6. Spostalo

Rinomina e sposta con un solo aggiornamento. Quando non riesce:

1. Copia il file nella cartella `documents/` di destinazione con il nuovo nome, poi cestina l'originale.
2. Quando non riesce nemmeno la copia, lascia il file dov'è e segnalalo: "Non sono riuscito a spostare `landing/<nome>` in `<interesse>/<istanza>/documents/`: spostalo lì dall'app Drive". Di' la stessa cosa al membro in una frase quando l'esecuzione è presidiata, e fermati per questo file.
3. Quando la copia è riuscita ma il cestino no, prosegui con la copia e segnala l'originale rimasto perché venga rimosso.

Nel caso (c), restituisci queste segnalazioni a `review` invece di scriverle.

Per (a) vale solo la rinomina.

Fatto quando: il file esiste esattamente una volta, con il nuovo nome, nella cartella `documents/` di destinazione; oppure è segnalato.

### 7. Scrivi la voce di diario e individua le date

Fissa prima il percorso della voce: dai il nome alla voce secondo rules → Struttura, con il membro del passo 1.

Date per il calendario (rules → Convenzioni del calendario): quando il documento contiene un appuntamento, una scadenza o un pagamento dovuto, chiedi una volta sola se metterli in calendario, ciascuno con data e ora, ed esegui la skill `schedule` per ogni sì, passandole l'istanza, il percorso della voce e i dettagli. Senza presidio, non creare nulla: segnala invece ogni data ("`nina/salute`: richiamo del vaccino entro il 2026-12-01, trovato in `documents/…`; lo metto in calendario?") e restituisci le segnalazioni a `review`.

Poi crea la voce dal modello Voce di diario (rules → Tipi di file → modelli):

- `documents`: il nuovo percorso, `documents/<nuovo nome>`.
- `events`: i titoli restituiti da `schedule`.
- **Summary**: una riga su cosa dice il documento ("Esami del sangue: tutti i valori nella norma").
- **Entry**: le parole del membro sul file, poi `Archiviato: documents/<nuovo nome>`. Caso (c): `Archiviato dalla revisione <periodo>; nessuno ha annunciato questo file.`

Fatto quando: la voce esiste al suo percorso e collega il documento, e ogni data del documento è in calendario, rifiutata o segnalata.

### 8. Aggiorna indici e stati

Per ogni istanza di destinazione, ricostruisci il suo `index.md` con il nuovo documento e la nuova voce in cima (rules → Sostituire). Poi propaga (rules → Regola di aggiornamento dello stato): lo `status.md` di ogni istanza, quello di ogni interesse toccato una volta, quello della radice una volta. Nel caso (c), fermati al livello dell'interesse: `review` ricostruisce una sola volta lo `status.md` della radice, con le segnalazioni che le restituisci.

Fatto quando: l'indice di ogni istanza toccata elenca il documento e la sua voce, e ogni stato fino alla radice (caso (c): fino all'interesse) riflette ciò che dice il documento.

### 9. Passo memoria

Solo esecuzioni presidiate: segui rules → Passo memoria.

Fatto quando: ciò che di duraturo hai imparato è in `memories/`, oppure non hai trovato nulla di nuovo, oppure l'esecuzione è non presidiata.

## Importa

Porta in Agora, una volta sola, un foglio esistente come file di lavoro di tipo registro. Sempre presidiata.

1. **Carica e identifica**: il passo 1 qui sopra.
2. **Trova il foglio**: cerca su Drive con il nome che ti dà il membro, oppure elenca `landing/` e la cartella `documents/` dell'istanza quando ha caricato un file. Quando più di uno potrebbe essere quello giusto, chiedi quale.
3. **Leggilo**: scarica un Foglio Google esportato come CSV (una scheda per download; chiedi quale scheda quando ce ne sono diverse); scarica un CSV così com'è; leggi un foglio di calcolo Office tramite l'estrazione del testo (rules → Lavorare con Drive). Annota le colonne, quante righe ci sono, e la prima e l'ultima data.
4. **Collocalo** in un'istanza, come al passo 4 (b).
5. **Mappa le colonne** su `date,time,author` più le altre, e proponi la mappatura in una sola domanda con una risposta consigliata ("Data → date, Ora → time, Tipo → type, tutto il resto → notes; author: simona, che teneva il foglio?"). Le righe senza autore prendono lo slug del membro che teneva il foglio.
6. **Crea il registro** con la procedura dei file di lavoro della skill `log` (il suo `references/working-files.md` → Crea), passandole l'istanza, il nome e le colonne concordati, e le righe importate dalla più vecchia, ancora senza Convenzione.
7. **Lascia intatto l'originale**. Un Foglio Google altrove su Drive resta dov'è. Un file caricato in `landing/` o `documents/` viene archiviato come documento caricato attraverso i passi 5–8, così l'indice mostra da dove viene il registro.
8. **Proponi la Convenzione** (rules → Convenzioni → Suggerire): d'ora in poi, ogni evento va nel registro come riga. Al sì, la scrive la skill `log` (`references/working-files.md` → Cambia una Convenzione).

Fatto quando: il registro esiste una sola volta con ogni riga dell'originale, mappata come concordato; l'originale è invariato (o archiviato, quando era stato caricato in Agora); e la Convenzione è stata proposta.

## Output

Casi (a) e (b), un solo messaggio breve nella lingua della radice:

- Per ogni file: dove è andato (`interesse/istanza`) e il riepilogo in una riga del suo contenuto.
- Modifiche al calendario, una riga ciascuna, se ce ne sono.
- Ciò che richiede il membro: un file che non sei riuscito a spostare e cosa fare nell'app Drive, una domanda ancora aperta.
- La riga di `remember`, se c'è.

Importa: quante righe sono entrate e da quali date, dove si trova il registro, che il foglio originale è intatto e può essere dismesso, e la domanda sulla Convenzione.

Caso (c): nessun messaggio tuo; restituisci a `review` i file archiviati (con destinazione e riepilogo) e le segnalazioni sollevate.
