# Agora

Una base di conoscenza condivisa, tenuta dall'AI, per famiglie e piccoli team. Le persone raccontano all'AI che cosa è successo; l'AI lo archivia su Google Drive, tiene uno stato aggiornato di ogni cosa, mette le date su Google Calendar e invia revisioni giornaliere, settimanali e mensili.

Funziona su Claude e ChatGPT. Progetto: [agora-spec.md](https://github.com/Cruna-Noise-To-Signal/cruna-plugins/blob/main/docs/agora/agora-spec.md).

## Che cosa ti serve

- Claude o ChatGPT con un piano a pagamento (per le revisioni pianificate).
- I connettori di Google Drive e Google Calendar.
- Un account Google per l'assistente quando lo usa più di una persona (chi lo usa da solo può usare il proprio).

La skill `setup` controlla tutto questo e ti guida in ciò che manca.

## Skill

| Skill | Funziona su | Che cosa fa |
|---|---|---|
| `rules` | Claude, ChatGPT | Modello dei dati e regole sui file che ogni altra skill segue |
| `setup` | Claude, ChatGPT | Controlla i prerequisiti, ti intervista, crea le cartelle e il calendario, imposta le routine; aggiunge membri |
| `add-interest` | Claude, ChatGPT | Inizia a seguire un nuovo interesse o una nuova istanza |
| `log` | Claude, ChatGPT | Registra un aggiornamento nel diario e aggiorna lo stato |
| `file-document` | Claude, ChatGPT | Archivia un documento dalla cartella `landing/` o indicizza uno già al suo posto |
| `schedule` | Claude, ChatGPT | Crea, sposta o elimina appuntamenti e promemoria |
| `ask` | Claude, ChatGPT | Risponde alle domande usando indici, stati, diario, documenti e calendario |
| `review` | Claude, ChatGPT | Revisione giornaliera, settimanale e mensile con riordino; il cuore di ogni routine |
| `remember` | Claude, ChatGPT | Conserva ciò che l'assistente sa di ogni persona e del gruppo |
| `help` | Claude, ChatGPT | Spiega che cosa sa fare Agora, risponde alle domande frequenti, risolve i problemi |

## Installazione

- **Claude**: `/plugin marketplace add Cruna-Noise-To-Signal/cruna-plugins`, poi `/plugin install agora-ita@cruna-plugins`. Su claude.ai, aggiungi il marketplace dalle impostazioni dei plugin.
- **ChatGPT**: installa `agora-ita` dalla pagina Plugin (potrebbe servire che un amministratore del workspace lo pubblichi prima).

Poi scrivi: «Configura Agora».
