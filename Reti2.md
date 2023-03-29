# Multicast
Non esiste diretta corrispondenza fra tipologia di canale e tipologia di servizio -> lo stesso servizio multicast puo' essere implementato con: canali broadcast, canali punto a punto o architettura mista.

## TDM
### Slotted
- Il tempo varia in modo prestabilitio per periodi di tempo prefissato
- Le unita' informative hanno tutte la stessa lunghezza commisurata al singolo slot
#### Slotted Framed
- Slot strutturati in trame (frame)
- Si sincronizza la trama con una parola di allineamento
#### Slotted Unframed
- Gli slot si susseguono senza trama
- E' necessaria la sincronizzazione per ogni slot
### Unslotted
- Il tempo non e' suddiviso a priori
- Le unita' informative possono avere lunghezza variabile

### Assegnazione della banda
#### Statica
- Ottima per slotted framed
- Flusso informativo = banda dedicata (bit/sec)
- La banda non puo' cambiare a comunicazione in corso
#### Dinamica
- Molti flussi condividono liberamente la banda in base alle necessita'
- La banda puo' cambiare a comunicazione in corso
- La richiesta complessiva di banda puo' diventare intollerabile (congestione)

S-TDM -> Synchronous Static Time Division Multiplexing

## Funzioni di rete
- Trasmissione -> trasferimento fisico del segnale
- Commutazione -> instradamento delle informazioni
- Segnalazione -> scambio delle informazioni necessarie per la gestione della comunicazione e della rete stessa
- Gestione -> mantenimento delle funzioni della rete