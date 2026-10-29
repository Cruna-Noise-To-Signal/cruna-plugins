# Intervista di configurazione

Raccogli ciò che serve a `org.md` e ai primi interessi. Chiedi a giri di due–quattro domande. Ogni domanda porta con sé una **risposta consigliata**, dedotta da ciò che già sai, così l'utente può rispondere "sì" o con una parola. Insisti con garbo quando una risposta creerà problemi più avanti (un interesse che in realtà è un'istanza, un membro senza account Google), poi accetta la decisione dell'utente.

Usa le parole dell'utente per nomi e descrizioni. Chiedi nella lingua in cui scrive l'utente; una volta stabilita la lingua, usa quella.

## Giro 1: Membri

Per ogni persona che userà la radice o che vi sarà seguita:

- **Nome** (lo slug segue rules → Vocabolario).
- **Ruolo** nel gruppo (genitore, figlio, partner, titolare, dipendente, commercialista).
- **Compleanno** (facoltativo; utile per i promemoria legati all'età).
- **Email dell'account Google personale**, per i membri che useranno la radice, così si possono condividere con loro la cartella e il calendario.
- **Condivisione del calendario personale**: solo libero/occupato (consigliato: basta per trovare tempo libero) o tutti i dettagli.
- **Come lo useranno**: con la propria app di AI (Claude o ChatGPT), o solo tramite altri (un bambino seguito che non lo usa).

Consiglia: elenca le persone che l'utente ha già nominato e chiedi solo ciò che manca.

Registra l'email nella colonna Email del membro e l'app nella sua riga Access.

## Giro 2: La radice

- **Nome della radice** (consiglia il cognome della famiglia o il nome dell'attività: "Famiglia Fabiole").
- **Lingua** di tutti i contenuti (consiglia la lingua in cui scrive l'utente).
- **Fuso orario** (consiglia quello che si ricava dalla sua città o dal suo calendario).

## Giro 3: Convenzione di struttura

Che cosa rappresenta un interesse, così ogni aggiunta futura resta coerente. Proponi in base al tipo di radice, poi lascia decidere all'utente:

- **Famiglia**: un interesse per persona, più `famiglia` per le cose comuni (casa, vacanze, auto). Le istanze sono aree della vita di quella persona: `nina/salute`, `nina/scuola`, `famiglia/vacanze`.
- **Attività**: un interesse per cliente, più `azienda` per le questioni interne. Le istanze sono lavori o progetti: `acme/sito-2026`, `azienda/tasse`.
- **Altro**: per argomento (`casa`, `soldi`, `salute`), le istanze sono sotto-argomenti.

Scrivi la convenzione scelta come una frase in `layout` di `org.md`. Ricorda all'utente che tutto ciò che è più piccolo di un'istanza (una visita, una fattura) è una voce di diario, non una cartella.

## Giro 4: Primi interessi e istanze

Proponi un insieme iniziale dalla convenzione e da ciò che l'utente ha detto (per una famiglia di quattro: un interesse per persona con `salute` e `scuola`, più `famiglia/casa`). Per ognuno, raccogli una descrizione di una riga. Ogni interesse ha almeno un'istanza, perché solo le istanze contengono voci di diario e documenti. Proponi un colore del calendario per interesse (facoltativo).

Tienilo piccolo: da tre a sei interessi bastano; se ne possono aggiungere altri in qualsiasi momento con "inizia a seguire…".

Chiedi se tengono già traccia di qualcosa in un foglio o in una nota (poppate, pannolini, spese, una lista della spesa). Consiglia "no" a meno che non ne abbiano parlato. Per ogni sì, annota che cos'è, dove si trova e a quale istanza appartiene: dopo la costruzione viene proposto per l'importazione (Output di setup).

## Riepilogo finale

Mostra all'utente un unico riepilogo compatto (membri, nome della radice, lingua, fuso orario, struttura, interessi e istanze) e chiedi un sì finale. Le correzioni tornano al giro corrispondente.
