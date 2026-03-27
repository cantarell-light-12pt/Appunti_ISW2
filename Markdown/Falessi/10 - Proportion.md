Per definire una entità software (commit, metodo o classe) come buggy, è necessaria la fix del problema. C'è inoltre del tempo nel quale il bug vive, prima che venga eliminato. Questo tempo può provocare del "rumore", poiché possono essere effettuate delle misurazioni per cui il bug è presente ma non è stato ancora scoperto. Non sempre un bug dormiente crea rumore (snoring): ciò avviene solo se impatta il labeling, ossia se non c'è nessun altro bug che viene risolto. Ad esempio, se una classe ha 5 bug, di cui 4 verranno risolti tra 30 release (perché scoperti solo allora), e 1 risolto alle release successiva, la classe sarà già etichettata come buggy (fino a) quest'ultima release.

Si ricorda che, per determinare i metodi buggy nelle varie release, si può vedere il commit della fix del bug, quindi le entità modificate nel commit e marcare tutte queste come buggy utilizzando il campo "affected version". In particolare, si distinguono:
- **introduction version** (o **injection version**, **IV**): la versione in cui il bug è stato introdotto
- **opening version** (**OV**): la versione in cui il bug è stato scoperto
- **affected versions** (**AVs**): tutte le versioni che contengono il bug, ossia quelle comprese tra la IV e la OV
- **fixed version** (**FV**): la versione in cui il bug è stato risolto.

**SZZ** è il processo che mira a identificare quando un difetto è stato introdotto in un progetto software, sfruttando il meccanismo di annotazione del sistema di versionamento (ad esempio `git blame`).
Dato un commit di tipo "fix", l'algoritmo determina, per le linee di codice sorgente che sono state cambiate, quando sono state cambiate prima di tale fix.
L'approccio presenta alcune limitazioni:
- non è accurato alla perfezione
- non si possono trovare bug di tipo regressivo
- non si possono trovare bug in cui sono solo state scritte nuove righe di codice e non ne è stata modificata alcuna.
# Proportion
## Introduzione
### Scopo
Si vuole capire fino a che punto l'informazione relativa alla AV è disponibile in progetti open-source e se il suo utilizzo sia affidabile nella classificazione delle classi come difettive o meno, al fine di costruire dei dataset dei difetti.

Si vuole dunque proporre, valutare e confrontare a SZZ un nuovo metodo per la categorizzazione di classi difettive, basato sull'idea che i difetti, anche su progetti diversi hanno un ciclo di vita stabile.

Una **revisione** è una modifica subita dal codice a seguito di un commit. Una **versione** (**release**) è una revisione che va in produzione. La versione di un ticket è la prima versione del codice che segue la data di creazione del ticket. Per il fix vale lo stesso, ossia la versione di un fix è la versione seguente del fix.
### Research question
- **RQ1**: la AV è disponibile e consistente?
- **RQ2**: i metodi hanno un'accuratezza diversa nel labeling della AV?
- **RQ3**: i metodi hanno un'accuratezza diversa nel labeling delle classi?
- **RQ4**: i metodi portano a identificazioni diverse di feature importanti?
#### RQ1
##### Rationale
Si vuole sapere se la AV è disponibile, ossia, se gli sviluppatori inseriscono la AV nelle informazioni dei difetti dei ticket. Se ciò è vero, si vuole anche capire se la AV è consistente, ossia se la AV più vecchia non è successiva alla OV.
##### Progettazione
La variabile indipendente è il difetto, mentre le variabili dipendenti sono la percentuale di AV disponibili e quelle disponibili e consistenti.

Sono stati utilizzati 212 progetti open source collegati con Git e Jira. I bug selezionati seguono questo criterio:
```
Type=="Bug" AND (status=="Closed" OR status=="Resolved") AND Resolution=="Fixed"
```
In questo modo, si considera solo i bug corretti, evitando errori di segnalazione. Si escludono inoltre i bug che non hanno un commit di fix in Git e quelli che non sono post-release.
##### Risultati
![[Pasted image 20250518161856.png]]
Il grafico riporta la proporzione dei progetti analizzati aventi report di bug con una AV non affidabile (sinistra) o senza la AV (destra).
##### Discussione
Dai risultati, si nota che in circa il 30% dei ticket di tipo "bug" non è indicata la AV. In particolare, il numero totale di bug chiusi e agganciati con Git nei 212 progetti analizzati è 125860, di cui 65539 (~51%) non hanno indicata la AV, oppure è inaffidabile.

Segue che, nella maggior parte dei progetti Apache, non si può usare la AV e dunque è necessario un metodo automatico per classificare le classi.