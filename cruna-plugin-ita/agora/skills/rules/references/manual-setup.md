# Servizi condivisi: la configurazione manuale

I clic che servono a una radice in modalità servizi condivisi, per l'amministratore e per ogni membro, all'ingresso e all'uscita. Nessun connettore crea Drive condivisi, gruppi o calendari, né condivide calendari, quindi ogni passo qui si fa a mano, dalla persona che indica; Agora verifica ciascuno con una chiamata reale, e la verifica della voce 3 o 7 che un passo indica è ciò che lo rende fatto. Accompagna le persone una persona e un passo alla volta. La voce 4 dei prerequisiti rimanda qui: "voce 4, passo N" vuol dire il passo N qui sotto. Anche qui vale la nota "i nomi dei menu cambiano" all'inizio dei prerequisiti.

## Una volta per radice

In quest'ordine:

1. **Account Workspace per ogni membro** (amministratore). admin.google.com → **Directory** → **Utenti** (Users) → **Aggiungi nuovo utente** (Add new user) → nome, cognome, email principale → **Aggiungi nuovo utente**. Verifica: il membro accede con questo account e la verifica Chi della voce 3 lo mostra.
2. **Drive condiviso** (amministratore, o un membro autorizzato a creare Drive condivisi). drive.google.com → **Drive condivisi** (Shared drives) (pannello a sinistra) → **Nuovo** (New) → dagli il nome della radice → **Crea** (Create). Aprilo → **Gestisci membri** (Manage members) → l'email Workspace di ogni membro → ruolo **Gestore contenuti** (Content manager) (il ruolo più basso che può spostare nel cestino) → **Invia** (Send). Poi copia l'URL della barra degli indirizzi (`https://drive.google.com/drive/folders/<id>`) per la configurazione. Verifica: la verifica di Drive della voce 3, nella cartella principale del drive.
3. **Gruppo Google** (amministratore). Obiettivo: un gruppo che solo i suoi membri possono vedere e a cui solo loro possono unirsi, ma a cui chiunque, clienti compresi, può scrivere.
   1. **Fai accettare ai gruppi la posta dall'esterno** (una volta per organizzazione). Console di amministrazione (Admin console) → **App** (Apps) → **Google Workspace** → **Gruppi per le aziende** (Groups for Business) → **Impostazioni di condivisione** (Sharing settings) → sotto "Gli utenti esterni possono partecipare ai gruppi?" (Can external users participate in groups?), spunta **I proprietari dei gruppi possono consentire le email in arrivo dall'esterno dell'organizzazione** (Group owners can allow incoming email from outside the organization) → **Salva** (Save). Lascia **Accesso ai gruppi dall'esterno dell'organizzazione** (Accessing groups from outside this organization) su **Privato** (Private). Senza la spunta, il passo 3.2 non ha nessuna pubblicazione dall'esterno da offrire e la posta dei clienti non raggiunge mai il gruppo.
   2. **Crea il gruppo.** Console di amministrazione → **Directory** → **Gruppi** (Groups) → **Crea gruppo** (Create group) → **Nome del gruppo** (Group name) (il nome della radice), **Email del gruppo** (Group email) (l'indirizzo a cui i membri inoltrano e mettono in copia, es. `agora@acme.it`), **Proprietari del gruppo** (Group owners): l'amministratore → **Avanti** (Next) → **Etichette** (Labels): **Mailing** spuntata (il valore predefinito), **Sicurezza** (Security) non spuntata → **Avanti** → **Tipo di accesso** (Access type): **Limitato** (Restricted) → nella tabella delle impostazioni di accesso, riga **Chi può pubblicare** (Who can post), spunta anche **Esterno** (External) → **Chi può unirsi al gruppo** (Who can join the group): **Solo utenti invitati** (Only invited users) → **Consenti membri esterni al gruppo** (Allow external members in the group): disattivato → **Avanti** → **Crea gruppo**. Spuntare Esterno cambia il tipo di accesso in **Personalizzato** (Custom): è previsto ed è lo stato finale giusto, e le altre impostazioni di Limitato (solo i membri vedono le conversazioni e l'elenco dei membri) restano. Nessuna casella **Esterno** da spuntare: il passo 3.1 non è ancora salvato, o non ha ancora raggiunto il gruppo (servono fino a 24 ore). La stessa impostazione, vista da groups.google.com → il gruppo → **Impostazioni del gruppo** (Group settings), si legge **Chi può pubblicare**: **Chiunque sul Web** (Anyone on the web); impostala lì quando il gruppo è stato creato lì.
   3. **Aggiungi i membri.** groups.google.com → il gruppo → **Membri** (Members) → **Aggiungi membri** (Add members) → l'email Workspace di ogni membro → **Iscrizione** (Subscription): **Ogni email** (Each email) → **Aggiungi direttamente i membri** (Directly add members) attivato → **Aggiungi membri**. Aspetta qualche minuto prima della prima email.
   Verifica: la verifica della voce 7 in modalità servizi condivisi, la cui prova arriva dall'esterno dell'organizzazione.
4. **Calendario condiviso** (idealmente l'amministratore; chi lo crea viene registrato come `calendar_owner`, e la sua uscita aspetta un trasferimento). calendar.google.com, con l'accesso al suo account Workspace → accanto ad **Altri calendari** (Other calendars), **+** → **Crea nuovo calendario** (Create new calendar) → dagli il nome della radice, **Fuso orario** (Time zone) quello della radice → **Crea calendario** (Create calendar). Poi **Impostazioni** (Settings) → sotto "Impostazioni dei miei calendari" (Settings for my calendars), il nuovo calendario → **Condividi con persone o gruppi specifici** (Share with specific people or groups) → **Aggiungi persone e gruppi** (Add people and groups) → l'email Workspace di ogni membro, permesso **Apporta modifiche agli eventi** (Make changes to events) (nelle schermate più recenti, **Apporta modifiche e visualizza i dettagli degli eventi** (Make changes and see event details)) → **Invia** (Send). Poi **Integra calendario** (Integrate calendar) → copia l'**ID calendario** (Calendar ID) (`…@group.calendar.google.com`) per la configurazione. Verifica: la verifica di Calendar della voce 3.
5. **Connettori abilitati per l'organizzazione** (l'amministratore del client AI), quando il piano ne ha uno:
   - Claude Team o Enterprise: un Owner o un Primary Owner attiva Google Drive, Google Calendar e Gmail in **Impostazioni dell'organizzazione** (Organization settings) → **Connettori** (Connectors).
   - ChatGPT Business: un amministratore del workspace abilita le app Google Drive, Google Calendar e Gmail in **Impostazioni del workspace** (Workspace settings) → **App** (Apps), e attiva le azioni di scrittura per Gmail.
   Verifica: ogni membro trova i tre connettori nel proprio elenco (voce 3).

## Per ogni nuovo membro, prima che si unisca

L'amministratore, e il proprietario del calendario per il calendario. Ogni passo spunta la casella con lo stesso nome nella sezione del membro in `setup.md` una volta che passa la verifica fatta dal membro che si unisce.

1. Passo 1: il suo account Workspace. Casella: Workspace account.
2. Passo 2: aggiungilo al Drive condiviso (**Gestisci membri**) come **Gestore contenuti**. Casella: In the shared drive.
3. Passo 3.3: aggiungilo al gruppo, iscrizione **Ogni email**. Casella: In the group.
4. Passo 4: il proprietario del calendario condivide il calendario con lui con **Apporta modifiche agli eventi**. Casella: Shared calendar.
5. Passo 5, quando il suo piano AI ha un amministratore dell'organizzazione e i connettori non sono ancora attivi.

## Il membro che si unisce

Dopo i passi dell'amministratore, nella sua app AI con l'accesso al suo account Workspace:

1. Installa il plugin Agora (voce 2).
2. Collega Google Drive, Google Calendar e Gmail, ciascuno con l'accesso al suo account Workspace (voce 3; Gmail solo quando la radice ha l'email attiva, voce 7).
3. Accetta l'email di invito del calendario condiviso, così il calendario compare sul suo telefono.
4. Di' "unisciti ad Agora". La sua AI verifica ogni passo dell'amministratore con una chiamata reale e, quando uno fallisce, indica il passo da rifare e chi lo fa.

## Un membro che esce

1. **Prima il proprietario del calendario.** Quando il membro è `calendar_owner`, fermati qui finché il calendario non è trasferito: un calendario secondario viene eliminato insieme all'account del suo proprietario. Il membro lo trasferisce a un altro membro del dominio: Google Calendar → **Impostazioni** → il calendario → **Condiviso con** (Shared with) → il menu a discesa accanto al nuovo proprietario → **Trasferisci la proprietà** (Transfer ownership) → conferma; il nuovo proprietario accetta dall'email entro 60 giorni. Quando il membro non può, lo trasferisce l'amministratore nella Console di amministrazione. Poi imposta `calendar_owner` in `org.md` allo slug del nuovo proprietario (rules → Sostituire). Verifica: il nuovo proprietario conferma che il calendario è sotto i suoi "I miei calendari" (My calendars), e la verifica di Calendar della voce 3 dei prerequisiti passa ancora.
2. **Toglilo dai servizi condivisi** (amministratore; il proprietario del calendario per il calendario):
   - Drive condiviso: aprilo → **Gestisci membri** → accanto a lui, il menu a discesa del ruolo → **Rimuovi accesso** (Remove access);
   - gruppo: groups.google.com → il gruppo → **Membri** → spuntalo → **Rimuovi membro** (Remove member);
   - calendario: **Impostazioni** → il calendario → **Condiviso con** → **×** accanto a lui.
3. **Il suo account Workspace** (amministratore): sospendilo o eliminalo, a scelta dell'amministratore: Console di amministrazione → **Directory** → **Utenti** → il suo nome → **Sospendi utente** (Suspend user) o **Elimina utente** (Delete user). I file nel Drive condiviso appartengono al drive e restano.
4. Chiedigli, se è raggiungibile, di scollegare i connettori nel suo client e di eliminare le sue routine Agora.
5. Aggiorna le tabelle Members e Access in `org.md`.
