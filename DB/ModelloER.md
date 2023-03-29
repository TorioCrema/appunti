# Modello Entity-Relationship
Rappresentazione grafica intuitiva che consente di disegnare schemi E/R facilitando la comprensione e l'interpretazione dei requisiti informativi modellati.

## Concetti fondamentali
### Entita'
- **Entity** -> rappresenta una ==classe== (insieme) di oggetti della realta' di interesse che possiedono _caratteristiche comuni_ e che hanno un esistenza "autonoma".
	Rappresentate con un rettangolo al cui inerno e' evidenziata la denominazione dell'entita' stessa, e' **fortemente sconsigliato** usare sigle, l'insieme dell'entita' e' inteso in senso matematico, quindi _non_ possono essere presenti elementi ripetuti.
	==Livello intensionale==: schema che rappresenta l'entita' stessa, ne descrive la struttura cioe' l'aspetto intensionale.
	Ogni istanza di una classe per essere ammissibile deve avere certe caratteristiche.
	==Livello estensionale==: parliamo di un istanza di un entita' che e' uno specifico oggetto appartenente all'insieme che quella entita' rappresenta.
	Ne deriva che un'entita' e' un insieme di istanze ammissibili per quella entita'.
	E' importante _evitare qualsiasi tipo di ambiguita' tra entita'_, usare nomi diversi per ogni entita'. Non esiste una convenzione universalmente accettata, preferibile usare nomi al singolare al livello concettuale, mentre per lo schema logico e' preferibile usare nomi al plurale.
	Sostantivo singolare per il modello ER, in maiuscolo.

### Relazione
- **Relationship** -> (associazione) rappresenta un ==legame logico== tra entita', rilevante nella realta' che si sta considerando.
	Una sua istanza e' una ==combinazione (aggregazione) di istanze delle entita'==.
	==Livello intensionale==: legame tra due tipologie di entita'.
	==Livello estensionale==: coppie di istanze delle entita' collegate.
	Anche _le istanze delle associazioni sono insiemi_, quindi non e' possibile avere elementi ripetuti.
	Il nome di un'associazione deve essere univoco, preferibile un _sostantivo singolare_ invece di un verbo o di una composizione di nomi di entita'.
	Un'associazione n-aria coinvolge n entita' non necessariamente distinte. Il grado di un'associazine e' il numero di istanze di entita' che sono coinvolte in un'istanza dell'associazione.
	==Associazione binaria== -> grado = 2
	==Associazione ternaria== -> grado = 3
	Un'associazione ad anello lega un'entita' con se' stessa, e quindi mette in relazione tra loro le istanze di una stessa entita'. Puo' essere ==simmetrica, riflessiva e transitiva==.
	![Associazioni ad anello](associazioniAnello.png)
	Nelle associazioni ad anello non simmetriche e' necessario specificare, per ogni ramo dell'associazione, il relativo ruolo.
	![Anello non generiche](anello_non_generiche.png)

### Attributo
- **Attribute**  -> proprieta' elementare di un'entita' o di un'associazione. Denotato con un nome che deve essere univoco all'interno dell'entita' o associazione a cui si riferisce. Ogni attributo e' definito su un ==dominio di valori==. Un attributo associa a ogni istanza di entita' o associazione un valore del corrispondente dominio.
Per un attributo e' possibile specificare anche ==il numero minimo== e ==il numero massimo== di valori che possono essere associati a un'istanza della corrispondente associazione o entita' a cui l'attributo appartiene.
![Cardinalita' di attributi](cardinalitaattributi.png)
	Gli ==attributi composti== si ottengono ==aggregando== altri sotto-attributi, i quali _presentano una forte affinita' nel loro uso e significato_. Un attributo non composto e' detto ==semplice==.

### Vincolo di cardinalita'
- **Vincolo di cardinalita'** -> alcuni sono ==impliciti==, in quanto dipendono dalla semantica stessa dei costrutti del modello, altri sono ==espliciti== e sono definiti da chi progetta lo schema E/R sulla base della conoscenza della realta' che sta modellando.
==Sono coppie di valori (min-card, max-card) associati a ogni entia' che partecipa a un'associazione; specificano il numero minimo e massimo di istanze dell'associazione a cui un'istanza dell'entita' puo' partecipare.==
Definiscono le relazioni uno a uno, uno a molti e molti a molti.
Definiscono i tipi di associazioni:
1. Sulla base della cardinalita' minima:
	- Opzionale
	- Obbligatoria
2. Sulla base della cardinalita' massima:
	- A valore singolo
	- A valore multiplo

La cardinalita' minima deve prendere anche in considerazione il modo in cui verranno inseriti i dati nel sistema.
Il modello E/R **non modella aspetti dinamici**. In termini matematici quando si definisce una relazione $R$ fra due insiemi $A$ e $B$, si supponone che gli insiemi esistano gia'. In un DB gli insiemi cambiano dinamicamente, pertanto per rispettare i vncoli di cardinalita' si devono operare opportuni accorgimenti nelle fasi di inserimento.

### Identificatore
- **Identificatore** -> serve ad individuare in modo univoco ogni istanza di un identita'. Ogni entita' deve averne un numero $\ge 1$.
Possono essere interni o esterni:
- Interno: si usano uno o piu' attributi dell'entita'.
- Esterno: si usano altre (uno o piu') entita', collegate all'entita' da associazioni, piu' eventuali attributi dell'entita', in questo caso e' definito misto.

Se il numero di attributi e' $1$ si chiama semplice altrimenti composto.
Un identificatore esterno composto e' ammissibile se e solo se la relazione da cui si prende l'attributo per formare l'identificatore ha cardinalita' $(1, 1)$.
Ogni entita' deve avere almeno un identificatore ma _ne puo' avere piu' di uno._
In base al contesto dei requisiti informativi, ==tutti i possibili identificatori di un entita' **devono** essere indicati nello schema.==
Cio' riveste estrema rilevanza in fase di progettazione logica, dove uno di essi sara' scelto come chiave primaria e **per gli altri si dovra' comunque imporre un vincolo di unicita'.**

#### Entita' deboli
Nelle relazioni uno a uno, e' possibile utilizzare un identificatore esterno semplice per una delle due entita'. In questo caso l'entita' che utilizza questo identificatore e' chiamato entita' debole, perche non esisterebbe una istanza di questa entita' senza una istanza dell'altra. ==In generale un'entita' e' definita debole se ha solo identificatori esterni, e forte se ha solo identificatori interni. O se per la sua esistenza e' richiesta l'esistenza di un altra istanza di un entita' con cui e' in relazione.==
Nel caso di eliminazione dell'istanza di riferimento le istanze deboli collegate devono essere eliminate.

### Gerarchia di generalizzazione
- **Gerarchia di generalizzazione** -> descritta in parte in ![IntroDB](./Intro).

Per una generalizzazione che non sia un semplice subset deve essere specificato anche il tipo di copertura.
![Tipi di generalizzazione](generalizzazione.png)

![Esempi di gerarchia](gerarchia_generalizzazione.png)

Gli attributi comuni lungo una gerarchia devono essere riferiti all'entita' piu' generica (in cui sono presenti obbligatoriamente).

==Le gerarchie non dovrebbero essere usate per modellare aspetti dinamici== della realta' di interesse.

##### Subset
E' un caso particolare di gerarchia _is a_ in cui si evidenzia una sola classe specializzata. Non ha senso parlare di tipo di copertura.
![](subset.png)

### Associazioni ternarie
Quando in un'associazione ternaria esistono ==dipendenze funzionali== tra le entita' in gioco ==e' preferibile== sostituire la ternaria con associazioni binarie, ==che modellano esplicitamente i vincoli del problema==.
Se una o piu' entia' partecipano ==con cardinalita' massima 1== a un'associazione ternaria possiamo sempre modellarla, in modo equivalente, attraverso assoiazioni binarie (ma non sempre solo due).
