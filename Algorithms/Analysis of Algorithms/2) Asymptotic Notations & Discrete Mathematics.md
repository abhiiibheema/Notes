CHAPTER 2: ASYMPTOTIC NOTATIONS & DISCRETE MATHEMATICS

---

1. PROGRESSIVE INTUITION: ASYMPTOTIC ENVELOPES

When evaluating the performance of an algorithm, physical execution times in seconds or memory in kilobytes vary across different hardware and software environments. Instead, asymptotic analysis evaluates how resource consumption scales mathematically as the input size approaches infinity ($n \to \infty$).

To describe this scaling behavior, **Asymptotic Notations** define the upper, lower, and tight growth limits of functions.

```
                     Asymptotic Notations
                              │
            ┌─────────────────┴─────────────────┐
            ▼                                   ▼
      Big Notations                      Small Notations
 (Tight or Loose Bounds)             (Strictly Loose Bounds)
   - Big-Oh (O)                        - Small-oh (o)
   - Big-Omega (Ω)                     - Small-omega (ω)
   - Theta (θ)
```

Loose vs. Tight Bounds

When bounding a function $f(n)$ representing an algorithm's running time:

- An **Upper Bound** represents the maximum rate of growth (a ceiling).
- A **Lower Bound** represents the minimum rate of growth (a floor).
- A bound can be **Tight** (closest possible match to the function) or **Loose** (a valid upper or lower limit that overestimates or underestimates the growth rate).

Exemplary Bound Classifications

Consider the function $f(n) = n + 10$:

- **Tight Upper Bound:** $O(n)$ — matches the exact linear rate of growth.
- **Loose Upper Bounds:** $O(n^2), O(n^3)$ — mathematically valid ceilings that overestimate growth.
- **Tight Lower Bound:** $\Omega(n)$ — matches the exact linear rate of growth.
- **Loose Lower Bounds:** $\Omega(\sqrt{n}), \Omega(1)$ — mathematically valid floors that underestimate growth.

---

2. FORMAL MATHEMATICAL DEFINITIONS

Asymptotic notations are divided into **Big Notations** (which can represent tight or loose bounds) and **Small Notations** (which strictly represent loose bounds).

---

2.1 Big-Oh Notation ($O$): Asymptotic Upper Bound

Big-Oh defines an asymptotic upper bound. It guarantees that a function $f(n)$ will not grow faster than a constant multiple of $g(n)$ for sufficiently large $n$.

Mathematical Definition

Given non-negative functions $f(n)$ and $g(n)$, we write: $$f(n) = O(g(n))$$ if and only if there exist positive constants $c > 0$ and $n_0 \ge 0$ such that: $$f(n) \le c \cdot g(n) \quad \text{whenever } n \ge n_0$$

- **Interpretation:** Beyond the threshold input size $n_0$, $f(n)$ is trapped beneath $c \cdot g(n)$.
- **Inequality Analogy:** $f(n) \le g(n)$.

---

2.2 Big-Omega Notation ($\Omega$): Asymptotic Lower Bound

Big-Omega defines an asymptotic lower bound. It guarantees that a function $f(n)$ will grow at least as fast as a constant multiple of $g(n)$ for sufficiently large $n$.

Mathematical Definition

Given non-negative functions $f(n)$ and $g(n)$, we write: $$f(n) = \Omega(g(n))$$ if and only if there exist positive constants $c > 0$ and $n_0 \ge 0$ such that: $$f(n) \ge c \cdot g(n) \quad \text{whenever } n \ge n_0$$

- **Interpretation:** Beyond the threshold input size $n_0$, $f(n)$ lies above $c \cdot g(n)$.
- **Inequality Analogy:** $f(n) \ge g(n)$.

---

2.3 Theta Notation ($\Theta$): Asymptotic Tight Bound

Theta defines an exact, tight bound. It sandwiches $f(n)$ from both above and below using the same rate of growth $g(n)$.

Mathematical Definition

Given non-negative functions $f(n)$ and $g(n)$, we write: $$f(n) = \Theta(g(n))$$ if and only if $f(n) = O(g(n))$ and $f(n) = \Omega(g(n))$. Formally, there exist positive constants $c_1 > 0$, $c_2 > 0$, and $n_0 \ge 0$ such that: $$c_1 \cdot g(n) \le f(n) \le c_2 \cdot g(n) \quad \text{whenever } n \ge n_0$$

- **Inequality Analogy:** $f(n) = g(n)$.

---

2.4 Small-oh Notation ($o$): Strict Loose Upper Bound

Small-oh defines a strict, proper upper bound. It indicates that $f(n)$ grows strictly slower than $g(n)$ as $n \to \infty$.

Mathematical Definition

We write $f(n) = o(g(n))$ if and only if, for **every** positive constant $c > 0$, there exists a threshold $n_0 \ge 0$ such that: $$f(n) < c \cdot g(n) \quad \text{whenever } n \ge n_0$$

The Limit Test

$$f(n) = o(g(n)) \iff \lim_{n \to \infty} \frac{f(n)}{g(n)} = 0$$

- **Key Distinction:** Every small-oh $o$ is also Big-Oh $O$, but Big-Oh $O$ may or may not be small-oh $o$. For example, $n \le 3n \implies n = O(n)$, but $n < 1 \cdot n$ fails for $c = 1$, so $n \neq o(n)$. For $f(n) = n^2 + n + 5$, $O(n^2)$ is the tight bound, $O(n^3)$ is a loose upper bound, and $o(n^3)$ is a strict loose upper bound, whereas $o(n^2)$ is false.
- **Inequality Analogy:** $f(n) < g(n)$.

---

2.5 Small-omega Notation ($\omega$): Strict Loose Lower Bound

Small-omega defines a strict, proper lower bound. It indicates that $f(n)$ grows strictly faster than $g(n)$ as $n \to \infty$.

Mathematical Definition

We write $f(n) = \omega(g(n))$ if and only if, for **every** positive constant $c > 0$, there exists a threshold $n_0 \ge 0$ such that: $$f(n) > c \cdot g(n) \quad \text{whenever } n \ge n_0$$

The Limit Test

$$f(n) = \omega(g(n)) \iff \lim_{n \to \infty} \frac{f(n)}{g(n)} = \infty$$

- **Key Distinction:** Every small-omega $\omega$ is also Big-Omega $\Omega$, but Big-Omega $\Omega$ may or may not be small-omega $\omega$. For example, for $f(n) = n$: $f(n) = \Omega(n)$ and $\Omega(\sqrt{n})$ are true; $\omega(\sqrt{n})$ and $\omega(1)$ are true, but $\omega(n)$ is false.
- **Inequality Analogy:** $f(n) > g(n)$.

---

3. DISCRETE RELATIONAL PROPERTIES GRID

The five asymptotic notations satisfy fundamental set-theoretic relational properties:

|Notation|Reflexive ($f = R(f)$)|Symmetric ($f=R(g) \implies g=R(f)$)|Transitive ($f=R(g) \land g=R(h) \implies f=R(h)$)|Transpose Symmetry ($f=R(g) \iff g=R'(f)$)|
|---|---|---|---|---|
|**Big-Oh (**$O$**)**|$\checkmark$|$\times$|$\checkmark$|$\checkmark$ (with $\Omega$)|
|**Big-Omega (**$\Omega$**)**|$\checkmark$|$\times$|$\checkmark$|$\checkmark$ (with $O$)|
|**Theta (**$\Theta$**)**|$\checkmark$|$\checkmark$|$\checkmark$|$\times$|
|**Small-oh (**$o$**)**|$\times$|$\times$|$\checkmark$|$\checkmark$ (with $\omega$)|
|**Small-omega (**$\omega$**)**|$\times$|$\times$|$\checkmark$|$\checkmark$ (with $o$)|

Formal Property Mappings

Let $a$ and $b$ represent the degrees/rates of growth of functions $f(n)$ and $g(n)$ respectively:

1. $f(n) = O(g(n)) \implies a \le b$.
2. $f(n) = \Omega(g(n)) \implies a \ge b$.
3. $f(n) = \Theta(g(n)) \implies a = b$.
4. $f(n) = o(g(n)) \implies a < b$.
5. $f(n) = \omega(g(n)) \implies a > b$.

---

Algebraic Composition Rules

1. **Constant Multiplier Rule:** For any constant $a > 0$, if $d(n) = O(f(n))$, then $a \cdot d(n) = O(f(n))$.
    - _Example:_ $d(n) = n^2 \implies O(n^2)$. Multiplying by $a = 10$ yields $10n^2 = O(n^2)$.
2. **Summation Rule:** When adding two functions, the total complexity is dictated by the dominating term: $$O(f(n)) + O(g(n)) = O(\max(f(n), g(n)))$$
    - _Example:_ $d(n) = 5n + 2 \implies O(n)$ and $e(n) = 10n^2 + 7n + 9 \implies O(n^2)$. $$d(n) + e(n) = 10n^2 + 12n + 11 = O(n^2) = O(\max(n, n^2))$$
3. **Multiplication Rule:** When multiplying two functions (such as nested operations): $$O(f(n)) \cdot O(g(n)) = O(f(n) \cdot g(n))$$ $$\Omega(f(n)) \cdot \Omega(g(n)) = \Omega(f(n) \cdot g(n))$$ $$\Theta(f(n)) \cdot \Theta(g(n)) = \Theta(f(n) \cdot g(n))$$
    - _Example:_ $d(n) = 5n \implies O(n)$ and $e(n) = 10n^2 + 2 \implies O(n^2)$. $$d(n) \cdot e(n) = 50n^3 + 10n = O(n^3) = O(n \cdot n^2)$$
4. **Transitivity Property:** If $d(n) = O(f(n))$ and $f(n) = O(g(n))$, then $d(n) = O(g(n))$.
    - _Example:_ $d(n) = O(n^2)$ and $f(n) = O(n^4) \implies d(n) = O(n^4)$.

---

4. TRICHOTOMY BREAKDOWN & OSCILLATING FUNCTIONS

The Trichotomy Property in Real Numbers

For any two fixed real numbers $x$ and $y$, exactly one of the following three relations must hold:

1. $x = y$
2. $x > y$
3. $x < y$

The Algorithmic Exception (Sinusoidal Counterexample)

Asymptotic function comparisons **do not** always follow the Trichotomy Property. It is possible to construct two functions $f(n)$ and $g(n)$ such that **none** of the asymptotic relationships ($O, \Omega, \Theta, o, \omega$) hold between them.

Counterexample

Consider the two functions: $$f(n) = n$$ $$g(n) = n^{1 + \sin n}$$

Because the trigonometric function $\sin n$ oscillates continuously in the range $[-1, +1]$ as $n \to \infty$:

1. **At the peaks (**$\sin n = +1$**):** $$g(n) = n^{1+1} = n^2 \implies f(n) = n \le n^2 \implies f(n) = O(g(n))$$
2. **At the troughs (**$\sin n = -1$**):** $$g(n) = n^{1-1} = n^0 = 1 \implies f(n) = n \ge 1 \implies f(n) = \Omega(g(n))$$

Because $f(n)$ and $g(n)$ cross each other infinitely many times as $n \to \infty$, no fixed threshold $n_0$ exists beyond which one function remains consistently greater than or equal to the other.

Thus, $f(n)$ **and** $g(n)$ **do not hold the Trichotomy Property**, proving that Trichotomy fails for asymptotic growth comparisons.

---

5. MASTER GROWTH RATE HIERARCHY

To quickly compare functions in competitive and theoretical problems, memorize this master order of growth (from slowest-growing / most efficient to fastest-growing / least efficient):

$$\frac{1}{n} < 100 < \log \log n < \log n < \sqrt{n} < n < n \log n < n^{1.5} < n^2 < n^k < n^{\log n} < 2^n < 3^n < e^n < n! < n^n$$

```
  Rate of Growth
   ▲
   │                                                 / nⁿ (Super-Exponential)
   │                                                /
   │                                               / n! (Factorial)
   │                                              /
   │                                             / 2ⁿ (Exponential)
   │                                            /
   │                                   _..---' / n² (Polynomial/Quadratic)
   │                             _..-'        /
   │                       _..-'             / n (Linear)
   │                 _..-'                  /
   │           _..-'                       / log n (Logarithmic)
   │     _..-'                            /
   │_..-'────────────────────────────────/────────► Input Size (n)
   │                                               Constant (e.g., 100)
```

Essential Comparison Rules

1. **Polynomial vs. Exponential:** Any exponential function with base $a > 1$ grows strictly faster than any polynomial $n^x$ for $x > 0$: $$n^x = O(a^n) \quad \text{for any fixed } x > 0 \text{ and } a > 1 \text{ (Poly } < \text{ Expo)}$$
2. **Logarithm of Powers:** $$f(n) = \log(n^x) = x \cdot \log(n) = O(\log n) \quad \text{for any fixed } x > 0$$
3. **Logarithmic vs. Polynomial:** Any positive polynomial power $n^y$ ($y > 0$) grows strictly faster than any logarithmic power $(\log n)^x$ ($x > 0$): $$(\log n)^x = O(n^y) \quad \text{for any fixed } x > 0 \text{ and } y > 0 \text{ (Log } < \text{ Poly)}$$

---

6. COMPLETE SOLVED PROBLEM SET

---

Problem 1: Basic Functional Asymptotics

Find the tight asymptotic bound ($\Theta$) for each of the following functions:

1. $f(n) = 8n$
2. $f(n) = 100(\log n) + 50\sqrt{n}$
3. $f(n) = 500\sqrt{n} + 2n + 100$

Step-by-Step Solutions

1. $f(n) = 8n \implies \mathbf{\Theta(n)}$ (dropping constant coefficient 8).
2. Compare component growth rates: $\log n < \sqrt{n} = n^{0.5}$. The dominating term is $50\sqrt{n}$, so $f(n) = \mathbf{\Theta(\sqrt{n})}$.
3. Compare component growth rates: $\sqrt{n} < n$. The dominating term is $2n$, so $f(n) = \mathbf{\Theta(n)}$.

---

Problem 2: Product Series & Factorial Analysis

Evaluate $f(n) = \prod_{i=1}^n (i) = 1 \times 2 \times 3 \dots \times n = n!$. Is $n! = \Omega(n^n)$?

Step-by-Step Solution

1. $f(n) = n!$ is upper-bounded by $n^n$, so $f(n) = O(n^n)$.
2. To check if $n! = \Omega(n^n)$, test if $n! \ge c \cdot n^n$ for some constant $c > 0$.
3. Since $n \cdot (n-1) \cdot (n-2) \dots 1 < n \cdot n \cdot n \dots n = n^n$, $n!$ grows strictly slower than $n^n$.
4. Therefore, $n! \neq \Omega(n^n)$. **Answer:** **False**.

---

Problem 3: Cubic Summation Bounds

Given $f(n) = \sum_{i=1}^n i^3 = x$. Select all valid choices for $x$:

Step-by-Step Solution

1. Apply the sum of cubes formula: $f(n) = 1^3 + 2^3 + 3^3 \dots + n^3 = \left[\frac{n(n+1)}{2}\right]^2 = \Theta(n^4)$.
2. Since the tight bound is $\Theta(n^4)$:
    - $O(n^4)$ is a valid upper bound.
    - $O(n^5)$ is a valid loose upper bound.
    - $\Omega(n^4)$ is a valid lower bound.
    - $\Omega(n^3)$ is a valid loose lower bound.

---

Problem 4: Multi-Function Growth Ordering (5 Functions)

Arrange the functions $10, \sqrt{n}, n, \log_2 n, \frac{100}{n}$ in increasing order of asymptotic complexity:

Step-by-Step Solution

1. As $n \to \infty$, $\frac{100}{n} \to 0$ (slowest).
2. $10$ is a constant ($n^0$).
3. $\log_2 n$ is logarithmic.
4. $\sqrt{n} = n^{0.5}$ is sub-linear polynomial.
5. $n$ is linear polynomial.

**Correct Increasing Order:** $\mathbf{\frac{100}{n} < 10 < \log_2 n < \sqrt{n} < n}$.

---

Problem 5: Comprehensive True / False Set

Evaluate whether each of the following statements is True or False:

1. If $0 < x < y$, then $n^x = O(n^y)$.
2. $\log(n)$ is $\Omega(1/n)$.
3. $2^{n^2}$ is $O(n!)$.
4. $20 \cdot n \cdot \log n = O(n \log n)$.
5. $(n+c)^k \neq O(n^k)$ for some $k > 0, c > 0$.
6. $n^2$ is $O(2^{\log n})$.
7. $n^2 = O(2^{2 \log n})$.
8. $(\log n)^{1/2} = O(\log(\log n))$.
9. $a^n \neq O(n^x)$ for $a > 1, x > 0$.

Step-by-Step Solutions

1. For $x < y$ (e.g., $2 < 3$), $n^2 < n^3 \implies n^2 = O(n^3)$. **TRUE**.
2. $\log n \ge c \cdot \frac{1}{n}$ holds for all large $n$. **TRUE**.
3. Compare $2^{n^2}$ and $n! \approx n^n$. Taking logs gives $n^2$ vs $n \log n$. Since $n^2 > n \log n$, $2^{n^2} > n!$, so it cannot be upper-bounded by $n!$. **FALSE**.
4. Dropping constant coefficient 20 yields $n \log n = O(n \log n)$. **TRUE**.
5. $(n+c)^k = \Theta(n^k) = O(n^k)$. The statement claiming it is $\neq$ is **FALSE**.
6. Simplify $2^{\log_2 n} = n$. Since $n^2 > n$, $n^2 \neq O(n)$. **FALSE**.
7. Simplify $2^{2 \log_2 n} = 2^{\log_2 (n^2)} = n^2$. Since $n^2 \le 1 \cdot n^2$, $n^2 = O(n^2)$. **TRUE**.
8. Let $x = \log n \implies x^{1/2} > \log x$. Since $x^{0.5}$ grows faster than $\log x$, $(\log n)^{1/2} \neq O(\log(\log n))$. **FALSE**.
9. Exponential functions ($a^n$) grow strictly faster than polynomial functions ($n^x$). Therefore, $a^n$ cannot be upper-bounded by $n^x$. **TRUE**.

---

Problem 6: Function Transformation Relations

Let $f(n)$ and $g(n)$ be functions such that $f(n) = O(g(n))$. Evaluate if: (a) $f(n) = O(f(n)^2)$ (b) $2^{f(n)} = O(2^{g(n)})$

Step-by-Step Solution

1. **Test (a):** Let $f(n) = 1/n$. Then $f(n)^2 = 1/n^2$. Since $1/n > 1/n^2$, $f(n) \neq O(f(n)^2)$. Thus (a) is **FALSE** in general.
2. **Test (b):** Let $f(n) = 2n$ and $g(n) = n$. Here $f(n) = O(g(n))$ holds ($2n = O(n)$). However, $2^{f(n)} = 2^{2n} = 4^n$ and $2^{g(n)} = 2^n$. Since $4^n \neq O(2^n)$, (b) is **FALSE** in general.
3. **Result:** **Both (a) and (b) are false**.

---

Problem 7: Universal Polynomial Square Identity

Which one of the following statements is **TRUE** for all positive functions $f(n)$?

- (A) $f(n^2) = \Theta(f(n)^2)$, when $f(n)$ is a polynomial
- (B) $f(n^2) = O(f(n)^2)$
- (C) $f(n^2) = O(f(n)^2)$, when $f(n)$ is an exponential function
- (D) $f(n^2) = \Omega(f(n)^2)$

Step-by-Step Solution

1. **Test Polynomials (A):** Let $f(n) = n^k$. Then $f(n^2) = (n^2)^k = n^{2k}$, and $f(n)^2 = (n^k)^2 = n^{2k}$. Since LHS = RHS, $f(n^2) = \Theta(f(n)^2)$ holds for all polynomials. **Option A is TRUE**.
2. **Disprove (B) & (C):** Let $f(n) = 2^n$. Then $f(n^2) = 2^{n^2}$ and $f(n)^2 = 2^{2n}$. Since $2^{n^2} \gg 2^{2n}$, $2^{n^2} \neq O(2^{2n})$. This disproves B and C.
3. **Disprove (D):** Let $f(n) = \log n$. Then $f(n^2) = \log(n^2) = 2 \log n$, while $f(n)^2 = (\log n)^2$. Since $2 \log n < (\log n)^2$, $f(n^2) \neq \Omega(f(n)^2)$. This disproves D.

**Answer:** **A**.

---

Problem 8: Two-Function Comparisons

1. Compare $f(n) = 2^n$ and $g(n) = n^n$.
2. Compare $f(n) = n$ and $g(n) = n \log n$.
3. Compare $f(n) = n^2 \log n$ and $g(n) = n(\log n)^{10}$.

Step-by-Step Solutions

1. For $n > 2$, $2^n < n^n \implies f(n) \le g(n) \implies f(n) = O(g(n))$.
2. $n < n \log n \implies f(n) < g(n) \implies f = O(g)$ and $f = o(g)$.
3. Divide $f(n)$ by $g(n)$: $\frac{n^2 \log n}{n (\log n)^{10}} = \frac{n}{(\log n)^9} \to \infty$ as $n \to \infty$. Thus $f > g \implies f = \Omega(g)$ and $f = \omega(g)$.

---

Problem 9: Piecewise Function Upper Bound

Consider the piecewise functions: $$f(n) = \begin{cases} n^3 & 0 < n \le 10,000 \\ n & n > 10,000 \end{cases}$$ $$g(n) = \begin{cases} n & 0 < n \le 100 \\ n^3 & n > 100 \end{cases}$$

Which options are TRUE?

- (A) $f(n) = \Omega(g(n))$ for $n > 100$
- (B) $f(n) = o(g(n))$ for $n > 100$
- (C) $f(n) = O(g(n))$ for $n > 100$
- (D) $f(n) = O(g(n))$ for $n > 10,000$

Step-by-Step Solution

1. For $100 < n \le 10,000$: $f(n) = n^3$ and $g(n) = n^3 \implies f(n) = g(n)$.
2. For $n > 10,000$: $f(n) = n$ and $g(n) = n^3 \implies f(n) < g(n)$.
3. Combining all $n > 100$, $f(n) \le g(n)$ holds consistently, satisfying $f(n) = O(g(n))$. Thus **Statement C is TRUE**.
4. For $n > 10,000$, $f(n) = n < n^3 = g(n)$ also satisfies $f(n) = O(g(n))$. Thus **Statement D is TRUE**.

**Answer:** **C and D**.

---

Problem 10: Growth Rate Ordering Sets

1. Arrange $f_1 = 2^n, f_2 = n^{3/2}, f_3 = n \log n, f_4 = n^{\log n}$ in increasing order of growth rate.
2. Arrange $f_1 = n \log n, f_2 = n^2, f_3 = e^n, f_4 = n^{3/2}, f_5 = n, f_6 = 1/n, f_7 = 2^n$ in increasing order of growth rate.
3. Arrange $f_1 = n^{1/3}, f_2 = n, f_3 = n^{7/4}, f_4 = (2.002)^n, f_5 = e^n$ in decreasing order of growth rate.

Step-by-Step Solutions

1. $f_3 = n \log n < f_2 = n^{1.5} < f_4 = n^{\log n} = 2^{(\log n)^2} < f_1 = 2^n$.
    - **Increasing Order:** $\mathbf{f_3 < f_2 < f_4 < f_1}$.
2. $f_6 (1/n) < f_5 (n) < f_1 (n \log n) < f_4 (n^{1.5}) < f_2 (n^2) < f_7 (2^n) < f_3 (e^n)$.
    - **Increasing Order:** $\mathbf{f_6 < f_5 < f_1 < f_4 < f_2 < f_7 < f_3}$.
3. Compare exponentials: $e^n \approx (2.718)^n > (2.002)^n \implies f_5 > f_4$. Compare polynomials: $n^{1.75} > n^1 > n^{0.33} \implies f_3 > f_2 > f_1$.
    - **Decreasing Order:** $\mathbf{f_5 > f_4 > f_3 > f_2 > f_1}$.