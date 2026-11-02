CHAPTER 2: DFS EDGE CLASSIFICATIONS, TOPOLOGICAL SORT, & CONNECTED COMPONENTS

---

1. DFS EDGE CLASSIFICATIONS IN DIRECTED GRAPHS

When Depth-First Search (DFS) is executed on a directed graph $G = (V, E)$, it constructs a **DFS Spanning Forest**. Every edge $e = (u, v) \in E$ in the graph is partitioned into exactly one of four distinct edge types based on its relationship to the search tree structure and the discovery/finishing timestamps $[d, f]$.

```
                     DFS Directed Edge Classifications
                                     │
         ┌───────────────────────────┼───────────────────────────┐
         ▼                           ▼                           ▼
    Tree Edge                   Forward Edge               Backward Edge               Cross Edge
 (DFS Tree Branch)         (To Non-Child Descendant)        (To Ancestor)       (To Disjoint Branch / Node)
```

---

A. The Four Directed Edge Classifications

1. **Tree Edges:** Edges present directly in the DFS spanning tree or forest. An edge $(u, v)$ is a tree edge if vertex $v$ was discovered for the very first time by exploring edge $(u, v)$ from vertex $u$.
2. **Forward Edges:** Non-tree directed edges going from an ancestor node $u$ to a **non-child descendant** node $v$ in the DFS tree.
3. **Backward (Back) Edges:** Directed edges going from a node $u$ to one of its **ancestors** $v$ in the DFS tree (including self-loops).
    - **Cycle Detection Rule:** A directed graph contains at least one directed cycle if and only if a DFS traversal produces at least one **Backward Edge**.
4. **Cross Edges:** Directed edges going from a node $u$ to a node $v$ that is **neither an ancestor nor a descendant** of $u$. Cross edges can connect different branches within the same DFS tree or connect distinct trees in a DFS forest.

---

B. Timestamp Interval Criteria for Edge Types

By comparing the discovery timestamp $d[\cdot]$ and finishing timestamp $f[\cdot]$ of endpoints $u$ and $v$ during DFS, each directed edge $(u, v)$ can be classified deterministically:

```
                      Timestamp Relations for Edge (u ──► v)
Tree / Forward Edge:   [------------ u ------------]        (v nested inside u)
                            [----- v -----]

Backward Edge:         [------------ v ------------]        (u nested inside v)
                            [----- u -----]

Cross Edge:            [--- v ---]   [--- u ---]            (Disjoint, f[v] < d[u])
```

1. **Tree / Forward Edges (**$u \to v$**):** Interval $[d[v], f[v]]$ is nested strictly within $[d[u], f[u]]$ with $d[u] < d[v] < f[v] < f[u]$.
2. **Backward Edges (**$u \to v$**):** Interval $[d[u], f[u]]$ is nested strictly within $[d[v], f[v]]$ with $d[v] < d[u] < f[u] < f[v]$.
3. **Cross Edges (**$u \to v$**):** Intervals $[d[u], f[u]]$ and $[d[v], f[v]]$ are **entirely disjoint** with $f[v] < d[u]$ (i.e., vertex $v$ was completely finished before vertex $u$ was discovered).

---

2. TOPOLOGICAL SORTING ON DIRECTED ACYCLIC GRAPHS (DAGs)

A. Directed Acyclic Graphs (DAGs) & Precedence

A **Directed Acyclic Graph (DAG)** is a directed graph containing **no directed cycles**. DAGs model workflows, prerequisite structures, and task scheduling constraints where operations must follow a non-circular precedence order.

- **Source Node:** A vertex with **in-degree = 0** (no incoming edges).
- **Sink Node:** A vertex with **out-degree = 0** (no outgoing edges).

```
                     Topological Sort Linear Sequence
  [ Source (Indegree=0) ] ──► [ Task A ] ──► [ Task B ] ──► ... ──► [ Sink (Outdegree=0) ]
```

---

B. Definition & Existence Property

A **Topological Sort** (or Topological Ordering) of a DAG $G = (V, E)$ is a linear ordering of all its vertices such that for every directed edge $u \to v$, vertex $u$ appears **before** vertex $v$ in the sequence.

💡 **EXISTENCE THEOREM:**

- A graph admits a valid Topological Sort **if and only if it is a DAG**.
- If a graph contains even one directed cycle, a valid topological sort is **IMPOSSIBLE** because circular precedence dependencies cannot be linearized.

---

C. DFS-Based Topological Sort Algorithm

Topological sorting can be computed efficiently by running DFS on the DAG and tracking node finishing times $f[v]$:

```
Algo TopologicalSort_DFS(G) {
    // Step 1: Run DFS on DAG G to compute finishing time f[v] for all vertices
    DFS_Traversal(G);
    
    // Step 2: Arrange/output all vertices in DECREASING order of their finishing times f[v]
    Sort_Vertices_Descending_By_Finishing_Time();
    
    return Topological_Order_List;
}
```

Why Decreasing Finishing Times Yield Topological Order:

For any directed edge $u \to v$ in a DAG, vertex $v$ must finish before vertex $u$ finishes during DFS (i.e., $f[u] > f[v]$). Thus, sorting vertices in descending order of their finishing times guarantees that $u$ precedes $v$ in the output list.

---

D. Kahn’s Algorithm (In-Degree BFS Approach)

Kahn's algorithm is an alternative queue-based approach for topological sorting that iteratively removes vertices with zero in-degree:

```
Algo Kahn_TopologicalSort(G, V, E) {
    Queue Q;
    int indegree[V] = {0};
    
    // Step 1: Compute in-degree for all vertices
    for (each edge (u, v) in E) {
        indegree[v]++;
    }
    
    // Step 2: Enqueue all vertices with in-degree = 0
    for (i = 1; i <= V; i++) {
        if (indegree[i] == 0) {
            Enqueue(Q, i);
        }
    }
    
    count = 0;
    
    // Step 3: Process Queue
    while (!IsEmpty(Q)) {
        u = Dequeue(Q);
        topological_order.append(u);
        count++;
        
        // Reduce in-degree for all adjacent neighbors
        for (each neighbor v adjacent to u) {
            indegree[v]--;
            if (indegree[v] == 0) {
                Enqueue(Q, v);
            }
        }
    }
    
    // If count < V, graph contains a cycle!
    if (count != V) {
        Print("Graph contains a directed cycle! Topological sort impossible.");
        return NULL;
    }
    
    return topological_order;
}
```

---

3. CONNECTED COMPONENTS & COMPONENT DAG REDUCTION

A. Connected Components in Undirected Graphs

In an undirected graph, a **Connected Component** is a maximal connected subgraph. Every pair of vertices in a connected component is joined by a path, and no vertex outside the component shares an edge with any vertex inside.

- Computing connected components takes $O(|V| + |E|)$ time using BFS or DFS.

---

B. Weakly vs. Strongly Connected Components in Directed Graphs

For a directed graph $G = (V, E)$:

1. **Weakly Connected Component (WCC):** A maximal set of vertices connected when all directed edges are treated as undirected edges.
2. **Strongly Connected Component (SCC):** A maximal set of vertices such that for every pair of vertices $x, y \in SCC$, there exists a **directed path from** $x$ **to** $y$ **AND a directed path from** $y$ **to** $x$.

```
                   Weakly vs. Strongly Connected Components
            Weakly Connected:                  Strongly Connected:
        [ A ] ──► [ B ] ──► [ C ]           [ A ] ─────► [ B ]
                                              ▲            │
                                              │            ▼
                                              └────────── [ C ]
```

---

C. Component DAG Reduction

If each Strongly Connected Component of a directed graph is collapsed into a single meta-vertex, the resulting condensed graph is **GUARANTEED to be a Directed Acyclic Graph (Component DAG)**.

```
                  Directed Graph Condensed to Component DAG
        [ SCC 1: {A, B, E} ] ──► [ SCC 2: {C, D} ] ──► [ SCC 4: {H} ]
                 │                                            ▲
                 └─────────────► [ SCC 3: {F, G} ] ───────────┘
```

---

4. COMPARATIVE SUMMARY MATRIX

|Parameter / Metric|DFS Edge Classification|Topological Sort (DFS/Kahn)|Connected Components|
|---|---|---|---|
|**Applicable Graph Type**|Directed / Undirected|**Strictly Directed Acyclic (DAG)**|Directed / Undirected|
|**Core Mechanism**|Timestamp $[d, f]$ comparison|Decreasing $f[v]$ or In-degree Queue|BFS / DFS Search Reachability|
|**Cycle Sensitivity**|Detects cycles via Back Edges|**Fails if cycles exist**|Invariant to cycles|
|**Time Complexity**|\(\mathbf{O(|V|+|
|**Space Complexity**|\(O(|V|)\)|

---

5. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Full Step-by-Step Edge Classification Trace (NAT)

Consider a directed graph $G = (V, E)$ with 5 vertices $V = \{1, 2, 3, 4, 5\}$ and directed edges:

- $1 \to 2, \; 1 \to 3, \; 2 \to 4, \; 3 \to 4, \; 4 \to 5, \; 5 \to 2$.

DFS is executed starting from vertex 1. Assume neighbors are explored in numerical order:

1. Determine the discovery time $d[u]$ and finishing time $f[u]$ for all 5 vertices.
2. Classify all 6 directed edges into Tree, Forward, Backward, or Cross edges.

Step-by-Step Solution:

1. **DFS Execution Trace:**
    - Time 1: Discover **1** $\implies d[1] = 1$. Neighbors: 2, 3. Explore **2**.
    - Time 2: Discover **2** $\implies d[2] = 2$. Neighbor: 4. Explore **4**.
    - Time 3: Discover **4** $\implies d[4] = 3$. Neighbor: 5. Explore **5**.
    - Time 4: Discover **5** $\implies d[5] = 4$. Neighbor: 2 (already visited, $d[2]=2, f[2]=?$).
    - Time 5: Finish **5** $\implies f[5] = 5$. Backtrack to 4.
    - Time 6: Finish **4** $\implies f[4] = 6$. Backtrack to 2.
    - Time 7: Finish **2** $\implies f[2] = 7$. Backtrack to 1.
    - Time 8: From 1, explore unvisited neighbor **3**. Discover **3** $\implies d[3] = 8$.
    - Time 9: Neighbors of 3: 4 (already visited, $f[4]=6 < d[3]=8$).
    - Time 10: Finish **3** $\implies f[3] = 9$. Backtrack to 1.
    - Time 11: Finish **1** $\implies f[1] = 10$.
2. **Endpoints Timestamp Summary Table** $[d / f]$**:**

|Vertex $u$|Discovery Time $d[u]$|Finishing Time $f[u]$|Status|
|---|---|---|---|
|**1**|1|10|Tree Root|
|**2**|2|7|Descendant of 1|
|**3**|8|9|Descendant of 1|
|**4**|3|6|Descendant of 2|
|**5**|4|5|Descendant of 4|

1. **Edge Classification Trace:**
    - **Edge** $(1, 2)$**:** Discovered unvisited node 2 $\implies$ **Tree Edge**.
    - **Edge** $(2, 4)$**:** Discovered unvisited node 4 $\implies$ **Tree Edge**.
    - **Edge** $(4, 5)$**:** Discovered unvisited node 5 $\implies$ **Tree Edge**.
    - **Edge** $(1, 3)$**:** Discovered unvisited node 3 $\implies$ **Tree Edge**.
    - **Edge** $(5, 2)$**:** Points from node 5 $[4/5]$ to ancestor 2 $[2/7]$. Since interval $[4, 5]$ is nested inside $[2, 7]$, it is a **Backward Edge** (indicates cycle $2 \to 4 \to 5 \to 2$).
    - **Edge** $(3, 4)$**:** Points from node 3 $[8/9]$ to node 4 $[3/6]$. Intervals $[8, 9]$ and $[3, 6]$ are disjoint with $f[4] = 6 < d[3] = 8 \implies$ **Cross Edge**.

**Answer:**

1. Timestamps: **1:[1/10], 2:[2/7], 3:[8/9], 4:[3/6], 5:[4/5]**.
2. Classifications: **Tree Edges: (1,2), (2,4), (4,5), (1,3)**; **Backward Edge: (5,2)**; **Cross Edge: (3,4)**.

---

Problem 2: Topological Sort via Kahn’s In-Degree Trace (NAT)

Consider a DAG with 6 vertices $\{A, B, C, D, E, F\}$ and directed edges:

- $A \to C, \; A \to D, \; B \to D, \; B \to E, \; C \to F, \; D \to F, \; E \to F$.

Trace Kahn's algorithm step-by-step and determine the resulting Topological Sort order (enqueuing ties in alphabetical order).

Step-by-Step Solution:

1. **Calculate Initial In-Degrees:**
    - $\text{In}[A] = 0$
    - $\text{In}[B] = 0$
    - $\text{In}[C] = 1$ (from A)
    - $\text{In}[D] = 2$ (from A, B)
    - $\text{In}[E] = 1$ (from B)
    - $\text{In}[F] = 3$ (from C, D, E)
2. **Step 1:** Enqueue all vertices with $\text{In} = 0$:
    - Queue: `[A, B]`
3. **Step 2:** Dequeue **A**:
    - Output: `A`
    - Decrement in-degrees of neighbors C and D:
        - $\text{In}[C] = 1 - 1 = 0 \implies$ Enqueue **C**.
        - $\text{In}[D] = 2 - 1 = 1$.
    - Queue: `[B, C]`
4. **Step 3:** Dequeue **B**:
    - Output: `A, B`
    - Decrement in-degrees of neighbors D and E:
        - $\text{In}[D] = 1 - 1 = 0 \implies$ Enqueue **D**.
        - $\text{In}[E] = 1 - 1 = 0 \implies$ Enqueue **E**.
    - Queue: `[C, D, E]`
5. **Step 4:** Dequeue **C**:
    - Output: `A, B, C`
    - Decrement in-degree of neighbor F: $\text{In}[F] = 3 - 1 = 2$.
    - Queue: `[D, E]`
6. **Step 5:** Dequeue **D**:
    - Output: `A, B, C, D`
    - Decrement in-degree of neighbor F: $\text{In}[F] = 2 - 1 = 1$.
    - Queue: `[E]`
7. **Step 6:** Dequeue **E**:
    - Output: `A, B, C, D, E`
    - Decrement in-degree of neighbor F: $\text{In}[F] = 1 - 1 = 0 \implies$ Enqueue **F**.
    - Queue: `[F]`
8. **Step 7:** Dequeue **F**:
    - Output: `A, B, C, D, E, F`

**Answer:** Topological Order = **A B C D E F**.

---

Problem 3: Valid Topological Sort Verification (MSQ)

Given a DAG with vertices $\{1, 2, 3, 4, 5\}$ and directed edges $(1, 2), (1, 3), (2, 4), (3, 4), (4, 5)$, which of the following sequence(s) represent **valid topological orderings**?

- (A) `1 2 3 4 5`
- (B) `1 3 2 4 5`
- (C) `1 2 4 3 5`
- (D) `2 1 3 4 5`

Step-by-Step Solution:

- **Precedence Requirements:**
    - 1 must precede 2, 3.
    - 2 and 3 must precede 4.
    - 4 must precede 5.
- **Option (A)** **1 2 3 4 5****:** Satisfies all edge directions ($1 \to 2, 1 \to 3, 2 \to 4, 3 \to 4, 4 \to 5$) $\implies$ **VALID**.
- **Option (B)** **1 3 2 4 5****:** Satisfies all edge directions $\implies$ **VALID**.
- **Option (C)** **1 2 4 3 5****:** Violates edge $3 \to 4$ (node 4 appears before 3) $\implies$ **INVALID**.
- **Option (D)** **2 1 3 4 5****:** Violates edge $1 \to 2$ (node 2 appears before 1) $\implies$ **INVALID**.

**Answer:** **(A) and (B)**.

---

Problem 4: Cycle Detection via Topological Sort (MCQ)

If Kahn's algorithm is executed on a directed graph with 8 vertices and the final output list contains only 5 vertices before the queue becomes empty, what can be concluded?

- (A) The graph contains 3 isolated vertices.
- (B) The graph contains at least one directed cycle involving the remaining 3 vertices.
- (C) The graph is a valid DAG.
- (D) The algorithm encountered a memory allocation error.

Step-by-Step Solution:

In Kahn's algorithm, vertices forming a directed cycle always maintain an in-degree $\ge 1$ because every node in the cycle has an incoming edge from another node in the cycle. Thus, no node in a cycle can ever reach an in-degree of 0 or enter the queue. If the output count is less than $|V|$, a directed cycle exists.

**Answer:** **(B)**.
