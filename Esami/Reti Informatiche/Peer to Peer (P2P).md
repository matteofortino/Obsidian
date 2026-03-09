#uni 
Every peer looks more like a powerful client from the Client-Server paradigm.
But technically every peer is on the same level.
The availability of resources this way is not guaranteed from a powerful server, but from the sheer number of peers -> *self scaliability* 
There isn't an always on server.
End systems arbitrarily communicate with each other.

 >Peers request service from other peers, providing service to other peers in return.
 
Indexes are needed for mapping information to the right locations.
# Applications
## P2P File Sharing 
Peers make files available to share. Interested peers need to know where a desired file can be downloaded from.
## Instant Messaging
Username have to be mapped to IP addresses.
## Indexes
 A content index is a *database* with (key,value) **pairs**
 - key: content type
 - value: IP address
Peers query DB with a key and the DB return the matching value. 
Peers can also *insert* (key,value) pairs.
### Centralized Index
This service is provided by a server (or a server farm)
When a user become active the application notifies the index with its IP address and list of available files.
This system allows drowbacks: 
- Single point of failure 
- Performance bottleneck
- Copyright infringment
### Query Flooding
This is a completely decentralized approach.
When a peer makes a query it connects to other peers that flood the request to other peers and so on until the item is found and the asking peer starts the download.
This behaviour creates an **Overlay Network** with nodes (*peers*) and edges (*TCP connections*).
**Limited-Scope query flooding**:
- Reduces query traffic
- Decreases the probability to locate the content
This happens because the query flooding stop at a predefined "horizon"
### Heirachical Overlay
It combines the best features of the previous approaces
- No central server
- Not all peers are equal
It uses **Super Nodes (SN)**
- Peers with high-bandwidth connection and high availability
- Ordinary peers connects to Sns 
- Each SN has a local index
	- Peers informs their SN about content
	- SNs form a SN overlay net
- Peers query their SN
- Download form a single peer
### Distributed Hash Table (DHT)
It's a simple database, with the database records begin distributed over the peers in a P2P system, DHTs have been widley implemented (*eMule, BitTorrent*) and have been the subject of extensive reaserch.
