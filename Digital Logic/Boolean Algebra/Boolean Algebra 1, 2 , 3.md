Master Notes: Boolean Algebra Foundations & Theorems

---

**Section 1: Number Systems & Binary Fundamentals**

**1.1 Base (Radix) Principles**

- **Base (**$R$**) Definition:** A positional number system of base $R$ uses $R$ distinct symbols in the range $[0 \text{ to } R-1]$.
    - **Decimal System (**$R = 10$**):** Uses 10 symbols $\{0, 1, 2, 3, 4, 5, 6, 7, 8, 9\}$.
    - **Binary System (**$R = 2$**):** Uses 2 symbols $\{0, 1\}$.
- **Digit vs. Number:**
    - A **digit** is a single atomic symbol (e.g., $5$ or $1$).
    - A **number** represents a quantitative value composed of one or more digits (e.g., $24_{10}$ or $101_2$).

---

**1.2 Positional Weighting Mechanics**

In any positional number system, the total decimal value is calculated by multiplying each digit by its positional weight ($R^{\text{position}}$), indexed from $0$ starting at the rightmost position (Least Significant Digit):

$$\text{Decimal Value} = \sum_{i=0}^{N-1} (\text{digit}_i \times R^i)$$

---

**1.3 Binary to Decimal Conversion Step-by-Step**

To evaluate any binary string $(b_{n-1} \dots b_1 b_0)_2$:

$$\text{Value}_{10} = (b_{n-1} \cdot 2^{n-1}) + \dots + (b_1 \cdot 2^1) + (b_0 \cdot 2^0)$$

**Worked Examples:**

1. **Convert** $(101)_2$**:** $$(1 \times 2^2) + (0 \times 2^1) + (1 \times 2^0) = 4 + 0 + 1 = 5_{10}$$
2. **Convert** $(1001)_2$**:** $$(1 \times 2^3) + (0 \times 2^2) + (0 \times 2^1) + (1 \times 2^0) = 8 + 0 + 0 + 1 = 9_{10}$$
3. **Convert** $(010010)_2$**:** $$2^4 + 2^1 = 16 + 2 = 18_{10}$$
4. **Convert All-Ones String** $(11111)_2$**:** $$2^4 + 2^3 + 2^2 + 2^1 + 2^0 = 16 + 8 + 4 + 2 + 1 = 31_{10}$$

---

**1.4 Bit Limits & Capacity Formulas**

For $N$ binary variables (bits):

- **Total Combinations (**$2^N$**):** Number of unique states that can be represented.
- **Minimum Numerical Value:** $0$
- **Maximum Numerical Value:** $2^N - 1$

|Bits ($N$)|Total Combinations ($2^N$)|Value Range|Maximum Value ($2^N - 1$)|
|---|---|---|---|
|**1**|$2^1 = 2$|$0 \text{ to } 1$|$1$|
|**2**|$2^2 = 4$|$0 \text{ to } 3$|$3$|
|**3**|$2^3 = 8$|$0 \text{ to } 7$|$7$|
|**4**|$2^4 = 16$|$0 \text{ to } 15$|$15$|

---

**Section 2: Core Boolean Operations & Algebraic Laws**

**2.1 Logical OR Operation (**$+$**)**

The OR operation returns $1$ if at least one input is $1$.

- **Truth Table:**
    - $0 + 0 = 0$
    - $0 + 1 = 1$
    - $1 + 0 = 1$
    - $1 + 1 = 1$
- **Fundamental Laws:**
    1. **Identity Law:** $A + 0 = A$
    2. **Dominance (Null) Law:** $A + 1 = 1$ _(Adding 1 to any logic expression yields 1)_
    3. **Idempotent Law:** $A + A = A$ _(Cascading identical OR terms yields the term itself:_ $A + A + A = A$_)_
    4. **Complement Law:** $A + \bar{A} = 1$
- **Practical Shortcut Rules:**
    - $1 + \text{any expression} = 1$ (e.g., $1 + AB + BCD + A = 1$)
    - $X + \bar{X} = 1$ for any complex term $X$ (e.g., $PQ + \overline{PQ} = 1$)

---

**2.2 Logical AND Operation (**$\cdot$**)**

The AND operation returns $1$ only when all inputs are $1$.

- **Truth Table:**
    - $0 \cdot 0 = 0$
    - $0 \cdot 1 = 0$
    - $1 \cdot 0 = 0$
    - $1 \cdot 1 = 1$
- **Fundamental Laws:**
    1. **Identity Law:** $A \cdot 1 = A$
    2. **Dominance (Null) Law:** $A \cdot 0 = 0$
    3. **Idempotent Law:** $A \cdot A = A$
    4. **Complement Law:** $A \cdot \bar{A} = 0$

---

**2.3 Essential Boolean Expansion & Reduction Theorems**

**Absorption Theorems:**

- $A + AB = A(1 + B) = A(1) = A$
- $A(A + B) = A\cdot A + AB = A + AB = A$

**Redundant Literal Rule:**

- $A + \bar{A}B = (A + \bar{A})(A + B) = 1 \cdot (A + B) = A + B$
- $\bar{A} + AB = (\bar{A} + A)(\bar{A} + B) = 1 \cdot (\bar{A} + B) = \bar{A} + B$

**Distributive Laws:**

- **Standard Distributive:** $A(B + C) = AB + AC$
- **Second Distributive Law (OR over AND):** $$A + BC = (A + B)(A + C)$$
- **Multi-term Expansion:** $$(A + B)(A + C)(A + D) = A + BCD$$

**Multi-Term Reduction Identity:**

$$(A + B + C)(A + B + \bar{C})(\bar{A} + B + C) = B + AC$$

---

**Section 3: DeMorgan’s Theorems & Line-Breaking Logic**

DeMorgan’s Laws allow the distribution of negation across logical operations.

**3.1 The Fundamental Rules**

1. **NAND to OR-Invert Rule:** $$\overline{A \cdot B} = \bar{A} + \bar{B}$$ _(The complement of a product equals the sum of the individual complements)._
2. **NOR to AND-Invert Rule:** $$\overline{A + B} = \bar{A} \cdot \bar{B}$$ _(The complement of a sum equals the product of the individual complements)._

**Memory Rule:** _"Break the line (bar), change the sign (_$+$ _to_ $\cdot$_, or_ $\cdot$ _to_ $+$_)."_

---

**3.2 Generalization for** $N$ **Variables**

- $\overline{X_1 \cdot X_2 \cdots X_n} = \bar{X}_1 + \bar{X}_2 + \dots + \bar{X}_n$
- $\overline{X_1 + X_2 + \dots + X_n} = \bar{X}_1 \cdot \bar{X}_2 \cdots \bar{X}_n$

---

**3.3 Step-by-Step Complement Reductions**

- **Example 1:** $$\overline{PQ + R} = \overline{PQ} \cdot \bar{R} = (\bar{P} + \bar{Q})\bar{R}$$
- **Example 2:** $$\overline{P \cdot S \cdot (PQ + R)} = \bar{P} + \bar{S} + \overline{PQ + R} = \bar{P} + \bar{S} + (\bar{P} + \bar{Q})\bar{R} = \bar{P} + \bar{S} + \bar{Q}\bar{R}$$

---

**Section 4: Sum of Products (SOP) vs. Product of Sums (POS)**

Boolean functions are expressed in two standard canonical forms.

**4.1 Representation Rules**

|Feature|Sum of Products (SOP)|Product of Sums (POS)|
|---|---|---|
|**Building Blocks**|Minterms ($m_i$)|Maxterms ($M_i$)|
|**Focus**|Groups Output $1$**s**|Groups Output $0$**s**|
|**Mathematical Symbol**|Minterm Summation ($\Sigma$)|Maxterm Product ($\Pi$)|
|**Binary 0 Mapping**|Complemented variable ($\bar{A}$)|Uncomplemented variable ($A$)|
|**Binary 1 Mapping**|Uncomplemented variable ($A$)|Complemented variable ($\bar{A}$)|
|**Relation**|$\Sigma \text{ Minterms} + \Pi \text{ Maxterms} = 2^N$ Total States||

---

**4.2 Deriving Standard Forms from a Truth Table**

Consider a 2-variable truth table ($A, B$) and output $Y$:

|Row ($i$)|$A$|$B$|Minterm ($m_i$)|Maxterm ($M_i$)|Output ($Y$)|
|---|---|---|---|---|---|
|**0**|0|0|$\bar{A}\bar{B}$|$A + B$|**0**|
|**1**|0|1|$\bar{A}B$|$A + \bar{B}$|**1**|
|**2**|1|0|$A\bar{B}$|$\bar{A} + B$|**1**|
|**3**|1|1|$AB$|$\bar{A} + \bar{B}$|**1**|

- **Canonical SOP Form (**$\Sigma$**):** Take rows where $Y = 1$ (Rows 1, 2, 3): $$Y = \Sigma m(1, 2, 3) = \bar{A}B + A\bar{B} + AB$$
    - **Minimal SOP Reduction:** $$\bar{A}B + A(\bar{B} + B) = \bar{A}B + A = A + B$$
- **Canonical POS Form (**$\Pi$**):** Take rows where $Y = 0$ (Row 0): $$Y = \Pi M(0) = (A + B)$$

---

**Section 5: The Redundancy (Consensus) Theorem**

The Consensus Theorem allows the elimination of redundant terms in Boolean expressions without expanding them completely.

**5.1 Algebraic Formulations**

- **SOP Form:** $$AB + \bar{A}C + BC = AB + \bar{A}C$$
- **POS Form:** $$(A + B)(\bar{A} + C)(B + C) = (A + B)(\bar{A} + C)$$

---

**5.2 Proof of Consensus Theorem (SOP Form)**

$$LHS = AB + \bar{A}C + BC$$ $$= AB + \bar{A}C + BC(1)$$ $$= AB + \bar{A}C + BC(A + \bar{A})$$ $$= AB + \bar{A}C + ABC + \bar{A}BC$$ $$= AB(1 + C) + \bar{A}C(1 + B)$$ $$= AB(1) + \bar{A}C(1) = AB + \bar{A}C = RHS$$

---

**5.3 The 3-Step Checklist to Spot Redundant Terms**

To eliminate a term using consensus, confirm all three conditions:

1. The expression contains **3 distinct variables** (e.g., $A, B, C$).
2. One variable appears in **complemented and uncomplemented forms** ($A$ and $\bar{A}$) across two terms.
3. The remaining variables combine to form the **third term** ($BC$). _This third term is redundant and can be deleted._

---

**Section 6: Comprehensive Practice Problem Set**

**Problem 1: Algebraic Simplification**

**Simplify:** $F = A\bar{B}C + A\bar{B}\bar{C} + AB\bar{C} + ABC$

- **Solution:**
    1. Factor out $A\bar{B}$ from the first two terms: $$A\bar{B}(C + \bar{C}) = A\bar{B}(1) = A\bar{B}$$
    2. Factor out $AB$ from the last two terms: $$AB(\bar{C} + C) = AB(1) = AB$$
    3. Combine the results and factor out $A$: $$A\bar{B} + AB = A(\bar{B} + B) = A(1) = A$$
- **Final Answer:** $F = A$

---

**Problem 2: Consensus Theorem Elimination**

**Simplify:** $Y = \bar{W}X + WZ + XZ$

- **Solution:**
    1. Identify 3 variables ($W, X, Z$).
    2. Locate complemented/uncomplemented pair ($W$ in $WZ$ and $\bar{W}$ in $\bar{W}X$).
    3. Combine the remaining variables ($X$ and $Z$) to form the term $XZ$.
    4. Drop the consensus term $XZ$.
- **Final Answer:** $Y = \bar{W}X + WZ$

---

**Problem 3: Canonical SOP to POS Conversion**

**Given:** $F(A, B, C) = \Sigma m(0, 2, 4, 5, 7)$ **Find:** Canonical POS expression.

- **Solution:**
    1. Total combinations for 3 variables = $2^3 = 8$ (indices $0$ through $7$).
    2. Maxterm indices are the missing minterm indices: $\{1, 3, 6\}$.
    3. Write in product notation: $$F(A, B, C) = \Pi M(1, 3, 6)$$
    4. Convert each index to its maxterm sum:
        - Index $1 = 001_2 \rightarrow (A + B + \bar{C})$
        - Index $3 = 011_2 \rightarrow (A + \bar{B} + \bar{C})$
        - Index $6 = 110_2 \rightarrow (\bar{A} + \bar{B} + C)$
- **Final Answer:** $F = (A + B + \bar{C})(A + \bar{B} + \bar{C})(\bar{A} + \bar{B} + C)$

---

**Problem 4: Finding the Complement of a Logic Function**

**Find the complement (**$\bar{Y}$**) of:** $Y = AB + \bar{A}C$

- **Solution:**
    1. Apply DeMorgan’s Law to the sum: $$\bar{Y} = \overline{AB + \bar{A}C} = (\overline{AB}) \cdot (\overline{\bar{A}C})$$
    2. Apply DeMorgan’s Law to each product: $$\overline{AB} = (\bar{A} + \bar{B})$$ $$\overline{\bar{A}C} = (A + \bar{C})$$
    3. Combine terms: $$\bar{Y} = (\bar{A} + \bar{B})(A + \bar{C})$$
    4. Expand using distributive property: $$\bar{Y} = \bar{A}A + \bar{A}\bar{C} + A\bar{B} + \bar{B}\bar{C}$$ Since $\bar{A}A = 0$: $$\bar{Y} = A\bar{B} + \bar{A}\bar{C} + \bar{B}\bar{C}$$
    5. Apply Consensus Theorem ($A\bar{B} + \bar{A}\bar{C} + \bar{B}\bar{C} \rightarrow \bar{B}\bar{C} \text{ is redundant}$): $$\bar{Y} = A\bar{B} + \bar{A}\bar{C}$$
- **Final Answer:** $\bar{Y} = (\bar{A} + \bar{B})(A + \bar{C}) = A\bar{B} + \bar{A}\bar{C}$