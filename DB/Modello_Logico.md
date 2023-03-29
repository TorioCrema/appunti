### Traduzione delle entita'
- Ogni entita' e' tradotta con una relazione con gli stessi attributi.
- La chiave primaria coincide con l'identificatore principale dell'entita'.
- Gli attributi composti vengono ricorsivamente suddivisi nelle loro componenti, oppure sono mappati in un singolo attributo della relazione, il cui dominio deve seere oppurtunamente definito.

### Traduzione di associazioni
- Ogni associazione e' tradotta con una relazione con gli stessi attributi, cui si aggiungono gli identificatori di tutte le entia' che essa collega.
- La ==chiave== dipende dalle cardinalita' massime delle entia' nell'associazione.
	_E' importante rispettare i vincoli espressi dallo schema ER._
- Le ==cardinalita' minime== determinano, a seconda del tipo di traduzione effettuata, la presenza o meno di ==valori nulli==.

---
Inizio la traduzione dalle entita' che hanno identificatore interno e partecipano ad associazioni con `max-card = N`, dato che non avro' bisogno di importare alcun identificatore.