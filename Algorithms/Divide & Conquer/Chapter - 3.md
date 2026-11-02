MERGE SORT & PERFORMANCE ANALYSIS

---

1. ALGORITHMIC MECHANICS & RECURSIVE TRACE

**Merge Sort** is an external/internal sorting technique based strictly on the Divide and Conquer design paradigm. It operates by dividing an unsorted array into two equal halves, recursively sorting each half, and then merging the two pre-sorted halves using the **Two-Way Merging Principle**.

```
                           Unsorted Array A[1 ... n]
                                       │
                      ┌────────────────┴────────────────┐
                      ▼                                 ▼
             Left Subarray                      Right Subarray
             A[low ... mid]                    A[mid+1 ... high]
                  │                                     │
                  ▼                                     ▼
          MergeSort(low, mid)                  MergeSort(mid+1, high)
                  │                                     │
                  └────────────────┬────────────────────┘
                                   ▼
                       Merge(low, mid, high)
```

---

Step-by-Step Divide & Conquer Execution Flow

1. **Divide (Split Point Calculation):** Find the midpoint index: $$\text{mid} = \left\lfloor \frac{\text{low} + \text{high}}{2} \right\rfloor$$ This divides the active array range $A[\text{low} \dots \text{high}]$ into two equal or near-equal subproblems: $A[\text{low} \dots \text{mid}]$ of size $\lceil n/2 \rceil$ and $A[\text{mid}+1 \dots \text{high}]$ of size $\lfloor n/2 \rfloor$.
2. **Conquer (Recursive Sorting):** Recursively call `MergeSort` on both subproblems until each subproblem contains a single element (where $\text{low} == \text{high}$). A single-element array is inherently sorted (base case).
3. **Combine (Merging Step):** Call `Merge(low, mid, high)` to combine the two sorted sub-arrays into a single unified sorted sequence using an auxiliary global array $B$.

---

Pseudo-Code Implementation

Top-Level Recursive Merge Sort

```
Algo MergeSort(A, low, high) {
    if (low < high) { // Small(P) is False if there are 2 or more elements
        mid = (low + high) / 2;          // Step 1: Divide
        
        MergeSort(A, low, mid);          // Step 2a: Solve Left Subproblem T(n/2)
        MergeSort(A, mid + 1, high);      // Step 2b: Solve Right Subproblem T(n/2)
        
        Merge(A, low, mid, high);        // Step 3: Combine/Merge in O(n) time
    }
}
```

---

The Combining Routine (`Merge`)

```
Algo Merge(A, low, mid, high) {
    h = low; 
    i = low; 
    j = mid + 1;
    
    // Compare and copy smaller elements into auxiliary array B
    while (h <= mid && j <= high) {
        if (A[h] <= A[j]) {
            B[i] = A[h];
            h++;
        } else {
            B[i] = A[j];
            j++;
        }
        i++;
    }
    
    // Copy remaining elements from Left sub-array (if any)
    if (h > mid) {
        for (k = j; k <= high; k++) {
            B[i] = A[k];
            i++;
        }
    } else { // Copy remaining elements from Right sub-array (if any)
        for (k = h; k <= mid; k++) {
            B[i] = A[k];
            i++;
        }
    }
    
    // Copy merged elements from auxiliary array B back to original array A
    for (k = low; k <= high; k++) {
        A[k] = B[k];
    }
}
```

---

Conceptual Trace Example

Consider sorting an unsorted array $A =$1more_horiz of size $n = 10$:

```
Top-Down Splitting Phase:
                       (1, 10) [mid = 5]
                       /       \
               (1, 5)             (6, 10)
              /      \           /       \
          (1, 3)    (4, 5)   (6, 8)     (9, 10)
         /     \       |      /    \       |
      (1, 2)  (3, 3) (4, 4) (6, 7) (8, 8) (9, 9)
      /    \
   (1, 1) (2, 2)

Bottom-Up Merging Phase:
  - Merge [1] & [2]          ──► [1, 2]
  - Merge [1, 2] & [3]      ──► [1-3]
  - Merge [4] & [5]           ──► [4, 5]
  - Merge [1-3] & [4, 5] ──► [1-5]
  - Merge [6] & [7]          ──► [6, 7]
  - Merge [6, 7] &      ──► [6, 7]
  - Merge [8] & [9]           ──► [8, 9]
  - Merge [6, 7] & [8, 9]──► [6-9]
  - Final Merge Left & Right   ──► [1-9]
```

---

2. DERIVATION OF TIME & SPACE COMPLEXITY

A. Time Complexity Analysis

The execution time $T(n)$ of Merge Sort on an array of size $n$ is modeled by the symmetric Divide and Conquer recurrence:

$$T(n) = \begin{cases} C & \text{for } n = 1 \quad (\text{Base Case: Small Problem}) \\ 2 T\left(\frac{n}{2}\right) + b \cdot n & \text{for } n > 1 \quad (\text{Recursive Step}) \end{cases}$$

Here, $2 T(n/2)$ represents the time to sort both halves recursively, and $b \cdot n$ represents the linear time spent during the `Merge` function.

---

Step-by-Step Back-Substitution Derivation

1. **Base Equation:** $$T(n) = 2 T(n/2) + b \cdot n \quad \text{--- (Equation 1)}$$
2. **First Expansion:** Express $T(n/2) = 2 T(n/2^2) + b \cdot (n/2)$: $$T(n) = 2 \left[2 T\left(\frac{n}{2^2}\right) + b \cdot \frac{n}{2}\right] + b \cdot n = 2^2 T\left(\frac{n}{2^2}\right) + b \cdot n + b \cdot n = 2^2 T\left(\frac{n}{2^2}\right) + 2 b n$$
3. **Second Expansion:** Express $T(n/2^2) = 2 T(n/2^3) + b \cdot (n/2^2)$: $$T(n) = 2^2 \left[2 T\left(\frac{n}{2^3}\right) + b \cdot \frac{n}{2^2}\right] + 2 b n = 2^3 T\left(\frac{n}{2^3}\right) + b \cdot n + 2 b n = 2^3 T\left(\frac{n}{2^3}\right) + 3 b n$$
4. **General Expansion at Step** $k$**:** $$T(n) = 2^k T\left(\frac{n}{2^k}\right) + k \cdot b \cdot n$$
5. **Apply Base Condition Boundary:** Set subproblem size equal to 1: $$\frac{n}{2^k} = 1 \implies 2^k = n \implies k = \log_2 n$$
6. **Substitute** $k = \log_2 n$ **and** $2^k = n$ **into the General Expansion:** $$T(n) = n \cdot T(1) + (\log_2 n) \cdot b \cdot n = n \cdot C + b \cdot n \log_2 n$$

$$\mathbf{T(n) = \Theta(n \log_2 n)}$$

---

Case Analysis (Best, Worst, and Average Cases)

- **Best Case Comparison Count:** Occurs during merging when all elements in one sub-array are smaller than the first element of the other sub-array. The merging phase requires $n/2$ comparisons per level. $$T_{\text{best}}(n) = \mathbf{\Omega(n \log_2 n)}$$
- **Worst Case Comparison Count:** Occurs when elements strictly alternate during every merge step. The merging phase requires $n - 1$ comparisons per level. $$T_{\text{worst}}(n) = \mathbf{O(n \log_2 n)}$$
- **Overall Time Complexity:** Since the best, worst, and average cases differ only by constant multiplier factors, Merge Sort has a tight bound of: $$\mathbf{T(n) = \Theta(n \log_2 n) \quad \text{in ALL cases}}$$

---

B. Space Complexity Analysis

The memory footprint of Merge Sort is divided into two distinct components:

```
                        Merge Sort Auxiliary Space
                                    │
           ┌────────────────────────┴────────────────────────┐
           ▼                                                 ▼
 Auxiliary Array 'B'                               Recursion Call Stack
- Used by Merge routine to hold sorted output    - Height of binary call tree
- Requires array of size n                       - Maximum simultaneous frames
- Memory = O(n)                                  - Memory = O(log₂ n)
```

1. **Temporary Auxiliary Array Space:** The `Merge` algorithm requires an extra array $B$ of size $n$ to combine elements $\implies \mathbf{O(n)}$.
2. **Recursion Call Stack Memory:** The depth of the binary call tree is $k = \log_2 n$. At any given moment, the call stack holds at most $\log_2 n + 1$ activation records $\implies \mathbf{O(\log_2 n)}$.

$$\text{Total Auxiliary Space} = O(n) + O(\log_2 n) = \mathbf{O(n)}$$

---

3. ALGORITHMIC PROPERTIES & BOTTOM-UP VARIANT

Core Algorithmic Properties

1. **Inplace Status: NOT INPLACE.** An algorithm is called "Inplace" if it uses $O(1)$ auxiliary space. Since Merge Sort requires $O(n)$ auxiliary array memory, it is **Not Inplace** (Space Inefficient).
2. **Stability Status: STABLE.** An algorithm is "Stable" if it preserves the relative original order of duplicate elements. Merge Sort is **Stable** because during merging, the condition `A[h] <= A[j]` explicitly favors copying elements from the left sub-array first when values are equal.

---

Performance Comparison Matrix

|Sorting Algorithm|Best-Case Time|Worst-Case Time|Average-Case Time|Auxiliary Space|Stable?|Inplace?|
|---|---|---|---|---|---|---|
|**Bubble Sort**|$O(n)$|$O(n^2)$|$O(n^2)$|$O(1)$|Yes|Yes|
|**Selection Sort**|$O(n^2)$|$O(n^2)$|$O(n^2)$|$O(1)$|No|Yes|
|**Insertion Sort**|$O(n)$|$O(n^2)$|$O(n^2)$|$O(1)$|Yes|Yes|
|**Merge Sort**|$\mathbf{\Theta(n \log n)}$|$\mathbf{\Theta(n \log n)}$|$\mathbf{\Theta(n \log n)}$|$\mathbf{O(n)}$|**Yes**|**No**|

---

Version-2: Iterative Bottom-Up Merge Sort (2-Way Merging)

Instead of top-down recursive splitting, **Bottom-Up Merge Sort** avoids function call stack overhead by starting with single-element sub-arrays and merging adjacent pairs iteratively in passes.

```
Input Array: [ 31,  25,  16,  60,  34,  40,  86,  24 ]

Pass 1 (Merge pairs of size 1):
  - Merge [10] & [11]  ──► [10, 11]
  - Merge [12] & [13]  ──► [12, 13]
  - Merge [14] & [5]  ──► [5, 14]
  - Merge  & [15]  ──► [15]
  State: [ 25, 31, 16, 60, 34, 40, 24, 86 ]

Pass 2 (Merge pairs of size 2):
  - Merge [10, 11] & [12, 13]  ──► [10-13]
  - Merge [5, 14] & [15]  ──► [5, 14, 15]
  State: [ 16, 25, 31, 60, 24, 34, 40, 86 ]

Pass 3 (Merge pairs of size 4):
  - Merge [10-13] & [5, 14, 15] ──► [5, 10-15]
```

- **Number of Passes:** $\lceil \log_2 n \rceil$.
- **Work per Pass:** $O(n)$.
- **Time Complexity:** $\mathbf{O(n \log_2 n)}$.
- **Auxiliary Space Complexity:** $O(n)$ (No stack space needed, only work array).

---

4. COMPLETE SOLVED PROBLEM SET

---

Problem 1: Sorting Time Scaling Calculation (NAT)

A Merge Sort algorithm takes **30 seconds** to sort an array of **64 elements** in the worst case on a specific computer. Calculate the approximate number of elements that can be sorted by the same algorithm on the same machine in **6 minutes** (360 seconds).

Step-by-Step Solution:

1. Merge Sort running time formula: $T(n) = c \cdot n \log_2 n$.
2. Given for $n_1 = 64$: $$T(64) = c \cdot 64 \cdot \log_2(64) = c \cdot 64 \cdot 6 = 384c \text{ seconds}$$
3. Set equal to known time (30 seconds): $$384c = 30 \implies c = \frac{30}{384} = \frac{5}{64} \text{ seconds}$$
4. Now, find $n_2$ for total time $T(n_2) = 6 \text{ minutes} = 360 \text{ seconds}$: $$c \cdot n_2 \log_2 n_2 = 360$$
5. Substitute $c = \frac{5}{64}$: $$\left(\frac{5}{64}\right) n_2 \log_2 n_2 = 360 \implies n_2 \log_2 n_2 = 360 \times \frac{64}{5} = 72 \times 64 = 4608$$
6. Test powers of 2 for $n_2$:
    - Try $n_2 = 512 = 2^9$: $$512 \times \log_2(512) = 512 \times 9 = 4608$$
    - Exact match!

**Answer:** **512 elements**.

---

Problem 2: Number of Merging Passes (NAT)

In an iterative Bottom-Up 2-Way Merge Sort, how many merging passes are required to sort an array of $n = 100$ elements?

Step-by-Step Solution:

1. The number of passes in Bottom-Up Merge Sort is given by: $$P = \lceil \log_2 n \rceil$$
2. Substitute $n = 100$: $$\log_2(100) \approx 6.643$$
3. Taking the ceiling: $$P = \lceil 6.643 \rceil = \mathbf{7 \text{ passes}}$$

**Answer:** **7 passes**.

---

Problem 3: Auxiliary Memory Usage Breakdown

For sorting an array of size $n = 2048$ using recursive Merge Sort, calculate:

1. The exact size of the auxiliary merging array in elements.
2. The maximum stack depth of activation records.

Step-by-Step Solution:

1. **Auxiliary Array Size:** The merging procedure requires a global auxiliary array $B$ equal to the input array size: $$\text{Array Size} = n = \mathbf{2048 \text{ elements}}$$
2. **Maximum Stack Depth:** Equal to the binary tree height: $$\text{Stack Depth} = \log_2(2048) = \log_2(2^{11}) = \mathbf{11 \text{ frames}}$$

---

Problem 4: Multiple Choice Question (MCQ)

Which of the following statements regarding Merge Sort is **TRUE**?

- (A) Merge Sort is Inplace and Stable.
- (B) Merge Sort is Not Inplace, but it is Stable.
- (C) Merge Sort is Inplace, but Not Stable.
- (D) Merge Sort is Neither Inplace nor Stable.

Step-by-Step Solution:

Merge Sort requires an auxiliary array of size $O(n)$ to merge elements, so it is **Not Inplace**. During merging, equal elements are drawn from the left sub-array first (`A[h] <= A[j]`), maintaining relative order, so it is **Stable**.

**Answer:** **(B)**.

---

Problem 5: Recurrence Equation Solving (MCQ)

What is the tight asymptotic bound ($\Theta$) for an algorithm whose running time is defined by $T(n) = 2 T(n/2) + 17n$ with base case $T(1) = 5$?

- (A) $\Theta(n)$
- (B) $\Theta(n^2)$
- (C) $\Theta(n \log n)$
- (D) $\Theta(\log n)$

Step-by-Step Solution:

1. Identify Master's Theorem parameters: $a = 2, b = 2, f(n) = 17n \implies k = 1, p = 0$.
2. Compute $\log_b a = \log_2 2 = 1$.
3. Since $f(n) = \Theta(n^1)$ matches $n^{\log_b a} = n^1$ (Case 2 of Master's Theorem): $$T(n) = \Theta(n^{\log_b a} \log^{p+1} n) = \Theta(n \log n)$$

**Answer:** **(C)**.