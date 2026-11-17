> **Syllabus alignment.** These notes cover only the uploaded **Unit I**: Introduction; goals and applications of networks; network structure and architecture; OSI reference model and services; network topology design; delay analysis; backbone design; local access network design; physical-layer transmission media; switching methods; ISDN; and terminal handling. Teaching examples, comparisons, formulas and practice questions are **additional explanations**, not questions taken from an official past paper.

> **How to study:** For EVERY topic, (1) read the **Hinglish** part for understanding, (2) learn the **English exam answer**, (3) practise the **labelled diagram**, and (4) write the **20-mark answers** at the end in your own words. The same topic appears in both languages intentionally.

# PART A - DETAILED UNIT I NOTES

## 1. Introduction to a Computer Network

### 1.1 Hinglish explanation - imagine your college computer lab

Socho college computer lab mein **40 computers** hain. Har computer alag hai. Agar kisi student ko teacher ki PDF chahiye aur har baar pen drive se file copy karni pade, to time waste hoga. Agar lab mein ek printer hai aur har PC se alag printer lagana pade, to cost badhegi. Ab sab computers ko **switch** aur **router** se connect kar diya: teacher ek shared folder mein PDF upload karega, students network se access kar lenge, aur poori lab ek printer share kar sakti hai. Isi arrangement ko **computer network** kehte hain.

**Simple definition:** Jab do ya do se zyada devices ek communication medium (cable ya wireless) ke through connect hokar data aur resources share karte hain, tab woh computer network hai.

**Relatable examples:** Phone ka Wi-Fi par YouTube chalana; UPI payment ke time phone, Internet aur bank servers ke beech messages jaana; laptop se Wi-Fi printer par assignment print karna; college lab mein ek common attendance system; WhatsApp mein message send karna. Har example mein devices communicate kar rahe hain, lekin network ka **size, ownership aur protocols** alag ho sakte hain.

**Important difference:** Internet is a global network of networks. Wi-Fi sirf ek wireless connection technology hai. Wi-Fi hona aur Internet milna same cheez nahi: router se Wi-Fi connected ho sakte ho, lekin ISP down ho to Internet nahi chalega.

```text
               COLLEGE COMPUTER LAB NETWORK

 [PC-1] ---\
 [PC-2] ----\
 [PC-3] -----[ Ethernet SWITCH ]----[ ROUTER ]----[ INTERNET ]
 [PC-4] ----/        |
 [PC-5] ---/         +----[ Shared Printer ]
                     |
                     +----[ Local File Server ]
```

### 1.2 Complete English exam explanation

A **computer network** is a collection of interconnected computers and other digital devices that communicate through wired or wireless links for the purpose of exchanging data, accessing services and sharing resources. The devices follow agreed communication rules called **protocols**. A small home network, a university campus network, a company's branch network and the Internet are all examples.

**Five basic components of data communication:**

1. **Sender:** device that originates the information. Example: the student's laptop.
2. **Receiver:** device that receives the information. Example: a university server.
3. **Message:** information being transmitted, such as text, image, audio, file or video.
4. **Transmission medium:** path used to transfer information, such as twisted-pair cable, optical fibre or radio waves.
5. **Protocol:** agreed rules for message format, addressing, sequence, timing and error handling.

```text
              MESSAGE / DATA
 SENDER  ------------------------------> RECEIVER
             TRANSMISSION MEDIUM
      <------ Protocol rules ------>
```

**Common network terms (write these in short answers):**

- **Host/end system:** a device that runs user applications, e.g., computer, phone or server.
- **Node:** any device or connection point on the network; routers and switches are also nodes.
- **NIC (Network Interface Card):** hardware interface connecting a device to the network.
- **Link:** direct communication connection between adjacent nodes.
- **Protocol:** rules followed by communicating entities, such as IP, TCP and HTTP.
- **Packet:** a unit of data transmitted across a packet network; typically includes control information and user data.
- **Bandwidth/data rate:** theoretical or configured link capacity, commonly expressed in bits per second.
- **Throughput:** rate of successful data delivery actually achieved.
- **Latency:** time needed to transfer information across a path.
- **Server:** computer or process offering a service. **Client:** computer or process requesting it.

**Modes of communication:**

| Mode | Meaning | Relatable example |
|---|---|---|
| Simplex | Data flows in only one direction | A basic broadcast transmitter to receivers |
| Half-duplex | Both sides can transmit, but not simultaneously | Walkie-talkie: one speaks while the other listens |
| Full-duplex | Both sides can transmit simultaneously | A normal phone conversation; switched full-duplex Ethernet |

```text
 SIMPLEX:      A -----------------> B
 HALF-DUPLEX:  A <----------------> B  (one direction at a time)
 FULL-DUPLEX:  A ================= B  (both directions together)
```

A network is considered effective when it provides reasonable **performance**, **reliability**, and **security**. Performance concerns speed, throughput and delay; reliability concerns failures and recovery; security concerns confidentiality, integrity and availability of resources.

---

## 2. Goals of Computer Networks

### 2.1 Hinglish - why do we build networks?

Network banane ka goal sirf Internet chalana nahi hota. Socho ek coaching institute ke 100 students hain:

- **Resource sharing:** Ek expensive laser printer ko 100 computers share kar sakte hain. Har PC ke liye printer nahi kharidna.
- **File/data sharing:** Teacher ek notes file central server par rakhe; har student latest version use kare.
- **Communication:** Email, live class, chat aur video meeting instantly possible.
- **Cost saving:** Shared storage, shared software, shared Internet connection cost reduce karte hain.
- **Reliability:** Agar main server fail ho, backup server data/service provide kar sakta hai; however, redundancy design karni padti hai.
- **Scalability:** 40 computers se 80 computers karna ho to network extend kiya ja sakta hai, subject to capacity.
- **Remote access:** Ghar se student college portal ke resources access kar sakta hai, suitable authentication ke saath.
- **Central management/security:** Institute admin users, backups, permissions aur software deployment centrally control kar sakta hai.

**Difference yaad karo:** Goal means **network kyun banaya**; application means **network kaha use ho raha hai**. For example, "resource sharing" is a goal; "hospital patient record system" is an application.

### 2.2 Complete English exam explanation

The important **goals (objectives)** of computer networking are:

1. **Resource sharing:** Hardware resources such as printers, storage devices and high-performance servers can be shared among multiple users, avoiding unnecessary duplication.
2. **Information sharing:** Users can access common databases, documents, software and files. Central copies reduce inconsistency when properly managed.
3. **Fast communication and collaboration:** Email, messaging, voice calls, video conferencing and shared editing support communication across distances.
4. **Cost efficiency:** Organisations can share Internet connections, storage, applications and administration facilities, reducing per-user cost.
5. **Improved reliability and availability:** Backup servers, replicated data and alternative links can keep services accessible when one component fails. Note that reliability is **not automatic**; the design must provide redundancy.
6. **Scalability and flexibility:** More users, devices, sites or applications can be added using a planned network architecture.
7. **Remote access:** Employees and students can use permitted resources from other locations by connecting over secure networks.
8. **Centralised control:** User permissions, software updates, monitoring and backup policies can be managed centrally.
9. **Distributed processing:** Different connected machines can cooperate on a large problem, as in cloud platforms or distributed computing systems.

**Advantages:** better communication, shared resources, lower duplication cost, collaboration and central management. **Limitations:** initial installation cost; security threats; malware may spread; congestion can cause poor performance; and failures of shared infrastructure can affect many users.

**Conclusion for examinations:** Networks support efficient communication and resource sharing, but their benefits depend on correct design, security and maintenance.

---

## 3. Applications of Computer Networks

### 3.1 Hinglish - examples you already use

UPI payment imagine karo. Tum phone par Rs. 100 send karte ho. Payment app request bhejti hai, multiple bank/payment systems information exchange karte hain, aur receiver ke account ka status update hota hai. Isme networking, servers aur security sab work karte hain. Online classes mein video/audio continuously travel karte hain; Amazon/Flipkart jaise shopping apps network par product search, order and payment process karte hain; college ERP mein marks aur attendance server par stored hote hain.

### 3.2 Complete English exam explanation

| Area | Application | Why networking is needed |
|---|---|---|
| Education | Online lectures, digital library, LMS, examination portal | Students and faculty access shared content |
| Banking and finance | ATM networks, online banking, digital payments | Secure and timely transaction communication |
| Business | Email, ERP, inventory, office file server | Coordinated information across departments |
| Healthcare | Electronic health records and telemedicine | Sharing patient data and remote consultation |
| E-commerce | Product catalogue, ordering, payments, tracking | Connects customers, vendors and service backends |
| Entertainment | Video streaming, multiplayer games | Delivers multimedia and interactive content |
| Government | Citizen portals, online certificates and services | Provides access across locations |
| Science | Distributed computation and research databases | Collaboration and computing-resource sharing |
| Home / IoT | Smart TV, sensors, security cameras | Connects household devices and services |
| Cloud computing | Remote servers, databases and backups | Makes shared computing resources accessible |

**Common examination instruction:** If asked "explain goals and applications", give two distinct headings, explain each point in two to four sentences, draw one network diagram and include benefits and limitations.

---

## 4. Types of Networks and Network Structure

### 4.1 Hinglish

Tumhare phone aur earbuds ka Bluetooth connection ek **PAN** ka example ho sakta hai. College lab ke computers connected hain to **LAN**. City ke different locations connect karne wali metro-area network ko **MAN** category mein rakh sakte hain. Lucknow office ko Delhi office se leased/wide-area links se connect karo, to **WAN**. Internet duniya bhar ke networks ka interconnection hai.

Network ke andar do major areas samjho:

1. **Network edge:** tumhara phone, browser, laptop, backend server. Yeh actual user applications chalate hain.
2. **Network core:** routers aur high-capacity links. Inka kaam packets ko destination ki taraf forward karna hai.

College building mein **access layer** students ko connect karti hai, **distribution layer** multiple lab switches ko aggregate karti hai, aur **core/backbone** departments ko high-speed path se connect karta hai.

### 4.2 Complete English exam explanation

**Network types based on geographical coverage:**

| Network type | Full form | Coverage | Example |
|---|---|---|---|
| PAN | Personal Area Network | Personal/nearby devices | Bluetooth phone-earbuds |
| LAN | Local Area Network | Room, building, campus area | College laboratory Ethernet |
| WLAN | Wireless Local Area Network | Local area using radio | Wi-Fi in a house/classroom |
| MAN | Metropolitan Area Network | City/metropolitan area | City-level network interconnection |
| WAN | Wide Area Network | Regions/countries/continents | Company network connecting branches |

**Distinguish Internet, intranet and extranet:**

- **Internet:** worldwide interconnection of independently managed networks using Internet protocols.
- **Intranet:** private organisational network for authorised internal users.
- **Extranet:** controlled extension of a private network or its services to approved outsiders, such as suppliers or partners.

**Basic structure of a computer network:**

```text
    NETWORK EDGE               NETWORK CORE              NETWORK EDGE

 [Student Laptop]--\                                 /--[College Web Server]
 [Phone]------------[Access Switch]--[Router]--[Router]--[Database Server]
 [Teacher PC]------/                   |     |       \--[Other Network]
                                   Backbone links
```

A typical network contains **end systems**, **access networks**, **communication links**, **switches/routers**, **network services** and **protocols**. Hosts generate or consume application traffic. Switches usually forward frames within a local network using link-layer information. Routers connect different IP networks and forward packets using network-layer information.

**Client-server architecture:** A central server provides a service requested by clients. For example, a browser sends an HTTP request to a website server, which returns a response. It is easy to centralise data, backups and permissions, but an overloaded or failed server can affect users unless replicas and load balancing are added.

**Peer-to-peer (P2P) architecture:** Connected participants can both request and provide resources. For example, peers in a file-distribution system exchange pieces directly. P2P can distribute traffic but may be more difficult to coordinate and secure.

```text
 CLIENT-SERVER                               PEER-TO-PEER

   Client 1 --\                                  [A]------[B]
   Client 2 ---[ CENTRAL SERVER ]                | \      / |
   Client 3 --/                                  |  \    /  |
                                               [C]------[D]
```

**Comparison:**

| Basis | Client-server | Peer-to-peer |
|---|---|---|
| Control | Usually central administration | Distributed among peers |
| Data/service | Mainly server-hosted | May be spread across peers |
| Management | Easier central policies | May be harder to coordinate |
| Failure risk | Server can be critical | Depends on peer availability |
| Example | University student portal | Peer-assisted file distribution |

---

## 5. Network Architecture, Protocols, Services and Layering

### 5.1 Hinglish - courier-service analogy

Socho tum Kanpur mein friend ko ek book courier kar rahe ho. Tum book pack karte ho, courier company pickup karti hai, sorting centre address dekhta hai, truck/flight book le jaati hai, aur destination office receiver ko handover karta hai. Har stage ka **different kaam** hai. Agar truck change ho jaye, tumhe book ka content change nahi karna. Networking mein bhi layered architecture isi idea par based hai.

- **Layer:** ek category of responsibility (jaise routing, reliability ya physical signals).
- **Service:** upar wali layer ko kya facility mil rahi hai? Jaise "data deliver karne ki facility".
- **Protocol:** same layer ke do systems **kaise** communicate karenge - rules aur message format.
- **Interface:** adjacent layers ek doosre se service kaise request karte hain.

**WhatsApp message ka simplified flow:** App message create karta hai -> transport data delivery handle karta hai -> network layer route/address decide karti hai -> Wi-Fi/Ethernet local link par frame bhejta hai -> radio/cable bits carry karti hai. Receiver side par ulta process hota hai.

### 5.2 Complete English exam explanation

**Network architecture** is the structured design of communication functions, devices, protocols and their relationships. In a **layered architecture**, related communication tasks are divided among layers. Each layer provides defined **services** to the layer above and follows **protocols** to communicate logically with the corresponding layer on another machine.

The most important concepts are:

1. **Layer:** functional division performing a specific set of communication responsibilities.
2. **Service:** the set of operations offered by one layer to the layer immediately above it. It specifies *what* can be done.
3. **Protocol:** conventions and rules used by peer entities, including message format, sequencing, addressing, timing and responses to errors. It specifies *how* communication takes place.
4. **Interface:** the defined boundary through which one layer accesses another layer's services.
5. **Peer entities:** corresponding layers in different devices that follow the same protocol.
6. **Encapsulation:** addition of layer-specific control information (headers and sometimes trailers) as data passes toward lower layers. **Decapsulation** removes it at the receiver.

**Why layered design is used:**

- Reduces complexity by splitting a large problem into smaller functions.
- Allows one technology to be replaced without redesigning the whole system, as long as interfaces remain consistent.
- Supports interoperability and standardisation.
- Makes learning, testing, debugging and maintenance easier.
- Enables reuse of common networking services.

**Disadvantages of layering:** Added headers and processing may create overhead. Some functions appear at more than one layer, and strict layer boundaries can prevent useful cross-layer optimisation.

### 5.3 Connection-oriented and connectionless services

**Hinglish:** Phone call jaisa connection-oriented model socho: pehle connection/session establish, phir conversation, phir close. Normal postcard jaisa connectionless model socho: har postcard address ke basis par independently travel karta hai; pehle dedicated connection setup nahi. Lekin "connection-oriented" ka matlab **always reliable** nahi aur "connectionless" ka matlab **always lost** bhi nahi; reliability individual protocol ki design par depend karti hai.

**English:**

| Feature | Connection-oriented service | Connectionless service |
|---|---|---|
| Setup | Logical association usually established first | No prior association required |
| Data handling | Multiple messages belong to one connection | Each packet/message independently addressed |
| Ordering / reliability | Can provide ordering/reliability, depending on protocol | Not guaranteed by basic datagram service |
| Overhead | Setup and state management | Lower setup overhead |
| Example | TCP provides connection-oriented reliable byte-stream service | IP and UDP are connectionless at their respective layers |

**Service primitives (conceptual model):** LISTEN (wait for request), CONNECT (initiate connection), ACCEPT (agree to connection), SEND (transmit), RECEIVE (collect), DISCONNECT (close). Their exact names and behaviour vary across interfaces and protocols; these are classroom examples rather than a universal API.

---

## 6. OSI Reference Model - THE MOST IMPORTANT THEORY TOPIC

### 6.1 Hinglish - seven departments delivering one message

OSI ko ek 7-floor wali building samjho. Top floor par user ka kaam hota hai, bottom floor actual wires/signals handle karta hai. Jab tum browser se college result website open karte ho, top side request create hoti hai; neeche aate-aate address, transport control aur local-link information add hoti hai; phir bits wire ya Wi-Fi se travel karti hain. Receiver par process reverse hota hai. **OSI ek reference model hai**, Internet par har protocol exactly separate seven boxes mein implement hota hai aisa assume nahi karna.

**Memory trick, Layer 7 -> 1:** **All People Seem To Need Data Processing** = Application, Presentation, Session, Transport, Network, Data Link, Physical.

```text
                 OSI REFERENCE MODEL

    HIGHER / USER-SIDE SERVICES
     +-------------------------------+
  7  | APPLICATION   | User services |
     +-------------------------------+
  6  | PRESENTATION  | Format/security|
     +-------------------------------+
  5  | SESSION       | Dialogue      |
     +-------------------------------+
  4  | TRANSPORT     | End-to-end    |
     +-------------------------------+
  3  | NETWORK       | Routing & IP  |
     +-------------------------------+
  2  | DATA LINK     | Framing & MAC |
     +-------------------------------+
  1  | PHYSICAL      | Signals/bits  |
     +-------------------------------+
     PHYSICAL CABLE / FIBRE / RADIO
```

### 6.2 Complete English exam explanation: full layer-by-layer details

The **OSI (Open Systems Interconnection) Reference Model**, developed within international standardisation work by ISO, provides a conceptual framework for understanding how data moves between networked systems. Its functions are divided into **seven layers**. Each layer supplies services to the layer above it and uses services from the layer below it.

#### Layer 1: Physical Layer

**Main job:** transferring raw bits over a physical medium by representing them as electrical, optical or radio signals.

**Important functions:** specification of signals, voltages/optical levels, connectors, physical interfaces, bit timing, line coding, data rates, transmission mode and physical topology. It does **not** read a destination IP address or recover an entire lost TCP message.

**Relatable example:** When your laptop sends bits over an Ethernet cable, the physical layer concerns the actual signal travelling along the copper conductors. In fibre optic networks, light pulses represent data. **Devices/examples:** cables, connectors, repeaters, physical-layer hub concepts.

#### Layer 2: Data Link Layer

**Main job:** providing frame transfer between nodes that share a link or local network.

**Important functions:** framing, link-layer/MAC addressing, error detection using checks such as FCS/CRC, link access control, and sometimes link-level reliability/flow control. Modern Ethernet switches forward Ethernet frames based on MAC addresses.

**Relatable example:** Two PCs connected to the same local Ethernet switch exchange frames. Their local addresses identify the network interfaces on the local link. **PDU:** Frame. **Devices/protocol families:** bridges, Ethernet switches, Ethernet (IEEE 802.3), Wi-Fi MAC (IEEE 802.11).

#### Layer 3: Network Layer

**Main job:** moving packets across multiple interconnected networks.

**Important functions:** logical addressing, route selection, packet forwarding, internetworking and fragmentation-related considerations where supported by the protocol. Routers inspect network-layer information to choose next hops.

**Relatable example:** Your college laptop accesses a website hosted in another city. The packet goes through multiple routers and networks toward the server. **PDU:** Packet. **Examples:** IPv4, IPv6 and routing technologies.

#### Layer 4: Transport Layer

**Main job:** end-to-end communication between application processes on hosts.

**Important functions:** segmentation and reassembly, process identification using port numbers, multiplexing/demultiplexing, flow control, reliability and congestion response when provided by the specific transport protocol.

**Relatable example:** A browser downloads a webpage. TCP can deliver an ordered and reliable byte stream so that lost segments are retransmitted. UDP provides simpler datagram transport without TCP's reliability guarantees. **PDU:** TCP segment or UDP datagram. **Examples:** TCP, UDP.

#### Layer 5: Session Layer

**Main job:** controlling logical conversations between communicating applications.

**Functions:** establishing/maintaining/ending sessions, dialogue control (who communicates when), synchronisation points or checkpoints in long activities, and resuming a logically managed conversation where supported.

**Relatable example:** During a long remote operation, checkpoints can help resume from an intermediate stage rather than starting the whole logical task again. In real Internet software these functions are often performed by application protocols or libraries rather than a separately deployed layer.

#### Layer 6: Presentation Layer

**Main job:** making information understandable between systems that may store or represent it differently.

**Functions:** data representation/translation (such as character encodings), serialisation formats, compression/decompression and encryption/decryption as conceptual responsibilities.

**Relatable example:** A message encoded as Unicode text has to be interpreted with the correct encoding. An image may be compressed before transmission. Encryption protects information in transit where applied. The OSI presentation-layer assignment is conceptual; technologies such as TLS do not map perfectly to one fixed OSI layer.

#### Layer 7: Application Layer

**Main job:** offering network-related services used by application software.

**Functions:** web requests, email transfer, name resolution, file transfer, remote services and application-level commands. The OSI application layer is **not the whole browser application**; it refers to the network services/protocols that applications use.

**Examples:** HTTP/HTTPS for web access, SMTP for sending email, FTP for file transfer and DNS for name resolution. The concrete responsibilities of some protocols cross neat model boundaries.

### 6.3 Layers, data units, addresses and typical devices

| OSI layer | Key responsibility | Typical unit | Address/control example |
|---|---|---|---|
| 7 Application | Network services to user programs | Data/message | URL, hostname, application request |
| 6 Presentation | Encoding, compression, cryptographic representation | Data | Data format/encoding |
| 5 Session | Dialog/session coordination | Data | Session state/identifier where applicable |
| 4 Transport | Process-to-process communication | Segment/datagram | Port numbers |
| 3 Network | Packet forwarding across networks | Packet | IP addresses; router |
| 2 Data Link | Frames over a local link | Frame | MAC addresses; switch |
| 1 Physical | Signals over the medium | Bits | Cable/fibre/radio; repeater |

### 6.4 Encapsulation and decapsulation - full explanation

**Hinglish:** Courier parcel analogy: tumne gift diya, delivery company ne address label add kiya, next stage ne sorting label add kiya. Har layer data ke bahar apna header laga sakti hai. Receiver par outer label hata kar inner data appropriate layer ko diya jata hai. **Important:** Router har hop par old data-link frame remove karke next link ke liye new frame banata hai; packet ka IP layer forwarding decision important hota hai.

**English:** When an application sends data, lower layers add protocol-specific control information. This is encapsulation. The receiver examines and removes the corresponding information in reverse order, called decapsulation. Headers contain fields such as source/destination addresses, ports or error-check information as appropriate.

```text
Application:                 [ USER DATA ]
Transport (TCP):       [ TCP HEADER | USER DATA ]            SEGMENT
Network (IP):     [ IP H | TCP HEADER | USER DATA ]          PACKET
Data Link:     [ MAC H | IP H | TCP H | DATA | TRAILER ]      FRAME
Physical:              101010011001010...                   BITS

Sender:    Application -> ... -> Physical
Receiver:  Physical -> ... -> Application
```

**Step-by-step browser example:** (1) A browser creates a request to access a college website. (2) Transport handling adds information for delivering data to the correct application process. (3) Network-layer handling provides logical addresses used for routing. (4) The local network adds a frame header/trailer appropriate to the next link. (5) Signals cross that link. (6) Routers forward the packet onto later links. (7) The destination host processes the data upward to the receiving application.

### 6.5 Advantages and limitations of OSI

**Advantages:** clear division of responsibility, vendor-neutral reference, easier troubleshooting, supports teaching and modular design, encourages interoperability. **Limitations:** a conceptual model rather than one compulsory Internet implementation, some functions overlap, additional layers may seem complex, and modern Internet protocols do not fit every boundary exactly.

### 6.6 OSI versus TCP/IP model

**Hinglish:** OSI ek **7-layer learning map** hai; TCP/IP woh practical protocol family hai jis par Internet run karta hai. Dono ko enemies nahi samajhna. TCP/IP teaching model kabhi 4 layers, kabhi 5 layers mein dikhaya jata hai. Exam mein usually 4-layer version aur 7-layer OSI compare karna.

```text
             OSI (7 LAYERS)                 TCP/IP (4 LAYERS)
           +------------------+         +---------------------+
           | Application      | -----\  |                     |
           | Presentation     | -------> | Application         |
           | Session          | -----/  |                     |
           +------------------+         +---------------------+
           | Transport        | ------> | Transport           |
           +------------------+         +---------------------+
           | Network          | ------> | Internet            |
           +------------------+         +---------------------+
           | Data Link        | -----\  | Link / Network      |
           | Physical         | -------> | Access              |
           +------------------+         +---------------------+
```

**English comparison:**

| Feature | OSI | TCP/IP |
|---|---|---|
| Nature | Seven-layer reference model | Practical Internet protocol suite with a layered model |
| Number of layers | Seven | Four in common teaching model; five in some textbooks |
| Upper functions | Application, Presentation, Session distinct | Usually combined into Application |
| Network function | Network Layer | Internet Layer (IP) |
| Bottom functions | Data Link and Physical distinct | Often combined as Link/Network Access |
| Primary use | Learning, design, troubleshooting reference | Actual Internet communication technology |
| Examples | Conceptual layer functions | HTTP, TCP, UDP, IP, Ethernet/Wi-Fi |

**20-mark tip:** Draw the OSI diagram, explain all seven layers with at least four functions and one example each, include encapsulation, show layer mapping, then finish with OSI-vs-TCP/IP comparison and a conclusion.

---

## 7. Network Topology Design - Structure of Connections

### 7.1 Hinglish - how should computers be connected?

Socho college ke 20 systems ko connect karna hai. Tumhare paas multiple choices hain. Sab ko ek shared cable par laga sakte ho (bus), sab ko ek central switch se individual cable laga sakte ho (star), circular chain bana sakte ho (ring), ya critical routers ke beech multiple alternate links laga sakte ho (mesh). In arrangements ko **network topologies** kehte hain.

**Physical topology** batati hai cable physically kahan lagi hai. **Logical topology** batati hai data logically kis pattern se move karta hai. For instance, physical wiring aur logical traffic organisation zaroori nahi ki same ho.

**Topology choose karte waqt** socho: agar switch fail ho gaya to kya hoga? agar ek cable toot jaaye to kitne students disconnect honge? kitna cable kharidna padega? future mein extra 10 computers easily add ho sakte hain? These are design questions.

### 7.2 Complete English definition

**Network topology** is the physical or logical arrangement of computers, communication devices and links in a network. A well-designed topology balances cost, performance, fault tolerance, ease of management and future growth. Major forms are bus, star, ring, mesh, tree and hybrid.

### 7.3 Bus topology

**Hinglish:** Bus topology mein ek common main cable hoti hai aur sab computers usi cable se connected hote hain. Imagine ek single corridor jahan sab log ek hi narrow path use kar rahe hain; heavy crowd ho to interference/congestion feel hogi. Main cable break ho jaaye to bada portion stop ho sakta hai. Classic bus Ethernet historical example hai; modern wired LANs generally switched star use karte hain.

```text
  [A]        [B]        [C]        [D]
   |          |          |          |
===+==========+==========+==========+===
          SHARED BACKBONE CABLE
```

**English:** In a bus topology, all stations connect to a shared communication backbone. The backbone carries transmitted signals to the attached stations. Termination is needed in some historical coaxial bus systems to prevent signal reflection. **Advantages:** low cable requirement and simple initial construction for a small historical network. **Disadvantages:** shared-medium contention, harder troubleshooting, limited growth and vulnerability to backbone failure. **Example:** legacy coaxial Ethernet networks.

### 7.4 Star topology

**Hinglish:** Sab computers apni alag cable se ek **central switch** ko connect hote hain. College lab mein ye best relatable example hai. PC-4 ki cable toot jaaye, baaki PCs usually work karte rehte hain; lekin central switch off ho jaaye to connected systems ka communication affect hoga.

```text
                   [PC-1]
                      |
        [PC-2] --- [SWITCH] --- [PC-3]
                      |
                   [PC-4]
                      |
                  [PRINTER]
```

**English:** Every node has a separate connection to a central hub, switch or access device. Most modern wired local networks use a physical star with Ethernet switches. **Advantages:** straightforward installation, monitoring and fault isolation; easy addition/removal of stations; failure of one access link normally affects only that station. **Disadvantages:** central device is an important failure point; requires more access cable than a bus; switch capacity matters. **Example:** school computer labs and office Ethernet.

### 7.5 Ring topology

**Hinglish:** Har computer apne two neighbouring nodes se connected hota hai aur circular arrangement banta hai. Ek token-based ring mein transmission permission **token** se control ho sakti hai; sirf token holder send karega. Single ring mein one link break problem de sakta hai; protected/dual ring designs alternative path dete hain.

```text
              [A] -------- [B]
               |            |
               |            |
              [D] -------- [C]
                 CLOSED RING
```

**English:** In ring topology, each device is connected to neighbouring devices forming a closed path. Data may travel in one direction or both directions depending on the technology, and token-passing can provide controlled access. **Advantages:** orderly access and predictable operation in token systems. **Disadvantages:** some implementations are sensitive to node/link failures and require special mechanisms for adding nodes or protecting the ring. **Examples:** classic Token Ring and dual-ring FDDI concepts.

### 7.6 Mesh topology

**Hinglish:** Imagine Lucknow, Delhi, Mumbai aur Kolkata ke important routers. Agar har router ka har dusre router se direct link hai, to **full mesh**. Agar kuch important alternate links hain but every pair connected nahi, to **partial mesh**. Ek link fail ho jaaye to other route mil sakta hai, but cost badh sakti hai.

```text
     FULL MESH (FOUR NODES)

           [A]----------[B]
            | \        / |
            |  \      /  |
            |   \    /   |
            |    \  /    |
            |    /  \    |
            |   /    \   |
           [D]----------[C]

     Every node has a direct link to every other node.
```

**English:** A mesh topology provides multiple interconnections between nodes. In a **full mesh**, each of the n nodes is directly connected to the other n-1 nodes; in a **partial mesh**, only selected connections exist. Mesh networks improve path diversity and fault tolerance but require more links, interfaces, configuration and cost.

**Formula for full mesh, undirected point-to-point links:**

$$
L=\frac{n(n-1)}{2}
$$

**Solved example:** With **6 routers**, number of links = 6 x 5 / 2 = **15**. Each router needs **5 direct neighbour connections**.

### 7.7 Tree and hybrid topology

**Hinglish:** Tree topology ek family tree jaisi hoti hai: core switch se department switches, unse lab PCs. College campus networks often hierarchy use karte hain. Hybrid topology mein do ya more topology types combine hote hain, jaise different buildings ki internal star networks ko redundant backbone se connect karna.

```text
                 [ CORE SWITCH ]
                  /     |      \
           [Dept A] [Dept B] [Dept C]
             / \       / \       / \
           PC  PC    PC  PC     PC  PC
                 TREE TOPOLOGY
```

**English:** In a **tree topology**, nodes form a branching hierarchy. This supports structured expansion and departmental grouping, but failures higher in the hierarchy may affect many downstream devices. A **hybrid topology** combines multiple forms, such as stars connected by a partially meshed core. Hybrid designs offer flexibility but can increase planning and troubleshooting complexity.

### 7.8 Comparative answer - topology chart

| Topology | Cabling/cost | Fault tolerance | Easy to expand? | Common use |
|---|---|---|---|---|
| Bus | Low in small legacy systems | Poor if main bus breaks | Limited | Older shared networks |
| Star | Moderate, central switch required | Access cable failure isolated; switch critical | Yes | Modern office LAN |
| Ring | Moderate | Depends on single/dual ring protection | More specialised | Token-based/legacy systems |
| Full mesh | Very high for large n | High path redundancy | Expensive as n grows | Small critical router groups |
| Tree | Hierarchical | Upper-level devices are critical | Yes with planning | Campus/enterprise networks |
| Hybrid | Variable | Depends on combined design | Flexible | Large practical networks |

**Full-mesh vs star numeric example:** Eight PCs need **8 PC-to-switch access links** in a star, but 8 devices in a direct full mesh need 8 x 7 / 2 = **28 inter-device links**. A star uses a central switch (additional device), so these counts alone are not total equipment costs.

### 7.9 Steps to design an appropriate topology

1. Identify users, devices, buildings and physical distances.
2. Estimate present and future traffic loads and bandwidth needs.
3. Check the budget and cable/wireless feasibility.
4. Identify critical resources and potential single points of failure.
5. Choose the access arrangement (often switched star), distribution and core links.
6. Provide redundancy where required by the availability target.
7. Plan addressing, administration, security and monitoring.
8. Test coverage, delay, failure recovery and expansion capacity.

---

## 8. Network Delay Analysis - Concept and Numericals

### 8.1 Hinglish - why does the Internet feel slow?

Tum YouTube par play karte ho. Kabhi video instantly load, kabhi buffering. Sirf "speed 100 Mbps" se complete performance decide nahi hoti. Data ko device par process hona, router ki queue mein wait karna, wire par bits ek-ek karke push hona, aur signal ka physical distance travel karna padta hai. Isliye **delay ke 4 types** yaad karo:

1. **Processing delay:** Router packet ka header/check etc. examine karta hai. Example: parcel counter par address scan karna.
2. **Queueing delay:** Router busy hai; packet line mein wait karta hai. Example: bank mein token line.
3. **Transmission delay:** Packet ke saare bits link par daalne mein time. Example: water bottle ko narrow pipe mein pour karna. **Packet size / bit rate** se calculate hota hai.
4. **Propagation delay:** Pehla bit medium ke through end tak pahunchne mein time. Example: vehicle already road par hai but destination tak travel karna hai. **Distance / signal speed** se calculate hota hai.

**Most important confusion:** High bandwidth transmission delay kam kar sakti hai, lekin distance ki wajah se propagation delay automatically zero nahi hoti.

### 8.2 Complete English explanation

**Network delay (latency)** is the time taken for data to pass from a sender through one or more network links and devices to its destination. At a router or other intermediate node, the nodal delay commonly includes four components:

$$
D_{nodal}=D_{processing}+D_{queue}+D_{transmission}+D_{propagation}
$$

**1. Processing delay:** Time to examine packet headers, detect certain errors, select an output interface and perform required operations. It depends on hardware and processing work.

**2. Queueing delay:** Time the packet spends waiting before it can use an outgoing link. It depends on the arrival pattern, traffic load, scheduling policy and available capacity; it can vary greatly.

**3. Transmission delay:** Time required to put all bits of a packet on the link:

$$
D_{transmission}=\frac{L}{R}
$$

where **L** = packet length in **bits** and **R** = link transmission rate in **bits/second**. Example: a 1000-byte packet has 8000 bits; at 1 Mbps the transmission delay is 8000 / 1,000,000 = 0.008 second = **8 ms**.

**4. Propagation delay:** Time a signal takes to travel through the physical medium:

$$
D_{propagation}=\frac{d}{v}
$$

where **d** = link distance in metres and **v** = signal propagation speed in metres/second. Optical and electrical signals generally travel more slowly in a cable than in a vacuum; a teaching approximation for many cables is **2 x 10^8 m/s**.

**Diagram:**

```text
 [Computer]--(LINK 1)-->[Router]--(LINK 2)-->[Server]
       |                     |
    Tx + Prop          Process + Queue + Tx + Prop

                 TOTAL PATH DELAY
```

### 8.3 Solved numerical 1 - transmission versus propagation

**Question:** A 1500-byte packet is sent over a 12 Mbps link with a distance of 600 km. Propagation speed is 2 x 10^8 m/s. Find transmission and propagation delays (ignore processing and queueing).

**Solution:**

- Packet size L = 1500 x 8 = **12,000 bits**.
- Bit rate R = 12 x 10^6 bits/s.
- Transmission delay = 12,000 / 12,000,000 = 0.001 s = **1 ms**.
- Distance d = 600 x 1000 = 600,000 m.
- Propagation delay = 600,000 / (2 x 10^8) = 0.003 s = **3 ms**.
- Total = **1 + 3 = 4 ms**.

**Exam observation:** Transmission depends on *packet size and rate*; propagation depends on *distance and signal speed*.

### 8.4 Solved numerical 2 - nodal delay

**Question:** A router has processing delay 0.5 ms and queueing delay 2 ms. A 1000-byte packet is transmitted on a 4 Mbps link over 200 km; propagation speed = 2 x 10^8 m/s. Find total nodal delay.

**Solution:**

- L = 1000 x 8 = 8000 bits.
- Transmission = 8000 / 4,000,000 = 0.002 s = **2 ms**.
- Propagation = 200,000 / 200,000,000 = 0.001 s = **1 ms**.
- Nodal delay = 0.5 + 2 + 2 + 1 = **5.5 ms**.

### 8.5 Solved numerical 3 - store-and-forward over multiple links

**Question:** A 1200-byte packet moves through 3 identical links, each of 2 Mbps. There are 2 intermediate store-and-forward routers. Ignore propagation, processing, queueing and headers. How long to receive the whole packet?

**Solution:** L = 1200 x 8 = 9600 bits. Time per link = 9600 / 2,000,000 = **4.8 ms**. In **store-and-forward**, each router must receive the complete packet before sending it to the next link. Across three links: 3 x 4.8 = **14.4 ms**. If propagation and other delays are specified, add them.

**Note:** For multiple *packets*, pipelining can alter the time to deliver the entire batch. Do not automatically multiply packet count by an end-to-end single-packet time without considering the question's assumptions.

### 8.6 Solved numerical 4 - bandwidth delay product

**Question:** A link provides 10 Mbps with a round-trip time (RTT) of 40 ms. Calculate the bandwidth-delay product using this RTT.

**Solution:** BDP = bandwidth x RTT = 10,000,000 x 0.040 = **400,000 bits** = **50,000 bytes** (50 kB in decimal units). This represents the approximate volume of data that can be in flight over an RTT at that rate.

### 8.7 Other important performance terms

- **Bandwidth / link rate:** transmission capacity (e.g., 100 Mbps).
- **Throughput:** amount of useful data successfully delivered per second; usually below the raw link rate due to overhead, congestion and other limits.
- **Goodput:** application data delivered per second excluding retransmissions and protocol overhead.
- **RTT:** time for a signal/request to travel out and a response/acknowledgement to return, including relevant delays; it is not always exactly two times one-way delay.
- **Jitter:** variation in packet delay; it matters for video/audio calls.
- **Packet loss:** packets that fail to reach the destination.

| Quantity | Formula / basis | Primarily depends on |
|---|---|---|
| Transmission delay | L/R | Bits and link speed |
| Propagation delay | d/v | Distance and signal propagation speed |
| Nodal delay | Processing + queue + transmission + propagation | Device, traffic and link factors |
| Bandwidth-delay product | Rate x time (often RTT) | Capacity and delay |
| Full-mesh links | n(n-1)/2 | Number of nodes |

**Avoid unit mistakes:** 1 byte = 8 bits; 1 ms = 0.001 s; 1 km = 1000 m; 1 Mbps = 10^6 bits/s; 1 Gbps = 10^9 bits/s.

---

## 9. Backbone Network Design

### 9.1 Hinglish - college campus example

Imagine college mein three buildings hain: **BCA**, **Library** aur **Administration**. Har building mein computers apne local switch se connected hain. Ab tino buildings ko aapas mein high-speed connect karna hai. Jo central, high-capacity links/switches multiple local networks ko connect karte hain, unhe **backbone network** keh sakte hain. Bilkul human body ki spine jaisa central support, lekin network mein ye technical data path hai.

Agar BCA se library server par 50 students ek saath videos access karte hain, to building-to-building backbone slow nahi honi chahiye. Critical campus mein backup path bhi dena useful hai so that one fibre cut does not disconnect an entire building.

```text
                CAMPUS BACKBONE (EXAMPLE)

   [BCA Building LAN]       [LIBRARY LAN]
           |                       |
      [Distribution A]        [Distribution B]
            \                   /
             \--[CORE SWITCH]--/
                     |
                [ROUTER/FW]
                     |
                  [INTERNET]
                     |
              [ADMIN BUILDING LAN]
```

This simplified diagram shows connectivity, not guaranteed redundancy. To provide fault tolerance, additional independent core devices/links must be introduced.

### 9.2 Complete English explanation

A **backbone network** is a high-capacity central communication system that interconnects local or departmental networks. In a campus, a backbone connects buildings and distribution networks; in a service provider, backbones interconnect major sites and routers. Backbone design is the planning of links, devices, capacity, resilience and routing/switching arrangements for this central transport network.

**Purposes:** interconnect LANs; carry aggregated traffic; provide high capacity; support efficient routing and switching; isolate traffic domains where appropriate; improve availability through redundant paths; and allow campus/enterprise expansion.

**Backbone components:** high-speed switches or routers; optical fibre/copper links as appropriate; transceivers/interfaces; redundant power; routing protocols or switching control; firewalls/edge connections; monitoring; structured cabling and patch panels.

### 9.3 Backbone architecture types

**A. Serial (daisy-chain) backbone:** intermediate devices are connected one after another. This is simple in a very small setup but can create bottlenecks and multiple failure dependencies.

```text
 Building A -- Building B -- Building C -- Building D
```

**B. Distributed / hierarchical backbone:** local switches connect to distribution equipment, which connects to core devices. It supports administration and growth.

```text
 [Access A]--[Distribution A]--[CORE]--[Distribution B]--[Access B]
```

**C. Collapsed backbone (collapsed core):** core and distribution functions are combined into fewer central devices, common in smaller campus networks. It reduces device count but requires careful planning of concentration and failure points.

```text
  Department A --\
  Department B ---[ Central L3 Switch ]---[ Router/Internet ]
  Department C --/
```

**D. Redundant / meshed backbone:** two or more core devices and multiple links provide alternative paths when components fail, subject to correct routing and failover configuration.

```text
       [CORE 1]================[CORE 2]
         /   \                  /   \
       [A]   [B]              [C]   [D]
      Building LANs connected through protected paths
```

### 9.4 Main backbone-design requirements

1. **Capacity planning:** Estimate total simultaneous inter-building or inter-department traffic, rather than merely counting computers.
2. **High availability:** Provide redundant core equipment, power or independent fibre routes where business requirements justify cost.
3. **Scalability:** Reserve interfaces and capacity for new buildings, services and users.
4. **Physical constraints:** Consider fibre distance, conduits, electromagnetic interference, power and environment.
5. **Low delay / reasonable congestion:** Adequate links and buffers help maintain good performance during peak periods.
6. **Security and segmentation:** Use routing, VLAN design, access control and firewalls at suitable boundaries.
7. **Manageability:** Label cables, document topology, monitor utilisation and support easy troubleshooting.
8. **Budget:** Compare acquisition, installation, licensing, maintenance, power and replacement costs.

### 9.5 Step-by-step campus backbone design example

**Scenario:** Three departments have 60, 40 and 50 PCs. Each has its own access switches. The university needs a central file server, shared Internet and future building expansion.

1. Put users in appropriate access networks with enough switch ports and uplink speed.
2. Use a distribution switch in each building where its size warrants one.
3. Connect building distributions to a central core using appropriate high-capacity links, often fibre for building-to-building distances.
4. Route traffic between required IP networks and provide controlled access to common servers.
5. Connect the campus edge to the ISP through appropriate router/firewall functions.
6. Add independent links/core equipment where a single failure would be unacceptable.
7. Record IP networks, link capacity, fibre paths, rack locations and device responsibilities.
8. Test peak traffic, failure recovery and management access.

**Conclusion:** Backbone design focuses on reliable, scalable and high-speed interconnection between local networks; merely connecting everything with a cable is not sufficient.

---

## 10. Local Access Network Design

### 10.1 Hinglish - connecting each desk and classroom

Backbone buildings ko connect karta hai, jabki **local access network** each laptop, phone, printer aur lab PC ko actual network mein entry deta hai. Imagine BCA lab mein 40 desktops aur classroom mein 25 phones: desktops ke liye Ethernet switch plus cables sensible hain; mobile phones ke liye Wi-Fi access points. Dono ko router/core tak suitable uplink se connect karna hai.

**Everyday analogy:** Backbone main highway hai. Access network tumhare ghar/college ke gate se highway tak ki local roads hain. Highway fast ho, lekin tumhari gali blocked ho to travel slow hoga. Waise hi high-speed Internet plan ke baad bhi weak Wi-Fi ya overloaded access switch performance limit kar sakta hai.

### 10.2 Complete English explanation

A **local access network** is the portion of a network through which end-user devices connect to shared resources, organisational infrastructure and wider networks. It includes edge switches, network interfaces, Wi-Fi access points, cables and the connection from access devices to distribution/core networks.

**Access network alternatives:**

- **Wired Ethernet LAN:** devices connect using appropriate copper/fibre cables to switches. Good for stable desktops, laboratories and devices requiring predictable performance.
- **Wireless LAN (Wi-Fi):** devices communicate using radio with access points; useful for mobility, classrooms and common areas. Radio coverage and capacity require planning.
- **Mixed/hybrid access:** many campuses use Ethernet for fixed devices and Wi-Fi for mobile devices.

### 10.3 Typical LAN design diagram

```text
                      INTERNET
                         |
                    [Router/Firewall]
                         |
                    [Distribution]
                       /     \
             [Switch A]       [Switch B]
              /   |   \          |   \
           PC1   PC2 Printer    PC3  [Wi-Fi AP]
                                       /    \
                                   Phone  Laptop
```

### 10.4 Steps to design a campus computer lab LAN

**Step 1 - Count endpoints.** List desktops, printers, CCTV, IP phones, access points, servers and potential future devices. Avoid choosing a switch with exactly today's ports if expansion is expected.

**Step 2 - Determine traffic needs.** Estimate use cases: ordinary browsing, live classes, large file transfers, programming labs, backups or video streaming. One 100 Mbps Internet connection shared among 40 PCs does not guarantee 100 Mbps simultaneously to every PC.

**Step 3 - Choose technology.** Wired Ethernet is typically suitable for desktops; Wi-Fi supports mobile devices. Use appropriate media and supported standards based on physical distances and requirements.

**Step 4 - Plan topology and cabling.** An access-star arrangement with centrally managed switches is common. Use suitable cable paths, patch panels, labels and power.

**Step 5 - Plan addressing and services.** Allocate IP networks/subnets; decide how addresses are assigned (such as DHCP) and how hosts resolve names (DNS). These services are implementation choices for a functioning LAN.

**Step 6 - Separate groups where needed.** Staff, students, guests and important servers may require different VLANs, subnets and policies. A VLAN is logical segmentation; it does not by itself replace security policy.

**Step 7 - Plan Wi-Fi coverage.** Position APs where signal reaches intended areas. Walls, interference, radio channels and number of connected clients affect performance.

**Step 8 - Provide uplinks and Internet access.** Connect access switches/APs to distribution/core and then to a router/firewall and provider link as appropriate.

**Step 9 - Secure and maintain.** Use authenticated Wi-Fi, strong credentials, device updates, network monitoring and backup configuration. Document cable ports and IP addressing.

**Step 10 - Verify.** Test local file transfer, printing, Internet reachability, Wi-Fi coverage, maximum expected load and fault isolation.

### 10.5 Access versus backbone - high-scoring comparison

| Basis | Local access network | Backbone network |
|---|---|---|
| Primary purpose | Connect user devices to network | Connect LANs, buildings or major network segments |
| Typical devices | Access switches, APs, network cards | High-capacity switches/routers, core links |
| Traffic level | Individual user/device traffic | Aggregated traffic from many access networks |
| Physical placement | Rooms, floors, labs | Campus/buildings/core facilities |
| Design priority | User coverage, ports, cabling, Wi-Fi | Capacity, resilience, scalable interconnection |
| Example | PCs connecting to lab switch | Department links joining campus core |

**20-mark tip:** Explain access and backbone separately, draw one campus diagram showing both, compare their roles, describe a practical design procedure, and give advantages and design considerations.

---

## 11. Physical Layer Transmission Media

### 11.1 Hinglish - how do 0 and 1 travel?

Tum mobile se message bhejte ho, to messages magic se travel nahi karte. Device data ko **electrical signals**, **light signals** ya **radio signals** ke form mein represent karta hai. Signal ko travel karne ka physical path **transmission medium** kehte hain.

**Two main categories:**

- **Guided / wired media:** signal ko physical cable guide karti hai. Examples: twisted-pair, coaxial, fibre-optic cables.
- **Unguided / wireless media:** signals air/space ke through electromagnetic waves ki form mein travel karte hain. Examples: radio, terrestrial microwave, infrared, satellite communication.

**Relatable example:** College lab desktop ke liye Ethernet cable, building-to-building backbone ke liye optical fibre, mobile phone ke liye Wi-Fi radio. Same communication goal ke liye situation ke hisab se different media select karte hain.

### 11.2 Complete English explanation

A **transmission medium** is a physical channel or path that carries signals between a transmitting device and a receiving device. The Physical Layer deals with representing and transmitting bits using electrical, optical or electromagnetic signals. A medium is selected according to required bandwidth, distance, cost, environmental conditions, security and installation requirements.

### 11.3 Guided medium A - twisted-pair cable

**Hinglish:** Do insulated copper wires ko twist kar diya jata hai. Twist ka purpose interference/crosstalk reduce karna hai. **UTP** mein additional metallic shielding nahi hoti; **STP** forms mein shielding add hoti hai. College PC to switch Ethernet cable usually twisted-pair hoti hai.

```text
 TWISTED PAIR - SIMPLIFIED

 Wire 1:   /\    /\    /\    /\
          /  \  /  \  /  \  /  \
 Wire 2:  \  /  \  /  \  /  \  /
           \/    \/    \/    \/

 Two insulated conductors wound around each other
```

**English:** Twisted-pair cable contains pairs of insulated copper conductors twisted together to reduce electromagnetic interference and crosstalk. It is widely used in telephone systems and Ethernet local networks. Two common categories are **UTP (Unshielded Twisted Pair)** and **shielded twisted pair** designs.

**Advantages:** relatively inexpensive, flexible, widely available, easy installation and termination. **Disadvantages:** susceptible to interference compared with fibre, attenuation over distance and limits imposed by cable category and link length. **Applications:** desktop Ethernet, telephony and short building links.

**Exam note:** For common twisted-pair Ethernet structured cabling, a **100-metre channel** is a familiar design figure for many conventional standards, but the exact limit depends on the Ethernet technology, cable category and application. Do not claim all copper networking has one universal limit.

### 11.4 Guided medium B - coaxial cable

**Hinglish:** TV cable jaisi structure imagine karo. Beech mein copper conductor, uske around insulating material, phir metallic shielding, aur outside protective jacket. Shielding noise reduce karne mein help karti hai. Coaxial cable traditional cable TV aur older shared Ethernet designs mein use hui.

```text
    COAXIAL CABLE - CROSS SECTION (NOT TO SCALE)

     +----------------------------------------+
     | Outer protective plastic jacket        |
     |   +--------------------------------+   |
     |   | Metallic shield / outer       |   |
     |   | conductor                     |   |
     |   |   +-----------------------+   |   |
     |   |   | Dielectric insulation |   |   |
     |   |   |       [CORE]          |   |   |
     |   |   +-----------------------+   |   |
     |   +--------------------------------+   |
     +----------------------------------------+
```

**English:** Coaxial cable contains a central conductor surrounded by an insulating dielectric, a conductive shield and an outer jacket. Its construction provides shielding against external interference and supports high-frequency signal transmission. **Advantages:** good shielding, robust construction and useful frequency characteristics. **Disadvantages:** generally less convenient than twisted pair for modern office Ethernet installations, bulkier cable and more specialised connections. **Applications:** cable television networks, broadband cable access and older LANs.

### 11.5 Guided medium C - optical fibre

**Hinglish:** Fibre-optic cable mein copper electricity ki jagah **light pulses** travel karte hain. Hair-thin glass/plastic core ke around cladding hoti hai, jo light ko guide karne mein help karti hai. Imagine college ki different buildings 500 m apart hain: fibre often attractive because high-capacity signals aur longer distances ke liye strong choice hai.

```text
 FIBRE OPTIC CABLE - CONCEPTUAL VIEW

  OUTER JACKET
   +------------------------------------------+
   | PROTECTIVE COATING / BUFFER              |
   |  +------------------------------------+  |
   |  | CLADDING                           |  |
   |  |    ----------------------------    |  |
   |  |    CORE: light travels  ------>    |  |
   |  |    ----------------------------    |  |
   |  +------------------------------------+  |
   +------------------------------------------+
```

**English:** Optical fibre transmits information through pulses of light travelling in a glass or plastic core surrounded by cladding. It uses optical guiding based on refractive-index differences; the simplified school explanation often refers to **total internal reflection**. Fibre supports high data capacity and long transmission distances, and it is immune to electromagnetic interference affecting metallic cables.

**Types:**

- **Single-mode fibre:** a small core supports a single principal propagation mode; suitable for long distances and high-capacity links, commonly using laser-based optical systems.
- **Multimode fibre:** a larger core supports multiple optical modes; commonly used for shorter links, such as within buildings or data centres, subject to speed and fibre type.

**Advantages:** high capacity, low attenuation for suitable systems, electromagnetic-interference immunity, electrical isolation and strong suitability for backbone connections. **Disadvantages:** transceivers, termination and splicing can require specialised equipment and skill; fibres can be damaged by excessive bending; total installed cost varies.

### 11.6 Unguided medium A - radio waves

**Hinglish:** Wi-Fi router ke paas phone cable se connected nahi hota, phir bhi data travel karta hai because radio signals use hote hain. Radio signals walls/obstacles aur nearby devices ke interference se affect ho sakte hain. Different radio systems have different frequencies and propagation behaviour.

**English:** Radio communication uses electromagnetic waves radiated through space. Depending on frequency, antenna and environment, signals may penetrate obstacles or require more direct paths. **Advantages:** mobility, coverage without individual cables, relatively quick deployment. **Disadvantages:** interference, shared spectrum, variable signal quality and need for wireless security. **Examples:** Wi-Fi, Bluetooth and various broadcast/mobile systems.

### 11.7 Unguided medium B - microwave

**Hinglish:** Agar do buildings ke rooftop par antenna dish align kar do, to direct radio/microwave link se communicate kar sakte ho, provided suitable equipment, distance, line of sight and regulations. Building beech mein aa gayi to signal weaken/block ho sakta hai.

**English:** Microwave links use high-frequency electromagnetic signals and are often directional. Terrestrial point-to-point microwave commonly requires a sufficiently clear line of sight and suitable antennas. **Advantages:** rapid inter-building connections and high-capacity alternatives where installing fibre is difficult. **Disadvantages:** alignment, obstacles, weather effects at some frequencies and spectrum/licensing constraints.

### 11.8 Unguided medium C - infrared

**Hinglish:** TV remote ka infrared relatable example hai: short-range light-based communication, usually line of sight ya reflected path chahiye. Walls ke through normal remote signal directly pass nahi hota.

**English:** Infrared transmission uses electromagnetic radiation above visible-light wavelengths and can support short-range communication. It is typically limited by obstacles and environmental conditions. **Example:** television remote controls and some short-distance device links.

### 11.9 Unguided medium D - satellite

**Hinglish:** Remote mountain region mein cable reach karna difficult ho sakta hai; satellite link useful ho sakta hai. Earth station signal satellite ko bhejta hai (uplink), satellite relay karta hai, dusri side receive karti hai (downlink). Distance long ho sakti hai, to latency important design issue hai; orbit type se delay kaafi change hota hai.

```text
          [ COMMUNICATION SATELLITE ]
                    /      \
               UPLINK      DOWNLINK
                 /            \
       [GROUND STATION A]  [GROUND STATION B]
```

**English:** Satellite communication uses radio links between Earth stations and satellites. Satellites may relay traffic between distant regions and are useful for remote coverage, broadcasting and backup communication. **Advantages:** wide coverage and access to areas with weak terrestrial infrastructure. **Disadvantages:** capacity and service costs, interference/weather effects for some bands, and sometimes significant propagation delay. Delay varies greatly between low-earth-orbit and geostationary satellite systems.

### 11.10 Comparison: media selection

| Medium | Signal | Relative strengths | Limitations | Typical example |
|---|---|---|---|---|
| Twisted pair | Electrical | Low cost, flexible, common LAN | Distance/interference constraints | PC-to-switch Ethernet |
| Coaxial | Electrical | Shielded, suitable for cable systems | Bulky; less common modern LAN | Cable TV/broadband |
| Optical fibre | Light | High capacity, long distance, EMI immune | Installation/transceiver expertise | Campus/backbone links |
| Radio | Electromagnetic wave | Mobility, no wired drop to each device | Interference, shared radio medium | Wi-Fi |
| Microwave | Directed electromagnetic wave | Building-to-building without fibre trench | Often line of sight | Rooftop links |
| Infrared | Infrared radiation | Short range, simple devices | Obstacle sensitive | Remote control |
| Satellite | Radio via orbiting system | Wide coverage | Equipment/service cost and latency | Remote connectivity |

**Design factors:** distance, required data rate, expected users, electromagnetic noise, physical access, cost, installation skills, security, ease of maintenance and future expansion.

---

## 12. Switching Methods

### 12.1 Hinglish - three ways to send information

Socho tum Lucknow se Delhi tak ek shipment send karna chahte ho. **Circuit switching** mein aisa imagine karo jaise poore travel ke liye dedicated path reserve ho jaye, chahe kuch time tum kuch bhej hi na rahe ho. **Message switching** mein poora parcel ek centre par store hota hai, phir next centre ko forward hota hai. **Packet switching** mein long information ko chhote packets mein tod diya jata hai; network un packets ko aage forward karta hai. Modern Internet mainly packet switched hai.

### 12.2 Complete English definition

**Switching** is the process of transferring information from an incoming communication link to an appropriate outgoing link through intermediate network devices. Important classical switching methods are **circuit switching**, **message switching** and **packet switching**.

### 12.3 Circuit switching - working, advantages, disadvantages

**Hinglish:** Traditional telephone call mein pehle connection setup hota tha, phir conversation dedicated resources use kar sakti thi, aur last mein connection release hota tha. Call setup ke time required circuits/resources unavailable ho to call block ho sakti thi. Agar tum chup ho, reserved resource still allocated reh sakta hai.

```text
  A ----[SWITCH 1]====[SWITCH 2]====[SWITCH 3]---- B
                    RESERVED PATH

  SETUP -> DATA TRANSFER -> RELEASE
```

**English:** Circuit switching establishes a dedicated logical or physical communication path with reserved resources between communicating parties before data transfer. The same reserved route is then used during the connection, and resources are released at the end.

**Three phases:** (1) connection establishment; (2) data transmission; (3) connection termination.

**Advantages:** predictable service once established, reserved capacity, low per-unit switching uncertainty after setup, suitable for traditional continuous voice traffic. **Disadvantages:** setup delay, inefficient utilisation during silence/idle periods and inability to establish a new circuit when required capacity is unavailable. **Example:** traditional circuit-switched telephone systems.

### 12.4 Message switching - working, advantages, disadvantages

**Hinglish:** Imagine ek bada parcel warehouse A tak pahunchta hai. Warehouse poora parcel receive/store karta hai, phir next warehouse ko bhejta hai. Similarly message switching mein **entire message** store hota hai before forwarding. Agar message very large hai, intermediate node ko large storage chahiye aur delay badh sakta hai.

```text
  [SENDER] -> [STORE COMPLETE MESSAGE] -> [STORE] -> [RECEIVER]
                   NODE A                 NODE B
```

**English:** Message switching is a store-and-forward method in which the entire message is treated as a single unit. Each intermediate switch receives and stores the whole message, then forwards it when a suitable outgoing link becomes available. A dedicated end-to-end circuit is not reserved.

**Advantages:** capacity can be shared among users, messages can be queued and forwarded when links are free, no reserved circuit required. **Disadvantages:** potentially large storage requirements, high latency for long messages and poor suitability for real-time voice/video. **Example:** historical store-and-forward messaging systems.

### 12.5 Packet switching - working and forms

**Hinglish:** Long PDF ko network chhote packets mein break kar sakta hai. Har packet mein payload plus control information hoti hai. Router packet read karke next link par bhejta hai. Different users same network link share kar sakte hain. Traffic zyada ho to queueing delay ya packet loss possible hai, but available capacity ka utilisation generally efficient hota hai.

```text
 ORIGINAL MESSAGE:   [ AAAAAA | BBBBBB | CCCCCC ]

 PACKETS:            [H|A]    [H|B]    [H|C]

 SENDER ---> [ROUTER 1] ---> [ROUTER 2] ---> RECEIVER
                  \---------->[ROUTER 3]----/
          (routes depend on switching/routing design)
```

**English:** Packet switching divides transmitted information into smaller units called packets. Packets are forwarded through network devices, commonly using store-and-forward at each hop. Multiple users share network resources through statistical multiplexing. Packet networks avoid reserving a dedicated end-to-end circuit for every data exchange.

**Advantages:** flexible resource sharing, efficient for bursty data, scalability, support for alternative routes and many simultaneous users. **Disadvantages:** variable queueing delay, potential packet loss and out-of-order delivery, header overhead and need for congestion control or higher-layer recovery when required.

**Two conceptual packet-switching approaches:**

- **Datagram:** each packet is handled independently and carries destination information; routes can differ. **IP** is a familiar connectionless datagram-network example.
- **Virtual circuit:** a logical path is established first; packets follow an identified logical connection through the participating switches. Some classical technologies use virtual circuits.

**Important:** Neither "virtual circuit" nor "datagram" alone guarantees all reliability properties. These depend on the specific protocol/system.

### 12.6 Comparative table

| Feature | Circuit switching | Message switching | Packet switching |
|---|---|---|---|
| Setup | Usually required | No dedicated circuit | Datagram: no setup; VC: setup |
| Resource reservation | Yes for the connection | Normally no | Normally shared; QoS reservations are possible in some systems |
| Unit transferred | Continuous data stream over circuit | Entire message | Small packets |
| Intermediate storage | Not full-message store-and-forward | Entire message stored | Packets buffered/forwarded |
| Delay character | Setup delay then relatively predictable | Can be high for large messages | Variable with congestion |
| Efficiency for bursty traffic | May waste reserved capacity | Better sharing | Generally efficient |
| Common example | Traditional telephone calls | Historical message relays | Internet IP networking |

### 12.7 Small numerical example - store and forward

Suppose a message contains 12,000 bits and must cross two 1 Mbps links. Ignore propagation and processing.

- **Message switching:** at first intermediate node, the complete 12,000-bit message must arrive before forwarding. One link transmission = 12 ms. Two links = **24 ms** total.
- **Packet switching:** split into three packets of 4000 bits, and assume equally fast links with no headers or extra delay. Each packet takes **4 ms per link**. Packet 1 arrives at 8 ms, packet 2 at 12 ms and packet 3 at **16 ms**, because the two links can operate in a pipeline.

**Teaching conclusion:** Packetisation can reduce the completion time for a stream across store-and-forward links through pipelining. Actual performance also depends on packet headers, queueing and network conditions.

---

## 13. ISDN - Integrated Services Digital Network

### 13.1 Hinglish - old telephone system carrying digital services

ISDN ko samajhne ke liye pehle old telephone line imagine karo. Traditional analogue telephone systems voice ke liye designed the. ISDN ka idea tha telephone infrastructure ke through **digital voice, data aur other services** ko integrated form mein carry karna. Aaj broadband/fibre/IP technologies more common hain, but exam syllabus mein ISDN important theory topic hai.

**ISDN channels:**

- **B channel (Bearer):** 64 kbps; voice/data carry karne ke liye.
- **D channel (Delta/signalling):** signalling/control, and in some circumstances packet data; BRI mein 16 kbps, PRI mein generally 64 kbps.
- **H channel (High-rate):** higher-capacity channels, available in certain ISDN arrangements.

**BRI easy trick:** **2B + D** = two B channels x 64 kbps + one 16 kbps D channel = **144 kbps channel bit-rate total**, before considering extra line framing/overhead.

### 13.2 Complete English definition and objectives

**ISDN** stands for **Integrated Services Digital Network**. It refers to international telecommunications standards for digitally integrated voice, data and related services through a digital network interface. Its purpose was to offer multiple communication services through common digital telephone network infrastructure.

**Objectives/features:** integrated voice and data capabilities; digital transmission; standardised customer interfaces; separation of bearer and signalling channels; more flexible service combinations than conventional analogue telephone arrangements; and support for circuit-switched communication plus selected packet-oriented services.

### 13.3 ISDN channel types

| Channel | Typical rate | Main purpose |
|---|---:|---|
| B (Bearer) | 64 kbps | User voice/data |
| D (Signalling) | 16 kbps in BRI; 64 kbps in PRI | Call setup/control, some packet data |
| H (High-rate) | Various higher rates | Higher-bandwidth bearer services |

### 13.4 ISDN interfaces - BRI and PRI

**Basic Rate Interface (BRI):** Two 64 kbps B channels plus one 16 kbps D channel:

$$
2B + D = 2(64) + 16 = 144\;\text{kbps}
$$

This is the **sum of channel rates**, not necessarily the complete physical-line signalling rate. BRI supports simultaneous combinations such as two voice calls or voice plus data, subject to network equipment and service provisioning.

```text
            BRI = 2B + D
   +------------+------------+----------+
   | B1 64 kbps | B2 64 kbps | D 16 kbps|
   +------------+------------+----------+
     User data    User data    Signalling
```

**Primary Rate Interface (PRI):** Two well-known regional forms are:

- **E1-based PRI:** commonly **30B + D**, with 30 B channels x 64 kbps and a 64 kbps D signalling channel. The complete E1 line rate is **2.048 Mbps** (including framing/time slots).
- **T1-based PRI:** commonly **23B + D**, with a total T1 line rate of **1.544 Mbps**, including framing overhead.

The exact service arrangements may differ by region and provider, so state the standard variant in the answer.

### 13.5 ISDN architecture - simple block diagram

```text
   [ISDN TERMINAL / PHONE]
             |
      [TERMINAL ADAPTER]   (when required)
             |
   [NETWORK TERMINATION / NT]
             |
        DIGITAL LOCAL LOOP
             |
      [ISDN EXCHANGE / NETWORK]
             |
       [REMOTE SUBSCRIBER]
```

**Elements in classical descriptions:**

- **TE1:** terminal equipment designed for an ISDN interface.
- **TE2:** terminal equipment not directly ISDN-compatible, connected through a terminal adapter.
- **TA:** Terminal Adapter that supports connecting a non-ISDN terminal to the ISDN environment.
- **NT1/NT2:** network termination equipment providing physical/functional interface services according to the installation.

**Working:** (1) Terminal requests a connection; (2) signalling takes place, typically on a D channel; (3) network establishes the required communication service; (4) user voice/data uses B channel(s); (5) signalling ends/releases the connection when finished.

### 13.6 ISDN advantages and limitations

**Advantages:** digital connectivity, integrated voice/data support, standardised service interfaces, more predictable circuit-based communication and historically improved facilities compared with many analogue setups. **Limitations:** relatively low data rates by modern broadband standards, circuit/service charges, equipment complexity, and dependence on older telephone-network infrastructure. Modern fibre broadband and IP-based services have largely replaced ISDN in many settings.

**Short revision question:** What is BRI? **Answer:** ISDN Basic Rate Interface is 2B + D: two 64 kbps bearer channels and one 16 kbps signalling channel.

---

## 14. Terminal Handling

### 14.1 Hinglish - what is a terminal?

Imagine 1980s/1990s college computer centre mein one powerful mainframe computer hai. Multiple students us central computer ko screen+keyboard wale **terminals** se use kar rahe hain. Terminal local keyboard input bhejta hai; computer process karta hai; output screen par dikhta hai. Har user ke paas full independent powerful computer zaroori nahi. Aaj tum SSH se remote Linux server use karte ho - conceptually remote terminal access ka related modern example hai.

**Terminal handling** ka matlab hai multiple terminal devices/remote user sessions se input-output ko properly manage karna: kis user ka session hai, typed character kahan jayega, screen par kya display hoga, speed mismatch kaise handle hoga, errors aur line breaks kaise process honge.

### 14.2 Complete English definition

A **terminal** is an endpoint device or software interface through which a user communicates with a host computer or service. Historically terminals comprised a display and keyboard connected to a host or terminal server; a **terminal emulator** is modern software that provides similar text-based interaction. **Terminal handling** covers the management of communication, input, output, buffering, control, sessions and interfaces between a terminal and a host or communications network.

### 14.3 Categories of terminals

- **Dumb terminal:** mainly handles input and display output; relies heavily on host processing.
- **Intelligent/smart terminal:** has additional local processing, display control or data handling capabilities.
- **Virtual terminal / terminal emulator:** software representation of a terminal; examples include a system terminal window used to connect to a remote host.

### 14.4 DTE, DCE and terminal interfaces

**Hinglish:** User computer ya terminal data generate/consume karta hai: this is **DTE (Data Terminal Equipment)** in traditional communication terminology. Modem ya provider-facing communication device often acts as **DCE (Data Circuit-terminating Equipment)**, jo line/interface ke saath communication handle karta hai. Exact role depends on interface and standard.

```text
  [USER TERMINAL / PC] -- [MODEM or DCE] === COMMUNICATION LINE === [NETWORK]
          DTE                  DCE
```

**English:** DTE denotes equipment at the user/data-source side, while DCE denotes equipment that provides a communication interface to the transmission facility in classical serial communication systems. The DTE/DCE distinction helps explain signal timing, cabling and terminal-to-network interaction.

### 14.5 Main terminal handling functions

**1. Character input/output:** The system accepts keystrokes and displays characters or other responses. Special characters may be interpreted as commands rather than ordinary text.

**2. Buffering:** Data may be temporarily stored in memory to match different speeds of the keyboard, display, host and communication link.

**3. Flow control:** Mechanisms prevent the sender from sending faster than the receiver can handle. Classical examples include **software flow control** such as XON/XOFF and **hardware flow control** using appropriate interface signals.

**4. Echo control:** In **local echo**, the terminal displays typed input itself. In **remote echo**, the remote host sends back the display information. Incorrect settings may cause double characters or no visible characters.

**5. Line discipline and editing:** Input may be processed one character at a time or line by line; backspace, Enter, editing and control characters may have special meaning.

**6. Session control:** A host may need to identify the user, establish a logical session, maintain state and close access on logout or disconnection.

**7. Multiplexing/concentration:** A terminal server or concentrator can connect multiple terminals to a common host or communication system, sharing expensive links efficiently.

**8. Transmission timing and mode:** Serial communications may require agreement on speed, character framing, parity settings and full-/half-duplex behaviour; these details depend on the specific terminal/link technology.

**9. Error handling:** Corrupted input, timeout, loss of connectivity and session termination require appropriate detection and recovery behaviour.

### 14.6 Terminal handling diagram and working

```text
 [TERMINAL A] --\
 [TERMINAL B] ---[TERMINAL SERVER / CONCENTRATOR]---[HOST SYSTEM]
 [TERMINAL C] --/             |                       |
                         Session/control        Applications
                         Buffers and I/O        Data processing
```

**Step-by-step:** (1) User enters input through keyboard or terminal emulator; (2) terminal interface converts input into an agreed representation; (3) communication interface transmits input to the terminal server or host; (4) host validates session and processes command; (5) results are sent back; (6) terminal displays the output; (7) buffering, flow control and terminal state handle speed variations and line disruptions.

### 14.7 Terminal handling advantages and limitations

**Advantages:** centralised resource use, support for multiple users on one host, easier central application maintenance and the ability to access remote computing resources. **Limitations:** dependent on host/network availability; high user load can create bottlenecks; interface/encoding differences can lead to display problems; insecure remote-terminal mechanisms can expose credentials and data.

**Exam caution:** Do not confuse **terminal handling** with **network topology**. A terminal is a user access endpoint; topology describes the arrangement of connections. Also distinguish a software terminal emulator from a physical monitor or from the transport-layer TCP connection.

---

## 15. Last-Minute Unit I Revision - Differences and Diagrams

### 15.1 Five very important differences

**Bandwidth vs throughput:** Bandwidth is capacity; throughput is actual successful delivery rate. A 100 Mbps link may achieve lower application throughput.

**Transmission vs propagation delay:** Transmission is the time required to push L bits onto a link (L/R); propagation is the time required for a signal to travel distance d (d/v).

**Switch vs router:** An Ethernet switch typically forwards frames within a LAN using MAC addresses; a router forwards IP packets between networks using network-layer information.

**Physical vs logical topology:** Physical topology concerns actual links and wiring; logical topology concerns how traffic travels or is organised.

**Guided vs unguided media:** Guided media use a physical guiding structure (twisted pair/coax/fibre); unguided media use free-space electromagnetic waves (radio/microwave/infrared/satellite).

### 15.2 Diagrams you should practise by hand

1. College LAN - hosts, access switch, shared printer, router and Internet.
2. OSI model - all seven layers with one function each.
3. Encapsulation - data, segment, IP packet, frame and bits.
4. Topologies - bus, star, ring, mesh and tree.
5. Nodal delays - processing, queueing, transmission and propagation.
6. Campus backbone - access, distribution and core.
7. Twisted pair, coaxial cross-section and optical fibre cross-section.
8. Circuit switching versus message switching versus packet switching.
9. ISDN BRI (2B + D) channel diagram.
10. Terminal server/concentrator linking several terminals to a host.

\newpage

# PART B - IMPORTANT 20-MARK QUESTIONS WITH COMPLETE MODEL ANSWERS

> **Exam status:** These are **high-priority practice questions inferred from the uploaded Unit I syllabus**, not verified previous-year questions or guaranteed examination questions. For a 20-mark answer, write a clear introduction, a labelled diagram, main explanation under headings, examples, advantages/limitations where relevant, and a conclusion. The English model answer is what you can adapt in your answer sheet; the short Hinglish reminder tells you the core idea before writing.

## Q1 (20 marks). Define a computer network. Explain its goals, applications, network structure and client-server and peer-to-peer architectures.

**Hinglish understanding:** Network ko shared college computer lab samjho. Ek printer, common notes server, Internet aur 40 PCs sab connected. Goals = kyun network chahiye (cost, sharing, communication). Applications = kaha use hota hai (banking, education, hospital). Architecture = client-server ya P2P ka arrangement.

### Model answer (English)

**Introduction and definition:** A computer network is a collection of two or more interconnected computers or digital devices that exchange information and share resources through communication links. The devices communicate using protocols, which are sets of rules governing transmission, addressing, message formats, timing and error handling. A college lab LAN is a small network, whereas the Internet is a worldwide network of networks.

**Basic components:** Every data communication arrangement needs a **sender**, a **receiver**, a **message**, a **transmission medium** and a **protocol**. Hosts create/use the information, while switches and routers forward data within and between networks.

```text
 [STUDENT PC-1] ---\
 [STUDENT PC-2] ----[LAN SWITCH]----[ROUTER]----[INTERNET]
 [TEACHER PC] ----/     |                        |
                         |                  [WEB SERVER]
                      [PRINTER]
```

**Goals of computer networks:**

1. **Resource sharing:** A single printer, file server or database can be used by many networked devices. This reduces hardware duplication.
2. **Data and information sharing:** Shared files, repositories and databases let users access common information. For example, students download the latest assignment from a central server.
3. **Communication:** Messaging, email, voice/video conferencing and collaborative tools permit rapid exchange of information.
4. **Cost reduction:** Common storage, Internet access, administration and software facilities can reduce per-user expenses.
5. **Reliability:** Backup devices and multiple communication paths can help maintain service during equipment failure. Such reliability must be planned, not assumed.
6. **Scalability:** Additional devices or departments can be connected as the organisation grows.
7. **Remote access and management:** Authorised users can reach resources from outside the campus; administrators can manage shared users, permissions and systems.

**Applications:**

- **Education:** learning management systems, online examination portals, digital libraries and lecture delivery.
- **Banking:** online banking, ATM communication and digital transactions.
- **Business:** order processing, ERP, email and inter-branch information sharing.
- **Healthcare:** hospital information systems, electronic health records and remote consultations.
- **E-commerce:** browsing products, order tracking, payments and customer support.
- **Entertainment:** video streaming, multiplayer games, media sharing.
- **Government and research:** electronic public services, scientific data and distributed computing.
- **Home networks:** smart TVs, Wi-Fi printers and connected devices.

**Network structure:** A network commonly has the **edge**, where end-user applications and servers operate; the **access network**, which connects those devices; and the **core/backbone**, where routers and high-capacity links interconnect networks. The edge produces/consumes application data. The core forwards data through one or more paths toward its destination. Network protocols govern communication at different levels.

**Client-server architecture:** In the client-server model, a server provides a central service and clients request access to it. For example, a student's browser (client) sends a request to the university result server, which returns data. The model supports central administration, access control and backup, but dependence on a central server can cause bottlenecks or outages unless the service is replicated.

**Peer-to-peer architecture:** In a peer-to-peer network, participants can act as both consumers and providers of resources. A peer may request pieces of a file and also share pieces with other peers. This can distribute traffic and storage, but makes administration, availability and security more challenging.

**Comparison:** Client-server architecture usually concentrates management and services on servers, whereas peer-to-peer architecture distributes service roles among participants. The appropriate choice depends on application requirements and operational conditions.

**Conclusion:** Computer networks enable communication, sharing and distributed services. Their success depends on suitable architecture, capacity, security and maintenance. Organisations select network structures according to number of users, geography, expected applications, available resources and reliability requirements.

---

## Q2 (20 marks). Explain the OSI reference model in detail with a neat diagram. Explain encapsulation and compare OSI with the TCP/IP model.

**Hinglish understanding:** OSI seven kaam ko separate karta hai: application se user request aati hai; presentation format; session conversation; transport process-to-process; network routing; data link frame; physical actual bits. Sender par headers add hote hain aur receiver par remove. TCP/IP practical Internet suite hai, OSI reference map.

### Model answer (English)

**Introduction:** The Open Systems Interconnection (OSI) reference model is a seven-layer conceptual model for describing communication between networked systems. Standardised layering simplifies understanding, interoperability and network design. Each layer performs specified functions, provides services to the layer above, and uses the services of the layer below.

```text
 +-----------------------------------+
 | 7. APPLICATION  - User services    |
 +-----------------------------------+
 | 6. PRESENTATION - Data representation|
 +-----------------------------------+
 | 5. SESSION      - Dialogue/session |
 +-----------------------------------+
 | 4. TRANSPORT    - End-to-end delivery|
 +-----------------------------------+
 | 3. NETWORK      - Routing, IP      |
 +-----------------------------------+
 | 2. DATA LINK    - Frames and MAC   |
 +-----------------------------------+
 | 1. PHYSICAL     - Bits and signals |
 +-----------------------------------+
```

**Layer 1 - Physical layer:** The Physical Layer transmits raw bits as electrical, optical or radio signals through a communication medium. It specifies signal levels, connectors, interfaces, line coding, timing and transmission rate. Examples are copper Ethernet signalling, optical-fibre transmission and repeater behaviour. Its data unit is the **bit**.

**Layer 2 - Data Link layer:** This layer handles communication over a single link/local network. Its responsibilities include framing, MAC addressing, error detection, access to the shared medium where necessary, and sometimes link-level flow control or recovery. Ethernet switches operate primarily with link-layer frames and MAC addresses. Its unit is the **frame**.

**Layer 3 - Network layer:** The Network Layer supports communication across multiple interconnected networks. It performs logical addressing, route determination and packet forwarding. Routers select the next hop for packets using routing/forwarding information. IPv4 and IPv6 are major network-layer technologies. Its unit is the **packet**.

**Layer 4 - Transport layer:** This layer provides communication between application processes on end hosts. It identifies processes using transport endpoints such as port numbers, divides application data into segments, reassembles it and may provide flow control, error recovery and congestion response. TCP provides reliable, ordered byte-stream delivery, while UDP provides a simpler datagram service. Its units include the **segment** (TCP) and **datagram** (UDP).

**Layer 5 - Session layer:** The Session Layer is concerned with starting, maintaining and closing logical exchanges between applications. It also includes dialogue control and synchronisation/checkpoints as conceptual functions. A session can identify an ongoing exchange and organise continuation after some interruptions, depending on the application.

**Layer 6 - Presentation layer:** This layer deals with the representation of exchanged data, allowing different systems to understand information. Its conceptual functions include format translation, character encoding, serialisation, data compression and encryption/decryption. For example, the receiver must interpret text in the appropriate representation, and compressed data must be decompressed before use.

**Layer 7 - Application layer:** The Application Layer provides network services used by application software. It includes application-level access to web resources, file transfer, email and name resolution. HTTP, SMTP, FTP and DNS are familiar Internet application protocols.

**Encapsulation:** When data moves down the sender's stack, control information is added for the responsibilities of successive protocols. For example:

```text
 Application:            [ DATA ]
 Transport:          [TCP H|DATA]                SEGMENT
 Network:        [IP H|TCP H|DATA]              PACKET
 Data Link:    [MAC H|IP H|TCP H|DATA|FCS]      FRAME
 Physical:        101010... electrical/light/radio signals
```

At the destination, **decapsulation** removes the protocol information in reverse order and delivers the content to the receiving application. At routers, data-link framing is generally replaced for each outgoing link, while the network-layer packet is forwarded toward its destination.

**OSI vs TCP/IP comparison:**

| OSI model | Common TCP/IP model |
|---|---|
| Seven distinct layers | Four broad layers in common textbook version |
| Application, Presentation, Session | Combined Application |
| Transport | Transport - TCP/UDP |
| Network | Internet - IP |
| Data Link and Physical | Link / Network Access |
| Conceptual reference for networking | Practical suite used on the Internet |

**Advantages of OSI:** clear separation of responsibilities, easier troubleshooting, more organised standardisation and simpler learning. **Limitations:** real protocols do not always map neatly into exactly seven layers, and some responsibilities overlap or are implemented together.

**Conclusion:** OSI is an important conceptual model for understanding communication, while TCP/IP provides the practical protocols used by the Internet. Knowing their layers, service roles and encapsulation process is essential for network design and troubleshooting.

---

## Q3 (20 marks). What is network topology? Explain bus, star, ring, mesh, tree and hybrid topologies with diagrams, advantages and disadvantages.

**Hinglish understanding:** Topology = connection ka shape. Bus ek cable; star centre switch; ring circular; mesh many alternate paths; tree branches; hybrid mixture. Diagram + comparison exam mein high scoring.

### Model answer (English)

**Definition:** Network topology is the physical or logical arrangement of devices and communication links. Physical topology describes cable and device connections, whereas logical topology describes communication paths or traffic organisation. The topology selected affects cost, speed, fault tolerance, maintenance, expansion and installation complexity.

**1. Bus topology:** In a bus topology, stations share a common backbone link. Data transmitted on the shared medium is accessible along that backbone, and classic systems require appropriate termination.

```text
  A      B      C      D
  |      |      |      |
--+------+------+------+-- Shared main cable
```

**Advantages:** economical cabling for a small legacy deployment, simple basic concept. **Disadvantages:** the shared medium creates contention; backbone damage affects communication; fault location and expansion can be difficult. **Example:** historical shared coaxial Ethernet.

**2. Star topology:** Every station has a separate link to a central hub or switch.

```text
             A
             |
       B --- SW --- C
             |
             D
```

**Advantages:** easy addition/removal of hosts, easy monitoring and fault isolation, and an individual access-link failure normally affects only one host. **Disadvantages:** failure of the central device affects connected hosts; more cable and central hardware are needed. **Example:** modern switched Ethernet office/lab.

**3. Ring topology:** Nodes form a closed loop, each connected to its neighbour. In token-based systems, possession of a token controls transmission.

```text
      A ----- B
      |       |
      D ----- C
```

**Advantages:** orderly communication and predictable access in suitable token-ring implementations. **Disadvantages:** single-ring designs may be affected by one break and adding/removing nodes can be complex. Dual-ring technologies can add protection. **Example:** legacy Token Ring and FDDI concepts.

**4. Mesh topology:** Nodes are connected by several point-to-point links. In full mesh, each node has a direct link to every other node; partial mesh contains selected links only.

```text
        A ----- B
        | \   /|
        |  \ / |
        |  / \ |
        | /   \|
        D ----- C
```

**Advantages:** redundancy, multiple alternative paths and good resilience to individual link failures. **Disadvantages:** high cabling/interface costs and complicated installation. For **n** nodes, full-mesh links = **n(n-1)/2**. Example: 6 routers need 6 x 5 / 2 = **15 links**.

**5. Tree topology:** Devices are connected in levels with branches extending from a root or core.

```text
             CORE
           /      \
      SWITCH-A  SWITCH-B
       /   \      /  \
      PC   PC    PC  PC
```

**Advantages:** hierarchical growth, organised management and easy departmental grouping. **Disadvantages:** higher-level equipment can be a common failure point and careful uplink sizing is necessary. **Example:** campus LAN.

**6. Hybrid topology:** A hybrid combines different topologies, for example, stars within different buildings connected through a protected mesh or hierarchical backbone. **Advantages:** flexibility and ability to optimise different parts for their traffic and reliability requirements. **Disadvantages:** complex design, administration and cost planning.

**Comparative overview:**

| Topology | Strongest advantage | Main limitation |
|---|---|---|
| Bus | Low cabling in older small networks | Shared-backbone vulnerability |
| Star | Easy fault isolation | Central switch dependency |
| Ring | Orderly access | Ring failure unless protected |
| Mesh | Alternative paths | High cost and complexity |
| Tree | Good hierarchical scaling | Core/branch dependency |
| Hybrid | Flexible design | More complicated management |

**Factors affecting selection:** number of nodes, traffic load, maximum distance, available cable, budget, redundancy requirement, physical building layout, security, administration and future expansion. For a college laboratory, switched star is normally a practical access-network choice. For an important campus backbone, redundancy may be more valuable than minimum cable cost.

**Conclusion:** There is no universally best topology. A network designer must balance efficiency, cost, reliability and growth, often combining access stars with hierarchical or redundant backbone connections.

---

## Q4 (20 marks). Explain delay analysis in computer networks. Describe all delay components and solve a numerical problem.

**Hinglish understanding:** Four delays = packet ko check karna, queue mein wait, bits ko link par push karna, signal ka distance travel karna. Formulas **L/R** and **d/v** pakke yaad rakhna.

### Model answer (English)

**Definition:** Delay, or latency, is the time needed for data to travel from a sender to a receiver. Network performance is affected not only by raw link capacity but also by device processing, packet waiting time and propagation across physical distances. At a typical intermediate device, nodal delay is divided into four components:

$$
D_{nodal}=D_{processing}+D_{queueing}+D_{transmission}+D_{propagation}
$$

**1. Processing delay:** The time needed by a router or network node to examine packet information, perform error-related checks, look up forwarding information and select the outgoing interface. Faster processing equipment and simpler operations can reduce this component.

**2. Queueing delay:** Packets may have to wait in a buffer when the outgoing link is busy. The waiting time depends on traffic arrival patterns, competing traffic, link capacity, scheduling rules and buffer availability. This delay can rise sharply during congestion and is often the most variable component.

**3. Transmission delay:** The time required for a transmitting interface to place the packet's bits onto the link is:

$$
D_{transmission}=\frac{L}{R}
$$

where **L** is packet size in bits and **R** is link transmission rate in bits per second. Increasing R reduces transmission delay for a fixed packet size. Increasing packet size increases transmission delay at a fixed rate.

**4. Propagation delay:** The time required for a signal to travel across the medium is:

$$
D_{propagation}=\frac{d}{v}
$$

where **d** is physical distance in metres and **v** is the propagation speed through the medium in metres per second. This is not the same as transmission delay: even a high-capacity link cannot remove the physical travel time of signals.

```text
    Source ---(Transmission + Propagation)---> Router ---> Destination
                                                |
                                         Processing + Queueing
```

**Solved numerical:** A packet has a size of 2000 bytes. The link rate is 8 Mbps, the distance is 400 km and the signal propagation speed is 2 x 10^8 m/s. At the node, processing delay is 0.3 ms and queueing delay is 1.2 ms. Find the total nodal delay.

**Given:** L = 2000 x 8 = **16,000 bits**; R = **8,000,000 bps**; d = 400 x 1000 = **400,000 metres**; v = **200,000,000 m/s**.

**Transmission delay:** 16,000 / 8,000,000 = 0.002 s = **2 ms**.

**Propagation delay:** 400,000 / 200,000,000 = 0.002 s = **2 ms**.

**Total:** 0.3 + 1.2 + 2 + 2 = **5.5 ms**.

**Related terms:**

- **Bandwidth:** the maximum configured/theoretical transmission rate of a link.
- **Throughput:** actual successful data delivery rate.
- **Round-trip time (RTT):** time for an outward and return exchange, including relevant waiting/processing.
- **Jitter:** variation in end-to-end delay, significant for real-time audio/video.
- **Bandwidth-delay product:** bits potentially in transit, calculated as bit rate multiplied by a chosen delay (often RTT for a window/pipe illustration).

**Factors that increase delay:** small link capacity, large packets, long physical distances, overloaded routers, congested shared links, excessive retransmission and poorly designed routing. **Ways to improve:** suitable capacity planning, improved routes, prioritisation for time-sensitive traffic, congestion management, hardware capacity and reducing unnecessary hops where appropriate.

**Conclusion:** Total communication delay is the sum of several independent and traffic-dependent effects. A good designer analyses all components rather than assuming that higher bandwidth alone guarantees an instant response.

---

## Q5 (20 marks). Explain backbone network design and local access network design. Design a network for a college campus.

**Hinglish understanding:** Access network users ko connect karti hai, backbone departments/buildings ko. College campus ka diagram bana kar count -> topology -> media -> uplink -> addressing -> security -> redundancy -> testing explain karo.

### Model answer (English)

**Introduction:** A well-designed campus network must connect individual computers efficiently while also interconnecting different departments and buildings at suitable capacity. The **local access network** connects end-user devices such as desktops, printers, telephones and mobile devices. The **backbone network** interconnects major LANs, distribution devices and campus facilities through high-capacity paths.

**Backbone network design:** A backbone is the central connection infrastructure carrying combined traffic between different portions of an organisation. Its objectives are high-capacity communication, reliable interconnection, manageable traffic flow and future expansion. It may include core routers/switches, fibre links, distribution equipment, backup paths and monitoring systems.

Important backbone architectures include **serial** (devices in a chain), **hierarchical** (access/distribution/core), **collapsed core** (combined core/distribution at a smaller site) and **redundant/meshed** forms (multiple alternative paths). A chain may be simple but fail at several dependent points. A hierarchical backbone is easier to organise, while a redundant design improves availability if paths and equipment are properly independent.

**Local access network design:** Access networks provide the immediate connection from computers and mobile devices to an organisation's network. Typical components are Ethernet access switches, Wi-Fi access points, NICs, copper/fibre cables and local uplinks. Wired Ethernet is usually well suited to stationary laboratory machines; Wi-Fi serves laptops and phones that need mobility.

**Proposed campus scenario:** A university has a BCA laboratory with 60 PCs, a library with 30 computers and an administrative building with 25 computers, plus mobile-device access for students. The institution needs central records, library services, shared Internet and room for expansion.

```text
             [ ISP / INTERNET ]
                      |
               [FIREWALL/ROUTER]
                      |
              [CAMPUS CORE SWITCH]
                /      |       \
       FIBRE  /      FIBRE      \ FIBRE
            /          |         \
       [BCA DIST]  [LIB DIST]  [ADMIN DIST]
          /   \         |           |
    [LAN SW] [AP]   [LAN SW]     [LAN SW]
     / |  \          / | \         / | \
    PC PC  PC        PC PC PC      PC PC PC
```

**Design procedure:**

1. **Requirements:** Count all computers, printers, access points, servers and users. Identify applications such as attendance, downloads, video lectures and records.
2. **Topology:** Use switched star access within labs; connect the building distributions to a suitable central/core network.
3. **Media:** Choose twisted-pair Ethernet for appropriate short desk-to-switch links and fibre links for the inter-building backbone when their capacity/distance characteristics suit the site.
4. **Capacity:** Select switch ports, uplinks and core bandwidth based on the expected concurrent traffic, not the largest individual Internet-plan number.
5. **Addressing and segmentation:** Plan IP networks, DHCP/name services and isolation of student, staff, guest and critical administrative traffic where necessary.
6. **Wireless coverage:** Position access points after evaluating users, obstacles, channel interference and coverage.
7. **Security:** Use authenticated access, strong administrative credentials, firewalls, device maintenance, monitoring and backups.
8. **Fault tolerance:** Provide redundant power, core links or devices for services whose outage would be unacceptable. Check whether redundant paths are physically independent.
9. **Administration:** Label ports and cables, document equipment locations and keep configuration backups.
10. **Testing:** Verify printing, access to servers, Internet access, wireless coverage, performance under load and expected failover behaviour.

**Difference between access and backbone:** Access connects individual user devices; backbone connects access networks and departments. Access planning emphasises endpoint coverage and local switching, while backbone planning emphasises aggregation, resilience and interconnection.

**Advantages of this design:** easy expansion, organised department segmentation, strong local connectivity, central management and the option to provide protected inter-building communication. **Limitations:** higher initial cost than an ad hoc network, dependence on central systems without redundancy and ongoing administration requirements.

**Conclusion:** A robust campus network combines practical access networks with a well-sized and resilient backbone. Access and backbone must be designed together because traffic from all users ultimately flows through shared aggregation and external links.

---

## Q6 (20 marks). Explain guided and unguided transmission media with diagrams, advantages, disadvantages and applications.

**Hinglish understanding:** Guided = cable follow karne wale signals: copper pair, coax, optical fibre. Unguided = hawa mein signals: radio, microwave, infrared, satellite. Compare distance, interference, cost aur use-case.

### Model answer (English)

**Introduction:** Transmission media are the paths that carry communication signals between devices. The Physical Layer transforms bits into suitable signals and transfers them through an available medium. Transmission media are commonly classified as **guided** (wired) and **unguided** (wireless).

**Guided transmission media** guide signals along a physical structure:

**1. Twisted-pair cable:** It consists of insulated copper conductors twisted in pairs, reducing electromagnetic interference and crosstalk. Unshielded twisted pair (UTP) and shielded variants are used depending on the environment. **Advantages:** inexpensive, flexible, widely available, easily installed. **Disadvantages:** relatively susceptible to interference and distance limitations compared with fibre. **Applications:** wired Ethernet local networks, telephone wiring.

```text
 Two insulated copper conductors twisted together:
  /\  /\  /\  /\
  \/  \/  \/  \/
```

**2. Coaxial cable:** It has a central conductor, dielectric insulation, a metallic outer conductor/shield and a protective jacket. The shield reduces interference. **Advantages:** good shielding and useful high-frequency characteristics. **Disadvantages:** bulkier, less convenient for many modern office-LAN deployments. **Applications:** cable television, cable Internet and older shared Ethernet systems.

```text
 [Outer jacket]
    [Metallic shield]
       [Dielectric]
          [Core conductor]
```

**3. Optical fibre:** It carries light through a core surrounded by cladding and protective materials. Optical guiding depends on the refractive properties of the core and cladding. **Single-mode fibre** supports long-distance/high-capacity systems; **multimode fibre** is frequently used over shorter distances. **Advantages:** high capacity, low interference, electrical isolation and good long-distance suitability. **Disadvantages:** specialised transceivers/splicing and care during physical installation. **Applications:** campus backbones, telecom networks and data-centre interconnections.

```text
  [JACKET][BUFFER][CLADDING][LIGHT-CARRYING CORE]
```

**Unguided transmission media** carry electromagnetic waves through space:

**1. Radio waves:** Used for mobile and wireless communication. They allow mobility and broad coverage depending on the system, but face interference and shared-spectrum limitations. **Applications:** Wi-Fi, Bluetooth and radio services.

**2. Microwaves:** Directional high-frequency links often connect rooftops or towers. They can be installed without trenching fibre, but may require line of sight and are affected by obstacles and some weather conditions. **Applications:** point-to-point terrestrial links.

**3. Infrared:** Suitable for short-range line-of-sight or reflected links. It is easily blocked by obstacles, but works well for simple nearby controls. **Application:** television remote control.

**4. Satellite communication:** Earth stations communicate through a spacecraft using uplink and downlink radio connections. It provides wide geographic reach, especially in remote areas, but equipment/service cost and latency can be important. Propagation time depends strongly on the satellite orbit.

```text
 [EARTH STATION A] ---> [SATELLITE] ---> [EARTH STATION B]
          Uplink                           Downlink
```

**Comparison:**

| Guided media | Unguided media |
|---|---|
| Uses a physical guide such as copper/fibre | Uses air/space electromagnetic propagation |
| Often gives stable, controlled wired links | Supports mobility and rapid wireless access |
| Installation requires laying/terminating cable | Wireless planning requires antenna/spectrum/coverage work |
| Fibre is highly immune to EMI | Radio links may face environmental interference |
| Examples: twisted pair, coaxial, fibre | Examples: radio, microwave, infrared, satellite |

**Selection criteria:** required speed, geographical distance, environmental noise, budget, user mobility, installation restrictions, security, maintenance and scalability. For a college computer lab, copper Ethernet might connect desktops; fibre may interconnect buildings; and Wi-Fi provides mobility to phones and laptops.

**Conclusion:** Guided and unguided media serve different needs. A good network often combines several media instead of selecting one medium for every part of the system.

---

## Q7 (20 marks). Describe circuit switching, message switching and packet switching with their working, diagrams and comparison.

**Hinglish understanding:** Circuit = dedicated reserved road. Message = poora parcel each centre par store. Packet = message ko pieces mein break karke shared network par forward. Packet switching modern data networks ka key idea hai.

### Model answer (English)

**Introduction:** Switching is the method of directing information through intermediate network nodes toward a receiver. Three classical methods are **circuit switching**, **message switching** and **packet switching**. They differ in whether a dedicated path is reserved, the size of the transmitted unit and how intermediate nodes store or forward information.

**Circuit switching:** A dedicated end-to-end communication circuit or reserved resource arrangement is established before transferring data. It generally has three stages: **connection establishment**, **data transfer**, and **connection release**. Once established, the path/resources remain allocated throughout the connection, even while a participant is not sending data.

```text
 A --- [SW1] ===== [SW2] ===== [SW3] --- B
           Dedicated reserved circuit
```

**Advantages:** predictable reserved transmission capacity and suitable characteristics for traditional continuous voice calls. **Disadvantages:** initial setup time, possible call blocking if resources unavailable and inefficient use of reserved capacity during silent periods. **Application:** conventional telephone circuit networks.

**Message switching:** The entire message is received and stored by an intermediate node before forwarding. No dedicated end-to-end circuit is established. Intermediate systems queue messages and forward them when links become available.

```text
 A --> [Node 1: store whole message] --> [Node 2: store] --> B
```

**Advantages:** communication resources can be shared; messages can wait for available links. **Disadvantages:** large storage space may be required, and end-to-end delay can be significant for large messages. **Application:** classical store-and-forward messaging systems.

**Packet switching:** The data is divided into smaller packets. Each packet includes control information necessary for its delivery and is forwarded through a set of network nodes. Network resources are shared among many users, often through statistical multiplexing. A router may receive and buffer one packet while forwarding other packets.

```text
 Message:       [AAAAAAAA|BBBBBBBB|CCCCCCCC]
 Packetised:    [H|AAAA] [H|BBBB] [H|CCCC]

 A ---> [R1] ---> [R2] ---> B
          \------>[R3]-----/
```

Two major concepts are **datagram packet switching** and **virtual-circuit packet switching**. A datagram network treats each packet independently, and packets may take different paths. A virtual-circuit network establishes a logical path first and forwards packets through that circuit identifier. IP is a familiar datagram-network technology.

**Advantages:** efficient sharing for bursty computer traffic, scalable use of links, flexibility and support for many users. **Disadvantages:** possible congestion, queueing delays, packet loss, varying arrival time, and overhead from packet headers and handling.

**Comparison table:**

| Criterion | Circuit | Message | Packet |
|---|---|---|---|
| Dedicated path | Reserved | No | Not needed for datagram; VC has logical path |
| Data handling | Continuous over reserved connection | Whole message | Small packets |
| Storage at intermediate node | Not full-message based | Whole message stored | Individual packets buffered |
| Delay | Setup plus service delay | Large/variable for long messages | Variable and congestion dependent |
| Bursty traffic efficiency | Often poor | Shared links | Generally good |
| Example | Traditional telephony | Historical messaging | Internet |

**Worked comparison:** A 12,000-bit message crosses two 1 Mbps store-and-forward links. If transmitted as one complete message, its two-link completion time is 12 ms + 12 ms = **24 ms**, ignoring other delays. If it is divided into three 4000-bit packets, each packet needs 4 ms per link. With pipelining, packets arrive at 8 ms, 12 ms and **16 ms**; the whole message finishes at 16 ms under the simplified assumptions. The example demonstrates why packetisation can help pipeline transmission over multi-hop paths.

**Conclusion:** Switching choice depends on traffic characteristics, resource sharing and quality requirements. Circuit switching was central to traditional telephony, while packet switching forms the basis of modern Internet data networks.

---

## Q8 (20 marks). What is ISDN? Explain BRI, PRI, channels, architecture and working. Also explain terminal handling.

**Hinglish understanding:** ISDN = old digital telephone technology with voice+data. B = bearer user data, D = signalling. BRI 2B+D and PRI E1 30B+D/T1 23B+D. Terminal handling = keyboard/screen or remote session ke input-output, buffering, echo, flow control manage karna.

### Model answer (English)

**Introduction to ISDN:** Integrated Services Digital Network (ISDN) is a collection of digital telecommunications standards designed to carry voice, data and related services through an integrated digital network interface. It helped extend telephone networks from analogue-only service arrangements to standardised digital service facilities.

**ISDN features:** integrated services, digital interfaces, standardised terminal/network communication, separation of user bearer traffic and signalling, and support for different call/service arrangements.

**Channels:** The **B (Bearer) channel** typically operates at **64 kbps** and carries voice or user data. The **D channel** carries signalling/control information and, depending on the arrangement, certain packet-oriented data; it is **16 kbps in BRI** and normally **64 kbps in PRI**. **H channels** provide higher-capacity bearer arrangements in applicable services.

**BRI - Basic Rate Interface:** Contains **2B + D**, i.e., two 64 kbps bearer channels plus one 16 kbps D channel. Sum of channel rates = 64 + 64 + 16 = **144 kbps**. This channel total should not be confused with all framing and physical-line overhead.

```text
 ISDN BRI:  [B1 - 64 kbps] [B2 - 64 kbps] [D - 16 kbps]
            User traffic    User traffic    Signalling
```

**PRI - Primary Rate Interface:** In an E1-based arrangement, PRI commonly uses **30 B channels + one D channel**, with an E1 physical line rate of **2.048 Mbps** including its framing structure. In a T1-based arrangement, PRI commonly uses **23 B + one D channel**, with a T1 physical line rate of **1.544 Mbps**. The regional form must be specified rather than mixing channel counts.

**Typical ISDN components:** **TE1** (ISDN-compatible terminal equipment), **TE2** (non-ISDN terminal), **TA** (terminal adapter for a TE2), and **NT** (network termination functions). These connect customer equipment to the digital local loop and telecommunications exchange.

```text
 [TE2 Terminal] -- [Terminal Adapter] -- [NT] == Digital local loop == [ISDN Exchange]
```

**ISDN working:** (1) The terminal initiates a service request; (2) call/service signalling goes through the D channel; (3) the network establishes a bearer connection/service; (4) voice or data transfer takes place over B channel(s); and (5) signalling and resources are released when communication ends.

**ISDN advantages:** standardised digital voice/data integration, multiple simultaneous channels in supported arrangements and historically improved digital service capabilities. **Limitations:** low rates compared with modern broadband, equipment and service complexity and dependence on legacy telephone infrastructure. IP-based broadband has replaced it in many deployments.

**Terminal handling:** A terminal is a user interface to a computer or host system, historically a keyboard/display device and today often represented through a software terminal emulator. Terminal handling means organising the communication of user input and host output, while controlling sessions, display behaviour and communication timing.

**Functions of terminal handling:**

1. **Input/output processing:** accept typed characters and display responses.
2. **Character encoding and line discipline:** manage character representation, Enter, Backspace and line editing.
3. **Buffering:** temporarily store input/output to handle different processing speeds.
4. **Flow control:** prevent overrunning a receiver, using suitable hardware signalling or software techniques like XON/XOFF in classic serial systems.
5. **Echo control:** decide whether typed characters are displayed locally or returned by the remote host.
6. **Session management:** open, maintain, authenticate where relevant and close user sessions.
7. **Terminal concentration:** connect several terminals to a host or communication system through a terminal server/concentrator.
8. **Error and disconnection handling:** respond to communication interruptions and invalid data where supported.

```text
 [TERM-1] --\
 [TERM-2] --- [TERMINAL SERVER] ----- [HOST COMPUTER]
 [TERM-3] --/        Buffer / I-O       Programs / Data
```

**DTE/DCE connection:** Classical terminology distinguishes **DTE (Data Terminal Equipment)**, which originates/receives user data, from **DCE (Data Circuit-terminating Equipment)**, which interfaces with the communication circuit. A terminal connected through a modem is a common conceptual example.

**Advantages and limitations of remote terminals:** Terminals allow multiple users to access central resources with limited local processing, but the host/network may become a bottleneck and remote sessions must be properly secured.

**Conclusion:** ISDN represents a major stage in digital telecommunication integration, while terminal handling deals with how users interact reliably with shared or remote computers. Both topics explain the evolution of network access and digital communication.

\newpage

# PART C - IMPORTANT SHORT QUESTIONS, ANSWERS & PRACTICE

## 16. Very Important 2-Mark Questions (answers included)

1. **Define a computer network.** A computer network is an interconnection of computers/devices that exchange data and share resources through communication links following common protocols.
2. **What is a protocol?** A protocol is an agreed set of rules that governs how devices format, send, receive and interpret messages.
3. **What is a node?** A node is a device/connection point in a network, such as a host, switch or router.
4. **Differentiate LAN and WAN.** A LAN covers a limited local area; a WAN interconnects geographically separated networks over a wider area.
5. **What is network architecture?** The organisation of networking devices, functions, protocols, interfaces and their communication relationships.
6. **What does OSI stand for?** Open Systems Interconnection.
7. **Name all seven OSI layers.** Physical, Data Link, Network, Transport, Session, Presentation and Application (bottom to top).
8. **Which layer performs routing?** The Network Layer is responsible for logical addressing and packet routing/forwarding.
9. **Which layer uses frames?** The Data Link Layer.
10. **What is encapsulation?** Addition of relevant control headers/trailers as data moves down protocol layers at the sender.
11. **What is a star topology?** A topology in which each device is individually linked to a central connection device such as a switch.
12. **Formula for full-mesh links?** n(n-1)/2 undirected point-to-point links for n devices.
13. **Define bandwidth.** The data transmission capacity or rate of a communication channel, commonly in bps.
14. **Define transmission delay.** Time to put all L bits of a packet onto a link: L/R seconds.
15. **Define propagation delay.** Time for a signal to cross distance d through a medium: d/v seconds.
16. **What is a backbone?** A high-capacity network portion interconnecting access or departmental networks.
17. **Give two guided media.** Twisted-pair cable and optical fibre (coaxial cable is another).
18. **Give two unguided media.** Radio and microwave (infrared and satellite links are others).
19. **What is circuit switching?** Communication with a path/resources reserved for the duration of a connection.
20. **What is packet switching?** Sending data in smaller packets that share network links and are forwarded toward a destination.
21. **What does ISDN mean?** Integrated Services Digital Network.
22. **What is BRI?** The ISDN Basic Rate Interface: two 64 kbps B channels plus one 16 kbps D channel.
23. **What is the role of a D channel?** Primarily signalling/control in ISDN; its rate depends on the interface.
24. **What is terminal handling?** Management of terminal-host input, output, sessions, buffering and communication control.
25. **What is DTE?** Data Terminal Equipment: terminal/user equipment that originates or receives communicated data.

## 17. Common 5- and 10-Mark Question Practice

**Five-mark topics:** goals of networks; LAN vs WAN; client-server vs peer-to-peer; any three OSI layers; guided vs unguided media; BRI vs PRI; switch vs router; star vs bus; advantages of fibre; transmission vs propagation delay.

**Ten-mark topics:** OSI working with a labelled diagram; comparison of six topologies; packet switching types; difference between access and backbone design; compare three switching methods; ISDN channels and working; terminal handling functions; solved nodal-delay numerical with definitions.

**Recommended answer pattern for 5 marks:** definition (1), labelled diagram/example (1), functions/features (2), advantage/limitation (1).

**Recommended answer pattern for 10 marks:** clear introduction and definition (1-2), labelled diagram (2), six or more explained functions/steps (4), comparison or example (1), advantages/limitations and conclusion (1-2).

**Recommended answer pattern for 20 marks:** 2-3 introductory sentences; clear, correctly labelled diagram; 5-8 headings with explanations; worked example or comparison where applicable; balanced advantages/limitations; final summary. Mark allocations are practice guidance, not an official paper scheme.

## 18. Numerical Practice with Answers

### Problem 1. Full-mesh links

**Question:** How many direct links are needed to fully mesh seven network nodes?

**Solution:** n(n-1)/2 = 7 x 6/2 = **21 links**.

### Problem 2. Transmission delay

**Question:** A 500-byte packet moves across a 2 Mbps link. Find the transmission delay.

**Solution:** 500 x 8 = 4000 bits; 4000 / 2,000,000 = 0.002 s = **2 ms**.

### Problem 3. Propagation delay

**Question:** A physical link is 300 km long. Propagation speed = 2 x 10^8 m/s. Find propagation delay.

**Solution:** d = 300,000 m; delay = 300,000 / 200,000,000 = 0.0015 s = **1.5 ms**.

### Problem 4. Total nodal delay

**Question:** Processing = 0.4 ms, queueing = 0.6 ms, transmission = 1.5 ms and propagation = 2.5 ms. Find nodal delay.

**Solution:** 0.4 + 0.6 + 1.5 + 2.5 = **5 ms**.

### Problem 5. ISDN BRI channels

**Question:** Two B channels are 64 kbps each, and one D channel is 16 kbps. Find the sum of channel rates.

**Solution:** 64 + 64 + 16 = **144 kbps** (channel-rate sum; not total physical-line overhead).

### Problem 6. Multi-link packet transmission

**Question:** A 1000-byte packet crosses four identical 1 Mbps store-and-forward links. Ignore every delay other than transmission. Find the time to receive the entire packet at the destination.

**Solution:** 1000 x 8 = 8000 bits; per-link transmission = 8000 / 1,000,000 = 8 ms. Four links -> 4 x 8 = **32 ms**.

### Problem 7. Packet pipelining

**Question:** A 6000-bit message is divided into three equal 2000-bit packets; these cross two 1 Mbps store-and-forward links. Ignore headers and other delays. Find time until all packets arrive.

**Solution:** Each packet needs 2000/1,000,000 = 2 ms per link. Packet 1 arrives at 4 ms; packet 2 at 6 ms; packet 3 at **8 ms**. Thus the whole message completes in **8 ms**.

### Problem 8. Bandwidth-delay product

**Question:** A 20 Mbps link has an RTT of 25 ms. Find its BDP.

**Solution:** 20,000,000 x 0.025 = **500,000 bits** = **62,500 bytes**.

## 19. Most Important Revision Priorities

**Highest-priority syllabus coverage:** (1) OSI model + functions + diagram; (2) topologies + diagrams + comparison; (3) delay analysis + numericals; (4) guided/unguided physical media; (5) switching methods; (6) backbone/access design; (7) ISDN; (8) terminal handling; (9) goals/applications/network architecture/services. Priority is a practical study suggestion based on content breadth, not a claim about an official question distribution.

**Last-minute memory sheet:**

```text
 OSI 7 -> 1:  Application, Presentation, Session,
             Transport, Network, Data Link, Physical

 Transmission delay = Packet bits / Bit rate        = L / R
 Propagation delay  = Distance / Signal speed      = d / v
 Nodal delay        = Processing + Queue + Tx + Prop
 Full mesh links    = n(n-1)/2
 ISDN BRI           = 2B + D = 64 + 64 + 16 kbps
 E1 PRI (common)    = 30B + D; E1 line rate 2.048 Mbps
 T1 PRI (common)    = 23B + D; T1 line rate 1.544 Mbps

 LAN SWITCH -> Frames, MAC address
 ROUTER     -> IP packets, network-layer forwarding

 Guided media   -> Twisted pair, Coaxial, Optical fibre
 Unguided media -> Radio, Microwave, Infrared, Satellite

 SWITCHING METHODS -> Circuit, Message, Packet
```

## 20. Seven-Day Unit I Study Plan

| Day | Study plan | Self-test |
|---|---|---|
| 1 | Network basics, goals, applications, types, client-server, P2P | Explain network using a college lab example |
| 2 | Services, protocols, layered architecture, full OSI model | Write all seven layers without seeing notes |
| 3 | OSI encapsulation and topologies | Draw OSI + six topology diagrams |
| 4 | Delay analysis and all solved numericals | Solve 4 unseen delay sums |
| 5 | Backbone and local access design | Draw your own campus network design |
| 6 | Physical media and switching methods | Make two comparison tables from memory |
| 7 | ISDN, terminal handling, all eight model answers | Time yourself writing two 20-mark answers |

> **Final reminder:** In a written semester exam, a long answer is not scored by length alone. Make every paragraph relevant, draw clear labelled diagrams, use correct formulas with units, explain the working, and add examples and comparisons. Use the Hinglish explanation to understand; write the English model answer in your exam.
