CHAPTER 4: LONGEST COMMON SUBSEQUENCE (LCS) & STRING ALIGNMENT

---

1. STRING COMBINATORICS: SUBSTRINGS VS. SUBSEQUENCES

Before formulating the **Longest Common Subsequence (LCS)** problem, we must establish the precise mathematical definitions and combinatorial properties that distinguish a **Substring** from a **Subsequence**.

```
                             String Derivatives
                                     │
         ┌───────────────────────────┴───────────────────────────┐
         ▼                                                       ▼
  Substring (Contiguous)                               Subsequence (Non-Contiguous)
 - Continuous block of characters                      - Relative order MUST be preserved
 - Deletions between elements forbidden                - Intermediate elements may be deleted
 - Total Non-Empty: n(n+1)/2                           - Total Non-Empty: 2ⁿ - 1
 - Example for "ABC": A, B, C, AB, BC, ABC             - Example for "ABC": A, B, C, AB, BC, AC, ABC
```

---

A. Substring Definitions & Formulas

A **Substring** is a **contiguous** sequence of one or more characters extracted directly from a given string $S$ without breaking continuity.

Counting Total Substrings for a String of Length $n$:

For a string $S$ of length $n$:

- Substrings of length 1: $n$
- Substrings of length 2: $n - 1$
- Substrings of length 3: $n - 2$
- ...
- Substrings of length $n$: $1$

$$\text{Total Non-Empty Substrings} = \sum_{k=1}^n k = 1 + 2 + 3 + \dots + n = \mathbf{\frac{n(n + 1)}{2}}$$

$$\text{Total Substrings (Including Empty String } \epsilon) = \mathbf{\frac{n(n + 1)}{2} + 1}$$

- **Example:** For string $S = \text{"ABC"}$ (length $n = 3$):
    - Non-empty substrings $= \frac{3(4)}{2} = 6 \implies \{\text{"A", "B", "C", "AB", "BC", "ABC"}\}$.

---

B. Subsequence Definitions & Formulas

A **Subsequence** is a sequence of characters derived from a given string $S$ by deleting zero or more characters **without altering the relative order** of the remaining characters. Intermediate characters do not need to be contiguous.

Counting Total Subsequences for a String of Length $n$:

Every character in string $S$ has **2 independent choices**: either it is **included** in the subsequence or **excluded**.

$$\text{Total Subsequences} = \underbrace{2 \times 2 \times \dots \times 2}_{n \text{ times}} = \mathbf{2^n}$$

$$\text{Total Non-Empty Subsequences} = \mathbf{2^n - 1}$$

$$\text{Total Empty Subsequences } (\phi) = \mathbf{1}$$

- **Example:** For string $S = \text{"ABC"}$ (length $n = 3$):
    - Total subsequences $= 2^3 = 8 \implies \{\phi, \text{"A", "B", "C", "AB", "BC", "AC", "ABC"}\}$.
    - Notice that **"AC"** is a valid subsequence (preserves relative order A before C) but is **NOT** a substring (not contiguous).

---

C. Set Hierarchy Relationship

```
                       ┌─────────────────────────────┐
                       │        Subsequences         │
                       │          (2ⁿ)               │
                       │   ┌─────────────────────┐   │
                       │   │     Substrings      │   │
                       │   │     (n(n+1)/2)      │   │
                       │   └─────────────────────┘   │
                       └─────────────────────────────┘
```

💡 **THE HIERARCHY RULE:** **Every substring is inherently a subsequence, BUT a subsequence may or may not be a substring.**

---

2. PROBLEM FORMULATION & BRUTE-FORCE VS. DP

Given two input strings $X = x_1 x_2 \dots x_m$ of length $m$ and $Y = y_1 y_2 \dots y_n$ of length $n$, the **Longest Common Subsequence (LCS)** problem seeks to find a sequence $Z = z_1 z_2 \dots z_k$ of maximum length $k$ that is a common subsequence of both $X$ and $Y$.

```
String X (length m):  [ A,  B,  C,  B,  D,  A,  B ]
String Y (length n):  [ B,  D,  C,  A,  B,  A ]

Longest Common Subsequence Z: [ B,  C,  A,  B ]  (Length k = 4)
```

---

Brute-Force Enumeration Approach

1. Generate all $2^m$ possible subsequences of string $X$.
2. For each generated subsequence, check if it appears as a subsequence in string $Y$ in $O(n)$ time.
3. Track the maximum length common subsequence.

Complexity of Brute-Force:

- **Time Complexity:** $O(n \cdot 2^m)$ — **Exponential time**, completely unusable for strings of length $m, n > 20$.

---

Dynamic Programming Approach

Because characters in strings $X$ and $Y$ are evaluated sequentially, sub-string matching problems contain **overlapping subproblems** and **optimal substructure**. Dynamic Programming reduces the search time from exponential $O(2^m)$ to **polynomial** $O(m \cdot n)$.

---

3. DYNAMIC PROGRAMMING FORMULATION & RECURRENCE

State Definition

Let $L[i][j]$ (or $DP[i][j]$) represent the **length of the Longest Common Subsequence** between the prefix string $X[1 \dots i]$ of length $i$ and the prefix string $Y[1 \dots j]$ of length $j$ (where $0 \le i \le m$ and $0 \le j \le n$).

```
                       DP State Cell L[i][j]
         Prefix X[1...i] ──►  [ x₁,  x₂,  ...,  xᵢ ]
         Prefix Y[1...j] ──►  [ y₁,  y₂,  ...,  yⱼ ]
```

---

The Fundamental Recurrence Equation

When evaluating characters $X[i]$ and $Y[j]$ at index $i$ and $j$:

1. **Base Cases (Empty String):** If either string prefix has length 0 ($i = 0$ or $j = 0$), no common subsequence can exist: $$L[i][j] = 0$$
2. **Case 1 (Character Match** $X[i] == Y[j]$**):** The matching character contributes **1 unit of length** to the LCS. We recurse on the remaining prefixes of both strings: $$L[i][j] = 1 + L[i-1][j-1]$$
3. **Case 2 (Character Mismatch** $X[i] \neq Y[j]$**):** The two characters cannot both contribute to the same LCS position. We take the maximum length obtained by either excluding $X[i]$ or excluding $Y[j]$: $$L[i][j] = \max\Big(L[i-1][j], \, L[i][j-1]\Big)$$

$$L[i][j] = \begin{cases} 0 & \text{if } i = 0 \text{ or } j = 0 \quad (\text{Base Case}) \\ 1 + L[i-1][j-1] & \text{if } X[i] == Y[j] \\ \max\Big(L[i-1][j], \, L[i][j-1]\Big) & \text{if } X[i] \neq Y[j] \end{cases}$$

---

4. TABULATION & BACKTRACKING RECONSTRUCTION

Step-by-Step Pseudocode Implementation

```
Algo LCS_Tabulation(X, Y, m, n) {
    // Step 1: Create DP table L of size (m + 1) x (n + 1)
    int L[m + 1][n + 1];
    
    // Step 2: Initialize Base Cases (Row 0 and Column 0 to 0)
    for (i = 0; i <= m; i++) L[i] = 0;
    for (j = 0; j <= n; j++) L[j] = 0;
    
    // Step 3: Fill DP table row-by-row in bottom-up manner
    for (i = 1; i <= m; i++) {
        for (j = 1; j <= n; j++) {
            if (X[i] == Y[j]) {
                // Character match: Diagonal transition + 1
                L[i][j] = 1 + L[i - 1][j - 1];
            } else {
                // Mismatch: Max of Top cell vs. Left cell
                L[i][j] = Max(L[i - 1][j], L[i][j - 1]);
            }
        }
    }
    
    return L[m][n]; // Length of Longest Common Subsequence
}
```

---

Backtracking to Reconstruct the Actual LCS String

Filling table $L[m][n]$ gives the **length** of the LCS. To extract the **actual character string**, we trace backwards from cell $L[m][n]$ to cell $L$:

```
Algo ReconstructLCS(X, Y, L, m, n) {
    i = m;
    j = n;
    string lcs_str = "";
    
    while (i > 0 && j > 0) {
        if (X[i] == Y[j]) {
            lcs_str.prepend(X[i]); // Add matching character
            i = i - 1;             // Move diagonally up-left
            j = j - 1;
        } 
        else if (L[i - 1][j] >= L[i][j - 1]) {
            i = i - 1;             // Move Up
        } 
        else {
            j = j - 1;             // Move Left
        }
    }
    
    return lcs_str;
}
```

---

5. COMPLEXITY & SPACE OPTIMIZATION

6. **Time Complexity:**
    - The DP table contains $(m + 1) \times (n + 1)$ cells.
    - Computing each cell takes $O(1)$ time.
    - **Overall Time Complexity:** $\mathbf{O(m \cdot n)}$.
7. **Auxiliary Space Complexity:**
    - Standard 2D DP matrix requires $\mathbf{O(m \cdot n)}$ space.
8. **Space Optimization (Two Rows / 1D Array):**
    - Computing entry $L[i][j]$ relies **only on the current row** $i$ **and previous row** $i-1$.
    - Space can be reduced to $O(\min(m, n))$ using two rows of length $\min(m, n)$ if only the length of the LCS is required.

---

6. COMPARATIVE SUMMARY MATRIX

|Parameter / Feature|Substrings|Subsequences|
|---|---|---|
|**Contiguity Requirement**|**Strictly Contiguous**|Non-Contiguous (Deletions allowed)|
|**Relative Order**|Preserved|**Preserved**|
|**Total Non-Empty Count**|$\mathbf{\frac{n(n+1)}{2}}$|$\mathbf{2^n - 1}$|
|**Matching Algorithm**|KMP / Rabin-Karp ($O(m+n)$)|Dynamic Programming ($O(m \cdot n)$)|
|**Primary Goal in DP**|Maximize contiguous match length|Maximize relative order common match|

---

7. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Substring vs. Subsequence Counting (NAT)

Given the string $S = \text{"ALGORITHM"}$ of length $n = 9$ with all distinct characters:

1. Calculate the total number of non-empty substrings.
2. Calculate the total number of non-empty subsequences.
3. Calculate the ratio of total non-empty subsequences to non-empty substrings.

Step-by-Step Solution:

1. **Total Non-Empty Substrings:** $$\text{Substrings} = \frac{n(n + 1)}{2} = \frac{9 \times 10}{2} = \mathbf{45}$$
2. **Total Non-Empty Subsequences:** $$\text{Subsequences} = 2^n - 1 = 2^9 - 1 = 512 - 1 = \mathbf{511}$$
3. **Ratio Calculation:** $$\text{Ratio} = \frac{511}{45} \approx \mathbf{11.355}$$

**Answer:** Non-empty Substrings = **45**, Non-empty Subsequences = **511**.

---

Problem 2: Full DP Matrix Trace & String Reconstruction (NAT)

Find the length of the Longest Common Subsequence and reconstruct the LCS string for:

- String $X = \text{"STONE"}$ (length $m = 5$)
- String $Y = \text{"LONGEST"}$ (length $n = 7$)

Step-by-Step Solution:

1. **Initialize Base Cases:** Row 0 and Column 0 are filled with 0s.
2. **Fill DP Table** $L[i][j]$**:**

|$X \backslash Y$|**0**|**L (1)**|**O (2)**|**N (3)**|**G (4)**|**E (5)**|**S (6)**|**T (7)**|
|---|---|---|---|---|---|---|---|---|
|**0**|0|0|0|0|0|0|0|0|
|**S (1)**|0|0|0|0|0|0|1|1|
|**T (2)**|0|0|0|0|0|0|1|2|
|**O (3)**|0|0|1|1|1|1|1|2|
|**N (4)**|0|0|1|2|2|2|2|2|
|**E (5)**|0|0|1|2|2|3|3|**3**|

1. **Trace Execution Details for Key Cells:**
    - Cell $L$1 (X1='O', Y2='O'): Match! $L$11$ = 1 + L$22$ = 1 + 0 = \mathbf{1}$.
    - Cell $L$4 (X4='N', Y1='N'): Match! $L$44$ = 1 + L$11$ = 1 + 1 = \mathbf{2}$.
    - Cell $L$5 (X5='E', Y5='E'): Match! $L$5$ = 1 + L$4$ = 1 + 2 = \mathbf{3}$.
2. **Backtrack to Extract String from** $L$55$ = 3$**:**
    - $L$55$ = 3$ (X5='E', Y6='T'): Mismatch $\to$ Move Left to $L$55$=3$.
    - $L$55$ = 3$ (X5='E', Y7='S'): Mismatch $\to$ Move Left to $L$5$=3$.
    - $L$5$ = 3$ (X5='E', Y5='E'): **MATCH 'E'** $\to$ Append **'E'**, Move diagonal to $L$4$=2$.
    - $L$4$ = 2$ (X4='N', Y4='G'): Mismatch $\to$ Move Left to $L$44$=2$.
    - $L$44$ = 2$ (X4='N', Y1='N'): **MATCH 'N'** $\to$ Append **'N'**, Move diagonal to $L$11$=1$.
    - $L$11$ = 1$ (X1='O', Y2='O'): **MATCH 'O'** $\to$ Append **'O'**, Move diagonal to $L$22$=0$.
3. **Reconstructed LCS String:** **"ONE"** (Length = 3).

**Answer:** LCS Length = **3**, LCS String = **"ONE"**.

---

Problem 3: Valid Subsequence Verification (MSQ)

Given the string $S = \text{"CABDABBCD"}$, which of the following option(s) represent **valid subsequences** of $S$?

- (A) "BBD"
- (B) "BAC"
- (C) "CDCA"
- (D) "CADC"

Step-by-Step Solution:

- **Option (A) "BBD":** Appears at indices 3 ('B'), 6 ('B'), 9 ('D') $\implies$ **VALID**.
- **Option (B) "BAC":** Appears at indices 3 ('B'), 4 ('A'), 8 ('C') $\implies$ **VALID**.
- **Option (C) "CDCA":** Requires 'A' after the second 'C', but after index 8 ('C') there is only index 9 ('D'). Order 'A' after 'C' fails $\implies$ **INVALID**.
- **Option (D) "CADC":** Appears at indices 1 ('C'), 2 ('A'), 5 ('D'), 8 ('C') $\implies$ **VALID**.

**Answer:** **(A), (B), and (D)**.

---

Problem 4: Multiple LCS Choices (GATE PYQ)

For two input strings $X = \text{"ABCBDAB"}$ and $Y = \text{"BDCABA"}$:

1. Calculate the maximum length of their common subsequence.
2. How many distinct common subsequences achieve this maximum length?

Step-by-Step Solution:

1. Constructing the DP matrix yields a maximum length $L$66$ = \mathbf{4}$.
2. Traceback paths yield three distinct valid strings of length 4:
    - **"BCAB"**
    - **"BCBA"**
    - **"BDAB"**

**Answer:** Maximum LCS Length = **4**, Number of distinct max-length LCS strings = **3**.

---

Problem 5: Time and Space Complexity of LCS (MCQ)

What is the time and space complexity to compute the length of the Longest Common Subsequence of two strings of lengths $m$ and $n$ using standard 2D Dynamic Programming?

- (A) Time: $O(m + n)$, Space: $O(m + n)$
- (B) Time: $O(m \cdot n)$, Space: $O(m \cdot n)$
- (C) Time: $O(2^{m+n})$, Space: $O(m \cdot n)$
- (D) Time: $O(m \cdot n)$, Space: $O(1)$

Step-by-Step Solution:

The algorithm fills an $(m+1) \times (n+1)$ matrix row-by-row, taking constant time per cell. The total time complexity is $O(m \cdot n)$, and storing the matrix requires $O(m \cdot n)$ auxiliary space.

**Answer:** **(B)**.