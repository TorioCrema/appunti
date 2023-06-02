# Immagini Grayscale
Matrici di "punti di luce", il valore dei pixel e' la quantita' di luce. Maggiore e' il valore piu' alta e' la luminosita' del pixel. Coordinate cartesiane (x, y). Il pixel (0,0) e' quello in alto a sinistra dell'immagine, l'asse $y$ cresce verso il basso, l'ase $x$ cresce verso sinistra.

# Immagini a Colori
Tensori 3D. Ogni canale contiene la luminosita' di un colore primario (Red, Green, Blue). Sommando la luminosita' dei tre colori primari si ottengono i vari colori.
**Modello additivo**:
- I colori sono ottenuti mediante combinazione dei 3 colori primari Red, Green, Blue
- Il piu' utilizzato in informatica per la semplicita' con cui si generano i colori
**Spazio RGB**:
- Ogni colore puo' essere considerato come *un punto in uno spazio a tre dimensioni*
- Non idoneo per il raggruppamento spaziale di colori percepiti come simili dall'uomo

Coordinate cartesiane (x, y, canale)
OpenCV utilizza BGR

## HSV e HSL
Piu' vicine al modo con cui gli esseri umani percepiscono i colori.
Basate su:
- Tinta (Hue)
- Saturazione
- Luminosita' (Value o Lightness)

Vantaggi:
- Possibilita' di specificare i colori in modo intuitivo
- Possono essere utilizzate piu' efficacemente per localizzazione e riconoscimento di oggetti nelle immagini.

Lo Hue e' un angolo: rosso primario a $0^\circ$, verde primario a $120^\circ$ e blu primario a $240^\circ$.
L'asse verticale al centro comprende i grigi, dal nero (luminosita' $0$) al bianco (luminosita' $1$).
I colori primari (RGB) e secondari (CMY) si trovano attrono al cilindro con saturazione $1$.
- In HSV questi colori saturi hanno luminosita' $1$.
- In HSL invece hanno luminosita' $0.5$

In OpenCV HSL e' HLS

# Istogramma di un'immagine grayscale
Indica il numero di pixel dell'immagine per cascun livello di grigio.
Dall'istogramma si possono estrarre informazioni interessanti:
- se la maggior parte dei valori sono "condensati" in una zona, l'immagine ha un scarso contrasto
- se nell'istogramma sono predominanti le basse intensita', l'immagine e' molto scura

Funzione `calcHist(images, channels, mask, histSize, ranges)` in OpenCV
Analisi dell'istogramma:
- Se i diversi oggetti in un'immagine hanno livelli di grigio differenti, l'istogramma puo' fornire un primo semplice meccanismo di separazione degli oggetti dallo sfondo

---
# Operazioni sui pixel

Su una singola immagine: $I'[y,x]=f(I[y,x])$
- Ogni pixel dell'immagine di uscita e' funzione solo del corrispondente pixel dell'immagine di input
- Esempi:
	- Variazione della luminosita'
	- Variazione del contrasto
	- Conversione da livelli di grigio a (pseudo)colori
	- Binarizzazione con soglia globale

Su piu' immagini: $I[y,x]=f(I_1[y,x],I_2[y,x],\dots)$
- Ogni pixel dell'immagine di uscita e' funzione solo dei corrispondenti pixel delle immagini di input
- Esempio: operazioni aritmetiche fra immagini: somma, sottrazione, AND, OR, XOR

## Variazione luminosita' e contrasto

Tipica funzione: $f(v)=\alpha\times v + \beta$
- $\alpha$ controlla in contrasto, $\beta$ la luminosita'
- Valori di output forzati in $[0,255]$

Funzione non lineare: Gamma correction
$$f(v)=(\frac{v}{255})^\gamma\times255$$
- $\gamma < 1$ aumenta luminosita' toni scuri
- $\gamma > 1$ diminuisce luminosita' toni chiari

## Lookup Lable (LUT)

Se il numero di colori o livelli di grigio e' inferiore al numero di pixel nell'immagine, e' piu' efficiente memorizzare il risultato della funzione di mapping $f$ per ogni input in un array, da utilizzare poi come tabella di lookup per eseguire l'operazione su tutti i pixel.
$$I'[y,x]=f(I[y,x])\rightarrow I'[y,x]=LUT[I[y,x]]$$

```python
# Un esempio di funzione f (Gamma correction per un certo valore di 𝛾)  
𝛾 = 0.5  
# Calcolo di un singolo valore di f  
f = lambda p: 255 * (p/255.0)**𝛾  
# Calcolo di f su tutti i valori di un array NumPy  
f_np = lambda a: f(a).astype(np.uint8)  
# Calcolo dell'array LUT  
lut = f_np(np.arange(256))  
# Una semplice implementazione Python  
def applica_py_f(img):  
	res = np.empty_like(img)  
	h, w = res.shape  
	for y in range(h):  
		for x in range(w):  
			res[y,x] = f(img[y,x])  
	return res

# Semplice implementazione Python con LUT  
def applica_py_lut(img):  
	res = np.empty_like(img)  
	h, w = res.shape  
	for y in range(h):  
		for x in range(w):  
		res[y,x] = lut[img[y,x]]  
	return res

# Implementazione NumPy senza LUT  
def applica_np_f(img):  
	return f_np(img)

# Implementazione NumPy con LUT  
def applica_np_lut(img):  
	return lut[img]

# Funzione LUT di OpenCV  
def applica_cv_lut(img):  
	return cv.LUT(img, lut)
```

### LUT da grayscale a RGB

Percezione umana non adatta a osservare piccole variazioni fra toni di grigio
- I nostri occhi sono piu' sensibili a variazioni fra colori
- In molte applicazioni si ricolorano immagini grayscale per renderle meglio fruibili
```python
# Funzione OpenCV apposita applyColorMap
imgs = [cv.imread('immagini/'+n, cv.IMREAD_GRAYSCALE)
	for n in ('tbbt.jpg', 'radio1.png', 'radio2.png')]  
maps = (cv.COLORMAP_AUTUMN, cv.COLORMAP_JET, cv.COLORMAP_HSV)  
colored_imgs = [cv.applyColorMap(i, m) for m in maps for i in imgs]
```

# Operazioni aritmetiche fra immagini

## Differenza

La sottrazione dello "sfondo" puo' consentire di individuare gli oggetti di interesse
$$I[y,x]=f(I_1[y,x],I_2[y,x])$$
```python
img, back = cv.imread('esempi/mario-game.png'), cv.imread('esempi/mario-back.png')  
diff = img - back # N.B. diff = cv.subtract(img, back) è più efficiente  
mask = diff!=0 # maschera attiva nei pixel di diff diversi da zero
res = np.zeros_like(img)  
res[mask] = img[mask]
```

## Operatori Bitwise

Analogamente alle maschere di bit, l'operatore AND consente di azzerare selettivamente alcuni pixel, l'operatore OR consente di impostare il valore, etc.
$$I[y,x]=f(I_1[y,x],I_2[y,x])$$
```python
sprite = cv.imread('esempi/sprite.png')  
mask = cv.imread('esempi/mask.png')  
back = cv.imread('esempi/hill.jpg')  
res = back.copy()  
x, y = 200, 100  
h, w = sprite.shape[:2]  
roi = res[y:y+h,x:x+w]  
roi &= mask  
roi |= sprite
```

## Alpha blending

Combinazione fra uno sfondo (RGB) e un'immagine (RGB) con abbinato un valore di "trasparenza" per cascun pixel (fra $0$ e $1$)
$$I[y,x]=f(I_1[y,x],I_2[y,x],I_3[y,x])$$
$$f(p_1,p_3,a)=p_1\times(1-a)+p_2\times a$$
```python
img1 = cv.imread('esempi/hill.jpg')  
img2 = cv.imread('esempi/study.png')  
img2_alpha = cv.imread('esempi/study-alpha.png')  
h, w = img2.shape[:2]  
a = img2_alpha / 255 # Trasforma il canale alpha da [0,255] a [0,1]  
x, y = 200, 100  
res = img1.copy()  
res[y:y+h,x:x+w] = img1[y:y+h,x:x+w]*(1.0-a) + img2*a
```

# Binarizzare un'immagine grayscale

## Soglia globale

$$f(v,t)=\begin{cases}0&v<t\\255&v\ge t\end{cases}$$
Come scegliere la soglia?
- Manualmente
- Osservando l'istogramma
- Metodo di Otsu: in automatico cerca sull'istogramma la soglia che minimizza la varianza intra-classe dell'intensita' dei pixel delle due classi (foreground e background) determinate dalla soglia stessa

## Soglia Locale

Quando oggetti e sfondo non sono uniformi, spesso non e' possibile determinare una soglia globale appropriata
Soglia locale (o additiva):
- Determinata per ogni pixel considerando una piccola regione dell'immagine attorno ad esso
- Il modo piu' semplice consiste nel determinare la soglia come media dei pixel nella regione meno un valore constante

## In OpenCV

```python
# Soglia globale (128)  
img = cv.imread('esempi/bolts.png', cv.IMREAD_GRAYSCALE)  
_, res = cv.threshold(img, 128, 255, cv.THRESH_BINARY)

# Soglia globale determinata dall'algoritmo di Otsu  
img = cv.imread('immagini/torre.jpg', cv.IMREAD_GRAYSCALE)  
t, res = cv.threshold(img, -1, 255, cv.THRESH_OTSU)

# Soglia locale (media su intorno 11x11 meno il valore 10)  
img = cv.imread('immagini/sudoku.jpg', cv.IMREAD_GRAYSCALE)  
res = cv.adaptiveThreshold(img, 255, cv.ADAPTIVE_THRESH_MEAN_C, cv.THRESH_BINARY, 11, 10)
```

# Operazioni su istogramma

## Contrast stetching

Espansione dei livelli di grigioper aumentare il contrasto
- Si puo' ottenere con un semplice mapping lineare:
	$$f(I[y,x])=255\times\frac{I[y,x]-\alpha}{\beta-\alpha}$$
- I due parametri $\alpha$ e $\beta$ possono essere il minimo e massimo livello di grigio dell'immagine

Problema: un solo pixel pari a $0$ e uno pari a $255$ rende inutile l'operazione
- Idea: scegliere $\alpha$ e $\beta$ sull'istogramma (ad esempio in corrispondenza del $5^\circ$ e $95^\circ$ percentile)

```python
def contrast_stretching(img, a, b):  
	# Converte in floating point e applica la funzione di mapping  
	d = b-a if b!=a else 1  
	n = 255 * (img.astype(float)-a) / d  
	# Forza il range [0,255] e converte in byte  
	return np.clip(n, 0, 255).astype(np.uint8)  
img = cv.imread('immagini/rice.png', cv.IMREAD_GRAYSCALE)  
# Contrast stretching con 𝛼=𝑚𝑖𝑛(I) e 𝛽=𝑚𝑎𝑥(I)  
res = contrast_stretching(img, img.min(), img.max())  
img = cv.imread('esempi/kernel.png', cv.IMREAD_GRAYSCALE)  
# Contrast stretching con 𝛼=𝑃_5(I) e 𝛽=𝑃_95(I)  
res = contrast_stretching(img, np.percentile(img, 5), np.percentile(img, 95))
```

## Equalizzazione dell'istogramma

Metodo usato per:
- migliorare il contrasto
- rendere confrontabili immagini catturate in condizioni diverse di illuminazione

Obiettivo (ideale):
- Distribuire l'istogramma uniformemente sui livelli di grigio
- Zone dell'istogramma "scarsamente popolate" vengono "compresse"
- Zone "con molti pixel" vengono "allargate"

$$f(v)=\sum^v_{i=0}H[i]$$
Con $H$ istogramma dell'immagine normalizzato:
$$H[i]=\frac{255\times h[i]}{\sum h[i]}\implies\sum^{255}_{i=0}H[i]=255$$

## Equalizzazione immagini a colori

Non e' corretto applicare separatamente l'equalizzazione a ciascun canale RGB
Si puo' convertire in HSL per applicare l'equazione al solo canale L

```python
#Equalizzazione di un'immagine grayscale  
img = cv.imread('esempi/kernel.png', cv.IMREAD_GRAYSCALE)  
res = cv.equalizeHist(img)  
# Equalizzazione di un'immagine a colori  
# Esempio di come NON fare: equalizzazione di ciascun canale RGB  
bgr = cv.imread('immagini/tbbt.jpg')  
b, g, r = cv.split(bgr)  
b_eq, g_eq, r_eq = [cv.equalizeHist(x) for x in (b, g, r)]  
res_wrong = cv.merge((b_eq, g_eq, r_eq))  
# Esempio di come procedere convertendo in HSL ed equalizzando L  
hls = cv.cvtColor(bgr, cv.COLOR_BGR2HLS)  
h, l, s = cv.split(hls)  
l_eq = cv.equalizeHist(l)  
hls_eq = cv.merge((h, l_eq, s))  
res_ok = cv.cvtColor(hls_eq, cv.COLOR_HLS2BGR)
```

---
# Numeri reali come coordinate delle immagini

Un'immagine puo' essere vista come una funzione:
- $I:\mathbb{N}\times \mathbb{N} \times \mathbb{N}\rightarrow \mathbb{N}$
Possiamo in generale considerare come numeri reali sia le coordinate $(x,y)$ che i valori dei pixel:
- $I:\mathbb{R \times R \times N} \rightarrow \mathbb{R}$
Attenzione a:
- Origine in alto a sinistra
- Verso dell'asse $y$
- Coordinate negative

# Trasformazioni geometriche

Si tratta di trasformazioni che si applicano alle coordinate e non ai valori dei pixel
- La griglia dei pixel viene deformata e mappata sull'immagine di destinaizone
Funzione di mapping: $g: \mathbb{R\times R} \rightarrow \mathbb{R\times R}$

## Mapping inverso

Mapping inverso: dalla destinazione alla sorgente: $f:\mathbb{R\times R}\rightarrow \mathbb{R\times R}, f=g^{-1}$
- Per ogni pixel $(x,y)$ dell'immagine destinazione $I'$, si calcolano le coordinate corrispondenti nell'immagine di partenza $I$ e si copia il colore del pixel corrispondente

Problemi da risolvere:
- Quale valore per i pixel non esistenti (fuori dall'immagine sorgente)?
- Quale valore del pixel qunado $f(x,y)$ non restituisce coordinate intere?

### Gestire le coordinate fuori dall'immagine

Gli approcci piu' comuni per estrapolare il valore (con denominazione OpenCV):
- Colore di background costante (cv.BORDER_CONSTANT)
- Copia del pixel piu' vicino (cv.BORDER_REPLICATE)
- Riflessione dei pixel dell'immagine (cv.BORDER_REFLECT)  
- Replica dell'immagine (cv.BORDER_WRAP)  
- Nessuna modifica: lascia il valore già presente nell'immagine destinazione (cv.BORDER_TRANSPARENT)

### Stimare un valore per il pixel da coordinate non intere

In quasi tutte le trasformazioni geometriche, la maggior parte delle coordinate $f(x,y)$ dei valori da recuperare nell'immagine sorgente sono **numeri con la virgola**.
Si procede **stimando** il valore a partire dai pixel (con coordinate intere) piu' vicini.
Il metodo piu' semplice e' scegliere il pixel piu' vicino (nearest-neighbor)
Migliori risultati si ottengono con tecniche di **interpolazione** che adattano una funzione polinomiale ai pixel in un intorno ed eseguono la stima in base al valore di tale funzione alle coordinate $f(x,y)$.

#### Interpolazione

- Nearest-neighbor
- Bilineare
- Bicubica

## Ingrandimento di un'immagine

Il ridimensionamento e' li caso piu' semplice e probabilimente piu' utilizzato di trasformazione geometrica
- Caso particolare di trasformazione affine

OpenCV dispone di una funzione specifica: `resize()`
Interpolazione lineare:
- Buon compromesso fra efficenza e qualita' del risultato

Interpolazione bicubica:
- Di solito produce risultati migliori ma e' molto piu' lenta

```python
img = cv.imread('esempi/study.png')  
h, w = img.shape[:2]  
scale = 7.5  
size = (int(w*scale),int(h*scale))  
img_nn = cv.resize(img, size, interpolation=cv.INTER_NEAREST)  
img_bl = cv.resize(img, size, interpolation=cv.INTER_LINEAR)  
img_bc = cv.resize(img, size, interpolation=cv.INTER_CUBIC)
```

## Rimpicciolire un'immagine

In caso di grande riduzione delle dimensioni, l'interpolazione bilineare e bicubica possono considerare un umero di pixel troppo limitato rispetto alle informazioni presenti nell'immagine di partenza.
In questi casi in OpenCV si puo' utilizzare `INTER_AREA` che considera i pixel coinvolti.

# Trafsormazioni affini

La funzione
$$g(x,y)=
\begin{bmatrix}
s_xcos\theta & -s_ysin\theta\\
s_xsin\theta & s_ycos\theta
\end{bmatrix}
\begin{bmatrix}
x\\ y
\end{bmatrix}
+
\begin{bmatrix}
t_x\\ t_y
\end{bmatrix}
$$
- ruota di un angolo $\theta$ rispetto all'origine
- applica un fattore di scala $s_x$ lungo l'asse $x$ e $s_y$ lungo l'asse $y$
- esegue una traslazione $t_x$ pixel lungo l'asse $x$ e $t_y$ lungo l'ase $y$

Piu' in generale, una trasformazione affine 2D puo' essere rappresentata come moltiplicazione per una matrice $A = \begin{bmatrix}a_{00}&a_{01}\\a_{10}&a_{11}\end{bmatrix}$ e somma (traslazione) di un vettore $t = \begin{bmatrix}t_x\\ t_y\end{bmatrix}$:
$$
\begin{bmatrix}
x'\\ y'
\end{bmatrix}
=
\begin{bmatrix}
a_{00}&a_{01}\\a_{10}&a_{11}
\end{bmatrix}
\begin{bmatrix}
x\\ y
\end{bmatrix}
+
\begin{bmatrix}
t_x\\ t_y
\end{bmatrix}
$$

## Matrice aumentata e coordinate omogenee

 Si puo' scrivere $\begin{bmatrix}x'\\y'\end{bmatrix}=\begin{bmatrix}a_{00}&a_{01}\\a_{10}&a_{11}\end{bmatrix}\begin{bmatrix}x\\ y\end{bmatrix}+\begin{bmatrix}t_x\\ t_y\end{bmatrix}$ come $\begin{bmatrix}x'\\y'\\1\end{bmatrix}=\begin{bmatrix}a_{00}&a_{01}&t_x\\a_{10}&a_{11}&t_y\\0&0&1\end{bmatrix}\begin{bmatrix}x\\y\\1\end{bmatrix}$
 aggiungendo una terza coordinata (sempre pari a $1$) a tutti i vettori e aumentando la matrice con una nuova riga di zeri e una nuova colonna contenente il vettore traslazione.
 La traslazione nello spazio 2D puo' essere espressa come moltiplicazione in uno spazio 3D in cui la terza dimensione e' pari a $1$: le coordinate $(x,y,1)$ sono dette coordinate omogenee.
 Si possono combinare piu' trasformazioni affini semplicemente moltiplicando fra loro le corrispondenti matrici.

La funzione `warpAffine()` applica una trasformazione affine a un'immagine
Richiede di specificare le prime due righe della matrice di trafsormazione $\begin{bmatrix}a_{00}&a_{01}&t_x\\a_{10}&a_{11}&t_y\\0&0&1\end{bmatrix}$
- L'argomento $M$ della funzione e' qundi costituito da una matrice $2\times3$: $\begin{bmatrix}a_{00}&a_{01}&t_x\\a_{10}&a_{11}&t_y\end{bmatrix}$
`warpAffine()` provvede ad invertire la trasformazione, ottenendo $f=g^{-1}$, che applica alle coordinate dell'immagine destinazione con il tipo di interpolazione e la gestione dei bordi specificati.

```python
import math  
img = cv.imread('esempi/mario-c.png'); b = img.shape[0]//2;  
bc = img[0,0].tolist()  
img = cv.copyMakeBorder(img, b, b, b, b, cv.BORDER_CONSTANT, value = bc);  
h, w, _ = img.shape  
trasf = lambda M: cv.warpAffine(img,M,(w,h),None,cv.INTER_CUBIC,cv.BORDER_CONSTANT,bc)  
# Traslazione di 9 pixel verso destra e 9 verso l'alto  
M_T = np.array([[1., 0., 9], [0., 1., -9]])  
img_T = trasf(M_T)  
# Riduzione di scala x e aumento y  
M_S = np.array([ [0.7, 0.0, 0], [0.0, 1.3, 0]])  
img_S = trasf(M_S)  
# Rotazione di - π/8  
θ = -math.pi/8; cos_θ, sin_θ = math.cos(θ), math.sin(θ)  
M_R = np.array([[cos_θ,-sin_θ, 0], [sin_θ, cos_θ, 0]])  
img_R = trasf(M_R)
```

Le trasformazioni avvengono rispetto all'origine dell'immagine (in alto a sinistra).
Verso della rotazione: in un normale piano cartesiano un angolo positivo ruota in senso anti-orario, ma nell'immagine l'asse $y$ e' diretto verso il basso, quindi:
- angolo positivo: verso orario
- angolo negativo: verso anti-orario

### Rotazione rispetto a un punto diverso dall'origine

Traslare l'origine nel punto desiderato, eseguire la rotazione e infine la traslazione inversa.
E' disponibile la funzione OpenCV `getRotationMatrix2D()` che restituisce direttamente la matrice a partire dalle coordinate del punto, dall'angolo (verso antiorario) e da un eventuale fattore scala.

```python
# Rotazione rispetto al centro dell'immagine cx,cy  
cx, cy = w//2, h//2  
# Trasla il centro dell'immagine nell'origine  
M_T1 = np.array([ [1., 0., -cx], [0., 1., -cy], [0., 0., 1]])
img_T1 = trasf(M_T1[:2])  
# Rotazione di π/4  
θ = math.pi/4  
cos_θ, sin_θ = math.cos(θ), math.sin(θ)  
M_R = np.array([ [cos_θ,-sin_θ, 0], [sin_θ, cos_θ, 0], [0., 0., 1]])
img_R = trasf(M_R[:2])  
# Traslazione inversa  
M_T2 = np.array([ [1., 0., cx], [0., 1., cy], [0., 0., 1]])
img_T2 = trasf(M_T2[:2])  
# Compone le tre trasformazioni in una singola  
M_Rc = M_T2 @ M_R @ M_T1  
img_Rc = trasf(M_Rc[:2])
```

## Trasformazione affine da coppie di punti corrispondenti

Funzione OpenCV `getAffineTransform()`: date tre coppie di punti corrispondenti, calcola la matrice di trasformazione affine che mappa ciascun punto nel suo corrispondente.

```python
back = cv.imread('esempi/cake.png')  
m = cv.imread('esempi/mario.png')  
h, w = m.shape[:2]  
m_pts = np.float32([[1,1],[w-2,1],[1,h-2]])  
c_pts = np.float32([[[68,35],[106,35],[68,66]],[[4,116],[ 38,140],[4,163]]])
# ... 3 punti per ogni quadrato nell'immagine
def drawPoints(img, lp):  
	for pts in lp:  
		for i in range(3):  
			p = tuple(pts[i].round().astype(int))  
			cv.drawMarker(img, p, (255,0,0), i, 3)  
	return img  
back_points = drawPoints(back.copy(), c_pts)  
m_points = drawPoints(m.copy(), [m_pts])  
for pts in c_pts:  
	M = cv.getAffineTransform(m_pts,pts)  
	cv.warpAffine(m, M, back.shape[1::-1], back,cv.INTER_LINEAR, 
		cv.BORDER_TRANSPARENT)
```

## Trasformazione proiettiva

A differenza delle trasformazioni affini, non preserva il parallelismo fra rette
Definita da una matrice $3\times3$:
$$\begin{bmatrix}x'\\y'\end{bmatrix}=\begin{bmatrix}\frac{x_c}{w}\\\frac{y_c}{w}\end{bmatrix},\;\text{con }w=\begin{cases}z_c&\text{se }z_c\neq0\\\infty&\text{altrimenti}\end{cases}\;\text{e}\;\begin{bmatrix}x_c\\y_c\\z_c\end{bmatrix}=M_p\begin{bmatrix}x\\y\\1\end{bmatrix},\;M_p\in\mathbb{R}^{3\times3}$$

In OpenCV sono disponibili le funzioni:
- `getPerspectiveTransform()` che calcola la matrice $M_p$ a partire da quattro coppie di punti corrispondenti
- `warpPerspective()` che applica la trafsormazione definita da una matrice $M_p$ a un'immagine, dopo aver invertito la trasformazione ottenendo $f=g^{-1}$, con il tipo di interpolazione e la gestione dei bordi specificati

```python
img = cv.imread('esempi/building.jpg')  
pts1 = np.float32([[195,16], [466,183], [485,269], [181,293]])  
w, h = 400, 120 # Dimensioni dell'immagine destinazione  
pts2 = np.float32([[0,0], [w-1,0], [w-1, h-1], [0, h-1]])  
Mp = cv.getPerspectiveTransform(pts1, pts2)  
res = cv.warpPerspective(img, Mp, (w,h), flags = cv.INTER_CUBIC)  
m = cv.imread('esempi/sprite.png')  
h, w = m.shape[:2] # Dimensioni di m  
pts_m = np.float32([[0,0], [w-1,0], [w-1, h-1], [0, h-1]])  
pts_b = np.float32([[243,111], [269,122], [273,220], [244,217]])  
Mp = cv.getPerspectiveTransform(pts_m, pts_b)  
res2 = cv.warpPerspective(m, Mp, img.shape[1::-1], img.copy(),  
	cv.INTER_LINEAR, cv.BORDER_TRANSPARENT)
```