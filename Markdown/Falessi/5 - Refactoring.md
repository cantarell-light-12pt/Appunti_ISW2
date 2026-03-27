# Manutenzione
È più complicato manutenere vecchio codice piuttosto che scriverne di nuovo. Infatti, un codice tocca da più persone - con mindset diversi e in momenti diversi - è più difficile da manutenere. Quando sono presenti tante righe di codice, è più probabile trovare dei bug regressivi.

Si può parlare di manutenzione come gli sviluppatori spendono il loro tempo.

È estremamente importante fare evolvere sistemi di grandi dimensioni.
## Refactoring
Il **refactoring** è un'operazione che migliora la struttura interna di un software, senza intaccare il comportamento esterno.
Si tratta di solito di un'operazione di breve durata, che comporta tuttavia un investimenti a lungo termine sulla qualità del sistema.

È importante, oltre a scrivere bene il codice fin da subito, individuare le classi/metodi per cui è utile fare il refactoring.
### Motivazioni del refactoring
Ogni parte del codice del sistema ha tre scopi:
1. eseguire la funzionalità -> spesso ci si concentra solo su questo
2. consentire modifiche
3. comunicare correttamente agli sviluppatori che lo leggono -> la complessità è esponenziale

Se il sistema software non fa anche solo una di queste cose, è "rotto". A questo proposito, è utile applicare i pattern in modo da avere un riferimento comune su alcune pratiche del sistema. D'altra parte, si può correre il rischio di sovra-progettare una soluzione, dato che eccedono in manutenibilità, ma non nelle prestazioni. Chiaramente dipende, perché si possono sempre aggiungere più risorse di calcolo al sistema.

Il refactoring consente di migliorare la progettazione del software, rendendolo più facile da comprendere.
### Dove applicare il refactoring
- codice duplicato
- routine troppo lunghe
- cicli troppo lunghi o molto innestati
- classi con poca coesione
# Come eseguire il refactoring
## Processo di refactoring
Alcune best practice nel refactoring sono:
- eseguire piccole modifiche, dato che si potrebbe incombere in errori
- eseguire tutti i test dopo il refactoring, per assicurarsi che il codice ancora funzioni
- se tutto funziona, si passa al refactoring successivo
- se non funziona, si risolve il problema, o si cancella la modifica in modo da avere ancora un sistema funzionante
## Martin Fowler
Libri:
- sdfsdf
- fsdsd
## Catalogo
Queste sono tutte le possibili attività di refactoring:

[Reference](https://refactoring.com/). [Catalogo](https://refactoring.com/catalog/).
### Esempio 1
Gli statement `switch` sono molto rari nel codice OO progettato a dovere:
- di conseguenza, uno `switch` è un "bad smell" facilmente rintracciabile
- chiaramente, non tutti gli usi degli `switch` sono errati
- non bisognerebbe infatti usare lo `switch` per distinguere diversi tipi di oggetti

Ci sono diversi refactoring in questo caso: il più semplice è la creazione di sottoclassi.

Si consideri il seguente esempio:
```
 class Animal {
	final int MAMMAL = 0, BIRD = 1, REPTILE = 2;
	int myKind; // set in constructor
	...
	String getSkin() {
		switch (myKind) {
			case MAMMAL: return "hair";
			case BIRD: return "feathers";
			case REPTILE: return "scales";
			default: return “skin";
		}
	}
}
```

i può fare il refactoring il codice nel seguente modo:
```
class Animal {
	String getSkin() { return “skin"; }
}

class Mammal extends Animal {
	String getSkin() { return "hair"; }
}

class Bird extends Animal {
	String getSkin() { return "feathers"; }
}

class Reptile extends Animal {
	String getSkin() { return "scales"; }
}
```
### Esempio 2
Quando si lavora con sistemi OO, occorre tenere sempre in considerazione la proprietà di single responsibility di una classe.

Si può dividere una classe in due. Ad esempio, una classe `Customer` che al suo interno ha anche informazioni relativi al numero di telefono. Si può estrarre la classe `Phone`:
```
public class Customer {
	private String name;
	private String workPhoneAreaCode;
	private String workPhoneNumber;
}
```

```
public class Customer {
	private String name;
	private Phone workPhone;
}

public class Phone {
	private String areaCode;
	private String number;
}
```
Nonostante si sia aumentata la dimensione del 50%, non sempre questa metrica è  fondamentale: infatti, in questo caso, la dimensione totale è aumentata, ma quella delle singole classi è diminuita. Occorre sempre calibrare le metriche.
### Esempio 3
Si estrae un metodo da un altro metodo.
### Esempio 4
Si estrae una sottoclasse, magari rendendo la classe originale meno specializzata (ad esempio astratta).
### Esempio 5
Si estrae una super classe.
### Esempio 6
Si riduce un errore del codice con una Exception.
### Esempio 7
Rimpiazzare un parametro con un metodo esplicito.
### Esempio 8
Rimpiazzare una variabile temporanea con una query.


Per le classi, la proporzione tra metodi pubblici e privati può essere un indicatore di quanto sia buggy la classe. Per il progetto, l'attività di risoluzione deve essere sia semplice, sia funzionale: non basta solo definire la metrica per scegliere il metodo su cui applicare il refactoring.
# Debito tecnico
![[Pasted image 20250321123638.png]]

Le **teorie** consentono di definire delle regole di qualità (ad esempio, non i possono avere classi con più di 7 attributi). Si sviluppano dunque **analizzatori statici** che verifichino eventuali **violazioni** delle regole di qualità.

Idealmente, avendo **risorse** infinite, non c'è mai bisogno di eseguire refactoring. Infatti, un'azienda di base ha dei task in cui svolgere attività di **testing**, **feature implementation** e **miglioramenti interni della qualità** (ossia refactoring, che rimuovono le violazioni). L'azienda ha diversi goal, spesso in contrasto l'uno con l'altro, quindi occorre calibrare le attività per rientrare nei limiti delle risorse (tra cui il budget). In particolare, lo stato del sistema oggi va a impattare solo la **release** successiva.