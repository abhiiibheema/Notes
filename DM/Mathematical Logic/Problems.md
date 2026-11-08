**Master Discrete Mathematics Revision Notes: Mathematical Logic**

---

**SECTION 1: Propositional Logic & Truth Tables**

**1. Core Principles**

- **Propositional Statement**: A declarative sentence that evaluates strictly to either **True (1)** or **False (0)**, but never both.
- **Simple (Atomic) Propositions**: Primitive statements containing no logical connectives.
- **Compound Propositions**: Statements constructed by joining atomic propositions using logical operators.

---

**2. Connectives & Master Truth Table**

|Connective Name|Symbol|Operator Type|Truth Rule|
|---|---|---|---|
|**Negation**|$\neg$ or $\sim$|Modifier|Inverts the truth value ($\neg \text{True} = \text{False}$, $\neg \text{False} = \text{True}$).|
|**Conjunction**|$\land$|Binary (AND / But)|True **only** if both components are True ("Hates False").|
|**Disjunction**|$\lor$|Binary (OR)|True if **at least one** component is True ("Loves True").|
|**Single Implication**|$\rightarrow$|Binary (IF... THEN)|False **only** when antecedent is True and consequent is False.|
|**Double Implication**|$\leftrightarrow$|Binary (IF AND ONLY IF)|True **only** when both components share identical truth values.|

---

**Master Truth Table**

|$p$|$q$|$\neg p$|$p \land q$|$p \lor q$|$p \rightarrow q$|$p \leftrightarrow q$|
|---|---|---|---|---|---|---|
|**T**|**T**|F|**T**|**T**|**T**|**T**|
|**T**|**F**|F|**F**|**T**|**F**|**F**|
|**F**|**T**|T|**F**|**T**|**T**|**F**|
|**F**|**F**|T|**F**|**F**|**T**|**T**|

---

**3. Implication & Conditional Forms**

**Verbal Expressions of** $p \rightarrow q$

In the conditional statement $p \rightarrow q$, $p$ represents the **antecedent (hypothesis)** and $q$ represents the **consequent (conclusion)**. All of the following verbal expressions represent $p \rightarrow q$:

- _"If_ $p$_, then_ $q$_"_
- _"If_ $p$_,_ $q$_"_
- _"_$q$ _if_ $p$_"_
- _"_$q$ _when_ $p$_"_
- _"_$q$ _whenever_ $p$_"_
- _"_$p$ _only if_ $q$_"_
- _"_$q$ _unless_ $\neg p$_"_

---

**Conditional Variations**

Given a conditional statement $p \rightarrow q$:

- **Converse**: $q \rightarrow p$
- **Inverse**: $\neg p \rightarrow \neg q$
- **Contrapositive**: $\neg q \rightarrow \neg p$

**Equivalence Rules**:

- An implication is **logically equivalent** to its contrapositive: $$p \rightarrow q \equiv \neg q \rightarrow \neg p$$
- An implication is **not** equivalent to its converse or inverse: $$p \rightarrow q \not\equiv q \rightarrow p \quad \text{and} \quad p \rightarrow q \not\equiv \neg p \rightarrow \neg q$$
- The **converse** and **inverse** are logically equivalent to each other: $$(q \rightarrow p) \equiv (\neg p \rightarrow \neg q)$$

---

**4. Classification of Logical Expressions**

- **Satisfiable**: The truth table column contains **at least one True** value.
- **Valid (Tautology)**: The truth table column contains **all True** values.
- **Contradiction**: The truth table column contains **all False** values.
- **Contingency**: The truth table column contains **both True and False** values.
- **Structural Hierarchy**: All Tautologies are automatically Satisfiable, and all Contingencies containing at least one True entry are Satisfiable.

---

**5. Laws of Boolean Algebra & Equivalences**

**Fundamental Equivalence Laws**

- **Idempotent Laws**: $a \lor a \equiv a$, $a \land a \equiv a$.
- **Identity & Domination Laws**: $a \lor \text{True} \equiv \text{True}$, $a \land \text{False} \equiv \text{False}$, $a \land \text{True} \equiv a$, $a \lor \text{False} \equiv a$.
- **Complement Laws**: $a \land \neg a \equiv \text{False}$, $a \lor \neg a \equiv \text{True}$.
- **Commutative Laws**: $a \lor b \equiv b \lor a$, $a \land b \equiv b \land a$.
- **Associative Laws**: $a \lor (b \lor c) \equiv (a \lor b) \lor c$, $a \land (b \land c) \equiv (a \land b) \land c$.
- **Distributive Laws**: $a \lor (b \land c) \equiv (a \lor b) \land (a \lor c)$, $a \land (b \lor c) \equiv (a \land b) \lor (a \land c)$.
- **Absorption Laws**: $a \lor (a \land b) \equiv a$, $a \land (a \lor b) \equiv a$.
- **De Morgan's Laws**: $\neg(a \lor b) \equiv \neg a \land \neg b$, $\neg(a \land b) \equiv \neg a \lor \neg b$.

---

**Advanced Implication Equivalence Identities**

- **Disjunctive Representation**: $a \rightarrow b \equiv \neg a \lor b$.
- **Contrapositive Identity**: $a \rightarrow b \equiv \neg b \rightarrow \neg a$.
- **Distributing Implication**:
    - $(a \rightarrow b) \land (a \rightarrow c) \equiv a \rightarrow (b \land c)$
    - $(a \rightarrow c) \land (b \rightarrow c) \equiv (a \lor b) \rightarrow c$
    - $(a \rightarrow b) \lor (a \rightarrow c) \equiv a \rightarrow (b \lor c)$
    - $(a \rightarrow c) \lor (b \rightarrow c) \equiv (a \land b) \rightarrow c$
    - $[p \rightarrow (q \lor r)] \equiv [(p \land \neg q) \rightarrow r]$

---

**6. Solved Practice Problems: Propositional Logic**

**Problem: Algebraic Expression Simplification**

Reduce the expression: $(p \land q) \lor (p \land \neg q) \lor (p \land q \land r)$

- **Step 1**: Group the first two terms using the Distributive Law: $$(p \land q) \lor (p \land \neg q) \equiv p \land (q \lor \neg q)$$
- **Step 2**: Apply the Complement Law ($q \lor \neg q \equiv \text{True}$): $$p \land \text{True} \equiv p$$
- **Step 3**: Substitute back into the expression: $$p \lor (p \land q \land r)$$
- **Step 4**: Apply the Absorption Law: $$p \lor (p \land q \land r) \equiv p$$

**Final Result**: $p$

---

**Problem: Boolean Expression Reduction**

Simplify the expression: $(x \land y) \lor (x \land \neg y) \lor (\neg x \land y) \lor (\neg x \land \neg y)$

- **Step 1**: Group terms using the Distributive Law: $$[x \land (y \lor \neg y)] \lor [\neg x \land (y \lor \neg y)]$$
- **Step 2**: Apply the Complement Law ($y \lor \neg y \equiv \text{True}$): $$(x \land \text{True}) \lor (\neg x \land \text{True}) \equiv x \lor \neg x$$
- **Step 3**: Apply the Complement Law: $$x \lor \neg x \equiv \text{True} \quad (1)$$

**Final Result**: $\text{True} \quad (1)$

---

**Problem: Three-Variable Reduction**

Simplify: $(x \land y \land \neg z) \lor (x \land \neg y \land z) \lor (x \land \neg y \land \neg z)$

- **Step 1**: Group the last two terms: $$x \land \neg y \land (z \lor \neg z) \equiv x \land \neg y \land \text{True} \equiv x \land \neg y$$
- **Step 2**: Combine with the first term: $$(x \land y \land \neg z) \lor (x \land \neg y) \equiv x \land [(y \land \neg z) \lor \neg y]$$
- **Step 3**: Apply the Distributive Law inside the brackets: $$x \land [(\neg y \lor y) \land (\neg y \lor \neg z)] \equiv x \land [\text{True} \land (\neg y \lor \neg z)] \equiv x \land (\neg y \lor \neg z)$$

**Final Result**: $x \land (\neg y \lor \neg z)$

---

**Problem: Circuit Network Simplification**

Simplify the network: $[p \land (\neg r \lor q \lor \neg q)] \lor [(r \lor t \lor \neg r) \land \neg q]$

- **Step 1**: Evaluate the first branch: $$q \lor \neg q \equiv \text{True} \implies \neg r \lor \text{True} \equiv \text{True} \implies p \land \text{True} \equiv p$$
- **Step 2**: Evaluate the second branch: $$r \lor \neg r \equiv \text{True} \implies \text{True} \lor t \equiv \text{True} \implies \text{True} \land \neg q \equiv \neg q$$
- **Step 3**: Combine the results: $$p \lor \neg q$$

**Final Result**: **$p \lor \neg q$# **Mathematical Logic Complete Revision Notes**

---

**1. Propositional Logic & Truth Tables**

**Core Definitions**

- **Propositional Statement**: A declarative assertion that evaluates strictly to either **True (1)** or **False (0)**.
- **Atomic Propositions**: Base statements that cannot be decomposed further.
- **Compound Propositions**: Statements joined by logical connectives.

---

**Logical Connectives & Master Truth Table**

|Connective Name|Symbol|Operator Type|Truth Rule|
|---|---|---|---|
|**Negation**|$\neg$ or $\sim$|Modifier|Inverts truth value ($\neg \text{True} = \text{False}$, $\neg \text{False} = \text{True}$).|
|**Conjunction**|$\land$|Binary (AND)|True **only** when both components are True.|
|**Disjunction**|$\lor$|Binary (OR)|True if **at least one** component is True.|
|**Single Implication**|$\rightarrow$|Binary (IF... THEN)|False **only** when antecedent is True and consequent is False.|
|**Double Implication**|$\leftrightarrow$|Binary (IF AND ONLY IF)|True **only** when both components share identical truth values.|

|$p$|$q$|$\neg p$|$p \land q$|$p \lor q$|$p \rightarrow q$|$p \leftrightarrow q$|
|---|---|---|---|---|---|---|
|**T**|**T**|F|**T**|**T**|**T**|**T**|
|**T**|**F**|F|**F**|**T**|**F**|**F**|
|**F**|**T**|T|**F**|**T**|**T**|**F**|
|**F**|**F**|T|**F**|**F**|**T**|**T**|

---

**Implication & Expressions**

For a conditional statement $p \rightarrow q$:

- **Verbal Expressions**: _"If_ $p$_, then_ $q$_"_, _"_$p$ _only if_ $q$_"_, _"_$q$ _if_ $p$_"_, _"_$q$ _whenever_ $p$_"_, _"_$q$ _unless_ $\neg p$_"_.
- **Converse**: $q \rightarrow p$
- **Inverse**: $\neg p \rightarrow \neg q$
- **Contrapositive**: $\neg q \rightarrow \neg p$ (Logically equivalent to $p \rightarrow q$)

---

**Classifications of Expressions**

- **Satisfiable**: Column contains at least one True.
- **Valid (Tautology)**: Column contains all True.
- **Contradiction**: Column contains all False.
- **Contingency**: Column contains both True and False.

---

**Boolean Algebra Equivalence Laws**

- **Idempotent**: $a \lor a \equiv a$, $a \land a \equiv a$
- **Identity & Domination**: $a \lor \text{True} \equiv \text{True}$, $a \land \text{False} \equiv \text{False}$, $a \land \text{True} \equiv a$, $a \lor \text{False} \equiv a$
- **Complement**: $a \land \neg a \equiv \text{False}$, $a \lor \neg a \equiv \text{True}$
- **Commutative**: $a \lor b \equiv b \lor a$, $a \land b \equiv b \land a$
- **Associative**: $a \lor (b \lor c) \equiv (a \lor b) \lor c$, $a \land (b \land c) \equiv (a \land b) \land c$
- **Distributive**: $a \lor (b \land c) \equiv (a \lor b) \land (a \lor c)$, $a \land (b \lor c) \equiv (a \land b) \lor (a \land c)$
- **Absorption**: $a \lor (a \land b) \equiv a$, $a \land (a \lor b) \equiv a$
- **De Morgan's**: $\neg(a \lor b) \equiv \neg a \land \neg b$, $\neg(a \land b) \equiv \neg a \lor \neg b$
- **Implication**: $a \rightarrow b \equiv \neg a \lor b \equiv \neg b \rightarrow \neg a$

---

**Key Algebraic Solved Reductions**

1. **Simplify**: $(p \land q) \lor (p \land \neg q) \lor (p \land q \land r)$
    - Factor: $p \land (q \lor \neg q) \equiv p \land \text{True} \equiv p$
    - Combine: $p \lor (p \land q \land r) \equiv \mathbf{p}$ (by Absorption)
2. **Simplify**: $(x \land y) \lor (x \land \neg y) \lor (\neg x \land y) \lor (\neg x \land \neg y)$
    - Factor: $[x \land (y \lor \neg y)] \lor [\neg x \land (y \lor \neg y)] \equiv (x \land \text{True}) \lor (\neg x \land \text{True}) \equiv x \lor \neg x \equiv \mathbf{1 \; (\text{True})}$

---

**2. Rules of Inference & Argument Validity**

**Validity Condition**

An argument $[(P_1 \land P_2 \land \dots \land P_n) \rightarrow Q]$ is **valid** if assuming all premises are True forces conclusion $Q$ to be True (making the conditional a Tautology).

---

**Master Table of Inference Rules**

|Rule of Inference|Premises|Conclusion|Tautological Formula|
|---|---|---|---|
|**Modus Ponens**|$a \rightarrow b, \; a$|$b$|$[a \land (a \rightarrow b)] \rightarrow b$|
|**Modus Tollens**|$a \rightarrow b, \; \neg b$|$\neg a$|$[(a \rightarrow b) \land \neg b] \rightarrow \neg a$|
|**Hypothetical Syllogism**|$a \rightarrow b, \; b \rightarrow c$|$a \rightarrow c$|$[(a \rightarrow b) \land (b \rightarrow c)] \rightarrow (a \rightarrow c)$|
|**Disjunctive Syllogism**|$a \lor b, \; \neg a$|$b$|$[(a \lor b) \land \neg a] \rightarrow b$|
|**Addition**|$a$|$a \lor b$|$a \rightarrow (a \lor b)$|
|**Simplification**|$a \land b$|$a$|$(a \land b) \rightarrow a$|
|**Resolution**|$a \lor b, \; \neg b \lor c$|$a \lor c$|$[(a \lor b) \land (\neg b \lor c)] \rightarrow (a \lor c)$|

---

**Formal Fallacies**

- **Affirming the Consequent**: $a \rightarrow b$, $b \implies a$ (**Invalid**)
- **Denying the Antecedent**: $a \rightarrow b$, $\neg a \implies \neg b$ (**Invalid**)

---

**Worked Proof Example**

- **Premises**: $c$, $c \rightarrow \neg f$, $\neg f \rightarrow \neg a$
- **Target Conclusion**: $\neg a$
- **Proof**:
    - Apply Modus Ponens to $c$ and $c \rightarrow \neg f$: Infer $\neg f$.
    - Apply Modus Ponens to $\neg f$ and $\neg f \rightarrow \neg a$: Infer $\neg a$.
- **Result**: **Valid**.

---

**3. Predicate Logic & Single Quantifiers**

**Definitions & Operations**

- **Open Statement** $P(x)$: Contains variables over a Domain of Discourse ($D$).
- **Universal Quantifier (**$\forall x P(x)$**)**: True if $P(x)$ is True for **all** $x \in D$. False if $\ge 1$ counterexample exists.
- **Existential Quantifier (**$\exists x P(x)$**)**: True if $P(x)$ is True for **at least one** $x \in D$. False if all are False.

---

**Negation Rules**

- $\neg \forall x P(x) \equiv \exists x \neg P(x)$
- $\neg \exists x P(x) \equiv \forall x \neg P(x)$
- $\neg \forall x [P(x) \rightarrow Q(x)] \equiv \exists x [P(x) \land \neg Q(x)]$

---

**Domain Evaluation Matrix**

|Statement|Integer Domain ($\mathbb{Z}$)|Real Domain ($\mathbb{R}$)|Notes / Counterexample|
|---|---|---|---|
|$\forall n (n + 1 > n)$|**True**|**True**|Always holds|
|$\exists n (2n = 3n)$|**True**|**True**|Holds at $n = 0$|
|$\forall n (3n \le 4n)$|**False**|**False**|Fails for negative numbers ($n = -1$)|
|$\exists x (x^2 = 2)$|**False**|**True**|$\sqrt{2} \notin \mathbb{Z}$, but $\sqrt{2} \in \mathbb{R}$|
|$\forall x (2x > x)$|**False**|**False**|Fails at $x = 0$ and $x = -1$|

---

**4. Nested Quantifiers & Advanced Inferences**

**Commutativity & Hierarchy**

- **Same Quantifiers Commute**: $\forall x \forall y P(x,y) \equiv \forall y \forall x P(x,y)$ and $\exists x \exists y P(x,y) \equiv \exists y \exists x P(x,y)$.
- **Mixed Quantifiers Do Not Commute**: $\forall x \exists y P(x,y) \not\equiv \exists y \forall x P(x,y)$.
- **Implication Hierarchy**: $$\forall x \forall y P(x, y) \implies \exists y \forall x P(x, y) \implies \forall x \exists y P(x, y) \implies \exists x \exists y P(x, y)$$

---

**Quantifier Rules of Inference**

- **Universal Specification (US)**: From $\forall x P(x)$, infer $P(a)$ for any arbitrary or specific $a$.
- **Existential Specification (ES)**: From $\exists x P(x)$, infer $P(a)$ for a fixed, specific $a$.
- **Universal Generalization (UG)**: From $P(a)$ for an arbitrary $a$, infer $\forall x P(x)$.
- **Existential Generalization (EG)**: From $P(a)$ for a specific $a$, infer $\exists x P(x)$.

---

**Worked Deductive Proof**

- **Premise 1**: All students in class go to IIT ($\forall x [C(x) \rightarrow T(x)]$).
- **Premise 2**: Some students are not solving questions ($\exists x [C(x) \land \neg Q(x)]$).
- **Target Conclusion**: $\exists x [\neg Q(x) \land T(x)]$.
- **Proof Steps**:
    1. Apply **ES** to Premise 2: Fix student $a$ such that $C(a) \land \neg Q(a)$.
    2. Apply **Simplification**: Infer $C(a)$ and $\neg Q(a)$.
    3. Apply **US** to Premise 1: Infer $C(a) \rightarrow T(a)$.
    4. Apply **Modus Ponens** on $C(a)$ and $C(a) \rightarrow T(a)$: Infer $T(a)$.
    5. Form Conjunction: $\neg Q(a) \land T(a)$.
    6. Apply **EG**: Infer $\exists x [\neg Q(x) \land T(x)]$.
- **Result**: **Valid**.