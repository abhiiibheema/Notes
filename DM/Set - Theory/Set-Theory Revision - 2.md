**1. Poset Bounds & Extreme Elements**

**Greatest Element (Maximum Element / GE)**

- **Definition**: In a poset $(A, R)$, an element $x \in A$ is the **Greatest Element** if every element $a \in A$ is related to $x$: $$\forall a \in A, \quad a \le x$$
- **Uniqueness**: If a greatest element exists, it is **strictly unique**.
- **Key Observations**:
    - In $(D_{12}, \mid)$, $12$ is the greatest element because $a \mid 12$ for all $a \in \{1, 2, 3, 4, 6, 12\}$.
    - If a poset branches at the top into two or more incomparable maximal elements, **no greatest element exists**.

**Least Element (Minimum Element / LE)**

- **Definition**: In a poset $(A, R)$, an element $x \in A$ is the **Least Element** if $x$ is related to every element $a \in A$: $$\forall a \in A, \quad x \le a$$
- **Uniqueness**: If a least element exists, it is **strictly unique**.
- **Key Observations**:
    - In $(D_{12}, \mid)$, $1$ is the least element because $1 \mid a$ for all $a \in \{1, 2, 3, 4, 6, 12\}$.
    - If a poset has multiple incomparable minimal elements at the bottom, **no least element exists**.

**Poset Shape Classification**

- **X-Shape**: Neither greatest nor least element exists.
- **V-Shape (Open Top)**: Least element exists at the bottom vertex; no greatest element exists.
- **Inverted V-Shape (Open Bottom)**: Greatest element exists at the top vertex; no least element exists.
- **Closed Diamond / Hexagon**: Both greatest and least elements exist.

---

**Upper Bounds & Least Upper Bound (LUB / Join / Supremum)**

- **Upper Bound (UB)**: For a subset $B \subseteq A$ in a poset $(A, R)$, an element $x \in A$ is an **Upper Bound** of $B$ if: $$\forall b \in B, \quad b \le x$$ _(Upper bounds belong to the parent set_ $A$ _and need not belong to_ $B$_)_.
- **Least Upper Bound (LUB / Join)**: An element $x \in A$ is the **LUB** of $B$ if:
    1. $x$ is an upper bound of $B$.
    2. For every upper bound $u$ of $B$, $x \le u$.
- **Notation**: $\text{LUB}(a, b) = a \lor b$.

**Lower Bounds & Greatest Lower Bound (GLB / Meet / Infimum)**

- **Lower Bound (LB)**: For a subset $B \subseteq A$, an element $x \in A$ is a **Lower Bound** of $B$ if: $$\forall b \in B, \quad x \le b$$
- **Greatest Lower Bound (GLB / Meet)**: An element $x \in A$ is the **GLB** of $B$ if:
    1. $x$ is a lower bound of $B$.
    2. For every lower bound $l$ of $B$, $l \le x$.
- **Notation**: $\text{GLB}(a, b) = a \land b$.

---

**2. Lattice Theory Fundamentals**

**Definition of a Lattice**

- A poset $(A, R)$ is a **Lattice** if and only if **every pair of elements possesses both a unique LUB and a unique GLB**: $$\forall a, b \in A, \quad \text{lub}\{a, b\} \text{ exists} \quad \land \quad \text{glb}\{a, b\} \text{ exists}$$
- Represented algebraically as an algebraic structure with two binary operations: $(A, \lor, \land)$.

**Standard Posets as Lattices**

1. **Totally Ordered Sets / Chains** $(\{1, 2, 3, 4, 5\}, \le)$:
    - $\text{glb}\{a, b\} = \min(a, b)$
    - $\text{lub}\{a, b\} = \max(a, b)$
    - Every totally ordered set is **always a lattice**.
2. **Divisibility Poset** $(D_n, \mid)$:
    - $\text{glb}\{a, b\} = \gcd(a, b)$
    - $\text{lub}\{a, b\} = \text{lcm}(a, b)$
    - In $(D_{12}, \mid)$: $\text{glb}\{3, 4\} = \gcd(3, 4) = 1$, and $\text{lub}\{3, 4\} = \text{lcm}(3, 4) = 12$.
3. **Power Set** $(\mathcal{P}(A), \subseteq)$:
    - $\text{glb}\{S_1, S_2\} = S_1 \cap S_2$
    - $\text{lub}\{S_1, S_2\} = S_1 \cup S_2$
    - Bitstring equivalent: $\text{lub} = \text{OR}$, $\text{glb} = \text{AND}$.

---

**Algebraic Laws of Lattices**

For any lattice $(A, \lor, \land)$ and elements $a, b, c \in A$:

1. **Idempotent Laws**:
    - $a \lor a = a$
    - $a \land a = a$
2. **Commutative Laws**:
    - $a \lor b = b \lor a$
    - $a \land b = b \land a$
3. **Associative Laws**:
    - $a \lor (b \lor c) = (a \lor b) \lor c$
    - $a \land (b \land c) = (a \land b) \land c$
4. **Absorption Laws**:
    - $a \lor (a \land b) = a$
    - $a \land (a \lor b) = a$

---

**3. Classifications of Lattices**

**Bounded Lattice**

- **Definition**: A lattice $(A, R)$ is a **Bounded Lattice** if it contains both a **Greatest Element** (denoted $1$ or $I$) and a **Least Element** (denoted $0$ or $O$).
- **Boundary Identities**:
    - $a \lor 1 = 1$, $a \land 1 = a$
    - $a \lor 0 = a$, $a \land 0 = 0$
- **Theorem**: **Every finite lattice is bounded**.
- **Infinite Non-Bounded Example**: $(\mathbb{Z}^+, \le)$ has least element $1$, but no greatest element; therefore, it is not bounded.

---

**Complemented Lattice**

- **Definition of Complement**: In a bounded lattice with least element $0$ and greatest element $1$, an element $b$ is a **complement** of $a$ (denoted $a' = b$) if: $$a \lor b = 1 \quad \land \quad a \land b = 0$$
- **Complemented Lattice Definition**: A bounded lattice where **every element has at least one complement**.
- **Boundary Complements**:
    - $0' = 1$
    - $1' = 0$
- **Non-Uniqueness**: Complements in a general lattice **need not be unique**.
- **Counterexample** $(D_{12}, \mid)$:
    - For $4$: $4 \lor 3 = \text{lcm}(4, 3) = 12$ and $4 \land 3 = \gcd(4, 3) = 1 \implies 4' = 3$.
    - For $6$: Any complement $b$ requires $\gcd(6, b) = 1 \implies b = 1$, but $\text{lcm}(6, 1) = 6 \neq 12$. Thus, $6$ **has no complement**.
    - Therefore, $(D_{12}, \mid)$ is **NOT a complemented lattice**.

---

**Distributive Lattice**

- **Definition**: A lattice $(A, R)$ is **Distributive** if the operations $\lor$ and $\land$ distribute over each other:
    1. $a \lor (b \land c) = (a \lor b) \land (a \lor c)$
    2. $a \land (b \land c) = (a \land b) \lor (a \land c)$
- **Forbidden Sublattice Theorem**: A lattice is distributive if and only if it contains **neither** $M_3$ **(diamond lattice) nor** $N_5$ **(pentagon lattice)** as a sublattice.
    - **Diamond Lattice** $M_3$ (5 elements: bottom $a$, top $e$, and three pairwise incomparable middle elements $b, c, d$):
        - $b \lor (c \land d) = b \lor a = b$
        - $(b \lor c) \land (b \lor d) = e \land e = e$
        - Since $b \neq e$, the distributive law fails. $M_3$ is complemented but **NOT distributive**.
- $(D_{12}, \mid)$ is a **Distributive Lattice**.

---

**4. Boolean Algebra**

- **Definition**: A lattice is a **Boolean Algebra** if and only if it is **both Complemented AND Distributive**: $$\text{Boolean Algebra} = \text{Complemented Lattice} + \text{Distributive Lattice}$$
- **Uniqueness of Complement**: In every Boolean algebra, the complement of each element is **strictly unique**.
- **Number of Elements**: The cardinality of any finite Boolean algebra is always a **power of 2**: $$|B| = 2^n$$
- **Canonical Example**: The power set poset $(\mathcal{P}(A), \subseteq)$ with $|A| = n$ forms a Boolean algebra containing $2^n$ elements.

---

**5. Algebraic Structures & Group Theory Axioms**

**Algebraic Structure Hierarchy**

An **Algebraic Structure** consists of a non-empty set $G$ equipped with a binary operation $*$, denoted $(G, *)$.

To qualify as a **Group**, $(G, *)$ must satisfy four axioms:

1. **Closure**: $$\forall a, b \in G, \quad a * b \in G$$
2. **Associativity**: $$\forall a, b, c \in G, \quad a * (b * c) = (a * b) * c$$
3. **Identity Element**: $$\exists e \in G \text{ such that } \forall a \in G, \quad a * e = e * a = a$$
4. **Inverse Element**: $$\forall a \in G, \quad \exists a^{-1} \in G \text{ such that } a * a^{-1} = a^{-1} * a = e$$
5. **Abelian Group (Commutative Group)**: A group that additionally satisfies: $$\forall a, b \in G, \quad a * b = b * a$$

---

**6. Analysis of Standard Mathematical Sets & Operations**

**1.** $(\mathbb{Z}, +)$

- Identity: $e = 0 \in \mathbb{Z}$.
- Inverse: $a^{-1} = -a \in \mathbb{Z}$.
- **Result**: **Group** (Infinite Abelian Group).

**2.** $(\mathbb{Q}, \times)$ **vs.** $(\mathbb{Q} \setminus \{0\}, \times)$

- In $(\mathbb{Q}, \times)$: Identity $e = 1$. Inverse of $a$ is $\frac{1}{a}$.
- For $a = 0$, $\frac{1}{0}$ is undefined; zero has no multiplicative inverse.
- **Result**: $(\mathbb{Q}, \times)$ is **NOT a group**. Removing zero yields $(\mathbb{Q} \setminus \{0\}, \times)$, which is a valid **Group**.

**3.** $(\mathbb{R}_{\neq 0}, *)$ **where** $a * b = \frac{ab}{2}$

- Identity: $\frac{a \cdot e}{2} = a \implies e = 2$.
- Inverse: $\frac{a \cdot a^{-1}}{2} = 2 \implies a^{-1} = \frac{4}{a}$.
- **Result**: Valid **Group**.

**4.** $(\mathbb{R}_{\neq 1}, *)$ **where** $a * b = a + b - ab$

- Associativity: $(a*b)*c = a*(b*c) = a + b + c - ab - bc - ac + abc$.
- Identity: $a + e - ae = a \implies e(1 - a) = 0 \implies e = 0$.
- Inverse: $a + a^{-1} - a \cdot a^{-1} = 0 \implies a^{-1} = \frac{a}{a - 1}$.
- **Result**: Valid **Group**.

**5.** $(\mathbb{R}, *)$ **where** $a * b = a + b + 2$

- Identity: $a + e + 2 = a \implies e = -2$.
- Inverse: $a + a^{-1} + 2 = -2 \implies a^{-1} = -4 - a$.
- **Result**: Valid **Group**.

**6. Integer Powers of 2:** $(\{2^n \mid n \in \mathbb{Z}\}, \times)$

- Identity: $e = 2^0 = 1$.
- Inverse: $(2^a)^{-1} = 2^{-a}$.
- **Result**: Valid **Group**.

**7. Power Set under Symmetric Difference:** $(\mathcal{P}(A), \oplus)$

- Operation: $A \oplus B = (A \cup B) - (A \cap B)$.
- Identity: $e = \emptyset$.
- Inverse (Self-Inverse): $A \oplus A = \emptyset = e \implies A^{-1} = A$.
- **Result**: Valid **Group**.

---

**7. Finite Groups & Cayley Tables (Composition Tables)**

**Roots of Unity**

- **Fourth Roots of Unity**: $(\{1, -1, i, -i\}, \times)$ where $i^2 = -1$:
    - Order $|G| = 4$.
    - Identity: $e = 1$.
    - Inverses: $1^{-1} = 1$, $(-1)^{-1} = -1$, $i^{-1} = -i$, $(-i)^{-1} = i$.
- **Cube Roots of Unity**: $(\{1, \omega, \omega^2\}, \times)$ where $\omega^3 = 1$.

**Modulo Addition Group** $(\{0, 1, 2, 3, 4, 5\}, \oplus_6)$

- Operation: $a \oplus_6 b = (a + b) \bmod 6$.
- Identity: $e = 0$.
- Inverses:
    - $0^{-1} = 0$
    - $1^{-1} = 5$
    - $2^{-1} = 4$
    - $3^{-1} = 3$
    - $4^{-1} = 2$
    - $5^{-1} = 1$

---

**8. Subgroups & Lagrange's Theorem**

**Definition of a Subgroup**

Let $(G, *)$ be a group and let $H \subseteq G$ be a non-empty subset. $H$ is a **Subgroup** of $G$ (denoted $H \le G$) if $H$ itself forms a group under $*$:

1. $H \subseteq G$.
2. $H$ satisfies closure, contains identity $e$, and contains inverses for all its elements.

**Subgroup Testing in** $(\{0, 1, 2, 3, 4, 5\}, \oplus_6)$

- $H_1 = \{1, 3, 5\}$: $1 \oplus_6 1 = 2 \notin H_1$ (Fails closure; lacks identity) $\implies$ **Not a subgroup**.
- $H_2 = \{0, 3, 5\}$: $3 \oplus_6 5 = 2 \notin H_2$ (Fails closure) $\implies$ **Not a subgroup**.
- $H_3 = \{0, 2, 4\}$:
    - Closed under $\oplus_6$.
    - Contains identity $0$.
    - Contains all inverses ($0^{-1} = 0$, $2^{-1} = 4$, $4^{-1} = 2$).
    - $\implies$ **Valid subgroup**.

**Trivial vs. Proper Subgroups**

Every group $G$ contains at least **2 Trivial Subgroups**:

1. The singleton identity group: $\{e\}$.
2. The group itself: $G$.
3. Any other subgroup is termed a **Proper / Non-Trivial Subgroup**.

---

**Lagrange's Theorem**

- **Statement**: If $H$ is a subgroup of a finite group $G$, then the **order of** $H$ **divides the order of** $G$: $$\frac{|G|}{|H|} \in \mathbb{Z} \quad (|H| \text{ divides } |G|)$$
- **Implication**: For $|G| = 6$, the only permissible subgroup sizes are the divisors of 6: **1, 2, 3, and 6**. Subgroups of size 4 or 5 cannot exist.
- **Important Property**:
    - **The converse of Lagrange's Theorem is NOT necessarily true**.
    - If $k$ divides $|G|$, a subgroup of order $k$ is not guaranteed to exist.

---

**Exam Summary Matrix**

|Structure / Concept|Defining Axiom / Rule|Key Formula or Property|
|---|---|---|
|**Lattice**|Poset where all pairs have LUB & GLB|$\text{lub} = \lor$, $\text{glb} = \land$|
|**Divisibility Lattice** $(D_n, \mid)$|LUB is LCM, GLB is GCD|$\text{lub}\{a,b\} = \text{lcm}(a,b)$, $\text{glb}\{a,b\} = \gcd(a,b)$|
|**Power Set Lattice** $(\mathcal{P}(A), \subseteq)$|LUB is Union, GLB is Intersection|$\text{lub} = \cup$, $\text{glb} = \cap$|
|**Bounded Lattice**|Contains Greatest ($1$) & Least ($0$) elements|Every finite lattice is bounded|
|**Complemented Lattice**|Every element has at least one complement|$a \lor a' = 1$ and $a \land a' = 0$|
|**Distributive Lattice**|$\lor$ and $\land$ distribute over each other|Cannot contain $M_3$ or $N_5$ as a sublattice|
|**Boolean Algebra**|Complemented + Distributive Lattice|Cardinality must equal $2^n$; complements are unique|
|**Group**|Set + Operation satisfying 4 axioms|Closure, Associativity, Unique Identity, Unique Inverses|
|**Lagrange's Theorem**|For any subgroup $H \le G$ of a finite group|\(|