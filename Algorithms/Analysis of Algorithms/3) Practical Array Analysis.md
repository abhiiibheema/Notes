3: PRACTICAL ARRAY ANALYSIS (ARRAY LEADERS CASE STUDY)

---

1. INTUITION & PROBLEM DEFINITION

Array processing often requires evaluating relationships between an element and its surrounding elements. A classic algorithmic problem that demonstrates how clever design transforms quadratic time complexity into linear time is the **Array Leaders Problem**.

Definition of an Array Leader

An element $A[i]$ in an array $A$ of size $n$ is called an **Array Leader** if it is strictly greater than all elements located to its right.

$$A[i] \text{ is a Leader } \iff A[i] > A[j] \quad \forall j \in \{i+1, i+2, \dots, n\}$$

Fundamental Invariant

- The **rightmost element** ($A[n]$ using 1-based indexing, or $A[n-1]$ using 0-based indexing) is **always a Leader** because there are no elements to its right.

---

Conceptual Trace Example

Consider the array $A$ of size $n = 6$:

```
Index:      1     2     3     4     5     6
Array A:  [ 29,    5,   16,   14,    8,    9 ]
```

Let's evaluate each element from left to right against the Leader condition:

- **Index 6 (**$A$1$ = 9$**):** Rightmost element. No elements to its right $\implies$ **Leader** $\checkmark$.
- **Index 5 (**$A$2$ = 8$**):** Elements to right: $\{9\}$. Since $8 < 9$, it is not greater than all elements to its right $\implies$ **Not a Leader** $\times$.
- **Index 4 (**$A$3$ = 14$**):** Elements to right: $\{8, 9\}$. Since $14 > 8$ and $14 > 9 \implies$ **Leader** $\checkmark$.
- **Index 3 (**$A$4$ = 16$**):** Elements to right: $\{14, 8, 9\}$. Since $16 > 14, 8, 9 \implies$ **Leader** $\checkmark$.
- **Index 2 (**$A$5$ = 5$**):** Elements to right: $\{16, 14, 8, 9\}$. Since $5 < 16 \implies$ **Not a Leader** $\times$.
- **Index 1 (**$A$6$ = 29$**):** Elements to right: $\{5, 16, 14, 8, 9\}$. Since $29 > 5, 16, 14, 8, 9 \implies$ **Leader** $\checkmark$.

**Output Leaders:** $29, 16, 14, 9$.

---

2. THE NAIVE APPROACH (BRUTE FORCE)

Algorithmic Mechanism

The brute force approach uses nested loops:

1. The outer loop iterates through each element $A[i]$ from index $1$ to $n-1$.
2. The inner loop compares $A[i]$ against every element $A[j]$ to its right (from $j = i+1$ to $n$).
3. If any $A[j] > A[i]$, the inner loop terminates immediately (`break`) because $A[i]$ cannot be a Leader.
4. If the inner loop completes without breaking (i.e., $j$ reaches $n+1$), $A[i]$ is printed as a Leader.

```
Algo Leader_BruteForce(A[], n) {
    for (i = 1; i < n; i++) {
        for (j = i + 1; j <= n; j++) {
            if (A[i] < A[j]) {
                break;
            }
        }
        if (j == n + 1) {
            print(A[i]);
        }
    }
    print(A[n]); // Rightmost element is always a leader
}
```

---

Time Complexity Analysis of Brute Force

A. Best Case Analysis

- **Input Configuration:** Array elements are arranged in **strictly increasing order** (e.g., $A =$2more_horiz).
- **Execution Trace:**
    - For $i = 1$ ($A$6$ = 5$), the inner loop compares $A$6 with $A$5$ = 10$. Since $5 < 10$, the condition `A[i] < A[j]` is immediately true, and the inner loop breaks on its **very first comparison**.
    - This immediate break happens for every element $i$ from $1$ to $n-1$.
- **Total Comparisons:** $$T_{\text{best}}(n) = (n - 1) \times 1 = n - 1$$
- **Best Case Time Complexity:** $\mathbf{O(n)}$.

---

B. Worst Case Analysis

- **Input Configuration:** Array elements are arranged in **strictly decreasing order** (e.g., $A =$2more_horiz).
- **Execution Trace:**
    - For $i = 1$ ($A$6$ = 18$), $18$ is greater than all right elements, so the inner loop never breaks and performs $n - 1$ comparisons.
    - For $i = 2$ ($A$5$ = 15$), the inner loop performs $n - 2$ comparisons.
    - For $i = n-1$, the inner loop performs $1$ comparison.
- **Total Comparisons:** $$T_{\text{worst}}(n) = (n - 1) + (n - 2) + (n - 3) + \dots + 1 = \frac{(n - 1)n}{2} = \frac{n^2 - n}{2}$$
- **Worst Case Time Complexity:** $\mathbf{O(n^2)}$.

---

3. THE OPTIMIZED APPROACH (RIGHT-TO-LEFT SCAN)

Algorithmic Insight

The primary inefficiency of the brute force approach is that it repeatedly scans the right portion of the array for every single element.

Instead of looking to the right from every element, we can **scan the array once from right to left** while maintaining a single running variable: the **maximum element seen so far** ($L$).

An element $A[i]$ is a Leader if and only if: $$A[i] > L \quad \text{where } L = \max(A[i+1], A[i+2], \dots, A[n])$$

```
Algo Leader_Optimized(A[], n) {
    print(A[n]);      // Rightmost element is always a leader
    L = A[n];         // Initialize max_so_far with rightmost element
    
    for (i = n - 1; i >= 1; i--) {
        if (A[i] > L) {
            L = A[i]; // Update the maximum seen so far
            print(A[i]);
        }
    }
}
```

---

Step-by-Step Code Walkthrough

Using the array $A =$2more_horiz of size $n = 6$:

1. **Initialize:** Print $A$1$ = 9$. Set $L = 9$.
2. $i = 5$ **(**$A$2$ = 8$**):** Is $8 > 9$? False. $L$ remains $9$.
3. $i = 4$ **(**$A$3$ = 14$**):** Is $14 > 9$? True. Print $14$. Update $L = 14$.
4. $i = 3$ **(**$A$4$ = 16$**):** Is $16 > 14$? True. Print $16$. Update $L = 16$.
5. $i = 2$ **(**$A$5$ = 5$**):** Is $5 > 16$? False. $L$ remains $16$.
6. $i = 1$ **(**$A$6$ = 29$**):** Is $29 > 16$? True. Print $29$. Update $L = 29$.

**Printed Leaders:** $9, 14, 16, 29$ (printed right-to-left, representing the set $\{29, 16, 14, 9\}$).

---

Complexity Analysis of Optimized Approach

- **Loop Executions:** The loop runs exactly $n - 1$ times regardless of the arrangement of elements in the array.
- **Work per Iteration:** 1 comparison (`A[i] > L`) and at most 1 variable assignment per loop pass $\implies O(1)$ work.
- **Time Complexity:**
    - **Best Case:** $\mathbf{O(n)}$ (when array is strictly increasing).
    - **Worst Case:** $\mathbf{O(n)}$ (when array is strictly decreasing).
    - **Average Case:** $\mathbf{\Theta(n)}$ for all input distributions.
- **Auxiliary Space Complexity:** $\mathbf{O(1)}$ because only a single variable $L$ is used beyond the input array.

---

4. COMPARATIVE SUMMARY MATRIX

|Metric|Brute Force Approach|Optimized Right-to-Left Scan|
|---|---|---|
|**Best-Case Time Complexity**|$O(n)$|$O(n)$|
|**Worst-Case Time Complexity**|$O(n^2)$|$O(n)$|
|**Average-Case Time Complexity**|$O(n^2)$|$O(n)$|
|**Auxiliary Space Complexity**|$O(1)$|$O(1)$|
|**Number of Array Passes**|$n-1$ nested passes|Exactly $1$ single pass|
|**Best Input Order**|Strictly Increasing|Any Input Order|
|**Worst Input Order**|Strictly Decreasing|Any Input Order|

---

5. COMPLETE SOLVED PROBLEM SET

---

Problem 1: Leader Array Identification

Given the array $A =$2more_horiz, identify all Leaders and trace the values of $L$ using the optimized Right-to-Left algorithm.

Step-by-Step Solution:

1. **Start at** $i = 6$ **(**$A$1$ = 2$**):** Print $2$. Set $L = 2$.
2. $i = 5$ **(**$A$2$ = 5$**):** Is $5 > 2$? True. Print $5$. Set $L = 5$.
3. $i = 4$ **(**$A$3$ = 3$**):** Is $3 > 5$? False.
4. $i = 3$ **(**$A$4$ = 4$**):** Is $4 > 5$? False.
5. $i = 2$ **(**$A$5$ = 17$**):** Is $17 > 5$? True. Print $17$. Set $L = 17$.
6. $i = 1$ **(**$A$6$ = 16$**):** Is $16 > 17$? False.

**Answer:** The Leaders are $17, 5, 2$.

---

Problem 2: Numerical Comparison Count (NAT)

An array contains $n = 100$ elements. Determine the exact number of element-to-element comparisons performed by:

1. The Brute Force algorithm in its Worst Case.
2. The Optimized algorithm in any case.

Step-by-Step Solution:

1. **Brute Force Worst Case Comparisons:** $$C_{\text{worst}} = \frac{n(n - 1)}{2} = \frac{100 \times 99}{2} = 4950 \text{ comparisons}$$
2. **Optimized Algorithm Comparisons:** The loop runs $n - 1$ times, performing 1 comparison per iteration: $$C = n - 1 = 100 - 1 = 99 \text{ comparisons}$$

**Answer:**

1. **4950 comparisons**
2. **99 comparisons**

---

Problem 3: Standard Multiple Choice Question (MCQ)

An element in an array is called a **Leader** if it is greater than all elements to the right of it. The time complexity of the most efficient algorithm to print all Leaders of a given array of size $n$ is:

- (A) $O(\log n)$
- (B) $O(n)$
- (C) $O(n \log n)$
- (D) $O(n^2)$

Step-by-Step Solution:

Using the single-pass Right-to-Left scanning algorithm, we iterate through the array once in $n-1$ steps, doing $O(1)$ comparisons per element. Thus, the time complexity of the most efficient algorithm is $O(n)$.

**Answer:** **(B)** $O(n)$.

---

Problem 4: Modified Leader Definition with Duplicates

Suppose the definition of a Leader is modified to: "An element is a Leader if it is **greater than or equal to** ($\ge$) all elements to its right." How does this change affect:

1. The algorithm's time complexity?
2. The handling of duplicate maximum elements (e.g., $A =$2more_horiz)?

Step-by-Step Solution:

1. **Time Complexity:** The loop structure remains identical (scanning right-to-left in $n-1$ steps). Therefore, the time complexity remains $O(n)$.
2. **Duplicate Handling:** Changing the condition from `A[i] > L` to `A[i] >= L` allows equal maximum elements to be recognized as Leaders.
    - For $A =$2more_horiz:
        - $A$3$ = 2 \implies$ Leader, $L = 2$.
        - $A$4$ = 10 \implies 10 \ge 2 \implies$ Leader, $L = 10$.
        - $A$5$ = 5 \implies 5 \ge 10 \implies$ Not a Leader.
        - $A$6$ = 10 \implies 10 \ge 10 \implies$ Leader, $L = 10$.
    - Both occurrences of $10$ are printed as Leaders: $10, 10, 2$.