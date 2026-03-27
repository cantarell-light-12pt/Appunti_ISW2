# Q&A
**Domanda**: come trattare i metodi di interfacce che, non avendo un'implementazione di per sé, avranno molti valori di colonne pari a zero?
**Risposta**: non c'è un vero problema, dato che gli zeri rappresenteranno i valori delle metriche del modello, seppur il modello non sia in grado di comprendere effettivamente il significato che un numero rappresenta.
# L'importanza della qualità del software
I bug sono importanti perché hanno rilevanza, sia a livello economico ([ref](https://www.it-cisq.org/wp-content/uploads/sites/6/2023/09/The-Cost-of-Poor-Quality-Software-in-the-US-2018-Report.pdf)), sia a livello di safety.

Il software si evolve a grandissima velocità, con range da 6 mesi a ogni giorno.

Il desiderio di avere un sistema di grande qualità deriva dal fatto che questo avrà meno possibilità di portare alcuni bug.

I bug possono essere prevenuti in diverse maniere:
- usando strumenti di analisi statica per verificare la qualità del codice
- monitorando il debito tecnico, ad esempio impostando dei quality gate (insieme di regole che devono essere rispettate dal codice affinché il commit sia accettato) con SonarQube/Github Actions.

La manutenibilità è misurata in termini di effort o bug. Tuttavia, il problema dell'effort è che è poco correlabile al concetto di smell (poiché non tiene conto del numero di modifiche che devono essere effettuate), quando invece lo è il bug.

I bug possono essere trovati con:
- unit testing
- CI/CD
- code review
# Software analytics
La **software analytics** è una disciplina che combina l'analisi dei dati al software engineering: si occupa di analizzare i dati per migliorare il software. I dati vengono estratti dagli strumenti 
## ML per i difetti software
Si vuole utilizzare il ML per predire e spiegare i difetti futuri, creando al contempo delle teorie empiriche.

1. Si prendono dati grezzi da Git/Jira
2. Si puliscono i dati
3. SI fa un analisi dei dati
4. Si crea il modello
5. Si può usare il modello per trovare i bug, ma anche utilizzare per spiegare il motivo dei bug (ad esempio quali valori di feature evitare)

Le teorie hanno bisogno di un'osservazione, un'intuizione e una validazione.

Alcune teorie dell'ingegneria del software:
- impatto dei code smell sull'utente
- non avere codice duplicato
- in generale, ogni regola all'interno di SonarCloud
Le teoria vanno validate o dal punto di vista formale (informatica teorica) o dal punto di vista empirico: prima di facevano degli esperimenti (si facevano sviluppare delle feature su un codice con o senza smell), mentre ora si possono utilizzare le misure storiche.
## Misure
Dai Jira e Git si possono estrarre dati (oggetto della prossima lezione). Jira tiene traccia dei ticket, ossia insieme di task assegnati a persone.

Una volta preso il commit di git, si ha anche il riferimento all'id del ticket su Jira.

**Linkage**: proporzione dei commit che hanno un ticket (non tutti i commit hanno un ticket). Maggiori commit sono senza ticket, maggiori sono le informazioni perse. Di base, si vorrebbe che tutti i commit abbiano un ticket. Ad esempio, se si vuole capire se la maggior parte di commit sia bug fixing o feature implementation, ma molti sono senza ticket, non si può raggiungere una conclusione.

Una volta raccolte le metriche da Git e Jira, si possono identificare i difetti e crearne un database. SI vorrà creare una tabella con uno storico degli injection dei difetti (bug) all'interno del codice dei singoli metodi.
## Modello
Si hanno già dei dati di training (da Jira) che vengono dai in pasto ai modelli perché, una volta date delle classi di test, verificano se queste classi sono buggy o meno.

Di base, il modello non classifica in base alla classe, ma fornisce una probabilità che un'istanza appartenga a una classe piuttosto che a un'altra.
## Explainability
Dato un modello black box, a seguito dell'addestramento si può tirare fuori una spiegazione. Uno degli approcci è ad esempio tirare fuori un albero decisionale (modello explainable) da una rete neurale (modello non explainable)
# CMMI
CMMI: Capability Maturement Model Integration. Misura la maturità  di un processo.

I difetti che si vogliono evitare sono quelli in produzione. Si vuole migliorare il profitto per dipendente e il numero di requisiti per release.
Nel grafico, i dati sono stati normalizzati in una scala da 0 a 100: è importante sottolineare il trend a seguito anche delle certificazioni assunte dall'azienda. Ad esempio, dopo ML3 è stato suggerito di usare Jira, che ha permesso di contare i bug in produzione.

Una tecnica utilizzata per il controllo del processo è il processo control graph: serve a garantire la stabilità di un processo. C'è una metrica (ad esempio il numero di difetti, o lo stesso per numero di righe) che non deve variare eccessivamente nel tempo (in questo caso in release), ossia deve rimanere all'interno di un range (upper and lower limit) che vengono calcolati. Quando un dato futuro (ma anche passato) non rientra questi limiti, occorre - come best practice - convocare una riunione per analizzare cosa sia successo.

Release planning: consente di decidere quali release hanno quali ticket. 

Cliccando sul grafico (punto verde) dice che al 83% si è confidenti che il numero di bug minori in produzione sarà minore di 5.04.

Github è collegabile a Jira tramite il numero del ticket nel commento del commit.