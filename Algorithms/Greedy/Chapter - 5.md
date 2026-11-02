MINIMUM COST SPANNING TREES (MCST) — PRIM'S & KRUSKAL'S ALGORITHMS

---

1. SPANNING TREE DEFINITION & GRAPH PROPERTIES

In graph theory, a **Spanning Tree** is a fundamental structural sub-network of a connected, undirected, weighted graph.

```
                     Connected Undirected Graph G = (V, E)
                                       │
                                       ▼
                       Spanning Tree T = (V, E')
            - Includes ALL |V| = n vertices from G
            - Includes exactly |E'| = n - 1 edges
            - Connected & Contains ZERO cycles
```

A. Core Mathematical Definitions

Given a connected, undirected, weighted graph $G = (V, E)$ with $|V| = n$ vertices and $|E| = e$ edges:

1. **Spanning Tree (**$T$**):** A subgraph $T = (V, E')$ of $G$ that spans all vertices $V$ such that $T$ is **connected** and contains **no cycles**.
2. **The** $n - 1$ **Edge Rule:** Any valid spanning tree of a graph containing $n$ vertices contains **EXACTLY** $n - 1$ **edges**.
    - If edge count $< n - 1$: The graph becomes disconnected (cannot span all vertices).
    - If edge count $> n - 1$: The graph is guaranteed to contain at least one cycle (violates tree definition).
    - If edge count $= n - 1$ and connected: Guaranteed to be a valid spanning tree.

---

B. Counting Spanning Trees

1. Cayley's Formula for Complete Graphs

For a complete labeled graph $K_n$ containing $n$ vertices (where an edge exists between every pair of vertices):

$$\text{Total Number of Spanning Trees in } K_n = n^{n - 2}$$

- **For** $n = 2$**:** $2^{2-2} = 2^0 = \mathbf{1}$ spanning tree.
- **For** $n = 3$**:** $3^{3-2} = 3^1 = \mathbf{3}$ spanning trees.
- **For** $n = 4$**:** $4^{4-2} = 4^2 = \mathbf{16}$ spanning trees.
- **For** $n = 5$**:** $5^{5-2} = 5^3 = \mathbf{125}$ spanning trees.

2. Complete Bipartite Graphs

For a complete bipartite graph $K_{m, n}$ with partition sizes $m$ and $n$:

$$\text{Total Number of Spanning Trees in } K_{m, n} = m^{n - 1} \cdot n^{m - 1}$$

---

C. Minimum Cost Spanning Tree (MCST)

Given an edge-weighted graph $G = (V, E, W)$, the **Minimum Cost Spanning Tree (MCST)** is a spanning tree $T^*$ whose sum of edge weights is minimized:

$$\text{Cost}(T^*) = \min \left\{ \sum_{e \in E'} W(e) \right\}$$

💡 **MCST UNIQUENESS PRINCIPLE:**

- **Distinct Edge Weights:** If all edge weights in graph $G$ are **strictly distinct**, there exists **EXACTLY ONE UNIQUE MCST**.
- **Duplicate Edge Weights:** If some edge weights are identical, there may exist multiple structural MCST configurations, but the **minimum cost value itself is ALWAYS UNIQUE**.

---

2. PRIM'S ALGORITHM (VERTEX-EXPANDING GREEDY APPROACH)

A. Algorithmic Mechanics

Prim's algorithm is a **vertex-centric** greedy approach. It grows a single tree continuously starting from an arbitrary initial vertex until all $n$ vertices are spanned.

```
                      Prim's Algorithm Execution
                                   │
              ┌────────────────────┴────────────────────┐
              ▼                                         ▼
   Single Growing Tree T                      Cut Expansion Rule
- Starts at initial vertex v₁           - Selects minimum weight edge (u, v)
- Maintains single component            - u is inside T, v is outside T
```

1. Initialize a single component containing a start vertex.
2. At each step, identify the **lightest edge** $(u, v)$ crossing the cut between vertices in the growing tree $T$ and vertices outside $T$.
3. Add vertex $v$ and edge $(u, v)$ to $T$.
4. Repeat until $T$ contains $n - 1$ edges and all $n$ vertices.

---

B. Step-by-Step Pseudocode Implementation

```
Algo Prim_MCST(G, W, n, start_vertex) {
    // Step 1: Initialize helper arrays
    for (i = 1; i <= n; i++) {
        key[i] = ∞;          // Minimum weight edge connecting vertex i to T
        visited[i] = False;  // Tracking spanned vertices
        parent[i] = -1;     // Parent pointer in spanning tree
    }
    
    key[start_vertex] = 0;
    total_cost = 0;
    
    // Step 2: Loop to span all n vertices
    for (count = 1; count <= n; count++) {
        // Find unvisited vertex u with minimum key value
        u = Extract_Min_Key_Vertex(key, visited, n);
        visited[u] = True;
        total_cost += key[u];
        
        // Update key values of adjacent unvisited neighbors
        for (each vertex v adjacent to u) {
            if (!visited[v] && W[u][v] < key[v]) {
                key[v] = W[u][v];
                parent[v] = u;
            }
        }
    }
    return total_cost;
}
```

---

C. Data Structures & Time Complexity Analysis

The running time of Prim's algorithm depends directly on the data structure used to maintain key values and extract the minimum vertex:

1. Adjacency Matrix + Linear Array Search

- Finding the minimum key vertex among $n$ unvisited vertices takes $O(n)$ time.
- Repeating across $n$ iterations takes $n \times O(n) = O(n^2)$ time.
- Updating adjacent keys using adjacency matrix takes $O(n)$ per vertex $\implies O(n^2)$ total.
- **Time Complexity:** $\mathbf{O(n^2)}$ or $\mathbf{O(|V|^2)}$.
- **Optimal Context:** Highly effective for **Dense Graphs** where $|E| \approx |V|^2$.

2. Adjacency List + Min-Heap (Binary Heap)

- `Extract-Min` operation takes $O(\log_2 |V|)$ time, executed $|V|$ times $\implies O(|V| \log_2 |V|)$.
- `Decrease-Key` operation takes $O(\log_2 |V|)$ time, executed at most $|E|$ times $\implies O(|E| \log_2 |V|)$.
- **Time Complexity:** $\mathbf{O(|E| \log_2 |V|)}$ or $\mathbf{O(e \log n)}$.
- **Optimal Context:** Highly effective for **Sparse Graphs** where $|E| \ll |V|^2$.

3. Adjacency List + Fibonacci Heap

- `Extract-Min` takes $O(\log_2 |V|)$ amortized time.
- `Decrease-Key` takes $O(1)$ amortized constant time.
- **Time Complexity:** $\mathbf{O(|E| + |V| \log_2 |V|)}$.

---

3. KRUSKAL'S ALGORITHM (EDGE-EXPANDING GREEDY APPROACH)

A. Algorithmic Mechanics

Kruskal's algorithm is an **edge-centric** greedy approach. It operates by building a forest of disjoint trees, gradually merging them into a single tree.

```
                      Kruskal's Algorithm Execution
                                   │
              ┌────────────────────┴────────────────────┐
              ▼                                         ▼
    Global Edge Sorting                        Cycle Prevention (DSU)
- Sorts all |E| edges globally           - Checks if endpoints belong to same set
- Iterates from lightest to heaviest     - Uses Find-Set and Union operations
```

1. Sort all $|E|$ edges of graph $G$ in non-decreasing order of weight.
2. Initialize each vertex as a separate connected component using a **Disjoint-Set Union (DSU)** data structure.
3. Iterate through sorted edges. For an edge $(u, v)$:
    - If $\text{Find}(u) \neq \text{Find}(v)$ (endpoints belong to different components), add edge $(u, v)$ to the MCST and perform $\text{Union}(u, v)$.
    - If $\text{Find}(u) == \text{Find}(v)$, adding edge $(u, v)$ would form a cycle; **discard the edge**.
4. Terminate when $n - 1$ edges are selected.

---

B. Step-by-Step Pseudocode Implementation

```
Algo Kruskal_MCST(G, E, V) {
    // Step 1: Sort all edges in non-decreasing order of weight
    Sort_Edges_By_Weight(E); // O(|E| log |E|)
    
    // Step 2: Initialize DSU sets for each vertex
    for (v = 1; v <= |V|; v++) {
        Make_Set(v);
    }
    
    mcst_edges = ∅;
    total_cost = 0;
    
    // Step 3: Greedily process edges
    for (each edge (u, v) in sorted list E) {
        root_u = Find_Set(u);
        root_v = Find_Set(v);
        
        // If endpoints are in different components, no cycle is formed
        if (root_u != root_v) {
            mcst_edges = mcst_edges ∪ {(u, v)};
            total_cost += weight(u, v);
            Union_Sets(root_u, root_v);
            
            if (|mcst_edges| == |V| - 1) {
                break; // MCST complete
            }
        }
    }
    return total_cost;
}
```

---

C. Data Structures & Time Complexity Analysis

1. **Sorting Phase:** Sorting $|E|$ edges takes $O(|E| \log_2 |E|)$ time.
    - Since $|E| \le |V|^2$, $\log_2 |E| \le 2 \log_2 |V| = O(\log_2 |V|)$.
    - Thus, sorting phase takes $\mathbf{O(|E| \log_2 |V|)}$.
2. **Cycle Detection Phase (DSU Operations):**
    - Using **Union by Rank** and **Path Compression**, each `Find-Set` and `Union` operation runs in near-constant amortized time $O(\alpha(|V|))$, where $\alpha$ is the Inverse Ackermann function.
    - Total DSU processing for $|E|$ edges takes $O(|E| \cdot \alpha(|V|))$.
3. **Overall Time Complexity:** $$\text{Total Time} = O(|E| \log_2 |E|) + O(|E| \cdot \alpha(|V|)) = \mathbf{O(|E| \log_2 |V|)} \quad \text{or} \quad \mathbf{O(e \log n)}$$

---

4. COMPARATIVE SUMMARY MATRIX: PRIM'S VS. KRUSKAL'S

|Parameter / Feature|Prim's Algorithm|Kruskal's Algorithm|
|---|---|---|
|**Approach Basis**|Vertex-centric (Grows a single tree)|Edge-centric (Grows a forest of components)|
|**Intermediate Subgraph**|Always remains a single connected tree|Forest of disconnected components|
|**Cycle Prevention**|Implicit via visited vertex check|Explicit via **Disjoint-Set Union (DSU)**|
|**Adjacency Matrix Time**|\(\mathbf{O(|V|
|**Adjacency List + Min-Heap**|\(\mathbf{O(|E|
|**Optimal Graph Preference**|**Dense Graphs** (\(|E|

---

5. FUNDAMENTAL GRAPH & SPANNING TREE THEOREMS

Theorem 1: The Cycle Property

In any connected undirected weighted graph $G$, let $C$ be any simple cycle. If an edge $e$ in $C$ has a weight **strictly greater than all other edges in** $C$, then edge $e$ **CANNOT belong to any Minimum Cost Spanning Tree** of $G$.

Theorem 2: The Cut Property

In any connected undirected weighted graph $G$, let $(S, V \setminus S)$ be any cut that partitions vertices into two non-empty sets. If an edge $e = (u, v)$ crossing the cut has a weight **strictly smaller than all other edges crossing the cut**, then edge $e$ **MUST belong to every Minimum Cost Spanning Tree** of $G$.

Theorem 3: Constant Scalar Weight Invariance

1. **Additive Scaling:** If a constant real number $k$ is added to the weight of every edge in graph $G$, the **structural layout of the MCST remains completely unchanged**. The total MCST cost increases by exactly $(n - 1) \cdot k$.
2. **Multiplicative Scaling:** If every edge weight is multiplied by a positive constant $k > 0$, the **structural layout of the MCST remains completely unchanged**. The total cost is multiplied by $k$.

---

6. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Cayley's Formula Calculation (NAT)

A connected network is modeled as a complete graph $K_6$ with 6 labeled vertices. Calculate:

1. The total number of edges in $K_6$.
2. The total number of distinct spanning trees in $K_6$.

Step-by-Step Solution:

1. **Total Edges in Complete Graph** $K_n$**:** $$|E| = \frac{n(n - 1)}{2} = \frac{6 \times 5}{2} = \mathbf{15 \text{ edges}}$$
2. **Total Spanning Trees via Cayley's Formula (**$n^{n-2}$**):** $$T(K_6) = 6^{6 - 2} = 6^4 = 6 \times 6 \times 6 \times 6 = \mathbf{1296 \text{ spanning trees}}$$

**Answer:** Total Edges = **15**, Total Spanning Trees = **1296**.

---

Problem 2: Full Trace of Prim's and Kruskal's Algorithms (NAT)

Consider an undirected weighted graph $G$ with 6 vertices $\{A, B, C, D, E, F\}$ and the following edges:

|Edge|Weight|Edge|Weight|Edge|Weight|
|---|---|---|---|---|---|
|**(A, B)**|4|**(B, C)**|2|**(C, E)**|1|
|**(A, C)**|3|**(B, D)**|5|**(D, E)**|6|
|**(B, E)**|3|**(C, D)**|4|**(E, F)**|2|

1. Trace Kruskal's algorithm to find the sequence of accepted edges.
2. Calculate the total cost of the MCST.

Step-by-Step Solution:

1. **Sort All Edges in Ascending Order of Weight:**
    1. $(C, E)$ — Weight 1
    2. $(B, C)$ — Weight 2
    3. $(E, F)$ — Weight 2
    4. $(A, C)$ — Weight 3
    5. $(B, E)$ — Weight 3
    6. $(A, B)$ — Weight 4
    7. $(C, D)$ — Weight 4
    8. $(B, D)$ — Weight 5
    9. $(D, E)$ — Weight 6
2. **Kruskal's Selection Trace (Target =** $n - 1 = 5$ **edges):**
    - **Edge 1:** $(C, E)$**, Weight 1:** Connects $C$ and $E$. **ACCEPT**. Edges = 1.
    - **Edge 2:** $(B, C)$**, Weight 2:** Connects $B$ and $C$. **ACCEPT**. Edges = 2.
    - **Edge 3:** $(E, F)$**, Weight 2:** Connects $E$ and $F$. **ACCEPT**. Edges = 3.
    - **Edge 4:** $(A, C)$**, Weight 3:** Connects $A$ and $C$. **ACCEPT**. Edges = 4.
    - **Edge 5:** $(B, E)$**, Weight 3:** Endpoints $B$ and $E$ are already connected via path $B-C-E$. Forms cycle $B-C-E-B$. **REJECT**.
    - **Edge 6:** $(A, B)$**, Weight 4:** Endpoints $A$ and $B$ are already connected via $A-C-B$. **REJECT**.
    - **Edge 7:** $(C, D)$**, Weight 4:** Connects $D$ to component. **ACCEPT**. Edges = 5. (Complete!)
3. **Accepted MCST Edges:** $\{(C, E), (B, C), (E, F), (A, C), (C, D)\}$.
4. **Calculate Total MCST Weight:** $$\text{Total Cost} = 1 + 2 + 2 + 3 + 4 = \mathbf{12}$$

**Answer:** Accepted Edges = **(C,E), (B,C), (E,F), (A,C), (C,D)**, Total Cost = **12**.

---

Problem 3: Edge Weight Modification Theorem (GATE PYQ)

Let $T$ be a Minimum Cost Spanning Tree of a weighted graph $G$ with $n$ vertices and $e$ edges. Suppose the weight of every edge in $G$ is increased by 5. Which of the following statements is **TRUE**?

- (A) $T$ may no longer remain an MCST of $G$.
- (B) $T$ is guaranteed to remain an MCST, and its total cost increases by $5n$.
- (C) $T$ is guaranteed to remain an MCST, and its total cost increases by $5(n - 1)$.
- (D) $T$ is guaranteed to remain an MCST, and its total cost increases by $5e$.

Step-by-Step Solution:

1. By Constant Additive Weight Invariance (Theorem 3), adding a constant $k = 5$ to all edge weights preserves the relative ordering of all cuts and cycles.
2. Thus, the structural edge composition of tree $T$ remains **completely unchanged**.
3. Since tree $T$ contains exactly $n - 1$ edges, adding 5 to each edge increases the total tree weight by $5 \times (n - 1) = 5(n - 1)$.

**Answer:** **(C)**.

---

Problem 4: Adjacency List Prim's Running Time (MCQ)

What is the running time of Prim's algorithm for finding the MCST of a graph with $V$ vertices and $E$ edges when implemented using an Adjacency List and a Binary Min-Heap?

- (A) $O(V^2)$
- (B) $O(E \log_2 V)$
- (C) $O(V \log_2 E)$
- (D) $O(E + V)$

Step-by-Step Solution:

Using an Adjacency List with a Binary Min-Heap, extracting the minimum key vertex takes $O(\log_2 V)$ time across $V$ vertices, and decreasing key values takes $O(\log_2 V)$ time across $E$ edges. The total time complexity is $O(E \log_2 V)$.

**Answer:** **(B)**.
