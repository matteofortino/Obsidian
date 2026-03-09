#uni
*How much time it take to distribute a file of size F from one server to N peers/clients?*
keep in mind that a peer upload/download capicity is a limited resources.
# Client Server
A server need to send a copy to every client so the total bits that it need to send is $NF$. 
If we call the throuhput of the server $u_s$ the time it takes 
to send n copies is $\frac{NF}{u_s}$.
Now every client need to download the copy.
We call $d_{min}$ the minimum client download rate.

So the time to distribute a file of size F to N clients is $$D_{c-s} \geq \max\left( \frac{NF}{u_s}, \frac{F}{d_{min}} \right)$$
# Peer to Peer
The peer that acts as a sever must upload at least one copy, the time it takes is $\frac{F}{u_s}$  
The peer that acts a client needs to download one copy, the. time it takes is $\frac{F}{d_{min}}$ 
The peers that acts as clients as an aggregate must download $NF$ bits, the max upload rate is $u_s + \sum{u_i}$ 

So the time to distribute a file of size F to N peers is $$D_{p2p} > \max\left(\frac{F}{u_s}, \frac{F}{d_{min}}, \frac{NF}{u_s+\sum{u_i}}\right)$$
# BitTorrent
It's a popular [[Peer to Peer (P2P)]] protocol for file distribution.
The collection of al peers participating in the distribution of a particula file is called a **torrent**. Peers in torrent download/upload equal size **chunks** (256Kb) of the file from one another.
When a peer first joins a torrent, it has no chunks. Over time it accumulates more and more chunks.
When a peer downloads the whole file it can *selfishly* leave the torrent or *altruisticaly* remain in the torrent and continue to upload chunks to other peers. Also, any peer may leave the torrent at any time with only a subset of chunks and leater rejoin.
A peer which is still downloading the file is called a **leacher**.
A peer which downloaded the file and it still on the torrent is called a **seeder**.
## Tracker
Each torrent has an infrastructure node called a **tracker** which keeps track of the number of peers inside the torrent.
When a peer joins a torrent, it registers itself with the tracker nad periodically informs the tracker that it is still in the torrent.

When a new peer *Alice*, joins the torrent the tracker randomly select a subset of peers and send their IP addresses to Alice.
Alice try to establish TCP connections with all the peers, the once she succesfully connects to are called **neighboring peers**
## Requesting chunks
To request chunks Alice choose a technique called **rarest first**. She periodically asks each peer for list of chunks that they have. Then from all those lists she request the chunk which is the rarest among her subset of peers.
## Sending chunks
To send chunks Alice use a tecnique called **tit for tat**. Alice sends chunks to those four peers who are sending to her at the highest rate. That means that other peers are **chocked** by Alice because they do not recieve from her. Every 30s randomly select a peer and starts sending chunk to *optimisticaly unchoke* that peer.