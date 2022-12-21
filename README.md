# Reti di calcolatori
# Zordan Pietro 2022/2023
## Indice:

### Introduzione 03/10/2022

### Trasferimento 07/10/2022

### Indirizzo IP 19/10/2022

### Applicazioni Web 21/20/2022

### Potocolli HTTP e di posta elettronica 26/20/2022

### TCP nel livello di trasporto 28/10/2022

### Protocollo TCP 02/11/2022
- __Suddivisione in pacchetti__
- __Chiusura della connessione__
- __Controllo di flusso__
### Congestione 04/11/2022
- __Finestra di congestione__
- __Controllo di congestione__
### Protocollo IP 09/11/2022
- __Livello di rete__
- __Frammentazione IP__
### Routing 11/11/2022
- __Instradamento__
- __Algoritmi distance vector__
### Routing 16/11/2022
- __Tabella di routing__
- __Bellman Ford__
- __Inter/intra isp__
### Server DHCP 18/11/2022
- __Hot potato routing__
- __DHCP__
### Limitatezza degli indirizzi IPv4 23/11/2022
- __ICMP__
- __NAT__
- __IPv6__
### Livello data-link 25/11/2022
- __Next-header__
- __Accesso al mezzo condiviso__
### Tecniche dinamiche a contesa 02/12/2022
- __Aloha__
- __CSMA__
### Indirizzi logici e fisici 07/12/2022
- __Ethernet__
- __ARP__
- __Lan estese__





- - -
- - -

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

# Protocollo TCP 2/11/2022
## Comportamento del protocollo
### Suddivisione in pacchetti
Grazie al sequence number, nella suddivisione del messaggio in pacchetti, è l'offset rispetto l'inizio, in termini di byte. Quindi per il primo pacchetto, sarà 0 + ISN. L'ISN verrà dato all'inizio della condizione, e sarà un numero casuale.
Con un MSS di 1200 byte, il pacchetto sarà:
- da 0 a 1199 byte.
- il sequence number sarà 0 + ISN.
Il pacchetto successivo invece:
- da 1200 a 2399 byte.
- il sequence number sarà 1200 + ISN.

Mentre l'acknowledge number è il prossimo byte (che sarà il primo byte del pacchetto atteso) che il destinatario si aspetta di ricevere.

Se il flag __push__ è a 1, bisogna forzare il pacchetto per arrivare a destinazione.

Il protocollo TCP è un protocollo di  tipo __stream__.

### Chiusura della connessione
Dopo aver aperto la connessione, avviene lo scambio di dati, e successivamente inizia la fase di chiusura della connessione. Anch'essa avviene con uno scambio di header TCP, e può avvenire da entrambi gli host in maniera indipendente:
- il client invia un messaggio di FIN, dove sarà a 1 il flag __FIN__.
- il server risponderà con un messaggio di ACK, e trasmette i dati che deve trasmettere.
- il server invierà il suo messaggio di FIN, a cui il client risponderà con l'ACK.
Se invece il server non ha niente da inviare, invierà direttamente un messaggio di FIN-ACK.

### Affidabilità del protocollo
I pacchetti trasmessi arriveranno sempre alla destinazione. Al ricevimento di un pacchetto, l'host risponde con un acknoledge number, ma i pacchetti potrebbero andare persi turante il tragitto.
Se l'host sorgente non riceve conferma che il pacchetto è arrivato, attende il tempo RTO, e reinvia i dati. Avviene lo stesso anche nel caso venga perduto il messaggio di conferma.
- - -
__nb__: ISN (initial sequence number).

__nb__: l'apertura di connessione con messaggi di SYN e ACK, è detta "three way handshake".

__nb__: lo scenario di chiusura della connessione può avvenire anche al contrario.

__nb__: RTO (retrasmission time out). Viene calcolato in base all'attesa di risposta dei messaggi precedenti (media del RTT). RTO = betha SRTT attuale.

__nb__: RTT (round trip time). Si tratta del tempo che intercorre tra messaggio e risposta.

__nb__: SRTT attuale= alpha SRTT precedente + (1-alpha) RTT instantaneo. Alpha è 7/8 per lo standard. Betha è 2 per lo standard.
- - -

## Trasmissione dei pacchetti
La velocità di trasmissione dipende dell'RTT, infatti "1/RTT" in quanto mando un segmento ad ogni RTT. Se l'RTT è grande la trasmissione sarà molto lenta. Si potrebbe inviare una serie di pacchetti invece di uno singolo, ma si andrebbe a creare della __congestione di rete__, andando a riempire il buffer, risultando in perdita di pacchetti. La quantità di trasmessi che conviene trasmettere ci viene data da 2 azioni:
- controllo di flusso: azione preventiva che limita la quantità di dati immessi nella rete. Viene definità una dimensione detta "di finestra", e tale dimensione detta la dimensione della serie di pacchetti da inviare prima  di ricevere il riscontro. Questo permette una velocità di trasmissione di "finetra/RTT". La finestra si sposta solo se il pacchetto da confermare più longevo, viene riscontrato. L'umento della finestra avviene secondo due algoritmi, __slow start__ e __congestion avoidance__, mentre la sua diminuzione mediante due modalità, __vanilla__ e __fast retrasmit/ fast recovery__.
- controllo di congestione: reazione in caso di congestione.

I pacchetti non sempre arrivano però nell'ordine con cui vengono inviati, inoltre alcuni potrebbero persino andar persi. Dopo aver inviato un segmento, viene avviato per esso l'RTO, e verrà reinviato il pacchetto se non arriva riscontro.

- - -
__nb__: in un messaggio di riscontro, viene specificato quale pacchetto è atteso, ma anche che non è atteso il pacchetto che si sta riscontrando.

__nb__: la dimensione della finestra non è fissa, ma tenderà ad aumentare col tempo.
- - -

# Congestione 04/11/2022
## Algoritmi window
### Aumento della finestra di trasmissione
La finestra di congestione, oltre a scorrere lungo i pacchetti, aumenta di dimensione. Per farlo utilizza i seguenti algoritmi:
- slow start: per ogni riscontro ricevuto, aumento la finestra di un pacchetto, oltre a farla scorrere di un pacchetto. Caratterizzato da un'evoluzione esponenziale nel tempo.
- congestion avoiance: per ogni riscontro ricevuto, aumento la finestra di 1/w (w è il precedente valore della finestra). Caratterizzato da un'evoluzione lineare nel tempo.

### Diminuizione della finestra
Se non viene ricevuto un riscontro di un pacchetto inviato, allo scadere del RTO, la dimensione della finestra verrà ridotta, e lo farà secondo le seguenti funzionalità: 
- vanilla: la finestra viene posta a dimensione 1.
- fr/fr: la dimensione della finestra viene dimezzata.
I segmenti persi verranno poi ritrasmetti.

## Controllo di congestione del TCP
Nel controllo di congestione, l'algroritmo necessita di 4 variabile da tenere conto:
- CWND: dimensione attuale della finestra di congestione.
- RTO: calcolato dinamicamente in base all'RTT istantaneo.
- RCVWND: indica la finestra massima di ricezione.
- SSTHRESH: dimensione della finestra oltre la quale non posso usare lo slow start.

La prima fase, è l'__inizializzazione__ delle variabili:
- CWND = 1.
- RTO = valore calcolato durante l'apertura della connessione.
- RCVWND = valore comunicato dalla destinazione.
- SSTHRESH iniziale = RCVWND iniziale (in alcune implementazioni si usa RCVWND /2).

La seconda fase, è quella dell'algoritmo vero e proprio:
1. invia un numero di segmenti, pari alla CWND.
2. quando arrivano i riscontri:
  - CWND < SSTHRESH: uso lo slow start, CWND = minimo(CWND+ackNumber , RCVWND , SSTHRESH).
  - CWND > SSTHRESH: uso il congestion avoidance, CWND = minimo(CWND+(ackNumber/CWND) , RCVWND).
  A questo punto si torna al punto 1, con la nuova CWND.
3. se non arrivano i riscontri, e quindi è scaduto l'RTO, pongo la "SSTHRESH = CWND/2" e:
  - vanilla: CWND = 1.
  - fr/fr: CWND = SSTHRESH.
  Per i segmenti che vado a ritrasmettere, porrò "RTO = 2*RTO".

- - -
__nb__: CWND (congestion window).

__nb__: RCVWND (receive window). Viene contenuto nel campo window dell'header TCP.

__nb__: SSTHRESH (slow start threshold).
- - -

## Protocollo UDP

Il protocollo è connectionless non affidabile, chiaramente sarà provvisto di una struttura notevolemente più semplice del TCP.
### Header del messaggio
Anche in questo caso, l'header è suddiviso in righe da 32 bit:
- porte sorgente e destinazione.
- lunghezza  e checksum.

- - -
__nb__: UDP (user datagram protocol)
- - -

# Protocollo IP 09/11/2022
## Livello di rete
A differenza dei livelli di applicazione e trasporto, il livello di rete è presente in tutti gli apparati, non solo negli "end-system". Non dovendo preocuparsi di gestire eventuali perdite di dati (se ne occupa il livello di trasporto), il livello di rete deve fare il possibile per far arrivare il pacchetto a distenazione. Non è comunque garantito che il pacchetto arrivi a destinazione, in quanto il livello di rete è un livello __best effort__, ovvero fà il possibile, ma non garantisce nulla.

### Formato dei messaggi
Quando il livello di rete riceve i pacchetti dal livello applicativo, gli aggiunge il suo header. Oltre all'header, il pacchetto viene inserito dentro una "busta esterna" detta __payload__, a cui l'header si attaccherà. Quando il pacchetto verrà inviato, i vari router leggeranno solo l'header IP.
L'header dei messaggi del protocollo IP, che di base è di 20 byte, è diviso in righe da 32 bit e così strutturato:
- VERS (4 bit), linghezza dell'header (4 bit), service type(8 bit), lunghezza totale (16 bit).
- Frammentazione: indetificazione (16 bit), flags (3 bit), fragment offset (13 bit).
- TTL (8 bit), type (8 bit), checksum (16 bit).
- Indirizzo IP sorgente.
- Indirizzo IP destinazione.
- Eventuali opzioni.

Il TTL è necessario, in quanto si possono verificare dei problemi al routing, detti __routing loop__.
Il checksum verrà fatto sull'header e non sul pacchetto, per essere sicuri che non contenga errori.

- - -
__nb__: IP (internet protocol).

__nb__: il livello di rete si occuperà del dialogo tra macchine adiacenti. 

__nb__: VERS è la versione del protocollo utilizzato. Attualmente possediamo la v4. Esiste però una v6 che dovrebbe prima o poi rimpiazzarla.

__nb__: la lunghezza dell'header è espressa in byte. Se non ci sono opzioni, è lunga 20 byte. La lunghezza totale è invece la somma dei byte dell'header e del payload.

__nb__: Service type è un codice che identifica la classe di servizio. La classe di servizio servirà per stabilire delle file di priorità all'interno dei buffer che ricevono i pacchetti.

__nb__: TTL (time to live), è un contatore inizializzato dalla sorgente (inizializzato solitamente a 64 o 128), che identifica il numero massimo di hop che il pacchetto può attraversare. Ogni router attraversato decrementa di uno il valore del TTL, e se un router riceve un pacchetto con TTL=1 e che quindi riduce a 0, scarta il pacchetto inviando un messaggio di errore alla sorgente.

__nb__: il campo Type è un codice che identifica il protocollo trasportato nel payload. Permette ai router di capire che tipo di pacchetto stanno trasportando. Per esempio potrebbe essere utile come politica di scarto dei pacchetti, se il pacchetto ha protocollo TCP avrà più importanza di un con UDP, e nel caso sarò quest'ultimo ad essere scartato.
- - -

### Frammentazione IP
La frammentazione dei pacchetti avverà in base al valore di MTU, che indica la dimensione massima del pacchetto che la scheda di rete può trasmettere. L'MTU avrà un valore di partenza, che varierà col percorso lungo i router. I valori che acquisirà sono contenuti nelle interfaccie di uscita dei vari router.
Nel caso in cui l'MTU di uno dei router, e minore di quello precedente, il pacchetto verrà spezzato in frammenti con la dimensione del MTU del router. Ogni frammento sarà indipendente, e quindi deve avere un header che differirà di frammento in frammento per alcuni campi, per far capire alla destinazione che sono frammenti. Ad esempio Ip sorgente e destinazione saranno uguali in tutti gli header. Vediamo il campo di frammentazione:
- identificazione: numero progressivo dato dalla sorgente ad ogni pacchetto IP.
- flag "M", o more fragment: se 1 esistono altri frammenti, se 0 non è stato frammentato o è l'ultimo frammento.
- offset: posizione del frammento rispetto al pacchetto originario, in termini di "byte/8".

I frammenti verranno poi riassemblati alla destinazione. Una volta ricevuto il primo frammento, partirà un timer (250-500 ms), entro il quale dovranno arrivare gli altri frammenti, altrimenti verrà tutto scartato.

- - -
__nb__: la frammentazione non ha niente a che vedere sulla segmentazione a livello di trasporto.

__nb__: MTU (maximum tranfer unit).

__Esempio__: MTU di partenza di 5000byte, pacchetto generico di 4000byte + 20byte header ip. Il pacchetto non essendo frammentato avrà Id a 803, offset e flag-M a 0. Sul percorso incontra una MTU di 1420byte (1400 payload, 20 header), che lo frammenta in 3 frammenti da 1420byte ciascuno. I frammenti avranno:
1. id: 803, m-flag: 1, offset: 0.
2. id: 803, m-flag: 1, offset: 175 (1400/8).
3. id: 803, m-flag: 0, offset: 350 (2800/8).
- - -

# Routing 11/11/2022
## Consegna indiretta
In una rete locale, gli utenti generalmente si connetteranno con il router mediante uno __switch__. Quando un host deve comunicare con un altro, recupera il suo indirizzo ip (attraverso il dns per esempio) e controlla il suo prefisso, per verificare che coincida con il proprio. In tal caso vuole dire che appartengono alla stessa rete. Ipotizzando di avere tre utenti A e B della stessa rete, e C, dobbiamo distinguere la comunicazione in:
- consegna diretta: se A vuole comunicare con B, che fa parte della sua stessa rete, può farlo senza passare dal router, passando dallo switch.
- consegna indiretta: se A vuole comunicare con C, invierà il pacchetto al router di default, che lo inoltrerà alla destinazione.

## Instradamento
La consegna indiretta introduce il concetto di __routing__ ovvero il processo di scoperta del cammino migliore, da una sorgente al tutte le possibili destinazioni. Con "cammino migliore" si intende quello più conveniente, in base a differenti criteri come la distanza da parcorrere, la velocità di trasmissione.
Per risolvere il "problema decisionale" del routing, si adotta una "tecnica" di astrazione con grafi, dove i nodi sono i router, mentre gli archi sono i link tra i router.
Si può dire quindi che il routing è il calcolo del cammino minimo su un grafo, ed esistono due classi di algoritmi che ci permettono di calcolarlo:
- vettore distanza (distance vector).
- stado dei collegamenti (link state).

## Algoritmi Distance vector
Si basano sul __criterio di consistenza__ ovvero, una porzione di cammino minimo è il cammino minimo tra i nodi che determinano tale posizione. La decisione del cammino, non si base sull'intera tipologia della rete, infatti ogni nodo conosce solo il costo del proprio vicino, e il costo che il vicino gli comunicherà per andare al vicino successivo. In parole povere, partendo dalla destinazione i nodi comunicano ai propri vicini il costo per raggiungerla.

- - -
__nb__: il router della propra LAN viene detto di dafult, useremo questa notazione d'ora in poi.

__nb__: la distanza da percorrere si può misurare in router attraversati, ma anche la distanza vera e propria in termini di km, quindi la lunghezza del link tra un router ed un altro. 

__nb__: agli archi dei grafi, possiamo associare un peso, che ne va a descrivere la caratteristica che vogliamo enfatizzare o studiare nel grafo. Il costo del cammino sarà la somma dei costi degli archi che lo compongono.
- - -

# Routing 16/11/2022
## Criterio di consistenza
Ogni cammino è composto da più sottocammini, in base ai nodi e gli archi che attraversa. Il cammino per andare da un nodo A ad un nodo F può presentare un nodo C in esso, dividendo il cammino in 2 porzioni, dove la somma dei pesi darà quella del cammino A-F.

## Tabella di routing
Si tratta di una tabella che contiene per ogni destinazione, il next hop e il costo per raggiungere tale destinazione. La destinazione e il costo sono anche i due valori del distance vector.
Un router conosce solo i propri vicini, e durante la fase di __inizializzazione__, se il vicino diretto coincide con la destinazione, il router porrà "D(i,v) = c(i,v)" altrimenti "D(i,w) = infinito". Periodicamente un router manda ai propri vicini il proprio distance vector, con il quale loro aggiorneranno la propria tabella di routing.

## Algoritmo Bellman-Ford distribuito
Si può dimostrare che, se il grafo è completamente connesso e i pesi degli archi sono maggiori di 0, l'algoritmo distance vector converge alla soluzione ottimo. Il numero di iterazione necessarie per la convergenza è proporzionale al diametro del grafo, inteso come il percorso minimo, più lungo da percorrere da un nodo all'altro. Quindi questo algoritmo permette di computare appunto questo percorso minimo più lungo, rispettando le condizioni specificate sopra.

### Algoritmi Link State
Al contrario degli algorimto distance vector, utilizzano un approccio __centralizzato__ rispetto al router, ovvero ogni router ha a disposizione l'intera tipologia della rete (quindi conosce tutta la struttura del grafo, pesi compresi), e può quindi calcolare autonomamente i cammini minimi.

## Routing intra/inter - ISP
I vari ISP sono collegati tra di loro attraverso dei router di bordo (un po' come  i router di default delle LAN), router che possono essere considerati esterni. Gli algoritmi distance vectore e link state, vengono utilizzati per calcolare i cammini minimo all'interno dell'ISP (intra-ISP), mentre per gestire il routing al di fuori dall'ISP (inter-ISP) è stato definito un protocollo un unico protocollo, distinto dalle due categorie, il __BGP__. La tabella di routing di un ISP sarà formata da destinazioni che si trovano all'interno dello stesso ISP, e altre che si trovano all'esterno.

- - -
__nb__: ogni router possiede una tabella di routing.

__nb__: D(i,v): partenza i, destinazione v. In questo caso "v" è il vicino diretto.

__nb__: c(i,w): partenza i, destinazione v. In questo caso "w" è un'altra destinazione.

__nb__: infinito viene utilizzato in base al numero di hop, se troppo alto.

__Esempio__: avendo
- A --> B = 8
- A --> E = 4
- E --> B = 2
Quando A riceverà da B ed E i distance vector, aggiornerà la tabella di routing, e per la destinazione B, metterà E come next-hop perchè il costo è minore. Non verrà fatto solo per i vicini, ma col tempo per tutte le possibili destinazione.

__nb__: il distance vector può essere implementato da vari protocolli: RIP, IGP...

__nb__: un esempio di algoritmo centralizzato è quello di Dijkstra.

__nb__: in qualsiasi tipologia di algoritmo, in caso di guasti, serve del tempo perchè l'informazione si propaghi. Durante tale intervallo le tabelle di routing potrebbero essere errate e si potrebbero formare dei __routing loop__.

__nb__: BGP (border gateway protocol).

__nb__: oltre ad intra/inter-ISP, può essere utilizzata la dicitura intra/inter-AS.
- - -

# Server DHCP 18/11/2022
## Routing inter-AS
Due ISP possono comunicare mediante più router di bordo, in modo da smaltire il traffico di dati in maniera efficiente e omogenea. All'interno della rete dell'ISP, grazie al routing si calcolerà quale router di bordo conviene raggiungere (in base al costo) per comunicare con l'esterno. Quindi nel caso una destinazione sia raggiungibile da più router di bordo, essi annunceranno che passando da loro sarà possibile raggiungerla, e viene adottata la soluzione __hot potato routing__ in cui viene scelto il router di bordo più vicino alla sorgente (con sorgente si intende il router che deve instradare il pacchetto).
Visto che le destinazione inter-AS potrebbero essere estremamente numerose per essere memorizzate nella tabella di Routing, le destinazioni cercano di compattare gli indirizzi, poi con una successiva fase di __subnetting__ sarà possibile separare il blocco.

## Protocolli di supporto
### DHCP
Gli indirizzi IP appartengono alla rete, e vengono assegnati di volta in volta quando un host si collega alla rete, dinamicamente dal __server DHCP__ della rete. I messaggi che si scambiano per l'assegnamento dell'indirizzo sono i seguenti:
1. inizialmente l'host si assegna l'indirizzo IP "0.0.0.0". Ipotiziamo per il server "157.27.10.2".
2. l'host manda un messaggio in broadcast (che raggungerà ovviamente anche il server), composto da un header IP e dal payload che conterrerrà il protocollo DHCP. Questo messaggio è chiamato __DHCP discover__. L'header sarà composto dalle seguenti righe:
- campo type: contiene un codice specifico per il DHCP, in modo da poter capire il protocollo contenuto nel payload. Solo il server DHCP in questo modo aprirà il pacchetto.
- 32 bit a 0, l'attuale indirizzo dell'host.
- 32 bit a 1, che permette di indicare che il messaggio è di broadcast.
All'interno del payload, e quindi del protocollo DHCP, c'è un campo in particolare "transaction id" che conterrà un numero casuale scelto dall'host.
3. il server risponderà con un messaggio, composto sempre da un header IP e dal protocollo DHCP. Questo messaggio è chiamato __DHCP offer__. L'header sarà composto dalle stesse righe del messaggio di richiesta, ma con l'indirizzo del server al posto di "0.0.0.0". Nel payload sarà contenuto lo stesso "transaction id", più altri due campi, uno contenente l'indirizzo del server, e uno il nuovo indirizzo per l'host.
4. l'host invia in broadcast un altro messaggio, chiamato __DHCP request__, dove l'header sarà uguale al "DHCP descover" mentre il payload conterrà i campi dell'ip del server, e dell'ip offerto.
5. per finire il server invierà un messaggio chiamato __DHCP ack__, con l'header uguale a quello del "DHCP offer" e nel payload l'indirizzo del server, e quello offerto all'host.

Ora l'host può utilizzare l'indirizzo IP. Il tempo per cui il client avrà a disposizione l'IP è limitato, ed è detto __lease__, allo scadere del quale il client dovrà richiederne il rinnovo al server, o un nuovo indirizzo.

- - -
__nb__: nella tabella di routing, agli indirizzi saranno associate le maschere.

__nb__: DHCP (dynamic host configuration protocol). Ogni rete possiede un server DHCP.

- - -

# Limitatezza degli indirizzi IPv4 23/11/2022
## Interfaccie del router
Tra le informazioni fornite dal server DHCP ci sono anche la maschera, ovvero il prefisso della rete, l'indirizzo IP del router di dafault, l'indirizzo IP del DNS.
Le interfaccie del router avranno un indirizzo del blocco della rete a cui è connesso, quindi il numero di indirizzi sarà uguale a quello delle interfaccie connesse alle reti.

### Protocollo ICMP
Lo scopo principale del protocollo è quello di inviare messaggi di errore o informazione relativi al livello di rete. Se ad esempio scade il TTL, il router scarta il pacchetto e invia un messaggio di errore alla sorgente, in questo caso proprio un messaggio ICMP, composto da:
- header IP: composto da ip sorgente (router che invia il messaggio di errore), ip destinazione (host che aveva inviato il pacchetto), campo type con il codice ICMP.
- payload ICMP: conterrà l'informazione "TTL expired".

## Reti private
Considerando che per un indirizzo IPv4 vengono utilizzati 32 bit, il numero di indirizzi, se pur elevato è limitato, e relativamente basso (4 miliardi di indirizzi). Sono state sviluppate 2 soluzioni:
- IPv6: cambiare il protocollo IP.
- indirizzi privati.

Le seconda soluzione ci fa intendere che esistono indirizzi IP pubblici, che sono univoci, e privati, che possono essere usati in reti locali per il traffico locale. Quindi gli indirizzi privati non possono essere esposti verso la rete internet pubblica. Lo standard definisce i seguenti blocchi di indirizzi privati:
- 10.0.0.0 /8.
- 172.16.0.0 /12.
- 192.168.0.0 /16.
- 169.254.0.0 /16.

### NAT
I router che dividono una rete di indirizzi privati da una rete pubblica, dispongono di una funzionalità detta NAT, la cui idea base è quella di utilizzare l'indirizzo IP pubblico del router per l'instradamento.
Il router dispone di una tabella NAT, composta dai seguenti campi (seguendo l'esempio sotto):
- Ip sorgente: 192.168.1.3.
- Ip destinazione: 80.70.60.2.
- Porta sorgente: 10001
- Porta destinazione: 80
- Porta sorgente nuova: 20000

Le porte sorgente e destinazione, sono quelle presenti nell'header TCP o UDP, all'interno del payload del pacchetto. Al passaggio del router, anche gli header TCP o UDP sostituiranno la porta sorgente e destinazione con la "porta sorgente nuova". Questo meccanismo è detto __NAPT__. Ad ogni record della tabella viene associato un timer, rinnovato ogni volta che viene utilizzato, e che eliminerà il record al suo termine.
Nel caso un utente voglia comunicare con un'altro utente all'interno di una rete di indirizzi privati, il router scarterà ikl pacchetto, infatti in NAT non permette la comunicazione iniziale da host esterni. Alcune applicazioni in questo caso utilizzano __server di appoggio__ esterni, che terrà una comunicazione aperta con entrambi gli host.

## IPv6
L'header (detto header di base, 40 byte) IPv6 è sempre diviso in righe da 32 bit, e suddiviso sei seguenti campi:
1. version (4 bit), traffic class (8 bit), flow label (20 bit).
2. lunghezza payload (16 bit), next header (), hop limit ().
3. Ip sorgente (128 bit).
4. Ip destinazione (128 bit).

Quindi gli indirizzi saranno composti da 128 bit, non più divisi da una notazione decimale puntata, ma da una __esadecimale__ dividendo in 8 gruppi da 16 bit, rappresentati da 4 cifre esadicimali, separate dal simbolo ":". 

- - -
__nb__: i link dei router sono composti da un cavo di entrata e uno di uscita (duplex).

__nb__: ICMP (internet control message protocol).

__nb__: NAT (network address translation). NAPT (network address & port translation).

__Esempio__: router che collega la rete di indirizzi privati 192.168.1.0 /24, con internet (16.10.2.0 /24):
- interfaccia 1: 192.168.1.1.
- interfaccia 2: connesso alla rete dell'ISP che offre il servizio, 16.10.2.1.

Ipotizzando che un utente (192.168.1.3) della rete privata, voglia comunicare con un server di una rete, che ha indirizzo 80.70.60.2, invierà un pacchetto con un header IP (192.168.1.3 e 80.70.60.2). Quando il pacchetto passerà il router, l'indirizzo sorgente nell0header prenderà l'indirizzo dell'interfaccia pubblica del router. Anche in un eventuale pacchetto del server, la destinazione sarà l'interfaccia del router, che una volta ricevuto, andrà a sostituirlo con l'indirizzo originario di chi aveva generato il pacchetto.

__nb__: traffic class= gestisce la priorità tra i pacchetti.

__nb__: flow label= etichetta per far percorrere ai pacchetti dello stesso flusso lo stesso percorso.

__nb__: hop limit = equivale al TTL di IPv4.

__nb__: numero indirizzi IP per metro quadrato (oceani inclusi) è 6 * 10^23.

__Esempio__: indirizzo IPv6, 69DC:8864:FFFF:0000:...

__nb__: la soluzione migliore tra NAT e IPv6, è il NAT.
- - -

# Livello data-link 25/11/2022
## Campo next-header
Si tratta di un campo che possiede 2 significati:
- se c'è solo l'header di base, il campo identifica il protocollo trasportato nel payload. In poche parole equivale al campo type dell'IPv4.
- Se sono presenti uno o più header aggiuntivi, il campo ne indica il tipo. Questi header vengono detti __extension header__, sono sempre definiti dallo standard, e sono specifici per una determinata funzionalità, ad esempio la frammentazione.
L'extension header è composto dai campi: next header (8 bit) e header length (8 bit), il resto sono tutti dati dell'header.
Se in futuro venissero introdotte delle nuove funzionalità, è possibile creare un nuovo extension header. Se un router non è aggiornato, e non sa interpretare uno specifico extension header, lo ignora.

## Secondo livello
Ad ogni singolo hop, viene creato dal livello data-link, e scartato dall'host successivo, un __header di livello 2__. Lo scopo del livello è quello di gestire le problematiche associate alla trasmissione sul singolo hop. L'header del livello 2, dipende dalla tecnologia utilizzata nella scheda di rete (wifi (802.11), ethernet (802.3) ...).
Questo livello riscontra 2 principali problematiche:
- gestione del mezzo condiviso.
- delimitazione delle trame.

### Accesso al mezzo condiviso
Il principale mezzo di trasmissione condiviso è la trasmissione __wireless__. Nella trasmissione wireless, il router si interfaccia con gli hot attraverso un access-point, che comunica con loro tramite onde elettromagnetiche che si propagano in tutte le direzioni. Visto che gli host usano lo stesso mezzo, le loro trasmissioni con l'access-point potrebbero interferire.
Ormai non utilizzato più, un altro mezzo condiviso era la trasmissione su __cavo coassiale condiviso__. Ogni host possedeva un cavo coassiale che trasportava il segnale, e venivano collegati tramite un altro cavo coassiale. Il cavo era composto da un filo conduttore immerso in un materiale isolante, ricoperto da una maglia sempre di materiale conduttore, e infine un'altra guaina isolante.

### Tecniche di allocazione
In caso di trasmissione contemporanea, abbiamo collisione e quindi la distruzione dell'informazione, non potendo essere correttamente interpretata. Per ovviare a questo problema, viengono usate delle tecniche per l'allocazione del canale di trasmissione, che permettono di organizzare e allocare il canale alle diverse sorgenti che lo utilizzano. Queste tecniche sono varie, ci sono quelle centralizzate (master/slave, dove master è la CPU) e quelle __statiche__:
- FDM: viene assegnato ad ogni sorgente una frequenza diversa.
- TDM: le sorgenti hanno tutte un quanto di tempo, e si alternano (meccanismo simile al time-sharing).
Un altro tipo sono le tecniche __dinamiche__:
- a turno: ogni sorgente ha un quantità di tempo per trasmettere, al termine del quale passa il turno al vicino. Il problema più grande è stabilire l'ordine con cui le sorgenti si passano il turno .Un esempio è la tecnologia detta __token ring__.
- a contesa: il primo che trova il canale libero, trasmette, altrimenti resta in attesa. Di questo tipo, ci sono 3 protocolli principali, ALOHA, SLOTTED ALOHA, CSMA.

- - -
__Esempio__: ipotizzando che venga creato un messaggio abbastanza piccolo da non essere diviso in pacchetti. A livello di trasporto verrà aggiunto un header TCP, al livello di rete viene aggiunto l'header IP. Il livello di collegamento dati, come hanno fatto i livelli precedenti, considererà il prodotto come il suo payload, e gli ci aggiunge un header (L2). Dopo essere stato inviato e ricevuto da un'altro host, il livello di data-link scarterà l'header L2.

__nb__: un access-point è un appartato di livello 2, da un lato connesso al router, dall'altro dispone di un'interfaccia wireless, con cui comunica con i vari host.

__nb__: FDM (frequency division multiplex), TDM (time division multiplex).

__nb__: l'allocazione statica prevede di conoscere fin da subito il numero di sorgenti.
- - -

# Tecniche dinamiche a contesa 02/12/2022
## Algoritmo Aloha
1. Se ci sono dei dati da trasmettere, vengono trasmessi.
2. Mentre trasmette, ascolta il canale. Se c'è collisione viene estratto un tempo casuale, si attende e si torna al punto 1.
Per la casualità del tempo, il protocollo definisce una costante M (molto maggiore del tempo di trasmissione) e le stazione scelgono un tempo uniformemente distribuito nell'intervallo (0,M). M verrà raddoppiato in caso di collisioni consecutive.
Il __periodo di vulnerabilità__ è l'intervallo di tempo in cui una __trama__ può subire collisione. Se identifichiamo il tempo di trama con T, il periodo di vulnerabilità sarà 2*T. Se si volesse calcolare l'efficienza del protocollo ALOHA, con l'aumento delle trasmissioni si può notare che aumentano le collisioni.

### Slotted Aloha
In questa variante, il tempo è suddiviso a intervalli (slot) della durata di T secondi (come il tempo di trama), e le sorgenti seguono il protocollo ALOHA, ma quando generano una trama possono trasmetterla solo all'inizio dello slot successivo. In questo caso il periodo di vulnerabilità sarà pari a T, ovvero la dimensione dello slot, aumentando notevolmente l'efficienza.

## CSMA
1. Se ci sono dei dati da trasmettere, prima ascolta il canale.
2. Se il canale è libero, allora trasmette la trama, ma se è occupato aspetta che il canale si liberi e poi trasmette.
3. Se ci sono collisioni, estraggo un tempo casuale e torno al punto 1.
Le collisioni avvengono nel caso entrambe le sorgenti controllino se è occupato il canale nello stesso momento, finendo per iniziare la trasmissione e un'inevitabile collisione.

### Varianti CSMA
Questa è la versione base del CSMA, detta "persistent", ma ne esistono moltre altre. Una di queste è detta "non-persistent" e va a modificare il punto 2:
2. Se il canale è occupato, viene estratto un tempo casuale e si ripete dal punto 1.
Ne consegue un maggiore ritardo. Un'altra variante è detta "p-persistent" che va a modificare sempre il punto 2:
2. Se il canale è occupato, aspetta che il canale si liberi, e poi trasmetterà con probabilità "p" mentre con probabilità "1-p" rimanderà di un __microslot__ (la cui durata è molto minore di quella di trama T).

### CSMA-CD
Quest'ultima variante va a modificare il punto 3:
3. Se c'è collisione, interrompo la trasmissione, estraggo un tempo casuale e torno al punto 1. Nelle altre varianti di CSMA, la trasmissione non veniva interrotta.
Attualmente, per il protocollo ethernet (802.3), viene adottato il protocollo CSMA-CD persistent.

- - -
__nb__: la collisione avviene se la potenza rilevata è maggiore di quella trasmessa.

__nb__: il tempo di trasmissione è detto __tempo di trama__,

__nb__: efficienze aloha: 19%, slotted aloha: 38%, csma persistent: 58%, csma p(0,01): la più efficiente.

__nb__: CSMA (carrier sense multiple access). CD (collision detection).

__nb__: considerando che una trasmissione ci impiega del tempo ad arrivare ad altre sorgenti e qundi essere identificata, e questo è definito con "tau" e detto il ritardo massimo di propagazione tra 2 sorgenti, allora il periodo di vulnerabilità nel CSMA è pari a 2 * tau.
- - -

# Indirizzi logici e fisici 07/12/2022
## Ethernet IEEE 802.3
Divenuto uno standard, presenta diverse forme scalabili:
- base: 10 Mbit/s
- fast ethernet : 100 Mbit/s
- gigabit ethernet : 1 Gbit/s
Possono essere utilizzati per connettere calcolatori, ma anche apparati di rete, generalmente per questi ultimi vengono utilizzate velocità di 10 e 40 Gbit/s.

### Formato dei messaggi
L'header è composto dai seguenti campi:
- preambolo (7 byte): sequenza di 0 e 1 alternati, che permette di  sincronizzare in una comunicazione chi riceve e che trasmette. Si sincronizza con il clock di chi riceve.
- flag (1 byte): serve per la delimitazione delle trame (01111110).
- destinazione hardware (6 byte): contiene l'indirizzo MAC del destinatario.
- sorgente hardware (6 byte): contiene l'indirizzo MAC della sorgente.
- lunghezza payload (2 byte)
Successivo al payload contiene un'ultima sessione, detta __trailer__, che conterrà la checksum.

### Protocollo ARP
Questo protocollo permette ad un host di conoscere gli indirizzi MAC degli host della propria rete. Più precisamente, dato l'indirizzo IP permette di conoscere l'indirizzo MAC:
1. l'host manda in broadcast (livello 2) una richiesta ARP con l'indirizzo ip di cui non conosce il corrispondente MAC. Si parla di "ARP request".
2. solo chi possiede l'indirizzo IP richiesto, risponde direttamente con il proprio MAC. Si parla di "ARP reply".

Le risposte ARP vengono memorizzate in una __tabella ARP__, contenente i campi: IP, MAP, tempo di validità.

## Lan estese
Tornando al discorso del cavo coassiale che collega i vari calcolatori, se parliamo in termini di cablaggio strutturato, è facilmentre attuabile se i calcolatori sono nella stessa stanza. In caso contrario, ad esempio i calcolatori che si sviluppano in tutto un palazzo, bisogna attuare diverse soluzioni, mediante diversi apparati:
- Hub: apparato di livello 1 , che connette due cavi tra loro. Permette quindi di applicare ad un cavo vari estensioni.
- Bridge: apparato di livello 2, a due porte, avente lo stesso obiettivo dell'hub, ma che non si ferma appunto al livello 1. Può quindi ricevere completamente le trame, controllare l'indirizzo MAC di destinazione, e rigira la trama sul segmento che punta alla destinazione. Per farlo mantiene una tabella, che associa gli indirizzi MAC con il corrispondente segmento.
- Switch: si tratta di un bridge con n porte, e per ogni porta un segmento.

- - -
__nb__: per i 40 Gbit/s in realtà è necessaria la fibra ottica.

__nb__: nei 2 campi di hardware, sono contenuti gli indirizzi fisici delle macchine, assegnati alla scheda di rete quando viene prodotta, gli indirizzi __MAC__. In una consegna indiretta, l'indirizzo MAC di destinazione sarà quello del router di bordo, a quel punto il router scarterà l'header, aprirà il pacchetto, e assegnerà un nuovo header che permetta al messaggio di arrivare a destinazione.

__nb__: ARP (address resolution protocol).

__nb__: un Hub può avere 2 o più porte.

- - -

# WLAN  14/12/2022
## Dominio di broadcast e collisione
Il dominio di broadcast viene definito come la porzione di rete raggiunta da un messaggio di broadcast di livello 2. Il dominio di collisione è la porzione di rete dove si ha collisione, se due stazioni trasmettono contemporaneamente. Il dominio di broadcast viene spezzato dal router, mentre quello di collisione viene spezzato da bridge e switch.

## WLAN (802.11)
Presenta due modalità:
- ad hoc: le varie strutture comunicano direttamente tra di loro.
- infrastruttura: presenta un apparato detto __access point__, che rappresenta l'apparato di riferimento per la comunicazione. L'area wireless che rientra nel campo dell'access-point, è detta __BSS__, all'interno del quale, se gli host vogliono comunicare tra di loro, dovranno prima passare per l'access-point.

- - -
__Esempio__: avendo un router che collega 2 lan con internet, e un host della lan1 che manda un messaggio di broadcast, il dominio di broadcast sarà la lan1, infatti una volta arrivato al router, il secondo livello verrà scartato.

__Esempio__: se due segmenti sono collegati mediante un hub, il dominio di collisione verrà rappresentato da entrambi i segmenti, mentre se sono divisi da un bridge, avremo 2 domini di collisione per i rispettivi segmenti.

__Esercizio__: esercizi su algoritmi di livello 2 (Aloha, csma), e apparati di livello 2 (bridge).

__nb__: WLAN (wireless lan). La modalità "ad hoc" è poco utilizzata.

__nb__: BSS (basic service set).

__nb__: lo standard è composto da una famiglia di specifiche, rappresentate ogniuna da una lettera diversa. Ad esempio 802.11a definisce un insieme di frequenze e le corrispondenti velocità di trasmissione.
- - -

# CSMA-CA 16/12/2022
## Algoritmo CSMA-CA
Nelle wlan il tempo è diviso in time-slot che sono proporzionali al ritardo di propagazione, e viene detto SIFS. Definiamo iun altro intervallo detto __DIFS__, che rappresenta una sequenza di 3 SIFS. Ora che abbiamo definito gli intervalli, si può parlare di __CSMA-CA__, ovvero un algoritmo dove quando una sorgente ha una trama da trasmettere, ascolta il canale:
1. Se il canale è libero, continuo ad ascoltare il canale per un intervallo pari a DIFS, e se è ancora libero, trasmette la trama.
2. Se il canale è occupato, sia fin da subito che durante il DIFS, continuo ad ascoltare il canale fino a quando non si libera.
3. Quando il canale si libera, ascolto per un DIFS, in cui se torna occupato, torno al punto (2).
4. Se il canale è rimasto libero durante il DIFS, la stazione estrae un numero intero casuale "s", uniformemente distribuito tra 0 e "CW-1", che sarà il numero di SIFS da attendere prima di trasmettere. Ad ogni SIFS, se il canale rimane libero, viene decrementato "s" di 1, e arrivati a "s=0" la sorgente trasmette la trama. Se invece il canale torna occupato, prima di arrivare a 0, congelo il valore di "s", torno al punto (2), e quando sarò arrivato al punto (4) utilizzerò il valore di "s" congelato.
5. Se c'è collisione, viene interrotta la trasmissione, viene estratto un tempo casuale, si torna al punto 1, ma si raddioppia "CW".

## Terminale nascosto
Problema che afflige le WLAN. Nell comunicazione wireless, l'onda di trasmissione viene generata in tutte le dimensioni, non solo tra 2 host comunicanti, quindi è molto altra la dissipazione. Data la potenza di trasmissione, c'è una distanza massiama oltre la quale il segnale non è ricostruibile (rumore bianco). Nel tempo in cui un host trasmette ad un altro, questo potrebbe iniziare un'altra trasmissione verso quello stesso host. In quel caso le 2 trasmissioni entrano in collisione, viene rilevata dall'access-point, ma nessuno dei 2 host la rileva. Per risolvere questo problema:
- si limita lo spazio di responsabilità dell'access-point.
- vengono introdotti 2 messaggi, RTS e CTS. Se "A" deve trasmettere, manda un RTS, l'access-point risponde con un CTS in broadcast, così sia "A" che "B" sanno chè c'è una trasmissione, "A" trasmetterà, "B" starà fermo.

- - -
__nb__: SIFS : short inter-frame space.

__nb__: CSMA-CA: CSMA collision avoidance.

__nb__: la lunghezza dei time-slot SIFS e DIFS, è data dal clock dell'access-point.

__nb__: CW (contention window), valore massimo del numero intero estratto.

__nb__: lo standard vuole che l'ack di  livello 2, venga mandato dopo un SIFS.

__nb__: RTS (request to send), CTS (clear to send).
- - -

# NAV e Framing 21/12/2022
## NAV
Dal punto di vista energetico, la maggior parte dell'energia è spesa per l'ascolto del canale, quindi con RTS/CTS le stazioni che non trasmettono ricevono comunque il CTS, e possono usare le informazioni contenute nel CTS stesso per addormentarsi, ovvero spegnere il circuito d'ascolto. Non solo con RTS/CTS è possibile risparmiare energia, nell'header 802.11 c'è un campo __payload length__: quando viene ricevuto il pacchetto, se è destinato ad un altro host, si legge il campo payload length e si spegne l'ascolto del canale, per appunto la lunghezza della trama. Questo meccanismo è detto __NAV__.

## Delimitazione delle trame
Nella pila TCP/IP i vari livelli gestiscono i dati nelle loro varie forme. Il livello fisico si occupa della gestione di bit, tradotti da un'onda elettromanietica. Nel caso ci siano più trame in sequenza, si uniranno tutte in una onda elettromanietica che verrà tradotta in bit. Quindi è necessario distinguere una trama dall'altra:
- introdurre intervalli temporali tra una trama e la successiva. Comporta il problema che con la propagazione il segnale si distorce e rischia che i momenti di silenzio vengano coperti.
- introdurre l'informazione sulla dimensione della trama (character count) nell'header, in modo da sapere quando la trama finirà, e inizierà la successiva. Se però dovesse verificarsi un errore nel campo, tutte le trame verranno interpretate male.
- byte di flag. Viene aggiunta una sequenza specifica all'inizio e alla fine della trama. Il problema è lo stesso riscontrato nella soluzione precedente, con la differenza che verrà compromessa solo trama in cui c'è l'errore nel byte di flag.

### Bit stuffing
Nella trama può essere presente una sequenza uguale al byte di flag, quindi è stata adottata questa soluzione:
- in trasmissione: ogni volta che questa sequenza viene incontrata, viene agginto uno 0.
- in ricezione: ogni volta che viene individuata la sequenza, viene rimosso lo zero.

- - -
__nb__: NAV (network allocation vector), contatore che indica il tempo in cui il canale è occupato.

__Esempio__: con sequenza di byteflag si intende una sequenza di bit del tipo 000101011.

- - -