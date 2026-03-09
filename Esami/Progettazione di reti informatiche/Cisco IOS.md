#uni 

# Routers 
Sono dei dispositivi specializzati nell'inviare pacchetti sulle reti. Sono responsabili per l'*interconnesione* fra reti selezionando il percorso migliore verso la destinazione per il singolo pacchetto.

In genere interconnettono le **LAN**(*Local Area Network*), con cavi di tipo *ethernet* con le **WAN**(*Wide Area Network*), realizzate con protocolli *point-to-point* o *seriali*.

I router si occupano di fare port forwarding: 
- Ricevono un pacchetto con all'interno l'indirizzo IP di destinazione
- Controlla nella sua *routing-table* l'interfaccia destinataria per il pacchetto
- Incapsula il vecchio pacchetto in quello nuovo e lo invia sulla rete.
## Routing tables
Le routing tables contengono i dati relativi agli indirizzi di destinazione dei pacchetti.
Queste tabelle possono essere riempite in due modi:
- *Statico*: i record della tabella vengono inseriti manualmente.
- *Protocolli di routing*: che possono essere **intra-area** o **inter-area**

# Sistemi operativi dei dispositivi di rete
Per anni e ancora oggi i principali fornitori di *sistemi operativi per dispositivi di rete* sono Cisco e Juniper che implementano rispettivamente: 
- Cisco Internetwork Operating System (*C-IOS*)
- Juniper Network Operating System (*JuNOS*)
anche se negli ultimi 10 anni la situazione si è spostata in favore di competitor più piccoli gli standar seguiti sono quelli di Cisco e Juniper.

I principali servizi offerti sono:
- Sicurezza
- Addressing
- Interfacce
- Routing
- QoS
- Gestione delle risorse

## Accesso ai dispositivi
L'accesso per la configurazione di questi dispostivi è spesso offerto tramite **CLI**(*Command Line Interface*).

Questo corrisponde ad avere un *end-host* collegato ad un interfaccia seriale tramite la quale si possono mandare comandi di *configurazione* del dispositivo di rete.

Nelle versioni moderne si può accedere in remoto tramite protocolli non sicuri com **telnet** o con maggior sicurezza tramite **SSH**

## Interfaccia
Cisco IOS (e molti altri) mettono a disposizine una **interfaccia modale**. In una modilità si ha un determinato *livello di privilegio* a cui sono permessi un sottoinsieme di *direttive*. 

L'interfaccia mostra a quale dispositivo si è connessi e la modalità corrente.

Il simbolo `>` indica la modalità senza privilegi, `#` indica invece la modalità privilegiata.

Con i comandi `enable` e `disable` si passa dalla modalità senza privilegi a quella privilegiata e viceversa. Spesso questo passaggio è protetto da password per evitare malintenzionati a cambiare configurazione.

Con il comando `configure terminal` si passa in modalità di configurazione dalla queli si può appunto configurare un dispositivo di rete.

