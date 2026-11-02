CHAPTER 3: STRONGLY CONNECTED COMPONENTS, ARTICULATION POINTS, & NETWORK RESILIENCE

---

1. STRONGLY CONNECTED COMPONENTS (SCCs) & TRANSPOSE GRAPHS

In directed graph analysis, a **Strongly Connected Component (SCC)** represents a maximal sub-network where every vertex can reach every other vertex via directed paths.

```
                     Directed Graph Transposition
   Original Graph G = (V, E)                 Transpose Graph Gᵀ = (V, Eᵀ)
   [ A ] ──────► [ B ]                        [ A ] ◄────── [ B ]
     │             │                            ▲             ▲
     ▼             ▼                            │             │
   [ C ] ──────► [ D ]                        [ C ] ◄────── [ D ]
```

---

A. The Transpose Graph ($G^T$)

For a directed graph $G = (V, E)$, its **Transpose Graph** $G^T = (V, E^T)$ shares the exact same set of vertices $V$, but **all directed edge directions are reversed**:

$$E^T = \Big\{ (v, u) \;\big|\; (u, v) \in E \Big\}$$

Crucial Structural Theorem

💡 **TRANSPOSE INVARIANCE THEOREM:** Graph $G$ and its transpose graph $G^T$ have **EXACTLY THE SAME Strongly Connected Components**. Reversing all edge directions changes inter-component flow directions but preserves intra-component reachability loops.

---

B. Kosaraju’s 2-Pass DFS Algorithm

Kosaraju's algorithm uses two sequential Depth-First Searches—the first on $G$ to compute finishing order, and the second on $G^T$ to extract components in topological order.

```
                       Kosaraju's 2-Pass Pipeline
                                    │
         ┌──────────────────────────┴──────────────────────────┐
         ▼                                                     ▼
Pass 1: DFS on Graph G                                Pass 2: DFS on Transpose Gᵀ
- Computes finishing times f[v]                       - Pop nodes from Stack in decreasing f[v] order
- Pushes dead nodes onto Stack                        - Runs DFS on Gᵀ to extract individual SCCs
```

Step-by-Step Pseudocode:

```
Algo Kosaraju_SCC(G, n) {
    Stack S;
    bool visited[n + 1] = {False};
    
    // Step 1: Run First DFS Pass on G to fill stack by finishing times
    for (i = 1; i <= n; i++) {
        if (!visited[i]) {
            DFS_Pass1(G, i, visited, S);
        }
    }
    
    // Step 2: Construct Transpose Graph G^T
    G_transpose = ConstructTranspose(G);
    
    // Step 3: Reset visited array for Second Pass
    for (i = 1; i <= n; i++) visited[i] = False;
    
    scc_count = 0;
    
    // Step 4: Run Second DFS Pass on G^T in decreasing order of finishing time
    while (!IsEmpty(S)) {
        u = Pop(S);
        
        if (!visited[u]) {
            scc_count = scc_count + 1;
            Print("Strongly Connected Component " + scc_count + ":");
            DFS_Pass2(G_transpose, u, visited); // Prints all nodes in this SCC
            PrintNewline();
        }
    }
}

Algo DFS_Pass1(G, u, visited, S) {
    visited[u] = True;
    for (each neighbor v in G.Adj[u]) {
        if (!visited[v]) DFS_Pass1(G, v, visited, S);
    }
    Push(S, u); // Push node to stack AFTER all descendants finish
}
```

---

C. Tarjan’s 1-Pass DFS Algorithm

Tarjan’s algorithm computes SCCs in a **single DFS pass** by maintaining a discovery timestamp `d[u]` and a **low-link value** **low[u]** for every vertex.

```
                      Tarjan's Low-Link Value Rule
                      
        d[u]  ──► Discovery timestamp when vertex u is first reached
        low[u] ──► Minimum d[v] reachable from u via tree-edges and
                   at most one back-edge to an active stack node
```

Low-Link Recurrence Formula:

For a vertex $u$ during DFS:

$$low[u] = \min \begin{cases} d[u] & \text{(Initial value)} \\ low[v] & \text{for every tree edge } (u, v) \\ d[v] & \text{for every back edge } (u, v) \text{ where } v \text{ is on active stack} \end{cases}$$

Component Root Identification:

When DFS finishes exploring all outgoing paths from $u$ and $low[u] == d[u]$, vertex $u$ is identified as the **root of an SCC**. All nodes popped from the active stack down to $u$ belong to this single SCC.

---

2. NETWORK VULNERABILITY: ARTICULATION POINTS & BRIDGES

In network communications, identifying single points of failure is essential for evaluating network resilience.

```
                   Network Vulnerability Primitive Types
                                     │
         ┌───────────────────────────┴───────────────────────────┐
         ▼                                                       ▼
Articulation Point (Cut Vertex)                          Bridge (Cut Edge)
- Removal of VERTEX increases component count          - Removal of EDGE increases component count
- Formally tested via: low[v] ≥ d[u]                   - Formally tested via: low[v] > d[u]
```

---

A. Formal Criteria for Articulation Points (Cut Vertices)

In an undirected connected graph $G = (V, E)$, a vertex $u$ is an **Articulation Point** if and only if it satisfies one of two mathematical rules in a DFS tree:

Rule 1: Root Vertex Criterion

If vertex $u$ is the **root** of the DFS tree, it is an Articulation Point **if and only if it has two or more children** in the DFS tree.

Rule 2: Non-Root Vertex Criterion

If vertex $u$ is **not the root** of the DFS tree, it is an Articulation Point **if and only if it has a child** $v$ **in the DFS tree such that**:

$$low[v] \ge d[u]$$

💡 **INTUITION OF THE LOW-LINK INEQUALITY:** $low[v] \ge d[u]$ means that no vertex in the subtree rooted at $v$ has a back-edge pointing to a strict ancestor of $u$. Therefore, if vertex $u$ is removed, the subtree rooted at $v$ becomes completely isolated from the rest of the graph.

---

B. Formal Criteria for Bridges (Cut Edges)

An edge $e = (u, v)$ in an undirected graph is a **Bridge** if its removal increases the total number of connected components.

Bridge Test Inequality:

In a DFS tree, an edge $(u, v)$ (where $u$ is parent of $v$) is a Bridge **if and only if**:

$$low[v] > d[u]$$

💡 **INTUITION OF THE BRIDGE INEQUALITY:** $low[v] > d[u]$ means that there is **zero back-edge** from the subtree rooted at $v$ to vertex $u$ or any ancestor of $u$. Edge $(u, v)$ is the sole connection servicing vertex $v$ and its descendants.

---

3. BICONNECTED COMPONENTS (BCCs) & BLOCK-CUT TREES

A. Biconnected Graph Definition

An undirected graph is **Biconnected** if:

1. It is connected.
2. It contains **no articulation points**.
3. Between any pair of vertices $x$ and $y$, there exist at least **two vertex-disjoint paths**.

---

B. Decomposing Graphs into Biconnected Components

A **Biconnected Component (BCC)** is a maximal biconnected subgraph. When a graph is decomposed into its BCCs:

- Edges are partitioned into **disjoint sets** (no edge belongs to two different BCCs).
- Vertices can belong to multiple BCCs **if and only if they are Articulation Points**.

```
                   Block-Cut Tree Condensed Topology
                   
       [ BCC 1: {A, B, C} ] ─── ( Cut Vertex C ) ─── [ BCC 2: {C, D, E} ]
                                       │
                                       └─── [ BCC 3: {C, F} ]
```

---

4. COMPARATIVE SUMMARY MATRIX

|Metric / Parameter|Kosaraju’s SCC Algorithm|Tarjan’s SCC Algorithm|Articulation Point Algorithm|
|---|---|---|---|
|**DFS Passes Required**|**2 Passes** (Pass 1 on $G$, Pass 2 on $G^T$)|**1 Pass**|**1 Pass**|
|**Core Decision Metric**|Stack order of finishing times $f[v]$|Low-link values $low[u] == d[u]$|Low-link condition $low[v] \ge d[u]$|
|**Applicable Graph Type**|**Directed Graphs**|**Directed Graphs**|**Undirected Graphs**|
|**Time Complexity**|\(\mathbf{O(|V|+|
|**Auxiliary Space**|\(O(|V|+|

---

5. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Kosaraju’s Algorithm Trace (NAT)

Consider a directed graph $G = (V, E)$ with 6 vertices $V = \{1, 2, 3, 4, 5, 6\}$ and directed edges:

- $1 \to 2, \; 2 \to 3, \; 3 \to 1$ (Cycle 1)
- $3 \to 4$ (Inter-component edge)
- $4 \to 5, \; 5 \to 6, \; 6 \to 4$ (Cycle 2)

Trace Kosaraju's algorithm step-by-step:

1. State the stack contents after Pass 1 DFS on $G$.
2. Trace Pass 2 DFS on $G^T$ and list all extracted Strongly Connected Components.

Step-by-Step Solution:

1. **Pass 1: DFS on Graph** $G$ **(starting from vertex 1):**
    - Visit 1 $\implies$ Visit 2 $\implies$ Visit 3.
    - From 3, visit 4 $\implies$ Visit 5 $\implies$ Visit 6.
    - From 6, neighbor 4 is visited. Dead node 6! Finish 6, push to Stack: `[6]`.
    - Dead node 5! Push to Stack: `[6, 5]`.
    - Dead node 4! Push to Stack: `[6, 5, 4]`.
    - Backtrack to 3. Neighbor 1 is visited. Dead node 3! Push to Stack: `[6, 5, 4, 3]`.
    - Dead node 2! Push to Stack: `[6, 5, 4, 3, 2]`.
    - Dead node 1! Push to Stack: `[6, 5, 4, 3, 2, 1]`.
    - **Stack Top to Bottom:** `1, 2, 3, 4, 5, 6`.
2. **Construct Transpose Graph** $G^T$**:**
    - Edges in $G^T$: $2 \to 1, \; 3 \to 2, \; 1 \to 3, \; 4 \to 3, \; 5 \to 4, \; 6 \to 5, \; 4 \to 6$.
3. **Pass 2: DFS on** $G^T$ **by popping Stack:**
    - **Pop 1:** Unvisited in $G^T$. Run DFS from 1 on $G^T$:
        - From 1, reaches 3, from 3 reaches 2.
        - **SCC 1 Extracted:** $\{1, 2, 3\}$.
    - **Pop 2:** Already visited. Skip.
    - **Pop 3:** Already visited. Skip.
    - **Pop 4:** Unvisited in $G^T$. Run DFS from 4 on $G^T$:
        - From 4, reaches 5, from 5 reaches 6 (edge $4 \to 3$ hits visited node 3).
        - **SCC 2 Extracted:** $\{4, 5, 6\}$.
    - **Pop 5, 6:** Already visited. Skip.

**Answer:**

1. Stack top-to-bottom after Pass 1: **1, 2, 3, 4, 5, 6**.
2. Extracted Components: $SCC_1 = \{1, 2, 3\}$ and $SCC_2 = \{4, 5, 6\}$.

---

Problem 2: Full Low-Link Computation & Articulation Point Discovery (NAT)

Consider an undirected graph with 7 vertices $V = \{1, 2, 3, 4, 5, 6, 7\}$ and edges:

- $(1, 2), (1, 3), (2, 3)$ (Triangular Block 1)
- $(3, 4)$ (Connecting Edge)
- $(4, 5), (5, 6), (6, 7), (7, 4)$ (4-Cycle Block 2)

Run a DFS starting from root vertex 1 (exploring neighbors in numerical order).

1. Compute the discovery time `d[u]` and low-link value `low[u]` for all 7 vertices.
2. Apply formal rules to identify all **Articulation Points** and **Bridges**.

Step-by-Step Solution:

1. **DFS Traversal Trace & Discovery Times** **d[u]****:**
    - `d[1] = 1`: Explore neighbor 2.
    - `d[2] = 2`: Explore neighbor 3.
    - `d[3] = 3`: Neighbor 1 is back-edge. Explore unvisited neighbor 4.
    - `d[4] = 4`: Explore neighbor 5.
    - `d[5] = 5`: Explore neighbor 6.
    - `d[6] = 6`: Explore neighbor 7.
    - `d[7] = 7`: Neighbor 4 is back-edge (points to `d[4] = 4`). Finish 7!
2. **Compute Low-Link Values** **low[u]** **Bottom-Up:**
    - **Node 7:** Back-edge to 4 $\implies low[7] = \min(d[7], d[4]) = \min(7, 4) = \mathbf{4}$.
    - **Node 6:** Tree-edge to 7 $\implies low[6] = \min(d[6], low[7]) = \min(6, 4) = \mathbf{4}$.
    - **Node 5:** Tree-edge to 6 $\implies low[5] = \min(d[5], low[6]) = \min(5, 4) = \mathbf{4}$.
    - **Node 4:** Tree-edge to 5 $\implies low[4] = \min(d[4], low[5]) = \min(4, 4) = \mathbf{4}$.
    - **Node 3:** Tree-edge to 4 and Back-edge to 1:
        - From back-edge to 1: $\min(d[3], d[1]) = \min(3, 1) = 1$.
        - From tree-edge to 4: $\min(1, low[4]) = \min(1, 4) = 1$.
        - $low[3] = \mathbf{1}$.
    - **Node 2:** Tree-edge to 3 $\implies low[2] = \min(d[2], low[3]) = \min(2, 1) = \mathbf{1}$.
    - **Node 1 (Root):** Tree-edge to 2 $\implies low[1] = \min(d[1], low[2]) = \min(1, 1) = \mathbf{1}$.

Summary Table of Timestamps and Low-Links:

|Vertex $u$|Discovery Time $d[u]$|Low-Link $low[u]$|DFS Parent|
|---|---|---|---|
|**1 (Root)**|1|1|NULL|
|**2**|2|1|1|
|**3**|3|1|2|
|**4**|4|4|3|
|**5**|5|4|4|
|**6**|6|4|5|
|**7**|7|4|6|

1. **Apply Articulation Point Rules:**
    - **Root Node 1:** DFS tree child count = 1 (only node 2) $\implies$ **NOT an Articulation Point**.
    - **Node 2:** Child 3 has $low[3] = 1 < d[2] = 2 \implies$ NOT an Articulation Point.
    - **Node 3:** Child 4 has $low[4] = 4 \ge d[3] = 3 \implies$ **ARTICULATION POINT!**
    - **Node 4:** Child 5 has $low[5] = 4 \ge d[4] = 4 \implies$ **ARTICULATION POINT!**
    - **Nodes 5, 6, 7:** Children low-links equal 4, satisfying $\ge$ conditions, but node 7 is a leaf.
2. **Apply Bridge Test Rule (**$low[v] > d[u]$**):**
    - Test Edge $(3, 4)$: Child is 4, parent is 3.
    - Check: $low[4] = 4$ and $d[3] = 3$.
    - Since $4 > 3$, edge $(3, 4)$ **IS A BRIDGE!**

**Answer:**

1. **Low-link Values:** `[low[1]=1, low[2]=1, low[3]=1, low[4]=4, low[5]=4, low[6]=4, low[7]=4]`.
2. **Articulation Points:** **Nodes 3 and 4**.
3. **Bridge Edge:** **Edge (3, 4)**.

---

Problem 3: Tarjan’s vs. Kosaraju’s Complexity (MCQ)

Why is Tarjan's algorithm often preferred over Kosaraju's algorithm for finding Strongly Connected Components in large-scale system implementations?

- (A) Tarjan's algorithm has a lower asymptotic time complexity of $O(|V|)$.
- (B) Tarjan's algorithm operates in a single DFS pass without constructing a transposed graph.
- (C) Kosaraju's algorithm requires exponential auxiliary memory.
- (D) Tarjan's algorithm works on undirected graphs while Kosaraju's does not.

Step-by-Step Solution:

Both algorithms share the exact same asymptotic time complexity of $O(|V| + |E|)$. However, Kosaraju's algorithm requires constructing and storing the Transpose Graph $G^T$ and running two separate DFS passes. Tarjan's algorithm computes SCCs in a **single pass on the original graph**, saving memory allocation overhead.

**Answer:** **(B)**.

---

Problem 4: Biconnected Component Edge Count (NAT)

A connected graph consists of a simple cycle of 5 vertices $C_5$ connected to a single leaf vertex $v_6$ via an edge $(v_5, v_6)$. How many **Biconnected Components** exist in this graph?

Step-by-Step Solution:

1. The cycle $C_5$ contains 5 vertices and 5 edges. No single vertex removal can disconnect $C_5$, so all 5 edges of $C_5$ form **1 Biconnected Component**.
2. The single edge $(v_5, v_6)$ connects to leaf $v_6$. Removing vertex $v_5$ isolates $v_6$. A single edge always forms its own maximal biconnected component.
3. Total Biconnected Components = $1 + 1 = \mathbf{2}$.

**Answer:** **2 Biconnected Components**.

---