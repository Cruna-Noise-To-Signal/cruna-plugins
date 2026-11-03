# Passo email

Attiva l'email della radice o la registra come disattivata, e porta ogni membro a usarla. Le regole che imposta sono nel file `references/email.md` della skill `rules`; leggilo prima. Si esegue da:

- **Nuova radice**, passo 2d: la Parte radice, con le risposte già in `setup.md` → Interview, poi la Parte membro per ogni membro intervistato (la Parte membro dice quali passi aspettano l'app del membro stesso).
- **Ramo Email** (una radice esistente, "attiva l'email"): la Parte radice, facendo ora le domande di Domande sull'email di [interview.md](interview.md), poi la Parte membro per ogni membro presente.
- **Unisciti**, in una radice con `email: on`: la Parte membro per il nuovo arrivato, o per un membro esistente su una nuova app (Gmail va collegato in ogni app che usa).

## Parte radice

1. **Decidi.** Email disattivata (rifiutata nell'intervista, o l'utente cambia idea): scrivi `email: off` nel front-matter di `org.md` (rules → Sostituire), spunta la casella Email della radice "(off)" e fermati. La radice funziona esattamente come prima.
2. **Gmail.** Percorri la voce 7 dei prerequisiti nell'app del membro presente. Bloccata: il passo si ferma qui con la casella non spuntata, che dice cosa si aspetta.
3. **`org.md`**: `email: on` nel front-matter. I criteri di importanza per tutta la radice registrati come in rules → Email → Dove stanno i criteri; nessuno quando la risposta è stata "nessuna preferenza". Sostituisci `org.md` una volta sola, insieme agli indirizzi della Parte membro quando li hai.
4. **`memories/shared.md`**: la sua riga Email importance, come indicato in rules → Email → Dove stanno i criteri quando ci sono criteri della radice; `no preference`, oppure `not asked` quando la domanda è stata saltata.
5. **`inbox.md`**: crealo nella radice dal suo modello (rules → Tipi di file → modelli), con una tabella vuota e ancora senza `last_tidy_up`.
6. **`setup.md`**: spunta la casella Email della radice; aggiungi una casella Email non spuntata a ogni sezione Member (rules → Tipi di file → modelli). I membri non presenti tengono la loro non spuntata, con scritto cosa devono ancora fare ("Marta: collega Gmail nella sua app").

Fatto quando: `org.md` dice `email: on`, i suoi criteri della radice (o nessuno) sono registrati, `memories/shared.md` ha la sua riga Email importance, `inbox.md` esiste esattamente una volta e la casella Email della radice è spuntata.

## Parte membro

I passi 2–3 registrano risposte, per ogni membro che le ha date; saltali quando gli indirizzi del membro e la sua riga Email importance sono già registrati. I passi 1 e 4 si fanno solo con il membro presente, nella sua app.

1. **Gmail** nell'app di questo membro: percorri la voce 7 dei prerequisiti (salta se la Parte radice l'ha appena fatto nella stessa app).
2. **Indirizzi**: ogni indirizzo da cui scrive o inoltra (giro Membri dell'intervista), nella colonna Mail from della sua riga Members. Le radici solitarie saltano questo passo: non c'è posta dei membri.
3. **Importanza**: la sua risposta alla domanda sull'importanza, come riga `- Email importance: <criteria | no preference>` in `memories/<slug>.md`; `not asked` se saltata.
4. **`setup.md`**: spunta la sua casella Email.

Fatto quando: la voce 7 è pronta nella sua app, la sua cella Mail from è compilata (non in una radice solitaria), il suo file di memorie ha la riga Email importance e la sua casella Email è spuntata; oppure la voce 7 è bloccata e la casella dice cosa si aspetta.
