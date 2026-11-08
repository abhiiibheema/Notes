Graph Theory Group 1: Fundamentals & Degree Properties

1. Core Topology & Graph Representations

In discrete mathematics and network architecture, graph theory provides the foundational framework for modeling pairwise relationships between discrete objects. A graph is mathematically defined as an ordered pair $G = (V, E)$, where $V = \{v_1, v_2, \dots, v_n\}$ represents a non-empty set of **vertices** (also termed nodes, points, or joints) and $E = \{e_1, e_2, \dots, e_m\}$ represents a set of **edges** (also termed links, lines, or branches).

Topological Edge Types

- **Undirected Edge**: An edge $e = (v_i, v_j)$ associated with an **unordered pair** of vertices. In an undirected graph, connectivity is strictly reciprocal, meaning traversal between $v_i$ and $v_j$ can occur in either direction.
- **Directed Edge (Arc)**: An edge $e = (v_i, v_j)$ associated with an **ordered pair** of vertices in a directed graph (digraph). The edge establishes a unidirectional path originating at source vertex $v_i$ and terminating at target vertex $v_j$.
- **End Vertices**: The two vertices joined by a specific edge are designated as its end vertices.
- **Self-Loop (Loop)**: An edge whose end vertices are identical, denoted as $e = (v_i, v_i)$, connecting a vertex directly to itself.
- **Parallel (Multiple) Edges**: Two or more distinct edges that connect the exact same pair of end vertices.

2. Graph Classifications & Structural Taxonomy

Network structures are categorized into distinct graph families based on whether self-loops and parallel edges are permitted within the topological layout.

|Graph Category|Self-Loops Allowed|Parallel Edges Allowed|Structural Definition|
|---|---|---|---|
|**Simple Graph**|Forbidden|Forbidden|An undirected graph containing no self-loops and no parallel edges.|
|**Multigraph**|Forbidden|Allowed|A graph that permits multiple parallel edges between vertex pairs but forbids self-loops.|
|**Pseudograph**|Allowed|Allowed|An unconstrained graph structure permitting both self-loops and parallel edges.|

3. Vertex Degree Metrics & Valency Bounds

The local density of connections around any node is quantified by its vertex degree.

- **Degree / Valency (**$d(v_i)$**)**: The total number of edges incident upon vertex $v_i$. In undirected structures, each self-loop contributes **2** to the degree count of its host vertex because both endpoints of the loop terminate at that single node.
- **Isolated Vertex**: A vertex with a degree of zero ($d(v_i) = 0$), completely disconnected from all other nodes.
- **Pendant Vertex**: A vertex with a degree of one ($d(v_i) = 1$), attached to the network by a single incident edge.
- **Minimum Degree (**$\delta(G)$**)**: The smallest vertex degree present across the entire vertex set of graph $G$.
- **Maximum Degree (**$\Delta(G)$**)**: The largest vertex degree present across the entire vertex set of graph $G$.
- **Average Degree**: The arithmetic mean of all vertex degrees in graph $G$, calculated as:

$$\text{Average Degree} = \frac{\sum_{i=1}^{n} d(v_i)}{n} = \frac{2e}{n}$$

Fundamental Degree Inequalities

For any simple graph $G$ containing $n$ vertices and $e$ edges, the relationship between minimum degree, average degree, maximum degree, and total vertex count is governed by the strict chain of inequalities:

$$\delta(G) \le \frac{2e}{n} \le \Delta(G) \le n - 1$$

4. Fundamental Degree Theorems & Handshaking Principles

The structural properties of vertex degrees in discrete networks are governed by fundamental mathematical theorems.

Theorem 1: Handshaking Lemma

In any graph $G = (V, E)$, the sum of the degrees of all vertices is equal to twice the total number of edges:

$$\sum_{i=1}^{n} d(v_i) = 2e$$

**Core Principle**: Every edge possesses exactly two endpoints. Consequently, summing the degrees across all vertices counts every individual edge endpoint twice, establishing an invariant parity relationship.

Theorem 2: Odd-Degree Vertices Theorem

In any graph, the total number of vertices having an **odd degree** is **always even**.

**Proof Outline**: Partition the vertex set into even-degree vertices ($V_E$) and odd-degree vertices ($V_O$). By the Handshaking Lemma, $\sum_{v \in V_E} d(v) + \sum_{v \in V_O} d(v) = 2e$. Because $2e$ is even and the sum of even degrees is even, the sum of odd degrees $\sum_{v \in V_O} d(v)$ must also be even. A sum of odd integers equals an even number if and only if there is an even count of terms.

Theorem 3: Maximum Degree Bound

In a simple graph containing $n$ vertices, the maximum degree of any vertex cannot exceed $n - 1$:

$$\Delta(G) \le n - 1$$

Theorem 4: Maximum Edges in a Simple Graph

In a simple graph with $n$ vertices, the maximum possible number of edges occurs when every pair of vertices is connected by an edge (forming the complete graph $K_n$):

$$e_{\max} = \frac{n(n - 1)}{2}$$

5. Combinatorial Graph Enumeration & Edge Counting

Enumerative graph theory determines the total number of distinct labeled simple graph configurations that can be constructed on a fixed set of $n$ vertices.

Given $n$ vertices, the maximum number of potential edge placements is $N = \frac{n(n - 1)}{2}$. Each potential edge location can either be present (1) or absent (0).

- **Total Simple Graphs on** $n$ **Vertices**:

$$\text{Total Graphs} = 2^{\frac{n(n - 1)}{2}}$$

- **Total Simple Graphs with** $n$ **Vertices and Exactly** $e$ **Edges**:

$$\text{Total Graphs} = \binom{\frac{n(n - 1)}{2}}{e}$$

- **Total Simple Graphs with At Most** $k$ **Edges**:

$$\text{Total Graphs} = \sum_{e=0}^{k} \binom{\frac{n(n - 1)}{2}}{e}$$

- **Total Simple Graphs with At Least** $k$ **Edges**:

$$\text{Total Graphs} = 2^{\frac{n(n - 1)}{2}} - \sum_{e=0}^{k-1} \binom{\frac{n(n - 1)}{2}}{e}$$

6. Degree Sequences & The Havel-Hakimi Algorithm

Degree Sequence & Graphicality

A **degree sequence** is a non-increasing ordered list of the degrees of all vertices in a graph:

$$S = (d_1, d_2, \dots, d_n) \quad \text{where} \quad d_1 \ge d_2 \ge \dots \ge d_n$$

A degree sequence is defined as **graphical** if there exists at least one simple graph whose vertex degrees exactly match the sequence.

Necessary Conditions for a Graphical Sequence

Before applying reduction algorithms, a sequence must satisfy three basic preliminary checks:

1. **Even Sum Condition**: The sum of all degrees $\sum d_i$ must be an even integer.
2. **Even Parity of Odd Terms**: The sequence must contain an even count of odd integers.
3. **Maximum Term Bound**: The largest term must not exceed $n - 1$ ($d_1 \le n - 1$).

The Havel-Hakimi Reduction Algorithm

The Havel-Hakimi theorem establishes a recursive test to determine whether a non-increasing sequence $S$ is graphical.

1. **Sort**: Ensure the sequence $S = (d_1, d_2, \dots, d_n)$ is ordered in non-increasing fashion.
2. **Cut**: Remove the first (largest) term, $d_1$.
3. **Subtract**: Subtract **1** from each of the next $d_1$ terms in the remaining sequence.
4. **Re-sort**: Re-order the modified sequence into non-increasing order.
5. **Iterate**: Repeat steps 1–4 until one of two terminal states is reached:
    - **Success**: The sequence reduces entirely to zeros $(0, 0, \dots, 0)$, proving the sequence **is graphical**.
    - **Failure**: A negative integer appears, or $d_1$ exceeds the number of remaining elements, proving the sequence **is NOT graphical**.

6. Comprehensive Solved Practice Problems

Problem 1: Handshaking Lemma Application

**Question**: A graph contains $n = 14$ vertices and $e = 30$ edges. If every vertex has a degree of either 4 or 5, determine how many vertices have a degree of 5.

**Solution**:

1. Let $x$ denote the number of vertices with degree 5.
2. The remaining $14 - x$ vertices have degree 4.
3. Apply the Handshaking Lemma ($\sum d(v_i) = 2e$):

$$4(14 - x) + 5x = 2 \times 30$$

$$56 - 4x + 5x = 60$$

$$x = 60 - 56 = 4$$

**Result**: Exactly **4 vertices** have a degree of 5.

Problem 2: Edge Determination from Degree Sequence

**Question**: Calculate the total number of edges in a graph with the degree sequence $(5, 2, 2, 2, 2, 1)$.

**Solution**:

1. Sum all terms in the degree sequence:

$$\sum_{i=1}^{6} d(v_i) = 5 + 2 + 2 + 2 + 2 + 1 = 14$$

1. Apply the Handshaking Lemma ($2e = \sum d(v_i)$):

$$2e = 14 \implies e = 7$$

**Result**: The graph contains **7 edges**.

Problem 3: Havel-Hakimi Sequence Verification

**Question**: Use the Havel-Hakimi algorithm to test whether the degree sequence $(3, 3, 3, 3, 2)$ is graphical.

**Solution**:

1. Initial sorted sequence: $(3, 3, 3, 3, 2)$. Total terms $n = 5$.
2. Remove first term **3** and subtract 1 from the next 3 terms:

$$(3 - 1, 3 - 1, 3 - 1, 2) \implies (2, 2, 2, 2)$$

1. Remove first term **2** and subtract 1 from the next 2 terms:

$$(2 - 1, 2 - 1, 2) \implies (1, 1, 2)$$

1. Re-sort in non-increasing order:

$$(2, 1, 1)$$

1. Remove first term **2** and subtract 1 from the next 2 terms:

$$(1 - 1, 1 - 1) \implies (0, 0)$$

1. Terminal state reached with all zeros.

**Result**: The degree sequence $(3, 3, 3, 3, 2)$ **is graphical**.

thumb_upGood report thumb_downBad report