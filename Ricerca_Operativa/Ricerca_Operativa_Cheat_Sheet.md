# Simplesso Primale
- $b \ge 0,\; \forall b$
- $z$ diminuisce ad ogni iterazione
- $wa_j-c_j = 0,\; \forall x_B\;\text{in base}$
- metodo 2 fasi per ottenere una base, se alla fine della fase 1 $z > 0$ il problema non ha una base ammissibile, se $z = 0$ e nessuna variabile artificiale e' base il problema ha una base ammissibile, se $z =0$ ma almeno una variabile artificiale e' base [bisogna estrarre la base](Ricerca_Operativa#Estrazione%20base).
- se $\exists\; wa_j-c_j > 0 \land y_j \le 0$, (esiste una colonna in cui il costo ridotto e' maggiore di zero, ma non possiamo pivotare perche' tutta la colonna e' minore o uguale a zero) $\implies$ la soluzione e' illimitata
- portare le disequazioni dei vincoli a $\le$ (se possibile, cioe' se il vincolo e' $\ge$ e $b<0$) in modo da non dover usare le variabili artificiali per ottenere la base

# Simplesso Duale
- $z$ aumenta ad ogni iterazione
- $b_j$ puo' essere $\le0$, quindi possiamo portare tutti i vincoli alla forma $\le0$
- $wa_j-c_j \le0,\;\forall j$, usare il [Metodo Big M](Ricerca_Operativa#Metodo%20Big-M), pivotare su $\text{max}\{wa_j-c_j\}$
- se $\exists\; b_j<0\land y_j>0$ se esiste un $b_j<0$, ma non possiamo pivotare perche' tutta la riga e' $>0\implies$ non esiste una soluzione ammissibile