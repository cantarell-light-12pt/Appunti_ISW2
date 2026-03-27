L'esecuzione di casi di test non può confermare l'assenza di errori da un sistema. La generazione di casi di test consiste nel pianificare degli esperimenti per interagire con il sistema creato. Chiaramente, non è possibile interagire con il sistema con tutte le possibili configurazioni (principalmente per motivi di budget), quindi deve cercare di raggiungere un certo grado di confidenza con il sistema, selezionando appositamente le configurazioni necessarie e/o possibili. Ci si pongono in particolare tre domande:
- quali e quanti input selezionare?
- quando smettere di testare?
- come si può sapere se il risultato di un'operazione è corretto?
# Domande
## Quali e quanti input selezionare?
L'esplorazione esaustiva del dominio di input è spessi impraticabile (ad esempio la funzione fattoriale). Esistono diversi approcci per la selezione degli input:
- uso di dati da interazioni passate: si utilizzano gli stessi dati utilizzati in interazioni passate
	- vantaggi:
		- i dati passati possono ripresentarsi in futuro
		- si testato modi di utilizzo effettivamente sperimentati nel sistema
		- ottimo metodo per focalizzarsi sui casi più sollecitati
		- utile nelle attività che guardano alla validazione del sistema
		- può dare indicazioni sulla reliability effettiva del sistema in operazione
	- svantaggi:
		- se il progetto o la funzionalità sono nuovi, non esistono storici di dati
		- i dati passati potrebbero non coprire tutti input possibili del sistema, che possono cambiare nel tempo
		- potrebbe esservi un alto numero di casi di test dagli storici
- selezione randomica degli input: si scelgono degli input a caso e si testa il sistema con questi:
	- vantaggi:
		- semplicità nella realizzazione del metodo (benché apparente, dato che il metodo potrebbe essere vincolato a valori specifici)
		- utile nelle fasi iniziali dei test per rilevare malfunzionamenti grossolani
	- svantaggi:
		- la ricerca è cieca
		- gli input trovati potrebbero non essere rappresentativi e sarebbe necessario un alto numero di casi di test per raggiungere un livello di confidenza adeguato
- generazione automatica rispetto a un obiettivo di adequacy: si creano degli strumenti che, in maniera automatica, esplorano il dominio di input e cercano di ottimizzare una funzione obiettivo, in questo caso l'**adequacy** (misura della bontà di un insieme di test)
	- vantaggi:
		- altamente automatizzabile in molti casi
		- facilmente integrabile con approcci di sviluppo e CI
	- svantaggi:
		- nei casi in cui la generazione è guidata dal SUT, possono risultare molti test poco significativi per le effettive condizioni di utilizzo del sistema
		- nei casi in cui la generazione è guidata da modelli del SUT, si hanno strumenti di generazione meno versatili nell'integrazione con i processi di sviluppo
		- complessità legate alla scelta degli iperparametri e alle euristiche di generazione
- partizionamento del dominio in classi di equivalenza rispetto gli input atteso e allo stato del SUT: si possono riconoscere dei cluster di input "simili", in modo da scegliere un rappresentante per cluster:
	- vantaggi:
		- indipendente dall'implementazione
		- nella fase di progettazione dei test, di cerca di identificare aspetti salienti del dominio o modi di utilizzo effettivamente sperimentati nel sistema
	- svantaggi:
		- la qualità dei test è in funzione dei livelli di chiarezza dei requisiti e delle specifiche
		- si riescono a prendere in considerazione solo vincoli o limitazioni esplicitamente espressi
		- familiarità del SUT
		- conoscenza dei contesti operativi e di deployment
- interazione con strumenti di intelligenza artificiale (LLM): si sfruttano forme di AI addestrate, interagendo con loro tramite linguaggio naturale:
	- vantaggi:
		- semplicità (per quanto apparente) di interazione con gli LLM
		- grande potenza di ragionamento in tempi relativamente ristretti
		- onniscenza (anch'essa apparente) degli LLM
	- svantaggi:
		- soluzioni in funzione della formulazione del problema (prompt engineering)
		- eccessiva fiducia negli LLM, dato che le risposte osservate potrebbero essere frutto di bias accidentali
		- ragionamento black-box e probabilistico (gli outcome non possono essere considerati deterministici)
## Quando smettere di testare?
## Come si può sapere se il risultato di un'operazione è corretto?
Una volta scelti gli elementi significativi del dominio di input, è necessario assegnare ad ogni input un output atteso dal sistema. Questo si può fare in diverse modi:
- tramite un oracolo che risponde sempre correttamente
- utilizzando dei valori storici, specifiche, sistemi equivalenti, inferenze su proprietà ecc.

La specifica dei casi di test deve prevedere l'output per ogni interazione con il sistema o elemento testato. Inoltre, è possibile avere dei casi di test che prevedano fallimenti del sistema, ad esempio tramite lancio di un'eccezione o un errore.

**Nota**: la scelta dei valori di input del sistema è solo una parte della specifica del test. Affinché questa sia completa, occorre aggiungere l'output atteso dal sistema.
# Approcci
## Randoop
**Randoop** è un generatore randomico di unit test per programmi Java, che crea automaticamente dei programmi JUnit che implementano dei casi di test.

Può essere usato per la generazione dei test per il progetto.
## Classi di equivalenza
Un dominio di input può essere partizionato in **classi di equivalenza**, in modo tale che, per ogni elemento in una partizione, il SUT deve mostrare lo stesso comportamento, o quantomeno si può assumere che sia così dalla specifiche.

Nella test suite in costruzione $T_g$ deve essere incluso un elemento per ogni partizione. In particolare, la qualità di $T_g$ sarà dovuta a:
- chiarezza dei requisiti e delle specifiche: vincoli o limitazioni esplicitamente espressi
- familiarità del SUT: conoscenza dei contesti operativi e di deployment
- conoscenza dell'implementazione: dettagli su come sono realizzate procedure e controlli

(continua nelle slide successive).