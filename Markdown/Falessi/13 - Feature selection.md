L'attività di **labeling** consiste nell'attribuire ad un metodo di una release il fatto che sia stato buggy o non buggy. Similmente, durante la fase di **predizione**, si predice se un metodo in una certa release è stato buggy o meno.

Una **feature** è una caratteristica delle istanze del dataset utile al predittore ad eseguire predizioni. In particolare, le colonne rappresentano le feature. Le feature devono essere **correlate** alla variabile di interesse, in modo da rendere la predizione più precisa possibile.

Dunque, dato un dataset, si può eseguire una **selezione delle feature**. Questa attività ha diverse motivazioni:
- riduzione del costo: ogni feature data in pasto al modello implica che questo faccia fitting su di essa, dunque il costo di addestramento aumenta per ogni feature presente nel dataset
- si hanno prestazioni migliori per quanto riguarda l'apprendimento rispetto all'utilizzo dell'intero insieme di attributi
- si diminuisce il costo di aggiornamento del dataset, dato che il numero di colonne impatta il costo delle misurazioni
Inoltre, ovviamente, a parità di prestazioni, è meglio un modello che utilizza meno feature piuttosto che uno che ne utilizza di più - anche indipendentemente dai costi.

Un modello di predizione deve eseguire predizioni corrette ed essere facilmente utilizzabile. A parità di prestazioni, si preferisce dunque un modello meno costoso e potenzialmente esplicabile.

Eliminando alcune feature, il modello potrebbe migliorare in termini di prestazioni per diversi motivi:
- ridondanza
- incorrettezza della feature (rispetto alla variabile di interesse)
# Metodi di feature selection
Due approcci:
- filtri
- wrapper
## Filtri
Si utilizzano concetti matematici (come information gain o spearman correlation) per misurare quanto le feature impattano la variabile di interesse.

Il problema del ranking non tiene conto delle correlazioni interne tra le feature, ma solo di come queste sono correlate alla variabile di interesse. Inoltre, non si può sapere quante feature scegliere tra le prime, non avendo una misura dell'impatto sull'accuratezza. Spesso, c'è un compromesso tra la rimozione delle colonne e il mantenimento. Idealmente, si vorrebbero delle feature molto correlate con la variabile di interesse, ma non tra loro.

Metrica EPV (Entity Per Value): valore minimo dell'occorrenza per ogni valore per ogni variabile.
## Wrapper
Per quanto l'approccio filtro sia veloce, non dà informazioni sulla correttezza della selezione delle feature.

L'approccio **wrapper** consiste nel dividere il dataset in tre parti: training, validation e test.
#### Approccio esaustivo
Si provano tutte le combinazioni di feature in tutte le loro cardinalità. "Provare" significa addestrare un modello su una data combinazione di feature e verificarne l'accuratezza. L'approccio include la selezione di un predittore e anche di una metrica di correttezza in base alla quale prendere decisioni.

Lo svantaggio di un approccio esaustivo non è che costa tanto, ma che costa troppo, dipendendo dal numero di feature. Inoltre, questo approccio potrebbe rimuovere feature importanti o non rimuoverne di meno importanti, dato che le scelte vengono eseguite solo sul test set.TU
#### Approcci greedy
Questo tipo di approcci non trovano la combinazione perfetta, ma la combinazione 
perfetta rispetto a una metrica di accuratezza.

Il feature set trovato
##### Backward search
Si va a ritroso:
- si prendono inizialmente in considerazione tutte le feature
- si rimuove una feature alla volta
- ci si ferma quando non si vede un miglioramento nei risultati

Esempio:
- si considerino 4 feature A, B, C, D
- si inizia con cardinalità 4: ABCD
	- si misura il valore dell'accuratezza
- si passa a cardinalità 3: ABC, ABC, ACD, BCD
	- se tutte queste restituiscano un valore minore di ABCD, si sceglie quest'ultima
	- se, ad esempio, il valore di ABC è maggiore o uguale di quello di ABCD
##### Forward search
Si inizia con la cardinalità minore e si aggiunge di volta in volta una feature, scegliendo sempre la combinazione migliore (e con cardinalità più bassa) e fermandosi quando se ne trova una peggiore.

Ci si aspetta - ma non è detto che sia così - che il numero di feature selezionate da forward search sia minore di quelle scelte da backward search. In generale, l'approccio razionale è applicare entrambe le tecniche, ma dipende tutto dal contesto. Se, ad esempio, si da che in produzione si possono utilizzare al massimo 10 feature, si può applicare backward search a partire da combinazioni di 10 feature.

Errore comune nel feature selection: utilizzare l'intero dataset al posto del solo training set. Utilizzare il tab classify, che utilizza meta-modelli al posto dei modelli veri e propri.