Master Revision Guide: Discrete Mathematics & Graph Theory

1. Executive Framing & Foundation

Graph theory provides the mathematical architecture for modeling, analyzing, and optimizing discrete systems. A graph is formally defined as an ordered pair $G = (V, E)$, where $V = \{v_1, v_2, \dots, v_n\}$ represents a non-empty set of vertices (also termed nodes, points, or joints) and $E = \{e_1, e_2, \dots, e_m\}$ represents a set of edges (also termed links, lines, or branches) that establish pairwise connections between vertices.

Depending on the operational rules governing edge directionality and multiplicity, graphs are categorized into fundamental topological structural types:

- **Undirected Graph**: A graph in which every edge $e = (v_i, v_j)$ is associated with an unordered pair of vertices. The connection is symmetric, meaning passage is permitted equally in both directions.
- **Directed Graph (Digraph)**: A graph in which every edge $e = (v_i, v_j)$ is associated with an ordered pair of vertices, designating a strict direction from the source vertex $v_i$ to the target vertex $v_j$.
- **End Vertices**: The two vertices connected by an edge are defined as its end vertices.
- **Self-Loop (Loop)**: An edge whose end vertices are identical, denoted as $e = (v_i, v_i)$. It connects a vertex to itself and adds $2$ to that vertex's degree.
- **Parallel Edges**: Two or more edges associated with the exact same pair of end vertices.
- **Simple Graph**: An undirected graph that contains strictly **no self-loops** and **no parallel edges**.
- **Multigraph**: A graph that permits parallel edges but prohibits self-loops.
- **Pseudograph**: A graph that permits both self-loops and parallel edges without restriction.

The following matrix summarizes the structural allowances for each primary graph category:

|Graph Topology|Self-Loops Permitted|Parallel Edges Permitted|Edge Association Type|
|---|---|---|---|
|**Simple Graph**|Forbidden|Forbidden|Unordered Pair|
|**Multigraph**|Forbidden|Allowed|Unordered Pair / Multi-set|
|**Pseudograph**|Allowed|Allowed|Unordered Pair / Multi-set|
|**Directed Graph**|Allowed|Allowed (Directed)|Ordered Pair|

---

2. Part 1: Graph Fundamentals & Degree Properties (Group 1)

Vertex Degree Metrics & Fundamental Inequalities

The degree or valency of a vertex $v_i$, denoted $d(v_i)$, measures the total number of edges incident upon $v_i$. In pseudographs and multigraphs, every self-loop contributes $2$ to the degree of its incident vertex.

Specialized vertex degree classifications include:

- **Isolated Vertex**: A vertex with degree $d(v) = 0$. It is connected to no other vertex in the graph.
- **Pendant Vertex**: A vertex with degree $d(v) = 1$. It is connected by exactly one edge to the rest of the graph.
- **Minimum Degree (**$\delta(G)$**)**: The smallest vertex degree present across all vertices in $G$.
- **Maximum Degree (**$\Delta(G)$**)**: The largest vertex degree present across all vertices in $G$.
- **Average Degree**: The sum of all vertex degrees divided by the total vertex count $n$: $$\text{Average Degree} = \frac{\sum_{i=1}^{n} d(v_i)}{n} = \frac{2e}{n}$$

In any simple graph containing $n$ vertices and $e$ edges, the relationship between minimum degree, average degree, maximum degree, and total vertex count is governed by the strict inequality chain: $$\delta(G) \le \frac{2e}{n} \le \Delta(G) \le n - 1$$

Fundamental Degree Theorems & Handshaking Principles

The structural distribution of vertex degrees is bounded by foundational graph-theoretic theorems documented in the course lecture notes.

Theorem 1: The Handshaking Lemma

In any graph $G = (V, E)$, the sum of the degrees of all vertices equals twice the total number of edges: $$\sum_{i=1}^{n} d(v_i) = 2e$$

_Proof Principle_: Every individual edge possesses exactly two endpoints. When summing the degrees of all vertices across the graph, each edge endpoint is counted precisely once per incident vertex, resulting in every edge being counted exactly twice.

Theorem 2: Odd-Degree Vertices Theorem

In any graph, the total number of vertices possessing an **odd degree** is **always even**.

_Proof Principle_: Partition the vertex set $V$ into two disjoint sets: $V_E$ containing vertices of even degree, and $V_O$ containing vertices of odd degree. By the Handshaking Lemma: $$\sum_{v \in V_E} d(v) + \sum_{v \in V_O} d(v) = 2e$$ Since $2e$ is an even integer, and the sum of even degrees $\sum_{v \in V_E} d(v)$ is necessarily even, the sum of odd degrees $\sum_{v \in V_O} d(v)$ must also evaluate to an even integer. A sum of odd integers yields an even total if and only if the count of terms in the sum is even.

Theorem 3: Maximum Degree Bound

In a simple graph containing $n$ vertices, the maximum degree of any vertex cannot exceed $n - 1$: $$\Delta(G) \le n - 1$$

Theorem 4: Maximum Edges in a Simple Graph

In a simple graph with $n$ vertices, the maximum possible number of edges occurs when every pair of distinct vertices is joined by an edge (forming the complete graph $K_n$): $$e_{\max} = \frac{n(n - 1)}{2}$$

Combinatorial Graph Enumeration & Edge Counting

Enumerative graph theory calculates the number of distinct labeled simple graph configurations that can be constructed on a fixed set of $n$ vertices. Letting $N = \frac{n(n - 1)}{2}$ denote the maximum possible edges:

- **Total Labeled Simple Graphs on** $n$ **Vertices**: $$\text{Total Graphs} = 2^{\frac{n(n - 1)}{2}}$$
- **Total Simple Graphs with** $n$ **Vertices and Exactly** $e$ **Edges**: $$\text{Total Graphs} = \binom{\frac{n(n - 1)}{2}}{e}$$
- **Total Simple Graphs with At Most** $k$ **Edges**: $$\text{Total Graphs} = \sum_{e=0}^{k} \binom{\frac{n(n - 1)}{2}}{e}$$
- **Total Simple Graphs with At Least** $k$ **Edges**: $$\text{Total Graphs} = 2^{\frac{n(n - 1)}{2}} - \sum_{e=0}^{k-1} \binom{\frac{n(n - 1)}{2}}{e}$$

Degree Sequences & The Havel-Hakimi Algorithm

A **degree sequence** is an ordered list of the degrees of all vertices in a graph, conventionally arranged in non-increasing order: $$S = (d_1, d_2, \dots, d_n) \quad \text{where} \quad d_1 \ge d_2 \ge \dots \ge d_n$$

A sequence $S$ is defined as **graphical** if there exists at least one simple graph whose vertex degrees match $S$ exactly.

Preliminary Necessary Conditions for Graphical Sequences

Before executing reduction algorithms, a valid sequence must satisfy three preliminary criteria:

1. **Even Sum Condition**: The sum of all degrees $\sum d_i$ must be an even integer.
2. **Even Parity of Odd Terms**: The sequence must contain an even number of odd integers.
3. **Maximum Term Bound**: The largest element $d_1$ must satisfy $d_1 \le n - 1$.

The Havel-Hakimi Reduction Algorithm

The Havel-Hakimi theorem establishes a recursive test to determine whether a non-increasing sequence $S = (d_1, d_2, \dots, d_n)$ is graphical:

1. **Sort**: Ensure the sequence is sorted in non-increasing order.
2. **Cut**: Remove the first (largest) element, $d_1$.
3. **Subtract**: Subtract $1$ from each of the next $d_1$ elements in the remaining sequence.
4. **Re-sort**: Re-order the resulting sequence into non-increasing order.
5. **Iterate**: Repeat steps 1–4 until reaching a terminal state:
    - **Success**: The sequence reduces entirely to zeros $(0, 0, \dots, 0)$, establishing that the sequence **is graphical**.
    - **Failure**: A negative integer appears, or $d_1$ exceeds the remaining element count, establishing that the sequence **is NOT graphical**.

Solved Practice Problems (Group 1)

Problem 1.1: Handshaking Lemma Application

_Question_: A graph contains $n = 14$ vertices and $e = 30$ edges. If every vertex has a degree of either 4 or 5, determine how many vertices have a degree of 5.

_Solution_:

1. Let $x$ denote the number of vertices with degree 5.
2. The remaining $14 - x$ vertices have degree 4.
3. Apply the Handshaking Lemma ($\sum d(v_i) = 2e$): $$4(14 - x) + 5x = 2 \times 30$$ $$56 - 4x + 5x = 60$$ $$x = 60 - 56 = 4$$ _Result_: Exactly **4 vertices** have a degree of 5.

Problem 1.2: Edge Determination from Degree Sequence

_Question_: Calculate the total number of edges in a graph with the degree sequence $(5, 2, 2, 2, 2, 1)$.

_Solution_:

1. Sum all terms in the degree sequence: $$\sum_{i=1}^{6} d(v_i) = 5 + 2 + 2 + 2 + 2 + 1 = 14$$
2. Apply the Handshaking Lemma ($2e = \sum d(v_i)$): $$2e = 14 \implies e = 7$$ _Result_: The graph contains **7 edges**.

Problem 1.3: Havel-Hakimi Sequence Verification

_Question_: Use the Havel-Hakimi algorithm to test whether the degree sequence $(3, 3, 3, 3, 2)$ is graphical.

_Solution_:

1. Initial sorted sequence: $(3, 3, 3, 3, 2)$. Total terms $n = 5$.
2. Remove first term **3** and subtract 1 from the next 3 terms: $$(3 - 1, 3 - 1, 3 - 1, 2) \implies (2, 2, 2, 2)$$
3. Remove first term **2** and subtract 1 from the next 2 terms: $$(2 - 1, 2 - 1, 2) \implies (1, 1, 2)$$
4. Re-sort in non-increasing order: $$(2, 1, 1)$$
5. Remove first term **2** and subtract 1 from the next 2 terms: $$(1 - 1, 1 - 1) \implies (0, 0)$$
6. All-zero terminal state reached. _Result_: The sequence $(3, 3, 3, 3, 2)$ **is graphical**.

---

3. Part 2: Special Graph Families & Isomorphism (Group 2)

Standard Graph Families & Structural Formulations

Graph theory classifies networks into specialized families defined by standard geometric, structural, or combinatorial rules.

Complete Graph ($K_n$)

A simple graph on $n$ vertices ($n \ge 1$) in which every pair of distinct vertices is connected by an edge.

- **Vertex Degree**: Every vertex has degree $n - 1$.
- **Total Edges**: $e = \frac{n(n - 1)}{2}$.
- **Regularity**: $K_n$ is $(n - 1)$-regular, with $\delta(G) = \Delta(G) = n - 1$.

Regular Graph

A graph in which all vertices possess the exact same degree $k$, termed a $k$-regular graph.

- **Degree Condition**: $\delta(G) = \Delta(G) = \frac{2e}{n} = k$.
- **Total Edges**: $e = \frac{n \cdot k}{2}$.
- **Structural Relationship**: Every complete graph $K_n$ is $(n - 1)$-regular, but regular graphs are not necessarily complete.

Cycle Graph ($C_n$)

A closed simple graph on $n$ vertices ($n \ge 3$) consisting of a single continuous loop.

- **Edge-Vertex Equality**: $n = e$.
- **Regularity**: Every vertex has degree $2$, making $C_n$ a 2-regular graph.
- **Classification**: Odd cycles ($C_3, C_5, \dots$) contain an odd number of vertices; even cycles ($C_4, C_6, \dots$) contain an even number of vertices.

Wheel Graph ($W_n$)

A graph formed by adding a single central hub vertex to a cycle graph $C_{n-1}$ and connecting the hub to all $n - 1$ perimeter vertices ($n \ge 4$).

- **Total Edges**: $e(W_n) = 2(n - 1)$.
- **Degree Sequence**: Contains 1 central vertex of degree $n - 1$, and $n - 1$ rim vertices of degree $3$.

Hypercube Graph ($Q_n$)

An $n$-dimensional hypercube graph whose $2^n$ vertices represent $n$-bit binary signals, where an edge connects two vertices if and only if their binary labels differ by exactly 1 bit.

- **Vertex Count**: $|V| = 2^n$.
- **Vertex Degree**: Every vertex has degree $n$, making $Q_n$ an $n$-regular graph.
- **Total Edges**: $e = n \cdot 2^{n-1}$.
- **Bipartite Property**: Every hypercube $Q_n$ is inherently bipartite.

Bipartite & Complete Bipartite Structures

Bipartite Graph

A graph $G = (V, E)$ whose vertex set $V$ can be partitioned into two disjoint non-empty sets $V_1$ and $V_2$ ($V = V_1 \cup V_2$ and $V_1 \cap V_2 = \emptyset$) such that every edge connects a vertex in $V_1$ to a vertex in $V_2$. No edges exist within $V_1$ or within $V_2$.

- **The Odd-Cycle Theorem**: A graph is bipartite **if and only if** it contains **no odd-length cycles**.
- **Chromatic Number**: Every non-trivial bipartite graph has a chromatic number of $\chi(G) = 2$.

Complete Bipartite Graph ($K_{m,n}$)

A bipartite graph with partition sizes $|V_1| = m$ and $|V_2| = n$, where every vertex in $V_1$ is adjacent to every vertex in $V_2$.

- **Total Vertices**: $|V| = m + n$.
- **Total Edges**: $e = m \times n$.
- **Degree Bounds**: Minimum degree $\delta(K_{m,n}) = \min\{m, n\}$, maximum degree $\Delta(K_{m,n}) = \max\{m, n\}$.
- **Maximum Edges in Bipartite Graphs**: For any bipartite graph with $n$ vertices, the maximum edge count occurs when partitions are balanced: $$e_{\max} = \left\lfloor \frac{n^2}{4} \right\rfloor$$

Complementary & Self-Complementary Graphs

Complement of a Graph ($\overline{G}$)

Given a simple graph $G = (V, E)$, its complement $\overline{G} = (V, \overline{E})$ shares the same vertex set $V$, but an edge exists in $\overline{G}$ if and only if it is absent in $G$.

- **Union Identity**: $G + \overline{G} = K_n$.
- **Edge Parity**: $e(G) + e(\overline{G}) = \frac{n(n - 1)}{2}$.

Self-Complementary Graph

A graph $G$ that is structurally isomorphic to its own complement ($G \cong \overline{G}$).

- **Edge Formula**: $e = \frac{n(n - 1)}{4}$.
- **Vertex Constraint**: A self-complementary graph exists if and only if its vertex count satisfies: $$n \equiv 0 \pmod 4 \quad \text{or} \quad n \equiv 1 \pmod 4$$

Graph Isomorphism & The Petersen Graph

Definition of Graph Isomorphism ($G_1 \cong G_2$)

Two graphs $G_1 = (V_1, E_1)$ and $G_2 = (V_2, E_2)$ are isomorphic if there exists a bijective mapping $f: V_1 \to V_2$ such that any two vertices $u, v \in V_1$ are adjacent in $G_1$ if and only if $f(u), f(v)$ are adjacent in $G_2$.

Isomorphic Invariants (Necessary Conditions)

Isomorphic graphs **must** share identical structural invariants:

1. Equal vertex counts ($|V_1| = |V_2|$).
2. Equal edge counts ($|E_1| = |E_2|$).
3. Identical non-increasing degree sequences.
4. Identical counts of cycles of every specific length $k$.

_Critical Rule_: Matching vertex counts, edge counts, and degree sequences is **necessary**, but **NOT sufficient** to prove isomorphism. Structural cycle verification is required.

The Petersen Graph

The Petersen graph is constructed by selecting the 2-element subsets of a 5-element set $\{1, 2, 3, 4, 5\}$ as vertices, with an edge joining two vertices if their subsets are disjoint.

- **Vertex Count**: $|V| = \binom{5}{2} = 10$.
- **Edge Count**: $e = 15$.
- **Regularity**: 3-regular graph ($\Delta = \delta = 3$).
- **Girth**: Contains no 3-cycles or 4-cycles; its girth (shortest cycle length) is $5$.

Solved Practice Problems (Group 2)

Problem 2.1: Edges in a Hypercube

_Question_: Calculate the total number of edges in a 4-dimensional hypercube $Q_4$.

_Solution_:

1. For $Q_n$, total edges are given by $e = n \cdot 2^{n-1}$.
2. Substitute $n = 4$: $$e = 4 \cdot 2^{4-1} = 4 \cdot 2^3 = 4 \cdot 8 = 32$$ _Result_: $Q_4$ contains **32 edges**.

Problem 2.2: Maximum Edges in a Bipartite Graph

_Question_: Determine the maximum number of edges in a bipartite graph containing $n = 6$ vertices.

_Solution_:

1. Divide 6 vertices into equal partitions: $m = 3$ and $n = 3$.
2. Form the complete bipartite graph $K_{3,3}$.
3. Calculate edge count: $e = 3 \times 3 = 9$. Alternatively, apply $e_{\max} = \lfloor 6^2 / 4 \rfloor = 9$. _Result_: The maximum edge count is **9 edges**.

Problem 2.3: Self-Complementary Graph Existence

_Question_: Can a self-complementary graph exist on $n = 6$ vertices?

_Solution_:

1. Apply the edge formula $e = \frac{n(n - 1)}{4}$.
2. Substitute $n = 6$: $$e = \frac{6 \times 5}{4} = \frac{30}{4} = 7.5$$
3. Since edge count must be an integer, no self-complementary graph exists on 6 vertices. (Note that $6 \not\equiv 0, 1 \pmod 4$). _Result_: **Not possible**.

---

4. Part 3: Connectivity, Paths & Cut-Structures (Group 3)

Graph Traversals: Walks, Trails, Paths & Circuits

Traversing a network involves moving along alternating sequences of vertices and edges subject to specific structural constraints.

- **Walk**: An alternating sequence of vertices and edges $v_0, e_1, v_1, e_2, \dots, e_k, v_k$. Vertices and edges **may be repeated**.
- **Trail**: A walk in which **no edge is repeated**. Vertices may be repeated.
- **Path**: A walk in which **neither vertices nor edges are repeated** (except the start and end vertex in a closed path).
- **Closed Walk**: A walk whose initial vertex equals its terminal vertex ($v_0 = v_k$).
- **Circuit (Closed Trail)**: A closed walk with **no repeated edges**.
- **Cycle (Closed Path)**: A closed walk with **no repeated vertices** except $v_0 = v_k$.

The following matrix contrasts the operational rules for graph traversals:

|Traversal Category|Repeated Vertices Permitted|Repeated Edges Permitted|Start == End Vertex|
|---|---|---|---|
|**Walk**|Allowed|Allowed|Open or Closed|
|**Trail**|Allowed|Forbidden|Open|
|**Path**|Forbidden|Forbidden|Open|
|**Closed Walk**|Allowed|Allowed|Required|
|**Circuit**|Allowed|Forbidden|Required|
|**Cycle**|Forbidden (except start/end)|Forbidden|Required|

Connected Graphs, Components & Connectivity Theorems

A graph $G$ is **connected** ($k = 1$) if a path exists between every pair of distinct vertices. If no path exists between at least one pair of vertices, $G$ is **disconnected** ($k \ge 2$) and splits into $k$ maximal connected subgraphs termed **connected components**.

Theorem 1: Complement Connectivity of Disconnected Graphs

If a graph $G$ is disconnected ($k \ge 2$), its complement $\overline{G}$ is **always connected**.

Theorem 2: Complement Connectivity of Connected Graphs

If a graph $G$ is connected, its complement $\overline{G}$ **may be connected or disconnected**.

Theorem 3: Minimum Degree Bound for Guaranteed Connectivity

For any simple graph $G$ with $n$ vertices, if the minimum degree satisfies: $$\delta(G) \ge \frac{n - 1}{2}$$ then $G$ is **guaranteed to be connected**.

Theorem 4: Path Existence for Odd-Degree Vertices

If a graph $G$ contains **exactly two vertices of odd degree**, there **must exist a path** connecting those two odd-degree vertices.

Edge Bounds for Connected and Disconnected Graphs

For a simple graph $G$ with $n$ vertices:

- **Connected Graph Edge Range (**$k = 1$**)**: $$n - 1 \le e \le \frac{n(n - 1)}{2}$$
- **Disconnected Graph Edge Range (**$k$ **Components,** $k \ge 2$**)**: $$n - k \le e \le \frac{(n - k)(n - k + 1)}{2}$$

Cut-Structures & Whitney's Connectivity Theorem

Cut-Edges (Bridges) & Edge Connectivity ($\lambda(G)$)

- **Cut-Edge (Bridge)**: An edge whose removal increases the number of connected components in $G$.
- **Cycle Rule**: An edge $e$ is a cut-edge **if and only if** it does **not belong to any cycle** in $G$.
- **Edge Connectivity (**$\lambda(G)$**)**: The minimum number of edges whose removal disconnects $G$ or leaves an isolated vertex. If $G$ has a bridge, $\lambda(G) = 1$. For a disconnected graph, $\lambda(G) = 0$.

Cut-Vertices (Articulation Points) & Vertex Connectivity ($\kappa(G)$)

- **Cut-Vertex (Articulation Point)**: A vertex whose removal (along with all incident edges) increases the number of connected components in $G$.
- **Vertex Connectivity (**$\kappa(G)$**)**: The minimum number of vertices whose removal disconnects $G$ or leaves an isolated vertex.

Cut-Structure Interrelationships

- If a graph with $|V| > 2$ contains a **cut-edge**, it **must contain at least one cut-vertex**.
- If a graph contains a **cut-vertex**, it does **not necessarily contain a cut-edge** (e.g., two triangles sharing a single central vertex).

Whitney's Theorem on Connectivity

For any simple graph $G$, vertex connectivity, edge connectivity, and minimum degree satisfy the fundamental inequality chain: $$\kappa(G) \le \lambda(G) \le \delta(G)$$

The table below details connectivity metrics across standard graph families:

|Graph Family|Notation|Vertex Connectivity $\kappa(G)$|Edge Connectivity $\lambda(G)$|Minimum Degree $\delta(G)$|
|---|---|---|---|---|
|**Complete Graph**|$K_n$|$n - 1$|$n - 1$|$n - 1$|
|**Cycle Graph**|$C_n$|$2$|$2$|$2$|
|**Wheel Graph**|$W_n$|$3$|$3$|$3$|
|**Complete Bipartite**|$K_{m,n}$|$\min\{m, n\}$|$\min\{m, n\}$|$\min\{m, n\}$|
|**Hypercube**|$Q_n$|$n$|$n$|$n$|

Solved Practice Problems (Group 3)

Problem 3.1: Edge Bounds for Disconnected Graphs

_Question_: Determine the maximum possible number of edges in a disconnected graph with $n = 10$ vertices and $k = 3$ components.

_Solution_:

1. Apply the maximum edge formula for $k$ components: $$e_{\max} = \frac{(n - k)(n - k + 1)}{2}$$
2. Substitute $n = 10$ and $k = 3$: $$e_{\max} = \frac{(10 - 3)(10 - 3 + 1)}{2} = \frac{7 \times 8}{2} = 28$$ _Result_: The maximum edge count is **28 edges**.

Problem 3.2: Minimum Degree for Guaranteed Connectivity

_Question_: Find the smallest minimum degree $\delta(G)$ required to guarantee that a simple graph with $n = 8$ vertices is connected.

_Solution_:

1. Apply the connectivity condition $\delta(G) \ge \frac{n - 1}{2}$.
2. Substitute $n = 8$: $$\delta(G) \ge \frac{8 - 1}{2} = 3.5$$
3. Since degree must be an integer, round up to 4. _Result_: The required minimum degree is $\delta(G) \ge 4$.

Problem 3.3: Verifying Whitney's Inequality

_Question_: A graph $G$ has 6 vertices and is 3-regular. Determine the maximum possible values for its vertex connectivity $\kappa(G)$ and edge connectivity $\lambda(G)$.

_Solution_:

1. Since $G$ is 3-regular, minimum degree $\delta(G) = 3$.
2. Apply Whitney's Theorem: $\kappa(G) \le \lambda(G) \le \delta(G) = 3$. _Result_: The maximum possible value for both $\kappa(G)$ and $\lambda(G)$ is **3**.

---

5. Part 4: Eulerian & Hamiltonian Graphs (Group 4)

Traversal Categorization & Eulerian Characterization

Network traversals are classified based on whether they cover **all edges** (Eulerian) or **all vertices** (Hamiltonian), and whether the traversal forms a **closed loop** or an **open line**.

Definitions

- **Euler Trail (Euler Line)**: An open trail visiting **every edge** of the graph **exactly once**.
- **Euler Circuit (Eulerian Circuit)**: A closed trail visiting **every edge** of the graph **exactly once** and returning to the start vertex.
- **Eulerian Graph**: A connected non-trivial graph containing an **Euler circuit**.
- **Semi-Eulerian Graph**: A connected graph containing an **Euler trail** (open line), but no Euler circuit.

Eulerian Characterization Theorems

- **Eulerian Graph Theorem**: A connected non-trivial graph $G$ is Eulerian **if and only if** every vertex in $G$ has an **even degree**: $$\text{Eulerian Graph} \iff \forall v \in V, \; d(v) \text{ is even}$$
- **Semi-Eulerian Graph Theorem**: A connected graph $G$ is Semi-Eulerian **if and only if** it contains **exactly two vertices of odd degree**. These two odd-degree vertices serve as the distinct start and end points of the Euler trail: $$\text{Semi-Eulerian Graph} \iff \text{Count of odd-degree vertices} = 2$$

The matrix below contrasts Eulerian and Semi-Eulerian properties:

|Structural Attribute|Eulerian Graph|Semi-Eulerian Graph|
|---|---|---|
|**Traversal Type**|Closed Euler Circuit|Open Euler Trail / Line|
|**Edge Coverage**|All edges exactly once|All edges exactly once|
|**Vertex Degree Rule**|**All** vertices have **even** degree|**Exactly 2** vertices have **odd** degree|
|**Start & End Points**|Identical (Start == End)|Distinct (Start $\ne$ End)|

Hamiltonian Graphs & Structural Properties

Definitions

- **Hamiltonian Path**: An open path visiting **every vertex** of the graph **exactly once**.
- **Hamiltonian Circuit (Hamiltonian Cycle)**: A closed path visiting **every vertex** of the graph **exactly once** and returning to the start vertex.
- **Hamiltonian Graph**: A graph containing a **Hamiltonian circuit**.

Structural Rules & Constraints

- **Cycle to Path Implication**: Every graph containing a Hamiltonian Circuit automatically contains a Hamiltonian Path (by dropping the final closing edge).
- **Converse Failure**: A graph containing a Hamiltonian Path does **not** necessarily contain a Hamiltonian Circuit.
- **Cut-Vertex Constraint**: If a graph contains a **cut-vertex**, it **cannot be Hamiltonian** (removing the cut-vertex disconnects the graph, preventing a single continuous vertex cycle).

The table below outlines Hamiltonian properties across standard graph families:

|Graph Family|Notation|Hamiltonian Circuit Exists?|Cycle Count / Existence Formula|
|---|---|---|---|
|**Complete Graph**|$K_n$|**Yes** (for $n \ge 3$)|Contains $\frac{(n-1)!}{2}$ distinct cycles|
|**Cycle Graph**|$C_n$|**Yes** (for $n \ge 3$)|Exactly 1 unique cycle|
|**Wheel Graph**|$W_n$|**Yes** (for $n \ge 4$)|Outer perimeter forms the cycle|
|**Complete Bipartite**|$K_{m,n}$|**Yes** (if and only if $m = n$)|Requires equal partitions ($n \ge 2$)|
|**Hypercube**|$Q_n$|**Yes** (for $n \ge 2$)|Gray code sequence forms the cycle|

Combinatorial Counting & Edge-Disjoint Cycles

1. **Distinct Hamiltonian Circuits in** $K_n$: In a complete graph $K_n$ ($n \ge 3$), the total number of unique undirected Hamiltonian Circuits is: $$\text{Total HC in } K_n = \frac{(n - 1)!}{2}$$
2. **Distinct Hamiltonian Circuits in** $K_{n,n}$: In a complete bipartite graph $K_{n,n}$ ($n \ge 2$), the total number of unique Hamiltonian Circuits is: $$\text{Total HC in } K_{n,n} = \frac{n! \times (n - 1)!}{2}$$
3. **Edge-Disjoint Hamiltonian Cycles in** $K_n$: Two Hamiltonian cycles are **edge-disjoint** if they share no common edges. For a complete graph $K_n$ where $n$ is an **odd integer** ($n \ge 3$), the maximum number of edge-disjoint Hamiltonian cycles is: $$\text{Max Edge-Disjoint HC} = \frac{n - 1}{2}$$

Solved Practice Problems (Group 4)

Problem 4.1: Eulerian Degree Characterization

_Question_: A connected graph has 6 vertices, all having degree 4. Is the graph Eulerian?

_Solution_:

1. Check vertex degrees: All 6 vertices have degree 4, which is an even integer.
2. Apply the Eulerian Graph Theorem: A connected graph is Eulerian if and only if every vertex has an even degree. _Result_: **Yes, the graph is Eulerian**.

Problem 4.2: Hamiltonian Circuit Counting

_Question_: Calculate the number of distinct Hamiltonian Circuits in complete graphs $K_4$ and $K_5$.

_Solution_:

1. Apply the formula $\text{Total HC} = \frac{(n - 1)!}{2}$:
2. For $K_4$ ($n = 4$): $\frac{(4 - 1)!}{2} = \frac{3!}{2} = 3$.
3. For $K_5$ ($n = 5$): $\frac{(5 - 1)!}{2} = \frac{4!}{2} = 12$. _Result_: **3 cycles in** $K_4$ and **12 cycles in** $K_5$.

Problem 4.3: Edge-Disjoint Round-Robin Seating Game

_Question_: 19 children in a school play a game by sitting in a circle holding hands. Each day they must arrange themselves such that no child holds hands with a person they have previously held hands with. Determine the maximum number of days they can play this game.

_Solution_:

1. Model children as vertices in a complete graph $K_{19}$ ($n = 19$, an odd integer).
2. Each day's seating arrangement forms a unique Hamiltonian cycle.
3. To prevent repeating hand-holds, each day's cycle must be edge-disjoint from prior cycles.
4. Apply the edge-disjoint formula $\text{Max Days} = \frac{n - 1}{2}$: $$\text{Max Days} = \frac{19 - 1}{2} = \frac{18}{2} = 9$$ _Result_: The game can be played for a maximum of **9 days**.

---

6. Part 5: Graph Coloring, Matching & Planarity (Group 5)

Vertex Coloring, Chromatic Numbers & Independence Sets

- **Proper Vertex Coloring**: Assigning colors to vertices such that no two adjacent vertices share the same color.
- **Chromatic Number (**$\chi(G)$**)**: The minimum number of colors required for a proper vertex coloring of $G$.
- $k$**-Chromatic Graph**: A graph with chromatic number $\chi(G) = k$.

Chromatic Numbers across Standard Graph Families

- **Complete Graph (**$K_n$**)**: $\chi(K_n) = n$.
- **Cycle Graph (**$C_n$**)**: $\chi(C_n) = 2$ if $n$ is even; $\chi(C_n) = 3$ if $n$ is odd.
- **Wheel Graph (**$W_n$**)**: $\chi(W_n) = 3$ if $n$ is odd (outer cycle $C_{n-1}$ even); $\chi(W_n) = 4$ if $n$ is even (outer cycle $C_{n-1}$ odd).
- **Bipartite Graphs & Trees**: $\chi(G) = 2$ for all non-trivial trees and bipartite graphs $K_{m,n}$.
- **Hypercube Graph (**$Q_n$**)**: $\chi(Q_n) = 2$ (bipartite).

Independence Sets & Chromatic Bounds

- **Independence Set**: A subset of vertices in $G$ such that no two vertices in the set are adjacent.
- **Independence Number (**$\beta(G)$**)**: The number of vertices in the largest maximal independence set of $G$.
- **Chromatic Inequality Bound**: Chromatic number, vertex count $n$, and independence number $\beta(G)$ satisfy: $$\chi(G) \ge \frac{n}{\beta(G)}$$

Edge Coloring & Vizing's Theorem

- **Proper Edge Coloring**: Assigning colors to edges such that no two adjacent edges (edges sharing a common vertex) share the same color.
- **Edge Chromatic Number (**$\chi'(G)$**)**: The minimum number of colors required for a proper edge coloring of $G$.

Vizing's Theorem

For any simple graph $G$ with maximum vertex degree $\Delta(G)$, the edge chromatic number is restricted to exactly one of two values: $$\Delta(G) \le \chi'(G) \le \Delta(G) + 1$$

- **Class 1 Graphs**: Graphs where $\chi'(G) = \Delta(G)$.
- **Class 2 Graphs**: Graphs where $\chi'(G) = \Delta(G) + 1$.

Edge Chromatic Number of Complete Graphs ($K_n$)

- If $n$ is **even**: $\chi'(K_n) = n - 1$.
- If $n$ is **odd**: $\chi'(K_n) = n$.

Matching Theory & Perfect Matchings

- **Matching Set**: A set of non-adjacent edges in $G$ (no two edges share a vertex).
- **Maximum Matching Set (MMS)**: A matching set containing the maximum possible number of edges.
- **Matching Number (**$\alpha'(G)$**)**: The number of edges in a maximum matching set.
- **Perfect Matching (PM)**: A matching set that covers **every single vertex** in $G$.

Necessary Conditions & Perfect Matching Enumeration

- **Even Vertex Condition**: A graph can contain a perfect matching **only if** its vertex count $|V|$ is an **even integer** ($|V| = 2n$).
- **Perfect Matchings in Complete Graphs (**$K_{2n}$**)**: For a complete graph $K_{2n}$ on $2n$ vertices, the total number of distinct perfect matchings is given by the double factorial formula: $$\text{Total Perfect Matchings} = (2n - 1)!! = (2n - 1) \times (2n - 3) \times \dots \times 3 \times 1 = \frac{(2n)!}{2^n \cdot n!}$$

Planar Graphs, Euler's Formula & Kuratowski's Theorem

- **Planar Graph**: A graph that can be drawn in a plane such that no two edges cross.
- **Regions / Faces (**$f$**)**: The bounded (closed) and unbounded (open exterior) areas defined by a planar embedding.
- **Handshaking Lemma for Regions**: $\sum_{i=1}^{f} d(R_i) = 2e$.

Euler's Formula

For any connected planar graph with $n$ vertices, $e$ edges, and $f$ regions: $$n - e + f = 2$$

For a disconnected planar graph with $k$ connected components: $$n - e + f = k + 1$$

Planarity Edge Bounds

- **Simple Planar Graph Maximum Edge Bound**: $$e \le 3n - 6 \quad (\text{for } n \ge 3)$$
- **Triangle-Free / Bipartite Planar Graph Edge Bound**: $$e \le 2n - 4 \quad (\text{for } n \ge 3)$$

Kuratowski's Non-Planar Graphs

A graph is planar **if and only if** it contains no subgraph homeomorphic to $K_5$ or $K_{3,3}$.

- **Complete Graph** $K_5$: $|V|=5$, $e=10$, 4-regular. Minimum non-planar graph by **vertices**. Removing any single edge yields a planar graph.
- **Complete Bipartite Graph** $K_{3,3}$: $|V|=6$, $e=9$, 3-regular. Minimum non-planar graph by **edges**. Removing any single edge yields a planar graph.

Solved Practice Problems (Group 5)

Problem 5.1: Calculating Regions in a Planar Graph

_Question_: A connected planar graph has 10 vertices and 15 edges. Calculate the total number of regions (faces) and the number of closed regions.

_Solution_:

1. Apply Euler's Formula: $n - e + f = 2$.
2. Substitute $n = 10$ and $e = 15$: $$10 - 15 + f = 2 \implies -5 + f = 2 \implies f = 7$$
3. Total regions = 7. Subtracting the 1 exterior open region yields 6 closed regions. _Result_: **7 total regions** (6 closed, 1 open).

Problem 5.2: Edge Limit Planarity Verification

_Question_: Test whether a simple graph with 8 vertices and 20 edges can be planar.

_Solution_:

1. Apply the maximum edge bound $e \le 3n - 6$.
2. Substitute $n = 8$: $e_{\max} = 3(8) - 6 = 18$.
3. The graph has 20 edges, exceeding the threshold of 18. _Result_: **No, the graph cannot be planar**.

Problem 5.3: Counting Perfect Matchings

_Question_: Calculate the total number of distinct perfect matchings in the complete graph $K_6$.

_Solution_:

1. Here $2n = 6 \implies n = 3$.
2. Apply the double factorial formula $(2n - 1)!!$: $$5!! = 5 \times 3 \times 1 = 15$$ _Result_: **15 perfect matchings**.

---

7. Master Formula & Theorem Reference Table

The master reference table below consolidates all core graph-theoretic metrics, structural formulas, and fundamental theorems across Groups 1 through 5:

|Category / Family|Vertices ($n$)|Total Edges ($e$)|Degree / Regularity ($\Delta, \delta$)|Chromatic Number ($\chi$)|Connectivity ($\kappa, \lambda$)|Special Invariants & Formulas|
|---|---|---|---|---|---|---|
|**Handshaking Lemma**|$n$|$e$|$\sum d(v_i) = 2e$|N/A|N/A|Odd-degree vertex count is always even|
|**Complete Graph (**$K_n$**)**|$n$|$\frac{n(n-1)}{2}$|$n - 1$|$n$|$\kappa = \lambda = n - 1$|Hamilton Circuits = $\frac{(n-1)!}{2}$|
|**Cycle Graph (**$C_n$**)**|$n$|$n$|$2$ (2-regular)|$2$ (even), $3$ (odd)|$\kappa = \lambda = 2$|Single continuous closed loop|
|**Wheel Graph (**$W_n$**)**|$n$|$2(n-1)$|Hub: $n-1$, Rim: $3$|$3$ ($n$ odd), $4$ ($n$ even)|$\kappa = \lambda = 3$|Hub joined to outer cycle $C_{n-1}$|
|**Hypercube (**$Q_n$**)**|$2^n$|$n \cdot 2^{n-1}$|$n$ ($n$-regular)|$2$ (Bipartite)|$\kappa = \lambda = n$|Binary signal system, 1-bit Hamming distance|
|**Complete Bipartite (**$K_{m,n}$**)**|$m + n$|$m \cdot n$|$\delta=\min, \Delta=\max$|$2$ (Bipartite)|$\kappa = \lambda = \min\{m, n\}$|Max edges for $n$ vertices = $\lfloor n^2/4 \rfloor$|
|**Self-Complementary (**$G \cong \overline{G}$**)**|$n$|$\frac{n(n-1)}{4}$|N/A|N/A|N/A|Requires $n \equiv 0, 1 \pmod 4$|
|**Petersen Graph**|$10$|$15$|$3$ (3-regular)|$3$|$\kappa = \lambda = 3$|Girth = 5 (no 3- or 4-cycles)|
|**Whitney's Inequality**|$n$|$e$|$\delta(G)$|N/A|$\kappa(G) \le \lambda(G) \le \delta(G)$|Fundamental connectivity hierarchy|
|**Eulerian Graph**|$n$|$e$|All degrees even|N/A|Connected|Closed Euler circuit covers all edges|
|**Semi-Eulerian Graph**|$n$|$e$|Exactly 2 odd degrees|N/A|Connected|Open Euler trail between 2 odd vertices|
|**Edge-Disjoint HC (**$K_n$**)**|$n$ (odd)|$\frac{n(n-1)}{2}$|$n - 1$|$n$|$n - 1$|Max Edge-Disjoint HC = $\frac{n-1}{2}$|
|**Vizing's Edge Theorem**|$n$|$e$|$\Delta(G)$|$\chi'(G)$|N/A|$\Delta \le \chi'(G) \le \Delta + 1$|
|**Perfect Matchings (**$K_{2n}$**)**|$2n$|$\frac{2n(2n-1)}{2}$|$2n - 1$|$2n$|$2n - 1$|Total PMs = $(2n - 1)!!$|
|**Euler's Planar Formula**|$n$|$e$|Regions $f$|N/A|$n - e + f = 2$|Disconnected: $n - e + f = k + 1$|
|**Planar Edge Bounds**|$n \ge 3$|$e \le 3n - 6$|Triangle-free: $e \le 2n - 4$|$\le 4$ (4-Color Thm)|N/A|Non-planar iff contains $K_5$ or $K_{3,3}$|

thumb_upGood report thumb_downBad report