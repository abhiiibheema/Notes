**Part 1: Continuous Random Variables & Calculus-Based Statistics**

---

**1. Continuous Random Variable (CRV) & Probability Density Function (PDF)**

- **Definition**: A random variable $X$ is a **Continuous Random Variable** if its values are obtained through **measurement** across a continuous real interval rather than counting discrete objects.
- **Probability Density Function** $f(x)$: A function $f(x)$ represents a valid PDF for a CRV $X$ if the probability that $X$ falls within the interval $[a, b]$ is given by the integral: $$P(a \le X \le b) = \int_{a}^{b} f(x) \, dx$$
- **Fundamental Validity Conditions for a PDF**:
    1. Non-negativity: $f(x) \ge 0$ for all $x \in \mathbb{R}$.
    2. Total Probability Axiom: The area under the entire density curve must equal unity: $$\int_{-\infty}^{\infty} f(x) \, dx = 1$$

---

**2. Measures of Central Tendency & Spread for a CRV**

- **Arithmetic Mean / Expected Value** $E(X)$: $$\mu = E(X) = \int_{-\infty}^{\infty} x \cdot f(x) \, dx$$
    - **Expectation of a Function** $g(X)$: $$E[g(X)] = \int_{-\infty}^{\infty} g(x) \cdot f(x) \, dx$$
    - **Linear Properties of Expectation**:
        - $E(aX) = a E(X)$
        - $E(aX + b) = a E(X) + b$
        - $E[(X + c)^2] = E(X^2) + 2c E(X) + c^2$
- **Median (**$\alpha$**)**: The specific real value $x = \alpha$ that bisects the total probability density into two equal halves of area $0.5$: $$\int_{-\infty}^{\alpha} f(x) \, dx = \int_{\alpha}^{\infty} f(x) \, dx = 0.5$$
- **Mode (**$\beta$**)**: The real value $x = \beta$ where the probability density function $f(x)$ achieves its global maximum: $$f'(\beta) = 0 \quad \text{and} \quad f''(\beta) < 0$$
- **Variance (**$\sigma^2$**) & Standard Deviation (**$\sigma$**)**: $$\text{Var}(X) = \sigma^2 = E(X^2) - [E(X)]^2 = \int_{-\infty}^{\infty} x^2 f(x) \, dx - \mu^2$$ $$\text{Standard Deviation} = \sigma = \sqrt{\text{Var}(X)}$$

---

**3. Cumulative Distribution Function (CDF)**

- **Definition**: The Cumulative Distribution Function $F(x)$ measures the accumulated probability up to a threshold $x$: $$F(x) = P(X \le x) = \int_{-\infty}^{x} f(t) \, dt$$
- **Fundamental CDF Properties**:
    1. Boundary Values: $F(-\infty) = 0$ and $F(\infty) = 1$.
    2. Non-decreasing Nature: If $a \le b$, then $F(a) \le F(b)$.
    3. **Fundamental Theorem Relation**: The derivative of the CDF yields the original PDF: $$\frac{d}{dx} F(x) = f(x)$$

---

**Part 2: Fully Solved Continuous Distribution Problems**

---

**Problem 1: Normalizing Constant & Parameter Evaluation**

- **Question**: Find the value of constant $\lambda$ such that the function $f(x)$ is a valid PDF, and evaluate its Mode: $$f(x) = \begin{cases} \lambda(x-1)(2-x), & 1 \le x \le 2 \\ 0, & \text{otherwise} \end{cases}$$
- **Step-by-Step Solution**:
    1. **Enforce Total Probability Integral**: $$\int_{1}^{2} \lambda (x-1)(2-x) \, dx = 1 \implies \lambda \int_{1}^{2} (-x^2 + 3x - 2) \, dx = 1$$
    2. **Integrate**: $$\lambda \left[ -\frac{x^3}{3} + \frac{3x^2}{2} - 2x \right]_{1}^{2} = 1$$ $$\lambda \left[ \left( -\frac{8}{3} + 6 - 4 \right) - \left( -\frac{1}{3} + \frac{3}{2} - 2 \right) \right] = \lambda \left[ -\frac{2}{3} - \left(-\frac{5}{6}\right) \right] = \lambda \left( \frac{1}{6} \right) = 1 \implies \mathbf{\lambda = 6}$$
    3. **Evaluate Mode**: $$f(x) = 6(-x^2 + 3x - 2)$$ $$f'(x) = 6(-2x + 3) = 0 \implies -2x + 3 = 0 \implies \mathbf{x = 1.5}$$ $$f''(1.5) = -12 < 0 \implies \text{Mode} = \mathbf{1.5}$$

---

**Problem 2: Symmetric Density Interval Probability**

- **Question**: A random variable $X$ has PDF $f(x) = x^2$ for $-1 \le x \le 1$, and $0$ elsewhere. Calculate the percentage probability $P\left(-\frac{1}{3} \le X \le \frac{1}{3}\right)$.
- **Step-by-Step Solution**:
    1. Since $f(x) = x^2$ is an even function, exploit symmetry: $$P\left(-\frac{1}{3} \le X \le \frac{1}{3}\right) = \int_{-1/3}^{1/3} x^2 \, dx = 2 \int_{0}^{1/3} x^2 \, dx$$
    2. Integrate: $$2 \left[ \frac{x^3}{3} \right]_{0}^{1/3} = 2 \times \frac{1}{3 \times 27} = \frac{2}{81} \approx 0.024691 = \mathbf{2.47\%}$$

---

**Problem 3: Infinite Bilateral Density Function**

- **Question**: If $P_X(x) = M e^{-2|x|} + N e^{-3|x|}$ for $x \in (-\infty, \infty)$ is a valid PDF with $M, N > 0$, establish the equation relating $M$ and $N$.
- **Step-by-Step Solution**:
    1. Set total integral to 1: $$\int_{-\infty}^{\infty} \left( M e^{-2|x|} + N e^{-3|x|} \right) dx = 1$$
    2. Use symmetry about $x = 0$ for absolute value $|x|$: $$2 \int_{0}^{\infty} \left( M e^{-2x} + N e^{-3x} \right) dx = 1$$
    3. Evaluate standard exponential integrals: $$2 \left[ M \left( \frac{1}{2} \right) + N \left( \frac{1}{3} \right) \right] = 1 \implies \mathbf{M + \frac{2N}{3} = 1}$$

---

**Problem 4: Gamma Integral Expectation & Variance**

- **Question**: A continuous random variable $X \in [0, \infty)$ has PDF $f(x) = \frac{1}{2} e^{-x} x^2$ for $x \ge 0$. Find the Mean, Variance, and Standard Deviation.
- **Step-by-Step Solution**:
    1. **Recall Gamma Function Identity**: $\int_{0}^{\infty} e^{-x} x^{n-1} \, dx = \Gamma(n) = (n-1)!$
    2. **Calculate Mean** $\mu = E(X)$: $$E(X) = \int_{0}^{\infty} x \cdot \left( \frac{1}{2} e^{-x} x^2 \right) dx = \frac{1}{2} \int_{0}^{\infty} e^{-x} x^3 \, dx = \frac{1}{2} \Gamma(4) = \frac{1}{2} (3!) = \mathbf{3}$$
    3. **Calculate Second Moment** $E(X^2)$: $$E(X^2) = \int_{0}^{\infty} x^2 \cdot \left( \frac{1}{2} e^{-x} x^2 \right) dx = \frac{1}{2} \int_{0}^{\infty} e^{-x} x^4 \, dx = \frac{1}{2} \Gamma(5) = \frac{1}{2} (4!) = \mathbf{12}$$
    4. **Calculate Variance & Standard Deviation**: $$\text{Var}(X) = E(X^2) - [E(X)]^2 = 12 - (3)^2 = \mathbf{3}$$ $$\text{Standard Deviation } \sigma = \sqrt{3} \approx \mathbf{1.732}$$

---

**Problem 5: System of PDF Coefficients**

- **Question**: A continuous random variable $X$ has PDF $f(x) = a + bx$ for $0 < x < 1$, and $0$ otherwise. If $E(X) = \frac{2}{3}$, evaluate $P(X < 0.5)$.
- **Step-by-Step Solution**:
    1. **Condition 1 — Area under PDF**: $$\int_{0}^{1} (a + bx) \, dx = 1 \implies \left[ ax + \frac{bx^2}{2} \right]_{0}^{1} = 1 \implies a + \frac{b}{2} = 1 \implies \mathbf{2a + b = 2}$$
    2. **Condition 2 — Expectation**: $$E(X) = \int_{0}^{1} x(a + bx) \, dx = \int_{0}^{1} (ax + bx^2) \, dx = \left[ \frac{ax^2}{2} + \frac{bx^3}{3} \right]_{0}^{1} = \frac{a}{2} + \frac{b}{3} = \frac{2}{3} \implies \mathbf{3a + 2b = 4}$$
    3. **Solve Simultaneous Linear Equations**: Multiply $2a + b = 2$ by $2 \implies 4a + 2b = 4$. Subtract $(3a + 2b = 4) \implies a = 0$. Substitute $a = 0 \implies b = 2$. Hence, $f(x) = 2x$ for $0 < x < 1$.
    4. **Calculate** $P(X < 0.5)$: $$P(X < 0.5) = \int_{0}^{0.5} 2x \, dx = \left[ x^2 \right]_{0}^{0.5} = (0.5)^2 = \mathbf{0.25}$$

---

**Part 3: Discrete Random Variables & PMF Foundations**

---

**1. Discrete Random Variable (DRV) & Probability Mass Function (PMF)**

- **Definition**: A random variable $X$ is **Discrete** if its values are obtained by **counting** discrete occurrences, yielding a finite or countably infinite set of values $\{x_1, x_2, \dots, x_n\}$.
- **Probability Mass Function** $P(X = x_i)$: The function assigning discrete probabilities to specific points.
- **Fundamental Axioms of PMF**:
    1. $P(X = x_i) \ge 0$ for all $x_i$.
    2. $\sum_{i=1}^{n} P(X = x_i) = 1$.

---

**2. Summary Metrics for Discrete Distributions**

- **Discrete Expectation / Mean**: $$\mu = E(X) = \sum_{i=1}^{n} x_i \cdot P(X = x_i)$$
- **Discrete Variance**: $$\sigma^2 = \text{Var}(X) = E(X^2) - [E(X)]^2 = \sum_{i=1}^{n} x_i^2 \cdot P(X = x_i) - \mu^2$$

---

**Part 4: Standard Discrete Distributions: Binomial & Poisson**

---

**1. Binomial Distribution**

- **Framework**: Applies to a sequence of $n$ independent Bernoulli trials where each trial yields only two possible outcomes: **Success** (probability $p$) or **Failure** (probability $q = 1 - p$).
- **Probability Mass Function**: $$P(X = r) = {}^n C_r \cdot p^r \cdot q^{n-r} = \frac{n!}{(n-r)! \, r!} \cdot p^r \cdot (1-p)^{n-r}, \quad r = 0, 1, 2, \dots, n$$
- **Proof of Total Probability**: $$\sum_{r=0}^{n} P(X = r) = \sum_{r=0}^{n} {}^n C_r p^r q^{n-r} = (p + q)^n = (1)^n = 1$$
- **Mean Derivation**: $$\mu = E(X) = \sum_{r=0}^{n} r \cdot {}^n C_r p^r q^{n-r} = n p \sum_{r=1}^{n} {}^{n-1} C_{r-1} p^{r-1} q^{(n-1)-(r-1)} = np(p + q)^{n-1} = \mathbf{np}$$
- **Key Summary Parameters**:
    - $\text{Mean } \mu = np$
    - $\text{Variance } \sigma^2 = npq = np(1-p)$
    - $\text{Standard Deviation } \sigma = \sqrt{npq}$

---

**2. Poisson Distribution**

- **Framework**: Models the count of rare events occurring randomly across a fixed continuous interval of time or space at a constant average rate $\lambda$.
- **Probability Mass Function**: $$P(X = r) = \frac{e^{-\lambda} \cdot \lambda^r}{r!}, \quad r = 0, 1, 2, \dots, \infty$$
- **Proof of Total Probability**: $$\sum_{r=0}^{\infty} \frac{e^{-\lambda} \lambda^r}{r!} = e^{-\lambda} \sum_{r=0}^{\infty} \frac{\lambda^r}{r!} = e^{-\lambda} \cdot e^{\lambda} = 1$$
- **Limiting Form of Binomial Distribution**: As $n \to \infty$ and $p \to 0$ such that the product $np = \lambda$ remains a constant positive finite number, the Binomial distribution converges to the Poisson distribution.
- **Key Summary Parameters**:
    - $\text{Mean } \mu = \lambda$
    - $\text{Variance } \sigma^2 = \lambda$
    - $\text{Unique Identity}: \mathbf{\text{Mean} = \text{Variance} = \lambda}$

---

**Part 5: Fully Solved Discrete Distribution Problems**

---

**Problem 1: Discrete PMF Parameter & Variance Computation**

- **Question**: A discrete random variable $X$ takes values $1, 2, 3$ with probabilities $\frac{2+5P}{5}$, $\frac{1+3P}{5}$, and $\frac{1.5+2P}{5}$ respectively. Find the value of $P$, $E(X)$, and $\text{Var}(X)$.
- **Step-by-Step Solution**:
    1. **Enforce Sum of PMF Equals 1**: $$\frac{2+5P}{5} + \frac{1+3P}{5} + \frac{1.5+2P}{5} = 1 \implies \frac{4.5 + 10P}{5} = 1$$ $$4.5 + 10P = 5 \implies 10P = 0.5 \implies \mathbf{P = 0.05}$$
    2. **Calculate Numerical Probabilities**:
        - $P(X = 1) = \frac{2 + 5(0.05)}{5} = \frac{2.25}{5} = 0.45$
        - $P(X = 2) = \frac{1 + 3(0.05)}{5} = \frac{1.15}{5} = 0.23$
        - $P(X = 3) = \frac{1.5 + 2(0.05)}{5} = \frac{1.60}{5} = 0.32$
        - Check: $0.45 + 0.23 + 0.32 = 1.00$
    3. **Calculate Expected Value** $E(X)$: $$E(X) = 1(0.45) + 2(0.23) + 3(0.32) = 0.45 + 0.46 + 0.96 = \mathbf{1.87}$$
    4. **Calculate Second Moment** $E(X^2)$: $$E(X^2) = (1)^2(0.45) + (2)^2(0.23) + (3)^2(0.32) = 0.45 + 0.92 + 2.88 = 4.25$$
    5. **Calculate Variance**: $$\text{Var}(X) = E(X^2) - [E(X)]^2 = 4.25 - (1.87)^2 = 4.25 - 3.4969 = \mathbf{0.7531}$$

---

**Problem 2: Binomial Coin Flip Probability**

- **Question**: A fair coin is tossed 3 times. What is the probability of getting exactly 2 heads?
- **Step-by-Step Solution**:
    1. $n = 3$, $p = \frac{1}{2}$, $q = 1 - \frac{1}{2} = \frac{1}{2}$, $r = 2$.
    2. Apply Binomial PMF: $$P(X = 2) = {}^3 C_2 \left(\frac{1}{2}\right)^2 \left(\frac{1}{2}\right)^{3-2} = 3 \times \frac{1}{4} \times \frac{1}{2} = \mathbf{\frac{3}{8}} \quad (\text{or } 0.375)$$

---

**Problem 3: Poisson Distribution Interval Sum**

- **Question**: Let $X$ be a Poisson random variable with parameter $\lambda = 1$. Evaluate $P(2 \le X \le 4)$.
- **Step-by-Step Solution**:
    1. $P(2 \le X \le 4) = P(X = 2) + P(X = 3) + P(X = 4)$.
    2. Substitute $\lambda = 1$ into Poisson PMF $P(X = r) = \frac{e^{-1} \cdot 1^r}{r!}$: $$P(2 \le X \le 4) = e^{-1} \left( \frac{1}{2!} + \frac{1}{3!} + \frac{1}{4!} \right) = e^{-1} \left( \frac{1}{2} + \frac{1}{6} + \frac{1}{24} \right)$$
    3. Simplify fractional sum: $$\frac{12 + 4 + 1}{24} = \frac{17}{24}$$ $$P(2 \le X \le 4) = \frac{17}{24 e} \approx \frac{17}{24 \times 2.71828} \approx \mathbf{0.2606} \quad (\text{or } \approx 0.26)$$