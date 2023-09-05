# Operazioni locali

Un operatore locale utilizza un insieme di pixel nelle vicinanze (intorno) del pixel dell'immagine di partenza per determinare il valore del pixel nell'immagine risultato

## Filtri lineari

Sono le operazioni lineari piu' comuni: il valore di ciascun pixel e' calcolato come somma pesata dei valori dei pixel nell'intorno considerato: $I'[y,x]=\sum_{i,j}[i,j]\times F[i,j]$

### Il filtro

$F$ e' chiamato filtro, maschera o kernel: di norma e' quadrato, con dimensioni $m\times m$, con $m$ dispari.
- Gli elementi di $F$ sono chiamati coefficienti del filtro o pesi

Di solito si fa riferimento ai coefficienti del filtro considerando due assi cartesiani (con l'asse verticale diretto verso il basso) con origine in cossipondenza del centro di $F$.
- La formula per il calcolo del valore di un pixel nell'immagine destinazione puo' essere quindi scritta come:
$$I'[y,x]=\sum^d_{i=-d}\sum^d_{j=-d}F[i,j]\times I[y+i,x+j]$$
con $d=\lfloor\frac{m}{2}\rfloor$

### Gestione dei bordi

Come comportarsi vicino ai bordi, dove il filtro non puo' essere completamente sovrapposto all'immagine?
Analogamente a [quanto visto per le trasformazioni geometriche](Immagini_Digitali#Gestire%20le%20coordinate%20fuori%20dall'immagine), ci sono varie possibilita':
- Usare un valore di "background" costante
- Copiare il valore del pixel piu' vicino
- Riflessione dell'immagine

### Tipi di dati e normalizzazione del filtro

Tipi di valori dei pixel dell'immagine di partenza:
- Il caso piu' comune e' un byte per pixel $[0,255]$
- Altri tipi comuni sono interi con o senza segno (a $16$ o $32$ bit) e numeri floating point (a $32$ o $64$ bit): di solito avviene quando l'immagine e' a sua volta il risultato di precedenti applicazioni di filtri

Tipi dei valori dei pixel nell'immagine risultato:
- Se ci sono coefficienti negativi, i pixel del risultato dovranno essere di tipo intero ocn segno o floating point

Normalizzazone del filtro:
- Se i coefficenti del filtro sono tutti $\ge0$ e si vuole che i pixel dell'immagine risultato abbiano lo stesso range dei valori dell'immagine di partenza si normalizza il filtro dividendo per la somma dei coefficienti.

### Visualizzare immagini di tipo int o float

Di solito i valori dei pixel sono normalizzati in $[0,255]$ prima della visualizzazione.
- Immagini senza valori negativi: $I'[y,x]=\frac{255}{max(I)}\times I[y,x]$
	Il valore $0$, anche se non presente nell'immaigne, corrisponde al nero, il valore massimo presente nell'immagine corrisponde al bianco.
- Immagini che contengono valori negativi: $I'[y,x]=128+\frac{127}{max(abs(I))}\times I[y,x]$
	Valore $0 \rightarrow$ livello di grigio 128, valori negativi $\rightarrow$ livelli di grigio piu' scuro (fino al nero), valori positivi $\rightarrow$ livelli di grigio piu' chiaro (fino al bianco).

### In OpenCV

```python
# Applicazione di un filtro lineare a un'immagine con OpenCV  
# Il risultato è memorizzato su un'immagine di interi con segno a 16 bit  
def filter2D_cv(img, f):  
	return cv.filter2D(img, cv.CV_16S, f)

img = cv.cvtColor(cv.imread('esempi/bolts.png'), cv.COLOR_BGR2GRAY)  
f = np.ones((9,9))  
res = filter2D_cv(img, f)
```

## Filtri separabili

$$I'[y,x] = \sum^d_{i=-d}\sum^d_{j=-d}F[i,j]\times I[y+i,x+j]$$
con $d=\lfloor\frac{m}{2}\rfloor$, complessita' computazionale: $m^2$ moltiplicazioni e somme per ciascun pixel del risultato
Se la matrice $F$ puo' essere ottenuta come *prodotto di un vettore colonna per un vettore riga* ($F=F_y\times F^T_x$), il filtro e' separabile e si ha:
$$I'[y,x]=\sum_i\sum_jF_y[i]\times F_x[j]\times I[y+i,x+j]=$$
$$=\sum_iF_y[i]\times\sum_jF_x[j]\times I[y+i,x+j]$$ $$I'[y,x]=\sum^d_{i=-d}F_y[i]\times T[y+i,x]$$
con $T[y,x] = \sum^d_{j=-d}F_x[j]\times I[y,x+j]$
Si puo' quindi applicare il filtro $F_x$ (dimensioni $1\times m$) a tutta l'immagine, ottenendo un'immagine intermedia $T$, quindi applicare $F_y$ (dimensioni $m\times1$) a $T$ per ottenere il risultato $I'$.
- Complessita' computazionale: $2m$ moltiplicazioni e somme per ciascun pixel del risultato

#### In OpenCV

```python
# Applicazione di un filtro lineare separabile a un'immagine con OpenCV  
# Il risultato è memorizzato su un'immagine di interi con segno a 16 bit  
def sepFilter2D_cv(img, fx, fy):  
	return cv.sepFilter2D(img, cv.CV_16S, fx, fy)  

# con un filtro 9x9 con tutti i coefficienti pari a uno.  
img = cv.cvtColor(cv.imread('esempi/bolts.png'), cv.COLOR_BGR2GRAY)  
f = np.ones((9,9))  
fx, fy = np.ones((9,1)), np.ones((1,9))  
print("Verifica: ", np.array_equal(fx*fy, f))
 
res = sepFilter2D_cv(img, fx, fy)
```

## Correlazione e Convoluzione

L'applicazione di un filtro mediante la formula:
$$I'[y,x]=\sum^d_{i=-d}\sum^d_{j=-d}F[i,j]\times I[y+i,x+j]$$
e' chiamata ==correlazione== di $I$ con $F$ e puo' essere indicata con la notazione:
$$I'=I\otimes F$$
Cambiando il segno degli offset nella formula si ottiene:
$$I'[y,x]=\sum^d_{i=-d}\sum^d_{j=-d}F[i,j]\times I[y-i,x-j]$$
che e' chiamata ==convoluzione== di $I$ con $F$ e puo' essere indicata con la notazione:
$$I'=I*F$$
La convoluzione si puo' ottenere semplicemente ribaltando il filtro sui due assi:
$$F'[i,j]=F[-i,-j]$$

### Correlazione vs Convoluzione

Se il filtro e' simmetrico rispetto all'origine, $I*F=I\otimes F$
Proprieta' algebriche **comuni a entrambe**:
- Proprieta' distributiva rispetto all'addizione:
	$$\begin{array}I\otimes(F+G)=(I\otimes F)+(I\otimes G)\\
	I*(F+G)=(I*F)+(I*G){}\end{array}$$
- Proprieta' associativa rispetto a un fattore scalare:
	$$\begin{array}{}(\alpha\times I)\otimes F=\alpha\times(I\otimes F)\\(\alpha\times I)*F=\alpha\times(I*F)\end{array}$$

==Solo la convoluzione== e' commutativa e associativa:
$$\begin{array}{}
I*F=F*I\\
(I*F)*G=I*(F*G)
\end{array}$$
In OpenCV: la funzione `cv.filter2D()` implementa la correlazione; per ottenere la convoluzione e' sufficiente prima ribaltare il filtro con `cv.flip(filtro, -1)`

## Box filter

- Tutti i coefficenti a uno
- Di solito normalizzato dividendo per il numero di elementi $\rightarrow$ media mobile

Effetto "sfocatura" (blur): attenua il contrasto locale, riduce il rumore.
In OpenCV e' disponibile la funzione `cv.boxFilter()`: un'implementazione specificaper questo filtro che e' piu' efficiente di `cv.filter2D` e `cv.sepFilter2D`

Puo' produrre artefatti nell'immagine, che possono essere visibile all'aumentare della dimensione del filtro. 

## Filtro Gaussiano

Per risolvere i problemi del box filter servirebbe un filtro con pesi che diminuiscono spostandosi dal centro verso l'esterno.
La funzione gaussiana in due dimensioni e' l'ideale (e produce filtri separabili).

### Creare un filtro Gaussiano

La funzione gaussiana 2D $G_{2D}(x,y)=\frac{1}{2\pi𝜎^2}e^{-\frac{x^2+y^2}{2𝜎^2}}$ puo' essere riscritta come:
$$G_{2D}(x,y)=\left(\frac{1}{𝜎\sqrt{2\pi}}\right)^2e^{-\frac{x^2}{2𝜎^2}}e^{-\frac{y^2}{2𝜎^2}}= G_{1D}\times G_{1D}$$
Un filtro ottenuto da $G_{2D}$ e' il prodotto di due filtri 1D e quindi separabile
Per creare un filtro gaussiano $m\times m$, e' necessario scegliere la dimensione $m$ e il valore di 𝜎
```python
img = cv.imread('esempi/study.png')  
# Crea un filtro gaussiano 1D e lo passa a sepFilter2D  
g = cv.getGaussianKernel(5, 1) # m=5, σ=1  
img_blurred = cv.sepFilter2D(img, -1, g, g)  
# Ancora più semplice utilizzare direttamente l'apposita funzione GaussianBlur  
img_blurred2 = cv.GaussianBlur(img, (5,5), 1) # m=5, σ=1  
# N.B. se σ<=0, la funzione calcola σ da m come σ = 0.3*((m-1)*0.5 - 1) + 0.8  
list_img_blurred = [cv.GaussianBlur(img, (m,m), 0) for m in range(3,17,2)]
```

## Sharpening

I filtri precedenti, oltre all'effetto "blur", posono essere impiegati per ottenere un risultato opposto: una sorta di "miglioramento della messa a fuoco", chiamato sharpening.
Si procede calcolando un'immagine "blurred" mediante ==convoluzione con un flitro $F_b$ normalizzato== (es. box filter o gaussiano) e sottraendola all'originale: $M=I-I*F_b$
L'immagine $M$ (pixel con valori interi) e' infine sommata all'immagine originale, dopo essere stata moltiplicata per un parametro $k$ che controlla l'intensita' dell'effetto: $I'=I+k\times M$

Grazie alle proprieta' algebriche della convoluzione, possiamo costruire un singolo filtro che esegua l'intera operazione di sharpening.
$M=I-I*F_b=I*F_{id}+I*(-F_b)$, dove $F_{id}$ e' un filtro con solo $1$ al centro
Applicando (al contrario) la proprieta' distributiva $I*(F+G)=(I*F)+(I*G)$, si ha:
$M=I*F_{id}+I*(-F_b)=I*(F_{id}-F_b)\implies M=I*F_L$, con $F_L=F_{id}-F_b$
Il risultato dello sharpening $I'=I+k\times M$ puo' essere riscritto come:
$$I'=I+k\times(I*F_L)$$
Applicando la proprieta' associativa rispetto a uno scalare e la commutativa, si ottiene:
$$I'=I+k\times(i*F_L)=I+I*(k\times F_L)$$
Infine applicando di nuovo la propieta' distributiva:
$$I'=I+I*(k\times F_L)=I*F_{id}+I*(k\times F_L)=I*(F_{id}+(k\times F_L))$$
$I'=I*F_S$, con $F_S=F_{id}+(k\times F_L) = F_{id} +k\times(F_{id}-F_b)$

### In OpenCV
```python
# Metodo 1: filtro di blur e operazioni aritmetiche fra immagini  
def sharpen(img, gaussian_blur, m, k):  
	blurred = cv.GaussianBlur(img, (m,m), 0) if gaussian_blur else cv.blur(img, (m,m))  
	mask = img.astype(np.int16) - blurred  
	return np.clip(np.round(img.astype(float)+k*mask), 0, 255).astype(np.uint8)  
# Metodo 2: creazione di un unico filtro che fa tutto  
def create_sharpen_filter(gaussian_blur, m, k):  
	if gaussian_blur:  
		g = cv.getGaussianKernel(m, 0)
		F_b = g.reshape(1,-1)*g
	else:  
		F_b = np.ones((m,m), np.float32)  
		F_id = np.zeros_like(F_b); F_id[m//2, m//2] = 1  
		F_b /= F_b.sum() # Normalizza il filtro di blur  
	return F_id + k*(F_id - F_b)  
img = cv.imread('esempi/kernel.png', cv.IMREAD_GRAYSCALE)  
f = create_sharpen_filter(False, 5, 2)  
res1 = sharpen(img, False, 5, 2)
res2 = cv.filter2D(img, -1, f)
```

# Bordi

Bordo di un oggetto
- La separazione tra l'oggetto e lo sfondo o tra l'oggetto e altri oggetti
- In genere molto piu' stabile rispetto alle informazioni di colore e tessitura, rispetto a illuminazione, rumore, ...
- E' utile per poterne interpretare forma geometrica

Estrazione dei bordi (edge detection)
- Un operazione molto importante, spesso il primo passo per l'individuazione dell'oggetto

## Cosa sono i bordi?

Se consideriamo un'immagine grayscale $I$ come una funzione
$$\begin{array}{}f:\mathbb{R\times R\rightarrow R}&\text{con}\;f(x,y)=I[y,x]\end{array}$$
I bordi si trovano in corrispondenza di cambiamenti rapidi di $f$.

### Differenza fra immagine e risultato di un Gaussian blur

Sottraendo il risultato dello smooth di un'immagine dall'immagine originale, si possono evidenziare le zone caratterizzate da cambiamenti rapidi fra valori chiari e scuri dei pixel.

### Difference of Gaussians (DoG)

Il filtro gaussiano riduce le alte frequenze nell'immagine:
- $I*G_𝜎$ contiene sostanzialmente le basse frequenze dell'immagine
- $I-(I*G_𝜎)$ contiene le alte frequenze

Dati due filti $G_{𝜎1}$ e $G_{𝜎2}$, la differenza $(I*G_{𝜎1})-(I*G_{𝜎2})$ evidenzia uno specifico range di frequenze nell'immagine
Con un'opportuna normalizzazione si possono evidenziare i bordi di un determinato spessore.

#### Costruzione di un filto DoG

Applicando al contrario la proprieta' distributiva si ha:
$$(I*G_{1𝜎})-(I*G_{𝜎2})=I*(G_{1𝜎}-G_{𝜎2})=I*DoG_{1𝜎,𝜎2}\;\text{,con}\;DoG_{1𝜎,𝜎2}=G_{1𝜎}-G_{𝜎2}$$
L'operazione si puo' quindi ottenere applicando un unico filtro, ottenuto dalla differenza di due filtri gaussiani

## Derivata di una funzione discreta

Limite del rapporto incrementale: $f'=\lim_{t\to0}\frac{f(x+t)-f(x)}{t}$
Immagini digitali: i valori sono discreti, non e' possibile calcolare esattamente il limite, ma si puo' stimare per un valore piccolo di $t$.
Stima mediante differenza finita "centrata" (piu' accurata di altre): $f'(x)\approx\frac{f(x+1)-f(x-1)}{2}$

### Derivate parziali

L'immagine e' una funzione discreta di due variabili
Stima derivate parziali di $f(x,y)$:
$$\frac{\partial f(x,y)}{\partial x}\approx\frac{f(x+1,y)-f(x-1,y)}{2}$$
$$\frac{\partial f(x,y)}{\partial y}\approx\frac{f(x,y+1)-f(x,y-1)}{2}$$

### Derivate parziali di un immagine

Costruiamo un filtro che calcoli $\frac{\partial f(x,y)}{\partial x}$
- $I \otimes \frac{1}{2}\begin{bmatrix}-1&0&1\end{bmatrix}$

Costruiamo un filtro che calcoli $\frac{\partial f(x,y)}{\partial y}$:
- $I\otimes \frac{1}{2}\begin{bmatrix}-1\\0\\1\end{bmatrix}$

#### Problema: rumore nelle immagini

La stima delle derivate parziali puo' essere influenzata da rumore nell'immagine
Applichiamo un filtro di smooth per ridurre il rumore

### Riduzione rumore e calcolo derivate parziali

Prima di applicare il filtro derivativo $D$, si applica un filtro di smooth $S$:
$$I'=(I*S)*D$$
Applicando il filtro con la convoluzione e sfruttando la proprieta' associativa: la convoluzione del filtro $D$ con il filtro $S$ permette di ottenere un singolo filtro che esegue entrambe le operazioni:
$$\begin{array}{}I'=I*F&\text{con }F=S*D\end{array}$$
Nei filtri piu' utilizzati in questo contesto, lo smooth avviene nella direzione ortogonale a quella di derivazione.

I principali filtri derivativi $3\times 3$:
- Prewitt
- Sobel: `cv.Sobel()`
- Scharr: `cv.Scharr()`

`cv.spatialGradient()` restituisce entrambe le derivate parziali ($x$ e $y$) calcolate con il filtro di Sobel non normalizzato.

## Il gradiente

Il gradiente di un immagine in un punto $(x,y)$ e' il vettore che ha per componenti le due derivate parziali:
$$\nabla I[y,x]=\begin{bmatrix}\frac{\partial I}{\partial x}\begin{bmatrix}y,x\end{bmatrix},&\frac{\partial I}{\partial y}\begin{bmatrix}y,x\end{bmatrix}\end{bmatrix}^T$$
Indica la direzione di maggior variazione dell'immagine nel punto $(x,y)$.

### Modulo e orentazione del gradiente

Sia $\nabla = \begin{bmatrix}\nabla_x,\nabla_y\end{bmatrix}^T$ il gradiente nel punto $(x,y): \nabla=\begin{bmatrix}\frac{\partial I}{\partial x}\begin{bmatrix}y,x\end{bmatrix},&\frac{\partial I}{\partial y}\begin{bmatrix}y,x\end{bmatrix}\end{bmatrix}^T$
- Angolo del gradiente: $\theta = atan2(\nabla_x,\nabla_y)$. ==Attenzione misurato in senso orario== (asse $y$ verso il basso).
- Modulo del gradiente: $\lvert\nabla\rvert=\sqrt{\nabla^2_x+\nabla^2_y}$

#### In OpenCV

```python
img = cv.imread('esempi/cat.jpg', flags=cv.IMREAD_GRAYSCALE)
img = cv.GaussianBlur(img, (3,3), 0)
# Calcolo derivate parziali per ogni pixel  
dx = cv.Sobel(img, cv.CV_32F, 1, 0, scale=1/8)
dy = cv.Sobel(img, cv.CV_32F, 0, 1, scale=1/8)
# Calcolo modulo e orientazione del gradiente per ogni pixel  
mod = cv.magnitude(dx, dy) # modulo
ang = cv.phase(dx, dy, angleInDegrees=True) # orientazione
# Disegna e stampa i dati del gradiente in un punto  
x, y = 127, 116  
w, h = 35, 25  
scale, mult = 11, 1/4  
roi = img[y-h//2:y+h//2, x-w//2:x+w//2]  
# roi_large è l'immagine ingrandita dei pixel in roi  
px, py = w//2, h//2  
p1 = (px*scale+scale//2, py*scale+scale//2)  
p2 = (int(round((px+dx[y,x]*mult)*scale)), int(round((py+dy[y,x]*mult)*scale)))  
cv.arrowedLine(roi_large, p1, p2, (240,176,0), 3, cv.LINE_AA)  
print(f'Gradiente in ({x},{y}): ∇x={dx[y,x]:.1f}, ∇y={dy[y,x]:.1f}, ' +
	  f'𝜃={ang[y,x]:.0f}°, |∇|={mod[y,x]:.2f}')
```

### Orientazione del gradiente e bordi

Analizzando l'orientazione del gradiente, si puo' osservare che il bordo di un immagine e' ortogonale al gradiente.

#### Visualizzare modulo e angolo del gradiente in OpenCV

```python
img = cv.imread('esempi/cat.jpg', flags=cv.IMREAD_GRAYSCALE)
dx, dy = cv.spatialGradient(img)
dx, dy = dx.astype(np.float32), dy.astype(np.float32)
# Calcola il modulo del gradiente per ciascun pixel
m = cv.magnitude(dx, dy)
# Calcola l'angolo del gradiente per ciascun pixel (in radianti)
a = cv.phase(dx, dy)
# Converte i moduli nel range [0,255]
m = cv.normalize(m, None, 0, 255, cv.NORM_MINMAX, cv.CV_8U)
# Converte gli angoli da [0,2pi] a [0,255]
a = (a/(2*np.pi)*255).round().astype(np.uint8)
a = cv.applyColorMap(a, cv.COLORMAP_HSV) # Angolo -> Hue
# Visualizza solo i gradienti con modulo superiore a una soglia
t = 40
a1 = a.copy()
a1[m<t] = 0
m1 = m.copy()
m1[m<t] = 0
```

### Ottenere i bordi con il modulo del gradiente

Il modulo del gradiente indica quanto e' "importante"il corrispondente bordo:
- Si potrebbe pensare di applicare una soglia al modulo del gradiente per selezionare i bordi, scartando cosi' i "falsi positivi" dovuti a rumore o altro.

Nella pratica questo approccio e' difficilmente applicabile, in quanto produce:
- frammenti di bordi non connessi
- bordi spuri (dovuti al rumore)
- bordi piu' spessi di un pixel

## Canny edge detector

Algoritmo proposto da John F. Canny nel 1986. Consiste in quattro step:
1. Smooth gaussiano dell'immagine
2. Calcolo del gradiente per ogni pixel
3. Soppressione dei non-massimi in direzione ortogonale al bordo
4. Selezione dei bordi significativi mediante isteresi

### Soppressione dei non massimi

Obiettivo: eliminare dall'immagine del modulo del gradiente tutti i pixel in cui il modulo del gradiente non e' massimo locale rispetto all'orientazione del gradiente.
Come verificare la condizione di massimo locale nell'intorno $3\times 3$:
- Si stima il modulo del gradiente nei punti $p_1$ e $p_2$ mediante interpolazione lineare
- Il pixel $p$ viene conservato solo se $\lVert\nabla[p]\rVert\le\lVert\nabla[p_1]\rVert\;\land\;\lVert\nabla[p]\rVert\ge\lVert\nabla[p_2]\rVert$

### Selezione finale dei bordi mediante isteresi

Al fine di selezionare gli edge significativi, cercando allo stesso tempo di ottenere bordi continui, l'immagine ottenuta al passo precedente viene binarizzata utilizzando due soglie $T_1$ e $T_2$, con $T_1>T_2$:
- sono inizialmente considerati validi solo i pixel in cui il modulo del gradiente e' superiore a $T_1$
- i pixel il cui modulo e' inferiore a $T_1$ ma superiore a $T_2$ sono considerati validi solo se adiacenti a pixel validi

Una corretta scelta di $T_1$ e $T_2$, cosi' come un'adeguata scelta dei parametri dello smooth gaussiano nella prima fase, sono molto importanti per ottenere gli effetti desiderati.
La scelta dipende solitamente dall'applicazione e dalle caratteristiche dell'immagine; sono tipicamente necessari vari esperimenti per giungere ai valori ottimali dei parametri.

### Canny edge detector in OpenCV

```python
# Caricamento immagine
img = cv.imread('filtri/thunderbirds.jpg')
# Smooth gaussiano con filtro sxs e sigma calcolato da OpenCV
blurred = cv.GaussianBlur(cv.cvtColor(img, cv.COLOR_BGR2GRAY), (s, s), 0)
# Algoritmo di Canny con soglie t1 e t2
edges = cv.Canny(blurred, t1, t2)
img_e = img.copy()
img_e[edges!=0] = (0,255,255)
```

## Utilizzo della derivata seconda

### Derivata seconda di una funzione discreta

Stima a partire da
$$f'(x)\approx\frac{f(x+h)-f(x-h)}{2h}\approx f\left(x+\frac{1}{2}\right)-f\left(x-\frac{1}{2}\right)$$
$$f''(x)\approx f'\left(x+\frac{1}{2}\right)-f'\left(x-\frac{1}{2}\right)$$
$$f''(x)\approx\left(f\left(\left(x+\frac{1}{2}\right)+\frac{1}{2}\right)-f\left(\left(x+\frac{1}{2}\right)-\frac{1}{2}\right)\right)-\left(f\left(\left(x-\frac{1}{2}\right)+\frac{1}{2}\right)-f\left(\left(x-\frac{1}{2}\right)-\frac{1}{2}\right)\right)$$
$$f''(x)\approx f(x+1)-f(x)-f(x)+f(x-1)=f(x+1)-2f(x)+f(x-1)$$

$$\frac{\partial^2f(x,y)}{\partial x^2}\approx f(x+1,y)-2f(x,y)+f(x-1,y)\implies F^2_x=\begin{bmatrix}1,&-2,&1\end{bmatrix}$$
$$\frac{\partial^2f(x,y)}{\partial y^2}\approx f(x,y+1)-2f(x,y)+f(x,y+1)\implies F^2_x=\begin{bmatrix}1\\-2\\1\end{bmatrix}$$

## Laplaciano

$$\nabla f(x,y)=\frac{\partial^2f(x,y)}{\partial x^2}+\frac{\partial^2f(x,y)}{\partial y^2}$$
Il Lapliaciano e' definito come la somma delle due derivate parziali seconde in un punto.
Il un'immagine $I$, in presenza di un bordo sufficientemente contrastato, il laplaciano $\Delta I$ e':
- $0$ lontano dal bordo;
- $>0$ vicino al bordo (dal lato dove pixel piu' scuri diventano piu' chiari muovendosi verso il bordo);
- $<0$ vicino al bordo (dal lato opposto);
- $0$ in corrispondenza del bordo

### Filtro per calcolare il Laplaciano

Applicando la proprieta' distributiva della convoluzione rispetto all'addizione:
$$\Delta I=\frac{\partial^2I}{\partial x^2}+\frac{\partial^2I}{\partial y^2}=(I*F^2_x)+(I*F^2_y)=I*(F^2_x+F^2_y)=I*F_\Delta$$
$$F_\Delta=\begin{bmatrix}0&1&0\\1&-4&1\\0&1&0\end{bmatrix}$$

### Laplaciano con smooth gaussiano: LoG

Il Laplaciano di un'immagine a cui e' stato applicato uno smooth gaussiano si puo' ottenere mediante convoluzione con un filtro gaussiano $S_G$ seguita da convoluzione con il filtro laplaciano: $F_\Delta: \Delta(I*S_G)=(I*S_G)*F_\Delta$
Per la proprieta' associativa della convoluzione: $(I*S_G)*F_\Delta=I*(S_G*F_\Delta)=I*LoG$
Il filtro $LoG$ esegue quindi entrambe le operazioni e si puo' ottenere calcolando il laplaciano della funzione gaussiana $D_{2D}$:
$$\Delta(G_{2D}(x,y))=\left(\frac{x^2+y^2}{𝜎^4}-\frac{2}{𝜎^2}\right)G_{2D}(x,y)$$
Puo' essere molto simile, ma non identico, al filtro DoG (per opportuni valori $𝜎_1$ e $𝜎_2$)

# Filtro non lineare

## Median filter

Operazione locale in cui il valore di ciascun pixel e' la mediana del valori dei pixel nell'intorno considerato $I'[y,x]=median(w_{xy})$. Puo' ridurre alcuni tipi di rumore preservando maggiormente i bordi rispetto ai filtri di smooth lineari.

### In OpenCV

```python
# Carica un'immagine con rumore nello sfondo (alcuni piccoli punti più chiari)  
img = cv.imread('esempi/bolts.png', cv.IMREAD_GRAYSCALE)
# Applica un filtro gaussiano della dimensione minima per poter eliminare il rumore
noise_reduction_gaussian_filter = cv.GaussianBlur(img, (25,25), 0)
# Applica un filtro mediano della dimensione minima per poter eliminare il rumore
noise_reduction_median_filter = cv.medianBlur(img, 9)
```