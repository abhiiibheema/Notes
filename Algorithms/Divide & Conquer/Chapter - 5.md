ADVANCED MATRIX OPERATIONS, SELECTION, & INVERSION COUNTING

---

1. MATRIX MULTIPLICATION PARADIGMS

Matrix multiplication is a fundamental operation used in scientific computing, graphics, and machine learning. We evaluate three distinct methods for multiplying two $n \times n$ square matrices $A$ and $B$ to produce an $n \times n$ product matrix $C = A \times B$.

```
                        Matrix Multiplication Methods
                                      │
         ┌────────────────────────────┼────────────────────────────┐
         ▼                            ▼                            ▼
  Iterative / Naive          Simple Divide & Conquer            Strassen's D&C
 - 3 Nested Loops           - 8 Sub-matrix Multiplications    - 7 Sub-matrix Multiplications
 - Time: O(n³)              - Recurrence: T(n) = 8T(n/2)+O(n²) - Recurrence: T(n) = 7T(n/2)+O(n²)
 - Space: O(1)              - Time: O(n³)                     - Time: O(n^2.81)
```

---

Method 1: Iterative / Naive Matrix Multiplication

The standard definition computes each element $C[i][j]$ as the dot product of row $i$ of matrix $A$ and column $j$ of matrix $B$:

$$C[i][j] = \sum_{k=1}^n A[i][k] \times B[k][j]$$

```
Algo MatrixMultiply_Naive(A, B, C, n) {
    for (i = 1; i <= n; i++) {
        for (j = 1; j <= n; j++) {
            C[i][j] = 0;
            for (k = 1; k <= n; k++) {
                C[i][j] = C[i][j] + A[i][k] * B[k][j];
            }
        }
    }
}
```

Complexity Analysis:

- **Scalar Operations:** $n^3$ scalar multiplications and $n^2(n-1)$ scalar additions.
- **Time Complexity:** $\mathbf{O(n^3)}$.
- **Auxiliary Space Complexity:** $\mathbf{O(1)}$ (in-place accumulator).

---

Method 2: Simple Divide and Conquer Matrix Multiplication

Assume $n$ is a power of 2 ($n = 2^k$). We partition each $n \times n$ matrix into four $(n/2) \times (n/2)$ sub-matrices:

$$A = \begin{bmatrix} A_{11} & A_{12} \\ A_{21} & A_{22} \end{bmatrix}, \quad B = \begin{bmatrix} B_{11} & B_{12} \\ B_{21} & B_{22} \end{bmatrix}, \quad C = \begin{bmatrix} C_{11} & C_{12} \\ C_{21} & C_{22} \end{bmatrix}$$

The four sub-blocks of product matrix $C$ are computed as:

\[\begin{aligned} C_{11} &= A_{11} B_{11} + A_{12} B_{21} \ C_{12} &= A_{11} B_{12} + A_{12} B_{22} \ C_{21} &= A_{21} B_{11} + A_{22} B_{21} \ C_{22} &= A_{21} B_{12} + A_{22} B_{22} \end{aligned}\]

```
Algo MatrixMultiply_SimpleDnC(A, B, n) {
    if (n == 1) {
        return A * B; // Base Case: 1 multiplication
    } else {
        // Partition A and B into sub-blocks of size (n/2 x n/2)
        // Perform 8 recursive sub-matrix multiplications
        M1 = MatrixMultiply_SimpleDnC(A11, B11, n/2);
        M2 = MatrixMultiply_SimpleDnC(A12, B21, n/2);
        M3 = MatrixMultiply_SimpleDnC(A11, B12, n/2);
        M4 = MatrixMultiply_SimpleDnC(A12, B22, n/2);
        M5 = MatrixMultiply_SimpleDnC(A21, B11, n/2);
        M6 = MatrixMultiply_SimpleDnC(A22, B21, n/2);
        M7 = MatrixMultiply_SimpleDnC(A21, B12, n/2);
        M8 = MatrixMultiply_SimpleDnC(A22, B22, n/2);
        
        // Perform 4 sub-matrix additions in O(n^2) time
        C11 = M1 + M2;  C12 = M3 + M4;
        C21 = M5 + M6;  C22 = M7 + M8;
        
        return C;
    }
}
```

---

Recurrence Relation & Resolution

The Simple Divide and Conquer approach generates **8 sub-matrix multiplications** of size $n/2$ and performs **4 sub-matrix additions** taking $O(n^2)$ time:

$$T(n) = 8 T\left(\frac{n}{2}\right) + b \cdot n^2 \quad \text{with } T(1) = O(1)$$

Applying Master's Theorem ($a = 8, b = 2, f(n) = n^2 \implies k = 2$):

- Calculate $\log_b a = \log_2 8 = 3$.
- Compare $n^{\log_b a} = n^3$ with $f(n) = n^2$.
- Since $a > b^k$ ($8 > 2^2 = 4$), Case 1 applies:

$$T(n) = \Theta\left(n^{\log_2 8}\right) = \mathbf{\Theta(n^3)}$$

🚩 **KEY OBSERVATION:** Simple Divide and Conquer matrix multiplication requires $O(n^3)$ time, yielding **zero asymptotic improvement** over naive 3-nested-loop multiplication because the number of recursive calls remains $8$.

---

Method 3: Strassen's Matrix Multiplication Algorithm

Volker Strassen (1969) proved that the time complexity of matrix multiplication can be reduced if the number of recursive sub-matrix multiplications is reduced from **8 to 7**.

Strassen's 7 Intermediate Sub-Matrices ($P, Q, R, S, T, U, V$)

Using matrix additions and subtractions (spending $O(n^2)$ work), Strassen defines 7 intermediate products:

\[\begin{aligned} P &= (A_{11} + A_{22}) \times (B_{11} + B_{22}) \ Q &= (A_{21} + A_{22}) \times B_{11} \ R &= A_{11} \times (B_{12} - B_{22}) \ S &= A_{22} \times (B_{21} - B_{11}) \ T &= (A_{11} + A_{12}) \times B_{22} \ U &= (A_{21} - A_{11}) \times (B_{11} + B_{12}) \ V &= (A_{12} - A_{22}) \times (B_{21} + B_{22}) \end{aligned}\]

Combining Intermediate Matrices to Form $C$:

\[\begin{aligned} C_{11} &= P + S - T + V \ C_{12} &= R + T \ C_{21} &= Q + S \ C_{22} &= P + R - Q + U \end{aligned}\]

---

Recurrence Relation & Complexity Analysis

Strassen's algorithm generates **7 recursive sub-matrix multiplications** of size $n/2$ and performs **18 sub-matrix additions/subtractions** taking $O(n^2)$ work:

$$T(n) = 7 T\left(\frac{n}{2}\right) + b \cdot n^2 \quad \text{with } T(1) = O(1)$$

Applying Master's Theorem ($a = 7, b = 2, f(n) = n^2 \implies k = 2$):

- Calculate $\log_b a = \log_2 7 \approx 2.807$.
- Compare $n^{\log_b a} = n^{2.807}$ with $f(n) = n^2$.
- Since $a > b^k$ ($7 > 2^2 = 4$), Case 1 applies:

$$T(n) = \Theta\left(n^{\log_2 7}\right) \approx \mathbf{O(n^{2.81})}$$

Auxiliary Space Complexity of Strassen's Algorithm:

- Allocation of 7 intermediate sub-matrices $P, Q, R, S, T, U, V$ of size $(n/2) \times (n/2)$ at each level requires $O(n^2)$ space.
- Recursion stack memory depth is $O(\log n)$.
- **Total Auxiliary Space Complexity:** $\mathbf{O(n^2)}$.

---

2. THE SELECTION PROBLEM (QUICK-SELECT ALGORITHM)

Problem Definition

Given an unsorted array $A$ of $n$ distinct elements and an integer $k$ ($1 \le k \le n$), find the $k$**-th smallest element** in the array without fully sorting the entire array.

```
Input Array A: [ 12,   3,   5,   7,  19,   1,   4,  26 ]
Target:        k = 3 (Find 3rd smallest element)
Sorted View:   [  1,   3,   4,   5,   7,  12,  19,  26 ]  ──► Output = 4
```

---

Algorithmic Approaches Comparison

1. **Full Sorting Approach:** Sort the array using Merge Sort in $O(n \log n)$ time and return $A[k]$.
    - **Time Complexity:** $O(n \log n)$.
2. **Min-Heap Approach:** Build a Min-Heap of all $n$ elements in $O(n)$ time, then perform `Extract-Min` $k$ times.
    - **Time Complexity:** $O(n + k \log n)$.
3. **Divide and Conquer Approach (Quick-Select):** Uses Quick Sort's partitioning logic, but recurses on **only one partition branch**.

---

Quick-Select Algorithm Mechanics

```
Algo QuickSelect(A, low, high, k) {
    if (low == high) {
        return A[low]; // Base case: single element
    }
    
    // Partition array around pivot; p is final pivot index
    p = Partition(A, low, high);
    
    // Calculate length of left partition including pivot
    length = p - low + 1;
    
    if (k == length) {
        return A[p]; // Pivot IS the k-th smallest element!
    } 
    else if (k < length) {
        // Recurse ONLY on Left Subproblem
        return QuickSelect(A, low, p - 1, k);
    } 
    else {
        // Recurse ONLY on Right Subproblem (adjust k)
        return QuickSelect(A, p + 1, high, k - length);
    }
}
```

---

Complexity Analysis of Quick-Select

- **Best & Average Case:** The pivot divides the search space roughly in half at each step. $$T(n) = T(n/2) + O(n)$$ Expanding via back-substitution: $$T(n) = n + \frac{n}{2} + \frac{n}{4} + \frac{n}{8} + \dots + 1 = n \left(1 + \frac{1}{2} + \frac{1}{4} + \dots\right) \approx 2n$$
    - **Average-Case Time Complexity:** $\mathbf{O(n)}$ (Linear Time).
- **Worst Case:** The pivot always lands at the extreme boundary ($0 : n-1$). $$T(n) = T(n-1) + O(n) = n + (n-1) + (n-2) + \dots + 1 = \frac{n(n+1)}{2}$$
    - **Worst-Case Time Complexity:** $\mathbf{O(n^2)}$.
- **Auxiliary Space Complexity:** $\mathbf{O(1)}$ in-place swapping beyond $O(\log n)$ average / $O(n)$ worst-case stack depth.

---

3. COUNTING INVERSIONS IN AN ARRAY

Problem Definition

An **Inversion Pair** in an array $A$ is a pair of indices $(i, j)$ such that $i < j$ and $A[i] > A[j]$. The inversion count measures how far an array is from being completely sorted.

```
Array A: [ 2,  4,  1,  3,  5 ]
Inversion Pairs:
 - Index (1, 3): A[1]=2 > A[2]=1
 - Index (2, 3): A[3]=4 > A[2]=1
 - Index (2, 4): A[3]=4 > A[4]=3
Total Inversion Count = 3
```

---

Divide and Conquer Approach (Modified Merge Sort)

We divide the array into a left half $A[\text{low} \dots \text{mid}]$ and a right half $A[\text{mid}+1 \dots \text{high}]$. Inversions fall into three mutually exclusive categories:

1. **Left Inversions:** Pairs $(i, j)$ where both $i, j \in [\text{low} \dots \text{mid}]$.
2. **Right Inversions:** Pairs $(i, j)$ where both $i, j \in [\text{mid}+1 \dots \text{high}]$.
3. **Cross Inversions:** Pairs $(i, j)$ where $i \in [\text{low} \dots \text{mid}]$ and $j \in [\text{mid}+1 \dots \text{high}]$.

$$\text{Total Inversions} = \text{Left Inversions} + \text{Right Inversions} + \text{Cross Inversions}$$

```
Algo CountInversions(A, low, high) {
    count = 0;
    if (low < high) {
        mid = (low + high) / 2;
        
        count += CountInversions(A, low, mid);      // Left inversions
        count += CountInversions(A, mid + 1, high); // Right inversions
        count += MergeAndCount(A, low, mid, high);  // Cross inversions
    }
    return count;
}
```

---

Counting Cross Inversions during Merging

During the merging phase, both left and right sub-arrays are **already sorted**. When comparing $A[i]$ (left half) and $A[j]$ (right half):

- If $A[i] > A[j]$, then because the left half is sorted, **all remaining elements from** $A[i]$ **to** $A[\text{mid}]$ **are strictly greater than** $A[j]$.
- **Cross Inversions Added:** $(\text{mid} - i + 1)$.

```
Left Sub-array (Sorted):  [ 3,  7,  9 ]   (mid = 3, i = 2 -> A[i] = 7)
Right Sub-array (Sorted): [ 2,  5,  8 ]   (j = 2 -> A[j] = 5)

Comparison: A[i] = 7 > A[j] = 5
Since 7 > 5, all remaining elements in Left are > 5.
Inversions added = (mid - i + 1) = (3 - 2 + 1) = 2  (Pairs: (7,5) and (9,5))
```

Complexity Analysis:

- **Recurrence Relation:** $T(n) = 2 T(n/2) + O(n)$.
- **Time Complexity:** $\mathbf{O(n \log_2 n)}$.
- **Auxiliary Space Complexity:** $\mathbf{O(n)}$ (auxiliary merging array).

---

4. MASTER COMPREHENSIVE MATRIX

|Problem / Algorithm|Naive Approach Time|D&C Approach Time|D&C Recurrence Relation|Auxiliary Space|
|---|---|---|---|---|
|**Matrix Multiplication**|$O(n^3)$|$\mathbf{O(n^{2.81})}$ (Strassen)|$T(n) = 7 T(n/2) + O(n^2)$|$O(n^2)$|
|**Selection (**$k$**-th Smallest)**|$O(n \log n)$ (Sorting)|$\mathbf{O(n)}$ Avg (Quick-Select)|$T(n) = T(n/2) + O(n)$|$O(1)$ Inplace|
|**Counting Inversions**|$O(n^2)$|$\mathbf{O(n \log n)}$ (Merge-Count)|$T(n) = 2 T(n/2) + O(n)$|$O(n)$|

---

5. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Strassen's Matrix Multiplication Speedup Ratio (NAT)

Calculate the asymptotic speedup ratio of Standard Matrix Multiplication ($O(n^3)$) to Strassen's Matrix Multiplication ($O(n^{\log_2 7})$).

Step-by-Step Solution:

1. Standard Matrix Multiplication Time: $$T_1(n) = \Theta(n^3)$$
2. Strassen's Matrix Multiplication Time: $$T_2(n) = \Theta(n^{\log_2 7}) \approx \Theta(n^{2.807})$$
3. Compute asymptotic ratio: $$\text{Ratio} = \frac{\Theta(n^3)}{\Theta(n^{2.807})} = \mathbf{\Theta(n^{0.193})}$$

**Answer:** $\Theta(n^{0.193})$ **speedup factor**.

---

Problem 2: Master's Theorem Verification for D&C Recurrence (MCQ)

The running time of a Divide and Conquer algorithm is defined by the recurrence: $$T(n) = 9 T\left(\frac{n}{3}\right) + n^{2.5}$$ Which option represents the tight asymptotic time complexity?

- (A) $\Theta(n^2)$
- (B) $\Theta(n \sqrt{n})$
- (C) $\Theta(n \log n)$
- (D) $\Theta(n^2 \sqrt{n})$

Step-by-Step Solution:

1. Identify Master's Theorem parameters: $$a = 9, \quad b = 3, \quad f(n) = n^{2.5} = n^2 \sqrt{n}$$
2. Compute $n^{\log_b a}$: $$n^{\log_3 9} = n^2$$
3. Compare $f(n)$ with $n^{\log_b a}$: $$f(n) = n^{2.5} \quad \text{vs} \quad n^{\log_b a} = n^2$$
4. Since $f(n) = \Omega(n^{2 + \epsilon})$ for $\epsilon = 0.5 > 0$, $f(n)$ grows strictly faster than $n^2$ (Case 3 of Master's Theorem applies).
5. Therefore, $T(n) = \Theta(f(n)) = \Theta(n^{2.5}) = \mathbf{\Theta(n^2 \sqrt{n})}$.

**Answer:** **(D)** $\Theta(n^2 \sqrt{n})$.

---

Problem 3: Recurrence Relation Solving (MCQ)

The running time of an algorithm is represented by the recurrence: $$T(n) = \begin{cases} n & \text{for } n \le 3 \\ T\left(\frac{n}{3}\right) + c \cdot n & \text{otherwise} \end{cases}$$ Which option represents the time complexity of the algorithm?

- (A) $\Theta(n)$
- (B) $\Theta(n \log n)$
- (C) $\Theta(n^2)$
- (D) $\Theta(n^2 \log n)$

Step-by-Step Solution:

1. Identify Master's Theorem parameters: $a = 1, b = 3, f(n) = c \cdot n$.
2. Compute $n^{\log_b a}$: $$n^{\log_3 1} = n^0 = 1$$
3. Compare $f(n) = c \cdot n$ with $n^{\log_b a} = 1$.
4. Since $f(n) = \Omega(n^{0 + \epsilon})$ for $\epsilon = 1 > 0$, Case 3 applies: $$T(n) = \Theta(f(n)) = \mathbf{\Theta(n)}$$

**Answer:** **(A)** $\Theta(n)$.

---

Problem 4: Maximum Inversions in Reverse Sorted Array (NAT)

What is the maximum number of inversion pairs present in an array of size $n = 10$ that is sorted in strictly reverse order?

Step-by-Step Solution:

1. In a reverse sorted array $A =$, every pair $(i, j)$ with $i < j$ satisfies $A[i] > A[j]$.
2. Total inversion pairs equals the total number of un-ordered pairs $\binom{n}{2}$: $$\text{Total Inversions} = \frac{n(n - 1)}{2}$$
3. Substitute $n = 10$: $$\text{Total Inversions} = \frac{10 \times 9}{2} = \mathbf{45 \text{ inversions}}$$

**Answer:** **45 inversions**.

---

Problem 5: Selection Problem Worst-Case Trigger (MCQ)

In Quick-Select for finding the $k$-th smallest element, which input configuration triggers the worst-case running time of $O(n^2)$ when using the first element as pivot?

- (A) Randomly shuffled array
- (B) Array already sorted in ascending order
- (C) Array with all elements equal
- (D) Both (B) and (C)

Step-by-Step Solution:

When the array is already sorted (ascending or descending) or contains all identical elements, picking the first element as pivot causes Partitioning to produce an extreme split ($0 : n-1$) at every step. This leads to the worst-case recurrence $T(n) = T(n-1) + O(n) \implies O(n^2)$.

**Answer:** **(D) Both (B) and (C)**.

---

Problem 6: Strassen's Multiplication Recurrence (MCQ)

Strassen's algorithm for matrix multiplication reduces the number of sub-matrix multiplications to $7$. Its recurrence relation is given by:

- (A) $T(n) = 8 T(n/2) + O(n^2)$
- (B) $T(n) = 7 T(n/2) + O(n^2)$
- (C) $T(n) = 7 T(n/2) + O(n)$
- (D) $T(n) = 4 T(n/2) + O(n^2)$

Step-by-Step Solution:

Strassen's method computes 7 intermediate matrix products of size $(n/2) \times (n/2)$ using recursive calls, plus 18 matrix additions/subtractions taking $O(n^2)$ work. Thus, the recurrence is $T(n) = 7 T(n/2) + O(n^2)$.

**Answer:** **(B)**.