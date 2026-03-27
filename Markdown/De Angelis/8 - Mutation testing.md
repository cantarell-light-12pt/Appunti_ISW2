Le metriche di code coverage possono convincere l'interlocutore che l'approccio ai test è stato eseguito in maniera seria, ma non escludono la presenza di problemi nel sistema. Infatti, queste metriche misurano la copertura della profondità di esplorazione del SUT (o delle sue specifiche), ma non indicano nulla in termini di capacità di individuazione dei difetti da parte della test suite.

Un'altra categoria di tecniche di test ha come obiettivo non tanto quanto i test esplorino il sistema, quanto siano in grado di scoprire gli errori. In particolare, si cercano questi problemi introducendo artificialmente delle alterazioni nel SUT.

Questo approccio è detto **mutation testing**, dato che si altera artificialmente il SUT. Si vuole in particolare stimare l'adeguatezza della test suite esistente in base alla sua:
- capacità (potenziale) di rilevare errori
- robustezza rispetto ad evoluzioni nel codice
# Schema di processo
0. Si prende il programma P, ossia il SUT.
1. Per questo programma, si generano tante versioni alternative a partire dall'originale: si generano quindi M *mutanti*. Le mutazioni possono essere di vario tipo:
	- cambiare un `+` con un `-`, o un `>` con un `<`
	- cambiare una clausola `&&` con una `||`
	- cambiare l'ordine di valutazione di un'espressione booleana composta
2. Generati tutti i mutanti, si eseguono i test su P. Se non sono stati considerati tutti i mutanti, se ne prende uno e si eseguono i test su di esso. Si confrontano i risultati dei singoli test sul mutante M e su P:
3. Se sono diversi, significa che è stato rilevato l'errore e si aggiunge il mutante all'insieme dei mutanti individuati (o uccisi)
4. Se sono uguali, si continuano a provare i test nella test suite
5. Una volta considerati tutti i test, se hanno avuto gli stessi risultati del SUT, si rimette il mutante nell'insieme dei mutanti vivi
6. Una volta eseguiti i test su tutti i mutanti, si eliminano i mutanti equivalenti, ossia quei mutanti le cui modifiche rispetto al SUT non sono sostanziali (ad esempio un `+0` alla fine di un'operazione di somma); individuare questi mutanti equivalenti non è semplice
7. Si calcola un indice di adeguatezza in funzione de:
	- i mutanti generati
	- i mutanti equivalenti
	- i mutanti uccisi
![[Pasted image 20250520115411.png]]
- L: mutanti vivi
- D: mutanti rilevati/uccisi
- E: mutanti equivalenti
- Linee continue: flusso di controllo
- Linee tratteggiate: trasferimento di dati o artefatti

Mentre gli approcci control-flow puntano ad esplorare il codice, mentre gli approcci di mutation testing stimano la fault-detection del SUT. Ad esempio, un programma composto da una sola riga di codice può avere un MCDC del 100%, ma quella singola riga potrebbe contenere un errore nonostante la coverage.
## Modalità di calcolo dell'indice di adeguatezza
### Mutation score
Il **mutation score** è il numero di mutanti rilevati (o uccisi) dall'insieme di test T rispetto al numero totale di mutanti rimasti non rilevati. La misura è
$$
|D|\over|L|+|D|
$$
dove
- $D$ è l'insieme dei mutanti rilevati
- $L$ è l'insieme di mutanti vivi

Criteri:
- se $|L|=0$, allora T è una test suite adeguata per il SUT e il mutation score è 1
- se $|D|=0$, allora T non scopre nessun mutante del SUT e il mutation score è 0
### Mutation score alternativo
Il **mutation score alternativo** è il numero di mutanti rilevati dall'insieme di test T rispetto al numero totali di mutanti generati, ma non equivalenti al SUT. La misura è
$$
|D| \over |M|-|E|
$$
dove:
- $D$ è l'insieme dei mutanti rilevati
- $M$ è l'insieme dei mutanti generati dal SUT
- $E$ è l'insieme dei mutanti equivalenti al SUT (calcolati tra i soli mutanti vivi rimasti)

Criteri:
- se tutti i mutanti vengono rilevati, ossia se $D=M$, allora T è una test suite adeguata per il SUT e il mutation score è 1
- se $|D|=0$, allora T non scopre alcun mutante del SUT e il mutation score è 0.
## Spunti su procedure alternative per mutation testing
Si possono utilizzare diverse varianti dell'approccio:
1. tutti i mutanti sono generati prima dell'esecuzione dei test:
	- si possono usare approcci incrementali per gestire meglio la generazione di mutanti in termini di efficienza (poiché si gestiscono insiemi di mutanti ridotti) ed efficacia (ad esempio privilegiando la generazione di mutanti più significativi per il contesto)
	- l'evoluzione è iterativa rispetto al tipo di operatori di mutazione considerati:
		- il calcolo del mutation score avviene rispetto ai soli operatori considerati
		- se il mutation score non è 1, occorre far evolvere la test suite
		- se il mutation score è 1, allora si può considerare un nuovo operatore di mutazione
2. evoluzione incrementale dell'insieme di test
	- un minor numero di mutanti generati porta limitazioni sui mutanti non rilevati, pertanto approcci iterativi potrebbero consentire un'agevolazione sull'evoluzione dei casi di test
3. partizionamento degli operatori di mutazione:
	- parallelizzazione dei processo di mutation testing
	- ogni tester ha a disposizione un sotto-insieme di operatori di mutazione applicabili durante il processo
	- evoluzione congiunta dei casi di test
	- adeguatezza locale rispetto al sotto-insieme di operatori considerati
	- si combinano i risultati ottenuti da ogni tester
## Condizioni per il rilevamento di un mutante
Un mutante M di un SUT viene ucciso se valgono tutte le seguenti condizioni:
- **C1: raggiungibilità**: esiste un execution path dal punto di attivazione di M allo statement mutano dal SUT
- **C2: infezione dello stato**: lo stato di M e quello del SUT differiscono a seguito dell'esecuzione dello statement mutato
- **C3: propagazione dello stato**: a seguito dell'attivazione dello statement mutato, l'impatto sullo stato di M si propaga fino al termine dell'esecuzione considerata.

Si può dire che M è equivalente al SUT se non esiste un test case nel dominio di input che soddisfi contemporaneamente le condizioni C1, C2 e C3.
## Chiarimenti
Il mutation testing ha delle chiare proprietà.
- TUTTI gli approcci di mutation assumo la vera "Competent Programmer Hypotesis" (CPH),ossia che il programmatore conosca o abbia compreso l'algoritmo da realizzare, ma durante l'implementazione potrebbe commettere una serie di errori inserendo fault, per lo più sintattici. In sostanza, il programmatore è in grado di scrivere un programma non troppo diverso dal programma finale corretto.
	- ad esempio, l'approccio non funziona se nel programma "fattoriale" è stata implementata la logica per il calcolo della radice quadrata e i test (input e risultati attesi) sono pensati in tal modo.
- in generale, stabilire l'equivalenza tra SUT e M è indicibile, ossia non è possibile definire un algoritmo che, presi in ingresso un qualunque SUT e M, indica se sono equivalenti.
- nella pratica, spesso è il tester che verifica l'equivalenza tra istanze specifiche di SUT e M, facendo un'attenta analisi dei modelli, delle specifiche e del codice, o tramite debugging
- M si comporta come (o è equivalente a) il SUT se a parità di input:
	- ritorna gli stessi output del SUT (**strong mutation**, osservazioni esterne)
		- si vede la differenza solo in termini d output del sistema
		- spesso non è sufficiente, dato che lo stesso output potrebbe derivare da ragionamenti o calcoli diversi che il SUT esegue, oppure si vuole anche solo  capire il perché del risultato
	- presenta gli stessi stati intermedi (come valori di variabili locali, execution path ecc.) osservati nel SUT (**weak mutation**, osservazioni interne)
	- M potrebbe essere equivalente al SUT rispetto a strong mutation, ma non rispetto a weak mutation: infatti, M potrebbe evidenziare lo stesso risultato osservabile, ma ha percorso diversi execution path nell'esecuzione concreta.
## Applicazioni
L'applicazione di mutation testing può avvenire su progetti software esistenti. Ad esempio, si potrebbe implementare la procedura di mutation testing in un programma. Occorre considerare:
- gli operatori da dare al sistema di generazione di mutanti (ad esempio, scambia un `+` con un `-`)
- definizione di dove applicare le mutazioni
- definizione di come applicare le mutazioni (non random, dato che in più iterazioni si potrebbero avere risultati differenti)
### PIT
**PIT** ([ref](http://pitest.org/)) è uno strumento per la generazione automatica di mutanti e l'esecuzione di unit test sui mutanti. È in grado di generare anche report delle esecuzioni e sulle mutazioni rilevate. Aiuta ad evidenziare imperfezioni o mancanze nell'insieme di casi di test sviluppati.

È integrato con Maven:
- [Quickstart - Maven](http://pitest.org/quickstart/maven/)
- [Quickstart - mutators](http://pitest.org/quickstart/mutators/)
- [Quickstart - incremental analysis](http://pitest.org/quickstart/incremental_analysis/) per le strategie incrementali del mutation testing

Potrebbe essere utile andare a vedere i profili di Maven, ossia i modi di utilizzo del ciclo di vita. Si potrebbe ad esempio creare un profilo per mutation testing e lanciarlo solo quando la test suite diventa solida.

Vedere il test strenght di PIT.
## Per il progetto
1. generazione di test case in vari modi
	1. manuale
	2. più o meno automatizzati
	3. automatici (LLM)
2. validare la qualità dei casi di test dal punto di vista del control flow coverage con Jacoco
3. si definiscono altri casi di test per aumentare la coverage (sempre nei modi visti prima, partendo dalle funzionalità della classe, in maniera white-box (scoraggiata), o tramite LLM)
4. si ripetono i punti 2 e 3 finché gli indici non sono soddisfacenti
5. si applica mutation testing tramite PIT
6. in base ai risultati, si fa evolvere la test suite (sconsigliato creare test appositi per i singoli mutanti, ma cercare d coprire uno spettro di soluzioni con il test)