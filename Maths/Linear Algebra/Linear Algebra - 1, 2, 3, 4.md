**1. Matrix Fundamentals & Notation**

- **Matrix**: A rectangular array of numbers or functions arranged in horizontal rows and vertical columns, denoted by capital letters ($A, B, C$).
- **Order / Size**: An $m \times n$ matrix contains $m$ rows and $n$ columns.
- **Indicial Form**: $A = [a_{ij}]$, where $1 \le i \le m$ is the row index and $1 \le j \le n$ is the column index.
- **Classification by Dimensions**:
    - **Rectangular Matrix**: $m \neq n$
    - **Square Matrix**: $m = n$
- **Linear Systems / Operations**: Expressed in standard matrix form as $A\vec{x} = \vec{y}$.

---

**2. Principal Diagonal & Trace**

- **Principal Diagonal (Leading Diagonal)**: Defined strictly for square matrices ($n \times n$), consisting of all entries $a_{ii}$ where the row index equals the column index ($a_{11}, a_{22}, \dots, a_{nn}$).
- **Trace (**$\operatorname{tr}(A)$**)**: The sum of all principal diagonal elements: $$\operatorname{tr}(A) = \sum_{i=1}^{n} a_{ii} = a_{11} + a_{22} + \dots + a_{nn}$$

---

**3. Types of Square Matrices**

- **Upper Triangular Matrix**: All entries below the principal diagonal are zero: $$a_{ij} = 0 \quad \text{for all } i > j$$
- **Lower Triangular Matrix**: All entries above the principal diagonal are zero: $$a_{ij} = 0 \quad \text{for all } i < j$$
- **Diagonal Matrix**: All non-diagonal entries are zero: $$a_{ij} = 0 \quad \text{for all } i \neq j \quad \implies A = \operatorname{diag}[a_{11}, a_{22}, \dots, a_{nn}]$$
- **Scalar Matrix**: A diagonal matrix where every diagonal element equals the same scalar value $k$: $$a_{ij} = \begin{cases} k, & i = j \\ 0, & i \neq j \end{cases}$$
- **Identity Matrix (**$I$**)**: A scalar matrix with $k = 1$ (multiplicative identity): $$a_{ij} = \begin{cases} 1, & i = j \\ 0, & i \neq j \end{cases}$$
- **Null / Zero Matrix (**$O$**)**: All entries are zero (additive identity): $$a_{ij} = 0 \quad \text{for all } i, j$$

---

**4. Matrix Operations & Transpose Rules**

- **Matrix Addition**:
    - Compatible only for matrices of the same order: $(A + B)_{ij} = a_{ij} + b_{ij}$.
    - Addition is **commutative**: $A + B = B + A$.
- **Transpose (**$A^T$ **or** $A'$**)**:
    - Formed by interchanging rows and columns: if $A$ is $m \times n$, then $A^T$ is $n \times m$, with $(A^T)_{ji} = a_{ij}$.
- **Transpose Properties**:
    - $\operatorname{tr}(A) = \operatorname{tr}(A^T)$ (diagonal elements do not change position)
    - $\det(A) = \det(A^T)$
    - $(A + B)^T = A^T + B^T$
    - $(A - B)^T = A^T - B^T$
    - $(\alpha A)^T = \alpha A^T$ (for any scalar $\alpha$)

---

**5. Symmetric & Skew-Symmetric Matrices**

- **Symmetric Matrix**:
    - Condition: $A = A^T \iff a_{ij} = a_{ji}$
- **Skew-Symmetric Matrix**:
    - Condition: $A = -A^T \iff a_{ij} = -a_{ji}$
    - **Diagonal Elements**: Must all be zero ($a_{ii} = -a_{ii} \implies 2a_{ii} = 0 \implies a_{ii} = 0$).
    - **Sum of All Elements**: Always equals zero.
- **Scalar Multiplication**: Scaling a matrix does not alter its symmetric or skew-symmetric nature.

---

**6. Matrix Decomposition Theorem**

- Any square matrix $A$ can be uniquely expressed as the sum of a symmetric matrix and a skew-symmetric matrix: $$A = \frac{1}{2}(A + A^T) + \frac{1}{2}(A - A^T)$$
    - **Symmetric Component**: $\frac{1}{2}(A + A^T)$
    - **Skew-Symmetric Component**: $\frac{1}{2}(A - A^T)$

---

**7. Element Sum Formula**

- For an $m \times n$ matrix where each entry is defined by $a_{ij} = i + j$: $$\text{Sum of all elements} = \frac{mn}{2}(m + n + 2)$$

**Problem 1: Matrix Construction from an Algebraic Rule**

- **Statement**: If $A = [a_{ij}]$ where $1 \le i \le 2$, $1 \le j \le 3$, and $a_{ij} = i + j$, find matrix $A$.
- **Matrix Order**: $2 \times 3$ ($2$ rows, $3$ columns).
- **Element Evaluation**:
    - $a_{11} = 1 + 1 = 2$
    - $a_{12} = 1 + 2 = 3$
    - $a_{13} = 1 + 3 = 4$
    - $a_{21} = 2 + 1 = 3$
    - $a_{22} = 2 + 2 = 4$
    - $a_{23} = 2 + 3 = 5$
- **Answer**: $$A = \begin{bmatrix} 2 & 3 & 4 \\ 3 & 4 & 5 \end{bmatrix}_{2 \times 3}$$

---

**Problem 2: Matrix Construction from Definite Integrals**

- **Statement**: If $A = [a_{ij}]$ where $1 \le i \le 2$, $1 \le j \le 2$, and $a_{ij} = \int_{i}^{j} x^2 \, dx$, find matrix $A$.
- **Matrix Order**: $2 \times 2$ ($2$ rows, $2$ columns).
- **Element Evaluation**:
    - $a_{11} = \int_{1}^{1} x^2 \, dx = 0$
    - $a_{12} = \int_{1}^{2} x^2 \, dx = \left[ \frac{x^3}{3} \right]_{1}^{2} = \frac{8}{3} - \frac{1}{3} = \frac{7}{3}$
    - $a_{21} = \int_{2}^{1} x^2 \, dx = \left[ \frac{x^3}{3} \right]_{2}^{1} = \frac{1}{3} - \frac{8}{3} = -\frac{7}{3}$
    - $a_{22} = \int_{2}^{2} x^2 \, dx = 0$
- **Answer**: $$A = \begin{bmatrix} 0 & \frac{7}{3} \\ -\frac{7}{3} & 0 \end{bmatrix}_{2 \times 2}$$

---

**Problem 3: Sum of Elements in a Rectangular Matrix**

- **Statement**: If $A = [a_{ij}]_{5 \times 6}$ where $a_{ij} = i + j$, find the sum of all elements of matrix $A$.
- **General Derivation for an** $m \times n$ **Matrix**:
    - Sum of row $i$: $$\sum_{j=1}^{n} (i + j) = n \cdot i + \sum_{j=1}^{n} j = n \cdot i + \frac{n(n+1)}{2}$$
    - Sum of all $m$ rows: $$\text{Total Sum} = n \sum_{i=1}^{m} i + m \left[ \frac{n(n+1)}{2} \right] = n \left[ \frac{m(m+1)}{2} \right] + m \left[ \frac{n(n+1)}{2} \right] = \frac{mn}{2}(m + n + 2)$$
- **Computation for** $m = 5$**,** $n = 6$: $$\text{Total Sum} = \frac{5 \times 6}{2}(5 + 6 + 2) = 15 \times 13 = 195$$
- **Answer**: $195$

---

**Problem 4: Sum of Elements in a Skew-Symmetric Matrix**

- **Statement**: For matrix $A = [a_{ij}]_{4 \times 4}$ defined by $a_{ij} = i^5 - j^5$ for all $1 \le i \le 4$ and $1 \le j \le 4$, find the sum of all elements of matrix $A$.
- **Solution**:
    - Check the transpose symmetry condition: $$a_{ji} = j^5 - i^5 = -(i^5 - j^5) = -a_{ij}$$
    - Since $a_{ji} = -a_{ij}$, $A$ is **skew-symmetric** ($A = -A^T$).
    - For all diagonal elements ($i = j$): $a_{ii} = i^5 - i^5 = 0$.
    - For every off-diagonal pair ($i \neq j$): $a_{ij} + a_{ji} = (i^5 - j^5) + (j^5 - i^5) = 0$.
    - Summing across all entries gives $0$.
- **Answer**: $0$

---

**Illustrative Numerical Examples**

- **Example 1 — Transposition of a Rectangular Matrix**: $$A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \\ 5 & 6 \end{bmatrix}_{3 \times 2} \implies A^T = \begin{bmatrix} 1 & 3 & 5 \\ 2 & 4 & 6 \end{bmatrix}_{2 \times 3}$$
- **Example 2 — Transposition with Scalar Multiplication**: $$A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix} \implies 2A = \begin{bmatrix} 2 & 4 \\ 6 & 8 \end{bmatrix} \implies (2A)^T = \begin{bmatrix} 2 & 6 \\ 4 & 8 \end{bmatrix} = 2A^T$$
- **Example 3 — Verification of a Symmetric Matrix**: $$A = \begin{bmatrix} 1 & 2 & 4 \\ 2 & 2 & 1 \\ 4 & 1 & 3 \end{bmatrix} \implies A^T = \begin{bmatrix} 1 & 2 & 4 \\ 2 & 2 & 1 \\ 4 & 1 & 3 \end{bmatrix} = A$$
- **Example 4 — Scalar Multiple Preserving Symmetry**: $$A = \begin{bmatrix} 1 & 2 \\ 2 & 3 \end{bmatrix} \implies 3A = \begin{bmatrix} 3 & 6 \\ 6 & 9 \end{bmatrix} \quad (\text{remains symmetric})$$

---

📝 Would you like me to generate a clean PDF worksheet combining both these solved problems and the complete theory notes?


---
### Lecture - 2
---


**1. Special Types of Square Matrices**

- **Orthogonal Matrix**:
    - **Definition**: A square matrix that preserves the length and magnitude of a vector under transformation ($|\vec{y}| = |\vec{x}|$ when $A\vec{x} = \vec{y}$), executing a pure geometric rotation.
    - **Fundamental Identity**: $A \cdot A^T = A^T \cdot A = I$.
    - **Inverse Relationship**: The transpose equals the inverse: $$A^T = A^{-1}$$
    - **Orthonormality Condition**: Rows and columns are mutually pairwise orthogonal: $$(i\text{-th row}) \cdot (j\text{-th row}) = 0 \quad \text{for all } i \neq j$$
- **Involutory Matrix**:
    - **Definition**: A square matrix that satisfies: $$A^2 = I$$
    - **Self-Inverse Property**: $A^{-1} = A$.
    - **Even/Odd Power Rule**: $$A^{2k} = I, \quad A^{2k+1} = A \quad (k \in \mathbb{Z}^+)$$ Applying the transformation an even number of times produces no net change.
- **Idempotent Matrix**:
    - **Definition**: A square matrix that satisfies: $$A^2 = A$$
    - **Operational Behavior**: Acts effectively only once; repeated applications yield identical output: $$A^n = A \quad \text{for all } n \ge 1$$
    - **Application**: Projection matrices represent standard idempotent transformations.
- **Nilpotent Matrix**:
    - **Definition**: A non-zero matrix $A$ for which there exists a positive integer $n \in \mathbb{Z}^+$ such that: $$A^n = O$$
    - **Index of Nilpotence**: The least positive integer $n$ for which $A^n = O$.
    - **Higher Powers**: $A^m = O$ for all integers $m \ge n$.

---

**2. Counting Formulas for Matrices**

- **Independent Elements in a Symmetric Matrix**:
    - For an $n \times n$ symmetric matrix, the number of independent entries is the sum of the principal diagonal and the upper triangular elements: $$\text{Independent Entries} = \frac{n(n + 1)}{2} = \frac{n^2 + n}{2}$$
- **Total Permutations of Matrices**:
    - If entries are selected from a set of $k$ distinct values with replacement, the total number of distinct symmetric matrices is: $$k^{\frac{n(n + 1)}{2}}$$
    - _Example_: For $n = 3$ with entries chosen from the $9$ non-zero digits $\{1, 2, \dots, 9\}$: $$9^{\frac{3(4)}{2}} = 9^6 = 531{,}441$$

---

**3. Matrix Multiplication: Rules & Properties**

- **Compatibility / Conformability**:
    - The product $AB$ exists if and only if: $$\text{Number of columns of } A = \text{Number of rows of } B$$
    - If $A$ is of order $m \times n$ and $B$ is of order $n \times p$, then the product $AB$ is of order $m \times p$.
- **Indicial Computation**:
    - Each element is computed via the dot product of row $i$ of $A$ and column $j$ of $B$: $$(AB)_{ij} = \sum_{k=1}^{n} a_{ik} b_{kj}$$
- **Algebraic Laws**:
    - **Non-Commutative**: In general, $AB \neq BA$. Even when $AB$ exists, $BA$ may not be defined.
    - **Associative**: $A(BC) = (AB)C$ (when dimensions are compatible).
    - **Zero Divisors**: $AB = O$ does not imply $A = O$ or $B = O$; the product of two non-zero matrices can be the null matrix.
- **Geometric Interpretation**:
    - The linear mapping $A\vec{x} = \vec{y}$ geometrically rotates, shears, and scales the vector $\vec{x}$.

---

**4. Computational Complexity of Matrix Multiplication**

For the product of $A_{m \times n}$ and $B_{n \times p}$:

- **Per Element**: Requires $n$ scalar multiplications and $n - 1$ scalar additions.
- **Total Scalar Multiplications**: $$\text{Multiplications} = m \cdot n \cdot p$$
- **Total Scalar Additions**: $$\text{Additions} = m \cdot (n - 1) \cdot p$$

---

**5. Transposition & Trace with Multiplication**

- **Reversal Law of Transposition**:
    - For two matrices: $$(AB)^T = B^T A^T$$
    - Generalized for $n$ matrices: $$(A_1 A_2 A_3 \dots A_n)^T = A_n^T A_{n-1}^T \dots A_2^T A_1^T$$
- **Cyclic Property of the Trace**:
    - If $AB$ and $BA$ are both defined, their traces are identical: $$\operatorname{tr}(AB) = \operatorname{tr}(BA)$$ _(This holds even when_ $AB$ _and_ $BA$ _have different square sizes, such as_ $3 \times 3$ _versus_ $4 \times 4$_.)_
    - For three conformable matrices: $$\operatorname{tr}(ABC) = \operatorname{tr}(BCA) = \operatorname{tr}(CAB)$$

---

**6. Algebraic Properties of Symmetric Matrix Combinations**

Let $A$ and $B$ be symmetric matrices of the same order ($A = A^T$, $B = B^T$):

- **Sum Combination (**$AB + BA$**)**: Always **symmetric**.
    - _Verification_: $(AB + BA)^T = (AB)^T + (BA)^T = B^T A^T + A^T B^T = BA + AB = AB + BA$.
- **Difference Combination (**$AB - BA$**)**: Always **skew-symmetric**.
    - _Verification_: $(AB - BA)^T = (AB)^T - (BA)^T = B^T A^T - A^T B^T = BA - AB = -(AB - BA)$.

---

**7. Matrix Structure Reference**

- **Upper Triangular**: $a_{ij} = 0$ for all $i > j$.
- **Lower Triangular**: $a_{ij} = 0$ for all $i < j$.
- **Diagonal Matrix**: $a_{ij} = 0$ for all $i \neq j$.
- **Symmetric Matrix**: $A = A^T \iff a_{ij} = a_{ji}$.
- **Skew-Symmetric Matrix**: $A = -A^T \iff a_{ij} = -a_{ji}$; principal diagonal entries are zero ($a_{ii} = 0$), and $\operatorname{tr}(A) = 0$.
- **Matrix Decomposition**: Any square matrix $A$ can be uniquely expressed as: $$A = \frac{1}{2}(A + A^T) + \frac{1}{2}(A - A^T)$$ where $\frac{1}{2}(A + A^T)$ is symmetric and $\frac{1}{2}(A - A^T)$ is skew-symmetric.

---
**Problem 1: Minimum Scalar Multiplications for Matrix Product** $ABC$

- **Problem Statement**: Find the minimum number of multiplications required to compute the product $ABC$, given the matrices: $$A_{6 \times 5}, \quad B_{5 \times 4}, \quad C_{4 \times 7}$$
- **Step-by-Step Solution**: By the associative property of matrix multiplication, the product can be computed via two different parenthesizations:
    - **Case 1:** $(AB)C$
        1. Multiplications to compute $AB$ (order $6 \times 4$): $$6 \times 5 \times 4 = 120$$
        2. Multiplications to compute $(AB)C$ (multiplying $(AB)_{6 \times 4}$ by $C_{4 \times 7}$): $$6 \times 4 \times 7 = 168$$
        3. Total scalar multiplications: $$120 + 168 = 288$$
    - **Case 2:** $A(BC)$
        1. Multiplications to compute $BC$ (order $5 \times 7$): $$5 \times 4 \times 7 = 140$$
        2. Multiplications to compute $A(BC)$ (multiplying $A_{6 \times 5}$ by $(BC)_{5 \times 7}$): $$6 \times 5 \times 7 = 210$$
        3. Total scalar multiplications: $$140 + 210 = 350$$
- **Answer**: $$\min(288, 350) = \mathbf{288}$$

---

**Problem 2: Permutations of** $3 \times 3$ **Symmetric Matrices**

- **Problem Statement**: Determine the total number of distinct $3 \times 3$ symmetric matrices that can be formed using the non-zero digits $\{1, 2, 3, \dots, 9\}$ if repetition of elements is permitted.
- **Step-by-Step Solution**:
    - For an $n \times n$ symmetric matrix $A$, the condition $a_{ij} = a_{ji}$ means all entries below the principal diagonal are determined by those above it.
    - The number of independent entries is given by: $$\text{Independent Entries} = \frac{n(n + 1)}{2} = \frac{3(3 + 1)}{2} = 6$$ _(Specifically: 3 principal diagonal entries_ $a_{11}, a_{22}, a_{33}$ _and 3 upper-triangular entries_ $a_{12}, a_{13}, a_{23}$_)._
    - With $9$ possible choices for each independent position: $$\text{Total Matrices} = 9^6 = \mathbf{531{,}441}$$
- **Answer**: $531{,}441$

---

**Problem 3: Multiplications and Additions to Compute Matrix Product** $AB$

- **Problem Statement**: Determine the number of scalar multiplications and additions required to compute the product $AB$, where: $$A \text{ is of order } 3 \times 2, \quad B \text{ is of order } 2 \times 4$$
- **Step-by-Step Solution**: For matrices $A_{m \times n}$ and $B_{n \times p}$:
    - **Total Multiplications**: $$m \cdot n \cdot p = 3 \times 2 \times 4 = \mathbf{24}$$
    - **Total Additions**: $$m \cdot (n - 1) \cdot p = 3 \times (2 - 1) \times 4 = \mathbf{12}$$
- **Answer**: $24$ **multiplications** (and $12$ additions).

---

**Problem 4: Symmetry of Matrix Product Combinations**

- **Problem Statement**: If $A$ and $B$ are two symmetric matrices of the same order, determine the nature of:
    1. $AB + BA$
    2. $AB - BA$
- **Step-by-Step Solution**: Given: $A^T = A$ and $B^T = B$.
    - **Part 1**: Let $X = AB + BA$. Taking the transpose: $$X^T = (AB + BA)^T = (AB)^T + (BA)^T = B^T A^T + A^T B^T = BA + AB = AB + BA = X$$ Since $X^T = X$, $AB + BA$ **is symmetric**.
    - **Part 2**: Let $Y = AB - BA$. Taking the transpose: $$Y^T = (AB - BA)^T = (AB)^T - (BA)^T = B^T A^T - A^T B^T = BA - AB = -(AB - BA) = -Y$$ Since $Y^T = -Y$, $AB - BA$ **is skew-symmetric**.

---

**Part 2: Worked Numerical Examples & Verifications**

- **Example 1: Verification of a Nilpotent Matrix and its Nilpotency Index** Given: $$A = \begin{bmatrix} 2 & -1 \\ 4 & -2 \end{bmatrix}$$ Computing $A^2$: $$A^2 = \begin{bmatrix} 2 & -1 \\ 4 & -2 \end{bmatrix} \begin{bmatrix} 2 & -1 \\ 4 & -2 \end{bmatrix} = \begin{bmatrix} 4 - 4 & -2 + 2 \\ 8 - 8 & -4 + 4 \end{bmatrix} = \begin{bmatrix} 0 & 0 \\ 0 & 0 \end{bmatrix} = O$$
    - Because $A \neq O$ and $A^2 = O$, $A$ is **nilpotent** with **index of nilpotence = 2**.
- **Example 2: Verification of an Involutory Matrix** Given: $$A = \begin{bmatrix} 4 & -1 \\ 15 & -4 \end{bmatrix}$$ Computing $A^2$: $$A^2 = \begin{bmatrix} 4 & -1 \\ 15 & -4 \end{bmatrix} \begin{bmatrix} 4 & -1 \\ 15 & -4 \end{bmatrix} = \begin{bmatrix} 16 - 15 & -4 + 4 \\ 60 - 60 & -15 + 16 \end{bmatrix} = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix} = I$$
    - Because $A^2 = I$, $A$ is **involutory** and satisfies $A^{-1} = A$.
- **Example 3: Verification of an Idempotent Matrix** Given: $$A = \begin{bmatrix} 4 & -1 \\ 12 & -3 \end{bmatrix}$$ Computing $A^2$: $$A^2 = \begin{bmatrix} 4 & -1 \\ 12 & -3 \end{bmatrix} \begin{bmatrix} 4 & -1 \\ 12 & -3 \end{bmatrix} = \begin{bmatrix} 16 - 12 & -4 + 3 \\ 48 - 36 & -12 + 9 \end{bmatrix} = \begin{bmatrix} 4 & -1 \\ 12 & -3 \end{bmatrix} = A$$
    - Because $A^2 = A$, $A$ is **idempotent**.
- **Example 4: Orthogonal Matrix Identity Verification** Given the rotation matrix: $$A = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix}, \quad A^T = \begin{bmatrix} \cos\theta & \sin\theta & 0 \\ -\sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix}$$ Multiplying $A A^T$: $$A A^T = \begin{bmatrix} \cos^2\theta + \sin^2\theta & 0 & 0 \\ 0 & \sin^2\theta + \cos^2\theta & 0 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix} = I$$
    - Confirms $A A^T = I$ and $A^T = A^{-1}$.
- **Example 5: Linear Vector Transformation** Applying matrix $A = \begin{bmatrix} 1 & 2 \\ 2 & 3 \end{bmatrix}$ to vector $\vec{x} = \begin{bmatrix} 2 \\ 1 \end{bmatrix}$: $$\vec{y} = A\vec{x} = \begin{bmatrix} 1 & 2 \\ 2 & 3 \end{bmatrix} \begin{bmatrix} 2 \\ 1 \end{bmatrix} = \begin{bmatrix} 1(2) + 2(1) \\ 2(2) + 3(1) \end{bmatrix} = \begin{bmatrix} 4 \\ 7 \end{bmatrix}$$
- **Example 6: Zero Divisors in Matrix Multiplication** For non-zero matrices $A = \begin{bmatrix} a & 0 \\ 0 & 0 \end{bmatrix}$ and $B = \begin{bmatrix} 0 & 0 \\ 0 & b \end{bmatrix}$ with $a, b \neq 0$: $$AB = \begin{bmatrix} a & 0 \\ 0 & 0 \end{bmatrix} \begin{bmatrix} 0 & 0 \\ 0 & b \end{bmatrix} = \begin{bmatrix} 0 & 0 \\ 0 & 0 \end{bmatrix} = O$$
    - Confirms that $AB = O$ does not necessitate $A = O$ or $B = O$.

---
### Lecture - 3
---
**1. Geometric Interpretation & Definition of Determinant**

- **Geometric Meaning**: The determinant serves as a geometric **scaling factor**, representing area scaling in 2D space and volume scaling in 3D space.
- **Jacobian Connection**: Governs change of variables in coordinate transformations via the Jacobian determinant: $$f(x,y)\,dx\,dy = g(r,\theta)\,J\left(\frac{x,y}{r,\theta}\right)dr\,d\theta$$
- **Notation**: Denoted as $|A|$ or $\det(A)$ for any square matrix $A$.
- **Formal Definition**: The sum of the products of elements of any single row (or column) with their corresponding cofactors.

---

**2. Minors, Cofactors & The Adjoint Matrix**

- **Minor (**$M_{ij}$**)**: The determinant of the submatrix obtained by deleting the $i$-th row and $j$-th column of $A$.
- **Cofactor (**$A_{ij}$**)**: Computed by applying an alternating sign factor to the minor: $$A_{ij} = (-1)^{i+j} M_{ij}$$
- **Laplace Expansion Formula**: Along row 1 of a $3 \times 3$ matrix: $$|A| = a_{11}A_{11} + a_{12}A_{12} + a_{13}A_{13}$$
- **Orthogonal Expansion Rule (Zero Sum Rule)**: The sum of the products of entries of any row (or column) with the cofactors of any _other_ row (or column) is strictly **zero**: $$a_{11}A_{21} + a_{12}A_{22} + a_{13}A_{23} = 0$$
- **Cofactor Matrix**: The matrix formed by replacing each entry $a_{ij}$ with its cofactor $A_{ij}$.
- **Adjoint Matrix (**$\operatorname{adj}(A)$**)**: The **transpose of the cofactor matrix**: $$\operatorname{adj}(A) = (\text{Cofactor Matrix})^T$$

---

**3. Inversion Identities & Singularity**

- **Fundamental Adjoint Identity**: $$A \cdot \operatorname{adj}(A) = \operatorname{adj}(A) \cdot A = |A| \cdot I$$
- **Matrix Inverse Formula**: $$A^{-1} = \frac{1}{|A|} \operatorname{adj}(A) \quad (\text{defined if and only if } |A| \neq 0)$$
- **Singular Matrix**: A matrix where $\det(A) = 0$ (cannot be inverted).
- **Non-Singular Matrix**: A matrix where $\det(A) \neq 0$ (invertible).
- **Invertibility Criterion**: A matrix $A$ possesses an inverse $A^{-1}$ if and only if $A$ is **non-singular**.

---

**4. Determinant Expansion Laws & Sarrus Rule**

- **Number of Terms in Expansion**: The complete expansion of an $n \times n$ determinant contains exactly $n!$ terms:
    - $3 \times 3 \implies 3! = 6$ terms
    - $4 \times 4 \implies 4! = 24$ terms
    - $5 \times 5 \implies 5! = 120$ terms
- **Sarrus Rule (for** $3 \times 3$**)**: Evaluated by subtracting the sum of backward diagonal products from the sum of forward diagonal products.

---

**5. Fundamental Properties of Determinants**

- **Row / Column Swapping**: Interchanging any two rows (or two columns) multiplies the determinant by $-1$; the magnitude is preserved.
- **Transpose Invariance**: Transposing a matrix leaves its determinant unchanged: $$\det(A^T) = \det(A)$$
- **Factoring Scalars**: A common scalar factor in any single row or column can be factored outside the determinant.
- **Scalar Multiplication of an** $n \times n$ **Matrix**: $$\det(k A_{n \times n}) = k^n \det(A_{n \times n})$$
- **Multiplicative Identity**: The determinant distributes over matrix multiplication: $$\det(ABCD) = |A| \cdot |B| \cdot |C| \cdot |D|$$
- **Non-Additivity**: In general, determinants are not additive: $$|A + B| \neq |A| + |B|$$
- **Triangular & Diagonal Matrices**: The determinant of an upper triangular, lower triangular, or diagonal matrix equals the **product of its principal diagonal elements**.
- **Zero Row/Column Sum Theorem**: If the sum of entries in every row (or every column) equals zero, then $|A| = 0$.

---

**6. Determinants of Special Matrix Families**

- **Skew-Symmetric Matrix of Odd Order**: $$\det(A) = 0 \quad (\text{for any skew-symmetric } A_{n \times n} \text{ where } n \text{ is odd})$$
- **Orthogonal Matrix (**$A A^T = I$**)**: $$|A| = \pm 1 \implies A^{-1} = A^T$$
- **Involutory Matrix (**$A^2 = I$**)**: $$|A| = \pm 1 \implies A^{-1} = A$$
- **Idempotent Matrix (**$A^2 = A$**)**: $$|A| = 0 \quad \text{or} \quad |A| = 1$$
- **Zero Divisors Condition**: If $AB = O$ for non-zero matrices $A$ and $B$, then **both** $A$ **and** $B$ **must be singular** ($|A| = 0$ and $|B| = 0$).

---

**7. Advanced Adjoint Determinant Formulas**

For any $n \times n$ matrix $A$:

- **Determinant of the Adjoint**: $$\det(\operatorname{adj}(A)) = |A|^{n-1}$$
- **Determinant of the Double Adjoint**: $$\det(\operatorname{adj}(\operatorname{adj}(A))) = |A|^{(n-1)^2}$$

---

**8. Quick Summary of Special Matrix Definitions**

- **Orthogonal**: $A \cdot A^T = A^T \cdot A = I$
- **Involutory**: $A^2 = I$
- **Idempotent**: $A^2 = A$
- **Nilpotent**: $A^n = O$ for $A \neq O$ (least integer $n$ is the index of nilpotence)

---

## Lecture - 3

---
**1. Geometric Interpretation & Definition of Determinant**

- **Geometric Meaning**: The determinant serves as a geometric **scaling factor**, representing area scaling in 2D space and volume scaling in 3D space.
- **Jacobian Connection**: Governs change of variables in coordinate transformations via the Jacobian determinant: $$f(x,y)\,dx\,dy = g(r,\theta)\,J\left(\frac{x,y}{r,\theta}\right)dr\,d\theta$$
- **Notation**: Denoted as $|A|$ or $\det(A)$ for any square matrix $A$.
- **Formal Definition**: The sum of the products of elements of any single row (or column) with their corresponding cofactors.

---

**2. Minors, Cofactors & The Adjoint Matrix**

- **Minor (**$M_{ij}$**)**: The determinant of the submatrix obtained by deleting the $i$-th row and $j$-th column of $A$.
- **Cofactor (**$A_{ij}$**)**: Computed by applying an alternating sign factor to the minor: $$A_{ij} = (-1)^{i+j} M_{ij}$$
- **Laplace Expansion Formula**: Along row 1 of a $3 \times 3$ matrix: $$|A| = a_{11}A_{11} + a_{12}A_{12} + a_{13}A_{13}$$
- **Orthogonal Expansion Rule (Zero Sum Rule)**: The sum of the products of entries of any row (or column) with the cofactors of any _other_ row (or column) is strictly **zero**: $$a_{11}A_{21} + a_{12}A_{22} + a_{13}A_{23} = 0$$
- **Cofactor Matrix**: The matrix formed by replacing each entry $a_{ij}$ with its cofactor $A_{ij}$.
- **Adjoint Matrix (**$\operatorname{adj}(A)$**)**: The **transpose of the cofactor matrix**: $$\operatorname{adj}(A) = (\text{Cofactor Matrix})^T$$

---

**3. Inversion Identities & Singularity**

- **Fundamental Adjoint Identity**: $$A \cdot \operatorname{adj}(A) = \operatorname{adj}(A) \cdot A = |A| \cdot I$$
- **Matrix Inverse Formula**: $$A^{-1} = \frac{1}{|A|} \operatorname{adj}(A) \quad (\text{defined if and only if } |A| \neq 0)$$
- **Singular Matrix**: A matrix where $\det(A) = 0$ (cannot be inverted).
- **Non-Singular Matrix**: A matrix where $\det(A) \neq 0$ (invertible).
- **Invertibility Criterion**: A matrix $A$ possesses an inverse $A^{-1}$ if and only if $A$ is **non-singular**.

---

**4. Determinant Expansion Laws & Sarrus Rule**

- **Number of Terms in Expansion**: The complete expansion of an $n \times n$ determinant contains exactly $n!$ terms:
    - $3 \times 3 \implies 3! = 6$ terms
    - $4 \times 4 \implies 4! = 24$ terms
    - $5 \times 5 \implies 5! = 120$ terms
- **Sarrus Rule (for** $3 \times 3$**)**: Evaluated by subtracting the sum of backward diagonal products from the sum of forward diagonal products.

---

**5. Fundamental Properties of Determinants**

- **Row / Column Swapping**: Interchanging any two rows (or two columns) multiplies the determinant by $-1$; the magnitude is preserved.
- **Transpose Invariance**: Transposing a matrix leaves its determinant unchanged: $$\det(A^T) = \det(A)$$
- **Factoring Scalars**: A common scalar factor in any single row or column can be factored outside the determinant.
- **Scalar Multiplication of an** $n \times n$ **Matrix**: $$\det(k A_{n \times n}) = k^n \det(A_{n \times n})$$
- **Multiplicative Identity**: The determinant distributes over matrix multiplication: $$\det(ABCD) = |A| \cdot |B| \cdot |C| \cdot |D|$$
- **Non-Additivity**: In general, determinants are not additive: $$|A + B| \neq |A| + |B|$$
- **Triangular & Diagonal Matrices**: The determinant of an upper triangular, lower triangular, or diagonal matrix equals the **product of its principal diagonal elements**.
- **Zero Row/Column Sum Theorem**: If the sum of entries in every row (or every column) equals zero, then $|A| = 0$.

---

**6. Determinants of Special Matrix Families**

- **Skew-Symmetric Matrix of Odd Order**: $$\det(A) = 0 \quad (\text{for any skew-symmetric } A_{n \times n} \text{ where } n \text{ is odd})$$
- **Orthogonal Matrix (**$A A^T = I$**)**: $$|A| = \pm 1 \implies A^{-1} = A^T$$
- **Involutory Matrix (**$A^2 = I$**)**: $$|A| = \pm 1 \implies A^{-1} = A$$
- **Idempotent Matrix (**$A^2 = A$**)**: $$|A| = 0 \quad \text{or} \quad |A| = 1$$
- **Zero Divisors Condition**: If $AB = O$ for non-zero matrices $A$ and $B$, then **both** $A$ **and** $B$ **must be singular** ($|A| = 0$ and $|B| = 0$).

---

**7. Advanced Adjoint Determinant Formulas**

For any $n \times n$ matrix $A$:

- **Determinant of the Adjoint**: $$\det(\operatorname{adj}(A)) = |A|^{n-1}$$
- **Determinant of the Double Adjoint**: $$\det(\operatorname{adj}(\operatorname{adj}(A))) = |A|^{(n-1)^2}$$

---

**8. Quick Summary of Special Matrix Definitions**

- **Orthogonal**: $A \cdot A^T = A^T \cdot A = I$
- **Involutory**: $A^2 = I$
- **Idempotent**: $A^2 = A$
- **Nilpotent**: $A^n = O$ for $A \neq O$ (least integer $n$ is the index of nilpotence)

**Problem 1: Maximum Determinant of a Real Symmetric Matrix**

- **Question**: If $A$ is a $2 \times 2$ real symmetric matrix with trace $12$, find the maximum possible value of $\det(A)$.
- **Given Conditions**:
    - $A$ is a $2 \times 2$ real symmetric matrix.
    - $\operatorname{tr}(A) = 12$.
- **Step-by-Step Solution**:
    1. Define the general $2 \times 2$ real symmetric matrix: $$A = \begin{bmatrix} a & b \\ b & c \end{bmatrix} \quad \text{where } a, b, c \in \mathbb{R}$$
    2. Use the given trace: $$\operatorname{tr}(A) = a + c = 12 \implies c = 12 - a$$
    3. Express the determinant: $$\det(A) = ac - b^2$$
    4. Because $b \in \mathbb{R}$, $b^2 \ge 0$. To maximize $\det(A)$, minimize $b^2$ by setting $b = 0$: $$\max(\det(A)) = \max(ac)$$
    5. **Method 1 — Calculus**:
        - Substitute $c = 12 - a$: $$ac = a(12 - a) = 12a - a^2$$
        - Differentiate with respect to $a$ and set to zero: $$\frac{d}{da}(12a - a^2) = 12 - 2a = 0 \implies a = 6$$
        - With $a = 6$, $c = 12 - 6 = 6$.
        - Maximum product: $$\max(ac) = 6 \times 6 = 36$$
    6. **Method 2 — AM-GM Inequality**: $$\frac{a + c}{2} \ge \sqrt{ac} \implies \frac{12}{2} \ge \sqrt{ac} \implies 6 \ge \sqrt{ac} \implies ac \le 36$$
- **Answer**: $36$

---

**Problem 2: Determinants of First and Higher-Order Adjoints**

- **Question**: For an $n \times n$ square matrix $A$, evaluate:
    1. $\det(\operatorname{adj}(A))$
    2. $\det(\operatorname{adj}(\operatorname{adj}(A)))$
- **Derivations**:
    - **Part 1 — First Adjoint**:
        1. By the fundamental adjoint identity: $$A \cdot \operatorname{adj}(A) = |A| \cdot I$$
        2. Take the determinant on both sides: $$\det(A \cdot \operatorname{adj}(A)) = \det(|A| \cdot I)$$
        3. Apply the multiplicative property and scalar determinant law ($\det(kI_{n \times n}) = k^n$): $$\det(A) \cdot \det(\operatorname{adj}(A)) = |A|^n$$
        4. Divide by $\det(A) = |A|$: $$\det(\operatorname{adj}(A)) = |A|^{n-1}$$
    - **Part 2 — Second Adjoint**:
        1. Let $B = \operatorname{adj}(A)$, with $|B| = \det(\operatorname{adj}(A)) = |A|^{n-1}$.
        2. Using $\det(\operatorname{adj}(B)) = |B|^{n-1}$: $$\det(\operatorname{adj}(\operatorname{adj}(A))) = \left(|A|^{n-1}\right)^{n-1} = |A|^{(n-1)^2}$$
- **Answers**:
    - $\det(\operatorname{adj}(A)) = |A|^{n-1}$
    - $\det(\operatorname{adj}(\operatorname{adj}(A))) = |A|^{(n-1)^2}$

---

**Problem 3: Number of Terms in Determinant Expansion**

- **Question**: State the general formula for the number of terms in the expansion of an $n \times n$ determinant, and calculate the number of terms for $n = 3$, $n = 4$, and $n = 5$.
- **Formula**: The expansion of an $n \times n$ determinant contains exactly $n!$ terms (by Mathematical Induction).
- **Calculations**:
    - For $n = 3$: $$3! = 6 \text{ terms}$$
    - For $n = 4$: $$4! = 4 \times 6 = 24 \text{ terms}$$ _(Expanding along Row 1 yields 4 terms, each containing a_ $3 \times 3$ _minor with 6 terms:_ $4 \times 6 = 24$_)._
    - For $n = 5$: $$5! = 120 \text{ terms}$$

---

**Part 2: Worked Exercises & Counterexamples**

---

**Exercise 4: Orthogonal Expansion Identity Verification**

- **Question**: Verify that the sum of the products of entries of Row 1 with the cofactors of Row 2 in a general $3 \times 3$ matrix is zero: $$a_{11}A_{21} + a_{12}A_{22} + a_{13}A_{23} = 0$$
- **Verification**:
    1. Substitute cofactors $A_{ij} = (-1)^{i+j} M_{ij}$:
        - $A_{21} = -(a_{12}a_{33} - a_{32}a_{13})$
        - $A_{22} = +(a_{11}a_{33} - a_{13}a_{31})$
        - $A_{23} = -(a_{11}a_{32} - a_{31}a_{12})$
    2. Multiply by the elements of Row 1: $$a_{11}A_{21} + a_{12}A_{22} + a_{13}A_{23}$$ $$= -a_{11}a_{12}a_{33} + a_{11}a_{32}a_{13} + a_{12}a_{11}a_{33} - a_{12}a_{13}a_{31} - a_{13}a_{11}a_{32} + a_{13}a_{31}a_{12}$$
    3. Combining opposite pairs: $$= (-a_{11}a_{12}a_{33} + a_{12}a_{11}a_{33}) + (a_{11}a_{32}a_{13} - a_{13}a_{11}a_{32}) + (-a_{12}a_{13}a_{31} + a_{13}a_{31}a_{12}) = 0$$

---

**Exercise 5: Non-Additivity Counterexample of Determinants**

- **Question**: Show that $|A + B| \neq |A| + |B|$ in general using a $2 \times 2$ matrix construction.
- **Construction**:
    1. Let $A$ be any $2 \times 2$ non-singular matrix (so $|A| \neq 0$).
    2. Choose $B = -A$.
    3. Compute both sides:
        - $A + B = A + (-A) = O \implies |A + B| = |O| = 0$
        - $|B| = |-A| = (-1)^2 |A| = |A|$
        - $|A| + |B| = |A| + |A| = 2|A|$
    4. Because $|A| \neq 0$, we have $0 \neq 2|A|$, establishing: $$|A + B| \neq |A| + |B|$$

---

**Part 3: Mathematical Proof Problems**

---

**Problem 6: Determinant of an Odd-Order Skew-Symmetric Matrix**

- **Question**: Prove that if $A$ is an $n \times n$ skew-symmetric matrix and $n$ is an odd integer, then $\det(A) = 0$.
- **Proof**:
    1. A skew-symmetric matrix satisfies $A = -A^T$.
    2. Take the determinant of both sides: $$\det(A) = \det(-A^T) = (-1)^n \det(A^T)$$
    3. Since transposing leaves the determinant unchanged ($\det(A^T) = \det(A)$): $$\det(A) = (-1)^n \det(A)$$
    4. Rearrange terms: $$[1 - (-1)^n] \det(A) = 0$$
    5. Because $n$ is odd, $(-1)^n = -1$, giving: $$[1 - (-1)] \det(A) = 2 \det(A) = 0 \implies \det(A) = 0$$

---

**Problem 7: Determinant and Invertibility of Orthogonal Matrices**

- **Question**: Prove that for an orthogonal matrix $A$, $\det(A) = \pm 1$, and verify whether $A^{-1}$ exists.
- **Proof**:
    1. By definition, an orthogonal matrix satisfies $A \cdot A^T = I$.
    2. Taking determinants of both sides: $$\det(A \cdot A^T) = \det(I) \implies |A| \cdot |A^T| = 1$$
    3. Since $|A^T| = |A|$: $$|A|^2 = 1 \implies |A| = \pm 1$$
    4. Because $|A| \neq 0$, $A$ is non-singular; its inverse always exists and is given by: $$A^{-1} = A^T$$

---

**Problem 8: Determinants of Involutory and Idempotent Matrices**

- **Question**: Determine all possible determinant values for:
    1. An involutory matrix ($A^2 = I$)
    2. An idempotent matrix ($A^2 = A$)
- **Proof**:
    - **Involutory**: $$A^2 = I \implies \det(A^2) = \det(I) \implies |A|^2 = 1 \implies |A| = \pm 1$$
    - **Idempotent**: $$A^2 = A \implies \det(A^2) = \det(A) \implies |A|^2 = |A|$$ $$|A|^2 - |A| = 0 \implies |A|(|A| - 1) = 0 \implies |A| = 0 \quad \text{or} \quad |A| = 1$$

---

**Problem 9: Singularity of Zero-Divisor Matrices**

- **Question**: Prove that if $AB = O$ for two non-zero matrices $A$ and $B$, then both $A$ and $B$ must be singular.
- **Proof**:
    1. Taking the determinant: $$AB = O \implies \det(AB) = \det(O) \implies |A| \cdot |B| = 0$$ Therefore, at least one matrix must have a zero determinant.
    2. **Assume** $B$ **is non-singular (**$|B| \neq 0$**)**:
        - Then $B^{-1}$ exists.
        - Post-multiply $AB = O$ by $B^{-1}$: $$(AB)B^{-1} = O \cdot B^{-1} \implies A(B B^{-1}) = O \implies A \cdot I = O \implies A = O$$
        - This contradicts the premise that $A \neq O$. Hence, $|B| = 0$.
    3. **Assume** $A$ **is non-singular (**$|A| \neq 0$**)**:
        - Then $A^{-1}$ exists.
        - Pre-multiply $AB = O$ by $A^{-1}$: $$A^{-1}(AB) = A^{-1} \cdot O \implies (A^{-1}A)B = O \implies I \cdot B = O \implies B = O$$
        - This contradicts the premise that $B \neq O$. Hence, $|A| = 0$.
    4. Therefore, both matrices must be singular ($|A| = 0$ and $|B| = 0$


---
### Lecture - 4

**1. Determinant Theorems & Row/Column Properties**

- **Fundamental Identities**:
    - For conformable square matrices, the determinant distributes across multiplication: $$|AB| = |A| \cdot |B| = |BA|$$
    - Scaling an $n \times n$ matrix by a scalar $k \in \mathbb{R}$: $$|k A_{n \times n}| = k^n |A|$$
    - Determinant of the first adjoint: $$|\operatorname{adj}(A)| = |A|^{n-1}$$
    - Determinant of the second adjoint: $$|\operatorname{adj}(\operatorname{adj}(A))| = |A|^{(n-1)^2}$$
- **Identical or Proportional Rows/Columns**: If any two rows or columns of a determinant are identical or proportional, the determinant equals $0$.
- **Zero Row or Column**: If all entries in any single row or column are zero, the determinant equals $0$.
- **Invariance Under Elementary Row/Column Operations**: The determinant remains unchanged under operations of the form: $$R_i \to R_i - \sum_{\substack{j=1 \\ j \neq i}}^{n} a_j R_j$$
- **Linear Combination of Rows/Columns**: If the sum of any $n - 1$ rows (or columns) equals the $n$-th row (or column), the determinant is strictly $0$.

---

**2. Rank of a Matrix**

- **Definition by Submatrix Minors**: A matrix $A_{m \times n}$ has rank $r$ (where $r \le \min\{m, n\}$) if:
    1. All minors of order $(r + 1) \times (r + 1)$ and higher are equal to $0$.
    2. There exists **at least one non-zero minor** of order $r \times r$.
- **Notation**: Denoted by $\rho(A) = r$.
- **Core Rank Properties**:
    1. **Dimensional Bound**: $$\rho(A_{m \times n}) \le \min\{m, n\}$$
    2. **Transpose Invariance**: $$\rho(A) = \rho(A^T)$$
    3. **Product Bound**: $$\rho(AB) \le \min\{\rho(A), \rho(B)\}$$

---

**3. Row-Echelon Form of a Matrix**

- **Definition & Structural Rules**: A matrix $A_{m \times n}$ is in **row-echelon form** if:
    1. The number of leading zeros before the first non-zero entry in any row is **strictly less** than the number of leading zeros in the row directly below it.
    2. Any rows consisting entirely of zeros are located at the **bottom** of the matrix.
- **Gaussian Elimination Procedure**:
    - Select a non-zero leading diagonal entry as the pivot ($a_{11} \neq 0$).
    - Eliminate all entries beneath the pivot across subsequent rows using row operations: $$R_i \to R_i - \left(\frac{a_{i1}}{a_{11}}\right) R_1 \quad \text{for all } i = 2, 3, \dots, m$$
    - Repeat iteratively for each subsequent diagonal pivot.
    - If a zero appears in a pivot position, perform a row swap ($R_i \leftrightarrow R_j$) with a lower row.
- **Rank from Row-Echelon Form**: $$\rho(A) = \text{Number of non-zero rows in the row-echelon form}$$

---

**4. Linear Combinations of Vectors**

- **Definition**: Given a set of vectors $\vec{x}_1, \vec{x}_2, \dots, \vec{x}_n \in \mathbb{R}^n$, an expression formed by multiplying each vector by a scalar and summing them is a **linear combination**: $$\vec{x} = \sum_{i=1}^{n} c_i \vec{x}_i = c_1 \vec{x}_1 + c_2 \vec{x}_2 + \dots + c_n \vec{x}_n$$ where $c_1, c_2, \dots, c_n \in \mathbb{R}$.

---

**5. Linear Independence vs. Linear Dependence**

- **Linearly Independent Vectors**:
    - A set of vectors is **linearly independent** if: $$c_1 \vec{x}_1 + c_2 \vec{x}_2 + \dots + c_n \vec{x}_n = \vec{0}$$ holds **only** when all coefficients are simultaneously zero: $$c_1 = c_2 = \dots = c_n = 0$$
- **Linearly Dependent Vectors**:
    - A set of vectors is **linearly dependent** if: $$c_1 \vec{x}_1 + c_2 \vec{x}_2 + \dots + c_n \vec{x}_n = \vec{0}$$ holds for coefficients $c_1, c_2, \dots, c_n$ that are **not all zero simultaneously**.
    - Equivalently, at least one vector can be written as a linear combination of the remaining vectors.

---

**6. Determinant Criterion for Linear Independence**

- For a set of $n$ vectors $\vec{x}_1, \vec{x}_2, \dots, \vec{x}_n \in \mathbb{R}^n$ arranged as column vectors in a matrix:
    - **Linearly Dependent**: $$\det([\vec{x}_1, \vec{x}_2, \dots, \vec{x}_n]) = 0$$
    - **Linearly Independent**: $$\det([\vec{x}_1, \vec{x}_2, \dots, \vec{x}_n]) \neq 0$$

**Problem 1: Determinant Evaluation via Row Dependencies**

- **Question**: Evaluate the determinant of the matrix: $$A = \begin{bmatrix} 12 & 3 & 4 \\ 7 & 8 & 9 \\ 19 & 11 & 13 \end{bmatrix}$$
- **Step-by-Step Solution**:
    1. Observe the algebraic relationship between the rows:
        - First row: $R_1 = \begin{bmatrix} 12 & 3 & 4 \end{bmatrix}$
        - Second row: $R_2 = \begin{bmatrix} 7 & 8 & 9 \end{bmatrix}$
        - Third row: $R_3 = \begin{bmatrix} 19 & 11 & 13 \end{bmatrix}$
    2. Notice that the third row is the exact sum of the first two rows: $$R_1 + R_2 = \begin{bmatrix} 12+7 & 3+8 & 4+9 \end{bmatrix} = \begin{bmatrix} 19 & 11 & 13 \end{bmatrix} = R_3$$
    3. Apply the elementary row operation $R_3 \to R_3 - (R_1 + R_2)$: $$|A| = \begin{vmatrix} 12 & 3 & 4 \\ 7 & 8 & 9 \\ 19 & 11 & 13 \end{vmatrix} = \begin{vmatrix} 12 & 3 & 4 \\ 7 & 8 & 9 \\ 0 & 0 & 0 \end{vmatrix}$$
    4. Since an entire row consists of zeros, the determinant evaluates to zero.
- **Answer**: $0$

---

**Problem 2: Finding Matrix Rank Using Submatrix Minors**

- **Question**: Find the rank $\rho(A)$ of the $3 \times 4$ matrix using the minor method: $$A = \begin{bmatrix} 1 & 2 & 1 & 1 \\ 2 & 3 & 2 & 4 \\ 2 & 4 & 2 & 2 \end{bmatrix}$$
- **Step-by-Step Solution**:
    1. **Dimensional Constraint**: The maximum possible rank is $\min(3, 4) = 3$.
    2. **Evaluating all possible** $3 \times 3$ **Minors**: Observe that Row 3 is twice Row 1: $R_3 = 2R_1$ (12$ = 2$13). Because Row 1 and Row 3 are proportional in every $3 \times 3$ submatrix, all four possible $3 \times 3$ minors vanish:
        - Minor formed by columns 1, 2, 3: $$\begin{vmatrix} 1 & 2 & 1 \\ 2 & 3 & 2 \\ 2 & 4 & 2 \end{vmatrix} = 0$$
        - Minor formed by columns 1, 2, 4: $$\begin{vmatrix} 1 & 2 & 1 \\ 2 & 3 & 4 \\ 2 & 4 & 2 \end{vmatrix} = 0$$
        - Minor formed by columns 1, 3, 4: $$\begin{vmatrix} 1 & 1 & 1 \\ 2 & 2 & 4 \\ 2 & 2 & 2 \end{vmatrix} = 0$$
        - Minor formed by columns 2, 3, 4: $$\begin{vmatrix} 2 & 1 & 1 \\ 3 & 2 & 4 \\ 4 & 2 & 2 \end{vmatrix} = 0$$ Since all $3 \times 3$ minors are zero, $\rho(A) < 3$.
    3. **Evaluating** $2 \times 2$ **Minors**: Select the top-left $2 \times 2$ submatrix: $$\begin{vmatrix} 1 & 2 \\ 2 & 3 \end{vmatrix} = (1)(3) - (2)(2) = 3 - 4 = -1 \neq 0$$
    4. Since there exists at least one non-zero $2 \times 2$ minor and every larger minor is zero, the rank is 2.
- **Answer**: $\rho(A) = 2$

---

**Problem 3: Finding Matrix Rank via Row-Echelon Form (Gaussian Elimination)**

- **Question**: Reduce the matrix $A$ to row-echelon form using elementary row transformations, and determine its rank: $$A = \begin{bmatrix} 1 & 2 & 3 & 4 & 5 \\ 2 & 4 & 0 & 8 & 4 \\ 3 & 6 & 9 & 12 & 15 \\ 4 & 1 & 7 & 2 & 3 \end{bmatrix}_{4 \times 5}$$
- **Step-by-Step Solution**:
    1. **Step 1: Eliminate entries below the first pivot (**$a_{11} = 1$**)**:
        - $R_2 \to R_2 - 2R_1$: $$\begin{bmatrix} 2 & 4 & 0 & 8 & 4 \end{bmatrix} - 2\begin{bmatrix} 1 & 2 & 3 & 4 & 5 \end{bmatrix} = \begin{bmatrix} 0 & 0 & -6 & 0 & -6 \end{bmatrix}$$
        - $R_3 \to R_3 - 3R_1$: $$\begin{bmatrix} 3 & 6 & 9 & 12 & 15 \end{bmatrix} - 3\begin{bmatrix} 1 & 2 & 3 & 4 & 5 \end{bmatrix} = \begin{bmatrix} 0 & 0 & 0 & 0 & 0 \end{bmatrix}$$
        - $R_4 \to R_4 - 4R_1$: $$\begin{bmatrix} 4 & 1 & 7 & 2 & 3 \end{bmatrix} - 4\begin{bmatrix} 1 & 2 & 3 & 4 & 5 \end{bmatrix} = \begin{bmatrix} 0 & -7 & -5 & -14 & -17 \end{bmatrix}$$ The matrix becomes: $$A \sim \begin{bmatrix} 1 & 2 & 3 & 4 & 5 \\ 0 & 0 & -6 & 0 & -6 \\ 0 & 0 & 0 & 0 & 0 \\ 0 & -7 & -5 & -14 & -17 \end{bmatrix}$$
    2. **Step 2: Move the zero row to the bottom**:
        - Swap $R_3 \leftrightarrow R_4$: $$A \sim \begin{bmatrix} 1 & 2 & 3 & 4 & 5 \\ 0 & 0 & -6 & 0 & -6 \\ 0 & -7 & -5 & -14 & -17 \\ 0 & 0 & 0 & 0 & 0 \end{bmatrix}$$
    3. **Step 3: Eliminate the zero pivot and order the leading zeros**:
        - Swap $R_2 \leftrightarrow R_3$ so the leading non-zero entries advance strictly to the right: $$A \sim \begin{bmatrix} 1 & 2 & 3 & 4 & 5 \\ 0 & -7 & -5 & -14 & -17 \\ 0 & 0 & -6 & 0 & -6 \\ 0 & 0 & 0 & 0 & 0 \end{bmatrix}$$
    4. **Step 4: Check row-echelon conditions**:
        - Row 1 has 0 leading zeros.
        - Row 2 has 1 leading zero.
        - Row 3 has 2 leading zeros.
        - Row 4 is completely zero and located at the bottom.
        - The number of leading zeros strictly increases ($0 < 1 < 2$).
    5. The number of non-zero rows in row-echelon form is 3.
- **Answer**: $\rho(A) = 3$

---

**Problem 4: Identification of Row-Echelon Form**

- **Question**: Determine whether each of the following matrices is in row-echelon form: $$(a)\ A = \begin{bmatrix} 1 & 2 & 3 & 4 \\ 0 & 1 & 0 & 2 \\ 0 & 0 & 0 & 4 \end{bmatrix}, \qquad (b)\ B = \begin{bmatrix} 0 & 1 & 2 & 3 \\ 0 & 2 & 0 & 4 \\ 0 & 0 & 2 & 1 \end{bmatrix}$$
- **Step-by-Step Analysis**:
    - **Matrix** $A$:
        - Row 1 leading non-zero element is at column 1 (0 leading zeros).
        - Row 2 leading non-zero element is at column 2 (1 leading zero).
        - Row 3 leading non-zero element is at column 4 (3 leading zeros).
        - Since $0 < 1 < 3$, the leading zeros strictly increase.
        - **Conclusion**: **Matrix** $A$ **is in row-echelon form**.
    - **Matrix** $B$:
        - Row 1 leading non-zero element is at column 2 (1 leading zero).
        - Row 2 leading non-zero element is at### **Problem 1: Determinant Evaluation via Row Dependencies**

---


**Problem 5: Testing Linear Independence of Vectors**

- **Question**: Test whether the following vectors in $\mathbb{R}^3$ are linearly independent: $$\vec{x}_1 = \begin{bmatrix} 2 \\ 0 \\ 0 \end{bmatrix}, \quad \vec{x}_2 = \begin{bmatrix} 0 \\ 3 \\ 0 \end{bmatrix}, \quad \vec{x}_3 = \begin{bmatrix} 0 \\ 0 \\ 4 \end{bmatrix}$$
- **Step-by-Step Solution**:
    1. Set up the linear combination equal to the zero vector: $$c_1 \vec{x}_1 + c_2 \vec{x}_2 + c_3 \vec{x}_3 = \vec{0}$$ $$c_1 \begin{bmatrix} 2 \\ 0 \\ 0 \end{bmatrix} + c_2 \begin{bmatrix} 0 \\ 3 \\ 0 \end{bmatrix} + c_3 \begin{bmatrix} 0 \\ 0 \\ 4 \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}$$
    2. Combine into a single vector equation: $$\begin{bmatrix} 2c_1 \\ 3c_2 \\ 4c_3 \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}$$
    3. Equating components:
        - $2c_1 = 0 \implies c_1 = 0$
        - $3c_2 = 0 \implies c_2 = 0$
        - $4c_3 = 0 \implies c_3 = 0$
    4. The only solution is the trivial solution $c_1 = c_2 = c_3 = 0$.
- **Final Answer**: The vectors are **linearly independent**.

---

**Problem 6: Determining Linear Dependence and Linear Relations**

- **Question**: Show that the vectors are linearly dependent, and find the linear relation expressing one vector as a combination of the others: $$\vec{x}_1 = \begin{bmatrix} 1 \\ 1 \\ 2 \end{bmatrix}, \quad \vec{x}_2 = \begin{bmatrix} 1 \\ 3 \\ 1 \end{bmatrix}, \quad \vec{x}_3 = \begin{bmatrix} 3 \\ 7 \\ 4 \end{bmatrix}, \quad \vec{x}_4 = \begin{bmatrix} 1 \\ 2 \\ -1 \end{bmatrix}$$
- **Step-by-Step Solution**:
    1. Consider the linear combination of the first three vectors: $$c_1 \vec{x}_1 + c_2 \vec{x}_2 + c_3 \vec{x}_3 = \vec{0}$$ $$c_1 \begin{bmatrix} 1 \\ 1 \\ 2 \end{bmatrix} + c_2 \begin{bmatrix} 1 \\ 3 \\ 1 \end{bmatrix} + c_3 \begin{bmatrix} 3 \\ 7 \\ 4 \end{bmatrix} = \begin{bmatrix} c_1 + c_2 + 3c_3 \\ c_1 + 3c_2 + 7c_3 \\ 2c_1 + c_2 + 4c_3 \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}$$
    2. Solve the linear system:
        - Equation 1: $c_1 + c_2 + 3c_3 = 0$
        - Equation 2: $c_1 + 3c_2 + 7c_3 = 0$
        - Equation 3: $2c_1 + c_2 + 4c_3 = 0$
    3. Subtract Equation 1 from Equation 2: $$(c_1 + 3c_2 + 7c_3) - (c_1 + c_2 + 3c_3) = 2c_2 + 4c_3 = 0 \implies c_2 = -2c_3$$
    4. Substitute $c_2 = -2c_3$ into Equation 1: $$c_1 + (-2c_3) + 3c_3 = 0 \implies c_1 + c_3 = 0 \implies c_1 = -c_3$$
    5. Choose $c_3 = -1$: $$c_1 = 1, \quad c_2 = 2, \quad c_3 = -1$$
    6. Verify in Equation 3: $$2(1) + (2) + 4(-1) = 2 + 2 - 4 = 0 \quad \text{(consistent)}$$
    7. Form the non-trivial linear combination with all four vectors: $$1\vec{x}_1 + 2\vec{x}_2 - 1\vec{x}_3 + 0\vec{x}_4 = \vec{0}$$
    8. Rearranging gives the explicit dependence relation: $$\vec{x}_3 = \vec{x}_1 + 2\vec{x}_2$$
- **Final Answer**: The vectors are **linearly dependent**, satisfying $\mathbf{\vec{x}_3 = \vec{x}_1 + 2\vec{x}_2}$.

---

**Problem 7: Determinant Criterion for Vector Dependence**

- **Question**: Using the determinant method, verify that the column vectors $\vec{x}_1 = \begin{bmatrix} 1 \\ 1 \\ 2 \end{bmatrix}$, $\vec{x}_2 = \begin{bmatrix} 1 \\ 3 \\ 1 \end{bmatrix}$, and $\vec{x}_3 = \begin{bmatrix} 3 \\ 7 \\ 4 \end{bmatrix}$ are linearly dependent.
- **Step-by-Step Solution**:
    1. Construct matrix $A$ using the vectors as columns: $$A = [\vec{x}_1 \quad \vec{x}_2 \quad \vec{x}_3] = \begin{bmatrix} 1 & 1 & 3 \\ 1 & 3 & 7 \\ 2 & 1 & 4 \end{bmatrix}$$
    2. Because $\vec{x}_3 = \vec{x}_1 + 2\vec{x}_2$, apply the column operation $C_3 \to C_3 - (C_1 + 2C_2)$:
        - Column 1: $\begin{bmatrix} 1 \\ 1 \\ 2 \end{bmatrix}$
        - Column 2: $\begin{bmatrix} 1 \\ 3 \\ 1 \end{bmatrix}$
        - Updated Column 3: $$\begin{bmatrix} 3 \\ 7 \\ 4 \end{bmatrix} - \left(\begin{bmatrix} 1 \\ 1 \\ 2 \end{bmatrix} + 2\begin{bmatrix} 1 \\ 3 \\ 1 \end{bmatrix}\right) = \begin{bmatrix} 3 - (1 + 2) \\ 7 - (1 + 6) \\ 4 - (2 + 2) \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}$$
    3. The matrix becomes: $$\det(A) = \begin{vmatrix} 1 & 1 & 0 \\ 1 & 3 & 0 \\ 2 & 1 & 0 \end{vmatrix} = 0$$
    4. By theorem, since $\det([\vec{x}_1, \vec{x}_2, \vec{x}_3]) = 0$, the set of vectors is linearly dependent.
- **Final Answer**: $\det(A) = 0$, confirming that the vectors are **linearly dependent**.
### Lecture - 4

**1. Determinant Theorems & Row/Column Properties**

- **Fundamental Identities**:
    - For conformable square matrices, the determinant distributes across multiplication: $$|AB| = |A| \cdot |B| = |BA|$$
    - Scaling an $n \times n$ matrix by a scalar $k \in \mathbb{R}$: $$|k A_{n \times n}| = k^n |A|$$
    - Determinant of the first adjoint: $$|\operatorname{adj}(A)| = |A|^{n-1}$$
    - Determinant of the second adjoint: $$|\operatorname{adj}(\operatorname{adj}(A))| = |A|^{(n-1)^2}$$
- **Identical or Proportional Rows/Columns**: If any two rows or columns of a determinant are identical or proportional, the determinant equals $0$.
- **Zero Row or Column**: If all entries in any single row or column are zero, the determinant equals $0$.
- **Invariance Under Elementary Row/Column Operations**: The determinant remains unchanged under operations of the form: $$R_i \to R_i - \sum_{\substack{j=1 \\ j \neq i}}^{n} a_j R_j$$
- **Linear Combination of Rows/Columns**: If the sum of any $n - 1$ rows (or columns) equals the $n$-th row (or column), the determinant is strictly $0$.

---

**2. Rank of a Matrix**

- **Definition by Submatrix Minors**: A matrix $A_{m \times n}$ has rank $r$ (where $r \le \min\{m, n\}$) if:
    1. All minors of order $(r + 1) \times (r + 1)$ and higher are equal to $0$.
    2. There exists **at least one non-zero minor** of order $r \times r$.
- **Notation**: Denoted by $\rho(A) = r$.
- **Core Rank Properties**:
    1. **Dimensional Bound**: $$\rho(A_{m \times n}) \le \min\{m, n\}$$
    2. **Transpose Invariance**: $$\rho(A) = \rho(A^T)$$
    3. **Product Bound**: $$\rho(AB) \le \min\{\rho(A), \rho(B)\}$$

---

**3. Row-Echelon Form of a Matrix**

- **Definition & Structural Rules**: A matrix $A_{m \times n}$ is in **row-echelon form** if:
    1. The number of leading zeros before the first non-zero entry in any row is **strictly less** than the number of leading zeros in the row directly below it.
    2. Any rows consisting entirely of zeros are located at the **bottom** of the matrix.
- **Gaussian Elimination Procedure**:
    - Select a non-zero leading diagonal entry as the pivot ($a_{11} \neq 0$).
    - Eliminate all entries beneath the pivot across subsequent rows using row operations: $$R_i \to R_i - \left(\frac{a_{i1}}{a_{11}}\right) R_1 \quad \text{for all } i = 2, 3, \dots, m$$
    - Repeat iteratively for each subsequent diagonal pivot.
    - If a zero appears in a pivot position, perform a row swap ($R_i \leftrightarrow R_j$) with a lower row.
- **Rank from Row-Echelon Form**: $$\rho(A) = \text{Number of non-zero rows in the row-echelon form}$$

---

**4. Linear Combinations of Vectors**

- **Definition**: Given a set of vectors $\vec{x}_1, \vec{x}_2, \dots, \vec{x}_n \in \mathbb{R}^n$, an expression formed by multiplying each vector by a scalar and summing them is a **linear combination**: $$\vec{x} = \sum_{i=1}^{n} c_i \vec{x}_i = c_1 \vec{x}_1 + c_2 \vec{x}_2 + \dots + c_n \vec{x}_n$$ where $c_1, c_2, \dots, c_n \in \mathbb{R}$.

---

**5. Linear Independence vs. Linear Dependence**

- **Linearly Independent Vectors**:
    - A set of vectors is **linearly independent** if: $$c_1 \vec{x}_1 + c_2 \vec{x}_2 + \dots + c_n \vec{x}_n = \vec{0}$$ holds **only** when all coefficients are simultaneously zero: $$c_1 = c_2 = \dots = c_n = 0$$
- **Linearly Dependent Vectors**:
    - A set of vectors is **linearly dependent** if: $$c_1 \vec{x}_1 + c_2 \vec{x}_2 + \dots + c_n \vec{x}_n = \vec{0}$$ holds for coefficients $c_1, c_2, \dots, c_n$ that are **not all zero simultaneously**.
    - Equivalently, at least one vector can be written as a linear combination of the remaining vectors.

---

**6. Determinant Criterion for Linear Independence**

- For a set of $n$ vectors $\vec{x}_1, \vec{x}_2, \dots, \vec{x}_n \in \mathbb{R}^n$ arranged as column vectors in a matrix:
    - **Linearly Dependent**: $$\det([\vec{x}_1, \vec{x}_2, \dots, \vec{x}_n]) = 0$$
    - **Linearly Independent**: $$\det([\vec{x}_1, \vec{x}_2, \dots, \vec{x}_n]) \neq 0$$

**Problem 1: Determinant Evaluation via Row Dependencies**

- **Question**: Evaluate the determinant of the matrix: $$A = \begin{bmatrix} 12 & 3 & 4 \\ 7 & 8 & 9 \\ 19 & 11 & 13 \end{bmatrix}$$
- **Step-by-Step Solution**:
    1. Observe the algebraic relationship between the rows:
        - First row: $R_1 = \begin{bmatrix} 12 & 3 & 4 \end{bmatrix}$
        - Second row: $R_2 = \begin{bmatrix} 7 & 8 & 9 \end{bmatrix}$
        - Third row: $R_3 = \begin{bmatrix} 19 & 11 & 13 \end{bmatrix}$
    2. Notice that the third row is the exact sum of the first two rows: $$R_1 + R_2 = \begin{bmatrix} 12+7 & 3+8 & 4+9 \end{bmatrix} = \begin{bmatrix} 19 & 11 & 13 \end{bmatrix} = R_3$$
    3. Apply the elementary row operation $R_3 \to R_3 - (R_1 + R_2)$: $$|A| = \begin{vmatrix} 12 & 3 & 4 \\ 7 & 8 & 9 \\ 19 & 11 & 13 \end{vmatrix} = \begin{vmatrix} 12 & 3 & 4 \\ 7 & 8 & 9 \\ 0 & 0 & 0 \end{vmatrix}$$
    4. Since an entire row consists of zeros, the determinant evaluates to zero.
- **Answer**: $0$

---

**Problem 2: Finding Matrix Rank Using Submatrix Minors**

- **Question**: Find the rank $\rho(A)$ of the $3 \times 4$ matrix using the minor method: $$A = \begin{bmatrix} 1 & 2 & 1 & 1 \\ 2 & 3 & 2 & 4 \\ 2 & 4 & 2 & 2 \end{bmatrix}$$
- **Step-by-Step Solution**:
    1. **Dimensional Constraint**: The maximum possible rank is $\min(3, 4) = 3$.
    2. **Evaluating all possible** $3 \times 3$ **Minors**: Observe that Row 3 is twice Row 1: $R_3 = 2R_1$ (12$ = 2$13). Because Row 1 and Row 3 are proportional in every $3 \times 3$ submatrix, all four possible $3 \times 3$ minors vanish:
        - Minor formed by columns 1, 2, 3: $$\begin{vmatrix} 1 & 2 & 1 \\ 2 & 3 & 2 \\ 2 & 4 & 2 \end{vmatrix} = 0$$
        - Minor formed by columns 1, 2, 4: $$\begin{vmatrix} 1 & 2 & 1 \\ 2 & 3 & 4 \\ 2 & 4 & 2 \end{vmatrix} = 0$$
        - Minor formed by columns 1, 3, 4: $$\begin{vmatrix} 1 & 1 & 1 \\ 2 & 2 & 4 \\ 2 & 2 & 2 \end{vmatrix} = 0$$
        - Minor formed by columns 2, 3, 4: $$\begin{vmatrix} 2 & 1 & 1 \\ 3 & 2 & 4 \\ 4 & 2 & 2 \end{vmatrix} = 0$$ Since all $3 \times 3$ minors are zero, $\rho(A) < 3$.
    3. **Evaluating** $2 \times 2$ **Minors**: Select the top-left $2 \times 2$ submatrix: $$\begin{vmatrix} 1 & 2 \\ 2 & 3 \end{vmatrix} = (1)(3) - (2)(2) = 3 - 4 = -1 \neq 0$$
    4. Since there exists at least one non-zero $2 \times 2$ minor and every larger minor is zero, the rank is 2.
- **Answer**: $\rho(A) = 2$

---

**Problem 3: Finding Matrix Rank via Row-Echelon Form (Gaussian Elimination)**

- **Question**: Reduce the matrix $A$ to row-echelon form using elementary row transformations, and determine its rank: $$A = \begin{bmatrix} 1 & 2 & 3 & 4 & 5 \\ 2 & 4 & 0 & 8 & 4 \\ 3 & 6 & 9 & 12 & 15 \\ 4 & 1 & 7 & 2 & 3 \end{bmatrix}_{4 \times 5}$$
- **Step-by-Step Solution**:
    1. **Step 1: Eliminate entries below the first pivot (**$a_{11} = 1$**)**:
        - $R_2 \to R_2 - 2R_1$: $$\begin{bmatrix} 2 & 4 & 0 & 8 & 4 \end{bmatrix} - 2\begin{bmatrix} 1 & 2 & 3 & 4 & 5 \end{bmatrix} = \begin{bmatrix} 0 & 0 & -6 & 0 & -6 \end{bmatrix}$$
        - $R_3 \to R_3 - 3R_1$: $$\begin{bmatrix} 3 & 6 & 9 & 12 & 15 \end{bmatrix} - 3\begin{bmatrix} 1 & 2 & 3 & 4 & 5 \end{bmatrix} = \begin{bmatrix} 0 & 0 & 0 & 0 & 0 \end{bmatrix}$$
        - $R_4 \to R_4 - 4R_1$: $$\begin{bmatrix} 4 & 1 & 7 & 2 & 3 \end{bmatrix} - 4\begin{bmatrix} 1 & 2 & 3 & 4 & 5 \end{bmatrix} = \begin{bmatrix} 0 & -7 & -5 & -14 & -17 \end{bmatrix}$$ The matrix becomes: $$A \sim \begin{bmatrix} 1 & 2 & 3 & 4 & 5 \\ 0 & 0 & -6 & 0 & -6 \\ 0 & 0 & 0 & 0 & 0 \\ 0 & -7 & -5 & -14 & -17 \end{bmatrix}$$
    2. **Step 2: Move the zero row to the bottom**:
        - Swap $R_3 \leftrightarrow R_4$: $$A \sim \begin{bmatrix} 1 & 2 & 3 & 4 & 5 \\ 0 & 0 & -6 & 0 & -6 \\ 0 & -7 & -5 & -14 & -17 \\ 0 & 0 & 0 & 0 & 0 \end{bmatrix}$$
    3. **Step 3: Eliminate the zero pivot and order the leading zeros**:
        - Swap $R_2 \leftrightarrow R_3$ so the leading non-zero entries advance strictly to the right: $$A \sim \begin{bmatrix} 1 & 2 & 3 & 4 & 5 \\ 0 & -7 & -5 & -14 & -17 \\ 0 & 0 & -6 & 0 & -6 \\ 0 & 0 & 0 & 0 & 0 \end{bmatrix}$$
    4. **Step 4: Check row-echelon conditions**:
        - Row 1 has 0 leading zeros.
        - Row 2 has 1 leading zero.
        - Row 3 has 2 leading zeros.
        - Row 4 is completely zero and located at the bottom.
        - The number of leading zeros strictly increases ($0 < 1 < 2$).
    5. The number of non-zero rows in row-echelon form is 3.
- **Answer**: $\rho(A) = 3$

---

**Problem 4: Identification of Row-Echelon Form**

- **Question**: Determine whether each of the following matrices is in row-echelon form: $$(a)\ A = \begin{bmatrix} 1 & 2 & 3 & 4 \\ 0 & 1 & 0 & 2 \\ 0 & 0 & 0 & 4 \end{bmatrix}, \qquad (b)\ B = \begin{bmatrix} 0 & 1 & 2 & 3 \\ 0 & 2 & 0 & 4 \\ 0 & 0 & 2 & 1 \end{bmatrix}$$
- **Step-by-Step Analysis**:
    - **Matrix** $A$:
        - Row 1 leading non-zero element is at column 1 (0 leading zeros).
        - Row 2 leading non-zero element is at column 2 (1 leading zero).
        - Row 3 leading non-zero element is at column 4 (3 leading zeros).
        - Since $0 < 1 < 3$, the leading zeros strictly increase.
        - **Conclusion**: **Matrix** $A$ **is in row-echelon form**.
    - **Matrix** $B$:
        - Row 1 leading non-zero element is at column 2 (1 leading zero).
        - Row 2 leading non-zero element is at### **Problem 1: Determinant Evaluation via Row Dependencies**

---

**Problem 5: Testing Linear Independence of Vectors**

- **Question**: Test whether the following vectors in $\mathbb{R}^3$ are linearly independent: $$\vec{x}_1 = \begin{bmatrix} 2 \\ 0 \\ 0 \end{bmatrix}, \quad \vec{x}_2 = \begin{bmatrix} 0 \\ 3 \\ 0 \end{bmatrix}, \quad \vec{x}_3 = \begin{bmatrix} 0 \\ 0 \\ 4 \end{bmatrix}$$
- **Step-by-Step Solution**:
    1. Set up the linear combination equal to the zero vector: $$c_1 \vec{x}_1 + c_2 \vec{x}_2 + c_3 \vec{x}_3 = \vec{0}$$ $$c_1 \begin{bmatrix} 2 \\ 0 \\ 0 \end{bmatrix} + c_2 \begin{bmatrix} 0 \\ 3 \\ 0 \end{bmatrix} + c_3 \begin{bmatrix} 0 \\ 0 \\ 4 \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}$$
    2. Combine into a single vector equation: $$\begin{bmatrix} 2c_1 \\ 3c_2 \\ 4c_3 \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}$$
    3. Equating components:
        - $2c_1 = 0 \implies c_1 = 0$
        - $3c_2 = 0 \implies c_2 = 0$
        - $4c_3 = 0 \implies c_3 = 0$
    4. The only solution is the trivial solution $c_1 = c_2 = c_3 = 0$.
- **Final Answer**: The vectors are **linearly independent**.

---

**Problem 6: Determining Linear Dependence and Linear Relations**

- **Question**: Show that the vectors are linearly dependent, and find the linear relation expressing one vector as a combination of the others: $$\vec{x}_1 = \begin{bmatrix} 1 \\ 1 \\ 2 \end{bmatrix}, \quad \vec{x}_2 = \begin{bmatrix} 1 \\ 3 \\ 1 \end{bmatrix}, \quad \vec{x}_3 = \begin{bmatrix} 3 \\ 7 \\ 4 \end{bmatrix}, \quad \vec{x}_4 = \begin{bmatrix} 1 \\ 2 \\ -1 \end{bmatrix}$$
- **Step-by-Step Solution**:
    1. Consider the linear combination of the first three vectors: $$c_1 \vec{x}_1 + c_2 \vec{x}_2 + c_3 \vec{x}_3 = \vec{0}$$ $$c_1 \begin{bmatrix} 1 \\ 1 \\ 2 \end{bmatrix} + c_2 \begin{bmatrix} 1 \\ 3 \\ 1 \end{bmatrix} + c_3 \begin{bmatrix} 3 \\ 7 \\ 4 \end{bmatrix} = \begin{bmatrix} c_1 + c_2 + 3c_3 \\ c_1 + 3c_2 + 7c_3 \\ 2c_1 + c_2 + 4c_3 \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}$$
    2. Solve the linear system:
        - Equation 1: $c_1 + c_2 + 3c_3 = 0$
        - Equation 2: $c_1 + 3c_2 + 7c_3 = 0$
        - Equation 3: $2c_1 + c_2 + 4c_3 = 0$
    3. Subtract Equation 1 from Equation 2: $$(c_1 + 3c_2 + 7c_3) - (c_1 + c_2 + 3c_3) = 2c_2 + 4c_3 = 0 \implies c_2 = -2c_3$$
    4. Substitute $c_2 = -2c_3$ into Equation 1: $$c_1 + (-2c_3) + 3c_3 = 0 \implies c_1 + c_3 = 0 \implies c_1 = -c_3$$
    5. Choose $c_3 = -1$: $$c_1 = 1, \quad c_2 = 2, \quad c_3 = -1$$
    6. Verify in Equation 3: $$2(1) + (2) + 4(-1) = 2 + 2 - 4 = 0 \quad \text{(consistent)}$$
    7. Form the non-trivial linear combination with all four vectors: $$1\vec{x}_1 + 2\vec{x}_2 - 1\vec{x}_3 + 0\vec{x}_4 = \vec{0}$$
    8. Rearranging gives the explicit dependence relation: $$\vec{x}_3 = \vec{x}_1 + 2\vec{x}_2$$
- **Final Answer**: The vectors are **linearly dependent**, satisfying $\mathbf{\vec{x}_3 = \vec{x}_1 + 2\vec{x}_2}$.

---

**Problem 7: Determinant Criterion for Vector Dependence**

- **Question**: Using the determinant method, verify that the column vectors $\vec{x}_1 = \begin{bmatrix} 1 \\ 1 \\ 2 \end{bmatrix}$, $\vec{x}_2 = \begin{bmatrix} 1 \\ 3 \\ 1 \end{bmatrix}$, and $\vec{x}_3 = \begin{bmatrix} 3 \\ 7 \\ 4 \end{bmatrix}$ are linearly dependent.
- **Step-by-Step Solution**:
    1. Construct matrix $A$ using the vectors as columns: $$A = [\vec{x}_1 \quad \vec{x}_2 \quad \vec{x}_3] = \begin{bmatrix} 1 & 1 & 3 \\ 1 & 3 & 7 \\ 2 & 1 & 4 \end{bmatrix}$$
    2. Because $\vec{x}_3 = \vec{x}_1 + 2\vec{x}_2$, apply the column operation $C_3 \to C_3 - (C_1 + 2C_2)$:
        - Column 1: $\begin{bmatrix} 1 \\ 1 \\ 2 \end{bmatrix}$
        - Column 2: $\begin{bmatrix} 1 \\ 3 \\ 1 \end{bmatrix}$
        - Updated Column 3: $$\begin{bmatrix} 3 \\ 7 \\ 4 \end{bmatrix} - \left(\begin{bmatrix} 1 \\ 1 \\ 2 \end{bmatrix} + 2\begin{bmatrix} 1 \\ 3 \\ 1 \end{bmatrix}\right) = \begin{bmatrix} 3 - (1 + 2) \\ 7 - (1 + 6) \\ 4 - (2 + 2) \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \\ 0 \end{bmatrix}$$
    3. The matrix becomes: $$\det(A) = \begin{vmatrix} 1 & 1 & 0 \\ 1 & 3 & 0 \\ 2 & 1 & 0 \end{vmatrix} = 0$$
    4. By theorem, since $\det([\vec{x}_1, \vec{x}_2, \vec{x}_3]) = 0$, the set of vectors is linearly dependent.
- **Final Answer**: $\det(A) = 0$, confirming that the vectors are **linearly dependent**.