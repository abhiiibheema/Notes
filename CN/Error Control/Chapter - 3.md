Master Notes: Internet Routing Protocols (RIP, OSPF, BGP) & Hierarchical Architecture (Batch 3)

---

Module 1: Routing Protocol Hierarchy (IGP vs. EGP) & Autonomous Systems

On the Internet, routing protocols are categorized based on administrative boundaries known as **Autonomous Systems (AS)**. An Autonomous System is a collection of routers under a single administrative control (e.g., an ISP or a corporate network).

```
                        INTERNET ROUTING HIERARCHY
                                     |
         +---------------------------+---------------------------+
         |                                                       |
   INTRA-DOMAIN ROUTING (IGP)                              INTER-DOMAIN ROUTING (EGP)
   - Operates WITHIN an Autonomous System                  - Operates BETWEEN Autonomous Systems
   - Focus: Performance & Least-Cost Path                  - Focus: Policy Routing & Reachability
   - Examples: RIP, OSPF                                   - Example: BGP
```

1. Intra-Domain Routing (Interior Gateway Protocol - IGP)

- Used to route packets **within a single Autonomous System** (inside an ISP or enterprise domain).
- Focuses on optimizing interior path performance and link metrics.
- **Primary Examples**: **RIP** (Routing Information Protocol) and **OSPF** (Open Shortest Path First).

2. Inter-Domain Routing (Exterior Gateway Protocol - EGP)

- Used to route packets **between different Autonomous Systems** across the global Internet.
- Focuses on enforcing policy constraints, business agreements, and reachability.
- **Primary Example**: **BGP** (Border Gateway Protocol).

---

Module 2: Internet Routing Protocols (RIP, OSPF, BGP)

1. Routing Information Protocol (RIP)

- **Algorithm Type**: Classic Distance Vector Routing (implements the Bellman-Ford Algorithm).
- **Metric**: **Hop Count** (the number of router hops along the path).
    - Each link has a fixed cost of `1`.
    - The maximum metric limit is **15 hops** (a hop count of `16` represents infinity / unreachable destination).
- **Update Mechanism**: Periodic/synchronous updates exchanged with immediate neighbors every **30 seconds**.
- **Transport Encapsulation**: RIP messages are encapsulated inside **UDP segments (Port 520)**, which are then placed into IP datagrams: $$\text{IP Datagram} \left[ \text{UDP Segment (Port 520)} \left[ \text{RIP Message} \right] \right]$$
- **Network Structure**: Creates a flat network structure without regions or areas (not suitable for large hierarchical networks).

2. Open Shortest Path First (OSPF)

- **Algorithm Type**: Classic Link State Routing (implements Dijkstra's Algorithm).
- **Metric**: **Bandwidth / Cost** (link cost is inversely proportional to link bandwidth).
- **Network View**: Each router maintains a complete, global picture of the network topology.
- **Update Mechanism**: Asynchronous / event-driven updates flooded across the network when link states change.
- **Transport Encapsulation**: OSPF messages are encapsulated **directly into IP datagrams** using IP Protocol Number **89**, bypassing transport protocols like TCP/UDP entirely: $$\text{IP Datagram (Protocol 89)} \left[ \text{OSPF Message} \right]$$
- **Network Structure**: Fully supports **hierarchical routing** by partitioning large networks into structured areas and regions.

3. Border Gateway Protocol (BGP)

- **Algorithm Type**: **Path Vector Routing** (an extension of distance vector that includes full path sequences to prevent loops).
- **Domain Scope**: Inter-Domain Routing Protocol (EGP) connecting Autonomous Systems across the global Internet.
- **Routing Basis**: Policy-driven constraints, AS path sequences, and administrative rules.
- **Transport Encapsulation**: BGP messages are encapsulated into **TCP segments (Port 179)** to ensure reliable, connection-oriented delivery: $$\text{IP Datagram} \left[ \text{TCP Segment (Port 179)} \left[ \text{BGP Message} \right] \right]$$
- **Network Structure**: Fully supports hierarchical routing across global network boundaries.

---

Module 3: Protocol Transport & Feature Summary Matrix

```
+----------------------------------------------------------------------------------------------------------+
|                               INTERNET ROUTING PROTOCOLS COMPARISON TABLE                                |
+------------------+---------------------------+-----------------------+-------------------+---------------+
| Protocol         | Routing Type              | Metric                | Transport Layer   | Port / Proto  |
+------------------+---------------------------+-----------------------+-------------------+---------------+
| RIP              | Distance Vector (IGP)     | Hop Count (Max 15)    | UDP               | UDP Port 520  |
| OSPF             | Link State (IGP)          | Bandwidth / Cost      | Direct IP Payload | IP Proto 89   |
| BGP              | Path Vector (EGP)         | Policy / AS-Path      | TCP               | TCP Port 179  |
+------------------+---------------------------+-----------------------+-------------------+---------------+
```

---

Module 4: Hierarchical Routing & Source Routing

1. Hierarchical Routing

- **Concept**: A large network is divided into **regions and sub-regions** (such as OSPF Areas or Autonomous Systems).
- **Primary Benefit**: Enables **Route Aggregation / Summarization**, which drastically reduces routing table sizes and confines link-state flooding within local area boundaries.
- **Protocol Suitability**:
    - **OSPF and BGP**: Highly suitable for hierarchical routing.
    - **RIP**: Not suitable for hierarchical routing (forces a flat structure).

2. Source Routing

- **Concept**: The transmitting host (source) determines the partial or complete path that a packet must follow before sending it.
- **Path Addressing**: The full designated path is explicitly carried inside the header of every transmitted packet, rather than relying strictly on step-by-step router decisions.

---

Module 5: Distance Vector (DV) vs. Link State (LS) Head-to-Head Comparison

```
+----------------------------------------------------------------------------------------------------------+
|                               DISTANCE VECTOR VS. LINK STATE COMPARISON                                  |
+-------------------+------------------------------------------+-------------------------------------------+
| Characteristic    | Distance Vector Routing (e.g., RIP)      | Link State Routing (e.g., OSPF)           |
+-------------------+------------------------------------------+-------------------------------------------+
| Network View      | Local knowledge (neighbors only)         | Complete global network topology map      |
| Algorithm Used    | Bellman-Ford (Dynamic Programming)       | Dijkstra's Shortest Path Algorithm        |
| Resource Overhead | Low CPU and Memory usage                 | High CPU and Memory usage                 |
| Message Volume    | Fewer network messages per update        | Higher message complexity (O(n²))         |
| Convergence Speed | Slow convergence (Count-to-Infinity)     | Fast convergence after topology changes   |
| Routing Loops     | Susceptible to loops & Count-to-Infinity | Immune to persistent routing loops        |
| Optimal Network   | Best suited for small networks           | Ideal for large, enterprise-level networks|
+-------------------+------------------------------------------+-------------------------------------------+
```

---

Module 6: Step-by-Step Solved Questions & Exam Problems

Problem 1: Interior Gateway Protocols Classification

- **Question**: Which one of the following is TRUE about interior gateway routing protocols - Routing Information Protocol (RIP) and Open Shortest Path First (OSPF)?
    - (A) RIP uses distance vector routing and OSPF uses link state routing
    - (B) OSPF uses distance vector routing and RIP uses link state routing
    - (C) Both RIP and OSPF use link state routing
    - (D) Both RIP and OSPF use distance vector routing
- **Step-by-Step Solution**:
    1. RIP implements the Bellman-Ford algorithm, making it a **Distance Vector** routing protocol.
    2. OSPF implements Dijkstra's algorithm, making it a **Link State** routing protocol.
    3. Therefore, Option A is correct.
- **Answer**: **Option A**.

---

Problem 2: Transport Layer Protocol Association

- **Question**: Consider the following statements about IPv4 routing protocols:
    - **I.** RIP uses distance vector routing.
    - **II.** RIP packets are sent using UDP.
    - **III.** OSPF packets are sent using TCP.
    - **IV.** OSPF operation is based on link-state routing.
    - Which of the above statements are CORRECT?
        - (A) I and IV only
        - (B) I, II, and III only
        - (C) I, II, and IV only
        - (D) II, III, and IV only
- **Step-by-Step Solution**:
    1. **Statement I**: True. RIP is a Distance Vector routing protocol.
    2. **Statement II**: True. RIP messages are encapsulated in UDP segments on Port 520.
    3. **Statement III**: False. OSPF does **not** use TCP; OSPF packets are encapsulated directly into IP datagrams using Protocol 89.
    4. **Statement IV**: True. OSPF is based on Link State routing.
- **Answer**: **Option C (I, II, and IV only)**.

---

Problem 3: OSPF Protocol Properties (MSQ)

- **Question**: Which of the following statements is/are INCORRECT about the OSPF (Open Shortest Path First) routing protocol used in the Internet?
    - (A) OSPF implements Bellman-Ford algorithm to find shortest paths.
    - (B) OSPF uses Dijkstra's shortest path algorithm to implement least-cost path routing.
    - (C) OSPF is used as an inter-domain routing protocol.
    - (D) OSPF implements hierarchical routing.
- **Step-by-Step Solution**:
    1. **Statement A**: Incorrect. OSPF uses Dijkstra's algorithm, not Bellman-Ford. (Selected as an incorrect statement).
    2. **Statement B**: Correct. OSPF uses Dijkstra's algorithm for least-cost paths.
    3. **Statement C**: Incorrect. OSPF is an **intra-domain** (IGP) routing protocol, not an inter-domain protocol. (Selected as an incorrect statement).
    4. **Statement D**: Correct. OSPF supports hierarchical routing using areas.
- **Answer**: **Options A and C**.

---

Problem 4: Distance Vector vs. Link State Comparisons

- **Question**: Two popular routing algorithms are Distance Vector (DV) and Link State (LS) routing. Which of the following statements are TRUE?
    - **S1**: Count to infinity is a problem only with DV and not LS routing.
    - **S2**: In LS, the shortest path algorithm is run only at one node.
    - **S3**: In DV, the shortest path algorithm is run only at one node.
    - **S4**: DV requires a lesser number of network messages than LS.
        - (A) S1, S2, and S4 only
        - (B) S1, S3, and S4 only
        - (C) S2 and S3 only
        - (D) S1 and S4 only
- **Step-by-Step Solution**:
    1. **S1**: True. The Count-to-Infinity problem is caused by routing loops inherent to Distance Vector algorithms.
    2. **S2**: False. In Link State routing, **every node** independently runs Dijkstra's algorithm.
    3. **S3**: False. In Distance Vector routing, **every node** repeatedly updates its own vector using Bellman-Ford.
    4. **S4**: True. DV exchanges messages only with direct neighbors, generating fewer network messages than global LS flooding.
- **Answer**: **Option D (S1 and S4 only)**.

---

Problem 5: Protocol Behavior in Large Networks

- **Question**: Consider the following three statements about Link State and Distance Vector routing protocols for a large network with 500 network nodes and 4000 links:
    - **S1**: The computational overhead in link state protocols is higher than in distance vector protocols.
    - **S2**: A distance vector protocol (with split horizon) avoids persistent routing loops, but not a link state protocol.
    - **S3**: After a topology change, a link state protocol will converge faster than a distance vector protocol.
    - Which one of the following is correct about S1, S2, and S3?
        - (A) S1, S2, and S3 are all true
        - (B) S1, S2, and S3 are all false
        - (C) S1 and S2 are true, but S3 is false
        - (D) S1 and S3 are true, but S2 is false
- **Step-by-Step Solution**:
    1. **S1**: True. Link State runs Dijkstra on full topology maps, creating higher CPU and memory overhead.
    2. **S2**: False. Split horizon avoids 2-node loops in DV but cannot prevent loops of 3 or more nodes. Link State protocols are inherently immune to persistent loops because every node maintains a complete global topology map.
    3. **S3**: True. Link State floods updates immediately and recomputes paths locally, converging much faster than iterative DV vector exchanges.
- **Answer**: **Option D (S1 and S3 are true, but S2 is false)**.

keep_pinSave to notekeep_pin

copy

thumb_up

thumb_down

keyboard_arrow_down

tune