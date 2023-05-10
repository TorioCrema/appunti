# Programmazione Lineare
## Problema Primale
### Trasformazione in forma standard
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
$$\text{b'}\rightarrow (c_j-wa_j)x_j=0,\text{ }j=1,\dots,n$$
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
	$$\frac{wa_k-c_k}{y^k_r}=\text{min}\{\frac{wa_j-c_j}{y^j_r}:y^j_r<0,j=1,\dots,n\}$$
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
	Se al termine del simplesso duale $x_{n+1}>0$ la soluzione e' ottima, altrimenti se $x_{n+1}=0$ (cioe' la variabile artificiale e' fuori dalla base) la soluzione e' illimitata.
---

# Programmazione lineare intera
## Metodi Branch and Bound
Consideriamo il problema IP di programmazione lineare intera:
$$
\begin{array}{ll}
&z_{IP} & = & \text{min}& cx  \\
&&&\text{s.t.}&Ax&\ge&b \\
&&&&x&\ge&0& \text{and integer}
\end{array}
$$
Il rilassamento lineare LP del problema IP e' dato da:
$$
\begin{array}{ll}
&z_{LP} & = & \text{min}& cx  \\
&&&\text{s.t.}&Ax&\ge&b \\
&&&&x&\ge&0
\end{array}
$$
Se la soluzione ottima $x^*$ del problema LP e' intera, allora e' la soluzione ottima anche per il problema intero IP.
Cosa accade se invece la soluzione ottima $x^*$ e' frazionaria?
Una possibilita' e' l'applicazione del metodo Branch and Bound, in generale e' necessario ==stabilire un criterio per selezionare una variabile frazionaria== (che deve essere intera nella soluzione ottima di IP), per fare un ==branch==.
### Regola di Branching
Data la soluzione ottima del problema LP e la variabile frazionaria $x_j$ selezionata, una possibile ==regola di branching== puo' prevedere la creazione di due sottoproblemi:
- $P(x_j\le\lfloor x_j\rfloor)$: ottenuto da LP aggiungendo il vincolo $x_j\le \lfloor x_j\rfloor$
- $P(x_j\ge\lceil x_j\rceil)$: ottenuto da LP aggiungendo il vincolo $x_j\ge\lceil x_j\rceil$
La soluzione frazionaria $x_j$ e' esclusa in entrambi i sottoproblemi.
Se consideriamo il problema LP originale come ==nodo radice== di un albero di ricerca, i due sottoproblemi sono i due ==nodi figli==.
Il processo di branching descritto per il nodo radice e' ripetuto per i nodi figli, per cui la regola di branching e' applicata a ogni nodo $k$ dell'albero di ricerca (dove $k=0$ e' il nodo radice e $k=1,2$ i suoi nodi figli).
### Lower e Upper Bound
Per ogni nodo $k$ dobbiamo risolvere il sottoproblema $P_k$ ottenendo la soluzione $x_k$ di costo $z_k$.
- Se $P_k$ ==non ha soluzione ammissibile== poniamo $z_k=+\infty$ e non generiamo nodi figli.
- Se la soluzione $x_k$ di $P_k$ e' ==intera==, allora e' sicuramente un ==upper bound== alla soluzione ottima del problema intero originario IP e non e' necessario generare ulteriori nodi figli del nodo $k$.
- Se la soluzione $x_k$ di $P_k$ e' ==frazionaria==, allora il coso $z_k$ rappresenta un ==lower bound== ($LB_k = z_k$) al valore della migliore soluzione intera che si puo' ottenere nel sottoalbero di cui il nodo $k$ e' il nodo radice.
La miglior soluzione intera ammissibile finora trovata ha costo $z_{UB}$ (i.e. e' il miglior upper bound finora trovato).
L'upper bound $z_{UB}$ puo' essere inizializzato a $+\infty$ oppure con il valore della soluzione generata con un ==algoritmo euristico==.
Oltre a ottenere un upper bound quando la soluzione $x_k$ di $P_k$ e' intera, lo si puo' ottenere utilizzando un ==algoritmo euristico per ripristinare l'ammissibilita' intera== della soluzione $x_k$ quando e' frazionaria.
L'upper bound $z_{UB}$ e' molto importante perche' permette di ==eliminare in anticipo== quei nodi che non consentiranno di ottenere la soluzione ottima, evitando di esplorare il corrispondente sotto-albero.
Se per un nodo $k$ la soluzione di $P_k$ ha costo $z_k \ge z_{UB}$ allora il nodo non deve essere "==espanso==" (i.e. non devono essere generati i nodi figli) perche' non consentiranno di ottenere una soluzione di costo inferiore a $z_{UB}$ (conosciamo gia' una soluzione di costo $z_{UB}$).
Migliore e' il valore $z_{UB}$ e tanto piu' velocemente e piu' veloce sara' la convergenza e la quantita' di memoria risparmiata.
### Metodo di visita dell'albero
Quando a un nodo $k$ viene svolto un branching vengono generati due nodi figli che vengono inseriti in un ==heap==.
L'ordine in cui i nodi sono estratti dalla heap per essere *risolti* determina il metodo di visita dell'albero di ricerca.
I metodi di visita dell'albero di ricerca piu' noti sono i seguenti:
- ==Depth-First Search==: ricerca in profondita', esplorando i nodi figli prima di fare backtracking.
- ==Breadth-First Search==: esplora l'albero di ricerca un livello alla volta, i.e. estrae dalla heap prima i nodi dei livelli di indice piu' piccolo.
- ==Best-First Search==: ad ogni iterazione estrae dalla heap il nodo con il lower bound piu' basso.
Non esiste un metodo migliore di altri. La scelta dipende dal modello che si vuole risolvere e dalle performance a cui si e' interessati.
### Algoritmo Branch and Bound
- Step 1. Poni $z_{UB} = +\infty$ e inserisci nella heap vuota il problema $P_0 : z_0 = \text{min}\{cx:Ax\le b, x\ge0\}$.
- Step 2. Estrai il problema $P_k$ dalla heap (se e' vuota STOP) e ottieni la soluzione $x_k$ di costo $z_k$ ($z_k = +\infty$ se non ha soluzione).
- Step 3. Se $z_k \le z_{UB}$ torna allo Step 2 (il nodo $k$ non viene espanso).
- Step 4. Se la soluzione $x_k$ e' intera, allora aggiorna l'upper bound: $z_{UB} = z_k$ e $x = x_k$ (avendo passato lo Step 3, $z_k < z_{UB}$). Elimina dalla heap tutti i nodi $P_k$ per cui $z_k \le z{UB}$. Torna allo Step 2.
- Step 5. Seleziona una variabile $x_j$ frazionaria nella soluzione $x_k$ e inserisci nella heap due nodi figli $P(x_j \le \lfloor x_j \rfloor$) e $P(x_j \ge \lceil x_j \rceil)$. Torna allo Step 2.

I problemi $P_k$ possono essere risolti usando il ==metodo del simplesso==.
Per risolvere il problema $P_k$ in modo efficiente sarebbe utile partire dalla base del nodo padre. Per cui, quando si salva in heap il nodo dovrebbe essere necessario salvare anche questa informazione.
Per includere in $P_k$ tutti i vincoli imposti dalla regola di branching e' necessario individuare (o conservare) i vincoli definiti dal "cammino" dal nodo $k$ fino alla radice dell'albero di ricerca.

### Tagli di Gomory
Consideriamo un problema $P$ di programmazione lineare intera: $z = \text{min}\{cx:Ax\ge b, x\ge0\text{ e intera}\}$.
Se il problema $P$ ha una soluzione ammissibile e lo risolviamo con il simplesso (primale o duale), potremo ottenere il seguente tableau ottimo:
![](tableau.png)
Ipotizziamo che la variabile in corrispondenza della riga $i$ (in questo caso $x_i$) abbia un valore $b_i$ frazionario.
Quale vincol possiamo aggiungere per "==escludere==" questa soluzione frazionaria dall'insieme delle soluzioni ammissibili senza escludere anche le soluzioni intere?
Costruiamo delle disuguaglianze valide note come ==Tagli di Gomory==.
Se consideriamo il tableau ottimo, l'equazione corrispondente alla riga $i$ puo' essere scritta come segue:
$$x_i+\sum^n_{j=m+1}y^j_ix_j=\bar{b_i}$$
Possiamo decomporre le quantita' $y^j_i$ e $\bar{b_i}$ nelle componenti intere e frazionarie: $y^j_i = I^j_i + F^j_i$ e $\bar{b_i} = I_i + F_i$
dove
$I^j_i = \lfloor y^j_i\rfloor$ e $F^j_i = y^j_i - I^j_i$ con $(0\le F^j_i<1)$
$I_i = \lfloor \bar{b_i}\rfloor$ e $F_i = \bar{b_i} - I_i$ con $(0\le F_i<1)$
Siccome 
$$F_i - \sum^n_{j=m+1}F^j_ix_j < 1$$
ma $F_i - \sum^n_{j=m+1}F^j_ix_j$ deve essere anche intero, allora:
$$F_i - \sum^n_{j=m+1}F^j_ix_j \le 0$$
che corrisponde al seguente vincolo, detto ==Taglio di Gomory==:
$$-\sum^n_{j=m+1}F^j_ix_j\le -F_i$$
che non e' soddisfatto dalla soluzione corrente, perche' $x_j = 0$ per ogni $j = m + 1,\dots,n$ dato che sono non base, mentre $F_i > 0$ visto che abbiamo ipotizzato che $\bar{b_i}$ e' frazionario.
Per semplicita' avevamo ipotizzato che la base fosse costituita dalle prime $m$ colonne. Nel caso generale, il Taglio di Gomory relativo alla variabile in base frazionaria in corrispondenza della riga $i$ puo' essere riscritto come:
$$-\sum_{j\in N}F^j_ix_j\le -F_i$$
dove $N$ e' l'insieme delle colonne non in base.
Se risolviamo il problema con il Simplesso, quando aggiungiamo il nuovo vincolo al problema non e' necessario ripetere l'algoritmo dall'inizio, ma possiamo ripartire dalla base corrente.
Se le variabili attualmente incluse sono $n$, il taglio di Gomory corrisponde a una nuova riga da aggiungere al tableau:
$$-\sum_{j\in N}F^j_ix_j+x_{n+1}=-F_i$$

---
## Metodi di decomposizione
### Rilassamento Lagrangiano
Il rilassamento Lagrangiano permette di ottenere un lower bound per il problema P nel seguente modo:
- alcuni vincoli considerati *difficili* sono rimossi;
- i vincoli rimossi sono inseriti nella funzione obiettivo per mezzo delle *penalita' Lagrangiane*
Per esempio nel problema P, si rilassano i vincoli usando il vettore non negativo di penalita' Lagrangiane $\lambda$, si ottiene la seguente formulazione LR:
$$
\begin{array}{ll}
&z_{LR}(\lambda) & = & \text{min}& cx & + & \lambda(b-Ax)  \\
&&&\text{s.t.}&Bx&\ge&d \\
&&&&x&\ge&0 \text{ and integer} \\
\end{array}
$$

### Dualita' Lagrangiana Debole
$z_{LR}(\lambda)$ e' un valido lower bound al valore della soluzione ottima di P, i.e, $z_{LR}(\lambda) \le z_P$, per ogni $\lambda \ge 0$.
Dimostrazione:
Sia $x^*$ la soluzione ottima di P.
Siccome $x^*$ e' una soluzione ammissibile anche per $LR(\lambda)$, per ogni $\lambda \ge 0$, si ha che:
$$z_{LR}(\lambda,x^*) = cx^* + \lambda(b-Ax^*) \ge z_{LR}(\lambda)$$
Pero' $\lambda(b-Ax^*) \le 0$, perche' $\lambda \ge 0$ e $Ax^* \ge b$, quindi:
$$z_{LR} \le z_{LR}(\lambda, x^*) \le z_P, \forall \lambda \ge 0$$

Il sottoproblema LR e' detto ==problema Lagrangiano==.
Per identificare il vettore di penalita' $\lambda$ che massimizza il lower bound $z_{LR}$ si deve risolvere il cosiddetto ==Lagrangiano Duale==, che puo' essere formulato come segue:
$$z_{LR} = \text{max}\{z_{LR}(\lambda):\lambda\ge0\}$$
dove per ogni vettore di penalita' fissato deve essere risolto un problema Lagrangiano LR, che puo' essere riscritto come:
$$
\begin{array}{ll}
&z_{LR}(\lambda) & = & \text{min}& (c-\lambda A)x &+& \lambda b  \\
&&&\text{s.t.}&Bx&\ge&d \\
&&&&x&\ge&0 \text{ and integer}
\end{array}
$$
Il lower bound $z_{LR}$ fornito dal Lagrangiano duale e' maggiore o uguale a quello fornito dalla soluzione ottima $z_{LP}$ del rilassamento lineare del problema P, i.e. $z_{LR} \ge z_{LP}$.
Se il lower bound $z_{LR}$ e' strettamente minore alla soluzione ottima del problema P, $z_{LR} <  z_P$, allora si dice che c'e' ==Duality Gap.==
Si noti che e' possibile aggiungere al problema LR dei nuovi vincoli ridondanti nella formulazione originale P, ma che possono aiutare la convergenza ed eventualmente migliorare il lower bound $z_{LR}$.
Per risolvere il problema LR il metodo piu' usato e' il ==subgradiente==.

### Set Covering Problem
Il Set Covering Problem (SC) e' il problema di *coprire* le righe di una matrice A, con coefficienti $a_{ij} \in \{0,1\}$ e di dimensione $m \times n$, con un sottoinsieme S di colonne di costo minimo.
Sia $x_j$ una variabile binaria 0-1 uguale a 1 se la colonna $j$ di costo $c_j$ e' in soluzione, 0 altrimenti. Un modello per il problema SC e':
$$
\begin{array}{ll}
&z_{SC} & = & \text{min}& \sum_{j=1}^n c_jx_j  \\
&&&\text{s.t.}&\sum_{j=1}^n a_{ij}x_j&\ge&1, & i=1,\dots,m\\
&&&&x&\in&\{0,1\}, & j=1,\dots,n
\end{array}
$$
Il Rilassamento Lagrangiano del problema SC rispetto ai vincoli e' il seguente:
$$
\begin{array}{ll}
&z_{LR}(\lambda)&=&\text{min}\sum_{j=1}^nc_jx_j&+&\sum_{i=1}^m\lambda_i(1-\sum_{j=1}^na_{ij}x_j) \\
&&\text{s.t.}&x_j \in \{0,1\}, &&j=1,\dots,n
\end{array}
$$
Se si definisce il costo *penalizzato* $c'_j = c_j - \sum_{i-1}^m \lambda_ia_{ij}, j=1,\dots,n$, il problema $LR(\lambda)$ puo' essere riscritto come segue:
$$
\begin{array}{ll}
&z_{LR}(\lambda)&=&\text{min}\sum_{j=1}^nc'_jx_j&+&\sum_{i=1}^m\lambda_i \\
&&\text{s.t.}&x_j \in \{0,1\}, &&j=1,\dots,n
\end{array}
$$
la cui soluzione ottima $x^*$ puo' essere calcolata ponendo, per ogni $j=1,\dots,n$:
$$
x^*_j = 
\begin{cases}
1 & \text{se } c'_j \le 0\\
0 & \text{altrimenti}
\end{cases}
$$
### Dualita' Lagrangiana Forte
Dato il problema P, sia $\bar{x}$ la soluzione ottima di $z_{LR}(\bar{\lambda})$ per un dato $\bar{\lambda}\ge0$. Se $\bar{x} \text{ e }\bar{\lambda}$ soddisfano le seguenti condizioni:
- $\bar{x}$ e' ammissibile per P (i.e., $A\bar{x}\ge b$)
- $\bar{\lambda}(b-A\bar{x}) = 0$

Allora $\bar{x}$ e' la soluzione ottima di P e $z_{LR} = z_{LR}(\bar{\lambda})$.
Dimostrazione (la soluzione $\bar{x}$ e' una soluzione ottima di P):
Essendo $\bar{x}$ una soluzione ammissibile di P si ha:
$$c\bar{x} \ge z_P$$
Per il teorema della dualita' Lagrangiana debole si ha:
$$z_P \ge z_{LR}(\lambda) = c\bar{x}+\bar{\lambda}(b-A\bar{x})$$
dove $\bar{\lambda}(b-A\bar{x}) = 0$, per cui si ottiene:
$$c\bar{x} \ge z_P \ge c\bar{x} \implies z_P = c\bar{x}(=z_{LR}(\bar{\lambda}))$$
Dimostrazione ($z_{LR} = z_{LR}(\bar{\lambda}))$
Dalla definizione di Lagrangiano Duale $z_{LR}$ si ha:
$$z_{LR} \ge z_{LR}(\bar{\lambda})$$
Mentre, dalla dualita' Lagrangiana debole si ha:
$$z_P \ge z_{LR}$$
Quindi, dalle disequazioni precedenti si ottiene:
$$c\bar{x} \ge z_P \ge c\bar{x} \implies z_P = c\bar{x}(=z_{LR}(\bar{\lambda}))$$
Dimostrazione ($z_{LR} = z_{LR}(\bar{\lambda}))$
Dalla definizione di Lagrangiano Duale $z_{LR}$ si ha:
$$z_{LR} \ge z_{LR}(\bar{\lambda})$$
Mentre, dalla dualita' lagrangiana debole si ha:
$$z_P \ge z_{LR}$$
Quindi si ottiene:
$$z_{LR} = z_{LR}(\bar{\lambda})$$
### Rilassamento Lineare vs Rilassamento Lagrangiano
Per comodita' il problema P puo' essere riscritto nella seguente forma compatta:
$$
\begin{array}{ll}
&z_P & = & \text{min}& cx  \\
&&&\text{s.t.}&Ax&\ge&b \\
&&&&x&\in&X
\end{array}
$$
dove $X = \{x:Bx\ge d, x \ge 0 \text{ and integer}\}$
Il rilassamento lineare (LP) del problema p e' ottenuto rilassando il vincolo di interezza delle variabili $x$.
Il valore ottimo $x_{LP}$ del rilassamento lineare di P e' un valido lower bound al valore $z_P$ della soluzione ottima di P, i.e. $z_{LP} \le z_P$.
Si denota con $conv(X)$ l'inviluppo convesso di $X$, che e' dato dall'intersezione di tutti gli insiemi convessi che contengono $X$.
![Inviluppo convesso di X](inviluppo_convesso.png)
### Teorema (Caratterizzazione del Lagrangiano Duale)
Dato il problema $z_P = \text{min}\{cx:Ax \ge b, x \in conv(X)\}$. se si rilassano in modo Lagrangiano i vincoli $Ax \ge b$, il costo della soluzione ottima $z_{LR}$ del Lagrangiano Duale e' uguale al costo della soluzione ottima del seguente problema di programmazione lineare:
$$
\begin{array}{ll}
&z_{LR} & = & \text{min}& cx  \\
&&&\text{s.t.}&Ax&\ge&b \\
&&&&x&\in&X
\end{array}
$$
Dimostrazione:
Il Lagrangiano Duale e' definito come:
$$z_{LR} = \text{max}\{z_{LR}(\lambda):\lambda \ge 0\} = \text{max}_{\lambda\ge0}\{\text{min}_{x\in X}\{cx+\lambda(b-Ax)\}\}$$
Se $T$ e' l'insieme dei punti estremi del poliedro $X$ (chi si e' ipotizzato limitato e non vuoto) allora:
$$z_{LR} = \text{max}_{\lambda\ge0}\{\text{min}_{t\in T}\{cx^t+\lambda(b-Ax^t)\}\}$$
che equivale al seguente problema:
$$
\begin{array}{ll}
&z_{LR} & = & \text{max}& z  \\
&&&\text{s.t.}&z&\le&cx^t + \lambda(b-Ax^t), & t\in T \\
&&&&\lambda&\ge&0
\end{array}
$$
(Questo e' il master della decomposizione di Benders)
Il Duale del problema precedente e':
$$
\begin{array}{ll}
&z_{LR} & = & \text{min}& \sum_{t\in T}(cx^t)\alpha_t  \\
&&&\text{s.t.}&\sum_{t\in T}(b-Ax^t)\alpha_t&\le&0 \\
&&&&\sum_{t\in T}\alpha_t & = & 1\\
&&&&\alpha_t&\ge&0, & t\in T
\end{array}
$$
(Questo e' il master della decomposizione di Dantzing-Wolfe)
Definendo $x=\sum_{t\in T}x^t\alpha_t$, dal problema precedente si ottiente il teorema:
$$
\begin{array}{ll}
& \text{min}& cx  \\
&\text{s.t.}&Ax&\ge&b \\
&&x&\in&conv(X)
\end{array}
$$
Questo teorema e' importante perche':
- caratterizza il Lagrangiano Duale
- stabilisce l'equivalenza tra il rilassamento Lagrangiano e le decomposizioni di Dantzig-Wolte e Benders

### Teorema (Relazione tra Rilassamento Lagrangiano e LP)
Il lower bound ottenuto dal rilassamento Lagrangiano del problema P e' sempre maggiore o uguale del lower bound ottenuto dal Rilassamento Lineare di P, i.e. $z_{LP} \le z_{LR}$
Dimostrazione:
Siccome $conv(X) \subseteq \{x:Bx \ge d, x\ge 0\}$, l'insieme delle soluzioni del Lagrangiano Duale dato da $\{x:Ax\ge b, x\in conv(X)\}$ e' contenuto nell'insieme delle soluzioni di LP dato da $\{x:Ax\ge b,Bx \ge d, x\ge 0\}$:
$$\{x:Ax\ge b, x\in conv(X)\}\subseteq \{x:Ax\ge b, Bx\ge d, x\ge0\}$$
da cui segue che $z_{LP} \le z_{LR}$
Si hanno le seguenti proprieta':
- $z_P = z_{LR}$ se e solo se:
$$
\begin{array}{}
& conv(X \cap \{x:Ax \ge b, x \ge 0\}) \\
& = \\
& conv(X) \cap conv(\{x:Ax \ge b, x \ge 0\})
\end{array}
$$
- $z_{LP} = z_{LR}$ se (Proprieta' di Integralita'):
$$conv(X) = conv(\{x:Bx \ge d, x\ge0\})$$
---
### Metodo del Subgradiente
Il Lagrangiano Duale puo' essere risolto come problema di programmazione lineare, per esempio tramite le decomposizioni di Dantzig-Wolfe e Benders.
Risolvere il Lagrangiano Duale come problema di Programmazione Lineare puo' essere *oneroso* dal punto di vista computazionale.
Per risolvere il Lagrangiano Duale si possono utilizzare dei metodi *euristici*, come il metodo del *subgradiente*.
Per introdurre il metodo del subgradiente e' necessario dimostrare che la funzione Lagrangiana $z_{LR}(\lambda)$ e' concava e definire cos'e' un subgradiente.
#### Teorema
La Funzione Lagrangiana $z_{LR}(\bar{\lambda})=c\bar{x} + \bar{\lambda}(b-A\bar{x})$ e' concava.
Dimostrazione:
Si consideri il seguente esempio:
![Esempio di Funzione Lagrangiana](fun_lagrangiana_convessa.png)
Siano $\lambda_1, \lambda_2 \ge 0$ e $\bar{\lambda}$ una combinazione convessa di $\lambda_1$ e $\lambda_2$, i.e. $\bar{\lambda}=\alpha\lambda_1+(1-\alpha)\lambda_2$ con $\alpha \in [0,1]$. Sia $\bar{x}$ la soluzione ottima di $LR(\bar{\lambda})$:
$$z_{LR}(\bar{\lambda}) = c\bar{x}+\bar{\lambda}(b-A\bar{x})$$
$\bar{x}$ e' una soluzione ammissibile di $LR(\lambda_1)$ e $LR(\lambda_2)$, quindi:
$$
\begin{array}{}
z_{LR}(\lambda_1)\le c\bar{x}+\lambda_1(b-A\bar{x}) \\
z_{LR}(\lambda_2)\le c\bar{x}+\lambda_2(b-A\bar{x}) \\
\end{array}
$$
da cui si ottiene:
$$
\alpha z_{LR}(\lambda_1) + (1-\alpha)z_{LR}(\lambda_2) \le c\bar{x} + (\alpha\lambda_1 + (1-\alpha)\lambda_2)(b-A\bar{x}) = z_{LR}(\bar{\lambda})
$$
dove $\alpha\lambda_1 + (1-\alpha)\lambda_2 = \bar{\lambda}$
Un vettore $s$ e' detto *subgradiente* della funzione $f(\lambda)$ nel punto $\bar{\lambda}$ se soddisfa la seguente condizione:
$$f(\lambda)\le f(\bar{\lambda}) + s(\lambda - \bar{\lambda})$$
![Grafico del subgradiente](subgradiente.png)
Si vuole definire il subgradiente della funzione Lagrangiana:
$$
\begin{array}{}
z_{LR}(\bar{\lambda}) = c\bar{x}+\bar{\lambda}(b-A\bar{x}) & (1)
\end{array}
$$
Per ogni $\lambda \ge 0$ si ha che:
$$
\begin{array}{}
z_{LR}(\lambda) \le c\bar{x}+\lambda(b-A\bar{x}) & (2)
\end{array}
$$
Sottraendo dalla $(2)$ la $(1)$ si ottiene:
$$z_{LR}(\lambda) - z_{LR}(\bar{\lambda}) \le (\lambda - \bar{\lambda})(b-A\bar{x})$$
oppure:
$$z_{LR}(\lambda) \le z_{LR}(\bar{\lambda})+(\lambda-\bar{\lambda})(b-A\bar{x})$$
Per cui $s=(b-A\bar{x})$ e' un sabgradiente della funzione Lagrangiana $z_{LR}(\lambda)$ calcolata nel punto $\bar{\lambda}$.
Affinche' $z_{LR}(\lambda)$ sia maggiore di $z_{LR}(\bar{\lambda})$ e' necessario muoversi nella direzione del subgradiente, perche' $(\lambda - \bar{\lambda})(b-A\bar{x}) > 0$. Quindi, e' necessario che:
$$\lambda=\bar{\lambda}+\theta s$$
dove $\theta > 0$ e' lo spostamento lungo il subgradiente.
Lo spostamento $\theta$ puo' essere calcolato in diversi modi:
- in modo *esatto* risolvendo il problema:
$$\theta^*=\text{argmax}\{z_{LR}(\bar{\lambda}+\theta s): \theta > 0\}$$
garantendo che $z_{LR}(\bar{\lambda}+\theta^*s)\ge(\bar{\lambda})$, pero' potrebbe essere molto costoso.
- in modo euristico, solitamente valutando una semplice equazione, ma non puo' essere garantito che $z_{LR}(\bar{\lambda}+\theta^*s) \ge z_{LR}(\bar{\lambda})$