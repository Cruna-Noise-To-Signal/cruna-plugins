# Migrare una radice solitaria a un account dell'assistente dedicato

Una radice solitaria vive nell'account Google personale del proprietario. Quando entra una seconda persona, un **account dell'assistente** dedicato prende in carico la cartella radice e il calendario di sistema, così l'app di ogni membro può collegarsi come quell'unico account. Eseguilo con il proprietario; la maggior parte dei passi sono clic che fa lui in Google, mentre tu controlli ognuno con una chiamata reale.

Prima di iniziare spiega il perché in una o due frasi semplici: l'assistente di ciascuno deve accedere con lo stesso account Google, e non deve essere quello personale del proprietario.

## Passi

1. **Crea l'account dell'assistente.** Percorri la voce 4 dei prerequisiti. Fatto quando il proprietario ha il nuovo indirizzo e la password e il recupero è impostato.
2. **Dagli la cartella.** Il proprietario, in Google Drive con il proprio account:
   1. Clic destro sulla cartella radice → **Condividi** (Share) → aggiungi l'account dell'assistente come **Editor** → Invia.
   2. Apri di nuovo **Condividi** → accanto all'account dell'assistente scegli **Trasferisci la proprietà** (Transfer ownership) → conferma. Google invia un invito di trasferimento della proprietà.
   3. Accedi a Google Drive come account dell'assistente → apri l'invito (nella sua Gmail, o nella finestra Condividi della cartella) → **Accetta**.

   Trasferire una cartella sposta solo la cartella; i file al suo interno restano di proprietà del proprietario (sempre pienamente utilizzabili da tutti). Per spostare anche quelli, il proprietario apre ogni cartella, seleziona tutti i file e ripete Condividi → Trasferisci la proprietà; l'account dell'assistente accetta. Consiglialo quando il proprietario potrebbe lasciare il gruppo o ha poco spazio; altrimenti può aspettare.

   Fatto quando, dopo il passo 4, i permessi della cartella radice mostrano l'account dell'assistente come proprietario e il proprietario come editor.
3. **Condividi con lui il vecchio calendario.** Il proprietario, in Google Calendar → Impostazioni (Settings) → il calendario di sistema → Condividi con persone specifiche (Share with specific people) → aggiungi l'account dell'assistente con **Visualizzare tutti i dettagli degli eventi** (See all event details). Fatto quando il proprietario conferma.
4. **Ricollega i connettori del proprietario.** Percorri la voce 3 dei prerequisiti, caso "già collegato con l'account sbagliato": scollega Google Drive e Google Calendar, poi ricollegali entrambi come account dell'assistente. Fatto quando il calendario principale nella lista dei calendari riporta l'email dell'account dell'assistente e la ricerca su Drive trova `org.md`.
5. **Nuovo calendario di sistema.** Crea un calendario di proprietà dell'account dell'assistente con lo stesso nome (con il connettore se può, altrimenti con i passi da cliccare del passo 2c.7 di setup, con l'accesso come account dell'assistente). Copia ogni evento futuro dal vecchio calendario al nuovo, mantenendo titolo, orario, descrizione e colore. Fatto quando il nuovo calendario è elencato e contiene gli stessi eventi futuri del vecchio.
6. **Aggiorna `org.md`**: `assistant_account` impostato al nuovo indirizzo, `calendar` al nome del nuovo calendario, la riga Access del proprietario aggiornata. Sostituisci il file secondo rules → Sostituire. Fatto quando l'`org.md` scaricato mostra entrambe le modifiche.
7. **Condividi il nuovo calendario** con il proprietario (voce 5 dei prerequisiti), e chiedi al proprietario di condividere il suo calendario personale con l'account dell'assistente al livello indicato in `org.md`. Poi il proprietario rimuove il vecchio calendario dal suo Google Calendar (Impostazioni → il vecchio calendario → Rimuovi calendario (Remove calendar) → Elimina) così gli eventi non compaiono due volte.

Fatto quando la cartella radice è di proprietà dell'account dell'assistente, il nuovo calendario di sistema è elencato con gli eventi futuri, `org.md` indica entrambi e i connettori del proprietario si collegano come account dell'assistente. Le routine pianificate del proprietario non vanno cambiate.
