**1. Fundamental Definition of Eigenvalues & Eigenvectors**

- **Definition**: For a square matrix $A_{n \times n}$, a non-zero vector $\vec{x} \neq \vec{0}$ is called an **eigenvector** of $A$ corresponding to an **eigenvalue** $\lambda$ if it satisfies: $$A\vec{x} = \lambda\vec{x}$$
- **Scaling Factor & Direction**: The scalar $\lambda$ acts as a **scaling factor**. Under this transformation, the direction (line of action) of $A\vec{x}$ remains identical to that of $\vec{x}$.
- **Eigen Pair**: The ordered pair $(\lambda, \vec{x})$ is referred to as an **eigen pair**.

---

**2. Derivation of the Characteristic Equation**

- **Homogeneous System Formulation**: Rearranging $A\vec{x} = \lambda\vec{x}$ yields: $$(A - \lambda I)\vec{x} = \vec{0}$$
- **Non-Trivial Solution Criterion**: Because eigenvectors must be non-zero ($\vec{x} \neq \vec{0}$), this homogeneous system must possess a **non-trivial solution**. This requires the coefficient matrix to be singular ($\rho(A - \lambda I) < n$), meaning its determinant must equal zero: $$\det(A - \lambda I) = 0$$
- **Characteristic Equation**: The polynomial equation $\det(A - \lambda I) = 0$ of degree $n$ is called the **characteristic equation** of matrix $A$. Solving it yields the eigenvalues of $A$.

---

**3. Characteristic Polynomial for a** $3 \times 3$ **Matrix**

For a $3 \times 3$ matrix $A$, the characteristic polynomial $C_A(\lambda)$ is given by: $$C_A(\lambda) = \lambda^3 - \operatorname{tr}(A)\lambda^2 + (M_{11} + M_{22} + M_{33})\lambda - \det(A) = 0$$ where $M_{ij}$ denotes the minor of the principal diagonal element $a_{ij}$.

- **Vieta's Relations for Roots (**$\lambda_1, \lambda_2, \lambda_3$**)**:
    1. **Sum of Roots**: $\lambda_1 + \lambda_2 + \lambda_3 = \operatorname{tr}(A)$ (Sum of principal diagonal elements)
    2. **Pairwise Sum of Products**: $\lambda_1\lambda_2 + \lambda_2\lambda_3 + \lambda_3\lambda_1 = M_{11} + M_{22} + M_{33}$
    3. **Product of Roots**: $\lambda_1\lambda_2\lambda_3 = \det(A)$

---

**4. Fundamental Properties of Eigenvalues**

Let $\lambda_1, \lambda_2, \dots, \lambda_n$ be the eigenvalues of an $n \times n$ matrix $A$:

1. **Sum of Eigenvalues**: The sum of all eigenvalues equals the trace of the matrix: $$\sum_{i=1}^{n} \lambda_i = \operatorname{tr}(A)$$
2. **Product of Eigenvalues**: The product of all eigenvalues equals the determinant of the matrix: $$\prod_{i=1}^{n} \lambda_i = \det(A)$$
3. **Power Matrix (**$A^\alpha$**)**: The eigenvalues of $A^\alpha$ are $\lambda_1^\alpha, \lambda_2^\alpha, \dots, \lambda_n^\alpha$.
4. **Inverse Matrix (**$A^{-1}$**)**: If $A$ is non-singular ($\lambda_i \neq 0$), the eigenvalues of $A^{-1}$ are $\frac{1}{\lambda_1}, \frac{1}{\lambda_2}, \dots, \frac{1}{\lambda_n}$.
5. **Adjoint Matrix (**$\operatorname{adj}(A)$**)**: If $\lambda_i \neq 0$, the eigenvalues of $\operatorname{adj}(A)$ are $\frac{\det(A)}{\lambda_1}, \frac{\det(A)}{\lambda_2}, \dots, \frac{\det(A)}{\lambda_n}$.
6. **Shifted Identity Matrix (**$A + kI$**)**: For any scalar $k$, the eigenvalues of $A + kI$ are $\lambda_1 + k, \lambda_2 + k, \dots, \lambda_n + k$.
7. **Polynomial Combination (**$aA^2 + bA + cI$**)**: The eigenvalues of $aA^2 + bA + cI$ are $a\lambda_i^2 + b\lambda_i + c$ for each $i = 1, \dots, n$.

---

**5. Eigenvalues of Special Matrix Families**

- **Triangular & Diagonal Matrices**: If $A$ is an upper triangular, lower triangular, or diagonal matrix, its eigenvalues are simply its **principal diagonal elements**.
- **Real Symmetric Matrix (**$A = A^T$**)**: All eigenvalues of a real symmetric matrix are strictly **real numbers** ($\lambda \in \mathbb{R}$).
- **Skew-Symmetric Matrix (**$A = -A^T$**)**: The eigenvalues of a skew-symmetric matrix are either **zero** or **purely imaginary** ($0, \pm i a, \dots$).
- **Odd-Order Skew-Symmetric Matrix**: An odd-order skew-symmetric matrix ($A_{n \times n} = -A^T$ with $n$ odd) always has $\lambda = 0$ **as an eigenvalue**.

---

**6. Useful Polynomial Root Rules for Characteristic Equations**

For a polynomial equation $a_0 \lambda^n + a_1 \lambda^{n-1} + \dots + a_n = 0$:

1. If the sum of all coefficients equals zero ($\sum a_i = 0$), then $\lambda = 1$ **is a root**.
2. If the constant term $a_n = 0$, then $\lambda = 0$ **is a root**.
3. If the sum of coefficients of odd powers equals the sum of coefficients of even powers, then $\lambda = -1$ **is a root**.
4. If $\lambda_1, \dots, \lambda_n$ are roots of $f(\lambda) = 0$, then $\lambda_1 + k, \dots, \lambda_n + k$ are roots of $f(\lambda - k) = 0$.

---


## Part 1: Multiple Choice & Numerical Questions

---

**Problem 1: Parametric Linear System Analysis**

- **Question Statement**: Let $r$ and $s$ be real numbers. If $A = \begin{pmatrix} 1 & 2 & 0 \\ 2 & 0 & 3 \\ r & s & 0 \end{pmatrix}$ and $b = \begin{pmatrix} 1 \\ 1 \\ s - 1 \end{pmatrix}$, then the system of linear equations $AX = b$ has:
    - **(A)** no solutions for $s \neq 2r$
    - **(B)** infinitely many solutions for $s = 2r \neq 2$
    - **(C)** a unique solution for $s = 2r = 2$
    - **(D)** infinitely many solutions for $s = 2r = 2$
- **Step-by-Step Solution**:
    1. Form the augmented matrix $[A \mid b]$: $$[A \mid b] = \begin{pmatrix} 1 & 2 & 0 & 1 \\ 2 & 0 & 3 & 1 \\ r & s & 0 & s - 1 \end{pmatrix}$$
    2. Apply row transformations $R_2 \to R_2 - 2R_1$ and $R_3 \to R_3 - rR_1$: $$[A \mid b] \sim \begin{pmatrix} 1 & 2 & 0 & 1 \\ 0 & -4 & 3 & -1 \\ 0 & s - 2r & 0 & s - 1 - r \end{pmatrix}$$
    3. Analyze the condition for infinitely many solutions ($\rho(A) = \rho([A \mid b]) < 3$):
        - Requires $s - 2r = 0 \implies s = 2r$, giving $\rho(A) = 2$.
        - For $\rho([A \mid b]) = 2$, the last element must also vanish: $$s - 1 - r = 0 \implies s - r = 1$$
        - Substituting $s = 2r$ yields: $$2r - r = 1 \implies r = 1 \implies s = 2$$
        - Thus, $s = 2r = 2$.
- **Final Answer**: **(D) infinitely many solutions for** $s = 2r = 2$

---

**Problem 2: Idempotent Matrix Property**

- **Question Statement**: Let $A$ be a $3 \times 3$ matrix such that $A^2 = A$. Then it is necessary that:
    - **(A)** $A$ is the identity matrix or the zero matrix.
    - **(B)** the determinant of $A^4$ is either $0$ or $1$.
    - **(C)** the rank of $A$ is $3$.
    - **(D)** $A$ has one imaginary eigenvalue.
- **Step-by-Step Solution**:
    1. Since $A^2 = A$, taking the determinant on both sides yields: $$\det(A^2) = \det(A) \implies [\det(A)]^2 = \det(A) \implies \det(A)(\det(A) - 1) = 0$$ Therefore, $\det(A) = 0$ or $\det(A) = 1$.
    2. For $A^4$: $$\det(A^4) = [\det(A)]^4$$
        - If $\det(A) = 0$, then $\det(A^4) = 0^4 = 0$.
        - If $\det(A) = 1$, then $\det(A^4) = 1^4 = 1$.
    3. Thus, $\det(A^4)$ is strictly either $0$ or $1$.
- **Final Answer**: **(B) the determinant of** $A^4$ **is either 0 or 1**

---

**Problem 3: Skew-Symmetric Matrix Properties**

- **Question Statement**: Considering the matrix $A = \begin{pmatrix} 0 & -1 & 2 \\ 1 & 0 & 3 \\ -2 & -3 & 0 \end{pmatrix}$, which one of the following statements is **INCORRECT**?
    - **(A)** One of its eigenvalues is zero.
    - **(B)** It has two purely imaginary eigenvalues.
    - **(C)** It has a non-zero real eigenvalue.
    - **(D)** The sum of its eigenvalues is zero.
- **Step-by-Step Solution**:
    1. $A$ is a $3 \times 3$ skew-symmetric matrix ($A^T = -A$).
    2. The characteristic polynomial $C_A(\lambda)$ is: $$C_A(\lambda) = \lambda^3 - \operatorname{tr}(A)\lambda^2 + (M_{11} + M_{22} + M_{33})\lambda - \det(A) = 0$$
        - $\operatorname{tr}(A) = 0 + 0 + 0 = 0$
        - $M_{11} + M_{22} + M_{33} = 9 + 4 + 1 = 14$
        - $\det(A) = 0$ (since odd-order skew-symmetric matrices have determinant $0$).
    3. Thus, $C_A(\lambda) = \lambda^3 + 14\lambda = \lambda(\lambda^2 + 14) = 0$.
    4. The eigenvalues are: $$\lambda_1 = 0, \quad \lambda_2 = \sqrt{14}i, \quad \lambda_3 = -\sqrt{14}i$$
    5. Statement (C) claims the matrix has a non-zero real eigenvalue, which is false.
- **Final Answer**: **(C) It has a non-zero real eigenvalue**

---

**Part 2: Calculation & Homework Problems**

---

**Problem 4: Characteristic Polynomial & Eigenvalues Evaluation**

- **Question Statement**: Find the characteristic polynomial and eigenvalues for the matrix: $$A = \begin{bmatrix} 1 & 2 & 1 \\ 2 & 1 & 1 \\ 1 & 1 & 2 \end{bmatrix}$$
- **Step-by-Step Solution**:
    1. Use the $3 \times 3$ characteristic polynomial formula: $$C_A(\lambda) = \lambda^3 - \operatorname{tr}(A)\lambda^2 + (M_{11} + M_{22} + M_{33})\lambda - \det(A) = 0$$
    2. Calculate the components:
        - $\operatorname{tr}(A) = 1 + 1 + 2 = 4$
        - $M_{11} = \begin{vmatrix} 1 & 1 \\ 1 & 2 \end{vmatrix} = 1$
        - $M_{22} = \begin{vmatrix} 1 & 1 \\ 1 & 2 \end{vmatrix} = 1$
        - $M_{33} = \begin{vmatrix} 1 & 2 \\ 2 & 1 \end{vmatrix} = -3$
        - $M_{11} + M_{22} + M_{33} = 1 + 1 - 3 = -1$
        - $\det(A) = 1(1) - 2(3) + 1(1) = -4$
    3. Form the polynomial: $$C_A(\lambda) = \lambda^3 - 4\lambda^2 - \lambda + 4 = 0$$
    4. Factor by grouping: $$\lambda^2(\lambda - 4) - 1(\lambda - 4) = 0 \implies (\lambda^2 - 1)(\lambda - 4) = 0$$ $$\lambda = 1, \quad \lambda = 4, \quad \lambda = -1$$
- **Final Answer**:
    - **Characteristic Polynomial**: $\mathbf{\lambda^3 - 4\lambda^2 - \lambda + 4 = 0}$
    - **Eigenvalues**: $\mathbf{\lambda \in \{1, 4, -1\}}$

---

**Problem 5: Homework Problem 1 — Trace of Matrix Polynomial**

- **Question Statement**: If $1, 0,$ and $-1$ are the eigenvalues of a $3 \times 3$ matrix $A$, then the trace of $A^2 + 5A$ is equal to $\underline{\phantom{0000}}$.
- **Step-by-Step Solution**:
    1. Given eigenvalues of $A$: $\lambda_1 = 1, \lambda_2 = 0, \lambda_3 = -1$.
    2. The eigenvalues of the matrix polynomial $P(A) = A^2 + 5A$ are given by $P(\lambda_i) = \lambda_i^2 + 5\lambda_i$:
        - For $\lambda_1 = 1$: $1^2 + 5(1) = 6$
        - For $\lambda_2 = 0$: $0^2 + 5(0) = 0$
        - For $\lambda_3 = -1$: $(-1)^2 + 5(-1) = 1 - 5 = -4$
    3. The trace of a matrix equals the sum of its eigenvalues: $$\operatorname{tr}(A^2 + 5A) = 6 + 0 + (-4) = 2$$
- **Final Answer**: **2**

---

**Problem 6: Homework Problem 2 — Rank of Diagonalizable Matrix Shift**

- **Question Statement**: Let $I$ denote the identity matrix of order $7$, and $A$ be a $7 \times 7$ real matrix having characteristic polynomial $C_A(\lambda) = \lambda^2 (\lambda - 1)^\alpha (\lambda + 2)^\beta$, where $\alpha$ and $\beta$ are positive integers. If $A$ is diagonalizable and $\operatorname{rank}(A) = \operatorname{rank}(A + 2I)$, then $\operatorname{rank}(A - I)$ is $\underline{\phantom{0000}}$ (in integer).
- **Step-by-Step Solution**:
    1. The degree of the characteristic polynomial equals the matrix order $7$: $$2 + \alpha + \beta = 7 \implies \alpha + \beta = 5$$
    2. The eigenvalues of $A$ are:
        - $\lambda = 0$ with algebraic multiplicity $2$
        - $\lambda = 1$ with algebraic multiplicity $\alpha$
        - $\lambda = -2$ with algebraic multiplicity $\beta$
    3. Since $A$ is diagonalizable, the geometric multiplicity equals the algebraic multiplicity for each eigenvalue:
        - Geometric multiplicity of $\lambda = 0$: $\operatorname{nullity}(A) = 7 - \operatorname{rank}(A) = 2 \implies \operatorname{rank}(A) = 5$
        - Geometric multiplicity of $\lambda = -2$: $\operatorname{nullity}(A + 2I) = 7 - \operatorname{rank}(A + 2I) = \beta \implies \operatorname{rank}(A + 2I) = 7 - \beta$
        - Geometric multiplicity of $\lambda = 1$: $\operatorname{nullity}(A - I) = 7 - \operatorname{rank}(A - I) = \alpha \implies \operatorname{rank}(A - I) = 7 - \alpha$
    4. Given $\operatorname{rank}(A) = \operatorname{rank}(A + 2I)$: $$5 = 7 - \beta \implies \beta = 2$$
    5. Solve for $\alpha$: $$\alpha + 2 = 5 \implies \alpha = 3$$
    6. Calculate $\operatorname{rank}(A - I)$: $$\operatorname{rank}(A - I) = 7 - \alpha = 7 - 3 = 4$$
- **Final Answer**: **4**