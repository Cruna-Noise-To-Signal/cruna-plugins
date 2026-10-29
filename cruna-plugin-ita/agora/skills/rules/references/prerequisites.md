# Prerequisiti di Agora

Che cosa deve essere vero perché Agora funzioni, come verificare ogni voce e come sistemarla. `setup` percorre l'elenco dall'alto in basso (un membro che entra in una radice esistente percorre le voci 1–3, 5 e 6); `help` e qualsiasi skill che fallisce saltano alla voce che corrisponde al problema.

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

- **Requisito**: due connettori, **Google Drive** e **Google Calendar**, entrambi collegati con l'**account dell'assistente** indicato in `org.md` (o con l'account del proprietario in una radice con un solo membro). Un connettore collegato con l'account Google sbagliato è il problema più comune: l'AI vede allora un altro Drive.
- **Verifica**:
  - Drive: cerca `org.md` su Drive (oppure, prima del setup, elenca i file recenti). Se funziona, il connettore di Drive è a posto; conferma che il proprietario del file sia l'account dell'assistente.
  - Calendar: elenca i calendari. Se funziona, il connettore di Calendar è a posto; conferma che il calendario principale riporti l'email dell'account dell'assistente (`calendar` in `org.md`): è il calendario di sistema.
- **Soluzione**:
  - Claude: Impostazioni → Connettori (Settings → Connectors) → Google Drive → Connetti (Connect); lo stesso per Google Calendar. Accedi con l'account dell'assistente.
  - ChatGPT: Impostazioni → App (Settings → Apps, o Connectors) → Google Drive → Connetti (Connect); lo stesso per Google Calendar. Accedi con l'account dell'assistente.
  - Già collegato con l'account sbagliato: scollega, poi collega di nuovo con l'account dell'assistente.
  - In una conversazione, assicurati che entrambi i connettori siano attivi per la chat (il menu degli strumenti o il menu "+").

## 4. Account dell'assistente

- **Requisito** (solo radici con più membri): un account Google dedicato a questa radice, proprietario della cartella radice; il suo calendario principale è il calendario di sistema. Le radici con un solo membro saltano questa voce e usano l'account del proprietario.
- **Verifica**: le verifiche di Drive e Calendar della voce 3 restituiscono l'account dell'assistente come proprietario. Prima che `org.md` esista, elenca i calendari: il calendario principale riporta l'email dell'account collegato.
- **Soluzione**: crea un account Google gratuito (accounts.google.com → Crea un account → Per uso personale (Create account → For my personal use)), con il nome della radice (`fabiole.agora@gmail.com`). Imposta come telefono ed email di recupero quelli di un membro, così la radice non va mai persa per una password dimenticata. Gli account gratuiti hanno 15 GB tra Drive e Gmail; bastano ampiamente per i documenti, ma dillo.
- **Condividere l'accesso**: il client AI di ogni membro si collega con questo account, quindi ogni membro ha bisogno della sua password una volta, al momento del collegamento. Conservala nel gestore di password della famiglia o del team.

## 5. Condivisione

- **Requisito**:
  - La cartella radice condivisa con l'account Google personale di ogni membro come **Editor**, così possono caricare e sfogliare dal proprio Drive.
  - Il calendario di sistema (il calendario principale dell'account dell'assistente) condiviso con l'account Google personale di ogni membro con **Apporta modifiche agli eventi** (Make changes to events), così gli eventi compaiono sui loro telefoni.
  - Facoltativo: il calendario personale di ogni membro condiviso con l'account dell'assistente, con **Visualizza solo disponibilità (libero/occupato)** (See only free/busy) o **Visualizza tutti i dettagli degli eventi** (See all event details) (a scelta del membro, registrata in `org.md`).
- **Verifica**: leggi i permessi della cartella radice e conferma che ogni membro elencato in `org.md` sia editor. Elenca i calendari e conferma che ogni calendario personale condiviso sia visibile. La condivisione del calendario di sistema non si legge tramite il connettore: il membro conferma che il calendario compare sul suo telefono.
- **Soluzione**:
  - Cartella: il connettore di Drive può condividerla; fallo tu, con l'email di ogni membro come Editor, e riprova finché i permessi non li elencano. A mano, con l'account dell'assistente: in Drive, clic destro sulla cartella radice → Condividi (Share) → aggiungi l'email di ogni membro come Editor.
  - Calendario di sistema: sempre a mano, perché il connettore del calendario non può condividere calendari. Google Calendar (con l'account dell'assistente) → Impostazioni (Settings) → sotto "Impostazioni dei miei calendari" (Settings for my calendars), il nome dell'account dell'assistente → Condividi con persone specifiche o gruppi (Share with specific people or groups) → Aggiungi persone (Add people) → l'email di ogni membro, Apporta modifiche agli eventi → Invia (Send). Il membro apre l'email di invito e aggiunge il calendario.
  - Calendario personale: ogni membro, nel proprio Google Calendar → Impostazioni → il proprio calendario → Condividi con persone specifiche → aggiungi l'account dell'assistente.

## 6. Routine pianificate

- **Requisito**: tre attività pianificate per membro, ognuna esegue la skill `review`: giornaliera (al mattino), settimanale (domenica sera), mensile (il giorno 1).
- **Verifica**: chiedi al membro di confermare che le tre attività compaiano nel suo elenco delle attività pianificate. Le esecuzioni delle routine non si possono elencare da una conversazione su tutte le piattaforme.
- **Soluzione**: crea ogni attività con il prompt e l'orario indicati da `setup`:
  - Claude: apri la pagina delle attività pianificate (Pianificate (Scheduled), nella barra laterale) → Nuova attività (New task), oppure chiedi a Claude in una chat di pianificarla.
  - ChatGPT: apri Pianificate (Scheduled, o Tasks) → Nuova attività (New task), oppure chiedi a ChatGPT in una chat di pianificarla.
  - Conferma che l'attività possa usare i connettori di Drive e Calendar e il plugin Agora. Attiva le notifiche, così la revisione arriva al membro.

## Uscita di un membro

Rimuovi il suo accesso da editor alla cartella radice e al calendario di sistema (con l'account dell'assistente), chiedigli di scollegare i connettori nel suo client, cambia la password dell'account dell'assistente se la conosceva, e aggiorna le tabelle Members e Access in `org.md`. Le sue voci di diario e i suoi documenti restano: sono storia.
