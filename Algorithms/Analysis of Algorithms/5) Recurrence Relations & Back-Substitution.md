RECURRENCE RELATIONS & BACK-SUBSTITUTION

---

1. FOUNDATIONS OF RECURSIVE ANALYSIS

When an algorithm is iterative, its time complexity is computed by counting loop passes. However, when an algorithm is **recursive** (a function calling itself with smaller input sizes), its execution time cannot be measured by a single loop index. Instead, we model its execution time as a **Recurrence Relation**—a mathematical equation that defines the total time $T(n)$ required for an input of size $n$ in terms of the time required for smaller subproblem sizes plus the non-recursive work performed at that step.

```
                      Recursive Function Execution
                                   │
              ┌────────────────────┴────────────────────┐
              ▼                                         ▼
       Base Condition                           Recursive Step
  (Stops recursion, takes O(1) time)     (Reduces input size & makes recursive call)
```

Components of a Recurrence Relation

Every valid recurrence relation derived from recursive code consists of two mandatory parts:

1. **Base Condition (Terminating Case):** The smallest input size $n_0$ (typically $n=1$ or $n=2$) where the algorithm stops making recursive calls and returns a result directly in constant time $O(1)$.
2. **Recursive Case:** The mathematical expression defining $T(n)$ as a function of one or more smaller subproblems (such as $T(n-1)$ or $T(n/2)$), plus the additional non-recursive work $f(n)$ (such as comparisons, loop passes, or arithmetic operations).

$$\begin{cases} T(n) = c & \text{for } n = n_0 \quad (\text{Base Case}) \\ T(n) = a \cdot T(\text{subproblem size}) + f(n) & \text{for } n > n_0 \quad (\text{Recursive Case}) \end{cases}$$

---

Code-to-Equation Mapping Rules

To convert recursive pseudo-code into a recurrence relation:

- Count the number of internal recursive calls (gives coefficient $a$).
- Determine how the input argument is reduced in each call (gives subproblem size).
- Evaluate the time complexity of all non-recursive statements outside the recursive calls (gives $f(n)$).

|Pseudo-Code Control Structure|Subproblem Split|Non-Recursive Work|Resulting Recurrence Relation|
|---|---|---|---|
|**Linear Decrement (****n - 1****)**|1 call of size $(n-1)$|$O(1)$ constant work|$T(n) = T(n-1) + C$|
|**Loop + Decrement**|1 call of size $(n-1)$|$O(n)$ loop work|$T(n) = T(n-1) + C \cdot n$|
|**Branching Decrement**|2 calls of size $(n-1)$|$O(1)$ constant work|$T(n) = 2T(n-1) + C$|
|**Single Halving (****n / 2****)**|1 call of size $(n/2)$|$O(1)$ constant work|$T(n) = T(n/2) + C$|
|**Branching Halving**|2 calls of size $(n/2)$|$O(1)$ constant work|$T(n) = 2T(n/2) + C$|
|**Divide & Conquer**|2 calls of size $(n/2)$|$O(n)$ combining work|$T(n) = 2T(n/2) + C \cdot n$|
|**High Branching Division**|8 calls of size $(n/2)$|$O(1)$ constant work|$T(n) = 8T(n/2) + C$|
|**Square Root Reduction**|1 call of size $\sqrt{n}$|$O(1)$ constant work|$T(n) = T(\sqrt{n}) + C$|

---

2. THE BACK-SUBSTITUTION METHOD (STEP-BY-STEP DERIVATIONS)

The **Back-Substitution Method** (also known as the Method of Successive Substitution) is a universal technique for solving recurrence relations.

The Systematic 6-Step Framework

1. **State the base recurrence equation** for $T(n)$.
2. **Express the subproblem term** (e.g., $T(n-1)$ or $T(n/b)$) by substituting the smaller parameter into the base equation.
3. **Substitute this expression back** into the original equation and simplify algebraically.
4. **Repeat substitutions for 2 to 3 steps** until a clear algebraic pattern emerges for the $k$-th expansion step.
5. **Set the subproblem size equal to the base condition** to solve for $k$.
6. **Substitute** $k$ **back into the general equation** to obtain the final closed-form asymptotic solution.

---

Master Derivation 1: Linear Decrement Constant Work

$$T(n) = T(n-1) + C \quad \text{with base condition } T(1) = C$$

- **Step 1 (Original Equation):** $$T(n) = T(n-1) + C \quad \text{--- (Equation 1)}$$
- **Step 2 (First Substitution):** Express $T(n-1)$ by replacing $n$ with $n-1$ in Eq 1: $$T(n-1) = T(n-2) + C$$ Substitute back into Eq 1: $$T(n) = [T(n-2) + C] + C = T(n-2) + 2C$$
- **Step 3 (Second Substitution):** Express $T(n-2) = T(n-3) + C$: $$T(n) = [T(n-3) + C] + 2C = T(n-3) + 3C$$
- **Step 4 (General Term at Step** $k$**):** $$T(n) = T(n-k) + k \cdot C$$
- **Step 5 (Apply Base Condition):** Set $n-k = 1 \implies k = n-1$.
- **Step 6 (Closed-Form Solution):** $$T(n) = T(1) + (n-1)C = C + nC - C = C \cdot n$$

$$\mathbf{T(n) = O(n)}$$

---

Master Derivation 2: Linear Decrement Polynomial Work

$$T(n) = T(n-1) + C \cdot n \quad \text{with base condition } T(1) = C$$

- **Step 1 (Original Equation):** $$T(n) = T(n-1) + C \cdot n \quad \text{--- (Equation 1)}$$
- **Step 2 (First Substitution):** Express $T(n-1) = T(n-2) + C(n-1)$: $$T(n) = [T(n-2) + C(n-1)] + C \cdot n = T(n-2) + C(n-1) + C \cdot n$$
- **Step 3 (Second Substitution):** Express $T(n-2) = T(n-3) + C(n-2)$: $$T(n) = T(n-3) + C(n-2) + C(n-1) + C \cdot n$$
- **Step 4 (General Term at Step** $k$**):** $$T(n) = T(n-k) + C \sum_{j=0}^{k-1} (n-j)$$
- **Step 5 (Apply Base Condition):** Set $n-k = 1 \implies k = n-1$.
- **Step 6 (Closed-Form Solution):** $$T(n) = T(1) + C [2 + 3 + 4 + \dots + n] = C \left[1 + 2 + 3 + \dots + n\right] = C \cdot \frac{n(n+1)}{2}$$

$$\mathbf{T(n) = O(n^2)}$$

---

Master Derivation 3: Exponential Branching Decrement

$$T(n) = 2T(n-1) + a \quad \text{with base condition } T(1) = b$$

- **Step 1 (Original Equation):** $$T(n) = 2T(n-1) + a \quad \text{--- (Equation 1)}$$
- **Step 2 (First Substitution):** Express $T(n-1) = 2T(n-2) + a$: $$T(n) = 2[2T(n-2) + a] + a = 2^2 T(n-2) + 2a + a = 2^2 T(n-2) + 3a$$
- **Step 3 (Second Substitution):** Express $T(n-2) = 2T(n-3) + a$: $$T(n) = 2^2 [2T(n-3) + a] + 3a = 2^3 T(n-3) + 4a + 3a = 2^3 T(n-3) + 7a$$
- **Step 4 (General Term at Step** $k$**):** $$T(n) = 2^k T(n-k) + (2^k - 1)a$$
- **Step 5 (Apply Base Condition):** Set $n-k = 1 \implies k = n-1$.
- **Step 6 (Closed-Form Solution):** $$T(n) = 2^{n-1} T(1) + (2^{n-1} - 1)a = 2^{n-1} b + (2^{n-1} - 1)a$$

$$\mathbf{T(n) = O(2^n)}$$

---

Master Derivation 4: Logarithmic Division

$$T(n) = T(n/2) + C \quad \text{with base condition } T(1) = 1$$

- **Step 1 (Original Equation):** $$T(n) = T(n/2) + C \quad \text{--- (Equation 1)}$$
- **Step 2 (First Substitution):** Express $T(n/2) = T(n/2^2) + C$: $$T(n) = [T(n/2^2) + C] + C = T(n/2^2) + 2C$$
- **Step 3 (Second Substitution):** Express $T(n/2^2) = T(n/2^3) + C$: $$T(n) = T(n/2^3) + 3C$$
- **Step 4 (General Term at Step** $k$**):** $$T(n) = T\left(\frac{n}{2^k}\right) + k \cdot C$$
- **Step 5 (Apply Base Condition):** Set $\frac{n}{2^k} = 1 \implies 2^k = n \implies k = \log_2 n$.
- **Step 6 (Closed-Form Solution):** $$T(n) = T(1) + (\log_2 n) \cdot C = 1 + C \log_2 n$$

$$\mathbf{T(n) = O(\log n)}$$

---

Master Derivation 5: Linear Division

$$T(n) = 2T(n/2) + C \quad \text{with base condition } T(1) = 1$$

- **Step 1 (Original Equation):** $$T(n) = 2T(n/2) + C \quad \text{--- (Equation 1)}$$
- **Step 2 (First Substitution):** Express $T(n/2) = 2T(n/2^2) + C$: $$T(n) = 2[2T(n/2^2) + C] + C = 2^2 T(n/2^2) + 2C + C = 2^2 T(n/2^2) + 3C$$
- **Step 3 (Second Substitution):** Express $T(n/2^2) = 2T(n/2^3) + C$: $$T(n) = 2^2 [2T(n/2^3) + C] + 3C = 2^3 T(n/2^3) + 7C$$
- **Step 4 (General Term at Step** $k$**):** $$T(n) = 2^k T\left(\frac{n}{2^k}\right) + (2^k - 1)C$$
- **Step 5 (Apply Base Condition):** Set \(\frac{n}{2^k} = 1 # CHAPTER 5: RECURRENCE RELATIONS & BACK-SUBSTITUTION

---

1. RECURRENCE FORMULATION FROM CODE

Iterative algorithms are evaluated by counting loop passes, whereas **recursive algorithms** (functions calling themselves with reduced parameters) are modeled mathematically using **Recurrence Relations**. A recurrence defines the total execution time $T(n)$ on input size $n$ in terms of the time needed for smaller subproblems plus non-recursive work.

```
                      Recursive Function Execution
                                   │
              ┌────────────────────┴────────────────────┐
              ▼                                         ▼
       Base Condition                           Recursive Step
  (Stops recursion, takes O(1) time)     (Reduces input size & makes recursive call)
```

Every recurrence derived from code must specify:

1. **Base Case:** The smallest parameter (usually $n=1$ or $n=2$) where execution halts without further recursive calls, taking $O(1)$ time.
2. **Recursive Step:** An equation expressing $T(n)$ using smaller call parameters, plus non-recursive overhead $f(n)$ (loop passes, comparisons, or arithmetic).

$$\begin{cases} T(n) = c & \text{for } n = n_0 \quad (\text{Base Case}) \\ T(n) = a \cdot T(\text{subproblem size}) + f(n) & \text{for } n > n_0 \quad (\text{Recursive Case}) \end{cases}$$

Common Code Patterns & Recurrence Formulas

|Code Structure|Subproblem Size|Non-Recursive Work|Recurrence Relation|
|---|---|---|---|
|**Linear Decrement (****n - 1****)**|1 call of size $(n-1)$|$O(1)$ constant work|$T(n) = T(n-1) + C$|
|**Loop + Decrement**|1 call of size $(n-1)$|$O(n)$ loop work|$T(n) = T(n-1) + C \cdot n$|
|**Branching Decrement**|2 calls of size $(n-1)$|$O(1)$ constant work|$T(n) = 2T(n-1) + C$|
|**Single Halving (****n / 2****)**|1 call of size $(n/2)$|$O(1)$ constant work|$T(n) = T(n/2) + C$|
|**Branching Halving**|2 calls of size $(n/2)$|$O(1)$ constant work|$T(n) = 2T(n/2) + C$|
|**Divide & Conquer**|2 calls of size $(n/2)$|$O(n)$ combining work|$T(n) = 2T(n/2) + C \cdot n$|
|**High Branching Division**|8 calls of size $(n/2)$|$O(1)$ constant work|$T(n) = 8T(n/2) + C$|
|**Square Root Reduction**|1 call of size $\sqrt{n}$|$O(1)$ constant work|$T(n) = T(\sqrt{n}) + C$|

---

2. THE BACK-SUBSTITUTION METHOD

The **Back-Substitution Method** resolves recurrences by repeatedly substituting the recursive term into itself, identifying an algebraic pattern at step $k$, and applying the base condition boundary to eliminate $k$.

---

Key Derivation Walkthroughs

1. Linear Decrement Constant Work: $T(n) = T(n-1) + C$, $T(1) = C$

Substituting iteratively:

- Step 1: $T(n) = T(n-1) + C$
- Step 2: Replace $T(n-1)$ to get $T(n) = [T(n-2) + C] + C = T(n-2) + 2C$
- Step 3: Replace $T(n-2)$ to get $T(n) = T(n-3) + 3C$
- General Step $k$: $T(n) = T(n-k) + k \cdot C$

Setting $n - k = 1 \implies k = n - 1$: $$T(n) = T(1) + (n - 1)C = C + (n - 1)C = C \cdot n \implies \mathbf{O(n)}$$

---

2. Linear Decrement Polynomial Work: $T(n) = T(n-1) + C \cdot n$, $T(1) = C$

Expanding step-by-step:

- Step 1: $T(n) = T(n-1) + C \cdot n$
- Step 2: $T(n) = [T(n-2) + C(n-1)] + C \cdot n = T(n-2) + C(n-1) + C \cdot n$
- Step 3: $T(n) = T(n-3) + C(n-2) + C(n-1) + C \cdot n$
- General Step $k$: $T(n) = T(n-k) + C \sum_{j=0}^{k-1} (n - j)$

Setting $n - k = 1 \implies k = n - 1$: $$T(n) = T(1) + C[2 + 3 + \dots + n] = C \cdot \frac{n(n+1)}{2} \implies \mathbf{O(n^2)}$$

---

3. Exponential Branching Decrement: $T(n) = 2T(n-1) + a$, $T(1) = b$

Expanding step-by-step:

- Step 1: $T(n) = 2T(n-1) + a$
- Step 2: $T(n) = 2[2T(n-2) + a] + a = 2^2 T(n-2) + 3a$
- Step 3: $T(n) = 2^2 [2T(n-3) + a] + 3a = 2^3 T(n-3) + 7a$
- General Step $k$: $T(n) = 2^k T(n-k) + (2^k - 1)a$

Setting $n - k = 1 \implies k = n - 1$: $$T(n) = 2^{n-1} b + (2^{n-1} - 1)a \implies \mathbf{O(2^n)}$$

---

4. Logarithmic Division: $T(n) = T(n/2) + C$, $T(1) = 1$

Expanding step-by-step:

- Step 1: $T(n) = T(n/2) + C$
- Step 2: $T(n) = T(n/2^2) + 2C$
- Step 3: $T(n) = T(n/2^3) + 3C$
- General Step $k$: $T(n) = T(n / 2^k) + k \cdot C$

Setting $\frac{n}{2^k} = 1 \implies 2^k = n \implies k = \log_2 n$: $$T(n) = T(1) + (\log_2 n) \cdot C = 1 + C \log_2 n \implies \mathbf{O(\log n)}$$

---

5. Linear Division: $T(n) = 2T(n/2) + C$, $T(1) = 1$

Expanding step-by-step:

- Step 1: $T(n) = 2T(n/2) + C$
- Step 2: $T(n) = 2[2T(n/2^2) + C] + C = 2^2 T(n/2^2) + 3C$
- Step 3: $T(n) = 2^3 T(n/2^3) + 7C$
- General Step $k$: $T(n) = 2^k T(n / 2^k) + (2^k - 1)C$

Setting $2^k = n \implies k = \log_2 n$: $$T(n) = n \cdot T(1) + (n - 1)C = n + (n - 1)C \implies \mathbf{O(n)}$$

---

6. Standard Divide & Conquer: $T(n) = 2T(n/2) + n$, $T(1) = a$

Expanding step-by-step:

- Step 1: $T(n) = 2T(n/2) + n$
- Step 2: $T(n) = 2[2T(n/2^2) + n/2] + n = 2^2 T(n/2^2) + 2n$
- Step 3: $T(n) = 2^3 T(n/2^3) + 3n$
- General Step $k$: $T(n) = 2^k T(n / 2^k) + k \cdot n$

Setting $2^k = n \implies k = \log_2 n$: $$T(n) = n \cdot T(1) + n \log_2 n = a \cdot n + n \log_2 n \implies \mathbf{O(n \log n)}$$

---

7. High Branching Factor Division: $T(n) = 8T(n/2) + a$, $T(1) = 1$

Expanding step-by-step:

- Step 1: $T(n) = 8T(n/2) + a$
- Step 2: $T(n) = 8[8T(n/2^2) + a] + a = 8^2 T(n/2^2) + 8a + a$
- Step 3: $T(n) = 8^3 T(n/2^3) + (8^2 + 8 + 1)a$
- General Step $k$: $T(n) = 8^k T(n / 2^k) + a \sum_{j=0}^{k-1} 8^j = 8^k T(n / 2^k) + a \frac{8^k - 1}{7}$

Setting $2^k = n \implies 8^k = (2^3)^k = (2^k)^3 = n^3$: $$T(n) = n^3 T(1) + a \frac{n^3 - 1}{7} \implies \mathbf{O(n^3)}$$

---

3. ADVANCED RECURRENCES & TREES

---

1. Variable Substitution Method (Square Root Reductions)

When subproblems reduce via square roots (e.g., $T(n) = 2T(\sqrt{n}) + C$), standard back-substitution becomes tedious. We use the **Change of Variable Method**.

Walkthrough: $T(n) = 2T(\sqrt{n}) + C$ with $T(2) = C$

1. **Change Variable:** Let $n = 2^m \implies m = \log_2 n$.
2. **Rewrite Subproblem:** $\sqrt{n} = n^{1/2} = (2^m)^{1/2} = 2^{m/2}$.
3. **Substitute into Recurrence:** $$T(2^m) = 2T(2^{m/2}) + C$$
4. **Define New Function:** Let $S(m) = T(2^m)$: $$S(m) = 2S(m/2) + C$$
5. **Solve** $S(m)$**:** This is a standard linear division recurrence $\implies S(m) = \Theta(m)$.
6. **Substitute back** $m = \log_2 n$**:** $$T(n) = S(\log_2 n) = \mathbf{\Theta(\log n)}$$

---

Walkthrough: $T(n) = T(\sqrt{n}) + C$ with $T(2) = C$

1. Express input as repeated fractional exponents: $$T(n) = T(n^{1/2}) + C = T(n^{1/2^2}) + 2C = T(n^{1/2^k}) + k \cdot C$$
2. Set base condition $n^{1/2^k} = 2$: $$\log_2 n = 2^k \implies k = \log_2(\log_2 n)$$
3. Substitute $k$ back: $$T(n) = T(2) + C \log_2(\log_2 n) = \mathbf{\Theta(\log \log n)}$$

---

Walkthrough: $T(n) = \sqrt{n} \cdot T(\sqrt{n}) + n$ with $T(2) = 2$

1. Divide both sides by $n$: $$\frac{T(n)}{n} = \frac{\sqrt{n} \cdot T(\sqrt{n})}{n} + 1 \implies \frac{T(n)}{n} = \frac{T(\sqrt{n})}{\sqrt{n}} + 1$$
2. Substitute $G(n) = \frac{T(n)}{n}$: $$G(n) = G(\sqrt{n}) + 1$$
3. Solve $G(n)$ using the square-root logarithmic result above: $$G(n) = \Theta(\log \log n)$$
4. Multiply back by $n$: $$T(n) = n \cdot G(n) = \mathbf{\Theta(n \log \log n)}$$

---

2. The Recursion Tree Method

A **Recursion Tree** tracks work done across execution levels. It is especially useful for asymmetric recurrences where subproblems split unevenly.

```
                         Level 0:  n               ──► Cost: n
                                  / \
                        Level 1: n/3  2n/3         ──► Cost: n/3 + 2n/3 = n
                                / \    / \
                        Level 2: ...               ──► Cost: n
```

Asymmetric Recurrence Analysis: $T(n) = T(n/3) + T(2n/3) + n$

1. **Cost per Level:**
    - Level 0: $n$
    - Level 1: $\frac{n}{3} + \frac{2n}{3} = n$
    - Level 2: $\frac{n}{9} + \frac{2n}{9} + \frac{2n}{9} + \frac{4n}{9} = n$
2. **Tree Depth:**
    - Shortest path to a leaf (left branch): $n/3^k = 1 \implies k = \log_3 n$.
    - Longest path to a leaf (right branch): $n \cdot (2/3)^k = 1 \implies k = \log_{3/2} n$.
3. **Total Work:** Every full level contributes $n$ work over a depth of $\Theta(\log n)$. $$\mathbf{T(n) = \Theta(n \log n)}$$

---

3. Master's Theorem for Divide and Conquer

For recurrences of the standard form: $$T(n) = a T\left(\frac{n}{b}\right) + \Theta(n^k \log^p n) \quad \text{where } a \ge 1, b > 1, k \ge 0, p \in \mathbb{R}$$

We compare $a$ against $b^k$:

```
                        Compare 'a' with 'bᵏ'
                                  │
         ┌────────────────────────┼────────────────────────┐
         ▼                        ▼                        ▼
      a > bᵏ                   a = bᵏ                   a < bᵏ
 (Leaf Dominates)         (Balanced Work)          (Root Dominates)
  T(n) = Θ(n^(log_b a))    Depends on 'p'           Depends on 'p'
```

Case 1: Leaf Work Dominates ($a > b^k$)

$$T(n) = \Theta\left(n^{\log_b a}\right)$$

Case 2: Balanced Work Across Levels ($a = b^k$)

- If $p > -1 \implies T(n) = \Theta\left(n^{\log_b a} \log^{p+1} n\right)$
- If $p = -1 \implies T(n) = \Theta\left(n^{\log_b a} \log(\log n)\right)$
- If $p < -1 \implies T(n) = \Theta\left(n^{\log_b a}\right)$

Case 3: Root / Non-Recursive Work Dominates ($a < b^k$)

- If $p \ge 0 \implies T(n) = \Theta\left(n^k \log^p n\right)$
- If $p < 0 \implies T(n) = O\left(n^k\right)$

---

4. COMPLETE SOLVED PROBLEM SET

---

Problem 1: Recursive Call with Fractional Decrement

Calculate the time complexity of the algorithm:

```
int Algo1(int n) {
    if (n == 1) return 1;
    else return Algo1(n / 2) + Algo1(n / 2) + 5;
}
```

Solution:

1. **Formulate Recurrence:** Two calls of size $n/2$ with $O(1)$ addition work: $$T(n) = 2T(n/2) + C \quad \text{with } T(1) = 1$$
2. **Apply Master's Theorem:** $a = 2, b = 2, k = 0, p = 0$.
3. Compare $a$ and $b^k$: $a = 2$, $b^k = 2^0 = 1$. Since $a > b^k$ ($2 > 1$), Case 1 applies: $$T(n) = \Theta\left(n^{\log_2 2}\right) = \mathbf{\Theta(n)}$$

---

Problem 2: Divide and Conquer with Linear Combining Work

Find the time complexity of the recurrence: $$T(n) = 2T(n/2) + 7n \quad \text{for } n > 1, \quad T(1) = 1$$

Solution:

1. Master's parameters: $a = 2, b = 2, k = 1, p = 0$.
2. Compare $a$ and $b^k$: $a = 2$, $b^k = 2^1 = 2$. Since $a = b^k$ and $p = 0 > -1$, Case 2 applies: $$T(n) = \Theta\left(n^{\log_2 2} \log^{0+1} n\right) = \mathbf{\Theta(n \log n)}$$

---

Problem 3: Recurrence with Logarithmic Overhead

Solve the recurrence $T(n) = 2T(n/2) + n \log n$ with $T(1) = 1$.

Solution:

1. Master's parameters: $a = 2, b = 2, k = 1, p = 1$.
2. Compare $a$ and $b^k$: $a = 2$, $b^k = 2^1 = 2$. Since $a = b^k$ and $p = 1 > -1$, Case 2 applies: $$T(n) = \Theta\left(n^{\log_2 2} \log^{1+1} n\right) = \mathbf{\Theta(n \log^2 n)}$$

---

Problem 4: High Branching Factor Recurrence

Solve $T(n) = 8T(n/2) + 5$ with $T(1) = 1$.

Solution:

1. Master's parameters: $a = 8, b = 2, k = 0, p = 0$.
2. Compare $a$ and $b^k$: $a = 8$, $b^k = 2^0 = 1$. Since $a > b^k$ ($8 > 1$), Case 1 applies: $$T(n) = \Theta\left(n^{\log_2 8}\right) = \mathbf{\Theta(n^3)}$$

---

Problem 5: Advanced Square Root Recurrence

Determine the time complexity of:

```
int Algo2(int n) {
    if (n <= 2) return 2;
    else return Algo2(sqrt(n)) + Algo2(sqrt(n)) + 10;
}
```

Solution:

1. **Formulate Recurrence:** $T(n) = 2T(\sqrt{n}) + C$ with $T(2) = C$.
2. **Variable Substitution:** Let $n = 2^m \implies m = \log_2 n$. $$T(2^m) = 2T(2^{m/2}) + C \implies S(m) = 2S(m/2) + C$$
3. **Solve** $S(m)$**:** $a = 2, b = 2, k = 0 \implies a > b^k \implies S(m) = \Theta(m)$.
4. **Substitute back** $m = \log_2 n$**:** $$T(n) = S(\log_2 n) = \mathbf{\Theta(\log n)}$$

---

Problem 6: Conditional Branching Recurrence

An algorithm branches based on conditional checks:

- In Path A, it executes 2 calls of size $n/2$.
- In Path B, it executes 1 call of size $n/2$. What are the Best Case and Worst Case time complexities?

Solution:

- **Best Case Path (Path B - Minimum Calls):** $$T(n) = T(n/2) + O(1) \implies \mathbf{\Theta(\log n)}$$
- **Worst Case Path (Path A - Maximum Calls):** $$T(n) = 2T(n/2) + O(1) \implies \mathbf{\Theta(n)}$$