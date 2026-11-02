2: BINARY SEARCH & PRINCIPLE OF TWO-WAY MERGING

---

1. SEARCHING PARADIGMS: LINEAR SEARCH VS. BINARY SEARCH

Searching is the process of locating a specific target element (key $x$) within a collection of data elements. Depending on the structural organization of the input array, we employ different searching paradigms.

```
                           Searching Paradigms
                                    │
         ┌──────────────────────────┴──────────────────────────┐
         ▼                                                     ▼
   Linear Search                                         Binary Search
(Sequential Scanning)                                (Divide & Conquer)
 - Works on unsorted arrays                           - MANDATORY: Input MUST be sorted
 - Worst Case: O(n)                                   - Worst Case: O(log₂ n)
 - Explores every index                               - Explores only one half (Left OR Right)
```

---

A. Linear Search Mechanics

Linear Search scans array elements sequentially from index $1$ to $n$ until either the key $x$ is found or the end of the array is reached.

```
Algo LinearSearch(A, n, x) {
    for (i = 1; i <= n; i++) {
        if (A[i] == x) {
            return i; // Target key found
        }
    }
    return -1; // Target key not present
}
```

Time Complexity Analysis of Linear Search:

- **Best Case (**$\Omega(1)$**):** Target key $x$ is located at the very first position ($A$1$ == x$). Requires **1 comparison**.
- **Worst Case (**$O(n)$**):** Target key $x$ is located at the last position ($A[n] == x$) or is completely absent. Requires $n$ **comparisons**.

🚩 **FUNDAMENTAL LIMITATION OF LINEAR SEARCH:** Linear Search **does NOT gain any performance advantage when the input array is sorted**. Even if an array of $n$ elements is pre-sorted in ascending order, Linear Search still takes $O(n)$ time in the worst case to determine if an element is absent or located near the end.

---

B. Binary Search Paradigm

To exploit the structure of an **already sorted array**, we switch from sequential scanning to a **Divide and Conquer** paradigm: **Binary Search**.

Mandatory Prerequisite

The input array $A$ **MUST be sorted in non-decreasing order** (e.g., $A$1$ \le A$2$ \le \dots \le A[n]$). If the input is unsorted, Binary Search cannot be applied directly.

---

2. BINARY SEARCH MECHANICS & RECURSIVE DERIVATION

Algorithmic Strategy

Binary Search calculates the middle index $m = \lfloor \frac{\text{low} + \text{high}}{2} \rfloor$ of the active search range $[\text{low} \dots \text{high}]$ and compares the target key $x$ against the middle element $A[m]$.

```
                      Search Space [low ... high]
                                   │
                                   ▼
                       Calculate mid = ⌊(low+high)/2⌋
                                   │
         ┌─────────────────────────┼─────────────────────────┐
         ▼                         ▼                         ▼
   Case 1: Key == A[m]       Case 2: Key > A[m]        Case 3: Key < A[m]
  (Small problem solved)    (Search RIGHT subproblem)  (Search LEFT subproblem)
      Return mid              Explore [mid+1 ... high]  Explore [low ... mid-1]
```

💡 **THE SINGLE-BRANCH RULE:** Unlike Merge Sort or Quick Sort which recurse on both subproblems, Binary Search recurses on **either the left subproblem OR the right subproblem, BUT NEVER BOTH**.

---

Implementation 1: Recursive Binary Search

```
Algo BinarySearch_Recursive(A, low, high, x) {
    if (low == high) { // Base Case: Single element (Small Problem)
        if (x == A[low]) return low;
        else return 0;
    } 
    else { // Recursive Step: Reduce search space
        mid = (low + high) / 2;
        
        if (x == A[mid]) {
            return mid; // Case 1: Key found at mid
        } 
        else if (x > A[mid]) {
            return BinarySearch_Recursive(A, mid + 1, high, x); // Case 2: Explore Right
        } 
        else {
            return BinarySearch_Recursive(A, low, mid - 1, x); // Case 3: Explore Left
        }
    }
}
```

---

Implementation 2: Iterative (Non-Recursive) Binary Search

```
Algo BinarySearch_Iterative(A, n, x) {
    low = 1;
    high = n;
    
    while (low <= high) {
        mid = (low + high) / 2;
        
        if (A[mid] == x) {
            return mid; // Target key found
        } 
        else if (A[mid] < x) {
            low = mid + 1; // Narrow search space to Right half
        } 
        else {
            high = mid - 1; // Narrow search space to Left half
        }
    }
    return -1; // Target key not present
}
```

---

Step-by-Step Back-Substitution Derivation of Time Complexity

Let $T(n)$ represent the time complexity of Binary Search on a search space of size $n$:

$$T(n) = \begin{cases} b & \text{for } n = 1 \quad (\text{Base Case}) \\ T\left(\frac{n}{2}\right) + C & \text{for } n > 1 \quad (\text{Recursive Step}) \end{cases}$$

Here, $C$ represents the constant time taken to calculate $\text{mid}$ and perform comparisons.

Back-Substitution Expansion:

1. **Base Equation:** $$T(n) = T(n/2) + C \quad \text{--- (Equation 1)}$$
2. **First Substitution:** Substitute $n \to n/2$: $$T(n/2) = T(n/2^2) + C$$ Substitute back into Eq 1: $$T(n) = [T(n/2^2) + C] + C = T(n/2^2) + 2C$$
3. **Second Substitution:** Substitute $n \to n/2^2$: $$T(n/2^2) = T(n/2^3) + C$$ Substitute back: $$T(n) = T(n/2^3) + 3C$$
4. **General Expansion at Step** $k$**:** $$T(n) = T\left(\frac{n}{2^k}\right) + k \cdot C$$
5. **Apply Base Condition Boundary:** Set the subproblem size equal to 1: $$\frac{n}{2^k} = 1 \implies 2^k = n \implies k = \log_2 n$$
6. **Substitute** $k = \log_2 n$ **back into the General Expansion:** $$T(n) = T(1) + (\log_2 n) \cdot C = b + C \log_2 n$$

$$\mathbf{T(n) = O(\log_2 n)}$$

---

Space Complexity & Recursion Call Stack Depth

- **Iterative Binary Search:** Operates using a simple `while` loop with scalar variables (`low`, `high`, `mid`). It allocates no extra memory structures and makes no recursive calls. $$\text{Auxiliary Space Complexity} = \mathbf{O(1)}$$
- **Recursive Binary Search:** Each recursive call pushes an activation record onto the system call stack. Since the search space halves at every step, the maximum simultaneous depth of active frames on the stack is $k = \log_2 n$. $$\text{Auxiliary Space Complexity} = \mathbf{O(\log_2 n)}$$

---

Comparison Matrix: Linear Search vs. Binary Search on Sorted Array

|Feature / Metric|Linear Search|Binary Search (Iterative)|Binary Search (Recursive)|
|---|---|---|---|
|**Prerequisite Input Structure**|Any Array (Unsorted or Sorted)|**MUST be Pre-Sorted**|**MUST be Pre-Sorted**|
|**Best-Case Time Complexity**|$\Omega(1)$|$\Omega(1)$|$\Omega(1)$|
|**Worst-Case Time Complexity**|$O(n)$|$\mathbf{O(\log_2 n)}$|$\mathbf{O(\log_2 n)}$|
|**Average-Case Time Complexity**|$O(n)$|$\mathbf{O(\log_2 n)}$|$\mathbf{O(\log_2 n)}$|
|**Auxiliary Space Complexity**|$O(1)$|$\mathbf{O(1)}$|$\mathbf{O(\log_2 n)}$ (Stack Depth)|
|**D&C Step Requirement**|Sequential (No Divide)|Divide Only (No Combine)|Divide Only (No Combine)|

---

3. PRINCIPLE OF TWO-WAY MERGING (CONQUER STEP)

Problem Definition

Given two pre-sorted lists/arrays $L_1$ of size $m$ and $L_2$ of size $n$ (without loss of generality, assume $m \le n$), merge them into a single unified sorted array $B$ of size $m + n$.

```
Input List L1 (m = 3): [ 2,   4,   5 ]
Input List L2 (n = 6): [ 10, 12, 14, 17, 20, 25 ]

Merged Output Array B (m + n = 9):
[ 2,  4,  5,  10,  12,  14,  17,  20,  25 ]
```

---

Algorithmic Mechanism of Two-Way Merging

1. Maintain three index pointers: `i` for $L_1$, `j` for $L_2$, and `k` for the target output array $B$.
2. Compare $L_1[i]$ with $L_2[j]$:
    - Copy the smaller element into $B[k]$.
    - Advance the pointer of the list from which the smaller element was taken, along with pointer `k`.
3. When one of the lists is completely exhausted, directly **copy the remaining uncompared elements** of the other list into $B$ without performing any additional element-to-element comparisons.

```
Algo TwoWayMerge(L1, m, L2, n, B) {
    i = 1; j = 1; k = 1;
    
    // Compare elements while both lists have unexamined items
    while (i <= m && j <= n) {
        if (L1[i] <= L2[j]) { // 1 Comparison
            B[k] = L1[i];
            i++;
        } else {
            B[k] = L2[j];
            j++;
        }
        k++;
    }
    
    // Copy remaining elements of L1 (if any) without comparison
    while (i <= m) {
        B[k] = L1[i];
        i++; k++;
    }
    
    // Copy remaining elements of L2 (if any) without comparison
    while (j <= n) {
        B[k] = L2[j];
        j++; k++;
    }
}
```

---

Comparison Analysis during Two-Way Merging

Let $m$ and $n$ represent the sizes of the two pre-sorted input lists (where $m \le n$).

1. Minimum Number of Comparisons (Best Case)

- **Configuration:** All elements in the smaller list $L_1$ are strictly smaller than the first element of the larger list $L_2$ (i.e., $L_1[m] < L_2$1).
- **Execution Trace:**
    - $L_1$1 is compared with $L_2$1 $\implies L_1$1 copied.
    - $L_1$2 is compared with $L_2$1 $\implies L_1$2 copied.
    - ...
    - $L_1[m]$ is compared with $L_2$1 $\implies L_1[m]$ copied.
- At this point, list $L_1$ is completely exhausted after **exactly** $m$ **comparisons**. All $n$ elements of list $L_2$ are then appended to $B$ via direct array copying without further comparisons.
- **Minimum Comparisons Formula:** $$C_{\text{min}} = \min(m, n) = m$$

---

2. Maximum Number of Comparisons (Worst Case)

- **Configuration:** Elements strictly alternate between $L_1$ and $L_2$ such that $L_1[m-1] < L_2$1 and the last element of $L_1$ ($L_1[m]$) is strictly greater than all elements of $L_2$ ($L_1[m] > L_2[n]$).
- **Execution Trace:**
    - Comparisons occur continuously until exactly **1 element** remains in one of the lists.
    - To place $(m + n - 1)$ elements into the output array $B$, exactly $(m + n - 1)$ comparisons are performed.
    - The last remaining element is copied into $B$ without comparison.
- **Maximum Comparisons Formula:** $$C_{\text{max}} = m + n - 1$$

---

General Comparison Inequality of Two-Way Merging

For merging two pre-sorted lists of sizes $m$ and $n$:

$$\mathbf{\min(m, n) \le \text{Element Comparisons} \le (m + n - 1)}$$

Time and Space Complexity of Merging

- **Time Complexity:** $\mathbf{O(m + n)}$ in all cases (since elements are copied $m+n$ times).
- **Auxiliary Space Complexity:** $\mathbf{O(m + n)}$ (to store the merged auxiliary array $B$).

---

4. COMPLETE SOLVED PROBLEM SET

---

Problem 1: Recursive Search Range Trace (NAT)

Given the pre-sorted array $A =$3more_horiz indexed from 1 to 8, trace the sequence of `(low, high)` ranges evaluated during a search for key $x = 15$. Determine the total number of comparisons.

Step-by-Step Solution:

1. **Initial Search Space:** `low = 1`, `high = 8`.
    - Calculate $\text{mid} = \lfloor \frac{1 + 8}{2} \rfloor = 4$.
    - $A$9$ = 50$. Compare key $15$ with $50$: $15 < 50 \implies$ Explore Left half.
2. **Second Range:** `low = 1`, `high = mid - 1 = 3`.
    - Calculate $\text{mid} = \lfloor \frac{1 + 3}{2} \rfloor = 2$.
    - $A$2$ = 25$. Compare key $15$ with $25$: $15 < 25 \implies$ Explore Left half.
3. **Third Range:** `low = 1`, `high = mid - 1 = 1`.
    - Calculate $\text{mid} = \lfloor \frac{1 + 1}{2} \rfloor = 1$.
    - $A$1$ = 15$. Compare key $15$ with $15$: $15 == 15 \implies$ Key found!
4. **Sequence of Evaluated Ranges:** `(1, 8) -> (1, 3) -> (1, 1)`.

**Answer:** Range Sequence = **(1, 8), (1, 3), (1, 1)**, Total Key Comparisons = **3**.

---

Problem 2: Min-Max Comparisons in Two-Way Merging (NAT)

Two pre-sorted lists $L_1$ of size $m = 12$ and $L_2$ of size $n = 20$ are merged into a single sorted list using two-way merging. Calculate:

1. The minimum possible number of key comparisons.
2. The maximum possible number of key comparisons.

Step-by-Step Solution:

1. **Minimum Comparisons:** $$C_{\text{min}} = \min(m, n) = \min(12, 20) = \mathbf{12 \text{ comparisons}}$$
2. **Maximum Comparisons:** $$C_{\text{max}} = m + n - 1 = 12 + 20 - 1 = \mathbf{31 \text{ comparisons}}$$

**Answer:** Minimum = **12**, Maximum = **31**.

---

Problem 3: Recursive Call Stack Allocation (NAT)

A recursive Binary Search is executed on a pre-sorted array of size $n = 1024$. If each stack frame consumes 48 bytes of memory, calculate the maximum stack memory in bytes allocated at the deepest point of recursion.

Step-by-Step Solution:

1. Calculate the maximum recursion stack depth: $$k = \log_2 n = \log_2(1024) = 10 \text{ frames}$$
2. Multiply stack depth by frame size: $$\text{Total Stack Memory} = 10 \times 48 \text{ bytes} = \mathbf{480 \text{ bytes}}$$

**Answer:** **480 bytes**.

---

Problem 4: Multiple Choice Question (MCQ)

Which option correctly describes the time complexity required to search for an element in an **already sorted array** of size $n$ using Linear Search vs. Binary Search in the worst case?

- (A) Linear Search: $O(\log_2 n)$, Binary Search: $O(n)$
- (B) Linear Search: $O(n)$, Binary Search: $O(n)$
- (C) Linear Search: $O(n)$, Binary Search: $O(\log_2 n)$
- (D) Linear Search: $O(1)$, Binary Search: $O(\log_2 n)$

Step-by-Step Solution:

Linear Search cannot take advantage of sorted array order and still checks all $n$ elements in the worst case ($O(n)$). Binary Search divides the search space in half at each step, taking $O(\log_2 n)$ time.

**Answer:** **(C)**.

---

Problem 5: Equal Size Array Merging (MCQ)

For merging two sorted lists of equal size $m$ into a single sorted list of size $2m$, the number of comparisons required ranges between:

- (A) $m$ and $2m$
- (B) $m$ and $2m - 1$
- (C) $2m$ and $2m - 1$
- (D) $1$ and $m$

Step-by-Step Solution:

Setting $n = m$ in the general merging bounds:

- Minimum Comparisons = $\min(m, m) = m$.
- Maximum Comparisons = $m + m - 1 = 2m - 1$.