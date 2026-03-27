Tutti i sistemi software possono presentare delle difformità rispetto alle attese dell'utilizzatore, dovute sia in parte all'utilizzatore, che non ne capisce l'utilizzo, sia a difetti stessi del sistema, dovuti ad errori avvenuti nelle fasi di analisi o progettazione.

Si vogliono introdurre delle garanzie per fare in modo che le richieste del cliente siano rispettate nel prodotto finale.
## Qualità dei sistemi software
Assicurare la qualità significa effettuare verifica e validazione del prodotto:
- **verifica** -> il prodotto è conforme alle specifiche?
	- conformità ai requisiti
	- esecuzione del giusto processo di realizzazione
	- in un certo momento dello sviluppo, si verifica che le specifiche fornite da qualcuno interno al processo di sviluppo siano rispettate
- **validazione** -> il prodotto che si sta sviluppando è effettivamente quello richiesto?
	- l'input è composto dai requisiti utente, che vengono applicati al prodotto sviluppato
	- si controlla che quanto fatto fino a un certo punto rispecchi le richieste dell'utente.

Tra gli obiettivi delle attività di V&V c'è rassicurare l'utilizzatore finale che il sistema sia adatto allo scopo richiesto.
Si intende fornire un **livello di fiducia**, il quale tuttavia dipende dal contesto, ossia da fattori quali:
- funzione del software all'interno dell'organizzazione
- attesa dell'utente
- politiche di mercato
### Contesto generale
Sia S una descrizione di un comportamento, ossia una **specifica** da realizzare con un programma. In particolare, una specifica S è una descrizione che dichiara per gli input in D quali sono gli output attesi in C.
![[Pasted image 20250311114703.png]]
Un **programma** P è una funzione da un dominio D a un codominio C, ossia una particolare realizzazione di una specifica che tenta di essere più precisa di essa nella realizzazione delle funzionalità descritte da S.

Una tecnica di V&V cerca di stabilire se un programma P soddisfa una specifica S. Ciò avviene se e soltanto se $\forall ~ d \in D ~ P(d)=S(d)$. Questa condizione può essere verificata del tutto o approssimata. S può essere sia una specifica utente (quindi si parla di validazione), sia una specifica interna (si parla di verifica).
### Approcci di V&V
Le tecniche di V&V possono essere utilizzate sia in verifica sia in validazione, a seconda del tipo di specifica:
- specifica di requisiti utente -> validazione
- specifica dei requisiti tecnici -> verifica

Esistono due approcci diametralmente opposti per la verifica della condizione di soddisfacimento di una specifica:
- **verifica formale**: si dimostra che il programma P soddisfa effettivamente la specifica S su ogni input
	- si rappresenta il programma come una rappresentazione matematica (grafi o clausole logiche) e si dimostrano dei teoremi su tale rappresentazione
		- ad esempio, si rappresenta il programma come un grafo e si verifica se esistono situazioni di deadlock
		- ancora, per i compilatori si può utilizzare una grammatica libera dal contesto (CFG) per verificarne il funzionamento, ossia verificare che il programma (visto come stringa) è derivabile dalla grammatica del linguaggio.
	- si effettuala verifica per ogni input possibile, oppure si trovano dei controesempi
	- approccio più auspicabile, ma non sempre fattibile
- **software testing and debugging**: si cercano di scoprire piccole imperfezioni nel programma P
	- anziché verificare un'astrazione matematica di P, si effettuano le verifiche direttamente su P
	- si accetta il rischio che P nasconda qualche imperfezione rispetto a S
#### Software testing e debugging
Il **software testing** è la pianificazione di strategie che evidenzino eventuali differenze tra P ed S.

Le attività atte a capire le cause delle differenze tra P ed S sono attività di **debugging**. Tramite debugging, è anche possibile risolvere tali anomalie.

Quanto detto fin'ora non è un processo, ma individua degli ambiti su cui si può lavorare.
### Correttezza
Una possibile (e banale) definizione di **correttezza** è l'assenza di errori. Tuttavia, un errore è solo una delle possibili cause di un malfunzionamento. Pertanto, si preferisce disporre un sistema privo di malfunzionamenti.

Tradizionalmente, si distinguono tre concetti in relazione tra loro:
$$
errore \implies difetto \implies malfunzionamento
$$
Quando si parla di errore, si parla del motivo che ha introdotto un difetto all'interno del sistema, il quale potrebbe - potenzialmente - portare a un malfunzionamento. Un errore può essere di varia natura (organizzativo, implementativo ecc.).

Si può evitare che il sistema raggiunga delle failure con meccanismi di **defect containing** tramite meccanismi di **failure prevention**. Ad esempio, per Open Street Maps, si può implementare un meccanismo che, impostando il luogo di partenza o destinazione su un'isola, la controparte deve essere posizionata all'interno della stessa isola, per evitare di consigliare tragitti nell'acqua. In questo modo, non si risolvono le cause del problema, ma si evita che l'utente finale sperimenti il malfunzionamento.

D'altra parte, si possono utilizzare tecniche di **defect reduction** tramite **fault detection and removal**, che consistono nell'individuazione delle cause dei fault e la loro risoluzione.

Infine, esistono tecniche di **defect prevention**, come l'utilizzo di particolari pattern, architetture o framework. Sono le più difficile da gestire, perché prendono in considerazione sia le parti tecniche, sia le parti organizzative di chi può produrre l'errore.
#### Failure, fault ed error
Una **failure** (malfunzionamento) si riferisce al comportamento non corretto e osservabile del sistema software. In particolare, una failure è presente quando il sistema (visto come una scatola nera) non si comporta come previsto, a prescindere dalla sua implementazione.

Un **fault** (bug, difetto) si riferisce allo specifico codice o modello. È una condizione necessaria (ma non sufficiente) affinché si presenti una failure. Infatti, una failure deriva dall'interazione con il sistema, che non sempre può andare a toccare la parte che presenta un fault. Un fault può consistere di:
- circostanze non previste
- azione non correttamente implementate/modellate

Infine, un **errore** è la causa di un fault. Generalmente, è un errore umano (concettuale, di interpretazione, di digitazione ecc.).

Il software testing si occupa di individuare le failure, mentre il debugging individua i fault e cerca di rimuoverli, cercando anche di capire gli errori che li causano.
#### Esempio
Si consideri la seguente funzione
```
1. int makeItDouble(int param) {
2.   int result;
3.   result = param * param;  <^>
4.   return result
5. }
```

La chiamata `makeItDouble(3)` restituisce `9`.
- `9` rappresenta la failure (comportamento errato)
- la failure è causata dal fault in `<^>`
- possibili fonti dell'errore sono:
	- un typo: è stato digitato `*`  al posto di `+`
	- un errore di interpretazione:
		- `double = param * 2`
		- `power = param * param`

D'altra parte, la chiamata `makeItDouble(2)` non evidenzia una failure, anzi fa sembrare che il programma funzioni correttamente. 
## Software testing
Il **software testing** consiste nell'esecuzione di alcuni **esperimenti** in un ambiente controllato al fine di acquisire sufficiente fiducia sul funzionamento. Di solito riguarda aspetti funzionali, ma può riguardare anche caratteristiche extra-funzionali.

Vantaggi:
- prova che il sistema (quello reale, non la rappresentazione) risponde alle esigenze per cui era stato creato (avendo almeno un test per ogni requisito)
- fa manifestare eventuali malfunzionamenti
Svantaggi:
> Il testing non può dimostrare l'assenza di guasti, ma solo la loro presenza ~ E.W. Dijkstra.

Un tipico contesto del software testing è composta da una procedura di test, in cui si effettua un setup, l'esecuzione di un'operazione (exercise) e la verifica delle asserzioni sul valore restituito dall'operazione.
- **inizializzazione** -> si inizializza il sistema under test
- **exercise** -> interazione della suit di test con il sistema inizializzato
- **verifica** -> controllo dei risultati ottenuti, asserendo delle condizioni; il passaggio positivo del test consiste nel match tra il risultato atteso e il risultato ricevuto dal sistema under test
- **teardown** -> si tira giù l'ambiente per l'under test
![[Pasted image 20250224124231.png]]
### Esempio
**Specifica**: fattoriale su tutti gli interi. Il fattoriale di un negativo è da intendersi come il fattoriale del suo opposto.
Si esegue la seguente implementazione:
```
int myBuggyFact(int p) {
	int result = abs(p);
	if (result > 1){
		int next = result–1;
		result = result*myBuggyFact(next);
	}
	return result;
}
```
Si supponga di voler testare questo programma. Occorre chiedersi:
- quanto è ampio il dominio su cui fare il test?
- quanto è costoso esplorare l'intero dominio di input?
- si può considerare solo una sua sottoparte
- tutti i possibili input sono "equivalenti" tra loro?
### Domande fondamentali del software testing
- Quali input selezionare?
	- Si possono scegliere valori rappresentativi, oppure partizionare il dominio di input e identificare delle classi di equivalenza rispetto a un obiettivo
- Quando smettere di testare?
	- Esistono delle definizioni di criteri di copertura (statement, archi, condizioni, flussi, funzionalità ecc.), così come valori minimi per l'accettazione
- Come si capisce se il risultato di un'operazione è corretto?
	- Si può creare un oracolo che restituisce sempre il risultato corretto (*oracle problem*), oppure lo si può dedurre da risultati storici del sistema o da sistemi equivalenti in esecuzione.
### Correttezza e reliability
Per quanto detto fin'ora, la definizione iniziale di correttezza risulta essere troppo banale.

Si definisce dunque la **correttezza** come l'aderenza completa di un sistema alle sue specifiche. Essa viene asserita tramite prove formali, ossia ragionamenti logico/matematici. La validazione tramite testing richiederebbe l'esplorazione esaustiva del dominio di input.

D'altro lato, si definisce **affidabilità** (**reliability**) la stima della probabilità che un'operazione con il programma, a partire da un insieme di parametri di ingresso <u>estratti casualmente</u> dal dominio di input del programma, sia di successo. La reliability è un attributo statico del sistema, non un attributo di correttezza. In particolare, la reliability è pari a 1 meno la probabilità di errore (PFD, Probability of Failure on Demand).
#### Esempio
Si consideri il metodo `myBuggyFact` è si supponga che, nella pratica, il metodo verrà invocato in maniera equiprobabile con uno dei seguenti parametri: `[-5, -3, 0, 2, 4]`.

La PFD di `myBuggyFact` è 0.2, dunque la reliability è 0.8.
### Profilo operazionale
Si introduce il **profilo operazionale**: è una descrizione numerica del modo o un insieme di modi di utilizzo del sistema. Ad esempio, il sistema viene utilizzato da queste tipologie di utenti su queste funzionalità su queste percentuali di invocazioni di metodi e su queste percentuali di invocazioni di feature. In sostanza, è un insieme di descrizioni di come diversi attori interagiscono con le boundary del sistema e lo stimolano.

Ad esempio, per `myBuggyFactorial`, la reliability è $1 - p(0)$, dove $p(0)$ è la probabilità che venga inserito 0, ossia l'`80%`, vale a dire in questa percentuale di utilizzo il sistema apparirà funzionare correttamente.

Cambiando il profilo operazionale, cambia anche la percezione del sistema: esso permette di calcolare la reliability su un'idea di utilizzo del sistema, ma non la reliability in generale. In generale, la misura di "bontà" del sistema vale solo per il dominio di utilizzo definito dal profilo operazionale.

Sostanzialmente, un profilo operazionale è una descrizione numerica del sistema ed è il punto di partenza per calcolare i diversi profili di reliability, ossia avere una confidenza dell'adeguatezza del sistema allo scenario ipotizzato. Averne uno aiuta nella pianificazione delle attività di test nei confronti degli utilizzatori, ma non sempre è utile basare una fase di test su di esso.

Si immagini, ad esempio, che ogni volta che una macchina finisca la benzina esploda. Un profilo operazionale sull'utilizzo tipico della macchina, ossia con il serbatoio pieno, non consentirebbe di testare il caso in cui il serbatoio si svuoti, lasciando un serio problema sconosciuto nel sistema.

Usare il profilo operazionale significa guardare l'utilizzo tipico del sistema in un certo contesto derivato dalla fase di analisi (happy scenario o worst case scenario).

Nel report del progetto, partendo dall'insieme di test progettati e sviluppati, si devono fare queste considerazioni sulla reliability del sistema al variare degli input inseriti.
## Extra (dalla lezione successiva)
Le tecniche di verifica formale consente di avere una prova logico/matematica della correttezza di un sistema, o meglio di una sua rappresentazione logico/matematica (ad esempio un grafo o delle istruzioni formali). Tuttavia, questo dipende anche da come viene ricavata la rappresentazione.

D'altra parte, il software testing consente di scoprire gli errori in un sistema, ma non consente di affermarne l'assenza. Infatti, il testing consente di esplorare solo in un sottoinsieme del dominio di input, nella speranza che gli errori si trovino in una piccola sottoparte del dominio. Ci vuole bravura nel trovare questa sottoparte.

Se si utilizza il software testing, non si può ragionare della correttezza come l'assenza totale di errori nel sistema: aver passato una o più fasi del testing non assicura la sicurezza del sistema, dato che potrebbero esserci failure che non sono state esposte, non raggiungibili oppure è stato usato un sottoinsieme non adeguato del dominio di input.

Se non ci si trova in questi ultimi casi, si può parlare di **probabilità** che non si trovi un bug all'interno del sistema. In particolare, questa probabilità è una stima della reliability (attributo statistico): sotto certe condizioni il sistema aderisce alle specifiche con questa probabilità.