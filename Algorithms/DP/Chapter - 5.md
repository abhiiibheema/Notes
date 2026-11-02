CHAPTER 5: SUBSET SUM PROBLEM & EQUAL PARTITION

---

1. PROBLEM FORMULATION & DECISION MODEL

The **Subset Sum Problem** and **Equal Partition Problem** are classic decision problems in computer science solved using Dynamic Programming.

```
                           Subset Sum & Partition
                                     │
         ┌───────────────────────────┴───────────────────────────┐
         ▼                                                       ▼
Subset Sum Problem                                    Equal Partition Problem
- Given set S and target value                         - Divide set S into two equal sum subsets
- Target is explicit in input                          - Target = TotalSum / 2
- Output: True / False                                 - Returns False immediately if TotalSum is ODD
```

---

A. Subset Sum Problem Definition

Given a set of $n$ non-negative integers $S = \{w_1, w_2, \dots, w_n\}$ and a target sum $target$, determine whether there exists a subset $S' \subseteq S$ whose elements sum up exactly to $target$.

- **Decision Output:** Binary boolean (`TRUE` / `FALSE`).
- **Mathematical Constraint:** $$\sum_{i \in S'} w_i = target$$

---

B. Equal Partition Problem Definition

Given a set of $n$ non-negative integers $S = \{w_1, w_2, \dots, w_n\}$, determine whether the set $S$ can be partitioned into two disjoint subsets $S_1$ and $S_2$ such that the sum of elements in $S_1$ equals the sum of elements in $S_2$ ($S_1 \cup S_2 = S$ and $S_1 \cap S_2 = \emptyset$).

Let $TotalSum = \sum_{i=1}^n w_i$.

1. **Parity Check Rule (Odd Sum):** If $TotalSum$ is **ODD**, it is mathematically **IMPOSSIBLE** to divide $S$ into two equal integer sum subsets. The algorithm returns **FALSE** **immediately in** $O(1)$ **time**.
2. **Even Sum Reduction:** If $TotalSum$ is **EVEN**, the problem reduces directly to the **Subset Sum Problem** with: $$target = \frac{TotalSum}{2}$$

---

2. DYNAMIC PROGRAMMING FORMULATION

A. Relation to 0/1 Knapsack Problem

The Subset Sum problem is a special decision variant of the 0/1 Knapsack problem where:

- Item weights equal item profits ($w_i = p_i$).
- Knapsack capacity $M = target$.
- Instead of maximizing profit, we check if an **exact capacity fill** equal to $target$ is achievable.

---

B. DP State Definition

Let $DP[i][j]$ represent a boolean truth value indicating whether a subset sum of $j$ ($0 \le j \le target$) can be formed using a subset of the first $i$ elements ($0 \le i \le n$).

```
                      DP Table Structure DP[i][j]
                               Target Sum j (0 ──► target)
                       ┌───┬───┬───┬───┬───┬───┬───┬───┐
                     0 │ T │ F │ F │ F │ F │ F │ F │ F │
                       ├───┼───┼───┼───┼───┼───┼───┼───┤
  Elements i         1 │ T │   │   │   │   │   │   │   │
 (1 ──► n)             ├───┼───┼───┼───┼───┼───┼───┼───┤
                     2 │ T │   │   │   │   │   │   │   │
                       └───┴───┴───┴───┴───┴───┴───┴───┘
```

- $DP[i][j] = \text{TRUE}$**:** A valid subset from the first $i$ elements sums to $j$.
- $DP[i][j] = \text{FALSE}$**:** No subset from the first $i$ elements can sum to $j$.

---

C. Base Cases

1. $DP[i][0] = \text{TRUE}$ **for all** $0 \le i \le n$**:** A target sum of $0$ can always be formed by selecting the empty subset $\emptyset$.
2. $DP[0][j] = \text{FALSE}$ **for all** $1 \le j \le target$**:** With $0$ elements, no positive target sum can be formed.

---

D. The Fundamental Recurrence Equation

For element $w_i$ at index $i$ and current target sum $j$:

1. **Case 1 (Element** $w_i > j$**):** Element $w_i$ exceeds the target $j$. It cannot be included in the subset: $$DP[i][j] = DP[i-1][j]$$
2. **Case 2 (Element** $w_i \le j$**):** We evaluate two options using logical OR:
    - **Exclude element** $w_i$**:** $DP[i-1][j]$
    - **Include element** $w_i$**:** $DP[i-1][j - w_i]$

$$DP[i][j] = \begin{cases} \text{TRUE} & \text{if } j = 0 \quad (\text{Base Case}) \\ \text{FALSE} & \text{if } i = 0 \text{ and } j > 0 \quad (\text{Base Case}) \\ DP[i-1][j] & \text{if } w_i > j \\ DP[i-1][j] \quad \mathbf{OR} \quad DP[i-1][j - w_i] & \text{if } w_i \le j \end{cases}$$

---

3. PSEUDO-CODE IMPLEMENTATION

```
Algo SubsetSum_DP(S, n, target) {
    bool DP[n + 1][target + 1];
    
    // Step 1: Set Base Cases
    for (i = 0; i <= n; i++) {
        DP[i][0] = True;  // Sum 0 is always achievable
    }
    for (j = 1; j <= target; j++) {
        DP[0][j] = False; // Positive sum impossible with 0 items
    }
    
    // Step 2: Fill DP table row-by-row
    for (i = 1; i <= n; i++) {
        for (j = 1; j <= target; j++) {
            if (S[i] > j) {
                DP[i][j] = DP[i - 1][j]; // Exclude element
            } else {
                DP[i][j] = DP[i - 1][j] || DP[i - 1][j - S[i]]; // Exclude OR Include
            }
        }
    }
    
    return DP[n][target]; // Returns True or False
}

Algo EqualPartition_DP(S, n) {
    total_sum = 0;
    for (i = 1; i <= n; i++) {
        total_sum += S[i];
    }
    
    // Parity Check: Odd sums cannot be partitioned equally
    if (total_sum % 2 != 0) {
        return False;
    }
    
    target = total_sum / 2;
    return SubsetSum_DP(S, n, target);
}
```

---

4. COMPLEXITY ANALYSIS & PSEUDO-POLYNOMIAL NATURE

- **Time Complexity:**
    - **Subset Sum:** $\mathbf{O(n \cdot target)}$.
    - **Equal Partition:** $\mathbf{O(n \cdot TotalSum)}$.
- **Pseudo-Polynomial Property:** Like 0/1 Knapsack, the running time depends on the numerical value of $target$. Since representing $target$ in binary requires $k = \lceil \log_2 target \rceil$ bits, the complexity in terms of bit length is exponential: $\mathbf{O(n \cdot 2^k)}$. Thus, Subset Sum is an **NP-Complete decision problem**.
- **Space Complexity:**
    - Standard 2D DP Table: $\mathbf{O(n \cdot target)}$.
    - **1D Array Optimization:** Since computing row $i$ references only row $i-1$, auxiliary space can be reduced to a single 1D boolean array of size $target + 1$ iterated backwards (from $j = target$ down to $w_i$), yielding $\mathbf{O(target)}$ space.

---

5. COMPARATIVE SUMMARY MATRIX

|Parameter / Metric|Subset Sum Problem|Equal Partition Problem|
|---|---|---|
|**Problem Type**|Decision Problem (`TRUE`/`FALSE`)|Decision Problem (`TRUE`/`FALSE`)|
|**Target Definition**|Given explicitly as input|Calculated as $TotalSum / 2$|
|**Early Exit Rule**|None|Returns `FALSE` if $TotalSum$ is ODD|
|**Time Complexity**|$O(n \cdot target)$|$O(n \cdot TotalSum)$|
|**Auxiliary Space**|$O(n \cdot target)$ or $O(target)$|$O(n \cdot TotalSum)$ or $O(TotalSum)$|

---

6. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Full DP Table Trace & Subset Reconstruction (NAT)

Given set $S = \{2, 3, 5, 7\}$ ($n = 4$) and $target = 9$:

1. Construct the complete boolean DP table $DP[i][j]$ for $0 \le i \le 4$ and $0 \le j \le 9$.
2. Determine if a valid subset sum exists.
3. Backtrack to find the exact subset elements.

Step-by-Step Solution:

1. **Initialize Base Cases:**
    - Column $j=0$ is filled with `T` (`True`).
    - Row $i=0$ (for $j \ge 1$) is filled with `F` (`False`).
2. **Row 1 (**$w_1 = 2$**):**
    - $j=1$: $2 > 1 \implies F$.
    - $j=2$: $2 \le 2 \implies DP[0][0] = T$.
    - $j \ge 3$: $F$.
    - Row 1: `[T, F, T, F, F, F, F, F, F, F]`
3. **Row 2 (**$w_2 = 3$**):**
    - $j=1$: $F$.
    - $j=2$: Inherits $T$ from Row 1.
    - $j=3$: $DP[1][3] \lor DP[1][0] = F \lor T = T$.
    - $j=5$: $DP[1][5] \lor DP[1][2] = F \lor T = T$.
    - Row 2: `[T, F, T, T, F, T, F, F, F, F]`
4. **Row 3 (**$w_3 = 5$**):**
    - Inherits $j=2, 3, 5 \implies T$.
    - $j=7$: $DP[2][7] \lor DP[2][2] = F \lor T = T$.
    - $j=8$: $DP[2][8] \lor DP[2][3] = F \lor T = T$.
    - Row 3: `[T, F, T, T, F, T, F, T, T, F]`
5. **Row 4 (**$w_4 = 7$**):**
    - Inherits $j=2, 3, 5, 7, 8 \implies T$.
    - $j=9$: $DP[3][9] \lor DP[3][2] = F \lor T = \mathbf{T}$.
    - Row 4: `[T, F, T, T, F, T, F, T, T, T]`

Completed Boolean DP Table ($T = \text{True}, F = \text{False}$):

|$i \backslash j$|**0**|**1**|**2**|**3**|**4**|**5**|**6**|**7**|**8**|**9**|
|---|---|---|---|---|---|---|---|---|---|---|
|**0**|T|F|F|F|F|F|F|F|F|F|
|**1 (2)**|T|F|T|F|F|F|F|F|F|F|
|**2 (3)**|T|F|T|T|F|T|F|F|F|F|
|**3 (5)**|T|F|T|T|F|T|F|T|T|F|
|**4 (7)**|T|F|T|T|F|T|F|T|T|**T**|

1. **Backtracking to Extract Subset from** $DP[4][9] = T$**:**
    - Compare $DP[4][9] = T$ with $DP[3][9] = F$: They differ $\implies$ **Element** $w_4 = 7$ **is INCLUDED**.
    - New target $= 9 - 7 = 2$.
    - Move to $DP[3][2] = T$. Compare with $DP[2][2] = T$: Identical $\implies$ Element $w_3 = 5$ is EXCLUDED.
    - Move to $DP[2][2] = T$. Compare with $DP[1][2] = T$: Identical $\implies$ Element $w_2 = 3$ is EXCLUDED.
    - Move to $DP[1][2] = T$. Compare with $DP[0][2] = F$: They differ $\implies$ **Element** $w_1 = 2$ **is INCLUDED**.
    - Target becomes $2 - 2 = 0$ (Complete).

**Answer:**

1. Target sum 9 is achievable: **TRUE**.
2. Extracted Subset: $\{2, 7\}$ (Sum $= 2 + 7 = 9$).

---

Problem 2: Equal Partition Verification (MCQ)

Can the set $S = \{1, 5, 11, 5\}$ be partitioned into two subsets of equal sum?

- (A) No, because $TotalSum$ is odd.
- (B) Yes, into $\{1, 5, 5\}$ and $\{11\}$.
- (C) No, because no subset sums to 11.
- (D) Yes, into $\{1, 11\}$ and $\{5, 5\}$.

Step-by-Step Solution:

1. Calculate $TotalSum = 1 + 5 + 11 + 5 = 22$.
2. Check parity: 22 is EVEN, so equal partition is possible.
3. Compute target sum: $target = 22 / 2 = 11$.
4. Test for a subset summing to 11:
    - Subset $S_1 = \{1, 5, 5\} \implies 1 + 5 + 5 = 11$.
    - Remaining subset $S_2 = \{11\} \implies 11$.
5. Both subsets have equal sum 11.

**Answer:** **(B)**.

---

Problem 3: Odd Sum Fast Exit Property (MCQ)

What is the time complexity of the Equal Partition algorithm on a set of $n$ numbers if the sum of all elements in the set is an odd number?

- (A) $O(n \cdot TotalSum)$
- (B) $O(2^n)$
- (C) $O(1)$
- (D) $O(n \log n)$

Step-by-Step Solution:

If the sum of all elements in the set is odd, it is mathematically impossible to divide the sum into two equal integer halves. The parity check condition `if (total_sum % 2 != 0) return False;` executes immediately after computing the array sum, exiting in $O(1)$ time after the initial $O(n)$ sum accumulation.

**Answer:** **(C)** (or $O(n)$ including sum accumulation).