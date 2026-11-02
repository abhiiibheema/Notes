CHAPTER 7: BELLMAN-FORD ALGORITHM (SSSP WITH NEGATIVE WEIGHTS)

---

1. PROBLEM FORMULATION & COMPARISON WITH DIJKSTRA

The **Single-Source Shortest Path (SSSP)** problem requires finding the minimum-cost paths from a designated source vertex $s$ to all other vertices in a weighted graph $G = (V, E)$.

While **Dijkstra's Algorithm** solves SSSP in $O(|E| \log |V|)$ time using a **Greedy strategy**, it strictly fails when graph edges have **negative weights** ($W(e) < 0$). To handle graphs with negative edge weights and detect **negative-weight cycles**, we use the **Bellman-Ford Algorithm**, which is built on **Dynamic Programming**.

```
                        Single-Source Shortest Path (SSSP)
                                       │
              ┌────────────────────────┴────────────────────────┐
              ▼                                                 ▼
Dijkstra's Algorithm (Greedy)                  Bellman-Ford Algorithm (DP)
- Requires non-negative edges (W(e) ≥ 0)      - Allows negative edge weights (W(e) < 0)
- Lock-in (Permanent) vertex extraction       - Systematic |V| - 1 relaxation passes
- Fails on negative edge weights              - Detects negative-weight cycles
- Time Complexity: O(|E| log |V|)             - Time Complexity: O(|V| · |E|)
```

---

Why Greedy Fails & Why Dynamic Programming Works

1. **Greedy Lock-in Failure (Dijkstra):** Dijkstra permanently finalizes a vertex's shortest distance as soon as it is extracted from the priority queue. It assumes that path distances can only increase or stay constant as edges are added. If a negative edge appears later, a previously finalized vertex might actually have a shorter path, but Dijkstra cannot update finalized nodes.
2. **DP Exhaustive Relaxation (Bellman-Ford):** Bellman-Ford does not greedily finalize any vertex prematurely. Instead, it systematically relaxes **all** $|E|$ **edges in the graph across** $|V| - 1$ **iterative passes**, ensuring that distance improvements propagate across all possible path lengths.

---

2. ALGORITHMIC MECHANICS & THE $|V| - 1$ RELAXATION PRINCIPLE

The Simple Path Depth Principle

In any connected graph $G = (V, E)$ with $|V|$ vertices that **does not contain negative-weight cycles**, the shortest simple path between any two reachable vertices contains **at most** $|V| - 1$ **edges**.

```
Path Length Limit:  [ Source s ] ──► (e₁) ──► (e₂) ──► ... ──► (e_|V|-1) ──► [ Target v ]
                    Maximum edges in a simple path = |V| - 1
```

- **Pass 1:** Guarantees correct shortest path distances for all vertices whose shortest path from source $s$ consists of $\le 1$ edge.
- **Pass 2:** Guarantees correct shortest path distances for all vertices whose shortest path from source $s$ consists of $\le 2$ edges.
- ...
- **Pass** $|V| - 1$**:** Guarantees correct shortest path distances for all vertices whose shortest path from source $s$ consists of $\le |V| - 1$ edges.

---

Negative-Weight Cycle Detection Rule

A **Negative-Weight Cycle** is a closed directed loop where the sum of edge weights is strictly negative ($\sum_{e \in C} W(e) < 0$).

```
                 Negative Cycle: (B ──5──► C ──3──► D ──(-10)──► B)
                 Total Cycle Weight = 5 + 3 + (-10) = -2 < 0
                 Path cost decreases endlessly (-∞) by traversing cycle!
```

If a graph contains a negative-weight cycle reachable from source $s$, shortest paths are **undefined** (they can be made infinitely small $-\infty$ by traversing the cycle repeatedly).

The Detection Test (Pass $|V|$)

After completing $|V| - 1$ relaxation passes:

- Perform **one extra (**$|V|$**-th) relaxation pass** over all $|E|$ edges.
- If any distance estimate $d[v]$ can still be reduced ($d[u] + W(u, v) < d[v]$), then a **negative-weight cycle exists in the graph**.

---

3. STEP-BY-STEP PSEUDOCODE IMPLEMENTATION

```
Algo BellmanFord_SSSP(G, W, V, E, s) {
    // Step 1: Initialize single-source distance estimates
    for (each vertex v in V) {
        d[v] = ∞;
        parent[v] = NULL;
    }
    d[s] = 0; // Source distance is 0
    
    // Step 2: Relax ALL |E| edges (|V| - 1) times
    for (i = 1; i <= |V| - 1; i++) {
        updated = False; // Flag to enable early termination optimization
        
        for (each edge (u, v) in E) {
            if (d[u] != ∞ && d[u] + W[u][v] < d[v]) {
                d[v] = d[u] + W[u][v]; // Relax edge
                parent[v] = u;        // Update predecessor pointer
                updated = True;
            }
        }
        
        // Early Exit Optimization: If no edge was updated, converged early!
        if (!updated) {
            break;
        }
    }
    
    // Step 3: Check for Negative-Weight Cycles (Pass |V|)
    for (each edge (u, v) in E) {
        if (d[u] != ∞ && d[u] + W[u][v] < d[v]) {
            Print("Graph contains a Negative-Weight Cycle!");
            return NULL; // Shortest path undefined
        }
    }
    
    return d; // Valid shortest path distances
}
```

---

4. TIME & SPACE COMPLEXITY ANALYSIS

5. **Time Complexity:**
    - **Initialization:** $O(|V|)$ time.
    - **Relaxation Phase:** $|V| - 1$ outer iterations $\times$ $|E|$ inner edge relaxations $\implies \mathbf{O(|V| \cdot |E|)}$.
    - **Negative Cycle Check:** Single pass over $|E|$ edges $\implies O(|E|)$.
    - **Overall Time Complexity:** $\mathbf{O(|V| \cdot |E|)}$ or $\mathbf{O(v \cdot e)}$.
        - **Dense Graphs (**$|E| \approx |V|^2$**):** Runs in $\mathbf{O(|V|^3)}$ time.
        - **Sparse Graphs (**$|E| \approx |V|$**):** Runs in $\mathbf{O(|V|^2)}$ time.
6. **Auxiliary Space Complexity:**
    - Requires a distance array `d` of size $|V|$ and a `parent` array of size $|V|$.
    - **Auxiliary Space Complexity:** $\mathbf{O(|V|)}$.

---

5. COMPARATIVE SUMMARY MATRIX: DIJKSTRA VS. BELLMAN-FORD

|Parameter / Feature|Dijkstra's Algorithm|Bellman-Ford Algorithm|
|---|---|---|
|**Algorithmic Paradigm**|**Greedy Method**|**Dynamic Programming**|
|**Edge Weight Restrictions**|Non-negative weights ($W(e) \ge 0$)|**Allows negative edge weights (**$W(e) < 0$**)**|
|**Negative Cycle Behavior**|Fails / Gives incorrect values|**Detects negative-weight cycles**|
|**Time Complexity**|\(\mathbf{O(|E|
|**Auxiliary Space**|\(O(|V|
|**Graph Application**|Directed & Undirected ($W \ge 0$)|Directed Graphs (Undirected negative edge $\implies$ 2-cycle)|

---

6. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Full Step-by-Step Execution Trace (NAT)

Consider a directed graph $G = (V, E)$ with 5 vertices $V = \{A, B, C, D, E\}$ and source vertex $s = A$. Edge weights are given below:

|Edge|Weight|Edge|Weight|Edge|Weight|
|---|---|---|---|---|---|
|**(A, B)**|-1|**(B, C)**|3|**(C, D)**|-2|
|**(A, C)**|4|**(B, D)**|2|**(D, B)**|1|
|**(B, E)**|2|**(D, C)**|5|**(E, D)**|-3|

Trace the Bellman-Ford algorithm for all $|V| - 1 = 4$ passes and state the final shortest distances from source $A$.

Step-by-Step Solution:

1. **Initialization:** $d[A] = 0, \quad d[B] = \infty, \quad d[C] = \infty, \quad d[D] = \infty, \quad d[E] = \infty$.
2. **Pass 1 (Relax all edges):**
    - Relax $(A, B)$ with weight $-1$: $d[B] = \min(\infty, 0 - 1) = \mathbf{-1}$.
    - Relax $(A, C)$ with weight $4$: $d[C] = \min(\infty, 0 + 4) = \mathbf{4}$.
    - Relax $(B, C)$ with weight $3$: $d[C] = \min(4, -1 + 3) = \mathbf{2}$.
    - Relax $(B, D)$ with weight $2$: $d[D] = \min(\infty, -1 + 2) = \mathbf{1}$.
    - Relax $(B, E)$ with weight $2$: $d[E] = \min(\infty, -1 + 2) = \mathbf{1}$.
    - Relax $(C, D)$ with weight $-2$: $d[D] = \min(1, 2 - 2) = \mathbf{0}$.
    - Relax $(E, D)$ with weight $-3$: $d[D] = \min(0, 1 - 3) = \mathbf{-2}$.
    - **Distances after Pass 1:** $d = [A:0, \, B:-1, \, C:2, \, D:-2, \, E:1]$.
3. **Pass 2 (Relax all edges):**
    - Relax $(B, C)$: $d[C] = \min(2, -1 + 3) = 2$ (no change).
    - Relax $(C, D)$: $d[D] = \min(-2, 2 - 2) = -2$ (no change).
    - Relax $(E, D)$: $d[D] = \min(-2, 1 - 3) = -2$ (no change).
    - **No distance updates occur in Pass 2!**
4. **Early Exit Optimization:** Since zero distance updates occurred in Pass 2, the algorithm terminates early!
5. **Pass 5 (Negative Cycle Check):** Testing all edges confirms $d[u] + W(u, v) \ge d[v]$ for all edges $\implies$ **No Negative Cycle Exists**.

**Answer:** Final Shortest Distances: $d[A] = 0, \, d[B] = -1, \, d[C] = 2, \, d[D] = -2, \, d[E] = 1$.

---

Problem 2: Negative-Weight Cycle Detection Trace (NAT / MCQ)

Consider a directed graph with 4 vertices $\{A, B, C, D\}$ and directed edges:

- $(A, B) = 4$, $(B, C) = 2$, $(C, D) = -8$, $(D, B) = 3$.

If Bellman-Ford is executed starting from source $A$:

1. Does the graph contain a negative-weight cycle?
2. What happens during the 4th relaxation pass?

Step-by-Step Solution:

1. **Identify the Cycle:** The cycle is $B \to C \to D \to B$. Total Cycle Weight $= W(B, C) + W(C, D) + W(D, B) = 2 + (-8) + 3 = \mathbf{-3 < 0}$.
2. **Cycle Impact:** Since the cycle sum is negative, traversing it infinitely reduces path costs without bound.
3. **Pass 4 (Negative Cycle Check):** When checking edge $(D, B)$ with weight $3$: Since $d[D] + 3 < d[B]$, the inequality holds, triggering the **Negative-Weight Cycle Alert**.

**Answer:**

1. **YES**, a negative cycle $B \to C \to D \to B$ exists with weight **-3**.
2. Pass 4 detects $d[D] + W(D, B) < d[B]$, causing the algorithm to report a **Negative-Weight Cycle**.

---

Problem 3: Early Exit Optimization (MCQ)

In the worst case, Bellman-Ford requires $|V| - 1$ passes. What is the minimum number of passes required if the edges are processed in topological order on a Directed Acyclic Graph (DAG)?

- (A) $|V| - 1$ passes
- (B) **1 pass**
- (C) $|E|$ passes
- (D) $\log_2 |V|$ passes

Step-by-Step Solution:

If edges are processed in topological sort order, the shortest path distances to all vertices propagate completely in **a single pass** (Pass 1). The algorithm detects no updates in Pass 2 and terminates immediately.

**Answer:** **(B)**.

---

Problem 4: Complexity on Dense Complete Graphs (MCQ)

What is the time complexity of the Bellman-Ford algorithm when executed on a complete directed graph $K_n$ with $n$ vertices?

- (A) $O(n^2)$
- (B) $\mathbf{O(n^3)}$
- (C) $O(n \log n)$
- (D) $O(n^4)$

Step-by-Step Solution:

For a complete directed graph $K_n$, $|V| = n$ and total edges $|E| = n(n - 1) = \Theta(n^2)$. Substituting $|V| = n$ and $|E| = n^2$ into $O(|V| \cdot |E|)$: $$\text{Time Complexity} = O(n \cdot n^2) = \mathbf{O(n^3)}$$

**Answer:** **(B)**.