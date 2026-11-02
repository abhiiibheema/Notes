4: ITERATIVE ALGORITHM ANALYSIS & SYNTAX TRAPS

---

1. CORE MECHANICS OF LOOP ANALYSIS

Analyzing the time complexity of an iterative algorithm (loops) requires evaluating two fundamental factors:

1. **The total number of iterations:** How many times the loop condition evaluates to `true`.
2. **The time complexity of the statements inside the loop body:** The work performed per iteration.

$$\text{Total Time Complexity} = \sum_{\text{iteration}=1}^{k} \left(\text{Work done in loop body at iteration } i\right)$$

```
                      Iterative Loop Execution
                                 │
           ┌─────────────────────┴─────────────────────┐
           ▼                                           ▼
  Linear Step Sizes                           Non-Linear Step Sizes
 (Additive increments)                     (Multiplicative / Divisive)
  - i = i + b  ──► O(n)                     - i = i * b  ──► O(log_b n)
  - i = i - b  ──► O(n)                     - j = j / b  ──► O(log_b n)
```

---

Fundamental Step-Size Rules

A. Additive Increment (`i = i + b`)

```
for (i = 1; i <= n; i = i + b) {
    printf(i);
}
```

- **Iteration Count:** The loop variable $i$ takes values $1, 1+b, 1+2b, \dots, 1+kb$. The loop terminates when $1+kb > n \implies k \approx \frac{n}{b}$.
- **Rule:** An additive step size by a constant $b > 0$ runs $\frac{n}{b}$ times.
- **Time Complexity:** $O\left(\frac{n}{b}\right) = \mathbf{O(n)}$.

B. Multiplicative Increment (`i = i * b`)

```
for (i = 1; i <= n; i = i * b) {
    printf(i);
}
```

- **Iteration Count:** Assuming $b > 1$, the variable $i$ grows exponentially as a Geometric Progression: $1, b^1, b^2, b^3, \dots, b^k$.
- **Termination Condition:** The loop stops when $b^k > n$. Taking $\log_b$ on both sides: $$b^k = n \implies k = \log_b n$$
- **Time Complexity:** $\mathbf{O(\log_b n)}$.

C. Divisive Decrement (`j = j / b`)

```
j = n;
while (j > 0) {
    j = j / b;
}
```

- **Iteration Count:** Assuming $b > 1$, the variable $j$ shrinks exponentially: $n, \frac{n}{b}, \frac{n}{b^2}, \frac{n}{b^3}, \dots, \frac{n}{b^k}$.
- **Termination Condition:** The loop stops when $\frac{n}{b^k} < 1 \implies b^k = n \implies k = \log_b n$.
- **Time Complexity:** $\mathbf{O(\log_b n)}$.

---

2. SYNTAX TRAPS & COMPILER BEHAVIORS

Exam questions frequently use C language syntax subtleties and control-flow mechanisms to obscure loop execution counts.

---

Trap 1: The Semicolon Detachment Trap

A semicolon placed directly after a `for` or `while` loop header detaches the loop body from the control header.

```
int c = 0, i;
for (i = 1; i <= n; i++);
    c = c + i;
```

Execution Mechanism:

1. The `for (i = 1; i <= n; i++);` line represents an **empty loop**.
2. The empty loop executes $n$ times doing zero work in its body.
3. Upon loop termination, the index variable holds the value $i = n + 1$.
4. The statement `c = c + i;` lies **outside** the loop and executes **exactly once**.
5. Final value of `c`: $$c = 0 + (n + 1) = n + 1$$
6. **Time Complexity:** $\mathbf{O(n)}$ (due to the $n$ condition checks in the empty header).

---

Trap 2: Assignment vs. Comparison Trap

In C-style languages, `=` is an assignment operator, while `==` is an equality comparison operator.

```
i = 1, a = 0;
while (i = 2) {
    a = a + 1;
}
```

Execution Mechanism:

1. The expression `i = 2` assigns the value `2` to variable `i`.
2. In C, an assignment expression evaluates to the assigned value (`2`).
3. Since `2` is a non-zero integer, the condition evaluates to `True` unconditionally.
4. The loop body executes, incrementing `a`. On the next pass, `i = 2` evaluates to `True` again.
5. This creates an **infinite loop**.
6. **Time Complexity:** $\boldsymbol{\infty}$ (Non-terminating execution).

---

Trap 3: Immediate Break Statements

A `break` statement inside a loop immediately terminates the innermost enclosing loop.

```
for (i = 1; i <= n; i++) {
    a = a + 3;
    break;
}
```

Execution Mechanism:

1. At $i = 1$, `a` is incremented by 3.
2. The `break;` statement executes immediately.
3. The loop terminates on its very first iteration, ignoring the boundary condition $i \le n$.
4. **Time Complexity:** $\mathbf{O(1)}$.

---

Trap 4: Data-Dependent Break (Prime Checker Case)

```
Algo PrimeCheck(x, n) {
    for (i = 2; i <= n; i++) {
        if (x % i == 0) {
            break;
        }
    }
}
```

Execution Mechanism & Case Analysis:

- **Best Case Configuration:** $x$ is an even number (e.g., $x = 10$).
    - At $i = 2$, $10 \% 2 == 0$ evaluates to `True`.
    - The `break` statement executes immediately on iteration 1.
    - **Best Case Complexity:** $\mathbf{\Omega(1)}$.
- **Worst Case Configuration:** $x$ is a prime number strictly greater than $n$ (e.g., $n = 10, x = 13$).
    - The condition `x % i == 0` is `False` for all $i \in \{2, 3, \dots, n\}$.
    - The loop never breaks early and executes all $n - 1$ iterations.
    - **Worst Case Complexity:** $\mathbf{O(n)}$.

---

3. NESTED LOOPS: INDEPENDENT VS. DEPENDENT

When loops are nested inside one another, they fall into two distinct structural categories.

```
                           Nested Loop Types
                                   │
              ┌────────────────────┴────────────────────┐
              ▼                                         ▼
   Nested Independent Loops                  Nested Dependent Loops
(Inner bounds ignore outer 'i')            (Inner bounds depend on outer 'i')
  - TC = Outer TC × Inner TC                 - TC = Evaluate Summation Series
```

---

3.1 Nested Independent Loops

If the inner loop's start, end, and step parameters do not contain the outer loop variable $i$, the loops are **independent**.

The Multiplicative Rule

For nested independent loops, compute the total complexity by multiplying the time complexity of the outer loop by the time complexity of the inner loop:

$$\text{Total Time Complexity} = \text{TC}(\text{Outer Loop}) \times \text{TC}(\text{Inner Loop})$$

```
a = 0;
for (i = 1; i <= n; i++) {       // Outer loop runs n times -> O(n)
    for (j = 1; j <= n; j++) {   // Inner loop runs n times -> O(n)
        a = a + 1;               // O(1) statement
    }
}
```

$$\text{Total TC} = O(n) \times O(n) = \mathbf{O(n^2)}$$

---

Mutually Exclusive (Sequential) Loops Rule

When loops are written sequentially (one after another, not inside each other), add their complexities:

```
for (i = 1; i <= n; i++) { printf(i); }    // Loop 1 -> O(n)
for (j = 1; j <= m; j++) { printf(j); }    // Loop 2 -> O(m)
```

$$\text{Total TC} = O(n) + O(m) = O(n + m) = \mathbf{O(\max(n, m))}$$

---

Fractional Step Inner Loops

When an inner loop's boundaries use fractional steps proportional to $n$:

```
for (i = 1; i <= n; i++) {
    for (j = 1; j <= n; j++) {
        for (k = n/2; k <= n; k = k + n/2) {
            printf("AJ");
        }
    }
}
```

- **Innermost Loop (**$k$**):** The variable $k$ starts at $n/2$, increments by $n/2$, and terminates at $n$. It takes values $n/2$ and $n$, running **exactly 2 times** regardless of input size $n \implies O(1)$.
- **Outer Loops (**$i, j$**):** Both run $n$ times.
- **Total TC:** $O(n) \times O(n) \times O(1) = \mathbf{O(n^2)}$.

---

3.2 Nested Dependent Loops

When the inner loop's initialization or termination condition depends on the outer loop index variable $i$, you must evaluate the total executions using **mathematical summation series**.

```
c = 0;
for (i = 1; i <= n; i++) {
    for (j = i; j <= n; j++) {
        c = c + 1;
    }
}
```

Step-by-Step Summation Derivation:

- When $i = 1$, inner loop $j$ goes from $1$ to $n \implies n$ executions.
- When $i = 2$, inner loop $j$ goes from $2$ to $n \implies n - 1$ executions.
- When $i = 3$, inner loop $j$ goes from $3$ to $n \implies n - 2$ executions.
- When $i = n$, inner loop $j$ goes from $n$ to $n \implies 1$ execution.

Summing all iterations: $$S = n + (n - 1) + (n - 2) + \dots + 1 = \sum_{i=1}^n (n - i + 1) = \frac{n(n + 1)}{2} = \frac{n^2 + n}{2}$$

$$\text{Total Time Complexity} = \mathbf{O(n^2)}$$

---

3.3 Dynamic Shrink Series (Square Root Pattern)

Consider a loop where the control variable grows non-linearly by accumulating another variable:

```
a = 1, b = 1;
while (a <= n) {
    b = b + 1;
    a = a + b;
}
```

Step-by-Step Execution Trace:

Let $k$ represent the iteration number:

- **Start:** $a = 1, b = 1$
- **Iteration 1 (**$k=1$**):** $b = 1 + 1 = 2$, $a = 1 + 2 = 3$
- **Iteration 2 (**$k=2$**):** $b = 2 + 1 = 3$, $a = 3 + 3 = 6$
- **Iteration 3 (**$k=3$**):** $b = 3 + 1 = 4$, $a = 6 + 4 = 10$
- **Iteration** $k$**:** $b = k + 1$, and $a$ accumulates the sum of consecutive integers: $$a = 1 + 2 + 3 + \dots + (k + 1) = \frac{(k + 1)(k + 2)}{2} \approx \frac{k^2}{2}$$

The `while` loop terminates when $a > n$. Substituting our expression for $a$: $$\frac{k^2}{2} > n \implies k^2 > 2n \implies k \approx \sqrt{2n}$$

Since the loop executes $k$ times: $$\text{Time Complexity} = \mathbf{O(\sqrt{n})}$$

---

4. COMPLETE SOLVED PROBLEM SET

---

Problem 1: Value Tracking in Additive Loop (Simple)

Find the Time Complexity and the final value of variable `a` after the code terminates:

```
a = 0;
for (i = 1; i <= n; i++) {
    a = a + 5;
}
```

Step-by-Step Solution:

1. **Loop Executions:** The loop starts at $i = 1$, increments by 1, and stops when $i > n$. It runs exactly $n$ times.
2. **Work Per Iteration:** Constant time addition $a = a + 5 \implies O(1)$.
3. **Time Complexity:** $n \times O(1) = \mathbf{O(n)}$.
4. **Final Value of** **a****:** Variable `a` starts at 0 and adds 5 on every pass: $$a = \underbrace{5 + 5 + 5 + \dots + 5}_{n \text{ times}} = 5n$$

**Answer:** **Time Complexity =** $O(n)$**, Final Value of** **a** **=** $5n$.

---

Problem 2: Value Tracking in Additive Loop (Summation)

Find the Time Complexity and the output of variable `a`:

```
a = 0;
for (i = 1; i <= n; i++) {
    a = a + i;
}
print(a);
```

Step-by-Step Solution:

1. **Loop Executions:** The loop runs $n$ times doing $O(1)$ work per pass. Thus, **Time Complexity =** $O(n)$.
2. **Output Value of** **a****:**
    - At $i = 1 \implies a = 0 + 1 = 1$
    - At $i = 2 \implies a = 1 + 2 = 3$
    - At $i = 3 \implies a = 1 + 2 + 3 = 6$
    - At $i = n \implies a = 1 + 2 + 3 + \dots + n$
3. Apply the sum of first $n$ natural numbers formula: $$a = \frac{n(n + 1)}{2}$$

**Answer:** **Time Complexity =** $O(n)$**, Output =** $\frac{n(n+1)}{2}$.

---

Problem 3: Loop with Fractional Step Bound

Find the Time Complexity of:

```
for (i = 1; i <= n/2; i++) {
    a = a + 10;
}
```

Step-by-Step Solution:

1. The variable $i$ starts at 1 and increments by 1 up to $n/2$.
2. Total iterations = $n/2$.
3. Drop constant divisor $1/2$: $$O(n/2) = \mathbf{O(n)}$$

---

Problem 4: Generalized Additive Loop

Find the Time Complexity of the loop for any constant step size $b > 0$:

```
for (i = 1; i <= n; i = i + b) {
    printf(i);
}
```

Step-by-Step Solution:

1. Total loop iterations = $\lfloor n/b \rfloor$.
2. Since $b$ is a constant scalar, dropping $1/b$ gives: $$O\left(\frac{n}{b}\right) = \mathbf{O(n)}$$

---

Problem 5: Multiplicative Step Loop (Base 2)

Find the Time Complexity of:

```
for (i = 1; i <= n; i = i * 2) {
    printf(i);
}
```

Step-by-Step Solution:

1. Trace values of $i$: $1, 2, 4, 8, 16, \dots, 2^k$.
2. Let the loop run for $k$ iterations.
3. At termination: $2^k = n \implies k = \log_2 n$.

**Answer:** $O(\log_2 n)$.

---

Problem 6: Multiplicative Step Loop (Base 5)

Find the Time Complexity of:

```
for (i = 1; i <= n; i = i * 5) {
    printf(i);
}
```

Step-by-Step Solution:

1. Trace values of $i$: $1, 5^1, 5^2, 5^3, \dots, 5^k$.
2. At the $k$-th iteration: $5^k = n \implies k = \log_5 n$.

**Answer:** $O(\log_5 n)$.

---

Problem 7: Divisive Step Decrement Loop

Find the Time Complexity of:

```
j = n;
while (j > 0) {
    j = j / 5;
    printf(j);
}
```

Step-by-Step Solution:

1. Trace values of $j$: $n, \frac{n}{5}, \frac{n}{5^2}, \frac{n}{5^3}, \dots, \frac{n}{5^k}$.
2. At the final iteration, the condition fails when $\frac{n}{5^k} = 1 \implies 5^k = n \implies k = \log_5 n$.

**Answer:** $O(\log_5 n)$.

---

Problem 8: Semicolon Detachment Program

Find the Time Complexity and the exact final value of variable `c`:

```
int c = 0, i;
for (i = 1; i <= n; i++);
c = c + i;
```

Step-by-Step Solution:

1. The semicolon `;` after the `for` header makes the loop body empty.
2. The empty header executes $n$ times, incrementing $i$ from 1 to $n+1$.
3. When $i = n + 1$, condition $n + 1 \le n$ fails, and loop terminates.
4. Line `c = c + i;` executes once outside the loop: $$c = 0 + (n + 1) = n + 1$$

**Answer:** **Time Complexity =** $O(n)$**, Final Value of** **c** **=** $n + 1$.

---

Problem 9: C-Assignment in While Header

Determine the Time Complexity of the code segment:

```
i = 1, a = 0;
while (i = 2) {
    a = a + 1;
}
```

Step-by-Step Solution:

1. `i = 2` is an assignment, not a comparison (`i == 2`).
2. In C language, the expression returns value `2`, which evaluates to `True`.
3. The loop condition is permanently `True`, causing an infinite loop.

**Answer:** **Infinite Loop / Non-Terminating (**$\infty$**)**.

---

Problem 10: Loop with Early Break

Find the Time Complexity of:

```
for (i = 1; i <= n; i++) {
    a = a + 3;
    break;
}
```

Step-by-Step Solution:

1. On iteration $i = 1$, `a` is incremented.
2. The `break;` statement executes immediately, exiting the loop.
3. Total iterations = 1.

**Answer:** $O(1)$.

---

Problem 11: Data-Dependent Break Check

Find the Best Case and Worst Case time complexity of `Algo(x, n)`:

```
Algo(x, n) {
    for (i = 2; i <= n; i++) {
        if (x % i == 0) {
            break;
        }
    }
}
```

Step-by-Step Solution:

- **Best Case:** $x$ is an even number (e.g., $x = 4$). At $i = 2$, $4 \% 2 == 0$ is `True`, loop breaks in 1 pass $\implies \mathbf{\Omega(1)}$.
- **Worst Case:** $x$ is a prime number strictly greater than $n$ (e.g., $n = 10, x = 13$). Loop never breaks and checks all $i$ from 2 to $n \implies \mathbf{O(n)}$.

---

Problem 12: Sequential and Nested Loops Combination

Find the overall Time Complexity of `Algo(n)`:

```
Algo(n) {
    for (i = 1; i <= n; i++) { print(i); }
    for (j = 1; j <= n; j++) { print(i + j); }
    for (i = 1; i <= n; i++) {
        for (j = 1; j <= n/5; j++) {
            for (k = 1; k <= n; k++) {
                break;
            }
        }
    }
}
```

Step-by-Step Solution:

1. **Block 1 (****for i****):** Sequential loop running $n$ times $\implies O(n)$.
2. **Block 2 (****for j****):** Sequential loop running $n$ times $\implies O(n)$.
3. **Block 3 (Nested 3-level loop):**
    - Innermost $k$ loop hits `break` on $k = 1 \implies O(1)$ work per iteration.
    - Middle $j$ loop runs $n/5$ times.
    - Outer $i$ loop runs $n$ times.
    - Complexity of Block 3 = $n \times \frac{n}{5} \times 1 = O(n^2)$.
4. Combine sequential blocks using Mutually Exclusive rule: $$\text{Total TC} = O(n) + O(n) + O(n^2) = O(\max(n, n, n^2)) = \mathbf{O(n^2)}$$

---

Problem 13: Loop Executing External Function

Find the overall Time Complexity of `Algo(n)`, given that external function `ExternalFunc(n)` takes $O(n^2)$ time:

```
Algo(n) {
    for (j = 1; j <= n/2; j++) {
        printf(j);
    }
    i = 0;
    while (i <= n) {
        printf(i);
        i = i + 3;
    }
    ExternalFunc(n);
}
```

Step-by-Step Solution:

1. **First loop:** Runs $n/2$ times $\implies O(n)$.
2. **Second** **while** **loop:** Increments $i$ by 3, running $n/3$ times $\implies O(n)$.
3. **Function Call** **ExternalFunc(n)****:** Given as $O(n^2)$.
4. **Total Complexity:** $O(n) + O(n) + O(n^2) = \mathbf{O(n^2)}$.

---

Problem 14: Fractional Step Inner Loop

Find the Time Complexity of:

```
for (i = 1; i <= n; i++) {
    for (j = 1; j <= n; j++) {
        for (k = n/2; k <= n; k = k + n/2) {
            printf("AJ");
        }
    }
}
```

Step-by-Step Solution:

1. **Innermost** $k$ **loop:** Starts at $n/2$ and increments by $n/2$. Takes values $n/2$ and $n$, executing **exactly 2 times** $\implies O(1)$ work.
2. **Middle** $j$ **loop:** Runs $n$ times $\implies O(n)$.
3. **Outer** $i$ **loop:** Runs $n$ times $\implies O(n)$.
4. **Total TC:** $O(n) \times O(n) \times O(1) = \mathbf{O(n^2)}$.

---

Problem 15: Nested Dependent Loop Summation

Find the Time Complexity of:

```
c = 0;
for (i = 1; i <= n; i++) {
    for (j = i; j <= n; j++) {
        c = c + 1;
    }
}
```

Step-by-Step Solution:

1. Inner loop runs $n - i + 1$ times.
2. Total executions $= \sum_{i=1}^n (n - i + 1) = n + (n-1) + (n-2) + \dots + 1 = \frac{n(n+1)}{2}$.
3. Order of magnitude: $\mathbf{O(n^2)}$.

---

Problem 16: Exponential Upper-Bound Inner Loop

Given $m = 2^n$, find the Time Complexity and the final value of `c`:

```
c = 0;
for (i = 1; i <= n; i = i + 2) {
    for (j = 1; j <= m; j = j * 2) {
        c = c + 1;
    }
}
```

Step-by-Step Solution:

1. **Outer** $i$ **loop:** Increments by 2, running $n/2$ times.
2. **Inner** $j$ **loop:** Multiplies by 2, running $\log_2 m$ times.
3. Substitute $m = 2^n$: $$\text{Inner Iterations} = \log_2(2^n) = n \text{ times}$$
4. **Total Increment Count** **c****:** $$c = \text{Outer} \times \text{Inner} = \frac{n}{2} \times n = \frac{n^2}{2}$$
5. **Time Complexity:** $\mathbf{O(n^2)}$.

---

Problem 17: Multiplicative Loop Return Value

Determine the return value of `Algo(n)`:

```
int Algo(n) {
    int i, j, k = 0;
    for (i = n/2; i <= n; i = i + 1) {
        for (j = 2; j <= n; j = j * 2) {
            k = k + n/2;
        }
    }
    return (k);
}
```

Step-by-Step Solution:

1. **Outer** $i$ **loop:** Runs from $n/2$ to $n \implies (n - n/2 + 1) \approx \frac{n}{2}$ times.
2. **Inner** $j$ **loop:** Starts at 2 and doubles, running $\log_2 n$ times.
3. **Total Inner Passes:** $\frac{n}{2} \cdot \log_2 n$.
4. On each pass, $k$ adds $n/2$: $$k = \left(\frac{n}{2} \log_2 n\right) \times \frac{n}{2} = \frac{n^2 \log_2 n}{4}$$
5. Order of magnitude of returned value: $\mathbf{O(n^2 \log n)}$.

---

Problem 18: Dynamic Shrink Series

Find the Time Complexity of:

```
a = 1, b = 1;
while (a <= n) {
    b = b + 1;
    a = a + b;
}
```

Step-by-Step Solution:

Trace values of $b$ and $a$ at iteration $k$:

- Iteration 1: $b = 2, a = 1 + 2 = 3$
- Iteration 2: $b = 3, a = 3 + 3 = 6$
- Iteration 3: $b = 4, a = 6 + 4 = 10$
- Iteration $k$: $a = 1 + 2 + 3 + \dots + (k+1) \approx \frac{k^2}{2}$

Loop terminates when $a > n \implies \frac{k^2}{2} > n \implies k \approx \sqrt{2n} \implies k = O(\sqrt{n})$.

**Answer:** $O(\sqrt{n})$.