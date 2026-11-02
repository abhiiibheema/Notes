CHAPTER 6: MULTISTAGE GRAPHS & SHORTEST PATH PIPELINE

---

1. PROBLEM DEFINITION & NETWORK TOPOLOGY

A **Multistage Graph** is a directed, weighted, acyclic graph (DAG) where the set of vertices $V$ is partitioned into $k \ge 2$ disjoint sets or stages:

$$V = V_1 \cup V_2 \cup \dots \cup V_k$$

where:

1. $V_1 = \{s\}$ contains a single **source vertex** $s$.
2. $V_k = \{t\}$ contains a single **target / destination vertex** $t$.
3. Every edge $e = (u, v) \in E$ goes strictly from a vertex $u$ in stage $V_i$ to a vertex $v$ in the immediate next stage $V_{i+1}$ (for $1 \le i < k$).

```
Stage 1 (V₁)       Stage 2 (V₂)       Stage 3 (V₃)      ...     Stage k (Vₖ)
  [ Source s ] ──► [ Vertices ]  ──► [ Vertices ]  ──►  ... ──► [ Destination t ]
```

Key Topological Constraints

- **No Intra-stage Edges:** No edge connects two vertices within the same stage $V_i$.
- **No Backward or Cross-stage Edges:** No edge skips stages (e.g., from $V_i$ to $V_{i+2}$) or points backward (from $V_i$ to $V_{i-1}$).
- **Acyclic Structure:** Because edges move strictly forward across sequential stages, the graph contains **zero cycles**.

---

2. DYNAMIC PROGRAMMING FORMULATION

Multistage graph shortest path problems can be solved systematically using two dynamic programming paradigms: the **Backward Approach** (Destination-to-Source) and the **Forward Approach** (Source-to-Destination).

---

Approach A: The Backward Approach (Destination to Source)

In the **Backward Approach**, computations begin at the destination vertex $t$ in stage $k$ and move backward level-by-level toward the source $s$ in stage 1.

1. State Definition

Let $cost(i, j)$ represent the **minimum cost of a path** from vertex $j$ in stage $i$ to the destination vertex $t$ in stage $k$.

2. Base Case

$$cost(k, t) = 0$$

3. Fundamental Recurrence Relation

For any vertex $j \in V_i$ (where $1 \le i < k$):

$$cost(i, j) = \min_{r \in V_{i+1}, \, (j, r) \in E} \Big\{ c(j, r) + cost(i+1, r) \Big\}$$

where $c(j, r)$ is the weight of the edge from vertex $j$ in stage $i$ to vertex $r$ in stage $i+1$.

4. Decision Tracker Array

To reconstruct the optimal path, we maintain an array $D(i, j)$ storing the neighbor $r^* \in V_{i+1}$ that achieves the minimum cost:

$$D(i, j) = \text{arg min}_{r \in V_{i+1}, \, (j, r) \in E} \Big\{ c(j, r) + cost(i+1, r) \Big\}$$

---

Approach B: The Forward Approach (Source to Destination)

In the **Forward Approach**, computations begin at the source vertex $s$ in stage 1 and move forward level-by-level toward the destination $t$ in stage $k$.

1. State Definition

Let $d(i, j)$ represent the **minimum cost of a path** from the source vertex $s$ in stage 1 to vertex $j$ in stage $i$.

2. Base Case

$$d(1, s) = 0$$

3. Fundamental Recurrence Relation

For any vertex $j \in V_i$ (where $1 < i \le k$):

$$d(i, j) = \min_{r \in V_{i-1}, \, (r, j) \in E} \Big\{ d(i-1, r) + c(r, j) \Big\}$$

---

3. ALGORITHM IMPLEMENTATION & PATH RECONSTRUCTION

```
Algo MultistageGraph_Backward(G, c, k, n) {
    // Step 1: Initialize cost and decision arrays
    int cost[n + 1];
    int D[n + 1];
    
    // Base Case at Destination
    cost[n] = 0; // Destination vertex n in Stage k
    
    // Step 2: Compute costs backward from vertex (n - 1) down to 1
    for (j = n - 1; j >= 1; j--) {
        // Find neighbor r in next stage that minimizes c(j, r) + cost[r]
        min_cost = ∞;
        best_neighbor = -1;
        
        for (each neighbor r adjacent to j) {
            if (c[j][r] + cost[r] < min_cost) {
                min_cost = c[j][r] + cost[r];
                best_neighbor = r;
            }
        }
        
        cost[j] = min_cost;
        D[j] = best_neighbor;
    }
    
    // Step 3: Reconstruct Optimal Path from Source (vertex 1)
    path[1] = 1;
    for (j = 2; j <= k; j++) {
        path[j] = D[path[j - 1]];
    }
    
    return cost[1]; // Minimum cost from Source to Destination
}
```

---

4. COMPLEXITY ANALYSIS

5. **Time Complexity:**
    - Each vertex $u \in V$ processes all of its outgoing edges $(u, v) \in E$.
    - Summing across all vertices and edges, every edge is examined exactly once.
    - Total Running Time: $\mathbf{O(|V| + |E|)}$.
    - Since $|E| \ge |V| - 1$ in a connected multistage graph, this simplifies to $\mathbf{O(|E|)}$.
6. **Auxiliary Space Complexity:**
    - Storing the $cost[\cdot]$ array and decision $D[\cdot]$ array for $n = |V|$ vertices requires $\mathbf{O(|V|)}$ memory.

---

5. COMPARATIVE SUMMARY MATRIX

|Parameter / Feature|Multistage Graph DP|Dijkstra's Algorithm|Bellman-Ford Algorithm|
|---|---|---|---|
|**Graph Topology Constraint**|**Strict Stage-based DAG**|General Weighted Graph|General Weighted Graph|
|**Edge Weight Restriction**|Negative weights allowed (no cycles)|**Non-negative weights only**|Negative weights allowed|
|**Time Complexity**|\(\mathbf{O(|V|+|
|**Computation Direction**|Backward or Forward across stages|Greedy vertex extraction|All-edge relaxation passes|
|**Space Complexity**|\(O(|V|)\)|

---

6. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Full Backward DP Trace & Path Reconstruction (NAT)

Consider a 4-stage multistage graph with 8 vertices $V = \{1, 2, 3, 4, 5, 6, 7, 8\}$ structured as follows:

- **Stage 1 (**$V_1$**):** $\{1\}$ (Source)
- **Stage 2 (**$V_2$**):** $\{2, 3, 4\}$
- **Stage 3 (**$V_3$**):** $\{5, 6, 7\}$
- **Stage 4 (**$V_4$**):** $\{8\}$ (Destination)

Edge weights $c(u, v)$ are given below:

- From Stage 1 to Stage 2: $c(1, 2) = 2, \, c(1, 3) = 1, \, c(1, 4) = 5$
- From Stage 2 to Stage 3:
    - $c(2, 5) = 4, \, c(2, 6) = 11$
    - $c(3, 5) = 9, \, c(3, 6) = 5, \, c(3, 7) = 16$
    - $c(4, 6) = 2, \, c(4, 7) = 7$
- From Stage 3 to Stage 4: $c(5, 8) = 18, \, c(6, 8) = 13, \, c(7, 8) = 2$

Find the minimum cost path from source $1$ to destination $8$ using the **Backward Approach**, and reconstruct the optimal vertex path.

Step-by-Step Solution:

1. **Stage 4 Base Case (**$V_4 = \{8\}$**):** $$cost(4, 8) = 0$$
2. **Stage 3 Computation (**$V_3 = \{5, 6, 7\}$**):**
    - **Vertex 5:** $cost(3, 5) = c(5, 8) + cost(4, 8) = 18 + 0 = \mathbf{18}, \quad D(3, 5) = 8$
    - **Vertex 6:** $cost(3, 6) = c(6, 8) + cost(4, 8) = 13 + 0 = \mathbf{13}, \quad D(3, 6) = 8$
    - **Vertex 7:** $cost(3, 7) = c(7, 8) + cost(4, 8) = 2 + 0 = \mathbf{2}, \quad D(3, 7) = 8$
3. **Stage 2 Computation (**$V_2 = \{2, 3, 4\}$**):**
    - **Vertex 2:** Outgoing edges to $\{5, 6\}$
        - Path through 5: $c(2, 5) + cost(3, 5) = 4 + 18 = 22$
        - Path through 6: $c(2, 6) + cost(3, 6) = 11 + 13 = 24$
        - $cost(2, 2) = \min(22, 24) = \mathbf{22}, \quad D(2, 2) = 5$
    - **Vertex 3:** Outgoing edges to $\{5, 6, 7\}$
        - Path through 5: $c(3, 5) + cost(3, 5) = 9 + 18 = 27$
        - Path through 6: $c(3, 6) + cost(3, 6) = 5 + 13 = 18$
        - Path through 7: $c(3, 7) + cost(3, 7) = 16 + 2 = 18$
        - $cost(2, 3) = \min(27, 18, 18) = \mathbf{18}, \quad D(2, 3) = 6 \text{ (or } 7)$
    - **Vertex 4:** Outgoing edges to $\{6, 7\}$
        - Path through 6: $c(4, 6) + cost(3, 6) = 2 + 13 = 15$
        - Path through 7: $c(4, 7) + cost(3, 7) = 7 + 2 = 9$
        - $cost(2, 4) = \min(15, 9) = \mathbf{9}, \quad D(2, 4) = 7$
4. **Stage 1 Computation (**$V_1 = \{1\}$**):**
    - **Vertex 1:** Outgoing edges to $\{2, 3, 4\}$
        - Path through 2: $c(1, 2) + cost(2, 2) = 2 + 22 = 24$
        - Path through 3: $c(1, 3) + cost(2, 3) = 1 + 18 = 19$
        - Path through 4: $c(1, 4) + cost(2, 4) = 5 + 9 = 14$
        - $cost(1, 1) = \min(24, 19, 14) = \mathbf{14}, \quad D(1, 1) = 4$
5. **Path Reconstruction:**
    - Start at $V_1$: **Vertex 1**
    - Next hop: $D(1, 1) = \mathbf{4}$
    - Next hop: $D(2, 4) = \mathbf{7}$
    - Next hop: $D(3, 7) = \mathbf{8}$

**Answer:**

1. Minimum Cost = **14**.
2. Optimal Path = $1 \to 4 \to 7 \to 8$.

---

Problem 2: Forward Approach Trace Verification (NAT)

Verify the minimum cost for the same 4-stage network using the **Forward Approach**.

Step-by-Step Solution:

1. **Stage 1 Base Case (**$V_1 = \{1\}$**):** $$d(1, 1) = 0$$
2. **Stage 2 Computation (**$V_2 = \{2, 3, 4\}$**):**
    - $d(2, 2) = d(1, 1) + c(1, 2) = 0 + 2 = \mathbf{2}$
    - $d(2, 3) = d(1, 1) + c(1, 3) = 0 + 1 = \mathbf{1}$
    - $d(2, 4) = d(1, 1) + c(1, 4) = 0 + 5 = \mathbf{5}$
3. **Stage 3 Computation (**$V_3 = \{5, 6, 7\}$**):**
    - **Vertex 5:** incoming from $\{2, 3\}$
        - $d(2, 2) + c(2, 5) = 2 + 4 = 6$
        - $d(2, 3) + c(3, 5) = 1 + 9 = 10$
        - $d(3, 5) = \min(6, 10) = \mathbf{6}$
    - **Vertex 6:** incoming from $\{2, 3, 4\}$
        - $d(2, 2) + c(2, 6) = 2 + 11 = 13$
        - $d(2, 3) + c(3, 6) = 1 + 5 = 6$
        - $d(2, 4) + c(4, 6) = 5 + 2 = 7$
        - $d(3, 6) = \min(13, 6, 7) = \mathbf{6}$
    - **Vertex 7:** incoming from $\{3, 4\}$
        - $d(2, 3) + c(3, 7) = 1 + 16 = 17$
        - $d(2, 4) + c(4, 7) = 5 + 7 = 12$
        - $d(3, 7) = \min(17, 12) = \mathbf{12}$
4. **Stage 4 Computation (**$V_4 = \{8\}$**):**
    - Incoming from $\{5, 6, 7\}$:
        - $d(3, 5) + c(5, 8) = 6 + 18 = 24$
        - $d(3, 6) + c(6, 8) = 6 + 13 = 19$
        - $d(3, 7) + c(7, 8) = 12 + 2 = \mathbf{14}$
        - $d(4, 8) = \min(24, 19, 14) = \mathbf{14}$

**Answer:** Forward Approach yields exact same minimum cost = **14**.

---

Problem 3: Multistage Graph Complexity (MCQ)

What is the time complexity of finding the shortest path in a multistage graph with $|V|$ vertices and $|E|$ edges using Dynamic Programming?

- (A) $O(|V|^2)$
- (B) $O(|E| \log |V|)$
- (C) $O(|V| + |E|)$
- (D) $O(|V|^3)$

Step-by-Step Solution:

In a multistage graph, each edge is evaluated exactly once when transitioning between adjacent stages. The total work performed across all stages is proportional to the number of vertices and edges, yielding $O(|V| + |E|)$ time.

**Answer:** **(C)**.

---