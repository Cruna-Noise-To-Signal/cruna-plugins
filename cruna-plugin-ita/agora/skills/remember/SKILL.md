---
name: remember
description: Ricorda cose durature sui membri di Agora e sul gruppo — preferenze, abitudini, chi fa cosa, fatti permanenti come le allergie — e le mostra o le dimentica su richiesta. Usala quando un membro dice "ricordati che…", "prendi nota che preferisco…", "che cosa ricordi di me?" o "dimentica che…", oppure quando il passo memoria di un'altra skill Agora trova qualcosa che vale la pena tenere.
---

# Ricorda

`memories/` contiene ciò che resta vero da una conversazione all'altra: tratti, preferenze, abitudini, relazioni, fatti permanenti. Ogni skill Agora lo legge per impostare tono e valori predefiniti. È **curato**: breve, aggiornato, un fatto per riga, modificato sostituendo le righe invece di accumularle (rules → Tipi di file).

Ti si raggiunge in due modi: un membro te lo chiede, oppure il passo memoria di un'altra skill ti passa un fatto.

## Passi

### 1. Carica le regole, trova la radice, sappi chi sta parlando

Carica la skill `rules`. Segui rules → Trovare la radice e rules → Identità. Se un'altra skill ti ha passato un fatto, questo l'ha già fatto chi ti ha chiamato.

Fatto quando: `org.md` è stato letto e conosci lo slug del membro.

### 2. Scegli il ramo

- **Salva**: "ricordati che…", "prendi nota che preferisco…", o un fatto che ti è stato passato. Vai al passo 3.
- **Mostra**: "che cosa ricordi di me?" Leggi `memories/<slug>.md` e le righe di `memories/shared.md` che parlano di lui o di lei; mostrale come un semplice elenco. Finito.
- **Dimentica**: "dimentica che…". Trova la riga o le righe corrispondenti nel file del membro o in `shared.md`; quando più di una potrebbe corrispondere, chiedi quale. Rimuovile e sostituisci il file (rules → Sostituire). Finito.

Fatto quando: sai quale ramo, e Mostra o Dimentica è completato.

### 3. Classifica il fatto

Una memoria è duratura: sarà ancora vera il mese prossimo e orienta il modo in cui aiuti. Classifica ogni fatto:

- **Memoria**: "Simona si occupa delle cose della scuola", "Preferisco risposte brevi", "Marco paga le fatture dei clienti il 10". Tienila.
- **Fatto da diario**: un evento in un'istanza ("la lezione di Nina è stata spostata a giovedì"). Passalo invece alla skill `log`.
- **Convenzione**: come Agora gestisce i dati, non chi è qualcuno ("d'ora in poi metti le poppate di Greta in un file", "non registrare ogni pannolino"). Va in `org.md` → Conventions (rules → Convenzioni): passala alla skill `log`. Le memorie restano sulle persone.
- **Entrambi**: un fatto permanente che appartiene anche alla storia di un'istanza ("Nina è allergica alla penicillina"). Tienilo qui, e registralo anche nell'istanza (`nina/salute`) con la skill `log`.
- **Segreto**: password, PIN, numeri di carta, codici di sicurezza, codici di recupero. Tienili del tutto fuori da Agora: i file su Drive sono leggibili da ogni membro. Di' al membro di conservarli in un gestore di password.

Fatto quando: ogni fatto è una memoria, è stato passato a `log` (come fatto da diario o come Convenzione), o è stato respinto come segreto.

### 4. Scegli il file

- Su un membro (le sue preferenze, abitudini, come preferisce essere aiutato): `memories/<slug>.md` di **quel** membro, chiunque stia parlando.
- Sul gruppo (chi si occupa di cosa, routine condivise), o su qualcuno che non è un membro (un figlio, un cliente, un fornitore): `memories/shared.md`.

Fatto quando: ogni memoria ha un solo file di destinazione.

### 5. Cura e scrivi

Leggi il file di destinazione (rules → Lavorare con Drive); se non esiste ancora, parti dal modello del file di memoria (rules → Tipi di file → modelli).

Scrivi il fatto come una riga datata: `- YYYY-MM-DD <fatto>`, nella lingua della radice, in modo semplice e specifico. Poi **sostituisci il vecchio**: quando una riga copre già lo stesso argomento ("preferisce le chiamate al mattino" contro un nuovo "preferisce le chiamate dopo le 18:00"), sostituisci quella riga invece di aggiungerne una seconda; quando la nuova riga ripete una riga esistente, aggiorna solo la sua data. Unisci i quasi-duplicati che noti mentre ci sei. Tieni il file abbastanza breve da leggerlo in un minuto.

Sostituisci il file (rules → Sostituire).

Fatto quando: il file contiene il nuovo fatto esattamente una volta, nessuna riga lo contraddice ed esiste una sola copia del file.

## Output

Una riga breve nella lingua della radice che dice che cosa ricorderai ("Fatto: Simona si occupa delle cose della scuola."), o che cosa hai dimenticato. Per Mostra, l'elenco. Per un segreto, la frase che rimanda a un gestore di password. Quando sei chiamata dal passo memoria di un'altra skill, quella skill aggiunge la stessa riga alla fine della propria risposta.
