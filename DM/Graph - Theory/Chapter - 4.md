**Graph Theory: Group 4 — Eulerian & Hamiltonian Graphs**

---

**1. Fundamental Classification: Edge vs. Vertex Coverage**

Network traversals are categorized based on whether they systematically cover **all edges** or **all vertices**, and whether the traversal forms an **open line** or a **closed loop**.

```
                           Graph Traversal Coverage
                                      |
            +-------------------------+-------------------------+
            |                                                   |
    [Cover ALL EDGES]                                   [Cover ALL VERTICES]
   (Eulerian Traversal)                                (Hamiltonian Traversal)
   No edge repeated                                    No vertex repeated
            |                                                   |
      +-----+-----+                                       +-----+-----+
      |           |                                       |           |
   Closed       Open                                   Closed       Open
      |           |                                       |           |
  Eulerian    Semi-Eulerian                            Hamiltonian  Hamiltonian
   Graph         Graph                                    Graph        Path
 (Euler Ckt)  (Euler Line)                              (Closed Path) (Open Path)
```

---

**2. Eulerian Graphs & Characterization Theorems**

**Definitions**

- **Euler Trail (Euler Line)**: An open trail that visits **every edge** of the graph **exactly once**.
- **Euler Circuit (Eulerian Circuit)**: A closed trail that visits **every edge** of the graph **exactly once** and returns to the starting vertex.
- **Eulerian Graph**: A connected non-trivial graph that contains an **Euler circuit**.
- **Semi-Eulerian Graph**: A connected graph that contains an **Euler trail** (open line), but no Euler circuit.

```
      Eulerian Graph (All Even Degrees)         Semi-Eulerian Graph (2 Odd Degrees)
                 (1)---(2)                                   (1)---(2)
                  | \ / |                                     |     |
                  |  X  |                                     |     |
                  | / \ |                                     (3)---(4)
                 (3)---(4)                                  d(1)=2, d(2)=2
              d(v) = 4 for all                            d(3)=3, d(4)=3
```

---

**Characterization Theorems**

- **Eulerian Graph Theorem**: A connected non-trivial graph $G$ is Eulerian **if and only if** every vertex in $G$ has an **even degree**. $$\text{Graph is Eulerian} \iff \forall v \in V, \; d(v) \text{ is even}$$
- **Semi-Eulerian Graph Theorem**: A connected graph $G$ is Semi-Eulerian **if and only if** it contains **exactly two vertices of odd degree**. These two odd-degree vertices serve as the distinct start and end points of the Euler trail. $$\text{Graph is Semi-Eulerian} \iff \text{Number of odd-degree vertices} = 2$$

**Comparison Matrix: Eulerian vs. Semi-Eulerian**

|Property|Eulerian Graph|Semi-Eulerian Graph|
|---|---|---|
|**Traversal Type**|Closed Euler Circuit|Open Euler Trail / Line|
|**Edge Coverage**|All edges exactly once|All edges exactly once|
|**Vertex Degree Rule**|**All** vertices have **even** degree|**Exactly 2** vertices have **odd** degree|
|**Start & End Vertex**|Same vertex (Start == End)|Distinct vertices (Start $\ne$ End)|

---

**3. Hamiltonian Graphs & Path Relations**

**Definitions**

- **Hamiltonian Path**: An open path that visits **every vertex** of the graph **exactly once**.
- **Hamiltonian Circuit (Hamiltonian Cycle)**: A closed path that visits **every vertex** of the graph **exactly once** and returns to the starting vertex.
- **Hamiltonian Graph**: A graph that contains a **Hamiltonian circuit**.

```
    Hamiltonian Circuit in C_4                     Hamiltonian Path in P_4
           (1)=======(2)                               (1)-------(2)
            ||       ||                                           |
            ||       ||                                           |
           (4)=======(3)                               (4)-------(3)
      Closed loop covering all 4                 Open route visiting all 4
```

---

**Structural Relationships & Rules**

- **Cycle to Path Implication**: Every graph containing a Hamiltonian Circuit automatically contains a Hamiltonian Path (by dropping the final returning edge).
- **Converse Failure**: A graph containing a Hamiltonian Path does **not** necessarily contain a Hamiltonian Circuit.
- **Cut-Vertex Constraint**: If a graph contains a **cut-vertex**, it **cannot be Hamiltonian** (removing the cut-vertex disconnects the graph, making a single continuous vertex cycle impossible).

---

**4. Hamiltonian Properties Across Graph Families**

|Graph Family|Notation|Hamiltonian Circuit Exists?|Condition / Formula|
|---|---|---|---|
|**Complete Graph**|$K_n$|**Yes** (for $n \ge 3$)|Contains $\frac{(n-1)!}{2}$ distinct cycles|
|**Cycle Graph**|$C_n$|**Yes** (for $n \ge 3$)|Exactly 1 unique cycle|
|**Wheel Graph**|$W_n$|**Yes** (for $n \ge 4$)|Outer perimeter forms the cycle|
|**Complete Bipartite**|$K_{m,n}$|**Yes** (if and only if $m = n$)|Requires equal partition sizes ($n \ge 2$)|
|**Hypercube**|$Q_n$|**Yes** (for $n \ge 2$)|Gray code sequence forms the cycle|

---

**5. Combinatorial Counting & Edge-Disjoint Cycles**

**1. Distinct Hamiltonian Circuits in** $K_n$

In a complete graph $K_n$ with $n \ge 3$, the total number of unique, undirected Hamiltonian Circuits is: $$\text{Total HC in } K_n = \frac{(n - 1)!}{2}$$

**2. Distinct Hamiltonian Circuits in** $K_{n,n}$

In a complete bipartite graph $K_{n,n}$ with equal partition sizes $n \ge 2$, the total number of unique Hamiltonian Circuits is: $$\text{Total HC in } K_{n,n} = \frac{n! \times (n - 1)!}{2}$$

**3. Edge-Disjoint Hamiltonian Cycles in** $K_n$

Two Hamiltonian cycles are **edge-disjoint** if they share **no common edges**. For a complete graph $K_n$ where $n$ is an **odd integer** ($n \ge 3$):

- Total edges in $K_n$: $e = \frac{n(n - 1)}{2}$.
- Edges per Hamiltonian Cycle: $n$.
- Maximum number of Edge-Disjoint Hamiltonian Cycles: $$\text{Max Edge-Disjoint HC} = \frac{n - 1}{2}$$

---

**6. Worked Practice Problems**

**Problem 1: Eulerian Degree Characterization**

- **Question**: A connected graph has 6 vertices, and all vertices have a degree of 4. Is the graph Eulerian?
- **Solution**:
    - Check vertex degrees: All 6 vertices have degree 4, which is an even integer.
    - Apply the Eulerian Graph Theorem: A connected graph is Eulerian if and only if every vertex has an even degree.
- **Result**: **Yes, the graph is Eulerian**.

---

**Problem 2: Distinct Hamiltonian Circuits in Complete Graphs**

- **Question**: Calculate the number of distinct Hamiltonian Circuits in complete graphs $K_4$ and $K_5$.
- **Solution**:
    - Apply the counting formula $\text{Total HC} = \frac{(n - 1)!}{2}$:
    - For $K_4$ ($n = 4$): $$\text{Total HC} = \frac{(4 - 1)!}{2} = \frac{3!}{2} = \frac{6}{2} = 3$$
    - For $K_5$ ($n = 5$): $$\text{Total HC} = \frac{(5 - 1)!}{2} = \frac{4!}{2} = \frac{24}{2} = 12$$
- **Result**: **3 cycles in** $K_4$ and **12 cycles in** $K_5$.

---

**Problem 3: Edge-Disjoint Round-Robin Seating Game**

- **Question**: 19 children in a school play a game by sitting in a circle holding hands. Each day they must sit such that no child holds hands with a person they have previously held hands with. For how many maximum days can they play this game?
- **Solution**:
    - Model each child as a vertex in a complete graph $K_{19}$ (where $n = 19$, an odd integer).
    - Each day's seating arrangement forms a unique Hamiltonian cycle of hand-holds.
    - To avoid repeating hand-holds, each day's cycle must be **edge-disjoint** from previous cycles.
    - Apply the formula for maximum edge-disjoint Hamiltonian cycles: $$\text{Max Days} = \frac{n - 1}{2} = \frac{19 - 1}{2} = \frac{18}{2} = 9$$
- **Result**: **9 days**.
---
![[Pasted image 20261108235312.png]]