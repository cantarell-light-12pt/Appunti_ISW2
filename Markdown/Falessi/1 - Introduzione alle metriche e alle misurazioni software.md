# Introduzione
## L'importanza della misurazione
Se un qualcosa non si può migliorare, allora non si può migliorare. Di conseguenza, un qualcosa non misurabile non è neanche controllabile.
## Definizioni
Si consideri un uomo la cui altezza è 1,81 metri. In questo caso, l'altezza rappresenta l'**attributo** che si desidera misurare. La **misurazione** è il processo che porta ad ottenere una **misura** (1,81) dell'attributo in termini della **metrica** scelta (il metro). Se si fosse scelto il centimetro come metrica, allora la misura sarebbe stata 181.

Formalmente:
- una **misura** è una rappresentazione numerica di una caratteristica o attributo di un prodotto o un processo software.
- una **metrica** è una scala o sistema di misurazione usata per quantificare una misura, ossia definisce la regola per l'assegnazione dei numeri alle misure
	- esempi: linee di codice (LOC), cyclomatic complexity (CC), defect density (DD)
	- è estremamente raro che una metrica rappresenti a pieno un concetto, pertanto, nella maggior parte dei casi, è necessario utilizzare più metriche per prendere una decisione.
- la **misurazione** è il processo di assegnazione dei numeri alle misure usando una metrica
	- può essere effettuata su una singola istanza o più istanze
### Esempio
Si consideri l'asserzione "Progetto di ISPW = 5000 LOC".
- la misura è 5000
- la metrica è LOC
- la misurazione è il processo di conteggio delle LOC.
## Scopo delle misurazioni
Una misurazione può essere motivata da diversi fattori:
- **valutazione**: è possibile valutare la qualità e l'efficienza di un processo di sviluppo software e di attività di manutenzione del codice
- **miglioramento**: è possibile identificare aree per effettuare miglioramenti e implementare cambiamenti che possono portare a migliori qualità, produttività ed efficienza del software 
- **pianificazione**: si possono stimare le risorse necessarie a un progetto di sviluppo di software, quali tempo, effort e costo. Possono anche essere usate per pianificare e schedulare task e milestone del progetto
- **controllo**: si può monitorare e controllare il progresso di un progetto si sviluppo software, assicurandosi che sia in linea nei limiti del budget
- **comunicazione**: si possono comunicare lo stato e la qualità del progetto di sviluppo software agli stakeholder, quali project manager, clienti e altri membri del team di sviluppo.
## Attributi di qualità delle metriche
Una buona metrica deve essere:
- **rilevante**: la metrica deve misurare un attributo importante del prodotto o processo software
- **obiettiva**: la metrica deve essere basata su dati verificabili e riproducibili (entro un certo errore), non soggetti a opinioni o stime
- **affidabile**: la metrica deve produrre risultati consistenti sotto diverse condizioni e da valutatori diversi
- **consistente**: la metrica deve essere consistente con gli obiettivi e il contesto del progetto software
- **facile da utilizzare**: la metrica deve essere facile da capire, calcolare e interpretare e non richiedere un effort o risorse eccessivi
### Esempi di metriche
- **dimensione del software**: LOC, function points (FP), object points (OP)
- **qualità del software**: defect density (DD), fault frequency (FF), affidabilità
- **processo software**: produttività, efficienza, tempo di ciclo, tempo di esecuzione
- **complessità del software**: cyclometric complexity (CC), information flow metrics (IFM), Halstead's software science metrics
- **effort software**: costi, effort, schedulazione
# Tipi di scale nelle misurazioni e l'analisi software
La misurazione e l'analisi del software include utilizzare metriche quantitative per valutare e migliorare la qualità, la produttività e l'efficienza del software. Al fine di effettuare misurazioni sensate, occorre usare delle scale appropriate per riflettere la natura dei dati. In particolare, una **scala** è un attributo che caratterizza una metrica.

Di solito, la metrica si mappa 1:1 con la scala.

Esistono diversi tipi di scale:
- **nominale**
- **ordinale**
- **intervallo**
- **ratio**
## Scale nominali
Una **scala nominale** è il tipo più semplice delle scale, in cui i valori sono assegnati a categorie o etichette senza alcun ordine o magnitudine. Le scale nominali rappresentano dati qualitativi, come i difetti software per tipo, come di UI, logica, prestazioni e sicurezza. Altri esempi sono i colori e i nomi.
## Scale ordinali
Una **scala ordinale** è una scale in cui i valori sono assegnati a categorie o etichette in ordine, ma la magnitudine delle differenze tra i valori non è significante. Le scali ordinali rappresentano i dati che possono essere ordinati, ma non misurati, come i difetti software in base alla priorità (critica, alta, media e bassa).
## Intervalli
Un **intervallo** è un tipo di scala in cui i valori sono assegnati a categorie o etichette in ordine, e la magnitudine della differenza tra i valori è significativa. Tuttavia, non esiste un vero "punto zero" sulla scala. Le scale a intervallo sono usate per rappresentare dati che possono essere misurati e confrontati, come l'effort nello sviluppo software in ore o giorni.
## Ratio
La **ratio** è il tipo più sofisticato di scala, in cui i valori sono assegnati a categorie o etichette in ordine, la magnitudine della differenza tra i valori è significativa ed esiste un vero "zero point" sulla scala. Le scale ratio sono usate per rappresentare i dati che possono essere misurati e confrontati, come linee di codice, difetti per kLOC, o test coverage.
## Statistiche per le scale
Le di una scala una scala sono incluse anche nelle scale più complesse.

Per capire il tipo si scala, si può ragionare sul tipo di operazioni che si possono effettuare su di esse.
![[Pasted image 20250305121737.png]]
In generale, una scala più complessa consente di effettuare operazioni più complesse, quindi è più precisa.
### Esempi
- capitoli di un libro -> ordinale
- colori -> nominale, ma dipende dal contesto (se si tratta dei colori delle cinture delle arti marziali, la scala è nominale)
- gate dell'aeroporto -> mista tra nominale (lettere) e ordinale (numeri)
- targa delle macchine -> intervallo (salvo eccezioni)
- numero di tratta dell'autobus -> come utente, nominale, ma per chi assegna i numeri è ordinale
- voti universitari -> intervallo
- voti alle elementari -> ordinali
- profitto -> ratio
## Conclusione
Non c'è una scelta migliore nella scelta della scala: nelle misurazioni e nell'analisi software, essa dipende dalla natura dei dati misurati e l'uso che si intende fare dei risultati.

Capendo le differenze tra i tipi di scala, ci si può assicurare che le misurazioni siano appropriate, accurate e significative, portando a processi decisionali migliori e migliore qualità del software.