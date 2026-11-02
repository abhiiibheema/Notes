1. PHILOSOPHY & DESIGN PARADIGM MECHANICS

The **Divide and Conquer (D&C)** strategy is a fundamental top-down algorithm design paradigm used to solve complex computational problems. When a problem instance is large or complex, solving it directly can be inefficient or intractable. Divide and Conquer breaks the main problem instance into smaller, similar subproblems, solves those subproblems recursively, and then combines their solutions to form the final result.

```
                             Original Problem P (Size n)
                                         │
                   ┌─────────────────────┴─────────────────────┐
                   ▼                                           ▼
          Subproblem P1 (Size n/2)                    Subproblem P2 (Size n/2)
                   │                                           │
                   ▼                                           ▼
             Solve Subproblem P1                         Solve Subproblem P2
                   │                                           │
                   └─────────────────────┬─────────────────────┘
                                         ▼
                             Combine Solutions (O(n^k))
```

---

The Three Core Execution Steps

1. **Divide:** Partition the original problem instance $P$ of size $n$ into $k$ smaller, independent subproblems $P_1, P_2, \dots, P_k$ ($k \ge 1$), each of reduced size.
2. **Conquer:** Solve each subproblem recursively.
    - **Definition of a Small Problem:** A subproblem is considered "small" if its size falls below a threshold $n_0$ (typically $n \le 2$), allowing it to be solved directly using a few basic operations in constant time $O(1)$ without further division.
3. **Combine:** Merge the individual solutions of the $k$ subproblems into a unified solution for the original problem $P$.

---

Mandatory vs. Optional Steps Rule

In the Divide and Conquer design paradigm:

- **Divide:** **MANDATORY**. Every D&C algorithm must partition the problem into smaller subproblems.
- **Conquer (Recursive Call):** **MANDATORY**. Subproblems must be processed.
- **Combine:** **OPTIONAL**. Not all D&C algorithms require a combine step.
    - _Example:_ In **Binary Search**, the algorithm divides the search space in half, but once the target key is found or narrowed down, no combining step is performed.

---

General Pseudo-Code Template

```
Algorithm DAndC(P) {
    if (Small(P)) {
        return Solution(P); // Base Case: Solve directly in O(1)
    } else {
        // Step 1: Divide P into smaller instances P1, P2, ..., Pk
        (P1, P2, ..., Pk) = Divide(P);
        
        // Step 2 & 3: Conquer subproblems & Combine results
        return Combine(DAndC(P1), DAndC(P2), ..., DAndC(Pk));
    }
}
```

---

2. GENERAL RECURRENCE FRAMEWORK & CLASSIFICATION

The time complexity $T(n)$ of any Divide and Conquer algorithm is modeled mathematically using a **Recurrence Relation**:

$$T(n) = \begin{cases} g(n) & \text{if } n \le n_0 \quad (\text{Small Problem / Base Case}) \\ \sum_{i=1}^k T(n_i) + f(n) & \text{if } n > n_0 \quad (\text{Divide \& Combine Overhead}) \end{cases}$$

Here, $f(n)$ represents the combined computational cost of dividing the problem and merging the subproblem results.

```
                   Divide & Conquer Recurrences
                                │
         ┌──────────────────────┴──────────────────────┐
         ▼                                             ▼
Symmetric D&C Recurrences                   Asymmetric D&C Recurrences
 (Subproblems equal size)                    (Subproblems unequal size)
  - T(n) = a*T(n/b) + f(n)                    - T(n) = T(n/3) + T(2n/3) + f(n)
  - Solved via Master's Theorem               - Solved via Recursion Tree
```

---

Classification of D&C Recurrence Relations

1. **Symmetric Divide and Conquer:**
    - All subproblems generated at any step are of **equal size**.
    - **General Form:** $T(n) = a \cdot T\left(\frac{n}{b}\right) + f(n)$ where $a \ge 1$ and $b > 1$.
    - **Solving Technique:** Master's Theorem or Back-Substitution.
    - _Examples:_ Merge Sort ($T(n) = 2T(n/2) + n$), Binary Search ($T(n) = T(n/2) + C$).
2. **Asymmetric Divide and Conquer:**
    - Subproblems generated at a step are of **unequal sizes**.
    - **General Form:** $T(n) = \sum T(n_i) + f(n)$ where $\sum n_i = n$.
    - **Solving Technique:** Recursion Tree Approach.
    - _Examples:_ $T(n) = T(n/3) + T(2n/3) + n$, Quick Sort Average Case ($T(n) = T(n/4) + T(3n/4) + n$).

---

3. CASE STUDY: THE MIN-MAX PROBLEM

Problem Definition

Given an unsorted array $A$ containing $n$ elements, find both the **minimum** and **maximum** elements simultaneously.

```
Input Array A: [ 7,  15,  -1,  -6,  17,   1,   8,  59,  29,  24 ]
Output:        Min = -6,  Max = 59
```

We evaluate three distinct algorithmic approaches to analyze comparison efficiency.

---

Approach 1: Non-D&C Linear Scan (Independent `if` Statements)

This approach initializes `min` and `max` to the first element and scans the remaining $n-1$ elements using two independent comparison checks per iteration.

```
Algo MinMax_Simple(A, n) {
    min = max = A[1];
    for (i = 2; i <= n; i++) {
        if (A[i] < min) {    // Comparison 1
            min = A[i];
        }
        if (A[i] > max) {    // Comparison 2
            max = A[i];
        }
    }
}
```

Comparison Analysis:

- **Loop Iterations:** Exactly $n - 1$ passes (from $i = 2$ to $n$).
- **Comparisons per Iteration:** Always 2 (one for `min`, one for `max`).
- **Total Key Comparisons:** $2 \times (n - 1) = \mathbf{2n - 2}$.
- **Behavior:** Strictly deterministic. The best, worst, and average cases execute the exact same $2n - 2$ comparisons.

---

Approach 2: Non-D&C Linear Scan (Optimized `else if` Branching)

We can optimize the linear scan by observing that if an element $A[i]$ is strictly smaller than `min`, it **cannot** simultaneously be larger than `max`. Thus, we replace the second `if` statement with an `else if`.

```
Algo MinMax_OptimizedLoop(A, n) {
    min = max = A[1];
    for (i = 2; i <= n; i++) {
        if (A[i] < min) {
            min = A[i];
        } else if (A[i] > max) {
            max = A[i];
        }
    }
}
```

Detailed Case Analysis:

1. **Best Case Configuration:** Array elements are in **strictly decreasing order** (e.g., 1more_horiz).
    - For every element, `A[i] < min` evaluates to `True`.
    - The `else if` branch is **never executed**.
    - **Best-Case Comparisons:** $1 \times (n - 1) = \mathbf{n - 1}$.
2. **Worst Case Configuration:** Array elements are in **strictly increasing order** (e.g., 1more_horiz).
    - For every element, `A[i] < min` evaluates to `False`.
    - The `else if (A[i] > max)` condition **always executes**.
    - **Worst-Case Comparisons:** $2 \times (n - 1) = \mathbf{2n - 2}$.
3. **Average Case Configuration:** Assuming random permutations, the first condition succeeds 50% of the time, so the `else if` executes for half the elements.
    - **Average-Case Comparisons:** $(n - 1) + \frac{n - 1}{2} = \mathbf{\frac{3}{2}(n - 1)}$.

---

Approach 3: Divide and Conquer Min-Max Algorithm

Instead of scanning linearly, the Divide and Conquer approach splits the array into two equal halves, recursively computes the $(\text{min}, \text{max})$ of each half, and combines the results.

```
                           A[1 ... n]
                             /    \
                 Left Half          Right Half
               A[1 ... n/2]       A[n/2+1 ... n]
                  /    \             /    \
               (min1, max1)       (min2, max2)
                   \                 /
             Combine: min = min(min1, min2)  [1 Comp]
                      max = max(max1, max2)  [1 Comp]
```

```
Algo MinMax_DnC(A, low, high, min, max) {
    // Base Case 1: Single element
    if (low == high) {
        min = max = A[low];
    }
    // Base Case 2: Two elements
    else if (high == low + 1) {
        if (A[low] < A[high]) {  // 1 Comparison
            min = A[low];
            max = A[high];
        } else {
            min = A[high];
            max = A[low];
        }
    }
    // Recursive Step
    else {
        mid = (low + high) / 2;
        MinMax_DnC(A, low, mid, min1, max1);
        MinMax_DnC(A, mid + 1, high, min2, max2);
        
        // Combine Step: 2 Comparisons
        if (min1 < min2) min = min1; else min = min2;
        if (max1 > max2) max = max1; else max = max2;
    }
}
```

---

4. STEP-BY-STEP MATHEMATICAL DERIVATION

Let $C(n)$ represent the exact number of key comparisons required by the Divide and Conquer Min-Max algorithm for an array of size $n = 2^k$:

$$C(n) = \begin{cases} 0 & \text{for } n = 1 \quad (\text{Base Case 1}) \\ 1 & \text{for } n = 2 \quad (\text{Base Case 2}) \\ 2 C\left(\frac{n}{2}\right) + 2 & \text{for } n > 2 \quad (\text{Recursive Step}) \end{cases}$$

---

Expansion via Back-Substitution Method

1. **Base Equation:** $$C(n) = 2 C(n/2) + 2 \quad \text{--- (Equation 1)}$$
2. **First Substitution:** Substitute $n \to n/2$ into Eq 1: $$C(n/2) = 2 C(n/2^2) + 2$$ Substitute back into Eq 1: $$C(n) = 2 [2 C(n/2^2) + 2] + 2 = 2^2 C(n/2^2) + 2^2 + 2$$
3. **Second Substitution:** Substitute $n \to n/2^2$: $$C(n/2^2) = 2 C(n/2^3) + 2$$ Substitute back: $$C(n) = 2^2 [2 C(n/2^3) + 2] + 2^2 + 2 = 2^3 C(n/2^3) + 2^3 + 2^2 + 2$$
4. **General Expansion at Step** $k$**:** $$C(n) = 2^k C\left(\frac{n}{2^k}\right) + \sum_{j=1}^k 2^j$$
5. **Apply Base Condition Boundary:** Set the subproblem size equal to the base case size $2$: $$\frac{n}{2^k} = 2 \implies 2^k = \frac{n}{2} \implies k = \log_2\left(\frac{n}{2}\right) = \log_2 n - 1$$
6. **Substitute** $2^k = n/2$ **into the General Expansion:** $$C(n) = \left(\frac{n}{2}\right) \cdot C(2) + \sum_{j=1}^{k} 2^j$$ Since $C(2) = 1$, and using the geometric series sum formula $\sum_{j=1}^k 2^j = 2^{k+1} - 2$: $$C(n) = \frac{n}{2} (1) + \left(2^{(\log_2 n - 1) + 1} - 2\right)$$ $$C(n) = \frac{n}{2} + 2^{\log_2 n} - 2 = \frac{n}{2} + n - 2$$

$$\mathbf{C(n) = \frac{3n}{2} - 2}$$

---

5. COMPARATIVE SUMMARY MATRIX

|Metric / Parameter|Non-D&C Simple Scan (2 `if`s)|Non-D&C Optimized Scan (`else if`)|D&C Min-Max Algorithm|
|---|---|---|---|
|**Best-Case Comparisons**|$2n - 2$|$n - 1$|$\frac{3n}{2} - 2$|
|**Worst-Case Comparisons**|$2n - 2$|$2n - 2$|$\mathbf{\frac{3n}{2} - 2}$|
|**Average-Case Comparisons**|$2n - 2$|$\frac{3}{2}(n - 1)$|$\mathbf{\frac{3n}{2} - 2}$|
|**Overall Time Complexity**|$O(n)$|$O(n)$|$\mathbf{O(n)}$|
|**Auxiliary Space Complexity**|$O(1)$|$O(1)$|$\mathbf{O(\log n)}$ (Stack Space)|
|**Comparison Savings vs. Simple**|$0\%$ Baseline|$0\%$ in Worst Case|**~25% Reduction in Comparisons**|

---

6. COMPLETE SOLVED PROBLEM SET

---

Problem 1: Non-D&C Comparison Counts (NAT)

An unsorted array contains $n = 100$ elements. Calculate the exact number of key comparisons executed by:

1. The Non-D&C Simple Scan (`MinMax_Simple`).
2. The Non-D&C Optimized Loop (`MinMax_OptimizedLoop`) in its Best, Worst, and Average cases.

Step-by-Step Solution:

1. **Simple Scan (****MinMax_Simple****):**
    - Formula: $C = 2(n - 1)$
    - For $n = 100$: $C = 2(100 - 1) = 2 \times 99 = \mathbf{198 \text{ comparisons}}$.
2. **Optimized Loop (****MinMax_OptimizedLoop****):**
    - **Best Case:** $C_{\text{best}} = n - 1 = 100 - 1 = \mathbf{99 \text{ comparisons}}$.
    - **Worst Case:** $C_{\text{worst}} = 2(n - 1) = 2(99) = \mathbf{198 \text{ comparisons}}$.
    - **Average Case:** $C_{\text{avg}} = \frac{3}{2}(n - 1) = \frac{3}{2}(99) = \mathbf{148.5 \text{ comparisons}}$.

---

Problem 2: Divide and Conquer Min-Max Exact Comparisons

Calculate the exact number of comparisons performed by the Divide and Conquer Min-Max algorithm for an array of size $n = 64$.

Step-by-Step Solution:

1. Apply the D&C Min-Max formula: $$C(n) = \frac{3n}{2} - 2$$
2. Substitute $n = 64$: $$C(64) = \frac{3(64)}{2} - 2 = 3(32) - 2 = 96 - 2 = \mathbf{94 \text{ comparisons}}$$

---

Problem 3: Recurrence Relation Categorization (MCQ)

Categorize the following recurrence relations into **Symmetric** or **Asymmetric** Divide and Conquer:

1. $T(n) = 2T(n/2) + n$
2. $T(n) = T(n/4) + T(3n/4) + n$
3. $T(n) = 8T(n/2) + n^2$
4. $T(n) = T(n/3) + T(2n/3) + n$

Step-by-Step Solution:

- **Relation 1 (**$2T(n/2) + n$**):** Both subproblems are of equal size $n/2 \implies \mathbf{\text{Symmetric}}$.
- **Relation 2 (**$T(n/4) + T(3n/4) + n$**):** Subproblems have unequal sizes $n/4$ and $3n/4 \implies \mathbf{\text{Asymmetric}}$.
- **Relation 3 (**$8T(n/2) + n^2$**):** All 8 subproblems have equal size $n/2 \implies \mathbf{\text{Symmetric}}$.
- **Relation 4 (**$T(n/3) + T(2n/3) + n$**):** Subproblems have unequal sizes $n/3$ and $2n/3 \implies \mathbf{\text{Asymmetric}}$.

---

Problem 4: D&C Step Requirements Verification

Which of the following statements regarding the Divide and Conquer paradigm is **TRUE**?

- (A) Both Divide and Combine steps are mandatory for all algorithms.
- (B) Divide is mandatory, but Combine is optional.
- (C) Combine is mandatory, but Divide is optional.
- (D) Both Divide and Combine steps are optional.

Step-by-Step Solution:

By definition of the paradigm, every D&C algorithm **must** partition the input instance (Divide is mandatory). However, algorithms like Binary Search perform no merging/combining phase after locating the element. Thus, Combine is optional.

**Answer:** **(B)**.

---

Problem 5: Space Complexity of D&C Min-Max

What is the Auxiliary Space Complexity of the Divide and Conquer Min-Max algorithm on an array of size $n$, and why?

Step-by-Step Solution:

1. The algorithm uses no auxiliary array (operates in-place on input $A$).
2. However, it generates a recursive call tree by halving the array at each level.
3. The maximum depth of active activation records on the system call stack is equal to the height of the binary recursion tree: $$\text{Stack Depth} = \log_2 n$$
4. Each stack frame consumes $O(1)$ memory.

**Answer:** **Auxiliary Space Complexity =** $O(\log n)$ (due to system call stack depth).

---

Problem 6: Percentage Comparison Reduction (NAT)

For an array of size $n = 1000$, calculate the percentage reduction in worst-case key comparisons achieved by switching from the simple Non-D&C scan ($2n - 2$) to the Divide and Conquer Min-Max algorithm ($\frac{3n}{2} - 2$).

Step-by-Step Solution:

1. **Simple Scan Comparisons (**$n = 1000$**):** $$C_1 = 2(1000) - 2 = 1998$$
2. **D&C Min-Max Comparisons (**$n = 1000$**):** $$C_2 = \frac{3(1000)}{2} - 2 = 1500 - 2 = 1498$$
3. **Absolute Reduction:** $$\Delta C = 1998 - 1498 = 500 \text{ comparisons}$$
4. **Percentage Reduction:** $$\text{Percentage} = \left(\frac{500}{1998}\right) \times 100\% \approx \mathbf{25.025\%}$$

**Answer:** **~25.03% reduction in comparisons**.