# Architettura del Web

Le 3 tecnologie fondamentali che ancora oggi sono alla base del funzionamento del Web erano pronte entro l'ottobre del 1990:
- HTML (HyperText Markup Language): **Formato** per le pagine
- URI (Uniform Resource Identifier): **Identificazione** delle risorse
- HTTP (HyperText Transfer Protocol): **Interazione** C/S sopra TCP

## Principi architetturali

- **Formato**: la disponibilita' di apposite applicazioni sul computer ricevente e la corretta identificazione del formato dati con cui la risorsa e' stata comunicata permette di accedere al suo contenuto in maniera chiara e non ambigua. I formati sono molti, tra cui HTML, PNG, RDF, ecc.
- **Identificazione**: Il WWW e' uno spazio informativo (per umani e applicazioni) in cui ogni elemento di interesse e' chiamato risorsa ed e' identificato da un identificatore globale chiamato URI.
- **Interazione**: Un protocollo di comunicazione chiamato HTTP permette di scambiare messaggi su una rete informatica (in particolare TCP/IP)

### HTML

HTML e' un linguaggio di markup progettato per dare formato a documenti scientifici con stuttura ipertestuale.
- Il contenuto del documento e' inframezzato da elementi di markup (chiamati tag) che ne definiscono stuttura e semantica.
- I tag sono stringhe contenute tra < e >.
- Il linguaggio e' evoluto dal 1998 (W3C standard 5.3)

### CSS

Gli aspetti presentazionali della pagina sono gestiti attraverso un linguaggio specifico che ne definisce gli stili, CSS (Cascading Style Sheets).
- Separazione presentazionale (CSS) e contenuto (HTML)
- CSS permette di controllare le caratteristiche presentazionali dei documenti HTML
- La definizione di CSS1 e' del 1996 e il supporto dei browser e' iniziato da IE3 (1996) e Netscape Communicator 4 (1997)
- Attuale versione 3

### Browser

Il client o browser e' un visualizzatore di documenti ipertestuali e multimediali in HTML:
- Inizia l'interazione (come client)
- Puo' visualizzare testi, immaigni e semplici interfacce grafiche
- Puo' agire da editor, ma solo localmente
- I browser hanno anche:
	- plug-in che permettono di visualizzare ogni tipo di formato speciale
	- un liguaggio di programmazione interno (Javascript)

### Server

Il server e' una applicazione in grado di rispondere a richieste di risorse locali (file, record di database, ecc.) individuate da un identificatore univoco:
- La funzione primaria del sever web e' rispondere alle richieste di pagine effettuate dai client.
- Il server web puo' collegarsi ad applicazioni server-side ed agire da tramite tra il browser e l'applicazione in modo che il browser diventi l'interfaccia di una applicazione di gira altrove.

### HTTP: Pull e Push

Modello di interazione C/S:
- Il modello di funzionamento di HTTP prevede che l'interazione tra client e server sia iniziata esclusivamente dal client. Quindi il client attira a se' (pull) i contenuti richiesti. Questo impone che il client (o, piu' precisamente l'utente) richieda ogni volta l'informazione.
- In certi altri casi, invece, si vuole prediligere la immediata disponibilita' di informazione anche se l'utente non le ha richieste. Quindi il sever spinge al client (push) i contenuti. Questo e' il funzionamento di molte applicazioni di casting (webcasting, podcasting, streaming, ecc) e anche di tutti i sistemi di instant messaging.
- Sistemi piu' recenti utilizzano un modello pseudo-push basato su polling (pool): un processo automatico, ad intervalli regolari, interroga il server (poll) per sapere se ci sono nuovi contenuti. Viene usato, per esempio da RSS o dalla mail.

### Cookies

Il termine cookie indica un bloco di dati opaco lasciato dal server in consegna ad un richiedente per poter ristabilire in seguito il suo diritto alla risorsa richiesta:
- I cookies sono usati nella gestione delle sessioni
- Ogni server lascia in consegna suoi cookies e associa a questi dati informazioni sulla transazione
- Ogni volta che il browser accedera' al sever che ha lasciato i cookie, rifornira' i dati del cookie che permettono al server di identificare il richiedente, e creare cosi' un profilo specifico

### Javascript

Javascript e' un liguaggio di scripting orientato agli oggetti e agli eventi, comunemente utilizzato nella programmazione Web lato client:
- E' stato introdotto da Netscape come LiveScript
- E' un marchio Oracle, il linguaggio standard e' ECMAScript, standardizzato da ECMA
- Un linguaggio interpretato debolmente tipizzato, debolmente orientato agli oggetti
- Funziona anche lato server

### AJAX

AJAX (Asynchronous JavaScript and XML), un gruppo di tecnologie utilizzate per la realizzazione di RIA (Rich Internet Application) ovvero applicazioni web client-side e server-side fortemente interattive:
- Lo sviluppo AJAX si basa su uno scambio di dati in background fra web browser e server, che consente l'aggiornamento dinamico di una pagina web senza esplicito ricaricamento da parte dell'utente.

### Framework di sviluppo

I framework sono librerie che rendono piu' ricco, sifisticato e semplice l'uso di tecnologia, come un liguaggio server-side, un linguaggio client-side o le specifiche grafiche di una pagina web.
- Server-side esitono dalla fine degli anni novanta, e hanno reso la programmazione a tre livelli drasticamente piu' facile.
- Client-side si sono sviluppate a partire dal 2002, su CSS e Javascript, con scopi molto difformi.

Si puo' utilizzare un full-stack framework che fornisce piattaforme completamente integrate oppure un glue-framework che usa contestualemente strumenti con completamente integrati

### Web solution stack

Un solution stack e' un insieme di componenti o sottoinsiemi software che sono necessari per creare una piattaforma completa in modo che nessun software aggiuntivo sia indispensabile allo sviluppo applicazioni.
Per esempio per lo sviluppo Web un solution stack e' solitamente costituito da 4 elementi: sistema operativo, web server, database, e linguaggio di programmazione.

#### LAMP

Uno dei solution stack piu' noti e' LAMP, in cui:
- Linux e' il sistema operativo
- Apache e' il sever web
- Il DBMS e' MySQL
- Il linguaggio di programmazione comunemente e' PHP ma vengono usati anche Perl e Python

Tutto il solution stack e' completamente opensource.

### PHP

PHP e' un linguaggio di scripting, interpretato originariamente concepito per la programmazione di pagine web dinamiche lato server.
- L'interprete PHP e' un software libero distribuito sotto la PHP License.

Il client fa una richiesta al server per una pagina PHP, il sever esegue il codice contenuto il quella pagina, che produce il output una pagina HTML, che poi invia al client.

# Set di Caratteri

Viene utilizzato lo standard UTF che sfrutta una codifica a lunghezza variabile, in questo modo e' possibile visualizzare anche i documenti codificati in ascii e Latin 1, con alcune eccezioni.