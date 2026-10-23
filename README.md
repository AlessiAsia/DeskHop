# Sistema di Gestione e Prenotazione di Spazi di Lavoro Pubblici - DeskHop

## Introduzione al Progetto 

Il presente progetto consiste nello sviluppo di un sistema software dedicato alla ricerca, gestione e prenotazione di spazi di lavoro e studio all'interno di strutture pubbliche o private, come aule studio, biblioteche e caffetterie. 
Il sistema si rivolge a due categorie principali di attori: gli studenti ed i lavoratori alla ricerca di un ambiente adeguato alle proprie necessità, ed i proprietari delle strutture che necessitano di uno strumento per gestire l'affluenza e la propria vetrina virtuale. 

Il progetto è stato sviluppato come elaborato pratico per il corso di Ingegneria del Software, rispettandone i vincoli architetturali, di documentazione e di qualità del codice.

## Funzionalità del Sistema - Modulo Utente (Studenti e Lavoratori)

Il sistema prevede un utilizzo differenziato in base alla registrazione dell'utente. 

In modalità ospite è possibile selezionare la città in cui cercare una venue, applicare filtri di ricerca (quali la presenza di parcheggio riservato, la vicinanza ai mezzi pubblici entro un raggio di cinquecento metri, la somministrazione di cibo e bevande, la disponibilità di prese elettriche, la connessione Wi-Fi gratuita e le specifiche fasce orarie di apertura) e visualizzare la bacheca delle strutture disponibili.

La registrazione di un account sblocca l'accesso alle funzionalità di prenotazione di un tavolo nella struttura ritenuta idonea, specificando la data (entro un limite massimo di due settimane), l'orario di arrivo, il numero di persone e i propri recapiti.
Una volta confermata la prenotazione è possibile cancellarla entro 2 ore dall'inizio della stessa.
Il sistema implementa delle funzionalità di penalizzazione per gli utenti che assumono comportamenti di no-show.

A supporto della prenotazione, il sistema prevede l'invio automatico di una comunicazione via email due ore prima dell'arrivo previsto, contenente numero telefonico ed indirizzo della struttura e un codice univoco di prenotazione da usare all'arrivo in loco per l'identificazione 
Dopo aver usufruito dello spazio, l'utente è invitato dal sistema a rilasciare una recensione testuale, con la possibilità di allegare materiale fotografico, contribuendo così ad arricchire la bacheca pubblica della struttura.

## Funzionalità del Sistema - Modulo Proprietario (Gestore Venue)

I proprietari delle strutture dispongono di un pannello di controllo dedicato per la configurazione della propria vetrina. 
Tramite un apposito modulo, il gestore definisce le informazioni del locale: l'indirizzo, i giorni e gli orari di apertura, i contatti (email e numero di telefono) e l'eventuale menu digitale. 
Nella vetrina del locale si trovano tutte le informazioni logistiche definite dal proprietario ed inoltre il proprietario può aggiungere delle foto, fino ad un massimo di 10, ed un tag specifico per indicare l'obbligo di silenzio tipico delle biblioteche.  

Il modulo di gestione delle prenotazioni, attivabile a discrezione del proprietario, fornisce una panoramica completa degli arrivi previsti, mostrando i dettagli dei clienti e consentendo un'ottimizzazione degli spazi. 
Al momento dell'arrivo del cliente, il gestore utilizza il sistema per validare il codice univoco della prenotazione, registrandone il successo.
In caso di necessità, il proprietario ha i permessi per cancellare manualmente una prenotazione su richiesta diretta del cliente, purché lo stato della stessa lo consenta.

## Logica di reward 

Il sistema implementa una logica di gamification a livelli:
Un utente di livello Bronzo ha accesso a tutte le funzionalità descritte nelle funzionalità del sistema - modulo utente
Un utente di livello Argento ha accesso a tutte le funzionalità del livello bronzo ed ha a disposizione un'ora in più per cancellare le prenotazioni (può cancellare la prenotazione fino ad un'ora prima dal suo inizio) 
Un utente di livello Oro ha accesso a tutte le funzionalità del livello argento ed ha diritto ad un coupon mensile di sconto per la consumazione presso le strutture aderenti.
Gli utenti accumulano punti tramite le validazioni di prenotazioni e le recensioni 

## Gestione delle Prenotazioni 

Il software implementa una logica di business basata su una macchina a stati per le prenotazioni.
Le prenotazioni salvate nel sistema nascono con stato "attivo e cancellabile", che permette all'utente di annullare l'impegno entro il tempo massimo stabilito dal livello di appartenenza.
Esiste anche la possibilità per il proprietario dell'attività di cancellare la prenotazione se comunicato dal cliente per via telefonica 
Superata la soglia temporale della cancellazione, la prenotazione transita in uno stato "non cancellabile".

All'orario di chiusura dell'attività, il sistema effettua un controllo delle prenotazioni con duplice scopo:
Il sistema invia una notifica a tutti gli utenti che hanno validato la loro prenotazione spronandoli a lasciare una recensione
Inoltre, per disincentivare i comportamenti scorretti (no-show), se una prenotazione non cancellabile non è stata validata dal proprietario tramite il codice univoco, l'utente riceve un avviso formale
L'utente ha a disposizione tre giorni per contestare l'avviso; in caso contrario, esso diventa definitivo. 
Al raggiungimento di tre avvisi, il sistema applica una penalità bloccando le capacità di prenotazione dell'utente per una settimana. 
