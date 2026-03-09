#uni
A DNS (Domain Name System) is a system to map between IP addres and name and vice versa.
It a *distributed database* implemented in hierarchy of many *name servers*. A name needs top map to a single IP address.
There is an *application-layer protocol*: hosts, name-servers communicate to resolve names (address/name translation)
# Translation
Two types of translation:
- **Strandard**: hostname to IP address
- **Host aliasing**: common used name to real name to IP address
**NOTE**: Host aliasing takes to steps to resolve the name.

DNSs help to alleviete traffic like web servers since many IP addresses correspond to one name
# Hierarchy
*Why not centralize DNS?*
There are various reasons:
- single point of faliure
- more traffic volume
- distand centralized database
- maintenance
- doesn't scale
	- Comcast DNS servers alone: 600DB DNS queries per day
Ence, DNS servers are **distributed** and stored in an **hierarchical** database.
- **Root**: Root DNS Servers
- **Top Leven Domain**: .com, .org, .edu DNS Servers
- **Authoritative**: yhaoo.com, pbs.org, umass.edu DNS Servers
**NOTE**: only authoritative DNS servers can resolve names.

So if a client wants the IP addres of www.amazon.com: 
	- client queries root server to find .com DNS server
	- client queries .com DNS server to get amazon.com DNS server
	- client queries amazon.com DNS server to get IP address for www.amazon.com
# ICANN
The distributed DNS server is managed by  **Internet Corporation for Assigned Names and Numbers** (*ICANN*), which also defines what the **top layer domains** are.
# Local DNS name servers
Also called default name server, these are installed into each ISP and they act as a sort of proxy DNS server, they have a local cache of recent name-to-address translations pairs, BUT it may be out of date!

# DNS Records
A DNS is a distributed databse storing records (**RR**)
RR format: (name, value, type, ttl):
- type = A
	- name is a **hostname**
	- value is **IP address**
- type = CNAME
	- name is an **alias** name for come "*canonical*" (the real) name
	- valie is the **canonical name**
- type = NS
	- name is the **domain**
	- value is the **hostname of an authoritative name server** for this domain
- type = MX
	- value is the **name of the mailserver** associated with the name
# DNS Protocol Messages
The local DNS is client-server application which uses the UDP comminication protocol.
DNS *query* and *reply* have the same format:
- message header:
	- identification: 16 bit (ID) for query and reply
	- flags:
		- query or reply
		- recursion desired
		- recursion available
		- reply is authoritative

| Identification  | Flags            |
| --------------- | ---------------- |
| # questions     | # answer RRs     |
| # authority RRs | # additional RRs |
| questions       |                  |
| answers         |                  |
| authority       |                  |
| additional info |                  |
# Inserting Records into DNS
1. register name (name.topleveldomain) at *DNS registar* 
	1. provide names, Ip addresses of authoritative name server
	2. registar inserts NS (Name server) and A (Authoritative) RRs into .com TLD server.
2. create authoritative server locally with ip address (your address)