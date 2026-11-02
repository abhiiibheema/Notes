SPACE COMPLEXITY & RECURSION STACK DEPTH

---

1. CORE CONCEPTS OF SPACE COMPLEXITY

While time complexity measures CPU clock cycles required by an algorithm, **Space Complexity** measures the total memory footprint (RAM) consumed by an algorithm during its execution.

```
                            Total Space Complexity S(P)
                                         │
                   ┌─────────────────────┴─────────────────────┐
                   ▼                                           ▼
              Input Space                               Auxiliary Space
     (Memory required to store input)         (Extra RAM needed during execution)
      - Fixed by problem definition            - Used for variables & stack frames
      - Example: Array A of size n -> O(n)     - Primary metric for algorithm design
```

Breakdown of Space Complexity

The total memory required by a program $P$ running on an input of size $n$ is divided into two distinct components:

$$\text{Total Space } S(P) = \text{Input Space} + \text{Auxiliary Space}$$

1. **Input Space:** The fixed memory required to store the raw input data supplied to the algorithm (e.g., an array of $n$ integers requires $O(n)$ space, a matrix of size $n \times n$ requires $O(n^2)$ space).
2. **Auxiliary Space (Work Space):** The extra or temporary memory used by the algorithm beyond the input space. This includes:
    - Temporary scalar variables (e.g., loop counters, temporary min/max storage).
    - Dynamically allocated data structures (e.g., auxiliary merging arrays).
    - The system **Recursion Call Stack** (for recursive algorithms).

💡 **KEY PRINCIPLE:** In algorithm analysis, efficiency is judged primarily by **Auxiliary Space**. Since the input space is mandated by the problem specification itself, the quality of an algorithm is determined by how little additional memory (auxiliary space) it consumes to solve the problem.

---

2. ACTIVATION RECORDS & THE RECURSION CALL STACK

When an algorithm executes iteratively, local scalar variables require a constant amount of memory $O(1)$. However, when an algorithm executes **recursively**, every function call creates a memory block called an **Activation Record** (or Stack Frame) on the system call stack.

```
                      System Call Stack Memory
                                 │
                 ┌───────────────┴───────────────┐
                 ▼                               ▼
       Activation Record Contents       Stack Depth Mechanics
      - Function parameters            - Each active call = 1 frame
      - Local variables                - Frame size = O(1) bytes
      - Return memory address          - Max stack height = Aux Space
      - Saved registers
```

Anatomy of an Activation Record

Every time a function is invoked, an activation record containing the following is pushed onto the stack:

1. **Function Parameters:** Arguments passed into the call (e.g., value of $n$, array pointers).
2. **Local Variables:** Variables declared within the function body.
3. **Return Address:** Memory address to resume execution after the current call completes.
4. **Saved Frame Pointers & Registers:** Internal state required to restore the caller's context.

Since the number of local variables and parameters inside a single function block is fixed, **each individual activation record consumes a constant amount of memory** $O(1)$.

---

Stack Depth vs. Total Function Calls

A crucial distinction in space complexity analysis is the difference between the **Total Number of Function Calls** and the **Maximum Recursion Stack Depth**:

- **Total Function Calls:** Influences the **Time Complexity** $T(n)$ of the algorithm.
- **Maximum Recursion Stack Depth:** Influences the **Auxiliary Space Complexity** $S(n)$ of the algorithm.

```
                     Sequential Stack Reuse Principle
                                     │
    Call A(n) pushed ──► Call A(n-1) pushed ──► Call A(n-1) finishes & POPPED!
                                                         │
                                                         ▼
                                               Call B(n-1) pushed into 
                                               SAME stack memory space
```

🚩 **CRITICAL EXAM PRINCIPLE:** Stack frames are **pushed** when a function is called and **popped (freed)** as soon as that function returns. When a program makes multiple sequential recursive calls, the stack frame for the first call is popped before the second call is pushed. Therefore, the auxiliary space used by recursion is equal to the **maximum number of simultaneously active stack frames** (height of the tree), NOT the sum of all nodes in the recursion tree.

---

3. STACK DEPTH ANALYSIS ACROSS RECURSIVE PATTERNS

The auxiliary space of a recursive algorithm depends directly on the depth of its call tree. Below are the canonical recursive patterns and their exact stack depth derivations.

---

Pattern A: Iterative Execution (No Recursion)

```
int IterativeSum(int A[], int n) {
    int sum = 0;                  // Local scalar -> O(1)
    for (int i = 0; i < n; i++) { // Local scalar -> O(1)
        sum += A[i];
    }
    return sum;
}
```

- **Active Stack Frames:** Only 1 main frame.
- **Input Space:** $O(n)$ (for array `A`).
- **Auxiliary Space:** $\mathbf{O(1)}$ (for scalar variables `sum` and `i`).

---

Pattern B: Linear Recursion (Linear Decrement)

```
int RecursiveSum(int A[], int n) {
    if (n <= 0) return 0;
    else return A[n - 1] + RecursiveSum(A, n - 1);
}
```

- **Call Sequence:** `RecursiveSum(n)` $\to$ `RecursiveSum(n-1)` $\to$ `RecursiveSum(n-2)` $\dots \to$ `RecursiveSum(0)`.
- **Maximum Active Stack Frames:** At the base case `n=0`, there are $n + 1$ activation records active on the call stack simultaneously.
- **Auxiliary Space:** $(n + 1) \times O(1) = \mathbf{O(n)}$.

```
Stack Growth for Linear Recursion:
┌───────────────────────────┐
│ RecursiveSum(0)  [Base]   │  ◄── Maximum Stack Depth = n + 1
├───────────────────────────┤
│ RecursiveSum(1)           │
├───────────────────────────┤
│ ...                       │
├───────────────────────────┤
│ RecursiveSum(n - 1)       │
├───────────────────────────┤
│ RecursiveSum(n)  [Root]   │
└───────────────────────────┘
```

---

Pattern C: Halving Recursion (Binary Search Style)

```
int BinarySearchRec(int A[], int low, int high, int key) {
    if (low > high) return -1;
    int mid = low + (high - low) / 2;
    if (A[mid] == key) return mid;
    else if (A[mid] > key) return BinarySearchRec(A, low, mid - 1, key);
    else return BinarySearchRec(A, mid + 1, high, key);
}
```

- **Call Sequence:** Input size reduces as $n \to \frac{n}{2} \to \frac{n}{4} \to \frac{n}{8} \dots \to 1$.
- **Maximum Active Stack Frames:** The depth of recursion satisfies $\frac{n}{2^k} = 1 \implies 2^k = n \implies k = \log_2 n$.
- **Auxiliary Space:** $k \times O(1) = \mathbf{O(\log n)}$.

---

Pattern D: Square Root Decrement Recursion

```
void SqrtRec(int n) {
    if (n <= 2) return;
    else SqrtRec(sqrt(n));
}
```

- **Call Sequence:** Input size reduces as $n \to n^{1/2} \to n^{1/4} \to n^{1/8} \dots \to 2$.
- **Maximum Active Stack Frames:** The depth of recursion satisfies $n^{1/2^k} = 2 \implies 2^k = \log_2 n \implies k = \log_2(\log_2 n)$.
- **Auxiliary Space:** $\mathbf{O(\log \log n)}$.

---

Pattern E: Tree Branching Recursion (Merge Sort Style)

```
void MergeSort(int A[], int low, int high) {
    if (low < high) {
        int mid = (low + high) / 2;
        MergeSort(A, low, mid);       // Left branch call
        MergeSort(A, mid + 1, high);   // Right branch call
        Merge(A, low, mid, high);     // Auxiliary merging step
    }
}
```

- **Call Execution:**
    1. `MergeSort` calls the left branch repeatedly until it reaches a single element at depth $\log_2 n$.
    2. The stack frame for the left leaf pops off when it returns.
    3. The right branch is then called, reusing the freed stack depth.
- **Maximum Active Stack Frames:** Equal to the maximum height of the tree $= \log_2 n$.
- **Auxiliary Memory Breakdown:**
    - Recursion Call Stack Space $= O(\log n)$.
    - Auxiliary Array Space for `Merge` step $= O(n)$.
- **Total Auxiliary Space:** $O(n) + O(\log n) = \mathbf{O(n)}$.

---

4. MASTER SUMMARY COMPARISON TABLE

|Algorithm / Pattern|Subproblem Progression|Total Function Calls (Time Factor)|Max Active Stack Depth (Space Factor)|Auxiliary Space Complexity|
|---|---|---|---|---|
|**Iterative Search / Loop**|No recursion|$0$|$0$|$\mathbf{O(1)}$|
|**Linear Search (Recursive)**|$n \to n - 1$|$O(n)$|$n$|$\mathbf{O(n)}$|
|**Binary Search (Recursive)**|$n \to n / 2$|$O(\log n)$|$\log_2 n$|$\mathbf{O(\log n)}$|
|**Square Root Reduction**|$n \to \sqrt{n}$|$O(\log \log n)$|$\log_2(\log_2 n)$|$\mathbf{O(\log \log n)}$|
|**Fibonacci (Naive Recursive)**|$n \to (n-1), (n-2)$|$O(2^n)$|$n$|$\mathbf{O(n)}$|
|**Merge Sort**|$n \to n / 2$ (2 calls)|$O(n \log n)$|$\log_2 n$ stack + $O(n)$ array|$\mathbf{O(n)}$|

---

5. COMPLETE SOLVED PROBLEM SET

---

Problem 1: Auxiliary Space of Iterative vs. Recursive Summation

Compare the Input Space and Auxiliary Space for the following two algorithms:

```
// Algorithm 1
int SumIterative(int A[], int n) {
    int s = 0;
    for (int i = 0; i < n; i++) s += A[i];
    return s;
}

// Algorithm 2
int SumRecursive(int A[], int n) {
    if (n <= 0) return 0;
    return A[n - 1] + SumRecursive(A, n - 1);
}
```

Step-by-Step Solution:

1. **Algorithm 1 (Iterative):**
    - _Input Space:_ Storing array `A` of size $n \implies O(n)$.
    - _Auxiliary Space:_ Scalar variables `s` and `i` use a constant number of memory locations $\implies \mathbf{O(1)}$.
2. **Algorithm 2 (Recursive):**
    - _Input Space:_ Storing array `A` of size $n \implies O(n)$.
    - _Auxiliary Space:_ Makes $n$ nested recursive calls before reaching base case `n=0`. At the deepest point of recursion, $n+1$ activation records exist on the stack simultaneously.
    - Each activation record takes $O(1)$ space $\implies \mathbf{O(n)}$.

**Answer:**

- **Algorithm 1:** Input Space = $O(n)$, Auxiliary Space = $\mathbf{O(1)}$.
- **Algorithm 2:** Input Space = $O(n)$, Auxiliary Space = $\mathbf{O(n)}$.

---

Problem 2: Stack Depth of Halving Recursion

Find the Auxiliary Space Complexity of the following procedure:

```
void HalveDemo(int n) {
    if (n <= 1) {
        return;
    }
    HalveDemo(n / 2);
}
```

Step-by-Step Solution:

1. Trace parameter sequence across active calls: $n, \frac{n}{2}, \frac{n}{4}, \frac{n}{8}, \dots, 1$.
2. The recurrence for stack depth $D(n)$ is: $$D(n) = D(n / 2) + 1 \quad \text{with } D(1) = 1$$
3. Expanding via back-substitution: $$D(n) = D(n / 2^k) + k$$
4. Base case reached when $\frac{n}{2^k} = 1 \implies 2^k = n \implies k = \log_2 n$.
5. The maximum number of simultaneous stack frames is $\log_2 n + 1$.
6. Multiply stack depth by frame size $O(1)$: $$S(n) = \mathbf{O(\log n)}$$

**Answer:** **Auxiliary Space =** $O(\log n)$.

---

Problem 3: Auxiliary Space of Square Root Stack

Calculate the maximum stack depth and Auxiliary Space for:

```
void SqrtSpaceDemo(int n) {
    if (n <= 2) {
        return;
    }
    SqrtSpaceDemo(sqrt(n));
}
```

Step-by-Step Solution:

1. Trace parameter sequence: $n, n^{1/2}, n^{1/4}, n^{1/8}, \dots, 2$.
2. Let $k$ be the number of active stack frames when base case $n^{1/2^k} = 2$ is reached.
3. Taking $\log_2$ on both sides: $$\log_2\left(n^{1/2^k}\right) = \log_2(2) \implies \frac{1}{2^k} \log_2 n = 1 \implies 2^k = \log_2 n$$
4. Taking $\log_2$ again: $$k = \log_2(\log_2 n)$$
5. Maximum simultaneous stack depth $= \log_2(\log_2 n)$.

**Answer:** **Auxiliary Space =** $O(\log \log n)$.

---

Problem 4: Auxiliary Space of Naive Fibonacci

Calculate the Time Complexity and Auxiliary Space Complexity of naive recursive Fibonacci:

```
int Fib(int n) {
    if (n <= 1) return n;
    return Fib(n - 1) + Fib(n - 2);
}
```

Step-by-Step Solution:

1. **Time Complexity:**
    - Recurrence: $T(n) = T(n-1) + T(n-2) + O(1)$.
    - Solves to $T(n) = O(\phi^n) = \mathbf{O(2^n)}$ (where $\phi \approx 1.618$).
2. **Auxiliary Space Complexity:**
    - Total function calls made $\approx 2^n$.
    - However, `Fib(n-1)` completes and pops its stack frames before `Fib(n-2)` is called.
    - The maximum simultaneous stack depth occurs along the leftmost chain: `Fib(n)` $\to$ `Fib(n-1)` $\to$ `Fib(n-2)` $\dots \to$ `Fib(1)`.
    - Height of this longest tree branch $= n$.
    - Therefore, maximum active stack depth $= n$.

**Answer:** **Time Complexity =** $O(2^n)$**, Auxiliary Space =** $O(n)$.

---

Problem 5: Array Reverse In-Place vs. Auxiliary Array

Compare the space requirements of two array reversal approaches:

- **Approach A:** Create a new array `B` of size $n$, copy elements from `A` in reverse order.
- **Approach B:** Swap symmetric elements `A[i]` and `A[n - 1 - i]` in-place using an iterative loop.

Step-by-Step Solution:

1. **Approach A:**
    - Requires creating an extra array `B` of size $n$.
    - Auxiliary Space $= \mathbf{O(n)}$.
2. **Approach B:**
    - Uses only 2 scalar variables (`i` and `temp`). No new array is allocated.
    - Auxiliary Space $= \mathbf{O(1)}$.

**Answer:** **Approach A Auxiliary Space =** $O(n)$**, Approach B Auxiliary Space =** $O(1)$ **(In-Place)**.

---

Problem 6: Stack Memory Allocation Calculation (Numerical / NAT)

A recursive function has a maximum stack depth of $k = \log_2 n$. If each activation record consumes exactly 64 bytes of stack memory, calculate the maximum auxiliary stack memory in bytes used when $n = 1024$.

Step-by-Step Solution:

1. Input size $n = 1024 = 2^{10}$.
2. Maximum stack depth $k = \log_2(1024) = 10$ stack frames.
3. Total stack memory $= \text{Stack Depth} \times \text{Frame Size}$: $$\text{Memory} = 10 \times 64 \text{ bytes} = 640 \text{ bytes}$$

**Answer:** **640 bytes**.

---

Problem 7: Multiple Choice Question (MCQ)

What is the auxiliary space complexity of binary search implemented recursively vs. iteratively on a sorted array of size $n$?

- (A) Recursive: $O(1)$, Iterative: $O(\log n)$
- (B) Recursive: $O(\log n)$, Iterative: $O(1)$
- (C) Recursive: $O(n)$, Iterative: $O(1)$
- (D) Recursive: $O(\log n)$, Iterative: $O(\log n)$

Step-by-Step Solution:

- **Iterative Binary Search:** Uses a simple `while` loop with scalar variables (`low`, `high`, `mid`). No call stack overhead $\implies Auxiliary Space = O(1)$.
- **Recursive Binary Search:** Makes recursive calls on subproblems of size $n/2$, resulting in a call stack depth of $\log_2 n$ $\implies Auxiliary Space = O(\log n)$.

**Answer:** **(B)**.