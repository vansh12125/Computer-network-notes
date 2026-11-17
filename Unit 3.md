> **Exact syllabus focus - Unit III (Network Layer):** point-to-point networks; routing; congestion control; internetworking and TCP/IP; IP packet; IP address; IPv6. The extra explanations of network-layer design, routing algorithms, RIP/OSPF/BGP, IPv4 subnetting/fragmentation, ICMP/ARP/NAT and related examples are supporting topics used to understand the prescribed syllabus, not separately prescribed headings.

> **Your reading method:** STEP 1: **HINGLISH** (real-life story, what/why/how). STEP 2: **COMPLETE ENGLISH** (definition, working, diagrams, features, examples and comparisons). STEP 3: **NUMERICAL/TRACE** (where applicable). STEP 4: **20-MARK EXAM ANSWER**. Do not skip the English section: it is written in straightforward exam language you can practise reproducing.

> **Important-question accuracy:** The 20-mark questions are syllabus-based *practice predictions*, not authenticated previous-year questions or guaranteed exam questions. IP and routing examples are simplified for learning; real implementations contain additional details.


# PART A - COMPLETE UNIT III THEORY: HINGLISH FIRST, ENGLISH AFTER

## 1. Network Layer - The Big Picture

### 1.1 Hinglish: a parcel going from Lucknow to Delhi

Maan lo tum Lucknow se Delhi mein apne friend ko parcel bhejna chahte ho. Parcel tumhari building se local courier office jaata hai, phir Lucknow sorting centre, phir Delhi sorting centre, aur finally friend ke address tak. **Har centre ko next centre decide karna hota hai**. Ye poora route ek single cable nahi hota: beech mein kaafi links aur cities hote hain. Networking mein bhi sender se receiver tak data ko different networks ke through le jaane ka kaam mainly **Network Layer** karti hai.

- Tumhara **home Wi-Fi** = ek local network.
- College ka network = doosra network.
- Internet par beech ke **routers** = courier sorting centres.
- **IP address** = logical destination address; packet kis network/host ki taraf jaana hai.
- **Routing** = map par suitable raasta choose karna.
- **Forwarding** = ek router ka packet ko actual next interface par bhejna.
- **Congestion** = bahut saare parcels ek hi road par; traffic jam, queue aur delay.

**Golden memory line:** Data Link Layer ek *hop* par frame bhejti hai; Network Layer multiple hops/networks ke across packet ko route karti hai. A hop often means one forwarding step from one router/device to the next.

```text
       NETWORK A          INTERNET / MULTIPLE ROUTERS          NETWORK B
  +-------------+      +-----+      +-----+      +-----+     +-------------+
  | Your laptop |------| R1  |------| R2  |------| R3  |-----| Web server  |
  | 10.0.0.5    |      +-----+      +-----+      +-----+     | 203.0.113.8 |
  +-------------+         \___________ Path ___________/     +-------------+
        |                 Routers read IP header                     |
  LAN frames carry     and choose the next hop                LAN frames carry
     IP packets                                                    IP packets

  Source IP and destination IP identify logical endpoints.
  Link-layer source/destination addresses change on each hop.
```

### 1.2 Complete English - definition and objectives

The **Network Layer** is Layer 3 of the OSI reference model. It provides the functions needed to transfer packets from a source device to a destination device across one or more interconnected networks. When multiple paths exist, the Network Layer supports the choice of suitable paths and the forwarding of packets through intermediate routers.

Its major objectives are **logical addressing, routing, packet forwarding and internetworking**. Depending on the protocol design, the wider network architecture may also provide fragmentation-related processing, diagnostic error reporting, priority handling and mechanisms associated with quality of service. In the Internet, IP offers a **best-effort, connectionless datagram service**: it does not promise delivery, ordering, or lack of duplication. End-to-end reliability is normally handled by an upper-layer protocol such as TCP when the application requires it.

**Important functions:**

1. **Logical addressing:** devices/interfaces receive IP addresses used for locating destination networks and hosts.
2. **Routing:** routers and routing protocols determine paths according to costs, policies and network information.
3. **Packet forwarding:** an individual router uses its forwarding table to send a received packet to a next hop.
4. **Internetworking:** different physical and link technologies can communicate through a common network-layer protocol such as IP.
5. **Packet lifetime limitation:** IPv4 Time To Live (TTL) and IPv6 Hop Limit limit endless forwarding loops.
6. **Fragmentation behaviour:** IPv4 routers may fragment packets when permitted; IPv6 routers do not fragment transit packets.
7. **Diagnostics and control support:** ICMP reports certain errors and provides diagnostic messages; ICMP itself does not make IP delivery reliable.

### 1.3 Network Layer versus Data Link versus Transport

| Basis | Data Link (Layer 2) | Network (Layer 3) | Transport (Layer 4) |
|---|---|---|---|
| Main scope | One link / local LAN | Across multiple networks | End-to-end applications/processes |
| Typical unit | Frame | IP packet/datagram | TCP segment or UDP datagram |
| Example address | MAC address | IP address | Port number (TCP/UDP) |
| Typical device/function | Ethernet switch | Router | TCP/UDP stack on hosts |
| Typical question | Which local interface? | Which network and next hop? | Which application; reliable transfer? |

**Exam paragraph:** The Network Layer sits above the Data Link Layer and below the Transport Layer. It uses the transmission service supplied by individual links while providing packet-based communication between hosts located on different networks. The Transport Layer can add application-to-application functions such as reliable ordered delivery.

### 1.4 Packet, frame, segment and hop - no confusion

A web browser may create application data. TCP forms a segment (in a TCP example). IP adds an IP header to create a packet. Ethernet adds its own header and trailer to make a frame. At the next router the incoming Ethernet frame is processed, but the router then sends the IP packet inside a **new outgoing link-layer frame** suitable for the next link. This is why **MAC addresses are hop-specific while IP addresses identify network endpoints** (apart from address translation or tunnelling).

```text
APPLICATION DATA             [ REQUEST / MESSAGE ]
TRANSPORT (TCP)              [ TCP HDR | APP DATA ]
NETWORK (IPv4)               [ IP HDR | TCP HDR | APP DATA ]
LINK (Ethernet)          [ ETH HDR | IP HDR | TCP HDR | DATA | FCS ]
PHYSICAL                     101010... signals on medium

ROUTER:
  Old link frame --> [IP packet] --> New link frame
                      route lookup
```

***

## 2. Point-to-Point Networks - Meaning, Design and Working

### 2.1 Hinglish: one private road between two buildings

Socho do college buildings A aur B ke beech ek private underground fibre cable hai. Cable ke **sirf do ends** hain: ek Building A aur doosra Building B. Ye **point-to-point link** hai. Agar A kisi third building C tak jaana chahe, aur A-C direct cable nahi, to data ko B se forward karwana pad sakta hai. Jab bahut saare point-to-point links routers ke through connect ho jaate hain, tab **point-to-point network** banta hai.

**Ek important confusion:** *Point-to-point* ka matlab **physical/logical connection ke two endpoints** hai. *Peer-to-peer* ka matlab computers ka application/resource-sharing architecture hai. Dono alag terms hain. Ek point-to-point link par client-server traffic bhi chal sakta hai.

Imagine:

- A-to-B direct dedicated telephone/fibre connection = point-to-point.
- 100 students ek shared wireless channel use karein = shared/broadcast medium, na ki har station ke liye ek hi dedicated P2P link.
- A home router and ISP router may be joined by a point-to-point logical WAN circuit.

```text
SINGLE POINT-TO-POINT LINK:
        [ Router A ] ================= [ Router B ]
                           one link

NETWORK OF POINT-TO-POINT LINKS:
                   R2 -------- R4
                  /  \         / \
   Host A -- R1 --    \-- R3 --/   R5 -- Host B
                  \____________/
       Intermediate nodes choose next hops for packets.
```

### 2.2 Complete English definition

A **point-to-point communication link** directly connects two endpoints. In a **point-to-point network**, many such links can interconnect routers or switching nodes, making a graph of possible paths between source and destination. Each link has properties such as capacity, delay, error rate and operational cost. Packets may cross a sequence of links to reach the destination.

**Main characteristics:**

1. A direct link connects exactly two endpoints at the link level.
2. A dedicated point-to-point link does not require shared-medium collision arbitration between many stations on the same link.
3. There can be several alternative routes across a larger network of interconnected routers.
4. Intermediate devices receive packets, inspect relevant headers, decide outgoing interfaces and forward them.
5. A link can be simplex, half duplex or full duplex according to the underlying technology; point-to-point alone does not determine duplex mode.
6. WANs and router interconnections often contain point-to-point links.

### 2.3 Store-and-forward packet switching

In a simplified **store-and-forward** model, an intermediate router receives an entire packet into a buffer, examines it, and then forwards it. Real high-speed forwarding hardware may use optimised pipelines or cut-through behaviour in some contexts, but the store-and-forward model is useful for examination calculations.

```text
Time moves downward

 Host A            Router R1             Router R2          Host B
   |--- Packet P ---->|                     |                 |
   |                  |---- Packet P ------>|                 |
   |                  |                     |---- Packet P -->|
   |                  |                     |                 |

  Each hop incurs possible processing, queueing, transmission
  and propagation delays. No single end-to-end cable is required.
```

**Advantages:** flexible paths through interconnected links; faults can sometimes be bypassed using alternate paths; point-to-point dedicated links avoid local shared-medium contention; capacity and routing can be planned per link. **Disadvantages:** each inter-router hop can introduce delay and complexity; a failed bridge link may isolate portions of a sparse topology; buffering and routing resources are needed; many dedicated links can increase cost.

### 2.4 Links, nodes and hop-count numerical

For a route `A -> R1 -> R2 -> R3 -> B`, there are **4 links/hops** (A-R1, R1-R2, R2-R3, R3-B) and **3 intermediate routers**. Count the lines between consecutive devices, not simply the number of router names.

**Solved delay example:** A 1,000-byte packet travels across three store-and-forward links, each at 1 Mbps. Ignore propagation/processing/queueing and assume no pipelined competition. Packet = `1000 x 8 = 8000 bits`; transmission per link = `8000/1,000,000 = 0.008 s = 8 ms`; three links require approximately `3 x 8 = 24 ms` for the packet to be completely received at the final device. With positive propagation delays, add propagation for all three links.

### 2.5 Point-to-point protocol (PPP) - useful related concept

**PPP (Point-to-Point Protocol)** is a Data Link Layer protocol used to carry network-layer packets over point-to-point links. Its historical and educational functions include framing, Link Control Protocol (LCP) negotiation and Network Control Protocols (NCPs) for network-layer options; authentication options such as PAP/CHAP may be used depending on the implementation. Point-to-point **network** (topology) and **PPP** (specific link protocol) are not the same thing.

**Example PPP-style framing (conceptual):**

```text
  +------+---------+---------+----------+---------+--------+------+
  | Flag | Address | Control | Protocol | Payload |  FCS   | Flag |
  +------+---------+---------+----------+---------+--------+------+

  Flag commonly: 0x7E. Protocol field identifies encapsulated protocol.
  FCS detects errors in transmitted frames.
```

***

## 3. Network Layer Design: Datagram Service and Virtual Circuits

### 3.1 Hinglish: courier parcels versus reserved train seats

Do ways se travel imagine karo:

1. **Datagram:** tum Delhi ke liye 5 separate parcels bhejte ho. Har parcel par complete address likha hai. Courier har parcel ke liye route independently choose kar sakta hai. Koi parcel alag road se bhi ja sakta hai, isliye order change ho sakta hai.
2. **Virtual circuit:** tum ek planned connection/route establish karte ho. Uske baad traffic us established logical connection ka identifier use karta hai. Network ke switches/routers ko connection state maintain karni pad sakti hai.

Internet ka **IP network layer datagram approach** use karta hai. TCP reliable logical connection banata hai, lekin TCP ka connection **transport layer** par hota hai; isse underlying IP service virtual-circuit network nahi ban jaati.

### 3.2 Complete English explanation

The design of a network layer may offer a **connectionless datagram service** or a **connection-oriented virtual-circuit service**. In a connectionless service, each packet includes sufficient destination information for independent forwarding, and forwarding decisions may vary as routes change. In a virtual-circuit service, the path or logical connection is established before data transfer, and intermediate devices may keep mapping/state for the logical connection.

| Comparison | Datagram subnet | Virtual-circuit subnet |
|---|---|---|
| Setup before data | Usually no | Generally required |
| Packet forwarding | Each packet individually | Along established logical circuit |
| Address in packet | Full destination network address | Often shorter connection identifier |
| Router/switch state | Routes, not per-flow virtual circuit state | May hold per-connection state |
| Path change | Packets may use alternate routes | Connection may require repair/re-establishment |
| Examples | Internet Protocol | ATM virtual circuits, Frame Relay (historical) |

**Store-and-forward vs virtual circuit:** store-and-forward is a forwarding technique/model; virtual-circuit is a logical service/connection model. They are not mutually exclusive categories.

### 3.3 Design responsibilities in an internetwork

When networks of different types must communicate, network-layer design considers addressing, choice of route, transfer across different link technologies, packet size limits (MTU), possible fragmentation, security and management, and handling of congestion. The same IP packet can travel over Ethernet, Wi-Fi or other links because each link wraps it in a frame suitable for that link.

***

## 4. Routing and Forwarding - Core Network Layer Concepts

### 4.1 Hinglish: Google Maps decides, driver takes the turn

Maan lo tum Lucknow se Kanpur jaana chahte ho. Google Maps poora route choose karta hai = **routing**. Tum har intersection par left/right/straight decide karte ho based on next instruction = **forwarding**. Router bhi table consult karke **next hop** decide karta hai, poora Internet map har packet par manually calculate nahi karta.

Routing **control-plane** decision hai (paths kaise learn/select honge). Forwarding **data-plane** action hai (packet ko actual output par bhejna). Routing table aur forwarding table related hain but same abstraction hone zaroori nahi.

### 4.2 Complete English explanation

**Routing** is the process of determining paths through interconnected networks, using topology information, metrics and sometimes policy rules. **Forwarding** is the process performed for a particular packet at a router: inspecting the destination, finding the matching forwarding-table entry and sending the packet through the chosen outgoing interface (possibly after relevant processing).

A router forwarding decision commonly uses **longest prefix matching**: among matching IP prefixes, the most specific matching prefix wins, subject to protocol and policy constraints.

```text
 Example forwarding table (illustrative)
 +------------------+----------------+
 | Destination      | Next hop       |
 +------------------+----------------+
 | 10.2.0.0/16      | Router B       |
 | 10.2.3.0/24      | Router C       |
 | 0.0.0.0/0        | Default route  |
 +------------------+----------------+

 Packet to 10.2.3.9 matches /16 AND /24.
 The /24 route is more specific, so Router C is selected.
```

### 4.3 What is a routing table?

A routing table stores destination network/prefix information and how to reach it: outgoing interface, next-hop router, metric or other control details. Router software may learn entries from **directly connected networks**, **static configuration** and **dynamic routing protocols**. A **default route** is used when there is no more specific matching route.

A **routing metric** is a numerical or policy-dependent measure used to compare candidate routes: hop count, cost based on bandwidth, delay, reliability, administrative preference or combinations. Different protocols calculate metrics differently, so costs from unrelated protocols must not be compared blindly.

### 4.4 Graph model for routing

A network can be represented by a graph `G = (V, E)`, where vertices `V` are routers and edges `E` are links. A non-negative weight on an edge may represent cost or delay. A **shortest path** minimises the *sum of selected edge weights*, not necessarily physical distance in kilometres.

```text
                 2
            A -------- B
            | \        | \
           5|  \1     2|  \3
            |   \      |   \
            C --- D --- E --- F
               3     1     2

 Sample for practice: treat numbers as link costs.
 Lower total cost is preferred in this illustrative exercise.
```

### 4.5 Static routing versus dynamic routing

**Static routing:** an administrator manually enters route information. Useful for small stable networks, default routes and specific security/policy needs. Advantages: simple, predictable and no protocol update traffic. Disadvantages: network changes often require manual adjustment, can become hard to maintain.

**Dynamic routing:** routers exchange or derive route information using a routing protocol. Advantages: can adapt to failures and topology changes, scales better in many networks. Disadvantages: consumes CPU, memory, bandwidth and requires convergence time, security precautions and correct configuration.

### 4.6 Optimality principle and shortest-path tree

Under a consistent additive shortest-path model, if router A-to-D shortest route passes through B, then the B-to-D subpath is also shortest between B and D. This **optimality principle** underpins shortest-path computation. The union of chosen shortest routes from a source gives a **shortest-path tree** rooted at that source, subject to tie-breaking.

***

## 5. Shortest Path Routing - Dijkstra Algorithm with Solved Example

### 5.1 Hinglish: cheapest roads choose karna

Maan lo tumhe A se E tak jaana hai. Roads ke costs diye hain. Sirf next road cheap hone ka matlab ye nahi ki total journey cheapest hogi. Dijkstra algorithm **total cumulative cost** maintain karta hai. Shuru mein A ka cost 0 aur baaki nodes ka infinity. Har step mein lowest currently tentative distance wala unvisited node pick karte hain aur uske neighbours ke costs update karte hain.

**Easy memory:** *Dijkstra = nearest confirmed node choose + neighbours relax.* **Relax** ka matlab: agar naya route purane record se better hai, to cost update kar do.

```text
          2             3
      A ------- B ------------- E
      |         |               |
     5|        1|               |1
      |         |               |
      C ------- D --------------+
          2

Edges: A-B=2, A-C=5, B-D=1, C-D=2, B-E=3, D-E=1
Find shortest path from A to E.
```

### 5.2 English algorithm and properties

**Dijkstra's shortest-path algorithm** finds minimum-cost routes from one source to all reachable vertices in a graph with **non-negative edge weights**. It maintains tentative shortest distances and repeatedly finalises the unvisited vertex with smallest tentative distance, relaxing its outgoing edges.

**Steps:**

1. Initialise `dist[source] = 0` and `dist[v] = infinity` for other vertices.
2. Mark every vertex unvisited; set predecessor pointers to empty.
3. Select the unvisited vertex with smallest tentative distance.
4. For each neighbour `v`, compute `candidate = dist[u] + weight(u,v)`.
5. If candidate improves `dist[v]`, update the distance and predecessor.
6. Mark `u` visited/finalised; repeat until destination is finalised or reachable vertices are done.

**Worked table** (graph shown above):

| Step | Settled router | d(A) | d(B) | d(C) | d(D) | d(E) |
|---|---|---:|---:|---:|---:|---:|
| Initial | none | 0 | infinity | infinity | infinity | infinity |
| 1 | A | 0 | 2 | 5 | infinity | infinity |
| 2 | B | 0 | 2 | 5 | 3 | 5 |
| 3 | D | 0 | 2 | 5 | 3 | 4 |
| 4 | E | 0 | 2 | 5 | 3 | 4 |

At E, the minimum cost is **4**, and the predecessor chain is `E <- D <- B <- A`. Therefore **A -> B -> D -> E**, with cost `2 + 1 + 1 = 4`.

**Strengths:** finds optimal non-negative shortest paths; suitable for link-state routing; can use a priority queue. **Limitations:** requires a view of the relevant graph and correct consistent link costs; topology updates may require recomputation; it is not generally valid for negative-weight edges. With a binary heap and adjacency lists a standard implementation is typically `O((V+E) log V)`, while a simple array implementation is `O(V^2)`.

### 5.3 Dijkstra versus simple BFS

A **breadth-first search (BFS)** gives the shortest number-of-edges path when all links have equal cost. Dijkstra is used when non-negative link weights can differ. A route with 2 hops might have cost 100, while a route with 4 hops might have cost 8: Dijkstra selects cost 8 if total weight is the objective.

### 5.4 Exam-ready pseudocode

```text
DIJKSTRA(graph, source):
    for every vertex v:
        dist[v] = infinity
        parent[v] = null
    dist[source] = 0
    priority_queue.push((0, source))

    while priority_queue not empty:
        (cost, u) = pop_min()
        if cost != dist[u]: continue
        for each (v, weight) from u:
            if dist[u] + weight < dist[v]:
                dist[v] = dist[u] + weight
                parent[v] = u
                push((dist[v], v))
    return dist, parent
```

***
## 6. Distance Vector Routing and Bellman-Ford

### 6.1 Hinglish: har friend se nearby routes poochhna

Maan lo tumhari building ke baahar road band hai. Tum poore India ka live road map nahi rakhte. Tum apne nearby friends se poochte ho, "Delhi tak tumhe kitna cost lagta hai?" Har friend jo estimate bataata hai, usme **apne ghar se us friend tak ka cost add** karke tum decide karte ho ki best neighbour kaun hai. Distance Vector Routing (DVR) bhi similar principle use karta hai.

Har router apne neighbouring routers ko destination-wise distance estimates send karta hai. Router check karta hai: "Neighbour ke paas destination ka cost 3 hai, aur mere se neighbour tak link cost 2 hai, to mera possible cost 5 hoga." Agar current cost 8 tha, to 5 update kar dega.

**Important:** Router ko har time complete network topology pata hona zaroori nahi; wo neighbour ke advertised distances par kaafi depend karta hai. Galat ya outdated information se routing loops ya **count-to-infinity** problem aa sakti hai.

```text
           link=2             link=3
       A -------------- B -------------- C
       |                                  |
       +------------ link=10 ------------+

A to C options:
  Direct A-C = 10
  Via B = A-B (2) + B-C (3) = 5  <-- better

A's table after learning from B:
  Destination C | Next hop B | Cost 5
```

### 6.2 Complete English - distance vector principle

**Distance Vector Routing** is a distributed routing approach in which every router maintains estimates of the cost to reach destinations and the next hop to use. Routers periodically or when changes occur exchange route information with neighbours, depending on the protocol. Each router updates its own estimates using neighbour advertisements.

**Bellman-Ford recurrence:**

$$
D_x(y)=\min_{v\in N(x)}\{c(x,v)+D_v(y)\}
$$

Where `D_x(y)` is router x's estimated minimum cost to destination y; `N(x)` is the set of x's neighbours; `c(x,v)` is the cost of x-to-v link; and `D_v(y)` is neighbour v's current advertised distance to y. For the source's own destination, cost is zero.

**Main operating steps:**

1. Each router initially knows its directly connected links and their costs.
2. It maintains a table with destinations, cost estimates and next hops.
3. It receives advertised distances from neighbours.
4. For each destination, it computes `cost to neighbour + neighbour's distance`.
5. It selects the lowest valid candidate and updates the next hop.
6. Updates propagate and the network gradually converges if conditions permit.

**Example:** A-B cost 2; B-C cost 3; A-C cost 10. B advertises cost 3 to C. A calculates `2 + 3 = 5`, chooses B as next hop, and prefers that over its direct route cost 10.

### 6.3 Count-to-infinity problem

Imagine three routers: `A -- B -- C`, with C's network behind C. Initially A reaches C via B. If B loses the C link but A still says "I can reach C in 2 hops," B may incorrectly conclude that it can reach C **via A**. However, A itself relies on B: they can keep misleading one another by increasing their advertised metric. This is the **count-to-infinity problem**.

```text
    Before failure: A ---- B ---- C(network)
                    A:2   B:1

    B--C fails. If stale advertisements remain:
      A: "C is 2 hops away via B" (stale)
      B: "Then via A it is 3 hops"
      A: "Then via B it is 4 hops" ...

      Routing loop / rising metric until a configured limit.
```

**Mitigation mechanisms:**

- **Split horizon:** do not advertise to a neighbour a route learned from that neighbour.
- **Poison reverse:** advertise such a route back with an unreachable metric instead of a usable one.
- **Route poisoning:** announce a failed route as unreachable.
- **Triggered updates:** send information soon after important changes, rather than waiting for the next periodic update.
- **Hold-down or timing mechanisms:** in some protocols, help avoid accepting unstable updates too quickly.

These mechanisms help but do not eliminate every possible loop in every complex topology.

### 6.4 RIP as a distance-vector example

**Routing Information Protocol (RIP)** is a traditional distance-vector routing protocol. It uses **hop count** as the routing metric, and 16 represents unreachable in the familiar RIP convention, so its usable maximum route hop count is 15. RIP is easy to understand but has scaling and convergence limitations relative to more modern interior protocols.

**Advantages of DVR:** distributed operation, relatively simple calculations, limited need for a complete topology map. **Limitations:** loops or transient inconsistencies, possible slow convergence, metric limitations and neighbour-update overhead.

***

## 7. Link State Routing - More Complete Network Map

### 7.1 Hinglish: local rumours versus shared Google Map

Distance Vector mein tum neighbours se poochte the, "Delhi ka cost batao." Link State mein har router apne **direct link neighbours and costs** ki information sab relevant routers ke saath share karta hai. Har router apna **network map** banata hai aur Dijkstra run karta hai.

College network example: Admin building ka router directly Library aur Lab se connected hai. Wo bolta hai, "Meri Library-link cost 2 aur Lab-link cost 5 hai." Library ka router apni links batata hai. Jab sab routers ki valid link information sabko mil jaati hai, har router locally shortest route calculate kar sakta hai.

### 7.2 Complete English explanation

**Link State Routing** is a routing approach where routers discover the state and cost of their immediate links and distribute that information through a routing domain. Routers build a **link-state database (LSDB)** representing the topology, and each computes shortest routes based on that database, commonly using Dijkstra's Shortest Path First (SPF) algorithm.

**Typical sequence:**

1. **Neighbour discovery:** identify adjacent routers through protocol-specific exchanges.
2. **Link measurement/assignment:** determine relevant link properties or configured costs.
3. **Link-state advertisement (LSA):** describe local links and related state.
4. **Reliable flooding:** distribute advertisements to the routing domain with duplicate/staleness controls.
5. **LSDB maintenance:** maintain a consistent current network topology database.
6. **SPF calculation:** compute best routes and install suitable results into the routing/forwarding system.

```text
 Router R1 knows: R1-R2 cost 2; R1-R3 cost 5
 Router R2 knows: R2-R1 cost 2; R2-R4 cost 1
 Router R3 knows: R3-R1 cost 5; R3-R4 cost 2
 Router R4 knows: R4-R2 cost 1; R4-R3 cost 2

           R1 -----2----- R2
           |               |
           5               1
           |               |
           R3 -----2----- R4

LSAs are flooded -> routers build a shared topology view.
Each router uses SPF to calculate its own routes.
```

### 7.3 OSPF - an example of link-state routing

**Open Shortest Path First (OSPF)** is a widely taught Interior Gateway Protocol (IGP) that uses link-state information and SPF computation. Its routes are typically based on configured or derived costs; OSPF can divide a routing system into **areas** to manage scale. The backbone is **Area 0** in conventional OSPF design. OSPF supports hierarchical organisation, neighbour formation, database synchronisation and topology-change information.

**Example:** R1-to-R4 via R2 costs `2+1=3`; via R3 costs `5+2=7`. Under this simple additive metric, SPF selects R1-R2-R4.

### 7.4 Distance vector versus link state comparison

| Factor | Distance Vector | Link State |
|---|---|---|
| Basic information | Destination costs learned from neighbours | Link descriptions distributed across domain |
| Local network view | Does not normally require full topology | Maintains topology database |
| Typical computation | Bellman-Ford style | Dijkstra/SPF |
| Example | RIP | OSPF, IS-IS |
| Common difficulty | Count-to-infinity / transient loops | Flooding/LSDB and computation complexity |
| Convergence | Can be slow in simple traditional protocols | Often faster with well-designed mechanisms |
| Resource requirement | Often lower for simpler networks | More topology state, processing and memory |

**Exam caution:** "Link-state is *always* faster" is too absolute. Actual convergence depends on configuration, network size, failure detection, timers and implementation. Use "often converges faster than basic distance-vector methods".

***

## 8. Other Routing Techniques: Flooding, Hierarchical and Interdomain

### 8.1 Hinglish: one announcement versus one personal route

Agar college ko ek urgent fire-drill announcement karna hai, ek teacher har room ko message forward kar sakta hai. Ye **flooding** ka simplified idea hai: message ko multiple outgoing links par replicate kiya jata hai. Delivery ki chance badh sakti hai, but copies bahut zyada ho sakti hain. Doosri taraf, agar college ke 500 rooms hain, har room ke liye detailed external route store karna inefficient ho sakta hai: floors/buildings ko groups mein arrange karna **hierarchical routing** jaisa hai.

### 8.2 Flooding - working and controls

**Flooding** is a forwarding technique in which a node sends a packet to multiple or all relevant outgoing links (usually excluding the incoming link). It may be useful for discovery and dissemination, but unrestricted flooding can create huge numbers of duplicates and consume network capacity.

**Controls:** unique packet identifiers with duplicate suppression, hop limits/TTL, sequence numbers, spanning trees or controlled distribution rules. Link-state protocols employ forms of controlled/reliable flooding for LSAs, not unlimited user-data flooding.

```text
     R2 -------- R4
    / |          /|
  R1  |    R5 --/ |
    \ |       \   |
     R3 ------- R6

 Uncontrolled: one incoming packet creates many copies.
 Controlled: duplicates are discarded; scope/TTL is limited.
```

### 8.3 Hierarchical routing

**Hierarchical routing** divides a large routing system into areas, regions or domains. Routers maintain detailed information about local portions and summarised or policy-filtered information about remote portions. It can reduce information size and processing burden, but summarisation may hide exact internal paths and sometimes produce a non-optimal route.

```text
             INTERNET (many autonomous systems)
                /                  \
            AS 65001             AS 65002
            /     \                |
         Area 0  Area 1         Multiple networks
         /   \      \
       LAN A LAN B   LAN C

  Hierarchy: LAN -> Area -> Autonomous System -> Internet
  (illustrative; exact hierarchy depends on protocol).
```

### 8.4 Interior versus exterior routing and BGP

An **Autonomous System (AS)** is a routing-policy administrative domain on the Internet, assigned an AS number where needed. **Interior Gateway Protocols (IGPs)**, such as RIP and OSPF, generally distribute routing information *within* an AS. **Border Gateway Protocol (BGP)** is the standard interdomain routing protocol for exchanging reachability information and applying policy between autonomous systems. BGP is a **path-vector** routing protocol, and its selection logic is policy-based, not simply "choose the fewest hops" or basic Dijkstra.

| Protocol | Routing family | Scope | Main idea |
|---|---|---|---|
| RIP | Distance vector | Inside AS | Hop-count metric |
| OSPF | Link state | Inside AS | SPF and link-state information |
| IS-IS | Link state | Inside AS | Link-state database and SPF |
| BGP | Path vector | Between/within AS according to configuration | AS-path and policy attributes |

**Simple memory:** RIP = neighbour distances; OSPF = topology map; BGP = Internet-scale reachability and policies.

***

## 9. Congestion Control - Definition, Causes and Effects

### 9.1 Hinglish: a traffic jam near Charbagh

Road par ek minute mein 100 cars comfortably pass ho sakti hain, lekin 500 cars aa jaayein to queue banegi. Thodi der ke liye queue manageable hai, but continuously demand zyada hui to waiting time bahut badhega aur road almost jam ho jaayegi. Network router ke output link par bhi same situation hoti hai: **incoming packet rate > available service capacity**. Buffer fill hota hai, queue badhti hai, phir packets drop ho sakte hain.

**Congestion** = *network ke andar overload*. **Flow control** = *sender itna fast mat bhejo ki receiver handle na kar paaye*. Dono related but alag issues hain.

Another real-life example: 200 students ek hi college portal par simultaneously result download kar rahe hain. Server ya Internet uplink overloaded ho sakta hai. Delay badh sakta hai, retries load ko aur badha sakti hain. "Fast Internet plan" alone congestion solve nahi karta, kyunki bottleneck kisi shared intermediate link/server par bhi ho sakta hai.

```text
  5 input links:  each 10 Mbps
       \   |   /       |
        \  |  /        |
          ROUTER -------> one 20 Mbps output link
         [QUEUE]
         [QUEUE]          Arrival can exceed service rate.
         [DROP?]          Buffers cannot hold packets forever.

  Excess arrivals -> queue growth -> delay -> drops/retransmissions
```

### 9.2 Complete English - congestion control

**Network congestion** is a condition in which demand for resources such as bandwidth, buffers and router processing exceeds the network's ability to provide service efficiently. Congestion can lead to increased queueing delay, packet loss, reduced useful throughput, high jitter and inefficient resource use. **Congestion control** is the collection of methods used to prevent excessive load, detect overload and regulate traffic to maintain useful network performance.

**Causes:**

1. **Traffic bursts** from multiple senders arriving at the same bottleneck.
2. **Insufficient outgoing link capacity** compared with incoming offered traffic.
3. **Limited router buffers**, eventually causing packet dropping.
4. **Limited processing capacity** in forwarding devices.
5. **Poor traffic distribution** or routing that concentrates load on one path.
6. **Retransmissions after loss**, which can increase offered load if uncontrolled.
7. **Sudden popularity or attack traffic**, overwhelming a link or service.

**Effects:**

- Increased **queueing delay** and end-to-end latency.
- **Packet loss** when buffer capacity is exhausted or intentional queue management drops packets.
- **Jitter**, meaning variation in packet delay, harmful to interactive audio/video.
- Throughput can stop increasing or even decline as load continues to rise.
- More timeouts and retransmissions, which can amplify congestion.
- Unfair bandwidth distribution between flows.

### 9.3 Congestion versus flow control - table for 5 marks

| Aspect | Flow Control | Congestion Control |
|---|---|---|
| Main problem | Receiver may be overwhelmed | Shared network may be overloaded |
| Main concern | Sender-receiver pacing | Network paths and bottlenecks |
| Typical signal | Receiver-advertised window | Loss, delay, ECN, congestion window |
| Common example | TCP receiver window `rwnd` | TCP congestion window `cwnd` |
| Goal | Avoid receiver-buffer overflow | Avoid excessive network load |

For TCP, sendable outstanding data is constrained approximately by **both** receiver window and congestion window; a useful simplified statement is `usable window <= min(rwnd, cwnd)`, with other implementation and protocol limits also applying.

### 9.4 Network load and throughput concept

At very low load, a network carries little traffic. As offered load increases, useful throughput normally grows, until a bottleneck is approached. Queues can then grow, and throughput may level off or deteriorate depending on network conditions and control mechanisms. This is a **qualitative graph**, not a universal exact function.

```text
 Useful throughput
 ^
 |                     ________  near capacity
 |                 ___/
 |              __/
 |          ___/
 |      ___/
 |   __/
 +----------------------------------> Offered load
                  |
          Bottleneck approached

 Delay may rise sharply near persistent overload.
```

***

## 10. Congestion-Control Methods: Prevention, Feedback and Traffic Shaping

### 10.1 Hinglish: traffic rules before and after a jam

Road jam hone se pehle preventive rules laga sakte ho: staggered office timings, heavy trucks ko alternate roads, vehicle entry limits. Jam hone par traffic police live signals aur diversion use karti hai. Networking mein bhi roughly two categories hoti hain:

- **Open-loop/prevention-oriented:** network ko design/control karke overload ki chance kam karna; feedback par constantly depend nahi karte.
- **Closed-loop/feedback-oriented:** network performance observe karna, congestion detect karna aur senders/traffic behaviour adapt karna.

**Traffic shaping** ka relatable example: canteen mein students ko tokens milte hain. Sab students ek hi moment counter par rush karne ke bajay limited rate se enter karte hain. Traffic shaper bhi outgoing packets ka timing/rate regulate karta hai.

### 10.2 Open-loop and closed-loop approaches

**Open-loop mechanisms** may include sensible admission policies, retransmission/timeout design, resource reservation, routing and scheduling choices, and traffic shaping that limits offered load before congestion becomes severe. **Closed-loop mechanisms** collect feedback about congestion and alter network or sender behaviour. They may use packet loss, increasing delay, explicit congestion notification (ECN), queue measurements or other control signals.

**Specific mechanisms:**

1. **Admission control:** accept a new traffic flow only if required resources or policy conditions can be satisfied.
2. **Traffic shaping:** smooth or regulate injection of packets, e.g., leaky bucket and token bucket.
3. **Queue scheduling:** choose transmission order among classes/flows, e.g., fair queueing or priority scheduling.
4. **Active Queue Management (AQM):** detect an emerging queue problem and intentionally mark/drop some packets before a large standing queue develops; RED is a classic example.
5. **Load balancing and alternate routing:** spread traffic where valid capacity and policy permit.
6. **End-host adaptation:** TCP-style congestion control changes sending behaviour based on observed congestion signals.

**Warning:** No single mechanism guarantees absence of congestion. These methods address different parts of a complex system.

### 10.3 Leaky bucket algorithm

**Hinglish:** Socho ek bucket mein paani randomly aata hai lekin neeche chhota hole se paani **fixed rate** se nikalta hai. Agar paani bahut quickly aa jaye aur bucket full ho jaye, extra water overflow ho sakta hai. In packet traffic, leaky bucket shaping releases queued traffic at a controlled rate. Under a fixed-rate, packet-level simplified model, output is relatively smooth.

**English:** The **leaky bucket** algorithm models a finite buffer served at a fixed or controlled rate. Incoming packets are stored; the outgoing traffic is drained at the configured rate. If arrivals exceed available buffer space, packets may be dropped (or the sender blocked, depending on system design).

```text
     Bursty arrivals:  ||||||  ||  ||||||||
                           v
                   +---------------+
                   |   BUCKET      |  finite capacity B
                   | queued data   |
                   +-------+-------+
                           |
                           v
                   fixed output rate r
                     |   |   |   |
```

**Simple example:** Capacity = 30 packets; outgoing rate = 5 packets/second; immediately 20 packets arrive to an empty bucket. If no more packets arrive and one packet is counted as a unit, draining all 20 takes `20/5 = 4 seconds` under this simplified service model. If 40 packets arrive instantaneously to an empty 30-packet buffer, up to 10 cannot fit without another policy or concurrent drain.

**Advantage:** smooth predictable outgoing rate. **Limitation:** may not allow bursts at line rate, even when the network could accommodate them.

### 10.4 Token bucket algorithm

**Hinglish:** Imagine tumhare paas movie hall entry ke tokens hain. Har second 2 naye tokens milte hain. Jab hall entry karni ho, ek token spend karna hai. Agar tum tokens save kar loge, short burst mein multiple friends enter kar sakte hain. Isi tarah token bucket **average rate ko control karte hue burst allow** kar sakta hai.

**English:** The **token bucket** generates tokens at a specified rate `r` and stores them up to bucket capacity `B` tokens. Sending a packet consumes tokens proportional to packet size (depending on implementation). If enough tokens are present, the packet may be transmitted immediately; otherwise it waits (shaping) or may be rejected/marked (policing).

```text
   Tokens created: +r each second
                       |
                       v
                 +-----------+
                 | Tokens    | <--- capacity B
                 +-----+-----+
                       |
   packet needs tokens? Yes -> transmit / consume tokens
                        No -> wait or drop/mark (policy)
```

**Worked numerical:** A byte-based token bucket generates `1000 bytes/s` worth of tokens and holds at most `3000 bytes` of tokens. Assume full bucket at time zero. Three 1000-byte packets can be sent immediately as a 3000-byte burst; afterward, allowing another 1000-byte packet requires roughly one second of replenishment (assuming no other tokens and ignoring overhead).

**Differences:** leaky bucket (fixed-rate service model) smooths outgoing data, while token bucket permits a limited burst while maintaining a long-term average budget. Real implementations may combine both.

### 10.5 TCP congestion control - introductory understanding

TCP is part of the **Transport Layer**, but discussing it helps understand congestion control in the TCP/IP architecture. Classic TCP congestion control uses a sender-side **congestion window (`cwnd`)** and feedback mechanisms. Main classical concepts:

- **Slow start:** beginning with a small window, growth can be approximately exponential per round-trip time while ACKs arrive, until a threshold or signal changes behaviour.
- **Congestion avoidance:** commonly uses additive increase under steady successful delivery.
- **Multiplicative decrease:** on congestion, reduce the congestion window substantially, with details depending on the algorithm/event.
- **Fast retransmit/recovery:** infer certain losses from acknowledgements and recover without waiting for every possible timer in the classic designs.
- **ECN:** some networks can explicitly mark packets to indicate congestion instead of relying only on drops, when supported end-to-end.

**Illustrative cwnd evolution** (not exact for every modern TCP implementation):

```text
Round-trip number:       1   2   3   4   5   6   7
Window (segments):       1   2   4   8   9  10  11
                         \_____ /  \_____________/
                          slow      congestion
                          start       avoidance

After a congestion indication, the window is reduced.
```

**Exam distinction:** the TCP/IP stack combines network-layer IP (best-effort packet forwarding) with transport-layer TCP (reliability, flow control and congestion control). Do not say IP itself guarantees TCP-style congestion control.

***
## 11. Internetworking - Connecting Different Networks

### 11.1 Hinglish: Lucknow college LAN and an overseas website

Tumhare laptop ka Wi-Fi ek network hai, ISP ka fibre infrastructure doosra, aur website ka data-centre network alag. In networks mein different hardware, cable, Wi-Fi standards aur link speeds ho sakte hain. Phir bhi browser website khol leta hai because **IP ek common internetworking protocol** provide karta hai. Routers har link ki technology ke hisaab se new link frame create karke IP datagram ko next hop tak le jaate hain.

Imagine courier parcel same address ke saath kabhi bike par, kabhi truck par, kabhi aeroplane mein travel karta hai. **Transport vehicle change ho sakta hai, destination address ka logical meaning same rehta hai.** Same idea: Ethernet/Wi-Fi frames change, while IP packet contains network-layer addresses.

```text
  Laptop --Wi-Fi--> Home router --Fibre/ISP--> Internet router
       \                                          /
        \------------ IP datagram -------------/
                              |
                          Data-centre
                              |
                           Web server

  Link 1 uses Wi-Fi frames; link 2 may use another encapsulation.
  IP provides common addressing and routing across links.
```

### 11.2 Complete English definition and requirements

**Internetworking** means connecting multiple separate computer networks to permit communication between their hosts. Internetworking enables devices attached to different local networks and link technologies to exchange packets through gateways/routers using an agreed network-layer protocol. The global Internet is the most prominent example of internetworking based on IP.

**Why internetworking is needed:**

1. Organisations have multiple LANs across buildings and locations.
2. Different communication technologies may use incompatible local frame structures.
3. Hosts need a shared logical addressing system.
4. Routers must select paths across networks.
5. Packet size and quality differences between links must be handled.
6. Network failures, congestion and security policies influence forwarding.

**Key components:** IP addresses/prefixes, routers, forwarding tables, link technologies, ARP or neighbour discovery as appropriate, routing protocols, and end-host protocol stacks.

### 11.3 A packet's journey from browser to server

Step 1: **Application** forms request data; if a domain name is used, address resolution using DNS may already have occurred. Step 2: **Transport** such as TCP creates segments and manages its connection, or UDP is used for suitable applications. Step 3: **IP** adds source/destination network-layer addresses. Step 4: Host selects next hop (local destination directly or default router) and resolves a link-layer next-hop address if necessary. Step 5: Link layer sends a frame to the local next hop. Step 6: Router removes incoming link encapsulation, processes the IP packet, decreases TTL/Hop Limit, looks up a route and constructs the outgoing frame. Step 7: The process continues until the destination network/host receives it. Step 8: Appropriate upper layers process the received data.

**Important:** Domain names do not usually appear as IP forwarding destinations in the IP header; routers forward using IP destination addresses and routes, not human-readable website names.

### 11.4 TCP/IP model - layers, protocols and encapsulation

**Hinglish:** OSI ko 7 floors wali building ki tarah padhe the. TCP/IP Internet ki practical protocol architecture ko group karta hai: application, transport, internet and link/network-access. Different textbooks show 4 or 5 layers, but network communication ka role same way explain ho sakta hai.

**English:** The **TCP/IP protocol suite** is the set of protocols underlying Internet communication. A common four-layer description is:

| TCP/IP layer | Main responsibility | Representative protocols |
|---|---|---|
| Application | Services used by applications | HTTP, DNS, SMTP, FTP |
| Transport | Process-to-process delivery | TCP, UDP |
| Internet | Logical addressing and inter-network packet forwarding | IPv4, IPv6, ICMP |
| Link / Network access | Delivery over a local medium | Ethernet, Wi-Fi, PPP |

```text
  TCP/IP APPLICATION                 HTTP, DNS, email protocols
            |
       TRANSPORT                     TCP / UDP
            |
        INTERNET                     IP / ICMP
            |
     LINK / NETWORK ACCESS           Ethernet / Wi-Fi / PPP
            |
      Physical medium                cable, fibre, radio
```

**TCP versus IP:** TCP is a connection-oriented reliable byte-stream transport protocol with sequence numbers, acknowledgements, retransmission, flow control and congestion control. IP is a connectionless best-effort packet-forwarding protocol. TCP operates **on top of IP**; it is not correct to say that TCP determines the Internet's inter-router paths.

### 11.5 Common supporting internetworking protocols

- **ARP (IPv4 LANs):** resolves an IPv4 address to a link-layer hardware address when required on an appropriate LAN. IPv6 uses **Neighbor Discovery** (ICMPv6), not ARP.
- **ICMP:** carries network control/diagnostic messages, e.g., certain unreachable notifications and echo messages used by `ping`.
- **DHCP (IPv4 or IPv6 variants):** can automatically provide address/configuration information; IPv6 also supports SLAAC.
- **DNS:** converts domain names to records such as IPv4 A or IPv6 AAAA addresses; DNS is generally a higher-layer service, not routing itself.
- **NAT:** often translates addresses/ports at network edges, especially to allow multiple private IPv4 devices to share public IPv4 connectivity.

***

## 12. Internet Protocol (IP) - Characteristics and Datagram Service

### 12.1 Hinglish: IP is a delivery attempt, not a delivery promise

Tum courier counter par parcel dete ho. Address complete hai, but courier service agar *best effort* type ho, to guarantee nahi ki parcel definitely pahunch hi jayega. IP ka model is tarah samjho: **IP tries to forward packets, but alone guarantee nahi deta**.

Agar packet drop ho gaya, duplicate ban gaya, ya out-of-order aaya, to application ki needs ke hisaab se TCP jaise protocol reliability provide kar sakte hain. Isliye baar-baar yaad rakho: **IP = addressing and routing; TCP = transport reliability (when used).**

### 12.2 Complete English explanation

The **Internet Protocol (IP)** is the principal network-layer protocol used to address and route packets across IP internetworks. Two important versions are **IPv4** and **IPv6**. An IP packet contains a header with necessary information such as source and destination addresses and protocol-related control fields, followed by the payload.

**Main IP characteristics:**

1. **Connectionless datagrams:** no IP connection setup is required before forwarding an individual datagram.
2. **Best-effort delivery:** IP does not guarantee arrival, order or exactly-once delivery.
3. **Logical addressing:** IPv4 and IPv6 identify network-layer endpoints/interfaces.
4. **Routing across links:** routers examine destination prefixes and select next hops.
5. **Encapsulation flexibility:** IP can be carried over different link technologies.
6. **Packet lifetime restriction:** TTL in IPv4 and Hop Limit in IPv6 prevent permanent circulation.
7. **Different header designs:** IPv4 and IPv6 have different address lengths and packet-header fields.

A **datagram** is a packet treated as an individually forwarded unit. For an ordinary IPv4 datagram, header + payload length is represented by the IPv4 Total Length field. An IPv6 packet uses a different fixed base-header structure and length representation.

***

## 13. IPv4 Packet Header - Every Field Explained

### 13.1 Hinglish: parcel label ke columns

Socho tum kisi package par sticker chipka rahe ho. Us par sender, receiver, package weight aur special instructions likhe hote hain. Router ko bhi packet process karne ke liye kuch control fields chahiye. **IPv4 header ek standard label** ki tarah hota hai.

- **Source IP / Destination IP:** kahan se aaya, kahan jaana hai.
- **TTL:** kitne remaining router hops tak allowed hai; endlessly loops nahi.
- **Protocol:** aage payload ka type kya hai, e.g., TCP=6, UDP=17, ICMP=1.
- **Total Length:** poora IPv4 datagram kitna bada hai.
- **Identification, flags, fragment offset:** fragmentation ke parts ko identify/assemble karne mein useful.
- **Header Checksum:** IPv4 header mein bit errors detect karne ka mechanism, payload ka complete end-to-end checksum nahi.

### 13.2 Complete English and 32-bit-row diagram

An **IPv4 packet** consists of a variable-length IPv4 header, normally **20 bytes without options** and up to **60 bytes with options**, followed by payload. Its conventional base-header structure is:

```text
    0                   15                  31 (bit positions)
    +----+----+--------+--------------------+
    |Ver |IHL |DSCP/ECN|   Total Length     |
    +------------------+----+---------------+
    |   Identification |Flags|Frag. Offset  |
    +--------+---------+--------------------+
    |  TTL   |Protocol |   Header Checksum  |
    +---------------------------------------+
    |          Source IPv4 Address          |
    +---------------------------------------+
    |       Destination IPv4 Address        |
    +---------------------------------------+
    | Options + Padding (if present)        |
    +---------------------------------------+
    |            Payload (Data)             |
    +---------------------------------------+

  Diagram is schematic, not an exact proportional bit drawing.
  IPv4 minimum header = 5 words x 4 bytes = 20 bytes.
```

### 13.3 Important IPv4 header fields - answer-ready

| Field | Size | Function |
|---|---:|---|
| Version | 4 bits | Value 4 identifies IPv4 |
| IHL | 4 bits | Header length in 32-bit words; minimum 5 |
| DSCP/ECN (formerly ToS byte) | 8 bits | Traffic classification and congestion notification |
| Total Length | 16 bits | Entire IPv4 datagram size, in bytes |
| Identification | 16 bits | Helps identify fragments belonging to one original datagram |
| Flags | 3 bits | Reserved bit, DF (Don't Fragment), MF (More Fragments) |
| Fragment Offset | 13 bits | Fragment position in original payload, units of 8 bytes |
| Time To Live | 8 bits | Reduced during forwarding to limit packet lifetime |
| Protocol | 8 bits | Type of next payload, e.g., TCP, UDP or ICMP |
| Header Checksum | 16 bits | Detects errors in IPv4 header only |
| Source Address | 32 bits | IPv4 address of source |
| Destination Address | 32 bits | IPv4 address of destination |
| Options + Padding | Variable | Optional extra header information and alignment |

**IHL example:** If IHL = 6, header length = `6 x 4 = 24 bytes`. With Total Length = 1400 bytes, IPv4 payload is `1400 - 24 = 1376 bytes`.

**TTL example:** A datagram sent with TTL 64 passes through 3 forwarding routers, each reducing TTL by at least 1. Its TTL upon arrival may be 61 in the simple model. A packet whose TTL expires during router forwarding is discarded and may trigger an ICMP Time Exceeded message.

**Protocol vs port confusion:** IPv4 *Protocol* = 6 tells the receiving host that the payload is TCP. It is not the TCP *port* (for example 443). TCP/UDP port numbers live in the transport header.

### 13.4 IPv4 fragmentation - why and how

**Hinglish:** Tumhare paas 4 kg ka package hai, aur kisi connecting vehicle mein maximum 1.5 kg-size parcels allowed hain. Tum package ko pieces mein divide karke labels lagate ho: pehla piece, doosra piece, last piece. IPv4 mein link ki **Maximum Transmission Unit (MTU)** limit ke kaaran packet ko smaller fragments mein split karna pad sakta hai. Har fragment ka IP header hota hai.

**English:** Fragmentation divides an IPv4 datagram into smaller IP fragments when necessary to traverse a link with a smaller MTU, provided fragmentation is allowed. IPv4 uses the Identification, More Fragments (MF) flag and Fragment Offset fields to enable the destination to reassemble the original datagram. **Routers may fragment IPv4 datagrams when DF is not set**, while **IPv4 reassembly is performed at the destination**, not ordinarily at each transit router.

**Important rules:**

1. Each fragment includes its own IPv4 header.
2. Fragment offset counts blocks of **8 payload bytes**, not ordinary bytes.
3. All fragments except the final fragment must carry data lengths divisible by 8 bytes.
4. `MF=1` means more fragments follow; `MF=0` indicates the last fragment (or an unfragmented datagram).
5. MTU applies to the full IP packet on that link, including IP header.
6. DF set means the router must not fragment; it discards an oversized packet and can send an appropriate ICMP error for path MTU discovery.

### 13.5 Fully solved fragmentation numerical (20-mark favourite)

**Question:** An IPv4 datagram has **Total Length = 4000 bytes**, **Header Length = 20 bytes**, and must travel through a link with **MTU = 1500 bytes**. Assume no options and fragmentation is permitted. Determine fragment payload lengths, total lengths, offsets and MF flags.

**Step 1 - original payload:** `4000 - 20 = 3980 bytes`.

**Step 2 - maximum fragment payload:** `1500 - 20 = 1480 bytes`. 1480 is divisible by 8 (`1480/8 = 185`), so a full non-final fragment can hold 1480 bytes.

**Step 3 - split:** first 1480 bytes; second 1480 bytes; remainder `3980-2960 = 1020 bytes`.

**Step 4 - find offsets:** first starts at byte 0 -> `0/8=0`. Second starts at byte 1480 -> `1480/8=185`. Third starts at byte 2960 -> `2960/8=370`.

| Fragment | Data bytes | IP total bytes | Offset (8-byte units) | MF |
|---|---:|---:|---:|---:|
| 1 | 1480 | 1500 | 0 | 1 |
| 2 | 1480 | 1500 | 185 | 1 |
| 3 | 1020 | 1040 | 370 | 0 |

**Verification:** `1480+1480+1020=3980` payload bytes. Three fragments contain `3x20=60` total header bytes; transmitted IP bytes total `4040`, which is larger than the 4000-byte original because fragmentation duplicates headers.

```text
 Original IP packet:  [ 20-byte IP Header | 3980-byte Payload ]
 MTU = 1500

 F1: [20|       Payload 1480       ] offset=0   MF=1
 F2: [20|       Payload 1480       ] offset=185 MF=1
 F3: [20|   Payload 1020   ]         offset=370 MF=0

 Destination uses identification, offsets and flags to reassemble.
```

### 13.6 Why fragmentation is undesirable when avoidable

Fragments add extra header overhead and reassembly work. Loss of a fragment can prevent successful reassembly of the original datagram, leading to additional recovery at other layers. **Path MTU Discovery** helps select packet sizes that avoid fragmentation. IPv6 takes a different approach: routers do not fragment packets in transit.

***

## 14. IPv4 Addressing - Understand Address, Network and Host

### 14.1 Hinglish: society address and house number

Imagine tumhara address hai: "Building C, Flat 27, Charbagh, Lucknow." Isme city/area = network portion jaisa, aur flat = host portion jaisa. IPv4 address ko bhi samajhne ke liye do parts helpful hain: **network prefix** aur **interface/host identifier**. Subnet mask decide karta hai ki address ke kaunse bits network ke hain aur kaunse host ke.

Example: `192.168.1.25/24`. `/24` means first 24 bits are prefix. Remaining 8 bits are local host part. Isliye network `192.168.1.0/24` hai. Ek ordinary /24 IPv4 subnet mein last-octet values 0 through 255 hoti hain; traditionally `.0` network identifier, `.255` broadcast, and `.1` to `.254` assignable unicast host addresses, subject to actual network policies.

### 14.2 Complete English IPv4 addressing

An **IPv4 address** is a **32-bit** logical network-layer address normally written in four decimal octets separated by dots, such as `192.168.1.25`. Each octet ranges from 0 through 255 because it represents 8 bits. The address can be represented in binary, and network prefixes identify which leading bits correspond to the routing network/subnet.

Example:

```text
  IPv4 address: 192 . 168 . 1 . 25
               11000000.10101000.00000001.00011001
               \____________ 24 prefix bits ______/ \host/

  CIDR: 192.168.1.25/24
  Mask: 255.255.255.0
  Network: 192.168.1.0
```

### 14.3 Classful addresses (historical) and modern CIDR

Early IPv4 routing used classful A/B/C assignments; modern networks use **Classless Inter-Domain Routing (CIDR)** with explicit prefix lengths. Learn A/B/C because exam syllabi and textbooks often include them, but do not assume every contemporary network strictly follows classful boundaries.

| Historical class | First octet range | Default mask | Default prefix |
|---|---|---|---|
| A | 1-126 (with reserved qualifications) | 255.0.0.0 | /8 |
| B | 128-191 | 255.255.0.0 | /16 |
| C | 192-223 | 255.255.255.0 | /24 |
| D | 224-239 | Multicast (not normal host subnet class) | N/A |
| E | 240-255 | Reserved/experimental ranges | N/A |

`127.0.0.0/8` is reserved for loopback; `0.0.0.0` and other ranges have special meanings. The table above is a historical study chart, not a substitute for the current IP registry.

**CIDR notation** `a.b.c.d/n` explicitly gives the first `n` network prefix bits. For instance, `/27` means 27 bits of network prefix and 5 bits remaining for addresses within that block.

### 14.4 Important IPv4 address categories

- **Public globally routable IPv4 addresses:** usable on the global Internet when legitimately allocated and reachable; not every non-private address is automatically publicly usable.
- **Private IPv4 blocks (RFC 1918):** `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`; usually used inside organisations/homes, not directly globally routed.
- **Loopback:** `127.0.0.0/8`; `127.0.0.1` commonly represents the local host.
- **Link-local:** `169.254.0.0/16`, often appears in automatic address configuration when other methods are unavailable.
- **Limited broadcast:** `255.255.255.255`, special local broadcast use in IPv4.
- **Unspecified address:** `0.0.0.0`, used in contexts such as an unassigned source or wildcard local binding.

**Addressing caution:** `172.20.5.9` is private because it belongs to `172.16.0.0/12`; `172.32.5.9` is outside that private block. Not every `172.x.x.x` address is private.

### 14.5 Subnet mask and bitwise AND

A subnet mask has 1-bits in the prefix portion and 0-bits elsewhere. To obtain the **network address**, compute the bitwise AND of IPv4 address and mask.

Example: `192.168.10.77/26`.

```text
  Last octet of host:    77 = 01001101
  Last octet of mask:   192 = 11000000  (/26)
                           -------- AND
  Last octet of net:     64 = 01000000

  Network address = 192.168.10.64
  Mask            = 255.255.255.192
```

***

## 15. Subnetting and CIDR - Detailed Worked Examples

### 15.1 Hinglish: one big hostel divided into floors

Maan lo college hostel mein 256 room numbers allotted hain. Administration sochta hai: "Is big block ko alag floors mein divide karte hain, taaki management easier ho." Network mein bhi large IP address block ko smaller **subnets** mein divide kar sakte hain. Isse separate departments, better routing organisation and address management possible hota hai.

**Subnetting formula yaad karo:** An IPv4 `/n` network contains `2^(32-n)` total addresses. In a **conventional IPv4 subnet** where the first address is network and last is broadcast, usable host count is `2^(32-n)-2`. But `/31` point-to-point links and `/32` single-address routes are important exceptions to the simple minus-two teaching rule.

### 15.2 Important formulas

$$
\text{IPv4 block size} = 2^{(32-n)}
$$

$$
\text{Traditional usable hosts} = 2^{(32-n)}-2
$$

$$
\text{Subnets created by borrowing }b\text{ host bits}=2^b
$$

**Last-octet block-size shortcut:** when the interesting octet is the last octet, `block size = 256 - mask value of that octet`. E.g., `/26` has mask last octet 192 -> block size 64. This shortcut must be applied to the correct changing octet for other prefixes.

| Prefix | Mask | Total addresses | Traditional usable hosts |
|---|---|---:|---:|
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |
| /31 | 255.255.255.254 | 2 | 2 usable endpoints in suitable P2P use |
| /32 | 255.255.255.255 | 1 | Single host address/route |

### 15.3 Solved example 1 - find full subnet range

**Question:** Find network address, broadcast address, usable range and host count for `192.168.10.77/26`.

**Step 1:** `/26` mask = `255.255.255.192`; 6 host bits, `2^6 = 64` addresses.

**Step 2:** Last-octet blocks are: `0-63`, `64-127`, `128-191`, `192-255`.

**Step 3:** 77 lies in the `64-127` block.

**Answer:** Network `192.168.10.64`, broadcast `192.168.10.127`, host range `192.168.10.65` through `192.168.10.126`, usable host count `64-2=62`.

### 15.4 Solved example 2 - divide /24 into eight equal subnets

**Question:** Divide `192.168.20.0/24` into 8 equal-size subnets.

**Step 1:** Need `2^b >= 8`, so borrow `b=3` host bits.

**Step 2:** New prefix `/24+3 = /27`; mask `255.255.255.224`.

**Step 3:** Each subnet has `2^5 = 32` addresses, traditionally `30` usable.

| Subnet | Network address | Broadcast | Host range |
|---|---|---|---|
| 1 | 192.168.20.0/27 | .31 | .1 to .30 |
| 2 | 192.168.20.32/27 | .63 | .33 to .62 |
| 3 | 192.168.20.64/27 | .95 | .65 to .94 |
| 4 | 192.168.20.96/27 | .127 | .97 to .126 |
| 5 | 192.168.20.128/27 | .159 | .129 to .158 |
| 6 | 192.168.20.160/27 | .191 | .161 to .190 |
| 7 | 192.168.20.192/27 | .223 | .193 to .222 |
| 8 | 192.168.20.224/27 | .255 | .225 to .254 |

In the table, shortened `.x` values refer to the final octet of `192.168.20.x`.

### 15.5 Solved example 3 - /20 network address

**Question:** Determine the network containing `172.16.37.146/20`.

**Step 1:** `/20` mask is `255.255.240.0`. Interesting octet is the **third**, mask 240. Block size `256-240=16`.

**Step 2:** Third-octet blocks: `0-15`, `16-31`, `32-47`, `48-63` ... The number 37 lies in `32-47`.

**Answer:** Network `172.16.32.0/20`; broadcast `172.16.47.255`; host range `172.16.32.1` through `172.16.47.254`. Total `2^12=4096`, traditionally usable `4094`.

### 15.6 Solved example 4 - minimum subnet for 50 computers

**Question:** A computer lab needs at least 50 ordinary IPv4 host addresses. Which is the smallest conventional IPv4 subnet?

Need `2^h-2 >= 50`. For `h=5`, `32-2=30` (insufficient). For `h=6`, `64-2=62` (enough). Thus prefix `32-6=/26`; mask `255.255.255.192`. **Answer: /26 with 62 traditional usable addresses.**

### 15.7 VLSM - different sizes for different departments

**Hinglish:** Admin department mein 50 students, Library lab mein 20, aur accounts mein 10 computers hain. Sabko same-size 64-address network doge to waste hoga. **Variable Length Subnet Masking (VLSM)** mein requirement ke hisaab se different-size prefixes assign karte hain.

**Question:** From `192.168.50.0/24`, design subnets for Lab A: 100 hosts, Lab B: 50 hosts, Office: 20 hosts, Link: 2 hosts.

Largest first:

| Need | Chosen block | Traditional usable | Assigned range |
|---|---|---:|---|
| 100 | /25 (128 addresses) | 126 | 192.168.50.0-127 |
| 50 | /26 (64 addresses) | 62 | 192.168.50.128-191 |
| 20 | /27 (32 addresses) | 30 | 192.168.50.192-223 |
| 2 | /30 (4 addresses) | 2 | 192.168.50.224-227 |

Here each assigned range includes network and broadcast endpoints where applicable. Unused addresses remain from `.228` onward. **Reason for largest first:** large blocks need stricter alignment; allocating small arbitrary ranges first can make later large allocations impossible or fragmented.

### 15.8 Supernetting/route aggregation

**Supernetting** combines suitably aligned contiguous IP prefixes under a shorter common prefix when address allocation and routing allow. Example: `192.168.0.0/24` and `192.168.1.0/24` can be described collectively by `192.168.0.0/23`. Aggregation reduces routing table detail; routers must still avoid advertising routes that cause incorrect forwarding.

### 15.9 Longest-prefix-match numerical

Routing entries:

```text
  192.168.0.0/16  --> R1
  192.168.10.0/24 --> R2
  0.0.0.0/0       --> ISP
```

Destination `192.168.10.34` matches /16 and /24; select **R2** because `/24` is the longer prefix. Destination `192.168.20.6` matches only `/16` (plus default); select **R1**. Destination `8.8.8.8` matches only default; choose **ISP**.

***
## 16. IPv6 - New-Generation Internet Protocol

### 16.1 Hinglish: har device ke liye zyada room numbers

Imagine ek enormous city mein sirf limited phone numbers available hain. New people aur devices aate jaa rahe hain, but number supply limited hai. IPv4 **32-bit addresses** use karta hai. IPv6 **128-bit addresses** use karta hai, so address space bahut bada ho gaya. Isse address allocation mein flexibility milti hai, though address space size by itself security/performance guarantee nahi deta.

IPv4 address short hota hai: `192.168.1.5`. IPv6 address lamba, colon-separated hexadecimal form mein hota hai: `2001:db8:abcd:12::5`. Lekin IPv6 mein zero blocks shorten karne ke rules hain, so practical notation manageable hai.

**Relatable analogy:** Old hostel mein 32-bit system = comparatively limited room-number combinations. New campus mein 128-bit addressing = vastly more combinations. But hostel discipline, locks, gate security still required: similarly **IPv6 automatically hack-proof nahi hai**.

### 16.2 Complete English - definition and motivation

**Internet Protocol version 6 (IPv6)** is a network-layer protocol designed as the successor to IPv4. It uses **128-bit** IP addresses and a fixed **40-byte base header**. It supplies connectionless best-effort datagram delivery and supports hierarchical addressing, streamlined base-header processing, multicast, anycast and extensible optional information using extension headers.

**Major motivations/features:**

1. Greatly enlarged address space compared with 32-bit IPv4.
2. Fixed-length 40-byte base header for simpler basic parsing.
3. Separate **extension headers** for optional information.
4. **Hop Limit** replaces IPv4 TTL's role as hop-lifetime protection.
5. No IPv6 base-header checksum; integrity is provided by other mechanisms as appropriate.
6. IPv6 routers **do not fragment** transit packets; source-based fragmentation with a Fragment extension header is possible.
7. Supports **Neighbor Discovery** and **SLAAC**, as well as DHCPv6 in appropriate deployments.
8. Does not define IPv4-style broadcast; IPv6 uses multicast and other addressing approaches.

### 16.3 IPv6 address format and shortening rules

An IPv6 address has **128 bits**, conventionally shown as **eight groups of four hexadecimal digits**, separated by colons. Each group is 16 bits. Valid hexadecimal digits are `0-9` and `a-f` (case-insensitive in notation).

```text
 Full: 2001:0db8:0000:0000:0000:0000:0000:0001

 1. Remove leading zeros per block:
       2001:db8:0:0:0:0:0:1

 2. Replace one consecutive run of zero blocks by '::':
       2001:db8::1

 Use '::' at most once in one IPv6 address.
```

**Rule 1:** omit leading zeros inside a 16-bit block: `00ab` -> `ab`. **Rule 2:** a single `::` can represent a contiguous run of one or more all-zero 16-bit blocks; in canonical text formatting, use the longest zero run (and follow the normal tie-break convention). It **cannot appear twice** because then the number of omitted zero blocks would be ambiguous.

**Examples:** `2001:db8:0:0:4:0:0:1` -> `2001:db8::4:0:0:1` (one run selected). `0:0:0:0:0:0:0:1` -> `::1` (loopback). All-zero address `::` = unspecified address.

### 16.4 IPv6 address types

| Type | Meaning | Representative range/example |
|---|---|---|
| Global unicast | Globally scoped unicast addressing | Common allocation space `2000::/3` |
| Link-local unicast | Link-local neighbour communication | `fe80::/10` |
| Unique local | Private-like local IPv6 use | `fc00::/7`; commonly `fd...` prefixes |
| Multicast | One-to-many group delivery | `ff00::/8` |
| Anycast | Same address on multiple interfaces, routed to an appropriate nearest instance | From unicast address space, not its own fixed prefix |
| Loopback | Host talks to itself | `::1/128` |
| Unspecified | Absence of a chosen source address in certain contexts | `::/128` |

**Unicast** = one target interface; **multicast** = subscribed group; **anycast** = one of several interfaces sharing an address, according to routing. IPv6 does **not** provide IPv4-style broadcast addresses.

### 16.5 IPv6 header - labelled schematic

**Hinglish:** IPv4 label mein fragmentation fields, header checksum aur optional information base header mein mil sakti hai. IPv6 ne fixed main label rakha aur optional things ko extension headers mein arrange kiya.

**English:** The IPv6 base header is always **40 bytes** long and includes eight fields: Version, Traffic Class, Flow Label, Payload Length, Next Header, Hop Limit, Source Address and Destination Address.

```text
             IPv6 FIXED BASE HEADER = 40 BYTES
 +--------+---------------+-----------------------+
 |Version | Traffic Class |       Flow Label      |
 | 4 bits |    8 bits     |        20 bits        |
 +------------------------+-----------------------+
 |      Payload Length    | Next Hdr | Hop Limit  |
 |         16 bits        |  8 bits  |  8 bits    |
 +------------------------------------------------+
 |                                                |
 |           Source IPv6 Address (128 bits)       |
 |                                                |
 +------------------------------------------------+
 |                                                |
 |        Destination IPv6 Address (128 bits)      |
 |                                                |
 +------------------------------------------------+
 |  Optional extension headers and upper payload  |
 +------------------------------------------------+

  This is a conceptual diagram; first three fields total 32 bits.
```

| Field | Length | Function |
|---|---:|---|
| Version | 4 bits | Identifies IPv6 (value 6) |
| Traffic Class | 8 bits | Packet classification / congestion-related bits |
| Flow Label | 20 bits | Can identify a flow for relevant treatment |
| Payload Length | 16 bits | Bytes after fixed 40-byte header (normal case) |
| Next Header | 8 bits | Identifies next extension header or upper-layer protocol |
| Hop Limit | 8 bits | Decremented at forwarding hops |
| Source | 128 bits | Source IPv6 address |
| Destination | 128 bits | Destination IPv6 address |

**Note:** The normal Payload Length field is 16 bits; IPv6 supports a special *jumbogram* mechanism for larger payloads, beyond ordinary semester examples. Do not conclude that the IPv6 **address** size is 40 bytes; only the **base header** is 40 bytes, while each address is 16 bytes (128 bits).

### 16.6 IPv6 extension headers and fragmentation

IPv6 optional information can be placed in extension headers after the fixed base header. The **Next Header** field indicates the following header, which may be another extension header or an upper-layer protocol such as TCP or UDP. Examples include Hop-by-Hop Options, Routing, Fragment and Destination Options headers. Routers do not perform IPv6 fragmentation in transit; when required, the **source** sends appropriately sized packets using fragmentation-related mechanisms.

```text
 IPv6 base header -> Extension Header -> TCP Header -> App Data
        40 B           if present          ...        ...

 IPv6 router sees packet larger than outgoing MTU:
    Cannot fragment in transit -> may send ICMPv6 Packet Too Big.
    Source adjusts effective packet size.
```

**Why this matters:** IPv6 relies heavily on effective **Path MTU Discovery** and correct treatment of ICMPv6 Packet Too Big messages. Blocking all ICMPv6 is not a sensible general network design.

### 16.7 SLAAC, DHCPv6 and Neighbor Discovery

**Hinglish:** New student hostel mein aata hai. Kisi ko admission desk se room details milti hain, aur kuch building information notice board par hoti hai. IPv6 hosts bhi configuration discover karte hain. **SLAAC** mein router announcements se prefix information use ho sakti hai; **DHCPv6** deployments mein additional or address configuration de sakta hai; **Neighbor Discovery** local nodes and routers ke baare mein information exchange karta hai.

**English:** **Stateless Address Autoconfiguration (SLAAC)** permits an IPv6 host to configure addresses using IPv6 router advertisement information and local interface identifier-generation methods. **DHCPv6** can provide additional information and, in stateful modes, address assignments. **Neighbor Discovery (ND)** is based on ICMPv6 messages and supports router discovery, neighbour reachability and link-layer address resolution. IPv6 does not use IPv4 ARP.

**Exam caution:** IPv6 does not require every host to get a public IPv6 address via SLAAC in every network. Actual deployment depends on routing, configuration and policy.

### 16.8 IPv4 and IPv6 - full exam comparison

| Feature | IPv4 | IPv6 |
|---|---|---|
| Address bits | 32 | 128 |
| Example | 192.0.2.1 | 2001:db8::1 |
| Text notation | Four decimal octets | Colon-separated hexadecimal groups |
| Base header size | Usually 20 bytes; variable with options | Fixed 40 bytes |
| Header checksum | Present | Absent |
| Fragmentation | Permitted by routers when appropriate/allowed | Transit routers do not fragment |
| Lifetime field | TTL | Hop Limit |
| Broadcast | Defined | No broadcast |
| Local address resolution | ARP on relevant IPv4 LANs | ICMPv6 Neighbor Discovery |
| Config methods | Static, DHCPv4, others | Static, SLAAC, DHCPv6 |
| Optional fields | IPv4 header options | Separate extension-header mechanism |

### 16.9 IPv6 prefix numerical

**Question:** Find the likely `/64` network prefix of `2001:db8:abcd:12:3456:789a:bcde:10`.

**Solution:** A `/64` prefix preserves the first four 16-bit groups and sets the remaining 64 bits to zero. Network prefix = **`2001:db8:abcd:12::/64`**. Note that IPv6 ordinary subnetting does not generally use IPv4's "subtract network address and broadcast address" rule because IPv6 has no broadcast.

**Question:** How many possible different 128-bit IPv6 addresses exist in the mathematical address space? Answer **`2^128`**. Not every bit pattern is available as an ordinary global unicast address; many types, ranges and policies exist.

***

## 17. Putting It Together - Complete End-to-End Worked Story

### 17.1 Hinglish: mobile hotspot to a website

Tum phone ke hotspot se laptop connect karke `example.com` open karte ho. System domain ko IP address mein resolve kar sakta hai. Laptop dekhta hai destination same local subnet mein nahi hai, so packet default gateway ko bhejta hai. Gateway se ISP aur phir multiple routers ke through destination network tak jaata hai. Har router forwarding table check karta hai. Kisi bottleneck link par crowding hui to queue delay/drop aa sakta hai. TCP connection use ho raha ho to TCP lost data retransmit kar sakta hai, congestion window adjust kar sakta hai. Finally server response same ya different return route se aa sakta hai.

**What the terms mean in this one story:**

- **Point-to-point link:** a direct router-to-router physical/logical link on the path.
- **Routing:** which network path is chosen/learned.
- **Forwarding:** one hop sends one packet to the next interface.
- **Congestion:** bottleneck queue and load problems.
- **IP address:** network-layer destination identification.
- **IP packet:** address/control header plus data.
- **TCP/IP:** Internet protocol family; TCP may carry an application's reliable byte stream.
- **IPv6:** alternative 128-bit IP protocol if network endpoints/path support it.

### 17.2 Complete English - numbered packet journey

1. A user enters a website name in a browser, and the system obtains a suitable server IP address through DNS or a pre-existing result.
2. The chosen application transport protocol creates data units; TCP is an example for reliable stream transport.
3. The IP layer builds an IPv4 or IPv6 datagram containing the source and destination IP addresses.
4. The source host checks its local routing information: if the destination is not on-link, it chooses a default router or another matching next hop.
5. A local-link protocol resolves/uses a next-hop link-layer address where applicable. The host transmits the encapsulating link frame.
6. A router accepts the frame, processes the IP packet, reduces TTL/Hop Limit, performs route lookup and chooses an outgoing interface.
7. The router re-encapsulates the packet for the next link. Multiple such hops may cross different network technologies.
8. If congestion develops, queues and drops may affect packet delivery. Any needed end-to-end reliability and congestion adaptation depend on the transport protocols.
9. At the destination host, IP passes the payload to the indicated next protocol. The application eventually receives usable data.
10. Reply packets travel through the network in the reverse communication direction, though they need not traverse exactly the same routers.

```text
 [User Browser]
       |
  [Transport: TCP/UDP]
       |
  [Internet: IPv4/IPv6 packet]
       |
  [Link frame] -> R1 -> R2 -> R3 -> [Link frame]
                  |    |    |
               route / forwarding decisions
                    |      |
                 queue?   TTL/Hop Limit
                               |
                          [Server host]
```

***

# PART B - EXAM-DIAGRAM REVISION AND QUICK COMPARISONS

## 18. One-page big-picture map of Unit III

```text
                          UNIT III: NETWORK LAYER
                                   |
        +--------------------------+--------------------------+
        |                          |                          |
  POINT-TO-POINT               ROUTING                    INTERNETWORKING
        |                          |                          |
   Nodes and links        Static / Dynamic               TCP/IP suite
   Hop-to-hop path         Shortest path                      |
   Store and forward       Dijkstra, Bellman-Ford           IPv4 / IPv6
   PPP (related)           Distance Vector, Link State       IP packet
        |                  RIP, OSPF, BGP                       |
        |                          |                   Logical addressing
        |                          |                   IPv4 / CIDR / subnet
        +--------------------------+--------------------------+
                                   |
                            CONGESTION CONTROL
                                   |
                     causes -> delays, losses, jitter
                                   |
                  prevention + feedback + traffic shaping
                     leaky bucket / token bucket / TCP
```

## 19. Common 10-mark comparison tables

### 19.1 Packet forwarding versus route computation

| Item | Routing | Forwarding |
|---|---|---|
| Level | Network path selection | Per-packet router action |
| Plane | Control plane | Data plane |
| Information | Topology, metrics, policy | Forwarding entries and packet header |
| Frequency | On updates/failures/changes | For each forwarded packet |
| Example | SPF chooses A-B-D | Router B forwards packet to D |

### 19.2 Datagram network versus virtual circuit network

| Criterion | Datagram | Virtual Circuit |
|---|---|---|
| Setup | No VC setup | Logical circuit setup |
| Packet identification | Usually full destination IP address | Often connection label/identifier |
| Route | Can change independently | Follows pre-established connection path |
| Device state | Usually no per-VC state | May require VC state |
| Examples | IP network service | ATM / Frame Relay concepts |

### 19.3 Dijkstra versus Bellman-Ford

| Criterion | Dijkstra / SPF | Bellman-Ford style |
|---|---|---|
| Information model | Graph/topology and costs | Neighbour distance advertisements |
| Common routing family | Link state | Distance vector |
| Link weights | Non-negative for standard Dijkstra | Bellman-Ford can handle negative weights in abstract graphs without negative cycles; routing metrics normally non-negative |
| Typical concern | LSDB computation/flooding | Count-to-infinity / convergence |
| Core step | Finalise smallest tentative distance | Relax using neighbour's advertised cost |

### 19.4 Fast factual memory list

- OSI Network Layer = **Layer 3**.
- Packet = typical Network Layer unit; frame = Link Layer unit.
- Point-to-point link = **two link endpoints**, not the same as peer-to-peer application architecture.
- Dijkstra = **non-negative weighted shortest path**.
- Distance Vector = **neighbour distance tables / Bellman-Ford**.
- Link State = **flooded LSAs + SPF / Dijkstra**.
- Routing = **path selection**; forwarding = **next-hop packet action**.
- Flow control protects **receiver**; congestion control protects **network**.
- IPv4 address = **32 bits**; IPv6 address = **128 bits**.
- Minimum IPv4 header = **20 bytes**; IPv6 base header = **40 bytes**.
- IPv4 fragment offset is in **8-byte units**.
- Routers do not fragment **IPv6 transit packets**.
- CIDR = **prefix length notation**; `/24` leaves 8 host bits in IPv4.
- RIP uses hop count; OSPF is link state; BGP is path vector.
- TCP's congestion control is **Transport Layer** behaviour, related to network congestion.

\newpage
# PART C - IMPORTANT 20-MARK QUESTIONS WITH COMPLETE MODEL ANSWERS

> **How to attempt a 20-mark question:** Begin with a precise definition (2-3 marks), add a labelled diagram, explain the working step by step (8-10 marks), include examples/comparisons/advantages (4-6 marks), and end with a 3-4 line conclusion. Marks allocation below is a *writing suggestion*, not an official university marking scheme. For a paper asking only one subtopic, omit unrelated material.

## Q1 (20 Marks). Explain the Network Layer, point-to-point networks, its services and major design issues.

### Model answer - complete English

**Introduction.** The Network Layer is the third layer of the OSI model. It enables a packet to travel from a source host to a destination host across possibly many interconnected networks. While the Data Link Layer concentrates on one direct link, the Network Layer provides logical addressing, routing and forwarding through intermediate nodes. It is therefore essential to the functioning of the Internet.

**1. Functions of the Network Layer.** Logical addressing gives each endpoint/interface an IP address or network prefix. Routing selects useful network paths according to available information and policies. Forwarding performs the actual next-hop action at a router. Internetworking permits data to cross LANs and WANs with different lower-layer technologies. The layer also uses mechanisms such as TTL/Hop Limit to prevent indefinite packet loops, and it interacts with error-reporting mechanisms such as ICMP. IP itself is best effort and does not guarantee delivery or correct order.

**2. Point-to-point networks.** A point-to-point link connects exactly two link endpoints. An interconnection of such links forms a network graph of routers and alternative paths. For example, a leased line from one campus router to an ISP router is a point-to-point connection. Unlike a shared broadcast medium, a dedicated P2P link does not need collision arbitration among many stations on the same physical link. The network can, however, have multiple forwarding hops, and each hop adds possible transmission, propagation, processing and queueing delay.

```text
                +---- Router B ----+
                |                  |
  Host A --- Router A          Router D --- Host Z
                |                  |
                +---- Router C ----+

  The data may go through B or C depending on routes and failures.
```

**3. Store-and-forward operation.** In the educational model, each router receives a packet, checks the necessary information, chooses an outgoing interface and forwards the packet. The router usually removes the incoming link-layer frame and creates a new frame for the outgoing link. Thus the link-layer header can change at every hop although the IP packet continues through the internetwork.

**4. Connectionless service.** In a datagram network, each packet is considered an individual forwarding unit. Packets include destination addresses and may follow different routes. No virtual-circuit setup is needed before IP packets can be forwarded. The Internet's IP service follows this connectionless, best-effort model. Packet delivery, timing and order are not guaranteed by IP alone.

**5. Connection-oriented network service.** In a virtual-circuit network, a logical circuit is established first. The network can maintain a mapping from a virtual-circuit identifier to a next outgoing link. Packets belonging to the logical circuit use that established path. Historical technologies such as ATM illustrate a virtual-circuit service. It must not be confused with TCP connections: TCP is transport-layer and can operate over connectionless IP.

**6. Network design issues.** The designer must plan addresses and prefixes, interconnection of different link technologies, routing strategy, scalability, packet-size limits, buffering, failure handling, congestion, traffic prioritisation and security. Different links can have different MTUs, error rates and capacities. Routes should adapt correctly to failures without forming persistent loops. Routers need efficient forwarding tables, sometimes using aggregated routes to reduce size. Appropriate congestion-control methods limit the damage caused by high traffic load.

**7. Advantages and challenges.** Internetworking makes separate networks operate as one communication system and permits flexible deployment of LAN, Wi-Fi and WAN technologies. The challenges are varying link capacity, route convergence, packet losses, congestion and the need for consistent addressing and network administration.

**Conclusion.** The Network Layer is the main layer responsible for communication across multiple networks. Point-to-point links form the pathways between nodes, while routing, forwarding and addressing permit data to reach remote hosts. Datagram and virtual-circuit approaches explain two fundamental ways a network service can be organised.

***

## Q2 (20 Marks). What is routing? Explain static/dynamic routing and Dijkstra's shortest-path algorithm with a numerical example.

### Model answer - complete English

**Definition.** Routing is the process of determining a suitable path for packets to reach destination networks through routers. A router maintains information about reachable prefixes, candidate next hops and metrics. Routing is distinct from forwarding: routing selects or learns paths; forwarding sends each received packet through an appropriate output interface.

**1. Need for routing.** In a large internetwork, a source and destination may be separated by several routers and there may be multiple possible paths. If no routing mechanism existed, routers would not know where to send packets for remote networks. Routing is also needed to deal with link failures, new networks, variable path costs and organisational policies.

**2. Routing table.** A routing table includes destination prefixes, next-hop routers or outgoing interfaces, and possibly metrics and route origins. For example, a route to `10.5.0.0/16` via R2 can guide packets for that network. If a host matches more than one route, the forwarding process normally prefers the longest matching prefix. A default route handles destinations without a more specific match.

**3. Static routing.** Static routes are configured manually by an administrator. They are simple and stable, with minimal routing-protocol overhead. They work well in small or predictable topologies but do not automatically discover an alternate path after every link failure. Manual changes become inconvenient in large networks.

**4. Dynamic routing.** Dynamic routing uses protocols that learn and update route information. RIP uses a distance-vector method and hop count; OSPF uses link-state information and SPF; BGP exchanges path and policy information between routing domains. Dynamic routing improves adaptation to changes but requires computation, protocol traffic and careful configuration.

**5. Graph representation.** A routing topology can be modelled as a graph whose vertices are routers and whose edges are links. A weight may represent configured cost, delay or another metric. The cost of a path is the sum of its link weights when using a simple additive metric. A minimum-cost route is not necessarily the route with the fewest hops.

**6. Dijkstra's algorithm.** Dijkstra's algorithm finds the shortest-cost paths from a source to reachable destinations in a graph with non-negative link weights. It initially assigns distance zero to the source and infinity to the remaining vertices. The smallest tentative distance is repeatedly finalised. For each neighbour, the algorithm checks whether going through the newly selected vertex provides a lower total distance, and updates the distance and predecessor if so.

**7. Solved numerical.** Let edges be `A-B=2`, `A-C=5`, `B-D=1`, `C-D=2`, `B-E=3`, `D-E=1`. Find shortest A-to-E path.

```text
        A ---2--- B ---3--- E
        |         |         |
        5         1         1
        |         |         |
        C ---2--- D --------+
```

Initial: `d(A)=0`, others infinity. After visiting A, set `d(B)=2, d(C)=5`. Visit B, update `d(D)=3, d(E)=5`. Visit D, update `d(E)=min(5,3+1)=4`; route A-B-D-E. Thus shortest cost is **4**, not 5.

**8. Advantages.** Dijkstra gives optimal non-negative weighted shortest paths and forms the basis of SPF route computation in link-state systems. It is suitable for large graphs with efficient data structures. **Limitations** are the need for appropriate topology information and recalculation when routes change; standard Dijkstra is not designed for negative-weight edges.

**Conclusion.** Routing connects source and destination networks. Static routes offer simplicity and control, while dynamic routing adapts to changes. Dijkstra provides a systematic method of finding the least-cost route in link-state routing, as shown in the example.

***

## Q3 (20 Marks). Explain Distance Vector Routing and Link State Routing. Differentiate them and describe the count-to-infinity problem.

### Model answer - complete English

**Introduction.** Dynamic routing techniques allow routers to learn paths without requiring every route to be entered manually. Two important theoretical families are **Distance Vector Routing (DVR)** and **Link State Routing (LSR)**. Both seek useful paths, but they differ in the information routers exchange and how they compute routes.

**1. Distance Vector Routing.** In a distance-vector system, a router maintains estimated costs to destination networks and next hops. It exchanges route information with neighbouring routers. Upon receiving a neighbour's estimate, the router adds the local link cost and compares that candidate with its current estimate. This is the distributed Bellman-Ford principle.

**2. Bellman-Ford relation.** In symbolic form, the cost from x to y is the minimum over neighbours v of `cost(x,v) + advertised distance(v,y)`. For example, if A-to-B costs 2, B-to-C costs 3 and A-to-C directly costs 10, A learns that the route to C through B costs **5** and prefers it over the direct link of cost 10.

**3. Merits and limitations of distance vector.** The algorithm is simple and does not require every router to maintain a complete network graph. However, it relies on potentially stale neighbour estimates, so temporary routing loops and slow convergence may occur. RIP is a traditional example: its hop-count metric regards 16 as unreachable.

**4. Count-to-infinity.** Suppose A reaches a network through B. When B loses connectivity to that network, A may still advertise an outdated route learned from B. B can mistakenly conclude that the destination is reachable through A. They then repeatedly increase route costs in subsequent exchanges, causing a loop until the route is considered unreachable. This is count-to-infinity.

```text
 Initially:   A ---- B ---- C (destination)
                  ^
             A uses B to reach C

 Failure:     A ---- B -X- C
             A retains stale route via B.
             B may believe route via A exists.
             Costs rise as advertisements circulate.
```

**5. Remedies.** **Split horizon** prevents advertising a route back to the neighbour from which it was learned. **Poison reverse** sends it back with an infinite/unreachable metric. **Triggered updates** spread failure information quickly. **Route poisoning** marks failed routes unreachable. Timers and hold-down policies can also reduce instability. These methods reduce but do not universally remove loops.

**6. Link State Routing.** In a link-state system, routers discover immediate neighbours and link costs, then flood link-state advertisements through a suitable routing domain. Routers build a link-state database containing topology information. Each router independently computes shortest paths, commonly with Dijkstra's Shortest Path First algorithm.

**7. Example.** Suppose R1-R2 costs 2, R2-R4 costs 1, R1-R3 costs 5 and R3-R4 costs 2. Link-state calculations at R1 compare cost `2+1=3` through R2 with `5+2=7` through R3, selecting R1-R2-R4.

**8. Merits and limitations of link state.** Routers have a more detailed view of the routing domain and can respond effectively to many topology changes. However, link-state advertisements, database synchronisation and SPF calculations require processing, memory and correct management. OSPF is a common example.

**9. Comparison.** Distance vector exchanges destination-cost information with neighbours, while link state distributes local link descriptions through the routing domain. The former uses Bellman-Ford-style relaxation and may suffer count-to-infinity; the latter uses a topology database and SPF computation and may require greater state-management overhead. Neither is automatically best under every possible set of requirements.

**Conclusion.** Distance-vector and link-state routing are foundational dynamic routing approaches. DVR is comparatively simple but can converge slowly with stale information. LSR supports topology-based shortest-path calculation, generally at the cost of more sophisticated control-plane machinery.

***

## Q4 (20 Marks). Explain flooding, hierarchical routing and Internet routing protocols RIP, OSPF and BGP.

### Model answer - complete English

**Introduction.** Networks can be small campus internetworks or global collections of thousands of independent administrative domains. Routing techniques must therefore handle different needs: rapid dissemination, scalable organisation, and policy-based selection of remote routes. Flooding, hierarchical routing and modern routing protocols address different parts of this problem.

**1. Flooding.** Flooding is a forwarding method in which a node sends a copy of a packet or control message across several eligible outgoing links. It can distribute information widely without first knowing a single optimal path. Its disadvantages are duplicate packets, unnecessary traffic and potentially uncontrolled growth in transmissions if packets are blindly copied. Practical designs use hop limits, unique identifiers, duplicate suppression or tree-based distribution.

**2. Where flooding is used.** Controlled forms of flooding are important for distributing topology-related information in link-state routing. It can also appear in discovery mechanisms. Flooding must not be confused with ordinary unicast IP forwarding, which generally chooses a particular next hop rather than duplicating every packet along every outgoing link.

**3. Hierarchical routing.** Large networks are divided into areas, regions or autonomous systems. Routers have detailed knowledge of local paths but may use summarised information for distant regions. Hierarchy reduces the volume of routing data and improves management, at the possible cost of some suboptimal routes or summarisation complexity.

```text
           INTERNET / INTERDOMAIN ROUTING
                   /         \
                 AS1         AS2
                /   \         |
             Area 0 Area 1  Remote networks
             /  \     |
            R1  R2    R3
```

**4. Routing inside an autonomous system.** An Autonomous System (AS) is a domain administered under a common routing policy. Interior routing protocols typically distribute routes within the domain. RIP and OSPF are examples of Interior Gateway Protocols, though their mechanisms differ.

**5. Routing Information Protocol (RIP).** RIP uses distance-vector principles and a hop-count metric. It is easy to explain and configure in small educational networks. Its familiar maximum usable metric is 15 hops and 16 represents unreachable. Limitations include its simple metric and convergence challenges in some topologies.

**6. Open Shortest Path First (OSPF).** OSPF uses link-state advertisements, a link-state database and shortest-path calculation. Routers discover neighbours and calculate paths using link costs. It can use hierarchical areas; Area 0 is the conventional backbone. OSPF is suitable for many medium and large internal routing designs and adapts to topology changes, though its configuration and database management are more involved than basic RIP.

**7. Border Gateway Protocol (BGP).** The global Internet includes many autonomous systems belonging to different providers and organisations. BGP exchanges reachability information and applies routing policy across these domains. BGP is a path-vector protocol. Attributes, AS paths and local policies influence route selection, so it is inaccurate to describe BGP simply as "shortest-hop routing".

**8. Comparative summary.** RIP: distance-vector, inside an AS, hop count. OSPF: link-state, inside an AS, SPF-based costs. BGP: path-vector, especially between autonomous systems, policy-based reachability. Flooding describes a dissemination technique, whereas hierarchical routing describes network organisation; neither replaces all route selection mechanisms on its own.

**9. Real-life example.** A university might use OSPF inside its campus routing domain and rely on an ISP that participates in BGP for worldwide routes. A building's local links are represented in the campus topology, while aggregated prefixes allow the university to exchange manageable routing information with external systems.

**Conclusion.** Flooding helps disseminate information, hierarchy helps scale routing knowledge, and RIP, OSPF and BGP illustrate different algorithms and administrative scopes. Understanding their relationships is essential for answering Internet-routing examination questions.

***

## Q5 (20 Marks). Explain network congestion, its causes and effects, and congestion-control methods including leaky and token buckets.

### Model answer - complete English

**Definition.** Network congestion occurs when the traffic offered to network resources exceeds the capacity available to serve that traffic efficiently. Congestion is especially important at bottleneck links, where multiple flows compete for limited output bandwidth, buffering and processing capacity. Congestion control attempts to keep delays and losses manageable while achieving useful throughput and fairness.

**1. Causes.** A shared link can receive bursts faster than it can transmit them. Network devices may have limited buffers and limited processing speed. Traffic may concentrate on one route because of topology or policy. Sudden events can create many simultaneous user requests. Poorly controlled retransmissions after packet loss can worsen overload.

**2. Effects.** Queues grow, causing **queueing delay**. Full buffers or queue management may cause **packet drops**. Changes in waiting time produce **jitter**. Users may see slow web pages, disrupted video calls and retransmission timeouts. Extremely high offered traffic may fail to increase useful throughput, because the network spends more effort carrying lost/repeated packets.

```text
  INPUT: 10 + 10 + 10 Mbps
              |
       +--------------+
       | Router Queue |
       +--------------+
              |
        OUTPUT: 15 Mbps

 If long-term offered load exceeds the effective output capacity,
 the queue cannot grow forever; delay and loss increase.
```

**3. Flow control versus congestion control.** Flow control protects the receiving device from being sent too much information at once. Congestion control protects network bottlenecks from excessive load. In TCP, an advertised receiver window helps flow control and a congestion window helps regulate network load. These are related, but they solve different problems.

**4. Prevention-oriented approaches.** Open-loop methods aim to avoid overload through resource planning and policy before serious congestion is observed. Examples include admission control, reasonable retransmission policies, traffic shaping, priority scheduling, and routing design that avoids concentration on unsuitable links.

**5. Feedback-oriented approaches.** Closed-loop control observes congestion through packet loss, queue delay, ECN marks or other measurements and changes behaviour accordingly. A sender may reduce its rate, a network may mark or drop traffic before queues become excessive, or routing policies may adapt where appropriate.

**6. Leaky bucket.** A leaky-bucket traffic shaper accepts bursty incoming packets into a finite queue and releases them at a configured service rate. The analogy is a bucket whose water leaves through a small fixed hole. If incoming data exceeds buffer capacity, the excess may be dropped or deferred under the system's policy. The main advantage is smooth traffic output; a limitation is that it may prevent desirable short bursts.

**Numerical:** If 20 packets arrive in an empty queue that drains at 5 packets per second, total drain time is approximately `20/5=4 seconds` in a simple constant-service model, ignoring propagation and further arrivals.

**7. Token bucket.** A token bucket adds tokens at a fixed rate up to a maximum capacity. To transmit data, a sender must possess sufficient tokens; otherwise the data waits or is policed according to configuration. Unlike strict leaky-bucket shaping, the bucket can accumulate tokens during idle periods and allow a limited burst.

**Numerical:** At a rate of 1000 byte-tokens per second and capacity 3000 byte-tokens, a full bucket permits a 3000-byte burst immediately. Another 1000-byte packet can use newly accumulated tokens after roughly one second.

**8. TCP-based congestion control.** In classic TCP operation, the sender uses a congestion window, slow start, congestion avoidance, and loss/ECN-related response mechanisms. A successful transmission period can increase the permissible outstanding data, while congestion feedback leads to reduction. Modern TCP implementations may use different congestion-control algorithms, but the central idea is adapting offered traffic to network conditions.

**9. Prevention plus correction.** Effective congestion control combines sensible capacity planning, traffic management and feedback. Increasing bandwidth can improve a bottleneck, but it does not by itself prevent congestion elsewhere. Techniques must also account for fairness and important application requirements.

**Conclusion.** Congestion is a network-wide overload problem, not merely a slow receiver. Leaky and token buckets control packet input patterns, while feedback approaches such as TCP congestion control adapt sending behaviour when network conditions change.

***
## Q6 (20 Marks). Explain internetworking and the TCP/IP architecture, with the role of IP and a packet journey.

### Model answer - complete English

**Introduction.** An internetwork is created when two or more separate computer networks are connected so their hosts can communicate. Different LANs and WANs may use Ethernet, Wi-Fi, fibre or other media. The Internet uses the TCP/IP suite to make communication possible across this diversity. IP supplies a common logical addressing and forwarding method, while link technologies handle each individual hop.

**1. Need for internetworking.** A college can have independent networks in the library, laboratory, accounts office and administration building. Users need to reach shared resources and remote Internet services without knowing the details of every intervening link. Internetworking provides common addressing and pathways across networks with potentially different hardware and link-layer protocols.

**2. Major components.** Hosts run the TCP/IP stack, routers connect different networks, routes indicate reachable destinations, and lower-layer protocols transport IP packets over each hop. Additional services such as DNS, DHCP, ARP/Neighbor Discovery and ICMP assist with name resolution, address configuration, local next-hop resolution and diagnostics.

**3. TCP/IP architecture.** The model is commonly represented with four layers: Application, Transport, Internet and Link/Network Access. The **Application Layer** supplies services such as web communication (HTTP), DNS and email protocols. The **Transport Layer** contains TCP and UDP; TCP offers reliable ordered byte-stream service, while UDP provides an unordered datagram transport without TCP's delivery guarantees. The **Internet Layer** includes IPv4/IPv6 and related control protocols such as ICMP. The **Link Layer** uses Ethernet, Wi-Fi, PPP and other link technologies for transfer over local media.

```text
     APPLICATION  [HTTP, DNS, SMTP, FTP]
             |               data
       TRANSPORT  [TCP / UDP]
             |           segment/datagram
        INTERNET  [IPv4 / IPv6]
             |              packet
            LINK  [Ethernet / Wi-Fi]
             |               frame
          PHYSICAL MEDIUM
```

**4. Encapsulation.** An application creates data. TCP, when used, adds a transport header containing port and sequencing information. IP places that transport unit into an IP packet with source and destination addresses. A link-layer protocol then places the IP packet inside a frame for the next hop. This wrapping is called encapsulation. A router strips the incoming link frame, forwards the IP packet based on its destination address and builds a suitable new frame.

**5. Difference between TCP and IP.** TCP belongs to the Transport Layer. It handles end-to-end sequence ordering, acknowledgements, retransmission, flow control and congestion control. IP belongs to the Internet/Network Layer. It handles logical addressing and best-effort forwarding across routers. An IP packet may be lost or arrive out of order; TCP can repair many such delivery issues for a TCP connection.

**6. Example of communication.** A laptop on a home Wi-Fi network requests data from a server. The browser obtains a destination IP address through DNS, and the operating system selects a route. The laptop sends an IP packet in a Wi-Fi frame to the default gateway if the destination is remote. The router sends the packet onward over an ISP link, perhaps with a different frame technology. Each intermediate router makes a forwarding decision. At the server, the IP layer passes data to the appropriate transport protocol and application. A response follows a possibly different return route.

**7. Other supporting protocols.** IPv4 ARP resolves a local next-hop IPv4 address to a link-layer address when necessary. IPv6 uses Neighbor Discovery instead. ICMP supports diagnostics and some error reporting. DHCP may provide IP configuration, and NAT may translate private IPv4 address/port information at a gateway.

**8. Benefits and limitations.** TCP/IP supports scalable communication across different network technologies, globally useful addressing and routing, and independent evolution of applications and links. Network failures, packet loss, congestion, security threats and operational complexity remain important challenges. IP is not a guarantee of delivery, nor is the TCP/IP four-layer representation identical to the seven-layer OSI teaching model.

**Conclusion.** Internetworking makes separate networks communicate as one system. The TCP/IP suite separates application, transport, Internet and link responsibilities, and IP is central to routing packets across diverse physical networks.

***

## Q7 (20 Marks). Draw and explain the IPv4 packet format and solve a fragmentation numerical.

### Model answer - complete English

**Definition.** An IPv4 datagram is a network-layer data unit with an IPv4 header followed by payload. Its header contains the information required for addressing, forwarding, lifetime control and fragmentation. The minimum header is 20 bytes (IHL 5), and the maximum standard header length with options is 60 bytes.

**IPv4 header diagram:**

```text
  +-----+-----+----------+----------------+
  | Ver | IHL | DSCP/ECN |  Total Length  |
  +----------------------+-----+----------+
  |     Identification   |Flags| Frag Off |
  +-----------+----------+----------------+
  |    TTL    | Protocol | Header Checksum|
  +---------------------------------------+
  |          Source IPv4 address          |
  +---------------------------------------+
  |        Destination IPv4 address       |
  +---------------------------------------+
  |        Options / Padding if any       |
  +---------------------------------------+
  |                Payload                |
  +---------------------------------------+
```

**1. Version and IHL.** Version is a 4-bit field equal to 4 for IPv4. IHL means Internet Header Length, measured in units of four bytes. If IHL is 5, the header has 20 bytes; if IHL is 6, it has 24 bytes.

**2. Service fields and Total Length.** The old Type of Service byte is now interpreted for DSCP and ECN uses. DSCP can support differentiated traffic handling, while ECN can communicate congestion marking when supported. Total Length is a 16-bit field giving the complete IPv4 packet size in bytes, including the header.

**3. Identification, flags and fragment offset.** These support fragmentation and reassembly. The Identification field is used to associate fragments of one original datagram. DF means Don't Fragment; MF means More Fragments. Fragment Offset measures the location of the fragment's data in the original payload in **8-byte units**.

**4. TTL and Protocol.** Time To Live limits a datagram's forwarding lifetime by being decremented at routers. The Protocol field identifies the upper-layer payload, for example TCP (6), UDP (17) or ICMP (1). Protocol does not specify an application port number.

**5. Header checksum and addresses.** The IPv4 Header Checksum detects errors in the IPv4 header, not in the complete packet payload. Routers must account for relevant header changes while forwarding. The Source and Destination Address fields are each 32 bits. Options and Padding, when used, extend the header beyond 20 bytes.

**6. Fragmentation concept.** A network link's MTU limits the size of an IP packet that can traverse it. When an IPv4 packet exceeds the next link MTU and DF is clear, a router may divide it into smaller fragments, each with its own header. The final host reassembles them. The data lengths of all non-final fragments must be multiples of 8 bytes.

**7. Numerical.** Original datagram Total Length = 4000 bytes, header = 20 bytes, outgoing MTU = 1500 bytes. Original data = 3980 bytes. Largest per-fragment data = `1500-20=1480` bytes; this is a multiple of 8. Split data into `1480, 1480, 1020`. Offsets in 8-byte units are `0`, `1480/8=185`, `2960/8=370`. MF values are 1, 1 and 0.

| Fragment | Data | Total Length | Offset | MF |
|---|---:|---:|---:|---:|
| First | 1480 | 1500 | 0 | 1 |
| Second | 1480 | 1500 | 185 | 1 |
| Final | 1020 | 1040 | 370 | 0 |

**8. Advantages and limitations of fragmentation.** Fragmentation allows a datagram to traverse a smaller-MTU IPv4 link, but adds header overhead and loss sensitivity. A missing fragment can prevent full reassembly. Path MTU Discovery can help avoid unnecessary fragmentation. IPv6 routers, unlike IPv4 routers, do not fragment packets in transit.

**Conclusion.** The IPv4 header contains the fields required for logical addressing and network forwarding. Understanding IHL, Total Length, TTL, Protocol, Identification, MF and Fragment Offset is particularly important for diagram-based and numerical questions.

***

## Q8 (20 Marks). Explain IPv4 addressing, classful addressing, CIDR, subnet masks and subnetting with solved examples.

### Model answer - complete English

**Introduction.** IPv4 uses 32-bit addresses to identify network-layer interfaces and support routing. Such addresses are written as four decimal octets separated by dots, for example `192.168.10.77`. Each octet represents eight bits and ranges from 0 to 255. Modern IPv4 routing uses a prefix length to distinguish the routing network portion from the remaining address bits.

**1. Network and host portions.** An IPv4 prefix such as `/26` means that 26 leading bits identify the network prefix and 6 remaining bits identify addresses inside the subnet. A subnet mask shows this separation with 1s for prefix bits and 0s for remaining bits. The network address is found through a bitwise AND between an IP address and its subnet mask.

**2. Classful addressing.** Historically, IPv4 networks were described as Class A, B and C with default prefix lengths /8, /16 and /24. Class D addressed multicast and Class E described reserved/experimental ranges. The historical Class A first octet was generally 1-126, Class B 128-191, and Class C 192-223, while several ranges such as 127/8 are reserved. Modern networks generally use CIDR rather than fixed class boundaries.

**3. CIDR.** Classless Inter-Domain Routing writes a network with an explicit prefix, such as `192.168.10.0/24`. It permits flexible subnet sizes and route aggregation. A router uses longest-prefix matching to find the most specific applicable route. For instance, `10.2.3.4` matches both `10.2.0.0/16` and `10.2.3.0/24`; `/24` is more specific.

**4. Address categories.** Private IPv4 ranges are `10.0.0.0/8`, `172.16.0.0/12` and `192.168.0.0/16`. Loopback includes `127.0.0.0/8`; the unspecified address is `0.0.0.0`. Publicly routable addresses are assigned and routed under relevant Internet allocation policies, so non-private alone is not sufficient to imply global reachability.

**5. Formula for subnetting.** A `/n` IPv4 block contains `2^(32-n)` addresses. A conventional subnet with network and broadcast endpoint reservations has `2^(32-n)-2` assignable host addresses. Subnetting borrows bits from the former host portion to create more prefixes. `/31` point-to-point addressing and `/32` single-host routes are exceptions to the simple minus-two calculation.

**6. Solved example A.** For `192.168.10.77/26`, the subnet mask is `255.255.255.192`. Block size is `256-192=64`, so ranges end at 63, 127, 191 and 255. Address 77 lies in `64-127`. Network address: `192.168.10.64`. Broadcast: `192.168.10.127`. Traditional host range: `.65` through `.126`. Usable hosts: `2^6-2=62`.

**7. Solved example B.** Divide `192.168.20.0/24` into eight equal subnets. Eight subnets require borrowing three bits because `2^3=8`. New prefix `/27`, mask `255.255.255.224`. Each subnet contains 32 addresses and traditionally supports 30 hosts. Network starts are `.0, .32, .64, .96, .128, .160, .192, .224`.

**8. Solved example C.** A lab needs 50 usable host addresses. Since `/27` supports only 30, choose `/26` supporting 62 traditional hosts. The required mask is `255.255.255.192`.

**9. VLSM and summarisation.** Variable Length Subnet Masking allocates subnets of different sizes based on requirements. For example, 100-host, 50-host and 20-host groups can receive `/25`, `/26` and `/27` subnets respectively from a suitably aligned larger block. Aggregation combines contiguous aligned prefixes into a shorter prefix to reduce route-table detail.

**Conclusion.** IPv4 addressing, subnet masks and CIDR form the basis of practical network design. Subnetting gives administrators flexible allocation, and route aggregation assists scalable internetworking. Clear binary and block-size calculations are valuable in exams.

***

## Q9 (20 Marks). Explain IPv6 addressing, header format, advantages, working and differences from IPv4.

### Model answer - complete English

**Introduction.** IPv6 is Internet Protocol version 6, designed to support modern internetworking with a vastly larger address space and a redesigned packet header. IPv6 uses **128-bit addresses**, compared with the **32-bit addresses** of IPv4. Like IPv4, it provides best-effort, connectionless datagram communication.

**1. Why IPv6 was developed.** The large number of Internet-connected systems made IPv4 address scarcity a major challenge. IPv6 provides much greater address capacity and a different arrangement of header fields and optional information. It supports hierarchical address allocation, multicast/anycast and methods of automatic configuration. A larger address space alone is not a guarantee of security or speed.

**2. IPv6 address representation.** An IPv6 address contains eight 16-bit hexadecimal groups separated by colons, for example `2001:0db8:0000:0000:0000:0000:0000:0001`. Leading zeros in a group may be removed; one run of all-zero groups can be represented by `::`. Thus the address becomes `2001:db8::1`. `::` may appear only once in a single shortened address.

**3. Address types.** Unicast identifies one addressable interface, multicast identifies group delivery, and anycast uses the same unicast-style address on more than one interface so routing selects an appropriate instance. Link-local addresses commonly fall within `fe80::/10`, unique-local within `fc00::/7`, and multicast within `ff00::/8`. `::1` is loopback and `::` is unspecified. IPv6 does not use a broadcast address.

**4. Base header.** IPv6 has a fixed **40-byte base header**. The fields are Version (4 bits), Traffic Class (8 bits), Flow Label (20 bits), Payload Length (16 bits), Next Header (8 bits), Hop Limit (8 bits), Source Address (128 bits) and Destination Address (128 bits).

```text
  +-----------+----------------+------------------+
  | Version 6 | Traffic Class  |   Flow Label     |
  +----------------------------+-------+----------+
  |      Payload Length        | Next  | Hop Limit|
  +----------------------------+-------+----------+
  |       Source IPv6 Address (128 bits)          |
  +----------------------------------------------+
  |     Destination IPv6 Address (128 bits)       |
  +----------------------------------------------+
```

**5. Extension headers.** Optional information can be stored in extension headers after the base IPv6 header. The Next Header field indicates the following header or the upper-layer payload protocol. Extension headers permit the main IPv6 header to remain a standard fixed length.

**6. IPv6 fragmentation.** Unlike IPv4 routers, IPv6 routers do **not** fragment packets while forwarding. If a packet is too large for a next-hop link, a router may send an ICMPv6 Packet Too Big message; the sending host then needs a suitable packet size. When fragmentation is required, the source uses the IPv6 Fragment extension-header mechanism.

**7. Configuration and neighbour discovery.** SLAAC can allow hosts to configure addresses using router-advertised prefix information. DHCPv6 may provide addresses or other configuration, depending on design. Neighbor Discovery is based on ICMPv6 and helps with neighbour, router and reachability information. IPv6 does not use IPv4 ARP.

**8. Comparison with IPv4.** IPv4 has 32-bit addresses and a variable-length header with checksum; IPv6 has 128-bit addresses and a fixed 40-byte base header without header checksum. IPv4 routers may fragment when permitted, but IPv6 routers do not fragment transit packets. IPv4 supports broadcast, whereas IPv6 uses multicast and other approaches. IPv4 uses TTL; IPv6 uses Hop Limit.

**9. IPv6 prefix example.** If the address is `2001:db8:abcd:12:3456:789a:bcde:10/64`, the network prefix is `2001:db8:abcd:12::/64`. A /64 IPv6 prefix contains `2^64` possible address combinations in its lower 64 bits, but practical assignment is governed by IPv6 architecture and policies rather than the IPv4 broadcast subtraction formula.

**Conclusion.** IPv6 expands address capacity and modernises header and configuration mechanisms while retaining the essential IP concept of packet-based internetworking. Learning the address format, base header, Hop Limit, fragmentation and IPv4 differences is important for long-answer examinations.

***

## Q10 (20 Marks). Explain how IP packets are forwarded across networks using routing, ARP/ND, ICMP and address translation. Include an example.

### Model answer - complete English

**Introduction.** Internet communication often crosses many local and wide-area networks. A host creates an IP datagram, and intermediate routers examine destination network prefixes to forward it. Supporting protocols allow devices to locate next-hop interfaces, configure addresses and diagnose errors. Some networks also apply Network Address Translation at their boundaries.

**1. Initial IP configuration.** A host needs a suitable IP address, prefix length, and routing information. In many IPv4 networks, configuration can be obtained from DHCP. For IPv6, SLAAC and/or DHCPv6 may be used. A default route tells the host which router to use when it lacks a more specific on-link route for the destination.

**2. Domain name versus IP address.** A person commonly enters a domain name such as `example.com`, which can be resolved by DNS into an IPv4 A record or IPv6 AAAA record. Routers do not normally route by looking at the website's text name; routing decisions operate on destination network-layer address information.

**3. Next-hop resolution.** An IPv4 host on an Ethernet-like LAN uses ARP when necessary to learn the link-layer address of the selected local next hop. An IPv6 host uses ICMPv6 Neighbor Discovery for comparable local neighbour resolution. If a destination is outside the local prefix, the link frame is addressed to the appropriate next-hop router's local interface rather than directly to the faraway server's MAC address.

**4. Link-layer encapsulation.** The IP datagram is carried inside an outgoing Ethernet, Wi-Fi or other link frame. Once a router receives the frame, it processes the IP packet and sends it in a new frame on the outgoing link. Source/destination MAC information on each local link can differ, while the network-layer addresses continue to identify logical endpoints, subject to translation and other special processing.

**5. Forwarding table.** The router performs a destination-prefix lookup and selects the most specific matching prefix, generally through longest-prefix matching. The result specifies an outgoing interface and possibly another router's next-hop IP address. Static routing or dynamic protocols such as OSPF, RIP and BGP help determine the routes installed in the routing system.

**6. Packet lifetime and ICMP.** Each IPv4 router decreases TTL, while IPv6 routers decrease Hop Limit. This prevents a packet from circulating forever if routing errors create a loop. If the lifetime expires, the router discards the packet and may send a suitable ICMP Time Exceeded message. ICMP also supports certain error messages and echo request/reply diagnostics. These messages do not guarantee that every failure will always be reported.

**7. Address translation.** Many home networks use private IPv4 addresses, such as `192.168.1.10`, which are not directly routed across the public Internet. A gateway can use **NAPT**, a common form of NAT, to translate source address and port information into a public-side mapping and translate matching replies back. NAT is widely deployed with IPv4 address scarcity, but it changes some end-to-end addressing assumptions and requires suitable state/configuration.

**8. Full example.** A laptop at `192.168.1.10/24` accesses a remote web server. It sees that the server is not within `192.168.1.0/24` and selects its default gateway `192.168.1.1`. ARP, when necessary, obtains the gateway interface's MAC address. The laptop sends a frame containing a TCP/IP packet. The home router may translate private source IPv4 address/port information, then forwards the packet to the ISP. ISP and other routers use forwarding tables, reduce TTL and send the datagram across multiple links. The server processes the request and returns data. If TCP is used, the endpoints handle transport reliability and congestion behaviour.

**9. IPv6 differences.** On an IPv6 path, 128-bit IPv6 addresses and Neighbor Discovery are used. IPv6 routers do not fragment transit packets; ICMPv6 Packet Too Big messages help path MTU behaviour. Native IPv6 connectivity generally does not need IPv4-style address-sharing NAT, although translation may be used for interoperability or specific policies.

**Conclusion.** Efficient internetworking depends on coordinated responsibilities: IP supplies addressing, routers perform prefix-based forwarding, local neighbour protocols support each hop, ICMP gives diagnostics and configuration/translation mechanisms help connect real networks.

\newpage
# PART D - SHORT ANSWERS, FIVE-MARK QUESTIONS AND NUMERICAL PRACTICE

## 20. Forty important 2-mark questions with answers

**1. What is the Network Layer?** The Network Layer is OSI Layer 3. It supports logical addressing and forwarding of packets between hosts across one or more connected networks.

**2. Define routing.** Routing is the process of selecting or learning paths through networks to reach destinations according to metrics, topology and/or policy.

**3. Define forwarding.** Forwarding is a router's per-packet action of using the destination and forwarding information to select an outgoing interface or next hop.

**4. What is a point-to-point link?** It is a communication link that directly joins two link-level endpoints, such as two routers connected by a dedicated circuit.

**5. Is point-to-point the same as peer-to-peer?** No. Point-to-point describes a link with two endpoints. Peer-to-peer describes a resource-sharing/application architecture among peers.

**6. What is store-and-forward switching?** It is a model in which an intermediate node receives a packet, buffers/processes it, and then forwards it to the next link.

**7. What is a hop?** A hop is a forwarding step over a link between consecutive network devices on a path; a route can consist of several hops.

**8. What is a routing metric?** A value or preference used to compare routes, such as hop count, link cost, delay or administratively set policy-related criteria.

**9. What is a routing table?** A data structure containing reachability information for destination prefixes and the appropriate outgoing interface or next hop, with relevant route properties.

**10. What is a default route?** A less-specific fallback route, commonly `0.0.0.0/0` for IPv4 or `::/0` for IPv6, used when no more specific applicable route is found.

**11. What is static routing?** Routing information entered by an administrator rather than automatically learned by a dynamic routing protocol.

**12. What is dynamic routing?** Routing where routers use protocols to exchange/derive reachability information and adapt to topology changes.

**13. State the purpose of Dijkstra's algorithm.** It computes the minimum cumulative cost paths from a source in a graph with non-negative edge weights.

**14. What is link relaxation?** Updating a tentative route cost when a newly discovered path through a node has lower total cost than the stored estimate.

**15. What is Distance Vector Routing?** A distributed routing method in which routers learn destination-cost estimates from neighbours and update routes using a Bellman-Ford principle.

**16. What is count-to-infinity?** A distance-vector failure mode in which stale circular route advertisements cause routers to repeatedly increase their distance estimates to an unreachable destination.

**17. Define split horizon.** A route learned through a neighbour is not advertised back toward that same neighbour as a usable route.

**18. Define poison reverse.** A router advertises a route back toward the neighbour it learned it from with an unreachable metric rather than a usable metric.

**19. What is Link State Routing?** A method where routers distribute descriptions of their links, build a topology database and compute routes, usually using SPF.

**20. What is an LSA?** Link-State Advertisement: a control advertisement containing information about a router's or network's state for relevant link-state routing processes.

**21. Expand RIP and OSPF.** RIP: Routing Information Protocol. OSPF: Open Shortest Path First.

**22. What is BGP?** Border Gateway Protocol is a path-vector Internet routing protocol for distributing reachability and routing-policy information, notably between autonomous systems.

**23. What is congestion?** A condition where offered traffic exceeds effective network resource capacity, causing queues, delays, drops or inefficient performance.

**24. Differentiate flow and congestion control briefly.** Flow control prevents overwhelming the receiver, while congestion control responds to overload in shared network resources.

**25. Define leaky bucket.** A shaping model that buffers arriving packets and releases data at a controlled rate, smoothing bursts and potentially dropping excess arrivals.

**26. Define token bucket.** A rate-control mechanism that creates tokens at a configured rate and allows transmissions when enough tokens are available, permitting limited bursts.

**27. What is TCP congestion window?** `cwnd` is sender-maintained TCP state limiting how much data may be outstanding according to the congestion-control algorithm.

**28. What is internetworking?** The interconnection of separate networks to allow their hosts to communicate through a common addressing/forwarding architecture, commonly IP.

**29. List the common four TCP/IP layers.** Application, Transport, Internet and Link/Network Access.

**30. What is an IP datagram?** A network-layer packet containing an IP header and payload and forwarded independently under the IP service model.

**31. How many bits are in IPv4 and IPv6 addresses?** IPv4 is 32 bits; IPv6 is 128 bits.

**32. What is the minimum IPv4 header length?** 20 bytes, corresponding to IHL value 5 when no options are present.

**33. What is TTL?** IPv4 Time To Live is a header field decremented on forwarding to limit packet lifetime and prevent endless loops.

**34. What is MTU?** Maximum Transmission Unit: the largest packet or protocol data unit a particular link can carry at the relevant layer without fragmentation or other special handling.

**35. What is the IPv4 fragment-offset unit?** The Fragment Offset field measures position in the original payload in units of 8 bytes.

**36. What is CIDR?** Classless Inter-Domain Routing is a method of describing IP networks with explicit prefix lengths, such as `/24`, enabling flexible allocation and aggregation.

**37. Write all three RFC 1918 private IPv4 blocks.** `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`.

**38. What is the IPv6 base-header size?** The IPv6 fixed base header is 40 bytes, and each source/destination IPv6 address is 128 bits.

**39. What is `::1` in IPv6?** It is the IPv6 loopback address, used for communications with the local host.

**40. Does IPv6 use ARP or broadcast?** IPv6 uses ICMPv6 Neighbor Discovery rather than IPv4 ARP, and has no IPv4-style broadcast address.

***

## 21. High-value 5-mark questions - answer-writing checklist

1. **Routing versus forwarding:** define both, identify control/data planes, routing table example, conclusion.
2. **Point-to-point networks:** diagram with two routers and multi-hop topology, advantages and limitations, distinguish P2P from peer-to-peer.
3. **Static versus dynamic routing:** definition, four comparison points, applications.
4. **Dijkstra's algorithm:** objective, algorithm steps, one three/four-node numerical, non-negative weight requirement.
5. **Count-to-infinity:** stale-route loop diagram, example, split horizon and poison reverse.
6. **Distance vector versus link state:** comparison table and RIP/OSPF examples.
7. **Causes of congestion:** bottleneck, buffer, burst, loss/retransmission, effects on delay and throughput.
8. **Leaky versus token bucket:** both labelled diagrams, fixed-rate output versus permitted bursts, simple example.
9. **TCP/IP model:** four layers with protocol names, encapsulation diagram.
10. **IPv4 header:** minimum 20-byte structure and principal fields.
11. **IPv4 classes and CIDR:** historical A/B/C defaults; modern explicit prefix; one `/26` example.
12. **IPv4 versus IPv6:** address sizes, header format, checksum, fragmentation and address resolution.
13. **IPv6 address notation:** eight groups, leading zero deletion, `::` rule and three examples.
14. **IPv6 header:** 40 bytes, eight fields, extension headers and no transit-router fragmentation.

## 22. High-value 10-mark questions - suggested answer structure

1. **Network Layer design:** introduction, P2P diagram, datagram versus virtual circuit, design issues, functions.
2. **Dijkstra with example:** graph representation, algorithm steps, worked table, route calculation, merits/limitations.
3. **Distance Vector Routing:** Bellman-Ford relation, neighbour updates, example, count-to-infinity and remedies.
4. **Link State Routing:** neighbour discovery, LSAs, flooding, LSDB, SPF, OSPF, comparison.
5. **Routing protocols:** short explanation of RIP/OSPF/BGP, IGP vs EGP, AS, hierarchy.
6. **Congestion:** definition, sources, symptoms, flow control difference, prevention/feedback.
7. **Traffic shaping:** leaky bucket and token bucket diagrams, rate and burst numerical, compare.
8. **TCP/IP internetworking:** four-layer table, packet encapsulation, host-router-host path.
9. **IPv4 fragmentation:** complete header-relevant fields, MTU, offset units, solved table.
10. **IPv4 subnetting:** masks and binary method, CIDR, block-size method, two numericals.
11. **IPv6 addressing:** format, compression, address types, `/64` example.
12. **IPv6 design:** base header, extension headers, Neighbor Discovery, IPv4 comparison.

***

## 23. Twenty solved and practice numericals (with answers)

### N1. Hop count

**Q:** Route `A-R1-R2-R3-B` has how many hops and intermediate routers? **A:** Four links/hops; three intermediate routers.

### N2. Store-and-forward delay

**Q:** A 1200-byte packet crosses 4 links, each 2 Mbps; ignore other delays. **Method:** `1200x8=9600 bits`; per link `9600/2,000,000=0.0048 s`. Four links: `4x4.8=19.2 ms`. **A:** 19.2 ms.

### N3. Weighted shortest path

**Q:** A-B=2, A-C=7, B-C=3, C-D=2, B-D=10. Find A-D. **A:** `A-B-C-D`, cost `2+3+2=7`; other options are more expensive.

### N4. Distance-vector update

**Q:** A-to-B link cost 4. B advertises D cost 6. A's direct D cost is 12. **A:** Via B cost `4+6=10`; choose B, update cost from 12 to 10.

### N5. Alternate route after failure

**Q:** A-to-D via B costs 6; via C costs 9. A-B fails and only C route remains valid. **A:** Select via C with cost 9 (assuming no other better valid route).

### N6. Leaky bucket drain

**Q:** 35 packets arrive at once; shaping rate 7 packets/s; enough buffer. **A:** `35/7=5 s` in the simple constant-rate model.

### N7. Token bucket rate

**Q:** Tokens accumulate at 2 KB/s up to capacity 6 KB. Initially full. How many KB can be sent immediately? **A:** 6 KB of accounted data, then about 2 KB more after a second of replenishment if nothing else consumes tokens.

### N8. IHL and payload

**Q:** IPv4 IHL=7, Total Length=1700. **A:** Header `7x4=28 bytes`; payload `1700-28=1672 bytes`.

### N9. IPv4 fragmentation

**Q:** IPv4 datagram 3000 bytes total, header 20, MTU 1000. **Solution:** Payload=2980; maximum non-final payload is `floor((1000-20)/8)x8 = 976` bytes. Three full payloads `976x3=2928`, remainder `52`. Four fragments:

| Fragment | Payload | Total Length | Offset | MF |
|---|---:|---:|---:|---:|
| 1 | 976 | 996 | 0 | 1 |
| 2 | 976 | 996 | 122 | 1 |
| 3 | 976 | 996 | 244 | 1 |
| 4 | 52 | 72 | 366 | 0 |

**Check:** `976+976+976+52=2980`. The simple fragment-offset calculation assumes a common initial header and no extra options.

### N10. /24 network

**Q:** `192.168.7.120/24`. **A:** Network `192.168.7.0`, broadcast `192.168.7.255`, traditional usable `254`.

### N11. /26 range

**Q:** `10.1.5.191/26`. **A:** Last-octet range `128-191`; network `10.1.5.128`, broadcast `10.1.5.191`, assignable `.129-.190`. Here `.191` itself is the broadcast address and should not be assigned to an ordinary host in that subnet.

### N12. /27 range

**Q:** `192.168.1.155/27`. **A:** Mask `.224`, block size 32. Address 155 lies in `128-159`: network `192.168.1.128`, broadcast `.159`, usable `.129-.158` (30 addresses).

### N13. /20 calculation

**Q:** `172.20.91.4/20`. **A:** mask `255.255.240.0`; third-octet block 16. The 91 lies in 80-95. Network `172.20.80.0`, broadcast `172.20.95.255`.

### N14. Hosts for /28

**Q:** How many traditional usable IPv4 host addresses in `/28`? **A:** `2^(32-28)-2 = 2^4-2 = 14`.

### N15. Choose mask for 120 hosts

**Q:** Minimum conventional subnet for at least 120 hosts? **A:** `/25`, since `2^7-2=126`; mask `255.255.255.128`.

### N16. Subnet count

**Q:** Split `/24` into `/28` networks. **A:** Borrow 4 bits; `2^4=16` subnets, 16 total addresses each and 14 traditional usable addresses each.

### N17. VLSM

**Q:** Allocate 60, 25 and 10 hosts from `192.168.30.0/24`, largest first. **A:** 60 -> `192.168.30.0/26` (addresses 0-63); 25 -> `192.168.30.64/27` (64-95); 10 -> `192.168.30.96/28` (96-111).

### N18. Route aggregation

**Q:** Aggregate `10.20.4.0/24` and `10.20.5.0/24`. **A:** `10.20.4.0/23`, assuming correct aligned contiguous coverage.

### N19. IPv6 prefix

**Q:** Network address of `2001:db8:ab:cd:1234:0:0:1/64`? **A:** `2001:db8:ab:cd::/64`.

### N20. IPv6 zero compression

**Q:** Compress `2001:0db8:0000:0000:0000:0000:0123:0004`. **A:** `2001:db8::123:4`.

***

## 24. Most common mistakes to avoid in the exam

1. Writing that the Network Layer guarantees reliable delivery. It usually does **not** in IP.
2. Confusing **point-to-point** with **peer-to-peer**.
3. Using **hop count** as the only possible shortest-path metric; weighted costs can differ.
4. Claiming Dijkstra works with arbitrary negative edge weights.
5. Saying distance-vector routers must know the whole network topology; link-state is the method normally associated with building a topology database.
6. Treating **flow control** and **congestion control** as identical.
7. Claiming TCP is an IP-layer routing protocol; TCP is a Transport Layer protocol.
8. Forgetting `IHL x 4` to convert an IPv4 header length to bytes.
9. Using IPv4 Fragment Offset in bytes rather than **units of 8 bytes**.
10. Taking the maximum fragment payload as MTU without subtracting the IP header.
11. Subtracting two from IPv6 address counts because of IPv4 network/broadcast conventions.
12. Claiming all `172.x.x.x` addresses are private; the RFC 1918 private block is specifically `172.16.0.0/12`.
13. Compressing an IPv6 address with **two separate `::`** sequences; only one is permitted.
14. Saying IPv6 routers fragment transit packets; IPv6 fragmentation must be done by the source.
15. Mixing up **TTL/Hop Limit** with a measured round-trip delay; TTL/Hop Limit is a limiting field, not an Internet speed metric.

***

## 25. Seven-day Unit III revision plan

| Day | Study targets | Practice |
|---|---|---|
| Day 1 | Network Layer, point-to-point links, datagram/VC | Q1 + 2-mark Q1-10 |
| Day 2 | Routing, tables, metrics, Dijkstra | Dijkstra Q2 + N3-N5 |
| Day 3 | Distance vector, link state, RIP, OSPF, BGP | Q3 and Q4, graph diagrams |
| Day 4 | Congestion, leaky/token bucket, flow control | Q5, N6-N7 |
| Day 5 | Internetworking, TCP/IP, IPv4 packet, fragmentation | Q6-Q7, N8-N9 |
| Day 6 | IPv4 address, subnetting, CIDR, VLSM | Q8, N10-N18 |
| Day 7 | IPv6, all comparison tables, complete mock | Q9-Q10, N19-N20 |

### Night-before exam checklist

- Draw **at least six diagrams** without looking: point-to-point graph, Dijkstra graph, distance-vector loop, leaky bucket, IPv4 header, IPv6 header.
- Revise **four formulas**: `L/R` transmission time; `2^(32-prefix)` IPv4 block size; `2^(host bits)-2` conventional host count; fragment offset in **8-byte units**.
- Solve **three subnets** in under ten minutes each, then one VLSM problem.
- Practise full English definitions and 20-mark introductions; learn the Hinglish story for understanding but write technical English when your exam expects English.
- Explicitly label diagrams and identify data units (frame versus packet versus segment).

## 26. Glossary of essential Unit III terms

| Term | Very simple meaning |
|---|---|
| Router | Device that forwards IP packets between networks |
| IP packet | Network-layer address/control information plus payload |
| Prefix | Leading address bits identifying a routed network |
| Next hop | Next router/interface chosen on the packet path |
| Routing | Calculating/learning useful paths |
| Forwarding | Sending a packet toward its next hop |
| Metric | Cost or preference used when comparing routes |
| Dijkstra | Shortest-path method with non-negative edge weights |
| Bellman-Ford | Relaxation principle commonly associated with DVR |
| RIP | Traditional hop-count distance-vector protocol |
| OSPF | Link-state Interior Gateway Protocol |
| BGP | Policy-aware path-vector routing protocol |
| Autonomous System | Network administrative routing-policy domain |
| Congestion | Too much offered load for available network resources |
| Queueing delay | Waiting time in device buffer before transmission |
| Leaky bucket | Controlled-rate output shaping model |
| Token bucket | Average-rate budget with limited accumulated burst allowance |
| CIDR | Routing prefix notation such as `/24` |
| Subnet mask | IPv4 bit pattern marking prefix versus host bits |
| VLSM | Different subnet prefix sizes under an address allocation |
| MTU | Maximum packet size supported by a link at relevant layer |
| Fragment offset | IPv4 fragment position measured in eight-byte units |
| TTL / Hop Limit | Packet-lifetime limiting field at routers |
| ICMP | IP-related diagnostics and control-message protocol |
| Neighbor Discovery | IPv6 router/neighbour discovery and address resolution |
| SLAAC | IPv6 stateless address autoconfiguration |
| IPv6 extension header | Optional or specialised header after fixed base header |

***

# UNIT III - FINAL EXAM STRATEGY

**Learn the idea in Hinglish; write the technical answer in English.** For a 20-mark answer, use a clean structure: meaning and introduction; a labelled diagram; steps/working; at least one real-world or numerical example; a comparison table if suitable; advantages and limitations; and a brief conclusion. If you study only the 10 model answers, you may miss flexible subquestions; cover the complete theory and practise the calculations as well.

**Scope note:** The syllabus line for Unit III names point-to-point networks, routing, congestion control, internetworking/TCP-IP, IP packet, IP address and IPv6. The supporting algorithms, protocols and numericals in this guide are included to explain these subjects deeply and improve exam preparation; a university paper may emphasise a different selection or depth.
