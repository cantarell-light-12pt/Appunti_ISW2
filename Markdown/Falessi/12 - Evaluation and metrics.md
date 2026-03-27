# Valutazione di un classificatore
In qualsiasi sistema informatico che include una componente di ML, questa deve essere validata oltre che configurata. Occorre quindi stabilire quanto un classificatore è accurato.

Perché? Perché il futuro è simile al passato; l'idea è dunque che non ci interessa capire quanto il classificatore è accurato sui dati che già si hanno, piuttosto quanto lo sarà su dati nuovi.
## Training e testing
Ci si concentra su classificatori binari. Si ha sempre un valore *actual*, ossia quello reale del'istanza, e uno *predicted*, predetto dal classificatore.
## Dataset
In generale, per capire quanto è accurato un classificatore, è necessario avere a disposizione un dataset.

Esistono diverse tecniche di validazione dei classificatori. In generale, una tecnica migliore indica il classificatore migliore, ma dipende sempre da caso a caso. Ergo, non esiste una metrica sempre migliore.
### Stima holdout
Il metodo **holdout** consiste nella separazione del dataset in training set e test set.

Il training test serve a calibrare il modello, mentre il test set permette di valutare quanto il modello sia affidabile su dati nuovi.

Il problema di questa tecnica è che i campioni dei set potrebbero non essere rappresentativi della realtà, altrimenti il modello non potrà fare predizioni accurate. Di conseguenza, si vogliono avere dei set rappresentativi, sia nel training che nel test.

Un altro problema è la proporzione della variabile di interesse sulla quale di fanno predizioni: è necessario che training set e test set rappresentino questa variabile in maniera proporzionata. Si possono quindi avere i due set **stratificati**: training set e test set avranno la stessa proporzione delle istanze della variabile di interesse. In realtà, questa tecnica dovrebbe essere applicata su tutte le variabili, ma questo non è possibile per tutte.
### Metodo repeated holdout
Se i dati non sono ordinati e il tempo non ha impatto, si può ripetere la stessa tecnica di holdout su diversi sotto-campioni del dataset. In particolare, si vorrebbe anche che tutti i campioni venissero usati lo stesso numero di volte sia nel training che nel test set. 

C'è un punto di incontro tra il costo della valutazione e l'accuratezza del modello finale.
### K-fold cross-validation
**K-fold cross-validation** è una tecnica di validazione che evita la sovrapposizione dei test set, in modo che tutte le parti saranno usate nel test set lo stesso numero di volte.
1. si divide il dataset in K sottoinsieme di egual misura
2. si usa ogni sottoinsieme alla volta per il testing e il restante per il training
Ciò implica che l'algoritmo di addestramento è applicato a k training set diversi.

Per aumentare la randomness dei dati, si può applicare questa tecnica più volte, assegnando i dati a parti ("foglie") diverse.

In generale, per ogni coppia training-test set si ha poi un valore di accuratezza, dei quali si calcola la media. Si può allora definire il modello migliore, dunque si riaddestra il modello da zero. Il k-fold viene ogni volta applicato sul modello non addestrato.

La pratica e la ricerca dicono che k-fold va applicato 10 volte.
### Leave-one-out cross-validation
Pro: tutti
Contro: costo computazionale

Si toglie una sola riga alla volta per il test set, rendendo il dataset più simile possibile a quello di produzione.
### Bootstrap
In generale, i dati del test set non devono essere presenti nel training set.

Tuttavia, la tecnica **bootstrap** consente di costruire un training set utilizzando un campionamento dei dati dal dataset con rimpiazzo, mentre il test test è composto dalle istanze rimanenti. Dato che il training set potrebbe contenere lo stesso dato ripetuto più volte, oppure non presentare per nulla un certo dato, comporta diversi rischi, come quello di overfitting.
### Tecniche di validazione per serie temporali
Spesso è necessario mantenere l'ordine dei dati per la creazione di training e test set.
#### Ordered holdout
Come holdout, ma il training è composto dalle prime istanze e il test dalle ultime.
#### Walk-forward
Il walk-forward consente di dividere il dataset in parti uguali, che vengono poi ordinate temporalmente. Ad ogni esecuzione, per il training set vengono utilizzati tutti i dati precedenti alla parte considerata, mentre essa è usata per il test set.

**Concept drift**: i dati lontani temporalmente non sono così realistici rispetto ai dati più vicini temporalmente. Dunque, alcuni dei dati passati devono essere rimossi, ma occorre capire quanti.
#### Sliding window
Per risolvere questo problema, si può utilizzare un approccio sliding window, in cui ti tiene conto solo delle ultime N unità del dataset per il training. Il problema principale è la dimensione della finestra: troppo piccola implica pochi dati, troppo grande non risolve il problema di prima.

**Nota paper**: le tecniche che mantengono l'ordine dei dati hanno una maggiore accuratezza delle altre.
# Metriche di prestazioni
Il concetto di accuratezza è definito in base al dato considerato come positivo. La definizione di un positivo è predisposta dal dominio.

In generale, un falso negativo ha costi maggiori rispetto a un falso positivo.

Si hanno le seguenti metriche:
- **recall**: $TP \over TP+FN$ -> la proporzione di istanze positive classificate come positive sul numero totale di istanze realmente positive (tra cui quelle classificate erroneamente come negative)
	- indica quante volte ci si prende sul totale
	- problema: non basta, perché basta classificare tutte le istanze come positive per ottenere una recall alta
- **precision**: $TP \over TP + FP$ -> la proporzione di istanze positive classificate come positive sul numero di istanze classificate come positive ("a lupo a lupo")
	- indica quante volte la predizione di positive era corretta
	- in generale, a parità di condizioni, si preferisce avere una recall alta piuttosto che una precision alta, dato che un falso negativo costa di più di un falso positivo
	- problema: si ottiene facilmente classificando tutto come negativo
- **F1-score**: media armonica tra precision e recall
- **accuratezza**: $Tx \over Tx + Fx$ -> in realtà non implica niente
- **Kappa**: indica l'accuratezza rispetto a un classificatore dummy (random)
	- misurare il Kappa (deve essere in teoria tra 0.2 e 0.4)
## Curve ROC
Precision e recall sono affette dal problema della threshold.

Si calcola una curva che calcola precision e recall al variare di ogni threshold e poi si calcola l'area sotto la curva (AUC) che indica il valore dell'accuratezza.

Problema: non tutte le threshold valgono ugualmente, dato che ognuna ha un significato diverso. L'idea dovrebbe dunque essere definire un insieme di threshold che abbia senso, attribuire a ognuna un peso e poi fare un media pesata.