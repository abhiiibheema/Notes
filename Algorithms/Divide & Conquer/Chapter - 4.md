4: QUICK SORT & PARTITIONING ANALYSIS

---

1. PHILOSOPHY & ALGORITHMIC PARADIGM

**Quick Sort** is an internal, in-place sorting algorithm designed around the **Divide and Conquer** paradigm. While algorithms like Merge Sort divide an array strictly at its geometric midpoint ($n/2$) and perform heavy work ($O(n)$) during the _Combine_ step, Quick Sort performs its primary computational work during the **Divide phase** using a **Partitioning Algorithm**, completely eliminating the need for a combine phase.

```
                           Unsorted Array A[p ... q]
                                       │
                                       ▼
                       Partition(A, p, q+1)  ──► O(n) Work (Divide)
                                       │
                     ┌─────────────────┴─────────────────┐
                     ▼                                   ▼
             Left Subarray                       Right Subarray
             A[p ... j-1]                        A[j+1 ... q]
         (All elements ≤ Pivot)              (All elements ≥ Pivot)
                     │                                   │
                     ▼                                   ▼
              QuickSort(p, j-1)                   QuickSort(j+1, q)
                     │                                   │
                     └─────────────────┬─────────────────┘
                                       ▼
                         No Combining Step Needed!
```

---

Key Paradigm Differences: Merge Sort vs. Quick Sort

1. **Division Mechanism:**
    - **Merge Sort:** Divides the array deterministically at index $\text{mid} = \lfloor \frac{\text{low} + \text{high}}{2} \rfloor$ regardless of the element values.
    - **Quick Sort:** Divides the array dynamically based on the value and final sorted position of a chosen **pivot** element.
2. **Distribution of Work:**
    - **Merge Sort:** Minimal divide work ($O(1)$), heavy combine work ($O(n)$) during array merging.
    - **Quick Sort:** Heavy divide work ($O(n)$) during partitioning, **zero combine work** ($O(1)$).

---

2. THE PARTITIONING ALGORITHM MECHANICS

The **Partitioning Algorithm** is the foundational operation of Quick Sort. Given an array range $A[p \dots q]$, it selects a designated element as the **pivot** (by default, the first element $v = A[p]$) and rearranges the elements in-place to satisfy three structural invariants.

Invariants of Partitioning

1. The pivot element $v$ is placed at its **exact final position** $j$ in the sorted array.
2. Every element in the left sub-array $A[p \dots j-1]$ is **less than or equal to** the pivot ($\le v$).
3. Every element in the right sub-array $A[j+1 \dots q]$ is **greater than or equal to** the pivot ($\ge v$).

---

Step-by-Step Pseudo-Code Implementation

Top-Level Quick Sort Routine

```
Algo QuickSort(A, p, q) {
    if (p < q) { // Base case check: array has 2 or more elements
        // Step 1: Divide array using Partitioning routine
        j = Partition(A, p, q + 1); // Returns final pivot index j
        
        // Step 2: Recursively sort left and right subproblems
        QuickSort(A, p, j - 1);     // Left sub-array (elements <= pivot)
        QuickSort(A, j + 1, q);     // Right sub-array (elements >= pivot)
        
        // Step 3: Combine (No action required!)
    }
}
```

---

The Partitioning Routine

```
Algo Partition(A, m, p) {
    v = A[m]; // First element selected as pivot
    i = m; 
    j = p;
    
    do {
        // Advance pointer i rightward while elements are <= pivot
        do {
            i = i + 1;
        } while (A[i] <= v);
        
        // Advance pointer j leftward while elements are >= pivot
        do {
            j = j - 1;
        } while (A[j] >= v);
        
        // Swap out-of-order elements if pointers have not crossed
        if (i < j) {
            Swap(A, i, j);
        }
    } while (i < j);
    
    // Place pivot in its correct position by swapping with A[j]
    A[m] = A[j];
    A[j] = v;
    
    return j; // Return final pivot position
}

Algo Swap(A, i, j) {
    temp = A[i];
    A[i] = A[j];
    A[j] = temp;
}
```

---

3. COMPLEXITY ANALYSIS & CASE DERIVATIONS

The performance of Quick Sort depends entirely on the position where the pivot lands after partitioning at each recursive level.

---

Case 1: Best & Average Case Analysis (Balanced Partitioning)

Best Case Condition

Occurs when the partitioning routine **always places the pivot at the exact middle position**, dividing the array into two equal subproblems of size $\approx n/2$.

```
                      T(n)
                     / | \
                 T(n/2) v T(n/2)  ──► Partitioning Cost = O(n)
```

Recurrence Relation:

$$T(n) = 2 T\left(\frac{n}{2}\right) + O(n) \quad \text{with } T(1) = 0$$

Applying Master's Theorem ($a=2, b=2, k=1 \implies a = b^k$): $$T(n) = \mathbf{O(n \log_2 n)}$$

Average Case Condition

Even if the pivot does not split the array perfectly in half, any proportional split (such as $1/4 : 3/4$ or $1/10 : 9/10$) produces a recursion tree of logarithmic depth $O(\log n)$. $$T(n) = T(n/4) + T(3n/4) + O(n) \implies \mathbf{O(n \log_2 n)}$$

---

Case 2: Worst Case Analysis (Unbalanced Partitioning)

Worst Case Condition

Occurs when the partitioning routine **always places the pivot at an extreme boundary (first or last position)**, reducing the active problem size by only **1 element** at each recursive step (a split ratio of $0 : n-1$).

```
Input: [ 5, 10, 15, 20, 25 ]  (Already Sorted Ascending)
Pass 1: Pivot = 5  ──► Left: [] (0 elems), Right: [10, 15, 20, 25] (4 elems)
Pass 2: Pivot = 10 ──► Left: [] (0 elems), Right: [15, 20, 25] (3 elems)
```

Input Configurations Triggering Worst Case:

1. When the input array is **ALREADY SORTED in Ascending Order**.
2. When the input array is **ALREADY SORTED in Descending Order**.

---

Step-by-Step Back-Substitution Derivation for Worst Case

1. **Base Recurrence:** $$T(n) = T(n - 1) + n \quad \text{--- (Equation 1)}$$
2. **First Substitution:** Replace $T(n-1) = T(n-2) + (n-1)$: $$T(n) = [T(n-2) + (n-1)] + n = T(n-2) + (n-1) + n$$
3. **Second Substitution:** Replace $T(n-2) = T(n-3) + (n-2)$: $$T(n) = T(n-3) + (n-2) + (n-1) + n$$
4. **General Expansion at Step** $k$**:** $$T(n) = T(n-k) + \sum_{j=0}^{k-1} (n - j)$$
5. **Apply Base Condition Boundary:** Set $n - k = 1 \implies k = n - 1$: $$T(n) = T(1) + \left[2 + 3 + 4 + \dots + n\right] = T(1) + \frac{n(n + 1)}{2} - 1$$

$$\mathbf{T(n) = O(n^2)}$$

---

Space Complexity & Recursion Stack Depth

Quick Sort does not use any temporary auxiliary array to hold elements (it swaps elements in-place within array $A$). However, it consumes memory for its **recursion call stack**.

```
    Best/Average Stack Depth: O(log₂ n)          Worst Case Stack Depth: O(n)
           ┌──────────┐                                 ┌──────────┐
           │   n/2^k  │                                 │    1     │
           ├──────────┤                                 ├──────────┤
           │    ...   │                                 │   ...    │
           ├──────────┤                                 ├──────────┤
           │   n/2    │                                 │   n - 1  │
           ├──────────┤                                 ├──────────┤
           │    n     │                                 │    n     │
           └──────────┘                                 └──────────┘
```

- **Best/Average Case Auxiliary Space:** Maximum recursion stack height $= \mathbf{O(\log_2 n)}$.
- **Worst Case Auxiliary Space:** Maximum recursion stack height $= \mathbf{O(n)}$.

---

4. ALGORITHMIC PROPERTIES & SUMMARY MATRIX

Core Properties

1. **Inplace Status: INPLACE.** Quick Sort requires $O(1)$ auxiliary memory structures beyond the system call stack $\implies$ **Inplace**.
2. **Stability Status: UNSTABLE.** Long-distance swaps performed across the pivot during the `Partition` routine can alter the relative original order of duplicate key values $\implies$ **Unstable**.

---

Comprehensive Complexity Matrix

|Case|Time Complexity|Auxiliary Space (Stack Memory)|Partition Split Ratio|
|---|---|---|---|
|**Best Case**|$\mathbf{O(n \log_2 n)}$|$O(\log_2 n)$|Balanced ($n/2 : n/2$)|
|**Average Case**|$\mathbf{O(n \log_2 n)}$|$O(\log_2 n)$|Proportional ($n/4 : 3n/4$)|
|**Worst Case**|$\mathbf{O(n^2)}$|$O(n)$|Extreme ($0 : n-1$)|

---

5. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Time Comparison for Sorted Inputs

Let $P$ be a Quick Sort program that sorts numbers in ascending order using the first element as pivot. Let $t_1$ and $t_2$ be the execution times taken by $P$ for the inputs $A_1 = [10, 20, 30, 40]$ and $A_2 = [10, 20, 30, 40, 50]$ respectively. Which of the following holds?

- (A) $t_1 = t_2$
- (B) $t_1 > t_2$
- (C) $t_1 < t_2$
- (D) $t_1 = t_2 = 5 \log_2 5$

Step-by-Step Solution:

1. Both $A_1 = [10, 20, 30, 40]$ (sorted ascending, size $n_1 = 4$) and $A_2 = [10, 20, 30, 40, 50]$ (sorted ascending, size $n_2 = 5$) trigger the **Worst Case** of Quick Sort when using the first element as pivot.
2. Time complexity for the worst case on size $n$ is $T(n) = c \cdot n^2$.
3. For input 1: $n_1 = 4 \implies t_1 \propto 4^2 = 16$ operations.
4. For input 2: $n_2 = 5 \implies t_2 \propto 5^2 = 25$ operations.
5. Since $n_1 < n_2$, $t_1 < t_2$.

**Answer:** **(C)** $t_1 < t_2$.

---

Problem 2: Comparison Counts (Sorted vs. Unsorted Input)

Let $P$ be a Quick Sort program that sorts numbers in ascending order using the first element as pivot. Let $t_1$ and $t_2$ be the number of key comparisons made by $P$ for the inputs $A_1 = [1, 2, 3, 4, 5]$ and $A_2 = [4, 1, 5, 3, 2]$ respectively. Which one of the following holds?

- (A) $t_1 = 5$
- (B) $t_1 < t_2$
- (C) $t_1 > t_2$
- (D) $t_1 = t_2$

Step-by-Step Solution:

1. **Input 1 (**$A_1 = [1, 2, 3, 4, 5]$**):** Already sorted in ascending order. Triggers the **Worst Case**. $$t_1 = \frac{n(n - 1)}{2} = \frac{5 \times 4}{2} = \mathbf{10 \text{ comparisons}}$$
2. **Input 2 (**$A_2 = [4, 1, 5, 3, 2]$**):** Unsorted array with a balanced distribution.
    - Pass 1: Pivot 4 splits into $[1, 3, 2]$ and $[5]$ (4 comparisons).
    - Pass 2: Pivot 1 splits $[3, 2]$ (2 comparisons).
    - Pass 3: Pivot 3 splits $[2]$ (1 comparison).
    - Total comparisons $t_2 = 4 + 2 + 1 = \mathbf{7 \text{ comparisons}}$.
3. Since $t_1 = 10$ and $t_2 = 7$, $t_1 > t_2$.

**Answer:** **(C)** $t_1 > t_2$.

---

Problem 3: Equal Comparison Count Identity

Quick Sort is run on two inputs of size $n$ to sort in ascending order using the first element as pivot:

- Input (i): $1, 2, 3, \dots, n$ (Sorted Ascending)
- Input (ii): $n, n-1, n-2, \dots, 1$ (Sorted Descending)

Let $C_1$ and $C_2$ be the number of comparisons made for inputs (i) and (ii) respectively. Which holds?

- (A) $C_1 > C_2$
- (B) $C_1 < C_2$
- (C) $C_1 = C_2$
- (D) Cannot be determined

Step-by-Step Solution:

1. Both ascending and descending sorted inputs cause the pivot to land at an extreme boundary (index 1 or index $n$) at every pass.
2. Both configurations execute the exact worst-case comparison count: $$C_1 = C_2 = \frac{n(n - 1)}{2}$$

**Answer:** **(C)** $C_1 = C_2$.

---

Problem 4: Randomized Quick Sort Worst Location Probability

An array of **25 distinct elements** is to be sorted using Quick Sort. Assume that the pivot element is chosen uniformly at random. What is the probability (expressed as a percentage) that the pivot element gets placed in the **worst possible location** in the first round of partitioning?

Step-by-Step Solution:

1. For an array of size $n = 25$, the worst possible locations for a pivot after partitioning are the **extreme boundaries**:
    - Index 1 (0 elements to the left, 24 to the right).
    - Index 25 (24 elements to the left, 0 to the right).
2. Out of 25 available indices, exactly **2 positions** represent the worst location.
3. Probability $P$: $$P = \frac{2}{25} = 0.08$$
4. Converting probability to percentage: $$\text{Percentage} = 0.08 \times 100\% = \mathbf{8\%}$$

**Answer:** **8%** (or probability **0.08**).

---

Problem 5: Fractional Selection Pivot Recurrence

In Quick Sort on an unsorted list of size $n$, suppose the $(n/4)$-th smallest element is selected as pivot each time. Write the time complexity recurrence relation and determine its tight asymptotic bound.

Step-by-Step Solution:

1. Selecting the $(n/4)$-th element as pivot splits the array into a left subproblem of size $n/4$ and a right subproblem of size $3n/4$.
2. The recurrence equation is: $$T(n) = T\left(\frac{n}{4}\right) + T\left(\frac{3n}{4}\right) + O(n)$$
3. Level-wise work in the recursion tree:
    - Level 0: $n$
    - Level 1: $\frac{n}{4} + \frac{3n}{4} = n$
    - Level 2: $n$
4. Depth of longest branch $k$: $n \cdot (3/4)^k = 1 \implies k = \log_{4/3} n = \Theta(\log n)$.
5. Total work over $k$ levels $= n \cdot \Theta(\log n) = \mathbf{O(n \log_2 n)}$.

**Answer:** Recurrence = $T(n) = T(n/4) + T(3n/4) + O(n)$, Time Complexity = $O(n \log_2 n)$.