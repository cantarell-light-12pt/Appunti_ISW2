Alcuni degli strumenti più noti per l'esecuzione di test sono:
- JUnit come test driver
- Mockito per la creazione di stub/mock ([ref](https://site.mockito.org/))
# Mockito
**Mockito** è un framework per la definizione di stub a supporto del testing in Java.

Fornisce un insieme di primitive che consentono di:
- dichiarare stub/mock
- definire valori di ritorno per operazioni su specifiche istanze
- (ri)definire comportamenti per operazioni in base a specifiche istanze in input

Supporta nativamente paradigmi di programmazione agile guidati dalla scrittura di casi di test, quali:
- Test Driven Development (TDD)
- Behavior Drive Development (BDD)

Alcuni costrutti chiave di Mockito sono:
- creazione programmatica dei mock/stub:
	- `mock()` -> altera il comportamento della classe, aggiungendo o rimuovendo funzionalità, anche corner case (**Nota**: non è bene definire mock che interagiscano ad ogni scenario, bensì definirne di diversi per ogni scenario possibile)
	- `spy()` -> una certa istanza di un'unità è osservata e si vuole verificare se ne viene invocato un metodo, modificato lo stato ecc.
- dichiarazioni semplificate per mezzo di annotazioni sintattiche:
	- `@Mock`
	- `@Spy`
	- `@Captor`
	- `@InjectMocks` -> l'istanza annotata in questo modo verrà considerata da Mockito per creare al suo interno dei mock, ossia fare il mock di alcuni suoi attributi (specificati in fase di definizione dei test); la differenza è che, in questo caso, non si vuole fare il mock di tutte le istanze di una classe, ma solo delle istanze che compongono gli attributi della classe annotata con `@InjectMocks`

Esempio:
```
// Let's import Mockito staticallu so that the code looks clearer
import static org.mockito.Mockito.*;

// Mock creation
List mockedList = mock(List.class);

// You can mock concrete classes, not just interfaces
LinkedList mockedLinkedList = mock(LinkedList.class);
```
**Nota**: quando si ha a che fare con singleton o attributi statici, possono essere creati effetti indesiderati dall'esecuzione di un test che vanno a impattare altri test, venendo meno alla loro indipendenza. In questi casi, vanno definiti dei metodi per rimuovere questi elementi.

- definizione programmatica dei valori di ritorno
	- `Mockito.when(mockedInstance.fooOperation()).thenReturn(true)` -> cattura la chiamata a `fooOperation()` di `mockedInstance` e sostituisce la vera implementazione il valore di ritorno definito in `thenReturn`, ossia `true`
	- `BDDMockito.given(mickedInstance.fooOperation()).willReturn(true)`
- programmazione personalizzata dei comportamenti
	- `Mockito.when(mockedInstance.fooOperation()).thenAnswer(new Answer<Object>() { ... })` -> al posto dei `...` si mette una nuova implementazione personalizzata del metodo `fooOperation`, detta Answer, che viene sostituita da Mockito nella chiamata all'implementazione vera e propria del metodo
- verifica programmatica se i mock sono stati usati/riferiti
	- `Mockito.verify(mockedInstance).fooOperation()` -> si verifica che il metodo `fooOperation` sia stato chiamato
	- `BDDMockito.then(mockedInstance).should().fooOperation()`
- integrazione con JUnit:
	- `MockitoJUnitRunner`
	- `MockitoJUnit.rule()`