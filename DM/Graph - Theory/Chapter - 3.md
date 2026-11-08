**Graph Theory: Group 3 — Connectivity, Paths & Cut-Structures**

---

**Traversals: Walks, Trails, Paths & Circuits**

In graph theory, traversing a graph involves moving along vertices and edges according to specific structural rules.

- **Walk**: An alternating sequence of vertices and edges $v_0, e_1, v_1, e_2, v_2, \dots, e_k, v_k$.
    - **Repeated Vertices**: Permitted.
    - **Repeated Edges**: Permitted.
- **Trail**: A walk in which **no edge is repeated**.
    - **Repeated Vertices**: Permitted.
    - **Repeated Edges**: Strictly Forbidden.
- **Path**: A walk in which **neither vertices nor edges are repeated** (except possibly the start and end vertex in a closed path).
    - **Repeated Vertices**: Strictly Forbidden.
    - **Repeated Edges**: Strictly Forbidden.
- **Closed Walk**: A walk that starts and ends at the exact same vertex ($v_0 = v_k$).
- **Circuit (Closed Trail)**: A closed walk with **no repeated edges**.
- **Cycle (Closed Path)**: A closed walk with **no repeated vertices** (except the start and end vertex).

```
   Walk:  1 -> 2 -> 3 -> 2 -> 4       (Vertices & Edges can repeat)
   Trail: 1 -> 2 -> 3 -> 4 -> 2 -> 5  (Vertices can repeat, Edges cannot)
   Path:  1 -> 2 -> 3 -> 4 -> 5       (No Vertices or Edges can repeat)
```

**Classification Matrix of Traversals**

|Traversal Type|Repeated Vertices Allowed|Repeated Edges Allowed|Start == End Vertex|
|---|---|---|---|
|**Walk**|Yes|Yes|Open or Closed|
|**Trail**|Yes|No|Open|
|**Path**|No|No|Open|
|**Closed Walk**|Yes|Yes|Yes|
|**Circuit**|Yes|No|Yes|
|**Cycle**|No (except start/end)|No|Yes|

---

**Connected Graphs & Components**

- **Connected Graph (**$k = 1$**)**: A graph in which a path exists between **every pair of vertices**. The number of connected components is $k = 1$.
- **Disconnected Graph (**$k \ge 2$**)**: A graph in which there exists at least one pair of vertices with no connecting path. The graph is split into $k$ disjoint connected subgraphs called **connected components**.

```
    Connected Graph (k = 1)             Disconnected Graph (k = 2)
       (1)-------(2)                      (1)-------(2)       (4)
        |         |                        |         |         |
        |         |                        |         |         |
       (3)-------(4)                      (3)-------(5)       (6)
```

**Complement Graph Connectivity Theorems**

- **Theorem 1**: If a graph $G$ is **disconnected** ($k \ge 2$), then its complement $\overline{G}$ is **always connected**.
- **Theorem 2**: If a graph $G$ is **connected**, its complement $\overline{G}$ **may be connected or disconnected**.

**Degree Bounds for Guaranteed Connectivity**

- **Theorem 3**: For any simple graph $G$ with $n$ vertices, if the minimum degree satisfies: $$\delta(G) \ge \frac{n - 1}{2}$$ then $G$ is **guaranteed to be connected**.

**Theorem on Odd-Degree Vertices**

- **Theorem 4**: If a graph $G$ contains **exactly two vertices of odd degree**, then there **must exist a path** connecting those two odd-degree vertices.

**Edge Bounds for Connected and Disconnected Graphs**

For a simple graph $G$ with $n$ vertices:

- **Edge Range for Connected Graph (**$k = 1$**)**: $$n - 1 \le e \le \frac{n(n - 1)}{2}$$
- **Edge Range for Disconnected Graph with** $k$ **Components (**$k \ge 2$**)**: $$n - k \le e \le \frac{(n - k)(n - k + 1)}{2}$$

---

**Cut-Edges (Bridges) & Edge Connectivity**

- **Cut-Edge (Bridge)**: An edge whose removal increases the number of connected components of the graph (turning a connected graph into a disconnected graph).
- **Cycle Rule for Cut-Edges**: An edge $e$ is a cut-edge **if and only if** it does **not belong to any cycle** in the graph.
- **Cut-Edge Set**: A set of edges whose removal from $G$ disconnects the graph.
- **Edge Connectivity (**$\lambda(G)$**)**: The **minimum number of edges** whose removal disconnects the graph $G$ (or leaves a single isolated vertex).

```
          Triangle 1          Bridge          Triangle 2
            (1)---(2)                         (4)---(5)
             \   /            (3-4)            \   /
              (3)===============================(6)
                          Cut-Edge (Bridge)
```

**Key Properties of Edge Connectivity**

- If a graph has a cut-edge, its edge connectivity is $\lambda(G) = 1$.
- For a disconnected graph, $\lambda(G) = 0$.

---

**Cut-Vertices (Articulation Points) & Vertex Connectivity**

- **Cut-Vertex (Articulation Point)**: A vertex whose removal (along with all edges incident on it) increases the number of connected components of the graph.
- **Cut-Vertex Set**: A set of vertices whose removal disconnects the graph.
- **Vertex Connectivity (**$\kappa(G)$**)**: The **minimum number of vertices** whose removal disconnects the graph $G$ (or leaves a single isolated vertex).

```
         Triangle 1                          Triangle 2
           (1)---(2)                         (4)---(5)
            \   /                             \   /
             (3)-------------------------------(6)
                                Cut-Vertex
```

**Interrelationships Between Cut-Edges and Cut-Vertices**

- If a graph contains a **cut-edge** (and has more than 2 vertices), it **must contain at least one cut-vertex**.
- If a graph contains a **cut-vertex**, it **does not necessarily contain a cut-edge** (e.g., two triangles sharing a single vertex).

---

**Whitney's Theorem on Connectivity**

For any simple graph $G$, the vertex connectivity, edge connectivity, and minimum degree satisfy the fundamental inequality chain:

$$\kappa(G) \le \lambda(G) \le \delta(G)$$

$$\text{Vertex Connectivity } \kappa(G) \le \text{Edge Connectivity } \lambda(G) \le \text{Minimum Degree } \delta(G)$$

```
                         Whitney's Hierarchy
             [ \kappa(G) ]  <=  [ \lambda(G) ]  <=  [ \delta(G) ]
               Vertex               Edge               Minimum
             Connectivity        Connectivity          Degree
```

---

**Connectivity Metrics for Standard Graph Families**

|Graph Family|Notation|Vertex Connectivity $\kappa(G)$|Edge Connectivity $\lambda(G)$|Minimum Degree $\delta(G)$|
|---|---|---|---|---|
|**Complete Graph**|$K_n$|$n - 1$|$n - 1$|$n - 1$|
|**Cycle Graph**|$C_n$|2|2|2|
|**Wheel Graph**|$W_n$|3|3|3|
|**Complete Bipartite Graph**|$K_{m,n}$|$\min\{m, n\}$|$\min\{m, n\}$|$\min\{m, n\}$|
|**Hypercube**|$Q_n$|$n$|$n$|$n$|

---

**Worked Practice Problems**

**Problem: Edge Bounds for Disconnected Graphs**

- **Question**: What is the maximum number of edges in a disconnected graph with $n = 10$ vertices and $k = 3$ components?
- **Solution**:
    - Apply the maximum edge formula for $k$ components: $$e_{\max} = \frac{(n - k)(n - k + 1)}{2}$$
    - Substitute $n = 10$ and $k = 3$: $$e_{\max} = \frac{(10 - 3)(10 - 3 + 1)}{2} = \frac{7 \times 8}{2} = 28$$
- **Result**: **28 edges**.

---

**Problem: Minimum Degree & Guaranteed Connectivity**

- **Question**: What is the smallest minimum degree $\delta(G)$ required to guarantee that a graph with $n = 8$ vertices is connected?
- **Solution**:
    - Apply the connectivity theorem: $$\delta(G) \ge \frac{n - 1}{2}$$
    - Substitute $n = 8$: $$\delta(G) \ge \frac{8 - 1}{2} = \frac{7}{2} = 3.5$$
    - Since degree must be an integer, round up to 4.
- **Result**: Minimum degree $\delta(G) \ge 4$.

---

**Problem: Verifying Whitney's Inequality**

- **Question**: A graph $G$ has 6 vertices and is 3-regular. Determine the possible range of values for its vertex connectivity $\kappa(G)$ and edge connectivity $\lambda(G)$.
- **Solution**:
    - Since $G$ is 3-regular, minimum degree $\delta(G) = 3$.
    - Apply Whitney's Theorem: $$\kappa(G) \le \lambda(G) \le \delta(G) = 3$$
    - Thus, $\lambda(G) \le 3$ and $\kappa(G) \le 3$.
- **Result**: Maximum value for both $\kappa(G)$ and $\lambda(G)$ is **3**.

---
![[Pasted image 20261108235049.png]]