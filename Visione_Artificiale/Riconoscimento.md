# Riconoscimento

- Classificazione
- Localizzazione
- Object detection
- Instance segmentation

## Confronto diretto fra immagini

Il confronto pixel-a-pixel di due immagini raramente puo' essere di qualche utilita', per molteplici ragioni, fra cui:
- Differenze di traslazione, rotazione, scala e prospettiva
- Deformazione e variabilita' degli oggetti
- Cambiamenti di illuminazione
- Presenza di rumore nelle immagini e utilizzo di tecniche di acquisizione diverse

## Template matching

Invece di confrontare direttamente due immagini, si definiscono uno o piu' pattern modello (template): piccole immagini che vengono poi "cercate" all'interno dell'immagine, misurandone il grado di "somiglianza" (matching) in tutte le possibili posizioni.
- La tecnica piu' semplice consiste nel confrontare i pixel del template con quelli dell'immagine, anche se in determinate applicazioni si possono ottenere migliori risultati confrontando altre caratteristiche, come i bordi o l'orientazione del gradiente.

### Distanza fra immagine e template: 

#### SQDIFF

Somma delle differenze al quadrato (in OpenCV `CV_TM_SQDIFF`):
$$R[y,x]=\sum^{h-1}_{j=0}\sum^{w-1}_{j=0}(T[i,j]-I[y+i,x+j])^2$$

#### SQDIFF normalizzata

Somma delle differenze al quadrato normalizzata (in OpenCV `CV_TM_SQDIFF_NORMED`):
$$R[y,x]=\frac{\sum^{h-1}_{j=0}\sum^{w-1}_{j=0}(T[i,j]-I[y+i,x+j])^2}{\sqrt{(\sum^{h-1}_{i=0}\sum^{w-1}_{j=0}(T[i,j])^2)\times(\sum^{h-1}_{i=0}\sum^{w-1}_{j=0}(I[y+i,x+j])^2)}}$$

#### Correlazione

Correlazione (in OpenCV `CV_TM_CCORR`):
$$R[y,x]=\sum^{h-1}_{i=0}\sum^{w-1}_{j=0}(T[i,j]\times I[y+i,x+j])$$

#### Correlazione normalizzata

Correlazione normalizzata (in OpenCV `CV_TM_CCORR_NORMED`):
$$R[y,x]=\frac{\sum^{h-1}_{i=0}\sum^{w-1}_{j=0}(T[i,j]\times I[y+i,x+j])}{\sqrt{(\sum^{h-1}_{i=0}\sum^{w-1}_{j=0}(T[i,j])^2)\times(\sum^{h-1}_{i=0}\sum^{w-1}_{j=0}(I[y+i,x+j])^2)}}$$

#### Coefficiente di correlazione

Coefficente di correlazione (in OpenCV `CV_TM_COEFF`):
$$R[y,x]=\sum^{h-1}_{i=0}\sum^{w-1}_{j=0}(T'[i,j]\times I'_{x,y}[y+i,x_j])$$
con $$T'[i,j]=T[i,j]-\frac{\sum^{h-1}_{i'=0}\sum^{w-1}_{j'=0}T[i',j']}{w\times h}$$ e $$I'[i,j]=I[i,j]\frac{\sum^{h-1}_{i'=0}\sum^{w-1}_{j'=0}I[y+i',x+j']}{w\times h}$$

#### Coefficiente di correlazione normalizzato

Coefficiente di correlazione normalizzato (in OpenCV `CV_TM_CCOEFF_NORMED`):
$$R[y,x]=\frac{\sum^{h-1}_{i=0}\sum^{w-1}_{j=0}(T'[i,j]\times I'_{x,y}[y+i,x_j])}{\sqrt{(\sum^{h-1}_{i=0}\sum^{w-1}_{j=0}(T'[i,j])^2)\times(\sum^{h-1}_{i=0}\sum^{w-1}_{j=0}(I_{x,y}[y+i,x+j])^2)}}$$
con $$T'[i,j]=T[i,j]-\frac{\sum^{h-1}_{i'=0}\sum^{w-1}_{j'=0}T[i',j']}{w\times h}$$ e $$I'[i,j]=I[i,j]\frac{\sum^{h-1}_{i'=0}\sum^{w-1}_{j'=0}I[y+i',x+j']}{w\times h}$$

### Template matching in OpenCV

```python
I = cv.imread('esempi/sheldon.jpg', cv.IMREAD_GRAYSCALE)
T = cv.imread('esempi/cat-template.png', cv.IMREAD_GRAYSCALE)
h, w = T.shape
metodi = ['cv.TM_SQDIFF', 'cv.TM_SQDIFF_NORMED',
'cv.TM_CCORR', 'cv.TM_CCORR_NORMED',
'cv.TM_CCOEFF', 'cv.TM_CCOEFF_NORMED']
@interact(metodo=metodi)
def test_templateMatching(metodo):
	metodo = eval(metodo)
	# Confronta T con tutte le posizioni (x,y) di I, risultato in R
	R = cv.matchTemplate(I, T, metodo)
	# Cerca il minimo e il massimo di R
	r_min, r_max, pos_min, pos_max = cv.minMaxLoc(R)
	if metodo in [cv.TM_SQDIFF, cv.TM_SQDIFF_NORMED]:
	pos, val = pos_min, r_min
	else:
	pos, val = pos_max, r_max
	# Disegna un rettangolo nella miglior posizione trovata
	res = cv.rectangle(I.copy(), pos, (pos[0]+w, pos[1]+h), (0,255,255), 2)
```

## Estrazione caratteristiche (feature) e riconoscimento

Un modo per ridurre la quantita' di informazioni di un'immagine, rappresentando le parti piu' interessanti sotto forma di vettori numerici compatti.
Una volta che le feature sono state estratte, possono essere utilizzate per individuare e riconoscere oggetti con varie tecniche.

### Machine learning

Un sistema di machine learning (apprendimento automatico) durante la fase di "training" (addestramento), apprende a partire da una serie di esempi.
Successivamente, in fase di inference, e' in grado di generalizzare e gestire anche nuovi dati nello stesso dominio applicativo.

### Deep learning

Una tipologia di machine learning in cui sia la scelta delle feature che la classificazione viene appresa direttamente dagli esempi.
L'addestramento avviene utilizzando un grande set di dati etichettati e reti neurali "profonde", ossia contenenti un numero elevato di livelli hidden (decine o centinaia).
L'addestramento puo' richiedere molto tempo (giorni o settimane). L'utilizzo di una o piu' GPU consente di velocizzare il processo.

### Convolutional Neural Network

Reti neurali profonde specializzate per immagini e video: sono fra le tecniche di deep learning piu' utilizzate.
A ciascuna immagine, a diverse risoluzioni, vengono applicati filtri, il cui output viene utilizzato come input per il livello seguente. I filtri possono catturare inizialmente feature molto semplici, come la luminosita' e i bordi, per assumere via via forme piu' complesse, fino a definire l'oggetto da riconoscere.
Utilizzano connessioni locali e condivisione dei pesi per ridurre drasticamente il numero di parametri.

#### CNN con OpenCV

```python
# Carica la rete da file
net = cv.dnn.readNet('minst/model.onnx')
# Carica un'immagine di test
img = cv.imread('minst/test_6.png', cv.IMREAD_GRAYSCALE)
# Fornisce l'immagine di test in ingresso alla rete
net.setInput(cv.dnn.blobFromImage(img))
# Esegue la rete per generare l'ouput (inferenza)
out = net.forward()
classe = np.argmax(out) # Risultato della classificazione (0..9)
```

Esempio di utilizzo di una rete (gia' addestrata) per classificare immagini di cifre numeriche scritte a mano.
La rete riceve in ingresso un'immagine grayscale di $28\times28$ pixel e restituisce un vettore di $10$ elementi (corrispondenti alle cifre $[0,9]$): la classe piu' probabile secondo la rete e' quella corrispondente al valore piu' alto nel vettore.

```python
# Stampa tipo e forma dell'input e ouput di ogni livello
for i, l_in, l_out in zip(*net.getLayersShapes((1,1,28,28))):
	i = int(i[0]); l = net.getLayer(i)
	print(i, l.type, l_in[0].squeeze()[1:], l_out[0].squeeze()[1:])
# Esegue la rete e restituisce l'output di tutti i livelli
all_outs = net.forward(net.getLayerNames())
va.show(*all_outs[1][0]) # Visualizza l'output dei filtri del livello 1 come 8 immagini
```

#### Object detection: YOLO

YOLO v3 e' un sistema di object detection addestrato su $80$ classi di oggetti, in grado di restituire, per ogni oggetto, il corrispondente bounding box.
- Si tratta di una CNN con $106$ livelli