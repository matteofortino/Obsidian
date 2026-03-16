#uni 

In routers instradano i pacchetti in 3 modi diversi principali: 
1. *Routing by destination address*
	- I routers analizzazano l'indirizzo IP destinatario del pacchetto e consultano una **tabella di routing** per decidere il prossimo *hop*.
2. *Label swapping*
	- Caratterizzati da alta velocità instradamento e usati nelle **MPLS**(*MultiProtocol Label Switching*), dove i pacchetti sono instradati lungo percorsi predeterminati basati su corte e fissate etichette.
3. *Source routing*
	- Chi invia il messaggio è responsabile della determinazione dell'intero percorso che il pacchetto seguirà all'interno della reta per raggiungere il destinatario.

# Tabelle di routing
Abbiamo già visto che il compito principale di un router è:
- Prendere un pacchetto da una porta di ingresso
- Ricavare l'indirizzo IP del destinatario del tale pacchetto
- Consultare una **routing table** per determinare il prossimo "salto" da far fare al pacchetto tramite un operazione di **lookup**
- Instradare il pacchetto sulla porta di uscita

In una tabella di routing tipicamente troviamo: 
- L'indirzzo IP del next hop
- L'interfaccia (porta di uscita) per raggiungere il next hop.
- Il tempo trascorso da quanto il router conosce queste informazini. Dipende dal protocollo che si usa.

# Metriche di routing
Come scegliere il percorso migliore da far fare ad un pacchetto? Si guardano delle metriche, fra le quali il numero di HOP da fare e la larghezza di banda del link sul quale il pacchetto deve traversare.

I protocolli scelgono una delle metriche tipo:
- Protocollo *RIP* si basa sull hop count
- Protocollo *SPF* si basa sulla larghezza di banda

Una metrica imporante è la **distanza amministrativa**, non ha nulla a che fare con una misura reale, è una cosa decisa a tavolino e dipende dal router. Minore è il numero meglio è, indica quale algoritmo viene preferito. Ovviamente "Connected" ha costo zero.

Su packet tracer con il comando `show ip route` possiamo ottenere la tabella di routing di un router che ci mostra delle informazioni fra le quali il *codice* (C), la *distanza amministrativa*, l'*hop count* a altro.

# Route statiche
All'interno di un router si possono definire staticamente dei percorsi da far seguire hai pacchetti. Una volta definiti questi percorsi saranno inseriti nella routing table. 
I percorsi di routing vanno definiti in entrabe le vie in quanto se si devono fare più hop i router "di mezzo" devono sapere sia chi li precede che chi li segue
