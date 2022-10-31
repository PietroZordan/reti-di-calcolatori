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
__Esempio__: il pachetto viene inviato al calcolatore che nell'header del livello di rete, possiede l'IP interessato. Successivamente il livello di trasporto individua il processo specifico attraverso la porta.
- - -

### Indirizzo del calcolatore
Un indirizzo IP è composto da 32 bit, separati a blocchi di 8 per convenzione (universale). Ogni blocco corrisponderà, se tutti i bit sono a 1, a 255 ( 2^8 -1 perchè si parte da 0 ovviamente). I 32 bit sono separati logicamente in 2 porzioni: prefisso (rete) e suffisso (utente nella rete).

- - -
__Esempio__: Verona, parlando di numeri fissi, ha prefisso 045, mentre Padova ha 049. Se a Verona un utente ha il numero "045.803.1459", a Padova sicuramente esiste "049.803.1459"
- - -

# Indirizzo IP 19/10/2022
## Struttura indirizzo
I bit che saranno dedicati all'indirizzo IP dipendono dalla grandezza della rete.
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
Il processo di suddivisione della rete (e del suo indirizzo) in più sottoreti è detto __CIDR__ (classless inter domain routing).

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
|applicativo|     |
|trasporto|       |
|rete   |rete     |
|datalink|datalink|
|fisico |fisico   |
- - -
Quando un processo (istanza di un'applicazione) vuole comunicare con un processo di un altra macchina, deve conoscere l'indirizzo IP e la porta. La tupla, già definita in precedenza (IP sorgente, IP destinazione, porta sorgente, porta destinazione) è detta __socket__. Andiamo ora a identificare 2 tipi di processi:
- processo server: processo che gira su un host, sempre raggiungibile (si spera), e che ha un indirizzo IP fisso.
- processo client: processo responsabile dell'apertura della comunicazione, e che ha un indirizzo IP dinamico.
Il server è quindi il processo sempre in ascolto, mentre il client è il processo che manda la richiesta di comunicazione (appunto è lui a realizzare la comunicazione). Proprio per questo il server ha un indirizzo fisso, perchè il client deve poterlo conoscere.
Al client viene assegnato un indirizzo IP appunto dinamico, ed è indipendente dall'effettivo indirizzo del dispositivo.

- - -
__nb__: gli indirizzi dinamici in genere verranno forniti da un server DHCP.

__nb__: un'applicazione web sfrutta il protocollo HTTP.
- - -
L'applicazione utilizza il livello di trasporto per inviare i messaggi. Il livello di trasporto offre 2 tipologie di servizio:
- connection oriented, affidabile (non accetta perdite di dati).
- connectionless, non affidabile (può accettare delle perdite).

# Applicazione Web 21/10/2022
## Protocollo HTTP
Il server mantiene una pagina Web, e il client, interessato a ricevere la pagina, invia la richiesta verso il server per quello determinata pagina.
La __pagina web__ è una collezione di documenti o file:
- file che contiene la struttura della pagina (.html).
- altri elementi (immagini e video).
- script e stylesheet.

A grandi linee, la comunicazione tra client e server è la seguente:
1. Il server resta in attesa.
2. Il client manda la richiesta della pagina, al server, eseguendo la ricerca sul browser.
3. Il server invia la risposta, negativa o positiva.
4. Il client manda a livello applicativo, un messaggio detto __get__.
5. Il server "scinde" il file (solo .html) in pacchetti, che invia al client.
6. Il client ricompone i pacchetti, e ottiene il file.
Per ricevere gli altri file della pagina web, ci sono 2 soluzioni:
- connessione persistente: vengono mandati ulteriori GET in parallelo, per ogni file, nella stessa connessione.
- connessione non persistente: la connessione viene chiusa, e viene ripetuta la procedura per ogni file, ma le connessioni vengono aperte in parallelo.

- - -
__nb__: HTTP (hyper text transfer protocol).

__nb__: i punti 2,3,4 fanno parte dell'apertura della connessione TCP.

__nb__: il protocollo HTTP è un protocollo testuale.
- - -

### Messaggi di richiesta e risposta
Ogni messaggio di richiesta è composto dalla __riga di richiesta__ e da una o più __righe di intestazione__.
La riga di richiesta, serve appunto per richiedere la risorsa al server, ed userà i vari metodi messi a disposizione della classe HTTP: GET per richiedere una risorsa, POST per inviare le informazioni, DELETE...
La righe di intestazione sono invece delle etichette contenenti delle informazioni utili per la risorsa richiesta.

Il server, quando riceve una richiesta HTTP, è sempre obbligato a mandare una risposta. In questo caso, il messaggio sarà composto da __riga di stato__, righe di intestazione e la risosrse richiesta. Come già detto, in qualsiasi caso il server deve rispondere, e lo può fare con i seguenti codici (nella riga di stato) a seconda della situazione: 200, 400, 404, 405.

- - -
__Esempio__: www.univr.it/index.html -> "richiesta"= GET /index.html http/1.1  "intestazione"= Host:www.univr.it User-agent: mozzilla/4.0

__Esempio__: "stato"= HTTP/1.1 200 descrizione "intestazione"= ...

__Esercizio__: IP: 140.120.84.20 /20  -> Ind.rete: 140.120.80.0 /20

__Esercizio__: 165.5.1.0 /24 -> 165.5.1.192  _128  _64 /26

__Esercizio__: 165.5.1.0 /25  165.5.1.128 _64 /26
- - -

# Potocolli HTTP e di posta elettronica 26/20/2022
## Cookie
Permettono di capire (al server) se c'è già stata un interazione in precedenza tra un server e un client specifico.
In una prima interazione, quando il client invia la richiesta, il server associa all'utente un numero identificativo, che lo invia nella risposta in una riga di intestazione "set-cookie: ". Il client può decidere se memorizzare l'ID o meno.
In una richiesta successiva, il client invierà il codice in una riga di intestazione "cookie: ", e in base ad esso, il server agirà di conseguenza, con una risposta adatta all'utente in particolare.

## Caching
Durante una comunicazione, è presente un server __cache__ che riceve le richieste del client e le risposte del server. Se un qualunque client, invia un richiesta che è già stata posta alla cache, essa invierà la risposta del server, che aveva memorizzato in precedenza. Questo permette di ridurre notevolmente il traffico. Ci sono vari livelli di chaching: rete locale, browser, ISP. La percentuale con cui la chace soddisfa le richieste, è detta __hit ratio__.

### Get condizionale
C'è la possibilità che il server aggiorni una propria pagina, e quindi la copia della pagina contenuta della cache non sarebbe aggiornata. Per sviare a questo problema nella richiesta GET viene aggiunta la riga di intestazione "if-modified-since: ", che permette alla cache di capire, se può usare la copia di risposta che essa contiene.
Quindi in realtà, la comunicazione con il server è sempre presente, ma i messaggi non contengono i dati della risposta.

## DNS
Il server DNS è locale alla rete, e permette di tradurre i nomi logici nei corrispondenti indirizzi IP. Il DNS opera prima che la connessione tra client e server comincino la connessione, intercettando la richiesta del client, e traducendo all'indirizzo del server il nome logico del sito. Per "memorizzare" queste associazioni, viene usato un __sistema distribuito generico di server DNS__:
- Server DNS Root.
- Server DNS TLD (top level domain): contiene le informazioni relative ai domini principali.
- Server DNS locale.
In questa gerarchia, un server locale può andare ad individuare a quale IP corrisponde un certo nome logico, richiedendolo ai server TLD e Root.
Vedia ora una cominicazione generica, in cui un cliente invia la richiesta del sito "www.site.org":
1. Il client invia la richiesta www.site.org al DNS locale della rete.
2. Il DNS locale, non trovando nessuna corrispondenza, manda la richiesta ad un server Root.
3. Il Root invia in risposta il dns per il server TLD di dominio ".org".
4. Il DNS locale allora richiede al server TLD .org.
5. Il server TLD invia al server locale, il DNS locale di "site.org".
6. Il server locale richiede l'IP per "www.site.org" al locale di "site.org".
7. Il server locale di "site.org" invia l'IP a server, che poi lo passa al client.
Le associazioni più vecchie verranno eliminate, in quanto la memoria è limitata.

- - -
__Esempio__: grazie al cookie il server potrebbe rispondere in future interazioni, mostrando all'utente elementi già cercati in precedenza.

__nb__: DNS (domain name system).

__Esempio__: il DNS traduce "www.univr.it" nel indirizzo ip del server.

__Esempio__: www.site.org "site" è il dominio di un server locale, mentre "org" è il dominio di gerarchia superiore, contenuto nel proprio server TLD.
- - -

## Protocolli di posta elettronica
Per la prosta elettronica, ci sono 2 protocolli principali, __SMTP__ e __IMAP__, rispettivamente per l'invio e la ricezione dei messaggi.
Entrambi sono protocolli di tipo client/server, e si appoggiano a quello TCP, che è affidabile e connection oriented.
Ogni dominio avrà un server di posta elettronica, ed esso contiene una __coda__ (buffer) per i messaggi in uscita, e delle __caselle__ ognuna per ogni utente del dominio. L'architettura di un dominio di posta elettronica può essere rappresentata con:
- utente client del dominio (nome.cognome@dominioPostaElettronica.dominio).
- server posta elettronica del dominio (@dominioPostaElettronica.dominio).
La comunicazione tra 2 utenti di dominio differente, può essere rappresentata con:
1. Utente del dominio @dom1.it, manda una mail ad un altro utente del dominio @dom2.it.
2. Essendo il dominio diverso da quello del server, la mail viene messa nella coda di uscita del server.
3. La mail viene spedita al server @dom2.it e inserita nella casella dell'utente.
4. Per visualizzare la mail, attraverso il protocollo IMAP (e a quello HTTP) l'utente accede alla propria posta elettronica.

- - -
__nb__: SMTP (simple mail transfer protocol) e IMAP ().

__nb__: quando il server invia il messaggio al server di un'altro dominio, svolge il ruolo di client.

__nb__: SMTP è un protocollo testuale.

__nb__: per la ricezione è usato anche il protocollo HTTP (POP è deprecato).

__Esercizio__: 101.75.79.255 101.75.80.0  Lan1:/21 Lan2:1000host Lan3:/23 Lan4:400host Lan5:bloccoIndirizzi/2
- - -

## Livello di trasporto
Si occupa di dividere il messaggio in pacchetti, aventi ognuno un header contenente informazioni utili, come ad esempio l'ordine con cui i pacchetti vanno assemblati una volta arrivati a destinazione. Possono essere utilizzati 2 protocolli, ovvero TCP e __UDP__.

dns e posta elettronica da controllare nella rec.

# TCP nel livello di trasporto 28/10/2022
## Formato dei messaggi

L'header nel TCP è composto da 20 byte, ed è composto dai seguenti campi suddivisi in righe da 32 bit ciascuna:
1. campo porta sorgente e campo destinazione (2+2 byte).
2. campo sequence number.
3. campo acknowledge number.
4. campo offset (4 bit), reserved (6 bit), flag (6 bit), window (2 byte).
5. campo checksum, urgent pointer.

### Campi porta
Sono campi identificativi dei processi sorgenti e destinazione, coinvolti nello scambio di informazioni.
Le porte possono essere __statiche__ o __dinamiche__. Le prime sono associati ad applicazioni __lato server__ definite da standard, le seconde sono assegnati dal OS ai processi __lato client__.

### Campo sequence number
Le applicazioni generano messaggi di lunghezza arbitraria (molto variabile). Ai livello più bassi ( fisico e datalink), viene definita dalla scheda di rete una dimensione massima __MTU__ che deve avere il pacchetto. Questa dimesione è conosciuta dal TCP, e gli permette di definire una __MSS__ e quindi la segmentazione del messaggio in pacchetti. Il sequence number è quindi un identificativo per ricostruire l'ordine dei pacchetti.

### Campo acknowledge number
Tenendo conto che il TCP è un protocollo affidabile, viene usata la tecnica __positive acknowledge with retrasmission__. Viediamo un'ipotetica comunicazione tra hostA e hostB:
1. A manda il primo pacchetto.
2. B manda in risposta un acknowledge o __ack__, per riferire che è arrivato il pacchetto.
3. A manda i seguenti pacchetti nello stesso modo.

### Campo checksum
Controlla che non ci siano errori nel pacchetto arrivato. Per farlo, prende il pacchetto (escluso il checksum, essendo lui) e attraverso una particolare funzione che prende un input, che indipendentemente dalla dimensione restituisce un output di dimensione fissa:
1. A manda il pacchetto a B.
2. B esegue appunto la funzione da cui ottiene l'output.
3. B confronta il checksum con l'output.
Se checksum e output sono diversi c'è sicuramente un errore, in caso contrario n on è detto che sia primo di errori.
- - -
__Esempio__: la porta statica per HTTP è la porta 80. Per SMTP è 25.

__nb__: ricorda che una comunicazione è definita da (ipC, portaC, ipS, portaS).

__nb__: la porta del client è dinamica, mentre quella del server è statica, visto che deve stare sempre in ascolto.

__nb__: le porte dinamiche sono >= 1024.

__nb__: MTU (maximum trasmission unit).

__nb__: MSS (maximum segment size).

__nb__: l'ack è un header TCP che non trasporta dati.
- - -

## Gestione delle connessioni
Essendo il TCP connection oriented, aperta la connessione, prima di scambiare i dati vengono scambiati dei parametri. I due processi coinvolti si scambiano una serie di header TCP, vediamo generalmente quali:
1. Messaggio di SYN (client): porte sorgente e destinazione, sequence number e __flag syn__ a 1.
2. Messaggio di SYN-ACK (server): porte sorgente e destinazione (invertite), sequence number, acknoledge number,__flag ack__ e flag syn a 1.
3. Messaggio di ACK (client): porte sorgente e destinazione, flag di ack a 1.
Il campo sequence number del massaggio di SYN, contiene un valore scelto casualmente e usato come riferimento per la ricezione successiva.
- - -
__nb__: in fase di apertura della connessione, nelle opzioni viene inserito il valore della MSS. Il messaggio si adatterà alla MSS minore.

__nb__: nel messaggio di SYN viene usato un sequence number casuale, per questioni di sicurezza.
- - -