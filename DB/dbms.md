# Funzionalita' DBMS
---
Il Database Management System e' un insieme di programmi che permettono agli utenti di ==definire, costruire, manipolare e condividere== una base di dati.

Condividere -> gestione dell'accesso concorrente ai dati.

## Caratteristiche
- supporto per almeno un modello dei dati
- uso di cataloghi
- supporto di viste multiple sui dati condivisi
- ==indipendenza== tra programmi e dati, e tra programmi e operazioni
- gestione efficiente ed efficacie di grandi quantita' di dati ==persistenti e condivisi==
- linguaggi di alto livello per la definizione dei dati, interazione con il DB
- Funzionalita' di supporto per semplificare la descrizione delle informazioni, lo sviluppo delle applicazioni etc.

## Modello logico dei dati
L'astrazione logica con cui i dati sono resi disponibili all'utente definisce un ==modello dei dati== (collezione di concetti utilizzati per descrivere i dati, le loro associazioni, e i vincoli che questi devono rispettare).

Il sistema di basi di dati contiene anche una definizione o ==descrizione completa della struttura== e dei suoi vincoli. Tale definizione e' memorizzata nel ==catalogo== del sistema.

Un importante obiettivo di un DBMS consiste nel fornire caratteristiche di ==indipendenza logica e indipendenza fisica==.

## Architettura a tre livelli di un DBMS
Schemi esterni -> Schema logico -> Schema interno

### Livello fisico (interno)
Realizzazone fisica del DB -> file.
Serie di file, residenti su dispositivi di memoria permanenti che contengono dati. Lo shema fisico descrive come il DB e' rappresentato a livello fisico.
La gestione del ==DB fisico== e' a carico del ==DBA== (Data Base Administrator) e non degli utenti.

### Livello delle viste (esterno)
A ogni utente devono essere associate ==autorizzazioni== distinte.

## Concorrenza e protezione di guasti
Un DBMS deve garantire che gli accessi ai dati, da parte di diverse applicazioni, non interferiscano tra loro violando vincoli di integrita' e alterando la consistenza del DB.
A tale scopo e' pertanto necessario far ricorso a opportuni meccanismi di ==controllo della concorrenza==.
Nel caso di una o piu' operazioni mal effettuate il DB deve mantere uno stato finale ==consistente==.

### RDBMS: Proprieta' ACID
- ==A==tomicity: la transazione e' indivisibile nella sua esecuzione, cioe' o viene portata a termine o viene annullata.
- ==C==onsistency: all'inizio e alla fine di ogni operazione lo stato del DB deve essere consistente.
- ==I==solation: ogni transazione deve essere ==eseguita in modo isolato== e indipendente dalle altre transazioni.
- ==D==urability: persistenza dei dati, i dati modificati devono essere stati memorizzati in un dispositivo durevole.

## Linguaggi
Oltre alle istruzioni necessarie per la manipolazione della base di dati, per realizzare applicazioni e' necessario disporre di linguaggi general purpose.
- **Linguaggio nativo**
- **Linguaggio ospite** con procedure esterne
- **Linguaggio ospite** con precompilatore

## Moduli
- Query manager
- Transaction manager
- DDL compiler
- Storage manager
- Logging manager
- Logging & Recovery manager
- Concurrency manager

### Utenti: DBA
Lo staff di amministrazione lavora sulla ==definizione della base di dati== e della sua ==manutenzione==, modificano le definizioni per mezzo del ==linguaggio DDL== e di altri comandi privileggiati.

### Utenti: occasional users
Gli utenti occasionali utilizzano le cosiddette ==interfacce interattive==. Non utilizzano applicazioni che possono generare le interrogazioni in modo automatico.

### Utenti: application programmers
I programmatori di applicazioni scrivono programmi in ==liguaggi ospite== che vengono quindi passati a un pre-compilatore che estrae ==comandi DML==.

## Processore runtime
Si occupa di eseguire:
- i comando privileggiati
- i piani di esecuzione delle interrogazioni
- le transazioni parametriche predefinite con specifici parametri
Ingeragisce con il ==catalogo== per aggiornare le statistiche
Interagisce con il gestore dei dati memorizzati, il quale a sua volta usa i servizi del ==sistema operativo== per realizzare le operazioni in input e output di basso livello tra disco e memoria centrale.

## Architettura File Server
Un unico server in rete che agisce come gestore di ==shared store==; l'elaborazione avviene nelle vaarie workstation dotate di DBMS.

## Architettura Client/Server
Come file server ma esiste solo una istanza del DBMS all'interno del server.

