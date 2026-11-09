Master Notes: Routing Fundamentals, Data/Control Plane & Link State Routing

---

Module 1: Data Plane vs. Control Plane (Forwarding vs. Routing)

In a network router, packet handling is partitioned into two distinct functional planes: the **Data Plane** and the **Control Plane**.

```
+-----------------------------------------------------------------------------------+
|                            ROUTER FUNCTIONAL PLANES                               |
+-------------------+------------------------------------+--------------------------+
| Plane             | Core Function                      | Scope & Time Scale       |
+-------------------+------------------------------------+--------------------------+
| Data Plane        | Forwarding: Moves packets from an  | Local per-router action  |
| (Hardware Level)  | input link to an output link [3].  | (Nanoseconds/Microsec)   |
+-------------------+------------------------------------+--------------------------+
| Control Plane     | Routing: Determines the end-to-end | Global network-wide      |
| (Software Level)  | path taken by IP packets [2].      | (Milliseconds/Seconds)   |
+-------------------+------------------------------------+--------------------------+
```

```
                          DATA PLANE vs. CONTROL PLANE
                          
    CONTROL PLANE (Software Logic):
    Executes Routing Algorithms/Protocols ----> Constructs Routing Table [4]
                                                       |
                                                       v
    DATA PLANE (Switching Fabric):
    Incoming Packet on Input Link --------> Forwarding Lookup --------> Output Link [3]
```

Key Router Forwarding Rules

- **Header Modifications**: During forwarding, a router modifies specific header fields in the IP packet (it decrements the **Time-To-Live [TTL]** value and recalculates the **Header Checksum**).
- **Routing Protocol Requirement**: Running a dynamic routing protocol is **not mandatory** for a router; routers can operate purely on manually configured static routes.
- **Fragmentation Reassembly Rule**: Routers **never reassemble** incoming IP fragments mid-path, even if an outgoing link has a larger MTU than the incoming packet size3. Reassembly occurs strictly at the final destination host.

---

Module 2: Static Routing vs. Dynamic Routing

Routing methods are classified into **Static Routing** and **Dynamic Routing** based on how routing tables are constructed and updated4.

```
                                ROUTING STRATEGIES
                                        |
         +------------------------------+------------------------------+
         |                                                             |
   STATIC ROUTING [6]                                           DYNAMIC ROUTING [6, 7]
   - Fixed, predetermined paths [8]                             - Routes adapt automatically to network
   - Manual administrator configuration                         - Topology & traffic changes [7]
   - Low router CPU/memory overhead                             - Uses dynamic routing algorithms [4]
   - Types: Shortest Path, Flooding [8]                         - Types: Link State, Distance Vector [9]
```

1. Static Routing

- Routes are manually configured or calculated prior to packet transmission.
- Does not automatically adapt to link failures or topology changes.
- **Types of Static Routing**:
    1. **Shortest Path Algorithm**: Paths calculated in advance using fixed link metrics (hop count, delay, or bandwidth).
    2. **Flooding**: An incoming packet is replicated and forwarded over every outgoing link except the interface on which it arrived8. The packet following the shortest path arrives first.

2. Dynamic Routing

- Routers run dynamic algorithms to automatically discover network topology and update their forwarding tables.
- Self-learning and adaptive to link failures, congestion, and structural changes.
- **Types of Dynamic Routing**:
    1. **Link State Routing Algorithm** (Centralized / Global topology view).
    2. **Distance Vector Routing Algorithm** (Distributed / Local neighbor view).

---

Module 3: Link State Routing (LSR) & Dijkstra's Algorithm

Link State Routing is a **centralized dynamic routing algorithm** where every router builds a complete, global view of the entire network graph1215.

```
                     LINK STATE ROUTING (3 OPERATIONAL STEPS)
                     
   Step 1: Maintain Local Link State
   Each router discovers adjacent link costs to its immediate neighbors [17].
                                 |
                                 v
   Step 2: Link State Broadcast (Flooding)
   Each router floods its link state packet to ALL other routers in the network [16].
   ===> Every router receives full topology info (Adjacency Matrix / List) [16, 18].
                                 |
                                 v
   Step 3: Local Execution of Dijkstra's Algorithm
   Each router runs Dijkstra's algorithm locally using itself as the source node [18, 19].
   ===> Computes least-cost tree & populates local Routing Table [18, 19].
```

1. Three Operational Steps of Link State Routing

2. **Maintain Link State Information**:
    - Each router measures and records the cost of its directly connected (adjacent) links to immediate neighbor nodes.
3. **Link State Broadcast (Flooding)**:
    - Every router floods (broadcasts) its local link state packet across the network to all other routers15.
    - Through network-wide flooding, **every router builds an identical Adjacency Matrix or Adjacency List** representing the entire network topology.
4. **Execute Dijkstra's Algorithm Locally**:
    - Each router independently executes **Dijkstra's Shortest Path Algorithm** using itself as the source node.
    - Calculates the shortest path tree to all destination nodes and fills its local routing table.

5. Message Complexity & Updates

- **Overall Message Complexity**: In a network with $n$ nodes where each broadcast packet traverses $O(n)$ links, the total message complexity for global flooding is $O(n^2)$.
- **Asynchronous Updates**: When a link cost changes or a link fails, only the affected routers broadcast updated link state packets. Upon receipt, each router updates its local Adjacency Matrix/List and re-executes Dijkstra's algorithm.

3. Inputs & Outputs of Dijkstra's Algorithm

```
   [ Graph: Adjacency Matrix / List ] --+
                                        |---> [ Dijkstra's Algorithm ] ---> Shortest Paths ---> [ Routing Table ] [19]
   [ Source Router (Ri) ] --------------+        (Calculates least-cost       (Populates destination
                                                 paths to all nodes)         next-hop entries)
```

- **Inputs**:
    1. The complete network graph (Adjacency Matrix or Adjacency List).
    2. The identity of the **Source Router (Ri)** running the algorithm.
- **Outputs**:
    1. The shortest/least-cost path tree from the source router to all other network nodes.
    2. Populated **Routing Table** containing destination network addresses and next-hop interfaces.

---

Module 4: Step-by-Step Solved Questions & Exam Problems

Problem 1: Algorithm Classification Matching

- **Question**: Match the routing protocol type in List I with the corresponding algorithm in List II:
    - **List I**: (i) Distance vector routing, (ii) Link state routing
    - **List II**: (a) Bellman-Ford, (b) Dijkstra
- **Step-by-Step Solution**:
    1. **Distance Vector Routing** uses the **Bellman-Ford algorithm** (Dynamic Programming) to iteratively compute paths.
        - Matches: **(i) — (a)**
    2. **Link State Routing** uses **Dijkstra's algorithm** to compute shortest paths from a complete topology map.
        - Matches: **(ii) — (b)**
- **Answer**: **(i) - (a) and (ii) - (b)**.

---

Problem 2: Router Functionality Verification

- **Question**: Evaluate the following statements regarding IP-based router functionality:
    - **Statement I**: A router does not modify IP packets during forwarding.
    - **Statement II**: It is not necessary for a router to implement any routing protocol.
    - **Statement III**: A router should reassemble IP fragments if the MTU of the outgoing link is larger than the size of the incoming IP packet.
- **Step-by-Step Solution**:
    1. **Evaluate Statement I**: False. A router **does modify** IP headers during forwarding by decrementing the TTL field and updating the header checksum.
    2. **Evaluate Statement II**: True. A router can forward packets using manually configured static routing tables without running dynamic routing protocols.
    3. **Evaluate Statement III**: False. Routers **never reassemble** IP fragments mid-transit; reassembly is handled exclusively by the destination host.
- **Answer**: **Statement II only**.

---

Problem 3: Header Field Modifications in Transit

- **Question**: Host A sends an IP datagram D to host B through an intermediate router R. Assuming no transmission errors, which header fields of datagram D may change when it reaches host B?
    - (i) TTL
    - (ii) Checksum
    - (iii) Fragment offset
- **Step-by-Step Solution**:
    1. **TTL**: Decremented by 1 at the intermediate router (`TTL = TTL - 1`), so it changes3.
    2. **Checksum**: Recalculated at the router due to the modified TTL field, so it changes3.
    3. **Fragment Offset**: If the packet exceeds an outgoing link MTU and undergoes fragmentation, the fragment offset changes.
- **Answer**: **(i), (ii), and (iii)**.