1. Creare il dataset A (milestone 1)
2. Confrontare l'accuratezza di tre classificatori su A (milestone 2)
3. scegliere il migliore classificatore (BClassifier)
4. Calcolare la correlazione di ogni feature con la bugginess
5. Trovare la feature actionable con la maggior correlazione con la bugginess (ad esempio il numero di smell), ossia AFeature
6. Prendere, dall'ultima release di A, il metodo che ha AFeature massima, ossia AFMethod; questo metodo è quello con il maggior beneficio atteso nella rimozione di AFeature)
7. Creare AFMethod2 facendo refactoring di AFMethod, decrementando AFeature (possibilmente portandola a zero)
8. Si ricalcolano tutte le feature per AFMethod2, ossia Feature 2
9. Confrontare le Feature 1 (calcolate per AFMethod su A) con Feature 2
# Domande preliminari
- si può verificare se, tra le feature positivamente correlate con la bugginess, alcune sono aumentate a seguito del refactoring
	- in tal caso, non si è migliorata la manutenibilità
- si può verificare se, tra le feature negativamente correlate con la bugginess, alcune sono aumentate a seguito del refactoring
	- in tal caso, la manutenibilità è stata aumentata

In generale, si deve scegliere un metodo che o diminuisca le variabili positivamente correlate con la bugginess, oppure aumenti quelle negativamente correlate.
# What-if scenario
10. Creare
	1. B+: porzione di A in cui il numero di smell è positivo
	2. C: porzione di A con numero di smell pari a zero
	3. B: alterazione di B+ in cui il numero di smell viene forzatamente posto a zero
11. Addestrare il classificatore BClassifier su A, ottenendo BClassifierA
12. Predire A, B, B+, C e creare una tabella come nella slide 14
13. Analizzare la tabella e rispondere alla domanda: quanti metodi buggy si sarebbero potuti prevenire avendo zero smell?
	1. in totale
	2. in proporzione
	3. al di fuori di quelli prevenibili
# Report
1. Introduzione
2. Motivazione per la scelta del metodo
3. Descrizione del metodo
4. Criteri di refactoring all'interno del metodo
5. Risultati del refactoring
6. Analisi what-if a seguito del refactoring del metodo

OGNI PARTE DEVE ESSERE ESTREMAMENTE SINTETICA.
# Domande
- quante feature?
	- vedere slide milestone 1: due tipi di feature, quelle relative ai cambiamenti possono essere misurate all'inizio e alla fine della release e sono actionable; le feature relative agli smell sono invece azionabili; in totale circa 10-20 feature, tra cui alcune actionable e altre no; descrivere ogni singola scelta effettuata
- approccio da seguire: nessuna inferenza, seguire esattamente lo schema di "What if I had no smells?"
- interdipendenze tra numero di smell e altre feature
	- non è un problema
- calcolo del numero di smell
	- è possibile effettuarlo in molti modi: in totale, per tipo, per categoria ecc; in ogni caso, bisogna essere precisi
- rimuovere almeno il 50% delle release
- per vedere se l'approccio funziona, ci si aspetta un valore di Kappa di almeno 0.2-0.3; se è minore, la misurazione non è accurata, a meno che non si abbia un valore alto di AUC.
- se Opening version coincide con fixing version, il bug non contribuisce al calcolo della proportion e può essere ignorato; infatti il metodo non è buggy. Si può dire che si prendono solo i bug in produzione
- le diverse versioni devono essere ordinate in maniera crescente sul numero della versione, a prescindere se è una major o minor release
- risentire domande sulla correttezza della predizione (min. ~34)
	- in generale, si vogliono le feature più precise possibili, quindi si possono calcolare ad oggi
- quale tecnica usare per validare i classificatori?
	- si ha libera scelta, se ne possono usare anche più di una e confrontarle; vedere paper "Preserving the order of data..."
- la qualità della codebase ha influenzato lo sviluppo del codice, ma l'influenza è dovuta alla release precedente; quindi si possono calcolare gli smell sull'ultimo commit della versione precedente per confrontarlo con i bug alla fine della release corrente
- per ogni argomento trattato, non concentrarsi sulle definizioni, quanto sulla capacità di ragionare al momento sulle domande poste
	- diverse opzioni, aspetti positivi e negativi di ognuna, quando si usano
- alcune domande note:
	- schema architetturale (slide 31 SW Architecture)
	- function point
	- metriche, misure, misurazioni, tipologie di scala
	- GQM
	- planning poker
	- lezione MBDA
	- refactoring
	- cos'è il ML
	- cos'è feature selection e metodi
	- SZZ
	- snoring
	- presentazione Calavaro
	- presentazione La Prova
	- cos'è proportion
	- approccio what-if
	- cos'è una presentazione
- di solito, una domanda è sul ML e una sul SW Engineering