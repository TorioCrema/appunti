# Analisi di immagini binarie

## Topologia digitale

Disciplina che studia proprieta' e caratteristiche topologiche delle immagini.
Considera immagini binarie, ossia con due soli colori:
- background (valore $0$)
- foreground (in genere $255$ o diverso da $0$)

Sia $F$ l'insieme di tutti i pixel di foreground e $F^*$ l'insieme di quelli di background di un'immagine $Img$:
$$F=\{p|p=[x,y]^T,\;Img[y,x]\neq0\}$$
$$F^*=\{p|p=[x,y]^T,\;Img[y,x]=0\}$$

### Metriche e distanze

Le metriche discrete piu' comuni sono basate su:
- distanza $d_4$ (chiamata anche City-block, Manhattan, $L_1$)
- distanza $d_8$ (chiamata anche Chessboard, Chebyshev, $L_\infty$)

I vicini di un pixel $p$ sono quelli aventi distanza unitaria da $p$
Un percorso di lunghezza $n$ da $p$ a $q$ e' una sequenza di pixel $\{p_0=p,p_1,\dots,p_n=q\}$ tale che, in accordo con la metrica scelta, $p_i$ e' un vicino di $p_{i+1},\; 0\le i<n$.
Una ==componente connessa== e' un sottoinsieme di $F$ (o di $F^*$) tale che, presi due qualsiasi dei suoi pixel, esiste tra questi un percorso appartenente a $F$ (o di $F^*$). A seconda della metrica adottata si parla di 4-connessione o di 8-connessione.

$$\begin{array}{}
p=[x_p,y_p]^T,&q=[x_q,y_q]^T\\
d_4(p,q)=&|x_q-x_p|+|y_q-y_p|\\
d_8(p,q)=&max\{|x_q-x_p|,|y_q-y_p|\}
\end{array}$$

### Trasformata distanza

La trasformata distanza di $F$ rispetto a $F^*$ e' una replica di $F$ in cui i pixel sono etichettati con il valore della loro distanza da $F^*$.

#### Algoritmo sequenziale

Nel caso della metrica $d_4$
La trasformata puo' essere calcolata con due semplici scansioni dell'immagine:
- una diretta (dall'alto verso il basso da sinistra verso destra),
- una inversa (dal basso verso l'alto da destra verso sinistra),
- durante le scansioni i pixel di $F$ vengono trasfromati; i pixel di $F^*$ sono posti a zero (distanza nulla)

$\forall p\in F$, il valore trasformato $Img'[p]$ e' calcolato come segue:
- Scansione diretta: la distanza viene propagata dai vicini in alto a sinistra (che sono gia stati considerati)
	$$Img'[p]=min\{Img'[p_O],Img'[p_N]\}+1$$
- Scansione inversa: la distanza viene aggiornata tenendo conto anche dei percosi verso il basso e a destra
	$$Img'[p]=min\{Img'[p_E]+1,Img'[p_S]+1,Img'[p]\}$$

$$\begin{bmatrix}&p_N&\\p_O&p&p_E\\&p_S\end{bmatrix}$$

#### In OpenCV

```python
img = cv.imread('esempi/mario-bw.png', cv.IMREAD_GRAYSCALE)
dt4 = cv.distanceTransform(img, cv.DIST_L1, 3) # L1
dt8 = cv.distanceTransform(img, cv.DIST_C, 3) # Chebyshev
```

#### Applicazioni della trasformata distanza

- Puo' essere utilizzata per la misurazione geometrica di: lunghezze, spessori,...
- Produce massimi locali di intensita' in corrispondenza degli assi mediani dell'oggetto (scheletro).
	- Nel caso di oggetti allungati la trasformata puo' essere utilizzata per algoritmi di assottigliamento o thinning.
- Applicazioni piu' avanzate della trasformata (o di sue varianti) includono:
	- Template matching
	- Robotica (ricerca direzione di movimento ottimale, aggiramento ostacoli, ...)

## Estrazione del contorno

Il contorno di $F$ e' costituito dalla sequenza ordinata dei pixel che hanno distanza unitaria da $F^*$ secondo la metrica adottata per $F^*$.
Per l'estrazione del contorno di un oggetto viene normalmente impiegata una tecnica di inseguimento che percorre il bordo nella stessa direzione, fino a tornare al pixel di partenza.
Il contorno puo' essere memorizzato in vari modi:
- Semplice lista ordinata di pixel (OpenCV: `cv.CHAIN_APPROX_NONE`)
- Lista dei vertici dei segmenti che lo compongono (OpenCV: `cv.CHAIN_APPROX_SIMPLE`)
- Approssimandolo con una serie di curve (OpenCV: `cv.CHAIN_APPROX_TC89_L1` e `cv.CHAIN_APPROX_TC89_KCOS`)

### In OpenCV

```python
esempi = ('labirinto', 'leaf', 'butterfly', 'bat')
for n in esempi:
	img = cv.imread('esempi/' + n + '.png', cv.IMREAD_GRAYSCALE)
	c, _ = cv.findContours(img, cv.RETR_EXTERNAL, cv.CHAIN_APPROX_NONE)
	res = cv.drawContours(cv.cvtColor(img, cv.COLOR_GRAY2BGR), c, -1, (255,0,0), 2)
```

```python
img = cv.imread('esempi/donuts.png')  
gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)  
# Binarizza con soglia 253 (lo sfondo è bianco)  
_, binarized = cv.threshold(gray, 253, 255, cv.THRESH_BINARY_INV)  
# La funzione restituisce anche una struttura dati ad albero che contiene eventuali  
# relazioni fra contorni (un contorno "padre" contiene uno o più "figli")  
contours, tree = cv.findContours(binarized, cv.RETR_TREE, cv.CHAIN_APPROX_SIMPLE)  
# Disegna tutti i contorni trovati  
allc = cv.drawContours(img.copy(), contours, -1, (255,0,0), 5)  
# Esclude i contorni con area piccola e abbina due flag booleani per sapere se  
# il contorno ha un padre e/o figli  
info = [(c, tree[0,i,2]>=0, tree[0,i,3]>=0) for i, c in enumerate(contours)  
if cv.contourArea(c)>100]  
# Colora diversamente i contorni selezionati che hanno "padri" o "figli"  
res = img.copy()  
for c, f, p in info:  
	cv.drawContours(res, [c], -1, (0,0,255) if p else (255,0,0) if f else (0,255,0), 5)
```

## Etichettatura delle componenti connesse

Procedura molto importante nell'analisi di immagini binarie:
- Individuare automaticamente le diverse componenti connesse in un'immagine, assegnando loro etichette (tipicamente numeriche)

Algoritmo comunemente utilizzato:
1. Si scorre l'immagine e, per ogni pixel di foreground $p$, si considerano i pixel di foreground vicini gia' visitati:
	- se nessuno e' etichettato, si assegna una nuova etichetta a $p$;
	- se uno e' etichettato, si assegna a $p$ la stessa etichetta;
	- se piu' di uno e' etichettato, se assegna a $p$ una delle etichette e si annotano le equivalenze
2. Si definisce un'unica etichetta per ogni insieme di etichette marcate come equivalenti e si effettua una seconda scansione assegnando le etichette finali.

### In OpenCV

```python
img = cv.imread(name, cv.IMREAD_GRAYSCALE)
_, bw = cv.threshold(img, thr, 255, cv.THRESH_BINARY)
n, cc = cv.connectedComponents(bw)
# n è il numero di componenti connesse
# cc è un'immagine di interi in cui il valore di ogni pixel
# è l'indice della componente connessa a cui appartiene
print(f'Componenti connesse: {n}'
```

```python
img = cv.imread('esempi/bolts.png', cv.IMREAD_GRAYSCALE)
_, bw = cv.threshold(img, 152, 255, cv.THRESH_BINARY)
# La funzione cv.connectedComponentsWithStats, oltre all'immagine
# con le etichette, restituisce, per ogni componente connessa,
# le coordinate del baricentro, l'area e il bounding box.
n, cc, stats, centroids = cv.connectedComponentsWithStats(bw)
@interact(i=(0,n-1))
def show_stats(i=0):
	res = cv.cvtColor(img, cv.COLOR_GRAY2BGR)
	res[cc==i,1] = res[cc==i,1]//4 + 255*3//4
	x, y = stats[i,cv.CC_STAT_LEFT], stats[i,cv.CC_STAT_TOP]
	w, h = stats[i,cv.CC_STAT_WIDTH], stats[i,cv.CC_STAT_HEIGHT]
	area = stats[i,cv.CC_STAT_AREA]
	cv.rectangle(res, (x,y), (x+w,y+h), (255,0,0), 3)
	va.center_text(res, f'A[{i}]={area}', centroids[i].astype(int), (0,0,255))
```

## Morfologia matematica

Tecnica di analisi ed elaborazione di immagini binarie derivata dalla teoria degli insiemi
Fornisce strumenti utili a:
- estrarre informazioni per rappresentare e descrivere la forma (contorno, scheletro,...)
- rimuovere particolari irrilevanti mantenendo le informazioni importanti sulla forma degli oggetti.

==Elemento strutturante==:
- Piccola immagine binaria (es. $3\times3$ o $5\times5$) che viene utilizzata come parametro nelle operazioni morfologiche (anch'essa considerata un insieme di pixel di foreground)
- Tipicamente quadrata (lato dispari) e centrata rispetto all'origine

### Notazione

Sia $F$ l'insieme di tutti i pixel di foreground e $F^*$ l'insieme di quelli di background di un'immagine $Img$:
- $F=\{p|p=[x,y]^T,\;Img[y,x]\neq0\}$
- $F^*=\{p|p=[x,y]^T,\;Img[y,x]=0\}$

Operazioni derivate dalla teoria degli insiemi:
- Intersezione e unione: $A\cap B=\{p|p\in A\land p\in B\},\;A\cup B=\{p|p\in A\lor p\in B\}$
- Completamento: $A^c=\{p|p\notin A\}$
- Differenza: $A-B=\{p|p\in A\lor p\notin B\}=A\cap B^c$
- Traslazione rispetto a un punto $q$: $(A)_q=\{p|p=a+q,\;a\in A\}$
- Riflessione rispetto all'origine: $A^r=\{p|p=-q,\;q\in A\}$

### Dilatazione ed erosione

La maggior parte delle operazioni morfologiche si basano su due soli operatori:
- Dilatazione: $F\oplus S$
- Erosione: $F\ominus S$

#### Dilatazione

La nuova immagine e' l'insieme dei pixel tali che, traslando in essi $S^r$, almeno uno dei suoi elementi e' sovrapposto a $F$
$$F\oplus S=\{q|(S^r)_q\cap F\neq\emptyset\}$$

#### Erosione

La nuova immagine e' l'insieme dei pixel tali che, traslando in essi $S$, l'intero elemento strutturante e' contenuto in $F$
$$F\ominus S=\{q|(S)_q\subseteq F\}$$

#### In OpenCV

```python
img = cv.imread('esempi/labirinto.png', cv.IMREAD_GRAYSCALE)
# Elemento strutturante: quadrato 15x15
se = cv.getStructuringElement(cv.MORPH_RECT, (15,15))
d = cv.morphologyEx(img, cv.MORPH_DILATE, se)
e = cv.morphologyEx(img, cv.MORPH_ERODE, se)
# Colora i pixel aggiunti dalla dilatazione
d1 = cv.cvtColor(d, cv.COLOR_GRAY2BGR)
d1[d!=img] = (0,255,0)
# Colora i pixel rimossi dall'erosione
e1 = cv.cvtColor(img, cv.COLOR_GRAY2BGR)
e1[e!=img] = (0,128,255)
```

### Apertura e Chiusura

#### Apertura:
$$F\circ S=(F\ominus S)\oplus S$$
- Erosione seguita da dilatazione
- Separa oggetti debolmente connessi e rimuove regioni piccole

#### Chiusura:
$$F\bullet S= (F\oplus S)\ominus S$$
- Dilatazione seguita da erosione
- Riempie buchi e piccole concavita' e rafforza la connessione di regioni unite debolmente

#### In OpenCV

```python
img = cv.imread('esempi/bolts.png', cv.IMREAD_GRAYSCALE)
_, bw = cv.threshold(img, 140, 255, cv.THRESH_BINARY)
# L'immagine appena binarizzata presenta due problemi:
# - Alcune piccole componenti connesse dovute al rumore
# - Alcuni "buchi" all'interno degli oggetti
s = cv.getStructuringElement(cv.MORPH_ELLIPSE, (5,5))
# L'apertura permette di risolvere il primo problema
res1 = cv.morphologyEx(bw, cv.MORPH_OPEN, s)
# La chiusura permette di risolvere il secondo
res2 = cv.morphologyEx(res1, cv.MORPH_CLOSE, s)
res = img.copy()
res[res2==0]=0
```