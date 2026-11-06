# Configurazione in modalità servizi condivisi

Che cosa fa ogni passo della configurazione quando la radice è in modalità servizi condivisi (`mode: shared-services` in `org.md`, rules → Vocabolario → Modalità), o quando Nuova radice l'ha scelta. Ogni sezione qui sotto si aggiunge al passo con lo stesso numero in SKILL.md, o sostituisce i passi che indica. Niente qui condivide una cartella o un calendario: in questa modalità la condivisione è l'appartenenza, fatta a mano dall'amministratore e dal proprietario del calendario, e Agora si limita a controllarla. I passi manuali sono il file `references/manual-setup.md` della skill `rules`; "voce 4, passo N" indica il suo passo N.

## 2a. Prerequisiti (Nuova radice)

Al posto dei passi della modalità base di SKILL.md.

1. Di' all'utente, in due frasi semplici, che cosa si fa a mano: l'amministratore Workspace crea il Drive condiviso, il gruppo e il calendario condiviso e vi aggiunge ogni membro, seguendo i passi che gli dai; tu controlli ognuno con una chiamata reale, e Agora non condivide mai niente da sé.
2. Percorri la voce 1, la voce 2, poi la voce 4 (Una volta per radice). Poi la voce 3 (servizi condivisi): il suo controllo Chi dà l'account Workspace dell'utente, che diventa la sua Email in Members; i suoi controlli Drive e Calendar sono i passi 3 e 4, una volta che hai gli id.
3. **Drive condiviso.** Chiedi l'URL del Drive condiviso (voce 4, passo 2). Ricava tu l'id: il segmento del percorso dopo `/folders/`, senza l'eventuale query `?…`. Controllalo con una chiamata reale: crea un file di prova in quella cartella, poi cestinalo. Non trovato o rifiutato: l'utente non è Gestore contenuti (Content manager) di quel Drive, oppure l'URL è di un'altra cartella; percorri di nuovo la voce 4, passo 2.
4. **Calendario condiviso.** Chiedi l'ID calendario (Calendar ID) (voce 4, passo 4) e chi ha creato il calendario (il suo slug, quando il giro Membri lo stabilisce, è `calendar_owner`). Controllalo con una chiamata reale: crea un evento di prova su quell'id, poi eliminalo. Agora non crea mai questo calendario.
5. **Gruppo.** Chiedi l'indirizzo del gruppo (voce 4, passo 3).

Tieni l'id del Drive, l'ID calendario, chi ha creato il calendario e l'indirizzo del gruppo per il giro della radice: vanno in `setup.md` → Interview → The root, e più avanti in `org.md` come `drive_id`, `calendar`, `calendar_owner` e `inbox`.

Fatto quando le voci 1–4 sono tutte pronte, i controlli crea e poi cestina del Drive e crea e poi elimina del calendario sono passati, e hai i quattro valori qui sopra.

## 2b. Intervista

- La cartella radice si crea dentro il Drive condiviso, con `drive_id` come cartella madre; il suo id diventa `root_id`.
- Ogni sezione Member riceve le caselle dei servizi condivisi del modello (rules → Tipi di file → modelli). Anche le caselle Workspace account, In the shared drive e Shared calendar del proprietario sono spuntate, perché i controlli del passo 2a sono passati nella sua app. Le caselle dell'amministratore di ogni altro membro restano non spuntate, ognuna con il nome di chi la fa (voce 4, Per ogni nuovo membro, prima che si unisca).

## 2c. Costruzione

- Elemento 2, `org.md`: il front-matter ha anche `mode: shared-services`, `drive_id`, `root_id`, `calendar` (l'ID calendario), `calendar_owner` e `inbox` (l'indirizzo del gruppo), e niente `assistant_account`.
- Elemento 7, calendario di sistema: l'ID calendario del passo 2a, creato a mano e mai da Agora. Conferma nella lista dei calendari che il suo fuso orario sia quello di `org.md`; un fuso diverso si corregge con un clic del proprietario del calendario: **Impostazioni** (Settings) → il calendario → **Fuso orario** (Time zone).

## 2e. Condivisione

Al posto dei passi 1–3 di SKILL.md. La voce 5 è fatta solo di controlli, e i controlli di ogni membro si eseguono nella sua app. Quelli del proprietario sono passati al passo 2a: scrivi `gestore contenuti` e `può modificare` nella sua riga Access. Le caselle dell'amministratore di ogni altro membro restano non spuntate, con il nome di chi fa ciascuna, finché non esegue Unisciti.

Fatto quando la riga Access del proprietario mostra ciò che i suoi controlli hanno provato, e le caselle dell'amministratore di ogni altro membro nominano chi le fa.

## 3. Unisciti

Al posto dei passi 1–6 di Unisciti in SKILL.md. Eseguilo nell'app del membro che entra, con l'accesso come suo account Workspace. Unisciti non condivide nulla e non crea cartelle di interessi, con qualsiasi struttura.

1. **Prerequisiti e chi.** Percorri le voci 1 e 2. Elenca i calendari: l'id del calendario principale è l'account connesso (voce 3, Chi). Trova la radice (rules → Trovare la radice). Non trovata: o i connettori hanno l'accesso con un altro account (voce 3), o l'amministratore non ha ancora aggiunto questo account al Drive condiviso (voce 4, passo 2, l'amministratore): di' entrambe le cose, con chi fa ciascuna, e fermati qui.
2. **Quale ingresso.** Leggi `org.md` e confronta l'account connesso con la colonna Email di Members (rules → Identità):
   - nessuna corrispondenza: un **nuovo membro**;
   - una corrispondenza la cui sezione Member in `setup.md` ha caselle non spuntate: un membro nominato in Nuova radice, che entra ora; salta le domande dell'intervista a cui la sua riga risponde già;
   - una corrispondenza la cui sezione è tutta spuntata: un **membro esistente su una nuova app**.
   Un nuovo membro riceve ora una sezione Member, con le caselle dei servizi condivisi (rules → Tipi di file → modelli), tutte non spuntate tranne Connectors and plugin; un membro esistente su una nuova app ha Connectors and plugin spuntata e Routines non spuntata (ed Email, quando l'email è attiva).
3. **Controlla il lavoro dell'amministratore.** Esegui tutti i controlli qui sotto prima di riferire, così il membro sente in una volta sola ogni passo mancante. Spunta ogni casella il cui controllo passa; lascia non spuntata ognuna che fallisce, con il passo e chi lo fa ("In the shared drive (amministratore: voce 4, passo 2)").
   - **Workspace account**: l'account connesso è sul dominio di `inbox` (lo stesso Workspace). Se no: ha fatto l'accesso con un account personale; deve ricollegarsi come suo account Workspace (voce 3), oppure l'amministratore gliene crea uno (voce 4, passo 1).
   - **In the shared drive**: crea un file di prova sotto `root_id`, poi cestinalo (voce 3, Drive). Non trovato: non è nel Drive; cestinare rifiutato: non è Gestore contenuti. In entrambi i casi voce 4, passo 2, l'amministratore.
   - **Shared calendar**: crea un evento di prova su `calendar`, poi eliminalo (voce 3, Calendar). Fallito: voce 4, passo 4, il proprietario del calendario (`calendar_owner`, per nome).
   - **In the group** (email attiva; email disattivata: "(email off)"): il controllo dei servizi condivisi della voce 7. Fallito: voce 4, passo 3.3, l'amministratore, oppure il collegamento Gmail del membro.
   - Un connettore che manca nella lista del membro: voce 4, passo 5, l'amministratore del client AI.
   Se In the shared drive fallisce, il resto di Unisciti è bloccato: non si può scrivere nulla nella radice. Ogni altro fallimento viene registrato e Unisciti prosegue.
4. **Solo nuovo membro**: intervistalo con il giro Membri di [interview.md](interview.md), solo per sé. La sua Email è l'account connesso: diglielo, non chiederla mai. Scrivi una volta sola (rules → Sostituire):
   - la sua riga Members in `org.md`, con Kind quando la tabella ha la colonna e Personal calendar `libero/occupato del dominio` a meno che non scelga di più;
   - la sua riga Access, con ciò che il passo 3 ha provato: Drive `gestore contenuti`, System calendar `può modificare` (altrimenti `in attesa`), Client connected, e `riceve la posta` nelle Notes quando In the group è passato;
   - `memories/<slug>.md`;
   - In Members and Access, con un file di memorie, spuntata.
5. **Membro esistente, nuova app**: aggiorna la sua riga Access ("Client connected").
6. **Email attiva**: esegui per lui la Parte membro di [email.md](email.md).
7. Dagli le sue routine da [routines.md](routines.md) e percorri la voce 6.

Fatto quando la persona è in Members e Access con ciò che i suoi controlli hanno provato, ha un file di memorie, la voce 6 è pronta o bloccata, (email attiva) la sua casella Email è spuntata o dice cosa si aspetta, e ogni casella della sua sezione Member è spuntata o nomina il passo e chi lo fa. Vai al passo 7 di SKILL.md.

## 6. Uscita

Al posto dei passi 1–4 di Uscita in SKILL.md, confermando ogni passo con il proprietario (l'amministratore, per un team) prima di farlo o di affidarlo:

1. **Prima il proprietario del calendario.** Quando il membro che esce è `calendar_owner`, Uscita è **bloccata**: non rimuovere nulla e non scrivere nient'altro finché il trasferimento non è fatto e controllato come dice il file `references/manual-setup.md` della skill `rules` → Un membro che esce, passo 1, `calendar_owner` compreso. A chi chiede di saltare il trasferimento dai il motivo (il calendario viene eliminato insieme all'account del suo proprietario) e il blocco resta.
2. **Toglilo** dal Drive condiviso, dal gruppo e dalla condivisione del calendario, poi sospendi o elimina il suo account Workspace, e chiedigli di scollegare i suoi connettori e di eliminare le sue routine: i passi 2–4 di Un membro che esce, ognuno fatto dalla persona che indica. Dove una chiamata può leggere il risultato (i membri del Drive condiviso, tramite i permessi del connettore di Drive), controllalo; altrimenti lo conferma chi l'ha fatto.

Poi i passi 5 e 6 di SKILL.md.

Fatto quando `calendar_owner` nomina un membro attuale diverso da quello che esce, ogni rimozione è controllata o confermata da chi l'ha fatta, le righe Members e Access in `org.md` lo mostrano rimosso, l'indirizzo dismesso dell'assistente (se c'è) è nella cella Mail from di un membro attuale, e `setup.md` non ha una sezione per lui. Vai al passo 7 di SKILL.md.

## 7. Verifica

Al posto delle righe della modalità base di SKILL.md:

- `root_id` è la cartella che contiene `org.md`, e risalendo le sue cartelle madri si arriva a `drive_id` (rules → Lavorare con Drive).
- Un file di prova creato sotto `root_id` viene cestinato, e un evento di prova creato su `calendar` viene eliminato, entrambi con chiamate reali.
- Ogni membro con un'Email ha una riga Access, e `calendar_owner` è un membro attuale (in Members, non rimosso).
- Nella riga dell'email, "la casella di posta è quella dell'account dell'assistente" diventa il controllo dei servizi condivisi della voce 7: una ricerca con il filtro della posta del gruppo restituisce risultati.
