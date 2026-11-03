**Part 1: Standard Continuous Probability Distributions**

---

**1. Normal / Gaussian Distribution**

- **Probability Density Function (PDF)**: A continuous random variable $X$ follows a Normal (or Gaussian) distribution with mean $\mu$ and variance $\sigma^2$ (denoted $X \sim N(\mu, \sigma^2)$) if its PDF is given by: $$f(x) = \frac{1}{\sigma \sqrt{2\pi}} \, e^{-\frac{(x - \mu)^2}{2\sigma^2}}, \qquad -\infty < x < \infty$$ where $\mu$ is the mean ($-\infty < \mu < \infty$) and $\sigma > 0$ is the standard deviation.
- **Key Mathematical Properties**:
    1. **Symmetry & Measures of Central Tendency**:
        - The density curve $f(x)$ is strictly symmetric about the vertical line $x = \mu$.
        - **Mean = Median = Mode =** $\mu$.
    2. **Total Area / Probability**: $$\int_{-\infty}^{\infty} f(x) \, dx = 1$$
    3. **Points of Inflection**:
        - The points where the curvature of the bell-shaped graph changes from concave down to concave up occur at: $$x = \mu - \sigma \quad \text{and} \quad x = \mu + \sigma$$
    4. **Empirical Area Rules (**$z$**-bounds)**:
        - Area within $1\sigma$: $P(\mu - \sigma \le X \le \mu + \sigma) = P(-1 \le Z \le 1) \approx 0.6828 \quad (68.28\%)$
        - Area within $2\sigma$: $P(\mu - 2\sigma \le X \le \mu + 2\sigma) = P(-2 \le Z \le 2) \approx 0.9545 \quad (95.45\%)$
        - Area within $3\sigma$: $P(\mu - 3\sigma \le X \le \mu + 3\sigma) = P(-3 \le Z \le 3) \approx 0.9973 \quad (99.73\%)$

---

**2. Standard Normal Distribution (**$Z$**-score Transformation)**

- **Standardization**: Any normal random variable $X \sim N(\mu, \sigma^2)$ can be transformed into a Standard Normal Random Variable $Z$ with mean $\mu_Z = 0$ and variance $\sigma_Z^2 = 1$ (denoted $Z \sim N(0, 1)$) using the $Z$-score transformation: $$Z = \frac{X - \mu}{\sigma}$$
- **Standard Normal PDF**: $$f(z) = \frac{1}{\sqrt{2\pi}} \, e^{-\frac{z^2}{2}}, \qquad -\infty < z < \infty$$
- **Cumulative Distribution Function (CDF)**:
    - The CDF curve $F(z)$ for the standard normal distribution exhibits an S-shape, bounded between $0$ at $z = -\infty$ and $1$ at $z = \infty$, with $F(0) = 0.5$.

---

**3. Continuous Exponential Distribution**

- **Probability Density Function**: A continuous random variable $X$ follows an Exponential distribution with rate parameter $\lambda > 0$ if its PDF is: $$f(x) = \begin{cases} \lambda e^{-\lambda x}, & x \ge 0 \\ 0, & x < 0 \end{cases}$$
- **Total Probability Integral Verification**: $$\int_{0}^{\infty} \lambda e^{-\lambda x} \, dx = \left[ -e^{-\lambda x} \right]_{0}^{\infty} = 0 - (-1) = 1$$
- **Summary Metrics**:
    - **Mean**: $E(X) = \frac{1}{\lambda}$
    - **Second Moment**: $E(X^2) = \frac{2}{\lambda^2}$
    - **Variance**: $\operatorname{Var}(X) = E(X^2) - [E(X)]^2 = \frac{2}{\lambda^2} - \frac{1}{\lambda^2} = \frac{1}{\lambda^2}$
    - **Standard Deviation**: $\sigma = \frac{1}{\lambda}$
    - **Key Identity**: $\mathbf{\text{Mean} = \text{Standard Deviation} = \frac{1}{\lambda}}$

---

**4. Continuous Uniform Distribution**

- **Probability Density Function**: A continuous random variable $X$ is uniformly distributed over a continuous interval $[a, b]$ (denoted $X \sim U[a, b]$) if its probability density is constant: $$f(x) = \begin{cases} \frac{1}{b - a}, & a \le x \le b \\ 0, & \text{otherwise} \end{cases}$$
- **Summary Metrics Derivations**:
    - **Mean**: $$E(X) = \int_{a}^{b} x \cdot \frac{1}{b - a} \, dx = \frac{1}{b - a} \left[ \frac{x^2}{2} \right]_{a}^{b} = \frac{b^2 - a^2}{2(b - a)} = \frac{a + b}{2}$$
    - **Second Moment**: $$E(X^2) = \int_{a}^{b} x^2 \cdot \frac{1}{b - a} \, dx = \frac{1}{b - a} \left[ \frac{x^3}{3} \right]_{a}^{b} = \frac{b^3 - a^3}{3(b - a)} = \frac{a^2 + ab + b^2}{3}$$
    - **Variance**: $$\operatorname{Var}(X) = \frac{a^2 + ab + b^2}{3} - \left( \frac{a + b}{2} \right)^2 = \frac{4(a^2 + ab + b^2) - 3(a^2 + 2ab + b^2)}{12} = \frac{(b - a)^2}{12}$$
    - **Standard Deviation**: $$\sigma = \frac{b - a}{\sqrt{12}}$$

---

**Part 2: Functions of Random Variables & Linear Combinations**

---

**1. Transformation of a Continuous Random Variable**

Given a continuous random variable $X$ with PDF $f_X(x)$, and a monotonic function $Y = g(X)$:

- **CDF Method**:
    1. Determine the cumulative distribution function of $Y$: $$F_Y(y) = P(Y \le y) = P(g(X) \le y) = P(X \le g^{-1}(y)) = \int_{-\infty}^{g^{-1}(y)} f_X(x) \, dx$$
    2. Compute the PDF of $Y$ by differentiating $F_Y(y)$: $$f_Y(y) = \frac{d}{dy} \left[ F_Y(y) \right]$$
- **Expectation of** $h(Y)$: $$E[h(Y)] = \int_{-\infty}^{\infty} h(g(x)) f_X(x) \, dx$$

---

**2. Important Combination Theorems**

1. **Bernoulli to Binomial**: If $X_1, X_2, \dots, X_n$ are $n$ independent and identically distributed (i.i.d.) Bernoulli random variables, then their sum $Y = \sum_{i=1}^n X_i$ follows a **Binomial Distribution** $B(n, p)$.
2. **Linear Combination of Independent Normals**: If $X_1, X_2, \dots, X_n$ are independent normal random variables where $X_i \sim N(\mu_i, \sigma_i^2)$, then any linear combination $Y = \sum_{i=1}^n c_i X_i$ is also **Normally Distributed**: $$Y \sim N\left( \sum c_i \mu_i, \sum c_i^2 \sigma_i^2 \right)$$
3. **Combination of Independent Exponentials**: If $X_1, X_2$ are independent exponential random variables, then $Y = a X_1 + b X_2$ follows a **Hypoexponential Distribution**.

---

**Part 3: Linear Regression Analysis & Least Squares**

---

**1. Least Squares Regression Method**

For $n$ bivariate observations $(x_1, y_1), (x_2, y_2), \dots, (x_n, y_n)$, we fit a straight line $y = ax + b$.

- **Residual / Error Deviation**: $$d_i = y_{i, \text{actual}} - y_{i, \text{calculated}} = y_i - (a x_i + b)$$
- **Objective Function**: Minimize the sum of squared residuals: $$f(a, b) = \sum_{i=1}^{n} d_i^2 = \sum_{i=1}^{n} (y_i - a x_i - b)^2$$
- **System of Normal Equations**: Setting partial derivatives $\frac{\partial f}{\partial a} = 0$ and $\frac{\partial f}{\partial b} = 0$ yields two linear equations in two unknowns ($a$ and $b$): $$\begin{cases} \sum y_i = a \sum x_i + n b \\ \sum x_i y_i = a \sum x_i^2 + b \sum x_i \end{cases}$$

---

**Part 4: Bivariate / Joint Distributions & Correlation**

---

**1. Joint Probability Density Functions**

- **Continuous System**: Two continuous random variables $(X, Y)$ have a valid joint PDF $f(x, y)$ if: $$\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f(x, y) \, dy \, dx = 1$$
- **Discrete System**: Represented via a Joint Probability Table where $\sum_i \sum_j P(X = x_i, Y = y_j) = 1$.

---

**2. Expectation, Covariance & Correlation Identities**

- **Linear Expectation Rule**: $$E(aX + bY) = a E(X) + b E(Y)$$
- **Covariance**: $$\operatorname{Cov}(X, Y) = E(XY) - E(X)E(Y)$$
- **Variance of Linear Combinations**: $$\operatorname{Var}(aX + bY) = a^2 \operatorname{Var}(X) + b^2 \operatorname{Var}(Y) + 2ab \operatorname{Cov}(X, Y)$$
- **Independence Property**: If $X$ and $Y$ are independent random variables, then $\operatorname{Cov}(X, Y) = 0$, implying: $$E(XY) = E(X) E(Y) \quad \text{and} \quad \operatorname{Var}(aX + bY) = a^2 \operatorname{Var}(X) + b^2 \operatorname{Var}(Y)$$
- **Correlation Coefficient (**$\rho_{XY}$**)**: $$\rho_{XY} = \frac{\operatorname{Cov}(X, Y)}{\sigma_X \sigma_Y}, \qquad -1 \le \rho_{XY} \le 1$$

---

**Part 5: Fully Solved Classroom & Examination Problems**

---

**Problem 1: Normal Course Fee Probability**

- **Question**: Course fees follow a normal distribution with mean $\mu = \text{Rs. } 6000$ and standard deviation $\sigma = \text{Rs. } 500$. Find the percentage probability that a course fee lies between $\text{Rs. } 6500$ and $\text{Rs. } 7500$, given $P(-3 \le Z \le 3) = 0.9978$ and $P(-1 \le Z \le 1) = 0.6828$.
- **Step-by-Step Solution**:
    1. **Convert to** $Z$**-scores**:
        - For $X_1 = 6500$: $z_1 = \frac{6500 - 6000}{500} = 1$
        - For $X_2 = 7500$: $z_2 = \frac{7500 - 6000}{500} = 3$
    2. **Express Probability**: $$P(6500 \le X \le 7500) = P(1 \le Z \le 3) = P(0 \le Z \le 3) - P(0 \le Z \le 1)$$
    3. **Use Symmetry of Normal Curve**:
        - $P(0 \le Z \le 3) = \frac{1}{2} P(-3 \le Z \le 3) = \frac{0.9978}{2} = 0.4989$
        - $P(0 \le Z \le 1) = \frac{1}{2} P(-1 \le Z \le 1) = \frac{0.6828}{2} = 0.3414$
    4. **Subtract Areas**: $$P(1 \le Z \le 3) = 0.4989 - 0.3414 = 0.1575 = \mathbf{15.75\%}$$

---

**Problem 2: Normal PDF Comparison**

- **Question**: Consider two normal distributions: $$f_1(x) = \exp(-\pi x^2)$$ $$f_2(x) = \frac{1}{2\pi} \exp\left\{ -\frac{1}{4\pi} (x^2 + 2x + 1) \right\}$$ Compare their means ($\mu_1, \mu_2$) and variances ($\sigma_1^2, \sigma_2^2$).
- **Step-by-Step Solution**:
    1. **Analyze** $f_1(x)$: Rewrite in standard normal form $\frac{1}{\sigma_1 \sqrt{2\pi}} \exp\left( -\frac{(x - \mu_1)^2}{2\sigma_1^2} \right)$: $$f_1(x) = \exp(-\pi x^2) \implies \mu_1 = 0$$ $$\frac{1}{2\sigma_1^2} = \pi \implies \sigma_1^2 = \frac{1}{2\pi}$$
    2. **Analyze** $f_2(x)$: Note $x^2 + 2x + 1 = (x - (-1))^2$: $$f_2(x) = \frac{1}{2\pi} \exp\left( -\frac{(x - (-1))^2}{4\pi} \right) \implies \mu_2 = -1$$ $$\frac{1}{2\sigma_2^2} = \frac{1}{4\pi} \implies \sigma_2^2 = 2\pi$$
    3. **Compare Parameters**:
        - $\mu_1 = 0 > \mu_2 = -1 \implies \mathbf{\mu_1 > \mu_2}$
        - $\sigma_1^2 = \frac{1}{2\pi} < \sigma_2^2 = 2\pi \implies \mathbf{\sigma_1^2 < \sigma_2^2}$

---

**Problem 3: Transformation of Uniform Variable**

- **Question**: Let $X$ be uniformly distributed over 12 and let $Y = \ln X$. Calculate the expected value $E(Y)$.
- **Step-by-Step Solution**:
    1. **Find Density of** $X$: $$f_X(x) = \frac{1}{3 - 1} = \frac{1}{2}, \qquad 1 \le x \le 3$$
    2. **Determine CDF of** $Y$: $$F_Y(y) = P(Y \le y) = P(\ln X \le y) = P(X \le e^y) = \int_{1}^{e^y} \frac{1}{2} \, dx = \frac{e^y - 1}{2}$$
    3. **Determine PDF of** $Y$: $$f_Y(y) = \frac{d}{dy} \left( \frac{e^y - 1}{2} \right) = \frac{e^y}{2}, \qquad 0 \le y \le \ln 3$$
    4. **Calculate Expectation via Integration by Parts**: $$E(Y) = \int_{0}^{\ln 3} y \cdot \frac{e^y}{2} \, dy = \frac{1}{2} \left[ y e^y - e^y \right]_{0}^{\ln 3}$$ $$E(Y) = \frac{1}{2} \left[ (\ln 3 \cdot e^{\ln 3} - e^{\ln 3}) - (0 - e^0) \right] = \frac{1}{2} \left[ (3 \ln 3 - 3) - (-1) \right] = \mathbf{\frac{3 \ln 3 - 2}{2} \approx 0.648}$$

---

**Problem 4: Bivariate Exponential Probability Density**

- **Question**: Continuous random variables $X$ and $Y$ have joint distribution $f_{XY}(x, y) = e^{-(x + y)}$ for $x \ge 0, y \ge 0$. Find $P(\max(X, Y) \le 1)$.
- **Step-by-Step Solution**:
    1. **Translate Event Condition**: $$\max(X, Y) \le 1 \iff (0 \le X \le 1) \text{ and } (0 \le Y \le 1)$$
    2. **Set Up Double Integral**: $$P(\max(X, Y) \le 1) = \int_{0}^{1} \int_{0}^{1} e^{-(x + y)} \, dy \, dx = \left( \int_{0}^{1} e^{-x} \, dx \right) \left( \int_{0}^{1} e^{-y} \, dy \right)$$
    3. **Evaluate Single Integrals**: $$\int_{0}^{1} e^{-t} \, dt = \left[ -e^{-t} \right]_{0}^{1} = 1 - e^{-1}$$
    4. **Combine**: $$P(\max(X, Y) \le 1) = (1 - e^{-1})^2 \approx (1 - 0.36787)^2 \approx (0.63212)^2 = \mathbf{0.40}$$

---

**Problem 5: Linear Combination of Independent Normal Variables**

- **Question**: Let $X \sim N(2, 8)$ and $Y \sim N(1, 6)$ be independent normal random variables. Define $Z = \frac{X}{4} + \frac{Y}{2}$ with mean $a$ and variance $b$. Evaluate $\int_{a}^{b} |t| \, dt$.
- **Step-by-Step Solution**:
    1. **Calculate Mean** $a = E(Z)$: $$a = E\left( \frac{X}{4} + \frac{Y}{2} \right) = \frac{1}{4} E(X) + \frac{1}{2} E(Y) = \frac{1}{4}(2) + \frac{1}{2}(1) = \frac{1}{2} + \frac{1}{2} = \mathbf{1}$$
    2. **Calculate Variance** $b = \operatorname{Var}(Z)$: Since $X$ and $Y$ are independent: $$b = \operatorname{Var}\left( \frac{X}{4} + \frac{Y}{2} \right) = \left(\frac{1}{4}\right)^2 \operatorname{Var}(X) + \left(\frac{1}{2}\right)^2 \operatorname{Var}(Y) = \frac{1}{16}(8) + \frac{1}{4}(6) = \frac{1}{2} + \frac{3}{2} = \mathbf{2}$$
    3. **Evaluate Definite Integral**: $$\int_{a}^{b} |t| \, dt = \int_{1}^{2} t \, dt = \left[ \frac{t^2}{2} \right]_{1}^{2} = \frac{4}{2} - \frac{1}{2} = \mathbf{1.5}$$