

> **Exact Unit II syllabus coverage.** **Medium Access Sub Layer:** channel allocations; LAN protocols; ALOHA protocols; overview of IEEE standards; FDDI. **Data Link Layer:** elementary data-link protocols; sliding-window protocols; error handling. To make the prescribed ideas understandable, these notes also explain supporting ideas such as CSMA, framing, Ethernet, parity, CRC, ARQ and Hamming code. Those supporting topics are study explanations, not extra syllabus units.

> **Your study method:** **FIRST: Hinglish** (real-life story, why/how). **SECOND: COMPLETE ENGLISH** (definition, principle, steps, advantages, disadvantages, comparison, diagram). **THIRD: PRACTICE** (solved numerical / trace). **FOURTH: EXAM** (long 20-mark model answers). The long English explanations intentionally repeat the Hinglish concepts so that you can first understand and then write formal answers.

> **Exam honesty:** The important questions below are **syllabus-based likely practice questions**, not a verified list of past-paper questions or guaranteed future questions. Technologies such as FDDI and CSMA/CD are taught for historical and conceptual understanding; modern switched Ethernet commonly uses full-duplex links.

# PART A - COMPLETE UNIT II THEORY (HINGLISH FIRST, ENGLISH AFTER)

## 1. Understanding Where Unit II Fits: Data Link Layer and MAC Sublayer

### 1.1 Hinglish explanation - one classroom, one shared microphone

Socho classroom mein 40 students baithe hain aur **sirf ek microphone** available hai. Agar sabhi students ek saath microphone use karne lagen, awaaz mix ho jayegi. Isliye rule chahiye: kaun kab bolega, kitni der bolega aur agar do log saath bol dein to kya hoga. Computer network mein bhi agar multiple devices same transmission medium ko share kar rahe hon, to **channel access rules** zaroori hote hain. Isi problem ko **Medium Access Control (MAC)** solve karta hai.

Ab second problem socho: tum ek assignment ko WhatsApp ke through send karte ho. Receiver ko pata hona chahiye **message kahaan start hota hai, kahaan end, kya galti hui, aur agar receiver slow hai to sender kitni speed se bheje**. Ye kaam Data Link Layer ke framing, error control aur flow control se relate karta hai.

**Yaad rakhne ki trick:** MAC = "**M**ujhe **A**b **C**hannel milega?"; Data Link = "**ek direct link par data safely arrange karna**". MAC is not a completely different OSI layer; it is commonly described as a sublayer of the Data Link Layer in IEEE LAN architecture.

```text
               OSI LAYER 3: NETWORK
            Packets, IP addressing, routing
                         |
          OSI LAYER 2: DATA LINK LAYER
          +-----------------------------+
          | LLC (Logical Link Control)  |
          | MAC (Medium Access Control) |
          +-----------------------------+
                         |
               OSI LAYER 1: PHYSICAL
            Bits, electrical/optical/radio

 MAC question: Who may use this shared link NOW?
 Data-link question: How is a frame delivered over a link?
```

**Relatable example:** College Wi-Fi mein students simultaneously video, notes aur messages bhej rahe hain. Wireless shared channel hai; coordination zaroori hai. Ethernet switch ke har full-duplex wired port par separate link hota hai, isliye classic shared-medium collisions generally nahi hoti.

### 1.2 Complete English exam explanation

The **Data Link Layer** is the second layer of the OSI reference model. Its main responsibility is to provide services for communication between nodes connected by a single physical or logical link. It receives packets from the Network Layer, organises them into **frames**, and uses the Physical Layer for transmission of bits. It is concerned with **node-to-node (hop-to-hop)** operation; complete end-to-end delivery across many networks depends on higher layers as well.

In the traditional IEEE LAN model the Data Link Layer is described using two sublayers:

1. **Logical Link Control (LLC):** the upper sublayer, historically standardised as IEEE 802.2, provides a logical interface to upper layers and may identify network-layer protocols. Not every modern network stack uses all traditional LLC features.
2. **Medium Access Control (MAC):** the lower sublayer controls use of a shared medium, defines relevant LAN frame functions and uses local link-layer addresses such as MAC addresses.

**Important functions of the Data Link Layer:**

- **Framing:** marks boundaries between frames and adds relevant control information.
- **Physical/link addressing:** uses link-layer addresses to identify interfaces on the same LAN when the technology requires them.
- **Error detection and possible correction/recovery:** detects damaged frames with checks such as FCS/CRC; some data-link protocols use acknowledgements and retransmissions.
- **Flow control:** protects a receiver from excessive incoming frames when supported by the specific protocol.
- **Medium access control:** coordinates transmission by devices sharing a physical medium.
- **Link management:** may establish, maintain or release a data-link relationship, depending on the protocol.

**Data units by layer:** Network Layer = packet (or datagram), Data Link Layer = frame, Physical Layer = bits. A frame is not automatically guaranteed to reach its destination merely because the Data Link Layer exists; the exact reliability depends on the link protocol.

```text
 Sending host                                       Receiving host
 +-------------+                                   +-------------+
 | Network     | -- packet --+        +----------->| Network     |
 +-------------+             |        |            +-------------+
 | Data Link   | [ H | PAYLOAD | FCS ] ----------> | Data Link   |
 +-------------+             |        |            +-------------+
 | Physical    | ======= raw bits/signals ======> | Physical    |
 +-------------+                                   +-------------+
             <---------- One link ---------->
```

### 1.3 MAC address versus IP address (common exam confusion)

| Point | MAC address | IP address |
|---|---|---|
| OSI association | Data Link Layer | Network Layer |
| Purpose | Identifies interfaces for link-local frame delivery | Identifies logical network location and supports routing |
| Typical example | `02:1A:2B:3C:4D:5E` | `192.168.1.20` |
| Format | Common Ethernet MAC is 48 bits | IPv4 = 32 bits; IPv6 = 128 bits |
| Forwarding device | Ethernet switch uses MAC forwarding table | Router forwards using IP routes |
| On path across routers | Link header usually changes hop by hop | IP source/destination normally persist across routers, except cases such as NAT |

**One-mark memory line:** A router forwards **packets across networks**; a bridge or switch forwards **frames inside a LAN** based on link-layer information.

***

## 2. Channel Allocation - Meaning, Need and Types

### 2.1 Hinglish explanation - distributing the college Wi-Fi channel

College canteen mein 10 friends hain aur sab ek hi Wi-Fi medium par data bhejna chahte hain. Problem: **same resource ko sab mein kaise distribute karein?** Isko channel allocation kehte hain. Example ke liye ek road imagine karo:

- **FDMA:** road par alag-alag permanent lanes ban gayin; har user ko apna frequency band.
- **TDMA:** single-lane bridge hai lekin har user ke liye alag time slot.
- **CDMA:** sab ek hi time aur broad frequency resources share kar sakte hain, par alag spreading codes se signals distinguish kiye jaate hain.
- **Dynamic allocation:** jisko abhi zaroorat hai, wo contention ya scheduling ke through channel use karta hai.

Fixed allocation tab easy hai jab sab users regularly busy rehte hon. Lekin agar 10 students mein 9 log offline hain, aur phir bhi unke reserved slots unused hain, to waste hota hai. **Bursty traffic** (kabhi zyada, kabhi bilkul zero) ke liye dynamic sharing useful hoti hai.

```text
 A. FDMA: same TIME, different FREQUENCY portions
 Frequency
   ^    +-------+-------+-------+
   |    |User A |User B |User C |
   +--------------------------------> Frequency bands

 B. TDMA: same frequency resource, different TIME slots
 Time --> [ A ][ B ][ C ][ A ][ B ][ C ]

 C. Dynamic shared medium:
 Time --> [idle][A sends][B waits][B sends][idle]
             ^ contention or scheduling decisions
```

### 2.2 Complete English exam explanation

**Channel allocation** is the process of assigning capacity of a shared communication medium to multiple users or stations. A channel may be divided by frequency, time, code or other resource dimensions, or stations may compete for its use under a medium-access protocol. The main goals are to obtain high utilisation, fair access, acceptable delay, low overhead and efficient operation under the expected traffic load.

There are two broad approaches:

**A. Static (fixed) channel allocation.** A portion of capacity is reserved in advance for each user or class of users.

- **FDMA - Frequency Division Multiple Access:** total available spectrum is separated into different frequency bands, often separated by guard bands. Multiple users can transmit at the same time in different portions of the frequency resource.
- **TDMA - Time Division Multiple Access:** users share a channel in different scheduled time slots. Synchronisation and framing are important.
- **CDMA - Code Division Multiple Access:** user data is spread using appropriate codes; signals are separated using code properties. Power control and interference management are significant in practical CDMA systems.

**Advantages of static allocation:** predictable access, limited collisions if properly configured, simpler allocation analysis and potentially bounded waiting times. **Disadvantages:** idle reserved resources may be wasted; fixed shares respond poorly to changing or bursty user demand; synchronisation and guard overhead may be needed.

**B. Dynamic channel allocation.** A station receives or attempts to use shared capacity according to current demand. This can be further viewed as:

1. **Random access/contention:** a ready station attempts transmission, possible collision is resolved by retry rules (ALOHA, CSMA and their variations).
2. **Controlled access:** stations use permission/turn-taking mechanisms such as **polling**, **reservation** or **token passing**.

**Advantages of dynamic allocation:** suitable for bursty data traffic, may improve use of idle capacity and can adapt to varying active stations. **Disadvantages:** contention systems may suffer collisions and unpredictable delay, while controlled systems require coordination and management overhead.

### 2.3 Static versus dynamic allocation - answer-ready comparison

| Feature | Static channel allocation | Dynamic channel allocation |
|---|---|---|
| Basic idea | Share is fixed/reserved | Capacity assigned as demand appears |
| Examples | FDMA, TDMA (fixed slots) | ALOHA, CSMA, polling, token passing |
| Idle capacity | Can remain unused | May be reused by active stations |
| Bursty data | Often inefficient | Generally better suited |
| Collision | Avoided through separated resources when correctly configured | Possible in contention systems; token systems can avoid classic collisions |
| Access delay | Often predictable | Depends on traffic and protocol |
| Coordination | Partitioning/scheduling | Contention rules or explicit coordination |

### 2.4 Important assumptions used in elementary multiple-access analysis

Textbook discussions of ALOHA and shared LANs often use simplified assumptions: a single shared channel, independently generated frames, similar frame durations, and traffic described by an offered-load parameter. Real systems additionally contain propagation delays, hidden stations, variable packet sizes, interference, noise, prioritisation and complex scheduling. Whenever you use a throughput formula, state its assumptions.

**Exam structure (10/20 marks):** definition -> need -> fixed and dynamic classifications -> explain FDMA/TDMA/CDMA -> controlled versus random access -> diagram -> advantages/disadvantages -> comparison -> conclusion.

***

## 3. LAN Medium Access Protocols - Classification and Working

### 3.1 Hinglish - how students take turns answering a teacher

Teacher question poochta hai. Ek approach: **sab jo chahein bol dein** (random access, collisions possible). Second: **teacher students ke naam se turn de** (polling). Third: **ek talking token circulate ho** aur jiske paas token ho sirf wahi bole (token passing). Networking mein bhi broad ideas yahi hain.

**Three main families:** (a) contention/random access, (b) controlled access, (c) channelisation. In classrooms these categories are sometimes described using different textbook labels; explain the actual working rather than memorising only the headings.

```text
                   MEDIUM ACCESS METHODS
                            |
       +--------------------+---------------------+
       |                    |                     |
  CHANNELISATION       RANDOM ACCESS       CONTROLLED ACCESS
  (divide resource)    (compete to send)   (permission/turns)
  FDMA, TDMA, CDMA     ALOHA, CSMA          Polling, reservation,
                      CSMA/CD, CSMA/CA     token passing
```

### 3.2 Complete English explanation of major families

**Random access protocols:** stations transmit according to contention rules. They do not have an exclusive permanent allocation. If two stations interfere, their transmissions may be lost, requiring retransmission. ALOHA illustrates blind attempts; CSMA senses the channel before sending; CSMA/CD historically detects collisions on shared half-duplex Ethernet; CSMA/CA reduces collision risk in Wi-Fi.

**Polling:** a controller invites stations to transmit in sequence. A station transmits when polled. Advantages include order and collision avoidance; drawbacks include polling overhead, controller dependence and delay when many devices are polled despite having no data.

**Token passing:** a special control frame or permission called a token moves among stations. A station may send data only when allowed by possession of the token. This gives orderly access and helps bound access delay if token-management conditions are satisfied. Lost tokens, token duplication and management complexity require specific recovery mechanisms. Historically important in Token Ring and FDDI.

**Reservation:** stations reserve future slots or transmission opportunities. This can reduce collision during the data phase, but reservation itself requires control information and may add delay.

**Channelisation:** multiple access users receive separate dimensions of a shared resource (frequency/time/code). It is highly structured, but reservation inefficiency can arise when assignments are static.

### 3.3 Collision, contention, throughput and fairness

- **Collision:** two or more overlapping transmissions interfere so that frames cannot be correctly received under that medium/protocol model.
- **Contention:** attempts by multiple devices to obtain use of the shared channel.
- **Throughput:** useful data successfully delivered per unit time, often shown as bits/s or fraction of channel capacity.
- **Offered load:** total attempted traffic, including retries, presented to the medium.
- **Fairness:** whether stations obtain a reasonable share without one being starved.
- **Access delay:** waiting time before a station can transmit a frame.

**Subtle point:** In a modern full-duplex switched Ethernet link, classic CSMA/CD is not used, because both link endpoints can send simultaneously over their link without shared-media collisions. Wireless still has medium-access coordination challenges.

***

## 4. PURE ALOHA - The Simplest Random Access Protocol

### 4.1 Hinglish explanation - everyone speaks whenever ready

Imagine group project meeting. Tumhe idea aaya aur **tum turant bolne lag gaye**, bina ye check kiye ki koi aur bol raha hai ya nahi. Agar do students ek saath bolen to sentences overlap, dono ki baat clear nahi hoti. Phir dono randomly thoda wait karke dobara bolte hain. Ye **Pure ALOHA** ka simple logic hai.

Network station ke paas frame ready hote hi station bhej deta hai. Agar overlap hua to frame fail ho sakta hai. Sender ko success/failure ka pata acknowledgement ya timeout se chal sakta hai, protocol details par depend karta hai. Fail hone par random delay ke baad retry karta hai. **Random wait kyon?** Agar dono collision ke baad same time par retry karein, to fir se collision ho sakti hai.

```text
 PURE ALOHA - no fixed slot boundary
 Time:  ----0-----1-----2-----3-----4-----5---->
 Station A:     [======= FRAME A =======]
 Station B:            [======= FRAME B =======]
                         ^ OVERLAP = COLLISION

 A frame sent at time t is vulnerable to frames that START
 during approximately (t - T, t + T), a total 2T interval.
 T = duration of one equal-sized frame.
```

### 4.2 Complete English exam explanation

**Pure ALOHA** is an early random-access medium-sharing protocol in which a station transmits a frame whenever it has one ready, without waiting for a fixed time slot or first detecting whether the channel is currently occupied. If transmissions overlap on the shared channel, a collision may occur and the involved frames may be lost. A station waits for feedback/timeout and retries after a random interval when necessary.

**Working step by step:**

1. A station generates a frame.
2. It transmits immediately, without time-slot synchronisation.
3. If the frame is delivered successfully, the attempt ends.
4. If a collision or loss prevents successful delivery, the sender detects lack of success through the protocol's feedback/timeout mechanism.
5. The sender waits for a random backoff interval.
6. It retransmits, subject to retry policy.

**Vulnerable period:** Let a frame take $T$ seconds to transmit. A frame beginning at $t$ can be corrupted by another equal-length frame beginning during approximately $t-T$ to $t+T$. Therefore:

$$\boxed{\text{Pure ALOHA vulnerable period} = 2T}$$

**Throughput analysis (ideal Poisson traffic model):** Let $G$ be the average number of total transmission attempts (including retransmissions) per frame time. Let $S$ be successfully transmitted frames per frame time. The probability of zero interfering transmissions during vulnerable time $2T$ is $e^{-2G}$ under the standard Poisson model. Hence:

$$\boxed{S=Ge^{-2G}}$$

Differentiate: $dS/dG=e^{-2G}(1-2G)$. The maximum occurs at $G=0.5$:

$$S_{\max}=0.5e^{-1}=\frac{1}{2e}\approx 0.184=18.4\%$$

**Advantages:** extremely simple, no need for slot synchronisation, practical for explaining fundamental random access. **Disadvantages:** frequent collisions with increasing load, low maximum throughput under the ideal model, random delays and wasted capacity through unsuccessful transmissions.

### 4.3 Solved numerical - Pure ALOHA maximum throughput

**Question:** A pure ALOHA channel transmits equal frames. What is its maximum normalised throughput? For an ideal 1 Mbps channel, what is the corresponding useful bit rate under equal-length full-size frame assumptions?

**Solution:** $S=Ge^{-2G}$, maximum at $G=0.5$. Thus $S_{\max}=1/(2e)=0.18394$. Approximate useful capacity = $0.18394\times 1,000,000=183,940$ bits/s = **about 184 kbps**. This is a simplified ideal throughput conversion; headers and physical overhead may reduce application throughput further.

**Common mistake:** 18.4% is **maximum ideal normalised channel throughput**, not a statement that every deployed ALOHA system always runs at that exact percentage.

***

## 5. SLOTTED ALOHA - ALOHA With Shared Clock Slots

### 5.1 Hinglish explanation - class mein bell ke baad hi bolna

Pure ALOHA mein koi bhi kabhi bhi bol sakta tha. Ab teacher rule bana dete hain: **har 10 seconds ke slot start par hi student bol sakta hai**. Agar ek student slot ke start par bole aur doosra halfway par na bole, to unnecessary partial overlaps kam ho jayenge. Lekin same slot mein do students bol padein to collision ab bhi hogi. Ye **Slotted ALOHA** hai.

```text
 SLOTTED ALOHA - each frame starts at a slot boundary
 Time --> | SLOT 1 | SLOT 2 | SLOT 3 | SLOT 4 |
 A       | Frame A|        | Frame A|        |
 B       |        | Frame B| Frame B|        |
                               ^
                         SAME SLOT COLLISION
```

### 5.2 Complete English exam explanation

**Slotted ALOHA** improves Pure ALOHA by dividing time into equal slots, each generally long enough for one frame. A station is allowed to start transmitting only at the beginning of a slot. All stations require suitable synchronisation. If exactly one station transmits in a slot, its frame is successful in the ideal model. If multiple stations transmit in the same slot, a collision occurs. Failed frames are retransmitted in a later slot using random retry decisions.

**Main working steps:** frame generated -> wait until next slot boundary -> transmit -> check success -> if lost, choose random later slot -> retry.

**Vulnerable period:** only one slot (one frame time), because interference is restricted to simultaneous attempts in that slot in the ideal model.

$$\boxed{\text{Slotted ALOHA vulnerable period}=T}$$

For Poisson offered load $G$ attempts per slot, probability of exactly one transmission in a slot is $Ge^{-G}$:

$$\boxed{S=Ge^{-G}}$$

Differentiate: $dS/dG=e^{-G}(1-G)$. At $G=1$, maximum throughput is:

$$S_{\max}=e^{-1}=\frac1e\approx0.368=36.8\%$$

**Advantages:** approximately double the maximum throughput of Pure ALOHA under ideal assumptions; shorter vulnerable period; conceptually simple. **Disadvantages:** global/adequate time-slot synchronisation, collisions still possible, stations may wait for a slot even when medium is available, poor heavy-load behaviour relative to more advanced protocols.

### 5.3 Pure ALOHA versus Slotted ALOHA (must learn)

| Basis | Pure ALOHA | Slotted ALOHA |
|---|---|---|
| Transmission start | Any moment | Beginning of a slot |
| Synchronisation | No common slot timing | Needed |
| Vulnerable period | $2T$ | $T$ |
| Throughput formula | $S=Ge^{-2G}$ | $S=Ge^{-G}$ |
| Maximum ideal throughput | 18.4% at $G=0.5$ | 36.8% at $G=1$ |
| Collision possibility | Yes | Yes, if multiple stations select a slot |
| Complexity | Lower | Slightly higher |

### 5.4 Two solved numerical examples

**Numerical A:** At offered load $G=0.5$, compare normalised throughput.

- Pure: $S=0.5e^{-1}=0.18394$ or **18.39%**.
- Slotted: $S=0.5e^{-0.5}\approx0.30327$ or **30.33%**.
- Interpretation: with this particular offered load, Slotted ALOHA provides a larger expected successful frame rate under the ideal model.

**Numerical B:** A frame occupies $5$ ms. What are vulnerable periods?

- Pure ALOHA = $2T = 2\times5=10$ ms.
- Slotted ALOHA = $T=5$ ms.

**Exam warning:** Do not use $G=1$ for Pure ALOHA's maximum; its maximum occurs at $G=0.5$.

***
## 6. CSMA, CSMA/CD and CSMA/CA - Important LAN Protocols

### 6.1 Hinglish explanation - listen before speaking

ALOHA mein bina dekhe bolna start hota tha. Ab ek simple improvement: **pehle suno ki koi aur bol raha hai ya nahi**. Agar channel busy hai to wait; agar free hai to transmit. Isko **CSMA (Carrier Sense Multiple Access)** kehte hain. "Carrier Sense" ka matlab transmission shuru karne se pehle medium ko sense karna.

Lekin socho corridor ke opposite ends par A aur B hain. Dono ko kuch seconds tak koi sound nahi aati; dono ek hi waqt bolna start kar dete hain. Signal ko ek end se doosre end tak aane mein time lagta hai. Isliye **CSMA ke baad bhi collision possible** hai. Iski wajah propagation delay hai.

```text
 CSMA EXAMPLE - carrier sensing cannot remove all collisions

 A [Station] ------------------ long shared cable ------------- [Station] B
      |                                                            |
      | carrier seems idle                        carrier seems idle|
      | START at t0                                  START near t0   |
      +-----------------> <-------------------------------+          
                            collision region
```

**CSMA/CD** mein station transmission ke waqt collision detect karne ki koshish karta hai, collide hone par stop karke random backoff ke baad retry karta hai. Classical shared-media, half-duplex Ethernet se iska association hai. **CSMA/CA** mein collision ko transmit hone se pehle reduce karne ke liye random backoff aur Wi-Fi acknowledgement jaise rules hote hain. Wi-Fi mein apne radio se reliable collision detection karna tough hota hai, isliye avoidance approach useful hai.

**Example:** Do students Wi-Fi router se same time file upload karna chahte hain. Dono shared air channel ki availability ko consider karte hain; random wait periods transmissions ko separate karne mein help karte hain. But wireless retries/hidden nodes ke problems complete eliminate nahi hote.

### 6.2 Complete English definition and principle

**Carrier Sense Multiple Access (CSMA)** is a family of contention-based media-access protocols in which a station first checks whether the communication medium appears busy before transmitting a frame. This usually reduces avoidable collisions compared with transmitting blindly as in ALOHA. However, collisions can still arise because signals take time to propagate and the channel observation at one station may differ from that at another.

**CSMA basic operation:**

1. A frame becomes ready for transmission.
2. The station senses the shared medium.
3. If the medium appears idle, transmission is attempted according to its CSMA variant.
4. If busy, the station defers or follows a randomised sensing/backoff procedure.
5. After unsuccessful transmission, a retry algorithm determines a later attempt.

### 6.3 Variants of CSMA

**1-persistent CSMA:** A station monitors the channel. If idle, it transmits immediately (probability 1). If busy, it continues sensing and transmits as soon as the channel becomes idle. Simple and responsive, but several waiting stations may all start together when a busy channel becomes idle, leading to collisions.

**Non-persistent CSMA:** When the channel is busy, the station does not continuously attempt to seize it; it waits a random period before sensing again. This can reduce simultaneous attempts and collisions, at the cost of potentially higher waiting time or unused idle periods.

**p-persistent CSMA:** In a slotted version, when the channel is idle the station transmits in the eligible slot with probability $p$, and defers to a subsequent slot with probability $(1-p)$ (subject to protocol rules). It aims to balance collision risk and delay. Do not confuse this parameter $p$ with probability of packet error.

| Variant | Behaviour on detecting idle medium | Behaviour on busy medium | Main trade-off |
|---|---|---|---|
| 1-persistent | Send immediately | Continue listening | Low wait, greater risk of simultaneous attempts |
| Non-persistent | Send when found idle | Random wait and sense again | Fewer collisions possible, but more idle time |
| p-persistent | Transmit with probability $p$ in an eligible slot | Follow medium/slot rules | Adjustable delay-collision balance |

### 6.4 CSMA/CD - collision detection in classic Ethernet

**CSMA/CD (Carrier Sense Multiple Access with Collision Detection)** augments carrier sensing with detection of collisions while a station is transmitting on a shared half-duplex Ethernet medium.

**Sequence:** sense -> if idle, begin transmission -> monitor for collision -> if collision is detected, abort transmission and send appropriate collision/jam signalling under the classic method -> wait a random backoff interval -> retry if permitted.

```text
          CSMA/CD BASIC FLOW (classic half-duplex Ethernet)
                           START
                             |
                      Frame waiting?
                             |
                          Sense link
                             |
                       Is link busy?
                        /         \
                      YES          NO
                       |            |
                    Wait         Transmit
                       |            |
                       +-- Sense    |
                                    v
                              Collision?
                              /       \
                            NO         YES
                            |           |
                        Complete     Abort frame
                                      & back off
                                          |
                                         Retry
```

**Binary exponential backoff (concept):** after repeated collisions, the sender chooses a random delay from a range that expands after successive collisions. This decreases the likelihood that two stations repeatedly collide in exactly the same way.

**Why minimum frame size matters in classic collision-detection Ethernet:** A transmitting station must still be sending when a worst-case collision indication can propagate back to it. This connects maximum network diameter, propagation speed, bit rate and minimum frame transmission time. For classic Ethernet, the well-known minimum frame size is 64 bytes from destination address through FCS (excluding the preamble and start delimiter). The detailed engineering includes slot-time and repeater constraints.

**Modern relevance:** Ethernet switches normally provide dedicated, full-duplex links on which CSMA/CD collisions do not occur; the algorithm is therefore mainly a historical and exam topic for shared half-duplex Ethernet.

### 6.5 CSMA/CA - collision avoidance in Wi-Fi

**CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance)** is associated with IEEE 802.11 wireless LAN medium access. Rather than relying on collision detection during wireless transmission, it tries to reduce competing starts through sensing, interframe timing and random contention backoff.

**Simplified operation:** station has frame -> sense medium -> wait required idle period -> select random backoff counter -> count down while channel remains eligible -> freeze/resume as required when busy -> transmit -> receiver may return link-layer ACK -> if expected ACK does not arrive, retry according to contention/retry rules. Real 802.11 implementations contain additional details and newer modes.

**Optional RTS/CTS:** Request To Send and Clear To Send frames can reserve the medium virtually for a data transfer and reduce some hidden-node problems, but they create overhead and cannot solve every interference problem.

**Hidden station problem - easy example:**

```text
    Station A         ACCESS POINT B         Station C
        [A]  ----------- [B] ----------- [C]

 A can communicate with B; C can communicate with B.
 A and C cannot reliably hear each other.
 If both send to B at once, their signals can conflict at B.
```

**Exposed station problem:** one station senses another nearby transmission and defers even when its own intended transmission might have been possible without harmful interference at the receivers. This is another reason wireless channel sharing is complex.

### 6.6 Compare ALOHA, CSMA, CSMA/CD and CSMA/CA

| Protocol | Senses before transmit? | Main collision approach | Associated situation |
|---|---|---|---|
| Pure ALOHA | No | Random retransmission | Elementary random-access model |
| Slotted ALOHA | No carrier sensing; slot timing | Limit start times + retries | Elementary synchronised random access |
| CSMA | Yes | Sense before transmitting | Shared-medium contention |
| CSMA/CD | Yes | Detect, abort and retry | Classic half-duplex Ethernet |
| CSMA/CA | Yes, with timing/backoff rules | Try to avoid collisions | Wi-Fi/802.11 MAC |

**20-mark answer tip:** write a definition of CSMA, three persistence methods, one flowchart, a CSMA/CD working paragraph and Wi-Fi CSMA/CA comparison. Explain that "listen first" is not equal to "zero collision".

***

## 7. LAN Protocols and IEEE 802 Standards - Full Overview

### 7.1 Hinglish explanation - why should different brands work together?

Imagine college buys a laptop from one brand, router from second, switch from third, and Wi-Fi access point from fourth. Agar sab devices apne alag random rules follow karein, to connect nahi ho paayenge. **Standard** ek common rule-book hai: frame ka format kya hoga, media access ka rule kya hoga, physical signals kaise represent honge, etc.

IEEE ko networking mein bahut standards ke liye jaana jaata hai, specially **IEEE 802 family** of local/metropolitan-area network standards. Exam mein names yaad rakhna: **802.3 Ethernet**, **802.11 Wi-Fi**, historical **802.5 Token Ring**, aur bridge/VLAN activities ke liye **802.1**. **FDDI ko directly IEEE 802.5 bolna galat hai**; FDDI was standardised primarily through ANSI and related international standards.

### 7.2 Complete English explanation - standardisation

A **network standard** is a documented technical specification or set of requirements that describes how devices, frames, interfaces or protocols should operate so that compatible products can interoperate. The **Institute of Electrical and Electronics Engineers (IEEE)** develops many network standards. The IEEE 802 programme covers families of LAN/MAN technologies and bridging-related mechanisms.

**Benefits of standards:** interoperability across manufacturers, predictable behaviour, large equipment ecosystem, easier development, wider acceptance and consistent terminology. Standards are not themselves guarantees of security or performance; correct implementation and configuration remain necessary.

**Exam-oriented overview (some entries are historical):**

| IEEE group/standard | Main association | Why you should remember it |
|---|---|---|
| IEEE 802.1 | LAN bridging, VLANs, management and related architecture | Switches, 802.1Q VLAN tagging; many related standards |
| IEEE 802.2 | Logical Link Control (LLC), historical | Traditional Data Link sublayer discussion |
| IEEE 802.3 | Ethernet | Wired Ethernet frame and medium specifications |
| IEEE 802.4 | Token Bus (historical) | Controlled token access on logical ring/bus |
| IEEE 802.5 | Token Ring (historical) | Token passing on a ring |
| IEEE 802.11 | Wireless LAN | Wi-Fi physical and MAC technology |
| IEEE 802.15 | Wireless personal-area network activities | Short-range/personal-area technologies |
| IEEE 802.16 | Broadband wireless metropolitan-area access (historical WiMAX association) | Wireless broadband MAN concept |

**Important accuracy note:** Each IEEE designation is a *family or historically used designation*, and individual standards and amendments evolve. This table is the high-level overview usually expected in introductory university examinations, not a claim that every old family remains commercially common.

### 7.3 Ethernet (IEEE 802.3) - detailed frame concept

**Ethernet** is the widely used family of wired local-area networking technologies standardised under IEEE 802.3. Devices exchange Ethernet **frames**. Classic shared Ethernet used contention access with CSMA/CD; modern point-to-point full-duplex switched Ethernet avoids the shared collision domain.

```text
                  COMMON ETHERNET FRAME LAYOUT

 +----------+------+----------+----------+-------+---------+------+
 |Preamble  | SFD  |Dest. MAC |Source MAC|Type/  | Payload | FCS  |
 | 7 bytes  |1 byte| 6 bytes  | 6 bytes  |Length |46-1500*|4 bytes|
 |          |      |          |          |2 bytes| bytes   |      |
 +----------+------+----------+----------+-------+---------+------+

 *In a basic untagged Ethernet frame; padding may be needed.
 Typical minimum frame length = 64 bytes from Dest MAC through FCS.
 802.1Q VLAN tags add fields; other formats/extensions exist.
```

**Explanation of fields:**

- **Preamble:** helps receiver synchronise with the arriving signal.
- **Start Frame Delimiter (SFD):** signals the start of the frame proper.
- **Destination MAC:** identifies intended receiver or group on the LAN.
- **Source MAC:** identifies sender's link-layer interface address.
- **Type/Length:** indicates encapsulated protocol or payload length depending on frame interpretation.
- **Payload and padding:** carries data passed from higher layers; padding meets minimum frame size when needed.
- **FCS (Frame Check Sequence):** CRC-based error detection. A receiver that detects an invalid FCS normally discards the frame.

**Learning point:** The common 64-byte minimum and 1518-byte maximum (basic untagged Ethernet frame excluding preamble/SFD) refer to classic frame fields from Destination MAC to FCS. VLAN-tagged and extended Ethernet formats may be larger. Do not add the 8 bytes of preamble and SFD to the usual 64-byte minimum count when using this convention.

### 7.4 IEEE 802.11 wireless LAN - simplified working

**Wireless LAN** lets devices communicate over radio using access points and/or other Wi-Fi arrangements. IEEE 802.11 provides PHY/MAC specifications, includes carrier-sense and collision-avoidance mechanisms, supports frames for management, control and data, and has security mechanisms configured by supported wireless security protocols.

```text
 [Student Phone] )))\
 [Teacher Laptop])))  [Wi-Fi Access Point] --- [Switch] --- [Router]
 [Tablet]        ))) /                               |
                                               [College Server]
```

**Advantages of WLAN:** mobility, easier deployment without a cable to each mobile device and flexible extension. **Limitations:** radio interference, shared airtime, range/obstacle effects, security misconfiguration risk and variable throughput.

### 7.5 Token Bus and Token Ring - historical exam points

**Token Bus (historical IEEE 802.4):** devices share a physical bus-like arrangement but pass permission according to a defined logical ordering. The token grants a station an opportunity to transmit. **Token Ring (historical IEEE 802.5):** stations form a logical ring, and token passing determines which station transmits. Both illustrate controlled access, contrasting with contention-based Ethernet.

| Concept | Ethernet 802.3 (classic/shared) | Token Ring 802.5 (historical) | Wireless LAN 802.11 |
|---|---|---|---|
| Medium access | Historically CSMA/CD in half-duplex shared mode | Token passing | Primarily CSMA/CA family |
| Common medium | Wired copper/fibre | Historically ring arrangements | Shared radio spectrum |
| Collisions in basic operation | Could occur on shared half-duplex medium | Token control avoids ordinary contention collisions | May occur or be inferred from failed reception |
| Modern use | Extremely widespread | Legacy/rare | Extremely widespread |

### 7.6 IEEE LAN architecture and LLC/MAC diagram

```text
      UPPER LAYERS (NETWORK / IP OR OTHER PROTOCOLS)
                         |
              +----------------------+
              | LLC - historical     |
              | IEEE 802.2 framing/  |
              | service concepts     |
              +----------------------+
              | MAC                  |
              | 802.3 / 802.11 / ... |
              +----------------------+
              | PHYSICAL MEDIUM      |
              | wires/fibre/radio    |
              +----------------------+
```

**Exam marking suggestion:** For an overview question, write the purpose of IEEE and a table with standards. For a deeper answer, draw the Ethernet frame and explain at least Ethernet, Wi-Fi and token-passing LANs.

***

## 8. FDDI - Fibre Distributed Data Interface (A Complete Long-Answer Topic)

### 8.1 Hinglish - a backup ring around the college campus

Imagine college ke 4 departments: Computer Science, Library, Administration aur Examination Centre. Sabko high-speed link chahiye. Ab tum campus mein **do fibre rings** create karte ho. Normal case mein ek main ring data carry karti hai, aur second ring protective/recovery role mein kaam aa sakti hai. Agar cable ka part cut ho gaya to devices design ke according rings ko **wrap** karke alternate route banane ki koshish kar sakte hain. Ye older network technology **FDDI** ka important idea hai.

Ek aur concept: **token**. Socho ek special access card circulate hota hai. Jiske paas card hai, wahi agreed rule ke according data send kar sakta hai. Isse sab ek saath channel seize nahi karte.

**Remember:** FDDI was popular for high-speed campus/backbone networks historically. Today's networks more commonly use switched Ethernet over fibre instead. Exam answers should explain FDDI as a technology, not claim it is today's normal campus backbone standard.

```text
           FDDI - SIMPLIFIED DUAL COUNTER-ROTATING RINGS

             [A] --------> [B]
              ^             |
              |   PRIMARY   v
             [D] <-------- [C]

             [A] <-------- [B]
              |             ^
              v  SECONDARY  |
             [D] --------> [C]

  Logical arrows show counter-rotating directions.
  Physical implementations vary; wrap operation can join paths
  after a supported failure.
```

### 8.2 Complete English definition and background

**Fibre Distributed Data Interface (FDDI)** is a historical high-speed LAN/MAN and backbone technology that typically operates at **100 Mbps** and uses a token-passing access method on a **dual counter-rotating ring** architecture. FDDI was developed through the ANSI X3T9.5 work and related standardisation; it is **not** the same as IEEE 802.5 Token Ring, even though both use tokens.

FDDI was intended to provide high-speed transmission, fault tolerance and support for relatively long-distance connections among devices or buildings. It commonly used optical fibre, which provides resistance to electromagnetic interference and supports high-bandwidth communication over appropriate distances. Some related standards, such as CDDI, used copper media, but the standard exam description of FDDI focuses on fibre.

### 8.3 Architecture and dual rings

FDDI uses two rings that circulate in opposite directions:

1. **Primary ring:** normally carries ordinary data traffic.
2. **Secondary ring:** commonly supports resilience, and may carry traffic in some operating configurations.

**Fault recovery concept:** If the topology experiences a supported break, participating stations can join/wrap the available ring paths to preserve communication among reachable stations. This does not mean every possible multi-failure situation is automatically recoverable.

```text
             NORMAL DUAL RING               AFTER A SINGLE BREAK

              +--- [A] ---+                    +--- [A] ---+
              |           |                    |           |
             [D]         [B]                 [D]         [B]
              |           |                    |           |
              +--- [C] ---+                    +--- [C] -X-+
               Two opposite                     Ring wrap / alternate
               paths exist                       path subject to design

  PRIMARY and SECONDARY are counter-rotating logical rings.
```

### 8.4 Token passing - how FDDI access works

A special permission called a **token** circulates in the ring. A station with information ready waits until the permitted token becomes available. Once it holds the token, it transmits according to FDDI token-timing rules. Other stations forward traffic as required. After the permitted transmission, token passing continues. Real FDDI includes timed-token mechanisms and allocation of synchronous and asynchronous traffic; the simplified classroom statement is that token possession controls media access.

**Synchronous traffic** has reserved timing/bandwidth behaviour for traffic requiring more predictable service. **Asynchronous traffic** uses available token time subject to the operating rules. FDDI uses a **Timed Token Protocol** in its MAC.

```text
    TOKEN PASSING (logical view)

           token ---> [A] ---> [B] ---> [C] ---> [D] ---> [A]
                          |
                       B has data
                          |
                 Waits for token; then
                 transmits as permitted
```

### 8.5 FDDI stations and attachment categories

**Dual-Attachment Station (DAS):** directly connects to both rings; often used for important network devices. **Single-Attachment Station (SAS):** connects through a concentrator or a suitable single-ring attachment arrangement. A **concentrator** can connect multiple devices and help manage/maintain ring connectivity. These categories help network designers reduce the impact of end-device disconnection on the backbone.

### 8.6 FDDI physical and performance features

- **Typical standard data rate:** 100 Mbps.
- **Media:** optical fibre in conventional FDDI deployments.
- **Topology:** dual counter-rotating rings, often with star-wired physical arrangements through concentrators.
- **Access:** token passing with timed-token controls.
- **Encoding:** 4B/5B coding is associated with FDDI physical signalling; additional line signalling mechanisms are used at the PHY.
- **Reliability:** ring-reconfiguration features enable fault recovery for supported single failures.
- **Historical application:** large LANs, campus networks and backbone connectivity.

**4B/5B mini-example:** every 4-bit data group is encoded into a 5-bit code group according to a mapping table. The ratio is $4/5=80\%$ before considering other physical-layer overhead. This is a line-code property, not "only 80 Mbps application throughput"; distinguish data rate, signalling rate and protocol overhead correctly.

### 8.7 Advantages, disadvantages and comparison

**Advantages of FDDI:** 100 Mbps was high speed for its era; fibre supports distance and EMI resistance; dual-ring fault tolerance; token-based regulated access; suitable for historical campus backbone requirements.

**Disadvantages:** higher fibre/equipment cost in its era, token and ring management complexity, technology has largely been replaced by newer Ethernet options, and recovery has constraints under multiple or complex failures.

| Basis | FDDI | Classic Ethernet | Token Ring |
|---|---|---|---|
| Main traditional medium | Optical fibre | Copper/coax/fibre, depending on variant | Copper-based ring-oriented products historically |
| Access | Timed token passing | CSMA/CD on shared half-duplex LAN | Token passing |
| Traditional rate to remember | 100 Mbps | Historical rates vary; modern Ethernet scales far beyond 100 Mbps | Historical rates included 4 and 16 Mbps |
| Logical topology | Dual counter-rotating rings | Historically bus/shared; now switched links | Ring |
| Resilience feature | Dual-ring wrap mechanisms | Depends on switching redundancy topology | Depends on ring implementations |
| Today | Historical/legacy | Widely used | Historical/legacy |

### 8.8 20-mark answer plan for FDDI

Start with full form and definition. Draw two opposite rings. Explain 100 Mbps, token passing, primary/secondary ring, synchronous/asynchronous traffic, DAS/SAS/concentrator, fault recovery, advantages/disadvantages and one comparison with Ethernet. End with the statement that modern switched Ethernet has largely superseded FDDI.

***
## 9. Data Link Layer Design - Services, Framing and Flow Control

### 9.1 Hinglish - a train of messages needs boundaries

Suppose teacher tumhe 10 assignments ek hi large file stream mein send karta hai. Receiver ko kaise pata chalega ki **assignment 1 kahaan end** aur **assignment 2 kahaan begin**? Agar bit stream continuously aati rahe, to boundaries mark karne ki zaroorat padegi. Data Link Layer mein frames bana kar yahi kaam hota hai: **framing**.

Ab imagine ek student phone par slow Internet se file receive kar raha hai aur teacher high-speed LAN se rapidly data send kar raha hai. Agar receiver ka buffer limited hai, to overflow ho sakta hai. Isliye **flow control** important hai. Agar cable mein disturbance se frame ke bits badal gaye, to **error handling** chahiye.

Easy analogy: courier parcel = frame; shipping label = header; parcel contents = payload; safety seal/check code = FCS; delivery acknowledgment = ACK; recipient says "itna fast mat bhejo" = flow control.

### 9.2 Complete English explanation of Data Link Layer design issues

The Data Link Layer defines rules for transferring units of data over a single link. Its design concerns include the services supplied to the Network Layer, framing, flow control, error detection/recovery and shared-medium access. A frame normally includes control fields, a payload and error-detection information, depending on the protocol.

**Common textbook service types to the Network Layer:**

1. **Unacknowledged connectionless service:** frames are sent without connection setup or link-layer acknowledgement. A lost frame is not necessarily recovered by the link layer. Frequently suitable for reliable physical media where higher layers can handle failures; ordinary Ethernet frame delivery does not provide link-layer ARQ.
2. **Acknowledged connectionless service:** each frame can be acknowledged without first establishing a logical link connection. Useful in some less-reliable environments, though details depend on protocol.
3. **Acknowledged connection-oriented service:** a logical link relationship is established and frames are sent in controlled order, with mechanisms to support reliable delivery as defined by the protocol.

These are **abstract design categories**; not every installed LAN implements all three.

**Data Link Layer design responsibilities:** framing, physical-address handling where appropriate, error control, flow control, ordering/duplicate handling in reliable protocols and media access in shared links. Separation into MAC and LLC is an architectural description associated with IEEE LANs.

### 9.3 Framing method 1 - character count / length field

The frame header contains a field specifying how many bytes belong to the frame. The receiver counts the indicated number to find the end. It is straightforward and efficient when the header is intact, but corruption of a length field can cause loss of framing until resynchronisation is achieved.

```text
 [LENGTH=5][ D1 ][ D2 ][ D3 ][ D4 ] ... next frame ...
  ^ Receiver reads header and counts according to the format.
```

**Important:** Frame-length conventions vary: a field may count data bytes or frame bytes depending on a specific protocol. Never assume one universal interpretation.

### 9.4 Framing method 2 - byte stuffing

Use a special flag/control byte to indicate start/end of a frame. If the same flag value appears inside user data, an **escape byte** is inserted to distinguish it from the control flag. Receiver removes the escape during decoding.

```text
 FLAG = F        ESCAPE = E
 Original payload:    A  F  B
 On the link:       F | A  E F  B | F
                      ^start       ^end
 Receiver strips inserted E and restores A F B.
```

**Advantages:** suitable for byte-oriented systems, enables unique frame delimiters after escaping. **Limitations:** extra bytes/processing and the details of what gets escaped depend on the protocol.

### 9.5 Framing method 3 - bit stuffing (VERY IMPORTANT)

A protocol can use a special bit pattern as a flag, e.g. **01111110** in HDLC-style bit-oriented framing. To prevent this flag from appearing as ordinary data, the sender inserts a **0 after any sequence of five consecutive 1 bits** in the data region. The receiver removes that inserted 0 under the corresponding rule.

**Worked example:** Payload bits = `01111110`.

```text
 Original data:     0 11111 1 0
                         ^ after 5 consecutive ones
 Stuffed data:      0 11111 0 1 0
 Combined:          011111010

 Transmitted frame:  01111110 | 011111010 | 01111110
                     FLAG       STUFFED      FLAG
                                PAYLOAD
```

On reception the receiver recognises the flags as frame markers and removes the stuffed 0 from the protected data to reconstruct original bits `01111110`.

**Hinglish trick:** five baar continuously `1` dikhe to **ek extra `0` ghusa do** so data flag se confuse na ho. Receiver us `0` ko nikaal deta hai. This is *transparency*, not error correction.

### 9.6 Framing method 4 - physical-layer coding violations

Some line-coding methods have signal patterns that are not used for ordinary data. A protocol may use special invalid or reserved physical patterns to identify frame boundaries. This approach requires close coordination with the chosen physical encoding and is not universal across every medium.

### 9.7 Flow control versus error control (frequent exam comparison)

| Item | Flow control | Error control |
|---|---|---|
| Main question | Is the receiver being sent data too quickly? | Was data corrupted, lost, duplicated or received incorrectly? |
| Goal | Prevent receiver-buffer overflow | Support integrity and, where implemented, recovery |
| Common methods | Stop-and-wait, receiver windows, credits | CRC/parity, ACK/NAK, sequence numbers, timeout/ARQ |
| Analogy | Wait, I cannot read this many pages at once | Page 3 is missing/damaged; resend it |
| Important distinction | Manages sending rate/quantity | Detects and/or repairs transmission failures |

**Exam conclusion:** Framing, flow control and error control solve different problems and are often combined within the same protocol.

***

## 10. Elementary Data Link Protocols - Three Basic Models

### 10.1 Hinglish explanation - learning step by step

Networks padhte time ekdum sliding window par jump karoge to ACK aur timeout confusing lagenge. Pehle teen simplified situations samjho:

1. **Ideal channel + fast receiver:** Sender continuously frame bhej sakta hai. Error nahi hota; receiver always ready hai.
2. **Ideal channel + slow receiver:** Error to nahi hota, lekin receiver ko time chahiye. Send one, wait, phir next.
3. **Noisy channel:** Frame missing/incorrect bhi ho sakta hai. ACK, timeout, sequence numbers se missing frame retry karna padega.

These are teaching models, not three universal brand-name Internet protocols.

### 10.2 Unrestricted Simplex Protocol (simple ideal protocol)

**Hinglish:** A teacher continuously bol raha hai, student sab sun sakta hai, aur koi bhi word lost nahi hota - bilkul imaginary perfect situation. Sender continuously bhejta jaaye.

**English explanation:** An **unrestricted simplex protocol** is a simplified data-link design in which frames travel only from sender to receiver; the channel is assumed error-free, frames are not lost or reordered, and the receiver can process incoming data as fast as required. Therefore, the sender may continually send frames without waiting for acknowledgements or flow-control messages.

```text
   SENDER                                        RECEIVER
        |--- Frame 0 ----------------------------->|
        |--- Frame 1 ----------------------------->|
        |--- Frame 2 ----------------------------->|
        |--- Frame 3 ----------------------------->|

 Perfect channel and unlimited receiver handling are assumed.
```

**Features:** one-way flow of data, no feedback requirement, no sequence number needed for recovery, no timeout, no retransmission. **Limitation:** unrealistic if physical corruption, loss or receiver overload can occur; useful only as an introductory conceptual baseline.

### 10.3 Simplex Stop-and-Wait Protocol (flow control, error-free link)

**Hinglish:** Teacher ek paragraph dictate kare, student bole "likh liya", tab next paragraph. Receiver ki speed limited ho sakti hai. Sender ko har frame ke baad permission/ACK ka wait karna hota hai.

**English explanation:** The **simplex stop-and-wait protocol for an error-free channel** assumes the channel does not corrupt frames but the receiver may require time to process one frame. The sender transmits one frame and waits for a control signal/acknowledgement before sending the next. It prevents the sender from overwhelming the receiver and demonstrates basic **flow control**.

```text
   SENDER                                            RECEIVER
      |-- Frame 0 -------------------------------------->|
      |<------------------------------------------- ACK--|
      |-- Frame 1 -------------------------------------->|
      |<------------------------------------------- ACK--|
      |-- Frame 2 -------------------------------------->|
```

**Advantages:** simple, low receiver-buffer requirement, prevents uncontrolled frame bursts. **Disadvantages:** poor capacity use on long-delay links because only one frame can be outstanding; no error handling is required under the ideal assumption.

### 10.4 Simplex Protocol for a Noisy Channel / Stop-and-Wait ARQ

**Hinglish:** Ab student ne "paragraph 2" padha hi nahi because mic mein disturbance aa gayi. Teacher ko repeat karna padega. Lekin agar reply "received" lost ho gaya, teacher same paragraph dobara bolega; student ko pata hona chahiye ki **ye duplicate paragraph hai**. Isliye sequence number zaroori hai.

**English explanation:** In a **noisy-channel stop-and-wait ARQ** protocol, frames or acknowledgements can be lost or corrupted. The sender transmits a frame and starts a timer. If successful acknowledgement arrives, it moves to the next frame. Otherwise, timeout causes retransmission. A small sequence-number field, often alternating 0 and 1, lets the receiver recognise an identical retransmitted frame and avoid delivering it to the Network Layer twice.

**Components:** data frame, sequence number, ACK or equivalent feedback, timer/timeout, retransmission and duplicate rejection.

```text
   SENDER                                         RECEIVER
      |-- Frame(0) ---------------------------------->|
      |<----------------------------------- ACK(0) ---|
      |-- Frame(1) ---------- X lost                   |
      |         [timer expires]                       |
      |-- Frame(1) ---------------------------------->|
      |<----------------------------------- ACK(1) ---|
      |-- Frame(0) ---------------------------------->|

 Sequence numbers alternate 0, 1, 0, 1 ...
```

**Acknowledgement convention notice:** Some textbooks label ACK by **last successfully received frame** (ACK 0, ACK 1); others label it as **the next expected frame**. Both conventions can express the same reliability idea; always explain the convention in your exam diagram.

### 10.5 Pseudocode and explanation - stop-and-wait ARQ

```text
 Sender:
   current_seq = 0
   for each frame:
       transmit(frame, current_seq)
       start_timer()
       wait until correct ACK for current_seq or timeout
       if timeout: retransmit SAME frame and sequence
       if correct ACK: current_seq = 1 - current_seq

 Receiver:
   expected_seq = 0
   upon valid frame with seq == expected_seq:
       deliver data ONCE
       send acknowledgement
       expected_seq = 1 - expected_seq
   upon valid duplicate frame:
       do NOT deliver duplicate data
       send acknowledgement again as appropriate
```

**Why this works:** The sender never has two unacknowledged frames, so a single alternating sequence bit is sufficient under this model to distinguish the next intended frame from the previous retransmission. Correct handling still depends on precise timer/ACK rules and assumptions.

### 10.6 Lost data frame versus lost acknowledgement

**Case A - Data frame lost:** Receiver does not receive expected frame; sender sees no valid ACK; sender times out and retransmits the same frame.

**Case B - ACK lost:** Receiver accepted original frame and delivered payload. Sender's ACK fails to arrive; sender retransmits. Receiver detects the duplicate by sequence number, discards duplicate payload, but sends an ACK again. **Without sequence numbers**, the receiver might deliver the same logical data twice.

```text
         CASE B - ACK LOST
 SENDER                                  RECEIVER
    |--- Frame(seq=0) ---------------------->|
    |<--------- ACK(0) ----X                 |  data delivered
    |          [timeout]                     |
    |--- Frame(seq=0) ---------------------->|  duplicate rejected
    |<----------------------------- ACK(0) --|
```

### 10.7 Stop-and-wait efficiency and numerical

Let $T_t$ be frame transmission time, $T_p$ be one-way propagation time and ACK transmission/processing time be negligible. A successful send + feedback cycle requires approximately $T_t+2T_p$. Only $T_t$ is spent transmitting useful frame bits. Therefore:

$$\boxed{U=\frac{T_t}{T_t+2T_p}=\frac1{1+2a},\quad a=\frac{T_p}{T_t}}$$

**Solved question:** Frame size = 1000 bytes, rate = 1 Mbps, one-way propagation delay = 20 ms, negligible ACK time. Compute utilisation.

1. Frame bits = $1000\times8=8000$ bits.
2. Transmission delay $T_t=8000/1,000,000=0.008$ seconds = **8 ms**.
3. Round-trip propagation time = $2\times20=40$ ms.
4. Cycle = $8+40=48$ ms.
5. $U=8/48=0.1667$ = **16.67%**.

**Interpretation in Hinglish:** 48 ms ke cycle mein sirf 8 ms actual useful frame transmission ho raha hai; baaki feedback wait mein ja raha hai. Isi reason se large-delay networks mein sliding window better hai.

***

## 11. Sliding Window Protocols - Why and How They Improve Efficiency

### 11.1 Hinglish explanation - sending five books instead of one

Imagine tum friend ko books courier kar rahe ho. Stop-and-wait mein **ek book bhejo -> delivery confirmation ka wait -> second book bhejo**. Agar courier ko 2 din lagte hain, tum bahut slow ho jaoge.

Ab tum decide karte ho: **ek saath maximum 4 books transit mein reh sakti hain**. Acknowledgement aate hi next book dispatch kar do. Ye idea sliding window ka hai. **Window size** batata hai kitne outstanding (sent but not yet acknowledged) frames allowed hain.

Sliding window sirf throughput badhane ke liye nahi; it provides a basis for flow control and reliable sequencing. Sender aur receiver ke windows alag constraints follow karte hain.

```text
 SENDER VIEW - WINDOW SIZE W = 4

 Sequence numbers: 0   1   2   3   4   5   6   7 ...
                  +---------------+
                  |0   1   2   3  | 4   5   6   7
                  +---------------+
                    may be outstanding

 After acknowledgement of frame 0:
                  0 +---------------+
                    |1   2   3   4  | 5   6   7
                    +---------------+
                         WINDOW SLIDES RIGHT
```

### 11.2 Complete English definition

A **sliding window protocol** permits a sender to transmit multiple frames up to a specified window limit without waiting for an individual acknowledgement for every frame. Sequence numbers identify frames, acknowledgements report reception, and the sender's window moves forward as frames are acknowledged. Compared with Stop-and-Wait, pipelining can improve link utilisation, particularly when propagation delay is large.

**Essential terms:**

- **Sender window:** range of sequence numbers currently allowed to be sent or outstanding.
- **Receiver window:** range of sequence numbers that the receiver is prepared to accept (size depends on protocol).
- **Sequence number:** field identifying a frame within a finite modulo-number space.
- **ACK:** feedback indicating successful reception under a stated convention.
- **Timeout:** event causing retransmission when expected progress does not occur.
- **Cumulative acknowledgement:** confirms all frames up to a boundary in order; commonly used in Go-Back-N.
- **Individual/selective acknowledgement:** permits confirmation of specific frames as in selective protocols.
- **Piggybacking:** places an acknowledgement for one direction inside a data frame travelling in the reverse direction, where applicable.

### 11.3 Sender and receiver window - moving picture

```text
 FRAME NUMBERS -->  0  1  2  3  4  5  6  7
                   [-----------]             W = 4
                     0..3 can be outstanding

 ACK for frame 0 processed:
                    0 [-----------]           W = 4
                       1..4 can be outstanding

 ACK for frames 1 and 2 processed:
                    0  1  2 [-----------]     W = 4
                             3..6 allowed
```

The sender **cannot move the left edge past the first unacknowledged frame** under ordinary contiguous-window logic. Some later ACKs may be stored in Selective Repeat while an earlier missing frame keeps the delivery boundary from advancing.

### 11.4 Relationship of sliding window to sequence-number bits

With $m$ bits dedicated to the sequence number, the available sequence-number values are **$0$ to $2^m-1$**; they wrap around modulo $2^m$. Windows must obey limits that prevent new frames from being confused with old outstanding frames after wraparound.

For the commonly taught standard protocols:

$$\boxed{W_{\text{GBN, sender}}\le 2^m-1}$$

$$\boxed{W_{\text{SR, sender}}\le 2^{m-1}}$$

These are maximum window sizes under the common sequence-space model; real protocols and receiver advertisements can impose smaller limits.

**Example:** For $m=3$, there are $2^3=8$ sequence values, 0 to 7. A standard Go-Back-N sender window may be at most **7**; Selective Repeat sender/receiver windows are typically at most **4** in the equal-window model.

### 11.5 Window efficiency formula and solved numerical

In a simplified **error-free link** with negligible ACK and processing time, normalised sender utilisation under a pipelined window of $W$ equal-sized frames is approximately:

$$\boxed{U\approx\min\left(1,\frac{W}{1+2a}\right),\quad a=\frac{T_p}{T_t}}$$

**Solved question:** $T_t=4$ ms, $T_p=8$ ms, sender window $W=3$. Find ideal utilisation.

1. $a=T_p/T_t=8/4=2$.
2. $1+2a=1+4=5$.
3. $U=\min(1,3/5)=0.6=\mathbf{60\%}$.
4. With $W=5$, $U=\min(1,5/5)=\mathbf{100\%}$ in the idealised model.

**Interpretation:** A larger window keeps the link busier while earlier frames travel and ACKs return. A real network may achieve less due to loss, limited receivers, ACK overhead, competing traffic and protocol implementation.

### 11.6 Piggybacking - adding ACK to a return message

**Hinglish:** Tum friend ko text bhejte ho: "Notes mil gayi. Waise kal exam 10 baje hai." Tumhara original reply *ACK* bhi hai aur extra information bhi. Separate "received" message bhejne ki zaroorat nahi.

**English:** **Piggybacking** is a method in bidirectional data communication in which an acknowledgement for received frames is included in a data frame travelling in the reverse direction. It reduces the overhead of separate acknowledgement frames when reverse-direction data is available in time. Excessive waiting for reverse traffic can delay feedback, so implementations generally have limits/timers rather than waiting indefinitely.

```text
 Host A                                     Host B
   |----- DATA A0 ---------------------------->|
   |<---- DATA B0 + ACK(A0) -------------------|
   |----- DATA A1 + ACK(B0) ------------------>|
```

***
## 12. Go-Back-N ARQ - Working, Examples and Window Limits

### 12.1 Hinglish - missing page means resend from that page onward

Imagine tumhare friend ko notes ke pages 0, 1, 2, 3, 4 bheje. Page 0 aur 1 mil gaye, lekin **page 2 lost** ho gaya. Page 3 aur 4 aa gaye par receiver rule hai: "Mujhe page 2 chahiye, main 3 aur 4 out of order accept nahi karunga." Sender timeout hone par **2, 3, 4 dobara bhejta hai**. Isko **Go-Back-N (GBN)** kehte hain.

GBN ka receiver window usually 1 hota hai: only next expected frame accepted. Sender multiple outstanding frames rakh sakta hai. ACKs **cumulative** hote hain. Example mein convention define karenge: **ACK(k) means receiver is expecting frame k next**, so all earlier in-order frames in the current sequence have been received.

```text
 GO-BACK-N: FRAME 2 IS LOST

 Sender                                    Receiver
    |---- F0 ---------------------------------->|  accept 0
    |<--------------------------------- ACK(1) -|
    |---- F1 ---------------------------------->|  accept 1
    |<--------------------------------- ACK(2) -|
    |---- F2 --------- X                        |  F2 lost
    |---- F3 ---------------------------------->|  discard F3
    |<--------------------------------- ACK(2) -|  still expects 2
    |---- F4 ---------------------------------->|  discard F4
    |<--------------------------------- ACK(2) -|
    |       [timer for oldest missing frame]    |
    |---- F2 ---------------------------------->|  accept 2
    |---- F3 ---------------------------------->|  accept 3
    |---- F4 ---------------------------------->|  accept 4
    |<--------------------------------- ACK(5) -|
```

### 12.2 Complete English definition and working

**Go-Back-N Automatic Repeat reQuest (GBN ARQ)** is a pipelined error-control protocol in which a sender may transmit several frames before receiving acknowledgements, but the receiver normally accepts only the next expected frame in sequence. It discards out-of-order frames. Acknowledgements are cumulative; when an expected frame is lost or fails validation, the sender eventually retransmits that frame and subsequent unacknowledged frames within the outstanding sequence.

**Sender operation:** maintain lower window edge (oldest unacknowledged frame), next sequence to send, window size and a retransmission timer (often for oldest outstanding frame). While within window, send new frames. On a valid cumulative ACK, advance the window. On timeout, retransmit all unacknowledged frames from the oldest outstanding frame onward.

**Receiver operation:** maintain next expected sequence. Accept in-order valid frame, deliver it and advance expectation. When an out-of-order frame arrives, discard it and repeat cumulative acknowledgement for the last contiguous progress (depending on ACK convention).

**Why the name?** Sender goes back to the oldest missing/unacknowledged frame and repeats the following outstanding frames, rather than resending only a single frame.

### 12.3 Sequence space and window example

With 3 sequence bits, frames are numbered modulo 8: 0,1,2,3,4,5,6,7,0,1,... Standard Go-Back-N permits maximum sender window $2^3-1=7$. The receiver window is 1 in the conventional model.

**Example:** $m=4$ -> 16 sequence numbers -> maximum sender window $2^4-1=\boxed{15}$.

**Why not 16?** With an entire sequence space outstanding, after wraparound the receiver/sender may be unable to distinguish a delayed copy of an old frame from a new one under the standard cumulative-ACK model. Keeping the window smaller than sequence space prevents that ambiguity in the textbook setup.

### 12.4 Advantages, disadvantages and application insight

**Advantages:** better utilisation than stop-and-wait, relatively simple receiver with only one in-order acceptance position, cumulative ACKs reduce feedback needs. **Disadvantages:** when a frame is lost, later frames may be discarded even if physically received correctly; retransmitting a long suffix wastes capacity as error rate rises; per-frame buffering at the sender is required.

**When suitable:** conceptual and practical scenarios where the error rate is low and receiver simplicity is valued. In real systems other recovery designs may be preferred for high-loss or high-delay links.

***

## 13. Selective Repeat ARQ - Retransmit Only What Was Missing

### 13.1 Hinglish - keep good pages, ask only for missing page

Same notes example. Page 2 lost hua, page 3 aur 4 sahi aa gaye. Ab smart receiver bole: "Page 3 aur 4 main sambhaal ke rakh leta hoon, mujhe **sirf page 2 dobara bhejo**." Sender selective retransmission karega. Isko **Selective Repeat (SR) ARQ** kehte hain.

**Real difference:** GBN mein correctly received out-of-order frames discard ho sakte hain; SR mein receiver **buffer** karta hai. Isi wajah se SR fewer retransmissions kar sakta hai, lekin memory aur control complex ho jaata hai.

```text
 SELECTIVE REPEAT: FRAME 2 IS LOST

 Sender                                      Receiver
    |---- F0 ---------------------------------->| accept 0
    |---- F1 ---------------------------------->| accept 1
    |---- F2 --------- X                        | missing 2
    |---- F3 ---------------------------------->| buffer 3
    |---- F4 ---------------------------------->| buffer 4
    |<--------------------------- ACK(3),ACK(4) | individually
    |       [timeout / loss indication for F2]  |
    |---- F2 ---------------------------------->| accept 2
    |                                           | now deliver 2,3,4
    |<--------------------------------- ACK(2) -|
```

### 13.2 Complete English definition and working

**Selective Repeat ARQ** is a pipelined error-control protocol in which both sender and receiver maintain windows over a sequence-number space. The receiver can accept and buffer valid out-of-order frames within its receiver window. When particular frames are missing or corrupted, the sender retransmits the affected frames rather than automatically retransmitting every later outstanding frame.

**Sender procedure:** send frames while within window; retain copies of unacknowledged frames; use ACK information for individual frames; maintain retransmission timing or other loss-detection state for outstanding frames; retransmit only frames considered missing; slide the window when the oldest required frames have been acknowledged.

**Receiver procedure:** accept valid frames within receive window, store any that arrive out of order, acknowledge frames individually or with selective reporting, deliver data to the higher layer in sequence when all preceding frames are present, and reject duplicates or frames outside the allowed window.

**Important:** Buffering frame 4 before frame 2 arrives does not mean an ordered data stream should be delivered to the application as frame 4 first. A reliable in-order service normally waits until gaps are filled before in-order delivery.

### 13.3 Selective Repeat window-size restriction

If sequence numbers use $m$ bits, they wrap modulo $2^m$. To avoid confusing old delayed frames with new frames after wrapping, the usual equal-window Selective Repeat model limits its sender/receiver windows to at most half the sequence-number space:

$$\boxed{W_{\text{SR}}\le 2^{m-1}}$$

**Example:** $m=3$ -> 8 values -> $W_{\max}=2^2=4$.

**Example:** Required SR window $W=8$. Minimum number of sequence bits $m$ satisfies $2^{m-1}\ge8$, so $m\ge4$. **Answer: at least 4 bits.**

### 13.4 Advantages and disadvantages

**Advantages:** saves bandwidth by avoiding redundant retransmissions, better recovery efficiency on noisy/high-delay links, allows out-of-order reception buffering, better suitability when multiple frames are in flight. **Disadvantages:** more complex sender and receiver, receiver buffer memory needed, multiple outstanding frame states/timers or sophisticated scheduling, careful sequence-number/window control needed.

### 13.5 Go-Back-N versus Selective Repeat - 20-mark comparison

| Feature | Go-Back-N | Selective Repeat |
|---|---|---|
| Receiver accepts out-of-order frames? | Generally no | Yes, within receiver window |
| Receiver window | 1 | Greater than 1 possible |
| ACK style | Cumulative | Individual/selective |
| After F2 is lost | May retransmit F2,F3,F4... | Retransmits F2 only if others acknowledged |
| Sender buffer | Copies of unacknowledged frames | Copies and individual frame status |
| Receiver buffer | Minimal for out-of-order frames | Buffers correctly received out-of-order frames |
| Complexity | Lower | Higher |
| Efficiency when errors occur | Can waste retransmissions | Usually avoids more redundant retransmissions |
| Max standard sender window with $m$ bits | $2^m-1$ | $2^{m-1}$ |
| Standard receiver window | 1 | $\le2^{m-1}$ |

**Exam line:** "Go-Back-N sacrifices retransmission efficiency to keep receiver processing simple; Selective Repeat uses buffering and per-frame knowledge to retransmit selectively."

***

## 14. Error Handling - Detection, Correction and Recovery

### 14.1 Hinglish - three different ideas you must not mix up

Tum teacher ko PDF bhejte ho, transmission mein disturbance se bits change ho jaate hain. Receiver ko teen cheezein alag-alag sochni padengi:

1. **Error detection:** "Mujhe lagta hai data badal gaya" (parity/checksum/CRC check).
2. **Error correction:** "Extra information se main galat bit identify karke theek kar sakta hoon" (suitable error-correcting code such as Hamming).
3. **Error recovery:** "Sahi frame dobara bhej do" (ARQ with ACK, timeout and retransmission).

Inhe same mat likhna. CRC normally **detects** many errors; standard Ethernet CRC apne aap bit fix nahi karta. ACK/timeout itself does not calculate which bit changed; it is part of retransmission-based recovery.

### 14.2 Complete English classification of transmission errors

An **error** occurs when received data differs from sent data. Interference, noise, interference between signals, hardware faults or other transmission conditions can change or lose data.

- **Single-bit error:** exactly one bit of a data unit changes, e.g. sent `101101` versus received `101001`.
- **Multiple-bit error:** more than one bit changes, possibly at separate locations.
- **Burst error:** errors occupy a span of successive bit positions from first corrupted bit to last; not every bit in the span must be wrong. Burst-error length is measured across that span.
- **Lost frame:** a frame does not arrive or fails validation and is discarded.
- **Duplicate frame:** a retransmission arrives after the original was already accepted.

**Error control systems** may use redundancy bits, sequence numbers, acknowledgement messages, retransmission timers, and error-correcting codes. Error detection checks the presence of invalid data; forward error correction (FEC) adds structured redundancy to reconstruct some errors without requesting a retransmission; ARQ requests/repeats transmissions to recover missing or damaged frames.

### 14.3 Parity check - simplest error-detection method

**Hinglish:** Total `1`s count karo. **Even parity** mein final bits mein 1s ki count even honi chahiye, **odd parity** mein odd. Ek parity bit add karte hain.

**English:** A parity bit is an extra bit chosen so that the total number of 1s in data plus parity bit has the desired parity. A single changed bit flips parity and can be detected. However, many even-numbered bit changes can escape a single parity check, so parity is a limited error-detection method.

**Solved example:** Data = `1011001` -> number of 1 bits = 4 (already even).

- **Even parity bit = 0**, transmitted data plus parity = `10110010` if appended.
- **Odd parity bit = 1**, transmitted data plus parity = `10110011` if appended.

**Another example:** Data `1100100` contains 3 ones. Even parity bit = 1; odd parity bit = 0.

**Limitation:** A 2-bit error might preserve parity, so a receiver could incorrectly accept corrupted data.

### 14.4 Two-dimensional parity (block parity)

In two-dimensional parity, data bits are arranged in rows and columns; parity is computed for both directions. This can localise a single-bit error to the intersection of the affected row and column, allowing single-bit correction in the simplified scheme. Some multiple-error patterns can still go undetected. Do not treat this as an unlimited error-correction system.

```text
     4x4 DATA      ROW EVEN PARITY
       1 0 1 0  |  0
       0 1 1 0  |  0
       1 1 0 1  |  1
       0 0 1 1  |  0
       ---------+---
       0 0 1 0    (column parity bits)

 If one data bit flips, its row and column parity checks fail.
```

### 14.5 Checksum - binary word sum method

**Hinglish:** Ek packet ko chhote fixed-size words mein baant kar sum nikaalo, complement create karo. Receiver same rule follow karke check kare. Ye simple way hai data change hua ya nahi dekhne ka.

**English:** A common class of checksum uses one's-complement arithmetic over fixed-size words. The sender adds the words using end-around carry, complements the result and transmits that checksum. A receiver recomputes an appropriate sum and checks whether it matches the expected all-one pattern. This is a common Internet-protocol idea; not every Data Link protocol uses this exact checksum format.

**Solved 8-bit one's-complement example:**

```text
 Word 1:    10010011   (0x93)
 Word 2:  + 01101010   (0x6A)
            --------
 Sum:       11111101   (0xFD; no end-around carry)
 Checksum:  00000010   (bitwise complement)

 Receiver sum of words + checksum = 11111111
 -> passes one's-complement checksum check.
```

**Strength and limitation:** easy to compute but less effective against some structured error patterns than an appropriate CRC polynomial. Passing a checksum does not prove that data is perfectly correct in all circumstances.

***
### 14.6 CRC - Cyclic Redundancy Check (VERY IMPORTANT THEORY + NUMERICAL)

**Hinglish explanation:** Imagine tum ek assignment ke saath **special verification code** send karte ho. Sender original data par ek mathematical division-like XOR process run karta hai aur jo **remainder** milta hai use frame ke end mein attach kar deta hai. Receiver poore received frame ko same generator se divide karke check karta hai. Agar remainder non-zero hai to frame damaged maana jaata hai. Ye **CRC** hai. CRC normal use mein **error detection** karta hai, data ka repair nahi.

**Formal English definition:** Cyclic Redundancy Check is an error-detection method based on polynomial arithmetic over binary coefficients (GF(2)). It treats a binary bit string as a polynomial, divides the message with appended zeros by a chosen generator polynomial using XOR operations (no ordinary carries or borrows), and appends the remainder as redundancy bits. The receiver divides the received codeword by the same generator; a non-zero remainder identifies an error according to the check.

**Important terms:**

- **Dataword ($D$):** original information bits.
- **Generator/divisor ($G$):** known binary pattern defining the CRC polynomial; its degree $r$ equals the number of remainder bits.
- **Remainder ($R$):** $r$ bits obtained after modulo-2 division.
- **Codeword ($C$):** transmitted bit sequence, equal to dataword followed by remainder.

**Sender steps:**

1. Choose generator with $r+1$ bits (degree $r$).
2. Append $r$ zeros to original dataword.
3. Divide resulting sequence by generator using modulo-2 subtraction (XOR).
4. Obtain $r$-bit remainder, including leading zeros if needed.
5. Replace appended zeros with remainder and transmit resulting codeword.

**Receiver steps:**

1. Receive codeword.
2. Divide codeword by same generator.
3. If remainder is zero, check passes; otherwise the frame has detectable corruption.
4. Note that **zero remainder does not mathematically guarantee no possible error**: certain error patterns can escape a given CRC polynomial.

```text
                    CRC SENDER AND RECEIVER

 SENDER                                          RECEIVER
 [Original bits D]                              [Received C']
        |                                             |
 Append r zeros                                 Divide by G
        |                                             |
 Modulo-2 divide by G                           Remainder?
        |                                        /        \
   r-bit R                                  ZERO        NON-ZERO
        |                                    |             |
 C = D followed by R                     Pass check    Detect error
        |                              (not proof of   discard/recover
   Send on link                          perfection)
```

**Why XOR?** Binary polynomial arithmetic over GF(2) has $1+1=0$, so subtraction equals XOR. Basic rules: `0 XOR 0 = 0`, `0 XOR 1 = 1`, `1 XOR 0 = 1`, `1 XOR 1 = 0`.

**Solved CRC numerical - full working:**

**Question:** Dataword = `1101011011` and generator = `10011`. Calculate CRC remainder and transmitted codeword.

**Step 1:** Generator has 5 bits; its polynomial degree is $r=4$. Append four zeros.

```text
 Dataword D:              1101011011
 Generator G:             10011
 Number of appended 0s:   4
 Extended dividend:       11010110110000
```

**Step 2:** Perform modulo-2 division of `11010110110000` by `10011`. At each leftmost `1` in the working dividend, XOR the 5-bit generator aligned to that position; at a leading `0`, move to the next position. This produces final 4-bit remainder:

```text
 EXTENDED DIVIDEND = 11010110110000
 GENERATOR         = 10011
 XOR long division, no carries or borrows
 --------------------------------------
 REMAINDER         = 1110
```

**Step 3:** Attach remainder to original dataword.

$$\boxed{C=D\Vert R=1101011011\Vert1110=11010110111110}$$

**Step 4 - receiver verification:** Dividing the transmitted codeword `11010110111110` by `10011` gives `0000` remainder if unchanged, so the CRC check passes.

**Hinglish warning:** Generator ke 5 bits dekhkar **5 zeros mat lagana**; degree 4 hai, so **4 zeros** append karoge. XOR division ordinary decimal division nahi hoti. Exam mein `data + appended zeros -> XOR division -> remainder -> codeword -> receiver check` clearly likho.

**Error-detection properties:** CRC can detect many commonly occurring corruption patterns very effectively. Specific guaranteed detection of burst lengths or patterns depends on the generator polynomial. A generator with non-zero constant term detects a single-bit error in ordinary polynomial-division settings; choosing an appropriate standard polynomial is essential. Avoid an unsupported statement like "CRC detects every error of any length."

### 14.7 Hamming code - single-bit error correction with parity positions

**Hinglish explanation:** Simple parity se pata chal sakta tha ki data wrong hai, lekin **kaunsa bit** wrong hai, ye often nahi pata lagta. Hamming code multiple carefully placed parity bits add karta hai so checking pattern se wrong bit ki **position** mil sakti hai. Imagine class mein 7 benches hain. Har inspector ka particular set of benches hai. Jo inspectors complaint karte hain unke combination se faulty bench number ban jaata hai.

**Formal English:** A Hamming code is a family of linear error-correcting codes that adds parity-check bits at carefully selected positions. In a standard Hamming(7,4) code, four information bits are mapped to a seven-bit codeword with three parity bits. A single-bit error creates a non-zero **syndrome** that identifies the erroneous position, enabling correction. Basic Hamming(7,4) is not a guarantee of correcting multiple simultaneous errors.

For $m$ data bits and $r$ parity bits, the conventional single-error-correction arrangement requires:

$$\boxed{2^r\ge m+r+1}$$

The condition supplies enough distinct patterns for "no error" and each possible one-bit error position. For $m=4$, try $r=3$: $2^3=8$ and $4+3+1=8$, so **three parity bits** are sufficient in the standard Hamming(7,4) code.

**Positions and parity groups (even parity):**

```text
 POSITION       1   2   3   4   5   6   7
 ROLE           P1  P2  D1  P4  D2  D3  D4
 PARITY P1 checks positions: 1,3,5,7
 PARITY P2 checks positions: 2,3,6,7
 PARITY P4 checks positions: 4,5,6,7
```

**Solved numerical:** Encode data bits `1011` using standard Hamming(7,4) with even parity.

1. Place data D1=1, D2=0, D3=1, D4=1 at positions 3,5,6,7.
2. P1 covers positions 1,3,5,7. Data values are 1,0,1 -> two ones -> **P1=0**.
3. P2 covers 2,3,6,7. Data values are 1,1,1 -> three ones -> **P2=1** to make even count.
4. P4 covers 4,5,6,7. Data values are 0,1,1 -> two ones -> **P4=0**.

```text
 Position:  1   2   3   4   5   6   7
 Role:      P1  P2  D1  P4  D2  D3  D4
 Value:     0   1   1   0   0   1   1

 TRANSMITTED CODEWORD = 0110011
```

**Single-bit correction example:** Suppose position 6 changes during transmission. Received bits become `0110001` (bit 6 is now 0). The parity tests produce:

- P1 group (1,3,5,7): parity still even, so syndrome bit $s_1=0$.
- P2 group (2,3,6,7): parity is odd, so $s_2=1$.
- P4 group (4,5,6,7): parity is odd, so $s_4=1$.

Syndrome position = $4s_4+2s_2+s_1=4(1)+2(1)+0=\boxed{6}$. Flip bit 6 to reconstruct `0110011`, from which data bits at 3,5,6,7 give `1011`.

**Precise limitation:** Standard Hamming(7,4) corrects one-bit errors. Additional overall parity can turn it into an extended SECDED scheme (single-error correction, double-error detection), but that is a separate code variant.

### 14.8 Automatic Repeat reQuest (ARQ) - comprehensive comparison

**ARQ** is a reliability mechanism using error detection, acknowledgements, sequence numbers and retransmission. It is sometimes taught alongside error-correcting codes but operates differently: it usually **recovers by repeating lost/damaged information**, not by computing the original bit from FEC redundancy.

| Mechanism | Outstanding frames | On a lost frame | Receiver memory |
|---|---|---|---|
| Stop-and-Wait ARQ | 1 | Retransmit same frame after timeout | Minimal |
| Go-Back-N ARQ | Up to sender window | Retransmit missing and later unacknowledged frames | Usually expects only next in-order frame |
| Selective Repeat ARQ | Up to sender window | Retransmit only affected unacknowledged frames | Buffers out-of-order frames |

**Negative acknowledgements (NAK/NACK):** Some protocols explicitly signal missing/corrupted frames; others rely on missing ACKs, cumulative ACKs and timeout. Do not write that every ARQ design requires a NAK message.

**Timeout selection:** if timeout is too short, unnecessary retransmissions increase; if too long, recovery is slow. Actual protocol designs consider propagation, processing, contention, variability and measured round-trip time.

### 14.9 Error detection versus correction versus ARQ

| Question | Error detection | Error correction (FEC) | ARQ recovery |
|---|---|---|---|
| Finds a suspected error? | Yes | Usually includes structured checking | Uses errors/loss indications and ACK state |
| Fixes bits without retransmission? | No | For errors within code capability | No; resends information |
| Common example | CRC, parity, checksum | Hamming code | Stop-and-Wait, GBN, SR |
| Extra cost | Check bits | More structured redundancy and processing | ACK traffic, timers and retransmissions |
| Practical limitation | Undetected patterns may exist | Limited correctable patterns | Retries cost delay and bandwidth |

***

## 15. Fast Revision Tables, Common Mistakes and Diagram Practice

### 15.1 Core formula sheet

| Topic | Formula | Important assumptions |
|---|---|---|
| Pure ALOHA | $S=Ge^{-2G}$ | Poisson offered traffic, equal frame time |
| Pure ALOHA maximum | $S_{\max}=1/(2e)=18.4\%$ | At $G=0.5$ |
| Slotted ALOHA | $S=Ge^{-G}$ | Slotted and synchronised model |
| Slotted ALOHA maximum | $S_{\max}=1/e=36.8\%$ | At $G=1$ |
| Transmission time | $T_t=L/R$ | $L$ in bits, $R$ in bits/s |
| Stop-and-wait utilisation | $U=1/(1+2a)$ | Negligible ACK/processing times, no loss |
| Propagation ratio | $a=T_p/T_t$ | One-way propagation / frame transmission |
| Sliding window utilisation | $U\approx\min(1,W/(1+2a))$ | Ideal error-free pipeline |
| GBN maximum sender window | $W\le2^m-1$ | Standard GBN model, $m$ sequence bits |
| SR maximum window | $W\le2^{m-1}$ | Standard equal-window SR model |
| Hamming parity count | $2^r\ge m+r+1$ | Single-error-correcting Hamming construction |
| CRC redundancy length | $r=\deg(G)$ | Generator polynomial degree |

### 15.2 Ten mistakes that lose easy examination marks

1. Calling MAC a separate OSI layer instead of a sublayer of Layer 2.
2. Writing Pure ALOHA maximum 36.8% and Slotted 18.4% (the correct values are the opposite).
3. Confusing **offered load $G$** with **successful throughput $S$**.
4. Stating CSMA can never collide; propagation effects allow collisions.
5. Claiming CSMA/CD is used on ordinary modern full-duplex switched Ethernet links.
6. Calling FDDI an IEEE 802.5 standard; FDDI and historical IEEE Token Ring are different.
7. Claiming a CRC always corrects corrupted bits; basic CRC checks for errors.
8. Omitting sequence numbers in noisy stop-and-wait, so duplicate deliveries remain unexplained.
9. Confusing GBN and SR receiver buffering and retransmission behaviour.
10. Forgetting the window limits $2^m-1$ for GBN and $2^{m-1}$ for standard SR.

### 15.3 Diagrams to practise neatly on paper

- OSI Layer 2 divided into LLC and MAC.
- Channelisation: FDMA and TDMA.
- Pure ALOHA collision and $2T$ vulnerable period.
- Slotted ALOHA collision and $T$ vulnerable period.
- CSMA/CD flowchart and CSMA/CA hidden-node diagram.
- Ethernet frame fields; IEEE overview table.
- FDDI dual counter-rotating rings and token passing.
- Frame format with byte stuffing and bit stuffing.
- Stop-and-wait with ACK loss.
- Sliding sender window moving after ACKs.
- GBN lost-F2 retransmission sequence.
- SR lost-F2 buffering sequence.
- CRC sender-receiver verification and Hamming(7,4) positions.

### 15.4 How to structure a complete 20-mark answer

**Suggested organisation (adjust to wording of the question):** definition/introduction (2 marks); labelled diagram (3 marks); principles and step-by-step working (7 marks); variants, numerical or example (4 marks); advantages, disadvantages and comparison (3 marks); short conclusion (1 mark). These are a *study-writing template*, not an official marks distribution. Use the headings required by the actual question and write around 3-5 handwritten pages when appropriate for your university answer-book pace.

\newpage

# PART B - IMPORTANT 20-MARK EXAM QUESTIONS WITH COMPLETE MODEL ANSWERS

> **How to use this section:** Read the Hinglish discussion above first. The following answers are deliberately written in **English exam language**, with introductions, subheadings, working, diagrams, comparisons and conclusions. They can be adapted to questions carrying 10, 15 or 20 marks. In an exam, the teacher may combine topics differently.

## Q1 (20 marks). Define channel allocation. Explain static and dynamic channel allocation and discuss FDMA, TDMA, CDMA and LAN access methods.

### Complete model answer (English)

**Introduction and definition.** Channel allocation is the process by which a communication system shares its available transmission resources among multiple users or stations. It determines when and how a sender is allowed to use a common communication medium. Allocation is important because uncoordinated simultaneous transmissions may interfere, while excessive reservations may waste resources. The design objectives are efficiency, fairness, appropriate access delay and reliable operation.

**Classification.** Channel allocation can broadly be classified as **static** and **dynamic**. Under static allocation, a user receives a predetermined part of the available channel capacity. Under dynamic allocation, access is determined according to current demand through contention or permission-based control.

**Static allocation methods:**

1. **Frequency Division Multiple Access (FDMA):** the available spectrum is divided into frequency bands. Each active user is assigned a distinct portion, often with guard regions to reduce interference. Users can transmit at the same time in different bands. This is easy to conceptualise but unused assigned frequency cannot always be shared efficiently.
2. **Time Division Multiple Access (TDMA):** the channel is divided into time slots. Different users use the resource in different slots; synchronisation is required. A user's reserved slot can remain idle when the user has nothing to send.
3. **Code Division Multiple Access (CDMA):** signals are separated using spreading codes over a shared resource. Different users' signals can overlap in time and frequency but remain distinguishable using code-based processing, subject to interference, power-control and receiver constraints.

```text
 FDMA: |Frequency A|Frequency B|Frequency C| (parallel bands)
 TDMA: | A | B | C | A | B | C |             (successive slots)
 CDMA: multiple coded transmissions share broad resources
```

**Dynamic allocation:** When a station needs to transmit, it competes or waits for permission. Random-access schemes include ALOHA and CSMA. In Pure ALOHA a station attempts transmission whenever ready; in Slotted ALOHA it starts at slot boundaries; in CSMA it senses the medium before attempting. **Controlled-access techniques** include polling, reservation and token passing. Polling lets a controller invite devices to send; token passing gives permission to the token holder; reservation establishes future access opportunities.

**Comparison of approaches:** Static assignment provides predictable separation but can waste capacity under bursty traffic. Dynamic sharing uses capacity according to activity, but random-access protocols can suffer collisions and variable delay, while controlled access requires management overhead. The most suitable method depends on traffic pattern, physical medium, number of users and delay requirements.

**Practical example:** A college Wi-Fi network has students using the medium at changing times. Allocating identical permanent time shares to inactive students would be inefficient. A dynamic wireless channel-access mechanism better accommodates intermittent traffic, although contention and fairness still need management.

**Conclusion.** Channel allocation is essential wherever users share limited transmission resources. Static schemes divide capacity in advance, whereas dynamic schemes allocate access according to demand. Effective medium-access design balances throughput, fairness, overhead and delay.

***

## Q2 (20 marks). Explain Pure ALOHA and Slotted ALOHA in detail. Derive their throughput equations and compare the protocols.

### Complete model answer (English)

**Introduction.** ALOHA is an early random-access protocol used to illustrate how multiple stations may share a common channel without a dedicated central scheduler. Its basic forms are **Pure ALOHA** and **Slotted ALOHA**. Both allow collisions and use randomised retransmission after unsuccessful attempts.

**Pure ALOHA working:** A station transmits immediately when it has a frame. If its frame overlaps with any interfering frame, collision may occur. On failure, the sender waits a random time and retransmits. There is no slot synchronisation. Let $T$ denote one frame transmission time. A transmission that begins at time $t$ is vulnerable to interfering frames starting between approximately $t-T$ and $t+T$. Therefore, vulnerable period = $2T$.

```text
 Pure ALOHA:
 A:        [---------- FRAME A ----------]
 B:               [---------- FRAME B ----------]
                   overlapping transmissions
 Vulnerable interval for A = 2T
```

**Throughput derivation for Pure ALOHA:** Under the standard Poisson model, assume offered traffic $G$ attempts per frame duration, including retries. The expected number of interfering attempts during the $2T$ vulnerable interval is $2G$. Probability of zero such attempts is $P_0=e^{-2G}$. Successful throughput is attempted traffic times probability of successful transmission:

$$S=G\times P_0=Ge^{-2G}.$$

Differentiate with respect to $G$: $dS/dG=e^{-2G}(1-2G)$. Setting this to zero gives $G=0.5$. Thus:

$$S_{\max}=0.5e^{-1}=\frac{1}{2e}\approx0.184=18.4\%.$$

**Slotted ALOHA working:** Time is divided into equal-sized slots. A station waits until the beginning of a slot to transmit a ready frame. Two stations transmitting in the same slot may collide, but a station cannot begin halfway through another permitted slot. This reduces vulnerable time to $T$. Synchronisation is required.

```text
 Slotted ALOHA:
 Time --> | Slot 1 | Slot 2 | Slot 3 | Slot 4 |
 A       |   A    |        |   A    |        |
 B       |        |   B    |   B    |        |
                              collision in slot 3
```

**Throughput derivation for Slotted ALOHA:** With Poisson offered traffic $G$ per slot, the probability of exactly one attempt in a slot is $Ge^{-G}$. Thus $S=Ge^{-G}$. The derivative is $e^{-G}(1-G)$; the maximum occurs at $G=1$:

$$S_{\max}=1/e\approx0.368=36.8\%.$$

**Comparison:** Pure ALOHA starts at any instant and has $2T$ vulnerable period, no slot synchronisation and 18.4% ideal maximum throughput. Slotted ALOHA starts only at boundaries, has $T$ vulnerable period, requires synchronisation and reaches about 36.8% maximum under ideal assumptions. Both still experience collisions and retransmission delay.

**Numerical example:** If $T=10$ ms, vulnerable period of Pure ALOHA is 20 ms, whereas for Slotted ALOHA it is 10 ms. If $G=0.5$, Pure ALOHA throughput is $0.5e^{-1}=0.1839$, while Slotted ALOHA throughput is $0.5e^{-0.5}=0.3033$.

**Conclusion.** Slotted ALOHA roughly doubles the ideal maximum throughput of Pure ALOHA by restricting transmission start times. However, carrier-sensing and more advanced LAN protocols can make more efficient use of the medium under many conditions.

***

## Q3 (20 marks). What is CSMA? Explain 1-persistent, non-persistent and p-persistent CSMA, CSMA/CD and CSMA/CA with diagrams.

### Complete model answer (English)

**Introduction.** Carrier Sense Multiple Access (CSMA) is a contention-based channel-access method in which a station listens to the shared medium before attempting to transmit. The purpose is to avoid starting a transmission when another station's signal is already detectable. CSMA reduces unnecessary collisions compared with ALOHA, but cannot eliminate them because of propagation delay and simultaneous decisions at distant stations.

**Basic CSMA procedure:** (1) station receives a frame from the higher layer; (2) it senses the medium; (3) when the medium appears idle, it may transmit according to the variant; (4) when busy, it waits or applies backoff; (5) failed transmissions are retried using suitable rules.

**Types of CSMA:**

1. **1-persistent CSMA:** continuously sense a busy medium; transmit immediately when it becomes idle. It is responsive but several waiting stations can begin together after the channel becomes free.
2. **Non-persistent CSMA:** after sensing a busy medium, wait a random interval before sensing again. It may reduce immediate collisions but also create additional idle intervals and waiting delay.
3. **p-persistent CSMA:** under an appropriate slotted model, transmit in an idle eligible slot with probability $p$; with probability $1-p$, defer to another slot and repeat the decision. This makes access probability adjustable.

**CSMA/CD:** Carrier Sense Multiple Access with Collision Detection adds monitoring during transmission on a shared, half-duplex medium. If a station detects a collision, it stops transmission, follows collision signalling as required and uses a random backoff before retrying. Classic Ethernet used this technique with a minimum frame transmission time related to collision detection across the permitted network diameter.

```text
 Sense carrier --> Busy? -- YES --> Wait and sense again
                      |
                     NO
                      |
                 Transmit frame
                      |
                Collision? -- YES --> Abort + random backoff
                      |
                     NO
                      |
                  Completed
```

**CSMA/CA:** Carrier Sense Multiple Access with Collision Avoidance is closely associated with IEEE 802.11 wireless LANs. Wireless stations sense the channel, wait applicable interframe periods and use random backoff before transmission. Link-layer acknowledgements support the detection of unsuccessful deliveries, and optional RTS/CTS exchange can help reduce hidden-station problems in some environments. CSMA/CA seeks to reduce collision likelihood rather than detect overlapping transmissions directly during the station's own radio transmission.

**CSMA/CD versus CSMA/CA:** CSMA/CD detects and reacts to collisions and is historically associated with shared half-duplex Ethernet. CSMA/CA uses avoidance through sensing/backoff and is associated with Wi-Fi. Modern switched, full-duplex Ethernet generally does not use CSMA/CD because those links have no shared collision domain.

**Hidden terminal:** If stations A and C can each reach access point B but cannot hear one another, both may see idle channel and transmit to B simultaneously. This explains why sensing alone cannot make wireless medium access perfectly collision-free.

**Conclusion.** CSMA improves channel sharing by sensing before sending. Persistence choices trade delay for collision probability; CSMA/CD provides historical collision recovery for Ethernet, and CSMA/CA addresses practical wireless sharing constraints.

***

## Q4 (20 marks). Describe the IEEE 802 LAN standards. Explain Ethernet frame format and compare Ethernet, Token Ring and Wi-Fi.

### Complete model answer (English)

**Introduction.** The IEEE 802 family provides technical standards for local and metropolitan area networking, including mechanisms for LAN frame formats, access methods, physical media and bridging. Standardisation allows compliant devices from different manufacturers to communicate and reduces implementation uncertainty.

**Data Link structure:** Traditional IEEE LAN architecture subdivides the Data Link Layer into Logical Link Control (LLC) and Medium Access Control (MAC). LLC is associated historically with IEEE 802.2; MAC definitions vary according to the LAN technology.

```text
        NETWORK LAYER
             |
       +-------------+
       | LLC 802.2   |
       +-------------+
       | MAC         | <-- e.g. Ethernet, WLAN
       +-------------+
       | PHYSICAL    |
       +-------------+
```

**Main IEEE standards:** IEEE 802.1 covers bridging/VLAN and related management fields; IEEE 802.2 is the historic LLC specification; IEEE 802.3 defines Ethernet; IEEE 802.4 historically described Token Bus; IEEE 802.5 historically described Token Ring; IEEE 802.11 covers wireless LAN; IEEE 802.15 addresses wireless personal-area networking; IEEE 802.16 is associated with broadband wireless access. The older Token Bus and Token Ring approaches are chiefly useful for historical comparisons today.

**IEEE 802.3 Ethernet:** Ethernet carries data in frames and uses MAC addresses for local delivery. The ordinary untagged Ethernet frame contains destination address (6 bytes), source address (6 bytes), Type/Length (2 bytes), data and padding (usually 46-1500 bytes for this basic frame) and a 4-byte FCS. The signal framing also uses a 7-byte preamble and one-byte Start Frame Delimiter outside the ordinary minimum frame-length count.

```text
 |Preamble 7|SFD 1|Dest 6|Source 6|Type/Len 2|Data+Pad|FCS 4|
```

The Destination MAC specifies the intended receiver, Source MAC specifies origin, Type/Length identifies encapsulation or length, and FCS holds an error-detecting CRC. The common minimum Ethernet frame size is 64 bytes from Destination MAC through FCS, excluding the preamble and SFD. IEEE 802.1Q VLAN tagging adds fields to some Ethernet frames.

**IEEE 802.11 Wi-Fi:** Wireless LAN stations share a radio channel and commonly use CSMA/CA-related medium access. Access points connect wireless devices into a LAN. Mobility and convenient installation are advantages, whereas radio interference, variable signal strength and shared medium capacity create challenges.

**IEEE 802.5 Token Ring:** In this historical technology, stations participate in a logical ring and a circulating token controls who may send. Because station access is permission-based, ordinary shared-medium contention collisions are avoided. However, the approach requires token management and specialised hardware.

**Comparative analysis:** Classical shared Ethernet used CSMA/CD, but modern switched Ethernet usually has full-duplex point-to-point connections. Wi-Fi uses contention avoidance because wireless nodes cannot reliably perform the same collision detection. Token Ring uses explicit token permission instead of random contention.

**Conclusion.** IEEE 802 standards are foundational to interoperable LAN operation. Ethernet and Wi-Fi are dominant modern examples, while Token Bus and Token Ring demonstrate historically important alternative access strategies.

***

## Q5 (20 marks). Explain FDDI in detail: architecture, token passing, dual rings, stations, working and advantages/disadvantages.

### Complete model answer (English)

**Introduction and definition.** FDDI stands for **Fibre Distributed Data Interface**. It is a historical 100 Mbps networking technology developed for high-speed LAN, campus and backbone communication. FDDI typically uses optical fibre, a timed-token medium-access method and two counter-rotating logical rings. The primary historical standardisation work was through ANSI; FDDI must not be confused with IEEE 802.5 Token Ring.

**Architecture:** In a dual-ring FDDI system, the primary ring normally carries ordinary traffic and the secondary ring provides resilience and may support traffic depending on the configuration. The rings carry information in opposite directions, hence the term *counter-rotating*. A key design purpose is to offer an alternative communication path if a supported failure occurs.

```text
 Primary direction:   A --> B --> C --> D --> A
 Secondary direction: A <-- B <-- C <-- D <-- A

             [A] ------- [B]
              |           |
             [D] ------- [C]
     Two logical rings run in opposite directions.
```

**Working of token passing:** A special token circulates in the ring. A station wanting to transmit waits until token rules permit it to obtain the token, then sends frames according to the permitted transmission period. A timed-token mechanism is used to manage access timing. Stations participate in the forwarding of traffic along the ring. When the permitted transmission interval ends, access passes onward so other stations receive an opportunity.

**Synchronous and asynchronous traffic:** Synchronous services reserve suitable transmission capacity/time to support predictable traffic requirements; asynchronous traffic uses other available access opportunities. These are FDDI MAC service concepts and should not be confused with "synchronous serial bit transmission" in general physical-layer terminology.

**Station types:** A **Dual-Attachment Station (DAS)** connects to both rings and is suitable for devices participating in the resilient backbone. A **Single-Attachment Station (SAS)** uses a suitable single attachment, commonly through a concentrator. A **concentrator** connects multiple stations and assists in maintaining an appropriate ring arrangement.

**Fault recovery:** If a supported single link or device segment failure breaks the normal ring arrangement, neighbouring dual-attachment equipment can wrap the primary and secondary paths to maintain an alternate logical ring. This improves fault tolerance but is not an unconditional guarantee against any combination of failures.

**Technical features:** traditional line rate 100 Mbps; optical fibre as the usual medium; 4B/5B coding associated with physical signalling; token-controlled access; dual rings for resilience; historically useful for campus backbones and high-speed interbuilding communication.

**Advantages:** high performance for its era; fibre immunity to electromagnetic interference; relatively long links with correct fibre equipment; orderly access through token passing; improved resilience through counter-rotating rings and wrap mechanisms.

**Disadvantages:** complex deployment and ring/token management; specialised hardware; cost of older fibre systems; multiple failures can defeat recovery; widespread replacement by higher-speed switched Ethernet.

**Comparison with Ethernet:** FDDI's classic system uses token passing over dual rings, whereas classic shared Ethernet used CSMA/CD and modern Ethernet generally uses switched full-duplex links. Modern Ethernet supports far higher data rates and has become the common LAN/backbone choice.

**Conclusion.** FDDI is an important historical example of high-speed, fault-tolerant, token-based networking. Its key examination points are 100 Mbps, dual counter-rotating rings, timed-token access, DAS/SAS and ring wrapping.

***
## Q6 (20 marks). Explain the functions and design issues of the Data Link Layer. Discuss framing and bit/byte stuffing in detail.

### Complete model answer (English)

**Introduction.** The Data Link Layer is Layer 2 of the OSI model. It operates between the Network Layer and Physical Layer, providing link-specific rules for transmitting frames between adjacent network interfaces. It converts the stream of bits supplied by the Physical Layer into meaningful units and handles functions such as framing, link addressing and error detection. Its exact reliability and flow-control services depend on the protocol.

**Main functions and design issues:**

1. **Services to the Network Layer:** unacknowledged connectionless, acknowledged connectionless and acknowledged connection-oriented services are common theoretical service models.
2. **Framing:** defines beginnings and ends of frames and allows the receiver to interpret the incoming bits as complete data units.
3. **Link addressing:** relevant protocols carry local interface addresses such as source and destination MAC addresses.
4. **Flow control:** regulates how quickly or how many frames a sender may inject so receiver resources are not exceeded.
5. **Error control:** detects corruption and can support retransmission/correction depending on protocol design.
6. **Medium access:** multiple stations sharing a medium need rules determining which one may transmit.

**Framing definition.** Framing is the procedure of grouping a continuous incoming bit stream into separate data-link-layer units called frames. A frame may contain a header, payload, and trailer (for instance, an FCS). The recipient needs unambiguous delimiters or frame lengths.

```text
 NETWORK PACKET
      |
 +----------+----------------+----------+
 | HEADER   |    PAYLOAD     | TRAILER  |
 +----------+----------------+----------+
      DATA LINK FRAME -> physical bit stream
```

**Framing techniques:**

**(i) Character count/length field:** a header indicates data or frame length. Receiver counts bytes. It is straightforward, but a corrupted length can cause a framing problem until recovery.

**(ii) Byte stuffing:** a special flag byte marks frame boundaries. If the same marker appears within payload, the sender inserts an escape byte. For example, using F as flag and E as escape, payload `A F B` is sent as `F | A E F B | F`. Receiver identifies outer F markers and removes the escape E before the data F. It ensures data transparency at byte granularity.

**(iii) Bit stuffing:** in HDLC-style framing, the flag bit pattern is `01111110`. To prevent payload data from looking like a flag, sender inserts a zero after each run of five consecutive ones in the data region. Receiver removes the inserted zeros. For payload `01111110`, stuffed payload becomes `011111010`. The flags remain unstuffed, giving `01111110 | 011111010 | 01111110`.

**(iv) Physical-layer code violations:** special reserved signal patterns of certain physical encodings identify boundaries, provided the technology supports them.

**Flow versus error control:** Flow control is about receiver capacity and speed; error control is about corruption, loss, duplication and recovery. Stop-and-wait can provide flow control. CRC can detect corrupted frames, while ARQ protocols can use acknowledgements, sequence numbers and retransmissions to recover lost frames.

**Practical example:** A laptop sending a frame on an Ethernet link places the destination MAC address and an FCS into the frame. On receipt the link implementation interprets the frame boundary and FCS. A frame with failed FCS is normally discarded rather than silently delivered as valid data.

**Conclusion.** Framing is foundational because a receiver cannot process a continuous bit stream correctly without recognising frame boundaries. Combined with addressing, media access, flow control and error detection, it makes organised communication possible over individual links.

***

## Q7 (20 marks). Explain unrestricted simplex, simplex stop-and-wait and noisy-channel Stop-and-Wait ARQ. Include lost-frame and lost-ACK examples and a numerical.

### Complete model answer (English)

**Introduction.** Elementary Data Link protocols are simplified teaching models that demonstrate the increasing mechanisms needed as channels and receivers become less ideal. Three important stages are the unrestricted simplex protocol, simplex stop-and-wait over an error-free channel, and stop-and-wait ARQ over a noisy channel.

**1. Unrestricted simplex protocol.** Sender transmits frames continuously in only one direction. The medium is assumed to be perfect, no frame is lost or damaged, and receiver is always ready to process new frames. No acknowledgements, timers or sequence numbers are needed. This model is theoretically simple but unrealistic for noisy or rate-mismatched communications.

```text
 Sender -------- F0, F1, F2, F3 ... --------> Receiver
       Error-free link and always-ready receiver assumed.
```

**2. Simplex stop-and-wait protocol.** The channel remains error-free, but the receiver may not be able to accept an unlimited number of frames instantly. Sender transmits a single frame, pauses for permission/acknowledgement and only then sends the next. This is elementary **flow control**, not error recovery under the error-free model.

```text
 Sender                                    Receiver
    |----------- F0 ------------------------->|
    |<------------------------------ ACK -----|
    |----------- F1 ------------------------->|
    |<------------------------------ ACK -----|
```

**3. Stop-and-Wait ARQ for a noisy channel.** Frame or ACK may be lost or damaged. Sender transmits one frame and starts a timer. On successful ACK, it proceeds to next frame; if no valid ACK arrives in time, it retransmits the same frame. Sequence numbers, usually 0 and 1 alternating, help the receiver reject duplicate transmissions after an ACK has been lost.

**Case A: frame lost.** Sender transmits frame with sequence 1 but it is not received. No acknowledgement arrives. Timer expires. Sender retransmits frame 1. Receiver accepts it once and acknowledges it.

**Case B: acknowledgement lost.** Receiver successfully accepts frame 0 and sends ACK, but ACK is lost. The sender times out and resends frame 0. Because the receiver now expects frame 1, it recognises retransmitted frame 0 as a duplicate, does not deliver it again and repeats a suitable acknowledgement.

```text
 Sender                             Receiver
    |--- Frame 0 ---------------------->|
    |<-- ACK 0 -------- X               | (ACK lost)
    |      timeout                      |
    |--- Frame 0 ---------------------->| (duplicate)
    |<-------------------------- ACK 0 --|
```

**Advantages of stop-and-wait ARQ:** simple implementation, one outstanding frame, low receiver buffering, straightforward duplicate rejection. **Disadvantages:** poor link utilisation on high propagation-delay paths, separate ACK or feedback overhead, one frame transmitted at a time, timeout can delay recovery.

**Efficiency numerical:** A frame has 1000 bytes, rate 1 Mbps, one-way propagation time 20 ms and ACK transmission is negligible.

$$T_t=\frac{1000\times8}{10^6}=8\ \text{ms}.$$

$$U=\frac{T_t}{T_t+2T_p}=\frac8{8+40}=\frac16=16.67\%.$$

Only about one-sixth of the ideal cycle is used for data-frame transmission. Increasing frame pipelining through sliding windows can improve utilisation, subject to protocol constraints.

**Conclusion.** The three protocols show why a useful reliable data-link service needs additional mechanisms as assumptions become realistic: unrestricted simplex assumes perfection, stop-and-wait adds flow control, and noisy-channel stop-and-wait ARQ adds feedback, timers, retransmission and sequence numbers.

***

## Q8 (20 marks). Explain Sliding Window Protocols, Go-Back-N and Selective Repeat. Compare their mechanisms and derive the important window formulas.

### Complete model answer (English)

**Introduction.** Sliding window protocols allow a sender to send several frames before receiving all individual acknowledgements. This is known as *pipelining*. It increases utilisation over long-delay links because the sender need not wait for each frame's round-trip acknowledgement before starting another frame.

**Sliding window terms:** Sender window is the set/range of frame numbers allowed to be outstanding; receiver window is the range of incoming frame numbers accepted; acknowledgements move or slide these windows. Sequence numbers identify frames, while timeout and retransmission recover failures. If $m$ bits are used, sequence numbers range from 0 to $2^m-1$ and wrap modulo $2^m$.

```text
 Sender window W=4
 Number:  0  1  2  3  4  5  6
         [---------]
 After ACK0:  [---------]
             1  2  3  4
 Window advances as earlier frames are acknowledged.
```

**Go-Back-N (GBN) ARQ:** Sender can have up to $W$ frames outstanding. Receiver accepts the next expected frame and discards out-of-order frames. ACKs are cumulative. If frame 2 is lost, later frames 3 and 4 are normally discarded even if received; on timeout sender retransmits 2, 3, 4 and other outstanding frames as needed. Sender may keep a timer for oldest outstanding frame.

**Selective Repeat (SR) ARQ:** Both sender and receiver use multiple-frame windows. Receiver may accept and buffer frames 3 and 4 even if frame 2 is missing. It acknowledges received frames individually or selectively. The sender retransmits only frame 2 when loss is established. On receiving 2, receiver can release buffered frames in order.

**Window-size formulas:** Under the standard modulo sequence-number model, **GBN maximum sender window** is $2^m-1$. Its typical receiver window size is 1. In standard equal-window Selective Repeat, the maximum window is $2^{m-1}$, half the sequence-number space, avoiding confusion between old delayed frames and new frames after wraparound.

For $m=3$: GBN window maximum = $2^3-1=7$; Selective Repeat maximum = $2^{3-1}=4$. For $m=4$: 15 and 8 respectively.

**Ideal utilisation formula:** Let $a=T_p/T_t$ be one-way propagation time divided by transmission time. Under error-free operation with negligible feedback overhead:

$$U\approx\min\left(1,\frac{W}{1+2a}\right).$$

For $a=2$ and $W=3$, $U=3/5=60\%$. A window of 5 gives theoretical 100% in this simplified case, whereas stop-and-wait gives $1/5=20\%$.

**Comparison:** GBN is easier to implement at the receiver but may waste bandwidth through retransmission of already-received later frames. Selective Repeat requires receiver buffering and more frame-specific status, but generally performs better when a significant number of isolated frame losses occur. The choice depends on channel conditions, complexity, memory and design priorities.

**Piggybacking:** In a bidirectional link, a receiver may place an acknowledgement in an outgoing data frame, reducing separate ACK-frame overhead. Piggybacking must still ensure acknowledgements are not delayed indefinitely.

**Conclusion.** Sliding windows overcome the chief weakness of Stop-and-Wait. Go-Back-N uses cumulative ACKs and in-order reception; Selective Repeat adds selective acknowledgement and buffering for more efficient retransmission.

***

## Q9 (20 marks). What is error handling in the Data Link Layer? Explain parity, checksum and CRC with a solved CRC example.

### Complete model answer (English)

**Introduction.** Data transmitted over a link may be altered, lost or repeated because of noise, interference and other communication problems. Data Link error handling uses techniques for detecting corruption, correcting some errors and recovering missing data where the link protocol supports reliability. Error *detection*, *correction* and *retransmission-based recovery* are separate concepts.

**Types of errors:** single-bit error (one bit changes); multiple-bit error (several bits change); burst error (corruption spans a contiguous interval of bit positions, though every bit within the span need not be wrong). Lost and duplicated frames also matter in reliable link protocols.

**Parity check:** one extra bit is added to give an even or odd total number of ones. For data `1011001`, number of ones is 4. Even parity bit is 0; odd parity bit is 1. Simple parity can detect a single-bit flip, but errors affecting an even number of bits may preserve parity and pass the check. Two-dimensional parity uses row and column checks and can localise/correct a single-bit error in an appropriate block model.

**Checksum:** a data unit is divided into fixed-size words, commonly added using one's-complement arithmetic. The sender appends the complement of the sum; the receiver verifies that the result matches the expected value. For 8-bit words `10010011` and `01101010`, the sum is `11111101` and one's-complement checksum is `00000010`. A receiver adding all three obtains `11111111` and passes the simple check.

**CRC definition:** Cyclic Redundancy Check uses polynomial division over binary coefficients. The sender appends $r$ zeros to dataword $D$, divides by the generator polynomial $G$ using XOR, and appends the $r$-bit remainder. Receiver divides the complete received codeword by the same generator. Non-zero remainder indicates detectable corruption.

**Solved example:** Dataword $D=1101011011$; generator $G=10011$; generator degree $r=4$.

```text
 Dataword:      1101011011
 Append zeros:  11010110110000
 Generator:     10011
 XOR division remainder = 1110
 Codeword:      11010110111110
```

**Calculation:** Four zeros are appended because the generator has degree four. Modulo-2 division uses XOR rather than arithmetic borrowing. Remainder `1110` is appended to original dataword, resulting in transmitted codeword `11010110111110`. If the codeword is received unchanged, division by `10011` produces zero remainder.

**Properties and limitations:** CRC is particularly effective at detecting many burst-error patterns when the polynomial is selected appropriately. It does not normally provide error *correction*. A zero remainder means the frame passes that CRC check, but some error patterns are theoretically undetectable by a chosen polynomial.

**Error recovery by ARQ:** With detected corruption, a protocol can discard the bad frame and recover using acknowledgement and retransmission. Stop-and-Wait repeats one frame, Go-Back-N may repeat multiple outstanding frames, and Selective Repeat resends selected frames.

**Conclusion.** Parity is simple but limited; checksums provide inexpensive integrity checking; CRC gives strong error detection for data-link frames; and ARQ mechanisms can recover lost/corrupted frames by retransmission where implemented.

***

## Q10 (20 marks). Explain Hamming Code and ARQ protocols for error correction and recovery. Solve a Hamming(7,4) example and compare ARQ schemes.

### Complete model answer (English)

**Introduction.** A communication system can handle errors in two broad ways: add sufficient redundancy to **correct** certain received errors without retransmission, or detect failed delivery and **retransmit** information using Automatic Repeat reQuest (ARQ). Hamming code illustrates the first approach, and Stop-and-Wait, Go-Back-N and Selective Repeat illustrate the second.

**Hamming code definition:** A Hamming code is a linear block code with parity bits arranged to produce unique patterns (syndromes) for different one-bit error locations. In standard Hamming(7,4), four data bits are encoded into seven transmitted bits, three of which are parity bits. It can correct any single-bit error in a seven-bit codeword.

**Number of parity bits:** For $m$ data bits and $r$ parity bits, a basic single-error-correcting Hamming arrangement requires $2^r\ge m+r+1$. For four data bits, $r=3$ gives $8\ge8$ and yields Hamming(7,4).

**Positioning:** Positions 1, 2 and 4 contain parity bits P1, P2 and P4, while positions 3, 5, 6 and 7 contain data D1, D2, D3 and D4. Under even parity, P1 covers 1,3,5,7; P2 covers 2,3,6,7; P4 covers 4,5,6,7.

**Solved Hamming example:** Encode data `1011`.

```text
 Position: 1  2  3  4  5  6  7
 Role:     P1 P2 D1 P4 D2 D3 D4
 Value:    0  1  1  0  0  1  1
 Hamming(7,4) codeword = 0110011
```

P1=0 because positions 3,5,7 contain two ones. P2=1 because positions 3,6,7 contain three ones and one parity one is needed for even parity. P4=0 because positions 5,6,7 contain two ones.

**Correction example:** If position 6 flips, received codeword becomes `0110001`. Parity-check results are P1 group correct, P2 group incorrect and P4 group incorrect. Syndrome bits are $s_1=0$, $s_2=1$, $s_4=1$, representing $4+2=6$. Correct bit 6; original codeword and data are recovered. Basic Hamming(7,4) does not reliably correct double-bit errors; an extended Hamming variant with extra overall parity can enable single-error-correcting/double-error-detecting operation.

**ARQ definition:** Automatic Repeat reQuest relies on sender-retained frame copies, acknowledgements, timing and sequence numbers to resend frames not known to be correctly accepted. Error detection such as a CRC may identify a damaged frame so the receiver discards it.

**Stop-and-Wait ARQ:** Only one frame outstanding; after a timeout the same frame is resent. A one-bit alternating sequence can prevent duplicate delivery when the ACK is lost. It is simple but wastes time on high-delay links.

**Go-Back-N ARQ:** Multiple frames are in flight. Receiver accepts frames in order; when an early frame is missing it discards later out-of-order frames. Sender retransmits missing and following unacknowledged frames when needed. Standard maximum sender window with $m$ sequence bits is $2^m-1$.

**Selective Repeat ARQ:** Receiver buffers valid out-of-order frames. Sender repeats only affected frames based on frame-specific ACK/loss information. Its standard equal-window maximum is $2^{m-1}$ with $m$ sequence bits. SR is more complex but avoids many redundant retransmissions.

**Comparison and conclusion:** Hamming code corrects certain bit errors **without retransmission** using redundancy. ARQ restores correct delivery **by retransmitting** when communication failures are detected or inferred. Stop-and-Wait is simplest, Go-Back-N has simple receiver logic but can waste bandwidth, and Selective Repeat uses buffers and more control state for efficient recovery.

***

\newpage

# PART C - SHORT QUESTIONS, ANSWERS, NUMERICAL PRACTICE AND REVISION

## 16. Forty Important 2-Mark Questions With Answers

1. **Define MAC.** Medium Access Control is the Data Link sublayer that coordinates access to a shared transmission medium and defines link access/frame-related rules for a LAN technology.
2. **What is LLC?** Logical Link Control is the upper traditional IEEE LAN Data Link sublayer associated historically with IEEE 802.2.
3. **Define channel allocation.** Assignment or sharing of communication channel resources among competing stations or users.
4. **Give two static allocation methods.** FDMA and TDMA; CDMA is another important channelisation method.
5. **Give two random access protocols.** Pure ALOHA and CSMA.
6. **Define collision.** Interfering overlapping transmissions that prevent correct reception in a shared-medium model.
7. **Define Pure ALOHA.** A random-access scheme that transmits frames whenever ready and retries after unsuccessful attempts.
8. **Define Slotted ALOHA.** A random-access scheme that permits frames to start only at synchronised slot boundaries.
9. **Pure ALOHA vulnerable period?** Two frame transmission times ($2T$).
10. **Slotted ALOHA vulnerable period?** One frame transmission time ($T$).
11. **Pure ALOHA maximum ideal efficiency?** Approximately 18.4% at $G=0.5$.
12. **Slotted ALOHA maximum ideal efficiency?** Approximately 36.8% at $G=1$.
13. **What is throughput?** Rate of successful useful information delivery per unit time.
14. **Define CSMA.** A medium-access method in which a station senses channel activity before attempting transmission.
15. **What is 1-persistent CSMA?** A station transmits immediately when it senses idle medium; while busy it continues waiting/sensing.
16. **What is non-persistent CSMA?** A station sensing a busy medium waits a random interval before sensing again.
17. **What is p-persistent CSMA?** A slotted CSMA variant that transmits with probability $p$ when an eligible idle slot occurs.
18. **Expand CSMA/CD.** Carrier Sense Multiple Access with Collision Detection.
19. **Expand CSMA/CA.** Carrier Sense Multiple Access with Collision Avoidance.
20. **Which IEEE family covers Ethernet?** IEEE 802.3.
21. **Which IEEE family covers WLAN?** IEEE 802.11.
22. **What is IEEE 802.5 known for?** Historical Token Ring networking.
23. **Expand FDDI.** Fibre Distributed Data Interface.
24. **Traditional FDDI data rate?** 100 Mbps.
25. **FDDI topology and access?** Dual counter-rotating rings with token-based access.
26. **What is framing?** Dividing a stream of bits into identifiable Data Link frames.
27. **What is byte stuffing?** Inserting escape bytes so data values cannot be confused with control delimiters.
28. **What is bit stuffing?** Inserting bits according to rules so delimiter patterns cannot appear unescaped in data.
29. **Common HDLC-style flag pattern?** `01111110`.
30. **What is flow control?** Regulating transmission to avoid overloading receiver capacity.
31. **What is error control?** Mechanisms for detecting or recovering communication errors such as corruption or loss.
32. **What is ACK?** An acknowledgement indicating successful receipt according to protocol-defined semantics.
33. **What is ARQ?** Automatic Repeat reQuest; a family of retransmission-based reliability techniques.
34. **Why use a timeout?** To trigger recovery when expected acknowledgement/progress does not occur.
35. **Why use sequence numbers?** To distinguish frames, order them and identify duplicates after retransmission.
36. **What is a sender window?** Sequence-number range currently permitted to be sent/outstanding.
37. **What is GBN receiver window traditionally?** One frame, the next expected in-order frame.
38. **What is Selective Repeat's key feature?** Buffer out-of-order frames and retransmit only affected frames.
39. **CRC used for?** Error detection based on modulo-2 polynomial division, not ordinary bit correction.
40. **Hamming(7,4) means?** A seven-bit codeword containing four data bits and three parity bits, correcting one-bit errors in the standard arrangement.

## 17. Important 5-Mark and 10-Mark Practice Questions

**5 marks each:** Define MAC and LLC; static versus dynamic allocation; list three persistence variants of CSMA; Pure versus Slotted ALOHA; IEEE 802.3 versus 802.11; FDDI characteristics; byte stuffing versus bit stuffing; flow versus error control; role of ACK and timeout; GBN versus SR; parity versus CRC; difference between FEC and ARQ.

**10 marks each:** Explain Pure ALOHA throughput; explain Slotted ALOHA and collision diagram; write notes on 1-persistent, non-persistent and p-persistent CSMA; explain Ethernet frame with labelled diagram; explain FDDI dual rings and token; explain framing methods; write elementary data-link protocol stages; explain sliding window with numerical; explain Go-Back-N flow with loss; explain Selective Repeat buffering; perform CRC division; solve Hamming(7,4) encoding and correction.

## 18. Ten Solved Numericals for Unit II

**N1.** At $G=0.5$, calculate Pure ALOHA throughput. **Answer:** $S=0.5e^{-1}=0.18394$, approximately **18.4%**.

**N2.** At $G=1$, calculate Slotted ALOHA throughput. **Answer:** $S=1/e=0.36788$, approximately **36.8%**.

**N3.** If one equal frame transmits in 12 ms, find vulnerable periods. **Answer:** Pure = **24 ms**; Slotted = **12 ms**.

**N4.** A 1600-byte frame uses an 8 Mbps link. Find frame transmission time. **Answer:** $1600\times8/(8\times10^6)=0.0016$ s = **1.6 ms**.

**N5.** A stop-and-wait link has $T_t=5$ ms, $T_p=15$ ms, negligible ACK overhead. Find utilisation. **Answer:** $5/(5+30)=1/7\approx\mathbf{14.29\%}$.

**N6.** Sliding window $W=4$, $T_p/T_t=1.5$, no errors. Find ideal utilisation. **Answer:** $U=\min(1,4/(1+3))=\mathbf{100\%}$.

**N7.** For Go-Back-N with $m=4$ sequence bits, find largest standard sender window. **Answer:** $2^4-1=\mathbf{15}$.

**N8.** For standard Selective Repeat with $m=5$, find largest equal sender/receiver window. **Answer:** $2^{5-1}=\mathbf{16}$.

**N9.** Data is `1100100`. Find even and odd parity bits. **Answer:** three ones, so even parity **1**, odd parity **0**.

**N10.** Hamming(7,4), data `1011`, even parity. Find encoded codeword. **Answer:** `0110011`. If bit 6 flips, syndrome = **6**, so flip bit 6 back.

## 19. Extra Numerical Questions for Self-Practice (Solutions Provided)

**P1.** For a 2 Mbps link, what is ideal maximum Pure ALOHA successful bit rate? **Solution:** $2\times10^6\times1/(2e)\approx367,879$ bits/s, or **368 kbps**, before extra protocol overhead.

**P2.** At $G=2$ in Slotted ALOHA, what is throughput? **Solution:** $2e^{-2}\approx0.27067=27.07\%$. The throughput is lower than the maximum because load is too high.

**P3.** Stop-and-wait with $T_t=10$ ms and $T_p=30$ ms; find link utilisation. **Solution:** $10/(10+60)=1/7=14.29\%$.

**P4.** Sliding window $W=3$, $T_t=10$ ms and $T_p=20$ ms. **Solution:** $a=2$, so $U=3/(1+4)=60\%$.

**P5.** What minimum sequence bits support Go-Back-N sender window 10? **Solution:** $2^m-1\ge10$; $m=4$ gives 15. **Answer 4 bits**.

**P6.** What minimum sequence bits support SR window 10? **Solution:** $2^{m-1}\ge10$; $m=5$ gives 16. **Answer 5 bits**.

**P7.** Four-bit data requires how many Hamming parity bits? **Solution:** $2^r\ge4+r+1$. For $r=3$, $8\ge8$ holds. **Answer 3 parity bits**.

**P8.** Generator binary `1011` has how many CRC remainder bits? **Solution:** Generator degree is 3; add **3 zeros** before division and append **3-bit remainder**.

**P9.** Original payload `01111110` in bit-stuffed HDLC-style field? **Solution:** after fifth 1 insert zero -> **`011111010`**.

**P10.** Data `1011001` uses even parity, one-bit error occurs at a data bit. Does the parity check detect it? **Solution:** yes, an odd number of flips changes total parity. **But two flips may be undetected**.

## 20. Seven-Day Unit II Revision Plan

| Day | Study focus | Written output / self-check |
|---|---|---|
| 1 | MAC, LLC, channel allocation, FDMA/TDMA/CDMA | Draw MAC hierarchy and answer Q1 without book |
| 2 | ALOHA protocols and throughput | Derive both throughput maxima; solve N1-N3 |
| 3 | CSMA, Ethernet/IEEE standards | Draw CSMA/CD flowchart and Ethernet frame |
| 4 | FDDI | Draw dual rings, explain DAS/SAS and token recovery |
| 5 | Data Link services, framing, elementary protocols | Solve bit stuffing, draw lost ACK sequence |
| 6 | Sliding window, GBN, SR | Draw F2 loss in GBN/SR; solve window numericals |
| 7 | Error handling, CRC, parity, Hamming | Solve CRC/Hamming; attempt two complete 20-mark answers |

## 21. Final Unit II One-Page Memory Sheet

**Media sharing:** static = divide channel; dynamic = compete/take turns. **ALOHA:** Pure sends anytime -> $2T$ vulnerable -> $18.4\%$ max. Slotted starts at slot boundaries -> $T$ vulnerable -> $36.8\%$ max. **CSMA:** sense first; **CSMA/CD:** collision detection in classic half-duplex Ethernet; **CSMA/CA:** collision avoidance in Wi-Fi. **IEEE:** 802.3 = Ethernet, 802.11 = Wi-Fi, 802.5 = historical Token Ring. **FDDI:** 100 Mbps; fibre; dual counter-rotating rings; timed-token access; historic backbone.

**Data Link:** framing, link addressing, medium access, error/flow control depending on protocol. **Bit stuffing:** insert `0` after five consecutive `1`s in protected data. **Stop-and-Wait:** one outstanding frame; timeout triggers resend; sequence bits reject duplicates. **Sliding window:** multiple outstanding frames; GBN discards out of order and replays suffix; SR buffers out of order and resends individual missing frames. **Windows:** GBN $2^m-1$, SR $2^{m-1}$ in standard models. **Error detection:** parity, checksum, CRC; **correction:** Hamming; **recovery:** ARQ. **CRC:** append $r$ zeros, XOR divide, attach remainder, receiver checks remainder.

**A note about exam writing:** Underline definitions and keywords, label diagrams clearly, state assumptions before each numerical and write the final answer with units. Long answers should be adaptable to the exact question; do not write unrelated extra topics just to increase length.
