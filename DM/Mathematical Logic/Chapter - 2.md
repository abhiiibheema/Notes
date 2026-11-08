
**Rules of Inference & Argument Validity**

---

**Argument Structure & Validity**

**Arguments, Premises, and Conclusions**

- **Logical Argument**: A sequence of statements consisting of one or more **premises** ($P_1, P_2, \dots, P_n$) followed by a final statement called the **conclusion** ($Q$).
- **Symbolic Structure**: An argument is formally represented as a single conditional implication where the conjunction of all premises implies the conclusion: $$[(P_1 \land P_2 \land \dots \land P_n) \rightarrow Q]$$

---

**Definition of Argument Validity**

- **Valid Argument**: An argument is **valid** if and only if assuming all premises are True guarantees that the conclusion $Q$ is also True.
- **Tautology Test**: An argument structure is valid if the conditional formula $[(P_1 \land P_2 \land \dots \land P_n) \rightarrow Q]$ is a **Tautology** (evaluates to True under all truth value assignments).
- **Invalid Argument (Fallacy)**: An argument is **invalid** if there exists at least one scenario where every premise evaluates to True, but the conclusion $Q$ evaluates to False.

---

**Core Rules of Inference**

Rules of inference are foundational valid argument forms used to deduce conclusions step-by-step.

---

**Modus Ponens (Law of Detachment)**

- **Rule**: If an implication $a \rightarrow b$ is True and the antecedent $a$ is True, then the consequent $b$ must be True.
- **Argument Form**: $$\frac{\begin{array}{l} a \rightarrow b \\ a \end{array}}{\therefore b}$$
- **Tautological Formula**: $[a \land (a \rightarrow b)] \rightarrow b$

---

**Modus Tollens (Law of Denying the Consequent)**

- **Rule**: If an implication $a \rightarrow b$ is True and the consequent is False ($\neg b$), then the antecedent must be False ($\neg a$).
- **Argument Form**: $$\frac{\begin{array}{l} a \rightarrow b \\ \neg b \end{array}}{\therefore \neg a}$$
- **Tautological Formula**: $[(a \rightarrow b) \land \neg b] \rightarrow \neg a$

---

**Hypothetical Syllogism (Chain Rule)**

- **Rule**: If $a$ implies $b$ and $b$ implies $c$, then $a$ implies $c$.
- **Argument Form**: $$\frac{\begin{array}{l} a \rightarrow b \\ b \rightarrow c \end{array}}{\therefore a \rightarrow c}$$
- **Tautological Formula**: $[(a \rightarrow b) \land (b \rightarrow c)] \rightarrow (a \rightarrow c)$

---

**Disjunctive Syllogism**

- **Rule**: If a disjunction $a \lor b$ is True and one of the disjuncts is False ($\neg a$), the remaining disjunct must be True ($b$).
- **Argument Form**: $$\frac{\begin{array}{l} a \lor b \\ \neg a \end{array}}{\therefore b}$$
- **Tautological Formula**: $[(a \lor b) \land \neg a] \rightarrow b$

---

**Addition Rule**

- **Rule**: From any true proposition $a$, one can validly infer the disjunction $a \lor b$ for any proposition $b$.
- **Argument Form**: $$\frac{a}{\therefore a \lor b}$$
- **Tautological Formula**: $a \rightarrow (a \lor b)$

---

**Simplification Rule**

- **Rule**: If a conjunction $a \land b$ is True, then each component proposition $a$ (or $b$) is independently True.
- **Argument Form**: $$\frac{a \land b}{\therefore a}$$
- **Tautological Formula**: $(a \land b) \rightarrow a$

---

**Resolution Rule**

- **Rule**: If $a \lor b$ is True and $\neg b \lor c$ is True, combining them resolves the variable $b$ to yield $a \lor c$.
- **Argument Form**: $$\frac{\begin{array}{l} a \lor b \\ \neg b \lor c \end{array}}{\therefore a \lor c}$$
- **Tautological Formula**: $[(a \lor b) \land (\neg b \lor c)] \rightarrow (a \lor c)$

---

**Formal Logical Fallacies**

A formal fallacy occurs when an argument structure superficially resembles a valid rule of inference but allows for a False conclusion despite True premises.

---

**Fallacy of Affirming the Consequent**

- **Invalid Form**: $$\frac{\begin{array}{l} a \rightarrow b \\ b \end{array}}{\therefore a} \quad \text{(Invalid)}$$
- **Explanation**: Knowing that $b$ is True does not guarantee that $a$ occurred; $b$ can be True while $a$ is False, keeping the implication $a \rightarrow b$ True while $a$ remains False.

---

**Fallacy of Denying the Antecedent**

- **Invalid Form**: $$\frac{\begin{array}{l} a \rightarrow b \\ \neg a \end{array}}{\therefore \neg b} \quad \text{(Invalid)}$$
- **Explanation**: Knowing that $a$ is False does not mean $b$ must be False; $b$ can still evaluate to True through other conditions.

---

**Summary Table of Inference Rules**

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

**Worked Examples**

**Step-by-Step Deductive Proof**

Evaluate the validity of the following argument:

- Premise: Rita is baking a cake ($c$).
- Premise: If Rita is baking a cake ($c$), then she is not practicing her flute ($\neg f$).
- Premise: If Rita is not practicing her flute ($\neg f$), then her father will not buy her a car ($\neg a$).
- **Conclusion**: Rita's father will not buy her a car ($\neg a$).

**Proof Steps**:

- Apply Modus Ponens to $c$ and $c \rightarrow \neg f$ to infer: $\neg f$.
- Apply Modus Ponens to $\neg f$ and $\neg f \rightarrow \neg a$ to infer: $\neg a$.
- The derived statement matches the target conclusion.

**Result**: Valid

---

**Multi-Premise Proof**

Verify the validity of the argument with premises $a$, $a \rightarrow b$, and $\neg b \lor c$, with conclusion $c$.

**Proof Steps**:

- From premises $a$ and $a \rightarrow b$, apply Modus Ponens to infer $b$.
- Take premise $\neg b \lor c$ alongside the derived fact $b$.
- Apply Disjunctive Syllogism on $\neg b \lor c$ and $b$ to infer $c$.

**Result**: Valid

---

**Verbal Statement Analysis**

Determine which of the following inferences is correct:

- **Inference A**: _"If it rains (_$r$_), then the cricket match will not be played (_$\neg p$_). The cricket match was played (_$p$_). Therefore, there was no rain (_$\neg r$_)."_
- **Inference B**: _"If it rains (_$r$_), then the cricket match will not be played (_$\neg p$_). It did not rain (_$\neg r$_). Therefore, the cricket match was played (_$p$_)."_

**Analysis**:

- **Inference A**: Premise $r \rightarrow \neg p$ and premise $p$ (which is $\neg(\neg p)$). By Modus Tollens, $p$ and $r \rightarrow \neg p$ yield $\neg r$. **(Valid)**
- **Inference B**: Premise $r \rightarrow \neg p$ and premise $\neg r$. Attempting to conclude $p$ commits the Fallacy of Denying the Antecedent. **(Invalid)**

**Result**: Inference A is valid; Inference B is invalid.