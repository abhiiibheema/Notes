Master Notes: Distance Vector Routing & Bellman-Ford Mechanics

---

Module 1: Distance Vector Routing Architecture & Bellman-Ford Equation

Distance Vector Routing (DVR) is a **distributed, iterative, asynchronous dynamic routing algorithm** based on the **Bellman-Ford algorithm** (Dynamic Programming).

```
                          DISTANCE VECTOR ROUTING LOGIC
                          
  Local Neighbor Knowledge Only                     Iterative Vector Exchange
 +-------------------------------+                +---------------------------+
 | Each router knows ONLY costs  |                | Routers send their full   |
 | to its direct neighbors.      |  ===========>  | Distance Vector ONLY to   |
 +-------------------------------+                | immediate neighbors.      |
                                                  +---------------------------+
                                                                |
                                                                v
                                                  Apply Bellman-Ford Equation
                                                  to update local estimates
```

1. The Bellman-Ford Equation

Let $D_X(Y)$ be the estimated cost of the least-cost path from source router $X$ to destination router $Y$. The cost is calculated recursively across all immediate neighbors $V$ of $X$:

$$D_X(Y) = \min_V \left\{ C(X, V) + D_V(Y) \right\}$$

Where:

- $C(X, V)$: Direct link cost from router $X$ to neighbor $V$.
- $D_V(Y)$: Neighbor $V$'s estimated least-cost path to destination $Y$.
- $\min_V$: Minimum value taken across all immediate neighbor nodes $V$ connected to $X$.

2. Core Algorithmic Properties

- **Vector Scope**: A router's "Distance Vector" consists of its best-known minimum distance estimates to all other routers in the network.
- **Information Exchange**: Routers exchange distance vectors **exclusively with their immediate adjacent neighbors**. They do not broadcast full topology maps across the network.
- **Iterative Process**: The exchange continues in rounds until no new minimum cost information is generated between neighbors, achieving network stabilization/convergence.
- **Time & Message Complexity**:
    - **Time Complexity per Router**: For a router with $k$ neighbors in an $n$-node network, updating its distance vector takes $O(k \cdot n)$ time.
    - **Message Complexity per Round**: The maximum number of vector messages transmitted across all links in one update round is $2 \cdot |E|$, where $|E|$ is the number of edges.
    - **Convergence Bounds**: In a stable network without link failures, the maximum number of update rounds required to achieve convergence equals the length of the longest simple path in hops.

---

Module 2: Update Schemes (Synchronous vs. Asynchronous)

Distance Vector Routing protocols update routing tables using one of two communication schemes:

```
+-----------------------------------------------------------------------------------------------+
|                                  SYNCHRONOUS VS. ASYNCHRONOUS UPDATES                         |
+-------------------+------------------------------------+--------------------------------------+
| Feature           | Synchronous Updates                | Asynchronous Updates                 |
+-------------------+------------------------------------+--------------------------------------+
| Trigger           | Periodic timer expirations         | Event-driven (link cost change or    |
|                   | (e.g., every 30 seconds)           | new vector from neighbor)            |
| Exchange Scope    | All routers send full vectors to   | Routers send updates ONLY when their |
|                   | neighbors in every round           | distance vector changes              |
| Idle Behavior     | Continues periodic exchanges even  | Does nothing if no distance vector   |
|                   | if no costs change                 | estimates change                     |
+-------------------+------------------------------------+--------------------------------------+
```

---

Module 3: Count-to-Infinity Problem & Loop Mitigation Techniques

A major vulnerability of Distance Vector Routing is its susceptibility to **routing loops** and the **Count-to-Infinity problem** when a link fails or link cost increases dramatically.

```
                     THE COUNT-TO-INFINITY PROBLEM
                     
  Link A-B Fails!
  [ A ] === X === [ B ] ------------- [ C ]
                  Cost: 1             Cost: 2 (via B)
                  
  1. B detects failure and sets D_B(A) = Infinity.
  2. Before B's update reaches C, C sends its old vector D_C(A) = 2 to B.
  3. B assumes C has an alternate path to A of cost 2 and sets D_B(A) = 1 + 2 = 3 (via C)!
  4. B and C iteratively increment their cost to A (3, 4, 5, 6...) until reaching "Infinity".
```

Loop Mitigation Techniques

1. **Path Advertising (Next Hop Inclusion)**:
    - Each entry in the distance vector includes both the path cost and the **Next Hop ("Via")** node identifier used to reach that destination.
2. **Split Horizon**:
    - **Rule**: A router does **not** advertise a route for a destination back to the neighbor from which it learned that route (i.e., if that neighbor is its Next Hop).
3. **Poison Reverse**:
    - **Rule**: A router advertises a distance of $\infty$ for a destination back to a neighbor if that neighbor is its Next Hop for that destination entry.
4. **Loop Elimination Scope**: Split Horizon and Poison Reverse successfully prevent **2-node routing loops**. However, they **cannot prevent routing loops involving 3 or more nodes**.

---

Module 4: Step-by-Step Solved Problems & Exam Questions

Problem 1: Distance Vector Calculation for Router R

- **Question**: A network consists of 5 routers $P, Q, R, S, T$. Router $R$ has direct neighbors $P$ (link cost $6$) and $T$ (link cost $2$). Given the distance vectors of $P$ and $T$:
    - **Vector** $P$: $P:0, Q:1, R:5, S:5, T:3$
    - **Vector** $T$: $P:3, Q:4, R:2, S:8, T:0$ Calculate the updated distance vector for Router $R$.
- **Step-by-Step Solution**: Apply the Bellman-Ford equation for each destination node $Y \in \{P, Q, R, S, T\}$ evaluating neighbors $V \in \{P, T\}$:
    1. **For Node** $P$: $$D_R(P) = \min \{ C(R,P) + D_P(P), \; C(R,T) + D_T(P) \} = \min \{6+0, \; 2+3\} = \min \{6, 5\} = \mathbf{5} \text{ (Via } T\text{)}$$
    2. **For Node** $Q$: $$D_R(Q) = \min \{ C(R,P) + D_P(Q), \; C(R,T) + D_T(Q) \} = \min \{6+1, \; 2+4\} = \min \{7, 6\} = \mathbf{6} \text{ (Via } T\text{)}$$
    3. **For Node** $R$: $$D_R(R) = \mathbf{0} \text{ (Via } R\text{)}$$
    4. **For Node** $S$: $$D_R(S) = \min \{ C(R,P) + D_P(S), \; C(R,T) + D_T(S) \} = \min \{6+5, \; 2+8\} = \min \{11, 10\} = \mathbf{10} \text{ (Via } T\text{)}$$
    5. **For Node** $T$: $$D_R(T) = \min \{ C(R,P) + D_P(T), \; C(R,T) + D_T(T) \} = \min \{6+3, \; 2+0\} = \min \{9, 2\} = \mathbf{2} \text{ (Via } T\text{)}$$
- **Answer**: Distance Vector of $R$: **Cost =** $(5, 6, 0, 10, 2)$**, Via =** $(T, T, R, T, T)$.

---

Problem 2: Distance Vector Update Iteration

- **Question**: Router $R$ has three neighboring routers $X, Y, Z$ with direct link costs $3, 2, 5$ respectively. In an update iteration, $R$ receives routing vectors from $X, Y, Z$ indicating distance to $P$ as $7, 6, 5$ and distance to $Q$ as $4, 6, 8$. Determine which of the statements are correct regarding $R$'s updated routing table:
    - (A) Distance from $R$ to $P$ will be stored as $10$
    - (B) Distance from $R$ to $Q$ will be stored as $7$
    - (C) The next-hop router for a packet from $R$ to $P$ is $Y$
    - (D) The next-hop router for a packet from $R$ to $Q$ is $Z$
- **Step-by-Step Solution**:
    1. **Calculate Least Cost from** $R$ **to** $P$: $$D_R(P) = \min \{ C(R,X)+D_X(P), \; C(R,Y)+D_Y(P), \; C(R,Z)+D_Z(P) \}$$ $$D_R(P) = \min \{ 3+7, \; 2+6, \; 5+5 \} = \min \{10, 8, 10\} = \mathbf{8} \text{ (Via } Y\text{)}$$
        - Statement (A) is False ($8 \neq 10$). Statement (C) is True (Next-hop is $Y$).
    2. **Calculate Least Cost from** $R$ **to** $Q$: $$D_R(Q) = \min \{ C(R,X)+D_X(Q), \; C(R,Y)+D_Y(Q), \; C(R,Z)+D_Z(Q) \}$$ $$D_R(Q) = \min \{ 3+4, \; 2+6, \; 5+8 \} = \min \{7, 8, 13\} = \mathbf{7} \text{ (Via } X\text{)}$$
        - Statement (B) is True ($7$). Statement (D) is False (Next-hop is $X$, not $Z$).
- **Answer**: **Options B and C**.

---

Problem 3: Distance Vector Routing Table Construction

- **Question**: Router $F$ has neighbors $A, D, E, G$ with estimated link delays $8, 10, 12, 6$ msec respectively. Given the routing tables of $A, D, E, G$:
    - $A$'s table: $A:0, B:40, C:14, D:17, E:21, F:9, G:24$
    - $D$'s table: $A:20, B:8, C:30, D:0, E:14, F:7, G:22$
    - $E$'s table: $A:24, B:27, C:7, D:20, E:0, F:11, G:22$
    - $G$'s table: $A:21, B:24, C:22, D:19, E:22, F:10, G:0$ Compute the updated routing table for $F$.
- **Step-by-Step Solution**: Note link costs: $C(F,A)=8, C(F,D)=12, C(F,E)=10, C(F,G)=6$.
    1. $D_F(A) = \min \{8+0, 12+20, 10+24, 6+21\} = \min \{8, 32, 34, 27\} = \mathbf{8} \text{ (Via } A\text{)}$
    2. $D_F(B) = \min \{8+40, 12+8, 10+27, 6+24\} = \min \{48, 20, 37, 30\} = \mathbf{20} \text{ (Via } D\text{)}$
    3. $D_F(C) = \min \{8+14, 12+30, 10+7, 6+22\} = \min \{22, 42, 17, 28\} = \mathbf{17} \text{ (Via } E\text{)}$
    4. $D_F(D) = \min \{8+17, 12+0, 10+20, 6+19\} = \min \{25, 12, 30, 25\} = \mathbf{12} \text{ (Via } D\text{)}$
    5. $D_F(E) = \min \{8+21, 12+14, 10+0, 6+22\} = \min \{29, 26, 10, 28\} = \mathbf{10} \text{ (Via } E\text{)}$
    6. $D_F(F) = \mathbf{0} \text{ (Via } F\text{)}$
    7. $D_F(G) = \min \{8+24, 12+22, 10+22, 6+0\} = \min \{32, 34, 32, 6\} = \mathbf{6} \text{ (Via } G\text{)}$
- **Answer**: $F$'s Routing Table: **Cost =** $(8, 20, 17, 12, 10, 0, 6)$**, Via =** $(A, D, E, D, E, F, G)$.

---

Problem 4: Unused Links Identification

- **Question**: Consider a network with 6 routers $R1$ to $R6$ connected with link weights:
    - $R1-R2 = 6, R1-R3 = 3, R2-R3 = 2, R2-R4 = 7, R3-R5 = 9, R4-R5 = 1, R4-R6 = 8, R5-R6 = 4$. After distance vector routing tables stabilize, how many links will never be used for carrying data?
    - (A) 1, (B) 2, (C) 3, (D) 4
- **Step-by-Step Solution**: Compute all-pairs shortest paths across the network:
    1. **Between** $R1$ **and** $R2$: Direct link weight is $6$. Path via $R3$ ($R1-R3-R2$) has cost $3 + 2 = 5$. Since $5 < 6$, traffic between $R1$ and $R2$ uses $R1-R3-R2$. Direct link $R1-R2$ **is UNUSED**.
    2. **Between** $R4$ **and** $R6$: Direct link weight is $8$. Path via $R5$ ($R4-R5-R6$) has cost $1 + 4 = 5$. Since $5 < 8$, traffic uses $R4-R5-R6$. Direct link $R4-R6$ **is UNUSED**.
    3. **Check all remaining links**:
        - $R1-R3$ (weight 3): Used for $R1 \to R3, R2, R5, R4, R6$.
        - $R2-R3$ (weight 2): Used for $R2 \to R1, R3$.
        - $R2-R4$ (weight 7): Used for $R1, R2 \to R4, R5, R6$ (cost $R2-R4-R5 = 7+1=8 < 2+9=11$).
        - $R4-R5$ (weight 1): Used for $R4 \to R5$.
        - $R3-R5$ (weight 9): Used for $R3 \to R5$ (cost $R3-R5 = 9 < 2+7+1=10$).
        - $R5-R6$ (weight 4): Used for $R5 \to R6$.
    4. Total unused links = 2 ($R1-R2$ and $R4-R6$).
- **Answer**: **Option B (2)**.

---

Problem 5: Unused Links Re-evaluation

- **Question**: Suppose the weights of all unused links in Problem 4 ($R1-R2$ and $R4-R6$) are changed to $2$. After the distance vector algorithm stabilizes, how many links will now remain unused?
    - (A) 0, (B) 1, (C) 2, (D) 3
- **Step-by-Step Solution**: Updated weights: $R1-R2 = 2$ and $R4-R6 = 2$.
    1. **Link** $R1-R2$ **(weight 2)**: Direct cost $2 < 3+2=5$. Now **USED**.
    2. **Link** $R4-R6$ **(weight 2)**: Direct cost $2 < 1+4=5$. Now **USED**.
    3. **Link** $R5-R6$ **(weight 4)**: Path via $R4$ ($R5-R4-R6$) has cost $1 + 2 = 3$. Since $3 < 4$, all traffic between $R5$ and $R6$ routes via $R4$. Direct link $R5-R6$ **becomes UNUSED**.
    4. All other 7 links remain active for optimal routes.
- **Answer**: **Option B (1)**.

---

Problem 6: Distance Vector Update After Link Cost Reduction

- **Question**: Five nodes $N1$ to $N5$ use Distance Vector Routing. Stabilized distance vectors:
    - $N1$: $(0, 1, 7, 8, 4)$
    - $N2$: $(1, 0, 6, 7, 3)$
    - $N3$: $(7, 6, 0, 2, 6)$
    - $N4$: $(8, 7, 2, 0, 4)$
    - $N5$: $(4, 3, 6, 4, 0)$ The cost of link $N2-N3$ reduces to $2$ in both directions. What will be the new distance vector at node $N3$ after the next round of updates?
    - (A) $(3, 2, 0, 2, 5)$
    - (B) $(3, 2, 0, 2, 6)$
    - (C) $(7, 2, 0, 2, 5)$
    - (D) $(7, 2, 0, 2, 6)$
- **Step-by-Step Solution**: Neighbors of $N3$ are $N2$ (new cost $2$) and $N4$ (cost $2$). Node $N3$ re-evaluates its vector $D_{N3}(Y) = \min \{ C(N3, N2)+D_{N2}(Y), \; C(N3, N4)+D_{N4}(Y) \}$:
    1. $D_{N3}(N1) = \min \{ 2 + 1, \; 2 + 8 \} = \min \{3, 10\} = \mathbf{3}$
    2. $D_{N3}(N2) = \min \{ 2 + 0, \; 2 + 7 \} = \min \{2, 9\} = \mathbf{2}$
    3. $D_{N3}(N3) = \mathbf{0}$
    4. $D_{N3}(N4) = \min \{ 2 + 7, \; 2 + 0 \} = \min \{9, 2\} = \mathbf{2}$
    5. $D_{N3}(N5) = \min \{ 2 + 3, \; 2 + 4 \} = \min \{5, 6\} = \mathbf{5}$
- **Answer**: **Option A (**$(3, 2, 0, 2, 5)$**)**.

---

Problem 7: Protocol Vulnerability Identification

- **Question**: Count to infinity is a problem associated with:
    - (A) Link state routing protocol
    - (B) Distance vector routing protocol
    - (C) DNS while resolving host name
    - (D) TCP for congestion control
- **Step-by-Step Solution**: The Count-to-Infinity problem occurs due to routing loops when bad news (link failure or cost increase) propagates slowly in Distance Vector Routing protocols.
- **Answer**: **Option B (Distance vector routing protocol)**.

---

Problem 8: Routing Loop Probability Analysis

- **Question**: Three routers $P, Q, R$ are connected sequentially ($P-Q-R$) with unit link costs. All routers have converged tables, after which link $Q-R$ fails. $P$ and $Q$ send routing updates at random times at the same average rate. Calculate the probability of a routing loop forming between $P$ and $Q$ leading to the Count-to-Infinity problem.
- **Step-by-Step Solution**:
    1. **Initial Converged State**:
        - $Q$'s table for $R$: Cost $1$, Via $R$.
        - $P$'s table for $R$: Cost $2$, Via $Q$.
    2. **When Link** $Q-R$ **Fails**:
        - $Q$ detects the failure immediately and sets $D_Q(R) = \infty$.
        - $P$ still holds $D_P(R) = 2$ (Via $Q$).
    3. **Timing Analysis**:
        - **Case 1 (**$Q$ **transmits first)**: $Q$ sends $D_Q(R) = \infty$ to $P$. $P$ updates $D_P(R) = \infty$. No routing loop forms.
        - **Case 2 (**$P$ **transmits first)**: $P$ sends $D_P(R) = 2$ to $Q$ before receiving $Q$'s update. $Q$ assumes $P$ has an alternate path to $R$ of cost $2$ and updates $D_Q(R) = 1 + 2 = 3$ (Via $P$)! This creates a 2-node routing loop between $P$ and $Q$, triggering Count-to-Infinity.
    4. Since $P$ and $Q$ transmit independently with equal average rates, $P$ transmits first with probability $0.5$.
- **Answer**: **0.5**.