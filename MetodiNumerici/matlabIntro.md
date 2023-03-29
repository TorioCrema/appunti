# MATLAB Intro

L'insieme delle variabili mantenute in memoria durante la sessione MATLAB viene chiamata spazio di lavoro (==workspace==).
### Comandi
- `who` -> visualizza la lista delle variabili attive nel corrente spazio di lavoro
- `whos` -> come `who` ma fornisce informazioni piu' dettagliate come lo spazio occupato, il numero di elementi e il tipo delle variabili
- `what` -> mostra un elenco degli m-files memorizzati
- `clear <nomevariabile>` -> cancella la variabile 'nomevariabile'
- `clear` o `clear all` -> cancella tutte le variabili di una sessione di lavoro
MATLAB e' un linguaggio case sensitive.

### Operatori
MATLAB utilizza i normali operatori matematici presenti nella maggior parte dei linguaggi di programmazione e' pero' privo dell'operatore `%` e l'operatore logico `not` e' rappresentato da `~`.
`\` -> divisione a sinistra:
	Utilizzato per operazioni come trovare `x` tale che `3x = 12`, in questo caso scriviamo `3\12

### Funzioni base
- `a = [1 2 3 4]`  -> definisce una matrice 1x4 di nome 'a' 
	`;`  -> nella definizione degli elementi di una matrice indica la fine di una riga
- `size(a)`  -> fornisce le dimensioni di a
- `lenght(a)`  -> usato per i vettori, indica la loro lunghezza
- `a'`  -> trasposta di a
- `a(1,2)`  -> fa riferimento all'elemento in posizione riga 1 colonna 2
- `:`  -> indica tutte le righe o tutte le colonne, esempio:
	- `c(1,:)` indica la prima riga, tutte le colonne 
	- `c(:,2)` indica tutte le righe, la seconda colonna 
	- `c(:,2:4)` indica tutte le righe, dalla seconda alla quarta colonna
- `sum(c)`  -> calcola la somma degli elementi di una matrice per colonne, il risultato e' un vettore
- `mean(c)`  -> fornisce la media per colonne
- `max(c)`  -> fornisce il massimo per colonne
- `min(c)`  -> fornisce il minimo per colonne

### Matrici speciali
- `ones(2,3)`  -> crea una matrice 2x3 con ogni elemento ad 1, se il secondo parametro e' omesso la matrice sara' quadrata
- `zeros(1,4)`  -> come `ones` ma gli elementi saranno 0
- `rand(3,3)`  -> crea una matrice con elementi double random
- `eye(2)`  -> crea una matrice identita' quadrata

### Operazioni su matrici
E' possibile sommare e sottrarre matrici purche' le dimensioni siano compatibili.
Due matrici possono essere moltiplicate solo se il numero delle colonne della prima e' uguale al numero di rige della seconda.
#### Operatori su elementi
Indicano operazioni aritmetiche tra elementi corrispondenti, sono:
`.*`, `./`,  `.\`, `.^`
Questi simboli applicano le operazioni ad ogni elemento della matrice separatamente, ad esempio per elevare alla seconda ogni elemento di una matrice `A`: `A.^2`.

## Programmare in MATLAB
Il linguaggio di MATLAB e' *interpretato*, si possono realizzare programmi (m-file) utilizzando le classiche strutture di programmazione (cicli, flussi di controllo, input-output).
Esistono due tipi di programmi, noti come `m-file`: `script`, `function`.
Uno script e' una lista di comandi che puo' richiamare funzioni MATLAB built-in o create utilizzando altri m-file. Non richiede input e non fornsce output espliciti, e' simile ad un main, tutte le variabili sono disponibili nel workspace; il file nome.m deve essere nel proprio path corrente. Per eseguire uno script si richiama il suo nome privo di estensione `.m`  nella Command Window.

Un m-file che contiene una funzione MATLAB si identifica nella prima riga con:
`function y = functionName(argomenti input)`
`y`  -> variabile in output, quando sono piu' di una si usa la seguente forma:
	`function [y,z] = functionName(argomenti input)`
**Il nome dell'm-file deve essere il nome dato alla funzione.** Le variabili create al suo interno sono viste solo *localmente* dalla funzione stessa, sono *variabili locali*. 
Uno stesso m-file puo' contenere piu' function: la function principale sta all'inizio del file e gli da il nome, mentre quelle seguenti sono secondarie. Soltanto la function principale puo' essere chiamata da altre function esterne o dal prompt.

#### Flussi di controllo
Sono presenti tutti i classici flussi di controllo:
`for counter = start:step:final end` -> nel caso in qui il parametro `step` venga omesso sara' usato il valore di default 1.
`while (condizione di entrata) end`
`if (elseif else) end` 
```MATLAB
switch (relation expression)
	case (value) block of statements
	case (value) block of statements
	case (value) block of statements
	otherwise block of statements
end
```

#### Plotting di grafici
`figure(1)` crea una figura in cui verranno visualizzati di grafici.
`plot(x, y, opzioni)` -> `x` deve essere un vettore, `y` deve essere un vettore della stessa grandezza di `x`, nella variabile `opzioni` vengono messe le preferenze per indicare come verra' visualizzato il grafico.
`hold on` permette di impostare una `figure()` in modo che mantenga tutti i grafici su cui chiamiamo `plot()`, `hold off` per disattivare.

`nomefunzione = @(x) x^3 - 3*x` -> crea un puntatore a funzine di nome nomefunzione e valore x^3 - 3*x.

#### Input da tastiera
`scleta = input('stringa che verra' visualizzata)'` -> la variabile scleta avra' come valore il valore dato in input dall'utente.
`scleta = menu('Inserire funzione desiderata: ', 1, 2, 3)` -> crea un menu con un bottone per ogni opzione, la variabile avra' come valore il valore del bottone premuto.

`rem(n, 2)` -> calcola il resto della divisione di n per 2.