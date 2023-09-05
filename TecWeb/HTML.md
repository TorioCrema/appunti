# HTML 5

## Markup

Un linguaggio di markup e' un linguaggio (con una specifica sintassi) che consente di annotare un documento fornendone una interpretazione delle sue parti.

### Tipi di markup

Procedurale/descrittivo: 
- i linguaggi di markup di tipo procedurale indicano le procedure di trattamento del testo, il markup specifica le istruzioni che devono essere eseguite per visualizzare la porzione di testo referenziata (es. TEX)
- i linguaggi di markup di tipo descrittivo identificano strutturalmente il tipo di ogni elemento del contenuto. Invece di specificare effetti grafici come l'allineamento o l'interlinea, ne individuano il ruolo all'interno del documento, per esempio specificando che un elemento e' un titolo, un paragrafo, o una citazione.

### XML

Definizione di documenti strutturati:
XML e' ideale come linguaggio per definire documenti strutturati o semi strutturati, e per esprimerli in maniera indipendentemente dalla loro destinazione finale.
XML permette di definire e utilizzare elementi e attributi personalizzati definendo in questo modo nuovi linguaggi di markup specifici.

XML fornisce una sintassi con cui e' possibile specificare gli elementi e gli attributi che possono essere utilizzati all'interno di particolari classi di documenti.
Ulilizzando XML e' possibile creare un modello, chiamato Document Type Definition (DTD), che descrive la struttura e il contenuto di una classe di documenti.

#### Well-formed vs Valid

In XML si possono generare documenti:
- Well-Formed conformi alle generiche specifiche XML. Occorre solo definire un file XML che le rispetti.
- Valid conformi ad una specifica DTD. Occorre definire un file XML e la sua DTD. La DTD puo' essere sia interna (nel file XML) che esterna (in un altro file). Il documento appartiene ad una classe di documenti, definiti dalla specifica DTD. Un documento valido deve essere sempre e comunque ben formato.

### Metadati

Gli elementi metadatazione consentono di descrivere il documento specificandone caratteristiche, comportamento, presentazione, relazioni con altri documenti.
I metadati sono inclusi nell'head del documento:
- \<title\>
- \<base\>
- \<link\>
- \<meta\>
- \<style\>

#### \<title\>

L'elemento \<title\> rappresenta il titolo o il nome del documento.
Deve essere scritto tenendo conto che potrebbe essere usato fuori dal contesto (come per esempio accade nei bookmark) ovvero senza avere corredo il contenuto del documento
Il documento deve avere il massimo un elemento \<title\>

#### \<base\>

L'elemento \<base\> deve essere unico all'interno del documento se ci sono piu' elementi \<base\>, viene preso in considerazione solo il primo.
Lo scopo principale dell'elemento \<base\> e' quello di indicare il path base del documento che servira' per risolvere gli URL relativi, sia in termini di href che di target.

#### \<link\>

L'elemento \<link\> viene usato per creare relazioni tra il documento e altri documenti o risorse.
Ha molti utilizzi ma quello principale e' creare la rilazione con il CSS usato dal documento.

#### \<style\>

L'elemento style permette di includere stili all'interno del document.
Il redering del documento sara' il risultato dei \<link\> a fogli di stile, degli elementi \<style\> e degli eventuali stili inline a cui si aggiungono gli stili dell'utente.

#### \<meta\>

I \<meta\> vengono usati per aggiungere altri metadati al documento. Sono spesso usati dai motori di ricerca.
Il tipo di metadati e' specificato dall'attributo name. In particolare il meta description e' usato per lo sniplet da Google.

## Tag e Attributi

I tag sono il markup che aggiungiamo al contenuto per dare struttura, enfasi, per definire il ruolo che late contenuto ricopre all'interno del documento web.
I tag possono essere corredati di uno o piu' **attributi**, che servono per meglio specificare la funzione o la tipologia dell'elemento, per memorizzare dati o per arricchire di significato il contenuto.

### Attributi

Sono coppie nome-valore separate dal carattere "=".
I valori sono racchiusi tra virgolette "".
Si scrivono lasciando uno spazio dopo il nome del tag di apertura.

## Elementi di blocco e elementi inline

**Elementi di blocco**: il loro comportamento di default nella finestra del browser e' quello di essere preceduti e seguiti da una andata a capo, sono nativamente rappresentati come un box.
**Elementi inline**: sono contenuti in un elemento di blocco e non ne intaccano il flusso, ovvero non implicano l'andata a capo, ne' prima ne' dopo.

### Sectioning

Gli elementi della categoria Sectioning hanno funzione strutturale, ovvero dividono la pagina in parti con semantica (ruolo) diverso a seconda dell'elemento usato:
- \<article\>
- \<aside\>
- \<figcaption\>
- \<figure\>
- \<footer\>
- \<header\>
- \<nav\>
- \<section\>

#### \<section\>

Definisce una sezione del documento. Nella recommandation HTML5 del W3C: "A section is a thematic grouping of content, typically with a heading."

#### \<article\>

Definisce informazioni indipendenti e auto-contenute: Un articolo dovrebbe essere un elemento con un suo senso proprio, che potrebbe essere letto in modo indipendente dal resto della pagina web.

#### \<header\>

Definisce l'intestazione di un documento o di una sua sezione. Puo' essere usato come contenitore di un contenuto di tipo introduttivo. Possono essere presenti piu' \<header\> in ogni pagina web.

#### \<footer\>

Definisce il footer di un documento o di una sua sezione. Solitamente un footer contiene le informazioni sull'autore del documento, le informazioni sul copyright, link ai termini d'uso, informazioni sui contatti, ecc. Possono essere presenti piu' \<footer\> in ogni pagina web.

#### \<nav\>

Definisce un insieme di link di navigazione (menu' o toolbar o altri set di link).

#### \<aside\>

Definisce un contenuto a latere rispetto a quelli principali (ma comunque correlati).
Puo' essere utilizzato per contenere i contenuti di una barra laterale (sidebar), ma anche per contenuti (separati da quello principale). che sono collaterali, ma non si posizionano necessariamente a lato (per esempio citazioni o banner pubblicitari).

## Heading

Gli heading introducono i titoli delle diverse sezioni del documento:
- vanno da \<h1\> a \<h6\> a seconda della loro rilevanza (rank)
- \<h1\> e' quello con maggiore rank e deve rappresentare il titolo principale della sezione.
- I titoli di rank inferiore devono intestare sottosezioni

Sezioni e heading si possono far corrispondere in molti modi diversi, preferibilmente:
- usare sezioni esplicite (sempre con elemento di sezione e titolo, mai solo il titolo)
- far combaciare il grado dell'intestazione con il livello di nidificazione previsto.

## Phrasing

Nella categoria phrasing rientra il contenuto che rappresenta il testo del documento:
- \<p\>: L'elemento inserisce un paragrafo testuale. L'andata a capo non basta, va utilizzato solo quando non esiste un elemento piu' specifico o semanticamente piu' idoneo per descrivere quel testo.
- \<br\>: rappresenta un interruzione di linea (line break), va scritto con \\, deve essere usato solo per interruzzioni che sono effettivamente parte del contenuto, come negli indirizzi, nelle poesie o nel codice.
- \<div\>: elemento di blocco, non ha alcun significato proprio ma ha lo scopo di rappresentare gli elementi in esso annidati e specificare per loro gli attributi *class, lang e title*. Viene usato soprattutto per definire l'attributo class, ovvero dare a un gruppo di elementi consecutivi uno stesso stile di presentazione. La caratteristica presentazionale non deve avere una connotazione semantica perche' in questo caso dovrebbe essere usato un elemento con semantica.
- \<span\>: opera in modo simile all'elemento \<div\> ma a livello di testo (e' un elemento inline). Non ha alcun significato proprio ma rappresenta il testo in esso contenuto e specifica per esso gli attributi *class, lang e title*.
- \<main\>: raggruppa gli elementi di struttura (come \<section\> o \<article\>) che rappresentano il contenuto principale del documento: il contenuto deve essere caratterizzante di quel documento, quindi vanno esclusi in contenuti che sono ripetuti in diverse pagine, ogni documento deve avere un solo \<main\>.
- \<a\>
- Altri elementi che definiscono il ruolo del testo come: \<blockquote\>, \<q\>, \<cite\>, \<i\>, \<b\>, \<strong\>, \<em\>, \<small\>, \<abbr\>, \<code\>, \<dfn\>, \<var\>, \<sub\>, \<sup\>, \<ins\>, \<del\>

## Embedded

Il contenuto Embedded ha lo scopo di importare risorse o contenuto dentro il documento. Gli embedded fanno riferimento all'area dei sistemi multimediali. Alcuni elementi embedded prevedono un contenuto fallback, che viene usato quando la risorsa esterna non puo' essere utilizzata.

### \<figure\> \<figcapion\>

\<figcaption\> definisce la didascalia di una immagine.
\<figure\> raggruppa l'immagine e la sua didascalia.

### \<img\>

Le immagini inline sono definite attraverso l'elemento \<img\>.
Gli attributi obbligatori sono:
- **src**: specifica l'URL del file contenente l'immagine. L'URL deve fare riferimento a una "non interactive, optionally animated, image resourse that is neither paged nor scripted". Sono consentiti i formati PNG, GIF, JPEG.
- **alt**: testo alternativo, viene visualizzato in caso il browser non riesca a mostrare l'immagine e in caso di immagini disabilitate. E' indispensabile agli utenti non vedenti.

Gli attributi opzionali sono:
- **usemap**: specifica che l'immagine e' una mappa lato client
- **ismap**: specifica che l'immagine e' una mappa lato server
- **width**: specifica la larghezza dell'immagine in pixel
- **height**: specifica l'altezza dell'immagine in pixel
- **longdesc**: URL che porta alla descrizione lunga per l'accessibilita'

### \<video\>

L'elemento \<video\> definisce un modo standard per includere un video in una pagina web. Prima di HTML5 non esisteva uno standard per mostrare i video nelle pagine web, che potevano essere mandati in play solo con un plug-in.
- **controls**: aggiunge i controlli per il video: play, pausa, volume, ecc.
- **width**: definisce la larghezza del video, se non specificato il valore di height (altezza), viene calcolato mantenendo le proporzioni originali del video.
- Il testo contenuto in \<video\>\<\/video\> viene mostrato nel caso in cui il browser non supporti l'elemento.

### \<audio\>

L'elemento \<audio\> definisce un modo standard per includere un audio in una pagina web. Prima di HTML5 non esisteva uno standard per includere gli audio. Stessi attributi di video.

### \<object\>

Supportato da tutti i browser. Definisce un oggetto incluso in un documento HTML. Usato per includere audio, video, animazioni e plug-in (come applet Java, lettori PDF, player Flash) nelle pagine web. In modo simile a source, ammete versioni alternative dell'oggetto.

### \<embed\/\>

Supportato dalla maggior parte dei browser, e' un elemento vuoto (non ha chiusura).
Definisce un oggetto incluso in un documento HTML, come \<object\>. 

### \<canvas\>

Una importante innovazione di HTML5 e' la possibilita' di disegnare direttamente sulla pagina. L'elemento \<canvas\> fornisce le API necessarie per la generazione e il rendering dinamico di grafica, diagrammi, immagini e animazioni. L'elemento \<canvas\> definisce un'area rettangolare in cui disegnare direttamente immagini bidimensionali e modificare in relazione a eventi, tramite funzioni Javascript.

## Mappe

Una mappa e' un immagine in cui alcone arre sono interattive, attivano un link o altre azioni.
Una mappa puo' essere realizzata:
- client-side: il browser esamina la locazione del click e attiva/gestisce l'interazione
- server-side: il server esamina la locazione del click e attiva/gestisce l'interazione

Esistono molti tipi di mappe che servono a fornire servizi avanzati di ricerca e localizzazione.
In HTML esiste anche un semplice tipo di mappe lato client che puo' essere realizzato attraverso markup.

## Link

### \<a\>

L'elemento \<a\> consente di inserire ancore nel documento, ovvero punti di partenza di un link.
- la destinazione si specifica con un URI attraverso l'attributo **href**.

## Risorsa

Una risorsa e' qualunque struttura che sia oggetto di scambio tra applicazioni all'interno del web. In concetto di risorsa e' indipendente dal meccanismo di memorizzazione effettiva o dal tipo di contenuto.
Anche se molti identificatori fanno riferimento a file memorizzati in un file system gerarchico, questo tipo di risorsa non e' l'unico. Una risorsa potrebbe essere:
- in un file system relazionale
- in un database, e l'URI essere la chiave di ricerca
- il risultato dell'elaborazione di un'applicazione, e l'URI essere i parametri di elaborazione
- una risorsa non elettronica
- un concetto astratto

### URI

Gli URI (Uniform Resourse Identifier) sono una sintassi usata in WWW per definire i nomi e gli indirizzi di oggetti (risorse) su internet.
- Gli URI risolvono il problema di creare un meccanismo ed una sintassi di accesso unificata alle risorse di dati disponibili via rete.
- Questi oggetti sono considerati accessibili tramite l'uso di protocolli esistenti, invetati appositamente, o ancora da inventare.
- Tutte le istruzioni d'accesso ai vari specifici oggetti disponibili secondo un dato protocollo sono codificate come una stringa di indirizzo.

Sono definiti come:
- Uniform Resourse Locator (URL): una sintassi che contiene informazioni immediatamente utilizzabili per accedere alla risorsa (ad esempio, il suo indirizzo di rete)
- Uniform Resourse Names (URN): una sintassi che permetta una etichettatura permanente e non ripudiabile della risorsa, indipendentemente dal riportare informazioni sull'accesso.

Un URI e' diviso in: `schema:[// authority] path [? query] [# fragment]`
- Lo schema (negli URL e' il protocollo) e' identificato da una stringa arbitraria (ma registrata presso IANA) usata come prefisso.
- L'authority individua un'organizzazione gerarchica dello spazio dei nomi a cui sono delegati i nomi (che possono essere ulteriormente delegati)
	- E' a sua volta divisa in: `[userinfo @] host [: port]`
	- La parte userinfo non deve essere presente se lo schema non prevede identificazione personale
	- La parte host e' o un nome di dominio o un indirizzo IP
	- La port puo' essere omessa se ci si riferisce ad una well-known port
- La parte path e' la parte identificativa della risorsa all'interno dello spazio di nomi identificato dallo schema e (se esistente) dalla authority.
- La parte query individua un'ulteriore specificazione della risorsa all'interno dello spazio di nomi identificato dallo schema, di solito usati per specificare un risultato dinamico
- La parte fragment individua una risorsa secondaria.

#### URI reference

Un URI assoluto contiene tutte le parti predefinite dal suo schema, esplicitamente precisate.
Un URI gerarchico puo' pero' anche essere relativo, (in questo caso e' detto URI reference) e riportare solo una parte dell'URI assoluto corrispondente, tagliando progressivamente cose da sinistra.
Un URI reference fa sempre riferimento ad un URI di base.
Un URI gerarchico puo' anche essere relativo (detto tecnicamente un URI reference) ed in questo caso riportare solo una parte dell'URI assoluto corrispondente, tagliando progressivamente cose da sinistra.
Un URI reference fa sempre riferimento ad un URI di base (ad esempio, l'URI assoluto del documento ospitante l'URI reference rispetto al quale fornisce porzioni differenti).

## Flow

### Tabelle

Sono realizzate tramite l'elemento \<table\>, organizzate per righe, realizzate attraverso l'elemento \<tr\>, table row, ciascuna riga e' poi divisa in celle.
Le celle sono possono essere:
- Celle normali, in questo caso sono rese dall'elemento \<td\>, table data
- Celle di intestazione, che invece sono realizzate con l'elemento \<th\>, table header

Intestazione orizzontale:
```html
<table>
	<tr>
		<th>Month</th>
		<td>January</td>
		<td>February</td>
	</tr>
	<tr>
		<th>Savings</th>
		<td>$100</td>
		<td>$80</td>
	</tr>
</table>
```
Intestazione verticale:
```html
<table>
	<tr>
		<th>Month</th>
		<th>Savings</th>
	</tr>
	<tr>
		<td>January</td>
		<td>$100</td>
	</tr>
	<tr>
		<td>February</td>
		<td>$80</td>
	</tr>
</table>
```

#### \<caption\>

Aggiunge il titolo ad una tabella:
```html
<table>
	<caption>Monthly savings</caption>
	<tr>
		<th>Month</th>
		<th>Savings</th>
	</tr>
	<tr>
		<td>January</td>
		<td>$100</td>
	</tr>
	<tr>
		<td>February</td>
		<td>$80</td>
	</tr>
</table>
```

#### rowspan e colspan

Una cella di tipo \<td\> o \<th\> puo' occupare piu' righe o piu' colonne utilizzando rispettivamente l'attributo rowspan e colspan:
```html
<table>
	<caption>Monthly savings</caption>
	<tr>
		<th>Month</th>
		<th>Savings</th>
	</tr>
	<tr>
		<td>January</td>
		<td>$100</td>
	</tr>
	<tr>
		<td>February</td>
		<td>$80</td>
	</tr>
	<tr>
		<td colspan="2">Sum: $180</td>
	</tr>
</table>
```

#### headers

Una cella di tipo \<td\> o \<th\> puo' fare riferimento (tramite l'attributo **headers**), ad altre celle, per specificare che queste rappresentano una intestazione della cella corrente:
- Lo scopo di questo sistema di relazioni tra celle e' quello di supportare gli screen reader usati dalle persione non vedenti nel riferire correttamente alle celle intestazione di una certa cella.
- headers deve avere come valore la lista degli id delle intestazioni per la cella separati da spazio.

### Liste

Tre tipi di liste:
- Liste non ordinate, definite da \<ul\><\/ul\>
- Liste ordinate, definite da \<ol\>\<\/ol\>
- Liste di definizioni, definite da \<dl\>\<\/dl\>

Nelle prime due ogni itme e' definito da \<li\>\<\/li\>, per liste di definizioni due elementi che specificano termine e definizione: \<dt\> e \<dd\>

Nelle liste ordinate possone essere specificati gli attributi:
- start: il valore iniziale della numeraizone
- type: il tipo di numerazione utilizzata
- reversed: la numerazione e' inversa

Le liste posono essere annidate.

## Interactive

### \<form\>

Una form e' una parte della pagina web che contiene controlli di input (form control), come per esempio campi di testo, bottoni e checkbox.
Le form sono realizzate dall'elemento \<form\>.
In HTML5 la form puo' anche avere campi per l'output.
- Puo' essere processata a lato client (via Javascript).
- Puo' essere processata a lato server.

Gli attributi di form sono:
- **action**: specifica l'URL dell'applicazione server-side che ricevera' di dati.
- **method**: specifica il metodo HTTP che deve essere usato per i dati, puo' essere:
	- **GET**: richiede di processare i dati a una specifica applicazione del server
	- **POST**: sottomette (invia) i dati da processare ad una specifica applicazione server.

Gli attributi comuni dei controlli sono:
- **name**: specifica il nome del controllo, usato anche nella sottomissione della form.
- **required**: e' un attributo booleano che indica se e' obbligatorio inserire un valore per quel campo.
- **autofocus**: mette il focus su questo controllo.
- **autocomplete**: e' un attributo booleano di input che consente di attivare l'autocompletamento. Puo' essere messo anche a livello di form.

#### GET

Richiede di processare i dati a una specifica applicazione server.
- Invia dati testuali, aggiungendo una query string alla URL richiesta. I dati, tutti testuali, sono visibili nella URL.
- La query string e' separata dalla URL da `?` ed e' composta da coppie nome=valore separate tra loro da `&` (`+` sostituisce gli spazi).
- Si puo' usare per mettere una chiamata a server tutta nascosta nell'HTML (parametri inclusi).
- Rimane in cache, resta nella history, puo' andare nei bookmark.
- **Non adatta**: per dati troppo voluminosi, non testuali o che e' opportuno non vedere.

#### POST

Sottomette (invia) i dati da processare ad una specifica applicazione server.
- Non ha restrizioni su dimensione e tipo di dati che vengono inviati al server. In particolare puo' inviare anche dati di tipo `multipart/form-data` incapsulando formati binari.
- I dati sono inviati nel body del messaggio HTTP e non sono visibili, quindi e' adatto al passaggio di dati riservati.
- Quando non necessario, pel l'utente e' piu' scomodo di GET perche' non rimane in cache, non resta nella history e non puo' andare nei bookmark.
- Con HTTP non e' sicuro.

#### \<label\>

Ogni controllo deve avere una \<label\> che descriva il controllo all'utente:
- La presenza di \<label\> e' fondamentale per l'accessibilita', se non si vuole che sia visibile, va resa invisibile ma senza rimuoverla.
- La label puo' essere associata al controllo:
	- Annidando il controllo nella \<label\>:
```html
<form>
	<p>
		<label>Customer name:
			<input/>
		</label>
	</p>
</form>
```

- Mettendo un id nel controllo e relazionando alla \<label\> mediante l'attributo for:
```html
<form>
	<p>
		<label for="CN">Customer name: </label>
		<input id="CN"/>
	</p>
</form>
```

#### \<fieldset\>

L'elemento \<fieldset\> raggruppa piu' controlli che hanno semantica comune.
```html
<form>
	<fieldset>
		<legend>Dati personali:</legend>
		<label>Nome: <input type="text" name="nome"/></label><br/>
		<label>Email: <input type="email" name="mail"/></label><br/>
		<label>Data di nascita: <input type="date" name="date"/></label>
	</fieldset><br/>
	<input type="submit" value="Invia"/>
</form>
```

#### \<input\>

L'elemento \<input\> consente di inserire nella pagina molti tipi diversi di controllo (alcuni introdotti da HTML5).
- Il tipo e' specificato mediante l'attributo **type** che puo' assumere i valori **hidden, text, search, tel, url, email, password, date, time, number, range, color, checkbox, radio, file, submit, image, reset, button**.

#### reset, submit e button

- Il valore **reset** per l'attributo **type** genera un bottone per ripristinare i valori di default della form.
- Il valore **submit** per l'attributo **type** genera un bottone per sottomettere la form.
- Il valore **button** per l'attributo **type** genera un bottone a cui puo' essere assiociato uno script Javascript attraverso l'attributo **onclick**.
- In tutti e tre gli elementi, attraverso l'attributo **value** viene modificato il testo sul bottone.

#### \<image\>

Il tipo **image** ha la stessa funzione di un bottone.
- L'immagine e' usata per la resa grafica.
- Va inserito l'elemento alt per rendere accessibile il bottone.
- Invia le coordinate del click (x e y) al server.

#### hidden

Gli input di tipo **hidden** sono inseriti nella form per creare una associazione variabile valore invisibile all'utente.
In controllo di \<input\> con type="hidden" non viene visualizzato.

#### password

Gli input di tipo password sono inseriti nella form per creare una assiociazione variabile valore invisibile all'utente.
Quando l'utente riempie il controllo, la visualizzazione della password viene oscurata.

#### radio

Gli input di tipo **radio** realizzano un radio button. Le diverse opzioni sono definite inserendo piu' radio con lo stesso **name**.

#### checkbox

Gli input di tipo **checkbox** realizzano checkbox. Le diverse opzioni possibili sono definite inserendo piu' checkbox con lo stesso **name**.

#### text

Gli intup di tipo **text** sono utilizzati come controlli per l'input di testo (generico). Per tipi particolari di testo (per esempio mail, colori o date), esistono controlli specifici.

#### search

Il tipo **search** consente di inserire testi che diventano chiavi di ricerca.
- Come nel tipo testo, si puo' inserire un testo qualunque (senza pattern specifici).
- In alcuni browser il campo e' visualizzato in modo da evidenziare la sua funzione.

#### color

Il tipo **color** consente di scegliere un colore, attraverso un color picker.

#### Data e ora

Per codificare data e ora sono disponibili vari tipi di input:
- **time**: orario
- **week**: settimana dell'anno
- **date**: data
- **datetime**: data e orario
- **datetime-local**: data e oriario locali

#### tel, url, email

Per inserire dati personali sono previsti tipi specifici per **tel, url** e **email**.

#### number e range

Tipi di controllo per i numeri sono:
- **number**: consente di inserire un numero
- **range**: consente di scegliere in un range

Per entrambi si possono definire:
- **min**: specifica il valore minimo
- **max**: specifica il valore massimo

#### file

Il tipo **file** consente di selezionare un file:
- Possono essere caricati piu' file ma il **name** deve essere senza path.
- Esiste uno stato che consente di monitorare l'upload.

#### \<textarea\>

L'elemento \<textarea\> definisce un controllo di input per testi multilinea.
Gli attributi **cols** e **rows** specificano la dimensione della \<textarea\>.

#### \<select\> e \<option\>

L'elemento \<select\> e' usato per realizzare menu' a tendina.
Le diverse opzioni sono introdotte attraverso l'elemento \<option\>.
```html
<form>
	<label>Scegli un insegnamento<br/>
		<select name="insegnamento">
			<option value="SO">Sistemi Operativi</option>
			<option value="TW">Tecnologie Web</option>
			<option value="SM">Sistemi Multimediali</option>
		</select>
	</label>
</form>
```

Gli attributi per \<select\> sono:
- **multiple**: booleano per effettuare selezioni multiple
- **size**: definisce le opzioni da mostrare all'utente

Gli attributi per \<option\> sono:
- **selected**: booleano e' true se quella opion e' il valore di default, altrimenti la prima option e' il default. Se si vuole un default nullo, si deve aggiungere una \<option\> vuota.
- **value**: indica il valore quando diverso dal testo della opzione, per esempio un codificato o abbreviato.

#### \<optgroup\>

L'elemento \<optgroup\> e' usato per raggruppare le \<option\> in una \<select\>.
```html
<form>
	<label>Scegli un insegnamento<br/>
		<select name="insegnamento">
			<optgroup label="2014/15">
				<option value="SO">Sistemi Operativi</option>
				<option value="TW">Tecnologie Web</option>
			</optgroup>
			<optgroup label="2015/16">
				<option value="SM">Sistemi Multimediali</option>
			</optgroup>
		</select>
	</label>
</form>
```

#### \<datalist\>

L'elemento \<option\>, insieme all'elemento \<datalist\> puo' essere usato per predisporre dei suggerimenti per i valori di un campo testuale.
```html
<form>
	<label>Scegli un insegnamento<br/>
		<input list="corsi" name="insegnamento"/>
	</label>
	<datalist id="corsi">
		<option value="SO">Sistemi Operativi</option>
		<option value="TW">Tecnologie Web</option>
		<option value="SM">Sistemi Multimediali</option>
	</datalist>
	<input type="submit" value="Invia richiesta"/>
</form>
```

#### \<output\>

L'elemento \<output\> rappresenta il risultato di un calcolo o azione dell'utente.