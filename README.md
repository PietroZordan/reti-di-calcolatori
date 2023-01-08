# Introduzione
## Comunicazione
La comunicazione tra 2 entità avviene all'interno di un'infrastruttura fisica della __architettura di rete__, e viene definita mediante un insieme di regole formali detto __protocollo__. L'insieme dei protocolli usati in un'architettura è detto __stack protocollare__.
L'architettura è composta da:
- end-host.
- intermediate-host (mid-host).
- link.

Gli end-host sono i calcolatori, quindi i terminali che trasmettono nell'architettura, comunicano tra di loro mediante l'avvio dei processi, che saranno identificati da:
- indirizzo IP del calcolatore.
- identificatore del processo, o porta.
La comunicazione tra 2 processi sarà quindi definita da una tupla (IpA, IpB, portaA, portaB) dove A e B sono le 2 entità. Questa tupla è detta __socket__.

## Trasferimento
Per il trasferimento di informazioni, esistono 2 modalità:
- reti a __commutazione di circuito__: il canale trasmissivo verrà aperto all'apertura della connessione, e sarà sempre disponibile per i 2 host, infatti le risorse del canale sono interamente dedicate alla comunicazione. Non è più utilizzato, perchè spreca molte risorse nei momenti di attesa.
- reti a __commutazione di pacchetto__: le informazioni vengono suddivise in pacchetti, che vengono contenuti nel __buffer__ del router. I pacchetti vengono spediti singolarmente, e ricevuti con l'ordine di arrivo. Un router può ricevere pacchetti da più router diversi, contemporaneamente, in modo da non sprecare risorse. Questà funzionalità è detta __multiplazione statistica__. Essendo i buffer limitato, se ci sono pacchetti di troppo verranno scartati.

Un pacchetto richiederà del tempo per essere inviato, spedito e ricevuto, quindi si genera un __ritardo__ variabile. Esso può dipendere da diversi fattori:
- numero di appartati del circuito.
- capacità di propagazione degli apparati, da cui se ne deriva la velocità di trasmissione.
- dimesione del pacchetto.
- accodamento di pacchetti nei buffer.
- elaborazione e interpretazione dell'intestazione.

Questo ritardo può essere stimato mediante 2 tecniche:
- __ping__: viene inviato un messaggio (echo request) al destinatario, che risponderà (echo reply), permettendo di calcolare il tempo di trasmissione __RTT__ (round trip time).
- __traceroute__: lo scambio di messaggi avviene per ogni apparato, in modo da individuare e conoscere anche il circuito.

## Modello a strati
Gli end-host di una rete, sono generalmente costituiti da vari livelli indipendenti, dove ogniuno esegue un compito e invia il prodotto al livello successivo. Il modello storico e teorico per eccellenza è la pila __ISO/OSI__:
1. applicazione
2. presentazione
3. sessione
4. trasporto
5. rete
6. collegamento
7. fisico

Mentre il modello realmente utilizzato è la pila __TCP/IP__:
1. applicazione
2. trasporto
3. rete
4. collegamento
5. fisico

Di questi 5 livelli sono dotati solo gli end-host, mentre i mid-host dispongono solo di degli utimi 3 livelli.

# Livello applicativo
## Processi
Le entità messe in comunicazione tramite un socket possono essere di 2 tipi:
- processo __server__: dispone di un indirizzo IP statico, e deve essere sempre raggiungibile, in modo da rimanere in attesa per eventuali richieste.
- processo __client__: dispone di un indirizzo IP dinamico, assegnatogli da un server __DHCP__, ed è il processo responsabile dell'apertura di una connessione.

Il server deve restare sempre in ascolto, ed avendo un IP statico può essere riconosciuto e trovato dai client.
Nel trasporto dei messaggi un protocollo può essere di 2 tipi:
- __connection oriented__: affidabile, ovvero non accetta perdite di dati (TCP).
- __connectionless__: non affidabile, ovvero può accettare alcune perdite di dati (UDP).

## Protocollo HTTP
Il protocollo HTTP è un protocollo testuale, e permette di gestire la comunicazione all'interno di una pagina web. Un pagina web è una collezione di documenti o file:
- file contenente la struttura della pagina.
- eltri elementi (immagini, video).
- script e stylesheet.

La pagina è contenuta all'interno di un server, che rimarrà in ascolto di eventuali richieste di quella pagina, da parte di un client. Il procedimento di richeista e risposta in un'applicazione web è il seguente:
1. il server rimane in ascolto.
2. il client esegue la ricerca sul browser, e invia così la richiesta al server. [inizio apertura della connessione]
3. il server risponde, confermando o negando la connessione.
4. il client manda a livello applicativo un messaggio di GET, richiedendo la pagina specificata. [fine apertura della connessione]
5. il server scinde in pacchetti il file di struttura, e li invia al client.
6. il client ricompone i pacchetti, e prosegue a richiedere gli altri file.

Per richiedere gli altri file, vengono adottati 2 metodi:
- connessione __persistente__: vengono inviate, per ogni file, delle richieste di GET in parallelo.
- connessione __non persistente__: viene chiusa la connessione, e ne vengono create in parallelo, una per ogni richiesta di GET.

Quindi il client e il server, si scambiano 2 tipi di messaggi:
- __richiesta__, composto da:
    - riga di richiesta: specifica l'azione da compiere (GET, POST, DELETE), e l'oggetto di tale azione.
    - righe di intestazione: contengono informazioni utili per la richiesta (versione browser, cookie).
- __risposta__, composto da:
    - riga di stato: conterrà un codice che descrive se l'operazione è andata a buon fine o meno (200, 400, 404, 405).
    - righe di intestazione.
    - risorsa richiesta.

## Funzioni a livello applicativo
Il protocollo HTTP, e il livello applicativo in generale, presentano diverse funzioni utili:
- __cookie__: permette ad un server di capire se ha già avuto un'interazione con un dato client. Durante la prima comunicazione, il server invia nella righa "set-cookie" un codice che assocerà al client. Il client può decidere di memorizzarlo, e in tal caso, lo invierà in un'eventuale richiesta successiva, nella riga "cookie".
- __caching__: durante una comunicazione, i messaggi del client e del server, vengono ricevuti da un server cache. Il suo compito è quello di ridurre il traffico, memorizzando la risposta di un server a una data richiesta, in modo da poter rispondere ad un client direttamente. La percentuale con cui la cache soddisfa la richieste è detta __hit-ratio__.
- __DNS__: i server DNS permetto di tradurre gli indirizzi IP in nomi logici, e viceversa, operando prima che inizi la connessione tra client e server. Esistono 3 tipi di server DNS, disposti un sistema gerarchico: __root DNS__, __TLD DNS__, __local DNS__.

Fornisce anche un servizio di __posta elettronica__, offerto da 2 principali protocolli, __STMP__ per l'invio, __IMAP__ per la ricezione. Ogni dominio possiede un server di posta elettronica, avente una __coda__ per i messaggi in uscita, e delle __caselle__ per ogni utente del dominio. Due clienti di dominio differente possono comunicare nel seguente modo:
1. utente del dominio @dom1.com invia una mail all'utente di dominio @dom2.com.
2. il server di posta elettronica @dom1.com riceve la mail, e la mette nella coda di uscita.
3. la mail viene inviata al server di dominio @dom2.com che la inserisce nella casella dell'utente.
4. grazie a IMAP e ad HTTP l'utente può accedere alla propria posta elettronica.

# Livello di trasporto
## Header TCP
Il compito del livello di trasporto è quello di suddividere il messaggio da trasferire, in __pacchetti__ indipendenti. Ogni pacchetto avrà un __payload__ ovvero il contenuto del messaggio, e un __header__ di 20 byte, suddiviso in righe da 32 bit e così composto:
1. porta sorgente, destinazione.
2. sequence number.
3. acknowledge number.
4. offset, reserved, flags, window.
5. checksum, urgent pointer.

Le __porte sorgente/destinazione__ sono gli identificativi dei 2 processi comunicanti. Un porta può essere:
- statica: definita dallo standard, destinata al processo server.
- dinamica: definita dall'OS, destinata al processo client (>=1024).

Il __sequence number__ è un serie di bit, che permette di identificare i pacchetti. Essendo i pacchetti indipendenti, attravererso il sequence number sarà possibile riordinarli una volta arrivati a destinazione. Per questo campo viene inizialmente definito un __initial sn__, che verrà assegnato al primo pacchetto, a cui in seguito verrà sommata la dimensione dei pacchetti, detta __MSS__. Il MSS viene definito mediante la dimensione massima di trasmissione della scheda di rete, detta __MTU__.

L'__acknowledge number__ è un campo che conterrà il sequence number del pacchetto che si vuole confermare. Infatti essendo il TCP un protocollo affidabile, utilizza una tecnica detta __positive acknowledge with retrasmission__ per capire se un pacchetto è arrivato a destinazione. Questo è il funzionamento generale:
1. il client manda il primo pacchetto, con sn= 1909.
2. il server, lo riceve e risponde con un messaggio di __ack__, un header TCP che conterrà 1909 nel campo _acknowledge number_.

In realtà, l'acknoledge number conterrà il sequence number + 1, ovvero il pacchetto che il server si aspetta di ricevere. Qundi il client saprà che il server ha ricevuto tutti i byte di informazione prima di quel valore. Il sequence number del pacchetto che vuole confermare sarà comunque presente in dei campi aggiuntivi, chiamati generalmente _opzioni_.

Il __checksum__ permette di verificare se un pacchetto arriva a destinazione con degli errori. Attraverso una funzione che prende in input il messaggio, viene prodotto il checksum. Una volta che il pacchetto verrà ricevuto, verrà eseguita nuovamente la funzione: se l'output è diverso dal checksum, vuol dire che sono presenti errori. Nel caso contrario, ci sarà un'alta probabilità che non siano presenti errori (non è detto al 100% che non siano presenti errori).

## Apertura e chiusura della connessione
Prima di poter scambiare i dati, le 2 entità devono aprire la connessione (three ways handshake), mediante lo scambio di specifici _header TCP_:
1. SYN (client): il flag __syn__ è posto a 1. Conterrà il MSS del client.
2. SYN-ACK (server): i flag _syn_ e __ack__ sono posti a 1. Conterrà il MSS del server.
3. ACK (client): il flag _ack_ è posto a 1.

Nel messaggio di SYN, viene definito l'_initial sequence number_. Per quanto riguarda il MSS, per la trasmissione verrà utilizzato quello minore.
Anche la chiusura della connessione avviene mediant lo scambio di _header TCP_:
1. FIN (client): il flag __fin__ è posto a 1. Specifica che non ha più nulla da trasmettere.
2. ACK (server): il flag _ack_ è posto a 1. A questo punto trasmette i dati che deve ancora trasmettere.
3. FIN (server): il flag _fin_ è posto a 1. Vuole dire che anche il server ha finito.
4. ACK (client): il flag _ack_ è posto a 1.

Se invece anche il server ha finito, e quindi non ha niente da trasmettere, risponderà con un messaggio di __FIN-ACK__, a cui il client risponderà con un _ACK_.

## Affidabilità
Il TCP non accetta perdite di dati, quindi se un pacchetto non arriva a destinazione (viene perso il pacchetto o il messaggio di ack), va inviato nuovamente. Un pacchetto viene ritrasmesso, se non arriva conferma entro il limite di tempo detto __RTO__, che viene calcolato grazie al _RTT istantaneo_. Ottenendo un "valore medio" di questi RTT, si ottiene lo __smoothed RTT__ con cui è possibili calcolare il RTO:
```C
double srtt_attuale = a * srtt_precedente + (1-a) * rtt;

double rto = b * srtt_attuale;
```
Quindi la velocità di trasmissione è di _1/RTT_, e più esso è grande, più la trasmissione sarà lenta. La soluzione è inviare più pacchetti invece di uno singolo, ma troppi pacchetti andrebbero a creare un __congestione di rete__. Quindi bisogna stabilire una giusta quantità di pacchetti, e lo si fa mediante 2 azioni, una preventiva detta __controllo di flusso__, e una risolutiva detta __controllo di congestione__.<br>
Nel controllo di flusso, viene stabilita una __congestion window__ ovvero la dimensione della serie di pacchetti da inviare. Man  mano che si ricevono i riscontri dei pacchetti, la _cw_ scorre della quantità di pacchetti confermati. Essendo i pacchetti indipendenti, la finestra scorrerà solo se non ci sono buchi (esempio con cw = 3):
1. arriva la conferma del pacchetto 1, la _cw_ scorre e invia il pacchetto 4.
2. arriva la conferma del pacchetto 3, ma non essendo arrivato il pacchetto 2, la _cw_ non scorre.
3. arriva la conferma del pacchetto 2, la _cw_ scorre per il pachcetto 2 e 3, quindi vengono inviati i pacchetti 5 e 6.

Ovviamente se i pacchetti vanno persi, verranno ritrasmessi terminato il RTO. Oltre a scorrere, la finestra può aumentare e diminuire, e vengono utilizzati 4 diversi algoritmi:
- __slow-start__: presenta un'evoluzione esponenziale nel tempo, perchè aumenta di 1 ad ogni riscontro.
- __congestion-avoidance__: presenta un'evoluzione lineare nel tempo, perchè aumenta di "1/cw" ad ogni riscontro.
- __vanilla__: diminuisce di 1 la dimensione.
- __fast-retrasmit/fast-recover__: dimezza la dimensione della congestion window.

Nel controllo di congestione, viene utilizzato il seguente algoritmo con le seguenti variabili:
- CWND: dimensione attuale della congestion window.
- RTO.
- RCVWND: dimensione massima di ricezione.
- SSTHRESH: dimensione oltre la quale non può essere usato lo slow-start.

```C
{
    double cwnd, rto, rcvwnd, ssthresh;

    inizializzazione(...);
    if(riscontro){
        aumenta();
    }else{
        riduci(scelta);
    }

    void inizializzazione(double new_rto, double max_cw){
        cwnd = 1;
        rto = new_rto;
        rcvwnd = max_cw;
        ssthresh = rcvwnd /2;
    }

    void aumenta(){
        for(int i=0; i<cwnd; i++){
            send(pacchetti[i]);
        }
        
        if(cwnd < ssthresh){
            cwnd = slow_start(cwnd);
        }else{
            cwnd = congestion_avoidance(cwnd);
        }
    }

    double slow_start(double cwnd){
        cwnd = cwnd + 1; //1 sarebbe il numero di riscontri
        return min(cwnd, rcvwnd, ssthresh);
    }
    double congestion_avoidance(double cwnd){
        cwnd = cwnd + (1/cwnd); //1 sarebbe il numero di riscontri
        return min(cwnd, rcvwnd);
    }

    void riduci(char *scelta){
        ssthresh = cwnd/2;

        if(scelta == "vanilla"){
            cwnd = 1;
        }else{
            cwnd = ssthresh;
        }

        rto = rto*2; //per i segmenti che vado a ritrasmettere
    }
}

```
## Protocollo UDP
Essendo un protocollo non affidabile, non presenta nessun controllo di flusso e congestione, e ha quindi una struttura estremamente semplice. L'header, diviso in righe da 32 bit, è composto da:
1. porte sorgente, destinazione.
2. lunghezza, checksum.

# Livello di rete
## Header IP
Il livello di rete si tratta di un livello _best effort_, che quindi fa il possibile per garantire che il pacchetto arrivi a destinazione, ma non lo garantisce (se ne occuperanno gli altri livelli). Quando il livello di rete riceve il pacchetto da un livello superiore, lo inserisce nel proprio payload e ci aggiunge un __header IP__ di 20 byte e righe da 32 bit, composto dai seguenti campi:
1. vers, lunghezza header, service type, lunghezza totale.
2. identificazione, flags, fragment offest.
3. TTL, type, checksum.
4. indirizzo IP sorgente.
5. indirizzo IP destinazione.
6. eventuali opzioni.

Il campo "vers" contiene la versione del protocollo (ipv4, ipv6), mentre le lunghezza header e totale vengono definmite in byte. La seconda riga, detta __riga di frammentazione__ presenta dei campi utili alla frammentazione dei pacchetti. Il campo "service type" serve per gestire la priorità tra i pacchetti, all'interno dei buffer della destinazione.

Il campo "__TTL__" è un contatore che viene decrementato ad ogni router e che indica il numero di massimo di hop che il pacchetto può attraversare (solitamente 64 o 128), e se arriva a 0, viene scartato il pacchetto e inviato un messaggio di errore alla sorgente. Il campo "type" contiene un codice che specifica il protocollo trasportato nel payload, in quanto i router non andranno ad analizzare il payload, ma solo l'header.

## Frammentazione
I router hanno differenti MTU tra loro, quindi è necessario alcune volte frammentare i pacchetti se superano tale dimensione. I frammenti avranno la dimensione specificata da tale MTU, saranno indipendenti e avranno quindi bisogno di un proprio header IP. La riga di frammentazione servirà proprio per distinguere i frammenti:
- identificazione: identificativo del pacchetto, che sarà quindi uguale per tutti i frammenti di quel paccheto.
- flag "M": posto a 0 se non è un frammento o se è l'ultimo, posto a 1 se ci sono altri frammenti.
- offset: posizione del frammento rispetto al pachetto originario. La dimensione è definita in termine di "byte/8".

Una volta ricevuto il primo frammento, partirà un timer (circa 500 ms) e allo scadere tutti i frammenti verranno scartati.

## Routing
Quando 2 host vogliono comunicare, verificano dall'indirizzo se appartengono alla stessa rete, e il loro scambio di informazioni può essere distinto a seconda dei casi:
- __consegna diretta__: i 2 host appartengono alla stessa rete, e il messaggio passa solo attravero lo switch che li collega.
- __consegna indiretta__: i 2 host appartengono a reti distinte, e quindi il messaggio dovrà passare dai 2 router di dafault.

Per la consegna indiretta bisogna introdurre il concetto di __routing__ ovvero una tecnica che permette di individuare il _cammino migliore_ per raggiungere la destinazione, in base a distanza, costo, velocità di trasmissione... è possibile rappresentare il routing attraverso un'astrazione con grafi, dove gli archi sono i link, e i nodi sono i router. I diversi algoritmi di routing si raggruppano in 2 classi:
- distance-vector.
- link-state.

Gli algoritmi __distance-vector__ di basano sul __criterio di consistenza__, quindi una porzione del cammino minimo, sarà anche il cammino minimo tra i 2 nodi che la delimitano. La decisione del cammino minimo non si basa sull'intera tipologia di rete, infatti ogni nodo conosce solo il costo del proprio vicino. Mediante un sistema a catena, partendo dalla destinazione i vicini si comunicheranno il costo per raggiungerla, fino ad arrivare alla sorgente. Sfruttando il criterio di consistenza, la sorgente conoscerà il costo del cammino minimo ottenuto sommando le porzioni minime tra i vari nodi, e da che vicino dovrà passare per _imboccarlo_. Il costo è il _next-hop_ per raggiungere una qualsiasi destinazione, compongono il _distance-vector_ e sono memorizzati dal router nella propria __tabella di routing__. Periodicamente una router comunica ai vicini il proprio distance-vector, con cui possono aggiornare la tabella.

Gli algoritmi __link-state__ invece utilizzano l'approccio centralizzato rispetto al router, ovvero ogni router conoscerà l'intera tipologia di rete, e può quindi calcolare autonomamente i cammini.

Queste 2 classi di algoritmi vengono utilizzate per il routing __intra-AS__ ovvero all'interno dell'ISP, ma non sono validi quando si tratta di routing __inter-AS__, dove viene utilizzato il protocollo __BGP__. Nella tabella di routing vengono memorizzate anche destinazioni esterne all'ISP, e attraverso quest'algoritmo viene calcolato il cammino minimo per raggiungere il router di bordo più vicino alla sorgente. Visto che le destinazioni esterne potrebbero essere estremamente numerose, molti indirizzi vengono compattati in blocchi, per essere separati in seguito con delle successive fasi di _subnetting_.

## DHCP
Agli host vengono assegnati dinamicamente gli indirizzi disponibili della rete, mediante un __server DHCP__. Per farlo l'host si assegna l'indirizzo 0.0.0.0, e scambia con il server una serie di messaggi DHCP, composti da un payload contenente il protocollo DHCP, e un header IP di cui terremo in considerazione i campi type, ip sorgente e destinazione. All'interno del payload è presente un campo __transaction ip__ ovvero un numero casuale scelto dal client. Lo scambio dei messaggi è il seguente:
1. __DHCP discover__ (client): messaggio in broadcast con [sorgente: 0.0.0.0, destinazione: 255.255.255.255]. Il client richiede di poter ricevere un indirizzo, inserendo nell'offset un numero casuale (transaction id).
2. __DHCP offer__ (server): messaggio in broadcast con [sorgente: ind.server, destinazione: 255.255.255.255]. Il server offre un indirizzo IP al client, inserendolo nell'offset insieme al _transaction id_ dell'host.
3. __DHCP request__ (client): messaggio in broadcast con [sorgente: 0.0.0.0, destinazione: 255.255.255.255]. Il client invia nel payload l'indirizzo offerto, per chiedere che gli venga assegnato.
4. __DHCP ack__ (server): messaggio in broadcast con [sorgente: ind.server, destinazione: 255.255.255.255]. Il payload sarà uguale al "DHCP offer", ma conterrà anche un codice ack.

Ora l'host può usare l'indirizzo IP assegnato, fino al limite di tempo detto __lease__, allo scadere del quale il client dovrà richiedere il rinnovo o la sostituzione. Tra le informazioni fornite dal server DHCP ci sono anche maschera, l'ip del default router e del server DNS.

## Limitatezza IPv4
Per segnalare errori, come lo scadere del TTL, ma anche per inviare informazioni relative al livello di rete, viene utilizzato il protocollo ICMP all'interno del payload di un messaggio IP. Il payload conterrà l'informazione "TTL expired".
Un indirizzo IP è composto da 32 bit, non ostante sia un numero molto alto, è comunque limitato e relativamente basso. Sono state sviluppate 2 soluzioni:
- IPv6.
- NAT.

L'IPv6 è la versione succesiva dell'IPv4, dove semplicemente aumentano i bit da 32 a 128. L'indirizzo non verrà più rappresentato con notazione decimale, ma con quella esadecimale, dividendo con il simbolo ":" in 8 blocchi da 16 bit. L'header dei messaggi è detto di _base_ ed è composto da 40 byte, e diviso sempre in righe da 32 bit:
1. version, traffic class, flow label.
2. lunghezza payload, next header, hop limit.
3456. ip sorgente.
78910. ip destinazione.

Il campo "traffic class" gestisce la priorità tra i pacchetti, "flow label" permette di far percorrere ai pacchetti dello stesso flusso lo stesso pacchetto, "hop limit" equivale al TTL.
Il campo "next-header" può aver 2 significati:
- se presente solo l'header di base, il campo farà il lavoro del campo _type_.
- possono essere presenti degli header aggiuntivi, detti __extension header__, che permettono di svolgere specifiche funzionalità. In quel caso il campo conterà il codice del protocollo dell'header successivo.

La soluzione migliore è quella di distinguere 2 tipi di indirizzi, quelli pubblici e quelli privati, quest'ultimi possono essere usati nelle reti locali per il traffico locale. Per poter comunicare con l'esterno viene utilizzata la tecnologia __NAT__, la cui idea di base è quella di utilizzare l'interfaccia pubblica del router. Quando un host con indirizzo privato vuole inviare un messaggio ad un host esterno alla rete, il router sostituirà l'indirizzo sorgente dell'host con quello della propria interfaccia pubblica, dell'header IP del pacchetto. Se l'host esterno vuole comunicare un'informazione a quello privato, il router dovrà poter ricordare qual'è l'host a cui aveva sostituito l'indirizzo. Per farlo utilizza una __tabella di NAT__, composta dai seguenti campi:
- ip sorgente: indirizzo privato.
- ip destinazione: indirizzo dell'host esterno.
- porta sorgente: contenuta nell'header TCP.
- porta destinazione: contenuta nell'header TCP.
- porta sorgente nuova: nuova porta scelta dal router.
Quindi al router basta guardare a quale indirizzo privato corrisponde l'indirizzo dell'host esterno. Se nota però che più indirizzi privati sono associati allo stesso indirizzo esterno, utilizzerà le porte sorgente e destinazione, contenute nell'header TCP. Nel corso della comunicazione, le porte verranno sostituite all'interno dell'header TCP nel payload. Questo meccanismo è detto __NAPT__. I record della tabella vengono eliminati allo scadere di un timer, che si rinnova ogni volta che viene utilizzato il record. Ovviamente un host esterno non può inviare il primo messaggio della comunicazione.

# Livello data-link
## Allocazione
Quando un pacchetto raggiunge il livello data-link, gli viene aggiunto un __header livello 2__, che verrà poi rimosso (e poi riaggiunto) dal livello data-link degli host successivi. Questo livello si occupa di risolvere 2 delicati compiti nella trasmissione:
- mezzo condiviso
- delimitazione delle trame

Nel secondo livello vengono usate 2 terminologie, ovvero la __trama__ che è l'informazione da trasmettere, il __periodo di vulnerabilità__ che è il lasso di tempo in cui possono avvenire collisioni tra le trame.<br>
Si parla di __mezzo condiviso__ quando gli host comunicano utilizzando lo stesso mezzo di propagazione del segnale, creando talvolta collisioni tra i segnali, compromettendo di fatto l'informazione. Un esempio è la tecnologia __wireless__ dove un'access-point comunica con più host attraverso onde-radio. La soluzione per evitare che più segnali si scontrino, è trasmetterli in tempi differenti, attraverso diverse tecniche di allocazione. Esistono tecniche definite __statiche__ che necessitano di conoscere fin da subito il numero di sorgenti, e __dinamiche__ che si dividono in tecniche a _turno_ e a _contesa_. In quelle a contesa, la prima sorgente che trova il canale libero trasmette, altrimenti resta in attesa. Le principali tecniche di allocazione a contesa sono:
- __ALOHA__:
1. Se ci sono dati da trasmettere vengono trasmessi. A questo punto ci si mette in ascolto del canale.
2. Si ci sono collisioni, viene estratto un tempo casuale tra (0,M), dove M è una costante molto maggiore del _tempo di trama_, si attende quel tempo e si ricomincia dal punto 1. Quando avvengono collisioni consecutive, viene raddoppiato M. Il tempo di vulnerabilità in questo caso è T*2.
- __slotted ALOHA__: uguale al precedente, ma la linea temporale è divisa in __slot__ di tempo T, ovvero il tempo di trama massimo. L'inizio delle trame dovrà coincidere con l'inizio degli slot. In questo modo il tempo di vulnerabilità scende a T.
- __CSMA persistent__:
1. Si ci sono dati da trasmettere, ascolta il canale.
2. Se il canale è libero, trasmette la trama, altrimenti aspetta che si liberi per poi trasmettere.
3. Se ci sono collisioni, estraggo un tempo casuale e torno al punto 1. Le collisioni avvengono se le sorgenti controllano nello stesso tempo se è libero il canale, e sarà proprio il tempo che ci impiegano il tempo di vulnerabilità.
- __CSMA non-persistent__: solo il punto 2 è differente: se il canale è occupato viene estratto un tempo casuale, e si riparte dal tempo 1.
- __CSMA p-persistent__: solo il punto 2 è differente: se il canale è occupato, si aspetta che il canale si liberi e poi, con probabilità "p" si trasmette, con probabilità "1/p" si attende un __microslot__ per trasmettere.
- __CSMA-CD__: solo il punto 3 è differente dalla persistent: se c'è collisione, interrompo la trasmissione, ed estraggo il tempo casuale, per poi tornare al punto 1.

## Indirizzi fisici
La tecnologia __ethernet__ (IEEE 802.3) presenta diverse tipologie:
- base: 10 Mbit/s.
- fast-ethernet: 100 Mbit/s
- gigabit-ethernet: 1 Gbit/s

Al giorno d'oggi viene utilizzato per collegare diversi apparati di rete, a velocità che si aggirano tra 10-40 Gbit/s (40 è più fibra ottica). Nella comunicazione verrà aggiunto un'header così formato:
- preambolo: serie di 0 e 1 alternati, che serve per far sincronizzare i clock dei due host in comunicazione.
- flag: byte che permettere di delimitare le trame.
- destinazione hardware: indirizzo MAC del destinatario.
- sorgente hardware: indirizzo MAC della sorgente.
- lunghezza payload.<br>
Alla fine del payload è presente un campo chiamato _trailer_ che conterrà la checksum.

Abbiamo parlato di __MAC__ ovvero l'indirizzo fisico dell'host, che viene assegnato alla sua scheda di rete quando viene prodotta. Il MAC è indipendente dall'indirizzo IP, ma è possibile ottenerlo attraverso quest'ultimo grazie al protocollo __ARP__:
1. arp-request: l'host manda in broadcast (livello 2) una richiesta ARP con l'indirizzo IP.
2. arp-reply: l'host con quell'indirizzo risponderà con il proprio indirizzo MAC.

## Lan estese
Una Lan non sempre si sviluppa in una singola stanza, ma spesso è condivisa da un'intero edificio. Per questo vengono utilizzati degli apparati di rete che permettono di collegare i vari calcolatori dei vari piani, estendendo di fatto la LAN, rendendola una __lan estesa__. Questi apparati sono:
- hub: può avere 2 o più porte, e lavora solo a livello 1, quindi si limiterà solo ad unire 2 collegamenti, facendo passare il segnale senza interpretarlo o modificarlo.
- bridge: può avere 2 porte, ed è un apparato di livello 2, quindi può controllare l'indirizzo MAC del destinatario e rigirare la trama dalla porta giusta.
- switch: apparato di livello 2, con 2 o più porte. A differenza del bride, gli host sono collegati direttamente con le porte dello switch, uno per ogni porta, e non attraverso il mezzo condiviso.

Ora è possibile definire 2 tipi di domini:
- dominio di broadcast: porzione di rete in cui si trasmette un messaggio di broadcast. Viene interrotto dal router.
- dominio di collisione: porzione di rete in cui si ha collisione tra sorgenti che trasmettono contemporaneamante. Viene interrotto da bridge e switch.

## CSMA-CA
Le __WLAN__ sono lan in cui viene utilizzata la tecnologia wireless, che si presenta con una delle 2 architetture:
- ad hoc: le varie strutture comunicano direttamente tra di loro.
- infrastruttura: presenta un'access-point come apparato di riferimento per la comunicazione. Gli host che si trovano nella __BSS__ overro l'area wireless dell'access-point, per comunicare tra di loro dovranno passare per esso.

Il tempo nelle wlan è diviso in time-slot detti __SIFS__, che sono proporzionali al ritardo di propagazione, e viene definito un intervallo __DIFS__ come una sequenza di 3 SIFS. Questi intervalli vengono sfruttati del algoritmo usato per la gestione del mezzo condiviso wireless, il __CSMA-CA__:
1. se il canale è libero, resta in ascolto per 1 DIFS, e se è ancora libero, trasmette.
2. se prima o durante il DIFS il canale è occupato, continuo ad ascoltare il canale fino a che non si libera
3. quando il canale si libera, ascolto per 1 DIFS, e se torna occupato, riparto dal punto 2.
4. se il canale rimane libero, viene estratto un numero "s" dall'intervallo "0-CW", che sarà il numero di SIFS da attendere. Se il canale rimane libero fino ad arrivare allo 0 ("s" si decrementa ad ogni SIFS), la sorgente trasmette. Se prima di arrivare allo 0, il canale torna occupato, viene congelato il valore di "s" e si torna al punto 2.
5. se avviene una collisione, viene interrotta la trasmissione, si attende un tempo casuale, e si torna al punto 1 con "CW" raddoppiato.<br>

Il CSMA_CA non riesce a risolvere il problema del __terminale nascosto__, dovuto al fatto che il segnale si espande in tutte le direzioni, comportando molta dissipazione. Una sorgente per capire se il canale è libero, o se c'è una collisione, verifica che riceve qualche segnale. Se 2 sorgenti sono troppo distanti tra loro, e sono entrambi fuori dal range dell'altro, non rileveranno che l'altro sta trasmettendo. Per risolvere questo problema, andrebbe limitato lo spazio di responsabilità di un access-point. Inoltre, viene introdotto un particolare scambio di messaggi:
- __RTS__: viene inviato da una sorgente che vuole trasmettere. Se l'access-point lo riceve, risponde con un CTS.
- __CTS__: viene inviato in broadcast dall'access-point. Se una sorgente non ha inviato precedentemente un RTS, quando riceve il CTS si ferma, al contrario di chi ha inviato un RTS, che trasmetterà.

Questo cambio di messaggi permette anche di rispariamre energia, destinata all'ascolto del canale, infatti nel CTS sono contenute diverse informazioni, tra cui: il destinatario della trasmissione, e il tempo. Quindi una sorgente che sà di non essere il destinatario di tale trasmissione, spegne il canale per quel dato tempo. L'energia può essere risparmiata anche leggendo il campo di "lunghezza payload", in modo da capire per quanto tempo si può spegnere l'ascolto del canale. Questo meccanismo è detto __NAV__.

## Delimitazione delle trame
Questo è il secondo compito che svolte il secondo livello. Siccome il livello fisico, quando riceve più trame in sequenza, traddurrà tutto in una sequenza di bit (ricevendo un'onda elettromagnetica unica), è necessario distringuere una trama dall'altra, mediante 3 tecniche:
- introdurre momenti di silenzio tra una trama all'altra. Con la propagazione il segnale si distorce, e questi momenti di silenzio potrebbero andare coperti.
- introdurre nell'header la dimensione della trama, ma se si verifica un errore in quei bit, tutte le trame successive verranno compromesse.
- usare il byte di flag. Viene aggiunta una sequenza di 8 bit, all'inizio e alla fine di ogni trama. In questo modo, se si verifica un errore in questi bit, verrà compromessa solo la trama successiva all'errore.

Nel caso in cui, nella trama sia presente una sequenza uguale a quella del _byte di flag_, viene addottato il __bit stuffing__, ovvero aggiungere in trasmissione un bit a 0 dopo la sequenza (non dopo il byte di flag), e rimuoverlo in ricezione.

- - -
# Acronimi
- - -
- MTU: maximum transfer unit
- MSS: maximum segment size
- RTT: roud trip time
- RTO: retrasmission time out
- TTL: time to live

- - -
# Header
- - -
## TCP
1. porta sorgente (2 byte), porta destinatario (2 byte)
2. sequence number (4 byte)
3. ack number (4 byte)
4. offset (4 bit), reserved (6 bit), flag (6 bit), window (2 byte)
5. checksum, urgent pointer

## IP
1. vers (4 bit), lunghezza header (4 bit), service type (8 bit), lunghezza totale(16 bit)
2. identificazione (16 bit), flags (3 bit), fragment offset (13 bit)
3. ttl (8 bit), type (8 bit), checksum (16 bit) 
4. ip sorgente (4 byte)
5. ip destinazione (4 byte)
- opzioni

## IPv6
1. version (4 bit), traffic class (8 bit), flow label (20 bit)
2. lunghezza payload (16 bit), next header (), hop limit ()
3. ip sorgente (128 bit)
4. ip destinazione (128 bit)

## Ethernet
1. preambolo (7 byte)
2. flag (1 byte)
3. MAC sorgente (6 byte)
4. MAC destinatario (6 byte)
5. lunghezza payload (2 byte)
6. trailer
