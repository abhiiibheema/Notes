 **1. Homogeneous Systems of Linear Equations**

- **Definition & Standard Representation**: A system of linear equations is called **homogeneous** when all constant terms on the right-hand side are zero: $$A_{m \times n} \vec{x}_{n \times 1} = \vec{0}_{m \times 1}$$
- **Trivial Solution**: The zero vector $\vec{x} = \vec{0}$ (i.e., $x_1 = x_2 = \dots = x_n = 0$) is **always** a valid solution for any homogeneous system. This is known as the **trivial solution**.
- **Augmented Matrix Rank Identity**: For any homogeneous system, appending a column of zeros to the coefficient matrix $A$ does not change its rank or introduce any new non-zero submatrix minors: $$\rho([A \mid \vec{0}]) = \rho(A)$$ Consequently, a homogeneous system is **always consistent**.
- **Existence of Non-Trivial Solutions**:
    - **Unique / Trivial Solution**: If $\rho(A) = n$ (where $n$ is the number of unknown variables), the system possesses **only the trivial solution** ($\vec{x} = \vec{0}$).
    - **Non-Trivial / Infinitely Many Solutions**: If $\rho(A) < n$, the system possesses **infinitely many solutions**, meaning there exists at least one **non-trivial solution** (a non-zero vector $\vec{x} \neq \vec{0}$).
- **Determinant Condition for Square Homogeneous Systems (**$n \times n$**)**: For a square system $A_{n \times n} \vec{x} = \vec{0}$:
    - The condition $\rho(A) < n$ occurs if and only if the determinant of $A$ vanishes: $$\det(A) = 0$$
    - **Summary Rules**:
        - \(\det(A) \neq 0 \implies \rho(A) = n \implies \text{Only Trivial Solution } (\vec{x} = \vec{0})\]
        - \(\det(A) = 0 \implies \rho(A) < n \implies \text{Non-Trivial / Infinitely Many Solutions}\]

---

**2. Linearly Independent Solutions of Linear Systems**

- **Number of Linearly Independent Solutions**: For a general linear system $A_{m \times n} \vec{x} = \vec{b}$ with $n$ variables and matrix rank $\rho(A) = r$, if the system has infinitely many solutions ($r < n$), the number of **linearly independent solution vectors** is: $$\text{Number of Linearly Independent Solutions} = n - \rho(A) = n - r$$
- **Basis of Solution Space**: If $n - \rho(A) = k$, then any arbitrary solution vector $\vec{x}$ can be expressed as a linear combination of $k$ fundamental, linearly independent solution vectors.
- **Superposition Principle**: For a homogeneous linear system $A\vec{x} = \vec{0}$, if $\vec{x}_1$ and $\vec{x}_2$ are two valid solutions, then any arbitrary linear combination: $$c_1 \vec{x}_1 + c_2 \vec{x}_2 \quad (c_1, c_2 \in \mathbb{R})$$ is also a valid solution to the system.

---

**3. Direct Solving Methods: Gaussian Elimination**

- **Applicability**: Suitable for any general linear system $A_{m \times n} \vec{x} = \vec{b}$.
- **Two-Step Solution Procedure**:
    1. **Forward Elimination**: Form the augmented matrix $[A \mid \vec{b}]$ and perform elementary row operations to reduce it to **Row-Echelon Form** (upper triangular structure).
    2. **Backward Substitution**: Starting from the last non-zero equation, solve for $x_n$, then substitute sequentially upwards to compute $x_{n-1}, x_{n-2}, \dots, x_1$.
- **Alternative Scheme (Forward Substitution after Backward Elimination)**: Alternatively, perform backward elimination to convert entries above the main diagonal pivots to zero, followed by forward substitution from $x_1$ down to $x_n$.

---

**4. Direct Solving Methods: LU Decomposition**

- **Applicability**: Restricted exclusively to **square systems** of $n$ equations with $n$ unknowns ($A_{n \times n} \vec{x} = \vec{b}$).
- **Core Concept**: Matrix $A$ is factorized into the product of a Lower Triangular matrix $L$ and an Upper Triangular matrix $U$: $$A = L U$$
- **Two-Step System Solution Protocol**:
    1. Substitute $A = LU$ into $A\vec{x} = \vec{b}$ to obtain: $$L(U\vec{x}) = \vec{b}$$
    2. Set $\vec{y} = U\vec{x}$, converting the original system into two triangular sub-systems:
        - **System 1**: $L\vec{y} = \vec{b}$
        - **System 2**: $U\vec{x} = \vec{y}$
    3. **Step 1 (Forward Substitution)**: Solve $L\vec{y} = \vec{b}$ for intermediate vector $\vec{y} = [y_1, y_2, \dots, y_n]^T$ from top to bottom.
    4. **Step 2 (Backward Substitution)**: Solve $U\vec{x} = \vec{y}$ for variable vector $\vec{x} = [x_1, x_2, \dots, x_n]^T$ from bottom to top.

---

**5. Factorization Algorithms for LU Decomposition**

- **a) Doolittle Algorithm**:
    - The Lower Triangular matrix $L$ is set as a **unit lower triangular matrix** (ones along its principal diagonal): $$L = \begin{bmatrix} 1 & 0 & 0 \\ l_{21} & 1 & 0 \\ l_{31} & l_{32} & 1 \end{bmatrix}_{3 \times 3}, \quad U = \begin{bmatrix} U_{11} & U_{12} & U_{13} \\ 0 & U_{22} & U_{23} \\ 0 & 0 & U_{33} \end{bmatrix}_{3 \times 3}$$
    - **Matrix Product Expansion**: $$A = LU = \begin{bmatrix} U_{11} & U_{12} & U_{13} \\ l_{21} U_{11} & l_{21} U_{12} + U_{22} & l_{21} U_{13} + U_{23} \\ l_{31} U_{11} & l_{31} U_{12} + l_{32} U_{22} & l_{31} U_{13} + l_{32} U_{23} + U_{33} \end{bmatrix}$$
- **b) Crout's Algorithm**:
    - The Upper Triangular matrix $U$ is set as a **unit upper triangular matrix** (ones along its principal diagonal): $$L = \begin{bmatrix} l_{11} & 0 & 0 \\ l_{21} & l_{22} & 0 \\ l_{31} & l_{32} & l_{33} \end{bmatrix}_{3 \times 3}, \quad U = \begin{bmatrix} 1 & U_{12} & U_{13} \\ 0 & 1 & U_{23} \\ 0 & 0 & 1 \end{bmatrix}_{3 \times 3}$$

**Part 1: Primary Numerical & Examination Problems**

---

**Problem 1: Solving a System via Gaussian Elimination**

- **Question**: Solve the following system of linear equations using Gaussian elimination: $$x + y + z = 6$$ $$2x - y + z = 3$$ $$x - y + 2z = 5$$
- **Step-by-Step Solution**:
    1. **Form the Augmented Matrix** $[A \mid \vec{b}]$: $$[A \mid \vec{b}] = \begin{bmatrix} 1 & 1 & 1 & 6 \\ 2 & -1 & 1 & 3 \\ 1 & -1 & 2 & 5 \end{bmatrix}$$
    2. **Forward Elimination to Row-Echelon Form**:
        - Eliminate entry beneath $a_{11}$ in Row 2: $R_2 \to R_2 - 2R_1$ $$\begin{bmatrix} 2 & -1 & 1 & 3 \end{bmatrix} - 2\begin{bmatrix} 1 & 1 & 1 & 6 \end{bmatrix} = \begin{bmatrix} 0 & -3 & -1 & -9 \end{bmatrix}$$
        - Eliminate entry beneath $a_{11}$ in Row 3: $R_3 \to R_3 - R_1$ $$\begin{bmatrix} 1 & -1 & 2 & 5 \end{bmatrix} - \begin{bmatrix} 1 & 1 & 1 & 6 \end{bmatrix} = \begin{bmatrix} 0 & -2 & 1 & -1 \end{bmatrix}$$
        - Matrix state: $$\begin{bmatrix} 1 & 1 & 1 & 6 \\ 0 & -3 & -1 & -9 \\ 0 & -2 & 1 & -1 \end{bmatrix}$$
        - Eliminate entry beneath $a_{22}$ in Row 3: $R_3 \to 3R_3 - 2R_2$ $$3\begin{bmatrix} 0 & -2 & 1 & -1 \end{bmatrix} - 2\begin{bmatrix} 0 & -3 & -1 & -9 \end{bmatrix} = \begin{bmatrix} 0 & 0 & 5 & 15 \end{bmatrix}$$
        - Final Row-Echelon Form: $$\begin{bmatrix} 1 & 1 & 1 & 6 \\ 0 & -3 & -1 & -9 \\ 0 & 0 & 5 & 15 \end{bmatrix}$$
    3. **Backward Substitution**:
        - From Row 3: $$5z = 15 \implies z = 3$$
        - From Row 2: $$-3y - z = -9 \implies -3y - 3 = -9 \implies -3y = -6 \implies y = 2$$
        - From Row 1: $$x + y + z = 6 \implies x + 2 + 3 = 6 \implies x = 1$$
- **Final Answer**: $$\vec{x} = \begin{bmatrix} x \\ y \\ z \end{bmatrix} = \begin{bmatrix} 1 \\ 2 \\ 3 \end{bmatrix}$$

---

**Problem 2: Solving a System via LU Decomposition (Doolittle Algorithm)**

- **Question**: Solve the system $A\vec{x} = \vec{b}$ using LU Decomposition (Doolittle Method): $$A = \begin{bmatrix} 1 & 1 & 1 \\ 2 & -1 & 1 \\ 1 & -1 & 2 \end{bmatrix}, \quad \vec{b} = \begin{bmatrix} 6 \\ 3 \\ 5 \end{bmatrix}$$
- **Step-by-Step Solution**:
    1. **Doolittle Factorization (**$A = LU$**)**: Set $L$ as unit lower triangular and $U$ as upper triangular: $$L = \begin{bmatrix} 1 & 0 & 0 \\ l_{21} & 1 & 0 \\ l_{31} & l_{32} & 1 \end{bmatrix}, \quad U = \begin{bmatrix} U_{11} & U_{12} & U_{13} \\ 0 & U_{22} & U_{23} \\ 0 & 0 & U_{33} \end{bmatrix}$$ Multiplying $LU$ and equating entries to $A$:
        - Row 1 of $U$: $U_{11} = 1, \; U_{12} = 1, \; U_{13} = 1$
        - Row 2 entries: $$l_{21} U_{11} = 2 \implies l_{21}(1) = 2 \implies l_{21} = 2$$ $$l_{21} U_{12} + U_{22} = -1 \implies 2(1) + U_{22} = -1 \implies U_{22} = -3$$ $$l_{21} U_{13} + U_{23} = 1 \implies 2(1) + U_{23} = 1 \implies U_{23} = -1$$
        - Row 3 entries: $$l_{31} U_{11} = 1 \implies l_{31}(1) = 1 \implies l_{31} = 1$$ $$l_{31} U_{12} + l_{32} U_{22} = -1 \implies 1(1) + l_{32}(-3) = -1 \implies l_{32} = \frac{2}{3}$$ $$l_{31} U_{13} + l_{32} U_{23} + U_{33} = 2 \implies 1(1) + \left(\frac{2}{3}\right)(-1) + U_{33} = 2 \implies U_{33} = \frac{5}{3}$$

Resulting Triangular Matrices: $$L = \begin{bmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ 1 & \frac{2}{3} & 1 \end{bmatrix}, \quad U = \begin{bmatrix} 1 & 1 & 1 \\ 0 & -3 & -1 \\ 0 & 0 & \frac{5}{3} \end{bmatrix}$$

- **Step 1: Forward Substitution (**$L\vec{y} = \vec{b}$**)**: $$\begin{bmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ 1 & \frac{2}{3} & 1 \end{bmatrix} \begin{bmatrix} y_1 \\ y_2 \\ y_3 \end{bmatrix} = \begin{bmatrix} 6 \\ 3 \\ 5 \end{bmatrix}$$
    - $y_1 = 6$
    - $2y_1 + y_2 = 3 \implies 2(6) + y_2 = 3 \implies y_2 = -9$
    - $y_1 + \frac{2}{3} y_2 + y_3 = 5 \implies 6 + \left(\frac{2}{3}\right)(-9) + y_3 = 5 \implies y_3 = 5$ $$\vec{y} = \begin{bmatrix} 6 \\ -9 \\ 5 \end{bmatrix}$$
- **Step 2: Backward Substitution (**$U\vec{x} = \vec{y}$**)**: \[\begin{bmatrix} 1 & 1 & 1 \ 0 & -3 & -1 \ 0 & 0 & \frac{### **Problem 1: Finding Parameter Value for Consistency**
- **Question**: Find the value of $\alpha$ for which the following system of linear equations has a solution: $$x - y - 3z = 3$$ $$2x + z = 0$$ $$-2y - 7z = \alpha$$
- **Step-by-Step Solution**:
    1. **Form the Augmented Matrix**: $$[A \mid \vec{b}] = \begin{bmatrix} 1 & -1 & -3 & 3 \\ 2 & 0 & 1 & 0 \\ 0 & -2 & -7 & \alpha \end{bmatrix}$$
    2. **Row Operations**:
        - $R_2 \to R_2 - 2R_1$: $$\begin{bmatrix} 2 & 0 & 1 & 0 \end{bmatrix} - 2\begin{bmatrix} 1 & -1 & -3 & 3 \end{bmatrix} = \begin{bmatrix} 0 & 2 & 7 & -6 \end{bmatrix}$$
        - Matrix becomes: $$\begin{bmatrix} 1 & -1 & -3 & 3 \\ 0 & 2 & 7 & -6 \\ 0 & -2 & -7 & \alpha \end{bmatrix}$$
        - $R_3 \to R_3 + R_2$: $$\begin{bmatrix} 0 & -2 & -7 & \alpha \end{bmatrix} + \begin{bmatrix} 0 & 2 & 7 & -6 \end{bmatrix} = \begin{bmatrix} 0 & 0 & 0 & \alpha - 6 \end{bmatrix}$$
    3. **Consistency Condition**:
        - For a solution to exist, the rank of $A$ must equal the rank of $[A \mid \vec{b}]$.
        - Since $\rho(A) = 2$, we require $\rho([A \mid \vec{b}]) = 2$, which means the last row of the augmented matrix must be entirely zero: $$\alpha - 6 = 0 \implies \mathbf{\alpha = 6}$$
- **Final Answer**: $\alpha = 6$

---

**Problem 2: Solution Classification of a Linear System**

- **Question**: Consider the system of linear equations: $$2x_2 + x_3 = 0$$ $$-2x_1 - x_3 = 0$$ $$-x_1 + x_2 = 1$$ Determine whether the system has:
    - **(A)** a unique solution
    - **(B)** an infinite number of solutions
    - **(C)** no solution
    - **(D)** only two distinct solutions
- **Step-by-Step Solution**:
    1. **Form the Augmented Matrix**: $$[A \mid \vec{b}] = \begin{bmatrix} 0 & 2 & 1 & 0 \\ -2 & 0 & -1 & 0 \\ -1 & 1 & 0 & 1 \end{bmatrix}$$
    2. **Row Transformations**:
        - Swap $R_1 \leftrightarrow R_2$: $$\begin{bmatrix} -2 & 0 & -1 & 0 \\ 0 & 2 & 1 & 0 \\ -1 & 1 & 0 & 1 \end{bmatrix}$$
        - $R_3 \to 2R_3 - R_1$: $$2\begin{bmatrix} -1 & 1 & 0 & 1 \end{bmatrix} - \begin{bmatrix} -2 & 0 & -1 & 0 \end{bmatrix} = \begin{bmatrix} 0 & 2 & 1 & 2 \end{bmatrix}$$
        - $R_3 \to R_3 - R_2$: $$\begin{bmatrix} 0 & 2 & 1 & 2 \end{bmatrix} - \begin{bmatrix} 0 & 2 & 1 & 0 \end{bmatrix} = \begin{bmatrix} 0 & 0 & 0 & 2 \end{bmatrix}$$
    3. **Rank Analysis**:
        - $\rho(A) = 2$
        - $\rho([A \mid \vec{b}]) = 3$
        - Since $\rho(A) \neq \rho([A \mid \vec{b}])$, the system is inconsistent.
- **Final Answer**: **(C) no solution**

---

**Problem 3: Parametric System Analysis**

- **Question**: Let $r$ and $s$ be real numbers. Given: $$A = \begin{bmatrix} 1 & 2 & 0 \\ 2 & 0 & 3 \\ r & s & 0 \end{bmatrix}, \quad \vec{b} = \begin{bmatrix} 1 \\ 1 \\ s - 1 \end{bmatrix}$$ Analyze the consistency of $A\vec{x} = \vec{b}$.
- **Step-by-Step Solution**:
    1. Compute $\det(A)$: $$\det(A) = 1(0 - 3s) - 2(0 - 3r) + 0 = -3s + 6r = 3(2r - s)$$
    2. **Case Analysis**:
        - **If** $s \neq 2r$: $\det(A) \neq 0 \implies \rho(A) = 3 = n$. The system has a **unique solution**.
        - **If** $s = 2r$: $\det(A) = 0 \implies \rho(A) < 3$. Evaluating the augmented matrix with $s = 2r$: Using row operations $R_3 \to R_3 - r R_1$: The third row becomes $\begin{bmatrix} 0 & 0 & 0 & (s - 1) - r \end{bmatrix}$. For infinitely many solutions, $(s - 1) - r = 0 \implies s - r = 1$. Combining $s = 2r$ and $s - r = 1$ yields $r = 1$ and $s = 2$.
- **Final Answer**: The system has **infinitely many solutions for** $s = 2r = 2$.

---

**Problem 4: Direct System Solution via Gaussian Elimination**

- **Question**: Solve the linear system using Gaussian elimination: $$x + y + z = 6$$ $$2x - y + z = 3$$ $$x - y + 2z = 5$$
- **Step-by-Step Solution**:
    1. **Row Reduction**:
        - $R_2 \to R_2 - 2R_1 \implies \begin{bmatrix} 0 & -3 & -1 & -9 \end{bmatrix}$
        - $R_3 \to R_3 - R_1 \implies \begin{bmatrix} 0 & -2 & 1 & -1 \end{bmatrix}$
        - $R_3 \to 3R_3 - 2R_2 \implies \begin{bmatrix} 0 & 0 & 5 & 15 \end{bmatrix}$
    2. **Backward Substitution**:
        - $5z = 15 \implies z = 3$
        - $-3y - 3 = -9 \implies -3y = -6 \implies y = 2$
        - $x + 2 + 3 = 6 \implies x = 1$
- **Final Answer**: $x = 1, y = 2, z = 3$

---

**Problem 5: Matrix Factorization via LU Decomposition**

- **Question**: Factorize matrix $A = \begin{bmatrix} 1 & 1 & 1 \\ 2 & -1 & 1 \\ 1 & -1 & 2 \end{bmatrix}$ into $LU$ using the Doolittle algorithm, and solve $A\vec{x} = \begin{bmatrix} 6 \\ 3 \\ 5 \end{bmatrix}$.
- **Step-by-Step Solution**:
    1. **Compute** $L$ **and** $U$: $$L = \begin{bmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ 1 & 2/3 & 1 \end{bmatrix}, \quad U = \begin{bmatrix} 1 & 1 & 1 \\ 0 & -3 & -1 \\ 0 & 0 & 5/3 \end{bmatrix}$$
    2. **Forward Substitution (**$L\vec{y} = \vec{b}$**)**:
        - $y_1 = 6$
        - $2(6) + y_2 = 3 \implies y_2 = -9$
        - $1(6) + \frac{2}{3}(-9) + y_3 = 5 \implies 6 - 6 + y_3 = 5 \implies y_3 = 5$ $$\vec{y} = \begin{bmatrix} 6 \\ -9 \\ 5 \end{bmatrix}$$
    3. **Backward Substitution (**$U\vec{x} = \vec{y}$**)**:
        - $\frac{5}{3} z = 5 \implies z = 3$
        - $-3y - 3 = -9 \implies y = 2$
        - $x + 2 + 3 = 6 \implies x = 1$
- **Final Answer**: $x = 1, y = 2, z = 3$