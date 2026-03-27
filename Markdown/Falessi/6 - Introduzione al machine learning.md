# Definizioni
Il **machine learning** è lo studio degli algoritmi che migliorano le loro prestazioni in determinati task dopo aver acquisito certa esperienza.

Si ottimizzano dei criteri di prestazioni usando dati di esempi o esperienza passata.

Il machine learning mette insieme statistica e informatica:
- **statistica** -> inferenza da un campione
- **informatica** -> algoritmi efficienti per risolvere il problema di ottimizzazione e rappresentare e valutare il modello per l'inferenza

Un classico esempio di uso del ML è il riconoscimento di cifre scritte a mano:
![[Pasted image 20250326115254.png]]

Il machine learning punta ad apprendere le associazioni tra delle **feature** $X$ e un **output** $Y$; in particolare, 
## Face recognition
Quando si parla di ML, si parla sempre di training set e test set.

Il **training set** è l'insieme di dati di addestramento del modello di ML, che consente di ottimizzare le prestazioni.

Il **test set** è un insieme di dati *non presenti nel test set* che verificano che il modello abbia buone prestazioni anche su dati nuovi che non ha mai visto.

[Face database](http://www.uk.research.att.com/facedatabase.html).
## Modelli predittivi
### Regressione
Uno dei modelli più semplici del ML è la regressione lineare.

Ad esempio, se si vuole predire il prezzo di una macchina, ipotizzando - per semplicità - che l'unica cosa che impatti il prezzo sia il numero di chilometri della macchina.

Siano $x$ un array contenente rappresentazioni numeriche degli attributi della macchina e $y$ il prezzo della stessa. Si può scrivere $y$ in funzione di $x$ come
$$
y = g(x | \theta)
$$
dove $g()$ rappresenta il modello e $\theta$ è l'insieme dei **parametri**.
Nel caso della regressione lineare,
$$
y=wx + w_0, \qquad \theta=(w, w_0)
$$
I parametri possono essere calcolati in infiniti modi: infatti, se la regressione lineare punta a trovare la retta che riduce maggiormente la distanza tra punti, il numero di modi per calcolare la distanza tra punti è infinito.
# Tipologie di apprendimento
Nel ML esistono 4 (principali) tipologie di apprendimento:
- **supervised learning** -> c'è una variabile di interesse che si vuole stimare e un insieme si dati storici sulla variabile
- **unsupervised learning** -> c'è una variabile da analizzare in base alle sue relazioni con altre variabili, ma senza avere i valori della variabile di interesse
- **semi-supervised learning** -> 

# Terminologia
L'**input** di un modello è un tabella di dati storici che gli vengono dati in pasto. Le colonne rappresentano le **feature** (o **attributi**). Un insieme di valori di più attributi appartenenti alla stessa riga rappresenta un'**istanza**, vale a dire l'esempio indipendente di un concetto da essere appreso dal modello. L'ultimo attributo nella tabella è quello da stimare.

Nel caso di classificazione di metodi come buggy o meno, si possono considerare diversi attributi: data della release in cui è presente il metodo, quanto è usato, quanto usa altri metodi, il numero di righe di codice e gli smell. Si potrebbe anche considerare il nome del metodo, ma:
- se sono già presenti tutti gli attributi necessari, potrebbe non essere necessario inserirlo
- si può considerare il nome del metodo per tracciare il numero di modifiche fatte

La dimensione di un dataset è data dal prodotto tra il numero di righe e numero di colonne. Tuttavia, non tutte le colonne hanno la stessa qualità, ossia non contribuiscono allo stesso modo alla predizione. Si può considerare la correlazione degli attributi con quello di predizione, ma non è sufficiente, essendo la predizione dovuta a più fattori che agiscono contemporaneamente. Ad esempio, la colonna `esperienza` e `haToccatoIlMetodoInPassato` singolarmente non indicano niente, ma insieme sì.

Oltre il concetto di qualità, è importante il concetto di variazione, ossia che le istanze siano rappresentate lo stesso numero di volte.

Quindi, è vero che *size does matter*, ma non è così semplice valutare la size. Ad esempio, sarebbe inutile duplicare ogni riga del dataset a parità di colonne.

Vedere EPV (entity per value).
## Attributi nominali

# Output
Ci sono diverse possibilità di rappresentare le informazioni in output:
- tabelle di decisione
- modelli lineari
- alberi decisionali
- cluster
# Explainable AI
Per quanto riguarda le regolazioni EU, tutti i sistemi basati su predizioni devono fornire informazioni non solo sulle predizioni, ma anche su come si è arrivati a quella predizione. Ad esempio, GPT e Gemini mostrano il ragionamento all'interno della risposta.

Ciò è dovuto al seguente motivo: si supponga di avere un meccanismo black-box (come le reti neurali, in cui i neuroni non rappresentano niente di concreto e servono solo ad astrarre i dati); d'altra parte, si hanno modelli come quelli di regressione e gli alberi decisionali in cui ogni elemento rappresenta una caratteristica del modello.

Ci si trova spesso di fronte a un compromesso: usare meccanismi precisi, senza sapere perché, e usare meccanismi imprecisi, ma sapere perché.

Weka non accetta variabili di tipo ordinali, né come input né come output. Ad esempio, LOW, MEDIUM e HIGH sono interpretati allo stesso modo di VERDE, GIALLO e ROSSO. Chiaramente, non vanno assegnati valori numerici a LOW, MEDIUM e HIGH.

C'è una relazione lineare tra il numero di bug e il numero di smell in una classe