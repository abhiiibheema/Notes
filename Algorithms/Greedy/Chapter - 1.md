1: FOUNDATIONS OF GREEDY METHOD & OPTIMIZATION TERMINOLOGY

---

1. PHILOSOPHY & CONTROL ABSTRACTION

The **Greedy Method** is a powerful algorithm design paradigm used primarily to solve **Optimization Problems**. It operates by making a sequence of choices, where at each step it selects the **locally optimal option**—the choice that looks best at that exact moment—with the hope that these local choices will lead to a **globally optimal solution**.

```
                       Greedy Algorithm Paradigm
                                   │
              ┌────────────────────┴────────────────────┐
              ▼                                         ▼
   Locally Optimal Choice                     Non-Backtracking
(Picks best option at step 'i')         (Decisions are final & permanent)
```

---

Fundamental Characteristics

1. **Greedy Choice Property:** A global optimal solution can be arrived at by making locally optimal (greedy) choices.
2. **Irrevocable Decisions (No Backtracking):** Once a choice is made at step $i$, it is **never reconsidered or modified** in subsequent steps. This distinguishes Greedy algorithms from Backtracking or Dynamic Programming, which explore multiple decision paths.
3. **Efficiency:** Because it makes a single choice per step without exploring alternative paths, Greedy algorithms are typically fast, operating in $O(n)$ or $O(n \log n)$ time.

---

General Control Abstraction (Algorithm Schema)

Every Greedy algorithm follows a standardized template that iteratively builds a solution vector by testing candidate elements for feasibility.

```
Algo Greedy(A, n) {
    Solution = ∅; // Initialize empty solution set
    
    for (i = 1; i <= n; i++) {
        x = Select(A); // Pick candidate element using greedy criteria
        
        if (Feasible(Solution, x)) { // Check if 'x' satisfies implicit constraints
            Solution = Union(Solution, x); // Include 'x' in the solution
        }
    }
    return Solution; // Return optimal or feasible solution
}
```

---

2. CORE OPTIMIZATION TERMINOLOGY

To analyze Greedy algorithms rigorously, we define five fundamental terminologies that classify problem constraints and solution spaces.

```
                     Problem Classification
                                │
         ┌──────────────────────┴──────────────────────┐
         ▼                                             ▼
 Decision Problems                             Optimization Problems
- Output: True / False                         - Output: Minimized or Maximized value
- Checks explicit/implicit constraints        - Has an Objective Function
- Examples: Searching, N-Queens               - Examples: Knapsack, Shortest Path
```

---

1. Problem Classification

- **Decision Problem:** A problem that requires a binary output—either `True` (Yes) or `False` (No). It tests whether a valid configuration exists under given constraints.
    - _Examples:_ Searching for an element, Sorting verification, $n$-Queens problem.
- **Optimization Problem:** A problem that requires finding a solution that either **minimizes** or **maximizes** a specific criteria (Objective Function).
    - _Examples:_ Fractional Knapsack (Maximize profit), Shortest Path (Minimize distance), Minimum Cost Spanning Tree (Minimize edge weights).

---

2. Constraints: Explicit vs. Implicit

Constraints define the rules that valid solutions must obey:

- **Explicit Constraints:** Rules that restrict the range or domain of individual variables directly provided in the input statement.
    - _Example:_ In the $n$-Queens problem on an $n \times n$ chessboard, placing one queen per row $x_i \in \{1, 2, \dots, n\}$ is an explicit constraint.
- **Implicit Constraints:** Rules that dictate the valid interaction between multiple variables within a solution.
    - _Example:_ In the $n$-Queens problem, no two queens can share the same column or diagonal.

---

3. Solution Spaces & Feasibility

- **Solution Space:** The set of all possible candidate arrangements or variable assignments that satisfy the **explicit constraints**, regardless of whether they violate implicit rules.
    - _Example:_ For $n$-Queens with 1 queen per row, there are $n^n$ possible placements in the solution space.
- **Feasible Solution:** Any candidate solution from the solution space that satisfies **all implicit constraints**.
    - _Example:_ A chessboard layout where no two queens attack each other horizontally, vertically, or diagonally is a feasible solution. A problem can have **zero, one, or multiple feasible solutions**.

---

4. Objective Function & Optimal Solution

```
               Solution Space (All Explicit Configurations)
                     │
                     ▼
             Feasible Solutions (Satisfies Implicit Rules)
                     │
                     ▼
             Optimal Solution (Unique Minimum / Maximum Value)
```

- **Objective Function:** A mathematical function $f(X)$ that measures the quality of a feasible solution $X$ to be optimized (maximized or minimized).
- **Optimal Solution:** A feasible solution that achieves the **best possible objective function value** (maximum or minimum).
    - **Value Uniqueness:** While multiple distinct feasible configurations may achieve the optimal score, the **optimal value itself is always UNIQUE**.

---

3. COMPARATIVE SUMMARY MATRIX

|Feature / Metric|Decision Problems|Optimization Problems|
|---|---|---|
|**Output Type**|`True` / `False`|Numeric Optimal Value (Min/Max)|
|**Objective Function**|**Absent**|**Mandatory**|
|**Feasible Solutions**|Can be multiple|Can be multiple|
|**Optimal Solution**|**Does not exist**|**Unique Optimal Value**|
|**Primary Design Paradigm**|Backtracking / Branch & Bound|Greedy Method / Dynamic Programming|

---

4. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Objective Function Analysis for N-Queens (MCQ)

What is the objective function of the $n$-Queens problem?

- (A) Maximize the number of queens placed on the board.
- (B) Minimize the number of conflicting queen pairs.
- (C) It does not have an objective function; it is a decision/configuration problem.
- (D) Maximize the distance between adjacent queens.

Step-by-Step Solution:

1. The $n$-Queens problem asks to place $n$ queens on an $n \times n$ board such that no two queens attack each other.
2. The problem does not seek to maximize or minimize a numerical cost function—it simply asks for a valid configuration satisfying implicit non-attacking constraints.
3. Therefore, $n$-Queens is a **Decision/Configuration Problem** and has **no objective function**.

**Answer:** **(C)**.

---

Problem 2: Solution Space vs. Feasible Solution Counting (NAT)

For placing 4 queens on a $4 \times 4$ chessboard with the explicit constraint of placing exactly one queen per row ($x_i \in \{1, 2, 3, 4\}$):

1. Calculate the total size of the Solution Space.
2. Calculate the total number of Feasible Solutions.

Step-by-Step Solution:

1. **Solution Space Size:** Each of the 4 rows can place a queen in any of the 4 columns independently: $$\text{Size} = 4^4 = 256 \text{ arrangements}$$
2. **Feasible Solutions:** A feasible placement requires no two queens to attack each other in columns or diagonals. For $n = 4$, the distinct non-attacking permutations are 1more_horiz and 1more_horiz. $$\text{Feasible Count} = 2$$

**Answer:** **Solution Space = 256, Feasible Solutions = 2**.

---

Problem 3: Decision vs. Optimization Classification (MCQ)

Which of the following problems is classified as an **Optimization Problem**?

- (A) Determining if a graph contains a Hamiltonian Cycle.
- (B) Searching for a target key in a binary search tree.
- (C) Finding the Single-Source Shortest Path in a weighted graph.
- (D) Verifying if a given number is prime.

Step-by-Step Solution:

- Hamiltonian Cycle, Binary Search, and Prime Verification return boolean results (`True`/`False`) based on constraints $\implies$ Decision Problems.
- Single-Source Shortest Path seeks to **minimize the path cost** using an objective function $\implies$ Optimization Problem.

**Answer:** **(C)**.

---

Problem 4: Optimal Value Uniqueness (True / False)

Evaluate the truth value of the statement: _"An optimization problem can have multiple distinct optimal solutions, but it always has a unique optimal value."_

Step-by-Step Solution:

- Consider finding the shortest path from node A to C in a graph. If Path 1 (A-B-C) has cost 12 and Path 2 (A-D-C) has cost 12, both paths are distinct optimal solutions.
- However, the **minimum cost value (12)** is strictly unique.

**Answer:** **TRUE**.