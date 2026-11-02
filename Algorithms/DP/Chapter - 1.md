CHAPTER 1: FOUNDATIONS OF DYNAMIC PROGRAMMING

---

1. PHILOSOPHY & CORE PRINCIPLES OF DYNAMIC PROGRAMMING

**Dynamic Programming (DP)** is a powerful algorithmic paradigm used to solve complex optimization and counting problems by breaking them down into simpler, overlapping subproblems. The core philosophy of Dynamic Programming can be summarized in one sentence: **"Remember past results to avoid redundant work."**

When solving a recursive problem naively, the algorithm may evaluate the exact same subproblem hundreds or thousands of times. Dynamic Programming systematically stores the results of solved subproblems in a memory table (a dictionary, array, or matrix) so that each unique subproblem is computed **only once**.

```
                   Algorithmic Design Paradigm Comparison
                                     │
         ┌───────────────────────────┼───────────────────────────┐
         ▼                           ▼                           ▼
  Divide & Conquer            Greedy Strategy           Dynamic Programming
 - Independent Subproblems   - Locally Optimal Choice   - Overlapping Subproblems
 - Top-Down Recursion        - Irrevocable Decisions    - Evaluates All Decision Paths
 - Example: Merge Sort       - Example: Fractional KS   - Example: 0/1 Knapsack, MCM
```

---

Comparison of Paradigm Mechanics

|Metric / Feature|Divide and Conquer|Greedy Strategy|Dynamic Programming|
|---|---|---|---|
|**Subproblem Relationship**|Independent / Disjoint|Independent Choice Sequences|**Overlapping / Dependent**|
|**Decision Nature**|Splits problem without choice|Makes single local optimal choice|**Evaluates all choices to find global optimal**|
|**Backtracking / Re-evaluation**|No|**No (Irrevocable)**|**Yes (Explores choices via recurrence)**|
|**Memory Overhead**|Recursion Stack $O(\log n)$|$O(1)$ or $O(n)$|**Table Storage (Auxiliary Space** $O(n)$ **or** $O(n \cdot M)$**)**|
|**Optimality Guarantee**|Always finds exact solution|Solves specific greedy structures|**Guaranteed Global Optimal Solution**|

---

2. THE TWO MANDATORY CONDITIONS FOR DYNAMIC PROGRAMMING

A problem can be efficiently solved using Dynamic Programming if and only if it satisfies two distinct mathematical properties:

```
                  Necessary Conditions for Dynamic Programming
                                       │
              ┌────────────────────────┴────────────────────────┐
              ▼                                                 ▼
    Optimal Substructure                              Overlapping Subproblems
- An optimal solution to a problem             - The recursive tree contains repeated
  contains optimal solutions to its               computations of identical subproblems
  subproblems.                                 - Stored in memory to avoid recalculation
```

---

Condition 1: Optimal Substructure

A problem exhibits **Optimal Substructure** if an optimal solution to the overall problem instance contains within it optimal solutions to its smaller constituent subproblems.

- **Mathematical Expression:** If $S^*$ is the optimal path/choice from state $A$ to state $C$ passing through an intermediate state $B$, then the sub-path from $A$ to $B$ within $S^*$ must also be the shortest/optimal path from $A$ to $B$.
- **Failure Example (Longest Simple Path):** Finding the _shortest path_ in a graph exhibits optimal substructure (and can be solved via Dijkstra / Bellman-Ford). However, finding the _longest simple path_ between two nodes does **not** exhibit optimal substructure because sub-paths can conflict by visiting the same vertex twice.

---

Condition 2: Overlapping Subproblems

A problem exhibits **Overlapping Subproblems** if a recursive formulation visits the same subproblems repeatedly, rather than generating new subproblems at each level.

Consider computing the $n$-th Fibonacci number using naive recursion: $F(n) = F(n-1) + F(n-2)$.

```
                            Naive Recursion Tree for F(5)
                                       F(5)
                                     /      \
                                 F(4)        F(3)
                                /    \      /    \
                             F(3)   F(2)  F(2)   F(1)
                            /   \
                         F(2)   F(1)
```

Observation of Redundancy

- $F(3)$ is computed **2 times**.
- $F(2)$ is computed **3 times**.
- As $n$ grows, the number of recursive calls grows exponentially as $O(2^n)$ or $O(\phi^n)$.
- By storing the result of $F(2)$ and $F(3)$ the first time they are calculated, we eliminate all redundant subtree branches, reducing the running time from **exponential** $O(2^n)$ to **linear** $O(n)$.

---

3. IMPLEMENTATION STRATEGIES: MEMOIZATION VS. TABULATION

Dynamic Programming can be implemented using two distinct computational workflows: **Top-Down (Memoization)** and **Bottom-Up (Tabulation)**.

```
                      Dynamic Programming Implementations
                                       │
         ┌─────────────────────────────┴─────────────────────────────┐
         ▼                                                           ▼
Top-Down (Memoization)                                     Bottom-Up (Tabulation)
- Recursive in structure                                   - Iterative in structure
- Fills table On-Demand (Lazy evaluation)                   - Fills table Systematic/Sequential
- Overhead: Recursion Call Stack Memory                     - Zero stack overhead
```

---

Strategy A: Top-Down DP (Memoization)

Memoization retains the intuitive recursive structure of the problem. Before making a recursive call to evaluate a subproblem, the algorithm checks a lookup table:

1. If the subproblem result exists in the table, it returns the precomputed value immediately ($O(1)$).
2. If the subproblem result is uninitialized (e.g., `-1` or `NULL`), it computes the value recursively, stores it in the lookup table, and then returns it.

```
// Top-Down Memoized Fibonacci
int Memo; // Global table initialized to -1

int Fib_Memo(int n) {
    if (n <= 1) return n;
    
    // Step 1: Check lookup table
    if (Memo[n] != -1) {
        return Memo[n]; // Return stored result
    }
    
    // Step 2: Compute recursively, store, and return
    Memo[n] = Fib_Memo(n - 1) + Fib_Memo(n - 2);
    return Memo[n];
}
```

---

Strategy B: Bottom-Up DP (Tabulation)

Tabulation eliminates recursion entirely. It identifies the base cases first and iteratively fills an explicit table (1D array, 2D matrix) from the smallest subproblems up to the target state.

```
// Bottom-Up Tabulated Fibonacci
int Fib_Tab(int n) {
    int Table[n + 1];
    
    // Step 1: Initialize base cases
    Table = 0;
    Table[1] = 1;
    
    // Step 2: Fill table iteratively in bottom-up order
    for (int i = 2; i <= n; i++) {
        Table[i] = Table[i - 1] + Table[i - 2];
    }
    
    return Table[n]; // Target solution
}
```

---

Structural Comparison Matrix

|Parameter / Metric|Top-Down (Memoization)|Bottom-Up (Tabulation)|
|---|---|---|
|**Execution Flow**|Recursion (Top-Down from target to base)|Iteration (Bottom-Up from base to target)|
|**Subproblem Evaluation**|**Lazy:** Computes only required subproblems|**Eager:** Computes all table entries systematically|
|**Memory Stack Overhead**|**Consumes Recursion Call Stack Memory**|**Zero Stack Memory Overhead**|
|**Code Modularity**|Easy to write directly from recurrence|Requires clear dependency order identification|
|**Efficiency Factor**|Slightly slower due to function call overhead|**Faster in practice due to loop efficiency**|

---

4. GENERAL WORKFLOW FOR SOLVING DP PROBLEMS

To tackle any Dynamic Programming problem systematically, follow this 4-step framework:

1. **Define the State:** Clearly define what a table entry (e.g., `DP[i]`, `DP[i][j]`) represents in the context of the problem.
2. **Formulate the Recurrence Relation:** Express the solution of a state `DP[i][j]` in terms of smaller subproblems.
3. **Identify Base Cases:** Set up boundary conditions that stop recursion (e.g., empty set, zero capacity, zero length).
4. **Determine the Fill Order & Compute:** Choose between Top-Down Memoization or Bottom-Up Tabulation, establish loop bounds, and retrieve the final answer at the target index (e.g., `DP[n]`, `DP[n][M]`).

---

5. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Time and Space Complexity Comparison (NAT)

Calculate the Time Complexity and Auxiliary Space Complexity for computing the $n$-th Fibonacci number using:

1. Naive Pure Recursion
2. Top-Down Memoized DP
3. Bottom-Up Tabulated DP

Step-by-Step Solution:

1. **Naive Pure Recursion:**
    - Recurrence: $T(n) = T(n-1) + T(n-2) + O(1)$.
    - Generates a full binary recursion tree of depth $n$.
    - **Time Complexity:** $\mathbf{O(2^n)}$ (or $O(\phi^n)$ where $\phi \approx 1.618$).
    - **Auxiliary Space Complexity:** $\mathbf{O(n)}$ (Recursion stack depth).
2. **Top-Down Memoized DP:**
    - Evaluates each unique subproblem $F(0), F(1), \dots, F(n)$ exactly once.
    - **Time Complexity:** $\mathbf{O(n)}$.
    - **Auxiliary Space Complexity:** $O(n)$ lookup table + $O(n)$ recursion stack $= \mathbf{O(n)}$.
3. **Bottom-Up Tabulated DP:**
    - Single loop running from $i = 2$ to $n$.
    - **Time Complexity:** $\mathbf{O(n)}$.
    - **Auxiliary Space Complexity:** $\mathbf{O(n)}$ (Table size).
    - _(Note: Can be further space-optimized to_ $O(1)$ _by storing only the last two state variables)._

**Answer:**

- Naive: Time = $O(2^n)$, Space = $O(n)$.
- Memoized: Time = $O(n)$, Space = $O(n)$.
- Tabulated: Time = $O(n)$, Space = $O(n)$ (or $O(1)$ optimized).

---

Problem 2: Identifying DP Suitability (MCQ)

Which of the following computational problems is **NOT suitable** for Dynamic Programming?

- (A) Finding the Single-Source Shortest Path in a graph with negative edges but no negative cycles (Bellman-Ford).
- (B) Finding the Longest Common Subsequence between two strings.
- (C) Sorting an array of $n$ elements using Merge Sort.
- (D) Matrix Chain Multiplication to minimize scalar operations.

Step-by-Step Solution:

- Bellman-Ford, LCS, and Matrix Chain Multiplication contain **overlapping subproblems** and **optimal substructure**, making them classic DP applications.
- Merge Sort divides the array into strictly **disjoint/independent subproblems** (left half and right half do not overlap). Thus, it uses **Divide and Conquer**, not Dynamic Programming.

**Answer:** **(C)**.

---

Problem 3: Unique Subproblem Counting (NAT)

A Dynamic Programming recurrence relation is given by: $$DP(i, j) = \min(DP(i-1, j), DP(i-1, j - W_i) + P_i)$$ For $1 \le i \le n$ and $0 \le j \le M$, how many **unique subproblems** exist in the entire state space table?

Step-by-Step Solution:

1. Parameter $i$ takes integer values from $0$ to $n$ (total $n + 1$ states).
2. Parameter $j$ takes integer values from $0$ to $M$ (total $M + 1$ states).
3. Since every state is uniquely identified by the pair $(i, j)$, the total number of table cells is: $$\text{Total Subproblems} = (n + 1) \times (M + 1) = \mathbf{O(n \cdot M)}$$

**Answer:** $(n + 1)(M + 1)$ **or** $O(n \cdot M)$.

---

Problem 4: Memoization vs. Tabulation Overhead (MCQ)

Why is Bottom-Up Tabulation generally preferred over Top-Down Memoization in production environments?

- (A) Tabulation has a better asymptotic time complexity than Memoization.
- (B) Tabulation avoids function call stack overhead and prevents stack overflow errors.
- (C) Memoization computes more subproblems than Tabulation.
- (D) Tabulation does not require any array or matrix storage.

Step-by-Step Solution:

Both approaches share the exact same asymptotic time complexity $O(\text{subproblems})$. However, Top-Down Memoization uses deep recursion, which introduces CPU stack frame allocation overhead and risks throwing a **StackOverflowError** for large inputs. Bottom-Up Tabulation uses simple iterative loops, completely avoiding stack overhead.

**Answer:** **(B)**.
