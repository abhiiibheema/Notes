CHAPTER 3: MATRIX CHAIN MULTIPLICATION (MCM) & PARENTHESIZATION

---

1. PROBLEM FORMULATION & MATRIX COMPATIBILITY

**Matrix Chain Multiplication (MCM)** is a classic optimization problem solved using Dynamic Programming. Given a sequence (chain) of $n$ matrices $A_1, A_2, \dots, A_n$, we want to compute the final product matrix:

$$A = A_1 \times A_2 \times \dots \times A_n$$

```
                       Matrix Multiplication Properties
                                      │
         ┌────────────────────────────┴────────────────────────────┐
         ▼                                                         ▼
Dimension Compatibility Rule                              Associative Property
- A of size p × q and B of size q × r                     - (A · B) · C = A · (B · C)
- Inner dimensions MUST match (q)                         - Final product matrix is identical
- Cost = p · q · r scalar multiplications                 - Scalar operation counts VARY WILDLY!
```

---

A. Matrix Multiplication Compatibility & Scalar Cost

Two matrices $A$ and $B$ can be multiplied if and only if they are **dimensionally compatible**:

- Matrix $A$ has dimensions $p \times q$.
- Matrix $B$ has dimensions $q \times r$.
- The resulting product matrix $C = A \times B$ has dimensions $p \times r$.
- The total number of scalar multiplications required to compute $C$ is:

$$\text{Cost}(A \times B) = p \times q \times r$$

---

B. The Impact of Parenthesization

Matrix multiplication is **associative**: for any compatible matrices $A, B, C$, $(A \cdot B) \cdot C = A \cdot (B \cdot C)$. While the final resulting matrix values are identical regardless of how the operations are grouped, the **total number of scalar multiplications required varies dramatically**.

Demonstrative Example

Consider multiplying three matrices $A_1, A_2, A_3$ with dimensions:

- $A_1$: $10 \times 100$ ($p_0 = 10, p_1 = 100$)
- $A_2$: $100 \times 5$ ($p_1 = 100, p_2 = 5$)
- $A_3$: $5 \times 50$ ($p_2 = 5, p_3 = 50$)

We compare the two valid fully parenthesized orderings:

```
                            Parenthesization Options
                                       │
         ┌─────────────────────────────┴─────────────────────────────┐
         ▼                                                           ▼
Option 1: ((A₁ · A₂) · A₃)                                  Option 2: (A₁ · (A₂ · A₃))
1. A₁ · A₂: (10 × 100 × 5) = 5,000 ops                     1. A₂ · A₃: (100 × 5 × 50) = 25,000 ops
   Result: (10 × 5) matrix                                    Result: (100 × 50) matrix
2. Result · A₃: (10 × 5 × 50) = 2,500 ops                  2. A₁ · Result: (10 × 100 × 50) = 50,000 ops
Total Cost = 5,000 + 2,500 = 7,500 ops                     Total Cost = 25,000 + 50,000 = 75,000 ops
```

- **Option 1** $((A_1 A_2) A_3)$**:** Requires **7,500** scalar multiplications.
- **Option 2** $(A_1 (A_2 A_3))$**:** Requires **75,000** scalar multiplications.
- **Difference:** Option 1 is **10 times faster** than Option 2!

---

C. Combinatorial Explosion: Catalan Numbers

The number of distinct ways to fully parenthesize a sequence of $n$ matrices is given by the $n$-th **Catalan Number** $P(n)$:

$$P(n) = \begin{cases} 1 & \text{if } n = 1 \\ \sum_{k=1}^{n-1} P(k) \cdot P(n-k) & \text{if } n \ge 2 \end{cases} = \frac{1}{n} \binom{2n-2}{n-1} = \mathbf{\Omega\left(\frac{4^n}{n^{3/2}}\right)}$$

Because $P(n)$ grows exponentially with respect to $n$ (e.g., $P(10) = 4,862$, $P(15) = 2,674,440$), testing all possible parenthesization structures using **brute-force enumeration takes** $O(2^n)$ **exponential time**, making it intractable for large chains.

---

2. DYNAMIC PROGRAMMING FORMULATION

To find the optimal parenthesization in polynomial time, we formulate the Matrix Chain Multiplication problem using Dynamic Programming.

Input Representation

Given a sequence of $n$ matrices $A_1, A_2, \dots, A_n$, where matrix $A_i$ has dimensions $p_{i-1} \times p_i$ for $1 \le i \le n$. The dimensions are specified as an array $P = [p_0, p_1, p_2, \dots, p_n]$ of length $n + 1$.

```
Matrix Dimensions Array P: [ p₀,  p₁,  p₂,  ...,  pₙ ]
                             │    │    │         │
                   A₁ ───────┘    │    │         │
                   A₂ ────────────┘    │         │
                   Aₙ ─────────────────┴─────────┘
```

---

State Definition & Optimal Substructure

Let $m[i][j]$ (or $DP[i][j]$) represent the **minimum number of scalar multiplications** needed to compute the sub-chain product $A_i \dots A_j$ for $1 \le i \le j \le n$.

To compute $m[i][j]$ for $i < j$, we must split the product $A_i \dots A_j$ at some position $k$ where $i \le k < j$:

$$(A_i A_{i+1} \dots A_k) \times (A_{k+1} A_{k+2} \dots A_j)$$

- The cost to evaluate the left sub-chain $A_i \dots A_k$ is $m[i][k]$.
- The cost to evaluate the right sub-chain $A_{k+1} \dots A_j$ is $m[k+1][j]$.
- The cost to multiply the two resulting matrices (of sizes $p_{i-1} \times p_k$ and $p_k \times p_j$) is $p_{i-1} \cdot p_k \cdot p_j$.

---

The Fundamental Recurrence Equation

$$m[i][j] = \begin{cases} 0 & \text{if } i = j \quad (\text{Base Case: Chain of 1 Matrix}) \\ \min_{i \le k < j} \Big( m[i][k] + m[k+1][j] + p_{i-1} \cdot p_k \cdot p_j \Big) & \text{if } i < j \end{cases}$$

To keep track of how to reconstruct the optimal parenthesization, we define an auxiliary table $s[i][j]$ that stores the index $k$ achieving the minimum cost for subproblem $m[i][j]$:

$$s[i][j] = \text{arg min}_{i \le k < j} \Big( m[i][k] + m[k+1][j] + p_{i-1} \cdot p_k \cdot p_j \Big)$$

---

3. TABULATION & DIAGONAL FILLING MECHANICS

Dependency & Fill Order

To compute $m[i][j]$, we require the values of $m[i][k]$ and $m[k+1][j]$. These correspond to sub-chains of strictly shorter lengths ($k - i + 1 < j - i + 1$).

Therefore, the 2D DP table $m[1 \dots n][1 \dots n]$ **CANNOT be filled row-by-row or column-by-column**. It must be filled **diagonal-by-diagonal along increasing chain length** $L = j - i + 1$ from $L = 2$ up to $L = n$.

```
                   DP Table m[i][j] Fill Order (n = 4)
             j = 1       j = 2       j = 3       j = 4
         ┌───────────┬───────────┬───────────┬───────────┐
  i = 1  │   L = 1   │   L = 2   │   L = 3   │   L = 4   │  ◄── Target Solution m[1][n]
         │ (Base=0)  │  Diag 1   │  Diag 2   │  Diag 3   │
         ├───────────┼───────────┼───────────┼───────────┤
  i = 2  │           │   L = 1   │   L = 2   │   L = 3   │
         │  Unused   │ (Base=0)  │  Diag 1   │  Diag 2   │
         ├───────────┼───────────┼───────────┼───────────┤
  i = 3  │           │           │   L = 1   │   L = 2   │
         │  Unused   │  Unused   │ (Base=0)  │  Diag 1   │
         ├───────────┼───────────┼───────────┼───────────┤
  i = 4  │           │           │           │   L = 1   │
         │  Unused   │  Unused   │  Unused   │ (Base=0)  │
         └───────────┴───────────┴───────────┴───────────┘
```

---

Step-by-Step Pseudocode Implementation

```
Algo MatrixChainOrder(P, n) {
    // Step 1: Initialize DP cost table 'm' and split table 's'
    int m[n + 1][n + 1];
    int s[n + 1][n + 1];
    
    // Step 2: Base Case - Single matrix chains require 0 multiplications
    for (i = 1; i <= n; i++) {
        m[i][i] = 0;
    }
    
    // Step 3: L is the chain length (from length 2 to n)
    for (L = 2; L <= n; L++) {
        for (i = 1; i <= n - L + 1; i++) {
            j = i + L - 1; // End index of chain
            m[i][j] = ∞;  // Initialize cost to infinity
            
            // Try all possible split positions k between i and j - 1
            for (k = i; k <= j - 1; k++) {
                cost = m[i][k] + m[k + 1][j] + P[i - 1] * P[k] * P[j];
                
                if (cost < m[i][j]) {
                    m[i][j] = cost;
                    s[i][j] = k; // Store optimal split index
                }
            }
        }
    }
    
    return m[1][n]; // Minimum scalar multiplications for full chain
}
```

---

Reconstructing Optimal Parenthesization String

Using the populated split table $s[i][j]$, we print the optimal parenthesization using a recursive traversal:

```
Algo PrintOptimalParentheses(s, i, j) {
    if (i == j) {
        Print("A" + i);
    } else {
        Print("(");
        PrintOptimalParentheses(s, i, s[i][j]);       // Left sub-chain
        PrintOptimalParentheses(s, s[i][j] + 1, j);   // Right sub-chain
        Print(")");
    }
}
```

---

4. DETAILED COMPLEXITY ANALYSIS

5. **Time Complexity:**
    - **Outer Loop (**$L$**):** Runs for chain lengths $L = 2$ to $n$ $\implies O(n)$ passes.
    - **Middle Loop (**$i$**):** Runs for starting indices $i = 1$ to $n - L + 1$ $\implies O(n)$ iterations.
    - **Inner Loop (**$k$**):** Evaluates split positions $k$ from $i$ to $j - 1$ $\implies O(n)$ steps.
    - **Total Operations:** Sum of operations across all triply nested loops equals $\sum_{L=2}^n (n - L + 1)(L - 1) = \frac{n^3 - n}{6}$.
    - **Time Complexity:** $\mathbf{O(n^3)}$.
6. **Auxiliary Space Complexity:**
    - Requires two 2D matrices ($m[n+1][n+1]$ and $s[n+1][n+1]$) to store costs and split indices.
    - **Auxiliary Space Complexity:** $\mathbf{O(n^2)}$.

---

5. COMPARATIVE SUMMARY MATRIX

|Method / Approach|Time Complexity|Auxiliary Space|Finds Global Minimum?|Constructive Output|
|---|---|---|---|---|
|**Brute-Force Enumeration**|$O\left(\frac{4^n}{n^{3/2}}\right)$ Exponential|$O(n)$ Stack Depth|Yes|Impractical for $n > 15$|
|**Greedy Dimension Sorting**|$O(n \log n)$|$O(1)$|**NO (Fails on many chains)**|Invalid ordering|
|**Dynamic Programming (MCM)**|$\mathbf{O(n^3)}$|$\mathbf{O(n^2)}$|**YES (Guaranteed Optimal)**|Exact split matrix $s[i][j]$|

---

6. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Full Matrix Trace & Parenthesization Reconstruction (NAT)

We are given $n = 4$ matrices $A_1, A_2, A_3, A_4$ with the dimension array:

$$P = [10, 20, 30, 40, 30]$$

1. Matrix $A_1$: $10 \times 20$
2. Matrix $A_2$: $20 \times 30$
3. Matrix $A_3$: $30 \times 40$
4. Matrix $A_4$: $40 \times 30$

Calculate the full DP cost table $m[i][j]$ and split table $s[i][j]$, determine the minimum scalar multiplications, and output the optimal parenthesization.

Step-by-Step Solution:

1. **Base Cases (**$L = 1$**):** $$m[1][1] = 0, \quad m[2][2] = 0, \quad m[3][3] = 0, \quad m[4][4] = 0$$
2. **Chain Length** $L = 2$**:**
    - **Sub-chain** $A_1 A_2$ **(**$i=1, j=2$**):** $k = 1$ $$m[1][2] = m[1][1] + m[2][2] + p_0 p_1 p_2 = 0 + 0 + (10 \times 20 \times 30) = \mathbf{6,000}, \quad s[1][2] = 1$$
    - **Sub-chain** $A_2 A_3$ **(**$i=2, j=3$**):** $k = 2$ $$m[2][3] = m[2][2] + m[3][3] + p_1 p_2 p_3 = 0 + 0 + (20 \times 30 \times 40) = \mathbf{24,000}, \quad s[2][3] = 2$$
    - **Sub-chain** $A_3 A_4$ **(**$i=3, j=4$**):** $k = 3$ $$m[3][4] = m[3][3] + m[4][4] + p_2 p_3 p_4 = 0 + 0 + (30 \times 40 \times 30) = \mathbf{36,000}, \quad s[3][4] = 3$$
3. **Chain Length** $L = 3$**:**
    - **Sub-chain** $A_1 A_2 A_3$ **(**$i=1, j=3$**):** Test $k = 1$ and $k = 2$
        - $k = 1$: $m[1][1] + m[2][3] + p_0 p_1 p_3 = 0 + 24,000 + (10 \times 20 \times 40) = 24,000 + 8,000 = 32,000$
        - $k = 2$: $m[1][2] + m[3][3] + p_0 p_2 p_3 = 6,000 + 0 + (10 \times 30 \times 40) = 6,000 + 12,000 = \mathbf{18,000}$
        - Minimum $= \mathbf{18,000}$ at $k = 2 \implies m[1][3] = 18,000, \quad s[1][3] = 2$
    - **Sub-chain** $A_2 A_3 A_4$ **(**$i=2, j=4$**):** Test $k = 2$ and $k = 3$
        - $k = 2$: $m[2][2] + m[3][4] + p_1 p_2 p_4 = 0 + 36,000 + (20 \times 30 \times 30) = 36,000 + 18,000 = 54,000$
        - $k = 3$: $m[2][3] + m[4][4] + p_1 p_3 p_4 = 24,000 + 0 + (20 \times 40 \times 30) = 24,000 + 24,000 = \mathbf{48,000}$
        - Minimum $= \mathbf{48,000}$ at $k = 3 \implies m[2][4] = 48,000, \quad s[2][4] = 3$
4. **Chain Length** $L = 4$**:**
    - **Full Chain** $A_1 A_2 A_3 A_4$ **(**$i=1, j=4$**):** Test $k = 1, 2, 3$
        - $k = 1$: $m[1][1] + m[2][4] + p_0 p_1 p_4 = 0 + 48,000 + (10 \times 20 \times 30) = 48,000 + 6,000 = 54,000$
        - $k = 2$: $m[1][2] + m[3][4] + p_0 p_2 p_4 = 6,000 + 36,000 + (10 \times 30 \times 30) = 42,000 + 9,000 = 51,000$
        - $k = 3$: $m[1][3] + m[4][4] + p_0 p_3 p_4 = 18,000 + 0 + (10 \times 40 \times 30) = 18,000 + 12,000 = \mathbf{30,000}$
        - Minimum $= \mathbf{30,000}$ at $k = 3 \implies m[1][4] = 30,000, \quad s[1][4] = 3$

Populated DP Tables

**Cost Table** $m[i][j]$**:**

|$i \backslash j$|**1**|**2**|**3**|**4**|
|---|---|---|---|---|
|**1**|0|6,000|18,000|**30,000**|
|**2**|—|0|24,000|48,000|
|**3**|—|—|0|36,000|
|**4**|—|—|—|0|

**Split Table** $s[i][j]$**:**

|$i \backslash j$|**1**|**2**|**3**|**4**|
|---|---|---|---|---|
|**1**|—|1|2|**3**|
|**2**|—|—|2|3|
|**3**|—|—|—|3|
|**4**|—|—|—|—|

1. **Parenthesization Reconstruction:**
    - $s[1][4] = 3 \implies$ Split chain $A_1 \dots A_4$ into $(A_1 A_2 A_3) \times A_4$.
    - $s[1][3] = 2 \implies$ Split sub-chain $A_1 A_2 A_3$ into $(A_1 A_2) \times A_3$.
    - Final Structure: `(((A1 A2) A3) A4)`

**Answer:**

1. Minimum Scalar Multiplications = **30,000**.
2. Optimal Parenthesization = **(((A1 A2) A3) A4)**.

---

Problem 2: Catalan Parenthesization Count (NAT)

Calculate the total number of distinct parenthesizations for a chain of $n = 5$ matrices.

Step-by-Step Solution:

1. Apply the Catalan number formula $P(n) = \frac{1}{n} \binom{2n-2}{n-1}$:
2. For $n = 5$: $$P(5) = \frac{1}{5} \binom{2(5)-2}{5-1} = \frac{1}{5} \binom{8}{4}$$
3. Compute the combination $\binom{8}{4}$: $$\binom{8}{4} = \frac{8 \times 7 \times 6 \times 5}{4 \times 3 \times 2 \times 1} = 70$$
4. Calculate $P(5)$: $$P(5) = \frac{70}{5} = \mathbf{14}$$

**Answer:** **14 distinct parenthesizations**.

---

Problem 3: Minimal Multiplications Calculation (GATE PYQ)

Consider matrices $A(10 \times 100)$, $B(100 \times 5)$, $C(5 \times 50)$, $D(50 \times 1)$. What is the minimum number of scalar multiplications needed to compute $A \times B \times C \times D$?

- (A) 1,750
- (B) 2,200
- (C) 1,200
- (D) 1,500

Step-by-Step Solution:

1. Dimension array $P = [10, 100, 5, 50, 1]$ ($n = 4$).
2. **Chain Length 2:**
    - $m[1][2] = 10 \times 100 \times 5 = 5,000$
    - $m[2][3] = 100 \times 5 \times 50 = 25,000$
    - $m[3][4] = 5 \times 50 \times 1 = 250$
3. **Chain Length 3:**
    - $m[1][3]$:
        - $k=1: 0 + 25,000 + (10 \times 100 \times 50) = 75,000$
        - $k=2: 5,000 + 0 + (10 \times 5 \times 50) = 5,000 + 2,500 = \mathbf{7,500}$
    - $m[2][4]$:
        - $k=2: 0 + 250 + (100 \times 5 \times 1) = 750$
        - $k=3: 25,000 + 0 + (100 \times 50 \times 1) = 30,000$
        - Minimum $= \mathbf{750}$ ($k=2$)
4. **Chain Length 4 (**$m[1][4]$**):**
    - $k=1: m[1][1] + m[2][4] + (10 \times 100 \times 1) = 0 + 750 + 1,000 = \mathbf{1,750}$
    - $k=2: m[1][2] + m[3][4] + (10 \times 5 \times 1) = 5,000 + 250 + 50 = 5,300$
    - $k=3: m[1][3] + m[4][4] + (10 \times 50 \times 1) = 7,500 + 0 + 500 = 8,000$
5. Minimum cost = **1,750** achieved at $k=1$ (Parenthesization: `(A (B (C D)))`).

**Answer:** **(A) 1,750**.

---

Problem 4: Matrix Chain Complexity Classification (MCQ)

What is the running time and auxiliary space complexity of the Dynamic Programming algorithm for Matrix Chain Multiplication on $n$ matrices?

- (A) Time: $O(n^2)$, Space: $O(n)$
- (B) Time: $O(n^3)$, Space: $O(n^2)$
- (C) Time: $O(2^n)$, Space: $O(n^2)$
- (D) Time: $O(n \log n)$, Space: $O(n)$

Step-by-Step Solution:

The algorithm uses a triply nested loop evaluating $O(n^2)$ subproblems where each subproblem tests up to $O(n)$ split positions $k$. This yields $O(n^3)$ running time. Storing the $m$ and $s$ tables of size $n \times n$ requires $O(n^2)$ auxiliary space.

**Answer:** **(B)**.