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

# Ottenere Problema Duale
- La funzione obiettivo diventa $\text{max}$
- I coefficienti $b$ dei vincoli primali diventano i costi delle variabili duali nella funzione obiettivo duale
- Le righe della matrice dei vincoli $A$ del primale diventano le colonne nel duale (i vincoli passano da $Ax \le b,\;x\ge0$ a $wA\le c$)
- I vincoli di segno del primale determinano il segno dei vincoli del duale (si inverte il segno da $\le$ a $\ge$ e viceversa)
- I vincoli del primale deteminano i vincoli di segno del duale (se $Ax\le b \implies w\le0$)

# Dimostrare ottimalita' della soluzione
- Verificare i vincoli di segno, se non sono rispettati la soluzione non e' ottima.
- Verificare i vincoli primali:
	Se un vincolo primale e' verificato tramite la disuguaglianza allora la rispettiva variabile duale e' pari a $0$.
- Scrivere il duale.
- Dai valori delle variabili primali nella soluzione da controllare:
	Se una variabile primale e' $\neq 0$ allora il rispettivo vincolo duale deve essere verificato tramite l'uguaglianza.
- Tramite le precedenti informazioni e' possibile create un sistema di equazioni da cui ottenere il valore delle variabili duali
- Controllare i vincoli duali con i valori ottenuti al punto precedente
- Se tutti i vincoli sono rispettati allora la soluzione e' ottima (sia la primale che la duale, per il teorema della [dualita' forte](Ricerca_Operativa#Teorema%201%20(Dualita'%20forte)))

# Branch and Bound LP Intera
Dato un problema LP intero $P$ e il rispettivo tableau ottimo del suo rilassamento lineare:
Scegliere una variabile non intera dalla soluzione fornita e generare i due nodi figli, che saranno uguali al rilassamento lineare, ma con un vincolo aggiuntivo in base alla variable scelta:
- Primo figlio aggiunge il vincolo $x_i \le \lfloor x^*_i\rfloor$, dove $x^*_i$ e' il valore che la variabile $x_i$ assumeva nella soluzione ottima del rilassamento lineare
- Secondo figlio aggiunge il vincolo $x_i \ge \lceil x^*_i\rceil$, dove $x^*_i$ e' il valore che la variabile $x_i$ assumeva nella soluzione ottima del rilassamento lineare

Per risolvere i figli aggiungere la riga del vincolo al tableau ottimo fornito insieme alla colonna della variabile di slack, nel caso del vincolo $\ge$ invertire la disequazione.
- Annullare il coefficiente della variabile selezionata nella riga appena aggiunta
- A questo punto $b_i$ deve essere $< 0$, in caso contrario e' stato fatto un errore nei punti precedenti
- Proseguire con il simplesso duale

# Taglio di Gomory
Dal tableau ottimo scegliere una variabile in base frazionaria e dalla rispettiva riga nel tableau generare un nuovo vincolo:
- Per ogni coefficiente frazionario nella riga selezionata calcolare $y_i - \lfloor y_i\rfloor$ (compreso $b_i$)
- Inserire $y_i - \lfloor y_i\rfloor$ nella nuova riga per ogni $y_i$ frazionario e $1$ per la nuova variabile di slack
- La nuova $b_i$ e' $<0$ (perche' $b_i$ della riga selezionata e' sempre $>0$), si puo' quindi procedere con il simplesso duale

# Rilassamento Lagrangiano / Subgradiente
- Per ogni vincolo rilassato aggiungere i coefficienti alla funzione obiettivo con segno invertito e moltiplicati per $\lambda_i$, non invertire $b_i$
- Raccogliere le variabili primali nella funzione obiettivo che avra' la forma $(c_1 - a_i\lambda)x_1 + \dots + (c_n - a_i\lambda)x_n + \lambda$
- Sostituire il valore di $\lambda$ con quello fornito
- Trovare il valore ottimo delle variabili primali in base ai vincoli rimasti e la funzione obiettivo
- Calcolare il subgradiente tramite la formula $s_i=b_i-a_ix_i-\dots-a_nx_n$
- Si puo' osservare se il vincolo rilassato e' stato rispettato in base al segno del subgradiente ($=0$ rispettato tramite l'uguaglianza, se il vincolo rilassato era $\le$ e' rispettato se $s>0$, se il vincolo era $\ge$ e' rispettato se $s<0$)
- Calcolare 
	$$\theta=\frac{0.001\times z(\lambda)}{\lVert s\rVert_2^2}$$
- Trovare il nuovo valore di $\lambda$:
	- se il vincolo rilassato era $\le$ : $\lambda = \text{min}\{0,\; \lambda+\theta s\}$ (sempre $\le0$)
	- se il vincolo rilassato era $\ge$: $\lambda = \text{max}\{0,\; \lambda+ \theta s\}$ (sempre $\ge0$)