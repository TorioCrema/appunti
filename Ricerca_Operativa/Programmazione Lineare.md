# Programmazione Lineare
## Problema Primale
### Trafromazione in forma standard
Dal problema primale in forma canonica:
$$
\begin{array}{ll}
	\text{min} & z=cx \\
	\text{s.t.} & Ax\ge b \\
	& x\ge0
\end{array}
$$
Otteniamo il problema primale in forma standard aggiungendo una variabile di _slack_ per trasformare i vincoli da disequazioni a equazioni:
$$
\begin{array}{ll}
	\text{min} & z = cx \\
	\text{s.t.} & Ax-Ix_s = b \\
	& x, x_s\ge0
\end{array}
$$
### Soluzioni base
Dato un problema primale in forma standard, la matrice $A$ puo' essere riscritta nella forma $A = [B, N]$ dove $B \in \mathbb{R}^{m,m}$ corrisponde a $m$ colonne linearmente indipendenti e $N \in \mathbb{R}^{m,n-m}$ sono le rimanenti $n-m$ colonne di $A$.
Ponendo $x^T=[x_B,x_N]$, il sistema dei vincoli puo' essere riscritto come:
$$
[B,N]=\left[\begin{array}{l}x_B\\x_N\end{array}\right]\rightarrow Bx_B+Nx_N=b
$$
e poiche' $B$ e' invertibile si ha: $x_B=B^{-1}b-B^{-1}Nx_N$
Se fissiamo $x_N=0$, la soluzione $x=[x_B,x_N]=[B^{-1},0]$ rappresenta una __soluzione base__. Nel caso in cui $x_B\ge0$ (i.e. soddisfa i vincoli di negativita') diremo che $x$ e' una ==soluzione base ammissibile==.
### Ottimizzazione della soluzione base
Il valore della funzione obiettivo corrispondente alla soluzione base $x=[x_B,x_N]=[B^{-1},0]$ e' dato dall'espressione:
$$
z=[c_B,c_N]\left[\begin{array}{l}x_B\\x_N\end{array}\right]= c_BB^{-1}b
$$
Per determinare come varia la funzione obiettivo per valori non nulli delle variabili non base $x_N$, dato che $x_B=B^{-1}b-B^{-1}Nx_N$ avremo:
$$
\begin{array}{l}
z=c_Bx_B+c_Nx_N=\\=c_B(B^{-1}b-B^{-1}Nx_N)+c_Nx_N=\\=c_BB^{-1}b-(c_BB^{-1}N-c_N)x_N
\end{array}
$$
Se definiamo $w=c_BB^{-1}$ possiamo scrivere:
$$
z=wb-(wN-c_N)x_N=wb-\sum_{k\in \mathbb{N}}(wa_k-c_k)x_k
$$
dove $\mathbb{N}$ e' l'insieme degli indici delle variabili/colonne non base.
Se $(wN-c_N)\le0$ la soluzione base ammissibile $x$ e' ottima. Nel caso, invece, esistesse una colonna $k$ non base tale che: $wa_k-c_k <0$ allora il valore della funzione obiettivo puo' decrescere dal valore attuale $z_0=c_BB^{-1}b=wb$ al valore: $z=z_0-(wa_k-c_k)x_k$
Il miglioramento della funzione obiettivo dipende dal valore massimo che la variabile $x_k$ puo' assumere, garantendo che la nuova soluzione sia sempre base ammissibile.
Per determinare quanto possiamo aumentare la variabile $x_k$ dobbiamo considerare l'equazione che determina la soluzione $x_B$ in funzione di $x_N$: $x_B=B^{-1}b-B^{-1}Nx_N$ che possiamo riscrivere come:
$$
x_B=\bar{b}-y^kx_k
$$
dove $\bar{b}=B^{-1}b$ e $y^k=B^{-1}a_k$ (ipotizzando che $x_j=0,\forall j\in \mathbb{N}\setminus\{k\}$)
Per ogni componente i-esima di $x_B, i=1,...,m$ abbiamo che: $x_i=\bar{b}-y^kx_k$
Se vogliamo che la soluzione base rimanga ammissibile dobbiamo aumentare $x_k$ in modo che $x_i=\bar{b_i}-y^kx_k \ge 0$
Quindi per ogni ogni $i$ la variabile $x_k$ deve rispettare la condizione: $x_k\le\frac{\bar{b_i}}{y^k_i}$
Il valore **massimo** che la variabile $x_k$ puo' assumere e' dato dal cosiddetto ==Rapporto di Minimo==:
$$
x_k=\frac{\bar{b_r}}{y^k_i}=min_{i=1,..,m}\left[\frac{\bar{b_i}}{y^k_i}:y^k_i>0\right]
$$
Nel caso in cui $y^k \le 0$ la funzione obiettivo e' illimitata, in quanto $(wa_k-c_K)>0$ e $x_k$ puo' arbitrariamente crescere garantendo l'ammissibilita' della soluzione.
Una volta aggiornato il valore della variabile $x_k$ tutte le variabili $x_i$ in base sono aggiornate come segue:
$$
x_i=\bar{b_i}-y^k_i\frac{\bar{b_r}}{y^k_r}
$$
mentre tutte le altre variabili non base diverse da $k$ rimangono nulle.
La variabile $x_r$ dopo essere stata aggiornata sara' nulla e la colonna $a_k$ sostituisce la colonna $a_r$ nella base $B$. Diciamo che $x_k$ entra in base, mentre $x_r$ esce dalla base.

## Algoritmo del Simplesso Primale
1. **Inizializzazione**: Definisce una soluzione base ammissibile: $x=[x_B,x_N]=[B^{-1}b,0]=[\bar{b},0]$ di costo $c_Bx_B=c_BB^{-1}b$
2. **Pricing**: Calcola $w=c_BB^{-1}$ che equivalea risolvere $wB=c_B$. Calcola i costi ridotti $wa_j-c_j$ per le variabili non-base $j\in \mathbb{N}$ e determina $wa_k-c_k=max_{j\in\mathbb{N}}\{wa_j-c_j\}$
3. **Condizioni di ottimalita'**: se $wa_k-c_k < 0$ allora la soluzione e' ottima
4. **La variabile $k$ e' candidata a entrare in base**: Calcola $y^k=B^{-1}a_k$ che equivale a risolvere $By^k=a_k$. Se $y^k\le0$ allora la soluzione e' illimitata.
5. **Rapporto minimo**: Calcola il valore da assegnare a $x_k$: $x_k=\frac{\bar{b_r}}{y^k_r}=min\{\frac{\bar{b_i}}{y^k_i}:y^k_i>0,i=1,..,m\}$ La variabile $x_r$ esce dalla base e $x_k$ entra al suo posto. Aggiorna $B,N$ e la soluzione base $x=[x_B,x_N]=[\bar{b},0]$. Ritorna allo step 2.
## Problema Duale
Si consideri il problema primale in forma canonica:
$$
\begin{array}{ll}
	z_P=\text{min} & cx \\
	\text{s.t.} & Ax\ge b \\
	& x\ge0
\end{array}
$$
Il cui insieme dei punti ammissibili e' $X=\{x:Ax\ge b,x\ge0\}$
Il suo problema duale e' il seguente:
$$
\begin{array}{ll}
	z_D=\text{max} & wb \\
	\text{s.t.} & wA\le c \\
	& w\ge0
\end{array}
$$
Il cui insieme dei punti ammissibili e' $W=\{w:wA\le c,w\ge0\}$
### Come ottenere il duale
Partendo dal problema primale in forma **standard**:
$$
\begin{array}{ll}
	x_P=\text{min}  & cx \\
	\text{s.t.}& Ax-Ix_s = b \\
	&x,x_s\ge0
\end{array}
$$
Dove $I=[a_{n+1},..,a_{n+m}]=[e_1,\dots,e_m]$ e' la matrice identita' di ordine m, questo perche' le variabili di **slack** del problema appaiono nelle equazioni dei vincoli come con coefficente $-1$ se le disequazioni de vincoli nella forma canonica del problema avevano segno $\ge$. (Quindi per ottenere il duale dobbiamo prima riportare tutti i vincoli nella forma canonica al segno $\ge$ e trasformare il problema nella sua forma standard.)
In corrispondenza di una soluzione ottima del primale deve esistere una base $B$ per cui: $wa_j-c_j\le0, i=1,\dots,n+m$ dove $w=c_BB^{-1}$
Riscrivendo la disequazione $wa_j-c_j\le0$ per le variabili originarie e quelle di slack si ha:
$$wa_j\le c_j, j=1,\dots,n$$
$$-we_i\le0, i=1,\dots,m$$
che in forma matriciale puo' essere riscritta come: $wA\le c$ $w\ge0$ da cui: $W=\{w: wA\le c, w\ge0\}$ e' l'insieme delle soluzioni ammissibili per problema duale.
## Dualita'
### Dualita' debole
Se $\tilde{x}\in X=\{x:Ax\ge b,x\ge0\}$ e $\tilde{w}\in W=\{w:wA\le c, w\ge0\}$ allora $\tilde{w}b\le c\tilde{x}$
### Corollario 1
Se $x^*\in X$ e $w^*\in W$ soddisfano $w^*b=cx^*$ allora $x^*$ e' soluzione ottima del primale e $w^*$ e' soluzione ottima del duale.
### Dualita' forte
Se esistono soluzioni ammissibili sia per il primale che per il duale, allora esistono due soluzioni ottime i cui valori coincidono
#### Teorema 1 (Dualita' forte)
Se $X\ne \emptyset$ e $W\ne \emptyset$, allora esiste una soluzione $x^*$ ottima per il primale e una soluzione $w^*$ ottima per il duale. Inoltre $w^*b=cx^*$
### Condizioni di Complementarieta'
#### Corollario 2
Le soluzioni $\tilde{x}\in X$ del primale e $\tilde{w}\in W$ del duale sono ottime $\iff$ $\text{a} \land \text{b}$ dove:
$$\text{a} \rightarrow \tilde{w}(A\tilde{x}-b)=0$$
$$\text{b} \rightarrow (c-\tilde{w}A)\tilde{x}=0$$
Questo corollario stabilisce che data una soluzione del primale $\tilde{x}\in X$ per dimostrarne l'ottimalita' e' sufficiente trovare una soluzione duale $\tilde{w}\in W$ che soddisfi le condizioni di complementarieta'.
Inoltre $\text{a}$ e $\text{b}$ corrispondono alle equazioni:
$$\text{a'}\rightarrow w_i(a^ix-b_i)=0,\text{ }i=1,\dots,m$$
$$\text{b'}\rightarrow (c_j-wa_j)x_j=0,\text{ }l=1,\dots,n$$
dalle quali si derivano le seguenti osservazioni:
- $w_i>0\implies a^ix=b_i$
- $a^ix>b_i\implies w_i=0$
- $x_j>0\implies wa_j=c_j$
- $wa_j<c_j\implies x_j=0$

## Simplesso Formato Tableau
Permette di semplificare le operazioni di aggiornamento della base, della corrispondente soluzione, e dei costi ridotti $wa_j-c_j$ ad ogni iterazione:
$$
\begin{array}{ll}
	\text{min z}=  & cx_B+cx_N \\
	\text{s.t.}& Bx_B+Nx_N = b \\
	&x_B,x_N\ge0
\end{array}
$$
che si puo' ridurre come:
$$
\begin{array}{ll}
	\text{min}&\text{z} \\
	&\text{z}- cx_B-cx_N=0 \\
	& x_B+B^{-1}Nx_N = B^{-1}b \\
	&x_B,x_N\ge0
\end{array}
$$
Moltiplicando la seconda equazione per $c_B$ e sommandola per la prima si ottiene:
$$
\begin{array}{ll}
	\text{min}&\text{z} \\
	&\text{z} &+&0x_B &+& (c_BB^{-1}N-c_N)x_N &=& c_BB^{-1}b \\
	&&& x_B&+&B^{-1}Nx_N &=& B^{-1}b \\
	&&&x_B&,&x_N&\ge&0
\end{array}
$$
Il risultato puo' essere inserito in un "tableau" come segue:
$$
\begin{array}{|c|c|}
\hline
& \text{z} & x_B & x_N & RHS \\ \hline
\text{z} & 1 & 0 & c_BB^{-1}N-c_N & c_BB^{-1}b \\ \hline
x_B      & 0 & I & B^{-1}N & B^{-1}b \\ \hline
\end{array}
$$
dove il Right Hand Side (RHS) contiene il valore della funzione obiettivo e delle variabili base.
In una versione di maggiore dettaglio il _"tableau"_ e' il seguente:
![tableau](tableau.png)
Come si puo' notare il tableau contiene tutte le informazioni necessarie per l'esecuzione dell'algoritmo del simplesso.
L'operazione base e' il *==pivoting==*. Che permette a una nuova variabile di entrare in base e di aggiornare *correttamente* tutte le informazioni nel tableau (costi ridotti, valore variabili base, etc.).
### Operazione di Pivoting
- Ad ogni iterazione si seleziona la variabile non base candidata ad entrare in base e si definisce con il criterio del rapporto minimo la variabile base che uscira':
	- La variabile entrante si seleziona scegliendo la colonna che massimizza il *costo ridotto* $wa_k-c_k$ presente nella riga $0$.
	- La variabile uscente si seleziona scegliendo la riga che minimizza il rapporto $\frac{\bar{b_i}}{y^k_i}$ con $y^k_i > 0$.
- Si divide la riga $i$ per $y^k_i$ (che sicuramente e' positivo).
- Ad ogni riga $i' \ne i$ si aggiunge la riga $i$ moltiplicata per $-y^k_{i'}$
- Alla riga $0$ si aggiunge la riga $i$ moltiplicata per $-(wa_k-c_k)$.
### Come determinare una base iniziale
#### Caso facile
Se il problema di $n$ variabili e $m$ vincoli ha la seguente forma:
$$
\begin{array}{ll}
z_P & = & \text{min}& cx \\
&&\text{s.t.}&Ax \le b \\
&&&x \ge 0
\end{array}
$$
quando si aggiungono le $m$ variabili $x_s$ di ==slack== alle $n$ variabili originarie, il primale in forma standard e' il seguente:
$$
\begin{array}{ll}
z_P & = & \text{min}& cx \\
&&\text{s.t.}&Ax&+&Ix_s&=&b \\
&&&x&,&x_s&\ge&0
\end{array}
$$
dove $I=[e_1,\dots,e_m]$ e' la matrice identita' di ordine $m$, che senz'altro puo' essere una base $B=I$, la quale e' anche ammissibile se $b\ge0$.
#### Metodo Big-M
Se il problema ha la seguente forma
$$
\begin{array}{ll}
z_P & = & \text{min}& cx \\
&&\text{s.t.}&Ax = b \\
&&&x \ge 0
\end{array}
$$
non e' detto sia facile individuare una base $B$ tra le colonne di $A$.
Nell'ipotesi che $b\ge0$, si possono aggiungere $m$ variabili $x_A$, dette *==artificiali==*, alle n variabili originarie, e risolvere il seguente problema:
$$
\begin{array}{ll}
z_P & = & \text{min}& cx &+&Mx_A  \\
&&\text{s.t.}&Ax&+&Ix_A&=&b \\
&&&x&,&x_A&\ge&0
\end{array}
$$
dove $I$ e' la matrice identita' di ordine $m$ e $M=MI$, con $M>0$ scelto *sufficientemente grande*.
#### Metodo 2-Fasi
Sia dato un problema della seguente forma:
$$
\begin{array}{ll}
(P)&z_P&=&\text{min}&cx \\
&&&\text{s.t.}&Ax&=&b\\
&&&&x&\ge&0
\end{array}
$$
Nell'ipotesi che $b\ge0$, si possono aggiungere $m$ variabili $x_A$, dette *==artificiali==*, alle $n$ variabili originarie, e risolvere il seguente problema:
$$
\begin{array}{ll}
(P')&z_{P'} & = & \text{min}& 1x_A  \\
&&&\text{s.t.}&Ax&+&Ix_A&=&b \\
&&&&x&,&x_A&\ge&0
\end{array}
$$
dove $I$ e' la matrice identita' di ordine $m$ e $1=\{1,1,\dots,1\}$, e' un vettore di $m$ componenti tutte pari a $1$.
In questo caso i problemi $P$ e $P'$ non sono equivalenti.
Risolvere il problema $P'$ serve solo a determinare una soluzione base ammissibile per il problema $P$.
Sia $(x^*,x_A^*)$ la soluzione ottima del problema $P'$ di valore $x_{P'}$. Si possono presentare tre casi:
- $z_{P'}>0\implies$ il problema $P$ non ha una base ammissibile;
- $z_{P'}=0$ e nessuna variabile artificiale e' base$\implies$ il problema $P$ ha una base ammissibile;
- $z_{P'}=0$ e almeno una variabile artificiale e' in base$\implies$ il problema $P$ ha una base ammissibile, ma bisogna *estrarla*:
---
Se $x_{P'}=0$ e una variabile artificiale e' in base, per generare una base senza variabili artificiali e' necessario farla uscire.
Questo caso si verifica quando la soluzione e' ==degenere==,ossia una variabile in base ha valore nullo:
![Tableau di una soluzione degenere](tableau_degenere.png)
se esiste un $y^j_i \ne 0$, allora possiamo ==pivotare== su questo coefficiente e la variabile $x_j$ entra in base al posto della variabile artificiale $x^A_h$.
Se $y^j_i = 0$, per ogni $j=1,\dots,n$, allora possiamo eliminare dal tableau sia la riga $i$ che la colonna della variabile artificiale $x^A_h$.
#### Degenerazione
La ==degenerazione== si presenta quando alcuni $\bar{b_i}$ sono nulli.
In caso di degenerazione, quando si applica il rapporto minimo, il valore da assegnare alla variabile entrante e' sempre nullo:
$$
x_k=\frac{\bar{b_r}}{y^k_r}=\text{min}\{\frac{\bar{b_i}}{y^k_i}:y^k_i>0,i=1,\dots,m\}=0
$$
Per cui cambia la base, ma non il punto estremo che coincide con il precedente. Il rischio e' che il metodo torni a *spostarsi* su una base gia' considerata nelle precedenti iterazioni.
Si consideri il seguente esempio:
![Problema degenerante](esempio_degenerazione.png)
Il metodo potrebbe avere un ==ciclo== che coinvolge le basi associate ai tre punti estremi, A, B e C che corrispondono a tre diverse soluzioni base, ma coincidono.
La ==Regola di Bland== stabilisce che per evitare la ==degenerazione ciclante== e' sufficiente scegliere tra le variabili candidate a entrare in base (i.e., $wa_j-c_j$ positivi) e quelle candidate a uscire dalla base (i.e. che soddisfano il criterio del rapporto minimo) quelle di indice minimo.
Il metodo del simplesso, se implementa un metodo per evitare la degenerazione ciclante, nel caso peggiore deve generare tutte le possibili basi che sono un numero finito:
$$
\binom{n}{m}=\frac{n!}{m!(n-m)!}
$$
## Il Metodo del Simplesso Duale
Il Simplesso primale, ad ogni iterazione, soddisfa:
- L'ammissibilita' primale della soluzione: $\bar{b_r}\ge0$ per ogni $r$;
- Le condizioni di Complementarieta'
Il Metodo del Simplesso Duale, ad ogni iterazione, soddisfa:
- L'ammissibilita' duale della soluzione: $wa_j-c_j\le0$ per ogni $j$;
- Le condizioni di Complementarieta'
Le Condizioni di Complementarieta' sono soddisfatte perche' le variabili $x_j$ che sono in base hanno $wa_j-c_j=0$ mentre quelle non in base hanno valore nullo ($x_j=0$).
![Tableau Duale](tableau.png)
Quando applichiamo il simplesso Duale dobbiamo avere che tutti i vincoli duali siano soddisfatti, quindi che $wa_j-c_j\le0$ per ogni $j$.
Se tutte le variabili sono non negative la soluzione e' OTTIMA.
Se invece almeno una variabile in base ha valore negativo (i.e. esiste almeno un $r$ tale che $\bar{b_r}<0$) allora dobbiamo farla diventare maggiore o uguale a 0.
Se effettuiamo una operazione di pivoting sulla riga $r$ e su una qualunque colonna $k$ tale che $y^k_r<0$ allora nel nuovo tableau avremo $\bar{b_r}>0$, perche' $\bar{b_r}=\frac{\bar{b_r}}{y^k_r}$
La colonna $k$ deve essere scelta in modo da conservare l'ammissibilita' duale: $wa_j-c_j\le0$ per ogni $j$.
Quando pivotiamo sulla riga $r$ e la colonna $k$ la riga $0$ del tableau viene aggiornata come segue:
$$
(wa_j-c_j)=(wa_j-c_j)-\frac{y^j_r}{y^k_r}(wa_k-c_k)
$$
Per conservare l'ammissibilita' duale abbiamo bisogno che:
$$
(wa_j-c_j)-\frac{y^j_r}{y^k_r}(wa_k-c_k)\le0
$$
Siccome $\frac{(wa_k-c_k)}{y^k_r}\ge0$, se $y^j_r\ge0$ avremo che il valore aggiornato di $(wa_j-c_j)$ sara' sicuramente non positivo.
Invece se $y^j_r<0$ avremo bisogno di soddisfare il seguente vincolo:
$$
\frac{wa_k-c_k}{y^k_r}=\text{min}\{\frac{wa_j-c_j}{y^j_r}:y^j_r<0,j=1,\dots,n\}
$$
### Algoritmo del Simplesso Duale
- Step 1: Inizializzazione:
	Sia **B** una base duale ammissibile: $wa_j-c_j=c_BB^{-1}a_j-c_j\le0$
- Step 2: Determinazione riga $r$:
	$\bar{b_r}=\text{min}\{\bar{b_i}:i=1,\dots,m\}$.
	Se $\bar{b}\ge0$, allora STOP perche' la soluzione e' primale ammissibile e quindi ottima.
- Step 3: Determinazione colonna $k$:
	Applica il rapporto minimo:
	$$\frac{wa_k-c_k}{y^k_r}=\text{min}\{\frac{wa_j-c_j}{y^j_r}:j=1,\dots,n\}$$
	Se $y^j_r\ge0$, per ogni $j=1,\dots,n$, allora STOP perche' il duale e' illimitato e la soluzione ammissibile del primale non esiste.
- Step 4: Pivoting
	Svolge una operazione di pivoting sull'elemento $(r,k)$.
	Ritorna allo step 2.
#### NOTE
- Il valore della funzione obiettivo $\text{z}=c_Bx_B=c_BB^{-1}b$ e' un lower bound.
- Durante l'esecuzione del simplesso duale il valore della funzione obiettivo cresce in modo monotonico non descrescente, perche: $c_B\bar{b}=c_B\bar{b}-\frac{\bar{b_r}}{y^k_r}(wa_k-c_k)$
- Come possiamo gestire il caso in cui la base non sia duale ammissibile (i.e., esiste almeno un $j$ per cui $wa_j-c_j>0$)?
	Possiamo aggiungere il seguente vincolo artificiale:
	$$\sum_{j\in N}x_j\le M\implies\sum_{j\in N}x_j+x_{n+1}=M$$
	Dove $N$ e' l'insieme delle variabili non base ed $M\tilde{A}$ un valore positivo sufficientemente grande.
	Dopodiche' si deve pivotare sulla colonna $k$ tale che:
	$$wa_k-c_k=\text{max}\{wa_j-c_j:j\in N\}$$
	Se al termine del simplesso duale $x_{n+1}>0$ la soluzione e' ottima, altrimenti se $x_{n+1}=0$ la soluzione e' illimitata.