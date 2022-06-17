## Understanding TCP

> Purpose: This article is intended to provide you with a summary of how TCP works and it's more important components.

## TCP (Transmission Control Protocol):
Connection-oriented protocol that ensures reliable transmission of packets in a Client-Server model.

- OSI Model: Layer 4 (transport)
- PDU: Segment
- TCP Connection-oriented (vs UDP Connectionless)
- Includes mechanisms to address issues that arise from:
  - Packet-based messaging
  - Loss of packets
  - Out of order packets
  - Duplicate packets
  - Corrupted packets.
- TCP works on top of IP (Layer 3 Routed-Protocol) to ensure packet transport. 

### **Function:**

> Important: The TCP/IP stack is responsible for breaking the stream of data into segments and place them into packets at the sender, then it sends those packets to the receiver, while the stack at the receiver side is responsible for reassembling the segments inside de packets, into a data stream using the information in the TCP headers.

-	No payload (app data) is transmitted until TCP establishes a connection between a source and a destination.
-	It dinamictly determines how much data can be sent before the receiver sends and acknowledgement.    

## **TCP Header Format:**
-	Minimum Size of 20-bytes
-	Maximum Size of 60-bytes (allowing 40-bytes for TCP Options)
-	TCP Segment: Contains a header and data. 

![tcp-header-format.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1655387929127/0TVlyd-OH.jpg align="left")

### **Window Size:**
-	As a standard, the largest possible value in the window size is 2^16 = 65535-bytes or 64-kb *(to increase this size please refer to window scaling below)*
-	Also called the TCP receiver window size. 
-	This is an advertisement of how much data (in bytes) the receiving device is willing to receive **at any point in time.** 

*Example:* A receiver device may set the value to 14,600-bytes, or 10 MSS segments of 1460-bytes, this indicates that it's buffer is able to handel this amount of data in a given period of time.  

> If the receiver is overwhelmed, meaning it can’t take anymore data, it will advertise a ZERO window size, this will tell the sender to stop sending packets until the receiver sends back a valid window size. 

### **TCP Flags**

Indicate a particular state during a TCP conversation.

| Flag | Description | tcpdump option | Example |
| - | - | - | - |
| U – URG | Indicates the TCP segment contains urgent data.  | # tcpdump 'tcp[13] & 32 != 0' #URG or <br /># tcpdump 'tcp[tcpflags] & tcp-urg != 0' #URG | TCP: ..0….. = No urgent data |
| A – ACK | Acknowledgment field contains the next byte expected on the connection | # tcpdump 'tcp[13] & 16 != 0' #ACK or <br /># tcpdump 'tcp[tcpflags] & tcp-ack != 0' #ACK | TCP: …0…. = Acknowledgement field not significant |
| P – PSH | Indicates that the contents of the TCP receive buffer should be passed to the Application Layer protocol | # tcpdump 'tcp[13] & 8  != 0' #PSH or <br /># tcpdump 'tcp[tcpflags] & tcp-push != 0' #PSH | TCP: ….0… = No Push function |
| R – RST | Indicates that the connection is being aborted | # tcpdump 'tcp[13] & 4  != 0' #RST or <br /># tcpdump 'tcp[tcpflags] & tcp-rst != 0' #RST | TCP: …..0.. = No Reset |
| S – SYN | Synchronize sequence number | # tcpdump 'tcp[13] & 2  != 0' #SYN or <br /># tcpdump 'tcp[tcpflags] & tcp-sync != 0' #SYN | TCP: ……1. = Synchronize sequence numbers |
| F – FIN | Indicates that the TCP segment sender is finished sending data on the connection | # tcpdump 'tcp[13] & 0  != 0' #FIN or <br /># tcpdump 'tcp[tcpflags] & tcp-fin != 0' #FIN | TCP: …….0 = No Fin |


Before going more in deep into TCP headers, I want you to understand the following concepts: 

```
MTU: Maximum Transmission Unit
-	Maximum Payload length. 
-	Consists of IP Header, TCP Header, MSS.  

Ethernet v2 frame: MTU: 1500 (IP 20-byte, TCP 20-byte, MSS 1460-byte)
-	Minimum size of 64-bytes (18-byte Ethernet Header + 46-bytes payload)
-	Maximum size of 1518-bytes (18-byte Ethernet Header + 1500-bytes payload)

Ethernet Jumbo Frame: MTU: 9000 (IP 20-byte, TCP 20-byte, MSS 8960-byte)
-	 Maximum size of 9018-bytes (18-byte Ethernet header + 9000-bytes payload)
```

> Note: Clarification in regards of MSS and TCP Options RFC6691:
> -	The MSS value to be sent in an MSS option should be equal to the effective MTU minus the fixed IP and TCP headers. By ignoring both IP and TCP options when calculating the value for the MSS option, if there are any IP or TCP options to be sent in a packet, then the sender must decrease the size of the TCP data accordingly.


### **Header Options:**
Provide protocol enhancement and are located at the end of the TCP header.

*Some of the options are required to appear only during the initial 3-way-handshake. Other options, however, can be used at will, during the TCP session.* 

Options are:
- **Maximum Segment Size (MSS):** (1460-byte Standard, 8960-byte Jumbo).  
  - Only sent over 3-way-handshake.
  - MSS consists of the Data Segment. (This is the payload)
  - MSS = MTU – (TCP header + IP Header) 
<br /><br />
- **Window Scaling Factor:** (30-bit, 16-bit from Original + 14-bit from Options)
  - Only sent over 3-way-handshake.
  - An extension to the Window size flag.
  - By using a Scaling Factor, the window size can be increased to a value of 1GB.
  - Scale Value can be set from 0 (no shift) to 14
    <br />**Calculation: 65535*2^s (where S is the scale value).**
<br /><br /> 
- **Selective Acknowledgements (SACK):** (Kind - 5) 
  - Can address gaps in a sequence space, to prevent the sender for having to retransmit everything after a single loss if the window is very large.
  - This option is triggered with there is a missing packet in the block of queued data.  
  - Lengh 8*n+2bytes, so the 40 bytes available for TCP options can specify a
   maximum of 4 blocks.
       - It is expected that SACK will often be used in conjunction with Timestamp, which takes an additional 10 bytes, thus
       - a maximum of 3 SACK blocks will be allowed in this case. 
  <br />

   > Note: TCP uses cumulative acknowledgment scheme, this means that if a packet is lost in between, it will retransmit everything from that point to above, even when subsequent packets have already been sent. 
<br /><br /> 
- **Timestamps (TSopt)**
  - Enables the endpoints to keep a current measurement of the roundtrip time (RTT) of the network between them.
    - RTTM( Round Trip Time Measurement)
    - PAWS( Protection against wrapped Sequence number)
<br /><br />
- **No-Operation (NOP) (Kind = 1 byte x option)**
  - As the TCP header must be a multiple of 4 bytes, this option is used to fill that gap when another tcp option is used that is not an exact 4-bytes lenght. 
  - In example:
    - MSS is a 4 byte option: it does NOT need NOP
    - Window scale is a 3 byte option: 1 NOP needs to be added to make it 4-bytes. 
    - SACK is a 2 byte option: 2 NOP needs to be added to make it 4-bytes


### **Connection state mechanisms:**
- **Streams:** TCP data is organized as a stream of bytes, much like a file.
- **Reliable delivery:** Sequence numbers are used to coordinate which data has been transmitted and received unidirectionally. TCP will signal for retransmission if it determines that segments have not being received.
- **Network adaptation:** TCP will dynamically learn the delay characteristics of a network and adjust its operation to maximize throughput without overloading the network.
- **Flow control:** TCP manages data buffers and coordinates traffic so its buffers will never overflow. Fast senders will be stopped periodically to keep up with slower receivers.
- **Round-trip:** time estimation: TCP continuously monitors the exchange of data packets, develops an estimate of how long it should take to receive an acknowledgement, and automatically retransmits if this time is exceeded.

### **Process of Transmitting a Packet:**

1- **Establish connection:** To establish connection in between two devices, the Three-Way Handshake must be initiated. 

Steps: <br />
  1) **SYN:**  Source device sends a packet with the SYN bit set to 1. <br />
  2) **SYN-ACK:** Destination device replies with a packet ACK bit set to 1 plus the SYN bit set to 1.<br />
  3) **ACK:** Source device replies with a packet ACK bit set to 1. 
* These values can be found on the TCP header. 
* These packets do not include any data. 

2- **Transmission of data:** Data is being send in form of packets as follows:

  1) **Seq Number + Data:** Source device sends a packet with source data and sequence number.
     * Used to identify the bytes within a stream. 
  2) **ACK Number:** Destination device acknowledges by setting the ACK bit and increasing the number by the length of the received data
     * These numbers are used to keep track of successful received data. 

3- **Close the connection:**
A device will close the connection when no more data needs to be sent nor received. 

There are two types: 

**Graceful connection relaease:**

In summary: 

(1) Client FIN, (2) Server ACK, (3) Server FIN, (4) Client ACK = Connection Closed

| Client | Server |
| -- | -- |
| **(1)** Send **FIN:**  <br />Sends a packet with the FIN bit set to 1. <br /> (FIN-WAIT-1) - Wait for ACK and Fin from Server | Normal operation |
| - |  Receive **FIN** <br /> **(2)** Send **ACK** <br /> Sends a packet with ACK bit set to 1. |
| (FIN-WAIT-2) - Receive ACK | Tell app to close <br /> (CLOSE-WAIT) - Wait for App  |
| - | **(3)** Send **FIN** <br /> Sends a packet with FIN bit set to 1. | |
| Receive **FIN** | - |
| - | (LAST-ACK) - Wait for ACK  |
| **(4)** Send **ACK** <br /> Sends a packet with ACK bit set to 1. | | - |
| (TIME-WAIT) - Closed | Received **ACK** and Closed |

**Abrupt connection release:**
- An abrupt connection release is carried out when an RST segment is sent


<br />

---

**Data resources:**

The most important Links: 

[TCP RFCs](https://rfcs.io/tcp)<br />
[IANA - TCP Options RFCs](https://www.iana.org/assignments/tcp-parameters/tcp-parameters.xhtml)

Other References:

[Wikipedia - TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)<br />
[Microsoft - TCP Features](https://docs.microsoft.com/en-us/troubleshoot/windows-server/networking/description-tcp-features)<br />
[Techrepublic - The Anatomy of a Data Packet](https://www.techrepublic.com/article/exploring-the-anatomy-of-a-data-packet/)<br />
[KanAcademy - TCP](https://www.khanacademy.org/computing/computers-and-internet/xcae6f4a7ff015e7d:the-internet/xcae6f4a7ff015e7d:transporting-packets/a/transmission-control-protocol--tcp )<br />
[Fortinent - TCP/IP Model](https://www.fortinet.com/resources/cyberglossary/tcp-ip#:~:text=TCP%20stands%20for%20Transmission%20Control,data%20and%20messages%20over%20networks.)<br />
[Firewall CX - TCP header Options](https://www.firewall.cx/networking-topics/protocols/tcp/138-tcp-options.html)<br />
[Packet Coders - MTU, Jumbo Frames, MSS](https://www.packetcoders.io/mtu-jumbo-mss/)<br />
[Network Computing - TCP Window Size](https://www.networkcomputing.com/data-centers/network-analysis-tcp-window-size )<br />
[Freesoft - TCP Header Format](https://www.freesoft.org/CIE/Course/Section4/8.htm)<br />
[How to use Linux - TCP Flags](https://www.howtouselinux.com/post/tcp-flags)
