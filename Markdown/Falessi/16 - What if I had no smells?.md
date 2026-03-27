Il concetto del paper è quantificare il numero di difetti quando non sono presenti smell.
- **failure**/**errore**: comportamento inatteso tangibile del sistema
- **bug**/**difetto**: porzione di codice che, se eseguita sotto determinate circostanze, produce una failure
- **smell**: porzione di codice che non impatta la qualità esterna del sistema, ma che potrebbe essere scritta meglio
Classi con più alta densità di violazioni presentano una maggiore defect injection frequency, ossia frequenza di difetti inseriti all'interno del codice per linea di codice toccata.

Facendo un'analisi what-if, si può stimare cosa sarebbe accaduto se il codice non avesse riportato code smell.
# Passi
## 1 - Seleziona delle metriche
Vengono definite una serie di metriche - che poi diventano feature -  sul codice in questione. Esempi sono:
- LOC
- NFix (numero di bug risolti)
- NAuth (numero di autori)
- NSmells (smell presenti)

Una volta definite, le metriche devono essere misurate. Questo può avvenire anche in istanti temporali diversi, dando vita a più versioni della stessa metrica.
## 2 - Manipolazione del dataset
Si supponga che A sia il dataset originale (supponendo anche di aver mantenuto solo il 20% più vecchio - quindi più affidabile - del dataset). Si definiscono:
- B+: porzione del dataset con un numero positivo di smell
- C: porzione di A senza smell (complemento di B+)
- B: deriva dalla manipolazione di B+, in cui il numero di smell è stato posto a zero

Ci si vuole chiedere, avendo osservato B+, qual'è la differenza con B predetto.

Si può osservare il valore (in media) delle feature in ogni porzione del dataset nella seguente tabella:
![[Pasted image 20250509120130.png]]

Per ogni variabile, si misura anche la correlazione con il numero di smell e la difettività. Il $\rho$-value indicato quantifica la correlazione.
## 3 - Selezione del modello
Dato il dataset A, si scelgono 4 classificatori (Begging, BayerNet, J48 e Logistic) e si utilizza la tecnica di cross-validation leave-one-out. Le misure considerate sono precision e recall.

Il modello migliore risulta essere bagging.
## 4 - Risultati
Si applica il modello di Bagging sui dataset A, B+, B e C, ossia si usano questi come test set. Nello specifico, i dataset A, B+ e C sono già stati visti in fase di addestramento, mentre B no, avendo una colonna modificata. I primi tre sono tuttavia utili a comprendere meglio i risultati su B.

Per costruzione, si ha che 69 difetti si 135 (circa la metà) non sono dovuti a smell. Tuttavia, i restanti 66 sono potenzialmente dovuti alla presenza di smell.

Di questi 66, se si rimuovono gli smell, si avrebbero avuto solo 38 difetti. Quindi, 66-38=28 sono i difetti potenzialmente causati da smell, vale a dire circa il 20% del totale.

![[Pasted image 20250509121842.png]]
## Validità dei risultati
A e B sono correlati positivamente se, se si osserva un aumento di A, si osserva anche un aumento di B e viceversa. La causalità indica che l'aumentare di A implica anche l'aumentare di B, ma questo non è sempre deducibile. Esiste anche la correlazione negativa, per cui all'aumentare di una A si osserva una diminuzione di B. L'azionabilità (actionability) indica che una variabile può essere modificata. Ad esempio:
- il numero di righe tocca in una fix non è actionable, dato che non si può dire a uno sviluppatore di modificare poche righe di codice
- il numero di smell introdotti è actionable, dato che si può limitare imponendo dei paletti
# Progetto
Fare una rivisitazione del paper per i progetti assegnati. In particolare:
1. creare A (milestone 1)
2. confrontare l'accuratezza di 3 classificatori su A (milestone 2)
3. scegliere il classificatore migliore
4. calcolare la correlazione di ogni feature con bugginess (con Spearman)
5. trovare la feature azionabile con la correlazione più alta con la bugginess (ad esempio il numero di smell), ossia AFeature
6. prendere dall'ultima release di A il metodo con maggior numero di smell (in generale, quello con più alto valore di AFeature), ossia AFMethod; questo è il metodo con beneficio atteso più alto nella rimozione di AFeature
7. modificare AFMethod e creare AFMethod2 tramite refactoring, diminuendo AFeature (portandola ragionevolmente a zero, anche se non sempre è possibile)
8. si ricalcolano tutte le feature su quel metodo, ossia AFeature2
9. confrontare le feature originali relative ad AFMethod con queste ultime

**Note**:
- il numero di autori aumenta quando si fa il refactoring
- il numero di release o commit può non variare, dato che si potrebbe dire "avrei potuto fare la stessa versione in quest'altro modo"

**Domande da porsi**:
- in AFMethod2, ci sono feature positivamente correlate con la bugginess che sono aumentate?
	- se sì, magari non è stata migliorata la manutenibilità in AFMethod2 rispetto a AFMethod
- stessa domanda, ma con correlazione negativa
	- se sì, magari è stata migliorata la manutenibilità

10. Creare:
	1. La porzione B+
	2. La porzione C
	3. La porzione B
11. addestrare il classificatore scelto su A, ossia BClassifierA
12. Predire A, B, B+, C e creare una tabella come nel paper (table 5)
13. analizzare la tabella e rispondere alla domanda: quanti metodi buggy potrebbero essere stati prevenuti avendo zero smell?
	1. in tutto
	2. in proporzione
	3. al di fuori da quelli prevenibili

Fare considerazioni sui progetti, facendo anche diverse prove.