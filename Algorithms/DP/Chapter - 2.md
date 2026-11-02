CHAPTER 2: 0/1 KNAPSACK PROBLEM

---

1. PROBLEM FORMULATION & PARADIGM COMPARISON

The **0/1 Knapsack Problem** is a fundamental combinatorial optimization problem. Given a knapsack with a maximum weight capacity $M$ and a set of $n$ items, each item $i$ (where $1 \le i \le n$) has an associated:

- **Weight (**$w_i > 0$**):** The physical capacity consumed by item $i$.
- **Profit / Value (**$p_i > 0$**):** The value gained by including item $i$.
- **Decision Variable (**$x_i \in \{0, 1\}$**):** Binary indicator stating whether item $i$ is excluded ($x_i = 0$) or included ($x_i = 1$) in full.

```
                           Knapsack Problem Variants
                                       │
         ┌─────────────────────────────┴─────────────────────────────┐
         ▼                                                           ▼
  Fractional Knapsack                                         0/1 Knapsack
- Division allowed: 0 ≤ xᵢ ≤ 1                               - Binary choice: xᵢ ∈ {0, 1}
- Solved via GREEDY METHOD                                    - Solved via DYNAMIC PROGRAMMING
- Time Complexity: O(n log n)                                 - Time Complexity: O(n · M)
- Always achieves optimal value by density sorting            - Greedy density sorting FAILS
```

---

Mathematical Model

The goal is to select a subset of items to **maximize total profit** without exceeding the **knapsack capacity** $M$:

$$\text{Maximize } f(X) = \sum_{i=1}^n x_i \cdot p_i$$

$$\text{Subject to the constraint: } \sum_{i=1}^n x_i \cdot w_i \le M$$

$$\text{Binary Decision Constraint: } x_i \in \{0, 1\} \quad \forall i \in \{1, 2, \dots, n\}$$

---

Why the Greedy Method Fails for 0/1 Knapsack

In the Fractional Knapsack problem, sorting items by profit-to-weight density ratio ($p_i / w_i$) yields a globally optimal solution because fractional parts can fill any remaining capacity. In the **0/1 Knapsack problem**, item division is strictly forbidden. A high-density item might leave unused capacity (slack space) that cannot be filled, causing the greedy strategy to miss the optimal solution.

Counterexample Demonstrating Greedy Failure

Suppose knapsack capacity $M = 6$ and we have $n = 3$ items:

|Item|Weight ($w_i$)|Profit ($p_i$)|Density ($p_i / w_i$)|
|---|---|---|---|
|**Item 1**|1|2|2.00|
|**Item 2**|3|5|1.67|
|**Item 3**|4|6|1.50|

- **Greedy Selection (by Density Ratio):**
    1. Pick Item 1 ($w_1 = 1, p_1 = 2$). Remaining capacity $= 6 - 1 = 5$.
    2. Pick Item 2 ($w_2 = 3, p_2 = 5$). Remaining capacity $= 5 - 3 = 2$.
    3. Item 3 ($w_3 = 4$) cannot fit into remaining capacity $2$.
    4. **Greedy Total Profit:** $2 + 5 = \mathbf{7}$ (Items $\{1, 2\}$, total weight $4$).
- **Dynamic Programming Selection (Optimal):**
    - Select Item 2 ($w_2 = 3$) and Item 3 ($w_3 = 4$). Total weight $= 3 + 4 = 7 > 6$ (Invalid).
    - Select Item 2 ($w_2 = 3, p_2 = 5$) and Item 1 ($w_1 = 1, p_1 = 2$) $\implies \text{Profit} = 7$.
    - Select Item 3 ($w_3 = 4, p_3 = 6$) and Item 1 ($w_1 = 1, p_1 = 2$) $\implies \text{Weight} = 5 \le 6, \text{Profit} = \mathbf{8}$.
- **Result:** Greedy yields $7$, whereas the global optimal profit is $8$.

---

2. DYNAMIC PROGRAMMING FORMULATION

To solve 0/1 Knapsack using Dynamic Programming, we define the problem in terms of optimal subproblems.

State Definition

Let $V[i][w]$ (or $DP[i][w]$) represent the **maximum profit** obtainable using a subset of the first $i$ items ($1 \le i \le n$) under a knapsack capacity limit of $w$ ($0 \le w \le M$).

```
                      DP State Table Structure V[i][w]
                                 Capacity w (0 ──► M)
                       ┌───┬───┬───┬───┬───┬───┬───┬───┐
                     0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │
                       ├───┼───┼───┼───┼───┼───┼───┼───┤
  Items i            1 │ 0 │   │   │   │   │   │   │   │
 (1 ──► n)             ├───┼───┼───┼───┼───┼───┼───┼───┤
                     2 │ 0 │   │   │   │   │   │   │   │
                       └───┴───┴───┴───┴───┴───┴───┴───┘
```

---

The Fundamental Recurrence Equation

For item $i$ and current capacity $w$:

1. **Case 1 (Item** $i$ **is too heavy):** If $w_i > w$, item $i$ cannot fit into the knapsack. We have no choice but to exclude it: $$V[i][w] = V[i-1][w]$$
2. **Case 2 (Item** $i$ **can fit):** If $w_i \le w$, we evaluate two choices and pick the one that yields the maximum profit:
    - **Choice A (Exclude Item** $i$**):** Take the optimal profit using the first $i-1$ items at capacity $w \implies V[i-1][w]$.
    - **Choice B (Include Item** $i$**):** Earn profit $p_i$ plus the optimal profit using the first $i-1$ items at remaining capacity $(w - w_i) \implies p_i + V[i-1][w - w_i]$.

$$V[i][w] = \begin{cases} 0 & \text{if } i = 0 \text{ or } w = 0 \quad (\text{Base Case}) \\ V[i-1][w] & \text{if } w_i > w \\ \max\Big(V[i-1][w], \, p_i + V[i-1][w - w_i]\Big) & \text{if } w_i \le w \end{cases}$$

---

Step-by-Step Pseudocode Implementation

```
Algo ZeroOneKnapsack_DP(P, W, n, M) {
    // Step 1: Initialize DP table of size (n + 1) x (M + 1)
    int V[n + 1][M + 1];
    
    // Step 2: Set Base Cases (Row 0 and Column 0 to 0)
    for (i = 0; i <= n; i++) V[i][0] = 0;
    for (w = 0; w <= M; w++) V[0][w] = 0;
    
    // Step 3: Fill table row-by-row in bottom-up manner
    for (i = 1; i <= n; i++) {
        for (w = 1; w <= M; w++) {
            if (W[i] <= w) {
                // Maximum of Excluding vs. Including item i
                V[i][w] = Max(V[i - 1][w], P[i] + V[i - 1][w - W[i]]);
            } else {
                // Item i exceeds capacity, exclude it
                V[i][w] = V[i - 1][w];
            }
        }
    }
    
    return V[n][M]; // Maximum profit located at bottom-right cell
}
```

---

3. BACKTRACKING TO RECONSTRUCT SELECTED ITEMS

Filling the DP table computes the maximum profit value $V[n][M]$. To determine the **exact set of items selected** ($x_1, x_2, \dots, x_n$), we trace back through the table starting from $V[n][M]$:

```
Algo ReconstructItems(V, W, n, M) {
    i = n;
    k = M;
    
    while (i > 0 && k > 0) {
        if (V[i][k] != V[i - 1][k]) {
            X[i] = 1;         // Item i WAS included
            k = k - W[i];     // Deduct weight of item i
            i = i - 1;        // Move to previous item
        } else {
            X[i] = 0;         // Item i WAS NOT included
            i = i - 1;        // Move to previous item
        }
    }
    return X; // Binary decision vector
}
```

---

4. COMPLEXITY & PSEUDO-POLYNOMIAL NATURE

Time and Space Complexity

- **Time Complexity:** The DP table contains $(n + 1) \times (M + 1)$ cells. Each cell takes $O(1)$ constant time to compute $\implies \mathbf{O(n \cdot M)}$.
- **Auxiliary Space Complexity:** Standard 2D table requires $\mathbf{O(n \cdot M)}$ storage.
- **Space Optimization (1D Array):** Since computing row $i$ relies only on values from row $i-1$, we can optimize space to a **single 1D array of size** $M + 1$ iterated backwards (from $w = M$ down to $w_i$), reducing space to $\mathbf{O(M)}$.

---

The Pseudo-Polynomial Concept

🚩 **IMPORTANT GATE CONCEPT:** $O(n \cdot M)$ is **NOT a polynomial time complexity in terms of input size**. It is a **Pseudo-Polynomial Time Complexity**.

Why $O(n \cdot M)$ is Pseudo-Polynomial:

1. In computational complexity theory, time complexity is measured relative to the **number of bits required to represent the input**.
2. Representing an integer capacity $M$ in binary requires $k = \lceil \log_2 M \rceil$ bits.
3. Therefore, $M = 2^k$. Expressed in terms of input bit length $k$: $$\text{Running Time} = O(n \cdot M) = O(n \cdot 2^k)$$
4. Because the execution time grows **exponentially** with respect to the bit-length $k$ of capacity $M$, 0/1 Knapsack is an **NP-Hard problem**, and its DP solution runs in pseudo-polynomial time.

---

5. COMPARATIVE SUMMARY MATRIX

|Parameter / Feature|Fractional Knapsack|0/1 Knapsack|
|---|---|---|
|**Algorithmic Paradigm**|**Greedy Method**|**Dynamic Programming**|
|**Item Decision (**$x_i$**)**|Fractional ($0 \le x_i \le 1$)|Binary ($x_i \in \{0, 1\}$)|
|**Optimal Substructure**|Yes|Yes|
|**Overlapping Subproblems**|No|**Yes**|
|**Time Complexity**|$\mathbf{O(n \log_2 n)}$|$\mathbf{O(n \cdot M)}$ (Pseudo-Polynomial)|
|**Auxiliary Space**|$O(n)$|$O(n \cdot M)$ or $O(M)$ optimized|

---

6. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Full DP Matrix Trace & Item Reconstruction (NAT)

Given $n = 4$ items and a knapsack capacity $M = 5$:

|Item ($i$)|Weight ($w_i$)|Profit ($p_i$)|
|---|---|---|
|**1**|2|3|
|**2**|3|4|
|**3**|4|5|
|**4**|1|6|

1. Construct the full DP table $V[i][w]$ for $0 \le i \le 4$ and $0 \le w \le 5$.
2. Find the maximum total profit $V[4][5]$.
3. Backtrack to find the binary decision vector $X = [x_1, x_2, x_3, x_4]$.

Step-by-Step Solution:

1. **Initialize Base Cases:** $V[0][w] = 0$ for all $0 \le w \le 5$, and $V[i][0] = 0$ for all $0 \le i \le 4$.
2. **Row 1 (Item 1:** $w_1 = 2, p_1 = 3$**):**
    - $w = 1$: $w_1 > 1 \implies V[1][1] = V[0][1] = 0$.
    - $w = 2$: $w_1 \le 2 \implies \max(V[0][2], 3 + V[0][0]) = \max(0, 3) = \mathbf{3}$.
    - $w = 3, 4, 5$: Value remains $\mathbf{3}$.
    - Row 1: `[0, 0, 3, 3, 3, 3]`
3. **Row 2 (Item 2:** $w_2 = 3, p_2 = 4$**):**
    - $w = 1, 2$: Inherit from Row 1 $\implies 0, 3$.
    - $w = 3$: $\max(V[1][3], 4 + V[1][0]) = \max(3, 4) = \mathbf{4}$.
    - $w = 4$: $\max(V[1][4], 4 + V[1][1]) = \max(3, 4 + 0) = \mathbf{4}$.
    - $w = 5$: $\max(V[1][5], 4 + V[1][2]) = \max(3, 4 + 3) = \mathbf{7}$.
    - Row 2: `[0, 0, 3, 4, 4, 7]`
4. **Row 3 (Item 3:** $w_3 = 4, p_3 = 5$**):**
    - $w = 1, 2, 3$: Inherit from Row 2 $\implies 0, 3, 4$.
    - $w = 4$: $\max(V[2][4], 5 + V[2][0]) = \max(4, 5) = \mathbf{5}$.
    - $w = 5$: $\max(V[2][5], 5 + V[2][1]) = \max(7, 5 + 0) = \mathbf{7}$.
    - Row 3: `[0, 0, 3, 4, 5, 7]`
5. **Row 4 (Item 4:** $w_4 = 1, p_4 = 6$**):**
    - $w = 1$: $\max(V[3][1], 6 + V[3][0]) = \max(0, 6) = \mathbf{6}$.
    - $w = 2$: $\max(V[3][2], 6 + V[3][1]) = \max(3, 6 + 0) = \mathbf{6}$.
    - $w = 3$: $\max(V[3][3], 6 + V[3][2]) = \max(4, 6 + 3) = \mathbf{9}$.
    - $w = 4$: $\max(V[3][4], 6 + V[3][3]) = \max(5, 6 + 4) = \mathbf{10}$.
    - $w = 5$: $\max(V[3][5], 6 + V[3][4]) = \max(7, 6 + 5) = \mathbf{11}$.
    - Row 4: `[0, 6, 6, 9, 10, 11]`

Completed DP Table $V[i][w]$:

|$i \backslash w$|**0**|**1**|**2**|**3**|**4**|**5**|
|---|---|---|---|---|---|---|
|**0**|0|0|0|0|0|0|
|**1**|0|0|3|3|3|3|
|**2**|0|0|3|4|4|7|
|**3**|0|0|3|4|5|7|
|**4**|0|6|6|9|10|**11**|

1. **Item Backtracking from** $V[4][5] = 11$**:**
    - Compare $V[4][5] = 11$ with $V[3][5] = 7$: Differ $\implies x_4 = 1$. Remaining capacity $k = 5 - w_4 = 5 - 1 = 4$.
    - Compare $V[3][4] = 5$ with $V[2][4] = 4$: Differ $\implies x_3 = 1$. Remaining capacity $k = 4 - w_3 = 4 - 4 = 0$.
    - Capacity is 0 $\implies x_2 = 0, x_1 = 0$.

**Answer:**

1. Maximum Profit = **11** ($V[4][5]$).
2. Selected Items = **Item 3 and Item 4** ($X = [0, 0, 1, 1]$).
3. Total Weight $= 4 + 1 = 5 \le 5$, Total Profit $= 5 + 6 = 11$.

---

Problem 2: Pseudo-Polynomial Complexity Analysis (GATE PYQ)

Consider an instance of the 0/1 Knapsack problem with $n = 100$ items and knapsack capacity $M = 10,000$.

1. Calculate the total number of operations performed by the Dynamic Programming algorithm.
2. If capacity $M$ is represented using $k$ binary bits, what is the time complexity in terms of $n$ and $k$?

Step-by-Step Solution:

1. **Total DP Table Operations:** $$\text{Operations} = (n + 1) \times (M + 1) = 101 \times 10,001 = \mathbf{1,010,101 \text{ operations}}$$
2. **Bit-length Complexity Formulation:**
    - Number of bits $k = \log_2 M \implies M = 2^k$.
    - Substituting $M = 2^k$ into $O(n \cdot M)$ yields $\mathbf{O(n \cdot 2^k)}$.

**Answer:**

1. Operations = **1,010,101**.
2. Complexity = $O(n \cdot 2^k)$ (Exponential in bit-length $k$, Pseudo-Polynomial in value $M$).

---

Problem 3: 1D Space-Optimized Array State Update (MCQ)

When optimizing the 0/1 Knapsack DP space from 2D $V[n+1][M+1]$ to a 1D array `DP[M+1]`, why must the inner capacity loop iterate **backwards from** $w = M$ **down to** $w_i$?

- (A) To improve CPU cache locality.
- (B) To prevent using updated values from the current item $i$ in the same iteration (avoiding multiple inclusions of item $i$).
- (C) To reduce time complexity from $O(n \cdot M)$ to $O(n \log M)$.
- (D) To maintain stability during selection.

Step-by-Step Solution:

If the inner loop iterates forward ($w = w_i \to M$), computing `DP[w]` overwrites earlier values in the same row. Later calculations in that same row (e.g., `DP[w + w_i]`) would accidentally use the newly updated `DP[w]`, resulting in item $i$ being included multiple times (which solves the **Unbounded Knapsack** problem, not 0/1 Knapsack). Iterating backwards ensures we only reference values from the previous item's state.

**Answer:** **(B)**.