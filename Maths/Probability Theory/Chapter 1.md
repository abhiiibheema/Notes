**Part 1: Linear Algebra Foundations & Matrix Diagonalization**

---

**1. Core Eigenvalue & Eigenvector Theorems**

- **Eigen Pair Definition**: For a square matrix $A_{n \times n}$, a non-zero vector $\vec{x} \neq \vec{0}$ satisfying $A\vec{x} = \lambda\vec{x}$ is an eigenvector, and $\lambda$ is its corresponding eigenvalue.
- **Spectrum**: The set of all eigenvalues of matrix $A$.
- **Trace & Determinant Rules**: $$\sum_{i=1}^{n} \lambda_i = \operatorname{tr}(A), \qquad \prod_{i=1}^{n} \lambda_i = \det(A)$$
- **Matrix Polynomial Rule**: If $B = aA^2 + bA + cI$, the eigenvalues of $B$ are $\lambda_i' = a\lambda_i^2 + b\lambda_i + c$.
- **Properties of Transpose & Powers**:
    - $A$ and $A^T$ share the exact same eigenvalues ($\det(A - \lambda I) = \det(A^T - \lambda I)$), but their eigenvectors are **not** identical.
    - $A$ and $A^n$ have the same eigenvectors.
    - For a real **symmetric matrix**, eigenvectors corresponding to distinct eigenvalues are strictly **orthogonal** ($\vec{x}_1 \cdot \vec{x}_2 = 0$).
- **Similar Matrices**: Two matrices $A$ and $B$ are similar if they share the same characteristic polynomial $C_A(\lambda) = C_B(\lambda)$, thereby having identical eigenvalues.

---

**2. Multiplicities & Diagonalization Criteria**

- **Algebraic Multiplicity (A.M.)**: The multiplicity of an eigenvalue $\lambda$ as a root of the characteristic equation $C_A(\lambda) = 0$.
- **Geometric Multiplicity (G.M.)**: The nullity of $(A - \lambda I)$, given by: $$\text{G.M.} = \eta(A - \lambda I) = n - \rho(A - \lambda I)$$ For any eigenvalue, $\text{A.M.} \ge \text{G.M.}$ holds.
- **Diagonalization Theorem**: A matrix $A_{n \times n}$ is diagonalizable if and only if it has $n$ linearly independent eigenvectors.
    - If $A$ is diagonalizable, it can be factored as: $$A = P D P^{-1} \quad \text{or} \quad AP = PD$$ where $P = [\vec{x}_1, \vec{x}_2, \dots, \vec{x}_n]$ is the modal matrix containing eigenvectors as columns, and $D = \operatorname{diag}(\lambda_1, \lambda_2, \dots, \lambda_n)$.
    - **Symmetric Matrix Case**: If $A$ is symmetric, $P$ is an orthogonal matrix ($P^{-1} = P^T$), giving: $$A = P D P^T$$
    - **Multiplicity Equality**: $A$ is diagonalizable if and only if $\text{A.M.} = \text{G.M.}$ for every eigenvalue.
    - **Rank Identity for Diagonalizable Matrices**: $$\rho(A) = n - (\text{Algebraic Multiplicity of zero eigenvalue})$$
    - **Matrix Functions**: If $A = P D P^{-1}$, then: $$A^k = P D^k P^{-1}, \qquad f(A) = P f(D) P^{-1}, \qquad e^A = P e^D P^{-1}$$

---

**3. Cayley-Hamilton Theorem**

- **Statement**: Every square matrix satisfies its own characteristic equation: $$C_A(A) = O$$
- **Application**: Used to express higher powers of $A$ (e.g., $A^4, A^5$) or matrix inverses ($A^{-1}$) in terms of lower-degree matrix polynomials.

---

**Part 2: Foundations of Probability Theory**

---

**1. Experiments: Deterministic vs. Random**

- **Deterministic Experiments**: Experiments where the outcome is certain and completely predictable under identical conditions (e.g., Ohm's Law $V = IR \implies I = \frac{100}{20} = 5\text{ A}$). The concept of probability does **not** apply.
- **Random Experiments (R.E.)**: Experiments where the exact outcome cannot be predicted with certainty prior to execution, even under identical conditions (e.g., tossing a coin, rolling a die).

---

**2. Sample Space, Events & Probability Definitions**

- **Sample Space (**$S$**)**: The set containing all possible outcomes of a random experiment.
    - _Tossing 1 coin_: $S = \{\text{Head}, \text{Tail}\} \implies n(S) = 2$
    - _Tossing 2 coins sequentially_: $S = \{HH, HT, TH, TT\} \implies n(S) = 4$
    - _Rolling 1 fair die_: $S = \{1, 2, 3, 4, 5, 6\} \implies n(S) = 6$
    - _Rolling a pair of dice_: $S = \{(1,1), (1,2), \dots, (6,6)\} \implies n(S) = 36$
- **Event (**$E$**)**: Any subset of a sample space ($E \subseteq S$).
    - _Cardinality_ $n(E)$: The number of outcomes favoring event $E$.
- **Classical Probability** $P(A)$: $$P(A) = \frac{\text{Number of elements in } S \text{ favoring event } A}{\text{Total number of elements in } S} = \frac{n(A)}{n(S)}$$

---

**3. Axioms of Probability**

1. **Boundedness**: For any event $A \subseteq S$: $$0 \le P(A) \le 1$$
2. **Certain Event**: The probability of the sample space is unity: $$P(S) = 1$$
3. **Additivity**: For mutually exclusive events, probability is additive over unions.

---

**3. Classification of Events & Fundamental Theorems**

---

**1. Event Types**

- **Mutually Exclusive Events**: Events $A$ and $B$ cannot occur simultaneously ($A \cap B = \emptyset$). $$P(A \cap B) = 0$$
- **Mutually Exhaustive Events**: Events $A$ and $B$ whose union covers the entire sample space ($A \cup B = S$). $$P(A \cup B) = P(S) = 1$$
- **Independent Events**: Occurrence of event $A$ has no influence on the occurrence of event $B$. $$P(A \cap B) = P(A) \cdot P(B)$$
- **Complement of an Event (**$E^C$**)**: The event that $E$ does not occur. $$P(E) + P(E^C) = 1 \implies P(E^C) = 1 - P(E)$$

---

**2. Addition Theorem of Probability**

For any two events $A$ and $B$ in a sample space $S$: $$P(A \cup B) = P(A) + P(B) - P(A \cap B)$$

- **Special Cases**:
    - **Mutually Exclusive (**$A \cap B = \emptyset$**)**: $$P(A \cup B) = P(A) + P(B)$$
    - **Mutually Exhaustive (**$A \cup B = S$**)**: $$1 = P(A) + P(B) - P(A \cap B)$$
- **Three Events Formula**: $$P(A \cup B \cup C) = P(A) + P(B) + P(C) - P(A \cap B) - P(B \cap C) - P(C \cap A) + P(A \cap B \cap C)$$
- **Generalized Sum Rule**: For $n$ pairwise mutually exclusive events $E_1, E_2, \dots, E_n$: $$P\left(\bigcup_{i=1}^{n} E_i\right) = \sum_{i=1}^{n} P(E_i)$$

---

**3. Useful Set & Complement Rules**

1. **De Morgan's Identity**: $$P(A^C \cap B^C) = P((A \cup B)^C) = 1 - P(A \cup B)$$
2. **Difference Rule**: $$P(A^C \cap B) = P(B - A) = P(B) - P(A \cap B)$$
3. **Conditional Complement Rule**: $$P(A^C / B) = \frac{P(A^C \cap B)}{P(B)} = \frac{P(A \cup B) - P(A)}{P(B)}$$

---

**4. Conditional Probability & Multiplication Theorem**

- **Conditional Probability** $P(A/B)$: The probability that event $A$ occurs, given that event $B$ has already occurred ($P(B) > 0$): $$P(A/B) = \frac{P(A \cap B)}{P(B)} = \frac{n(A \cap B)}{n(B)}$$
- **Multiplication Theorem**: $$P(A \cap B) = P(B) \cdot P(A/B) = P(A) \cdot P(B/A)$$
    - For independent events, $P(A/B) = P(A) \implies P(A \cap B) = P(A) \cdot P(B)$.

---

**Part 4: Fully Solved Classroom & Exam Problems**

---

**Problem 1: Trace of a Matrix Polynomial**

- **Question**: If $1, 0,$ and $-1$ are the eigenvalues of a $3 \times 3$ matrix $A$, find the trace of $A^2 + 5A$.
- **Solution**:
    1. Given eigenvalues of $A$: $\lambda_1 = 1$, $\lambda_2 = 0$, $\lambda_3 = -1$.
    2. The eigenvalues $\lambda_i'$ of $P(A) = A^2 + 5A$ are: $$\lambda_1' = (1)^2 + 5(1) = 6$$ $$\lambda_2' = (0)^2 + 5(0) = 0$$ $$\lambda_3' = (-1)^2 + 5(-1) = 1 - 5 = -4$$
    3. The trace is the sum of the eigenvalues: $$\operatorname{tr}(A^2 + 5A) = 6 + 0 + (-4) = 2$$
- **Final Answer**: **2**

---

**Problem 2: Rank of Shifted Diagonalizable Matrix**

- **Question**: Let $I$ be the $7 \times 7$ identity matrix, and $A_{7 \times 7}$ be a real matrix with characteristic polynomial $C_A(\lambda) = \lambda^2 (\lambda - 1)^\alpha (\lambda + 2)^\beta$, where $\alpha, \beta \in \mathbb{Z}^+$. If $A$ is diagonalizable and $\operatorname{rank}(A) = \operatorname{rank}(A + 2I)$, calculate $\operatorname{rank}(A - I)$.
- **Solution**:
    1. Matrix order is 7 $\implies 2 + \alpha + \beta = 7 \implies \alpha + \beta = 5$.
    2. Because $A$ is diagonalizable, $\text{A.M.} = \text{G.M.}$ for all eigenvalues:
        - For $\lambda = 0$: $\operatorname{rank}(A) = 7 - \text{A.M.}(0) = 7 - 2 = 5$.
        - For $\lambda = -2$: $\operatorname{rank}(A + 2I) = 7 - \text{A.M.}(-2) = 7 - \beta$.
        - For $\lambda = 1$: $\operatorname{rank}(A - I) = 7 - \text{A.M.}(1) = 7 - \alpha$.
    3. Given $\operatorname{rank}(A) = \operatorname{rank}(A + 2I) \implies 5 = 7 - \beta \implies \beta = 2$.
    4. Solve for $\alpha$: $\alpha + 2 = 5 \implies \alpha = 3$.
    5. Calculate rank: $$\operatorname{rank}(A - I) = 7 - 3 = 4$$
- **Final Answer**: **4**

---

**Problem 3: Sum Probability of Two Random Draws**

- **Question**: Given Set $A = \{2, 3, 4, 5\}$ and Set $B = \{11, 12, 13, 14, 15\}$, one number is selected at random from each set. Find the probability that the sum of the two numbers equals $16$.
- **Solution**:
    1. Total combinations $n(S) = n(A) \times n(B) = 4 \times 5 = 20$.
    2. Pairs yielding a sum of 16: $$(2, 14), \; (3, 13), \; (4, 12), \; (5, 11)$$ Number of favorable outcomes $n(E) = 4$.
    3. Probability: $$P(\text{Sum} = 16) = \frac{4}{20} = 0.20 = 20\%$$
- **Final Answer**: $20\%$ **(or** $0.2$**)**

---

**Problem 4: Replacement Drawing Probability**

- **Question**: A box contains $10$ screws, $3$ of which are defective. Two screws are drawn at random with replacement. Find the probability (in $\%$) that none of the drawn screws are defective.
- **Solution**:
    1. Total screws = 10 (3 defective, 7 non-defective).
    2. Draws are made with replacement $\implies$ independent trials.
    3. Probability of picking a non-defective screw on any draw: $P(ND) = \frac{7}{10}$.
    4. Probability that both screws are non-defective: $$P(\text{None defective}) = \frac{7}{10} \times \frac{7}{10} = \frac{49}{100} = 0.49 = 49\%$$
- **Final Answer**: $49\%$

---

**Problem 5: Conditional Dice Outcome Probability**

- **Question**: When a fair die is rolled, given that the outcome is greater than $3$, find the probability that the outcome is an even number.
- **Solution**:
    1. Sample space $S = \{1, 2, 3, 4, 5, 6\}$.
    2. Given condition event $B$ (outcome $> 3$): $B = \{4, 5, 6\} \implies n(B) = 3$.
    3. Target event $A$ (even number): $A = \{2, 4, 6\}$.
    4. Intersection $A \cap B = \{4, 6\} \implies n(A \cap B) = 2$.
    5. Conditional probability: $$P(A/B) = \frac{n(A \cap B)}{n(B)} = \frac{2}{3}$$
- **Final Answer**: $\frac{2}{3}$

---

**Problem 6: Independent Events Parameter Determination**

- **Question**: Let $E_1$ and $E_2$ be independent events in a probability space satisfying $P(E_1) = P(E_2)$ and $P(E_1 \cup E_2) = 1$. Find $P(E_1)$.
- **Solution**:
    1. Let $P(E_1) = P(E_2) = x$.
    2. Independence implies $P(E_1 \cap E_2) = P(E_1) \cdot P(E_2) = x^2$.
    3. Apply the Addition Theorem: $$P(E_1 \cup E_2) = P(E_1) + P(E_2) - P(E_1 \cap E_2)$$ $$1 = x + x - x^2 \implies x^2 - 2x + 1 = 0 \implies (x - 1)^2 = 0 \implies x = 1$$
    4. Therefore, $P(E_1) = 1$.
- **Final Answer**: **1**