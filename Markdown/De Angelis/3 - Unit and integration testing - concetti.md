# Aspetti del software testing
Quando si parla di ST, si possono considerare tre aspetti fondamentali:
- **livello** -> contesto degli obiettivi
	- unità: c'è una stretta vicinanza tra il programmatore/progettista dell'unità e il programmatore/progettista dei test, persone che spesso coincidono; si prende una porzione atomica del system under test (metodo, package, classe) e chi ne ha scritto il codice scrive dei test per verificare che tali funzionalità rispecchino le specifiche
	- integrazione: dopo i test di unità, si prendono più moduli del sistema per verificare che funzionino in cooperazione tra loro
	- sistema: si fa integrazione a livello di sistema, ossia test di integrazione con tutti i moduli del sistema
	- accettazione: è estremamente vicino all'idea di validazione; si valida il sistema rispetto ai requisiti utente, ergo si valuta se i committenti sono soddisfatti o no
- **metodo**
	- black-box (non si conosce l'implementazione del codice che si testa, ma solo specifiche in linguaggio naturale o modelli come SD/AD; si verifica che la scatola nera rispetta questi aspetti)
		- svantaggi: se gli errori non sono tracciabili da aspetti funzionali, ma solo dall'implementazione, trovare input per cui i test falliscono può risultare più costoso; è un metodo più difficile, poiché si basa sul *cosa* testare
		- vantaggi: questo metodo si basa sulle specifiche, quindi sulle funzionalità, astraendo le funzionalità
	- white-box (si conosce l'implementazione del codice che si vuole testare; si possono ignorare anche le specifiche, dato che questo metodo è guidato dalla specifica dell'implementazione)
		- svantaggi: il testing è guidato dall'implementazione, che potrebbe anche essere completamente sconnessa dai requisiti
		- vantaggi: è più semplice, poiché si basa sul *come* testare (dipende dal linguaggio); consente automazione più efficiente
	- non funzionale: si vede come viene svolta un'attività funzionale, come tempo di esecuzione, sicurezza, manutenibilità.
- **tipo**
	- manuale
	- automatico (automazione, generazione dei casi di test, prioritizzazione)
## Testing e V&V
![[Pasted image 20250401121039.png]]
Nei test di unità, integrazione e sistema si fa verifica, mentre i test di accettazione comportano validazione.

Notare la differenza delle aree della piramide: in un sistema software si tendono ad avere molti test di unità, dato che i programmatori che sviluppano i test scrivono molti casi di test; man mano che si sale di livello, è sempre più difficile trovare casi di test sensati. Inoltre, gli strumenti di supporto riflettono - più o meno - questa piramide: ci sono molti strumenti di supporto per i test di unità, meno per quelli di accettazione.
## V-model
![[Pasted image 20250401121413.png]]

Un'altra vista che si può avere dei test è il **v-model**, che schematizza il rapporto tra le attività di sviluppo e quelle di V&V. Le fasi di sviluppo sono semplificate in una cascata, ma vanno viste come uno spin di uno sviluppo iterativo.

Il fulcro di questo schema è che ad ogni fase dello sviluppo dovrebbe corrispondere un'attività parallela specifica di V&V. Ad esempio, quando si progetta un sottosistema, si dovrebbero pensare dei relativi test di integrazione.

Nel dettagli:
1. il designer specifica e implementa delle funzionalità
2. il designer rilascia le specifiche al tester
3. il tester specifica e implementa i test
4. il test esegue i casi di test

L'esecuzione automatica dei casi di test può avvenire:
- ad ogni proposta di modifica pervenuta (come un commit o una pull-request)
- al rilascio di una nuova funzionalità
- al rilascio di una nuova versione
# Test di unità
Gli obiettivi dei testi di unità sono:
- rilevare malfunzionamenti su un singolo modulo o unità funzionale (metodo, classe, package, ma non un modulo Maven). La granularità del modulo dipende dal contesto.
- acquisire confidenza che tale modulo del SUT segua il comportamento atteso se eseguito in isolamento

Solitamente i test di unità sono implementati dagli stessi sviluppatori che hanno implementato il modulo in questione, anche prima della scrittura del codice (Test Driven Development). **Nota**: il TDD può includere anche l'aggiunta di casi di test a seguito della scrittura del codice; in breve, è un processo iterativo.

Si possono stabilire l'adeguatezza e la bontà dei test di unità in funzione:
- del numero di funzionalità controllate, del numero dei requisiti controllati, del numero di aspetti rilevabili dalle specifiche -> si stabilisce se i casi di test sono buoni rispetto alle funzionalità rispetto al *cosa* (specifiche, requisiti)
- dalle metriche di copertura del codice sorgente a disposizione-> si stabilisce se i casi di test sono buoni rispetto alle funzionalità rispetto al *come* (implementazione)
## JUnit
**JUnit** è un framework nato originariamente per sviluppare l'implementazione di test di unità in ambienti Java.

Requisiti principali:
- supportare la definizione di casi di test utili
- supportare la definizione di casi di test ripetibili nel tempo
- ridurre i costi di scrittura dei casi di test tramite pratiche orientate al riuso del codice
### Esempio
Uno o più casi di test corrispondono a una classe, che definisce il "runner". I metodi di test (annotati con `@Test`) corrispondono all'implementazione di una sequenza di passi che specificano il test. I test possono essere parametrici, ossia indipendenti dai valori specifici utilizzati. I parametri possono essere passati (in JUnit 4) tramite un metodo pubblico e statico che restituisce una collezione di oggetti. Questo metodo può avere qualsiasi nome e serve a codificare i parametri su cui effettuare i test, che vengono scelti dal progettista dei casi di test. Il metodo per l'ottenimento dei parametri viene chiamato prima dell'istanziazione della classe, che viene istanziata una volta per ogni elemento nella collezione tramite il costruttore (non di default).
(spiegare slide 10)

Dentro JUnit, tutto questo lavoro è eseguito dal runner, in questo caso la classe `Parameterized` di JUnit, che esegue questi passi nella sequenza indicata.
### Pre-testing e post-testing
Esistono annotazione per eseguire operazioni una sola volta prima o dopo l'esecuzione di tutti i casi di test di una classe, quali `@BerforeClass` e `@AfterClass`. Non è specificato un ordine di esecuzione tra più metodi annotati con la stessa annotazione.
### Esempio
(riportare slide 12)

È responsabilità del designer dei casi di test l'identificazione dettagliata dei valori attesi da un test, attraversi valori puntuali o la consultazione di un oracolo (funzione di predizione o sistema esterno).

È responsabilità del programmatore dei casi di test implementare le scelte progettuali attraverso un insieme di istruzioni di verifica e controllo (come `Assert`).

Quando si progettano i casi di test, occorre definire il valore atteso dei test.
# Test di integrazione
I test di integrazione puntano a scovare problemi dovuto all'interazione tra diversi moduli del sistema software. In particolare, si vuole acquisire confidenza che:
- ci siano le dovute interconnessioni tra moduli/componenti del SUT
- le interazioni tra i moduli del SUT rispettano i "contratti funzionali" stabiliti da protocolli/specifiche di interazione, ossia nella maniera progettata

Ad esempio, se una classe A aggrega le classi B e C, il test di integrazione verifica sia che un'istanza di A sia collegata ad oggetti di classe B e C, sia che sia connessa agli oggetti corretti di B e C (ad esempio, si può verificare che lo studente sia connesso al proprio corso di laurea e non uno qualsiasi).

I test di integrazione assumono che ogni modulo funzioni correttamente in isolamento, ossia ha passato tutti i test di unità.

Chi esegue i test di integrazione dipende dal caso specifico: possono essere sviluppatori, integratori, tester.

I test di integrazione si possono eseguire secondi diversi approcci:
- white-box: si redaggono i test avendo a disposizione il codice dei moduli da testare
- black-box: si  conosce solo la specifica delle interfacce esposte dai moduli
Nella pratica, si tiene conto di entrambi gli approcci contemporaneamente.

Nei test di integrazione, spesso si ha la necessità di utilizzare degli **stub** (o **mock**), elementi software definiti in fase di test (come una classe, un modulo, un'unità ecc.) che non è soggetto a test di per sé - pur facendo parte dell'ambiente di test - e mima il comportamento dell'unità mancante, spesso attraverso comportamenti banali o costanti. In questo modo, il test viene eseguito in modo incrementale, considerando di volta in volta parti aggiuntive del SUT, simulando quelle al momento non disponibili.

I test di integrazione hanno anche bisogno di **test driver** avanzati per:
- configurare l'ambiente di test
- coordinare le risorse
- eseguire il clean-up dopo l'esecuzione

## Strategie per i test di integrazione
In generale, trovare la soluzione ottima è un problema difficile (NP-completo).

I test di integrazione si possono svolgere seguendo diverse strategie:
1. big bang -> si integrano tutte le componenti insieme e ne si testa il funzionamento
	- svantaggi: è difficile e non consente di individuare tutti i problemi
2. top-down -> si esegue l'interazione delle interfacce dei moduli
	- i test che si definiscono sono volti a verificare che le connessioni tra le componenti siano corrette e che lo siano anche i dati passati
	- è utile usare questo approccio quando si ha una chiara definizione delle interfacce dei moduli
	- spesso gli scenari sono definiti a partire dagli scenari d'uso del SUT
	- si utilizzano gli stub per simulare il comportamento o le funzionalità mancanti
	- esempio:
		1. si testa C1, usando degli stub per C2 e C3
		2. si testano C1 e C2, usando uno stub per C3
		3. si testano C1, C2 e C3, usando uno stub per C4
		4. si testano C1, C2, C3 e C4, usando uno stub per C5
		5. si testano tutte le classi
3. bottom-up -> si parte dallo strato profondo del sistema, fino ad arrivare ai bordi (boundary)
	- si usano i test driver per simulare il comportamento dei moduli o componenti più esterni del sistema
	- anche qui le funzionalità mancanti sono simulate tramite stub
	- si raffina considerando interfacce più generali o focalizzandosi sulle parti più periferiche del sistema
	- esempio:
		1. si testa C5
		2. si testano C5 e C4 (che "stimola" C5 come test driver)
		3. si testano C5, C4 e C2
		4. si testano C5, C4, C3 e C2
		5. si testano tutte le classi