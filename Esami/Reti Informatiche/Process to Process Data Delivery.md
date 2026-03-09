---
number headings: auto, first-level 1, max 6, 1.1
---
#uni 
> Internet Stack Protocol: ![[internetProtocolStack.svg|600]]
# 1 Transport-Layer services
The transport Layer manages the logical communication between application **processes** running on different hosts, whereas the network layer manages the logical communication between **host**.

The fact that there are multiple processes running on the same host calls for the need for multiplexing and demultiplexing.

The Sender in transport protocols breaks application messages into **segments** and passes them to the *network layer* (to the IP).
The Receiver in transport protocols reassembles these segments into messages and demultiplexes them to the *application layer* via a socket.
# 2 Multiplexing and Demultiplexing
## 2.1 Multiplexing
Multiplexing is done at the sender: data from multiple sockets is handled, to which the respective transport header is added, later used for demultiplexing.
## 2.2 Demultiplexing
The transport header info is used to deliver the received segment to the correct socket.

Multiplexing and demultiplexing are done based on segment and datagram header field values.
### 2.2.1 Connectionless demultiplexing
1. the host receives IP datagrams.
	- each datagram has a source IP address and a destination IP address
	- each datagram carries one transport-layer segment
	- each segment has source and destination port numbers
2. the host uses the *IP address and the port number* to direct the segment to the appropriate socket.
### 2.2.2 Connection-Oriented demultiplexing
The TCP socket gets identified by 4 tuples:
- source IP address
- source port number
- destination IP address
- destination port number
The receiver uses *all four* of this values (4-tuple, quadrupla di informazioni) to direct the segment to the appropriate socket.

A server may support many simultaneous TCP sockets:
- each socket is identified by its own 4-tuple
- each socket is associated with a different connecting client

> In TCP a connection is identified by 4 parameters!
# 3 Connectionless transport: UDP
RFC 768:
UDP (**User datagram protocol**) is a "best effort" service, like IP.
Segments may be lost and delivered out of order to the application.
This because UDP is ***connectionless***: there is no handshake between UDP sender and receiver, each UDP segment is handled independently of others.
## 3.1 Benefits and Uses
- no connection
	- connection can add RTT delay
- simplicity
- small header size
- no flow and congestion control
	- UDP can go faster
	- still works under congestion
This leads to UDP being useful in these areas:
- streaming and multimedia applications (loss tolerant, rate sensitive)
- [[DNS]]
- SNMP
- HTTP/3

If a reliable transfer is needed over UDP (for example in HTTP/3) reliability and congestion control are added at the application layer.
## 3.2 UDP segment header
> UDP segment header: ![[udp_segment_header.png]]
- the payload is a message from the application layer, it is of variable length so we have the length field
- length field: length of the UDP segment (including header) in bytes
## 3.3 UDP checksum
The sender treats the contents of the UDP segment (header and IP address included) as a sequence of 16-bit integers, then the checksum is computed as the sum of these 16-bit integers and the checksum value is put into the UDP checksum field.

The receiver computes the checksum as well and compares it to the checksum included in the header: if these are not equal and error is detected.
# 4 Connection-Oriented transport: TCP
RFCs: 793,1122, 2018, 5681, 7323
TCP (**transport control protocol**) introduces the **point-to-point** concept: one sender, one receiver.
It achieves a reliable, in-order *byte stream*, from the application standpoint.
The flow is **Full duplex**: the flow is bidirectional in the same connection.

Every segment has an **MSS**: maximum segment size: dimensione frame - dimensione massima dell'ether IP (20 byte) - dimensione massima dell'ether TCP (20 byte)

The TCP congestion and flow control set the window size of the **pipelining** ([[Direct Connection Networks#3.2 Pipelining]]).
## 4.1 TCP segment structure
> TCP segment structure: ![[tcp_segment_structure.png]]
- PDLated approach: ACKs are sent inside fragments
	- if A bit is set "Acknowledgement number" is significant (significativo)
- the checksum is the same as in UDP: internet checksum
- R: RST: reset: when connection is having problems and the connection needs a reset
- S: SYN: start: is set in the connection phase
- F: FIN: fine: is set when wanting to close the connection
- receive window: used to repeatedly tell the other end how many bytes are free in the receiving window
- "urgent data pointer": used to tell receiver that in the payload is important data (with priority). Is never used. When this field is significant, bit "U" in the flags is set.
> normally the header is 5 words (5* 32 bits = 20 byte), but it is of variable length, the length is inside the "header length" field
## 4.2 Sequence Numbers and ACKs
In TCP sequence numbers are the *byte stream number* of the first byte in the segment's data.

> TCP Window: ![[tcp_window.png|300]]

TCP uses a go-back-$N$ error recovering approach: it sends cumulative ACKs (see [[Direct Connection Networks#3.2.1.1 Go-back-N]]).

TCP doesn't specify how the receiver must handle out-of-order segments, that is up to the implementor.

## 4.3 TCP connection management
Before exchanging data, send/receiver have and "handshake": 
- They agree to establish a connection
- They agree on connection paramenters
### 4.3.1 The 3-way handshake
TCP 3-way handshake:
![[tcp_3_way_handshake.png]]
- if the initial sequence number was always the same, we risk mixing segments from two different connections, so we use a random number.
  EG: we have a first connection which is short, every packet is ACKd and the connection is closed, then we open a new connection shortly after, if there are still segments from connection 1 "in flight" the risk is they arrive while we have initiated the second connection, these segments arrive and are mistaken for segments from the new connection.
### 4.3.2 Closing a TCP connection
1. client sends TCP FIN control segment
2. server receives the FIN and replies with an ACK, closes the connection and sends a FIN
3. client receives the FIN, replies with an ACK and starts a **timed wait** (designed to close the connection shortly after the supposed arrival of the ACK to the server)
4. server receives the ACK. The connection is now closed.
   If the server doesn't receive the ACK (it timeouts), it resends the FIN, this is the reason of the timed wait of the client.

What does it mean to "close a connection"?
Deallocating the data needed for the connection
## 4.4 TCP: Reliable Data Transfer
TCP protocol create a reliable data transfer service on top of IP's unrealiable service. 
This is a window based ARQ scheme: 
- Acknowledgements
- Timeouts 
- Retransmission
### 4.4.1 TCP: timeout
*How to set TCP timeout value?*
It needs to be longet than **RTT** but it varies.
- If timeout is **too long** we get a slow reaction to segment loss
- If timeout is **too short** we get a premature timeout which leads to unnecessary retransmissions
So we try to estimate it.
First we get a sampleRTT: 
- mesaured time from segment transmission until ACK receipt, ignoring retransmissions
We average several recent measurements and we get and estimate.
Another issue is that RTT varies a lot because it's dependet on a lot of factors, so we add a **weight** to every measurements.

We compute **Exonential Weigheted Moving Average (EWMA)**: 
$$\text{EstimateRTT}_n = (1-\alpha ) \cdot \text{EstimateRTT}_{n-1}+ \alpha \cdot \text{SampleRTT}_{n-1}$$
The influence of past samples decreases exponentially fast. 
A typical value of $\alpha$ is 0.125.

Now this average is reactive but not stable.
We need a stabelizing term and since RTT has large variations we need this term to be large.

This the is called **DevRTT** and it's defined like this: $$\text{DevRTT}_n = (1 - \beta ) \cdot \text{DevRTT}_{n-1} + \beta \cdot | \text{SampleRTT}_{n-1} - \text{EstimateRTT}_{n-1} |$$
Which is the EWMA of the deviation of SampleRTT from EstimatedRTT.
A typical value for $\beta$ is 0.25.

In the end the timeout is: $$\text{TimeoutInterval} = \text{EstimatedRTT} + 4 \cdot \text{DevRTT}$$
### 4.4.2 TCP: Sender
event: data received from application:
1. Create a segment with a sequence # 
2. the sequence # is byte-stream number o first data byte in the segment
3. Start the timer if it's not already running
	- think of timer as for older unACKed segment
	- exiration interval: **TimeoutInterval**

event: timeout
1. retransmit segment that coused timeout and restart timer

event: ACK received
1. update what is known to be ACKed
2. start time if ther eare still unACKed segments
**Pseudocode**:
```c
NextSeqNum = InitialSeqNum
SendBase = InitialSeqNum
loop (forever) {
	switch(event)
		event: data received from application above
		create TCP segment with sequence number NextSeqNum
		if (timer currently not running) start timer
		pass segment to IP
		NextSeqNum = NextSeqNum + length(data)
	event: timer timeout
		retransmit not-yet-acknowledged segment with smallest sequence number
		start timer
	event: ACK received, with ACK field value of y
		if (y > SendBase) {
			SendBase = y
			if (there are currently not-yet-acknowledged segments) start timer
		}
} /* end of loop forever */
```
### 4.4.3 TCP: Receiver
### 4.4.4 ACK generation (RFC 5681)

| Event At Receiver                                                                                       | TCP Receiver Action                                                          |
| ------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| arrival of in-order segments with expected sequence #. All data up to exepcted sequence # already ACKed | delayed ACK. Wait up to 500ms for next segment, if no next segment, send ACK |
| arrival of in-order segment with expected sequence #. One other segments as ACK pending                 | immediately send single comulative ACK, ACKing both in-order segments        |
| arrival of out-of-order segment higher-than-expected sequence #. Gap detected                           | immediately send duplicate ACK, indicating seq # of next expected byte       |
| arrival of segment that partially of conpletely fills gap                                               | immediate send ACK, provided that segment statrs at lower end of gap         |
### 4.4.5 TCP: fast retransmit
If the sender receives three additional ACKs for the same date (triple duplicate ACKs), it resend unACKed segment with the smalles sequence #.
Most likely that unACKed segment is lost do we do not wait for the timeout
![[tcp_fast_retransmit.png]]
## 4.5 TCP: flow control
*What happens if the network layer delivers data faster than application layer removes data from the socket buffers?*

What happens is a **buffer overflow**. 

We prevent it using **flow control**: 
- The receiver controls the sender, so the sender won't overflow receiver's buffer by transmitting too much, too fast
![[flow_control.png|500]]
- TCP receiver "advertise" that there is a free buffer space in **rwnd** field in TCP header.
	- **RcvBuffer** size is set via socket options (4096 bytes typically)
	- many operating system autoadjust RcvBuffer
- Sender limits the amout of unACKed "in flight" datat to send according to rwnd
- It guarantees that the receiver buffer will not overflow
