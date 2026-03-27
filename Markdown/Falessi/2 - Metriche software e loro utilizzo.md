# ISO/IEC 25010:2011: System and software quality models
## Standard ISO
**ISO/IEC 25010:2011** è uno standard internazionale che fornisce un framework per valutare la qualità dei prodotti software. Lo standard è stato sviluppato dalla ISO (International Organization for Standardization) e dalla IEC (International Electrotechnical Commission).

Lo standard è progettato per essere usato da sviluppatori software, professionisti della quality assurance e altre agenti inclusi nel processo di sviluppo software.
## Caratteristiche di qualità
- **funzionalità**: grado in cui il software incontra le necessità dei propri utenti.
- **affidabilità**: l'abilità del software di eseguire le funzionalità intese  senza errori o failure (di solito misurata con il Mean Time to Failure, MTT)
- **usabilità**: facilità di utilizzo del software da parte degli utenti che vi interagiscono (misurata ad esempio con il numero di click necessari per eseguire una funzionalità)
- **efficienza**: 
- **manutenibilità**: la facilità con cui il software può essere modificato, corretto o dattato ai requisiti in cambiamento
- **portabilità**: 
- **compatibilità**: 
- **sicurezza**: abilità del software di proteggere sé stesso e i suoi dati da accessi non autorizzati o attacchi.
## Benefici 
Lo standard ISO/IEC 25010:2011 fornisce diversi benefici alle organizzazioni e agli sviluppatori, tra cui:
- un approccio standardizzato per valutare la qualità del software consente obiettivi di confronto tra prodotti software
- un insieme comprensivo di metriche che possono essere usate per valutare i prodotti software tra più caratteristiche e sotto-caratteristiche di qualità
- qualità del software migliorata e rischio ridotto di difetti o vulnerabilità di sicurezza
- migliore comunicazione tra gli stakeholder coinvolti nel processo di sviluppo software
- soddisfazione e confidenza del cliente nel prodotto software aumentate
# Metrice OO
Le **metriche OO** sono metriche usate per misurare la qualità dei sistemi costruiti con software OO. Aiutano gli sviluppatori a identificare falle nella progettazione, problemi di prestazioni e problemi di manutenibilità ancora prima nel ciclo di sviluppo software.

Ci sono diverse metriche OO, come le metriche CK, le quali possono essere usate per valutare la qualità di un sistema software object-oriented.
## Metriche CK
Le **metriche CK**, anche dette metriche di Chidamber e Kemerer, sono state sviluppate da Shyam R. Chidamber e Chris F. Kemerer nel 1994.

Le metriche CK consistono di 6 diverse metriche che possono essere usate per misurare vari aspetti della progettazione software object-oriented, quali ereditarietà, coupling e complessità. 
### Elenco delle metriche
- **Weighted Methods per Class** (**WMC**) -> misura la complessità di una classe contando i metodi che contiene
- **Depth of Inheritance** Tree (**DIT**) -> misura la profondità della gerarchia di ereditarietà di una classe
- **Number of Children** (**NOC**) -> numero di sottoclassi dirette di una classe
- **Coupling Between Object classes** (**CBO**) -> numero delle classi con cui la cui la classe in questione è coupled (**Nota**: una classe usa altre classe o è usata, quindi in base a questo i cambiamenti possono essere più o meno impattanti)
- **Response For a Class** (**RFC**) -> misura il numero di metodi che possono essere invocati per rispondere a messaggi in un oggetto di una classe
- **Lack of Cohesion in Methods** (**LCOM**) -> misura la mancanza di coesione tra i metodi di una classe
### Altre metriche OO
In aggiunta alle metriche CK, molte altre metriche object-oriented possono essere usate per misurare diversi aspetti della progettazione software. Alcune si queste metriche aggiuntive sono:
- **Lines of Code** (**LOC**)
- **Cyclomatic Complexity** (**CC**)
- **Fan-in** and e **Fan-out**
- **Class Responsibility Collaboration** (**CRC**)
- **Efferent Coupling** (**EC**)
- **Afferent Couplin**g (**AC**)
#### Cyclomatic Complexity (CC)
La **cyclomatic complexity** misura la complessità di in programma in termini del numero di cammini indipendenti attraverso il codice. I programmi con un alto CC sono più difficili da capire e manutenere. La CC può anche essere calcolata utilizzando altri strumenti, come quelli di analisi statica del codice.
#### Fan-in e Fan-out
Il **fan-in** misura il numero di altre classi che dipendono da una data classe. Il **fan-out**, d'altra parte, misura il numero di classi da cui una certa classe dipende. Queste metriche possono essere usate per identificare classi che sono strettamente accoppiate ad altre classi e necessitano un refactoring.
# Function point
I **function point** sono un insieme di tecniche di misurazione della dimensione del software che quantificano le funzionalità fornite da un'applicazione software. Essi sono indipendenti dal linguaggio di programmazione, dalla tecnologia e dai dettagli implementativi. Logicamente, misurano quante funzioni ha un'applicazione software, in particolare assegnando dei punti a ogni tipo di funzionalità. I FP sono basati sulla prospettiva dell'utente e dal valore di business dell'applicazione software.

Esiste anche un concetto di prossimità temporale che impatta l'affidabilità di una stima. In questo caso, si fa un compromesso tra accuratezza e tempo.

I FP tengono conto solo dei requisiti funzionali dell'applicazione e non di quelli non funzionali. **Problema**: questi ultimi impattano non poco su costi e tempo di sviluppo
## Componenti dei FP
- **Internal Logical Files** (**ILFs**) -> dati mantenuti e usati dall'applicazione
- **External Interface Files** (**EIFs**) -> dati passati tra l'applicazione software e altri sistemi esterni
- **External Inputs** (**EIs**) -> input utente che avviano qualche processamento all'interno dell'applicazione software
- **External Outputs** (**EOs**) -> output utente risultanti da certo processamento all'interno dell'applicazione 
- **External Inquiries** (**EQs**) -> indagini utente che risultano nella fornitura di informazioni da parte dell'applicazione 
### Calcolo de function point
Nei function point, ad ogni componente è assegnato un peso (basso, medio, alto) in base alla sua complessità e impatto sull'applicazione software. Le componenti pesate sono poi moltiplicate per un fattore di conversione per ottenere il numero totale di function point.

Il fattore di conversione varia a seconda della tecnologia di sviluppo software, della natura dell'applicazione software e il livello di dettaglio nei requisiti funzionali.

Pesi delle complessità dei function point:

| Parametro di misurazione     | Semplice | Medio | Alto |
| :--------------------------- | :------: | :---: | :--: |
| Numero di input utente       |    3     |   4   |  5   |
| Numero di output utente      |    4     |   5   |  7   |
| Numero di indagini utente    |    3     |   4   |  6   |
| Numero di file               |    7     |  10   |  15  |
| Numero di interfacce esterne |    5     |   7   |  10  |

| Parametro di misurazione     | Conteggio | Fattore di peso (medio) | Totale |
| :--------------------------- | :-------: | ----------------------: | -----: |
| Numero di input utente       |    50     |                       4 |    200 |
| Numero di output utente      |    40     |                       5 |    200 |
| Numero di indagini utente    |    35     |                       4 |    140 |
| Numero di file               |     6     |                      10 |     80 |
| Numero di interfacce esterne |     4     |                       7 |     28 |

Ogni tipo di FP viene moltiplicato per un peso, dovuto alla complessità (simple, average, complex). Questo tipo di conto comporta dei problemi:
- sono stati assegnati dei valori a delle parole
- due simple valgono un complex
- valori fuori dal range devono essere comunque adeguati a questi tre valori.
### Vantaggi
I FP forniscono uno standard e una misura consistente della dimensione software e della complessità. Sono basati sulla prospettiva dell'utente e il valore di business dell'applicazione software, non sulla tecnologia o sui dettagli implementativi. Possono essere usati per stimare gli sforzi di sviluppo software, il costo, la schedulazione, così come confrontare le applicazioni software tra diversi domini e piattaforme.
### Limitazioni
I FP non catturano tutti i tipi di funzionalità software e attributi di qualità, come usabilità, manutenibilità e affidabilità. Infatti, sistemi funzionalmente simili ma orientati a un pubblico diverso, posso prevedere picchi di carico diversi e quindi requisiti non funzionali diversi.

Inoltre, possono essere soggetti a interpretazioni e giudizi soggettivi, dato che i pesi e i fattori di conversione possono variare tra i vari stakeholder e contesti.

Possono anche essere non adatti a tutti i tipi di applicazioni software, come a quelli con grande enfasi sull'interfaccia utente o processamento real-time.
# Strategie GQM-S
È un approccio dell'ingegneria del software che mette insieme due metodi:
- **Goal-Question-Metric** (**GQM**), per definire gli obiettivi, le domande e le metriche 
- **Strategies**, per identificare e selezionare le strategie più appropriate per raggiungere i dati obiettivi

Si inizia con i **business goal**, ossia obiettivi business. Si inizia da questi per, ad esempio, migliorare la soddisfazione del cliente, ridurre i costi e i tempi di sviluppo. A volte occorre fare feature implementation piuttosto che testing per uscire prima sul mercato e accaparrarsi l'utenza, aggiustando poi in seguito l'applicazione.
Le **strategie** definiscono i compromessi per identificare gli specifici obiettivi software che contribuiscono a raggiungere gli obiettivi di business.

I **software goal** puntano invece a migliorare l'efficacia dei test sul sistema. Questi sono raggiunti usando degli scenari per definire degli obiettivi di misurazione.

Infine, gli obiettivi di misurazione consentono di derivare delle misure per interpretare obiettivi di livello più alto.
## Processo
Il processo è estremamente iterativo ed è diviso in fasi:
1. **Initialize**: si definiscono l'impegno, il personale e le responsabilità
2. **Characterize**: si definisce lo scopo dell'applicazione e si caratterizza l'ambiente e il contesto
3. **Set goals**: si determinano le strutture organizzative, si esegue un'analisi dello status quo, si pone una priorità sugli obiettivi e si costruisce una griglia di obiettivi/strategie
4. **Choose process**: si pianifica l'implementazione di strategie , si organizza la raccolta e l'analisi dei dati e si definiscono i meccanismi di feedback. Inoltre, il processo target può essere modificato in base alle necessità, se necessario, e si attua il piano di misurazione sul processo target
5. **Execution model**: 
	1. si eseguono le strategie
	2. si raccolgono e si analizzano i dati
	3. si fornisce un feedback
6. **Analyze results**: si analizzano i dati e si rivedono le strategie, si rivedono e si comunicano i risultati e si analizzano i costi/benefici
7. **Package and improve**: si adattano e si migliorano gli obiettivi e le strategie, si rivedono i contesti e le assunzioni e si registrano le lezioni apprese.
## Utilizzi
- miglioramento del processo di sviluppo software: un team di sviluppo software può usare GQM+S per identificare le area di miglioramento nel loro processo di sviluppo software. Possono impostare obiettivi e strategie per misurare e migliorare la qualità, l'efficienza e l'efficacia del loro processo di sviluppo software.
- valutazione della qualità del software: un'organizzazione di sviluppo software può usare GQM+S per verificare la qualità del loro prodotto software. Possono impostate obiettivi e strategie per misurare le caratteristiche di qualità dei loro prodotti software, quali affidabilità, usabilità e manutenibilità.
- gestione del software: un project manager può usare GQM+S per monitorare il progresso di un progetto di sviluppo software. Possono impostare gli obiettivi e le strategie per misurare il progresso del progetto, quali il tasso di completamento di milestone, la produttività dei membri del team e la soddisfazione degli stakeholder
- decision making: un'organizzazione di sviluppo software può usare GQM+S per supportare il decision making. Possono impostare gli obiettivi e le strategie per raccogliere e analizzare i dati per supportare il decision making, come decidere sull'adozione di una nuova tecnologia o selezionare una metodologia di sviluppo software.
- software testing: un team di software testing può usare GQM+S per migliorare il loro processo di testing. Possono impostare obiettivi e strategie per misurare l'efficacia del loro processo di testing, come la coverage dei test case, l'identificazione di difetti e il tempo richiesto per risolvere un difetto.
- manutenzione del software: un team di manutenzione del software può usare GQM+S per migliorare il processo di manutenibilità. Possono impostare gli obiettivi per misurare l'effort della manutenzione, come il tempo richiesto per risolvere un difetto, la frequenza dei cambiamenti e la soddisfazione degli utenti con gli aggiornamenti software.
## Esempio 1
1. 
2. Value -> tempo di esecuzione dei test ridotto da 10s a 8s
3. due punti
	- strategia -> aumentare la valutazione di 0,5 stelle
	- value -> rating passato da 4 a 4,5 stelle
La strategia non definisce esattamente "come" raggiungere l'obiettivo, ma parzialmente, aiutando poi a definirlo.
## Esempio 2
1. 3
	- metrica -> tempo medio necessario per chiudere un ticket 
	- strategia -> ridurre il tempo medio del 10%
	- valore -> riduzione del tempo da 10 giorni a 9 giorni
2. 3
	- metrica -> numero di ticket risolti per iterazione
	- strategia -> aumentare il numero di ticket del 10%
	- valore -> numero di ticket aumentato da 10 a 11
In generale, lo scopo non è fare un sistema in cui si può barare. L'obiettivo è creare un sistema abbastanza accurato, ma al contempo facile da utilizzare.