**1. Vector Spaces: Definition & Closure Rules**

- **Vector Space Definition**: A set $V = \{\vec{v}_1, \vec{v}_2, \dots\}$ of vectors is a **vector space** over the real numbers $\mathbb{R}$ if it is closed under both vector addition and scalar multiplication.
- **Closure under Vector Addition**: For any vectors $\vec{v}_1, \vec{v}_2 \in V$: $$\vec{v}_1 + \vec{v}_2 \in V$$
- **Closure under Scalar Multiplication**: For any scalar $\alpha \in \mathbb{R}$ and vector $\vec{v} \in V$: $$\alpha \vec{v} \in V$$
- **Mandatory Inclusion Properties**:
    - Setting $\alpha = 0$ requires that the **zero vector** is always in the space: $\vec{0} \in V$.
    - Setting $\alpha = -1$ requires that the **additive inverse** is always in the space: $-\vec{v} \in V$.
- **Closure under Arbitrary Linear Combinations**: If $\vec{v}_1, \vec{v}_2, \dots, \vec{v}_n \in V$, then every linear combination belongs to $V$: $$\sum_{i=1}^{n} \alpha_i \vec{v}_i \in V \quad \text{for all } \alpha_i \in \mathbb{R}$$

---

**2. Vector Subspaces**

- **Subspace Definition**: A subset $W \subseteq V$ is a **subspace** if $W$ itself forms a vector space under the same operations, satisfying closure under addition and scalar multiplication.
- **Subspace Criteria & Disqualifications**:
    - A valid subspace must contain the origin/zero vector $\vec{0}$.
    - Sets containing non-zero constant offsets (such as fixing a coordinate to $1$) violate addition closure (e.g., $1 + 1 = 2 \neq 1$) and cannot form a subspace.
    - Non-linear functions (such as logarithmic entries $\log s$) violate addition closure because $\log(s_1) + \log(s_2) = \log(s_1 s_2) \neq \log(s_1 + s_2)$, disqualifying the set from being a subspace.

---

**3. Linear Span**

- **Definition**: Given a set of vectors $\{\vec{v}_1, \vec{v}_2, \dots, \vec{v}_n\}$, their **linear span** (denoted $\operatorname{span}\{\vec{v}_1, \dots, \vec{v}_n\}$) is the set containing **all possible linear combinations** of these vectors: $$\operatorname{span}\{\vec{v}_1, \vec{v}_2, \dots, \vec{v}_n\} = \left\{ \sum_{i=1}^{n} \alpha_i \vec{v}_i \;\middle|\; \alpha_i \in \mathbb{R} \right\}$$
- **Spanning Criterion**: A set of vectors spans a vector space $V$ if every vector in $V$ can be written as a linear combination of that set.

---

**4. Basis of a Vector Space**

A subset of vectors $\{\vec{v}_1, \vec{v}_2, \dots, \vec{v}_r\}$ forms a **basis** for a vector space $V$ if and only if it fulfills two conditions:

1. **Maximal Linearly Independent Set**: The vectors $\vec{v}_1, \dots, \vec{v}_r$ are linearly independent, and adding any additional vector $\vec{v}_{r+1} \in V$ makes the set linearly dependent.
2. **Spanning Condition**: The vectors span the space: $$\operatorname{span}\{\vec{v}_1, \vec{v}_2, \dots, \vec{v}_r\} = V$$
3. **Non-Uniqueness of a Basis**: Multiple distinct sets of vectors can serve as bases for the same vector space.
4. **Basis Cardinality Invariance**: While the vectors in a basis may vary, the **total count of vectors in any basis of a given vector space is always the same**.

---

**5. Dimension of a Vector Space**

- **Definition**: The **dimension** of a vector space $V$ (denoted $\dim(V)$) is the number of linearly independent vectors needed to span $V$.
- **Dimension Formulation**: If a space is spanned by a basis containing $r$ vectors, then: $$\dim(V) = r$$
- **Connection to Matrix Rank**: When $m$ vectors in $\mathbb{R}^n$ are written as rows or columns of a matrix $A_{m \times n}$, the dimension of the subspace they span equals the rank of that matrix: $$\dim(\text{Subspace}) = \rho(A_{m \times n}) = r$$

---

**6. Systems of Linear Equations: Matrix Setup**

- **General Linear System**: A system of $m$ equations in $n$ variables: $$\begin{matrix} a_{11}x_1 + a_{12}x_2 + \dots + a_{1n}x_n = b_1 \\ a_{21}x_1 + a_{22}x_2 + \dots + a_{2n}x_n = b_2 \\ \vdots \\ a_{m1}x_1 + a_{m2}x_2 + \dots + a_{mn}x_n = b_m \end{matrix}$$
- **Matrix Representation**: $$A\vec{x} = \vec{b}$$
    - **Coefficient Matrix (**$A$**)**: Size $m \times n$, containing the coefficients $a_{ij}$.
    - **Variable Vector (**$\vec{x}$**)**: Size $n \times 1$, containing the unknowns $[x_1, x_2, \dots, x_n]^T$.
    - **Constant Vector (**$\vec{b}$**)**: Size $m \times 1$, containing the right-hand values $[b_1, b_2, \dots, b_m]^T$.
- **Augmented Matrix**: Denoted as $[A \mid \vec{b}]$, formed by appending $\vec{b}$ as an additional column to $A$, with overall size $m \times (n+1)$.

---

**7. Conditions for Consistency & Nature of Solutions**

A linear system $A\vec{x} = \vec{b}$ is **consistent** if at least one solution exists. Comparing the rank of the coefficient matrix $\rho(A)$, the rank of the augmented matrix $\rho([A \mid \vec{b}])$, and the number of variables $n$ determines the outcome:

1. **Unique Solution**: $$\rho(A) = \rho([A \mid \vec{b}]) = n$$
2. **Infinitely Many Solutions**: $$\rho(A) = \rho([A \mid \vec{b}]) < n$$
3. **No Solution (Inconsistent System)**: $$\rho(A) \neq \rho([A \mid \vec{b}]) \quad (\text{specifically, } \rho(A) < \rho([A \mid \vec{b}]))$$

---

**8. Square System Invertibility Theorem**

- For a square system of $n$ equations with $n$ unknowns ($A_{n \times n}\vec{x} = \vec{b}$):
    - The augmented matrix $[A \mid \vec{b}]$ has dimensions $n \times (n+1)$, so the largest possible square submatrix is of order $n \times n$.
    - One of these $n \times n$ minors is the determinant of the coefficient matrix, $\det(A)$.
    - If $\det(A) \neq 0$, then $\rho(A) = n$ and $\rho([A \mid \vec{b}]) = n$ automatically.
- **Theorem**: If $\det(A) \neq 0$, the system $A\vec{x} = \vec{b}$ **always possesses a unique solution**.

**Problem 1: Vector Subspace Identification**

- **Question**: Select all the sets that represent a vector subspace:
    - **(A)** $V = \left\{ \begin{bmatrix} 2x \\ y + z \\ 3z \end{bmatrix} \;\middle|\; x, y, z \in \mathbb{R} \right\}$
    - **(B)** $V = \left\{ \begin{bmatrix} t \\ s \\ \log s \end{bmatrix} \;\middle|\; t, s \in \mathbb{R} \right\}$
    - **(C)** $V = \left\{ \begin{bmatrix} 1 \\ p \\ q \end{bmatrix} \;\middle|\; p, q \in \mathbb{R} \right\}$
    - **(D)** $V = \left\{ \begin{bmatrix} 0 \\ p \\ q \end{bmatrix} \;\middle|\; p, q \in \mathbb{R} \right\}$
- **Step-by-Step Evaluation**:
    - **Option (A)**: Take two arbitrary vectors $\vec{v}_1 = \begin{bmatrix} 2x_1 \\ y_1 + z_1 \\ 3z_1 \end{bmatrix}$ and $\vec{v}_2 = \begin{bmatrix} 2x_2 \\ y_2 + z_2 \\ 3z_2 \end{bmatrix}$. Evaluating their vector sum: $$\vec{v}_1 + \vec{v}_2 = \begin{bmatrix} 2(x_1 + x_2) \\ (y_1 + y_2) + (z_1 + z_2) \\ 3(z_1 + z_2) \end{bmatrix}$$ Because $x_1 + x_2, y_1 + y_2, z_1 + z_2 \in \mathbb{R}$, the sum retains the same form. It also satisfies scalar multiplication closure and contains $\vec{0}$ (when $x = y = z = 0$). **Conclusion**: $V$ **is a subspace**.
    - **Option (B)**: Take $\vec{v}_1 = \begin{bmatrix} t_1 \\ s_1 \\ \log s_1 \end{bmatrix}$ and $\vec{v}_2 = \begin{bmatrix} t_2 \\ s_2 \\ \log s_2 \end{bmatrix}$. Evaluating the third component of their sum: $$\log s_1 + \log s_2 = \log(s_1 s_2) \neq \log(s_1 + s_2)$$ Because vector addition fails the logarithmic condition, the set is not closed under addition. **Conclusion**: $V$ **is NOT a subspace**.
    - **Option (C)**: Take $\vec{v}_1 = \begin{bmatrix} 1 \\ p_1 \\ q_1 \end{bmatrix}$ and $\vec{v}_2 = \begin{bmatrix} 1 \\ p_2 \\ q_2 \end{bmatrix}$. Evaluating their sum: $$\vec{v}_1 + \vec{v}_2 = \begin{bmatrix} 1 + 1 \\ p_1 + p_2 \\ q_1 + q_2 \end{bmatrix} = \begin{bmatrix} 2 \\ p_1 + p_2 \\ q_1 + q_2 \end{bmatrix}$$ The first entry becomes $2 \neq 1$, violating addition closure. Furthermore, $\vec{0} \notin V$. **Conclusion**: $V$ **is NOT a subspace**.
    - **Option (D)**: Take $\vec{v}_1 = \begin{bmatrix} 0 \\ p_1 \\ q_1 \end{bmatrix}$ and $\vec{v}_2 = \begin{bmatrix} 0 \\ p_2 \\ q_2 \end{bmatrix}$. Evaluating their sum: $$\vec{v}_1 + \vec{v}_2 = \begin{bmatrix} 0 + 0 \\ p_1 + p_2 \\ q_1 + q_2 \end{bmatrix} = \begin{bmatrix} 0 \\ p_1 + p_2 \\ q_1 + q_2 \end{bmatrix}$$ The set contains $\vec{0}$ (for $p = q = 0$) and satisfies closure under both addition and scalar multiplication. **Conclusion**: $V$ **is a subspace**.
- **Answer**: **(A) and (D)**

---

**Problem 2: Homework Problem 1 — Consistency Analysis**

- **Question**: Check the consistency of the system: $$2x + 4y = 7$$ $$x + 2y = 8$$
- **Step-by-Step Solution**:
    1. Express in matrix form $A\vec{x} = \vec{b}$: $$\begin{bmatrix} 2 & 4 \\ 1 & 2 \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix} = \begin{bmatrix} 7 \\ 8 \end{bmatrix}$$
    2. Compute the determinant and rank of the coefficient matrix $A$: $$\det(A) = (2)(2) - (4)(1) = 4 - 4 = 0 \implies \rho(A) = 1$$
    3. Form the augmented matrix $[A \mid \vec{b}]$: $$[A \mid \vec{b}] = \begin{bmatrix} 2 & 4 & 7 \\ 1 & 2 & 8 \end{bmatrix}$$
    4. Perform the elementary row operation $R_1 \to R_1 - 2R_2$: $$[A \mid \vec{b}] \sim \begin{bmatrix} 2 - 2(1) & 4 - 2(2) & 7 - 2(8) \\ 1 & 2 & 8 \end{bmatrix} = \begin{bmatrix} 0 & 0 & -9 \\ 1 & 2 & 8 \end{bmatrix}$$
    5. Check a $2 \times 2$ submatrix minor using columns 1 and 3: $$\begin{vmatrix} 0 & -9 \\ 1 & 8 \end{vmatrix} = (0)(8) - (-9)(1) = 9 \neq 0 \implies \rho([A \mid \vec{b}]) = 2$$
    6. Compare ranks: $$\rho(A) = 1 \neq \rho([A \mid \vec{b}]) = 2$$
- **Answer**: The system is **inconsistent (No Solution)**.

---

**Problem 3: Homework Problem 2 — Consistency Analysis**

- **Question**: Check the consistency of the system: $$x + 2y = 4$$ $$3x - y = 7$$
- **Step-by-Step Solution**:
    1. Express in matrix form $A\vec{x} = \vec{b}$: $$\begin{bmatrix} 1 & 2 \\ 3 & -1 \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix} = \begin{bmatrix} 4 \\ 7 \end{bmatrix}$$
    2. Evaluate $\det(A)$: $$\det(A) = (1)(-1) - (2)(3) = -1 - 6 = -7 \neq 0$$
    3. Since $\det(A) \neq 0$, the rank of $A$ is full: $$\rho(A) = 2$$
    4. For the augmented matrix $[A \mid \vec{b}] = \begin{bmatrix} 1 & 2 & 4 \\ 3 & -1 & 7 \end{bmatrix}_{2 \times 3}$, its maximum rank is $\min(2, 3) = 2$. Because the minor formed by the first two columns is non-zero: $$\rho([A \mid \vec{b}]) = 2$$
    5. Compare ranks with the number of variables ($n = 2$): $$\rho(A) = \rho([A \mid \vec{b}]) = n = 2$$
- **Answer**: The system is **consistent with a Unique Solution**.

---

**Part 2: Worked Examples & Verification Problems**

---

**Problem 4: Linear System with a Unique Solution**

- **Question**: Analyze consistency and solve: $$2x - y = 3$$ $$x + 2y = 4$$
- **Step-by-Step Solution**:
    1. Formulate in matrix form: $$A = \begin{bmatrix} 2 & -1 \\ 1 & 2 \end{bmatrix}, \quad \vec{x} = \begin{bmatrix} x \\ y \end{bmatrix}, \quad \vec{b} = \begin{bmatrix} 3 \\ 4 \end{bmatrix}$$
    2. Compute $\det(A)$: $$\det(A) = (2)(2) - (-1)(1) = 4 + 1 = 5 \neq 0 \implies \rho(A) = 2$$
    3. For $[A \mid \vec{b}]$, the rank is likewise $2$: $$\rho(A) = \rho([A \mid \vec{b}]) = n = 2$$
    4. Solving algebraically:
        - From equation 1: $y = 2x - 3$
        - Substitute into equation 2: $x + 2(2x - 3) = 4 \implies 5x - 6 = 4 \implies 5x = 10 \implies x = 2$
        - Back-substitute: $y = 2(2) - 3 = 1$
- **Answer**: The system is **consistent with a unique solution at** $(x, y) = (2, 1)$.

---

**Problem 5: System with Infinitely Many Solutions**

- **Question**: Determine the consistency and nature of solutions for: $$x + 2y = 4$$ $$2x + 4y = 8$$ $$3x + 6y = 12$$
- **Step-by-Step Solution**:
    1. Express in matrix form: $$A = \begin{bmatrix} 1 & 2 \\ 2 & 4 \\ 3 & 6 \end{bmatrix}, \quad \vec{b} = \begin{bmatrix} 4 \\ 8 \\ 12 \end{bmatrix}$$
    2. Notice that Row 2 is $2R_1$ and Row 3 is $3R_1$. All $2 \times 2$ minors of $A$ vanish: $$\begin{vmatrix} 1 & 2 \\ 2 & 4 \end{vmatrix} = 0, \quad \begin{vmatrix} 1 & 2 \\ 3 & 6 \end{vmatrix} = 0, \quad \begin{vmatrix} 2 & 4 \\ 3 & 6 \end{vmatrix} = 0 \implies \rho(A) = 1$$
    3. Apply row transformations to the augmented matrix $[A \mid \vec{b}]$:
        - $R_2 \to R_2 - 2R_1$
        - $R_3 \to R_3 - 3R_1$ $$[A \mid \vec{b}] = \begin{bmatrix} 1 & 2 & 4 \\ 2 & 4 & 8 \\ 3 & 6 & 12 \end{bmatrix} \sim \begin{bmatrix} 1 & 2 & 4 \\ 0 & 0 & 0 \\ 0 & 0 & 0 \end{bmatrix} \implies \rho([A \mid \vec{b}]) = 1$$
    4. Compare with the number of variables ($n = 2$): $$\rho(A) = \rho([A \mid \vec{b}]) = 1 < 2$$
- **Answer**: The system is **consistent with Infinitely Many Solutions**.

---

**Problem 6: Laplace Determinant Expansion of a** $4 \times 4$ **Matrix**

- **Question**: Set up the cofactor expansion along Row 1 for the matrix: $$A = \begin{bmatrix} 2 & 3 & 1 & 4 \\ 1 & 4 & 3 & 2 \\ 2 & 1 & 1 & 4 \\ 3 & 1 & 1 & 2 \end{bmatrix}$$
- **Solution**: Using the formula $\det(A) = \sum_{j=1}^{4} (-1)^{1+j} a_{1j} M_{1j}$: $$\det(A) = 2 \begin{vmatrix} 4 & 3 & 2 \\ 1 & 1 & 4 \\ 1 & 1 & 2 \end{vmatrix} - 3 \begin{vmatrix} 1 & 3 & 2 \\ 2 & 1 & 4 \\ 3 & 1 & 2 \end{vmatrix} + 1 \begin{vmatrix} 1 & 4 & 2 \\ 2 & 1 & 4 \\ 3 & 1 & 2 \end{vmatrix} - 4 \begin{vmatrix} 1 & 4 & 3 \\ 2 & 1 & 1 \\ 3 & 1 & 1 \end{vmatrix}$$

---

**Problem 7: Basis Verification for** $\mathbb{R}^3$

- **Question**: Determine whether the set $S = \left\{ \begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix}, \begin{bmatrix} 0 \\ 1 \\ 0 \end{bmatrix} \right\}$ forms a basis for $\mathbb{R}^3$.
- **Step-by-Step Solution**:
    1. **Linear Independence**: $$c_1 \begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix} + c_2 \begin{bmatrix} 0 \\ 1 \\ 0 \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix} \implies c_1 = 0, \; c_2 = 0$$ The vectors are linearly independent.
    2. **Spanning Criterion**: The span of $S$ is: $$\operatorname{span}(S) = \left\{ \begin{bmatrix} a \\ b \\ 0 \end{bmatrix} \;\middle|\; a, b \in \mathbb{R} \right\}$$ Because any vector with a non-zero third component (e.g., $[0, 0, 1]^T$) cannot be represented, $\operatorname{span}(S) \neq \mathbb{R}^3$.
    3. **Dimensional Requirement**: Since $\dim(\mathbb{R}^3) = 3$, any basis for $\mathbb{R}^3$ must contain exactly 3 linearly independent vectors. Here, $|S| = 2$.
- **Answer**: $S$ **does NOT form a basis for** $\mathbb{R}^3$.