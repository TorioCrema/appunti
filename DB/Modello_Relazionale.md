# Modello Logico Relazionale
### Caratteristiche
Non vengono utilizzati puntatori per realizzare legami, ma solo valori.

E' un modello logico nel senso che risponde al requisito di indipendenza dalla particolare rappresentazione dei dati adottata a livello fisico.
Nel contesto di un DB realazionale, gli utenti che accedono ai dati e i programmatori che sviluppano applicazioni fanno riferimento ==solo al livello logico==, senza specificare i percorsi di accesso per eseguire operazioni.
Nel modello relazionale l'unica astrazione e' il concetto di ==relazione==; non vi sono costrutti concettuali di altro livello in grado di descrivere entita', associazioni, generalizzazioni, specializzazioni, aggregazioni.

## Relazione matematica
Si considerino due insiemi $A, B$ non vuoti e non cesessariamente distinti:
ogni sottoinsieme non vuoto del prodotto cartesiano $A \times B$ e' detto ==relazione da A a B.==.
Concetto estendibile al prodotto cartesiano di $n$ insiemi.
#### Proprieta'
Una relazione e' un'insieme di n-ple...:
- tutte le n-ple sono distinte tra loro;
- non e' definito alcun ordinamento tra le diverse n-ple;

... ciascuna considerata al proprio interno ordinata rispetto ai domini ...:
- l'ordine in cui si considerano i domini e' rilevante;

... su domini non necessariamente distinit:
- uno stesso dominio puo' essere usato in piu' posizioni;

## Data Base relazionale
==Lo schema di un DB== relazionale e' un insieme di relazioni con nomi distinti.