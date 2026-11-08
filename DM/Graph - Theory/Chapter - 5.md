**Graph Theory: Group 5 — Graph Coloring, Matching & Planarity**

---

**Vertex Coloring & Chromatic Number**

- **Proper Vertex Coloring**: An assignment of colors to the vertices of a graph such that no two adjacent vertices share the same color.
- **Chromatic Number (**$\chi(G)$**)**: The minimum number of colors required to achieve a proper vertex coloring of graph $G$.
- $k$**-Chromatic Graph**: A graph $G$ is called $k$-chromatic if its chromatic number is $\chi(G) = k$.

```
   Proper 2-Coloring of C_4                  3-Coloring of C_3 (K_3)
          (Color 1)                               (Color 1)
           o-------o (Color 2)                       o
           |       |                                / \
           |       |                               /   \
 (Color 2) o-------o (Color 1)         (Color 2) o-----o (Color 3)
```

---

**Chromatic Numbers for Standard Graph Families**

- **Complete Graph (**$K_n$**)**: Every vertex is adjacent to all other vertices. $$\chi(K_n) = n$$
- **Cycle Graph (**$C_n$**)**:
    - If $n$ is **even**: $\chi(C_n) = 2$
    - If $n$ is **odd**: $\chi(C_n) = 3$
- **Wheel Graph (**$W_n$**)**: Formed by a central hub connected to cycle $C_{n-1}$ (where $n$ is total vertices):
    - If $n$ is **odd** (outer cycle $C_{n-1}$ is even): $\chi(W_n) = 3$
    - If $n$ is **even** (outer cycle $C_{n-1}$ is odd): $\chi(W_n) = 4$
- **Bipartite Graphs & Trees**:
    - Any non-trivial tree or bipartite graph $K_{m,n}$ is 2-chromatic: $\chi(G) = 2$.
- **Hypercube Graph (**$Q_n$**)**:
    - Since every hypercube is bipartite: $\chi(Q_n) = 2$.

---

**Independence Sets & Chromatic Bounds**

- **Independence Set**: A subset of vertices in $G$ such that no two vertices in the subset are adjacent.
- **Maximal Independence Set (MIS)**: An independence set to which no additional vertex from the graph can be added without violating the non-adjacency condition.
- **Independence Number (**$\beta(G)$**)**: The total number of vertices present in the **largest maximal independence set**.
- **Fundamental Inequality**: The chromatic number, vertex count $n$, and independence number $\beta(G)$ satisfy: $$\chi(G) \ge \frac{n}{\beta(G)}$$

```
                Graph Independence Set Example
               (a)-------(b)       Independent Set: {a, d, f}
               / \       / \       No two vertices in {a, d, f} 
             (c)  (d)---(e) (f)    are connected by an edge.
```

---

**Edge Coloring & Vizing's Theorem**

- **Proper Edge Coloring**: An assignment of colors to the edges of a graph such that no two adjacent edges (edges that share a common vertex) share the same color.
- **Edge Chromatic Number (**$\chi'(G)$**)**: The minimum number of colors required for a proper edge coloring of graph $G$.

**Vizing's Theorem**

For any simple graph $G$ with maximum vertex degree $\Delta(G)$, the edge chromatic number is restricted to exactly one of two values:

$$\Delta(G) \le \chi'(G) \le \Delta(G) + 1$$

- **Class 1 Graphs**: Graphs where $\chi'(G) = \Delta(G)$.
- **Class 2 Graphs**: Graphs where $\chi'(G) = \Delta(G) + 1$.

**Edge Chromatic Number of Complete Graphs (**$K_n$**)**

- If $n$ is **even**: $\chi'(K_n) = n - 1$
- If $n$ is **odd**: $\chi'(K_n) = n$

---

**Matching Theory**

- **Matching Set**: A set of non-adjacent edges in a graph (no two edges in the set share a common vertex).
- **Maximal Matching Set**: A matching set to which no other edge from the graph can be added.
- **Maximum Matching Set (MMS)**: A matching set that contains the **maximum possible number of edges**.
- **Matching Number (**$m(G)$ **or** $\alpha'(G)$**)**: The number of edges in a maximum matching set.

```
       Maximal Matching Set                 Maximum Matching Set (MMS)
          (1)=====(2)                          (1)=====(2)
           |       |                            |       |
           |       |                            |       |
          (3)-----(4)                          (3)=====(4)
    Matched Edges: {(1,2)}               Matched Edges: {(1,2), (3,4)}
```

---

**Perfect Matching**

- **Perfect Matching (PM)**: A matching set that covers **every single vertex** in the graph.
- **Necessary Vertex Condition**: A graph can contain a perfect matching **only if** the total number of vertices $|V|$ is an **even integer** ($|V| = 2n$).
- **Relationship**: Every perfect matching is a maximum matching set, but a maximum matching set is not necessarily a perfect matching.

**Counting Perfect Matchings in Complete Graphs**

For a complete graph $K_{2n}$ on $2n$ vertices, the total number of distinct perfect matchings is given by the double factorial formula:

$$\text{Total Perfect Matchings} = (2n - 1)!! = (2n - 1) \times (2n - 3) \times \dots \times 3 \times 1 = \frac{(2n)!}{2^n \cdot n!}$$

- For $K_2$ ($n=1$): $1$ perfect matching.
- For $K_4$ ($n=2$): $3 \times 1 = 3$ perfect matchings.
- For $K_6$ ($n=3$): $5 \times 3 \times 1 = 15$ perfect matchings.

---

**Planar Graphs & Euler's Formula**

- **Planar Graph**: A graph that can be drawn in a single plane such that **no two edges intersect or cross** each other.
- **Planar Embedding**: A drawing of a planar graph on a plane with zero edge crossings.
- **Regions / Faces (**$f$ **or** $R$**)**: The areas into which a planar embedding divides the plane.
    - **Closed (Bounded / Finite) Regions**: Regions enclosed by a cycle of edges.
    - **Open (Unbounded / Infinite) Region**: The single exterior region surrounding the entire drawing.

```
                  Planar Embedding Regions
                     +-----------------+
                     |   Region R_1    |
                     |     (Closed)    |
                     +-----------------+  Region R_2 (Open / Exterior)
```

---

**Degree of a Region & Handshaking Lemma for Regions**

- **Degree of a Region (**$d(R_i)$**)**: The number of edges forming the boundary of region $R_i$ (where an edge taking a walk along both sides of a region boundary contributes **2** to its degree).
- **Handshaking Lemma for Regions**: The sum of the degrees of all regions in a planar graph equals twice the total number of edges: $$\sum_{i=1}^{f} d(R_i) = 2e$$

---

**Euler's Formula**

For any connected planar graph with $n$ vertices, $e$ edges, and $f$ regions (faces):

$$n - e + f = 2$$

**Extension for Disconnected Planar Graphs**

For a disconnected planar graph containing $k$ connected components:

$$n - e + f = k + 1$$

---

**Planarity Edge Bounds**

Since every region in a simple planar graph is bounded by at least 3 edges ($d(R_i) \ge 3$): $$2e \ge 3f$$

Substituting $f = 2 + e - n$ into the inequality yields the **Maximum Edge Bound for Simple Planar Graphs**:

$$e \le 3n - 6 \quad (\text{for } n \ge 3)$$

**Bound for Triangle-Free / Bipartite Planar Graphs**

In a planar graph with no triangles (e.g., bipartite planar graphs), every region is bounded by at least 4 edges ($d(R_i) \ge 4 \implies 2e \ge 4f$):

$$e \le 2n - 4 \quad (\text{for } n \ge 3)$$

---

**Kuratowski's Non-Planar Graphs**

A graph is non-planar if it cannot be drawn on a plane without edge crossings. Kuratowski's Theorem states that a graph is planar **if and only if** it contains no subgraph homeomorphic to $K_5$ or $K_{3,3}$.

```
   Complete Graph K_5 (Non-Planar)           Complete Bipartite K_{3,3} (Non-Planar)
             o                                    Set V_1        Set V_2
           / | \                                    o==============o
          o  |  o                                    \X/        \ /
         / \ | / \                                    X          X
        o---o---o                                    / \        / \
                                                    o==============o
   n = 5, e = 10 (4-Regular)                    n = 6, e = 9 (3-Regular)
   Minimum non-planar by Vertices              Minimum non-planar by Edges
```

**Properties of Kuratowski Graphs**

- **Complete Graph** $K_5$:
    - Vertices: $n = 5$, Edges: $e = 10$.
    - 4-Regular graph.
    - Minimum non-planar graph in terms of **vertices**.
    - Removing any single edge from $K_5$ makes it **planar**.
- **Complete Bipartite Graph** $K_{3,3}$:
    - Vertices: $n = 6$, Edges: $e = 9$.
    - 3-Regular graph.
    - Minimum non-planar graph in terms of **edges**.
    - Removing any single edge from $K_{3,3}$ makes it **planar**.

---

**Worked Practice Problems**

**Problem: Calculating Faces in a Planar Graph**

- **Question**: A connected planar graph has 10 vertices and 15 edges. How many total regions (faces) does it have, and how many of them are closed?
- **Solution**:
    - Apply Euler's Formula: $n - e + f = 2$.
    - Substitute $n = 10$ and $e = 15$: $$10 - 15 + f = 2 \implies -5 + f = 2 \implies f = 7$$
    - Total regions = 7.
    - Since exactly 1 region is open (exterior), the number of closed regions is $7 - 1 = 6$.
- **Result**: **7 total faces** (6 closed, 1 open).

---

**Problem: Edge Limit Verification for Planarity**

- **Question**: Can a simple graph with 8 vertices and 20 edges be planar?
- **Solution**:
    - Apply the maximum edge bound for simple planar graphs: $e \le 3n - 6$.
    - Substitute $n = 8$: $$e_{\max} = 3(8) - 6 = 24 - 6 = 18$$
    - The graph has 20 edges, which exceeds the maximum threshold of 18.
- **Result**: **No, the graph cannot be planar**.

---

**Problem: Counting Perfect Matchings**

- **Question**: Calculate the total number of distinct perfect matchings in the complete graph $K_6$.
- **Solution**:
    - Here $2n = 6 \implies n = 3$.
    - Apply the double factorial formula: $$\text{Total Perfect Matchings} = (2n - 1)!! = (2 \times 3 - 1)!! = 5!!$$ $$5!! = 5 \times 3 \times 1 = 15$$
- **Result**: **15 perfect matchings**.

---

I have also generated and published the visual diagram artifact to your Studio panel:

- `graph_coloring_matching_planarity_diagram.png`: Visual reference card covering Chromatic Numbers, Vizing's Theorem, Matching Theory, Euler's Planarity Formula, and Kuratowski's Non-Planar Graphs.

![[Pasted image 20261108235448.png]]