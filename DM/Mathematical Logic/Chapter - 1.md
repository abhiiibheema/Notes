**Propositional Logic & Truth Tables**

---

**1. Fundamentals of Propositional Logic**

**Propositions & Truth Values**

- **Propositional Statement**: A declarative assertion that evaluates strictly to either **True (1 / T)** or **False (0 / F)**, but never both.
- **Simple (Atomic) Propositions**: Primitive statements that cannot be decomposed into simpler logical sub-statements.
- **Compound Propositions**: Complex logical formulas formed by combining simple propositions using **logical connectives**.

---

**Logical Connectives & Operators**

|Connective Name|Symbol|Operator Type|Truth Rule & Behavioral Property|
|---|---|---|---|
|**Negation**|$\neg$ or $\sim$|Modifier|Inverts truth value ($\neg \text{True} = \text{False}$, $\neg \text{False} = \text{True}$).|
|**Conjunction**|$\land$|Binary (AND / But)|Evaluates to True **only** when both components are True ("Hates False").|
|**Disjunction**|$\lor$|Binary (OR)|Evaluates to True if **at least one** component is True ("Loves True").|
|**Single Implication**|$\rightarrow$|Binary (IF... THEN)|Evaluates to False **only** when antecedent is True and consequent is False.|
|**Double Implication**|$\leftrightarrow$|Binary (IF AND ONLY IF)|Evaluates to True **only** when both components share identical truth values.|

---

**2. Truth Tables & Conditional Logic**

**Master Truth Table for Fundamental Connectives**

|$p$|$q$|$\neg p$|$p \land q$|$p \lor q$|$p \rightarrow q$|$p \leftrightarrow q$|
|---|---|---|---|---|---|---|
|**T**|**T**|F|**T**|**T**|**T**|**T**|
|**T**|**F**|F|**F**|**T**|**F**|**F**|
|**F**|**T**|T|**F**|**T**|**T**|**F**|
|**F**|**F**|T|**F**|**F**|**T**|**T**|

---

**Natural Language Formulations of Implication (**$p \rightarrow q$**)**

In the conditional statement $p \rightarrow q$, $p$ represents the **antecedent (hypothesis)** and $q$ represents the **consequent (conclusion)**. All of the following verbal expressions are logically equivalent to $p \rightarrow q$:

- _"If_ $p$_, then_ $q$_"_
- _"If_ $p$_,_ $q$_"_
- _"_$q$ _if_ $p$_"_
- _"_$q$ _when_ $p$_"_
- _"_$q$ _whenever_ $p$_"_
- _"_$p$ _only if_ $q$_"_
- _"_$q$ _unless_ $\neg p$_"_

---

**Conditional Statement Variations**

For any given conditional statement $p \rightarrow q$:

1. **Converse**: $q \rightarrow p$
2. **Inverse**: $\neg p \rightarrow \neg q$
3. **Contrapositive**: $\neg q \rightarrow \neg p$

**Core Logical Equivalence Rules**:

- A conditional statement is **logically equivalent** to its contrapositive: $$p \rightarrow q \equiv \neg q \rightarrow \neg p$$
- A conditional statement is **not** equivalent to its converse or inverse: $$p \rightarrow q \not\equiv q \rightarrow p \quad \text{and} \quad p \rightarrow q \not\equiv \neg p \rightarrow \neg q$$
- The **converse** and **inverse** of a conditional statement are logically equivalent to each other: $$(q \rightarrow p) \equiv (\neg p \rightarrow \neg q)$$

---

**3. Classifications of Logical Expressions**

Compound propositions are categorized into four structural types based on their truth table outputs:

- **Satisfiable**: An expression containing **at least one True** value in its truth table column.
- **Valid (Tautology)**: An expression where **every entry is True** in its truth table column.
- **Contradiction**: An expression where **every entry is False** in its truth table column.
- **Contingency**: An expression that is **neither a Tautology nor a Contradiction** (contains both True and False values).

**Classification Hierarchy**:

- All **Valid (Tautology)** statements are automatically **Satisfiable**.
- All **Contingencies** with at least one True assignment are **Satisfiable**.

---

**4. Logical Equivalence & Equivalence Laws**

**Definition of Logical Equivalence (**$\equiv$**)**

Two compound propositions $A$ and $B$ are logically equivalent ($A \equiv B$) if and only if they produce **identical truth table columns**, which means the biconditional statement $A \leftrightarrow B$ is a Tautology.

---

**Fundamental Laws of Boolean Algebra**

**1. Idempotent Laws**

- $a \lor a \equiv a$
- $a \land a \equiv a$

**2. Identity & Domination Laws**

- $a \lor \text{True} \equiv \text{True}$
- $a \land \text{False} \equiv \text{False}$
- $a \land \text{True} \equiv a$
- $a \lor \text{False} \equiv a$

**3. Complement Laws**

- $a \land \neg a \equiv \text{False}$
- $a \lor \neg a \equiv \text{True}$

**4. Commutative Laws**

- $a \lor b \equiv b \lor a$
- $a \land b \equiv b \land a$

**5. Associative Laws**

- $a \lor (b \lor c) \equiv (a \lor b) \lor c$
- $a \land (b \land c) \equiv (a \land b) \land c$

**6. Distributive Laws**

- $a \lor (b \land c) \equiv (a \lor b) \land (a \lor c)$
- $a \land (b \lor c) \equiv (a \land b) \lor (a \land c)$

**7. Absorption Laws**

- $a \lor (a \land b) \equiv a$
- $a \land (a \lor b) \equiv a$

**8. De Morgan's Laws**

- $\neg(a \lor b) \equiv \neg a \land \neg b$
- $\neg(a \land b) \equiv \neg a \lor \neg b$

---

**Advanced Implication Identities**

- **Implication as Disjunction**: $$a \rightarrow b \equiv \neg a \lor b$$
- **Contrapositive Equivalence**: $$a \rightarrow b \equiv \neg b \rightarrow \neg a$$
- **Distributing Implication over Conjunction & Disjunction**:
    - $(a \rightarrow b) \land (a \rightarrow c) \equiv a \rightarrow (b \land c)$
    - $(a \rightarrow c) \land (b \rightarrow c) \equiv (a \lor b) \rightarrow c$
    - $(a \rightarrow b) \lor (a \rightarrow c) \equiv a \rightarrow (b \lor c)$
    - $(a \rightarrow c) \lor (b \rightarrow c) \equiv (a \land b) \rightarrow c$
    - $[p \rightarrow (q \lor r)] \equiv [(p \land \neg q) \rightarrow r]$

---

**5. Switching Circuit Representations**

Logical expressions map directly to electric switching networks:

- **Series Connection**: Represents logical **Conjunction (**$\land$**)**.
- **Parallel Connection**: Represents logical **Disjunction (**$\lor$**)**.

---

**6. Solved Practice Problems with Worked Steps**

**Problem 1: Algebraic Simplification**

**Question**: Reduce the propositional expression to its simplest form: $$(p \land q) \lor (p \land \neg q) \lor (p \land q \land r)$$

**Step-by-Step Solution**:

1. Apply the **Distributive Law** to the first two terms: $$(p \land q) \lor (p \land \neg q) \equiv p \land (q \lor \neg q)$$
2. Apply the **Complement Law** ($q \lor \neg q \equiv \text{True}$): $$p \land \text{True} \equiv p$$
3. Substitute back into the expression: $$p \lor (p \land q \land r)$$
4. Apply the **Absorption Law** ($p \lor (p \land X) \equiv p$): $$p \lor (p \land q \land r) \equiv \mathbf{p}$$

**Final Answer**: $\mathbf{p}$

---

**Problem 2: Boolean Expression Reduction**

**Question**: Simplify the expression: $$(x \land y) \lor (x \land \neg y) \lor (\neg x \land y) \lor (\neg x \land \neg y)$$

**Step-by-Step Solution**:

1. Group terms using the **Distributive Law**: $$[x \land (y \lor \neg y)] \lor [\neg x \land (y \lor \neg y)]$$
2. Apply the **Complement Law** ($y \lor \neg y \equiv \text{True}$): $$(x \land \text{True}) \lor (\neg x \land \text{True})$$
3. Apply the **Identity Law** ($a \land \text{True} \equiv a$): $$x \lor \neg x$$
4. Apply the **Complement Law** ($x \lor \neg x \equiv \text{True}$): $$x \lor \neg x \equiv \mathbf{1 \; (\text{True})}$$

**Final Answer**: $\mathbf{1 \; (\text{True})}$

---

**Problem 3: Multiple Equivalence Verification**

**Question**: Determine which of the following expressions are equivalent to $P \lor \neg Q$:

- $\text{I. } P \lor \neg Q$
- $\text{II. } \neg(\neg P \land Q)$
- $\text{III. } (P \land Q) \lor (P \land \neg Q) \lor (\neg P \land \neg Q)$
- $\text{IV. } (P \land Q) \lor (P \land \neg Q) \lor (\neg P \land Q)$

**Step-by-Step Solution**:

- **Expression II**: Apply **De Morgan's Law** to $\neg(\neg P \land Q)$: $$\neg(\neg P) \lor \neg Q \equiv P \lor \neg Q \quad \text{(Equivalent)}$$
- **Expression III**: Factor $P$ out of the first two terms: $$[P \land (Q \lor \neg Q)] \lor (\neg P \land \neg Q) \equiv P \lor (\neg P \land \neg Q)$$ Apply the **Distributive Law**: $$(P \lor \neg P) \land (P \lor \neg Q) \equiv \text{True} \land (P \lor \neg Q) \equiv P \lor \neg Q \quad \text{(Equivalent)}$$
- **Expression IV**: Simplifies to $P \lor Q$, which is **not** equivalent to $P \lor \neg Q$.

**Final Answer**: **Only I, II, and III**

---

**Problem 4: Verbal Statement Transformations**

**Question**: Formulate the Converse, Inverse, and Contrapositive for the statement: _"If it rains, then the match will not be played."_

**Step-by-Step Solution**:

- Let $r$: _"It rains"_, and $m$: _"The match is played"_.
- Original Statement ($r \rightarrow \neg m$):
- **Converse** ($q \rightarrow p$): $\neg m \rightarrow r$ (_"If the match is not played, then it rains."_).
- **Inverse** ($\neg p \rightarrow \neg q$): $\neg r \rightarrow m$ (_"If it does not rain, then the match will be played."_).
- **Contrapositive** ($\neg q \rightarrow \neg p$): $m \rightarrow \neg r$ (_"If the match is played, then it does not rain."_).