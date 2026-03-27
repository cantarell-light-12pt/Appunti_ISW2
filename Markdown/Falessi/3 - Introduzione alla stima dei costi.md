# Definizioni
Una **stima** è un proxy per una misurazione, ossia viene utilizzata quando la misurazione non è possibile. Una **predizione** ha funzione simile, ma relativa al tempo. In breve, se un concetto è presente si parla di stima, se è futuro di predizione. 
# Domande fondamenti sulla stima
Quando si effettua una stima, occorre porsi diverse domande:
- quanto effort è richiesto per completare l'attività?
- quando tempo (in giorni) è necessario per completare l'attività?
- qual è il costo totale dell'attività?
## Componenti del costo software
Nello sviluppo si deve tener conto dei seguenti costi:
- costi hardware e software
- costi di viaggio e di training
- costo dell'**effort**, fattore dominante nella maggior parte dei progetti
	- salari degli ingegneri coinvolti nel progetto
	- costi sociali e assicurativi

L'ultimo punto deve considerare anche le spese generali:
- costo di edifici, riscaldamento, illuminazione
- costo di networking e comunicazione
- costo di strutture condivise (come librerie, staff dei ristoranti ecc.)
## Costo e prezzo
Le stime vengono effettuate per scoprire il costo dello sviluppatore affinché produca un sistema software. Non vi è una relazione semplice tra il costo di sviluppo e il prezzo richiesto al cliente. Infatti, quest'ultimo è influenzato da fattori economici, politici, aziendali e organizzativi a livello più alto.
## Ruolo del linguaggio di programmazione
![[Pasted image 20250312000655.png]]
## Tecniche di stima
Si possono utilizzare diverse tecniche di stima:
- modellazione algoritmica del costo
- giudizio esperto
- stima per analogia
- legge di Parkinson
- prezzi vincenti
### Modellazione algoritmica del costo
È un approccio **formulare** basato su informazioni storiche del costo e che generalmente si basa sulla dimensione del software (vedi COCOMO).
### Giudizio esperto
Uno o più esperti sia nello sviluppo software, sia nel dominio applicativo, usano la loro esperienza per predire il costo del software. Il processo itera fino al raggiungimento di un consenso.
**Vantaggi**: il metodo di stima è relativamente economico e può essere accurato se gli esperti hanno esperienza diretta su sistemi simili.
**Svantaggi**: è molto inaccurato se non ci sono esperti. Un altro problema è capire chi è esperto o meno relativamente a un dominio applicativo, ossia definire un grado di competenza che pio induca un grado di variabilità sulla stima.
### Stima per analogia
Il costo di un progetto è calcolato confrontando il progetto a un altro simile nello stesso dominio applicativo.
**Vantaggi**: è accurato se sono disponibili dati sul progetto
**Svantaggi**: è impossibile se non è disponibile alcun progetto simile; è richiesto infatti un database dei costi mantenuto sistematicamente.
### Legge di Parkinson
Il progetto costa tanto quanto le risorse sono disponibili. La gestione del tempo è solo psicologica. Ci viene spontaneo ritmare i tempi per portare a termine un progetto in tempi brevi. Lo stesso compito può richiedere un'ora o una settimana, a seconda del tempo che ci concediamo per portarlo a termine.
**Vantaggi**: nessuna spesa eccessiva
**Svantaggi**: il sistema di solito rimane incompiuto.
### Prezzo vincente
Il progetto costo tanto quanto il cliente ha da spendere su di esso.
**Vantaggi**: si ottiene comunque il contratto
**Svantaggi**: la probabilità che il cliente ottenga il sistema desiderato è piccola; infatti, i costi non riflettono accuratamente il lavoro richiesto.
# Introduzione al planning poker
Il **Planning Poker** è una tecnica di stima agile che è diventata molto diffusa negli ultimi anni. 

Nonostante le basi delle tecnica sia stati in giro per molti anni, le rifiniture apportate da Greening lo rendono usabile dai team agile e lo hanno sfruttato a pieno.

Il Planning Poker è estremamente semplice da giocare, ma al contempo abbastanza accurato da usare per il planning agile.
## Regole
1. Ogni partecipante ottiene un mazzo di carte di stima rappresentante una sequenza di numeri. Le sequenze più note richiedono il raddoppiamento di ogni carta (0, 1/2, 1, 2, 4, 8, 16 ecc.) oppure la sequenza di Fibonacci (1, 2, 3, 5, 8, 13 ecc.), ma quest'ultima è di solito più popolare. Il fatto di utilizzare un mazzo incompleto (senza tutti i numeri) serve a dare un concetto di limitatezza del range e di vicinanza tra i valori: l'approccio è sicuramente meno preciso, ma più facile da utilizzare (compromesso).
2. Il moderatore presenta una user story alla volta al team
3. Il Product Owner (o un ruolo equivalente) risponde a ogni domanda che il team può avere riguardo la story
4. Ogni partecipante seleziona *privatamente* una carta, rappresentante la sua stima sulla dimensione della user story. La seleziona privata evita il concetto di *anchoring*, ossia la tendenza ad essere d'accordo con gli altri ed pensare il meno possibile.
5. La size rappresenta un valore che tiene conto di tempo, rischio,  complessità e altri fattori rilevanti
6. Quando tutti sono pronti con una stima, tutte le carte sono presentate simultaneamente
7. Se c'è consenso su un particolare numero, poi la dimensione è registrata e il team si muove alla story successiva
8. Nel (molto probabile) caso in cui la stima differisca, il più alto e il più basso stimatore difendono la loro stima contro il resto del team
9. Il gruppo dibatte brevemente gli argomenti
10. Viene fatto un nuovo round di stima (passi 4-9) (**Nota**: non si presenta anchoring, dato che tutti hanno espresso la propria opinione e si ha modo di valutarle tutte autonomamente)
11. Si continua finché non si raggiunge il consenso e il moderatore registra la stima
12. Si ripete per tutte le story
## Dettagli
CI sono diverse buone ragioni per cui la maggior parte dei trainer e dei coach agile promuovono l'uso del Planning Poker:
- favorisce la **collaborazione** includendo l'intero team
- crea stime basate su **consenso** anziché avere una sola persona che deriva le stime
- espone i **problemi** prima tramite discussione di ogni user story.
## Consigli
- **coloro che possono davvero fare il lavoro sono quelli che votano**: molto spesso i team agile fanno votare tutti, anche se non hanno idea riguardo il lavoro richiesto dalla storia
- **i manager non votano**: i manager sono di solito incentivati a volere che il lavoro richieda meno tempo, quindi spesso abbassano il voto di troppo
- quando c'è un **pareggio** nella votazione tra due dimensioni che sono consecutive, si prende **la più grande** e si va avanti
- **usare un timer o qualsiasi altra cosa per limitare la discussione**: si possono ad esempio limitare le discussioni a non più di un minuto
- **usare la carta "Ho bisogno di una pausa"**: troppo spesso i team staranno lavorando duro per giocare Planning Poker e non realizzano che alcuni hanno bisogno di una pausa
- **fare incontrare la persona che crea le user story con i lead di QA e Sviluppo prima del Planning Poker** per assicurarsi che le user story abbiano risposte ai quesiti più ovvi che un team può chiedere. Ciò consente al team di concentrarsi sulla dimensione, senza usare il proprio tempo per raccogliere informazioni perché la user story è scritta male
- **ricordare la baseline**: qualunque baseline il team sceglie, essa deve essere consistente da iterazione a iterazione. Se un giorno ideale è di dimensione 1, allora tutte le iterazioni devono usare quel punto di riferimento.