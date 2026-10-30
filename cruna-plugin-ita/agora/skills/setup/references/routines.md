# Routine

Ogni membro ha tre attività pianificate nella propria app. Ognuna esegue la skill `review` di Agora per un periodo. Inserisci lo slug del membro e il nome della radice, suggerisci gli orari qui sotto (l'utente può cambiarli, nel fuso orario della radice) e presenta ogni prompt come un blocco che l'utente può copiare così com'è.

| Routine | Cosa copre | Quando (consigliato) | Si ripete |
|---|---|---|---|
| Giornaliera | Ieri in revisione, oggi in vista | 07:30 | Ogni giorno |
| Settimanale | Questa settimana in revisione, la prossima in vista | Domenica 19:00 | Ogni settimana |
| Mensile | Il mese scorso in revisione, questo mese in vista | 08:00 del giorno 1 | Ogni mese |

## Prompt

Ogni prompt è esattamente una riga, nella forma che la skill `review` si aspetta. Lo slug dice a un'esecuzione non presidiata per chi personalizzare la revisione; un membro che preferisce ricevere la revisione generale, con l'aggiornamento di ogni membro, toglie `per <slug>`.

Giornaliera:

```
Esegui revisione Agora: giornaliera per <slug> in <nome della radice>
```

Settimanale:

```
Esegui revisione Agora: settimanale per <slug> in <nome della radice>
```

Mensile:

```
Esegui revisione Agora: mensile per <slug> in <nome della radice>
```

## Crearle

Percorri con l'utente la voce 6 dei prerequisiti: dove si trova la pagina delle attività pianificate nella sua app, incollare ogni prompt con il suo orario, verificare che l'attività possa usare i connettori di Drive e Calendar e il plugin Agora, e attivare le notifiche. Molte app accettano anche "pianifica questo ogni giorno alle 07:30" scritto in una chat insieme al prompt; proponilo come la via più rapida.
