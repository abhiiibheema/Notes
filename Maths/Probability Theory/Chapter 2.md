**Advanced Probability Theorems**

---

**1. Theorem of Total Probability**

- **Statement**: Let $E_1, E_2, \dots, E_n$ be $n$ mutually exclusive ($E_i \cap E_j = \emptyset$ for $i \neq j$) and collectively exhaustive ($\bigcup_{i=1}^n E_i = S$) events in a sample space $S$. For any event $A$ in $S$: $$P(A) = \sum_{i=1}^{n} P(E_i) \cdot P(A/E_i) = P(E_1)P(A/E_1) + P(E_2)P(A/E_2) + \dots + P(E_n)P(A/E_n)$$
- **Mathematical Proof**:
    1. Express event $A$ in terms of the sample space partition: $$A = A \cap S = A \cap \left( \bigcup_{i=1}^{n} E_i \right) = \bigcup_{i=1}^{n} (A \cap E_i)$$
    2. Since $E_1, E_2, \dots, E_n$ are mutually exclusive, the sets $(A \cap E_1), (A \cap E_2), \dots, (A \cap E_n)$ are also pairwise mutually exclusive.
    3. Applying the additivity axiom of probability: $$P(A) = P\left( \bigcup_{i=1}^{n} (A \cap E_i) \right) = \sum_{i=1}^{n} P(A \cap E_i)$$
    4. Using the multiplication theorem $P(A \cap E_i) = P(E_i) \cdot P(A/E_i)$: $$P(A) = \sum_{i=1}^{n} P(E_i) \cdot P(A/E_i)$$

---

**2. Bayes' Theorem (Inverse Probability)**

- **Statement**: Let $E_1, E_2, \dots, E_n$ be $n$ mutually exclusive and collectively exhaustive events forming a partition of sample space $S$. For any event $A$ with $P(A) > 0$, the conditional probability that event $E_i$ occurs given that event $A$ has already occurred is: $$P(E_i/A) = \frac{P(E_i \cap A)}{P(A)} = \frac{P(E_i) \cdot P(A/E_i)}{\sum_{j=1}^{n} P(E_j) \cdot P(A/E_j)}$$
- **Key Terminology**:
    - **Prior Probabilities**: $P(E_i)$ — initial likelihoods of events prior to observing outcome $A$.
    - **Likelihoods**: $P(A/E_i)$ — conditional probability of observing $A$ given event $E_i$.
    - **Posterior Probabilities**: $P(E_i/A)$ — updated likelihood of $E_i$ after observing outcome $A$.

---

**Part 2: Fully Solved Probability Problems**

---

**Problem 1: Drawing Cards Without Replacement**

- **Question**: From a standard deck of 52 playing cards, two cards are drawn at random sequentially without replacement. What is the probability that both cards are Kings?
- **Solution**:
    1. Total cards = 52; total Kings in a deck = 4.
    2. Let $K_1$ = King on 1st draw, $K_2$ = King on 2nd draw.
    3. Probability of King on 1st draw: $$P(K_1) = \frac{^4C_1}{^{52}C_1} = \frac{4}{52} = \frac{1}{13}$$
    4. Remaining cards = 51; remaining Kings = 3. Probability of King on 2nd draw given 1st was a King: $$P(K_2/K_1) = \frac{^3C_1}{^{51}C_1} = \frac{3}{51} = \frac{1}{17}$$
    5. By the multiplication theorem: $$P(K_1 \cap K_2) = P(K_1) \cdot P(K_2/K_1) = \frac{4}{52} \times \frac{3}{51} = \frac{1}{13 \times 17} = \frac{1}{221}$$
- **Final Answer**: $\frac{1}{221}$

---

**Problem 2: Multiple-Choice Question Guessing & Bayes' Application**

- **Question**: A student answers a multiple-choice question with 4 options. The probability that the student knows the answer is $0.7$. If the student does not know the answer, he guesses randomly with a $0.25$ chance of being correct.
    1. Find the probability that the student answers the question correctly.
    2. If the student answers correctly, find the probability that he guessed the answer.
- **Solution**:
    1. **Define Events**:
        - $K$: Student knows the answer $\implies P(K) = 0.7$
        - $G$: Student guesses the answer ($K^C$) $\implies P(G) = 1 - 0.7 = 0.3$
        - $C$: Student answers correctly
    2. **Given Conditional Likelihoods**:
        - $P(C/K) = 1$ (if the student knows, he definitely answers correctly)
        - $P(C/G) = 0.25$ (given guessing, chance of correct answer is $\frac{1}{4}$)
    3. **Part 1 — Total Probability of Correct Answer**: $$P(C) = P(K) \cdot P(C/K) + P(G) \cdot P(C/G)$$ $$P(C) = (0.7 \times 1) + (0.3 \times 0.25) = 0.7 + 0.075 = 0.775$$
    4. **Part 2 — Bayes' Theorem for Guessing Given Correct Answer**: $$P(G/C) = \frac{P(G) \cdot P(C/G)}{P(C)} = \frac{0.3 \times 0.25}{0.775} = \frac{0.075}{0.775} = \frac{3}{31} \approx 0.09677 \quad (\text{or } 9.68\%)$$
- **Final Answers**: **(1)** $0.775$, **(2)** $\frac{3}{31} \approx 0.0968$

---

**Problem 3: The Truth-Teller Dice Verification Problem**

- **Question**: A person who speaks the truth $3$ out of $4$ times throws a fair six-faced die and reports that the outcome is $5$. What is the probability that the outcome is actually $5$?
- **Solution**:
    1. **Define Events**:
        - $E_5$: Die actually shows $5 \implies P(E_5) = \frac{1}{6}$
        - $E_5^C$: Die shows any number other than $5 \implies P(E_5^C) = \frac{5}{6}$
        - $R_5$: Person reports the outcome as $5$
    2. **Truth-Telling Probabilities**:
        - $P(\text{Truth}) = \frac{3}{4} = 0.75$, $P(\text{Lie}) = 1 - \frac{3}{4} = \frac{1}{4} = 0.25$
    3. **Conditional Likelihoods**:
        - $P(R_5/E_5) = P(\text{Truth}) = \frac{3}{4}$ (outcome is 5 and person speaks truth)
        - $P(R_5/E_5^C) = P(\text{Lie}) = \frac{1}{4}$ (outcome is not 5, but person lies and claims it is 5)
    4. **Apply Bayes' Theorem**: $$P(E_5/R_5) = \frac{P(E_5) \cdot P(R_5/E_5)}{P(E_5) \cdot P(R_5/E_5) + P(E_5^C) \cdot P(R_5/E_5^C)}$$ $$P(E_5/R_5) = \frac{\left(\frac{1}{6}\right) \times \left(\frac{3}{4}\right)}{\left(\frac{1}{6}\right) \times \left(\frac{3}{4}\right) + \left(\frac{5}{6}\right) \times \left(\frac{1}{4}\right)} = \frac{\frac{3}{24}}{\frac{3}{24} + \frac{5}{24}} = \frac{\frac{3}{24}}{\frac{8}{24}} = \frac{3}{8} = 0.375$$
- **Final Answer**: $\frac{3}{8}$ **(or** $0.375$**)**

---

**Problem 4: Sequential Infinite Dice Game**

- **Question**: Players A and B take turns throwing a fair six-faced die. If A throws first, calculate the probability that B is the first player to roll a $6$.
- **Solution**:
    1. **Probability of Rolling a 6**:
        - Success: $P(S) = \frac{1}{6}$
        - Failure: $P(F) = 1 - \frac{1}{6} = \frac{5}{6}$
    2. **Order of Play**:
        - Turn 1: A rolls.
        - Turn 2: B rolls.
        - Turn 3: A rolls.
        - Turn 4: B rolls... and so forth.
    3. **Winning Scenarios for B**:
        - B wins on Turn 2: A fails, B succeeds $\implies F \cdot S$
        - B wins on Turn 4: A fails, B fails, A fails, B succeeds $\implies F \cdot F \cdot F \cdot S = F^3 S$
        - B wins on Turn 6: $F^5 S$
    4. **Sum of Infinite Geometric Series**: $$P(\text{B wins first}) = \left(\frac{5}{6}\right)\left(\frac{1}{6}\right) + \left(\frac{5}{6}\right)^3\left(\frac{1}{6}\right) + \left(\frac{5}{6}\right)^5\left(\frac{1}{6}\right) + \dots$$
        - First term $a = \frac{5}{6} \times \frac{1}{6} = \frac{5}{36}$
        - Common ratio $r = \left(\frac{5}{6}\right)^2 = \frac{25}{36}$ $$P(\text{B wins first}) = \frac{a}{1 - r} = \frac{\frac{5}{36}}{1 - \frac{25}{36}} = \frac{\frac{5}{36}}{\frac{11}{36}} = \frac{5}{11} \approx 0.4545$$
- **Final Answer**: $\frac{5}{11} \approx 0.45$

---

**Part 3: Descriptive Statistics & Central Tendency**

---

**1. Measures of Central Tendency**

- **A. Arithmetic Mean (**$\bar{x}$ **or** $\mu$**)**:
    - **Ungrouped Data**: For $n$ observations $x_1, x_2, \dots, x_n$: $$\bar{x} = \mu = \frac{\sum_{i=1}^{n} x_i}{n}$$
        - _Example — Mean of First_ $n$ _Natural Numbers_: $$\text{Data}: 1, 2, 3, \dots, n \implies \mu = \frac{1 + 2 + \dots + n}{n} = \frac{\frac{n(n+1)}{2}}{n} = \frac{n+1}{2}$$
    - **Grouped Data (Frequency Distribution)**: For distinct values $x_1, x_2, \dots, x_n$ occurring with frequencies $f_1, f_2, \dots, f_n$ (where $N = \sum f_i$): $$\bar{x} = \mu = \frac{\sum_{i=1}^{n} f_i x_i}{\sum_{i=1}^{n} f_i} = \frac{\sum_{i=1}^{n} f_i x_i}{N}$$
        - _Connection to Probability_: Since $\frac{f_i}{N} = P(X = x_i)$: $$\mu = \sum_{i=1}^{n} x_i \cdot P(X = x_i) = E(X) \quad (\text{Expected Value of } X)$$
- **B. Median**:
    - The middle observation when raw data is arranged in **ascending or descending order**.
    - **Odd** $n$: $\text{Median} = \left( \frac{n+1}{2} \right)^{\text{th}} \text{observation}$
    - **Even** $n$: $\text{Median} = \text{Mean of } \left( \frac{n}{2} \right)^{\text{th}} \text{and } \left( \frac{n}{2} + 1 \right)^{\text{th}} \text{observations}$
    - _Worked Example_: Find the median of $12, 34, 17, 8, 2, 4, 41, 37, 2, 6, 7$.
        - Arrange in ascending order ($n = 11$): $2, 2, 4, 6, 7, \mathbf{8}, 12, 17, 34, 37, 41$
        - Median position: $\left( \frac{11+1}{2} \right)^{\text{th}} = 6^{\text{th}} \text{observation} = \mathbf{8}$
- **C. Mode**:
    - The observation that appears with the highest frequency in a dataset.

---

**Part 4: Measures of Dispersion & Variance Derivation**

---

**1. Absolute Deviation & Standard Deviation**

- **Absolute Deviation**: $|d_i| = |x_i - \mu|$
- **Mean Absolute Deviation**: $$\text{M.A.D.} = \frac{\sum_{i=1}^{n} |d_i|}{n}$$
- **Standard Deviation (**$\sigma$**)**: $$\sigma = \sqrt{\frac{\sum_{i=1}^{n} (x_i - \mu)^2}{n}}$$

---

**2. Mathematical Derivation of Variance (**$\sigma^2$**)**

- **Definition**: The variance $\sigma^2$ is the mean of the squared deviations of all data points about their arithmetic mean.
- **Proof**:
    1. For a frequency distribution with $N = \sum f_i$: $$\sigma^2 = \frac{\sum_{i=1}^{n} f_i (x_i - \mu)^2}{N}$$
    2. Expand the squared binomial term: $$\sigma^2 = \frac{1}{N} \sum_{i=1}^{n} f_i \left( x_i^2 + \mu^2 - 2x_i \mu \right)$$
    3. Distribute the summation and factor out constants ($\mu$): $$\sigma^2 = \frac{\sum_{i=1}^{n} f_i x_i^2}{N} + \mu^2 \frac{\sum_{i=1}^{n} f_i}{N} - 2\mu \frac{\sum_{i=1}^{n} f_i x_i}{N}$$
    4. Substitute $P(X = x_i) = \frac{f_i}{N}$, $\frac{\sum f_i}{N} = 1$, and $\mu = \frac{\sum f_i x_i}{N} = E(X)$: $$\sigma^2 = \sum_{i=1}^{n} x_i^2 P(X = x_i) + \mu^2(1) - 2\mu(\mu)$$ $$\sigma^2 = E(X^2) + \mu^2 - 2\mu^2 = E(X^2) - \mu^2$$
    5. Therefore: $$\mathbf{\sigma^2 = E(X^2) - [E(X)]^2}$$ where $E(X) = \sum x_i P(X = x_i)$ and $E(X^2) = \sum x_i^2 P(X = x_i)$.
- **Properties**:
    - Variance is non-negative: $\sigma^2 \ge 0$.
    - Standard deviation is the non-negative square root of variance: $\sigma = \sqrt{\text{Var}(X)}$.

---

**Part 5: Introduction to Random Variables**

---

**1. Definition**

A **random variable** $X$ is a real-valued function that maps every outcome of a random experiment in sample space $S$ to a real number $x \in \mathbb{R}$.

---

**2. Classification of Random Variables**

|Property|Discrete Random Variable (DRV)|Continuous Random Variable (CRV)|
|---|---|---|
|**Determination Method**|Obtained by **counting**|Obtained by **measuring**|
|**Possible Values**|Finite or countably infinite set|Uncountable continuum of real values within an interval|
|**Examples**|• Number of heads in 4 coin flips ($X \in \{0, 1, 2, 3, 4\}$) • Outcome of a die roll ($X \in \{1, 2, 3, 4, 5, 6\}$)|• Height of a person • Temperature of a city • Diameter of a manufactured tube|
|**Probability Function**|Probability Mass Function (PMF): $P(X = x_i)$|Probability Density Function (PDF): $f(x)$|
|**Sum / Total Rule**|$\sum P(X = x_i) = 1$|$\int_{-\infty}^{\infty} f(x) dx = 1$|