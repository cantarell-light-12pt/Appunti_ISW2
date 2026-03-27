Le classi di equivalenza consentono di ridurre il dominio per il testing a un insieme finito e trattabile di classi.
## Linee guida generali per le classi di equivalenza
Per le attività di test, si possono considerare uno o più parametri di input:
- direttamente derivanti dal dominio del sistema (key abstraction, feature, necessità, requisiti)
- esplicitato al solo livello di astrazione considerato (funzione di una particolare scelta implementativa)

Si può partizionare lo spazio dei valori ammissibili basandosi alla conoscenza esplicita o implicita di:
- **range** (impliciti o espliciti): considerare un valore nel range e due valori al di fuori, uno per ogni lato del range
- **stringhe**: considerare almeno un insieme di stringhe tutte valide, un insieme di stringhe tutte non valide, un insieme di stringhe vuote e un insieme di stringhe nulle
- **enumerazioni**: considerare ogni valore assegnato a una classe di equivalenza differente
- **array**: considerare almeno una partizione con array tutti legali, una con array tutti vuoti e una dove tutti gli array superano la lunghezza massima consentita
- **tipi di dato complessi**: considerare partizioni applicando iterativamente i criteri generali ai dati nella struttura; considerare i valori nulli.

Queste linee guida sono generali ed è giusto considerarle, ma non in modo esclusivo. Spesso, classi di equivalenza significative possono essere dedotte dalla **semantica** del parametro/attributo/variabile considerata. Ad esempio, un attributo password si tipo stringa può essere una stringa vuota, `null`, valida nel dominio considerato (e di conseguenza, corretta o errata per l'utente) oppure non valida nel dominio corretto.

I concetti di validità e correttezza dipendono dal dominio considerato:
- un valore assegnato a un parametro è **valido** se rispetta le regole del dominio; il valore è **non valido** se non le rispetta
- un valore valido è **corretto** in base alla specifica configurazione del SUT.

Ad esempio, si consideri l'istruzione:
```
User u = DAOUser.getInstance().retrieveUser(email);
```
Si possono avere i seguenti scenari:
- `email="-11jud7@sjlkla9873@jjdud."` non è un valore valido per il parametro `email`
- `email="mrfoo@nothing.org"` è un valore valido, ma sua correttezza dipende dal SUT: infatti, l'utente con questa email potrebbe esistere o anche no.
## Criteri per identificare i test
L'identificazione dei test passa per la combinazione di due metodologie: unidimensionale e multidimensionale.

Nell'approccio **multidimensionale**, si considera il prodotto cartesiano dei parametri di input e si scelgono i test coprendo la matrice delle combinazioni delle classi di equivalenza, ossia si sceglie un rappresentate per ogni classe di equivalenza (dimensione) e si effettuano le varie combinazioni.

Nell'approccio **unidimensionale**, non si provano tutte le combinazioni di parametri, bensì, per ogni classe di equivalenza considerata, si fa in modo che esista un test che rappresenti quella classe di equivalenza.
## Procedura sistematica per il partizionamento del dominio
1. identificare i domini di input
	- dalla definizione di key abstraction, feature, needs, requisiti
	- dai tipi di dato legati a particolari scelte implementative
	- da condizioni imposte (esplicitamente o implicitamente)
2. identificare le classi di equivalenza per ogni parametro del test
3. combinare le classi di equivalenza
4. eliminare le combinazioni di classi di equivalenza non ammissibili
### Esempio
Si consideri l'operazione nella classe `LedgerHandler` di BookKeeper
```
void asyncReadEntriesInternal(long firstEntry, long lastEntry, ReadCallback cb, Object ctx, boolean isRecoveryRead)
```
**Nota**: per semplicità di sta considerando che:
- il SUT sia un singolo metodo; in generale, questo non avviene, ma viene considerato come SUT in intera classe, un package, un modulo ecc.

Si comincia a fare le classi di equivalenza in base al tipo di dato:
- `isRecoveryRead`: {true}, {false}
- `cb`: {null}, {"valid_instance"}, {"invalid_instance"}
- `ctx`: {null}, {"valid_instance"}, {"invalid_instance"}
- `firstEntry`: {<=0}, {>0}
- `lastEntry`: {<=0}, {>0}
- `LedgerHandle`: {vuoto}, {ha entry sufficienti}, {non ha entry sufficienti}

In questo caso, le dimensioni sono rappresentate dai parametri formali del metodo under test, più lo stato della classe che lo contiene.

Si possono definire classi di equivalenza meno grossolane?
Si può ragionare sul fatto che `firstEntry` e `lastEntry` devono essere relazionate:
- `firstEntry`: {<=0}, {>0}
- `lastEntry`: {<`firstEntry`}, {=`firstEntry`}, {>`firstEntry`}
Si possono raffinare anche le classi di equivalenza di `LedgerHandle`:
- `LedgerHandle`: {#istanze < `lastEntry-firstEntry`}, {#istanze >= `lastEntry-firstEntry`}

**Nota**: non è accettabile escludere i casi di input "invalid_instance" per i tipi di dato complesso, giustificando la scelta con considerazioni del tipo "Il costruttore ritorna solo istanze corrette". Infatti, il codice del costruttore potrebbe cambiare, introducendo bug, e soluzioni polimorfe potrebbero consentire di legare istanze di sottoclassi che fanno riferimento a implementazioni del costruttore diverse da quelle considerate.
Un'istanza invalida è creabile con un mock, oppure estendendo la classe considerata e facendo in modo che restituisca degli errori.

Una volta definite e combinate le classi di equivalenza, occorre trovare un rappresentante per ognuna. Teoricamente, posto che le classi siano corrette, scegliere un rappresentante qualunque può andare bene. Nella pratica, è più probabile che l'errore 
si trovi ai margini delle classi di equivalenza e quindi si scelgono valori ai bordi delle classi.

Si considerino ad esempio `firstEntry` e `lastEntry`, lasciando fissi gli altri parametri. In base al dominio e alla conoscenza del SUT, si possono scegliere non solo valori di boundary, ma anche valore un po' più dentro al bordo. Si hanno dunque i seguenti boundary values:
- `firstEntry`: -1; 0; 1
- `lastEntry`: `firstEntry-1`; `firstEntry`; `firstEntry+1`

Per l'approccio multidimensionale, si hanno le seguenti combinazioni: (ricopiare da slide 40).
# Large Language Models
I LLM sono implementazioni di grandi reti neurali, costruiti secondo specifiche architetture di riferimento, addestrati su enormi quantità di dati utilizzando approcci supervisionati.

Uno dei loro punti di forza principali è nell'interazione uomo-macchina in linguaggio naturale:
- i linguaggi di programmazione sono in genere liberi dal contesto e possono essere considerati come un sottoinsieme ristretto dell'insieme dei linguaggi naturali
- sfruttare le capacità dei LLM di elaborare informazioni da e per specifici linguaggi di programmazione

Nel SE, si possono sfruttare le capacità di elaborazione dei LLM per supportare aspetti di generazione automatica di artefatti software; in particolare, nel software testing, la generazione automatica di software test.
## Prompting
L'interazione con un LLM avviene tramite una richiesta in linguaggio naturale, detta **prompt**. È possibile definire una specifica struttura dei prompt da sottoporre: questa è abbastanza arbitraria e può riguardare aspetti sintattici, semantici, conversazionali e di contesto.
#