Con l'approccio SZZ, esistono i concetti di:
- injected version (IV) -> versione in cui  il bug è stato inserito
- affected versions (AV) -> versioni affette dal bug
- fix version (FV) -> versione immediatamente successiva al commit con il fix del bug
Deve essere IV <= AV < FV.

Spesso nel ML si ha del rumore dei dati dovuto a bug "dormienti", che quindi provocano **snoring**. Infatti, i bug possono essere scovati solo dopo un'attenta analisi, che può avvenire sì allo stesso momento del'introduzione, ma il più delle volte avviene dopo.

L'approccio SZZ implica che un metodo sia buggy o non buggy a seconda del momento in cui viene effettuato il labeling. Ad esempio, si considerino le seguenti classi e l'evoluzione dei bug nelle varie release (I=Injected, N=Nothing, F=Fixed):
![[Pasted image 20250402124010.png]]

Dato che la fix permette di capire le classi difettose o meno, a seconda di quando si effettua la misurazione, la difettosità cambia:
- se ci misura alla release 2
	![[Pasted image 20250402124318.png]]
- se si misura alla release 3
	![[Pasted image 20250402124345.png]]
**Nota**: non importa a che punto della release di esegue la misurazione.

Senza la fix non si conosce neanche il metodo, mentre l'affected version server a capire quanto andare indietro nel tempo. In pratica, si può conoscere la presenza di un bug nel passato solo a seguito di una fix futura. È un discorso analogo alla presenta di un testimone in un processo, o la dimostrazione di una teoria errata.

Tuttavia, se un difetto è presente e non si ha la sua fix, ci sarà sicuramente un rumore nei dati.

I dati e le loro relazioni cambiano nel tempo. Si vorrebbero:
- dati freschi
- tanti dati (che non potranno essere tutti freschi)
e c'è il problema dello snoring.
## Positive & negative
Il **true** è una misura di predizione. Il valore **actual** è il valore vero di un'istanza, mentre il valore **predicted** è quello predetto dal modello.

In un contesto specifico, il **positive** indica ciò che si cerca: è una convinzione, su cui occorre essere d'accordo. In particolare, **positive** o **negative** indica il valore stimato. D'altra parte, **true** e **false** indicano la correttezza della predizione. Si hanno allora le seguenti combinazioni:
- **true positive** è un'actual positive/predicted positive
- **false positive** è un'actual negative/predicted positive
- **true negative** è un'actual negative/predicted negative
- **false negative** è un'actual positive/predicted negative

Questo approccio viene solitamente applicato sulla predizione, ma nel caso dello snoring si applica sulla misurazione, essendo questa - potenzialmente - errata a seconda del momento in cui viene eseguita. Lo snoring può dare vita al false negative, ma non al false positive.
# Domande
## Introduzione
- Quanto dorme un difetto?
- Quanto una classe (o entità) "russa"?
	- "Non sempre chi dorme russa" ~ D.F.
	- un bug russa se causa una labeling incorretta per quella data release; ne basta trovare uno per l'entità considerata, dato che gli altri non russano. Infatti, un bug russa per una release a seguito di una misurazione effettuata in un'altra release. Dunque, un'entità per non russare ha bisogno di un altro bug che viene fixato nella stessa entità.

In generale, ci si aspetta che l'avanzare del tempo migliori l'efficienza della predizione, dato che l'applicazione verrà utilizzata di più.

Ci interessa non tanto che ci sia snoring, quanto che esso impatti o meno sulla predizione.

Quando si eliminano le release dal dataset, non si eliminano tutte, ma si mantiene la porzione buggy, dato che lo snoring si nasconde solo dietro le istanze classificate come negative, producendo false negative. Il numero di release rimosse può differire il base al progetto.