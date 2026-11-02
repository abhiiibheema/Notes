Module 1: Network Core Operations — Forwarding vs. Routing

The Network Layer performs two distinct, complementary operations to transport data across an internetwork: **Forwarding** (Data Plane) and **Routing** (Control Plane).

```
                            ROUTER ARCHITECTURE
+-------------------------------------------------------------------------+
|                                                                         |
|  [Incoming Link]                                       [Outgoing Link]  |
|  ==============> +----------------------------------+ ================> |
|     (Frame 1)    |            DATA PLANE            |    (Frame 2)      |
|                  |        Forwarding Engine         |                   |
|                  | (Moves packet input -> output)   |                   |
|                  +----------------^-----------------+                   |
|                                   |                                     |
|                      Consults     | Updates                             |
|                   Forwarding Table| Table                               |
|                                   |                                     |
|                  +----------------v-----------------+                   |
|                  |          CONTROL PLANE           |                   |
|                  |          Routing Logic           |                   |
|                  |  (Executes Routing Algorithms)   |                   |
|                  +----------------------------------+                   |
|                                                                         |
+-------------------------------------------------------------------------+
```

1. Forwarding (Data Plane)

- **Definition**: The local action of transferring an arriving packet from a router's input physical interface to the appropriate output physical interface.
- **Plane**: Operates strictly within the **Data Plane** (hardware/line cards for microsecond packet switching).
- **Mechanism**: When an incoming frame arrives, the router inspects the destination IP address in the packet header, matches it against its local **Forwarding / Routing Table**, and switches it to the designated outgoing port.
- Question it answers: Which output port should this packet leave through?

1. Routing (Control Plane)

- **Definition**: The network-wide process that determines the end-to-end path taken by packets from the originating source host to the final destination host.
- **Plane**: Operates within the **Control Plane** (software/routing daemon).
- **Primary Objective**: Identifies the **optimal (least-cost/best) path** across the entire topology.
- **Key Function**: Dynamically constructs, manages, and updates the router's **Forwarding / Routing Table** by running routing algorithms and protocols.
- Question it answers: Which path should packets take to reach the destination?

3. End-to-End Transmission Pipeline

```
+-----------+     +-----------+     +---------+     +----+     +----+     +---------+     +-----------+     +-----------+
|  Source   | ==> |  Source   | ==> | Source  | ==> | R8 | ==> | R2 | ==> | Dest.   | ==> |   Dest.   | ==> |   Dest.   |
| Host (Hs) |     |Router (Rs)|     |   ISP   |     +----+     +----+     |   ISP   |     |Router (RD)|     | Host (HD) |
+-----------+     +-----------+     +---------+                           +---------+     +-----------+     +-----------+
```

4. Critical Device Differences (Router vs. NAT {Network Address Translation} (converts the 192.168.xx -> public) IP Device)

- **TTL Handling**: A standard router **always decrements TTL by 1** at each hop. A standalone NAT device **does not decrement TTL**; TTL is decremented exclusively by routing entities.
- **Header Modification**: NAT modifies the **Source IP** (private to public) and recomputes the **Header Checksum**. A NAT device does not perform fragmentation or modify the Total Length field (fragmentation is handled exclusively by routers).


---

Module 2: Link Cost Metrics & Proportionality Rules

Routing algorithms evaluate links by assigning numerical cost values based on physical and traffic parameters.

1. Primary Link Comparison Metrics

2. **Distance (Hop Count)**: The total number of intermediate routers along the path.
3. **Delay**: The aggregate transit latency (transmission + propagation + queuing + processing delays) across the link.
4. **Bandwidth**: The maximum transmission capacity (bottleneck bandwidth) of the link.

5. Mathematical Cost Relationships

Link cost is calculated using the following proportionality rules:

- **Cost vs. Distance**: $$\text{Cost} \propto \text{Distance (Hop Count)}$$
- **Cost vs. Delay**: $$\text{Cost} \propto \text{Delay}$$
- **Cost vs. Bandwidth**: $$\text{Cost} \propto \frac{1}{\text{Bandwidth}}$$
- Low bandwidth: A narrow road → fewer vehicles can pass → congestion is more likely.
- High bandwidth: A wide road → more vehicles can pass at once → better capacity.
  In routing, we can assign a lower cost to a link with higher bandwidth because it can carry more data.

---

Module 3: Classification of Routing Algorithms

```
                              ROUTING ALGORITHMS
                                      |
         +----------------------------+----------------------------+
         |                                                         |
   STATIC ROUTING                                            DYNAMIC ROUTING
   (Non-Adaptive)                                              (Adaptive)
         |                                                         |
   +-----+--------------------+                      +-------------+-------------+
   |                          |                      |                           |
SHORTEST PATH              FLOODING           DISTANCE VECTOR                LINK STATE
 ALGORITHM                    |                  ROUTING                      ROUTING
   |                 (Broadcasts packet             |                            |
   |                  out all ports              (Distributed)             (Centralized;
   |                  except incoming)                                    Dijkstra-based)
   +--------------------------+
   |                          |
SINGLE-SOURCE             ALL-PAIRS
SHORTEST PATH           SHORTEST PATH
   |                          |
+--+---------------+     (Floyd-Warshall
|                  |        Algorithm)
Dijkstra's    Bellman-Ford
Algorithm      Algorithm
```

1. Static Routing (Non-Adaptive Routing)

- **Definition**: Routes and forwarding tables are configured manually by network administrators. Tables remain fixed and do not adapt automatically to traffic shifts or topology failures.
- **Sub-types**:
    1. **Shortest Path Algorithms**:
        - **Single-Source Shortest Path**: Finds least-cost paths from one fixed source to all other nodes (**Dijkstra’s Algorithm**, **Bellman-Ford Algorithm**).
        - **All-Pairs Shortest Path**: Finds least-cost paths between all pairs of nodes (**Floyd-Warshall Algorithm**).
    2. **Flooding**:
        - **Mechanism**: Whenever a router receives a packet, it broadcasts a copy out through **every outgoing link except the link on which it arrived**.
        - **Major Drawback**: Generates an exponential explosion of duplicate packets, consuming massive bandwidth.
        - **Advantage**: High resilience; the packet that follows the shortest path is guaranteed to arrive first.

2. Dynamic Routing (Adaptive Routing)

- **Definition**: Routers run background routing protocols to automatically discover network topology and compute shortest paths.
- **Key Characteristics**: Self-learning; requires **no manual updates** when links go down, routers restart, or traffic loads change.
- **Categories**:
    - **Distance Vector Routing Algorithm** (Distributed computation via Bellman-Ford).
    - **Link State Routing Algorithm** (Centralized knowledge via Dijkstra's Algorithm).

---

Module 4: Link State Routing (LSR) Protocol

Link State Routing is classified as a **Centralized Routing Algorithm** because every individual router computes its routes using a **complete, identical view of the entire network topology**.

Reference Network Topology

```
                     (4)               (7)
                +----------- [ R2 ] -----------+
               /               |                \
              /                |                 \
             /                 | (8)              \
     (2)    /                  |                   \   (3)
[ Rs ] --- [ R1 ]              |                  [ R3 ] --- [ RD ]
            \                  |                   /
             \                 |                  /
              \                | (5)             /
               \               |                /
                +----------- [ R4 ] -----------+
                     (6)
```

_(Bidirectional edge weights represent link cost metrics)_

---

Module 5: Three Stages of Link State Routing

Link State Routing operates in **three sequential stages**:

```
+----------------------------------------------------------------------------------+
| Stage 1: Maintain Link State Information (One-Time / Initial)                    |
| Each router discovers its directly connected neighbors and records link costs.  |
+----------------------------------------+-----------------------------------------+
                                         |
                                         v
+----------------------------------------------------------------------------------+
| Stage 2: Link State Broadcast (Flooding LSP)                                     |
| Routers flood Link State Packets (LSP) to all other nodes.                       |
| Every router builds a complete Adjacency Matrix and runs Dijkstra locally.       |
+----------------------------------------+-----------------------------------------+
                                         |
                                         v
+----------------------------------------------------------------------------------+
| Stage 3: Share Updated Link State Information (Event-Driven / Asynchronous)      |
| Triggered ONLY on topology changes. Affected routers flood updates.              |
| Routers update their Adjacency Matrix and re-execute Dijkstra locally.           |
+----------------------------------------------------------------------------------+
```

---

Stage 1: Maintain Local Link State Information (Initial Phase)

- Each router establishes communication with its physical links and constructs a **Link State Record** containing information strictly about its **adjacent (neighbor) links only**.
- For the reference topology, the initial local state tables are:

```
+---------------------+     +---------------------+
| Rs Link State Info  |     | RD Link State Info  |
+----------+----------+     +----------+----------+
| Neighbor | Cost     |     | Neighbor | Cost     |
+----------+----------+     +----------+----------+
| R1       | 2        |     | R3       | 3        |
+----------+----------+     +----------+----------+

+---------------------+     +---------------------+
| R1 Link State Info  |     | R2 Link State Info  |
+----------+----------+     +----------+----------+
| Neighbor | Cost     |     | Neighbor | Cost     |
+----------+----------+     +----------+----------+
| Rs       | 2        |     | R1       | 4        |
| R2       | 4        |     | R3       | 7        |
| R4       | 6        |     | R4       | 8        |
+----------+----------+     +----------+----------+

+---------------------+     +---------------------+
| R3 Link State Info  |     | R4 Link State Info  |
+----------+----------+     +----------+----------+
| Neighbor | Cost     |     | Neighbor | Cost     |
+----------+----------+     +----------+----------+
| RD       | 3        |     | R1       | 6        |
| R2       | 7        |     | R2       | 8        |
| R4       | 5        |     | R3       | 5        |
+----------+----------+     +----------+----------+
```

---

Stage 2: Link State Broadcast & Local Routing Table Construction

1. **Flooding the Link State Packets (LSP)**:
    - Every router creates a Link State Packet containing its local table and floods (broadcasts) it to **all other routers in the network**.
    - At the end of this broadcast phase, **every router holds the complete link state information of every other router**, giving every node a global map of the network.
2. **Local Graph Construction**:
    - Each router translates these received LSPs into an internal graph representation: an **Adjacency Matrix** or **Adjacency List**.
3. **Local Route Computation via Dijkstra's Algorithm**:
    - Each router independently executes **Dijkstra’s Algorithm locally**, setting itself as the source root node.
    - Dijkstra's algorithm consists of:
        - **Step 1: Initialization** (preparing input data structures).
        - **Step 2: Iterative distance calculation** (relaxing edges to determine shortest paths).
    - The calculated paths populate the router's local **Routing Table**.

```
                       LOCAL DIJKSTRA PROCESSING AT ROUTER [Ri]
+------------------------------------+
| Inputs:                            |
| 1. Complete Graph Topology         |
|    (Adjacency Matrix / List)       | ====> +----------------------+ ====> +----------------------+
| 2. Source Node Coordinate [Ri]     |       | Dijkstra's Algorithm |       | Shortest Path to All |
+------------------------------------+       |  (Executed Locally)  |       |     Other Nodes      |
                                             +----------------------+       +----------+-----------+
                                                                                       |
                                                                                       v
                                                                            +----------------------+
                                                                            |     Router [Ri]      |
                                                                            |    Routing Table     |
                                                                            +----------------------+
```

---

Stage 3: Share Updated Link State Information (Maintenance Phase)

1. **Event-Driven Trigger**:
    - Updates are **not periodic**. When a link fails, recovers, or changes cost, only the routers directly incident to that link detect the change and update their local link state.
2. **Selective Broadcast**:
    - **Only the detecting routers** flood an updated Link State Packet to the rest of the network (**Asynchronous Update**).
3. **Local Recalculation**:
    - Upon receiving an updated LSP, every router modifies its internal Adjacency Matrix / List and **re-runs Dijkstra’s Algorithm locally** to update its Forwarding Table.

---

Module 6: Computational & Communication Complexity

1. Dijkstra’s Algorithm Time Complexity (Computation Overhead)

- Let $n$ be the total number of routers (nodes) in the network.
- The algorithm runs through $n$ iterations, performing comparisons across candidate nodes at each step.
- Total number of comparisons: $$\text{Time Complexity} = O(n^2)$$

2. Link State Routing Message Complexity (Communication Overhead)

- Let $n$ be the total number of routers (nodes) in the network.
- Each router generates an LSP that crosses $O(n)$ links during the network-wide flood.
- Since all $n$ routers must broadcast their link state to achieve global convergence: $$\text{Overall Message Complexity} = n \times O(n) = O(n^2)$$

																No Problems for this lecture.. 
																			-- xx --

---
																	Lecture - 2

Module 1: Foundations & Core Principles

**Distance Vector Routing (DVR)** is a dynamic, decentralized routing algorithm used at the Network Layer to compute the least-cost paths between all pairs of nodes in an internetwork.

```
                      DISTANCE VECTOR ROUTING ESSENCE
                      
      [ Router X ]  <======== Distance Vector ========>  [ Neighbor V ]
      (Maintains DV)          Exchange Only              (Maintains DV)
            |                                                  |
   Knows local link costs                             Knows local link costs
    + neighbor estimates                              + neighbor estimates
            \                                                /
             +-------------> [ Bellman-Ford ] <-------------+
                             Dynamic Programming
                                  Equation
```

1. Theoretical Foundations

- **Algorithmic Basis**: Implements the **Bellman-Ford Algorithm**, which is built on the paradigm of **Dynamic Programming**.
- **Mathematical Basis**: Operates entirely using the **Bellman-Ford Equation** to iteratively compute and update shortest paths.

2. Key Characteristics

- **Distributed (Decentralized)**:
    - Computation is distributed across all routers.
    - Each router’s Distance Vector is constructed **exclusively from information provided by its immediate neighbors**.
    - A router possesses **partial knowledge** of the network topology—it only knows its direct link costs and what its neighbors report. It does _not_ possess a global map of the whole network (unlike Link State Routing).
- **Iterative**:
    - Routers calculate, exchange, and recalculate paths in iterative rounds.
    - The algorithm iterates until **no new routing information is exchanged**, at which point the network reaches **convergence (stabilization)**.
- **Asynchronous**:
    - Nodes do not need to operate in lockstep; an update is triggered either periodically or whenever a local link metric changes.
- **Neighbor-Restricted Communication**:
    - A router sends its Distance Vector **exclusively to its directly connected neighbors**, never flooding it to the entire network.

---

Module 2: The Bellman-Ford Equation

The core calculation of Distance Vector Routing relies on expressing the shortest path from a source to a destination in terms of the direct paths to adjacent neighbors plus the neighbors' shortest paths to that destination.

```
                               THE BELLMAN-FORD FORMULA
                               
                         (Direct Cost)           (Neighbor Estimate)
                            C(X, V1)                  DV1(Y)
                        +-------------> [ V1 ] -------------------+
                       /                                           \
                      /     C(X, V2)                  DV2(Y)        \
     [ Source X ] ---+----------------> [ V2 ] ----------------------+---> [ Dest Y ]
                      \                                             /
                       \    C(X, V3)                  DV3(Y)       /
                        +-------------> [ V3 ] -------------------+
```

1. Mathematical Formulation

Let $D_X(Y)$ be the cost of the least-cost path from source node $X$ to destination node $Y$:

$$D_X(Y) = \min_{V} \Big\{ C(X, V) + D_V(Y) \Big\}$$

Where:

- $X$: The source router computing the path.
- $Y$: The target destination router.
- $V$: Any directly connected neighbor of $X$. The minimum is taken across **all direct neighbors** $V$ **of** $X$.
- $C(X, V)$: The direct physical link cost from $X$ to neighbor $V$.
- $D_V(Y)$: Neighbor $V$’s estimated least-cost path cost from itself to destination $Y$.

2. Expanded Multi-Neighbor Evaluation

If node $X$ has three physical neighbors $V_1, V_2, V_3$ with direct link costs $a_1, a_2, a_3$ and reported path estimates $b_1, b_2, b_3$ to destination $Y$:

$$D_X(Y) = \min \Big[ \big(a_1 + b_1\big), \; \big(a_2 + b_2\big), \; \big(a_3 + b_3\big) \Big]$$

The neighbor $V_i$ that yields this minimum cost becomes the **Next Hop ("Via")** interface in $X$'s routing table for destination $Y$.

---

Module 3: Routing Table Architecture & Distance Vectors

Each router maintains an internal table that maps destinations to metrics and forwarding interfaces.

```
                         ROUTER TABLE STRUCTURE
                         
                  +-------------+------+---------+
                  | Destination | Cost | NextHop |
                  |    Node     |      |  (Via)  |
                  +-------------+------+---------+
                  |      P      |  5   |    T    |
                  |      Q      |  6   |    T    |
                  |      R      |  0   |    R    |
                  |      S      |  10  |    T    |
                  |      T      |  2   |    T    |
                  +-------------+------+---------+
                                   |
                   Extracted as Distance Vector:
                   [ P:5,  Q:6,  R:0,  S:10,  T:2 ]
                   (Shared ONLY with neighbors)
```

1. Internal Routing Table Components

- **Destination Node**: The target router in the network.
- **Cost (Distance Metric)**: The current best-known minimum aggregate cost from the local router to that destination.
- **Via (Next Hop)**: The immediate adjacent neighbor to which outgoing packets destined for that node must be forwarded.

2. The Distance Vector

- The **Distance Vector** is a 1-dimensional array extracted from the routing table containing only the **(Destination, Cost)** pairs.
- **Privacy Rule**: A router shares its _costs_ with its neighbors, but it **does not share the Next Hop ("Via") field**. Next Hop is purely local forwarding state.

---

Module 4: Protocol Lifecycle — Initialization to Convergence

The protocol progresses through distinct phases: local discovery, vector exchange, iterative Bellman-Ford relaxation, and final convergence.

```
+-------------------------------------------------------------------------------+
| Phase 1: Local Initialization                                                 |
| - Distance to self is set to 0 (Via self).                                    |
| - Distance to direct neighbors = physical link cost (Via neighbor).           |
| - Distance to non-neighbors = Infinity (not yet reachable).                   |
+---------------------------------------+---------------------------------------+
                                        |
                                        v
+-------------------------------------------------------------------------------+
| Phase 2: Neighbor Distance Vector Exchange                                    |
| - Every router transmits its Distance Vector to its direct neighbors only.     |
+---------------------------------------+---------------------------------------+
                                        |
                                        v
+-------------------------------------------------------------------------------+
| Phase 3: Bellman-Ford Table Update (Relaxation)                               |
| - Router recalculates: New Cost = C(Local, Neighbor) + Reported_Cost           |
| - If New Cost < Current Cost: Update Cost and set Next Hop to that neighbor.   |
+---------------------------------------+---------------------------------------+
                                        |
                                        v
+-------------------------------------------------------------------------------+
| Phase 4: Convergence (Steady State)                                           |
| - Routers exchange vectors. No metrics change across any table in the network.|
| - Iteration stops; shortest paths are fully established.                      |
+-------------------------------------------------------------------------------+
```

---

Module 5: Algorithmic Mechanics via Network Models

The following structural network models demonstrate how routing information propagates across hops and how alternative paths are discovered.

---

Model 1: Hop-by-Hop Propagation in a Linear Network

Consider four routers arranged in a linear topology:

```
        (3)                 (2)                 (4)
[ A ] ------- [ B ] ------------------- [ C ] ------- [ D ]
```

1. Initial State (Direct Neighbors Known)

Each router knows the cost to itself and to its immediate physical neighbors:

```
+---+------+-----+   +---+------+-----+   +---+------+-----+   +---+------+-----+
|    Node A      |   |    Node B      |   |    Node C      |   |    Node D      |
+---+------+-----+   +---+------+-----+   +---+------+-----+   +---+------+-----+
| A |  0   |  A  |   | A |  3   |  A  |   | B |  2   |  B  |   | C |  4   |  C  |
| B |  3   |  B  |   | B |  0   |  B  |   | C |  0   |  C  |   | D |  0   |  D  |
+---+------+-----+   | C |  2   |  C  |   | D |  4   |  D  |   +---+------+-----+
                     +---+------+-----+   +---+------+-----+
```

2. First Round of Updates

- **Vectors Exchanged**:
    - $A \longleftrightarrow B$
    - $B \longleftrightarrow C$
    - $C \longleftrightarrow D$
- **Table Updates**:
    - Router $A$ receives $B$'s vector and learns a path to $C$: $$D_A(C) = C(A, B) + D_B(C) = 3 + 2 = 5 \quad (\text{Via } B)$$
    - Router $B$ receives $C$'s vector and learns a path to $D$: $$D_B(D) = C(B, C) + D_C(D) = 2 + 4 = 6 \quad (\text{Via } C)$$
    - Router $C$ receives $B$'s vector and learns a path to $A$: $$D_C(A) = C(C, B) + D_B(A) = 2 + 3 = 5 \quad (\text{Via } B)$$
    - Router $D$ receives $C$'s vector and learns a path to $B$: $$D_D(B) = C(D, C) + D_C(B) = 4 + 2 = 6 \quad (\text{Via } C)$$

```
              TABLES AFTER ROUND 1 (2-Hop Knowledge Established)
+---+------+-----+   +---+------+-----+   +---+------+-----+   +---+------+-----+
|    Node A      |   |    Node B      |   |    Node C      |   |    Node D      |
+---+------+-----+   +---+------+-----+   +---+------+-----+   +---+------+-----+
| A |  0   |  A  |   | A |  3   |  A  |   | A |  5   |  B  |   | B |  6   |  C  |
| B |  3   |  B  |   | B |  0   |  B  |   | B |  2   |  B  |   | C |  4   |  C  |
| C |  5   |  B  |   | C |  2   |  C  |   | C |  0   |  C  |   | D |  0   |  D  |
+---+------+-----+   | D |  6   |  C  |   | D |  4   |  D  |   +---+------+-----+
                     +---+------+-----+   +---+------+-----+
```

3. Second Round of Updates

- Information propagates an additional hop:
    - Router $A$ learns about $D$ through $B$: $$D_A(D) = C(A, B) + D_B(D) = 3 + 6 = 9 \quad (\text{Via } B)$$
    - Router $D$ learns about $A$ through $C$: $$D_D(A) = C(D, C) + D_C(A) = 4 + 5 = 9 \quad (\text{Via } C)$$
- Nodes $B$ and $C$ experience no changes.

```
              TABLES AFTER ROUND 2 (3-Hop Knowledge Established)
+---+------+-----+   +---+------+-----+   +---+------+-----+   +---+------+-----+
|    Node A      |   |    Node B      |   |    Node C      |   |    Node D      |
+---+------+-----+   +---+------+-----+   +---+------+-----+   +---+------+-----+
| A |  0   |  A  |   | A |  3   |  A  |   | A |  5   |  B  |   | A |  9   |  C  |
| B |  3   |  B  |   | B |  0   |  B  |   | B |  2   |  B  |   | B |  6   |  C  |
| C |  5   |  B  |   | C |  2   |  C  |   | C |  0   |  C  |   | C |  4   |  C  |
| D |  9   |  B  |   | D |  6   |  C  |   | D |  4   |  D  |   | D |  0   |  D  |
+---+------+-----+   +---+------+-----+   +---+------+-----+   +---+------+-----+
```

4. Third Round (Convergence Verification)

- Vectors are exchanged again.
- **No router discovers a lower-cost path or a new destination**.
- Tables remain identical $\implies$ **Network has converged**.

---

Model 2: Alternative Path Optimization in a Triangle Topology

This topology demonstrates how an indirect multi-hop path replaces a direct physical link when its aggregate metric is lower.

```
                      (3)
             [ A ] --------- [ B ]
               \               /
                \             /
             (4) \           / (9)
                  \         /
                   \       /
                    [ C ]
```

1. Initial### Comprehensive Class Notes: Distance Vector Routing

---

Module 1: Foundations & Core Principles

**Distance Vector Routing (DVR)** is a dynamic, decentralized routing algorithm used at the Network Layer to calculate the least-cost paths between all pairs of nodes in an internetwork.

```
                      DISTANCE VECTOR ROUTING ARCHITECTURE
                      
      [ Router X ]  <======== Distance Vector ========>  [ Neighbor V ]
      (Maintains DV)          Exchange Only              (Maintains DV)
            |                                                  |
   Knows local link costs                             Knows local link costs
    + neighbor estimates                              + neighbor estimates
            \                                                /
             +-------------> [ Bellman-Ford ] <-------------+
                             Dynamic Programming
                                  Equation
```

1. Theoretical Foundations

- **Algorithmic Basis**: Implements the **Bellman-Ford Algorithm**, which is built on the paradigm of **Dynamic Programming**.
- **Mathematical Basis**: Operates entirely using the **Bellman-Ford Equation** to iteratively compute and update shortest paths.

2. Key Characteristics

- **Distributed (Decentralized)**:
    - Route computation is distributed across all routers in the network.
    - Each router’s Distance Vector is constructed **exclusively from information provided by its immediate physical neighbors**.
    - A router possesses only **partial knowledge** of the network topology—it knows its direct link costs and what its neighbors report, but it does _not_ possess a global topology map.
- **Iterative**:
    - Routers calculate, exchange, and recalculate paths in successive rounds.
    - The process repeats until **no new routing information is available to be exchanged**, at which point the network reaches **convergence (stabilization)**.
- **Asynchronous**:
    - Routers do not need to operate in lockstep; updates are triggered either periodically or whenever a local link metric changes.
- **Neighbor-Restricted Communication**:
    - A router transmits its Distance Vector **exclusively to its directly connected neighbors**, never broadcasting or flooding across the entire network.

---

Module 2: The Bellman-Ford Equation & Path Computation

Distance Vector Routing calculates the shortest path from a source to a destination by combining the direct cost to an adjacent neighbor with that neighbor's estimated shortest path to the destination.

```
                               THE BELLMAN-FORD FORMULA
                               
                         (Direct Cost)           (Neighbor Estimate)
                            C(X, V1)                  Dv1(Y)
                        +-------------> [ V1 ] -------------------+
                       /                                           \
                      /     C(X, V2)                  Dv2(Y)        \
     [ Source X ] ---+----------------> [ V2 ] ----------------------+---> [ Dest Y ]
                      \                                             /
                       \    C(X, V3)                  Dv3(Y)       /
                        +-------------> [ V3 ] -------------------+
```

1. Mathematical Formulation

Let $D_X(Y)$ be the cost of the least-cost path from source node $X$ to destination node $Y$:

$$D_X(Y) = \min_{V} \Big\{ C(X, V) + D_V(Y) \Big\}$$

Where:

- $X$: The source router computing the path.
- $Y$: The target destination router.
- $V$: Any directly connected physical neighbor of $X$. The minimum is taken over **all direct neighbors** $V$ **of** $X$.
- $C(X, V)$: The direct physical link cost from $X$ to neighbor $V$.
- $D_V(Y)$: Neighbor $V$’s estimated least-cost path cost from itself to destination $Y$.

2. Multi-Neighbor Expansion

If router $X$ has three physical neighbors $V_1, V_2, V_3$ with direct link costs $a_1, a_2, a_3$ and reported path estimates $b_1, b_2, b_3$ to destination $Y$:

$$D_X(Y) = \min \Big[ (a_1 + b_1), \; (a_2 + b_2), \; (a_3 + b_3) \Big]$$

- **Next Hop Selection**: The neighbor $V_i$ that provides the minimum sum becomes the **Next Hop ("Via")** interface in $X$'s routing table for packets traveling to destination $Y$.

---

Module 3: Routing Table Architecture vs. Distance Vectors

Each router maintains an internal table that maps destinations to metrics and forwarding interfaces.

```
                         ROUTER TABLE STRUCTURE
                         
                  +-------------+------+---------+
                  | Destination | Cost | NextHop |
                  |    Node     |      |  (Via)  |
                  +-------------+------+---------+
                  |      P      |  5   |    T    |
                  |      Q      |  6   |    T    |
                  |      R      |  0   |    R    |
                  |      S      |  10  |    T    |
                  |      T      |  2   |    T    |
                  +-------------+------+---------+
                                   |
                   Extracted as Distance Vector:
                   [ P:5,  Q:6,  R:0,  S:10,  T:2 ]
                   (Shared ONLY with neighbors)
```

1. Internal Routing Table Fields

- **Destination Node**: The identifier of the remote target router.
- **Cost (Distance Metric)**: The current best-known minimum aggregate cost from the local router to that destination.
- **Via (Next Hop)**: The immediate adjacent neighbor to which packets for that destination must be forwarded.

2. The Distance Vector Format

- The **Distance Vector** is a 1-dimensional array extracted from the routing table containing only the **(Destination, Cost)** pairs.
- **Information Boundary**: A router advertises its path _costs_ to its neighbors, but it **does not advertise the Next Hop ("Via") field**. The Next Hop is kept private as local forwarding state.

---

Module 4: Protocol Lifecycle — Initialization to Convergence

The protocol progresses through distinct phases: local discovery, vector exchange, iterative Bellman-Ford relaxation, and final convergence.

```
+-------------------------------------------------------------------------------+
| Phase 1: Local Initialization                                                 |
| - Distance to self is set to 0 (Via self).                                    |
| - Distance to direct neighbors = physical link cost (Via neighbor).           |
| - Distance to non-neighbors = Infinity (not yet reachable).                   |
+---------------------------------------+---------------------------------------+
                                        |
                                        v
+-------------------------------------------------------------------------------+
| Phase 2: Neighbor Distance Vector Exchange                                    |
| - Every router transmits its Distance Vector to its direct neighbors only.     |
+---------------------------------------+---------------------------------------+
                                        |
                                        v
+-------------------------------------------------------------------------------+
| Phase 3: Bellman-Ford Table Update (Relaxation)                               |
| - Router recalculates: New Cost = C(Local, Neighbor) + Reported_Cost           |
| - If New Cost < Current Cost: Update Cost and set Next Hop to that neighbor.   |
+---------------------------------------+---------------------------------------+
                                        |
                                        v
+-------------------------------------------------------------------------------+
| Phase 4: Convergence (Steady State)                                           |
| - Routers exchange vectors. No metrics change across any table in the network.|
| - Iteration stops; shortest paths are fully established.                      |
+-------------------------------------------------------------------------------+
```

---

Module 5: Topology Propagation Models

The following structural network models demonstrate how routing information propagates hop-by-hop and how lower-cost alternative routes are discovered.

---

Model 1: Hop-by-Hop Distance Learning (Linear Topology)

Consider four routers connected in a straight line:

```
        (3)                 (2)                 (4)
[ A ] ------- [ B ] ------------------- [ C ] ------- [ D ]
```

1. Initialization State

Each node only knows its direct physical neighbors:

- **Router A**: Cost to $A = 0$, Cost to $B = 3$ (Via $B$).
- **Router B**: Cost to $B = 0$, Cost to $A = 3$ (Via $A$), Cost to $C = 2$ (Via $C$).
- **Router C**: Cost to $C = 0$, Cost to $B = 2$ (Via $B$), Cost to $D = 4$ (Via $D$).
- **Router D**: Cost to $D = 0$, Cost to $C = 4$ (Via $C$).

2. Round 1 of Updates (2-Hop Knowledge)

Routers exchange vectors with direct neighbors ($A \leftrightarrow B$, $B \leftrightarrow C$, $C \leftrightarrow D$):

- **Node A** receives $B$'s vector and discovers $C$: $$D_A(C) = C(A, B) + D_B(C) = 3 + 2 = 5 \quad (\text{Via } B)$$
- **Node B** receives $C$'s vector and discovers $D$: $$D_B(D) = C(B, C) + D_C(D) = 2 + 4 = 6 \quad (\text{Via } C)$$
- **Node C** receives $B$'s vector and discovers $A$: $$D_C(A) = C(C, B) + D_B(A) = 2 + 3 = 5 \quad (\text{Via } B)$$
- **Node D** receives $C$'s vector and discovers $B$: $$D_D(B) = C(D, C) + D_C(B) = 4 + 2 = 6 \quad (\text{Via } C)$$

3. Round 2 of Updates (3-Hop Knowledge)

Information propagates an additional hop across the network:

- **Node A** receives updated vector from $B$ and discovers $D$: $$D_A(D) = C(A, B) + D_B(D) = 3 + 6 = 9 \quad (\text{Via } B)$$
- **Node D** receives updated vector from $C$ and discovers $A$: $$D_D(A) = C(D, C) + D_C(A) = 4 + 5 = 9 \quad (\text{Via } C)$$
- **Nodes B and C** experience no metric changes.

4. Round 3 (Convergence)

Vectors are exchanged once more. No node discovers a lower-cost path or a new destination. The tables remain unchanged, and the network stabilizes.

---

Model 2: Path Optimization over Inefficient Direct Links (Triangle Topology)

This model illustrates how Distance Vector Routing replaces an expensive direct link with a cheaper multi-hop path.

```
                      (3)
             [ A ] --------- [ B ]
               \               /
                \             /
             (4) \           / (9)
                  \         /
                   \       /
                    [ C ]
```

1. Initialization State

- **Router B** has direct links to both $A$ and $C$:
    - Cost to $A = 3$ (Via $A$)
    - Cost to $C = 9$ (Via $C$)
- **Router C** has direct links to $A$ and $B$:
    - Cost to $A = 4$ (Via $A$)
    - Cost to $B = 9$ (Via $B$)

2. Round 1 Optimization

During the first update round:

- **Router B** receives the Distance Vector from neighbor $A$, which advertises $D_A(C) = 4$.
- Router $B$ runs the Bellman-Ford equation for destination $C$: $$D_B(C) = \min \Big[ C(B, C) + D_C(C), \; C(B, A) + D_A(C) \Big]$$ $$D_B(C) = \min \Big[ 9 + 0, \; 3 + 4 \Big] = \min$$12$$ = 7$$
- **Table Update at B**: Router $B$ updates its path to $C$:
    - **Cost drops from 9 to 7**.
    - **Next Hop changes from C to A**.
- **Table Update at C**: Symmetrically, $C$ discovers the path to $B$ through $A$: $$D_C(B) = \min \Big[ 9, \; 4 + 3 \Big] = 7 \quad (\text{Via } A)$$

The direct link between $B$ and $C$ (cost 9) is preserved in physical topology, but routing traffic dynamically diverts through $A$ because the aggregate cost is lower.

---

Model 3: Shortcut Optimization over Multi-Hop Paths

This model demonstrates how Distance Vector Routing resolves a choice between a direct link and an alternative path that offers a lower aggregate metric.

```
        +----------------------- (7) -----------------------+
        |                                                   |
        |             (2)                 (2)               |
      [ A ] ------- [ B ] ------- [ C ] ------- [ D ] ------+
```

1. Initial State

- **Router A** connects to $B$ (cost 2) and $D$ (cost 7 via direct link).
    - In $A$'s initial table: Destination $D$ has **Cost = 7 (Via D)**.
- **Router D** connects to $C$ (cost 2) and $A$ (cost 7 via direct link).
    - In $D$'s initial table: Destination $A$ has **Cost = 7 (Via A)**.

2. Propagation and Path Replacement

- **After Round 1**:
    - Node $B$ learns the path to $D$ via $C$: $D_B(D) = 2 + 2 = 4$.
    - Node $C$ learns the path to $A$ via $B$: $D_C(A) = 2 + 2 = 4$.
- **After Round 2**:
    - Node $A$ receives $B$'s vector advertising $D_B(D) = 4$.
    - Node $A$ recalculates the least-cost path to $D$: $$D_A(D) = \min \Big[ C(A, D) + D_D(D), \; C(A, B) + D_B(D) \Big] = \min [7 + 0, \; 2 + 4] = \min$$23$$ = 6$$
    - **Table Update at A**: Cost to destination $D$ is updated from **7 to 6**, and Next Hop shifts from **D to B**.
    - Symmetrically, Node $D$ updates its cost to destination $A$ from **7 to 6**, with Next Hop shifting from **A to C**.

The multi-hop path through $B$ and $C$ supersedes the direct physical link because $2 + 2 + 2 = 6 < 7$.

---

Module 6: Comparative Protocol Architecture

| Parameter              | Distance Vector Routing (DVR)                               | Link State Routing (LSR)                                 |
| ---------------------- | ----------------------------------------------------------- | -------------------------------------------------------- |
| **Algorithmic Basis**  | **Bellman-Ford Algorithm** (Dynamic Programming)            | **Dijkstra's Algorithm** (Greedy approach)               |
| **Computation Style**  | **Decentralized / Distributed**                             | **Centralized**                                          |
| **Topology Knowledge** | **Partial knowledge** (Local links + neighbor estimates)    | **Complete global knowledge** (Full network graph)       |
| **Information Shared** | **Entire routing table / Distance Vector**                  | **Direct link states only (LSP)**                        |
| **Sharing Scope**      | Transmitted **exclusively to direct neighbors**             | **Flooded to all routers** across the network            |
| **Update Trigger**     | **Iterative & Asynchronous** (Periodic or on metric change) | **Event-driven** (Triggered strictly on topology change) |

---
#### Problems from Lecture - 2
---

Problem Statement

Consider a network of five routers **P, Q, R, S, and T**. Routers **P** and **T** are the direct physical neighbors of router **R** with direct link costs of **6** and **2**, respectively.

```
               (6)
       [ P ] ------- [ R ]
                       |
                       | (2)
                       |
                     [ T ]
```

The distance vectors received by **R** from its neighbors **P** and **T** are:

| Destination   | P   | Q   | R   | S   | T   |
| ------------- | --- | --- | --- | --- | --- |
| Vector from P | 0   | 1   | 5   | 5   | 3   |
| Vector from T | 3   | 4   | 2   | 8   | 0   |

Calculate the updated **Routing Table** and **Distance Vector** of router **R**.

---

Step-by-Step Solution

Applying the **Bellman-Ford equation** at router **R**:

$$D_R(Y) = \min \Big[ C(R, P) + D_P(Y), \; C(R, T) + D_T(Y) \Big]$$

Where:

- Direct link cost to **P**: $C(R, P) = 6$
- Direct link cost to **T**: $C(R, T) = 2$
- **Path to Destination P**:
    - Via P: $C(R, P) + D_P(P) = 6 + 0 = 6$
    - Via T: $C(R, T) + D_T(P) = 2 + 3 = 5$
    - Metric: $\min(6, 5) = \mathbf{5}$ $\implies$ **Next Hop: T**
- **Path to Destination Q**:
    - Via P: $C(R, P) + D_P(Q) = 6 + 1 = 7$
    - Via T: $C(R, T) + D_T(Q) = 2 + 4 = 6$
    - Metric: $\min(7, 6) = \mathbf{6}$ $\implies$ **Next Hop: T**
- **Path to Destination R** (Self):
    - Distance to self is inherently $\mathbf{0}$ $\implies$ **Next Hop: R**
- **Path to Destination S**:
    - Via P: $C(R, P) + D_P(S) = 6 + 5 = 11$
    - Via T: $C(R, T) + D_T(S) = 2 + 8 = 10$
    - Metric: $\min(11, 10) = \mathbf{10}$ $\implies$ **Next Hop: T**
- **Path to Destination T**:
    - Via P: $C(R, P) + D_P(T) = 6 + 3 = 9$
    - Via T: $C(R, T) + D_T(T) = 2 + 0 = 2$
    - Metric: $\min(9, 2) = \mathbf{2}$ $\implies$ **Next Hop: T**

Final Output

| Destination | Cost (Metric) | Next Hop (Via) |
| ----------- | ------------- | -------------- |
| P           | 5             | T              |
| Q           | 6             | T              |
| R           | 0             | R              |
| S           | 10            | T              |
| T           | 2             | T              |

- **Distance Vector of R**: `[ P: 5, Q: 6, R: 0, S: 10, T: 2 ]`

---

Problem 2: Delay-Based Distance Vector Update at Node F

Problem Statement

For the network shown below, node **F** has estimated its link delays to its four physical neighbors **A, E, D, and G** as **8 ms, 10 ms, 12 ms, and 6 ms**, respectively.
![[Pasted image 20261010202941.png|521]]

Node **F** receives the following delay vectors from its neighbors:
### Table of A

|Destination|Cost|
|---|---|
|A|0|
|B|40|
|C|14|
|D|17|
|E|21|
|F|9|
|G|24|

### Table of D

|Destination|Cost|
|---|---|
|A|20|
|B|8|
|C|30|
|D|0|
|E|14|
|F|7|
|G|22|

### Table of E

|Destination|Cost|
|---|---|
|A|24|
|B|27|
|C|7|
|D|20|
|E|0|
|F|11|
|G|22|

### Table of G

| Destination | Cost |
| ----------- | ---- |
| A           | 21   |
| B           | 24   |
| C           | 22   |
| D           | 19   |
| E           | 22   |
| F           | 10   |
|  G          | 0    |

Using the distance vector routing algorithm, determine the updated delay values in the routing table of **F**.

- **(A)** `A: 8, B: 20, C: 17, D: 12, E: 10, F: 0, G: 6`
- **(B)** `A: 21, B: 8, C: 7, D: 19, E: 14, F: 0, G: 22`
- **(C)** `A: 8, B: 20, C: 17, D: 12, E: 10, F: 16, G: 6`
- **(D)** `A: 8, B: 8, C: 7, D: 12, E: 10, F: 0, G: 6`

---

Step-by-Step Solution

Direct link metrics: $$C(F, A) = 8, \quad C(F, D) = 12, \quad C(F, E) = 10, \quad C(F, G) = 6$$

Applying the Bellman-Ford evaluation across all 4 neighbors: $$D_F(Y) = \min \Big[ 8 + D_A(Y), \; 12 + D_D(Y), \; 10 + D_E(Y), \; 6 + D_G(Y) \Big]$$

1. **Destination A**: $$\min \big[ 8+0, \; 12+20, \; 10+24, \; 6+21 \big] = \min [8, 32, 34, 27] = \mathbf{8} \quad (\text{Via A})$$
2. **Destination B**: $$\min \big[ 8+40, \; 12+8, \; 10+27, \; 6+24 \big] = \min [48, 20, 37, 30] = \mathbf{20} \quad (\text{Via D})$$
3. **Destination C**: $$\min \big[ 8+14, \; 12+30, \; 10+7, \; 6+22 \big] = \min [22, 42, 17, 28] = \mathbf{17} \quad (\text{Via E})$$
4. **Destination D**: $$\min \big[ 8+17, \; 12+0, \; 10+20, \; 6+19 \big] = \min [25, 12, 30, 25] = \mathbf{12} \quad (\text{Via D})$$
5. **Destination E**: $$\min \big[ 8+21, \; 12+14, \; 10+0, \; 6+22 \big] = \min [29, 26, 10, 28] = \mathbf{10} \quad (\text{Via E})$$
6. **Destination F** (Self): $$D_F(F) = \mathbf{0}$$
7. **Destination G**: $$\min \big[ 8+24, \; 12+22, \; 10+22, \; 6+0 \big] = \min [32, 34, 32, 6] = \mathbf{6} \quad (\text{Via G})$$
8. **Correct Answer**: **(A)**

---

Problem 3: Partial Topology Updates & Next-Hop Verification

Problem Statement

A network layer uses the distance vector routing algorithm. The partial topology of the network is shown below:

```
                         [ R ]
                       /   |   \
                     /     |     \
                   /       |       \
                [ X ]    [ Y ]    [ Z ]
                  :        :        :
                  :        :        :
                 ...      [ Q ]    [ P ]
```

Router **R** has three physical neighbors: **X, Y, and Z**. During one iteration, **R** measures its direct distances to these neighbors as **3, 2, and 5**, respectively.

Router **R** receives the following vectors from its neighbors:

- **Reported distances to destination P**: From $X = 7$, from $Y = 6$, from $Z = 5$.
- **Reported distances to destination Q**: From $X = 4$, from $Y = 6$, from $Z = 8$.

Which of the following statements is/are **correct** regarding the updated routing table of **R**?

- **(A)** The distance from R to P will be stored as 10
- **(B)** The distance from R to Q will be stored as 7
- **(C)** The next hop router for a packet from R to P is Y
- **(D)** The next hop router for a packet from R to Q is Z

---

Step-by-Step Solution

Direct link costs from R: $$C(R, X) = 3, \quad C(R, Y) = 2, \quad C(R, Z) = 5$$

1. Evaluation for Destination P:

- **Via X**: $C(R, X) + D_X(P) = 3 + 7 = 10$
- **Via Y**: $C(R, Y) + D_Y(P) = 2 + 6 = 8$
- **Via Z**: $C(R, Z) + D_Z(P) = 5 + 5 = 10$

$$D_R(P) = \min(10, \mathbf{8}, 10) = \mathbf{8} \implies \text{Next Hop: Y}$$

- Statement (A) claims distance is 10 $\implies$ **Incorrect** (distance is 8).
- Statement (C) claims next hop is Y $\implies$ **Correct**.

2. Evaluation for Destination Q:

- **Via X**: $C(R, X) + D_X(Q) = 3 + 4 = 7$
- **Via Y**: $C(R, Y) + D_Y(Q) = 2 + 6 = 8$
- **Via Z**: $C(R, Z) + D_Z(Q) = 5 + 8 = 13$

$$D_R(Q) = \min(\mathbf{7}, 8, 13) = \mathbf{7} \implies \text{Next Hop: X}$$

- Statement (B) claims distance is 7 $\implies$ **Correct**.
- Statement (D) claims next hop is Z $\implies$ **Incorrect** (next hop is X).
- **Correct Answer**: **(B) and (C)**

---

Problem 4: Determination of Unused Physical Links

Problem Statement

Consider a network of 6 routers **R1 to R6** with bidirectional link weights as shown:

![[Pasted image 20261010204638.png|535]]

- Link $(R_1, R_2) = 6$
- Link $(R_1, R_3) = 3$
- Link $(R_2, R_3) = 2$
- Link $(R_2, R_4) = 7$
- Link $(R_3, R_5) = 9$
- Link $(R_4, R_5) = 1$
- Link $(R_4, R_6) = 8$
- Link $(R_5, R_6) = 4$

All routers execute the distance vector routing algorithm. Each router initializes its table with the weight of its directly connected links. After all routing tables stabilize, **how many links in the network will never be used for carrying data**?

- **(A)** 1
- **(B)** 2
- **(C)** 3
- **(D)** 4

---

Step-by-Step Solution

Governing Rule:

In any shortest-path routing algorithm, a direct link between two nodes $u$ and $v$ with physical cost $C(u, v)$ is **never used** if there exists an alternative path between $u$ and $v$ whose total aggregate cost is **strictly less than** $C(u, v)$.

If an alternative path is strictly cheaper, no pair of source and destination nodes will ever route traffic across the direct link $(u, v)$.

---

Systematic Link-by-Link Audit:

1. **Examine Link** $(R_1, R_2)$ **with Cost** $= 6$:
    - Direct cost: $6$
    - Alternative path via $R_3$: $R_1 \to R_3 \to R_2$ has cost: $$C(R_1, R_3) + C(R_3, R_2) = 3 + 2 = 5$$
    - Because $5 < 6$, traffic between $R_1$ and $R_2$ (and any path traversing this region) will always prefer the route via $R_3$.
    - $\implies$ **Link** $(R_1, R_2)$ **is NEVER used.**
2. **Examine Link** $(R_4, R_6)$ **with Cost** $= 8$:
    - Direct cost: $8$
    - Alternative path via $R_5$: $R_4 \to R_5 \to R_6$ has cost: $$C(R_4, R_5) + C(R_5, R_6) = 1 + 4 = 5$$
    - Because $5 < 8$, traffic will always route through $R_5$ rather than across the direct link.
    - $\implies$ **Link** $(R_4, R_6)$ **is NEVER used.**
3. **Check the Remaining 6 Links**:
    - **Link** $(R_1, R_3) = 3$: The alternative route via $R_2$ costs $6 + 2 = 8 > 3$. **Used.**
    - **Link** $(R_2, R_3) = 2$: The alternative route via $R_1$ costs $6 + 3 = 9 > 2$. **Used.**
    - **Link** $(R_4, R_5) = 1$: The shortest physical link in the network. **Used.**
    - **Link** $(R_5, R_6) = 4$: The alternative route via $R_4$ costs $1 + 8 = 9 > 4$. **Used.**
    - **Link** $(R_2, R_4) = 7$:
        - Direct cost: $7$
        - Alternative path through $(R_3, R_5)$: $R_2 \to R_3 \to R_5 \to R_4$ has cost $2 + 9 + 1 = 12 > 7$. **Used.**
    - **Link** $(R_3, R_5) = 9$:
        - Direct cost: $9$
        - Alternative path through $(R_2, R_4)$: $R_3 \to R_2 \to R_4 \to R_5$ has cost $2 + 7 + 1 = 10 > 9$. **Used.**
4. **Total Unused Links**: Exactly **2 links** ($(R_1, R_2)$ and $(R_4, R_6)$).
5. **Correct Answer**: **(B)**
---
# Lecture - 3
---
Comprehensive Class Notes: Distance Vector Routing — Dynamics, Count-to-Infinity & Solutions

Module 1: Update Paradigms in Distance Vector Routing

Distance Vector Routing (DVR) algorithms operate under two distinct execution and timing paradigms: **Asynchronous** and **Synchronous**.

```
                   UPDATE PARADIGMS IN DISTANCE VECTOR ROUTING
                   
         +----------------------------------+----------------------------------+
         |                                                                     |
         v                                                                     v
  ASYNCHRONOUS UPDATE                                                   SYNCHRONOUS UPDATE
  (Event-Driven Model)                                                  (Periodic Round Model)
  - Waits for local events:                                             - Periodic "Round of Update":
    * Change in adjacent link cost                                        * Every router transmits DV to
    * Receipt of new DV from neighbor                                       all neighbors at fixed intervals.
  - Recalculates via Bellman-Ford.                                      - Intermediate Triggered Updates:
  - If own DV changes -> Notify neighbors.                                * Routers may notify neighbors
  - If own DV unchanged -> DO NOTHING.                                      between rounds if a local metric
  - No notification received -> No action.                                  undergoes a change.
```

1. The Asynchronous Update Model (Event-Driven)

- **Trigger Conditions**: A router waits continuously for either of two events:
    1. A change in the cost of an adjacent, directly connected physical link.
    2. The arrival of an updated Distance Vector estimate from any directly connected neighbor.
- **Algorithmic Rule**:
    - Upon detecting an event, the router updates its own Distance Vector estimates using the **Bellman-Ford equation**.
    - **Conditional Transmission**:
        - **If its own Distance Vector changes** $\implies$ Transmit the updated vector to all direct neighbors.
        - **Else (no change in local vector)** $\implies$ **Do nothing** (no packets sent).
    - **Neighbor Cascade**: Neighbors receiving this update follow the exact same rule: they notify their own neighbors only if their local vectors are altered.
    - If no notification is received from a neighbor, no action is taken.

2. The Synchronous Update Model (Periodic Round-Based)

- **Periodic Transmission**: Every router transmits its Distance Vector to all directly connected neighbors during scheduled intervals called the **"Round of Update" phase**.
- **Triggered Updates Between Rounds**: If a local link cost changes between two periodic rounds, an affected router is permitted to immediately send a triggered update to its neighbors without waiting for the next scheduled round.

---

Module 2: The Core Routing Asymmetry

A fundamental behavioral characteristic of Distance Vector Routing is: $$\textbf{"Good News Travels Fast, Bad News Travels Slowly"}$$

- **Good News (Metric Decreases / New Links)**: Propagates across the network rapidly in very few rounds, achieving rapid convergence.
- **Bad News (Metric Increases / Link Failures)**: Propagates sluggishly through multiple iterative rounds, frequently creating transient routing loops and severe metric inflation.

---

Module 3: Link Cost Increase Dynamics & The Ping-Pong Escalation

When the cost of a link increases, routers may exchange mutually dependent metric estimates back and forth, causing a temporary ping-pong effect before stabilizing.

Initial Converged State

Consider three routers connected in a straight line with initial unit link costs:

```
             (1)                 (1)
    [ A ] --------- [ B ] --------- [ C ]
```

```
+-------------------+     +-------------------+     +-------------------+
|      Node A       |     |      Node B       |     |      Node C       |
+------+------+-----+     +------+------+-----+     +------+------+-----+
| Dest | Cost | Via |     | Dest | Cost | Via |     | Dest | Cost | Via |
+------+------+-----+     +------+------+-----+     +------+------+-----+
|  A   |  0   |  A  |     |  A   |  1   |  A  |     |  A   |  2   |  B  |
|  B   |  1   |  B  |     |  B   |  0   |  B  |     |  B   |  1   |  B  |
|  C   |  2   |  B  |     |  C   |  1   |  C  |     |  C   |  0   |  C  |
+------+------+-----+     +------+------+-----+     +------+------+-----+
```

---

The Cost Increase Event

The physical cost of link **A–B** increases from **1 to 6**:

```
             (6)                 (1)
    [ A ] --------- [ B ] --------- [ C ]
       (Old cost = 1)
```

Immediately upon detecting the physical change, **B** updates its direct link cost to **A** to **6**. The progression toward convergence depends on message timing:

---

Case I: Routers A and B Transmit First (Asynchronous)

1. Router **B** updates its internal cost to **A** as **6 (Via A)**.
2. Router **B** immediately transmits its updated vector `[A: 6]` to **C**.
3. Router **C** receives `[A: 6]` from **B**.
4. Since **C**'s path to **A** routes via **B**, **C** recalculates: $$D_C(A) = C(C, B) + D_B(A) = 1 + 6 = 7 \quad (\text{Via } B)$$
5. Router **C** updates its table entry to **Cost = 7 (Via B)** and transmits `[A: 7]` back to **B**.
6. Router **B** evaluates the Bellman-Ford equation: $$D_B(A) = \min \Big[ C(B, A) + D_A(A), \; C(B, C) + D_C(A) \Big] = \min [6 + 0, \; 1 + 7 = 8] = 6 \quad (\text{Via } A)$$
7. Router **B**’s vector does not change $\implies$ **Convergence is achieved cleanly**.

---

Case II: All Routers Transmit Simultaneously in Update Rounds (Synchronous)

If updates occur in synchronized rounds where all nodes exchange their vectors at the same instant:

```
                              ROUND-BY-ROUND TRACE
                              
Initial State: Direct link A-B is updated to 6.
B knows direct link = 6, but C still advertises old cost (A = 2).

Round 1:
  - C advertises old vector [A: 2] to B.
  - B recalculates: D_B(A) = min [ Direct(6), Cost(B, C) + D_C(A) = 1 + 2 = 3 ]
  - B incorrectly updates: Cost(A) = 3, Next Hop = C! (Routing loop formed)
  
Round 2:
  - B advertises updated vector [A: 3] to C.
  - C recalculates: D_C(A) = Cost(C, B) + D_B(A) = 1 + 3 = 4 (Via B).
  - C updates: Cost(A) = 4, Next Hop = B.
  
Round 3:
  - C advertises updated vector [A: 4] to B.
  - B recalculates: D_B(A) = min [ Direct(6), 1 + 4 = 5 ] = 5 (Via C).
  - B updates: Cost(A) = 5, Next Hop = C.
  
Round 4:
  - B advertises updated vector [A: 5] to C.
  - C recalculates: D_C(A) = Cost(C, B) + D_B(A) = 1 + 5 = 6 (Via B).
  - C updates: Cost(A) = 6, Next Hop = B.
  
Round 5:
  - C advertises updated vector [A: 6] to B.
  - B recalculates: D_B(A) = min [ Direct(6), 1 + 6 = 7 ]
  - The direct physical link (6) is now CHEAPER than the path via C (7)!
  - B updates: Cost(A) = 6, Next Hop = A (Switches back to direct link).
  
Round 6 (Convergence):
  - B advertises [A: 6] to C.
  - C recalculates: D_C(A) = 1 + 6 = 7 (Via B).
  - Both tables stabilize permanently with B at 6 (Via A) and C at 7 (Via B).
```

---

Case III: Router C Transmits First

If router **C** sends its periodic or asynchronous vector before processing any change from **B**:

1. **C** sends its stale vector `[A: 2]` to **B**.
2. **B** computes: $$D_B(A) = \min [6, \; 1 + 2 = 3] = 3 \quad (\text{Via } C)$$
3. **B** updates its Next Hop for **A** to **C** with cost **3**.
4. The exact same step-by-step ping-pong escalation from Case II begins, stepping up the metric iteratively until **B**'s cost exceeds the physical link cost of **6**, at which point the tables stabilize.

---

Module 4: Link Failure & The Count-to-Infinity Problem

When a physical link breaks completely (cost $= \infty$), the metric escalation is not bounded by a finite physical link cost. The two nodes increment their values without bound, producing the **Count-to-Infinity** problem.

```
                 (X)                 (1)
        [ A ] ---X--- [ B ] --------- [ C ]
             Link FAILS
             Cost = Infinity
```

The Routing Loop Mechanism

1. The link between **A** and **B** breaks. Router **B** updates its direct link cost to **A** to $\infty$.
2. If router **C** advertises its vector before receiving an update from **B**, **C** sends `[A: 2]`.
3. Router **B** receives `[A: 2]` and calculates: $$D_B(A) = \min \Big[ C(B, A), \; C(B, C) + D_C(A) \Big] = \min \Big[ \infty, \; 1 + 2 = 3 \Big] = 3 \quad (\text{Via } C)$$
4. **B** concludes that **C** has an alternate path to **A** of cost 2, unaware that **C**'s route actually runs back through **B** itself.
5. A **two-node routing loop** is created: **B** forwards traffic for **A** to **C**, and **C** forwards traffic for **A** to **B**.

---

Transmission Timing Scenarios on Link Failure

```
+-----------------------------------------------------------------------------------------+
| SCENARIO 1: Router B Transmits First                                                    |
| 1. B detects link failure and sets Cost(A) = Infinity.                                  |
| 2. B transmits [A: Infinity] to C.                                                      |
| 3. C receives update and recalculates: D_C(A) = 1 + Infinity = Infinity.                |
| 4. Both routers stabilize immediately at Infinity without looping!                      |
+-----------------------------------------------------------------------------------------+

+-----------------------------------------------------------------------------------------+
| SCENARIO 2: Routers B and C Transmit Simultaneously (Update Round)                      |
| 1. B sends [A: Infinity] to C; simultaneously, C sends [A: 2] to B.                     |
| 2. B processes [A: 2] -> Updates Cost(A) = 1 + 2 = 3 (Via C).                          |
| 3. C processes [A: Infinity] -> Sets Cost(A) = Infinity temporarily.                   |
| 4. In the next round: B sends [A: 3] to C -> C updates Cost(A) = 1 + 3 = 4 (Via B).     |
| 5. C sends [A: 4] to B -> B updates Cost(A) = 1 + 4 = 5 (Via C).                        |
| 6. The metrics count upward endlessly toward Infinity!                                  |
+-----------------------------------------------------------------------------------------+

+-----------------------------------------------------------------------------------------+
| SCENARIO 3: Router C Transmits First                                                    |
| 1. C sends stale vector [A: 2] to B before B has advertised the link break.             |
| 2. B recalculates: D_B(A) = min [Infinity, 1 + 2] = 3 (Via C).                          |
| 3. B sends [A: 3] to C -> C recalculates: D_C(A) = 1 + 3 = 4 (Via B).                   |
| 4. C sends [A: 4] to B -> B recalculates: D_B(A) = 1 + 4 = 5 (Via C).                   |
| 5. Continuous ping-pong loop: Cost(A) increments: 3 -> 4 -> 5 -> 6 -> ... -> Infinity.  |
+-----------------------------------------------------------------------------------------+
```

- **Definition**: The **Count-to-Infinity Problem** (also called the _Link-to-Infinity Problem_) occurs in Distance Vector Routing when a two-node (or multi-node) routing loop causes routers to repeatedly increment path metrics toward infinity following a link failure or severe cost degradation.

---

Module 5: Architectural Solutions to Count-to-Infinity

To eliminate two-node routing loops, Distance Vector Routing introduces three architectural solutions:

```
                     SOLUTIONS TO COUNT-TO-INFINITY
                                   |
         +-------------------------+-------------------------+
         |                         |                         |
         v                         v                         v
    SOLUTION 1                SOLUTION 2                SOLUTION 3
  Advertise Path            Split Horizon             Poison Reverse
(Include Next Hop)        (Suppression Rule)        (Poisoned Advertising)
- Include "Next Hop"      - Suppress routes         - Advertise metric as
  in vector exchange.       back to the Next          INFINITY to the
                            Hop neighbor.             Next Hop neighbor.
```

---

Solution 1: Explicit Path / Next Hop Advertisement

- **Mechanism**: The Distance Vector advertisement format is extended to include the **Next Hop ("Via")** alongside the destination and cost.
- **Loop Prevention**:
    - Router **C** sends its advertisement to **B** as: \text{Destination: } A, ; \text{Cost: } 2, ; \text{Next Hop: }
    - When **B** inspects this vector, it observes that **C**'s path to **A** routes through **B itself**.
    - Recognizing the circular dependency, **B** rejects the advertised route and refuses to route to **A** via **C**.

```
+-----------------------------------------+
| Router C Advertisement sent to B:       |
| Destination | Cost | Next Hop           |
+-------------+------+--------------------+
|      A      |  2   |        B           |  ===> B inspects: "C's next hop is ME!"
+-----------------------------------------+       B rejects route -> Loop prevented.
```

---

Solution 2: The Split Horizon Hack

- **Fundamental Rule**: **Do not advertise a destination to a neighbor if that neighbor is already the Next Hop for that destination's routing table entry**.

```
                 SPLIT HORIZON SUPPRESSION MECHANISM
                 
         [ A ] --------- [ B ] <========= [ C ]
                               (Transmits DV)
                               
   Router C Routing Table:
   +-------------+------+----------+
   | Destination | Cost | Next Hop |
   +-------------+------+----------+
   |      A      |  2   |    B     |  <-- Next Hop is B!
   |      B      |  1   |    B     |
   |      C      |  0   |    C     |
   +-------------+------+----------+
   
   Rule Applied:
   Because B is the Next Hop for reaching A and B, Router C SUPPRESSES 
   entries A and B when sending its Distance Vector to Router B!
   
   Distance Vector transmitted from C to B:
   +-------------+------+
   | Destination | Cost |
   +-------------+------+
   |      C      |  0   |   <-- Entries for A and B are COMPLETELY OMITTED!
   +-------------+------+
```

- **Loop Prevention**:
    - Router **C** never advertises a route for **A** back to **B**.
    - If link **A–B** fails, router **B** never receives a false path to **A** through **C**, preventing the loop from forming.

---

Solution 3: Poison Reverse (Split Horizon with Poisoned Reverse)

- **Fundamental Rule**: **Advertise the distance to a destination as INFINITY (**$\infty$**) to a neighbor if that neighbor is the Next Hop for that destination's routing table entry**.

```
                 POISON REVERSE ADVERTISING MECHANISM
                 
         [ A ] --------- [ B ] <========= [ C ]
                               (Transmits DV)
                               
   Router C Routing Table:
   +-------------+------+----------+
   | Destination | Cost | Next Hop |
   +-------------+------+----------+
   |      A      |  2   |    B     |  <-- Next Hop is B!
   |      B      |  1   |    B     |
   |      C      |  0   |    C     |
   +-------------+------+----------+
   
   Rule Applied:
   Because B is the Next Hop for reaching A and B, Router C sets 
   Cost = Infinity for A and B specifically when advertising to B!
   
   Distance Vector transmitted from C to B:
   +-------------+----------+
   | Destination |   Cost   |
   +-------------+----------+
   |      A      | Infinity |  <-- Poisoned specifically for B!
   |      B      | Infinity |  <-- Poisoned specifically for B!
   |      C      |    0     |
   +-------------+----------+
```

Fast Convergence on Link Failure with Poison Reverse:

When the link **A–B** fails (Cost $= \infty$):

1. Router **B** sets its internal table entry for **A** to $\infty$.
2. Under Poison Reverse, router **C**'s advertisement to **B** explicitly advertises **Cost(A) =** $\infty$.
3. Router **B** evaluates the Bellman-Ford equation: $$D_B(A) = \min \Big[ C(B, A), \; C(B, C) + D_C(A) \Big] = \min \Big[ \infty, \; 1 + \infty \Big] = \infty$$
4. Router **B** immediately concludes that **A** is unreachable and retains **Cost(A) =** $\infty$.
5. Both routers stabilize at **Cost(A) =** $\infty$ in a single update step, completely bypassing the Count-to-Infinity condition.

---

Module 6: Summary of Distance Vector Operating Modes

Distance Vector Routing is implemented in two standard modes:

```
+-----------------------------------------------------------------------------------------+
|                  DISTANCE VECTOR ROUTING OPERATIONAL MODES                              |
+----------------------------------+------------------------------------------------------+
| Configuration Mode               | Operational Characteristics & Loop Behavior          |
+----------------------------------+------------------------------------------------------+
| 1. Standard Distance Vector      | - Default specification.                             |
|    (Without Loop Protection)     | - Vulnerable to transient routing loops and the      |
|                                  |   Count-to-Infinity problem during link failures.    |
+----------------------------------+------------------------------------------------------+
| 2. Distance Vector with          | - Enhanced modern implementation.                    |
|    Split Horizon / Poison Reverse| - Completely eliminates two-node routing loops.      |
|                                  | - Immediately reflects link failures across the      |
|                                  |   network without metric escalation.                 |
+----------------------------------+------------------------------------------------------+
```
