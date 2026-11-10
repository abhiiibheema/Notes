**Diagram Notes: Module 1 — General & Binary Tree Fundamentals**

---

**1. Structural Taxonomy & Key Terminology**

**A. What is a Tree?**

- **Graph Definition**: A tree is a **non-linear data structure** defined as a **connected, acyclic graph**.
- **Vertices & Edges Rule**: A tree with $n$ vertices (nodes) contains **exactly** $n - 1$ **edges**.
- **Rooted Tree**: A tree with one designated node called the **Root**. All other nodes are arranged hierarchically in parent-child format.

```
                     ( A )   <--- Root Node (Level 0, Depth 0)
                    /  |  \
                   /   |   \
                 (B)  (C)  (D)  <--- Children of A / Siblings to each other (Level 1)
                / \    |   /|\
              (E) (F) (G) (H|I|J)
              /             |
            (K, L)         (M)  <--- Leaves / Deepest Descendants
```

---

**B. Node Classifications & Relationships**

- **Root**: The distinguished top-level node with no parent.
- **Parent & Children**: Node $A$ is the parent of $B, C, D$ if directed edges go from $A$ to $B, C, D$. $B, C, D$ are children of $A$.
- **Siblings**: Nodes that share the **same direct parent** (e.g., $B, C, D$ are siblings; $E, F$ are siblings; but $F, G$ are NOT siblings).
- **Leaf Node (External Node /** $L$ **or** $n_0$**)**: A node with **zero children** (e.g., $K, L, F, G, M, I, J$).
- **Internal Node (**$I$**)**: A node with **at least 1 child**. $$\mathbf{\text{Total Nodes } (N) = L + I} \quad \text{and} \quad \mathbf{\text{Total Edges} = L + I - 1}$$
- **Ancestors of Node** $X$: All nodes lying on the direct path from Node $X$ up to the Root (e.g., Ancestors of $K$ are $E, B, A$).
- **Descendants of Node** $X$: All nodes encountered on paths extending downwards from Node $X$ to any leaf (e.g., Descendants of $B$ are $E, F, K, L$).

---

**C. Height, Depth, & Node Degree**

- **Depth of a Node**: The number of edges along the path from the Root to that specific node.
    - Root is at **Depth 0 / Level 0**.
- **Height of a Tree (**$h$**)**: The **maximum number of edges** on any path from the Root to a Leaf.
    - If the Root is at height 0, a single-node tree has $h = 0$.
- **Degree of a Node**: The **number of children** attached to that node.
- **Sum of Degrees Theorem**: $$\sum \text{Degree of all nodes} = \text{Total Number of Edges} = N - 1$$

---

**2.** $k$**-ary Trees: Concepts & Derivations**

**A. Definition**

- **General Tree**: A tree where each node can have $0$ or more children.
- $k$**-ary Tree**: A specialized general tree where **every internal node has strictly** $0$ **or** $k$ **children**.

```
       k = 2 (Full Binary)          k = 3 (Ternary Tree)           k = 4 (4-ary Tree)
             ( )                           ( )                            ( )
            /   \                        /  |  \                       / / \ \
          ( )   ( )                    ( ) ( ) ( )                   ( )( )( )( )
          / \                          /|\
        ( ) ( )                      ( )( )( )
```

---

**B. Core Derivation of** $k$**-ary Formulas**

By applying the **Sum of Degrees Theorem**:

1. Internal nodes ($I$) each have a degree of $k$.
2. Leaf nodes ($L$) each have a degree of $0$.
3. Total Nodes $N = L + I$. Total Edges $= N - 1 = L + I - 1$.

$$\sum \text{Degree} = \text{Total Edges} \implies (I \cdot k) + (L \cdot 0) = I + L - 1$$ $$I \cdot k = I + L - 1 \implies \mathbf{L = I(k - 1) + 1}$$

Since $N = L + I$, substituting $L = I \cdot k - I + 1$ yields: $$N = (I \cdot k - I + 1) + I \implies \mathbf{N = I \cdot k + 1}$$

---

**3. Binary Trees & The Binary Tree Theorem**

**A. Definition & Key Theorem**

A **Binary Tree** is a tree structure where every node has **at most 2 children** (i.e., $0, 1,$ or $2$ children).

```
   Binary Tree Theorem Setup:
   - n₀ = Number of Leaf Nodes (Degree 0)
   - n₁ = Number of Nodes with 1 Child (Degree 1)
   - n₂ = Number of Nodes with 2 Children (Degree 2)
```

**The Binary Tree Theorem Statement**:

$$\mathbf{n_0 = n_2 + 1}$$ _(The number of leaf nodes is always exactly 1 more than the number of nodes with 2 children, regardless of tree shape or height)_.

---

**B. Proof of** $n_0 = n_2 + 1$

1. **Total Nodes (**$N$**)**: $$N = n_0 + n_1 + n_2 \quad$$2
2. **Total Edges**: $$\text{Edges} = N - 1 = n_0 + n_1 + n_2 - 1$$
3. **Sum of Degrees**: $$\sum \text{Degree} = (n_0 \cdot 0) + (n_1 \cdot 1) + (n_2 \cdot 2) = n_1 + 2n_2$$
4. **Equating Edges and Degree Sum**: $$n_1 + 2n_2 = n_0 + n_1 + n_2 - 1$$ $$2n_2 - n_2 + 1 = n_0 \implies \mathbf{n_0 = n_2 + 1}$$

---

**4. Types of Binary Trees & Height-Node Extremes**

```
 ┌──────────────────────┐  ┌──────────────────────┐  ┌──────────────────────┐
 │   Full Binary Tree   │  │ Perfect Binary Tree  │  │  Skewed Binary Tree  │
 ├──────────────────────┤  ├──────────────────────┤  ├──────────────────────┤
 │ Every node has 0 or  │  │ All levels are completely │ Every node has only │
 │ 2 children (k = 2).  │  │ filled.              │  │ 1 child except leaf. │
 │                      │  │                      │  │                      │
 │        ( )           │  │         ( )          │  │         ( )          │
 │       /   \          │  │        /   \         │  │        /             │
 │     ( )   ( )        │  │      ( )   ( )       │  │      ( )             │
 │     / \              │  │     / \     / \      │  │        \            │
 │   ( ) ( )            │  │   ( )( )   ( )( )    │  │        ( )          │
 └──────────────────────┘  └──────────────────────┘  └──────────────────────┘
```

---

**A. Summary Formula Comparison Matrix**

|Tree Type|Property / Description|Relation / Formula|
|---|---|---|
|**Full Binary Tree**|$k=2$ ($0$ or $2$ children per node)|$L = I + 1$, $N = 2I + 1$, $I = \frac{N-1}{2}$, $L = \frac{N+1}{2}$|
|**Perfect Binary Tree**|All internal nodes have $2$ children; all leaves at level $h$|$N_{\max} = 2^{h+1} - 1$, $h_{\min} = \log_2(N + 1) - 1$|
|**Skewed Binary Tree**|Every node has exactly $1$ child (except leaf)|$N = h + 1$, $h_{\max} = N - 1$|

---

**5. Step-by-Step Solved Problems**

**Problem 1: Finding** $k$ **in a** $k$**-ary Tree**

- **Question**: A complete $k$-ary tree has $24$ internal nodes and $97$ leaves. Find the value of $k$.
- **Step-by-Step Solution**:
    1. Use $L = I(k - 1) + 1$.
    2. Substitute $L = 97$ and $I = 24$: $$97 = 24(k - 1) + 1 \implies 96 = 24(k - 1)$$
    3. Solve for $k$: $$k - 1 = \frac{96}{24} = 4 \implies \mathbf{k = 5}$$

---

**Problem 2: Valid** $(L, I)$ **Pairs in** $k$**-ary Tree (MSQ)**

- **Question**: Which combination of leaf nodes ($L$) and internal nodes ($I$) is possible for some integer value $k \ge 2$ in a full $k$-ary tree?
    - **(A)** $L = 31, I = 5$
    - **(B)** $L = 40, I = 6$
    - **(C)** $L = 13, I = 2$
    - **(D)** $L = 52, I = 8$
- **Step-by-Step Solution**:
    - Rearrange $L = I(k-1) + 1 \implies k - 1 = \frac{L - 1}{I}$. $\frac{L-1}{I}$ must yield an integer.
    - **Option (A)**: $\frac{31 - 1}{5} = \frac{30}{5} = 6 \implies k = 7$ **(Valid)**.
    - **Option (B)**: $\frac{40 - 1}{6} = \frac{39}{6} = 6.5$ _(Invalid)_.
    - **Option (C)**: $\frac{13 - 1}{2} = \frac{12}{2} = 6 \implies k = 7$ **(Valid)**.
    - **Option (D)**: $\frac{52 - 1}{8} = \frac{51}{8} = 6.375$ _(Invalid)_.
- **Correct Options**: **(A) and (C)**.

---

**Problem 3: False Statements on Complete 2-ary Tree**

- **Question**: Which of the following is **FALSE** for a complete 2-ary (full binary) tree?
    - **(A)** If $T$ has $I$ internal nodes, then number of nodes is $2I + 1$.
    - **(B)** If $T$ has $L$ leaves, then number of internal nodes is $2L - 1$.
    - **(C)** If $T$ has $N$ total number of nodes, then number of internal nodes is $(N + 1)/2$.
    - **(D)** If $T$ has $N$ total number of nodes, then number of leaves is $(N - 1)/2$.
- **Step-by-Step Verification**:
    1. In a full binary tree ($k=2$): $L = I + 1$.
    2. $N = L + I = (I + 1) + I = \mathbf{2I + 1}$ $\implies$ **(A) is TRUE**.
    3. $I = L - 1 \implies N = 2(L - 1) + 1 = \mathbf{2L - 1}$ $\implies$ **(B) is FALSE** (total nodes $N = 2L-1$, not internal nodes).
    4. From $N = 2I + 1 \implies I = \mathbf{\frac{N - 1}{2}} \implies$ **(C) is FALSE** (claims $(N+1)/2$).
    5. From $N = 2L - 1 \implies L = \mathbf{\frac{N + 1}{2}} \implies$ **(D) is FALSE** (claims $(N-1)/2$).
- **False Options**: **B, C, and D**.

---

**Problem 4: Min/Max Nodes in Complete Ternary Tree**

- **Question**: Let $T$ be a complete ternary tree ($k=3$) of height $h=6$ (Root at height 0) where each node has $0$ or $3$ children. Find the maximum and minimum possible nodes.
- **Step-by-Step Solution**:
    1. **Maximum Nodes (**$N_{\max}$**)** (Perfect 3-ary Tree): $$N_{\max} = \frac{k^{h+1} - 1}{k - 1} = \frac{3^{6+1} - 1}{3 - 1} = \frac{3^7 - 1}{2} = \frac{2187 - 1}{2} = \mathbf{1093}$$
    2. **Minimum Nodes (**$N_{\min}$**)** (Single internal node branch per level): $$N_{\min} = k \cdot h + 1 = 3(6) + 1 = 18 + 1 = \mathbf{19}$$
- **Correct Answer**: **1093 and 19 (Option C)**.

---

**Problem 5: Max Height of Full Binary Tree**

- **Question**: The maximum possible height of a full binary tree with $N = 23$ nodes is _____?
- **Step-by-Step Solution**:
    1. Height is maximized when node count per level is minimized.
    2. For a full binary tree ($k=2$), $N = 2h + 1$.
    3. Substitute $N = 23$: $$23 = 2h + 1 \implies 2h = 22 \implies \mathbf{h = 11}$$

---

**Problem 6: Min/Max Height Sum for** $N = 127$

- **Question**: Let $T$ be a binary tree with $N = 127$ nodes. The minimum and maximum possible heights of $T$ are $x$ and $y$. What is $x + y$?
- **Step-by-Step Solution**:
    1. **Min Height (**$x$**)** (Perfect Binary Tree): $$x = \log_2(N + 1) - 1 = \log_2(127 + 1) - 1 = \log_2(128) - 1 = 7 - 1 = \mathbf{6}$$
    2. **Max Height (**$y$**)** (Skewed Binary Tree): $$y = N - 1 = 127 - 1 = \mathbf{126}$$
    3. **Sum** $x + y$: $$x + y = 6 + 126 = \mathbf{132}$$

---

**Problem 7: Expression Tree Maximum Value**

- **Question**: Consider the expression tree shown below. Each leaf represents a value of either $0$ or $1$. Over all possible choices, what is the maximum possible value of the root expression?

```
                       (+)  <--- Root
                     /     \
                   (-)     (-)
                  /   \   /   \
                (-)   (-)(-)  (-)
                / \   / \/ \  / \
               [0/1] ... leaves ... [0/1]
```

- **Step-by-Step Bottom-Up Evaluation**:
    1. To maximize $A - B$, make $A = 1$ and $B = 0$ (yielding $1 - 0 = +1$).
    2. To maximize $- (C - D)$, minimize $C - D$ to $0 - 1 = -1$, so that $-(-1) = +1$.
    3. Evaluating subtrees recursively yields a maximum value of $6$ at the root.

---

🌲 **Would you like to continue to Module 2 (Tree Traversals & Combinatorial Tree Counting) next, or practice some sample questions on these binary tree height and leaf formulas?**