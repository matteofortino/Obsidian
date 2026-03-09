---
number headings: auto, first-level 1, max 3, 1.1
---
#uni
*Goals*: 
- Introduce the concept of internetwork
- Understand the principles behind network layer services
- Instantiation, implementation in the internet
# 1 Network layer (overview)
The network layer transport **segment** from a sending to a receiving host
- *sender*: encapsulate segments into **datagrams**, passes to link layer
- *receiver*: delivers segments to transport layer protocol
There is a network layer protocol in **every internet device**
## 1.1 Routers
Routers only implement physical layer, link layer and network layer. 
A router examines header fields in all IP datagrams passing through it and moves datagrams from input ports to output ports to transfer datagrams along the end-end path.
## 1.2 Network layer functions
- **forwarding**: move packets from a router's input link to appropriate router output link
- **routing**: determine the route taken by packets from source to destination
### 1.2.1 data plane
**data plane** it's local, determines how datagrams arring on the router input port are forwarded to the router output port
### 1.2.2 control plane
**control plane**: it's a network-wide logic, determines how datagrams are routed among routers along end-end path from source host to destination host.

there are two control-plane approaches:
	- *traditional routing alogritms*
	- *software-defined networking* (**SDN**): 
		- the forwarding table is is filled by the servers, not by the routers

The [[#1.2.2 control plane]]e filles the forwarding table and the [[#1.2.1 data plane]] uses it.

# 2 Inside a Router
The **routering processor** is responsible for the control plane (software)
The **high-speed switching fabric** is responsibile for the data plane (hardware), operates in a nanosecond ($ns$) timeframe
## 2.1 Router input ports
> router input port: ![[input_port.svg | 600]]

- *destination-based forwarding*: the forward is based only on the desination IP address (traditional)
- *genralized forwarding*: the forward is based on any set of header field values
### 2.1.1 Destination-Based forwarding
#### Longest Prefix Matching
When looking for the entry of the forwarding table for a given destination address, we use the *longest* address prefix that matches the destination address.
This is often preformed using ternary content addressable memories (TCAMs): 
- present the address to TCAM and in one cycle retrive the address, regardless of table size
## 2.2 Switching Fabrics
They are used to transfer packets from input link to the appropriate output link.
The rate at which the packes can be transfered from inputs to outputs is called **switching rate**. 
- often measured as multiple input/output line rate 
- $N$ inputs: switching rate $N$ times line rate desirable
There are three major types of switching fabrics: 
1. ***memory***: first generation routers
	- traditional computers with switching under direct control of the CPU
	- packets were copied to system's memory
	- speed limited by memory bandwidth
2. ***bus***
	- datagram is transfered via bus
	- **bus contention**: switching speed limited by bus bandwith
3. ***interconnection** **network***:
	- Crossbar, Clos networks, other interconnection nets initally developed to connect processors in multiprocessor
	- **multistage switch**: NxN switch from multiple stages of smaller switches
	- **exploiting parallelism**: 
		- fragment datageram into fixed lenght cells on entry
		- switch cells through the fabric, reassemble datagram at exit
		- scaling: using multiple switching planes in parallel
## 2.3 Input port queueing
If the switching fabris is slower than the input ports combined -> queueing may occur!
**Head-Of-the-Line (HOL) blocking**: queued datagram at fron of queue prevents other in queue from moving forward
## 2.4 Output port queueing 
- **buffering**: required when datagrams arrive from fabric faster that link trasmission rate. 
	- There is a need of a *drop policy* which allow the router to know with datagrams to drop if there are no free buffers.
- **Scheduling discipline**: chooses among datagrams for transmission
### 2.4.1 Buffer management
- **drop**: which packet to add, drop when buffers are full
	- *tail drop*: drop the arriving packet
	- *priority*: drop/remove on priority basis
- **marking**: which packets to mark to signal congestion (ECN, RED).
## 2.5 Packet Scheduling
**packet scheduling**: deciding which packet to send next on the link
there are different approaches:
- first come, first served
- priority
- round robin
- weighted fair queueing
### 2.5.1 Priority Scheduling
The arriving traffic is classified, queued by class
- any header fields can be used for calssification
The packets are sent from the highest priority queue, within the queue FCFS is used.
### 2.5.2 Round Robin Scheduling
The arriving traffic is classified, queued by class
- any header fields can be used for classification
The server cyclically and repeatedly scans class queues, sending one complete packets from each class (if available) in turn
### 2.5.3 Weighted Fair Queueing Scheduling
**WFQ** is a generalization of round robin
- each class $i$ has a weight $w_i$ and gets its weighted amount of service in each cycle: $\frac{w_i}{\sum_j{w_j}}$
- mimimun bandwith guarantee per traffic-class
# 3 IP: the internet protocol
## 3.1 IP Datagram Format
![[ip_datagram_formato.png]]
## 3.2 IP Fragmentation & Reassembly
Since the datagram length can vary (max 64k bytes, typically 1500 bytes) and the limited link capacity, how can a router forward all datagram at once?
The solution is called **Fragmentation**, the router split the datagram in different frames and forward one at a time.
- *in:* one large datagram
- *out*: 3 smaller datagrams
This is were the secon row fields come in handy.
The datagram has and **16-bit identifier** so when it gets split we still know at which datagram the fragments belong.
The **fragmentation offest** is use to reasseble di datagram in the correct order.

Network links have and **MTU** (*Max Transfer Unit*) which vary between different links. If a datagram in bigger and the link MTU it needs to get framgmented.

The initial datagram is reassembled only at the final destiantion.
If some fragments do not arrive within a certain **timeout** the datagram is discarded.
## 3.3 IP addressing: introduction
And **IP address** is a *32-bit identifier* associated with each host or router interface.
And **interface** is a connection between host/router and physical link
- routers typically have multiple interfaces
- host typically has one or two interfaces
## 3.4 Subnets
*What's a subnet?*
**Subnets** are device interfaces that can physically reach each other without passing through an intervening router

An IP address has a structure:
- **subnet part**: devices in same subnet have common high order bits
- **host part**: *remaining* low order bits

There is a recepe for defining subnets:
- detach each interface from it host or router creating "islands" of isolted network.
- each isolated network is called a subnet.
## 3.5 IP addressing: CIDR
**CIDR**: **C**lassless **I**nter**D**omain **R**outing (pronounced "cider"). 
The subnet portion of address of arbitrary lenght
The format *a.b.c.d/x* where x is the number of bits in the subnet portion of address
![[subnet_address_format.png]]
## 3.6 IP addresses: how to get one?
That is actually two questions: 
1. How does a *host* get an IP address within its network?(host part of the address)
2. How does a network get an IP address for itself?(network part of the address)
Starting with the first one we have two options: 
- It's hard-coded by system admin in the configuration file (/etc/rc.config in UNIX)
- **DHCP**: Dynamic Host Configuration Protocol which dynamically get addresses from a server
	- "plug-and-play"
### 3.6.1 DHCP: Dynamic Host Configuration Protocol
*goal*: Host dynamically obtains IP address ffrom the network server when it "joins" the network.
- can renew its lease on address in use
- allows reuse of address (only hold address while connected/on)
- support for mobile users who join/leave network
**overview**:
- Host broadcasts DHCP discover msg (optional)
- DHCP server respond with DHCP offer msg (optional)
- Host request IP address: DHCP request msg
- DHCP server sends addresses: DHCP ack msg

DHCP in general can return more than just allocated IP address on a subnet. It also include: 
- address of the first-hop router for a client
- name and IP address of DNS server
- network mask
## 3.7 IP addresses: how to get one?
Now the second question:
- How does a network get an IP address for itself? (network part of the address)
- 
The network gets allocated a portion of its **ISP**(*Internet Service Provider*) address space.
Example: 
- ISP's block: 11001000 00010111 00010000 00000000 200.23.16.0/20
- ISP can allocate out its address space in 8 blocks
	- Organization 0 11001000 00010111 00010000 00000000 200.23.16.0/23
	- Organization 1 11001000 00010111 00010010 00000000 200.23.18.0/23
and so on ... 
## 3.8 IP addresses: Last words
*How does and IPS get his block of IP addresses?*
The **I**nternet **C**orporation for **A**ssigned **N**ames and **N**umbers (**ICANN**):
- allocates IP addresses through 5 regional registers (**RRs**)
- manages [[DNS]] root zone, including delegarion of individual TLD
*Are there enough 32-bit IP addresses?*
- ICANN allocated last chunk of IPv4 to RRs in 2011
- NAT helps IPv4 addresses space exhaustion
- IPv6 has a 128-bit addres space
# 4 NAT: Network address translation
All devices in a local network have 32-bit addresses in a "private" IP address space (10/8, 172.16/12, 192.168/16 prefixes) that can onlu be used in a local network
## 4.1 NAT: advatages
- There is just **one** IP address need from the ISP for all the devices
- It can change addresses of host in local network without notifying the outside world
- The IPS can be changed without changing addresses of devices in local network
- The devices inside the local network are not directly visible by the outside world
## 4.2 NAT: implementations
NAT router must (transparently):
- **outgoing datagrams**: *replace* (source IP, port #) of every outgoing datagram. to (NAT IP, new port #)
- **remember** (NAT Translation Table): every (source IP, port #) to (NAT IP, new port #) translation pair
- **incoming datagrams**: *replace* (NAT IP, new port #) in destination fields of every incoming datagram with corresponding (source IP, port #) stored in NAT table.
# 5 IPv6
## 5.1 IPv6: datagram format
![[ipv6_datagram_format.png]]
*What is missing compared to IPv4?*
- **no checksum** (to speed up processing at the router)
- **no fragmentation/reassembly**
- **no options** (available as upper-layer)
## 5.2 IPv6: transition
Not all routers can be upgraded simultaneously, so how will the network operate with mixed IPv4 and IPv6 routers?

The answer is **tunneling**: IPv6 datagram are carried as *payload* in IPv4 datagram amoung IPv4 routers.
# 6 ARP: Address Resolution Protocol
*How can we determine the MAC address of B knowing B's IP address?*
- each IP node has a ARP table (<IP; MAC; TTL>).
- TTL (time to live): time after which address mapping will be forgotten (typically 20 mins)
## 6.1 ARP: same LAN
- A wants to send datagram to B, and B's MAC address is not in A's ARP table. 
- A **broadcast** ARP query packet, containig B's IP address
	- all machines on LAN recives the ARP query
- B receivers ARP packet
- B replies to A with its MAC address
- A cahces IP-to-MAC address pair in its ARP table until the information becomes old
## 6.2 ARP: another LAN
- A creates IP datagram with source A, destination B
- A uses ARP to get R's MAC address as dest , frame contains A-to-B IP datagram
- A's NIC sends frame
- R's NIC receives frame
- R removes IP datagram from Ethernet frame, sees its destined to B
- R uses ARP to get B's MAC address 
- R creates frame containing A-to-B IP datagram and sends it to B
# 7 ICMP Protocol
Used by host and routers to communicate network-level information
- error reporting
- echo request/reply
ICMP message is carried inside an IP datagram
ICMP message has:
- type
- code
- header + first 8 bytes of IP datagram causing error
# 8 Generalized forwarding: match plus action
*Review*: 
- each routers contains a **forwarding table** (aka flow table)
**Match plus actions** abstactions: match the bits in arriving packets, take actions
- *destination-based forwarding*: forward bases on dest. IP address 
- *generalized forwarding*
	- many headers fields cna determine action
	- many action possible: drop/copy/modify/log packet
## 8.1 Flow table abstaction
**flow**: defined by header fields values
**generalized forwarding**: simple packet-handling rules
- *match*: pattern values in packet header fields
- *actions*: for matched packet: drop, forward, modify matched packet or send matched packet to controller
- *priority*: disambiguate overlapping patterns
- *conters*: # bytes and # packets

## 8.2 OpenFlow
flow table entries:
![[flow_table_entries.png]]
**Router**: 
- *match*: longest destination IP prefix
- *action*: forward out a link
**Switch**:
- *match*: destination MAC address
- *action*: forward a flood
**FireWall**:
- *match*: IP addresses and TCP/UDP port numbers
- *action*: permit or deny
**Nat**:
- *match*: IP address and port
- *action*: rewrite address and port
# 9 MiddleBoxes
*definition*: 
any intermediary box performing functions apart from normal, standard function of an IP router on the data path between a source host and a destination host
![[middelboxes.png]]
- Initally they were **proprietary** hardware solutions
- Then moved towards "**whitebox**" hardware implementing open API
	- **programmable local actions** via [[#8 Generalized forwarding match plus action]]
- **SDN**: centralized control and configuration management often in private/public cloud
- **NVF** (network functions virtualization): programmable services over with box networking, computation, storage