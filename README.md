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
L'entità è il processo avviato dalla macchina (calcolatore), e viene identificata mediante l'indirizzo del calcolatore (indirizzo IP) e l'identificatore del processo (porta). Quindi i calcolatori comunicano quando i loro processi comunicano.

Quindi un flusso di comunicazione viene identificato univocamente dalla trupla (IPA, IPB, portaA, portaB), dove il numero di porta si trova tra le informazioni dell'header del livello di trasporto, mentre l'indirizzo IP nell'header di rete.

- - -
__Esempio__: individua ip e porta
- - -

### Indirizzo del calcolatore
Un indirizzo IP è composto da 32 bit, separati a blocchi di 8 per convenzione (universale). Ogni blocco corrisponderà, se tutti i bit sono a 1, a 255 ( 2^8 -1 perchè si parte da 0 ovviamente). I 32 bit sono separati logicamente in 2 porzioni: prefisso (rete) e suffisso (utente nella rete).

- - -
__Esempio__: Verona, parlando di numeri fissi, ha prefisso 045, mentre Padova ha 049. Se a Verona un utente ha il numero "045.803.1459", a Padova sicuramente esiste "049.803.1459"

# Indirizzo IP 19/10/2022
## Struttura indirizzo
I bit che saranno dedicati all'indirizzo IP dipendono dalla rete.
Solitamente per notazione il numero di bit del prefisso viene indicato dopo l'IP.
In base ai bit dell'indirizzo di rete, possiamo individuare il numero di host che la rete può contenere.
- - -
__Esempio__: numero di bit del prefisso "157.27.12.63 /16"
__Esercizi__:
  1) 11100111.11011011.10001011.01101111 -> 231.218.139.111
  2) 221.34.255.82 -> 11011101.00100010.11111111.01010010
__Esempio__: una rete con prefisso /20, avrà 2 elevato a 32-20 host (4096).
__Esempio__: per conoscere il proprio indirizzo ip e la maschera di rete, si può digitare da shell ipconfig.
__nb__: se utilizziamo il comando "whois < indirizzo ip >" ci verrà restituito lo stesso indirizzo, ma con i bit degli host a 0.  Inoltre con ipconfig abbiamo 16 bit di rete, mentre 20 con whois. Questo è dovuto al subnetting, quindi whois ci restituisce l'indirizzo rispetto alla subnet in cui si trova.
- - -

## Indirizzo di rete
Nei 32 bit di indirizzo, possiamo individuare quelli di rete grazie alla __net mask__, ovvero una stringa di 32 bit dove saranno messi a 1 i bit dedicati alla rete, metre a 0 quelli didicati agli host. Anche la maschera può essere scritta in notazione decimale. Se ho 16 bit di rete, avrò "255.255.0.0". A volte viene rappresentata anche in notazione esadecimale.
### Subnetting
Concetto che permette la divisione di una rete in più sottoreti, ciascuna avente il priprio router di bordo. Per suddividere in sottereti un indirizzo:
1. convertiamo in notazione binaria l'indirizzo (157.27.0.0 /16 -> 10011101.00011011.00000000.00000000).
2. non potendo modificare la parte di rete, bisogna scomporre i bit degli host. Per dividere in 2 sottoreti, semplicemente aggiungerò all'indirizzo di rete il bit di host più significativo, che sarà 0 per la sottorete A, 1 per la sottorete B (entrambe saranno /17).
3. convertiamo ora in notazione decimale gli indirizzi (Rete A: 157.27.0.0 /17   Rete B: 157.27.128.0 /17).

Per semplificare il concetto, viene prima verificato l'indirizzo della rete, per raggiungere il router principale, che poi individuerà la destinazione attraverso il/i bit di sottorete.
Questo metodo utilizzato per la suddivisione dell'indirizzo IP  è il __CIDR__ (classless inter domain routing).

- - -
__nb__: precedentemente veniva usato un metodo che fosse classfull, ovvero basato su classi, dove se il primo bit di indirizzo era 0, allora i bit destinati alla rete erano solo i primi 8, se il secondo bit è 0 allora sono 16 di rete. Stessa cosa anche per il 3° bit. Questi indirizzi venivano rispettivamente classficati in A, B, C.
__nb__: l'ultimo indirizzo di una rete (tutti gli host a 1), per convenzione viene assegnato all'indirizzo di broadcast.
- - -

### Indirizzi riservati
Gli indirizzi ip riservati sono indirizzi che non possono essere assegnati a host. I principali sono:
- indirizzo di rete, che avrà i bit degli host tutti a 0.
- directed broadcast, che avrà i bit degli host tutti a 1.
- tutti i bit a 0.
- tutti i bit a 1.

## Livello applicativo (TCP/IP)
Abbiamo detto che nel modello TCP/IP, abbiamo 2 apparati:
- - -
|End    |Tramiti  |
|-------|---------|
|apllicativo|     |
|trasporto|       |
|rete   |rete     |
|datalink|datalink|
|fisico |fisico   |
- - -
Quando un processo (istanza di un'applicazione) vuole comunicare con un processo di un altra macchina, deve conoscere l'indirizzo IP e la porta. La tupla, già definita in precedenza (IP sorgente, IP destinazione, porta sorgente, porta destinazione) è detta __socket__. Andiamo ora a identificare 2 tipi di processi:
- processo server: processo che gira su un host, sempre raggiungibile (si spera), e che ha un indirizzo IP fisso.
- processo client: processo responsabile dell'apertura della comunicazione, e che ha un indirizzo IP dinamico.

L'applicazione utilizza il livello di trasporto per inviare i messaggi. Il livello di trasporto offre 2 tipologie di servizio:
- connection oriented, affidabile.
- connectionless, non affidabile.

### Applicazione WEB (protocollo HTTP)
