CHAPTER 6: SINGLE-SOURCE SHORTEST PATH (DIJKSTRA'S ALGORITHM)

---

1. PROBLEM FORMULATION & GRAPH REPRESENTATIONS

The **Single-Source Shortest Path (SSSP)** problem is a foundational graph optimization problem. Given a weighted directed or undirected graph $G = (V, E)$ with an edge weight function $W: E \to \mathbb{R}_{\ge 0}$ and a designated **source vertex** $s \in V$, the objective is to determine the minimum total path weight from $s$ to every other vertex $v \in V$.

```
                        Single-Source Shortest Path (SSSP)
                                       │
              ┌────────────────────────┴────────────────────────┐
              ▼                                                 ▼
Non-Negative Edge Weights                      Negative Edge Weights
- Handled by DIJKSTRA'S ALGORITHM             - Dijkstra's algorithm FAILS
- Greedy Choice Property holds                - Must use BELLMAN-FORD ALGORITHM
- Solved in O(|E| log |V|) time               - Solved in O(|V| · |E|) time
```

---

Graph Storage Representations & Trade-Offs

Before executing path-finding algorithms, the input graph $G$ must be represented in computer memory using one of two standard data structures:

```
                      Graph Storage Representations
                                    │
         ┌──────────────────────────┴──────────────────────────┐
         ▼                                                     ▼
Adjacency Matrix                                      Adjacency List
- 2D Array of size |V| × |V|                          - Array of linked lists (size |V|)
- Memory: O(|V|²)                                     - Memory: O(|V| + |E|)
- Edge Lookup: O(1) time                              - Edge Lookup: O(degree(v)) time
- Preferred for DENSE graphs (|E| ≈ |V|²)             - Preferred for SPARSE graphs (|E| ≪ |V|²)
```

1. **Adjacency Matrix:** Memory requirement is $O(|V|^2)$. Finding all adjacent neighbors of a vertex $u$ requires scanning an entire row of length $|V|$, taking $O(|V|)$ time regardless of graph density.
2. **Adjacency List:** Memory requirement is $O(|V| + |E|)$. Finding all neighbors of vertex $u$ takes $O(\text{degree}(u))$ time, which is optimal for sparse graphs.

---

The Non-Negative Edge Weight Constraint

🚩 **CRITICAL PRINCIPLE OF DIJKSTRA'S ALGORITHM:** Dijkstra's algorithm **REQUIRES ALL EDGE WEIGHTS TO BE NON-NEGATIVE (**$W(e) \ge 0$**)**. If any edge in the graph has a **negative weight (**$W(e) < 0$**)**, Dijkstra's algorithm **may fail to find the correct shortest path**, even if the graph contains no negative-weight cycles.

Why Dijkstra Fails on Negative Edges

Dijkstra's algorithm relies on a **Greedy Monotonicity Assumption**: once an unvisited vertex $u$ with the minimum tentative distance is extracted from the priority queue, its shortest path value $d[u]$ is finalized (marked visited/permanent) and will **never be updated again**.

If negative edge weights exist, a path discovered later through a negative edge could yield a smaller overall path cost to an already "finalized" vertex, breaking the greedy invariant.

---

2. DIJKSTRA'S ALGORITHM MECHANICS & EDGE RELAXATION

The Edge Relaxation Operation

The fundamental atomic operation in shortest-path algorithms is **Edge Relaxation**. For a directed edge $(u, v)$ with weight $W(u, v)$, relaxation tests whether routing the path to vertex $v$ through vertex $u$ improves the current best-known tentative distance $d[v]$.

```
                             Relaxation Rule
  If (d[u] + W(u, v) < d[v]) ──► Update: d[v] = d[u] + W(u, v), Parent[v] = u
```

$$\text{Relax}(u, v, W): \quad \text{if } d[u] + W(u, v) < d[v] \implies \begin{cases} d[v] = d[u] + W(u, v) \\ \text{Parent}[v] = u \end{cases}$$

---

Step-by-Step Pseudocode Implementation

```
Algo Dijkstra_SSSP(G, W, V, s) {
    // Step 1: Initialize single-source distance estimates
    for (each vertex v in V) {
        d[v] = ∞;          // Tentative distance from source
        parent[v] = NULL;  // Predecessor pointer in shortest path tree
        visited[v] = False;
    }
    
    d[s] = 0; // Distance from source to itself is 0
    
    // Step 2: Process all |V| vertices
    for (count = 1; count <= |V|; count++) {
        // Greedy Choice: Extract unvisited vertex u with minimum d[u]
        u = Extract_Min_Distance_Vertex(d, visited, V);
        visited[u] = True; // Mark vertex u as finalized (Permanent)
        
        // Step 3: Relax all outgoing edges from vertex u
        for (each neighbor v adjacent to u) {
            if (!visited[v] && d[u] + W[u][v] < d[v]) {
                d[v] = d[u] + W[u][v]; // Relax distance
                parent[v] = u;         // Update path predecessor
            }
        }
    }
    
    return d; // Distance array containing shortest paths from s
}
```

---

3. COMPLEXITY ANALYSIS ACROSS IMPLEMENTATIONS

The time complexity of Dijkstra's algorithm depends on the underlying data structures used to store the graph and maintain the tentative distances $d[v]$ during `Extract-Min` and `Decrease-Key` operations.

```
                  Dijkstra's Complexity Variations
                                   │
         ┌─────────────────────────┼─────────────────────────┐
         ▼                         ▼                         ▼
 Adjacency Matrix           Adjacency List            Adjacency List
  + Array Search            + Binary Min-Heap         + Fibonacci Heap
   - Extract-Min: O(|V|)     - Extract-Min: O(log|V|)  - Extract-Min: O(log|V|)
   - Time: O(|V|²)           - Decrease-Key: O(log|V|) - Decrease-Key: O(1) amortized
   - Optimal: DENSE          - Time: O(|E| log |V|)    - Time: O(|E| + |V| log |V|)
                             - Optimal: SPARSE
```

1. Adjacency Matrix + Linear Array Search

- `Extract-Min` scans an unvisited array of size $|V|$, taking $O(|V|)$ time per iteration $\implies O(|V|^2)$ total across $|V|$ vertices.
- Edge relaxation scans a matrix row of length $|V|$ per vertex $\implies O(|V|^2)$ total across all edges.
- **Overall Time Complexity:** $\mathbf{O(|V|^2)}$.
- **Optimal Choice:** Best suited for **Dense Graphs** where $|E| \approx |V|^2$.

---

2. Adjacency List + Binary Min-Heap

- Inserting and extracting $|V|$ vertices from the Min-Heap takes $|V| \times O(\log_2 |V|) = O(|V| \log_2 |V|)$ time.
- Edge relaxation triggers `Decrease-Key` in the Min-Heap at most $|E|$ times, taking $|E| \times O(\log_2 |V|)$ time.
- **Overall Time Complexity:** $O((|V| + |E|) \log_2 |V|) = \mathbf{O(|E| \log_2 |V|)}$ (assuming connected graph, $|E| \ge |V| - 1$).
- **Optimal Choice:** Best suited for **Sparse Graphs** where $|E| \ll |V|^2$.

---

3. Adjacency List + Fibonacci Heap

- `Extract-Min` takes $O(\log_2 |V|)$ amortized time $\implies O(|V| \log_2 |V|)$.
- `Decrease-Key` takes $O(1)$ amortized constant time $\implies O(|E|)$.
- **Overall Time Complexity:** $\mathbf{O(|E| + |V| \log_2 |V|)}$.

---

4. COMPARATIVE SUMMARY MATRIX

|Parameter / Metric|Adjacency Matrix + Array|Adjacency List + Binary Heap|Adjacency List + Fibonacci Heap|
|---|---|---|---|
|**Extract-Min** **Time**|\(O(|V|)\)|
|**Decrease-Key** **Time**|$O(1)$|\(O(\log_2|V|
|**Total SSSP Time**|\(\mathbf{O(|V|^2)}\)|
|**Auxiliary Space**|\(O(|V|^2)\)|
|**Graph Preference**|**Dense Graphs**|**Sparse Graphs**|Theoretical Optimal|

---

5. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Full Step-by-Step Dijkstra Trace (NAT)

Consider a directed graph with 5 vertices $\{A, B, C, D, E\}$ and source vertex $s = A$. Edge weights are given below:

|Edge|Weight|Edge|Weight|Edge|Weight|
|---|---|---|---|---|---|
|**(A, B)**|4|**(B, C)**|1|**(C, E)**|3|
|**(A, C)**|2|**(B, D)**|5|**(D, E)**|1|
|**(C, B)**|1|**(C, D)**|8|**(A, D)**|10|

Trace Dijkstra's algorithm step-by-step to find the final shortest distance array $d[\cdot]$ from source $A$ and the shortest path to vertex $E$.

Step-by-Step Solution:

1. **Initialization:** $d[A] = 0, d[B] = \infty, d[C] = \infty, d[D] = \infty, d[E] = \infty$. Visited = $\emptyset$.
2. **Iteration 1:**
    - Extract unvisited vertex with minimum distance: **Pick A** ($d[A] = 0$). Mark A visited.
    - Relax outgoing edges from A:
        - Neighbor B: $d[B] = \min(\infty, 0 + 4) = \mathbf{4}$, Parent[B] = A.
        - Neighbor C: $d[C] = \min(\infty, 0 + 2) = \mathbf{2}$, Parent[C] = A.
        - Neighbor D: $d[D] = \min(\infty, 0 + 10) = \mathbf{10}$, Parent[D] = A.
    - Current distances: $d = [A:0^*, B:4, C:2, D:10, E:\infty]$.
3. **Iteration 2:**
    - Extract unvisited vertex with minimum distance: **Pick C** ($d[C] = 2$). Mark C visited.
    - Relax outgoing edges from C:
        - Neighbor B: $d[B] = \min(4, 2 + 1) = \mathbf{3}$, Parent[B] = C.
        - Neighbor D: $d[D] = \min(10, 2 + 8) = \mathbf{10}$ (no change).
        - Neighbor E: $d[E] = \min(\infty, 2 + 3) = \mathbf{5}$, Parent[E] = C.
    - Current distances: $d = [A:0^*, C:2^*, B:3, E:5, D:10]$.
4. **Iteration 3:**
    - Extract unvisited vertex with minimum distance: **Pick B** ($d[B] = 3$). Mark B visited.
    - Relax outgoing edges from B:
        - Neighbor C: Already visited.
        - Neighbor D: $d[D] = \min(10, 3 + 5) = \mathbf{8}$, Parent[D] = B.
    - Current distances: $d = [A:0^*, C:2^*, B:3^*, E:5, D:8]$.
5. **Iteration 4:**
    - Extract unvisited vertex with minimum distance: **Pick E** ($d[E] = 5$). Mark E visited.
    - Relax outgoing edges from E: No outgoing edges.
    - Current distances: $d = [A:0^*, C:2^*, B:3^*, E:5^*, D:8]$.
6. **Iteration 5:**
    - Extract remaining vertex: **Pick D** ($d[D] = 8$). Mark D visited.
    - Relax outgoing edges from D:
        - Neighbor E: $d[E] = \min(5, 8 + 1) = 5$ (no change).
7. **Final Shortest Distance Array from Source A:**
    - $d[A] = \mathbf{0}$
    - $d[B] = \mathbf{3}$ (Path: $A \to C \to B$)
    - $d[C] = \mathbf{2}$ (Path: $A \to C$)
    - $d[D] = \mathbf{8}$ (Path: $A \to C \to B \to D$)
    - $d[E] = \mathbf{5}$ (Path: $A \to C \to E$)

**Answer:** Final distances = **[A:0, B:3, C:2, D:8, E:5]**, Shortest path to E = **A** $\to$ **C** $\to$ **E** with cost **5**.

---

Problem 2: Negative Weight Failure Counterexample (MCQ)

Consider a directed graph with 3 vertices $\{A, B, C\}$ and directed edges $(A, B)$ with weight $5$, $(A, C)$ with weight $2$, and $(B, C)$ with weight $-10$. If Dijkstra's algorithm is executed starting from source $A$:

- (A) It correctly calculates the shortest distance to $C$ as $-5$.
- (B) It incorrectly outputs $d[C] = 2$ because vertex $C$ is finalized before vertex $B$.
- (C) It enters an infinite loop.
- (D) It detects and reports a negative cycle error.

Step-by-Step Solution:

1. Initialize $d[A] = 0, d[B] = \infty, d[C] = \infty$.
2. Step 1: Extract $A$ ($0$). Relax outgoing edges: $d[C] = 2$, $d[B] = 5$.
3. Step 2: Unvisited candidate vertices are $C (d[C]=2)$ and $B (d[B]=5)$. The minimum is $C$. Extract $C$ ($d[C] = 2$) and **mark C visited (finalized)**.
4. Step 3: Extract $B$ ($d[B] = 5$). Neighbor $C$ is already marked visited, so edge $(B, C)$ with weight $-10$ is ignored.
5. The true shortest path from $A$ to $C$ is $A \to B \to C = 5 + (-10) = -5$.
6. However, Dijkstra outputs $d[C] = 2$ because it greedily finalized $C$ prematurely in Step 2.

**Answer:** **(B)**.

---

Problem 3: Adjacency List Dijkstra Time Complexity (MCQ)

What is the worst-case time complexity of Dijkstra's single-source shortest path algorithm on a graph $G = (V, E)$ represented as an adjacency list when using a binary min-heap?

- (A) $O(V^2)$
- (B) $O(E \log_2 V)$
- (C) $O(V \log_2 E)$
- (D) $O(V + E)$

Step-by-Step Solution:

With an adjacency list and binary min-heap, extracting minimum distance vertices takes $O(|V| \log_2 |V|)$ and relaxing edges triggers `Decrease-Key` taking $O(|E| \log_2 |V|)$. The overall time complexity is $O(|E| \log_2 |V|)$.

**Answer:** **(B)**.

---

Problem 4: Shortest Path Tree vs. Minimum Cost Spanning Tree (MCQ)

Which of the following statements is **TRUE** regarding the Shortest Path Tree (SPT) generated by Dijkstra's algorithm from a source node $s$?

- (A) The Shortest Path Tree is always structurally identical to the Minimum Cost Spanning Tree (MCST).
- (B) The Shortest Path Tree minimizes the total sum of edge weights in the tree, while MCST minimizes distances from $s$.
- (C) The Shortest Path Tree minimizes the path distance from source $s$ to every vertex, but its total tree weight may be greater than that of the MCST.
- (D) An SPT cannot be constructed for directed graphs.

Step-by-Step Solution:

- MCST minimizes the **total sum of all edge weights present in the tree** ($\sum_{e \in T} W(e)$).
- Shortest Path Tree minimizes the **individual path distance from root** $s$ **to each vertex** $v$ ($d(s, v)$).
- These two optimization goals are distinct. A tree that minimizes distances from $s$ can have a larger total edge weight sum than an MCST.

**Answer:** **(C)**.