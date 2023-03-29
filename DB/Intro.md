Necessita' di gestire grandi quantita' di dati, potenzialmente scalabili in dimensione, in modo persistente e condiviso e consentire di soddisfare esigenze applicative che mutano nel tempo.

==**DBMS**==:
- gestisce grandi quantita' di dati _persistenti e condivisi_
- offre supporsto per almeno un ==modello di dati==, fornisce agli utenti un atrazione di alto livello attraverso cui definire strutture di dati complesse e interagire con il DB
- attua indipendenza tra programmi e dati, e tra programmi e operazioni
- garantisce efficienza nella gestione di grandi quantita' di dati e persegue obiettivi di efficacia
La ==persistenza== e la ==condivisione== richiedono che un DBMS fornisca meccanismi per garantire l'affidabilita' dei dati (==fault tolerance==), per il controllo degli accessi e per il controllo della concorrenza.
Database engine: modulo software che un DBMS usa per gestire le operazioni su un DB: ==create, read, update, delete==.

- Analisi dei requisiti -> Si formalizzano i requisiti, avvelendosi di tecniche di modellazione della realta' e si producono macro-specifiche per la fase di progettazione.
- Progettazione del sistema -> Si interpretano i requisiti in una ==soluzione architetturale di massima==.

### Progettazione di un SI
L'obiettivo della progettazione e' fornire una descrizione organica e coerente dei requisiti informativi illustrando aspetti statici, funzionali e dinamici.
A parire dalle prime interviste, il processo di analisi porta, per passi successivi di ==raffinamento==, alla stesura di docuenti in grado di rappresentare un modello dell'organizzazione e descrivere i requisiti del sistema da realizzare.

Due aspetti di primaria importanza nella progettazione di un SI:
- progettazione della base di dati
- progettazione delle applicazioni
Il ruolo primario viene svolto dai dati.

Modellazione di aspetti statici -> schemi entity/relationship
Modellazione di aspetti funzionali -> data flow diagrams
Modellazione di aspetti dinamici (stati) -> state diagrams

Requisiti del sistema informativo -> Progettazione concettuale -> Schema concettuale -> Progettazione logica -> Schema logico -> Progettazine fisica -> Schema fisico

### Progettazione dell'applicazione
Obiettivo di realizzare ==moduli software== per le funzioni e per l'interfaccia utente.

### Progettazione di un DB
Puo' avvenire in momenti diversi di un ciclo di vita di un sistema. Puo' essere svolta nell'ambito della piu' ampia attivita' di sviluppo ==ex novo== di un sistema informativo; oppure Si richiede di sviluppare un nuovo DB con le relative applicazioni, o reingegnerizzare un DB esistente eventualmente aggiungendo nuove funzionalità, nel contesto di un sistema informatico già in esercizio

### Meccanismi di astrazione
- ==**Classificazione**== -> raggruppa gli oggetti in classi in base alla loro proprieta'
- ==**Generalizzazione**== -> cattura una relazione tra diverse classi di tipo "==e' un==", ovvero permette di astrarre le caratteristiche comuni fra piu' classi definendo superclassi
	Diviso in base al ==confronto fra unione delle specializzazioni e classe generalizzata==:
	- Totale -> ogni istanza della classe generalizzata fa parte di almeno una delle specializzazioni
	- Parziale -> una istanza della classe generalizzata puo' non far parte di una delle specializzazioni
	E in base al ==confronto fra le classi specializzate==:
	- Esclusiva -> gli insiemi delle specializzazioni sono fra loro disgiunti
	- Sovrapposta -> puo' esistere un'intersezione non vuota fra insiemi delle specializzazioni
	
Esistono quindi 4 proprieta' di copertura:
![Proprieta' di copertura](generalizzazione.png)

- ==**Aggregazione**== -> esprime le relazioni parte di che sussistono tra oggetti, tra funzioni, o fra stati
- ==**Proiezione**== -> cattura la vista delle relazioni strutturali fra gli oggetti, le funzioni, gli stati

#### Associazioni
Rappresentano i legami tra classi diverse, sono di fatto aggregazioni di cui le classi sono le componenti. E' importante caratterizzare queste corrispondenze in termini di vincoli di cardinalita'.
##### Tipologie di associazioni
La *cardinalita' minima* definisce la partecipazione _opzionale o obbligatoria_ di una associazione per una classe.
La *cardinalita' massima* definisce i seguenti tipi di associazioni:
==Associazione uno a uno== -> deve essere 1 in entrambe le classi coinvolte.
==Associazione uno a molti== -> per una classe deve essere >1 e nell'altra deve essere 1.
==Associazione molti a molti== -> deve essere >1 in entrame le classi.

### Aggregazioni
Definiscono relazioni "==parte di==".