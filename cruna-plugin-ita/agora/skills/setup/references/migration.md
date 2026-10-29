# Migrare una radice solitaria a un account dell'assistente dedicato

Una radice solitaria vive nell'account Google personale del proprietario. Quando entra una seconda persona, un **account dell'assistente** dedicato prende in carico la cartella radice, e il suo calendario principale diventa il calendario di sistema, così l'app di ogni membro può collegarsi come quell'unico account. Eseguilo con il proprietario; la maggior parte dei passi sono clic che fa lui in Google, mentre tu controlli ognuno con una chiamata reale.

Prima di iniziare spiega il perché in una o due frasi semplici: l'assistente di ciascuno deve accedere con lo stesso account Google, e non deve essere quello personale del proprietario.

## Passi

1. **Crea l'account dell'assistente.** Percorri la voce 4 dei prerequisiti. Fatto quando il proprietario ha il nuovo indirizzo e la password e il recupero è impostato.
2. **Dagli la cartella.** Il proprietario, in Google Drive con il proprio account:
   1. Clic destro sulla cartella radice → **Condividi** (Share) → aggiungi l'account dell'assistente come **Editor** → Invia.
   2. Apri di nuovo **Condividi** → accanto all'account dell'assistente scegli **Trasferisci la proprietà** (Transfer ownership) → conferma. Google invia un invito di trasferimento della proprietà.
   3. Accedi a Google Drive come account dell'assistente → apri l'invito (nella sua Gmail, o nella finestra Condividi della cartella) → **Accetta**.

   Trasferire una cartella sposta solo la cartella; i file al suo interno restano di proprietà del proprietario (sempre pienamente utilizzabili da tutti). Per spostare anche quelli, il proprietario apre ogni cartella, seleziona tutti i file e ripete Condividi → Trasferisci la proprietà; l'account dell'assistente accetta. Consiglialo quando il proprietario potrebbe lasciare il gruppo o ha poco spazio; altrimenti può aspettare.

   Fatto quando, dopo il passo 4, i permessi della cartella radice mostrano l'account dell'assistente come proprietario e il proprietario come editor.
3. **Condividi con lui il calendario del proprietario.** Il proprietario, in Google Calendar → Impostazioni (Settings) → sotto "Impostazioni dei miei calendari" (Settings for my calendars), il proprio nome → Condividi con persone specifiche o gruppi (Share with specific people or groups) → aggiungi l'account dell'assistente con **Apporta modifiche agli eventi** (Make changes to events) (serve per spostare gli eventi di Agora; si abbassa al passo 7). Fatto quando, dopo il passo 4, il calendario del proprietario compare nella lista dei calendari.
4. **Ricollega i connettori del proprietario.** Percorri la voce 3 dei prerequisiti, caso "già collegato con l'account sbagliato": scollega Google Drive e Google Calendar, poi ricollegali entrambi come account dell'assistente. Fatto quando il calendario principale nella lista dei calendari riporta l'email dell'account dell'assistente e la ricerca su Drive trova `org.md`.
5. **Sposta gli eventi di Agora.** Il calendario principale dell'account dell'assistente è il nuovo calendario di sistema. Copiaci ogni evento futuro del calendario del proprietario il cui titolo inizia con `[<interest>/<instance>]`, mantenendo titolo, orario, descrizione e colore; poi elimina ogni originale dal calendario del proprietario, così niente compare due volte. Fatto quando il calendario principale contiene ogni evento futuro di Agora e il calendario del proprietario non ne contiene nessuno.
6. **Aggiorna `org.md`**: `assistant_account` e `calendar` entrambi impostati al nuovo indirizzo, la riga Access del proprietario aggiornata. Sostituisci il file secondo rules → Sostituire. Fatto quando l'`org.md` scaricato mostra entrambe le modifiche.
7. **Condividi il calendario di sistema** con il proprietario (voce 5 dei prerequisiti, passi da cliccare come account dell'assistente). Poi il proprietario abbassa la condivisione del proprio calendario con l'account dell'assistente al livello indicato in `org.md` (stesso percorso del passo 3). Fatto quando il proprietario conferma entrambe le cose.

Fatto quando la cartella radice è di proprietà dell'account dell'assistente, il suo calendario principale contiene gli eventi futuri di Agora, `org.md` indica il nuovo indirizzo e i connettori del proprietario si collegano come account dell'assistente. Le routine pianificate del proprietario non vanno cambiate.
