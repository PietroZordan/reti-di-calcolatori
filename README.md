# Introduzione 05/10/2022
## Comunicazione
La prima proprietà di cui è giusto a parlare è la comunicazione tra 2 entità (o calcolatori).
La comunicazione dipende da:
### Protocollo
Insieme di regole formali che definiscono le modalità di comunicazione tra 2 entità.
### Architettura di rete
Si tratta dell'infrastruttura fisica che permette la comunicazione, quindi dove viaggiano le informazioni che le entità si scambiano.
- - -
__Esempio__: se A vuole scrivere un messaggio a B:
- A deve usare un linguaggio che B conosca.
- A inserisce il messaggio in una busta, che punta all'indirizzo di B. Il formato del messaggio deve essere comprensibile da B
- il messaggio non viene inviato direttamente a B, ma viaggia attraverso l'infrastruttura di rete, insieme ad altri messaggi.
- in base all'indirizzo, il messaggio arriva al destinatario B.
- - -

## Architettura di rete
Una rete è un insieme di elementi collegati tra loro. Un architettura presenta i seguenti elementi base: __calcolatori o end-host__, __router o intermediate-host__, __collegamenti__.
- - -
__Esempio__: la LAN solitamente è composta da vari end-host, collegati tra loro attraverso lo switch. Lo switch è collegato al Router.
  Le varie LAN si collegano tra loro attraverso i propri router, che si connettono ad una rete definita "backbone".
  Ogni rete è gestita autonomamente dell' ISP (internet service provider). I vari ISP comunicano tra loro, per permettere di comunicare a utenti con diversi ISP.
  
nb: l' ISP può essere di livello 1 (internazionale), 2 (nazionale), 3 (locale).
nb: un componente può disporre di un access point.
- - -

# Trasferimento 07/10/2022
## Modalità di trasferimento
Può avvenire in 2 modalità, di cui la prima deprecata:
- __Reti a commutazione di circuito__: le risorse del canale trasmissivo, sono interamente dedicate alla comunicazione tra i 2 host. All'inizio della comunicazione viene instaurato il circuito, e il canale sarà sempre disponibile per la comunicazione dei 2 host. Questo ne deriva un ritardo costante, ma uno spreco di risorse (nei momenti di attesa).
- __Reti a commutazione di pacchetto__: le informazioni vengono suddivise in pacchetti, ai quali viene aggiunta un'intestazione. I vari pacchetti vengono contenuti all'interno di un contenitore FIFO detto __buffer__, e vengono inviati al buffer del router, che li riceve uno ad uno, nell'ordine di arrivo. In questo caso l'utilizzo delle risorse è eccellente, ma aumenta il ritardo.
L'alternarsi dei pacchetti di host diversi, che il router riceve e trasmette, è il motivo per cui non vengono sprecate risorse, e si dice __multiplazione statistica__. Un problema di questa modalità è che essendo il buffer limitato, scarterà eventuali pacchetti in più (che non può contenere).

## Ritardo
Visto che prima di iniziare l'invio di un pacchetto, il router deve aspettare di averlo ricevuto completamente, andrà a generare un ritardo variabile. Il ritardo dipende da diversi fattori:
- numero di apparati.
Per ogni hop devo inoltre tenere conto di:
- __elaborazione__ e interpretazione dell'intestazione.
- __accodamento__ di pacchetti di eventuali altri host.
- dimensione del pacchetto e velocità di __trasmissione__.
- capacità di __propagazione__ dell'apparato di trasmissione, e tempo che ne deriva.

In base al collegamento, il ritardo assume i valori:
- locale/nazionale: 10 ms.
- internazionale: 30/40 ms
- intercontinentale: >100 ms

### Stima del ritardo
Il valore del ritardo può essere stimato, ma non con precisione ottima (può dipendere da vari fattori), mediante 2 strumenti:
- PING: l'host mittente invia dei pacchetti (echo request), e riceve dal destinatario (echo reply) una risposta che gli permette di calcolare il tempo di trasmissione, e viene detto __RTT__ (round trip time).
- Traceroute: questa modalità di messaggio e risposta, avviene ad ogni apparato, in questo modo è possibile anche individuare il percorso.
