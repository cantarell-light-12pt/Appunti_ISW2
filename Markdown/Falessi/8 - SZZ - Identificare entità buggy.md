Principi del ML: size does matter, garbage in, garbage out.

Dal punto di vista del PM, i valori di interesse sono l'effort e il costo dello sviluppo di un codice. Una metrica per determinare se un codice è ben scritto è la presenza di bug o meno.

L'approccio SZZ assume il fatto che tutte le righe di codice modificate all'interno di un commit di tipo bug fix erano tutte buggy.

Per il progetto, dovrà essere creata una tabella (dataset) in cui l'ultima colonna è l'attributo buggy/non-buggy e le altre tutte le feature che comportano la presenza o meno di bug. Tutte le metriche devono essere misurabili. Esempi sono:
- grandezza del metodo
- metodi usati dal metodo
- metodi che usano il metodo
- quante modifiche ha subito il metodo in passato (importante)
- altre misure di complessità
**Nota**: se le metriche vengono fatte a release, la metrica sarà la somma delle metriche su ogni commit.

Quindi:
1. si prendono tutti i commit il cui ticket nel commento è di tipo buggy
2. la versione precedente del metodo sarà buggy, quella nuova non-buggy

Problema: si conosce il tipo e la posizione di bug sono a seguito del fix, tramite il commit.

Quando si fa il blame tramite Git (per vedere quando il codice è stato inserito). Potrebbero capitare situazioni in cui si ha (considerando la colonna target come 'il bug è presente' e non 'il bug è stato inserito'):
- il metodo `a()` buggy dalla versione 1.11
- il metodo `b()` buggy dalla versione 1.14
- il metodo `c()` buggy dalla versione 1.16
- tutti i metodi non più buggy dalla versione 1.18, che ha risolto un bug modificando tutti e tre i metodi
## Limitazioni
- **backward search impreciso**: diventa difficile capire quanto andare indietro, essendo la riga del dataset un metodo di una release: non si sa quante righe del dataset corrispondenti a quel metodo devono essere classificate come buggy
	- per risolvere il problema, si possono utilizzare i campi *affect version* e *fix version* di Jira insieme alle date di rilascio delle suddette versioni; se questi due campi non ci sono, si possono stimare.
- **bug regressivi**: si può assumere che il problema si trovava dove sono state effettuate le modifiche
- **refactoring**: il blame potrebbe non indicare esattamente quanto andare indietro, potendo esistere alcuni cambiamenti non significativi (es. cambio di nome di una variabile)
	- soluzione: posto sempre di non poter utilizzare l'affect version, se le modifiche non sono significative, si continua a passare alla versione precedente
- **snoring**: next lesson

Se c'è l'affect version, non si usa il blame e si considerano i metodi interessati come buggy a partire dalla versione indicata. Altrimenti, si usa il blame.

Il processo SZZ si compone di due fasi:
- identificazione delle entità buggy
- identificazione delle release buggy

Per identificare al meglio le versioni, il blame non sempre funziona, ma si può usare l'affected version di Jira.