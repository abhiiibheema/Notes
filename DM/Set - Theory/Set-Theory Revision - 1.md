**1. Binary Relations & Fundamental Counting**

**Basic Definition & Total Relations**

- **Binary Relation**: A relation $R$ from set $A$ to set $B$ is any **subset of the Cartesian product** $A \times B$ ($R \subseteq A \times B$).
- **Size of Cartesian Product**: If $|A| = m$ and $|B| = n$, then $|A \times B| = m \cdot n$.
- **Total Relations from** $A$ **to** $B$: Equal to the total number of subsets of $A \times B$: $$2^{m \cdot n}$$
- **Relation on a Single Set** $A$: If $|A| = n$, then $|A \times A| = n^2$. $$\text{Total number of possible relations on } A = 2^{n^2}$$

---

**Decomposition of the Cartesian Product** $A \times A$

To derive relation-counting formulas, the $n^2$ elements of $A \times A$ are partitioned into three distinct groups:

1. **Diagonal Elements (Identical pairs)**:
    - Form: $(a, a)$
    - Count: $n$
2. **Off-Diagonal Elements (Distinct pairs)**:
    - Form: $(a, b)$ where $a \neq b$
    - Count: $n^2 - n$
3. **Symmetric Pair Boxes**:
    - Grouping of mutual pairs: $\{(a,b), (b,a)\}$
    - Total number of unordered pair boxes: $\frac{n^2 - n}{2}$

---

**Reflexive Relations**

- **Formal Definition**: A relation $R$ on $A$ is **Reflexive** if every element is related to itself: $$\forall a \in A, \quad (a, a) \in R$$ _(Demands all same elements)_.
- **Counting Derivation**:
    - For each of the $n$ diagonal pairs $(a,a)$: exactly **1 choice** (must be present).
    - For each of the $n^2 - n$ non-diagonal pairs: **2 choices** (either present or absent). $$\text{Total Reflexive Relations} = 1^n \cdot 2^{n^2 - n} = 2^{n^2 - n}$$

---

**Irreflexive Relations**

- **Formal Definition**: A relation $R$ on $A$ is **Irreflexive** if no element is related to itself: $$\forall a \in A, \quad (a, a) \notin R$$ _(Hates same elements — zero diagonal elements allowed)_.
- **Counting Derivation**:
    - For each of the $n$ diagonal pairs: exactly **1 choice** (must be absent).
    - For each of the $n^2 - n$ non-diagonal pairs: **2 choices** (present or absent). $$\text{Total Irreflexive Relations} = 1^n \cdot 2^{n^2 - n} = 2^{n^2 - n}$$

---

**Symmetric Relations**

- **Formal Definition**: A relation $R$ on $A$ is **Symmetric** if whenever $(a,b) \in R$, then $(b,a) \in R$: $$\forall a, b \in A, \quad (a, b) \in R \implies (b, a) \in R$$ _(Allows same elements, demands flipping)_.
- **Counting Derivation**:
    - For each of the $n$ diagonal pairs $(a,a)$: **2 choices** (present or absent; flipping $(a,a)$ yields $(a,a)$).
    - For each of the $\frac{n^2 - n}{2}$ symmetric pair boxes $\{(a,b), (b,a)\}$: **2 choices** (either both are absent or both are present). $$\text{Total Symmetric Relations} = 2^n \cdot 2^{\frac{n^2 - n}{2}} = 2^{\frac{n^2 + n}{2}}$$

---

**Compound Counting & Set Operations**

- **Reflexive AND Symmetric Relations**:
    - Diagonal pairs: **1 choice** (all must be present).
    - Symmetric pair boxes: **2 choices** each. $$\text{Total } (\text{Reflexive} \cap \text{Symmetric}) = 2^{\frac{n^2 - n}{2}}$$
- **Reflexive but NOT Symmetric**: $$\#(\text{Ref} \cap \overline{\text{Sym}}) = \#(\text{Ref}) - \#(\text{Ref} \cap \text{Sym}) = 2^{n^2 - n} - 2^{\frac{n^2 - n}{2}}$$
- **Closure under Operations**:
    - If $R_1$ and $R_2$ are Symmetric:
        - $R_1 \cap R_2$ is **always Symmetric**.
        - $R_1 \cup R_2$ is **always Symmetric**.

---

**2. Antisymmetric, Asymmetric, Transitive & Numerical Relations**

**Antisymmetric Relations**

- **Formal Definition**: If both $(a,b) \in R$ and $(b,a) \in R$, then $a$ must equal $b$: $$\forall a, b \in A, \quad [(a, b) \in R \land (b, a) \in R] \implies a = b$$ _Equivalent Contrapositive_: If $a \neq b$, then $(a,b)$ and $(b,a)$ cannot both belong to $R$. _(Allows same elements, forbids two-way flipping)_.
- **Counting Derivation**:
    - For each of the $n$ diagonal pairs: **2 choices** (present or absent).
    - For each of the $\frac{n^2 - n}{2}$ distinct pair boxes $\{(a,b), (b,a)\}$: **3 choices**:
        1. Neither $(a,b)$ nor $(b,a)$ is present.
        2. Only $(a,b)$ is present.
        3. Only $(b,a)$ is present. _(Both present is forbidden)_. $$\text{Total Antisymmetric Relations} = 2^n \cdot 3^{\frac{n^2 - n}{2}}$$
- **Closure under Operations**:
    - If $R_1, R_2$ are Antisymmetric $\implies R_1 \cap R_2$ is **Antisymmetric**.
    - $R_1 \cup R_2$ is **NOT necessarily Antisymmetric** (e.g., $R_1 = \{(1,2)\}$ and $R_2 = \{(2,1)\}$ are both antisymmetric, but $R_1 \cup R_2 = \{(1,2), (2,1)\}$ is not).

---

**Asymmetric Relations**

- **Formal Definition**: If $(a,b) \in R$, then $(b,a)$ cannot be in $R$ under any circumstances: $$\forall a, b \in A, \quad (a, b) \in R \implies (b, a) \notin R$$ _(Forbids same elements, forbids flipping — strictly irreflexive + strictly antisymmetric)_.
- **Counting Derivation**:
    - For the $n$ diagonal pairs $(a,a)$: **1 choice** (must all be absent, because $(a,a) \in R \implies (a,a) \notin R$ creates a contradiction).
    - For each of the $\frac{n^2 - n}{2}$ distinct pair boxes: **3 choices** (neither, only $(a,b)$, or only $(b,a)$). $$\text{Total Asymmetric Relations} = 1^n \cdot 3^{\frac{n^2 - n}{2}} = 3^{\frac{n^2 - n}{2}}$$

---

**Symmetry Comparison Matrix**

|Property|Diagonal Pairs $(a,a)$|Distinct Pairs $\{(a,b), (b,a)\}$|Total Formula|
|---|---|---|---|
|**Symmetric**|Allowed ($2$ choices)|Must match ($2$ choices: both in / both out)|$2^n \cdot 2^{\frac{n^2 - n}{2}} = 2^{\frac{n^2+n}{2}}$|
|**Antisymmetric**|Allowed ($2$ choices)|No mutual pairs ($3$ choices)|$2^n \cdot 3^{\frac{n^2 - n}{2}}$|
|**Asymmetric**|Strictly Forbidden ($1$ choice)|No mutual pairs ($3$ choices)|$3^{\frac{n^2 - n}{2}}$|

---

**Transitive Relations**

- **Formal Definition**: Whenever $(a,b) \in R$ and $(b,c) \in R$, then $(a,c)$ must also belong to $R$: $$\forall a, b, c \in A, \quad [(a, b) \in R \land (b, c) \in R] \implies (a, c) \in R$$
- **Vacuously True Cases**:
    - The empty relation $\emptyset$ is **always Transitive** (the premise is false, making the implication vacuously true).
    - Any relation containing no consecutive links (e.g., $R = \{(1,1)\}$ or $R = \{(1,3)\}$) is **Transitive**.
- **Counterexample**:
    - $R = \{(1,2), (2,1), (1,1)\}$ on $\{1,2\}$:
        - Taking $(2,1)$ and $(1,2)$, transitivity demands $(2,2) \in R$.
        - Since $(2,2) \notin R$, this relation is **NOT Transitive**.
        - Adding $(2,2)$ produces a transitive closure.

---

**Standard Mathematical Relations Analyzed**

1. $R = \{(a,b) \mid a \le b\}$ **on** $\mathbb{Z}$:
    - **Reflexive**: Yes ($a \le a$)
    - **Irreflexive**: No
    - **Symmetric**: No ($1 \le 2$ does not imply $2 \le 1$)
    - **Antisymmetric**: Yes ($a \le b \land b \le a \implies a = b$)
    - **Asymmetric**: No
    - **Transitive**: Yes ($a \le b \land b \le c \implies a \le c$)
2. $R = \{(a,b) \mid a = b + 1\}$ **on** $\mathbb{Z}$:
    - **Reflexive**: No ($a \neq a + 1$)
    - **Irreflexive**: Yes
    - **Symmetric**: No
    - **Antisymmetric**: Yes
    - **Asymmetric**: Yes
    - **Transitive**: No ($a = b+1 \land b = c+1 \implies a = c+2 \neq c+1$)
3. $R = \{(a,b) \mid a = b \lor a = -b\}$ **on** $\mathbb{Z}$:
    - **Reflexive**: Yes
    - **Symmetric**: Yes
    - **Transitive**: Yes
    - _(Forms an Equivalence Relation)_
4. $R = \{(a,b) \mid \gcd(a,b) = 1\}$ **on** $\mathbb{Z}^+$:
    - **Reflexive**: No ($\gcd(2,2) = 2 \neq 1$)
    - **Irreflexive**: No ($\gcd(1,1) = 1$)
    - **Symmetric**: Yes ($\gcd(a,b) = \gcd(b,a)$)
    - **Transitive**: No (Counterexample: $\gcd(3,7) = 1$ and $\gcd(7,9) = 1$, but $\gcd(3,9) = 3 \neq 1$)

---

**3. Equivalence Relations, Partitions & Bell Numbers**

**Equivalence Relation Definition**

A relation $R$ on set $A$ is an **Equivalence Relation (EQR)** if and only if it satisfies **R-S-T**:

1. **Reflexive**: $\forall a \in A, (a,a) \in R$.
2. **Symmetric**: $\forall a,b \in A, (a,b) \in R \implies (b,a) \in R$.
3. **Transitive**: $\forall a,b,c \in A, [(a,b) \in R \land (b,c) \in R] \implies (a,c) \in R$.

---

**Partitions & Equivalence Classes**

- **Equivalence Class** $[a]$: The set of all elements related to $a$: $[a] = \{x \in A \mid (a, x) \in R\}$.
- **Partitioning Principle**: Every equivalence relation on set $A$ **partitions** set $A$ into mutually disjoint, non-empty equivalence classes such that:
    1. $A_1 \cup A_2 \cup \dots \cup A_k = A$.
    2. $A_i \cap A_j = \emptyset$ for all $i \neq j$.
- **Formula for Relation Size** $|R|$: If the equivalence classes have sizes $|A_1|, |A_2|, \dots, |A_k|$, then the total number of ordered pairs in $R$ is: $$|R| = |A_1|^2 + |A_2|^2 + \dots + |A_k|^2$$
    - **Example 1**: If $A = \{1,2,3,4,5\}$ is partitioned into $\{1,2\}, \{3,4\}, \{5\}$: $$|R| = 2^2 + 2^2 + 1^2 = 4 + 4 + 1 = 9$$
    - **Example 2**: If $|A| = 30$ and is partitioned into 3 equal-sized classes $A_1, A_2, A_3$ ($|A_i| = 10$): $$|R| = 10^2 + 10^2 + 10^2 = 300$$

---

**Total Equivalence Relations = Bell Numbers (**$B_n$**)**

$$\text{Total number of Equivalence Relations on set of size } n = \text{Total Partitions of set} = B_n$$

**Bell Triangle Generation Method**:

1. Start with Row 0: `1`.
2. Start the next row with the last number of the previous row.
3. Each subsequent entry is the sum of the element to its left and the element directly above that left element.

\[\begin{array}{l|lllll} B_0 & \mathbf{1} & & & & \ B_1 & \mathbf{1} & 2 & & & \ B_2 & \mathbf{2} & 3 & 5 & & \ B_3 & \mathbf{5} & 7 & 10 & 15 & \ B_4 & \mathbf{15} & 20 & 27 & 37 & 52 \ B_5 & \mathbf{52} & & & & \ \end{array}\]

- **Values to Memorize**:
    - $B_0 = 1$
    - $B_1 = 1$
    - $B_2 = 2$
    - $B_3 = 5$
    - $B_4 = 15$
    - $B_5 = 52$
- **Closure under Operations**:
    - If $R_1, R_2$ are Equivalence Relations:
        - $R_1 \cap R_2$ is **always an Equivalence Relation**.
        - $R_1 \cup R_2$ is **NOT necessarily an Equivalence Relation** (often violates transitivity).

---

**4. Partial Order Relations (POSETs), Comparability & Hasse Diagrams**

**Partial Order Relation (POR / POSET)**

A relation $R$ on set $A$ is a **Partial Order Relation** if it satisfies **R-A-T**:

1. **Reflexive**: $\forall a \in A, a R a$.
2. **Antisymmetric**: $\forall a, b \in A, (a R b \land b R a) \implies a = b$.
3. **Transitive**: $\forall a, b, c \in A, (a R b \land b R c) \implies a R c$.
4. A set equipped with a partial order is called a **POSET**, denoted $(A, R)$.
5. **Standard Poset Examples**:
    - $(\mathbb{Z}, \le)$.
    - $(\mathbb{Z}^+, \mid)$ (divisibility on positive integers).
    - $(\mathcal{P}(A), \subseteq)$ (power set under subset inclusion).
6. **Non-Poset Divisibility Case**:
    - $(\mathbb{Z}, \mid)$ on all integers is **NOT a POSET** because:
        1. Division by zero is undefined ($0 \mid 0$ issue).
        2. Fails antisymmetry ($2 \mid -2$ and $-2 \mid 2$, but $2 \neq -2$).
7. **Operations**:
    - If $R_1, R_2$ are PORs $\implies R_1 \cap R_2$ is **always a POR**.
    - $R_1 \cup R_2$ is **NOT necessarily a POR**.

---

**Comparability & Total Order Relations (TOR)**

- **Comparability**: Two elements $a, b \in A$ are **comparable** if: $$a R b \quad \text{OR} \quad b R a$$
    - In $(\mathbb{Z}^+, \mid)$: $3$ and $9$ are comparable ($3 \mid 9$), but $3$ and $5$ are **incomparable** ($3 \nmid 5$ and $5 \nmid 3$).
- **Total Order Relation (TOR) / Linear Order**:
    - A partial order where **every single pair of elements is comparable**: $$\forall a, b \in A, \quad a R b \lor b R a$$
    - Visual test: A Total Order forms a single unbranching vertical chain in a Hasse diagram.

---

**Hasse Diagram Rules**

A Hasse diagram is constructed from a directed relation graph using three simplifications:

1. **Remove all self-loops** (Reflexivity is implicit).
2. **Remove all transitive edges** (Keep only direct covering relationships).
3. **Orient vertically without arrowheads**: If $a R b$, position $b$ higher than $a$ and connect them with an undirected straight segment.

**Core Hasse Diagram Structures**:

- $(D_{12}, \mid)$ **— Divisors of 12**:
    - Level 0: $1$
    - Level 1: $2, 3$
    - Level 2: $4$ (above $2$), $6$ (above $2$ and $3$)
    - Level 3: $12$ (above $4$ and $6$)
- $(D_{24}, \mid)$ **— Divisors of 24**:
    - Levels: $1 \to \{2, 3\} \to \{4, 6\} \to \{8, 12\} \to 24$.
- $(\mathcal{P}(\{1,2,3\}), \subseteq)$ **— Power Set of 3 elements**:
    - 8 nodes arranged across 4 levels forming a 3D cube:
        - Level 0: $\emptyset$
        - Level 1: ${1}, {2}, $\{3\}$
        - Level 2: $\{1,2\}, \{1,3\}, \{2,3\}$
        - Level 3: $\{1,2,3\}$

---

**Key Revision Checklist**

**1. Master Counting Formulas (**$|A| = n$**)**

- **Total Relations**: $2^{n^2}$
- **Reflexive Relations**: $2^{n^2 - n}$
- **Irreflexive Relations**: $2^{n^2 - n}$
- **Symmetric Relations**: $2^{\frac{n^2 + n}{2}}$
- **Reflexive** $\cap$ **Symmetric**: $2^{\frac{n^2 - n}{2}}$
- **Antisymmetric Relations**: $2^n \cdot 3^{\frac{n^2 - n}{2}}$
- **Asymmetric Relations**: $3^{\frac{n^2 - n}{2}}$
- **Equivalence Relations**: $B_n$ (Bell Numbers: $1, 1, 2, 5, 15, 52$)

**2. Boundary Set Behavior (**$\emptyset$ **and** $A \times A$**)**

- **Empty Relation** $\emptyset$:
    - Reflexive: **No** (unless $A = \emptyset$)
    - Irreflexive: **Yes**
    - Symmetric: **Yes**
    - Antisymmetric: **Yes**
    - Asymmetric: **Yes**
    - Transitive: **Yes**
    - Equivalence: **No**
    - POSET: **No**
- **Universal Relation** $A \times A$:
    - Reflexive: **Yes**
    - Symmetric: **Yes**
    - Transitive: **Yes**
    - Equivalence: **Yes** (creates 1 class containing all elements)
    - Antisymmetric: **No** (unless $|A| \le 1$)
    - POSET: **No** (unless $|A| \le 1$)

**3. Intersection (**$\cap$**) vs. Union (**$\cup$**) Summary**

- **Intersection (**$\cap$**)**:
    - Preserves Symmetric (**Yes**)
    - Preserves Antisymmetric (**Yes**)
    - Preserves Equivalence (**Yes**)
    - Preserves Partial Order (**Yes**)
- **Union (**$\cup$**)**:
    - Preserves Symmetric (**Yes**)
    - Preserves Antisymmetric (**No**)
    - Preserves Equivalence (**No**)
    - Preserves Partial Order (**No**)