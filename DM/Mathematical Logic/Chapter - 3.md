**Predicate Logic & Single Quantifiers**

---

**1. Fundamentals of Predicate Logic**

**Open Statements & Predicate Variables**

- **Predicate Logic**: An extension of propositional logic that allows for statements containing variables over a specified domain.
- **Open Statement**: An expression $P(x)$ containing one or more variables $x$ that does not possess a fixed truth value until a specific value from the domain is substituted for $x$.
    - Example: $P(x) = \text{"}x \text{ is an even number"}$.
    - When $x = 0$: $P(0)$ evaluates to **True**.
    - When $x = 1$: $P(1)$ evaluates to **False**.
    - When $x = 2$: $P(2)$ evaluates to **True**.

**Domain of Discourse (Universe)**

- The **Domain of Discourse (**$D$**)** or **Universe** is the set of all permissible values that can be substituted for the variable $x$ in an open statement.

---

**2. Quantifiers & Their Truth Conditions**

Quantifiers are logical operators used to state the quantity of elements in a domain that satisfy an open statement.

**Universal Quantifier (**$\forall x$**)**

- **Meaning**: _"For all_ $x$_"_, _"For every_ $x$_"_, or _"For each_ $x$_"_.
- **Symbolic Expression**: $\forall x P(x)$.
- **Truth Conditions**:
    - **True**: Evaluates to **True** if and only if $P(x)$ is True for **every single element** in the domain $D$.
    - **False**: Evaluates to **False** if there exists **at least one counterexample** in $D$ where $P(x)$ is False.

**Existential Quantifier (**$\exists x$**)**

- **Meaning**: _"There exists an_ $x$_"_, _"For at least one_ $x$_"_, or _"For some_ $x$_"_.
- **Symbolic Expression**: $\exists x P(x)$.
- **Truth Conditions**:
    - **True**: Evaluates to **True** if there is **at least one element** in the domain $D$ for which $P(x)$ is True.
    - **False**: Evaluates to **False** if and only if $P(x)$ is **False for all elements** in $D$.

---

**3. Negation of Quantified Statements**

Negating a quantified expression changes the quantifier type and negates the underlying predicate.

**De Morgan's Laws for Quantifiers**

1. **Negation of Universal Quantifier**: $$\neg \forall x P(x) \equiv \exists x \neg P(x)$$ _(Asserting "not all elements satisfy_ $P(x)$_" is logically equivalent to "there exists at least one element that does not satisfy_ $P(x)$_")_.
2. **Negation of Existential Quantifier**: $$\neg \exists x P(x) \equiv \forall x \neg P(x)$$ _(Asserting "no element satisfies_ $P(x)$_" is logically equivalent to "for all elements,_ $P(x)$ _is false")_.

**Negating Conditional Predicates**

To negate a universally quantified conditional statement $\forall x [P(x) \rightarrow Q(x)]$:

1. Apply quantifier negation: $$\neg \forall x [P(x) \rightarrow Q(x)] \equiv \exists x \neg [P(x) \rightarrow Q(x)]$$
2. Replace implication with disjunction ($P(x) \rightarrow Q(x) \equiv \neg P(x) \lor Q(x)$): $$\exists x \neg [\neg P(x) \lor Q(x)]$$
3. Apply De Morgan's Law to the disjunction: $$\exists x [P(x) \land \neg Q(x)]$$

---

**4. Summary Matrix of Single Quantifiers**

|Statement Form|Symbolic Notation|Condition for Truth|Condition for Falsehood|Negation Form|
|---|---|---|---|---|
|**Universal**|$\forall x P(x)$|True for **all** $x \in D$|At least **one** counterexample $x \in D$ makes $P(x)$ False|$\exists x \neg P(x)$|
|**Existential**|$\exists x P(x)$|At least **one** element $x \in D$ makes $P(x)$ True|False for **every** element $x \in D$|$\forall x \neg P(x)$|

---

**5. Worked Practice Problems with Step-by-Step Solutions**

**Problem 1: Evaluating Truth Values across Integer Domains**

**Question**: Determine the truth value of each statement where the domain $D$ consists of all integers ($\mathbb{Z}$):

1. $\forall n (n + 1 > n)$
2. $\exists n (2n = 3n)$
3. $\exists n (n = -n)$
4. $\forall n (3n \le 4n)$

**Step-by-Step Solution**:

1. **Evaluating** $\forall n (n + 1 > n)$:
    - Subtracting $n$ from both sides yields $1 > 0$, which is unconditionally True for all integers (positive, zero, and negative).
    - **Truth Value**: **True**
2. **Evaluating** $\exists n (2n = 3n)$:
    - Solving $3n - 2n = 0$ gives $n = 0$.
    - Since $n = 0 \in \mathbb{Z}$, there exists at least one integer satisfying the equation.
    - **Truth Value**: **True**
3. **Evaluating** $\exists n (n = -n)$:
    - Solving $2n = 0$ gives $n = 0$.
    - Since $n = 0 \in \mathbb{Z}$, the condition is satisfied by at least one element.
    - **Truth Value**: **True**
4. **Evaluating** $\forall n (3n \le 4n)$:
    - Test counterexample $n = -1$: $3(-1) \le 4(-1) \implies -3 \le -4$, which is **False**.
    - Since a counterexample exists in $\mathbb{Z}$, the universal claim fails.
    - **Truth Value**: **False**

---

**Problem 2: Evaluating Truth Values across Real Domains**

**Question**: Determine the truth value of each statement where the domain consists of all real numbers ($\mathbb{R}$):

1. $\exists x (x^3 = -1)$
2. $\exists x (x^4 < x^2)$
3. $\forall x ((-x)^2 = x^2)$
4. $\forall x (2x > x)$

**Step-by-Step Solution**:

1. **Evaluating** $\exists x (x^3 = -1)$:
    - Taking the cube root gives $x = -1 \in \mathbb{R}$.
    - **Truth Value**: **True**
2. **Evaluating** $\exists x (x^4 < x^2)$:
    - Test $x = \frac{1}{2}$: $(\frac{1}{2})^4 = \frac{1}{16}$ and $(\frac{1}{2})^2 = \frac{1}{4}$.
    - Since $\frac{1}{16} < \frac{1}{4}$, the condition holds for real numbers between $-1$ and $1$ (excluding 0).
    - **Truth Value**: **True**
3. **Evaluating** $\forall x ((-x)^2 = x^2)$:
    - Squaring any negative number yields a positive result, so $(-x)^2 = x^2$ holds for all real numbers.
    - **Truth Value**: **True**
4. **Evaluating** $\forall x (2x > x)$:
    - Test counterexample $x = 0$: $2(0) > 0 \implies 0 > 0$ (False).
    - Test counterexample $x = -1$: $2(-1) > -1 \implies -2 > -1$ (False).
    - **Truth Value**: **False**

---

**Problem 3: Negating Quantified Expressions**

**Question**: Negate and simplify each of the following expressions:

1. $\exists x [p(x) \lor q(x)]$
2. $\forall x [p(x) \land \neg q(x)]$
3. $\forall x [p(x) \rightarrow q(x)]$

**Step-by-Step Solution**:

1. **Negating** $\exists x [p(x) \lor q(x)]$:
    - Apply quantifier negation rule: $\neg \exists x \longrightarrow \forall x$.
    - Negate inner formula: $\neg [p(x) \lor q(x)] \equiv \neg p(x) \land \neg q(x)$.
    - **Simplified Result**: $\mathbf{\forall x [\neg p(x) \land \neg q(x)]}$
2. **Negating** $\forall x [p(x) \land \neg q(x)]$:
    - Apply quantifier negation rule: $\neg \forall x \longrightarrow \exists x$.
    - Negate inner formula: $\neg [p(x) \land \neg q(x)] \equiv \neg p(x) \lor q(x)$.
    - **Simplified Result**: $\mathbf{\exists x [\neg p(x) \lor q(x)]}$
3. **Negating** $\forall x [p(x) \rightarrow q(x)]$:
    - Apply quantifier negation rule: $\neg \forall x \longrightarrow \exists x$.
    - Rewrite conditional statement: $p(x) \rightarrow q(x) \equiv \neg p(x) \lor q(x)$.
    - Negate disjunction using De Morgan's Law: $\neg [\neg p(x) \lor q(x)] \equiv p(x) \land \neg q(x)$.
    - **Simplified Result**: $\mathbf{\exists x [p(x) \land \neg q(x)]}$