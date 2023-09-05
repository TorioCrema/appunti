Formato delle immagini:
- grayscale: shape (h, w) type: `np.uint8`
- bgr: shape (h, w, 3) type: `np.uint8`

Leggere un immagine: `cv.imread('imagePath', colorFormat)`, `colorFormat` puo' essere omesso, in questo caso il formato viene determinato in automatico, altrimenti:
- `cv.IMREAD_GRAYSCALE`: grayscale
- `cv.IMREAD_COLOR`: BGR

Convertire il formato dell'immagine: `cv.cvtColor(img, convType)`, `convType` puo' essere:
- `cv.COLOR_FROM2TO`, dove `FROM` e' il formato attuale e `TO` e' quello in cui si vuole convertire

`cv.calcHist([img], [0], None, [256], [0, 256]).squeeze()` calcola l'istogramma dell'immagine `img` per il canale `0`, con maschera `None`, di lunghezza `256` (cioe' con 256 valori), con range di valori da 0 a 256

`img = img * a + b` variazione luminosita' e contrasto, `a` controlla il contrasto, `b` la luminosita', valori forzati in `[0,256]` con `img.clip()`

`img = (img / 255)^gamma * 255`: gamma correction, con `gamma` < 1 aumenta luminosita' toni scuri, con `gamma` > 1 diminuisce luminosita' toni chiari

`cv.LUT(img, lut)`: applica la lookup table `lut` all'immagine `img`, `lut` puo' essere:
- `cv.COLORMAP_AUTUMN`
- `cv.COLORMAP_JET`
- `cv.COLORMAP_HSV`

Alpha blending:
Due immagini RGB e un valore di trasparenza per ciascun pixel fra 0 e 1
```python
img1 = cv.imread('img1.png')
img2 = cv.imread('img2.png')
img2_alpha = cv.imread('img2-alpha.png')
h, w = img2.shape[:2] # ottengo grandezze seconda immagine (in questo caso img1 > img2)
a = img2_alpha / 255 # trasformo il canale alpha da [0,255] a [0,1]
x, y = 200, 100 # coordinate in cui inserire img2 all'interno di img1
res = img1.copy()
res[y:y+h,x:x+w] = img1[y:y+h,x:x+w]*(1.0-a) + img2*a
```

Binarizzazione:
```python
# Soglia globale con valore 128
_, res = cv.threshold(img, 128, 255, cv.THRESH_BINARY)
# Soglia globale determinata dall'algoritmo di Otsu, t e' la soglia usata
t , res = cv.threshold(img, -1, 255, cv.THRESH_OTSU)
# Soglia locale (media su intorno 11x11 meno il valore 10)
res = cv.adaptiveThreshold(img, 255, cv.ADAPTIVE_THRESH_MEAN_C, cv.THRESH_BINARY, 11, 10)
```

Contrast stretching: Aumento del contrasto
```python
a, b = img.min(), img.max()
a, b = cv.percentile(img, 5), cv.percentile(img, 95)
img = 255 * ((img - a) / b - a) # con a < b
```

Equalizzazione dell'istogramma
```python
# Equalizzazione di un immagine grayscale
res = cv.equalizeHist(img)
# Equalizzazione di un immagine a colori
hls = cv.cvtColor(img, cv.COLOR_BGR2HLS) # convertiamo in HLS
h, l, s = cv.split(hls)
l_eq = cv.equilizeHist(l) # equalizziamo solo il canale l
hls_eq = np.merge((h, l_eq, s)) # uniamo i canali
res = cv.cvtColor(hls_eq, cv.COLOR_HLS2BGR) # riconvertiamo a BGR
```

Trasformazioni geometriche
Gestione delle coordinate fuori dall'immagine:
- `cv.BORDER_CONSTANT` colore di background costante
- `cv.BORDER_REPLICATE` copia del valore piu' vicino
- `cv.BORDER_REFLECT` riflessione dei pixel dell'immagine
- `cv.BORDER_WRAP` replica dell'immagine
- `cv.BOREDR_TRANSPARENT` lascia il valore gia' presente nell'immagine destinazione

Stima del valore di un pixel con coordinate non intere:
Per il ridimensionamento:
```python
h, w = img.shape[:2]
scale = 7.5
size = (int(w*scale), int(h*scale))
img_nn = cv.resize(img, size, interpolation=cv.INTER_NEAREST) # nearest-neighbor
img_bl = cv.resize(img, size, interpolation=cv.INTER_LINEAR) # bilineare
img_bc = cv.resize(img, size, interpolation=cv.INTER_CUBIC) # bicubica

# Se l'immagine viene rimpicciolita si utilizza INTER_AREA
scale = 0.1
size = (int(w*scale), int(h*scale))
img_small = cv.resize(img, size, interpolation=cv.INTER_AREA)
```

Trasformazioni affini:
`cv.warpAffine(img, M, (w,h), dest, stima, border, val)`
- `M` matrice della trasformazione
- `(w,h)` grandezze dell'immagine finale
- `dest` immagine in cui inserire le trasformazioni (puo' essere `None`)
- `stima` metodo con cui stimare il valore delle coordinate non intere (vedi sopra)
- `border` metodo con cui gestire i valori delle coordinate al di fuori dell'immagine (vedi sopra)
```python
h, w, _ = img.shape
background = 0
M_T = np.array([ [1.0, 0.0, 9], [0.0, 1.0, -9] ]) # traslazione
M_S = np.array([ [0.7, 0.0, 0], [0.0, 1.3, 0] ]) # cambio di scala
img_T = cv.warpAffine(img, M_T, (w,h), None, cv.INTER_CUBIC, cv.BORDER_CONSTANT,
					 background)
img_S = cv.warpAffine(img, M_S, (w,h), None, cv.INTER_CUBIC, cv.BORDER_CONSTANT,
					 background)

import math
theta = -math.pi/8
cos_theta, sin_theta = math.cos(theta), math.sin(theta)
# Rotazione di -pi/8
M_R = np.array([ [cos_theta, -sin_theta, 0], [sin_theta, cos_theta, 0] ])
img_R = cv.warpAffine(img, M_R, (w,h), None, cv.INTERCUBIC, cv.BORDER_CONSTANT,
					 background)
```

Trasformazione affine da coppie di punti corrispondenti:
```python
back = cv.imread('background.png')
f = cv.imread('foreground.png')
h, w = f.shape[:2]
f_pts = np.float32([1,1], [w-2,1], [1,h-2]) # punti nell'immagine f
b_pts = np.float32(([ [[68, 35], [106, 35], [68, 66]],
					[[4, 116], [38, 140], [4, 163]] ]))

M = cv.getAffineTransform(f_pts, b_pts)
h, w = back.shape[1:]
res = cv.warpAffine(f, M, (w,h), back, cv.INTER_LINEAR, cv.BORDER_TRANSPARENT)
```

Trasformazioni proiettive:
`cv.getPerspectiveTransform()` calcola la matrice `M` a partire da quattro coppie di punti corrispondenti
`warpPerspective()` applica la trasformazione definita da una matrice `M`, a un'immagine, con il tipo di interpolazione e gestione dei bordi specificato

```python
pts1 = np.float32([[195, 16], [466, 183], [465, 269], [181, 293]])
w, h = 400, 120 # dimensioni immagine destinazione
pts2 = np.float32([[0, 0], [w-1, 0], [w-1, h-1], [0, h-1]])
Mp = cv.getPerspectiveTransform(pts1, pts2)
res = cv.warpPerspective(img, Mp, (w,h), flags = cv.INTER_CUBIC)
```