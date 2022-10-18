# Introduzione 03/10/2022
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

# Modello a strati  12/10/2022
## Banda
Tra sorgente e destinazione, la quantità di informazione che può essere trasmessa varia dal tipo di collegamento:
- singolo link: la quantità è definita __banda di trasferimento__. La velocità dipende, ovviamente, dal percorso che compie il collegamento, dove può essere soggetta a diminuzione a causa di diversi fattori.
- multripli link: la velocità si determina a __bottleneck__ ovvero, si abbassa per tutti i collegamenti a quello con la manda più limitata.

La velocità di trasmissione end-to-end, che viene effettivamente utilizzata (e che varia da quella teorica) è detta __throughput__.

- - -
__nb__: ogni link dispone di un canale per il download e uno per l'upload. Per le diverse tecnologie, i due canali possono avere o non, la stessa velocità di trasmissione.
- - -

## Comunicazione fisica e logica
Nella comunicazione tra due entità, bisogna fare una distinzione:
- comunicazione fisica: si tratta della trasmissione dei bit di informazione.
- comunicazione logica: si tratta della trasmissione dell'informazione.

L'approccio utilizzato per la trasmissione porta ad un __modello a livelli__, dove ogni livello esegue delle operazioni di elaborazione e comunica con gli altri (adiacenti) mediante un protocollo.
Il __protocollo__ è un insieme di regole che stabiliscono come deve avvenire una comunicazione, dal formato dei messaggi e intestazione, al comportamento e le azioni da intraprendere.
- - -
__nb__: un livello o strato adiacente viene detto "peer".
- - -

## Modelli storici
Importante è il modello teorico __ISO/OSI__ (internationl organization for standardization, open system interconnection). Esso è composto da 7 livelli differenti, ogniuno con un compito differente:
- applicazione: interfaccia utente.
- presentazione: standardizza le informazioni.
- sessione: controlla la comunicazione tra le applicazioni.
- trasporto: gestisce la connessione tra sorgente e destinatario.
- rete: individua il percorso del pacchetto.
- collegamento dati: effettua controllo degli errori e delle perdite di segnale.
- fisico: trasmette il flusso di dati.

Solo i sistemi della sorgente e di destinazione finale possiedono tutti e 7 i livelli, mentre i sistemi intermedi solo quelli di rete, collegamento dati e fisico, in quanto è necessario conoscere solo le informazioni del pacchetto per trasferirlo, non il contenuto del messaggio.

Questo modello teorico è stato sostituito da uno pratico, il modello __TCP/IP__ (trasmission control protocol, ip internet protocol). L'unica variazione è che il livello di trasporto va ad inglobare quelli di sessione e presentazione.

- - -
__nb__: l'insieme dei protocolli di un architettura di rete è chiamato "stack protocollare".
- - -
### Entità
L'entità è il processo avviato dalla macchina, e viene identificata mediante l'indirizzo del calcolatore (indirizzo IP) e l'identificatore del processo (porta).

Quindi un flusso di comunicazione viene identificato univocamente dalla trupla (IPA, IPB, portaA, portaB), dove il numero di porta si trova tra le informazioni dell'header del livello di trasporto, mentre l'indirizzo IP nell'header di rete.

### Indirizzo IP
Un indirizzo IP è composto da 32 bit, separati a blocchi di 8. Ogni blocco corrisponderà, se tutti i bit sono a 1, a 255 ( 2^8 -1 perchè si parte da 0 ovviamente).

# 19/10/2022