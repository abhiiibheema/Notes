CHAPTER 8: ALL-PAIRS SHORTEST PATH (FLOYD-WARSHALL ALGORITHM)

---

1. PROBLEM FORMULATION & PARADIGM COMPARISON

The **All-Pairs Shortest Path (APSP)** problem requires finding the shortest path distance between **every pair of vertices** $(i, j)$ in a weighted directed or undirected graph $G = (V, E)$ with $|V| = n$ vertices.

```
                       All-Pairs Shortest Path (APSP)
                                     │
         ┌───────────────────────────┼───────────────────────────┐
         ▼                           ▼                           ▼
  Dijkstra × |V|              Bellman-Ford × |V|           Floyd-Warshall (DP)
 - O(|V| · |E| log |V|)       - O(|V|² · |E|)             - O(|V|³) Time
 - Non-negative weights only  - Handles negative weights  - Handles negative weights
 - Fails on negative edges    - Slow for dense graphs     - O(|V|²) Space
```

---

Comparison of APSP Strategies

Instead of designing a dedicated algorithm, one could repeatedly execute a Single-Source Shortest Path (SSSP) algorithm for each of the $|V|$ vertices as the source:

1. **Running Dijkstra's Algorithm** $|V|$ **Times:**
    - **Time Complexity:** $|V| \times O(|E| \log_2 |V|) = \mathbf{O(|V| \cdot |E| \log_2 |V|)}$.
    - **Limitation:** Fails if the graph contains **any negative edge weight**.
2. **Running Bellman-Ford Algorithm** $|V|$ **Times:**
    - **Time Complexity:** $|V| \times O(|V| \cdot |E|) = \mathbf{O(|V|^2 \cdot |E|)}$.
    - **Dense Graph Cost:** For a dense graph where $|E| \approx |V|^2$, this requires $O(|V|^4)$ time.
3. **Floyd-Warshall Dynamic Programming Algorithm:**
    - **Time Complexity:** $\mathbf{O(|V|^3)}$ for all graph types (sparse or dense).
    - **Capabilities:** Handles **negative edge weights** and detects **negative-weight cycles**.

---

2. DYNAMIC PROGRAMMING FORMULATION

Floyd-Warshall uses a **bottom-up Dynamic Programming** formulation based on the concept of **intermediate vertices**.

The Intermediate Vertex Concept

Let the vertices of graph $G$ be numbered $V = \{1, 2, \dots, n\}$. For any pair of vertices $i$ and $j$, an **intermediate vertex** on a path $p = \langle i, v_1, v_2, \dots, v_k, j \rangle$ is any vertex in the path other than the start vertex $i$ and end vertex $j$.

```
          Path i ──► j through intermediate subset {1, 2, ..., k}:
          [ Vertex i ] ──► (Intermediate Vertices ∈ {1...k}) ──► [ Vertex j ]
```

---

State Definition & Base Case

Let $A^k[i][j]$ (or $D^{(k)}[i][j]$) represent the **length of the shortest path** from vertex $i$ to vertex $j$ such that **all intermediate vertices belong to the subset** $\{1, 2, \dots, k\}$.

Base Case ($k = 0$):

When $k = 0$, no intermediate vertices are allowed. The shortest path from $i$ to $j$ is simply the direct edge cost given by the initial **Cost Adjacency Matrix** $C[i][j]$:

$$A^0[i][j] = C[i][j] = \begin{cases} 0 & \text{if } i = j \\ w(i, j) & \text{if } (i, j) \in E \\ \infty & \text{if } (i, j) \notin E \end{cases}$$

---

The Fundamental Recurrence Relation

When considering vertex $k$ as a potential intermediate vertex for the path from $i$ to $j$, we evaluate two mutually exclusive choices:

1. **Choice 1 (Exclude vertex** $k$**):** Vertex $k$ is not an intermediate node on the shortest path. The shortest path length remains $A^{k-1}[i][j]$.
2. **Choice 2 (Include vertex** $k$**):** Vertex $k$ is an intermediate node. The path is split into two sub-paths: from $i$ to $k$ and from $k$ to $j$, using intermediate vertices from $\{1, 2, \dots, k-1\}$. The combined cost is $A^{k-1}[i][k] + A^{k-1}[k][j]$.

$$A^k[i][j] = \min \Big\{ A^{k-1}[i][j], \;\; A^{k-1}[i][k] + A^{k-1}[k][j] \Big\}$$

for all $1 \le k \le n$, $1 \le i \le n$, and $1 \le j \le n$.

```
                      Floyd-Warshall Recurrence Transition
                                        i ──► j
                                       /       \
                                      /         \
                                     ▼           ▼
                               A^(k-1)[i][k] + A^(k-1)[k][j]
```

---

3. ALGORITHM IMPLEMENTATION & MATRIX TRANSITIONS

```
Algo FloydWarshall(G, C, n) {
    // Step 1: Initialize Distance Matrix A^0 using Cost Matrix C
    int A[n + 1][n + 1];
    
    for (i = 1; i <= n; i++) {
        for (j = 1; j <= n; j++) {
            A[i][j] = C[i][j];
        }
    }
    
    // Step 2: Triply nested loops to evaluate intermediate vertices k
    for (k = 1; k <= n; k++) {          // Outer loop: Intermediate vertex k
        for (i = 1; i <= n; i++) {      // Middle loop: Source vertex i
            for (j = 1; j <= n; j++) {  // Inner loop: Destination vertex j
                
                // Relaxation using intermediate vertex k
                if (A[i][k] != ∞ && A[k][j] != ∞) {
                    A[i][j] = Min(A[i][j], A[i][k] + A[k][j]);
                }
                
            }
        }
    }
    
    // Step 3: Check for Negative-Weight Cycles
    for (i = 1; i <= n; i++) {
        if (A[i][i] < 0) {
            Print("Graph contains a Negative-Weight Cycle!");
            return NULL;
        }
    }
    
    return A; // Final APSP Distance Matrix A^n
}
```

🚩 **CRITICAL IMPLEMENTATION RULE:** The intermediate vertex loop **k** **MUST be the OUTMOST loop**. Placing `k` inside the `i` or `j` loops breaks the dynamic programming state updates, yielding incorrect shortest path calculations.

---

4. COMPLEXITY ANALYSIS & NEGATIVE CYCLE DETECTION

5. **Time Complexity:**
    - **Matrix Initialization:** $O(n^2)$ time.
    - **Triply Nested Loops:** Outer loop $k$ ($n$ steps) $\times$ Middle loop $i$ ($n$ steps) $\times$ Inner loop $j$ ($n$ steps) $\implies O(n^3)$.
    - **Total Time Complexity:** $O(n^2 + n^3) = \mathbf{O(n^3)}$ or $\mathbf{O(|V|^3)}$.
6. **Auxiliary Space Complexity:**
    - Requires storing the $n \times n$ distance matrix $A$.
    - **Auxiliary Space Complexity:** $\mathbf{O(n^2)}$ or $\mathbf{O(|V|^2)}$.
7. **Negative-Weight Cycle Detection Rule:**
    - Distance from any vertex $i$ to itself is initially $A^0[i][i] = 0$.
    - If a negative-weight cycle exists, traversing it reduces the path distance from $i$ back to $i$ below zero.
    - **Rule:** If any diagonal entry $A[i][i] < 0$ after running Floyd-Warshall, the graph contains a **negative-weight cycle**.

---

5. COMPARATIVE SUMMARY MATRIX

|Parameter / Feature|SSSP (Dijkstra)|SSSP (Bellman-Ford)|APSP (Floyd-Warshall)|
|---|---|---|---|
|**Output Type**|1D Array (Distances from $s$)|1D Array (Distances from $s$)|**2D Matrix (Distances for all pairs)**|
|**Algorithmic Paradigm**|**Greedy Method**|**Dynamic Programming**|**Dynamic Programming**|
|**Negative Edges Allowed?**|No (Fails)|**Yes**|**Yes**|
|**Time Complexity**|\(O(|E|\log|
|**Space Complexity**|\(O(|V|+|

---

6. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Full Step-by-Step Matrix Trace (NAT)

Consider a directed graph with 3 vertices $V = \{1, 2, 3\}$ defined by the initial Cost Adjacency Matrix $C$:

$$C = \begin{bmatrix} 0 & 4 & 11 \\ 6 & 0 & 2 \\ 3 & \infty & 0 \end{bmatrix}$$

Trace the Floyd-Warshall algorithm for matrices $A^0, A^1, A^2, A^3$ and state the final APSP matrix.

Step-by-Step Solution:

1. **Initial Matrix** $A^0$ **(**$k = 0$**):** $$A^0 = \begin{bmatrix} 0 & 4 & 11 \\ 6 & 0 & 2 \\ 3 & \infty & 0 \end{bmatrix}$$
2. **Iteration 1 (**$k = 1$ **— Allow vertex 1 as intermediate):**
    - Formula: $A^1[i][j] = \min(A^0[i][j], \; A^0[i]$1$ + A^0$1$[j])$.
    - $A^1$2$ = \min(0, 6 + 4) = 0$
    - $A^1$22$ = \min(2, 6 + 11) = 2$
    - $A^1$33$ = \min(\infty, 3 + 4) = \mathbf{7}$
    - $A^1$3$ = \min(0, 3 + 11) = 0$ $$A^1 = \begin{bmatrix} 0 & 4 & 11 \\ 6 & 0 & 2 \\ 3 & \mathbf{7} & 0 \end{bmatrix}$$
3. **Iteration 2 (**$k = 2$ **— Allow vertex 2 as intermediate):**
    - Formula: $A^2[i][j] = \min(A^1[i][j], \; A^1[i]$2$ + A^1$2$[j])$.
    - $A^2$11$ = \min(11, A^1$11$ + A^1$22$) = \min(11, 4 + 2) = \mathbf{6}$
    - $A^2$33$ = \min(3, 7 + 6) = 3$
    - $A^2$3$ = \min(0, 7 + 2) = 0$ $$A^2 = \begin{bmatrix} 0 & 4 & \mathbf{6} \\ 6 & 0 & 2 \\ 3 & 7 & 0 \end{bmatrix}$$
4. **Iteration 3 (**$k = 3$ **— Allow vertex 3 as intermediate):**
    - Formula: $A^3[i][j] = \min(A^2[i][j], \; A^2[i]$3$ + A^2$3$[j])$.
    - $A^3$1$ = \min(0, 6 + 3) = 0$
    - $A^3$11$ = \min(4, 6 + 7) = 4$
    - $A^3$22$ = \min(6, 2 + 3) = \mathbf{5}$
    - $A^3$2$ = \min(0, 2 + 7) = 0$ $$A^3 = \begin{bmatrix} 0 & 4 & 6 \\ \mathbf{5} & 0 & 2 \\ 3 & 7 & 0 \end{bmatrix}$$

**Answer:** Final All-Pairs Shortest Path Matrix: $$A^3 = \begin{bmatrix} 0 & 4 & 6 \\ 5 & 0 & 2 \\ 3 & 7 & 0 \end{bmatrix}$$

---

Problem 2: Loop Order Reversal Violation (MCQ)

What happens if the outer loop `for (k = 1; k <= n; k++)` in Floyd-Warshall is moved to the innermost position `for (i... for (j... for (k...)))`?

- (A) The algorithm runs faster with time complexity $O(n^2)$.
- (B) The algorithm calculates shortest paths containing at most 1 intermediate vertex, failing for longer paths.
- (C) The algorithm produces a syntax error.
- (D) The space complexity increases to $O(n^3)$.

Step-by-Step Solution:

Placing the `k` loop innermost evaluates all intermediate nodes for a fixed pair $(i, j)$ using only the initial direct edge distances $A^0$. It fails to build upon paths containing 2 or more intermediate vertices, violating the dynamic programming dependency structure.

**Answer:** **(B)**.

---

Problem 3: Complete Graph Time Complexity Comparison (GATE PYQ)

Given a dense graph with $n$ vertices and $e = \Theta(n^2)$ edges with positive weights:

1. What is the time complexity of Floyd-Warshall?
2. What is the time complexity of running Dijkstra's algorithm $n$ times using a Binary Heap?

Step-by-Step Solution:

1. **Floyd-Warshall:** Runs in $O(n^3)$ regardless of edge density.
2. **Dijkstra** $n$ **times:** $n \times O(e \log n) = n \times O(n^2 \log n) = \mathbf{O(n^3 \log n)}$.
3. **Conclusion:** On dense graphs, Floyd-Warshall ($O(n^3)$) is asymptotically faster than running Heap-Dijkstra $n$ times ($O(n^3 \log n)$).

**Answer:**

1. Floyd-Warshall = $O(n^3)$.
2. $n$ $\times$ Dijkstra = $O(n^3 \log n)$.

---

Problem 4: Negative Cycle Detection via Matrix Diagonal (MCQ)

After executing the Floyd-Warshall algorithm on a graph, the final distance matrix $A$ contains $A$2$ = -3$. What does this indicate?

- (A) Vertex 2 is unreachable from source node 1.
- (B) The graph contains a negative-weight cycle passing through vertex 2.
- (C) Shortest distance from vertex 2 to all nodes is -3.
- (D) The algorithm encountered an overflow error.

Step-by-Step Solution:

Distance from a vertex to itself is initialized to 0. A diagonal entry $A[i][i] < 0$ means there exists a path starting and ending at vertex $i$ with a negative sum, which directly identifies a **negative-weight cycle**.

**Answer:** **(B)**.