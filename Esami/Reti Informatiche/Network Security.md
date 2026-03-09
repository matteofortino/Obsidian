---
number headings: auto, first-level 1, max 3, 1.1
---
#uni 
# 1 What is network security (8.1)
Network security consists of:
- **confidentiality**: only the sender and the intended receiver should "understand" the message contents. This is achieved through encryption.
- **End-point authentication**: the sender and the receiver want to confirm the identity of each other.
- **message integrity**: the sender and the receiver want to ensure that message is not altered, in transit or afterwards, without detection.
- **Operational security: access and availability**: services must be accessible and available to the users.
## 1.1 Actors: Alice, Bob, Trudy
Bob and Alice want to communicate securely, but Trudy may intercept, delete and add messages. Trudy listens on the channel that Alice and Bob use to communicate.
Bob and Alice are every couple of hosts on the internet that want to communicate.
### 1.1.1 Trudy
Trudy can:
- eavesdrop: intercept messages
- insert messages
- impersonification: spoofing the source address of a packet
- hijacking: take over an ongoing connection by taking the place of either the sender or the receiver
- denial of service: prevent the service from being used by others, for example by overloading resources
# 2 Principles of cryptography (8.2)
See [[Crittografia]].
## 2.1 Language of cryptography
Alice's plaintext gets encrypted by a encryption algorithm using Alice's encryption key. The ciphertext travels through the channel and gets delivered to Bob, then gets decrypted into plaintext by a decryption algorithm using Bob's decryption key.
$$
\begin{matrix}
m: \text{ plaintext message} \\
K_A(m): \text{ ciphertext, encrypted with the key } K_A \\ 
m=K_B(K_A(m))
\end{matrix}
$$
## 2.2 Breaking encryption
There are different ways encryption can be broken depending on the information available to Trudy:
- Trudy only has the plaintext transmitted over the insecure channel, they need to decrypt it, there are two approaches to do so:
	- brute force
	- statistical analysis
- known plaintext: Trudy knows some (plaintext , ciphertext) pairs
## 2.3 Symmetric Key cryptography (8.2.1)
In Symmetric key cryptography Bob and Alice share the same key $k$ (hence "symmetric"). $K_A = K_B = K$.
Alice and Bob need to agree on the value of the key, without leaking it to third parties.
### 2.3.1 Stream ciphers
In this approach we encrypt one bit at a time.
Used for SSL/TLS connections, bluetooth connections and cellular and 4G connections.
#### 2.3.1.1 Substitution cipher
Substituting letters:
- *Monoalphabetic cipher*: substitute one letter for another. here the encryption key is the mapping between letters
### 2.3.2 Block Ciphers
In this approach plaintext messages get broken into equal-size blocks, then each block gets encrypted as a unit.
#### 2.3.2.1 DES: data encryption standard
This is the US encryption standard. It uses a 56-bit symmetric key and 64-bit plaintext input, it is a block cipher with cipher block chaining.
It has been brute forced in less than a day, but there is no known good analytic attack.
It gets more secure with ...
#### 2.3.2.2 AES: advanced encryption standard
This is a symmetric-key NIST standard that replaced DES in November 2001.
Blocks are 128-bit wide.
Keys can be 128, 192 or 256 bit.
Brute force decryption that takes one second on DES, takes 149 trillion years on AES.
### 2.3.3 How entities establish a key
Symmetric key cryptography algorithms need the two communicating entities to establish a shared secret key.
Possible solutions are:
- **direct exchange**
- **key distribution center** (**KDC**): a trusted entity acts as an intermediary between the two entities
- **using public key cryptography** for communicating the key
#### 2.3.3.1 Key distribution center
Alice and Bob know their own symmetric key ($K_{A-KDC}$ and $K_{B-KDC}$) for communicating with the KDC. This keys need to be retrieved in person at the KDC.
1. Alice sends to the KDC $K_{A-KDC}(A,B)$
2. The KDC responds with $K_{A-KDC}(R1, K_{B-KDC}(A,R1))$, now Alice knows the session key $R1$ 
3. Alice now sends Bob $K_{B-KDC}(A,R1)$, now Bob knows the session key $R1$
4. Alice and Bob communicate using the shared session key $R1$ 
## 2.4 Public Key Cryptography
Public Key cryptography uses a radically different approach: the sender and the receiver do not share the secret key, every user now has a **public** encryption key, known to all, and a **private** decryption key known only to the receiver.
Alice encrypts the plaintext with Bob's *public* key $K_B^+$, Bob decrypts the message using his *private key* $K_B^-$: $m=K_B^-(K_B^+(m))$.

Unfortunately public key cryptography is much more computationally demanding than symmetric cryptography: 100 times slower than DES.
Because of this we use public key cryptography to establish a symmetric session key.
Bob and Alice use RSA to exchange a symmetric session key $K_S$, then use $K_S$ for symmetric key cryptography communication.
### 2.4.1 Public key Cryptography algorithms
Requirements:
- $m=K_B^-(K_B^+(m))$ 
- it should be impossible to compute the private key from the given public key
another important property is the following: $K_B^-(K_B^+(m))=m=K_B^+(K_B^-(m))$.
#### 2.4.1.1 RSA: Rivest, Shamir, Adleman
Nel 1977 Rivest, Shamir e Adleman propongono un sistema a chiave pubblica con funzione one-way trap-door. Turing Award 2002.
# 3 Message integrity (8.3)
Message integrity consists in allowing parties to verify that the received messages are authentic:
- the sender is who you think it is
- the contents are what the sender intended them to be
- message has not been replayed
- the sequence of the messages is maintained
## 3.1 Message digests
To implement this we introduce **message digests**: fixed-length, easy-to-compute digital footprints.
We apply a hash function $H$ to $m$ and get a fixed-size message digest $H(m)$.

Hash function properties:
- **easy to compute**
- **irreversibility**: given message digest $x$, it must be unfeasible to find $m$ such that $x=H(m)$
- **collision resistance**: given $[m,H(m)]$ it must be *computationally unfeasible* to produce $m'$ ($m' \neq m$) such that $H(m')=H(m)$.
  Si può mappare uno spazio infinito ad uno spazio finito, pur garantendo questa proprietà? Ovviamente NO ("attacco di buon compleanno"). Hence "computationally unfeasible" and not "impossible".
- **seemingly random output**
### 3.1.1 Internet checksum
The internet checksum has some of the properties of a hash function:
- fixed length output
- is many-to-one
But, given a message with its hash value, it is easy to find another message with the same hash value.
### 3.1.2 Hash function algorithms (8.3.1)
#### 3.1.2.1 MD5
MD5 computes a 128-bit message digest in a 4-step process.
Given an arbitrary 128-bit string $x$, it appears difficult to find the message digest.
#### 3.1.2.2 SHA-1
SHA-1 is the US standard. It produces a 160-bit message digest.
### 3.1.3 MAC: Message Authentication Code (8.3.2)
Using the MAC we can authenticate the sender and verify the message integrity. 
This approach uses no encryption and is also called "keyed hash".

We introduce a **shared secret**: a string only known to sender and receiver.
1. The sender computes the MAC of `< message , shared secret >` ($H(s,m)$).
2. the message and the MAC travel to the receiver
3. the receiver again computes the MAC of `< received message , shared secret >`, then compares it with the received MAC
4. if the two MACs are equal the message has not been altered AND the sender is the expected one.
#### 3.1.3.1 Playback attack
More precisely if the two MACs correspond, we know that the MAC was *generated* by Alice, but we don't know if the `< message , MAC >` was *transmitted* by Alice.

Trudy can intercept the message, and play it back to Bob.
##### 3.1.3.1.1 Defense
To defend from the "playback attack", we include the timestamp of generation in the message before computing the MAC. But this is quite difficult to implement, because sender and receiver clock may not be synchronized. The real solution is using **OTP** (one time password).

1. Alice contacts Bob
2. Bob sends an OTP $R$
3. Alice computes the MAC as follows: $\text{MAC}=H(R,s,m)$
4. Bob expects ONE message PER sent OTP
## 3.2 Digital signatures (8.3.3)
Digital signatures are a cryptographic technique that allows the sender to digitally sign a document.
A digital signature is:
- **verifiable**: the recipient can verify and prove that the sender and no one else signed the document
- **non-forgeable** 
And allows:
- **non repudiation**: the recipient can prove that the sender signed it
- **message integrity**

> MAC cannot be used as a digital signature because the secret is *shared* between the two communicating.

For authentication we use asymmetric cryptography ([[#2.4 Public Key Cryptography]]):
1. Bob signs $m$ by encrypting with his private key $K_B^-$ 
2. Bob sends the signed message $K^-_B(m)$
3. Alice can verify that the sender is Bob if the message can be decrypted with Bob's public key $K_B^+$ 

But as we said asymmetric cryptography is computationally demanding, so we sign the digest, not the message (the message can be arbitrarily long!):
1. Bob computes the digest: $H(m)$
2. Bob signes (encrypts) the digest with its private key
3. Bob sends the message $m$ with the encrypted message digest $K^-_B(H(m))$
4. Alice computes the digest of the received message: $H(m)$ 
5. Alice decrypts the received digest using Bob's public key and obtains (hopefully) the message digest $H'(m)$
6. Alice compares $H(m)$ and the decrypted messages digest $H'(m)$, if they are equal the message and the sender can be trusted.
### 3.2.1 CA: public key certification authorities
Who guarantees that Alice actually has Bob's public key?
A **certification authority** (**CA**) binds a public key to a particular entity $E$.
The entity $E$ registers its public key with the CA, providing a "proof of identity".
1. The CA creates a certificate binding the identity of $E$ to $E$'s public key
2. the certificate gets digitally signed by the CA using the CA's private key
3. when Alice wants to ensure Bob's identity, it decrypts the certificate using the CA's public key
The CA's public key is supposed to be known by everyone.
# 4 Authentication (8.3)
Goal: Bob wants Alice to prove her identity to him.
## 4.1 AP: Authentication Protocol
- AP1.0: "I am Alice"
- AP2.0: Alice says "I am Alice and this is my IP address"
- AP3.0: "I am Alice" and sends her secret password to prove it and sends her IP address (Trudy listens to the password the first time and then copies it)
- AP3.1: AP3.0 with encrypted password (Trudy can still paste the encrypted password)
- AP4.0: Bob sends Alice a **nonce** $R$, Alice must return $R$ encrypted with the shared secret key. Now Bob knows Alice is *live*. Requires a shared symmetric key.
- AP5.0: AP4.0 but using public key cryptography:
	1. -> I am Alice
	2. <- R
	3. -> $K_A^-(R)$
	4. <- send me your public key
	5. -> $K_A^+(R)$
	This protocol is susceptible to "man in the middle" attacks, when Alice sends "I am Alice", Trudy can intercept it and send it herself, now Bob will send Truly the **nonce** 
# 5 Securing e-mail (8.5)
## 5.1 Confidentiality
Alice wants to send *confidential* email $m$ to Bob so she: 
1. Generates random symmetric prive key $K_s$
2. encrypts message with $K_s$ (*for efficiency*)
3. also encrypts $K_s$ with Bos's public key
4. send both $K_s(m)$ and $K^+_b(K_s)$ to bob

then Bob:
1. uses his private key to decrypt and recover $K_s$
2. uses $K_s$ to decrypt $K_s(m)$ to recover m
## 5.2 Integrity and Authentication
Alice want to send $m$ to Bob, with **message integrity and authentication** 
1. Alice digitallly signs hash of her message with her private key, providing integruty and authentication
2. send both message (in the clear) and digital signature
## 5.3 authentication, integrity and confidentiality
In order to have authentication, integrity and confidentiality
**Alice uses three keys**: her private key, Bob's public key, new symmetric key
![[auth_integ_conf.png]]
# 6 Securing TCP connections: TLS (8.6: SSL)
**TLS** (**Transport-Layer Security**) is a widely deployed protocol above the transport layer which is supported by almost all browser and web servers.
it provides: 
- **confidentiality**: via [*simmetric encryption*](#2.3%20Symmetric%20Key%20cryptography%20(8.2.1))
- **integrity**: via [*cryptographic hashing*](#3.1.2%20Hash%20function%20algorithms%20(8.3.1))
- **authentication**: via [*public key cryptography*](#2.4%20Public%20Key%20Cryptography)

## 6.1 What's needed?
Let's build a toy TLS protocol, *t-tls*. we need: 
- **handshake**: Alice, Bob, use their certificates, provate keys to aurhenticate each other, exchange or create shared secret
- **key derivation**: Alice, Bob uses shared secret to derive set of keys
- **data transfer**: stream data transfer: data as a series of records
	- no just one-time transaction
- **connetcion closure**: special messages to securely close connection
### 6.1.1 Initial handshake
1. Bob establishes TCP connection with Alice
2. Bob verifies that Alice is really Alice
3. Bob sends Alice a master secret key (*MS*), used to generate all other keys fro TLS session
4. potential issues:
	- 3 RTT before client can start receiving data
![[bob_alice_handshake.png | 300]]
### 6.1.2 Cryptographic keys
It's considered bad to use same key fro more than one cryptographic function
- different keys for message authentication code (MAC) and encryption.
So we use **four keys**: 
- $K_c$ : encryption key for data sent from client to server
- $M_c$ : MAC key for data sent from client to server
- $K_s$ : encryption key for data sent from server to client
- $M_s$ : MAC key for data sent from server to client
those keys are derived from the **key derivation function**(*KDF*) which takes master secret and (possibly) some additional random data to create new keys
### 6.1.3 Encrypt data
*Recall*: TCP provides data byte stream abstraction.
*Question*: can we encrypt data in-stream as written into TCP socket?
Yes we can, but we need to break the stream in a series of **records**. 
Each client-server record carries a MAC, created with $M_c$ so the reciever can act on each record as it arrives.
The record are encrypted using symmetric key $K_c$, passed to TCP.
![[kc_key.png]]
There are possible attacks on data stream:
- *re-ordering*: the man-in-the-middle intercepts TCP segments and reorders them
solutions: 
- user TLS sequence numbers
- use nonce
### 6.1.4 Connection close
A *truncation attack* is possible. 
- The attackes forges a TCP connectin close segment. 
- One or both side thinks there is less data than there actually is.
Solution: 
- record types, with one type for closure
the new records is {lenght | type | data | MAC }
## 6.2 Transport-layer security
TLS provides and API that **any** application can use.
An HTTP view of TLS:
![[http_view_tls.png]]
### 6.2.1 TLS 1. cipher suite
TLS uses Chiper suite: 
- Algorithm from key generaltion
- Public-key encryption algorithm
- Symmetric-key encryption algorithm
- MAC algorithm
TLS 1.3 (2018): more limited cipher suite choice than TLS 1.2 (2008), only 5 choices rather than 37.

> This still means that client and server have to agree on which cipher suite to use.
### 6.2.2 TLS 1.3: Initail handshake
1. Client TLS hello msg:
	- gueses key agreement protocol, parameters
	- indicates cipher suite it support
2. server TLS hello msg chooses
	- key agreement protocol, parameters
	- cipher suite
	- server-signed certificate
3. client
	- checks server certificate
	- generates key
	- can now make application request
**Note**: this only require just ONE RTT.
# 7 Network Layer Security: IPsec (8.7)
IP Sec provides datagram-level encryption, authentication, integrity
- for both user traffic and control traffic
It has two modes: 
- **Transport mode**: only datagram payload is encrypted, authenticated
- **Tunnel mode**: entire datagram is encrypted, authenticated
	- encrypted datagram is encapsulated in new datagram with new ip header, tunneled to destination.
And there are two IPsec protocol:
- **Authentication Header**(AH) protocol (RFC 4302)
	- provides source authentication and data integrity but not confidentiality
- **Encapsulation Security Protocol**(ESP) (RFC 4303)
	- provides source authentication, data integrity and confidentiality
	- more widely used than AH
## 7.1 Virtual Private Networks (VPN)
Institutions often want a private network for security, but it's not cheap. It requires separate routers, links, DNS infrastructure.

With a **VPN**, instituition's inter-office traffic is sent over public internet instead. The inter-office traffic is encrypted before entering the public internet.
## 7.2 Security Associations (SAs)
Before sending data a **security association** is established from sending to receiving entity (directional)
Both entities maintain *state information* about SA
- recall: TCP endpoints also maintain state info
- IP is connectionless, IPsec is connection-oriented
## 7.3 IPsec datagram
![[tunnel_mode_ipsec_datagram.png]]
- **ESP trailer**: padding for block ciphers
- **ESP header**: 
	- SPI, so receiving entity know what to do
	- sequence number, to thwart replay attacks
- **MAC in ESP** auth field created with shared secret key
## 7.4 IPsec protocols (8.7.2)
There are two IPsec protocols:
- **authentication header (AH) protocol**: provides source authentication and data integrity but *not* confidentiality
- **encapsulation security protocol (ESP)**: provides source authentication, data integrity *and* confidentiality. It is more widely used than AH.
### 7.4.1 ESP tunnel mode: actions
Actions for the ESP tunnel mode at the router connected to the public internet:
1. Appends the ESP trailer to the original datagram (which includes the original header fields)
2. encrypts the result using the algorithm and the key specified by the SA
3. appends the ESP header to the front of this encrypted quantity
4. creates an authentication MAC using the algorithm and the key specified by the SA
5. appends the MAC forming the *payload*
6. creates a new IP header, new IP header fields and the addresses to the tunnel endpoint
## 7.5 IPsec sequence Numbers
For the new SA, the sender initializes the sequence number to 0, each time the datagram is sent to the SA:
- the sender increments the sequence number
- the senders places the value in the sequence number field
The goal is to prevent the attacker from sniffing and replaying a packet, because the receipt of duplicate authenticated IP packets may disrupt the service.
To solve this the receiver uses a windows (it doesn't keep track of *all* the packets) to check for duplicates.
## 7.6 IPsec Security databases
IPsec uses two databases:
- **Security Policy Database** (**SPD**): "*what to do*"
	- policy: for a given datagram the sender needs to know if it should use IPsec
	- the policy is stored in the SPD
	- the sender also needs to know which SA to use
- **Security Association Database** (**SAD**): "*how to do it*"
	- endpoints hold the SA state in the SAD
	- when sending an IPsec datagram, the outgoing router $R1$ accessed the SAD to determine how to process the datagram
	- when an IPsec datagram arrives to the ingoing router $R2$, it examines the SPI in the IPsec datagram, indexes the SAD using the SPI and processes the datagram accordingly
## 7.7 IPsec Conclusions
- IPsec is a protocol for datagram-level security
	- AH provides integrity and source authentication
	- the ESP protocol (using AH) adds also encryption
- IPsec peers can be:
	- two end systems
	- two routers/firewalls
	- a router/firewall and a end system
# 8 Operational security: firewalls and IDS (8.9)
## 8.1 Firewalls (8.9.1)
A **firewall** isolates and organization's internal network from the larger internet, allowing some packets to pass and blocking others.

We need firewalls to:
- prevent DOS attacks
	- SYN flooding: the attacker establishes many bogus TCP connections, leaving no resources for real connections
- prevent illegal modification or access of internal data
	- replacing an official website with something else
- allow only authorized access to the inside network
	- by defining a set of authenticated users/hosts

There are three types of firewalls:
- stateless packet filters
- stateful packet filters
- application gateways
### 8.1.1 Stateless packet filtering
The internal network gets connected to the Internet via a router **firewall**, which filters **packet-by-packet** and decides to forward/drop a packet based on information in the IP header:
- source IP address, destination IP address
- TCP/UDP source and destination port numbers
- ICMP message type
- TCP SYN and ACK bits
#### ACL: access control list
An **ACL** is a table of rules, applied top to bottom to incoming packets, that looks like the OpenFlowForwarding.
<div> <table> <tr> <th>
				action
			</th> <th>
				source address
			</th> <th>
				destination address
			</th> <th>
				protocol
			</th> <th>
				source port
			</th> <th>
				destination port
			</th> <th>
				flag bit
			</th> </tr> </table> </div>
### 8.1.2 Stateful packet filtering
The problem with stateless packet filtering is that it admits packets that make no sense, for example TCP packets without a TCP connection established.

Stateful packet filtering solves this by keeping track of every TCP connection, it in fact:
- tracks connection setup (SYN), teardown (FIN) to determine wether or not a packet makes sense
- timeouts inactive connections at the firewall: no longer admits these packets
<div> <table> <tr> <th>
				action
			</th> <th>
				source address
			</th> <th>
				destination address
			</th> <th>
				protocol
			</th> <th>
				source port
			</th> <th>
				destination port
			</th> <th>
				flag bit
			</th> <th>
			check connection
			</th> </tr> </table> </div>

### 8.1.2 Application Gateways
An application gateway filters packets on application data as well as on IP/TCP/UDP fields.
An application gateway is a separate host than the router/filter.

A possible application is: the firewall blocks every telnet connection, except the ones incoming from the application gateway, for authorized users the gateway sets up a telnet connection to the destination host by relaying the data between these two connections.
### 8.1.3 Limitations of firewalls and gateways
- The router is susceptible to IP spoofing, it can't know if the data "really" comes from the claimed source.
- If multiple applications need a special treatment, each need its own application gateway.
- Client software must know how to contact the gateway, for example by setting the IP address of the proxy in the web server
- filters often use a "all or nothing" policy for UDP
- *tradeoff*: the degree of communication with the outside world in exchange for the level of security
- many highly protected sites still suffer from attacks, even using firewalls
### 8.1.4 IDS: Intrusion detection systems
An intrusione detection system performs a deep packet inspection by also looking at the packet contents (for example checking character strings in the packet against a database of known virus and attack strings).
And IDS also examines the correlation among multiple packets:
- port scanning
- network mapping
- DoS attack

An organization may use multiple IDSs, each with different types of checking at different locations.