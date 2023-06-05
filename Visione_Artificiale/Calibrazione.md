# Lenti sottili

- Fuochi $F_l$ e $F_r$:
	Due punti esterni al corpo della lente situati sull'asse ottico a una certa distanza dal centro $O$.
	Assumiamo per semplicita' che $F_j$ e $F_r$ abbiano la stessa distanza $f$ da $O$ (==lunghezza focale==).
- Proprieta'
	- Ogni raggio di luce che entra parallelamente all'asse ottico e' deviato verso l'altro fuoco
	- Ogni raggio che entra da un lato passando per il fuoco esce parallelamente all'asse ottico
	- Ogni raggio che passa dal centro della lente mantiene la sua direzione

## Equazione fondamentale

Date le distanze $\tilde{Z}$ fra lente e oggetto e $\tilde{z}$ fra lente e immagine, si ha:
$$\frac{1}{\tilde{Z}}+\frac{1}{\tilde{z}}=\frac{1}{f}$$
Da questo deriva che: se un oggetto e' posto a distanza $\tilde{Z}$ sull'asse di una lente con lunghezza focale $f$, su uno schermo posto a distanza $\tilde{z}$ si formera' l'immagine dell'oggetto.

## Pofondita' di campo (Depth of Field)

Con una lente non tutto e' a fuoco
Profondita' di campo:
- L'intervallo di distanze considerabili "a fuoco" nell'immagine
- Minore e' l'apertura, maggiore e' la profondita' di campo

## Proiezione di una scena 3D su un'immagine 2D

Qualsiasi sia il sistema di acquisizione (camera oscura, occhi umano, fotocamera con lente sottile), l'immagine si forma attraverso la ==proiezione geometrica== di una scena 3D su un piano bidimensionale.
Cosa succede in questo tipo di proiezione geometrica:
- Le dimensioni degli oggetti sono inversamente proporzionali alla distanza
- Le linee rette rimangono tali
- Le linee parallele divengono linee convergenti
- Le lunghezze e gli angoli non sono preservati

## Proiezione prospettica

Il sistema di riferimento della camera coincide con quello del mondo 3D:
- il centro di prioiezione o fuoco della camera coincide con l'origine $O$ degli assi
- il piano immagine o piano di proiezione $\pi$ e' perpendicolare all'asse $Z$
- la distanza tra $O$ e $\pi$ e' la lunghezza focale $f$

### Equazioni fondamentali

Quale relazione lega le coordinate 3D di un punto $P = (X,Y,Z)$ nello spazio alle coordinate della sua proiezione $p = (x,y)$ sul piano immagine $\pi$?
- Dato che i triangoli $pOQ$ e $POR$ sono simili, si ha:
$$\overline{pQ}:\overline{PR}=\overline{OQ}:\overline{OR}\implies y:Y=f:Z$$
- da cui (e procedendo analogamente per $x$), si ottengono le due equazioni fondamentali:
$$x=f\times\frac{X}{Z}$$
$$y=f\times\frac{Y}{Z}$$

## Parametri estrinseci

Il modello precedente assume che il sistema di riferimento della camera sia lo stesso della scena 3D.
Nelle applicazioni pratiche questo di solito non avviene: e' necessario trasformare le coordinate della scena 3D in quelle della camera prima di applicare le equazioni fondamentali.
Sia $P_w=(X_w,Y_w,Z_w)$ un punto nelle coordinate della scena: per trasformarlo nelle coordinate $P_c=(X_c,Y_c,Z_c)$ del sistema di riferimento della camera sono necessari:
- una matrice di rotazione $R$ (ottenuta a partire da 3 angoli)
- un vettore di traslazione $t$ (3 componenti)

Questi $6$ parametri sono chiamati ==parametri estrinseci== della camera:
Possono essere utilizzati per descrivere il movimento di una camera attorno a una scena statica, oppure il movimento di un oggetto rispetto a una camera ferma.

### Modello generale

- Coordinate 3D "mondo": $P_w=(X_w,Y_w,Z_w)$
	$P_c = R\times P_w + t$
- Coordinate 3D "camera": $P_c=(X_c,Y_c,Z_c)$

Matrice di rotazione 3D: $R=\begin{bmatrix}r_{11}&r_{12}&r_{13}\\r_{21}&r_{22}&r_{23}\\r_{31}&r_{32}&r_{33}\end{bmatrix}$
$R$ ha $9$ elementi, ma i gradi di liberta' sono $3$: puo' essere descritta, ad esempio con $3$ angoli di rotazione (angoli di Eulero):
$$R=R(\phi,\theta,\psi)=R_Z(\phi)\times R_Y(\theta)\times R_X(\psi)$$
$$\begin{bmatrix}cos\phi&-sin\phi&0\\sin\phi&cos\phi&0\\0&0&1\end{bmatrix}\times\begin{bmatrix}cos\theta&0&-sin\theta\\0&1&0\\sin\theta&0&cos\theta\end{bmatrix}\times\begin{bmatrix}1&0&0\\0&cos\psi&-sin\psi\\0&sin\psi&cos\psi\end{bmatrix}$$

## Parametri intrinseci

Quando le coordinate sono espresse nel sistema di riferimento della camera, si possono applicare le equazioni fondamentali della proiezione prospettica:
$$\begin{array}{}x=f\times\frac{X_c}{Z_c}&y=f\times\frac{Y_c}{Z_c}\end{array}$$
Per determinare esattamente la trasformazione dalla scena all'immagine, sono necessari anche altri parametri, specifici della camera:
- Lunghezza focale $f$ (nelle telecamere e' una carattestica dell'obiettivo);
- Coordinate del punto principale $(c_x,c_y)$: non e' detto infatti che il centro del piano immagine risieda sull'asse ottico;
- la relazione tra la dimensione dei pixel e l'unita' di misura della scena 3D $(s_x,s_y)$, in altre parole quanto misura un pixel

Questi parametri, specifici della camera, sono chiamati ==parametri intrinseci==.

### Modello generale

- $P_w=(X_w,Y_w,Z_w)$
- $P_c = R\times P_w + t=(X_c,Y_c,Z_c)$
- $\begin{array}{}x=f\times\frac{X_c}{Z_c}&y=f\times\frac{Y_c}{Z_c}\end{array}$
- $P_c=(x,y)$
- $\begin{array}{}u=\frac{x}{s_x}+c_x&v=\frac{y}{s_y}+c_y\end{array}$
- $p=(u,v)$

## Coordinate omogenee e rototraslazione

In coordinate omogenee, due vettori (punti nello spazio 3D) sono equivalenti se differiscono solo di un fattore di scala:
$$P=(X,Y,Z)\iff \tilde{P}=(w\times X,w\times Y,w\times Z,w)=(X,Y,Z,1),\;w\neq0$$
Rototraslazione da $P_w$ a $P_c$ in coordinate omogenee:
$$P_c=\begin{bmatrix}X_c\\Y_c\\Z_c\end{bmatrix}=R\times P_w+t=\begin{bmatrix}r_{11}&r_{12}&r_{13}\\r_{21}&r_{22}&r_{23}\\r_{31}&r_{32}&r_{33}\end{bmatrix}\times\begin{bmatrix}X_w\\Y_w\\Z_w\end{bmatrix}+\begin{bmatrix}t_1\\t_2\\t_3\end{bmatrix}$$
$$\begin{bmatrix}P_c\\1\end{bmatrix}=\begin{bmatrix}X_c\\Y_c\\Z_c\\1\end{bmatrix}=\begin{bmatrix}r_{11}&r_{12}&r_{13}&t_1\\r_{21}&r_{22}&r_{23}&t_2\\r_{31}&r_{32}&r_{33}&t_3\\0&0&0&1\end{bmatrix}\times\begin{bmatrix}X_w\\Y_w\\Z_w\\1\end{bmatrix}=\begin{bmatrix}R&t\\0&1\end{bmatrix}\times\begin{bmatrix}P_w\\1\end{bmatrix}$$

Proiezione da $P_c=(X_c,Y_c,Z_c)$ a $p=(u,v)$ in coordinate omogenee:
$$p=\begin{bmatrix}u\\v\end{bmatrix}=\begin{bmatrix}\frac{f}{s_x}\times\frac{X_c}{Z_c}+c_x\\\frac{f}{s_y}\times\frac{Y_c}{Z_c}+c_y\end{bmatrix}$$
$$\begin{bmatrix}p\\1\end{bmatrix}=\begin{bmatrix}u\\v\\1\end{bmatrix}=\begin{bmatrix}Z_c\times u\\Z_c\times v\\Z_c\end{bmatrix}=\begin{bmatrix}\frac{f}{s_x}&0&c_x&0\\0&\frac{f}{s_y}&c_y&0\\0&0&1&0\end{bmatrix}\times\begin{bmatrix}X_c\\Y_c\\Z_c\\1\end{bmatrix}$$

### Modello Generale

$$\begin{bmatrix}u\\v\\1\end{bmatrix}=\begin{bmatrix}\frac{f}{s_x}&0&c_x&0\\0&\frac{f}{s_y}&c_y&0\\0&0&1&0\end{bmatrix}\times\begin{bmatrix}r_{11}&r_{12}&r_{13}&t_1\\r_{21}&r_{22}&r_{23}&t_2\\r_{31}&r_{32}&r_{33}&t_3\\0&0&0&1\end{bmatrix}\times\begin{bmatrix}X_w\\Y_w\\Z_w\\1\end{bmatrix}$$

## Distorsione radiale

Le lenti generalmente causano una certa distorsione nell'immagine, ossia uno spostamento dei punti rispetto a quanto previsto dal modello geometrico teorico.
Il tipo piu' comune di distorsione e' quella radiale, che puo' essere modellata come segue.
- Date le coordinate (non distorte) $p_c=(x,y)$, le coordinate effettive (distorte) $p'_c=(x',y')$ sono:
$$\begin{array}{}x'=x\times(1+k_1r^2+k_2r^4+k_3r^6)\\y'=y\times(1+k_1r^2+k_2r^4+k_3r^6)\end{array}$$
dove $r^2=x^2+y^2$

## Modello completo con distorsione

- $P_w=(X_w,Y_w,Z_w)$
- $P_c = R\times P_w + t=(X_c,Y_c,Z_c)$
- $\begin{array}{}x=f\times\frac{X_c}{Z_c}&y=f\times\frac{Y_c}{Z_c}\end{array}$
- $P_c=(x,y)$
- $\begin{array}{}x'=x\times(1+k_1r^2+k_2r^4+k_3r^6)\\y'=y\times(1+k_1r^2+k_2r^4+k_3r^6)\end{array}$
- $\begin{array}{}u=\frac{x'}{s_x}+c_x&v=\frac{y'}{s_y}+c_y\end{array}$
- $p=(u,v)$

## Calibrazione

Consiste nel determinare i parametri estrinseci ed intrinseci di una camera:
- Parametri estrinseci:
	- Matrice di rotazione $R$
	- Vettore di traslazione $t$
- Parametri intrinseci:
	- Lunghezza focale $f$
	- Parametri del modello di deformazione: $k_1,k_2,k_3,\dots$
	- Coordinate del punto principale $(c_x,c_y)$
	- Dimensione dei pixel $(s_x,s_y)$
Disponendo di un numero sufficiente di corrispondenze fra punti 3D della scena e punti 2D dell'immaigine, e' possibile stimare i vari parametri

### Ricerca di punti corrispondenti

Di solito avviene scegliendo un sistema di coordinate 3D per la scena e introducendo un oggetto rigido di aspetto noto su cui sia semplice individuare un insieme di $m$ punti.
Da una serie di $n$ immagini con posizioni diverse (dell'oggetto o della camera), si ottengono quindi, per ogni immagine, una serie di corrispondenze $(P^i_w,p^i)$ fra coordinate nella scena 3D $P^i_w$ e coorinate 2D $p^i$.
Il pattern piu' comunemente usato e' una scacchiera, che consente di individuare facilmente l'insieme di punti corrispondenti ai vertici interni (i punti di contatto fra quattro caselle).

### In OpenCV

OpenCV implementa il modello geometrico (pinhole camera model) e offre molte funzionalita', fra cui:
- Proiezione di punti 3D sull'immagine (conversione da $P_w$ a $p$) dati i parametri intrinseci ed estrinseci: `cv.projectPoints()`
- Individuazione dei vertici interni di un pattern scacchiera su un'immagine, con possibilita' di raffinare la ricerca con precisione sub-pixel e di disegnare i punti trovati: `cv.findChessboardCorners()`, `cv.cornerSubPix()`, `cv.drawChessboardCorners()`
- Stima dei parametri intrinseci ed estrinseci da un insieme di corrispondenze $(P^i_w,p^i)$ individuate in una serie di immagini di un pattern di calibrazione: `cv.calibrateCamera()`
- Rettifica della deformazione di un'immagine dati i parametri intrinseci della camera: `cv.undistort()`, `cv.getOptimalNewCameraMatrix()`
- Funzioni per lavorare con matrici di rotazione 3D: `cv.RQDecomp3x3()`, `cv.Rodrigues()`.