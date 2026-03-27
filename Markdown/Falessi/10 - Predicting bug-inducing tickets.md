# Introduzione
La **bug prediction** è una branca della ricerca che si occupa di identificare i bug in progetti software prima che si presentino.

Fin'ora la bug prediction si è occupata di cercare i bug - già presenti, ma non ancora trovati - all'interno di classi, metodi ecc.

Lo studio punta a fare predizioni sulla possibile presenza dei bug a partire dai ticket che rappresentano un requisito. In particolare, si definiscono, misurano e valutano 62 feature per un nuovo task detto **Ticket-Level Prediction** (**TLP**).

In un processo di sviluppo software, un **ticket** può avere tre diversi stati:
- **open** -> il ticket è stato appena aperto e indica una nuova funzionalità da sviluppare, un bug da risolvere o una funzione da ottimizzare
- **in progress** -> il ticket è in lavorazione; si possono definire altri due sotto-stati:
	- **assigned** -> il ticket è stato assegnato a uno sviluppatore
	- **first-commit** -> lo sviluppatore ha iniziato a lavorare sul ticket e ha eseguito il suo primo commit, al quale possono seguirne altri
- **closed** -> il ticket viene chiuso dopo il completamento della feature

A un ticket sono collegati uno o più commit, i quali contengono all'interno del loro commento l'id del ticket.

Il commit è quello che potrebbe effettivamente contenere il bug, mentre il ticket raccoglie più ticket correlati. In questo caso, si guarda soltanto al ticket più che al commit che contiene il bug effettivo. Questa scelta è dovuta al fatto che prima una predizione viene fatta, meglio è. Con questo in mente, si vuole adoperare la bug prediction per adoperarsi per tempo alla risoluzione/prevenzione del bug. Tuttavia, è un'arma a doppio taglio, poiché maggiore è l'anticipazione e minore è l'accuratezza della predizione.

Dato un ticket, dunque, si prendono la data di assegnazione e quelle dei vari commit e ci si pone in tre istanti temporali:
- appena prima dell'assegnazione
- appena prima del primo commit
- dopo l'ultimo commit
Si vuole valutare l'accuratezza delle predizioni nei tre istanti di tempo.
# Design
Ci si pone due domande:
- **RQ1**: la prossimità temporale impatta l'accuratezza del TLP?
- **RQ2**: la prossimità temporale impatta il potere predittivo delle feature TLP?

La procedura di misurazione è fatta nel seguente modo. Ci si pone nei panni del PM e si cercano dataset JIT già presenti e riutilizzabili, dai quali si ricavano dei dataset TLP: se almeno uno dei commit associati a un ticket è buggy, allora lo è anche il ticket. Il dataset è suddiviso in training e test set e la valutazione è fatta sull'accuratezza e

Notare che le feature dei ticket non esistevano prima ed è stato necessario progettarle. In particolare, sono state definite 7 famiglie:
- codice -> relativi alla codebase (non quella che implementa il ticket, ma il restante), indica quanto è semplice maneggiarlo, il debito tecnico ecc.
- sviluppatore -> relativi allo sviluppatore a cui è assegnato il ticket, tra cui la familiarità dello sviluppatore con il progetto.
- temperatura esterna -> quante volte il ticket è soggetto a cambiamenti a causa di azioni provenienti dall'esterno (ad esempio nuove leggi che implicano la modifica dei ticket); ad esempio la località temporale, vale a dire quanti ticket buggy in una finestra temporale (misurata in ticket) che termina sul ticket stesso.
- temperatura interna -> quante volte il ticket è soggetto a cambiamenti dovuti agli stessi sviluppatori (modifica della descrizione, collegamento a un altro ticket ecc.); ad esempio il numero di commenti sul ticket
- intrinseco -> relativi al ticket stesso, ad esempio la priorità o il tipo (bug, improvement, new feature, subtask)
- requisito vs requisito -> relative a un ticket rispetto a un altro ticket; in particolare, si definiscono delle misure di distanza tra i ticket, come la distanza di Levenshtein minima (o simiglianza massima) tra i titoli dei ticket.
- JIT -> feature dei dataset sui commit già visti; ad esempio, il numero di LOC aggiunte o il numero di commit
Queste 62 feature totali vanno misurate per ogni ticket e cambiano a seconda del momento in cui vengono misurate.

La misurazione delle feature avviene nel seguente modo. A partire dai dataset JIT, si estraggono le colonne di buggyness e le coordinate (dove si trova il bug).


Tra tutti i progetti, ne sono stati scelti solo due (HIVE e HBASE), poiché sono quelli con percentuale di buggy commit associati ai ticket maggiore, oltre ad avere meno rumore nei dati. In totale, si hanno 11000 ticket.
### RQ1
Variabile indipendenti: istante di misurazione
Variabili dipendenti: metriche di accuratezza
H10: la prossimità temporale non influenza l'accuratezza della predizione
### RQ2
Variabile indipendenti: istante di misurazione
Variabili dipendenti: IGR
H10: il potere predittivo delle feature TLP non dipende dall'istante di misurazione
### Tecnica di validazione
Si applica un approccio sliding time window: per ogni finestra, si prende 

Si tiene anche conto del fatto che può esserci context drift: le relazioni tra le feature sulla buggyness possono cambiare nel tempo. L'approccio a finestra scorrevole ne riduce l'impatto.
# Risultati
## RQ1
Per la metrica di AUC, si ha una maggiore accuratezza sulla predizione al passare del tempo.

Per la metrica Kappa (quanto si va meglio o peggio rispetto a un predittore dummy, ossia non addestrato sui dati), si può dire qualcosa sul bug che verrà fuori solo guardando il ticket.
## RQ2
Le stesse feature, misurate in momenti diversi, hanno un'importanza diversa sul risultato finale. Segue che non c'è una famiglia di feature ottima, così come non vi è un momento più importante.

Infatti, occorre prendere le coppie (famiglia di feature, istante di misurazione) migliore. Alcune delle più significative sono:
- numero di partecipanti e numero di commit in parallelo quando lo stato è open
- il numero di attività (sul ticket) quando in progress
- JIT e il numero di linguaggi quando in closed