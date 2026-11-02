CHAPTER 1: GRAPH REPRESENTATIONS & FUNDAMENTAL TRAVERSALS (BFS & DFS)

---

1. GRAPH STORAGE REPRESENTATIONS & COMPLEXITY TRADE-OFFS

A graph $G = (V, E)$ consists of a set of $|V| = n$ vertices (nodes) and $|E| = e$ edges. To process graphs algorithmically, they must be stored in computer memory using one of two primary data structures: the **Adjacency Matrix** or the **Adjacency List**.

```
                       Graph Storage Representations
                                     │
         ┌───────────────────────────┴───────────────────────────┐
         ▼                                                       ▼
Adjacency Matrix                                       Adjacency List
- 2D Array of size n × n                               - Array of linked lists (size n)
- Space Complexity: O(n²)                              - Space Complexity: O(n + e)
- Neighbor Search: O(n) per vertex                     - Neighbor Search: O(deg(v)) per vertex
- Optimal for DENSE graphs (e ≈ n²)                    - Optimal for SPARSE graphs (e ≪ n²)
```

---

A. Adjacency Matrix

An Adjacency Matrix is a 2D boolean or integer array $A$ of dimensions $n \times n$, where:

- $A[i][j] = 1$ (or weight $w$) if an edge exists from vertex $i$ to vertex $j$.
- $A[i][j] = 0$ (or $\infty$) if no edge exists from vertex $i$ to vertex $j$.

Operational Properties:

- **Space Complexity:** $\mathbf{O(n^2)}$ auxiliary memory, independent of the number of edges $e$.
- **Edge Lookup Time:** Checking whether an edge exists between vertex $u$ and vertex $v$ takes $\mathbf{O(1)}$ constant time.
- **Neighbor Traversal Time:** Finding all adjacent neighbors of vertex $u$ requires scanning an entire row of length $n$, taking $\mathbf{O(n)}$ time.

---

B. Adjacency List

An Adjacency List consists of an array of $n$ linked lists, where array entry `List[i]` contains a linked list of all vertices directly reachable from vertex $i$.

Operational Properties:

- **Space Complexity:** $\mathbf{O(n + e)}$ auxiliary memory. For undirected graphs, each edge appears twice (consuming $2e$ nodes); for directed graphs, each edge appears once (consuming $e$ nodes).
- **Edge Lookup Time:** Checking if an edge exists between $u$ and $v$ requires traversing `List[u]`, taking $\mathbf{O(\text{degree}(u))}$ time.
- **Neighbor Traversal Time:** Iterating over all adjacent neighbors of vertex $u$ takes $\mathbf{O(\text{degree}(u))}$ time.

---

C. Traversal Complexity across Storage Models

The running time of graph traversal algorithms (BFS and DFS) depends directly on how neighbors are retrieved from memory:

|Storage Representation|Memory Space|Neighbor Lookup per Vertex|Total BFS / DFS Time Complexity|
|---|---|---|---|
|**Adjacency Matrix**|$\mathbf{O(n^2)}$|$O(n)$|$\mathbf{O(n^2)}$|
|**Adjacency List**|$\mathbf{O(n + e)}$|$O(\text{degree}(v))$|$\mathbf{O(n + e)}$|

---

2. BREADTH-FIRST SEARCH (BFS) / BREADTH-FIRST TRAVERSAL (BFT)

A. Core Concept & Data Structure

**Breadth-First Search (BFS)** is a level-order graph traversal strategy. Starting from a source vertex $s$, it explores all immediate 1-hop neighbors of $s$ before moving to 2-hop neighbors, 3-hop neighbors, and so on.

- **Primary Data Structure:** First-In, First-Out **Queue (FIFO)**.
- **Visited Tracking:** A boolean array `visited[1...n]` initialized to `False` to prevent re-visiting nodes and infinite loops in cyclic graphs.

```
                      Level-by-Level Queue Processing
Level 0:                         [ Source s ]
                                 /     |    \
Level 1:                     [ v₁ ]  [ v₂ ]  [ v₃ ]
                             /   \     |
Level 2:                 [ u₁ ] [ u₂] [ u₃]
```

---

B. Single-Source Shortest Path (SSSP) Property

In an **unweighted graph** (or a uni-weighted graph where every edge has equal weight $w = 1$), **BFS guarantees finding the Single-Source Shortest Path (SSSP)** from the source vertex to all other reachable vertices in terms of edge count.

---

C. Step-by-Step Pseudocode Implementation

```
Algo BFS_Traversal(G, start_vertex, n) {
    // Step 1: Initialize helper arrays and Queue
    bool visited[n + 1] = {False};
    Queue Q;
    
    // Enqueue start_vertex
    visited[start_vertex] = True;
    Enqueue(Q, start_vertex);
    
    // Step 2: Loop until Queue is completely empty
    while (!IsEmpty(Q)) {
        u = Dequeue(Q);
        Process(u); // Print or inspect vertex u
        
        // Step 3: Explore all unvisited adjacent neighbors
        for (each neighbor v adjacent to u) {
            if (!visited[v]) {
                visited[v] = True;      // Mark visited BEFORE enqueuing
                parent[v] = u;          // Predecessor pointer
                level[v] = level[u] + 1;// Distance level from source
                Enqueue(Q, v);
            }
        }
    }
}
```

---

3. DEPTH-FIRST SEARCH (DFS) / DEPTH-FIRST TRAVERSAL (DFT)

A. Core Concept & Operational States

**Depth-First Search (DFS)** explores a graph by diving as deep as possible along each branch before backtracking.

- **Primary Data Structure:** Last-In, First-Out **Stack (LIFO)** or the **System Recursion Stack**.
- **Vertex Operational States:** During traversal, every vertex passes through three distinct phases:
    1. **Exploring Node (E-node):** The vertex currently under active exploration.
    2. **Live Node:** A vertex that has been discovered but not yet fully explored (currently present on the stack).
    3. **Dead Node:** A vertex whose adjacency list has been completely processed and has been popped off the stack.

---

B. Timestamping Mechanism: Discovery & Finishing Times

To analyze graph properties (such as cycle detection, topological sorting, and component identification), DFS assigns two integer timestamps to each vertex $v$:

1. **Discovery Time (**$d[v]$**):** The global clock step at which vertex $v$ is discovered/visited for the very first time (pushed onto stack).
2. **Finishing Time (**$f[v]$**):** The global clock step at which vertex $v$ becomes a dead node (popped off stack).

```
              Timestamp Lifecycle of Node v:   [ d[v] / f[v] ]
              - Discovered at time d[v] (Push to Stack)
              - Fully Explored at time f[v] (Pop from Stack)
              - Total Active Duration = f[v] - d[v]
```

---

C. Parenthesis Theorem (Interval Property)

For any two vertices $u$ and $v$ in a DFS traversal, the time intervals $[d[u], f[u]]$ and $[d[v], f[v]]$ satisfy **EXACTLY ONE** of the following three mutually exclusive conditions:

1. **Entirely Disjoint Intervals:** $[d[u], f[u]] \cap [d[v], f[v]] = \emptyset$. Neither vertex is an ancestor of the other in the DFS tree/forest.
2. $v$ **is a Descendant of** $u$**:** Interval $[d[v], f[v]]$ is contained strictly within $[d[u], f[u]]$ (i.e., $d[u] < d[v] < f[v] < f[u]$).
3. $u$ **is a Descendant of** $v$**:** Interval $[d[u], f[u]]$ is contained strictly within $[d[v], f[v]]$ (i.e., $d[v] < d[u] < f[u] < f[v]$).

```
                       Parenthesis Theorem Intervals
  Disjoint:         [----- u -----]       [----- v -----]
  Descendant:       [-------------- u --------------]
                          [----- v -----]
```

---

D. Step-by-Step Pseudocode Implementation

```
global_time = 0;

Algo DFS_Traversal(G, n) {
    bool visited[n + 1] = {False};
    
    // Outer loop handles disconnected graphs (DFS Forest)
    for (i = 1; i <= n; i++) {
        if (!visited[i]) {
            DFS_Visit(i, visited);
        }
    }
}

Algo DFS_Visit(u, visited) {
    visited[u] = True;
    global_time = global_time + 1;
    d[u] = global_time; // Discovery timestamp
    
    // Explore adjacent neighbors
    for (each neighbor v adjacent to u) {
        if (!visited[v]) {
            parent[v] = u;
            DFS_Visit(v, visited); // Recursive call
        }
    }
    
    global_time = global_time + 1;
    f[u] = global_time; // Finishing timestamp (Dead node)
}
```

---

4. COMPARATIVE SUMMARY MATRIX: BFS VS. DFS

|Parameter / Metric|Breadth-First Search (BFS)|Depth-First Search (DFS)|
|---|---|---|
|**Data Structure**|**Queue (FIFO)**|**Stack (LIFO) / Recursion**|
|**Traversal Behavior**|Level-by-level (Broad expansion)|Path-by-path (Deep probing)|
|**Adjacency Matrix Time**|$\mathbf{O(n^2)}$|$\mathbf{O(n^2)}$|
|**Adjacency List Time**|$\mathbf{O(n + e)}$|$\mathbf{O(n + e)}$|
|**Auxiliary Memory Space**|$O(n)$ (Queue storage)|$O(n)$ (Stack depth)|
|**Shortest Path Unweighted**|**Guarantees Shortest Path (SSSP)**|No guarantee|
|**Primary Applications**|Shortest path, Level listing|Topological Sort, SCC, Articulation Points|

---

5. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Full BFS & DFS Trace with Discovery/Finishing Times (NAT)

Consider an undirected connected graph $G = (V, E)$ with 8 vertices $V = \{A, B, C, D, E, F, G, H\}$ and edges:

- $(A, B), (A, C)$
- $(B, D), (B, E)$
- $(C, F), (C, G)$
- $(D, H), (E, H), (F, H), (G, H)$

Assuming neighbors are visited in **alphabetical order**:

1. Trace the **FIFO BFS traversal sequence** starting from vertex $A$.
2. Trace the **recursive DFS traversal sequence** starting from vertex $A$.
3. Compute the **Discovery Time (**$d$**)** and **Finishing Time (**$f$**)** for all 8 vertices during the DFS traversal.

Step-by-Step Solution:

1. **BFS Traversal Trace (Queue FIFO):**
    - Start at **A**: Mark A visited, Enqueue(A). Queue: `[A]`
    - Dequeue A. Neighbors of A in alphabetical order: **B, C**. Mark visited, Enqueue(B), Enqueue(C). Queue: `[B, C]`
    - Dequeue B. Neighbors of B: **D, E**. Mark visited, Enqueue(D), Enqueue(E). Queue: `[C, D, E]`
    - Dequeue C. Neighbors of C: **F, G**. Mark visited, Enqueue(F), Enqueue(G). Queue: `[D, E, F, G]`
    - Dequeue D. Neighbor of D: **H**. Mark visited, Enqueue(H). Queue: `[E, F, G, H]`
    - Dequeue E. Neighbors of E: H (already visited).
    - Dequeue F, G, H sequentially.
    - **BFS Output Sequence:** **A B C D E F G H**
2. **DFS Traversal Trace (Stack / Recursive Deep Probing):**
    - Time 1: Discover **A** $\implies d[A] = 1$. Neighbor alphabetical: B, C. Go to **B**.
    - Time 2: Discover **B** $\implies d[B] = 2$. Neighbor alphabetical: D, E. Go to **D**.
    - Time 3: Discover **D** $\implies d[D] = 3$. Neighbor: H. Go to **H**.
    - Time 4: Discover **H** $\implies d[H] = 4$. Neighbors of H: E, F, G (alphabetical). Go to **E**.
    - Time 5: Discover **E** $\implies d[E] = 5$. All neighbors of E (B, H) visited. Pop E!
    - Time 6: Finish **E** $\implies f[E] = 6$. Backtrack to H.
    - Time 7: From H, explore next unvisited neighbor **F**. Discover **F** $\implies d[F] = 7$. Neighbors of F: C, H. Go to **C**.
    - Time 8: Discover **C** $\implies d[C] = 8$. Neighbors of C: A, F, G. Go to **G**.
    - Time 9: Discover **G** $\implies d[G] = 9$. All neighbors of G (C, H) visited. Pop G!
    - Time 10: Finish **G** $\implies f[G] = 10$. Backtrack to C.
    - Time 11: All neighbors of C visited. Finish **C** $\implies f[C] = 11$. Backtrack to F.
    - Time 12: Finish **F** $\implies f[F] = 12$. Backtrack to H.
    - Time 13: Finish **H** $\implies f[H] = 13$. Backtrack to D.
    - Time 14: Finish **D** $\implies f[D] = 14$. Backtrack to B.
    - Time 15: Finish **B** $\implies f[B] = 15$. Backtrack to A.
    - Time 16: Finish **A** $\implies f[A] = 16$.
3. **Summary of Timestamps** $(d / f)$**:**
    - $A: (1 / 16)$
    - $B: (2 / 15)$
    - $D: (3 / 14)$
    - $H: (4 / 13)$
    - $E: (5 / 6)$
    - $F: (7 / 12)$
    - $C: (8 / 11)$
    - $G: (9 / 10)$

**Answer:**

1. **BFS Sequence:** `A B C D E F G H`
2. **DFS Sequence:** `A B D H E F C G`
3. **Total Timestamps:** Ranges from 1 to $2 \times 8 = 16$.

---

Problem 2: Parenthesis Interval Property Verification (MCQ)

Using the timestamps from Problem 1, which of the following statements is **TRUE** regarding the relationship between vertex $B$ $(2/15)$ and vertex $E$ $(5/6)$?

- (A) Intervals 12 and 34 are disjoint, so neither is a descendant.
- (B) Interval 34 is strictly nested inside 12, proving vertex $E$ is a descendant of vertex $B$ in the DFS tree.
- (C) Interval 12 is nested inside 34, proving $B$ is a descendant of $E$.
- (D) The timestamps violate the Parenthesis Theorem.

Step-by-Step Solution:

Since $d[B] = 2 < d[E] = 5 < f[E] = 6 < f[B] = 15$, the interval 34 is contained entirely inside 12. By condition 2 of the Parenthesis Theorem, vertex $E$ is a descendant of vertex $B$.

**Answer:** **(B)**.

---

Problem 3: Valid BFS Traversal Sequence Verification (MSQ)

Given an undirected graph with vertices $\{A, B, C, D, E\}$ and edges $(A, B), (A, C), (B, D), (C, D), (D, E)$. Which of the following sequence(s) represent a **valid BFS traversal** starting from vertex $A$?

- (A) `A B C D E`
- (B) `A C B D E`
- (C) `A B D C E`
- (D) `A C D B E`

Step-by-Step Solution:

1. Level 0: `A`.
2. Level 1: Neighbors of `A` are `B` and `C`. They must appear immediately after `A` in any order (`A B C...` or `A C B...`).
3. Level 2: Neighbors of `B` and `C` is `D`. Must appear after both `B` and `C`.
4. Level 3: Neighbor of `D` is `E`. Must appear after `D`.
5. Valid Level Orderings: `A -> (B, C) -> D -> E` or `A -> (C, B) -> D -> E`.
6. **Sequence (A)** **A B C D E****:** Valid (B then C at level 1).
7. **Sequence (B)** **A C B D E****:** Valid (C then B at level 1).
8. **Sequence (C)** **A B D C E****:** Invalid (`D` appears before `C` is visited at level 1).
9. **Sequence (D)** **A C D B E****:** Invalid (`D` appears before `B` is visited at level 1).

**Answer:** **(A) and (B)**.

---

Problem 4: Disconnected Graph Connected Components (NAT)

A graph has 10 vertices $\{1, 2, \dots, 10\}$. A DFS traversal produces discovery and finishing timestamps:

- Vertex 1: (1, 6)
- Vertex 2: (2, 5)
- Vertex 3: (3, 4)
- Vertex 4: (7, 12)
- Vertex 5: (8, 11)
- Vertex 6: (9, 10)
- Vertex 7: (13, 16)
- Vertex 8: (14, 15)
- Vertex 9: (17, 20)
- Vertex 10: (18, 19)

How many **connected components** exist in this graph?

Step-by-Step Solution:

1. Examine the root-level interval ranges (outermost non-overlapping intervals):
    - Component 1: Interval 45 contains sub-intervals 13 and 67 $\implies \{1, 2, 3\}$.
    - Component 2: Interval 89 contains sub-intervals 1011 and 1213 $\implies \{4, 5, 6\}$.
    - Component 3: Interval 1415 contains sub-interval 216 $\implies \{7, 8\}$.
    - Component 4: Interval 1718 contains sub-interval 1920 $\implies \{9, 10\}$.
2. There are 4 disjoint maximal interval blocks.

**Answer:** **4 connected components**.