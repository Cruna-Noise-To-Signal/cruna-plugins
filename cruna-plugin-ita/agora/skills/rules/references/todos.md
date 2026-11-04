# Convenzioni dei to-do di Agora

Le regole condivise da ogni skill che legge o scrive to-do (rules → To-do). I modelli dell'evento to-do, di `todo.md`, della riga Pending di `status.md` e della sezione To-dos della revisione: [templates.md](templates.md).

Una radice senza cartella `todos/` non ha to-do: ogni skill funziona come prima, e `todo.md` non esiste. La skill `setup` crea `todos/` quando costruisce una radice; in una radice configurata prima della 0.5.0, la skill `todo` la crea la prima volta che un membro aggiunge un to-do.

## Chi vede un to-do e a chi tocca

- Ogni to-do è visibile a ogni membro: non esiste un to-do privato. Dillo quando un membro sembra sul punto di aggiungere qualcosa di privato.
- Il **responsabile** (`owner`) è lo slug di un membro, oppure `everyone` per un to-do condiviso che aspetta chiunque lo prenda. Un to-do che un membro aggiunge è suo, a meno che non nomini un altro membro o `everyone`.
- Qualsiasi membro può chiudere, riaprire, riassegnare, prendere in carico ("ci penso io": il responsabile diventa il membro che parla), modificare o rimuovere qualsiasi to-do. Ogni cambiamento è un evento a sé con il suo `author`, così la storia dice chi ha fatto cosa.

## Collocazione

Un to-do è **legato** a un'istanza quando ce n'è una chiara: collocato con certezza dalla collocazione della skill `log` (gli indici, poi le parole del membro), o passato dalla skill che lo ha mandato (l'istanza di una voce di diario, l'istanza di un'email archiviata). Altrimenti vive **a livello di radice**, senza `interest`/`instance`. Aggiungere un to-do non chiede mai dove va.

Un to-do aperto legato a un'istanza compare nel Pending dello `status.md` della sua istanza (Nello stato, sotto).

## Scadenze

Un to-do può avere una data `due`, risolta nel fuso orario della radice ("entro venerdì" → il venerdì che arriva). Una scadenza non crea alcun evento di calendario: un evento si crea solo quando un membro lo chiede, tramite la skill `schedule`.

## Eventi to-do

Ogni cambiamento a un to-do è un **evento to-do**: un file sorgente, solo aggiunta (rules → Tipi di file). Un evento si crea, non si modifica mai; un errore si corregge con un evento successivo.

- **Nome**: `todos/<author>-YYYYMMDD_HHMM.md`, lo slug dell'autore e il minuto del cambiamento nel fuso orario della radice (un `add` da posta dei membri prende invece l'ora di ricezione dell'email: [email.md](email.md) → Posta dei membri). Quando quel nome è occupato, aggiungi `-2`, poi `-3`, prima di `.md`.
- **Id**: l'id di un to-do è il nome del suo evento `add`, senza `.md` (`simone-20261103_0912`).
- **Front-matter**: `agora: todo`, `action`, `author`, `created` (al secondo, `YYYY-MM-DDTHH:MM:SS`, così la ricomposizione ordina gli eventi per tempo reale), poi per azione:
  - `add`: `owner`; facoltativi `due`, `interest` e `instance`, `source`. Il corpo è una riga: il testo del to-do.
  - `done`, `reopen`: `todo` (l'id su cui agisce).
  - `assign`: `todo`, `owner`.
  - `edit`: `todo`, più ogni campo che cambia: `text`, `due` (`none` la toglie), `interest` e `instance` (`none` lo porta a livello di radice).
  - `remove`: `filter` (il filtro del membro, a parole), `count`, `todos` (gli id spostati nel cestino). Un evento remove è solo una traccia: non agisce sullo stato di alcun to-do.
- **Fonte** (`source`): un percorso dalla radice alla voce di diario o alla revisione da cui viene, oppure `email <Gmail message id>`.

### Chi scrive gli eventi

Solo una conversazione scrive eventi to-do, tramite la skill `todo`: un membro l'ha chiesto, o ha detto sì a una proposta. L'unica eccezione è la posta dei membri che chiede esplicitamente di aggiungere un to-do: vale come richiesta del membro stesso e si aggiunge anche in un'esecuzione non presidiata ([email.md](email.md) → Posta dei membri). Ogni altra email, e ogni routine, **suggerisce** soltanto (Suggerimenti, sotto).

## Stato attuale

Per conoscere lo stato di un to-do, **ricomponi** i suoi eventi: il suo evento `add` e ogni evento il cui `todo` è il suo id, in ordine di `created` (a parità di `created`, al secondo: per nome del file).

- Dopo `add`, è aperto. L'ultimo `done` o `reopen` decide se è aperto o chiuso; la sua **data di chiusura** è il `created` dell'ultimo `done`.
- Ogni campo (`text`, `owner`, `due`, collocazione) prende il suo valore più recente.
- È **scaduto** quando è aperto con una `due` precedente a oggi.
- Un evento il cui `todo` non corrisponde a nessun evento `add` in `todos/` o in `todos/archive/` è un orfano: lascialo fuori da `todo.md`, e il riordino della `review` lo segnala.

## `todo.md`

Il `todo.md` della radice è la lista dei to-do che i membri leggono: un file **derivato** (rules → Tipi di file), mai modificato a mano, ricostruito per sostituzione (rules → Sostituire) dalla ricomposizione di ogni evento in `todos/` (archivio escluso).

- Una sezione `## <Name>` per ogni membro responsabile di un to-do aperto, nell'ordine di Members in `org.md`; poi `## Everyone`; poi `## Recently done`. Un Everyone o un Recently done vuoto dice "—".
- In una sezione di membro o in Everyone: i suoi to-do aperti nell'**ordine della lista**: prima gli scaduti, poi per scadenza, poi quelli senza data (dal più vecchio).
- **Recently done**: i to-do chiusi negli ultimi 14 giorni, dal più recente, con chi li ha chiusi.
- Ogni riga: il testo, la scadenza, un collegamento all'istanza quando è legato a una, un collegamento alla fonte quando c'è, e l'id. Il titolo della sezione dice il responsabile, quindi solo le righe di Recently done lo ripetono.
- **Ricostruiscilo** dopo ogni cambiamento, elencando di nuovo `todos/` subito prima di scrivere, così gli eventi che un altro membro ha scritto nel frattempo entrano nella ricomposizione. I duplicati si gestiscono come per ogni file derivato (rules → Sostituire).
- È **obsoleto** quando un evento in `todos/` è più recente della sua riga Last updated, oppure quando non esiste mentre `todos/` contiene eventi.

Leggi la lista da `todo.md`; quando è obsoleto, ricomponi invece gli eventi, e ricostruiscilo quando la conversazione può scrivere.

## Nello stato

Il Pending dello `status.md` di un'istanza elenca ogni to-do aperto legato all'istanza, una riga ciascuno (modello in [templates.md](templates.md)), nell'ordine della lista (`todo.md`, sopra). Gli stati di interesse e di radice li riassumono come ogni elemento Pending. Ogni cambiamento che apre, chiude, riassegna, cambia la scadenza o sposta un to-do legato a un'istanza ricostruisce lo stato di ogni istanza a cui era o è legato, poi propaga (rules → Regola di aggiornamento dello stato).

## Suggerimenti

Le routine non scrivono mai un evento to-do: non aggiungono, chiudono, riassegnano né rimuovono mai un to-do. Ciò che notano, lo **suggeriscono**, nella sezione To-dos della revisione e sotto Needs attention:

- **Sembra fatto**: "sembra fatto: <testo> (<voce di diario o email>)", quando una voce di diario o un'email successiva mostra che il to-do è stato fatto.
- **Possibile nuovo to-do**: "possibile nuovo to-do: <testo> (<voce di diario o email>)", quando una voce di diario o un'email chiede a qualcuno di agire.

Un membro conferma in conversazione ("sì, fatto", "aggiungilo"), e la skill `todo` scrive l'evento. Le skill interattive (`log`, `email`) chiedono invece "aggiungo un to-do?", e ne aggiungono uno solo dopo un sì.

## Revisioni

Ogni revisione (giornaliera, settimanale, mensile) ha una sezione `## To-dos`, costruita dalla ricomposizione di `todos/`:

- **Scaduti**: i to-do aperti oltre la loro scadenza.
- **In scadenza**: i to-do aperti che scadono nella finestra dello sguardo avanti.
- **Aperti** e **Fatti**: i to-do aggiunti o chiusi nella finestra dello sguardo indietro.
- **Suggerimenti**: Sembra fatto e Possibile nuovo to-do (Suggerimenti, sopra).
- Solo **mensile**: **Senza data, aperti da oltre 30 giorni**: i to-do aperti senza `due`, aggiunti più di 30 giorni fa, come invito a lasciarli cadere o a dar loro una data.

Una revisione personale mostra prima i to-do del membro, poi quelli di Everyone. La revisione generale mostra quelli di ogni membro, poi quelli di Everyone. Togli le parti vuote; se non c'è niente: "Niente di nuovo." Una radice senza `todos/` non ha sezione To-dos.

## Rimozione

Rimuovere è un'azione della skill `todo`, solo in conversazione; le routine non rimuovono mai.

1. **Filtro**: per data (creati, o chiusi, fino a una data), per istanza o interesse, per responsabile, `everyone`, o per fonte (un percorso, o "dalle email"). I filtri si combinano: un to-do deve corrispondere a tutti. Anche i to-do in archivio corrispondono.
2. **Portata**: solo i to-do chiusi, a meno che il membro non dica di includere quelli aperti ("compresi quelli aperti").
3. **Conferma**: elenca le corrispondenze, numero e testi, con quelli aperti elencati a parte, e di' che vanno nel cestino di Drive, recuperabili per 30 giorni. Aspetta un sì a quell'elenco.
4. **Cestino**: sposta nel cestino ogni file evento di ogni to-do corrispondente (il suo `add` e ogni evento che nomina il suo id). Non eliminare mai definitivamente.
5. **Traccia**: scrivi un evento `remove` con filtro, numero e id, poi ricostruisci `todo.md` e gli stati interessati.

Un to-do ripristinato dal cestino di Drive torna com'era: i suoi eventi si ricompongono di nuovo.

## Archivio

Alla `review` mensile, il riordino sposta i file evento di ogni to-do chiuso da più di 90 giorni in `todos/archive/YYYY/`, dove YYYY è l'anno in cui è stato chiuso, e gli eventi `remove` più vecchi di 90 giorni nell'anno in cui sono stati scritti. Non si elimina nulla, e una seconda esecuzione non sposta nulla di nuovo.

I to-do in archivio sono fuori da `todo.md` e dalle revisioni. Cerca nell'archivio solo quando un membro chiede di vecchi to-do, o quando un to-do che nomina non è in `todos/`. Riaprire un to-do in archivio sposta prima i suoi eventi di nuovo in `todos/`.
