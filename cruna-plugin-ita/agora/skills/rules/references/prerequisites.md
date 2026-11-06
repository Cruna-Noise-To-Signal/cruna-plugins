# Prerequisiti di Agora

Che cosa deve essere vero perché Agora funzioni, come verificare ogni voce e come sistemarla. `setup` percorre l'elenco dall'alto in basso (un membro che entra in una radice esistente percorre le voci 1–3, 5 e 6; in modalità servizi condivisi, una volta fatti i passi della voce 4 che lo riguardano), e la voce 7 solo in una radice con l'email attiva (`email: on` in `org.md`); `help` e qualsiasi skill che fallisce saltano alla voce che corrisponde al problema.

**Due modalità** (rules → Vocabolario → Modalità): una voce che non nomina una modalità vale per entrambe; altrimenti segui la variante per `mode` in `org.md` (se manca: modalità base), oppure, prima che `org.md` esista, per la modalità scelta dalla configurazione.

**Verifica con una chiamata reale** ovunque ne esista una. Ogni voce finisce **pronta** (la verifica è passata) o **bloccata** (è fallita, la soluzione è stata indicata e si attende l'utente). Quando una chiamata può confermare una voce, considerala pronta solo dopo la conferma della chiamata, mai sulla sola parola dell'utente. Bloccata vale solo per una soluzione che l'utente deve applicare a mano (clic, una password, un piano); una soluzione alla portata dei connettori spetta a te: riprova la chiamata fallita finché la verifica non passa.

I nomi dei menu cambiano tra una versione e l'altra delle app. Se un percorso qui sotto non corrisponde a ciò che vede l'utente, digli che cosa cercare (la parola "Connettori", "App" o "Pianificate", o il loro equivalente inglese "Connectors", "Apps", "Scheduled") invece di insistere sul percorso esatto.

## 1. Client AI e piano

- **Requisito**: Claude (web, desktop o mobile) o ChatGPT (web, desktop o mobile) con un piano a pagamento. Le routine pianificate richiedono un piano a pagamento su entrambi.
- **Verifica**: chiedi quale app e quale piano usa il membro. È l'unica voce che solo l'utente può confermare.
- **Soluzione**: passa a un piano a pagamento, oppure continua senza routine e registralo in `org.md` (nella riga Access del membro, colonna Notes), così le revisioni si possono eseguire a mano.

## 2. Plugin Agora installato

- **Requisito**: il plugin Agora attivo nel client del membro.
- **Verifica**: stai leggendo questo testo, quindi è installato in questo client. Per gli altri membri, lo confermano quando eseguono `setup` loro stessi.
- **Soluzione**: Claude: Personalizza → Plugin (Customize → Plugins), oppure Impostazioni → Plugin (Settings → Plugins), aggiungi il marketplace `Cruna-Noise-To-Signal/cruna-plugins`, installa `agora-ita`. ChatGPT: Plugin (Plugins) → installa `agora-ita` (potrebbe servire che un amministratore del workspace lo pubblichi prima).

## 3. Connettori

Un **connettore** è un permesso che consente all'app AI di agire per tuo conto dentro un altro servizio: qui, leggere e scrivere file in Google Drive ed eventi in Google Calendar. Senza connettori l'AI può solo parlare; con i connettori può archiviare e pianificare.

### Modalità base

- **Requisito**: due connettori, **Google Drive** e **Google Calendar**, più **Gmail** quando l'email è attiva (voce 7), tutti collegati con l'**account dell'assistente** indicato in `org.md` (o con l'account del proprietario in una radice con un solo membro). Un connettore collegato con l'account Google sbagliato è il problema più comune: l'AI vede allora un altro Drive.
- **Verifica**:
  - Drive: cerca `org.md` su Drive (oppure, prima del setup, elenca i file recenti). Se funziona, il connettore di Drive è a posto. Poi leggi il proprietario di `org.md` dai suoi metadati e conferma che sia l'account connesso (rules → Vocabolario): in Il mio Drive (My Drive) solo il proprietario può spostare nel cestino, quindi qualsiasi altro account rompe ogni Sostituire. Una mancata corrispondenza è **bloccata**.
  - Calendar: elenca i calendari. Se funziona, il connettore di Calendar è a posto; conferma che il calendario principale riporti l'email dell'account dell'assistente (`calendar` in `org.md`): è il calendario di sistema.
- **Soluzione**:
  - Claude: Impostazioni → Connettori (Settings → Connectors) → Google Drive → Connetti (Connect); lo stesso per Google Calendar. Accedi con l'account dell'assistente.
  - ChatGPT: Impostazioni → App (Settings → Apps, o Connectors) → Google Drive → Connetti (Connect); lo stesso per Google Calendar. Accedi con l'account dell'assistente.
  - Già collegato con l'account sbagliato: scollega, poi collega di nuovo con l'account dell'assistente.
  - In una conversazione, assicurati che entrambi i connettori siano attivi per la chat (il menu degli strumenti o il menu "+").
  - `org.md` di proprietà di un altro account mentre i connettori sono collegati con l'account dell'assistente (una migrazione che ha spostato solo la cartella): quel proprietario, in Google Drive, seleziona i file della radice → Condividi (Share) → accanto all'account dell'assistente, **Trasferisci la proprietà** (Transfer ownership); l'account dell'assistente accetta. Oppure, quando il gruppo ha Google Workspace, sposta la radice su un Drive condiviso (modalità servizi condivisi, Sposta su Drive condiviso della skill `setup`), dove ogni Gestore contenuti (Content manager) può spostare nel cestino.

### Modalità servizi condivisi

- **Requisito**: gli stessi connettori, ciascuno con l'accesso all'**account Workspace del membro**, l'Email nella sua riga di Members. Nessuno condivide una password.
- **Verifica**, nell'app del membro:
  - Chi: elenca i calendari; l'id del calendario principale è l'account connesso. Deve essere l'account Workspace del membro (prima che sia in Members: l'account che gli ha dato l'amministratore).
  - Drive: crea un piccolo file di prova sotto `root_id` (prima che la radice esista: nella cartella principale del Drive condiviso, `drive_id`), poi spostalo nel cestino. Entrambe le chiamate riescono: pronta. Non trovato: il membro non è nel Drive condiviso; spostamento nel cestino rifiutato: non è Gestore contenuti (voce 4, passo 2).
  - Calendar: crea un evento di prova su `calendar` (con il suo id, rules → I tre id), poi eliminalo. Non trovato o rifiutato: il calendario non è condiviso con lui con **Apporta modifiche agli eventi** (Make changes to events) (voce 4, passo 4).
- **Soluzione**: i percorsi della modalità base qui sopra, con l'accesso all'account Workspace del membro. Gmail, Google Drive o Google Calendar mancano dall'elenco dei connettori: l'organizzazione non li ha abilitati (voce 4, passo 5).

## 4. Account dell'assistente, o la configurazione dell'amministratore

### Modalità base: account dell'assistente

- **Requisito** (solo radici con più membri): un account Google dedicato a questa radice, proprietario della cartella radice; il suo calendario principale è il calendario di sistema. Le radici con un solo membro saltano questa voce e usano l'account del proprietario.
- **Verifica**: le verifiche di Drive e Calendar della voce 3 restituiscono l'account dell'assistente come proprietario. Prima che `org.md` esista, elenca i calendari: il calendario principale riporta l'email dell'account collegato.
- **Soluzione**: crea un account Google gratuito (accounts.google.com → Crea un account → Per uso personale (Create account → For my personal use)), con il nome della radice (`fabiole.agora@gmail.com`). Imposta come telefono ed email di recupero quelli di un membro, così la radice non va mai persa per una password dimenticata. Gli account gratuiti hanno 15 GB tra Drive e Gmail; bastano ampiamente per i documenti, ma dillo.
- **Condividere l'accesso**: il client AI di ogni membro si collega con questo account, quindi ogni membro ha bisogno della sua password una volta, al momento del collegamento. Conservala nel gestore di password della famiglia o del team.

### Modalità servizi condivisi: la configurazione dell'amministratore

- **Requisito**: l'amministratore Google Workspace dell'organizzazione ha creato il Drive condiviso, il gruppo e il calendario condiviso, e vi ha aggiunto ogni membro: i passi in [manual-setup.md](manual-setup.md), la guida alla configurazione manuale per l'amministratore e per ogni membro, all'ingresso e all'uscita. "Voce 4, passo N" vuol dire il suo passo N.
- **Verifica**: la verifica che ciascuno dei suoi passi indica. Pronta quando la verifica di ogni passo è passata, per la radice (Una volta per radice) o per il membro che si unisce (Per ogni nuovo membro, prima che si unisca).
- **Soluzione**: il passo che fallisce, fatto dalla persona che indica.

## 5. Condivisione

### Modalità base

- **Requisito**:
  - La cartella radice condivisa con l'account Google personale di ogni membro come **Editor**, così possono caricare e sfogliare dal proprio Drive.
  - Il calendario di sistema (il calendario principale dell'account dell'assistente) condiviso con l'account Google personale di ogni membro con **Apporta modifiche agli eventi** (Make changes to events), così gli eventi compaiono sui loro telefoni.
  - Facoltativo: il calendario personale di ogni membro condiviso con l'account dell'assistente, con **Visualizza solo disponibilità (libero/occupato)** (See only free/busy) o **Visualizza tutti i dettagli degli eventi** (See all event details) (a scelta del membro, registrata in `org.md`).
- **Verifica**: leggi i permessi della cartella radice e conferma che ogni membro con un'email in `org.md` sia editor. Elenca i calendari e conferma che ogni calendario personale condiviso sia visibile. La condivisione del calendario di sistema non si legge tramite il connettore: il membro conferma che il calendario compare sul suo telefono.
- **Soluzione**:
  - Cartella: il connettore di Drive può condividerla; fallo tu, con l'email di ogni membro come Editor, e riprova finché i permessi non li elencano. A mano, con l'account dell'assistente: in Drive, clic destro sulla cartella radice → Condividi (Share) → aggiungi l'email di ogni membro come Editor.
  - Calendario di sistema: sempre a mano, perché il connettore del calendario non può condividere calendari. Google Calendar (con l'account dell'assistente) → Impostazioni (Settings) → sotto "Impostazioni dei miei calendari" (Settings for my calendars), il nome dell'account dell'assistente → Condividi con persone specifiche o gruppi (Share with specific people or groups) → Aggiungi persone (Add people) → l'email di ogni membro, Apporta modifiche agli eventi → Invia (Send). Il membro apre l'email di invito e aggiunge il calendario.
  - Calendario personale: ogni membro, nel proprio Google Calendar → Impostazioni → il proprio calendario → Condividi con persone specifiche → aggiungi l'account dell'assistente.

### Modalità servizi condivisi

La condivisione è l'appartenenza; in questa modalità il connettore di Drive non condivide mai la cartella radice.

- **Requisito**:
  - Ogni membro **Gestore contenuti** del Drive condiviso (voce 4, passo 2), al posto della condivisione della cartella radice.
  - Il calendario condiviso condiviso con ogni membro con **Apporta modifiche agli eventi**, dal proprietario del calendario (voce 4, passo 4).
  - Calendari personali: il libero/occupato di Workspace su tutto il dominio basta per trovare il tempo libero (Personal calendar `libero/occupato del dominio`). Un membro che vuole più dettagli condivide il suo calendario con gli altri: Google Calendar → Impostazioni → il suo calendario → Condividi con persone o gruppi specifici → ogni membro, **Visualizza tutti i dettagli degli eventi** (See all event details) (Personal calendar `tutti i dettagli`).
- **Verifica**: le verifiche di Drive e Calendar della voce 3 in modalità servizi condivisi, nell'app del membro. Registra ciò che hanno dimostrato nella riga di Access: Drive `gestore contenuti`, System calendar `può modificare`.
- **Soluzione**: il passo della voce 4 indicato dalla verifica che fallisce, fatto dalla persona che indica.

## 6. Routine pianificate

- **Requisito**: tre attività pianificate per membro, ognuna esegue la skill `review`: giornaliera (al mattino), settimanale (domenica sera), mensile (il giorno 1).
- **Verifica**: chiedi al membro di confermare che le tre attività compaiano nel suo elenco delle attività pianificate. Le esecuzioni delle routine non si possono elencare da una conversazione su tutte le piattaforme.
- **Soluzione**: crea ogni attività con il prompt e l'orario indicati da `setup`:
  - Claude: apri la pagina delle attività pianificate (Pianificate (Scheduled), nella barra laterale) → Nuova attività (New task), oppure chiedi a Claude in una chat di pianificarla.
  - ChatGPT: apri Pianificate (Scheduled, o Tasks) → Nuova attività (New task), oppure chiedi a ChatGPT in una chat di pianificarla.
  - Conferma che l'attività possa usare i connettori di Drive e Calendar e il plugin Agora. Attiva le notifiche, così la revisione arriva al membro.

## 7. Connettore Gmail (solo con l'email attiva)

Facoltativo: serve solo a una radice con `email: on`, ed è più adatto alle radici di lavoro. Una radice con l'email disattivata salta questa voce.

### Modalità base

- **Requisito**: il connettore **Gmail**, collegato con l'**account dell'assistente**, così l'AI legge la casella dell'assistente e la posta personale di nessuno. In una radice solitaria è l'account del proprietario.
- **Verifica**:
  1. Cerca nella casella con una chiamata reale (gli ultimi messaggi).
  2. Conferma che la casella sia quella dell'account dell'assistente: dai dettagli dell'account o del profilo del connettore quando li offre, altrimenti dall'indirizzo del destinatario di un messaggio ricevuto. Una casella nuova e vuota: chiedi a un membro di mandare un'email di prova all'indirizzo dell'assistente, poi cercala.
  La verifica passa solo quando l'indirizzo corrisponde ad `assistant_account` in `org.md`.
- **Soluzione**:
  - Claude: Impostazioni → Connettori (Settings → Connectors) → Gmail → Connetti (Connect), con l'accesso all'account dell'assistente. Su un piano Team o Enterprise, un Owner deve prima abilitare il connettore Gmail per l'organizzazione.
  - ChatGPT: Impostazioni → App (Settings → Apps, o Connectors) → Gmail → Connetti (Connect), con l'accesso all'account dell'assistente. L'invio funziona solo sul web, con un piano a pagamento (Plus, Pro, Business, Enterprise); su Business, un amministratore del workspace deve attivare le azioni di scrittura per Gmail. Lettura e bozze funzionano ovunque funzioni il connettore.
  - Già collegato con l'account sbagliato: scollega, poi collega di nuovo con l'account dell'assistente.
  - In una conversazione, assicurati che Gmail sia attivo per la chat (il menu degli strumenti o il menu "+").
- **Limiti** da dire al membro: l'AI vede solo la casella dell'assistente; non può aprire gli allegati, solo leggerne i nomi; non etichetta, non archivia e non elimina la posta.

### Modalità servizi condivisi

- **Requisito**: il connettore **Gmail**, con l'accesso all'account Workspace del membro, e il membro nel gruppo con la consegna **Ogni email** (Each email), così la posta del gruppo arriva nella sua casella.
- **Verifica**: cerca `deliveredto:<inbox>` (`inbox` in `org.md`, l'indirizzo del gruppo) con una chiamata reale; deve restituire risultati. Ancora nessuno (un gruppo nuovo): chiedi al membro di mandare un'email di prova all'indirizzo del gruppo da un indirizzo esterno all'organizzazione (un Gmail personale, per esempio), così la prova dimostra anche che i mittenti esterni raggiungono il gruppo; aspetta un minuto e cerca di nuovo. Ancora nessuno: il membro non è nel gruppo, la sua consegna non è Ogni email, oppure la pubblicazione dall'esterno è disattivata (voce 4, passi 3.1 e 3.2). Se passa, scrivi `riceve la posta` nelle Notes della sua riga di Access.
- **Soluzione**: i percorsi della modalità base qui sopra, con l'accesso all'account del membro; Gmail manca dall'elenco dei connettori: voce 4, passo 5.
- **Limiti** da dire al membro: il connettore potrebbe raggiungere tutta la sua casella di posta, ma Agora legge solo la posta consegnata al gruppo, per regola propria (rules → I tre id); non può aprire gli allegati, solo leggerne i nomi; non etichetta, non archivia e non elimina la posta.

## Uscita di un membro

Le sue voci di diario e i suoi documenti restano in entrambe le modalità: sono storia.

### Modalità base

Rimuovi il suo accesso da editor alla cartella radice e al calendario di sistema (con l'account dell'assistente), chiedigli di scollegare i connettori (anche Gmail, quando l'email è attiva) nel suo client, cambia la password dell'account dell'assistente se la conosceva, e aggiorna le tabelle Members e Access in `org.md`.

### Modalità servizi condivisi

Segui [manual-setup.md](manual-setup.md) → Un membro che esce: il trasferimento del proprietario del calendario viene per primo, e blocca il resto finché `calendar_owner` non indica il nuovo proprietario.
