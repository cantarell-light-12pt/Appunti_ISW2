Weka non riconosce le scale ordinali e le tratta come nominali, come (HIGH, MEDIUM, LOW). Per risolvere il problema:
- si può rimuovere la colonna con la scala ordinale
- si scompone la colonna in n colonne, una per valore, tutte true/false (meglio non mettere 0/1, perché si assegnerebbe un valore numerico a ognuna)
- se nella scala vi è un valore particolarmente interessante, si potrebbero usare solo due colonne, una per il valore e una per il non-valore, o addirittura una sola colonna true/false per indicare il valore d'interesse (ad esempio HIGH)

Weka non gestisce il nome del file, ma il nome del dataset è specificato nella prima riga del file `.arff`, dopo `@relation`. Il tag `@attribute` indica il nome della feature e il tipo, che può essere:
- `numeric`
- `date`
- label, indicata tra parantesi graffe (ad esempio `{true,false}`)

Il tag `@data` indica l'inizio del CSV contenente i dati. È possibile aprire un CSV direttamente da Weka.

La metrica areaUnderTheROC prende in considerazione tutte le threshold in maniera equa. D'altra parte, la metrica F1 si basa su un'unica threshold, quindi varia al variare di quest'ultima. In entrambi i casi, ci sono pro e contro. L'ideale sarebbe avere un peso per ogni threshold.