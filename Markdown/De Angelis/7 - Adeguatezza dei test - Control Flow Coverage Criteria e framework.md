**Nota**: se serve testare una classe singleton, si estende la classe e si eseguono i test utilizzando una classe figlia del singleton. In generale, occorre stare attenti quando si ha a che fare con metodi statici che fanno uso di attributi statici.
## Criteri di adeguatezza basati sul control flow
### Condition coverage
La **condition coverage**  è una metrica che misura la copertura dell'insieme di test T rispetto alle espressioni condizionali (semplici/atomiche o composte) presenti nel SUT. {In particolare, si verifica la copertura dell'albero/grafo computazionale definito dal codice (come rami dei costrutti if-else).} -> rivedere {}
$$
{|C_c| \over |C_e|-|C_i|}
$$
dove:
- $C_c$ è l'insieme delle condizioni validate a seguito dell'esecuzione dell'insieme di test T
	- condizioni semplici: è valida/coperta se T include almeno un caso di test che la rende `true` e uno che la rende `false`
	- condizioni composte: è valida/coperta se T include casi di test che validino/coprano ogni singola condizione che la compone
- $C_e$ è l'insieme delle espressioni condizionali nel SUT
- $C_i$ è l'insieme delle espressioni condizionali non soddisfacibili nel SUT (si sa a prescindere o si assume che sia così)

Criteri:
- T è adeguato per il SUT se la sua condition coverage è 1
	- è necessaria per le implicazioni formali tra criteri di adeguatezza
- T è adeguato per il SUT se la sua condition coverage è almeno X%.

A differenza della BDC, dove si vogliono percorrere tutti i possibili archi che portano a una determinata scelta, qui non basta esplorare tutti i casi dell'if-else o del case, ma si vuole analizzare il motivo per cui si è finiti in tali rami e ciò si fa analizzando tutte le condizioni che compongono la guardia.

**Problema: lazy evaluation**. La lazy evaluation è il modo (in Java e in altri linguaggi) di terminare l'analisi di un'espressione booleana quando ne si ricava il risultato già a una delle espressioni che la compongono. Ad esempio, nella condizione composta `a!=0 && a>b`, la condizione atomica `a>b` non sarà valutata se `a=0`, che rende la condizione composta `false`.
Questo potrebbe pregiudicare l'effettiva applicabilità di strategie orientate alla copertura si condizioni composte. Per questo motivo, la condition coverage si applica normalmente a condizioni semplici o atomiche, escludendo quelle composte.

La definizione viene quindi adattata:
$$
{|C_c| \over 2\cdot(|C_e|-|C_i|)}
$$
dove:
- $C_e$ deve essere considerato solo per condizioni semplici, che sono valide se T include almeno un caso di test che la renda `true` OPPURE `false`, ma non entrambe.
	- deve essere contato su due casi per ogni condizione semplice: `true` e `false`
- per ogni condizione semplice, $C_c$ dovrebbe riferire i due casi `true` e `false`
- per quanto dia una granularità maggiore, la metrica soffre della (potenziale) interdipendenza tra le varie condizioni semplici.
### Esempio
Ad esempio, per un semplice SUT:
```
bool areBothParamsPositive(int i, int j) {
	bool result;
	if (i>0 && j>0) {
		result = true;
	} else {
		result = false;
	}
	return result;
}
```
vengono generate due test suite:
- $T_{Dec}$:
	- `t1:([-3;2], false)`
	- `t2:([4;2], true)`
- $T_{Cond}$
	- `t3:([-3;2], false)`
	- `t4:([21;-1], false)`
Si trova che $T_{Dec}$ è adeguato a branch decision coverage, mentre $T_{Cond}$ è adeguata rispetto a condition coverage.

Problema: in queste suite di casi, per quanto singolarmente adeguate per i singoli tipo di coverage, non esiste un caso di test che sia valido per entrambe:
- $T_{Dec}$ non considera i casi in cui `j<0`
- $T_{Cond}$ non considera il ramo in cui la condizione è `true`

Quindi, la CC non implica la BDC e viceversa. Segue che massimizzare una sola delle due metriche potrebbe non portare a nulla di utile.
### Branch condition coverage
La **branch condition coverage** (ossia **condition/decision coverage**) è una metrica che fonde BDC e CC, considerando la copertura congiunta dell'insieme di test T rispetto ai punti di scelta/decisione e alle espressioni condizionali semplici/atomiche presenti nel SUT.
### Multiple condition coverage
La **multiple condition coverage** considera la copertura dell'insieme di test T rispetto a tutte le possibili combinazioni di espressioni condizionali semplici/atomiche presenti nel SUT.

Questa misura è generalmente molto dispendiosa e spesso non applicabile.
### Modified condition/decision coverage
La MC/DC è una copertura congiunta dell'insieme di test suite T rispetto ai punti di scelta/decisione e tutte le espressioni condizionali semplici/atomiche presenti nel SUT, ma evitando l'esplosione combinatoria degli stati.

Criteri richiesti:
- ogni blocco del SUT è coperto da T (block coverage = 1)
- ogni condizione semplice/atomica nel SUT è coperta sia con `true`, sia con `false` in T (condition coverage = 1)
- ogni decisione è stata coperta in ogni possibile ramo da T (condition coverage = 1)

La misura è dunque:
$$
MC_C={\sum_{j=1}^N e_j \over \sum_{k=1}^N (s_k-i_k)}
$$
Non si può dire che sono stati coperti tutti i possibili casi, ma ne è stata fatta un'ottima approssimazione.
## Conclusione
Queste metriche sono state viste negli esempi con il codice a disposizione, ma possono essere calcolate anche a partire da activity diagram. In particolare, nel primo caso si guarda a una rappresentazione eseguibile del SUT, mentre nel secondo si guarda a una sua astrazione.
# Framework e tool
Esistono diversi tool che possono essere utilizzati sia lato client, sia lato server per la CI/CD che consentono di definire software testing tramite approcci di control-flow:
- Jacoco
- Emma
- Cobertura
- SonarCloud (che usa Jacoco)
## Jacoco
**Jacoco** è una libreria per la misura di code-coverage in progetti Java. È la tecnologia di riferimento per l'analisi di adeguatezza in ambienti basati sulle JVM. Può essere utilizzato sia in modalità stand-alone, sia in combinazione con ambienti automatici di build (Maven o Ant).
[Reference](https://www.jacoco.org/jacoco/trunk/doc/index.html). [Guida Maven](https://www.jacoco.org/jacoco/trunk/doc/maven.html).

Le misure che mette a disposizione Jacoco sono 

Per l'integrazione con Maven ci sono due esempi fatti da SonarCloud:
- [Esempio di base](https://github.com/SonarSource/sonar-scanning-examples/tree/master/sonar-scanner-maven/maven-basic)
- [Esempio per progetti multi-modulo](https://github.com/SonarSource/sonar-scanning-examples/tree/master/sonar-scanner-maven/maven-multimodule) (da utilizzare per i progetti, essendo tutti multi-modulo)
	- in questo caso, è necessario creare un nuovo modulo che raccolga le analisi dagli altri moduli (vedere nello specifico il modulo `tests` nel tutorial)
	- tenere conto anche delle modifiche nei vari `pom.xml` e i file di configurazione per il framework di CI, come `travis.yaml`

Nel progetto, una volta definiti, generati e misurati i casi di test con Jacoco, occorre migliorare (almeno) BDC e CC.
**Nota**: occorre fare attenzione a definire i casi di test in base all'implementazione, dato che il codice potrebbe essere sbagliato.