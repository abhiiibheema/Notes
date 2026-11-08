**Graph Theory: Group 2 — Special Graph Families & Isomorphism**

---

**1. Complete Graphs & Regular Graphs**

**Complete Graph (**$K_n$**)**

- **Definition**: A simple graph on $n$ vertices (where $n \ge 1$) in which every pair of distinct vertices is connected by an edge.
- **Vertex Degree**: Every vertex has a degree of $n - 1$.
- **Total Edges Formula**: $$e = \frac{n(n - 1)}{2}$$
- **Regularity Property**: $K_n$ is a uniform structure where minimum degree equals maximum degree: $$\delta(G) = \Delta(G) = n - 1$$

```
  K_1          K_2          K_3           K_4
   o          o---o          o           o---o
                            / \          |\ /|
                           o---o         | X |
                                         |/ \|
                                         o---o
```

**Regular Graph**

- **Definition**: A graph in which all vertices have the exact same degree $k$ (known as a $k$-regular graph).
- **Degree Condition**: $$\delta(G) = \frac{2e}{n} = \Delta(G) = k$$
- **Edge Formula for** $k$**-Regular Graph**: $$e = \frac{n \cdot k}{2}$$
- **Structural Relationship**: Every complete graph $K_n$ is an $(n-1)$-regular graph, but not every regular graph is complete.

```
1-Regular Graph              2-Regular Graph (C_4)
    o---o                         o-------o
                                  |       |
    o---o                         |       |
                                  o-------o
```

---

**2. Cycle Graphs, Wheel Graphs & Hypercubes**

**Cycle Graph (**$C_n$**)**

- **Definition**: A closed simple graph on $n$ vertices (where $n \ge 3$) consisting of a single continuous loop.
- **Properties**:
    - Total vertices equals total edges: $n = e$.
    - Every vertex has degree **2** (making $C_n$ a 2-regular graph).
- **Classification**:
    - **Odd-Length Cycle**: $C_n$ where $n$ is an odd integer (e.g., $C_3, C_5$).
    - **Even-Length Cycle**: $C_n$ where $n$ is an even integer (e.g., $C_4, C_6$).

```
    C_3 (Odd Cycle)               C_4 (Even Cycle)
           o                            o-------o
          / \                           |       |
         /   \                          |       |
        o-----o                         o-------o
```

**Wheel Graph (**$W_n$**)**

- **Definition**: A graph formed by adding a single central hub vertex to a cycle graph $C_{n-1}$ and connecting it to all $n-1$ perimeter vertices (where $n \ge 4$).
- **Total Edges Formula**: $$e(W_n) = 2(n - 1)$$
- **Degree Distribution**: One central hub vertex has degree $n - 1$, and the remaining $n - 1$ rim vertices each have degree **3**.

```
    W_4 (4 Vertices)              W_5 (5 Vertices)
           o                            o---o
          /|\                          / \ / \
         / o \                        o---o---o
        / / \ \                        \ / \ /
       o-------o                        o---o
```

**Hypercube (**$Q_n$**)**

- **Definition**: An $n$-dimensional cube graph representing an $n$-bit signal system where vertices differ by a 1-bit position.
- **Vertex Count**: $$|V| = 2^n$$
- **Vertex Degree**: Every vertex has a degree equal to $n$ (making $Q_n$ an $n$-regular graph).
- **Total Edges Formula**: $$e = n \cdot 2^{n-1}$$
- **Bipartite Nature**: Every hypercube graph $Q_n$ is inherently a **Bipartite Graph**.

```
    Q_1 (1-Cube)       Q_2 (2-Cube)             Q_3 (3-Cube)
       o---o             o-------o             o-------o
                         |       |            /|      /|
                         |       |           o-------o |
                         o-------o           | o-----|-o
                                             |/      |/
                                             o-------o
```

---

**3. Bipartite Graphs & Complete Bipartite Graphs**

**Bipartite Graph**

- **Definition**: A graph $G = (V, E)$ whose vertex set $V$ can be partitioned into two disjoint non-empty sets $V_1$ and $V_2$ such that every edge in $E$ connects a vertex in $V_1$ to a vertex in $V_2$. No edges exist between vertices within the same partition.
- **Odd-Cycle Theorem**: A graph is bipartite **if and only if** it contains **no odd-length cycle**.
- **Chromatic Property**: Every non-trivial bipartite graph has a chromatic number of **2**.

```
    Bipartite Graph Partition (V_1 and V_2)
        Set V_1           Set V_2
          (o)-------------->(o)
           | \             /
           |  \           /
          (o)  \-------->(o)
```

**Complete Bipartite Graph (**$K_{m,n}$**)**

- **Definition**: A bipartite graph with partition sizes $|V_1| = m$ and $|V_2| = n$, where every vertex in $V_1$ is connected to every vertex in $V_2$.
- **Total Vertices**: $$|V| = m + n$$
- **Total Edges Formula**: $$e = m \times n$$
- **Extreme Degree Bounds**:
    - Minimum degree: $\delta(K_{m,n}) = \min\{m, n\}$.
    - Maximum degree: $\Delta(K_{m,n}) = \max\{m, n\}$.
- **Maximum Edges Bound**: In any bipartite graph with $n$ total vertices, the maximum possible number of edges occurs when partitions are as equal as possible: $$e_{\max} = \left\lfloor \frac{n^2}{4} \right\rfloor$$

```
  Complete Bipartite Graph K_{2,3}
       V_1 (m=2)        V_2 (n=3)
          o==============o
           \X/        \ /
            X          X
           / \        / \
          o==============o
                         o
```

---

**4. Complementary & Self-Complementary Graphs**

**Complement of a Graph (**$\overline{G}$**)**

- **Definition**: Given a simple graph $G = (V, E)$, its complement $\overline{G} = (V, \overline{E})$ has the exact same vertex set $V$, but an edge exists in $\overline{G}$ **if and only if** it does not exist in $G$.
- **Union Property**: $$G + \overline{G} = K_n$$
- **Edge Relationship**: $$e(G) + e(\overline{G}) = \frac{n(n - 1)}{2}$$

```
     Original Graph (G)               Complement Graph (G-bar)
          o-------o                         o       o
          |       |                          \     /
          |       |                           \   /
          o-------o                         o---o---o
```

**Self-Complementary Graph**

- **Definition**: A graph $G$ that is structurally isomorphic to its own complement ($G \cong \overline{G}$).
- **Edge Count Formula**: $$e = \frac{n(n - 1)}{4}$$
- **Vertex Constraint**: A self-complementary graph exists if and only if the number of vertices $n$ satisfies: $$n \equiv 0 \pmod 4 \quad \text{or} \quad n \equiv 1 \pmod 4$$

---

**5. Graph Isomorphism & Structural Invariants**

**Definition of Isomorphism (**$G_1 \cong G_2$**)**

- Two graphs $G_1 = (V_1, E_1)$ and $G_2 = (V_2, E_2)$ are **isomorphic** if they represent the exact same network topology despite having different visual drawings or vertex labels.
- Formally, there exists a 1-to-1 correspondence (bijection) $f: V_1 \to V_2$ such that vertices $u$ and $v$ are adjacent in $G_1$ **if and only if** $f(u)$ and $f(v)$ are adjacent in $G_2$.

```
   Graph G_1 (Path P_4)              Graph G_2 (Rotated P_4)
    (1)---(2)---(3)---(4)                    (a)
                                              |
                                             (b)
                                              |
                                             (c)
                                              |
                                             (d)
```

**Isomorphic Invariants (Necessary Conditions)**

Two isomorphic graphs **must** share:

- Identical number of vertices.
- Identical number of edges.
- Identical non-increasing degree sequence.
- Identical count of cycles of any specific length.

**Critical Sufficiency Rule**

Having equal vertex counts, edge counts, and degree sequences is **necessary**, but **NOT sufficient** to guarantee isomorphism. Two graphs can match in all these properties and still fail to be isomorphic.

---

**6. Petersen Graph**

- **Construction**: Formed by choosing the 2-element subsets of a 5-element set $\{1, 2, 3, 4, 5\}$ as vertices.
- **Vertex Count**: $$|V| = \binom{5}{2} = 10$$
- **Edge Rule**: An edge connects two vertices if and only if their corresponding subsets share no elements (are disjoint).
- **Structural Attributes**:
    - Total Edges: **15**.
    - **3-Regular Graph**: Every vertex has a degree of 3.
    - Contains no 3-cycles or 4-cycles (girth = 5).

```
                 Petersen Graph Topology
                        (o)
                       / | \
                      o  |  o
                     / \ | / \
                    o---(o)---o
                    |   / \   |
                    |  o---o  |
                    o---------o
```

---

**7. Solved Practice Problems**

**Edges in a Hypercube**

- **Question**: Calculate the total number of edges in a 4-dimensional hypercube $Q_4$.
- **Solution**:
    - For $Q_n$, total vertices $|V| = 2^n$ and total edges $e = n \cdot 2^{n-1}$.
    - Substitute $n = 4$: $$e = 4 \cdot 2^{4-1} = 4 \cdot 2^3 = 4 \cdot 8 = 32$$
- **Result**: **32 edges**.

**Maximum Edges in a Bipartite Graph**

- **Question**: What is the maximum number of edges in a bipartite graph with 6 vertices?
- **Solution**:
    - Partition 6 vertices into two equal sets: $m = 3$ and $n = 3$.
    - Construct the complete bipartite graph $K_{3,3}$.
    - Calculate edge count $e = 3 \times 3 = 9$.
- **Result**: **9 edges**.

**Self-Complementary Graph Existence**

- **Question**: Can a self-complementary graph exist on 6 vertices?
- **Solution**:
    - Apply the edge formula $e = \frac{n(n-1)}{4}$.
    - Substitute $n = 6$: $$e = \frac{6 \times 5}{4} = \frac{30}{4} = 7.5$$
    - Since the number of edges must be an integer, no self-complementary graph can exist on 6 vertices.
- **Result**: **Not possible**.