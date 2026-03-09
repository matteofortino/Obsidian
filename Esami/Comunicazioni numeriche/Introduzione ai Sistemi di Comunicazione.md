#uni 
# Teoria dell'informazione
La **teoria dell'informazione** viene formalizzata da Claude Shannon a partire dal 1948. Quello che si voleva calcolare era la **capacità** di un canale di comunicazione, ovvero quanti bit al secondo per una certa frequenza si possono trasmettere.

>Teorema di *Shannon-Hartley* $$\text{Capacity} = log_2
\left(1 + \frac{\text{Signal}}{\text{Noise}}\right)  bit/s/Hz$$

La **banda** è la frequenza a cui siamo capaci di trasmette informazione sul mezzo, mentre chiamiamo **throughput** la frequenza di trasmissione di informazione effettiva che otteniamo.
La capacità non è altro che il throughput che otteniamo sulla banda data $$ \text{Capacity} = \frac{\text{Throughput}}{\text{Bandwith}} $$
Il *rapporto segnale-rumore*(**SNR**) è fondamentale per determinatre la cosiddetta capacità *raggiungibile* dei sistemi di comunicazione.
![[capacita_raggiungibile.png]]
Shannon ci dice quale è la capacità massima raggiungibile ma non ci da nessuna informazione su come raggiungerla.
# Elementi di un sistema di comunicazione
![[sistema_di_comunicazione.png]]
Questo sistema è composto da: 
- un **trasmettitore** che trasmette l'informazione proveniente da una sorgente sotto forma di *segnale*
- Un **mezzo** attraverso il quale si propaga il segnale
- Un **ricevitore** che riceve il segnale e lo riporta in informazione per consegnarla all'*utente* dell'informazione

Possiamo vedere il sistema più nel dettaglio:
![[sistema_di_comunicazione_dettagliato.png]]
Vediamo i componenti da cui è composto il **trasmettitore**:
- **Source encoder**: sfrutta la *ridondanza* del messaggio per ridurne le dimensioni senza perdere troppa informazione
- **Channel encoder**: intruduce ridondanza all'interno del messagio per compensare gli errori che verrano commessi sul *canale di propagazione*.
- **Modulator**: converte il segnale digitale in ingresso in una *forma d'onda* adatta ad essere trasmessa.
# Modello OSI
Il modello **OSI**(*Open System Interconnect*) fornisce uno standard per l'organizzazione su *livelli* di un sistema di comunicazione.
Fino ad ora noi abbiamo esaminato il livello *fisico*.    
![[modello_osi.png]]
Il modello OSi prevede 7 livelli: 
- Livello **application**, dove operano le applicazione vere e proprie
- Livello **presentation**, trasforma i dati in ingresso in un formato comprensibile alle macchine degli utenti
- Livello **session**, gestisce le *sessioni* degli utenti
- Livello **Network**, permetto l'*interconnesione* di reti attraverso un formato comune 
- Livello **datalink**, si prende carico del trasferimento dei dati alle macchine utente. 
- Livello **physical**, *mezzo fisico* vero e proprio
Questo modello ha la particolarità di essere *modulare* e quindi facile da espandere.
# Strutture delle reti
Le reti sono uno serie di **nodi** interconnessi fra loro. 
Esistono due tipi principali di rete:
1. Rete a **commutazione di circuito**, dove le interconnesioni fra i nodi che collegano due stazioni in comunicazione vengono allocate completamente a quella comunicazione, impossibilitando altri ad usare quella comunicazione.
2. Rete a **commutazione di pacchetto**, dove sulla rete non circolano segnali ma *pacchetti*, cioè elementi finiti di informazione che vengono ricavati dal segnale vero e proprio e possono quindi viaggiare in maniera *interlacciata* sullo stesso mezzo.
La rete **Internet** è una rete a commutazione di pacchetto.

## Modalità di comunicazione
 Su una rete si possono avere diverse modalità di comunicazione:
 - **Broadcasting**, dove un singolo nodo trasmette a più ricevitori
 - **Point-to-point**, dove la comunicazione avviene su un singolo link fra trasmettitore e ricevitore
 - **Point-to-multipoint**, dove la comunicazione aviene su un link fra un trasmettitore e più ricevitori
## Accesso multiplo
I mezzi che sono condivisi tra più trasmettitori hanno bisogno di un **MAC**(*Medium Access Control*) per garantire:
- Divisione del mezzo fra trasmettitori
- Sicurezza da *collisioni* fra le comunicazioni dei trasmettitori

Esistono diversi approcci per effetturare l'accesso condiviso:
- **TDMA**(*Time Division Multiple Access*): si divide l'utilizzo del mezzo in più *slot* temporali.
- **FDMA**(*Frequency Division Multiple Access*): si divide lo *spettro* del mezzo in più *bande* di frequenza.
- **CDMA**(*Code Division Multiple Access*): si associa ad ogni dispositivo trasmettitore un *codice* univoco. I codici sono progettati in modo da non interferire tra di loro se più dispositivi comunicano contemporaneamente. 
- **SDMA**(*Space Division Multiple Access*): si usa la separazione spaziale degli utenti per effetturare la condivisione del mezzo. Si possono allocare più slot prevedento più antenne per dispositivo. 
## Spettro delle frequenze
Lo spettro delle frequenze usate per le comunicazioni *wireless* va da i 3Hz ai 300GHz (sopra i 300GHz l'*attenuazione* del segnale è troppo alta per dare risultati utili)

| **Nome** | **Banda(Frequenze)** |                   **Uso**                   |
| :------- | :------------------: | :-----------------------------------------: |
| ELF      |       3-30 Hz        |          Comunicazioni sottomarine          |
| SLF      |      30-300 Hz       |          Comuniazioni sottomarine           |
| ULF      |    300 Hz- 3 kHz     |      Comunicazioni speciali, geofisica      |
| VLF      |       3-30 kHz       |     Navigazione, comunicazioni militari     |
| LF       |      30-300 kHz      |           Radiofari, navigazione            |
| MF       |   300 kHz - 3 MHz    |                  Radio AM                   |
| HF       |      3 - 30 MHz      | Onde corte, comunicazioni a lunga distanza  |
| VHF      |      30-300 MHz      |             FM, TV, areonautica             |
| UHF      |   300 MHz - 3 GHz    |        TV digitale, cellulari, Wi-Fi        |
| SHF      |      3 - 30 GHz      |         Radar, satelliti, mircoonde         |
| EHF      |     30 - 300 GHz     | Comunicazioni ad onde millimetrali, ricerca |
