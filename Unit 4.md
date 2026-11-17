
> **EXACT UNIT IV SYLLABUS:** **Transport Layer:** transport-layer design issues, connection management. **Session Layer:** session-layer design issues and remote procedure call. **Presentation Layer:** presentation-layer design issues, data-compression techniques, cryptography. **TCP:** TCP and window management. The textbook wording groups these under Unit IV. Supporting concepts such as UDP, acknowledgements, TLS, retransmission and congestion window are included only to explain these prescribed topics clearly.

> **HOW TO STUDY EACH CHAPTER:** (1) **HINGLISH** - easy explanation with a relatable example, (2) **COMPLETE ENGLISH** - definition, working, steps, diagrams, advantages and disadvantages, (3) **EXAM NOTES** - important distinctions and solved examples, (4) **20-MARK MODEL ANSWERS** - practise writing long formal answers. **Important questions are syllabus-based practice predictions, not verified past-paper questions.**


# PART A - COMPLETE UNIT IV THEORY: HINGLISH FIRST, ENGLISH AFTER

## 1. Understand the Big Picture: Where Unit IV Fits

### 1.1 Hinglish - one video call, many networking jobs

Socho tum apne friend ke saath online video call kar rahe ho. Tumhara phone Wi-Fi se Internet par connected hai, friend kisi aur network par hai. Data ko uske phone tak pahunchana **Network Layer** ka important kaam hai (Unit III). Lekin socho, friend ke phone mein WhatsApp, browser aur games sab running hain. **Kaunsa data kis app ko dena hai?** Agar file download mein ek piece missing ho jaye to kya hoga? Agar receiver slow ho, sender kitna data bhej sakta hai? Yeh concerns **Transport Layer** se relate karte hain.

Ab socho video call ke beech tum hold kar dete ho ya meeting reconnect hoti hai. Conversation ka coordination aur dialogue control ek **Session Layer** concept hai. Tum kisi website ko request bhejte ho aur tumhe lagta hai ki server ka function bas call ho gaya - jaise local function - use **Remote Procedure Call (RPC)** kehte hain.

Friend ke phone ka text aur file tumhare device par sahi format mein dikhna chahiye. Data compress ho sakta hai, encrypted bhi ho sakta hai. Ye **Presentation Layer** ke textbook ideas hain. Real Internet software mein ye functions often applications, libraries and security protocols mein implement hote hain; strict seven-layer software stack zaroori nahi hota.

**Simple memory:** Transport = correct application and end-to-end delivery; Session = conversation coordination; Presentation = meaningful data format, compression and protection.

```text
   COMPUTER A                                                COMPUTER B
 +----------------+                                        +----------------+
 | APPLICATION    |  Browser, chat, video, email           | APPLICATION    |
 | PRESENTATION   |  Format / compression / encryption     | PRESENTATION   |
 | SESSION        |  Dialogue control / checkpoints        | SESSION        |
 | TRANSPORT      |  TCP / UDP, ports, flow and reliability | TRANSPORT      |
 +----------------+                                        +----------------+
          |                                                        ^
          v                                                        |
      Network + Data Link + Physical -> routers / links -> Network + ...

   UNIT IV MAIN FOCUS = TRANSPORT + SESSION + PRESENTATION + TCP WINDOWS
```

### 1.2 Complete English - scope of Unit IV

The Transport Layer (OSI Layer 4) supplies communication between applications running on end systems. It can provide process identification, segmentation, multiplexing, reliability, ordered delivery, flow control and connection management, depending on the transport protocol. **TCP** supplies a reliable, connection-oriented byte-stream service, while **UDP** supplies a simpler connectionless datagram service.

The Session Layer (OSI Layer 5) describes the organisation and control of communication sessions. Its design issues include establishment, maintenance and release of a session, dialogue control, synchronisation, checkpoints and recovery. RPC is an approach in which a program requests a function on another computer as though calling a local procedure, although the network introduces failures and delays.

The Presentation Layer (OSI Layer 6) focuses on how data is represented and understood: translation between formats, character encoding, serialisation, compression and cryptographic protection. These categories describe **functions**, not necessarily separate programs in actual TCP/IP networks.

**A frequent exam mistake:** Saying TCP guarantees that a remote application has *processed* a message. TCP acknowledges received bytes at the transport level; application-level processing acknowledgement is a different matter.

### 1.3 Important distinctions before starting

| Layer | Main question | Typical example | Unit IV topic |
|---|---|---|---|
| Network (3) | To which IP endpoint should the packet go? | Router forwards IP packet | Recap only |
| Transport (4) | Which application/process and how to deliver? | TCP connection with ports | Design, TCP, window |
| Session (5) | How is a dialogue organised and resumed? | Checkpoint / conversation | Design, RPC |
| Presentation (6) | How are bytes represented and protected? | UTF-8, compression, encryption | Design, techniques |

## 2. Transport Layer - Definition, Services and Design Issues

### 2.1 Hinglish - hostel reception and room numbers

Maan lo courier Lucknow University ke hostel ke address par pahunch gaya. Building tak parcel pahunch gaya, lekin hostel mein 100 rooms hain. Room number nahi hoga to parcel sahi student ko kaise milega? **IP address** ek device tak pahunchne mein help karta hai; **port number** device par suitable application service identify karne mein help karta hai. Jaise hostel address plus room number, waise IP address plus port.

Ab maan lo ek heavy textbook 20 envelopes mein divide karke bheji. Kuch envelopes missing, duplicate ya wrong order mein aa sakte hain. Ek reliable transport system in problems ko handle kar sakta hai. Lekin har application ko same service nahi chahiye: file download ke liye data accurate chahiye, real-time calling mein low delay important hota hai. Isi liye **different transport protocols** aur design choices hote hain.

### 2.2 Complete English - definition and responsibilities

The **Transport Layer** is the fourth layer of the OSI model. It offers end-to-end logical communication services between processes running on source and destination devices. It sits above the Network Layer and below the Session or Application Layer. Unlike routers that forward packets hop by hop, transport functions such as TCP acknowledgements are mainly implemented at the end hosts.

Major responsibilities and design issues are:

1. **Service to upper layers:** provide either reliable connection-oriented delivery or lightweight connectionless delivery according to the chosen protocol.
2. **Service-point addressing:** identify communicating processes through transport-layer ports. An IP address identifies an interface/end host location; a port identifies an application endpoint on that host.
3. **Segmentation and reassembly:** split a large stream of application data into manageable transport segments and reconstruct the stream at the receiver.
4. **Multiplexing and demultiplexing:** allow many applications/connections to use the same host networking stack and distribute incoming data to the appropriate socket.
5. **Connection establishment and release:** create shared connection state before sending a TCP byte stream, and terminate it gracefully or abortively.
6. **Error detection and recovery:** detect corrupted segments using checksums; TCP also uses acknowledgement and retransmission to recover from losses.
7. **Ordered delivery:** TCP hides out-of-order delivery from the application by reordering and buffering bytes as needed.
8. **Flow control:** protect a receiver whose buffer or processing capacity is limited.
9. **Congestion control:** adapt transmission behaviour when network capacity is threatened by too much traffic.
10. **Timer management:** use retransmission timers, delayed acknowledgements and other carefully chosen timers.
11. **Performance and quality of service:** make trade-offs among throughput, delay, overhead and resource consumption.
12. **Crash and failure handling:** handle duplicate messages, lost responses, half-open connections, peer restarts and network partitions as far as the protocol allows.

Not every transport protocol implements all these facilities: **TCP** implements several; **UDP** deliberately provides far fewer.

### 2.3 Transport service: connection-oriented versus connectionless

**Connection-oriented transport** first establishes shared state and then transfers data. TCP is the standard example. It uses sequence numbers, acknowledgements, retransmissions, flow control and ordered stream delivery. A TCP connection is not a physical circuit or a promise of uninterrupted network availability.

**Connectionless transport** sends self-contained datagrams without establishing a transport connection. UDP is the standard example. UDP provides port numbers and an integrity checksum, but does not itself guarantee delivery, retransmission, ordering or congestion control. Applications may add these mechanisms themselves.

| Feature | TCP | UDP |
|---|---|---|
| Communication | Connection-oriented | Connectionless |
| Data model | Ordered byte stream | Message/datagram boundaries |
| Reliability | ACK and retransmission | No built-in delivery guarantee |
| Ordering | Yes, to the application | Not guaranteed |
| Flow control | Receiver window | Not built in |
| Congestion control | Built in | Not built in by UDP |
| Header size | Minimum 20 bytes | 8 bytes |
| Typical cases | Web transport, file transfer, email | DNS queries, some calls and games |

**Clarification:** Modern video calls and web applications can use protocols built on UDP which add their own loss recovery, security and congestion control. Therefore "UDP means no reliability anywhere" is incorrect.

### 2.4 Process-to-process addressing and sockets

A **socket** is an operating-system communication endpoint. TCP connections are commonly identified by the combination of **source IP, source port, destination IP, destination port** (along with protocol/context). Different client connections can share a server destination port without being mixed up.

```text
Client browser: 192.0.2.10:51000      Server: 203.0.113.20:443
Client browser: 192.0.2.10:51001  ---> Server: 203.0.113.20:443
Client browser: 192.0.2.11:54000      Server: 203.0.113.20:443

Each TCP connection has distinct endpoint identifiers.
The SERVER port 443 can serve many simultaneous clients.
```

Well-known examples include 80 (HTTP), 443 (HTTPS), 25 (SMTP) and 53 (DNS, often UDP but also TCP). A port number alone is not a globally unique process address: it must be considered with the host, protocol and connection details.

### 2.5 Multiplexing and demultiplexing

**Multiplexing** is the gathering of data from multiple application sockets into transport segments sent through the network. **Demultiplexing** is the reverse action: the receiving transport module examines transport identifiers and delivers received data to the appropriate process/socket.

```text
   Browser ----\                               /---- Browser socket
   Email -------\-- TCP/UDP -> Internet -> ----/----- Email socket
   Chat --------/                               \---- Chat socket

     MULTIPLEXING                                 DEMULTIPLEXING
```

**Why necessary?** One computer must run many networking applications without giving each program a separate physical Internet connection.

### 2.6 Segmentation, reassembly and byte stream

For TCP, the application writes a **byte stream**, not a set of preserved message records. TCP may combine several application writes into one segment or split one write into several segments. The receiver therefore cannot assume that one `send()` call equals exactly one `recv()` call. Applications use explicit framing, such as a length field or delimiter, if they require messages.

For UDP, datagram boundaries are preserved: one UDP datagram represents one sent message, within size and receiver-buffer limitations.

**English exam example:** An application writes 3000 bytes to TCP. TCP might transmit it as segments carrying 1000, 1000 and 1000 bytes, or 1200, 1200 and 600 bytes, depending on MSS, buffering and network conditions. Transport reassembles the sequence into the same ordered byte stream at the receiver.

### 2.7 Transport design issue: flow control versus congestion control

- **Flow control** asks: "Can the receiving host accept more bytes?" TCP uses the receiver-advertised window (`rwnd`).
- **Congestion control** asks: "Is the network path becoming overloaded?" TCP uses a congestion window (`cwnd`) and related algorithms.

**Relatable example:** Receiver ke paas sirf 10 books rakhne ki jagah hai = flow control. College gate par traffic jam hai = congestion control. Dono limits ek saath matter karti hain.

### 2.8 Advantages and limitations of transport-layer services

Advantages include application multiplexing, end-to-end reliability when required, standard process addressing, improved throughput through pipelining, and receiver protection. The main design challenge is choosing overhead, state, buffering and timers appropriate to each application's requirements. A reliable byte stream may add retransmission delays, whereas a low-overhead datagram service may force the application to manage loss and ordering.

## 3. TCP - Complete Introduction, Features and Segment Header

### 3.1 Hinglish - a numbered notebook delivery

Maan lo tum 50 pages ka assignment apne friend ko courier se bhej rahe ho. Har page ka number likha hai. Friend batata hai "Mujhe page 1 se 20 mil gaye, ab 21 bhejo." Agar 17th page missing hota to friend complete ordered assignment ready nahi kar sakta. TCP similar idea use karta hai, lekin important difference: TCP usually **individual packets ko number nahi deta, bytes ko sequence numbers deta hai**. ACK number batata hai **next expected byte**.

TCP ko train reservation samjho: journey start karne se pehle agreement, travel ke beech confirmation and pace control, phir proper close. Ye comparison perfect nahi hai because TCP underlying Internet par dedicated path reserve nahi karta.

### 3.2 Complete English - definition and characteristics

**Transmission Control Protocol (TCP)** is a connection-oriented, reliable transport protocol used in the Internet protocol suite. It presents applications with a full-duplex, ordered byte-stream communication service. TCP establishes connection state, assigns sequence numbers to stream bytes, detects losses or corruption, retransmits when necessary, honours the receiver's advertised window, and adjusts its sending behaviour in response to congestion.

Main characteristics:

1. **Connection-oriented:** a three-way handshake normally establishes a connection.
2. **Reliable byte stream:** received application bytes are presented in proper order, without duplicate data in the stream.
3. **Full-duplex:** both endpoints can send and receive simultaneously.
4. **Cumulative acknowledgement:** ACK value identifies the next byte the receiver expects in sequence.
5. **Sequence numbering:** data bytes have sequence numbers; SYN and FIN each consume one sequence-number position.
6. **Flow control:** the advertised receive window limits outstanding unacknowledged data.
7. **Congestion control:** the sender tracks a congestion window and adjusts transmission when needed.
8. **Checksum:** TCP checks segment integrity with a checksum covering a pseudo-header and TCP fields/data.
9. **Ordered delivery:** out-of-order bytes can be buffered until missing preceding bytes arrive.
10. **Stream semantics:** TCP does not preserve application message boundaries.

### 3.3 TCP header format - labelled examination diagram

```text
            TCP SEGMENT HEADER (not to scale)
  0                  15 16                 31
 +--------------------+--------------------+
 | Source Port (16)   | Destination Port(16)|
 +--------------------+--------------------+
 |             Sequence Number (32)        |
 +-----------------------------------------+
 |          Acknowledgment Number (32)     |
 +-----------+----------+------------------+
 |Data Offset|Res/Flags | Window Size (16) |
 +-----------+----------+------------------+
 |   Checksum (16)      | Urgent Pointer(16)|
 +----------------------+------------------+
 |        Options and Padding (if any)     |
 +-----------------------------------------+
 |             TCP Payload (data)          |
 +-----------------------------------------+
```

**Fields explained one by one:**

- **Source port (16 bits):** sender's transport port.
- **Destination port (16 bits):** receiver's transport port.
- **Sequence number (32 bits):** number of the first data byte in the segment, with special SYN/FIN rules.
- **Acknowledgement number (32 bits):** next sequence number expected when ACK is set.
- **Data offset (4 bits):** TCP header length in units of 4 bytes; 5 means 20 bytes. Options can make the header longer, up to 60 bytes.
- **Reserved/flags:** control information; common flags are SYN, ACK, FIN, RST, PSH and URG. Modern headers also include ECN-related bits.
- **Window size (16 bits):** advertised receive window; the negotiated Window Scale option permits larger effective windows.
- **Checksum (16 bits):** checks integrity of the TCP segment plus pseudo-header information.
- **Urgent pointer (16 bits):** used with the URG flag, rarely used by modern applications.
- **Options and padding:** examples are MSS, Window Scale, SACK Permitted and timestamps.
- **Data:** the application bytes carried by the segment.

**Exam memory:** ports -> sequence -> ACK -> header/flags/window -> checksum/urgent -> options -> data.

### 3.4 TCP flags and meaning

| Flag | Meaning | Common use |
|---|---|---|
| SYN | Synchronise sequence numbers | Open connection |
| ACK | Acknowledgement field valid | Acknowledge received bytes |
| FIN | No more data to send in one direction | Graceful close |
| RST | Reset/abort connection | Refuse or terminate abruptly |
| PSH | Hint to deliver buffered data promptly | Interactive transmission |
| URG | Urgent pointer is significant | Legacy urgent data |

**Important:** ACK is not a promise that the application has written data to disk. It means TCP has accepted corresponding stream bytes according to its acknowledgement rules.

### 3.5 Sequence and ACK numbering - solved trace

Suppose the sender's next sequence number is **1000**. It sends **500 bytes** of data.

- First data byte: sequence 1000.
- Data occupies sequence numbers 1000 through 1499.
- Receiver replies **ACK = 1500** if all 500 bytes arrived in sequence and nothing earlier is missing.
- Sender's next 300-byte segment uses sequence 1500 and occupies 1500 through 1799.
- A cumulative ACK of **1800** says the receiver next expects byte 1800.

```text
SENDER                                        RECEIVER
SEQ=1000, len=500 ---------------------------->
                          <------------------- ACK=1500
SEQ=1500, len=300 ---------------------------->
                          <------------------- ACK=1800
```

**Hinglish trick:** ACK 1500 ka matlab "byte 1499 tak sab sequence mein aa gaya; ab byte 1500 chahiye", not "1500 packets received".

### 3.6 TCP versus UDP - a 10-mark-ready distinction

TCP is usually suitable when the application requires an accurate, ordered byte stream and does not want to implement recovery itself. UDP suits protocols that need datagram boundaries and choose their own reliability, timing or congestion mechanisms. The distinction is about **provided services**, not an absolute rule that one is always faster or better.


## 4. TCP Connection Management - Establishment, Transfer and Release

### 4.1 Hinglish - friends confirming a meeting

Socho tum friend ko phone karte ho: "Hello, meri awaaz aa rahi hai?" Friend bolta hai, "Haan, aa rahi hai; tum meri sun pa rahe ho?" Tum reply karte ho, "Haan, sun raha hoon." Ab **dono directions** mein communication possible confirm ho gaya. TCP ka three-way handshake isi tarah initial sequence numbers exchange karta hai aur connection state establish karta hai.

Lekin TCP handshake *exactly phone conversation nahi* hai: it exchanges TCP control segments and confirms that both endpoints can exchange initial signalling. Network failure later phir bhi possible hai. Connection terminate karne ke time bhi ek side apna send direction close kar sakti hai while second side thodi der data bhej sakti hai - isi liye graceful close often four segments mein dikhaya jata hai.

### 4.2 Complete English - why connection management is needed

**Connection management** is the process of establishing, maintaining and terminating communication state between transport endpoints. Connection establishment helps both endpoints agree on initial sequence numbers and options. Connection release clears resources and indicates that no more bytes will be sent in a direction. A protocol must consider delayed, duplicate or lost control segments, simultaneous operations, resets and endpoint crashes.

A TCP connection is identified by its endpoint information. Before it opens, the server commonly uses a listening socket bound to a port. On accepting a connection, the server uses a distinct connected socket for that client.

### 4.3 Three-way handshake - complete diagram

Consider a client with Initial Sequence Number (ISN) **100** and a server with ISN **800**. These are simplified sample values.

```text
 CLIENT                                              SERVER
 CLOSED                                              LISTEN
   |                                                    |
   |------------ SYN, seq=100 -------------------------->|
   |           (Client becomes SYN-SENT)                |
   |<----------- SYN+ACK, seq=800, ack=101 --------------|
   |          (Server becomes SYN-RECEIVED)              |
   |------------ ACK, seq=101, ack=801 ----------------->|
   |                                                    |
 ESTABLISHED                                       ESTABLISHED

       SYN consumes one sequence-number position.
       Server SYN also consumes one position.
```

**Step 1 - SYN:** client requests connection establishment by sending SYN containing its ISN. The client moves from CLOSED to SYN-SENT.

**Step 2 - SYN + ACK:** server receives SYN, records connection state, sends its own ISN with an acknowledgement of the client's SYN, and enters SYN-RECEIVED.

**Step 3 - ACK:** client acknowledges the server SYN; client enters ESTABLISHED. Server enters ESTABLISHED when it receives the expected final acknowledgement. Data communication can then occur; in certain cases TCP can carry permitted data with control segments, but the conventional diagram shows an empty handshake.

**Why three steps?** Both sides need sequence-number synchronisation and evidence that their messages can reach the other side. The third acknowledgement confirms receipt of the server's SYN. TCP also negotiates some options during the opening exchange, such as MSS and the ability to use window scaling or selective acknowledgements.

### 4.4 Numerical: handshake sequence numbers

**Question:** Client sends SYN with seq = 4500; server returns SYN with seq = 9000. What ACK numbers are expected?

- Server's SYN+ACK acknowledges client SYN: **ACK = 4501**.
- Client's final ACK acknowledges server SYN: **ACK = 9001**.
- The next client data byte usually begins at sequence 4501; the next server data byte begins at 9001.

**Rule:** A SYN consumes one sequence number, even if it carries no application bytes. Similarly, FIN consumes one.

### 4.5 Data transfer after connection setup

Once established, each side maintains its own send and receive state. A TCP connection is **full duplex**: the two byte streams have independent sequence-number spaces, windows and acknowledgements. The receiving application reads data from its buffer. ACKs, timers and retransmissions maintain transport reliability, but TCP may report failure after repeated retransmission attempts or if the connection is reset.

**Difference between reliable transport and application transaction:** Even if a client sends a payment instruction and the connection fails, the client may not know whether the server completed the payment. The application needs transaction identifiers and idempotency/reconciliation to address that uncertainty.

### 4.6 Graceful connection termination - FIN and ACK

TCP permits one direction to finish independently. A common termination sequence is:

```text
 CLIENT (active closer)                         SERVER (passive closer)
 ESTABLISHED                                     ESTABLISHED
     |                                                 |
     |---------------- FIN, seq=x --------------------->|
     |<---------------- ACK, ack=x+1 -------------------|
 FIN-WAIT-2                                      CLOSE-WAIT
     |        (server may finish sending data)          |
     |<--------------- FIN, seq=y ---------------------|
     |---------------- ACK, ack=y+1 ------------------->|
 TIME-WAIT                                         CLOSED
     |                                                 |
     +----- wait appropriate time, then CLOSED --------+
```

Typical states for the active closer include **FIN-WAIT-1**, **FIN-WAIT-2**, **TIME-WAIT** and CLOSED. The passive closer may enter **CLOSE-WAIT** after receiving FIN and **LAST-ACK** after sending its FIN. In a simplified normal four-step exchange, the active closer sends FIN, receives ACK, receives peer FIN and sends the final ACK. FIN and ACK may sometimes be combined, so "always four separate packets" is not strictly true.

**Why TIME-WAIT?** The active closer can respond to a retransmitted final FIN and allows old duplicate segments from the previous connection to expire. The standard conceptual duration is commonly expressed as **2 x MSL** (Maximum Segment Lifetime); implementations vary in their timer details.

### 4.7 TCP state transition overview

```text
          SERVER                           CLIENT
          ------                           ------
        CLOSED                            CLOSED
          | passive open                    | active open
        LISTEN                             SYN-SENT
          | SYN received                    | SYN+ACK received
      SYN-RECEIVED                          | ACK sent
          | final ACK                       |
          +------------ ESTABLISHED --------+
                           |
                exchange two byte streams
                           |
       active close -> FIN-WAIT-1 -> FIN-WAIT-2
                                     | peer FIN
                                  TIME-WAIT -> CLOSED
       passive close -> CLOSE-WAIT -> LAST-ACK -> CLOSED
```

Other valid transitions exist, including simultaneous open and simultaneous close; the diagram shows the usual learning path.

### 4.8 Connection reset, crashes and abnormal termination

An **RST** can abruptly reset a TCP connection instead of gracefully finishing both byte streams. A reset may be sent when a segment arrives for a nonexistent connection or when an application chooses to abort. An endpoint crash, Wi-Fi failure or server shutdown may leave the peer unaware for some time. TCP uses retransmission/timeouts, and some connections use keepalive mechanisms or application-level heartbeat messages, but TCP cannot distinguish all failures instantly.

### 4.9 Common mistakes and exam hints

- TCP **SYN** is connection setup, not guaranteed reservation of a physical path.
- **FIN** means "I have finished sending", not necessarily "I cannot receive"; a half-closed connection can exist.
- **SYN** and **FIN** each consume one sequence number.
- **RST** is an abrupt reset; **FIN** supports orderly release.
- **TIME-WAIT** is mainly associated with the active closer of a normal close, not a universal state for every close.

## 5. TCP Reliability - ACKs, Retransmission, Timers and Error Recovery

### 5.1 Hinglish - assignment pages and missing page 4

Maan lo tum 6 pages bhejte ho: 1, 2, 3, 4, 5, 6. Receiver ko 1, 2, 3, 5, 6 mil gaye, par 4 missing hai. Receiver ne 5 and 6 dekhe, lekin complete ordered content abhi available nahi. **Cumulative ACK** missing sequence ki taraf indicate karta hai. Sender timeout ya suitable duplicate ACK signals dekhkar retransmit kar sakta hai.

TCP ka main goal "har packet ko separately confirm karna" nahi; **ordered byte stream** ko reliable tarike se expose karna hai. Receiver multiple arriving segments ko ek cumulative ACK se acknowledge kar sakta hai. Kuch connections **SACK** option use karte hain, jisse out-of-order received byte blocks ki additional information milti hai.

### 5.2 Complete English - sources of transmission problems

Packets may be lost because of congestion, link errors, queue overflow, route changes or other failures. Segments may arrive out of order because of different paths or processing delays; retransmitted data can create duplicates. A checksum detects many accidental corruptions. TCP's recovery mechanisms include sequence numbers, cumulative acknowledgements, retransmission timers, duplicate-ACK-based recovery and optional selective acknowledgement (SACK).

**Sequence numbers** identify positions in the byte stream. **Acknowledgements** inform the sender about in-order bytes received. **Retransmission** resends bytes believed lost. **Timers** detect absence of acknowledgement when other signals are unavailable.

### 5.3 Timeout-based retransmission

```text
SENDER                                          RECEIVER
SEQ=1000, 400 bytes ----X (lost)
     |
     | retransmission timer expires
     |
SEQ=1000, 400 bytes --------------------------->|
                         <-------------------- ACK=1400
```

An RTO (Retransmission Timeout) should adapt to the network's round-trip time (RTT) and RTT variation. A simplistic textbook timer that always assumes the same network delay can retransmit unnecessarily or react too slowly. TCP implementations use estimates and backoff rules; detailed equations vary by algorithm and standard.

### 5.4 Duplicate ACKs and fast retransmit

Suppose the sender has transmitted multiple segments and a segment is missing. Later out-of-order data can cause the receiver to repeatedly acknowledge the same next missing byte. With the classic TCP fast-retransmit heuristic, **three duplicate ACKs** are a signal that data may be missing; the sender may retransmit before the RTO expires. More modern TCP loss detection can use additional signals and algorithms, so this is the standard exam-level explanation rather than a complete implementation specification.

```text
SENDER                    RECEIVER
SEQ 1000 len 200 --------> ACK 1200
SEQ 1200 len 200 ---X      (missing)
SEQ 1400 len 200 --------> ACK 1200 (duplicate)
SEQ 1600 len 200 --------> ACK 1200 (duplicate)
SEQ 1800 len 200 --------> ACK 1200 (duplicate)
RETRANSMIT 1200 ---------> ACK 2000  (if buffered 1400..1999)
```

### 5.5 Selective Acknowledgement (SACK)

Cumulative ACK alone does not explicitly list all higher byte ranges that arrived out of order. TCP's optional **SACK** extension allows the receiver to report selected received ranges. For example, "ACK = 1200; additionally I have bytes 1400-1999." That helps the sender retransmit missing areas instead of blindly repeating everything.

**Important comparison with Unit II:** Go-Back-N and Selective Repeat are textbook sliding-window ARQ protocols. TCP has its own byte-stream, cumulative ACK and optional SACK mechanisms; it is not exactly identical to either classroom algorithm.

### 5.6 Reliability does not mean secure or always successful

TCP reliability concerns correct ordered delivery **while the connection functions**. It does not provide encryption, remote application processing confirmation, or a guarantee that communication can never fail. Cryptography and end-to-end application transaction handling are separate topics.

## 6. TCP Window Management - Most Important Detailed Chapter

### 6.1 Hinglish - a library book counter with limited space

College library mein counter par ek time mein sirf **5 books** rakh sakte ho. Librarian ne bola: "Tum 5 books ek saath la sakte ho; jaise hi main 2 shelf par rakh doon, counter par 2 spaces free ho jayengi." Sender = tum. Receiver = librarian. Counter capacity = receiving buffer. Librarian ka advertised free space = **receive window (`rwnd`)**. ACK ke saath window update aane par tum aur books bhej sakte ho.

Ab doosra scenario: College road par traffic jam hai. Chahe librarian ke paas 5 spaces free hon, road par ek time mein 2 books ka packet bhejna safe hai. Ye network pressure **congestion window (`cwnd`)** jaisa hai. Practical sending limit generally in-flight data ke liye **min(rwnd, cwnd)** se controlled hoti hai, with other protocol limitations also mattering.

### 6.2 Complete English - meaning and purpose

**TCP window management** controls how much unacknowledged data a sender can have outstanding in the network and how much new data a receiver can accept. It combines a **sliding window**, the receiver-advertised window for flow control and the sender's congestion-control window. Effective sending is subject to protocol state, application data availability, outstanding bytes and additional restrictions.

The main aims are to:

1. keep the sender from overwhelming the receiver's buffer;
2. allow pipelining of multiple segments for better throughput;
3. efficiently use a link during its round-trip time;
4. react to receiver-buffer changes and network congestion;
5. recover when the advertised receiver window becomes zero;
6. avoid excessive small-packet overhead in inefficient window-update patterns.

### 6.3 What is a sliding window? Sender view

Assume next unacknowledged byte is **1000** and the advertised send limit permits 2000 bytes outstanding. Then the current permitted outstanding sequence range is approximately 1000 through 2999. The sender may transmit data within this range without waiting after each segment.

```text
Byte-stream positions:
    0 ... 999 | 1000 ............. 2999 | 3000 ...
     ACKED    |     SEND WINDOW        | NOT YET ALLOWED

After ACK advances from 1000 to 1500:
    0 ...1499 | 1500 ............. 3499 | 3500 ...
     ACKED    |       NEW WINDOW       | NOT YET ALLOWED

Window 'slides' forward as earlier bytes are acknowledged.
```

**Exam language:** A sliding window is a dynamic sequence-number interval which permits the transmission of multiple bytes or segments before individual acknowledgements are received. The window advances when new bytes are acknowledged. In TCP, sequence numbers count **bytes**, not segments.

### 6.4 Sender-window variables: simple conceptual model

- **`SND.UNA`**: oldest unacknowledged sequence number.
- **`SND.NXT`**: next sequence number to be sent.
- **`rwnd` / `SND.WND`**: current sender view of the receiver's advertised allowed window.
- **`cwnd`**: congestion-control limit maintained by sender.
- **Bytes in flight**: bytes already sent but not yet acknowledged (simplified model).

A common teaching approximation is:

$$
  \text{usable new sending space}\approx \max(0,\min(rwnd,cwnd)-\text{bytes in flight}).
$$

It is an **illustrative calculation**, not an exact replacement for all TCP send-window rules: the window edge, scaling, sequence wrap, outstanding retransmissions and other implementation details also matter.

### 6.5 Numerical 1 - receiver window versus congestion window

**Question:** Receiver advertises `rwnd = 8000 bytes`, sender's `cwnd = 5000 bytes`, and there are 3200 bytes already in flight. Estimate new sending allowance.

1. Effective flight limit = min(8000, 5000) = **5000 bytes**.
2. Already in flight = 3200 bytes.
3. Approximate space for new data = 5000 - 3200 = **1800 bytes**.

**Answer:** approximately **1800 new bytes** may be sent under these simplified assumptions.

### 6.6 Numerical 2 - sliding by acknowledgements

Suppose `rwnd = 4000 bytes`, congestion is not the limiting factor, and current first unacknowledged byte is 2000.

- The allowed receiver-window range is approximately **2000-5999**.
- The sender transmits 1500 bytes beginning at 2000. These occupy 2000-3499.
- Receiver replies **ACK = 3500**, with the advertised window still 4000.
- The new window base becomes 3500 and the right edge becomes **7499**.

**Why?** ACK shifted forward by 1500 bytes, so the right edge can shift forward by the same number if the receiver continues advertising the same window size.

### 6.7 Receiver buffer, advertised window and flow control

The receiver holds out-of-order data and/or data waiting for the application to read it. When its buffer has less free space, the receiver may advertise a smaller window. The sender must respect that advertised limit. When the application consumes received data, window space becomes available, and the receiver can advertise a larger window.

```text
          RECEIVER BUFFER (capacity 8 KB)
 +--------------------+---------------------+
 | Occupied = 5 KB    | Free = 3 KB         |
 +--------------------+---------------------+
                        |
                        +-- advertised rwnd ~ 3 KB

If application reads 2 KB of buffered data,
free buffer can increase and receiver can advertise more space.
```

The exact relationship between buffer occupancy and advertised window is affected by implementation and bookkeeping. The diagram communicates the central purpose.

### 6.8 Zero-window condition and persist mechanism

If receiver buffer is full, receiver may advertise **window = 0**. The sender then stops sending ordinary new data beyond the allowed window. A difficulty arises: if a later window-update ACK gets lost, the sender could wait indefinitely. TCP therefore includes a **persist mechanism** that can send carefully controlled zero-window probes to learn whether the window has opened again.

```text
SENDER                                        RECEIVER
                    <---------------------- ACK=5000, WIN=0
 (pause new data)
                    ... receiver frees buffer ...
                    <------ WIN=3000  --X (update lost)
 [persist probe] ----------------------------->
                    <---------------------- ACK=5000, WIN=3000
 (normal sending resumes when allowed)
```

Zero window is generally a **receiver-side flow-control** condition; it does not necessarily mean Internet congestion.

### 6.9 Silly window syndrome - meaning and prevention

**Silly Window Syndrome (SWS)** occurs when very small segments or very small window increments cause repeated transfers with disproportionately large headers and processing overhead. It may happen if the application produces data one byte at a time or the receiver advertises tiny increases in free buffer space.

Common textbook remedies:

- **Clark's solution (receiver side):** avoid advertising tiny window increases until a useful amount of buffer space is free.
- **Nagle's algorithm (sender side):** combine certain small outgoing writes while unacknowledged data exists, reducing tiny packets. It must be understood with delayed ACK behaviour and application latency requirements.
- **Application buffering:** send a useful group of bytes where suitable.

Not every application should buffer small data: interactive applications may prefer responsiveness. TCP implementations provide controls such as `TCP_NODELAY` where appropriate.

### 6.10 Window scale option

The base TCP header's Window Size field is **16 bits**, representing at most **65,535** before scaling. This may be too small for high bandwidth-delay product connections. TCP's **Window Scale** option, negotiated during setup, permits a larger effective advertised receive window by applying a shift factor. The sender's congestion window is a separate limit and is not encoded directly in this header field.

### 6.11 Bandwidth-delay product and window size

**Bandwidth-delay product (BDP)** approximates the amount of data that could be in flight over a connection path if the sender is to keep a link busy:

$$
\text{BDP(bits)}=\text{Bandwidth(bits/s)}\times \text{RTT(seconds)}.
$$

For a **20 Mbps** path and **100 ms RTT**:

$$
\text{BDP}=20{,}000{,}000\times0.1=2{,}000{,}000\text{ bits}
=250{,}000\text{ bytes}.
$$

An allowed flight size around **250 KB** is required to fully cover this idealised path's bandwidth-delay product. Actual achievable throughput depends on congestion, bottlenecks, protocol efficiency, loss, receiver behaviour and other factors.

### 6.12 Window efficiency: numerical

For a textbook Stop-and-Wait data exchange, if frame transmission time is **2 ms** and round-trip propagation/response delay is **18 ms** (ignoring ACK transmission and processing), approximate channel utilisation is:

$$
 U\approx\frac{2}{2+18}=0.1=10\%.
$$

With pipelining and a sufficiently large window, more frames can remain in flight, improving utilisation. TCP's window is measured in bytes, while some textbook ARQ formulas count frames; do not mix the units.

### 6.13 Window management compared with congestion control

| Question | Receiver Flow Control | Sender Congestion Control |
|---|---|---|
| Main concern | Receiver capacity | Network path capacity |
| Primary quantity | `rwnd` | `cwnd` |
| Managed by | Receiver advertises; sender obeys | Sender's congestion algorithm |
| Symptom | Receiver buffer is full | Loss, delay or ECN signals |
| Adjustment | Window update as space frees | Slow start, avoidance, loss response |

**High-scoring exam sentence:** TCP sender may transmit data only within its allowed send window, while respecting both the remote receiver's advertised window and the local congestion-control limits.

## 7. TCP Congestion Control - Supporting Window Management

### 7.1 Hinglish - lift versus crowded road

Tumhare apartment ki lift 6 people allow karti hai (receiver limit). Lekin agar 200 log building ke gate par rush kar rahe hain, rasta crowded ho jayega (network congestion). TCP ko dono situations ka khayal rakhna hota hai. Congestion Window (`cwnd`) sender ki apni limit hoti hai, jo feedback and algorithms ke basis par change hoti hai.

### 7.2 Complete English - concept and major techniques

TCP congestion control regulates the amount of unacknowledged data to avoid persistently overwhelming the network. Classic textbook algorithms discuss:

1. **Slow Start:** begin with a controlled initial congestion window and grow it rapidly, typically approximately doubling per RTT while acknowledgements arrive, until a threshold or another event. The actual initial size is implementation- and standards-dependent.
2. **Congestion Avoidance:** increase cautiously, often presented using additive increase around one MSS per RTT in classic algorithms.
3. **Fast Retransmit:** retransmit likely missing data after sufficient duplicate-ACK evidence instead of always waiting for a timeout.
4. **Fast Recovery:** adjust `cwnd` following inferred loss; exact operations differ among Reno, NewReno and modern algorithms.
5. **Timeout response:** treat an RTO as a significant sign of trouble and reduce sending aggressiveness.

Modern stacks can use CUBIC, BBR and other techniques, so **the textbook AIMD/Reno discussion is an illustration rather than the one algorithm used everywhere**.

### 7.3 Slow start example - simplified MSS units

If `cwnd` begins at 1 MSS in a simplified classroom model and doubles per RTT:

| RTT round | Approximate cwnd |
|---|---:|
| Initial | 1 MSS |
| 1 | 2 MSS |
| 2 | 4 MSS |
| 3 | 8 MSS |
| 4 | 16 MSS |

This is a conceptual model. Actual TCP initial windows are often larger than 1 MSS, and real ACK arrival, delayed ACK, receiver limits and implementation affect growth.

### 7.4 Congestion versus receiver flow: integrated numerical

Given `rwnd = 12,000 bytes`, `cwnd = 7,000 bytes` and `4,500 bytes` in flight, new sending allowance is approximately `min(12,000, 7,000) - 4,500 = 2,500 bytes`. If receiver later reports `rwnd = 3,000`, then new allowance becomes `max(0, min(3,000, 7,000) - 4,500) = 0`. The sender must wait for acknowledgements and/or further window changes. This simple formula is for teaching the difference between the two limits.

## 8. Session Layer - Design Issues and Functions

### 8.1 Hinglish - online examination interrupted

Imagine karo tum online mock test de rahe ho. Tumne 30 mein se 18 questions solve kar liye. Suddenly Internet disconnect ho gaya. Agar test system session ko manage karke progress save karta hai, reconnect karne par tum question 19 se continue kar sakte ho. Ye **session management** aur **checkpointing** ko samajhne ke liye easy example hai. Lekin real software mein ye features aksar Application Layer implement karti hai; OSI Layer 5 ka alag software hona zaroori nahi.

Ab video call mein ek person bol raha hai, doosra sun raha hai. Kaun kab speak karega, meeting kab start hogi, kab close hogi - ye **dialogue control** ke examples ban sakte hain. Session Layer logical conversation ko organise karta hai; TCP network bytes ko transport karta hai.

### 8.2 Complete English - definition

The **Session Layer** is Layer 5 of the OSI reference model. It provides conceptual services for establishing, managing, synchronising and terminating communication sessions between applications. A session is a logical dialogue or association between communicating entities. The layer aims to coordinate the conversation rather than choose an IP route or directly control individual TCP segments.

### 8.3 Session Layer design issues in detail

**1. Session establishment.** Determine that the participating processes are ready to communicate and set up relevant dialogue state. Depending on the application architecture, a session may carry identifiers, negotiated options or recovery context.

**2. Session maintenance.** Keep track of conversation progress, application requests, roles and state across a sequence of messages.

**3. Dialogue control.** Define which participant sends at which time. Protocols may support half-duplex turn-taking or full-duplex dialogue. In full duplex, both sides may transmit concurrently.

**4. Synchronisation.** Insert logical checkpoints into long operations, so that recovery does not require repeating the entire conversation from the beginning.

**5. Token management.** A conceptual control token can determine which participant is temporarily authorised to perform a particular operation, reducing conflicts in coordinated workflows.

**6. Session release.** Terminate the dialogue in an orderly way, clear session resources and communicate completion or failure.

**7. Recovery and exception handling.** Decide what happens after an interrupted operation, lost connectivity, timeout or application crash.

**8. Security association and control.** A session may track authentication and authorisation information at the application level; authentication and encryption are not automatic properties of the OSI Session Layer itself.

### 8.4 Diagram - checkpointing in a long operation

```text
BEGIN --- Chunk A --- [CHECKPOINT 1] --- Chunk B --- [CHECKPOINT 2]
                                                  |
                                                  X DISCONNECTION
                                                  |
RECONNECT -> RECOVER SESSION -> RESUME AFTER CHECKPOINT 2

Without checkpoints: entire operation may need to restart.
With checkpoints: only unfinished part may need repeating,
if the application/protocol supports recovery correctly.
```

### 8.5 Session Layer versus Transport Layer

| Feature | Transport Layer | Session Layer |
|---|---|---|
| Main focus | End-to-end delivery service | Organise logical dialogue |
| Example questions | Which port? Are bytes lost? | Who speaks now? Where to resume? |
| Reliability | TCP ACK/retransmission | Session-level recovery/checkpoints |
| Unit | TCP segment or byte stream | Conversation/session messages |
| Common implementation | TCP, UDP | Often application/framework logic |

### 8.6 Limitations and real-world perspective

The seven-layer OSI model separates Session and Presentation as conceptual layers. The widely used TCP/IP model commonly groups session- and presentation-like functions with the **Application Layer**. Therefore, saying every HTTP/TCP conversation uses a separate dedicated session-layer protocol is inaccurate. Instead, the textbook layer explains a class of responsibilities that real protocols and applications may implement.


## 9. Remote Procedure Call (RPC) - Working, Architecture and Problems

### 9.1 Hinglish - calling a function that lives on another computer

JavaScript/Java mein tum function call karte ho: `getStudentMarks(42)`. Normal local function same process ke code/memory environment mein execute hota hai. Ab socho marks ka data university ke **remote server** ke paas stored hai. Tumhare laptop par function likha hai, lekin us function ki actual implementation remote server par hai. RPC mechanism local-function jaisa interface de sakta hai:

```text
   result = getStudentMarks(42)
```

Backend ko request network se jaani padegi, server ko student 42 find karna hoga, aur result wapas bhejna hoga. Tumhe ek simple function call dikh sakti hai, lekin andar **request serialise, network transport, remote execution, response serialise, error handling** sab hota hai. Isi ko **Remote Procedure Call (RPC)** bolte hain.

**Relatable example:** Tum restaurant table par waiter ko order dete ho; waiter kitchen tak message le jaata hai, chef dish banata hai, waiter result laata hai. Tum directly kitchen mein nahi gaye. Client stub waiter jaisa, remote server chef jaisa.

### 9.2 Complete English - definition

**Remote Procedure Call (RPC)** is an interprocess communication technique that allows a program to invoke an operation offered by a process on another address space, often another computer, in a form resembling a local procedure call. RPC systems use request/response messages, serialisation and supporting software to hide some of the communication details. Because network failures, latency and remote state exist, remote calls cannot be made truly identical to local calls.

### 9.3 Components of RPC architecture

1. **Client:** calls the operation and supplies arguments.
2. **Client stub/proxy:** provides a local callable interface, encodes arguments and communicates with the RPC runtime.
3. **RPC runtime / transport:** manages request transfer, identifiers, timeouts, replies and other communication duties.
4. **Server stub/skeleton:** decodes the request, extracts arguments and invokes the real server function.
5. **Server procedure:** performs the requested computation.
6. **Response path:** result or error is encoded and sent back to the client.
7. **Interface definition:** an interface definition language or contract can describe operations and message schemas for consistent code generation.

### 9.4 RPC architecture - labelled diagram

```text
             CLIENT MACHINE                         SERVER MACHINE
 +---------------------------+          +--------------------------------+
 | Client Application        |          | Server Application             |
 |   getMarks(42)            |          |   getMarks(studentId)          |
 |            |              |          |             ^                  |
 |            v              |          |             |                  |
 |       Client Stub         |          |        Server Stub             |
 |     marshal arguments     |          |   unmarshal arguments          |
 |            |              |          |             ^                  |
 |            v              |          |             |                  |
 |       RPC Runtime --------+==========+-------- RPC Runtime           |
 +---------------------------+ NETWORK  +--------------------------------+

      Request travels ->                          <- Result / error returns
```

### 9.5 RPC working: complete eight-step explanation

**Step 1:** The client application calls a local proxy function using the remote procedure's name and arguments.

**Step 2:** The client stub **marshals** the arguments into a standard message format (for example, a binary schema or JSON representation).

**Step 3:** The client RPC runtime sends the request to the server through a network transport. It may attach a request ID, method name, deadline and authentication metadata.

**Step 4:** The server runtime receives the request and passes it to the corresponding server stub or handler.

**Step 5:** The server stub **unmarshals** the arguments and calls the actual local server procedure.

**Step 6:** The server procedure performs the computation and produces a result or an error.

**Step 7:** The server stub marshals the result, and the server runtime sends the reply.

**Step 8:** The client stub decodes the reply and returns the value or raises/reports an error to the caller.

### 9.6 Marshalling and unmarshalling

**Marshalling** is the conversion of procedure arguments or results into a standard transmissible representation. **Unmarshalling** is reconstruction of those values at the receiver. It is necessary because the client and server may have different languages, data formats, endianness, memory layouts or operating systems.

**Example:** `getTotal(10, 20)` could be encoded as a message containing operation name `getTotal`, integer arguments `10` and `20`, and a request identifier. Server decodes, calculates `30`, and returns an encoded reply. Encoding is a presentation/data-representation function even though RPC systems commonly handle it within their software.

### 9.7 Binding and locating remote services

**Binding** means connecting the client-side operation to the appropriate remote endpoint or service. A system might use configuration, DNS names, a service registry or discovery mechanism. Correct destination and protocol information are essential. Changing the remote server's address should ideally not require rewriting every client call; a service name or registry helps decouple callers from physical locations.

### 9.8 RPC call semantics - major exam topic

When a response is lost, the client may be uncertain whether the server completed the operation. The client might retry, but blindly repeating a non-idempotent operation can be dangerous.

| Semantics | Idea | Important limitation |
|---|---|---|
| At-most-once | Avoid executing same request more than once | Operation may execute zero times if communication fails |
| At-least-once | Retry until success response when feasible | Same operation may run more than once |
| Exactly-once | Ideal goal of one durable effect | Hard to guarantee end-to-end across failures without extra transactional machinery |

**Idempotent operation:** repeating an operation produces the same intended final effect, such as "set student status to ACTIVE" if designed correctly. "Increase balance by ₹500" is not inherently idempotent, because retry can cause double credit. Real systems often use an **idempotency key** or a durable request-ID record to make retries safer.

### 9.9 Synchronous and asynchronous RPC

A **synchronous RPC** makes the caller wait until a reply or failure/timeout. An **asynchronous RPC** returns control without blocking for the whole remote operation, often using futures, promises or callbacks. The method may still require careful response handling; asynchronous does not mean guaranteed delivery.

### 9.10 Advantages and disadvantages

**Advantages:** modular system design, simple operation-oriented interface, separation of client and server, reusable business functions, support for distributed applications, and possible cross-language communication via formal schemas.

**Disadvantages:** network latency, timeout uncertainty, server failure, partial failure, serialisation cost, version compatibility issues, security requirements, and the danger of treating a remote call as a cheap local function call.

### 9.11 RPC versus local procedure call

| Basis | Local call | RPC |
|---|---|---|
| Location | Usually same process | Another process or machine |
| Communication | In-process execution | Encoded network messages |
| Latency | Usually very low | Network and server delay |
| Failure | Process/runtime failures | Plus connection/timeout/remote failures |
| Arguments | In-memory values, pointers possible | Serializable values and explicit contracts |
| Transparency | Direct call | Approximate only; errors must be handled |

### 9.12 Modern examples and exam caveat

RPC ideas appear in systems such as gRPC and many internal distributed service frameworks. However, **RPC is not restricted to a single OSI Session Layer implementation**: a real system often places RPC at the application level over HTTP/2, TCP or another transport. The syllabus lists RPC with Session Layer design ideas, so describe it there in exams while recognising the real-life implementation difference.

## 10. Presentation Layer - Design Issues and Functions

### 10.1 Hinglish - same message, different languages and formats

Socho tumhare friend ne ek message Hindi text mein bheja aur tumhara system character encoding ko galat interpret kare. Message unreadable ho sakta hai. Doosri situation: tumne **2 MB** ki image send ki; compression se 500 KB ho gayi, to transmission resources kam lag sakte hain. Teesri situation: bank app se sensitive details ja rahi hain; encryption data ko unauthorised logon ke liye unreadable banane ka goal rakhti hai.

Presentation Layer ko **translator + packer + safe locker** samjho. Translator format ensure karta hai, packer data size reduce karta hai, locker data encrypt karta hai. Ye teen useful categories hain, lekin real programs ke andar alag libraries/protocols ye kaam karte hain.

### 10.2 Complete English - definition

The **Presentation Layer** is Layer 6 in the OSI reference model. It describes services that make information exchanged by communicating systems interpretable and suitably represented. Its conventional responsibilities include **data translation, character encoding, serialisation, data compression and encryption/decryption**. This layer helps reduce differences between internal data representations used by different computers.

A sending system may transform an application data structure into a standard external representation, compress it if appropriate, protect it cryptographically and transmit it. At the receiver, the corresponding operations reconstruct the original meaning and data according to the agreed format.

### 10.3 Detailed design issues

**1. Data representation and syntax.** Systems must agree about how numbers, text, dates, arrays and objects are represented. For example, the integer 500 can have different binary encodings, byte orders or data-type representations.

**2. Character encoding.** A program must know whether text is UTF-8, UTF-16 or another encoding. The same byte sequence can produce different characters under incompatible encodings.

**3. Endianness (byte order).** Multibyte numbers must be interpreted consistently. **Big endian** stores the most significant byte first; **little endian** stores the least significant byte first. Standard serialisation formats eliminate ambiguity by defining a representation.

**4. Data serialisation.** Objects or structures can be converted into transferable formats such as JSON, CBOR, Protocol Buffers or ASN.1-based representations. Receiver decoding must match the format and version.

**5. Compression.** Represent data with fewer bits when advantageous. Lossless methods reconstruct exact original data, while lossy methods intentionally discard some information.

**6. Encryption and confidentiality.** Convert plaintext into ciphertext using a cryptographic algorithm and key so unauthorised parties cannot understand its contents.

**7. Integrity and authenticity.** Cryptographic MACs, authenticated encryption and digital signatures help detect unwanted changes and verify origin, depending on the mechanism.

**8. Compatibility and negotiation.** Communicating systems must agree on encoding versions, compression methods, algorithms and data structures.

### 10.4 Presentation pipeline - clear diagram

```text
SENDER APPLICATION                 RECEIVER APPLICATION
       |                                    ^
       v                                    |
   Original data                       Original data
       |                                    ^
       v                                    |
  Encode / serialise                  Decode / parse
       |                                    ^
       v                                    |
     Compress                           Decompress
       |                                    ^
       v                                    |
      Encrypt                            Decrypt
       |                                    ^
       +-------- Network transfer ---------+

This is a CONCEPTUAL sequence, not a rule for all protocols.
Some systems omit operations or use a different arrangement.
```

**Security note:** Order of compression and encryption is context-dependent. Compression of secret-bearing data together with attacker-controlled content can leak information through ciphertext lengths. Do not apply "always compress before encrypt" as an unconditional security rule.

### 10.5 Example: data format conversion

A Java application stores a user record in an object. A Python server does not know the memory layout of that Java object. The data can be serialised as a JSON record:

```text
  {"studentId": 27, "name": "Aman", "semester": 5}
```

The receiving application parses the JSON fields and constructs its own in-memory values. Both sides require agreement about field names, valid data types, encodings and handling of missing fields. For dates, timestamps and decimal money values, additional conventions are important.

### 10.6 Advantages and limitations

Presentation-related functions support interoperability, efficient transmission, protection of confidential data and use of standard data formats. Limitations arise from encoding overhead, CPU cost, lossy quality reduction, cryptographic key management and security mistakes. The OSI conceptual division should not be confused with the exact placement of these functions in every Internet application.

## 11. Data Compression - Detailed Techniques, Examples and Numericals

### 11.1 Hinglish - repeating a sentence efficiently

Maan lo tum copy par likhte ho: `AAAAAABBBBCC`. Agar har character alag likhne ke bajay `6A4B2C` likho, to repeated letters ko short form mein store kar sakte ho. Ye **Run Length Encoding (RLE)** ka simple idea hai. Lekin agar input `ABCDXYZ` ho, har letter alag hai, to RLE se size kam nahi bhi ho sakta.

Ab photo ka example: Image ko EXACT same pixel values ke saath restore karna hai to **lossless** chahiye. Agar slightly reduced visual detail acceptable hai, to **lossy compression** size aur kam kar sakta hai. Yeh reason hai ki text/source code ke liye exact reconstruction critical hota hai, jabki photos and streamed media some loss tolerate kar sakte hain.

### 11.2 Complete English - definition and purpose

**Data compression** is the process of representing information using fewer bits than its original representation, subject to specified reconstruction requirements. It seeks to remove **statistical redundancy** or information judged less important, thereby reducing storage and transfer costs. Compression requires an encoder and a decoder; compressed data must be interpreted using the matching algorithm and settings.

Main benefits include lower transmission volume, reduced storage requirements and potentially faster transfers. Costs include encoding/decoding computation, possible memory use, compatibility requirements and quality loss in lossy methods. Some inputs cannot be compressed further by a given lossless algorithm, and some may even grow due to overhead.

### 11.3 Types of compression

**Lossless compression:** The decompressed output is exactly the original data. It is suitable for text, program files, databases and any data where correctness of every bit matters. Common algorithm families include RLE, Huffman coding, dictionary compression (LZ77/LZ78/LZW) and DEFLATE (which combines ideas).

**Lossy compression:** The decompressed data is an approximation, not bit-for-bit identical. It is used when a small reduction in fidelity is acceptable in return for a lower data rate. Examples include common photographic and audio/video codecs. A lossy system often uses perception models, quantisation and entropy coding.

| Basis | Lossless | Lossy |
|---|---|---|
| Reconstruction | Exact | Approximate |
| Information discarded | No required original information lost | Some information intentionally discarded |
| Suitable for | Text, code, records | Photos, audio, video |
| Examples | RLE, Huffman, LZ77 | JPEG, MP3, lossy video codecs |
| Main trade-off | Often less dramatic reduction | Higher reduction with possible quality loss |

**Technical accuracy:** PNG is commonly lossless image compression. JPEG is often lossy, though JPEG-family standards may also define other modes. The classroom examples use the usual common formats.

### 11.4 Run Length Encoding (RLE) - full working

RLE stores repeated runs as a **count + value** pair.

**Input:** `AAAAABBBCCCCDD`

**Step 1:** `AAAAA` = 5 of A, `BBB` = 3 of B, `CCCC` = 4 of C, `DD` = 2 of D.

**Step 2:** Encoded symbolic representation = **`5A3B4C2D`**.

**Step 3:** Decoder reads each count/value and repeats the symbol that many times.

```text
INPUT:       A A A A A | B B B | C C C C | D D
RUNS:          5 A    |  3 B  |   4 C   | 2 D
OUTPUT:            5A3B4C2D
```

**Size numerical (simplified):** If each original ASCII letter occupies one byte, 14 letters = 14 bytes. If each encoded count and symbol is stored as one ASCII character in this simplified example, `5A3B4C2D` = 8 bytes. Saving = 6 bytes; reduction = `(14-8)/14 * 100 = 42.86%`. Real RLE formats must specify how counts are encoded, how digits are escaped, and how long runs are represented.

**Advantages:** simple, fast, excellent for long constant runs. **Limitations:** poor for irregular data and may expand files when runs are short.

### 11.5 Huffman coding - principle

**Huffman coding** is a lossless variable-length prefix-code method. More frequent symbols generally receive shorter codewords and less frequent symbols receive longer codewords. A **prefix-free** code is one where no complete codeword is the prefix of another, allowing unambiguous decoding.

**Simple example:** Suppose symbols occur with frequencies A:5, B:2, C:1, D:1. A possible Huffman-like code assignment is:

| Symbol | Frequency | Example code | Bits per symbol |
|---|---:|---|---:|
| A | 5 | `0` | 1 |
| B | 2 | `10` | 2 |
| C | 1 | `110` | 3 |
| D | 1 | `111` | 3 |

Check prefix rule: no complete listed code begins another codeword. The encoded symbol-content length is `5*1 + 2*2 + 1*3 + 1*3 = 15 bits`. A fixed-length two-bit scheme for four symbols would use `9*2 = 18 bits`, so this sample saves 3 data bits **before accounting for dictionary/tree metadata**.

```text
                (9)
               /   \
             A:5   (4)
                  /   \
                B:2   (2)
                     /   \
                   C:1   D:1

Left edge=0, right edge=1
A=0, B=10, C=110, D=111
```

**Huffman construction steps:** (1) list symbols and frequencies, (2) place frequencies in an ascending-priority queue, (3) remove two smallest, (4) combine them into a new node with summed weight, (5) repeat until one tree remains, (6) label branches 0 and 1, (7) read codewords from root to leaves.

### 11.6 LZ77 / dictionary compression - principle

Dictionary-based compression replaces repeated strings with references to earlier occurrences. **LZ77** uses a sliding history window; a later repeated sequence may be represented by a reference such as **(distance, length)** rather than resending all characters.

**Hinglish example:** Tumhare paragraph mein "COMPUTER NETWORK" 30 baar aa raha hai. Pehli baar full phrase likhne ke baad next occurrences ke liye "same as earlier at position X" reference use kar sakte ho. Real algorithms have defined window sizes, match lengths and bit formats.

```text
Input pattern:  ABCDEF - ABCDEF - ABCDEF
History:        ABCDEF
Next repeat:    (back-reference to previous ABCDEF)
```

LZ78 and LZW build and refer to dictionary entries in related but different ways. ZIP-style compression commonly uses DEFLATE, which combines LZ77 matching with Huffman coding. Exact packet formats and compression ratios vary with the data.

### 11.7 Lossy compression and quantisation

Lossy compressors often transform signals into another representation, reduce precision of some coefficients by **quantisation**, and entropy-code the result. For example, tiny high-frequency image details may be reduced to save space. Decoding approximates the original but cannot recreate deliberately discarded information exactly.

**Advantages:** significant size reductions for suitable images/audio/video and lower bandwidth needs. **Disadvantages:** artefacts, reduced quality at high compression, possible cumulative degradation from repeated lossy re-encoding, and unsuitability for exact records or program files.

### 11.8 Compression ratio and percentage saving - formulas

$$
 \text{Compression ratio}=\frac{\text{Original size}}{\text{Compressed size}}
$$

$$
 \text{Percentage reduction}=\frac{\text{Original}-\text{Compressed}}{\text{Original}}\times100.
$$

**Example 1:** Original 800 KB, compressed 200 KB. Ratio = `800/200 = 4:1`. Reduction = `600/800 * 100 = 75%`.

**Example 2:** Original 1.2 MB, compressed 900 KB, using decimal MB for simplicity. Original 1200 KB. Ratio = `1200/900 = 1.333:1`. Reduction = `300/1200 * 100 = 25%`.

**Example 3:** Text 100 KB expands to 103 KB. Ratio = `100/103 = 0.971:1` and **percentage reduction = -3%**. A negative value means expansion, not savings.

### 11.9 Exam-ready comparison: RLE versus Huffman

| Basis | RLE | Huffman coding |
|---|---|---|
| Principle | Count repeated consecutive symbols | Shorter codes for frequent symbols |
| Main structure | Runs | Frequency tree / prefix codes |
| Best suited | Long repetitions, simple images | Unequal symbol frequencies |
| Challenge | Poor for varied content | Requires code-tree information |
| Reconstruction | Lossless | Lossless |

## 12. Cryptography - Detailed Concepts, Methods and Diagrams

### 12.1 Hinglish - locked box and secret key

Socho tum bank statement courier se bhej rahe ho. Normal envelope koi khol kar read kar sakta hai. Tum data ko ek **locked box** mein put karte ho. Jiske paas correct key hai, woh box open karega. Cryptography ka **encryption** data ko plaintext se ciphertext mein transform karta hai so that unauthorised observer data read na kar sake. **Decryption** suitable key se plaintext recover karti hai.

Lekin ek problem: Agar stranger parcel mein data replace kar de, kya receiver ko pata chalega? Sirf confidentiality se integrity guaranteed nahi hoti. Isi liye cryptography mein **hashing, authentication codes, digital signatures and authenticated encryption** bhi important hain.

### 12.2 Complete English - definition and objectives

**Cryptography** is the study and application of mathematical methods for protecting information and communication. Modern cryptographic systems address multiple security goals:

1. **Confidentiality:** prevent unauthorised reading of information.
2. **Integrity:** detect unauthorised or accidental modification.
3. **Authentication:** provide evidence of an entity's identity or a message's origin.
4. **Non-repudiation (in suitable contexts):** provide evidence that a signer performed a signing action; legal meaning depends on additional context and key control.
5. **Secure key establishment:** help parties establish shared secrets over untrusted networks.

**Terminology:** plaintext = readable data; ciphertext = encrypted data; key = secret/private/public cryptographic parameter; encryption algorithm = transformation controlled by a key; decryption = inverse recovery where supported.

### 12.3 Basic cryptosystem - labelled diagram

```text
SENDER                                                     RECEIVER
Plaintext ----> [ ENCRYPTION ] ----> Ciphertext ----> [ DECRYPTION ] ----> Plaintext
                    ^                                     ^
                    |                                     |
                Key or public key                    Matching key

An eavesdropper can observe ciphertext on the network.
Security depends on sound algorithms, correct use and key protection.
```

### 12.4 Symmetric-key cryptography

In **symmetric cryptography**, the same shared secret key (or directly related keys) is used by sender and receiver for encryption and decryption. The key must be kept confidential and distributed securely. **AES** is a widely used modern block cipher; modes such as **AES-GCM** combine encryption with an authentication tag when used properly.

**Working:** (1) sender and receiver establish shared secret, (2) sender encrypts plaintext with appropriate nonce/IV and scheme, (3) ciphertext plus required metadata is sent, (4) receiver verifies authentication tag where provided and decrypts with secret key.

**Advantages:** efficient for large data, comparatively low computation cost, widely standardised methods. **Limitations:** safe key sharing and key rotation are difficult at scale; encryption alone may not authenticate content unless an authenticated scheme is used.

```text
  Shared secret key K
        |                                  |
 Plaintext --AES-GCM with nonce--> Ciphertext + authentication tag
                                               |
                             Receiver verifies and decrypts with K
```

### 12.5 Asymmetric/public-key cryptography

In **asymmetric cryptography**, each participant has a **public key** that may be shared and a **private key** that must be protected. The pair supports particular operations depending on the scheme. For a public-key encryption system, someone encrypts a message for the receiver using the receiver's public key, and the receiver decrypts it with their private key. For digital signatures, a signer signs with the **private key**, and others verify using the corresponding **public key**.

**Example:** A college publishes a public key. Students use it to encrypt a small secret to the college, and only the matching private key can decrypt under the correct secure scheme. For larger data, practical systems normally use **hybrid encryption**: public-key techniques establish/protect a symmetric key, then symmetric encryption protects bulk data.

**Examples:** RSA is a public-key algorithm usable with suitable, secure padding/signature schemes; elliptic-curve cryptography includes different signing and key-agreement algorithms. Do not say all public-key encryption schemes use the same operation or performance.

### 12.6 Symmetric versus asymmetric comparison

| Basis | Symmetric | Asymmetric |
|---|---|---|
| Key arrangement | Shared secret | Public/private pair |
| Main strength | Efficient bulk encryption | Public-key distribution and authentication/key exchange |
| Key distribution | Secret must be shared | Public key can be shared but authenticity must be verified |
| Speed | Generally faster for bulk data | Generally more computation-intensive |
| Examples | AES | RSA, elliptic-curve-based schemes |
| Typical use | Encrypt application data | Establish keys, verify signatures |

### 12.7 Hash functions and integrity

A **cryptographic hash function** transforms data of arbitrary length into a fixed-size digest. A secure modern hash is designed to make it computationally difficult to find two distinct inputs with the same digest (collision resistance) or reverse a digest to find a suitable input (preimage resistance). **SHA-256** is a common example.

```text
  "Computer Network" --[ SHA-256 ]--> fixed-length digest
  "computer network" --[ SHA-256 ]--> very different digest (likely)
```

**Important:** A plain unkeyed hash by itself does **not** prove who sent a message; an attacker who modifies the message can also calculate its hash. Message authentication uses a secret-based MAC, a digital signature or an authenticated encryption scheme.

### 12.8 Message authentication code (MAC) and HMAC

A **Message Authentication Code (MAC)** is a tag generated using a shared secret key and message data. The receiver holding the same secret verifies the tag to detect unauthorised modification and obtain origin assurance relative to holders of that key. **HMAC** is a common construction using a cryptographic hash and a secret key. MACs do not provide public verifiability like digital signatures.

```text
SENDER:  message + shared key -> HMAC tag -> send both message and tag
RECEIVER: message + shared key -> expected tag -> compare securely
```

### 12.9 Digital signatures and certificates

A **digital signature** provides a way to verify that data was signed using a corresponding private signing key and has not changed since signing. The signer applies the signing algorithm with their private key; verifier checks using public key. Certificates bind a public key to an identity under an appropriate trust structure. A TLS client validates certificates and hostname information subject to protocol and trust policies.

**Difference:** Encryption mainly protects confidentiality; a signature verifies authenticity/integrity of signed content. Digital signing is **not** the same as encrypting with the private key, despite misleading textbook shortcuts.

### 12.10 Classical Caesar cipher - simple worked example

A classical Caesar shift with key `k=3` replaces A->D, B->E, ... and wraps around Z->C.

**Plaintext:** `CAT`

- C -> F
- A -> D
- T -> W

**Ciphertext:** `FDW`. Decryption shifts letters backward by 3, recovering CAT.

For letters encoded as 0..25:

$$
 E_k(x)=(x+k)\bmod26,\qquad D_k(y)=(y-k)\bmod26.
$$

**WARNING:** Caesar cipher is an educational example only. It is easily broken and should never be used for modern security.

### 12.11 TLS and modern Internet example

When a browser visits an HTTPS site, TLS typically authenticates the server using certificates, negotiates cryptographic parameters, establishes shared session keys and protects application data with authenticated encryption. In a typical modern deployment, TLS lies **above TCP and below HTTP**, as part of the real Internet protocol stack (HTTP/3 uses QUIC over UDP and has integrated TLS-related security). This shows why OSI presentation functions are not always implemented as a distinct Layer 6 protocol.

### 12.12 Common cryptography attacks and precautions (conceptual)

- **Eavesdropping:** an attacker listens to traffic; encryption helps protect contents.
- **Man-in-the-middle:** a party intercepts or alters exchanges; properly authenticated key establishment helps prevent impersonation.
- **Replay:** a previously valid request is resent; protocols can use nonces, sequence numbers, timestamps and replay protections.
- **Weak keys:** predictable or exposed keys undermine strong encryption algorithms.
- **Poor nonce/IV handling:** reusing a nonce in schemes that require uniqueness can severely damage security.
- **Credential confusion:** hashing a password is different from encrypting data; secure password storage uses purpose-built password hashing such as Argon2id, scrypt or PBKDF2 with salt and suitable parameters.

### 12.13 Cryptography comparison for exams

| Method | Purpose | Key required? | Reversible? |
|---|---|---|---|
| Encryption | Confidentiality | Yes | Yes, with authorised decryption key |
| Cryptographic hash | Digest/fingerprint | Not for a basic hash | No |
| HMAC | Message integrity and shared-key authentication | Shared secret | Tag verification, not decryption |
| Digital signature | Publicly verifiable integrity and signer proof | Private signing key; public verifying key | Verification, not decryption |
| Compression | Reduce data size | Usually no cryptographic key | Lossless exactly; lossy approximately |

## 13. An Integrated Example - A Student Logs Into a College Portal

### 13.1 Hinglish full story

Tum college portal open karte ho aur username/password enter karte ho. Browser login request create karta hai (application). Request data ko text/JSON format mein encode kiya jaata hai; HTTPS ke case mein TLS protect karta hai (presentation-like functions). App login process ke liye session token/cookie create kar sakti hai (session-like control). TCP connection use ho to handshake, ports, ACK and windows handle hote hain (Transport). IP routers next hop choose karke request server tak pahunchate hain (Network).

Server ka response return aata hai. Agar network packets drop ho jaayein to TCP lost stream bytes retransmit kar sakta hai. Agar receiver buffer busy hai to advertised window shrink ho sakta hai. Agar portal backend kisi marks service ko call karta hai, wo **RPC-style distributed call** ho sakti hai. Agar result JSON compress hota hai to bandwidth kam lag sakta hai. Important: ye conceptual layered mapping hai, rigid one-layer-one-software rule nahi.

### 13.2 English description and timeline

```text
USER / BROWSER                   NETWORK                   PORTAL SERVER
      |                                                            |
      |-- Resolve / select destination and open secure channel --->|
      |<----------------- TCP SYN+ACK (if TCP) ---------------------|
      |-- TCP ACK / TLS handshake (simplified ordering) ----------->|
      |<------------- authenticated TLS establishment ------------>|
      |-- protected HTTP request (login) ------------------------->|
      |                   IP packets across routers                |
      |<---------------- protected HTTP response -----------------|
      |                                                            |
      |  Browser and server manage application login/session state |
```

**Exam point 1:** Browser-to-server transport ports and TCP byte sequence numbers are not the same as application user IDs or login session IDs.

**Exam point 2:** TCP acknowledgement confirms transport byte receipt, not successful login or database commit.

**Exam point 3:** A browser cookie is an application/session mechanism; it is not the TCP receive window.

**Exam point 4:** TLS provides authenticated encryption for HTTPS traffic; it is not itself TCP's retransmission system.

## 14. Key Diagrams and Comparisons for Last-Minute Revision

### 14.1 Concept map of all prescribed Unit IV topics

```text
                            NBCA-501 UNIT IV
                                  |
         +------------------------+-------------------------+
         |                        |                         |
     TRANSPORT                  SESSION                PRESENTATION
         |                        |                         |
  Design issues, TCP       Session management        Data representation
  connection management    Dialogue control          Compression
  sequence + ACK           Synchronisation           Cryptography
  flow control & rwnd      Checkpoints/recovery      Encoding/decoding
  congestion & cwnd        Remote Procedure Call     Security services
         |
      TCP WINDOW MANAGEMENT
      sliding / zero window / persist / scale / BDP
```

### 14.2 Useful 10-mark comparisons

| Comparison | Key difference |
|---|---|
| TCP vs UDP | Reliable ordered byte stream vs connectionless datagrams |
| SYN vs FIN | Open connection / synchronise sequence vs close one send direction |
| FIN vs RST | Graceful direction close vs abrupt reset |
| Flow vs congestion control | Receiver capacity (`rwnd`) vs path congestion (`cwnd`) |
| Transport vs session | Process-to-process delivery vs logical dialogue control |
| RPC vs local call | Network and partial failure vs in-process operation |
| Presentation vs transport | Data format and protection vs delivery service |
| Lossless vs lossy | Exact reconstruction vs approximate reconstruction |
| Symmetric vs asymmetric | Shared secret vs public/private pair |
| Encryption vs hashing | Reversible secrecy vs one-way digest |

### 14.3 Three must-practise diagrams

**Diagram A: TCP three-way handshake** - SYN, SYN+ACK, ACK with sequence values and state labels. **Diagram B: Sliding window** - acknowledged data, outstanding bytes, allowed unsent bytes, forbidden right-side data. **Diagram C: RPC architecture** - client, client stub, network, server stub, server function, reply. Also practise the TCP segment header and cryptosystem block diagram.

\newpage

# PART B - IMPORTANT 20-MARK QUESTIONS WITH COMPLETE MODEL ANSWERS

> **How to write a 20-mark answer:** Start with a clear definition, include a neat labelled diagram, explain working in ordered points, add a relatable technical example, give comparison/advantages/limitations and conclude in 2-3 lines. Length depends on your examiner and handwriting; these are **full model answers to learn and adapt**, not guarantees of exact university marking schemes.


## Q1 (20 Marks). Define the Transport Layer. Explain its services and major design issues in detail with a diagram.

**Introduction.** The Transport Layer is the fourth layer of the OSI reference model. It supports end-to-end communication between processes located on different hosts. Whereas the Network Layer helps packets travel across interconnected networks, the Transport Layer supplies the service needed by applications, including identification of application endpoints and, in suitable protocols, reliable ordered data transfer. TCP and UDP are two important Internet transport protocols.

```text
 Application A                     Application B
       |                                  ^
       v                                  |
 +---------------+                  +---------------+
 | Transport TCP | <--- logical ---> | Transport TCP |
 | ports, ACKs,  |     transport     | reassembly,   |
 | windows       |      service      | flow control  |
 +---------------+                  +---------------+
       |                                  ^
       v                                  |
         NETWORK / ROUTERS / LINKS
```

**1. Service-point addressing.** The destination IP address takes a packet toward the destination host or interface, while the transport destination port helps deliver it to the correct application service. Source and destination ports together with host addresses distinguish connections and endpoints.

**2. Segmentation and reassembly.** Large application data may be segmented into manageable transport units. The receiving system reassembles them. In TCP the application receives a byte stream rather than preserved send-message boundaries.

**3. Multiplexing and demultiplexing.** Multiple applications can share a host's network connectivity. The sending transport layer multiplexes their traffic, and the receiving layer directs incoming information to appropriate sockets.

**4. Connection management.** A connection-oriented protocol such as TCP establishes state before normal byte-stream communication and releases it when finished. Opening normally uses SYN, SYN+ACK and ACK. Closing can use FIN/ACK exchange. A connectionless protocol such as UDP does not establish a transport connection.

**5. Error detection and reliability.** Transport checksums help identify corrupted segments. TCP adds byte sequence numbers, acknowledgements, retransmission timers and loss recovery to provide reliable ordered delivery while the connection is active.

**6. Flow control.** The sender must not overwhelm the receiver. TCP uses a receiver-advertised window describing the capacity currently offered for reception.

**7. Congestion control.** Traffic through a limited-capacity network must be regulated. TCP uses congestion control, including a congestion window and algorithms that react to feedback about network conditions.

**8. Timer and buffer management.** A transport protocol manages retransmission timeout, buffered data, out-of-order arrivals and delay/throughput trade-offs. Timers are important because a missing acknowledgement does not immediately prove that the peer has failed.

**9. Crash recovery and duplicate handling.** A network may drop, reorder or duplicate segments. Endpoint processes can also terminate unexpectedly. The protocol must make reasonable decisions about stale state, retransmissions and connection cleanup.

**Connection-oriented versus connectionless.** TCP is connection-oriented, reliable, ordered, full-duplex and byte-stream based; UDP is connectionless and datagram based, with minimal built-in delivery guarantees. Applications choose them according to their requirements.

**Example.** A student downloading a PDF prefers intact, ordered data. TCP handles losses and reordering. A real-time media application may use UDP with application-specific mechanisms to control delay, jitter or selective recovery.

**Conclusion.** Transport design solves the important gap between forwarding packets across a network and offering useful end-to-end communication to applications. Its core issues are addressing, multiplexing, service selection, reliability, connection management and efficient window control.

## Q2 (20 Marks). Explain TCP, its characteristics and TCP segment header. Compare TCP and UDP.

**Introduction.** TCP (Transmission Control Protocol) is a connection-oriented transport protocol that provides a reliable, ordered byte-stream service. It is one of the core protocols of the Internet. TCP hides many packet-loss and reordering details from applications by using sequence numbers, acknowledgements, retransmission and buffering. Unlike a fixed physical circuit, a TCP connection does not reserve a dedicated communication line.

**Main TCP features:** connection setup before communication, end-to-end byte-stream delivery, full duplex operation, cumulative ACKs, sequence numbers for bytes, error checking, retransmission, receiver-side flow control, sender-side congestion control and connection release. TCP may carry application data in differently sized segments according to network and implementation conditions.

```text
             TCP HEADER - MINIMUM 20 BYTES
 +---------------------+----------------------+
 | Source Port         | Destination Port     |
 +---------------------+----------------------+
 |          Sequence Number                   |
 +--------------------------------------------+
 |          Acknowledgement Number            |
 +--------------------------------------------+
 | Offset / Flags      | Window Size          |
 +---------------------+----------------------+
 | Checksum            | Urgent Pointer       |
 +---------------------+----------------------+
 | Options + Padding (optional)               |
 +--------------------------------------------+
 | Data / Payload                             |
 +--------------------------------------------+
```

**Source port and destination port (16 bits each).** These identify communicating application ports. A server can accept many distinct client connections on the same listening port because remote endpoints differ.

**Sequence number (32 bits).** In an ordinary data segment, this identifies the first data byte. It allows duplicate detection and ordering. SYN and FIN each consume one sequence-number position.

**Acknowledgement number (32 bits).** When the ACK flag is set, it represents the next byte expected from the other endpoint. An ACK of 1200 says preceding bytes through 1199 have been received in the cumulative in-order sense.

**Data offset and flags.** Data offset specifies TCP header length in 32-bit words; the minimum is five words, or 20 bytes. Flags such as SYN, ACK, FIN, RST, PSH and URG support connection control and transport semantics.

**Window size (16 bits).** The receiver-advertised window supports flow control. A negotiated Window Scale option permits larger effective windows than the basic field alone.

**Checksum and urgent pointer.** Checksum detects many accidental data errors. Urgent pointer is meaningful with URG and is rarely used for modern application protocols.

**Options and data.** Options can advertise MSS, scaling or selective-ACK support. Data carries application-stream bytes. A segment's header can exceed 20 bytes when options are present.

**TCP versus UDP:** TCP provides a reliable ordered stream, connection state and flow/congestion-control mechanisms. UDP provides datagrams, ports and an integrity checksum but lacks TCP's built-in retransmission and delivery guarantee. UDP's header is 8 bytes, whereas TCP's minimum is 20 bytes. TCP often supports applications such as ordinary HTTP over TCP and file transfer; UDP is common in DNS and certain real-time applications, often with higher-layer functionality added.

**Example.** If 300 bytes start at sequence 900, then the next byte expected after successful receipt is 1200. The receiver sends ACK=1200. If a segment is lost, TCP can retransmit the missing bytes.

**Conclusion.** TCP's header fields work together to supply ordered process-to-process communication. Its reliability and control mechanisms make it useful for applications requiring exact byte delivery; UDP remains valuable when the application needs different trade-offs.

## Q3 (20 Marks). Explain TCP connection establishment and termination with suitable sequence diagrams and states.

**Introduction.** Connection management is a major Transport Layer design issue. The two endpoints must agree on initial sequence numbers, establish communication state and eventually release resources. TCP uses a three-way handshake for ordinary connection establishment and FIN/ACK or RST mechanisms for termination.

**Three-way handshake diagram:**

```text
CLIENT                                                  SERVER
  |                                                       |
  |---- SYN seq=100 ------------------------------------->|
  |<--- SYN+ACK seq=900, ack=101 --------------------------|
  |---- ACK ack=901 ------------------------------------->|
  |                                                       |
  +--------------- ESTABLISHED CONNECTION ----------------+
```

**Step 1:** A client performs an active open and sends a SYN segment with an initial sequence number (ISN), here 100. The client enters SYN-SENT.

**Step 2:** The server, which is listening, receives the SYN and replies with SYN+ACK. Its own ISN is 900, and ACK=101 acknowledges the client's SYN. The server enters SYN-RECEIVED.

**Step 3:** The client acknowledges the server SYN with ACK=901 and enters ESTABLISHED. The server enters ESTABLISHED on receiving the final expected acknowledgement. SYN consumes one position in the sequence-number space, which explains the `+1`.

**Need for the handshake.** It coordinates initial state, gives each side the other's initial sequence number and allows TCP options to be negotiated. It also protects against some confusion caused by old duplicated segments. It cannot guarantee that connectivity will remain available forever.

**Data transfer stage.** After establishment, both participants can send and receive bytes. Their two directions have independent byte-numbering and acknowledgement information. Acknowledgements and retransmissions maintain a reliable stream; flow control and congestion control limit outstanding bytes.

**Graceful connection release diagram:**

```text
CLIENT                                               SERVER
  |---- FIN seq=x -------------------------------------->|
  |<--- ACK ack=x+1 -------------------------------------|
  |         (server may still send remaining data)      |
  |<--- FIN seq=y ---------------------------------------|
  |---- ACK ack=y+1 ------------------------------------>|
  |    TIME-WAIT -> CLOSED             LAST-ACK -> CLOSED|
```

**Release explanation.** FIN indicates the sender has no more data in that direction. The receiver acknowledges the FIN. When its own outgoing stream is finished, it sends its FIN, and the other side acknowledges. A FIN consumes a sequence number. The active closer may pass through FIN-WAIT-1, FIN-WAIT-2 and TIME-WAIT; the passive closer often passes through CLOSE-WAIT and LAST-ACK. FIN and ACK messages may sometimes be combined, so the four-message diagram is a typical, not mandatory, packet count.

**TIME-WAIT.** This state supports proper handling of delayed duplicates and retransmitted closing messages. The textbook concept refers to waiting for a period related to maximum segment lifetime.

**Abnormal termination.** RST may be used to reset a connection abruptly. Endpoint crashes, cable failure or network outage may also cause interruption. TCP uses timers and failure handling rather than always discovering a failed peer immediately.

**Example:** Client SYN=4500, server SYN=9000. Server SYN+ACK carries ACK=4501 and client final ACK=9001. These are the next sequence numbers after the control SYNs.

**Conclusion.** TCP connection management uses coordinated exchange of control segments to establish two reliable byte streams and release them. Correct sequence numbering and state transitions are essential to understand handshake and termination questions.

## Q4 (20 Marks). Explain TCP reliability, sequence numbers, cumulative acknowledgements, retransmissions and SACK.

**Introduction.** IP may lose, reorder or duplicate datagrams. TCP builds a reliable, ordered byte-stream service on top of this best-effort network. Its major mechanisms are sequence numbers, cumulative acknowledgements, timers, retransmission, buffering, checksums and optional selective acknowledgements.

**Sequence numbers:** TCP numbers stream **bytes**, not entire packets. If the sender transmits 400 bytes beginning at sequence 5000, the bytes occupy 5000-5399, and the next byte is 5400. Sequence information enables ordering and duplicate recognition.

**Cumulative acknowledgements:** The receiver typically acknowledges the highest continuous sequence of received bytes by naming the next missing/expected byte. ACK=5400 means all bytes before 5400 in that stream sequence have been received. ACK does not assert that a remote application has completed a business transaction.

**Error detection:** TCP's checksum detects many accidental changes during transmission. Corrupted segments normally cannot be accepted as valid stream data. The checksum alone cannot prevent drops or reconstruct missing data.

**Timeout and retransmission:** The sender estimates round-trip time and uses retransmission timeout (RTO) to decide when an unacknowledged segment might have been lost. Upon expiry, it can retransmit missing data. Appropriate timer adaptation is essential because actual network latency varies.

```text
SENDER                                    RECEIVER
SEQ=1000 len=200 --------------------------> ACK=1200
SEQ=1200 len=200 -------X   (lost)
SEQ=1400 len=200 --------------------------> ACK=1200
SEQ=1600 len=200 --------------------------> ACK=1200
SEQ=1800 len=200 --------------------------> ACK=1200
RETRANSMIT seq=1200 -----------------------> ACK=2000
```

**Duplicate ACK and fast retransmit:** Out-of-order segment arrivals can produce repeated ACK values. In classic textbook TCP, three duplicate ACKs are a strong sign of a missing segment and may trigger fast retransmission without waiting for the full timeout. Current TCP implementations can use additional techniques and different loss-detection rules.

**SACK:** Selective Acknowledgement enables a receiver to report blocks of data that arrived beyond a missing gap. For example, ACK=1200 plus SACK information for bytes 1400-1999 helps the sender identify which part requires retransmission.

**Buffering and ordered stream delivery:** TCP may temporarily buffer out-of-order bytes. The receiver's application normally sees bytes in sequence. If the missing segment arrives, previously buffered later bytes can now be delivered in the appropriate order.

**ACK of duplicate or lost data:** Retransmitted bytes may arrive after the original was actually received but its acknowledgement was lost. Sequence numbering prevents the receiver from handing the same byte twice to the application stream.

**Limits of TCP reliability:** TCP cannot force delivery through an indefinitely broken network. It may eventually time out. It also does not encrypt traffic or promise that the server application has used or committed every byte.

**Conclusion.** Reliable delivery is achieved by combining sequence numbers, ACKs, buffers, retransmissions and error checking. A full answer should describe a lost-segment timeline, clarify cumulative ACK behaviour and distinguish TCP from textbook Go-Back-N or Selective Repeat.

## Q5 (20 Marks). What is TCP window management? Explain sliding window, flow control, congestion window, zero window and numericals.

**Introduction.** TCP window management determines how much data can be sent without waiting for acknowledgements. It supports pipelining, protects receivers against buffer overflow and works with congestion control to regulate network load. A TCP window describes byte positions in the ordered stream.

**Sliding-window concept.** The sender has already acknowledged data, sent-but-not-yet-acknowledged data, and additional bytes currently permitted for sending. As the receiver acknowledges bytes, the left edge moves forward, permitting more data to be transmitted. This avoids the inefficiency of Stop-and-Wait transmission.

```text
  ACKED BYTES        SEND WINDOW                     FUTURE BYTES
 [ 0 ... 999 ]  [ 1000 ............ 2999 ]       [ 3000 ... ]
                    |                         |
           already sent and/or        right window edge
           permitted for sending

After ACK rises by 500 with same rwnd:
 [ 0 ...1499 ]  [ 1500 ............ 3499 ]       [ 3500 ... ]
```

**Receiver-advertised window (`rwnd`).** The receiver has finite buffer capacity. It advertises the amount of data it is prepared to accept. A sender must respect the receiver's permitted byte range. If the application reads buffered data, more space can become available and a larger window may be announced.

**Congestion window (`cwnd`).** The network path also has limited capacity. TCP maintains a local congestion-control limit that depends on congestion feedback and the sender algorithm. In the simplified classroom model, the permissible outstanding flight size is bounded by `min(rwnd,cwnd)`.

**Numerical example:** Suppose `rwnd=9000 bytes`, `cwnd=6000 bytes` and `3500 bytes` are already in flight. The approximate maximum outstanding flight size is `min(9000,6000)=6000`. New data allowance is `6000-3500=2500 bytes`, assuming data is available and other constraints do not bind.

**Zero window:** When the receiving buffer is full, the receiver may advertise `rwnd=0`. The sender pauses normal new data transmission. If the receiver later frees space but the update is lost, the sender can use the TCP persist mechanism and zero-window probes to prevent indefinite deadlock.

**Silly window syndrome:** Sending repeated tiny segments or advertising tiny increments of free buffer space can cause poor efficiency. Clark's receiver-side avoidance and Nagle's sender-side small-packet coalescing are textbook methods, though interactive applications may choose different latency trade-offs.

**Window scaling:** The TCP Window Size field is 16 bits, with a basic maximum of 65,535. The Window Scale option is negotiated during setup and enables much larger effective advertised windows for high-bandwidth, high-latency paths.

**Bandwidth-delay product:** `BDP = bandwidth x RTT`. For 20 Mbps and 0.1 s, BDP = 2 million bits = 250,000 bytes. An adequately large sending flight limit is important to fill an idealised link.

**Flow control versus congestion control:** Flow control responds to receiving host capacity; congestion control responds to network conditions. `rwnd` is the receiver's advertised limit; `cwnd` is sender-local. Both influence effective data transmission.

**Conclusion.** TCP window management improves throughput by allowing pipelined data, prevents receiver overload through the advertised window and coordinates with network congestion limits. Diagrams and byte-based numericals are essential for a high-quality exam answer.

## Q6 (20 Marks). Explain the Session Layer, its design issues, dialogue control, synchronisation and checkpoints.

**Introduction.** The Session Layer is Layer 5 of the OSI reference model. Its primary function is to describe the establishment, coordination and termination of logical communication sessions between applications. The layer is concerned with organising dialogue and recovery rather than routing IP packets or retransmitting TCP bytes.

**Session establishment:** Two cooperating programs set up a logical conversation. The system may negotiate options, assign session identifiers or establish application context. An example is an online examination system creating a stateful assessment session for a student.

**Session maintenance:** During a long interaction, the system may remember which step the participants have reached, which resources are held and what actions remain outstanding. State must be maintained correctly despite concurrent requests or interruptions.

**Dialogue control:** Communication can be **full duplex**, in which both sides exchange data at the same time, or **half duplex**, in which participants take turns. Session-style dialogue management can coordinate turn taking, requests and replies or access to shared resources.

**Token management:** Conceptually, a token can act as permission to perform a particular session operation. If both parties tried to perform a mutually exclusive action simultaneously, a token-passing rule could coordinate control. Token control is a design concept rather than a mandatory facility of ordinary TCP connections.

**Synchronisation and checkpointing:** A session can place checkpoints in a long process. Suppose a file-like operation requires eight stages. If checkpoint 6 has been durably recorded and the connection fails during stage 7, a recovery-capable system might resume after stage 6 rather than repeating all eight stages.

```text
START -> STEP 1 -> STEP 2 -> [CHECKPOINT A]
               -> STEP 3 -> STEP 4 -> [CHECKPOINT B]
               -> STEP 5 ---X network interruption
                  |
                reconnect -> resume after B (if supported)
```

**Session termination:** Upon completion, the dialogue must be closed and any session resources released. Unexpected failures may require expiry, timeout and cleanup rather than orderly release.

**Session recovery:** Interruption can result from network loss or process crashes. Recovery requires sufficient saved state and agreement about what operations can be retried safely. A mere TCP reconnection does not automatically reconstruct an application's lost state.

**Session versus Transport:** The Transport Layer focuses on communication between processes, ports, byte order, acknowledgements and windows. The Session Layer focuses on logical interaction, conversation control and checkpoints. Real TCP/IP software frequently implements session features in the application or middleware rather than a separate OSI Layer 5 module.

**Advantages:** better organisation of long interactions, recoverability where checkpoints exist, structured dialogue and systematic resource handling. **Limitations:** additional state management, complexity, storage for checkpoints, and potential ambiguity after partial failures.

**Conclusion.** Session design issues are establishment, maintenance, dialogue control, synchronisation, token control, exception recovery and termination. A complete answer should illustrate checkpointing and clearly explain how session-level recovery differs from TCP reliability.

## Q7 (20 Marks). Explain Remote Procedure Call (RPC) architecture, steps, marshalling, advantages and failures.

**Introduction.** Remote Procedure Call is a distributed communication technique by which a client invokes a procedure supplied by another process or machine through a function-like interface. RPC reduces the amount of explicit networking code an application must write, but it does not remove network delay, partial failure or the need to handle errors.

```text
CLIENT                     NETWORK                        SERVER
+-----------+                                         +-----------+
| Client fn | -> Client Stub -> request message ---> | Server    |
| call      |                                         | Stub      |
+-----------+                                         +-----+-----+
                                                           |
                                                     Server procedure
                                                           |
       return value <- Client Stub <- reply message <------+
```

**Client and server stubs:** A client stub/proxy makes a remote operation appear locally callable. The server stub/skeleton decodes the request and dispatches it to the actual server function. Frameworks may generate stubs from an interface definition, which describes operation names and parameter types.

**Marshalling:** The client converts arguments into a transmissible format. **Unmarshalling** converts the request into appropriate values on the remote machine. The reverse happens when the server creates a reply.

**Complete working steps:** (1) client calls proxy, (2) client stub marshals arguments, (3) runtime transmits request, (4) server runtime receives request, (5) server stub decodes and invokes procedure, (6) procedure executes, (7) response is encoded and transmitted, (8) client decodes result or error. Request IDs, deadlines, authentication metadata and retry policies may be part of the surrounding framework.

**Binding:** Before calling, the client must know which service endpoint implements the remote operation. Configuration, DNS or service discovery can supply endpoint information.

**Failure handling:** A local function call usually either returns or throws a local runtime exception. An RPC can time out even if the remote server performed the operation successfully, because the reply was lost. The client must consider uncertainty rather than assuming timeout means no server action occurred.

**Call semantics:** **At-least-once** attempts may retry and risk duplicate execution; **at-most-once** designs avoid duplicates where their duplicate tracking and other assumptions hold but may fail without execution; exactly-once durable effects require stronger application/transaction mechanisms and are difficult to guarantee end to end under arbitrary failures.

**Idempotency:** For a repeated request, an idempotent operation preserves its intended final state. "Set status = ACTIVE" can be idempotent. "Add ₹500 to balance" is not by default. Unique request IDs and durable deduplication can help protect financial or other sensitive transactions.

**Advantages:** easy interface for distributed services, modular design, reuse, cross-language communication and encapsulation of serialisation. **Disadvantages:** higher latency than local calls, network failures, version incompatibility, security issues and potential duplicate side effects.

**Example:** A student portal frontend calls a remote marks service using `getMarks(studentId)`. The client stub sends the ID, the server queries records and returns the result. The network transfer is not a normal in-memory function invocation.

**Conclusion.** RPC offers a procedure-style abstraction over network communication. For a full exam answer, draw stubs and arrows, explain marshalling and list timeout, retry and idempotency challenges.

## Q8 (20 Marks). Explain Presentation Layer design issues: translation, representation, compression and cryptography.

**Introduction.** The Presentation Layer is Layer 6 in the OSI reference model. It specifies how communicated information can be represented, encoded, transformed and protected so that systems with different internal formats understand the same data. Its traditional functions include translation, serialisation, compression and encryption/decryption.

```text
SENDER                                                     RECEIVER
Application data                                         Application data
      |                                                         ^
Encode / translate                                       Decode / parse
      |                                                         ^
  Compress                                               Decompress
      |                                                         ^
  Encrypt ------------------ network ------------------>   Decrypt
```

**1. Translation and standard representation.** Computers use different internal formats for characters, numbers and data structures. A standard format removes ambiguity. For example, UTF-8 identifies text encoding and a serialisation format such as JSON identifies fields and values. Systems must agree on types, names and meaning.

**2. Data serialisation.** Objects or records are converted into strings or binary messages. A sender may encode a record containing student ID, name and marks. The receiver decodes the record into its own program's data structures. A direct memory pointer from one process normally has no useful meaning on another host.

**3. Byte order and data types.** Big-endian and little-endian machines may represent multi-byte values differently in memory. A communication format specifies a canonical representation so that the recipient recovers the correct numeric values.

**4. Compression.** The layer conceptually removes redundant data to reduce transfer size. Lossless methods such as RLE, Huffman and LZ-based dictionary coding restore the exact original data. Lossy methods such as typical JPEG or media codecs intentionally sacrifice some information to reduce size further.

**5. Cryptography.** Encryption changes plaintext into ciphertext to protect confidentiality. In modern systems cryptographic integrity and authentication mechanisms are also important. Symmetric cryptography uses shared secret keys, whereas public-key methods use key pairs and can support encryption, signatures or key establishment according to the algorithm.

**6. Encoding negotiation and compatibility.** Both endpoints must agree on the formats, compression algorithms and security methods. Unsupported types or version mismatches may make a message undecodable even though the transport delivered its bytes correctly.

**7. Efficiency and security trade-offs.** Compression consumes CPU and may not help already compressed data. Encryption requires correct key management, authenticated algorithms and safe handling of nonces. Encoding adds overhead but improves interoperability.

**Practical example:** A mobile app serialises an order as JSON, might compress the response, and uses HTTPS to protect traffic. The server parses the request and reconstructs values. These are presentation-like functions; in practice libraries and application/TLS layers implement them rather than a separate mandatory Layer 6 protocol.

**Difference from other layers:** Transport controls ports, sequencing, retransmission and windows; Presentation concerns the meaning and representation of bytes. Session concerns logical dialogue state and recovery checkpoints.

**Conclusion.** Presentation design issues ensure data is correctly interpreted, efficiently represented and appropriately protected across different computers. The exam answer should show both sender and receiver transformations and explain why format compatibility is essential.

## Q9 (20 Marks). Define data compression. Explain lossless/lossy compression, RLE, Huffman coding, LZ methods and numerical examples.

**Introduction.** Data compression is the representation of information using fewer bits than its original form. It reduces transmission and storage requirements by eliminating redundancy or discarding selected information when some quality loss is acceptable.

**Main classification:** Lossless compression allows exact recovery of the original data. It is used for text, executable programs, databases and files where each bit matters. Lossy compression reconstructs an approximation and is frequently used for photos, sound and video where reduced fidelity may be acceptable.

**Run Length Encoding (RLE).** Consecutive repetitions are stored as symbol-count pairs. For the text `AAAAABBBCCCCDD`, RLE can create `5A3B4C2D`. The decoder expands `5A` to five As and so forth. Using one ASCII byte per original/encoded character in this illustrative notation, the input is 14 bytes and the output 8 bytes. Percentage saving is `(14-8)/14 * 100 = 42.86%`. Actual encoding formats may use extra control information. RLE is simple but poor for irregular data.

**Huffman coding.** Assign variable-length, prefix-free binary codes according to symbol frequency. More frequent symbols generally receive shorter codes. The algorithm repeatedly combines the two least frequent nodes into a weighted tree. Example frequencies A=5, B=2, C=1, D=1 yield a possible code scheme A=0, B=10, C=110, D=111.

```text
           [9]
          /   \
        A:5   [4]
             /   \
           B:2   [2]
                /   \
              C:1   D:1
```

Code bits = `5*1+2*2+1*3+1*3 = 15`, compared with 18 bits for 9 symbols using a fixed 2-bit code. The code tree or equivalent code descriptions need to be known to the receiver, so storage metadata matters.

**Dictionary compression: LZ77/LZW.** LZ-style methods exploit earlier repeated strings. In LZ77, the compressor can encode a match by reference to its previous location and length. LZW uses dictionary entries. DEFLATE, used in many ZIP-like contexts, combines LZ77 matching and Huffman coding.

**Lossy method.** Image/audio compressors may use transforms and quantisation to remove information that contributes less to perceived quality. The compressed result can be much smaller, but exact reconstruction is impossible after irreversible quantisation.

**Compression ratio:**

$$
 C=\frac{\text{original size}}{\text{compressed size}}.
$$

**Reduction percentage:**

$$
 R=\frac{\text{original size}-\text{compressed size}}{\text{original size}}\times100.
$$

For an original file of 800 KB compressed to 200 KB: ratio = **4:1** and reduction = **75%**.

**Advantages:** less storage, possible faster transmission, lower bandwidth charges. **Disadvantages:** processing overhead, decompression requirement, possibility of expansion for some data, and quality loss in lossy methods.

**Conclusion.** Compression is a useful Presentation Layer design topic because it changes the external form of data while retaining enough information for the intended application. Explain both types, show an RLE/Huffman example and solve a size numerical.

## Q10 (20 Marks). Define cryptography. Explain symmetric and asymmetric encryption, hashing, digital signatures and secure communication with diagrams.

**Introduction.** Cryptography consists of mathematical methods used to protect information and communications. Its primary objectives can include confidentiality, integrity, authentication and secure key establishment. A cryptographic system transforms plaintext into ciphertext using an encryption algorithm and an appropriate key, and an authorised receiver performs decryption.

```text
PLAINTEXT -> [ENCRYPT with appropriate key] -> CIPHERTEXT
CIPHERTEXT -> [DECRYPT with matching key] -> PLAINTEXT
```

**Symmetric-key cryptography.** A shared secret key is available to the communicating participants. Sender encrypts with that key and receiver decrypts with the shared secret using a suitable scheme. AES is a widely used modern cipher, and an authenticated encryption mode such as AES-GCM provides both confidentiality and integrity protection when implemented correctly. Symmetric encryption is efficient for large quantities of data. The major difficulty is securely establishing and storing the shared key.

**Asymmetric/public-key cryptography.** Each participant holds a public/private key pair for an appropriate cryptographic scheme. In public-key encryption, a sender uses the receiver's authenticated public key and the receiver uses the corresponding private key to decrypt. Public-key methods are also used for digital signatures and key agreement, though each operation uses its own mathematically suitable scheme. Public-key operations generally require more computation than efficient symmetric bulk encryption.

**Comparison:** Symmetric cryptography uses a shared secret and is fast for large data. Asymmetric systems use public/private keys and help establish secure communication without initially sharing the same secret through a private channel. In practice, **hybrid cryptography** combines public-key mechanisms for authenticating/establishing secrets with symmetric encryption for actual large-scale data transfer.

**Hashing.** A cryptographic hash produces a fixed-length message digest from input data. SHA-256 is a common example. Good hash functions are designed to resist preimage and collision attacks. Hashing is not reversible encryption. An ordinary unkeyed digest cannot by itself prove who created a message.

**Message authentication code (MAC).** A secret-key MAC such as HMAC provides integrity and shared-key authentication for a message. The receiver recomputes or verifies a tag using the common key. It does not offer the same public verification properties as a digital signature.

**Digital signature.** A signer creates a signature using a private signing key. A verifier uses the matching public key to check it. A valid signature can provide evidence of the signer's key and integrity of signed contents, assuming suitable key authenticity and algorithm security. A signature is not an encryption method for keeping text secret.

**Caesar cipher educational example:** For shift key 3, CAT becomes FDW. The method illustrates how a key affects transformation, but is insecure and not suitable for real communications.

**HTTPS/TLS example:** A browser validates the server's identity using certificates and establishes keys through TLS. Authenticated encryption then protects application data against many forms of passive observation and modification. TLS is a practical protocol implementing presentation/security functions and is not the same as TCP's loss recovery.

**Advantages:** confidentiality, better integrity protection, authentication, secure transactions. **Limitations:** key management complexity, CPU cost, incorrect algorithm use, weak credentials and implementation vulnerabilities.

**Conclusion.** Encryption, hashing, MACs and digital signatures solve different security problems. A 20-mark answer should define the goals, compare key approaches, draw a cryptosystem diagram and explain where secure Internet protocols use them.

\newpage

# PART C - EXAM PRACTICE, SHORT ANSWERS AND REVISION


## 15. Forty Important 2-Mark Questions with Answers

**Q1. Define Transport Layer.**  
**Answer:** Transport Layer is OSI Layer 4. It provides process-to-process logical communication services between end hosts. Depending on the protocol, it can provide segmentation, connection management, reliability, flow control and multiplexing.

**Q2. What is a port number?**  
**Answer:** A port number is a transport-layer identifier used to identify an application service/socket endpoint on a host. TCP and UDP each have 16-bit port fields; ports are interpreted in combination with IP addresses and protocol information.

**Q3. What is process-to-process delivery?**  
**Answer:** It means delivering network data to the suitable application process on the destination host, not merely reaching that host. Ports and socket identification support this function.

**Q4. Define multiplexing.**  
**Answer:** Multiplexing is the collection and transmission of data originating from multiple application endpoints through a common transport/network stack. Demultiplexing is the corresponding delivery to the proper receiving endpoints.

**Q5. What is a TCP connection?**  
**Answer:** It is a logical, stateful association between two TCP endpoints offering two independent directions of reliable, ordered byte-stream communication. It does not reserve a physical circuit.

**Q6. What is UDP?**  
**Answer:** UDP is a connectionless transport protocol that transfers datagrams between ports. It does not itself provide TCP-style reliable delivery, sequencing, flow control or congestion control.

**Q7. What is the minimum TCP header size?**  
**Answer:** The minimum TCP header is **20 bytes**. TCP options can increase it, with the header length encoded by the Data Offset field in units of four bytes.

**Q8. What is the size of a TCP sequence number?**  
**Answer:** The TCP sequence-number field is **32 bits** and identifies byte positions within the sender's TCP stream. SYN and FIN consume one sequence-number position each.

**Q9. What is the meaning of ACK=3000?**  
**Answer:** ACK=3000 ordinarily means the receiver's next expected in-order TCP byte is 3000. It cumulatively acknowledges all earlier bytes in that stream sequence, subject to valid connection state.

**Q10. What is the use of SYN?**  
**Answer:** SYN initiates a TCP connection and participates in synchronising initial sequence numbers. It is exchanged during the three-way handshake.

**Q11. What is the use of FIN?**  
**Answer:** FIN indicates that a TCP endpoint has finished sending data in one direction. Its acknowledgement supports graceful half-close/termination behaviour.

**Q12. What is the use of RST?**  
**Answer:** RST is a TCP reset signal used to abort/refuse a connection or report certain invalid connection states. It is not the same as an orderly FIN close.

**Q13. What is the three-way handshake?**  
**Answer:** It is TCP's common connection-opening sequence of **SYN**, **SYN+ACK**, and **ACK**. It exchanges initial sequence numbers and establishes connection state.

**Q14. What is TIME-WAIT?**  
**Answer:** TIME-WAIT is a TCP state commonly entered by the active closer after normal termination. It helps handle delayed duplicate segments and retransmitted closing messages.

**Q15. Define sliding window.**  
**Answer:** A sliding window permits multiple bytes/segments to be transmitted before their acknowledgements arrive. The permitted sequence-number range moves forward as in-order data is acknowledged.

**Q16. What is a receiver window (`rwnd`)?**  
**Answer:** It is the receiver-advertised allowance indicating how much additional TCP data it can accept. It supports flow control and helps avoid receiver-buffer overflow.

**Q17. What is a congestion window (`cwnd`)?**  
**Answer:** It is a sender-maintained limit used by TCP congestion-control algorithms to regulate unacknowledged data based on perceived network capacity and congestion feedback.

**Q18. Distinguish flow and congestion control.**  
**Answer:** Flow control protects the **receiver's buffer** and uses the advertised receiver window. Congestion control protects the **network path** and uses sender-controlled limits and algorithms.

**Q19. What is a zero window?**  
**Answer:** It is a receiver-advertised TCP window of zero, indicating that no ordinary new byte data may currently be accepted beyond the allowed boundary. The sender pauses normal new data sending.

**Q20. What is a persist timer?**  
**Answer:** TCP's persist mechanism helps avoid indefinite waiting if a zero-window update is lost. The sender can make controlled probes to discover whether receiving space has become available.

**Q21. Define silly window syndrome.**  
**Answer:** It is inefficient transmission caused by tiny segments or small advertised-window increments. It increases header overhead and can reduce effective throughput.

**Q22. What is Window Scale in TCP?**  
**Answer:** It is a TCP option negotiated during setup that permits effective advertised receive windows larger than the basic 16-bit header field alone can represent.

**Q23. Define bandwidth-delay product.**  
**Answer:** It is bandwidth multiplied by round-trip time, estimating the amount of data that can be in flight over a path. Its unit is bits when bandwidth is bits/s and RTT is seconds.

**Q24. What is fast retransmit?**  
**Answer:** A classic TCP loss-recovery technique in which sufficient repeated ACK evidence, commonly three duplicate ACKs, prompts retransmission without waiting for the full retransmission timeout.

**Q25. What is SACK?**  
**Answer:** Selective Acknowledgement is an optional TCP facility by which receivers report blocks of out-of-order received bytes, helping senders recover losses more efficiently.

**Q26. Define Session Layer.**  
**Answer:** OSI Layer 5 conceptually handles establishment, coordination, synchronisation, recovery and termination of communication sessions between applications.

**Q27. What is dialogue control?**  
**Answer:** Dialogue control determines how participating applications take turns or simultaneously exchange information within a session. It may describe half-duplex or full-duplex communication.

**Q28. What is session checkpointing?**  
**Answer:** Checkpointing stores recoverable progress information during a long communication task so that, with suitable application support, interrupted work can resume from a known point.

**Q29. What is token management?**  
**Answer:** It is a conceptual session-layer mechanism in which permission to perform a particular coordinated action is assigned using a logical token.

**Q30. What is RPC?**  
**Answer:** Remote Procedure Call is a technique enabling a program to invoke a procedure in a remote process using a function-like interface, with underlying message exchange across a network.

**Q31. Define marshalling.**  
**Answer:** Marshalling converts arguments/results into a transferable representation. Unmarshalling reconstructs usable values from received representations.

**Q32. What is a client stub?**  
**Answer:** A client stub is the proxy component presenting a local callable interface for a remote procedure. It usually encodes call arguments and communicates with the RPC runtime.

**Q33. Define idempotent operation.**  
**Answer:** An operation is idempotent if executing it repeatedly has the same intended final effect as executing it once. For example, setting a record's status to a fixed value can be idempotent.

**Q34. Define Presentation Layer.**  
**Answer:** OSI Layer 6 concerns how communicated data is represented, translated, compressed and protected so that different systems can interpret it correctly.

**Q35. What is lossless compression?**  
**Answer:** A compression method from which the original data is reconstructed exactly. Typical examples are RLE, Huffman coding and LZ-based methods.

**Q36. What is lossy compression?**  
**Answer:** A method that intentionally removes some information and reconstructs an approximation. It is useful in many image, audio and video applications.

**Q37. What is Run Length Encoding?**  
**Answer:** RLE compresses repeated consecutive symbols by representing them as a run length and the symbol, such as `AAAA` -> `4A` in a simple notation.

**Q38. What is a Huffman code?**  
**Answer:** A variable-length, prefix-free lossless code constructed from symbol frequencies. More frequent symbols usually receive shorter binary codewords.

**Q39. What is the difference between encryption and hashing?**  
**Answer:** Encryption transforms data into a reversible protected form using appropriate keys. A cryptographic hash produces a fixed-size digest and is not intended to be decrypted.

**Q40. What is a digital signature?**  
**Answer:** A digital signature is created with a private signing key and verified with the corresponding public key, providing origin and integrity evidence when used and validated correctly.

## 16. Important 5-Mark Questions - Answer Outlines

1. **Explain multiplexing and demultiplexing.** Define transport ports; draw three applications to one network-stack diagram; describe identifying sockets and distributing traffic; use browser/email example.
2. **Differentiate TCP and UDP.** Define each; provide at least six points including connection, message boundaries, reliability, sequencing, flow control and header size.
3. **Explain TCP sequence and ACK numbers.** Define byte numbering; state that ACK is next expected byte; give a 500-byte solved trace; mention SYN/FIN.
4. **What is TIME-WAIT?** Explain active close, delayed FIN retransmission and stale packet expiry; draw the tail of a FIN exchange.
5. **Explain zero-window and persist.** Define receiver buffer full; lost window update problem; zero-window probe timeline; conclude why it prevents deadlock.
6. **Define Session Layer design issues.** Give establishment, dialogue, checkpoints, token control, recovery and release with examples.
7. **Explain RPC marshalling.** Describe converting arguments into transferable format, client/server stubs and decoding returned values.
8. **Explain compression ratio.** Define original/compressed size, show formula and solve 500 KB -> 125 KB (ratio 4:1; reduction 75%).
9. **Explain RLE with example.** Define runs, code a repeated sequence, discuss advantage of repeated data and risk of expansion.
10. **Explain Huffman coding.** Describe prefix-free variable-length codes, construction tree and symbol frequency relationship.
11. **Differentiate symmetric/asymmetric encryption.** Key arrangement, performance, examples and a note on hybrid systems.
12. **Explain hashing and digital signatures.** Hash gives digest; signature verifies authorised signing and message integrity; avoid saying ordinary hash authenticates sender.

## 17. Important 10-Mark Questions - Suggested Answer Structure

**A. TCP Segment Header (10 marks):** 2 marks introduction, 3 marks neat diagram, 4 marks explain fields and flags, 1 mark real-life/example/conclusion.

**B. TCP Three-Way Handshake (10 marks):** 2 marks definition/need, 3 marks labelled SYN/SYN+ACK/ACK diagram, 3 marks step-by-step working, 2 marks SYN sequence-number example and key cautions.

**C. Sliding Window and Flow Control (10 marks):** 2 marks definition, 3 marks send-window diagram, 2 marks receiver buffer/rwnd explanation, 2 marks numerical with `min(rwnd,cwnd)`, 1 mark conclusion.

**D. Session Layer (10 marks):** 2 marks definition, 5 marks design issues, 2 marks checkpoint diagram, 1 mark relation with TCP/IP implementation.

**E. RPC (10 marks):** 2 marks definition, 3 marks client/stub/server diagram, 3 marks marshalling and working, 2 marks benefits and network failure risk.

**F. Data Compression (10 marks):** 2 marks definition, 2 marks lossless/lossy table, 3 marks RLE/Huffman examples, 2 marks numerical, 1 mark pros and cons.

**G. Cryptography (10 marks):** 2 marks definition/objectives, 2 marks encryption diagram, 3 marks symmetric/asymmetric, 2 marks hash/MAC/signature differences, 1 mark conclusion.

## 18. Twenty Solved and Practice Numericals / Traces

**Numerical 1 - next TCP ACK.** SEQ=2100 and payload=650 bytes received in order. **Answer:** ACK=`2100+650=2750`.

**Numerical 2 - two TCP segments.** Starting SEQ=1000, send 400 bytes and next 600 bytes consecutively. **Answer:** first occupies 1000-1399, second starts 1400 and occupies 1400-1999; cumulative ACK after both = 2000.

**Numerical 3 - handshake ACK.** Client SYN seq=700, server SYN seq=2500. **Answer:** server ACK=701; client final ACK=2501.

**Numerical 4 - FIN number.** A FIN has sequence number 4900 with no payload. **Answer:** the next expected byte/sequence position after the FIN is 4901.

**Numerical 5 - basic sending allowance.** `rwnd=9000`, `cwnd=7000`, in-flight=5000. **Answer:** `min(9000,7000)-5000 = 2000 bytes` new allowed under simplified assumptions.

**Numerical 6 - receiver restriction dominates.** `rwnd=2500`, `cwnd=10000`, in-flight=1800. **Answer:** `2500-1800=700 bytes` approximately.

**Numerical 7 - no new allowance.** `rwnd=3000`, `cwnd=4000`, in-flight=3500. **Answer:** `max(0,3000-3500)=0`; receiver limit currently leaves no new space.

**Numerical 8 - sliding window bounds.** Base sequence=5000, allowed 3000 bytes. **Answer:** approximate range 5000 through 7999. With same window after ACK=6200, range 6200 through 9199.

**Numerical 9 - basic TCP window bound.** A 16-bit unsigned window value without scaling can represent at most **65,535**. **Answer:** `2^16 - 1 = 65535`.

**Numerical 10 - BDP.** Bandwidth 10 Mbps and RTT 80 ms. **Answer:** `10,000,000*0.08 = 800,000 bits = 100,000 bytes`.

**Numerical 11 - BDP.** Bandwidth 50 Mbps and RTT 40 ms. **Answer:** multiply 50,000,000 by 0.04 to get **2,000,000 bits**, or **250,000 bytes**.

**Numerical 12 - window / RTT throughput bound.** Teaching approximation: 64 KB (65,536 bytes) window with 0.1 s RTT. **Answer:** `65,536/0.1=655,360 bytes/s` = about 5.24 Mbps before overhead. This is a simplified window-limited upper-bound calculation.

**Numerical 13 - RLE.** Compress `AAAAAABBBBCC`. **Answer:** simplified `6A4B2C`. Raw character count 12, encoded character count 6, symbolic reduction 50%, if each character consumes equal storage and ignores metadata.

**Numerical 14 - RLE.** Compress `AAABBBBBCCCCCCCC`. **Answer:** `3A5B8C` using count-value notation. Initial characters=16, encoded characters=6; simple representation reduction = 62.5%, ignoring format overhead.

**Numerical 15 - compression ratio.** Original 900 KB, compressed 300 KB. **Answer:** `900/300 = 3:1`; reduction=`(900-300)/900*100=66.67%`.

**Numerical 16 - reduction.** Original 2 MB, compressed 1.5 MB. **Answer:** `(2-1.5)/2*100 = 25%`; ratio `2/1.5 = 1.33:1`.

**Numerical 17 - compressed size target.** Original 1200 KB, require 60% reduction. **Answer:** remain 40% = `1200*0.4=480 KB`.

**Numerical 18 - Huffman length.** Frequency A=5 with 1-bit code, B=2 with 2-bit code, C=1 and D=1 with 3-bit codes. **Answer:** `5+4+3+3=15 bits` symbol content; fixed 2 bits for 9 symbols requires 18 bits.

**Numerical 19 - Caesar cipher.** Shift key k=3 encrypt `CAT`. **Answer:** `FDW`; decrypt by shifting each letter backward 3. Educational only, not secure encryption.

**Numerical 20 - combined applied problem.** A 100 KB file compresses to 40 KB; transmission speed is 8 Mbps, ignoring headers and delays. Use 1 KB=1000 bytes. **Answer:** reduction=`60%`; uncompressed transmission time=`100,000*8/8,000,000=0.1 s`; compressed transmission time=`40,000*8/8,000,000=0.04 s`; idealised saving=`0.06 s`.

**Numerical practice reminder:** Verify unit conversion every time. 1 byte = 8 bits. Mbps generally means **million bits per second**, not MB/s. RTT should be expressed in **seconds** when multiplying by bandwidth in bits/s. When sequence numbers count bytes, do not add the number of segments instead of the payload length.

## 19. Frequent Exam Mistakes - Avoid These

1. **Wrong:** IP address alone identifies the receiving application. **Correct:** Transport ports help identify application endpoints.
2. **Wrong:** TCP is a physical dedicated circuit. **Correct:** It is a logical connection over packet-switched networks.
3. **Wrong:** TCP preserves every write as an individual message. **Correct:** TCP is an ordered **byte stream**.
4. **Wrong:** TCP sequence number counts packets. **Correct:** It identifies byte positions.
5. **Wrong:** ACK=100 means exactly 100 packets were received. **Correct:** The number refers to the next expected byte position.
6. **Wrong:** FIN and SYN do not use sequence numbers. **Correct:** Both consume one position.
7. **Wrong:** `cwnd` is a field sent in TCP header. **Correct:** `cwnd` is maintained by the sender; the header carries an advertised receive-window field.
8. **Wrong:** Flow control and congestion control are identical. **Correct:** Receiver buffer versus network capacity.
9. **Wrong:** Zero window means server is offline. **Correct:** It normally indicates receiver-side lack of available window space.
10. **Wrong:** All RPC calls execute exactly once even after crashes. **Correct:** Partial failures and duplicate side effects must be addressed explicitly.
11. **Wrong:** A lost TCP connection means a remote money transfer definitely failed. **Correct:** Application transaction outcome may be unknown.
12. **Wrong:** OSI Session Layer is a separate deployed software component on every Internet host. **Correct:** It is a conceptual layer; session functions are often implemented at the application level.
13. **Wrong:** Compression always reduces file size. **Correct:** Some data expands for a given algorithm.
14. **Wrong:** Huffman codes are fixed length. **Correct:** They are variable-length prefix codes.
15. **Wrong:** Hashing is encryption. **Correct:** Hashes are one-way digests, not decryptable ciphertexts.
16. **Wrong:** A plain hash proves the sender's identity. **Correct:** Authentication requires a keyed or signature-based mechanism.
17. **Wrong:** Signing means encrypting with private key. **Correct:** Digital signatures are distinct signing/verification algorithms.
18. **Wrong:** Caesar cipher is suitable for real bank apps. **Correct:** It is insecure and only useful for basic learning.
19. **Wrong:** TCP reliability ensures successful application-level execution. **Correct:** TCP acknowledges bytes, not completion of business logic.
20. **Wrong:** Compression and encryption always happen in a fixed mandated order. **Correct:** Protocol context and side-channel risks determine design.

## 20. Seven-Day Revision Plan for Unit IV

| Day | Topics to prepare | Writing practice |
|---|---|---|
| Day 1 | Transport Layer services and issues; TCP vs UDP | Q1; six-point comparison table |
| Day 2 | TCP header, sequence numbering, reliability | Draw TCP header; Q2 and Q4; 4 traces |
| Day 3 | Handshake, FIN, RST, TCP state transitions | Draw both connection diagrams; Q3 |
| Day 4 | Sliding window, `rwnd`, `cwnd`, zero-window, BDP | Q5; solve 6 window/BDP numericals |
| Day 5 | Session design and RPC working | Draw checkpoint and RPC diagrams; Q6 and Q7 |
| Day 6 | Presentation Layer, RLE/Huffman/LZ, cryptography | Q8-Q10; compression calculations |
| Day 7 | Full practice: all prescribed syllabus headings | Write 2 full 20-mark answers with diagrams and check mistakes |

**How to use this plan:** Spend the first study session reading **Hinglish**, second session rewriting English definitions and diagrams, and third session attempting questions without looking. Underline technical keywords such as *cumulative acknowledgement, half-close, marshalling, synchronisation, prefix-free code, receiver-advertised window and authenticated encryption*.

## 21. Unit IV Glossary - Essential Terms

| Term | Easy English meaning |
|---|---|
| Transport Layer | End-to-end communication service between application processes |
| Port | Transport identifier for an application service/socket |
| Socket | Operating system's communication endpoint |
| Multiplexing | Combining data from several applications for transport |
| Demultiplexing | Delivering incoming data to the proper destination socket |
| TCP | Reliable, ordered, connection-oriented byte-stream protocol |
| UDP | Connectionless datagram transport protocol |
| Segment | Unit of TCP data plus TCP control/header information |
| Sequence number | Byte position used for TCP stream ordering |
| ACK | Acknowledgement identifying the next expected byte |
| SYN | TCP control flag used in connection establishment |
| FIN | TCP control flag indicating end of one sender direction |
| RST | TCP control flag used to reset/abort a connection |
| RTO | Retransmission timeout |
| RTT | Round-trip time |
| SACK | Selective acknowledgement of received byte blocks |
| Sliding window | Moving allowed send/receive sequence interval |
| `rwnd` | Receiver-advertised flow-control window |
| `cwnd` | Sender's congestion-control window |
| BDP | Bandwidth times round-trip delay/time |
| TIME-WAIT | TCP close state for delayed-duplicate/close safety |
| Session Layer | Conceptual logical conversation management layer |
| Dialogue control | Rules for who transmits or operates when |
| Synchronisation | Coordinating progress between communicating entities |
| Checkpoint | Saved state from which recovery may resume |
| RPC | Remote Procedure Call |
| Client stub | Local proxy for invoking a remote procedure |
| Marshalling | Encoding arguments into transmissible form |
| Unmarshalling | Decoding arguments/results into usable values |
| Presentation Layer | Data representation, compression and protection concept |
| Serialisation | Converting structures to transferable data form |
| Lossless | Exact original data recoverable |
| Lossy | Approximate reconstruction after information discarded |
| RLE | Run Length Encoding of repeated adjacent symbols |
| Huffman | Variable-length prefix coding based on frequency |
| LZ77 | Dictionary compression using earlier substring references |
| Plaintext | Readable, unencrypted information |
| Ciphertext | Encrypted representation |
| Symmetric key | Secret key shared by communicating parties |
| Public key | Shareable part of an asymmetric key pair |
| Private key | Secret part of an asymmetric key pair |
| Hash | Fixed-size digest produced from input data |
| HMAC | Secret-key message authentication code based on hashing |
| Digital signature | Signature verified with a public key |
| TLS | Protocol family protecting many Internet application connections |

## 22. One-Page Final Revision Checklist

- [ ] I can define Transport Layer and list **10-12 design issues** with examples.
- [ ] I can explain TCP versus UDP without claiming either is universally better.
- [ ] I can **draw and label the TCP segment header** without assistance.
- [ ] I know **SYN -> SYN+ACK -> ACK** and calculate handshake ACK numbers.
- [ ] I can draw TCP FIN/ACK closure and explain TIME-WAIT and RST.
- [ ] I understand TCP byte sequence numbers and cumulative ACK values.
- [ ] I can calculate **min(rwnd,cwnd) minus in-flight bytes** in examples.
- [ ] I can explain zero-window probes, silly window syndrome and scaling.
- [ ] I can solve a **bandwidth-delay product** numerical with correct units.
- [ ] I can explain all major **Session Layer design issues** and checkpoints.
- [ ] I can draw RPC architecture and describe **all eight steps**.
- [ ] I can distinguish marshalling, binding, retries and idempotency.
- [ ] I can define Presentation Layer design issues with examples.
- [ ] I can compare lossless/lossy and solve RLE/compression-ratio questions.
- [ ] I can describe Huffman codes and draw a simple coding tree.
- [ ] I can compare symmetric/asymmetric, hashes, HMAC and signatures.
- [ ] I have practised writing **at least two 20-mark answers** in full English.

**End of Unit IV.** These notes deliberately follow the order and terminology of the photographed university syllabus. All long-answer questions are **practice recommendations**, not sourced previous-year university papers. For the most efficient revision, practise the diagrams and numericals first, then reproduce complete English answers using your own words.
