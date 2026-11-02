Master Notes: Complete Digital Logic & Circuit Optimization

---

**PART 1: Boolean Algebra Foundations & Theorems**

**1. Number Systems & Binary Fundamentals**

**1.1 Base (Radix) Principles**

- **Base (**$R$**) Definition:** A positional number system of base $R$ uses $R$ distinct symbols in the range $[0 \text{ to } R-1]$.
    - **Decimal System (**$R = 10$**):** Uses 10 symbols $\{0, 1, 2, 3, 4, 5, 6, 7, 8, 9\}$.
    - **Binary System (**$R = 2$**):** Uses 2 symbols $\{0, 1\}$.
- **Digit vs. Number:**
    - A **digit** is a single atomic symbol (e.g., $5$ or $1$).
    - A **number** represents a quantitative value composed of one or more digits (e.g., $24_{10}$ or $101_2$).

**1.2 Positional Weighting Mechanics**

In any positional number system, the total decimal value is calculated by multiplying each digit by its positional weight ($R^{\text{position}}$), indexed from $0$ starting at the rightmost position (Least Significant Digit):

$$\text{Decimal Value} = \sum_{i=0}^{N-1} (\text{digit}_i \times R^i)$$

**1.3 Binary to Decimal Conversion Step-by-Step**

To evaluate any binary string $(b_{n-1} \dots b_1 b_0)_2$:

$$\text{Value}_{10} = (b_{n-1} \cdot 2^{n-1}) + \dots + (b_1 \cdot 2^1) + (b_0 \cdot 2^0)$$

- **Convert** $(101)_2$**:** $(1 \times 2^2) + (0 \times 2^1) + (1 \times 2^0) = 4 + 0 + 1 = 5_{10}$
- **Convert** $(1001)_2$**:** $(1 \times 2^3) + (0 \times 2^2) + (0 \times 2^1) + (1 \times 2^0) = 8 + 0 + 0 + 1 = 9_{10}$
- **Convert** $(010010)_2$**:** $2^4 + 2^1 = 16 + 2 = 18_{10}$
- **Convert All-Ones String** $(11111)_2$**:** $2^4 + 2^3 + 2^2 + 2^1 + 2^0 = 16 + 8 + 4 + 2 + 1 = 31_{10}$

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

**2. Core Boolean Operations & Algebraic Laws**

**2.1 Logical OR Operation (**$+$**)**

The OR operation returns $1$ if at least one input is $1$.

- **Truth Table:** $0+0=0, \quad 0+1=1, \quad 1+0=1, \quad 1+1=1$
- **Fundamental Laws:**
    1. **Identity Law:** $A + 0 = A$
    2. **Dominance (Null) Law:** $A + 1 = 1$
    3. **Idempotent Law:** $A + A = A$
    4. **Complement Law:** $A + \bar{A} = 1$
- **Practical Rules:**
    - $1 + \text{any expression} = 1$
    - $X + \bar{X} = 1$ for any complex term $X$ (e.g., $PQ + \overline{PQ} = 1$)

**2.2 Logical AND Operation (**$\cdot$**)**

The AND operation returns $1$ only when all inputs are $1$.

- **Truth Table:** $0\cdot 0=0, \quad 0\cdot 1=0, \quad 1\cdot 0=0, \quad 1\cdot 1=1$
- **Fundamental Laws:**
    1. **Identity Law:** $A \cdot 1 = A$
    2. **Dominance (Null) Law:** $A \cdot 0 = 0$
    3. **Idempotent Law:** $A \cdot A = A$
    4. **Complement Law:** $A \cdot \bar{A} = 0$

**2.3 Essential Boolean Expansion & Reduction Theorems**

- **Absorption Theorems:**
    - $A + AB = A(1 + B) = A$
    - $A(A + B) = A\cdot A + AB = A + AB = A$
- **Redundant Literal Rule:**
    - $A + \bar{A}B = (A + \bar{A})(A + B) = A + B$
    - $\bar{A} + AB = (\bar{A} + A)(\bar{A} + B) = \bar{A} + B$
- **Distributive Laws:**
    - **Standard Distributive:** $A(B + C) = AB + AC$
    - **Second Distributive Law (OR over AND):** $A + BC = (A + B)(A + C)$
    - **Multi-term Expansion:** $(A + B)(A + C)(A + D) = A + BCD$
- **Multi-Term Reduction Identity:** $$(A + B + C)(A + B + \bar{C})(\bar{A} + B + C) = B + AC$$

---

**3. DeMorgan’s Theorems & Line-Breaking Logic**

**3.1 The Fundamental Rules**

1. **NAND to OR-Invert Rule:** $$\overline{A \cdot B} = \bar{A} + \bar{B}$$ _(The complement of a product equals the sum of the individual complements)._
2. **NOR to AND-Invert Rule:** $$\overline{A + B} = \bar{A} \cdot \bar{B}$$ _(The complement of a sum equals the product of the individual complements)._

**Memory Rule:** _"Break the line (bar), change the sign (_$+$ _to_ $\cdot$_, or_ $\cdot$ _to_ $+$_)."_

**3.2 Generalization & Multi-Variable Reduction**

- $\overline{X_1 \cdot X_2 \cdots X_n} = \bar{X}_1 + \bar{X}_2 + \dots + \bar{X}_n$
- $\overline{X_1 + X_2 + \dots + X_n} = \bar{X}_1 \cdot \bar{X}_2 \cdots \bar{X}_n$
- **Example:** $\overline{PQ + R} = \overline{PQ} \cdot \bar{R} = (\bar{P} + \bar{Q})\bar{R}$

---

**4. Sum of Products (SOP) vs. Product of Sums (POS)**

**4.1 Representation Rules**

|Feature|Sum of Products (SOP)|Product of Sums (POS)|
|---|---|---|
|**Building Blocks**|Minterms ($m_i$)|Maxterms ($M_i$)|
|**Focus**|Groups Output $1$**s**|Groups Output $0$**s**|
|**Symbol**|Minterm Summation ($\Sigma$)|Maxterm Product ($\Pi$)|
|**Binary 0 Mapping**|Complemented variable ($\bar{A}$)|Uncomplemented variable ($A$)|
|**Binary 1 Mapping**|Uncomplemented variable ($A$)|Complemented variable ($\bar{A}$)|

**4.2 Deriving Forms from Truth Table**

|Row ($i$)|$A$|$B$|Minterm ($m_i$)|Maxterm ($M_i$)|Output ($Y$)|
|---|---|---|---|---|---|
|**0**|0|0|$\bar{A}\bar{B}$|$A + B$|**0**|
|**1**|0|1|$\bar{A}B$|$A + \bar{B}$|**1**|
|**2**|1|0|$A\bar{B}$|$\bar{A} + B$|**1**|
|**3**|1|1|$AB$|$\bar{A} + \bar{B}$|**1**|

- **Canonical SOP:** $Y = \Sigma m(1, 2, 3) = \bar{A}B + A\bar{B} + AB = A + B$
- **Canonical POS:** $Y = \Pi M(0) = (A + B)$

---

**5. The Redundancy (Consensus) Theorem**

**5.1 Formulations**

- **SOP Form:** $AB + \bar{A}C + BC = AB + \bar{A}C$
- **POS Form:** $(A + B)(\bar{A} + C)(B + C) = (A + B)(\bar{A} + C)$

**5.2 The 3-Step Checklist**

To eliminate a term using consensus, confirm:

1. The expression contains **3 distinct variables** (e.g., $A, B, C$).
2. One variable appears in **complemented and uncomplemented forms** ($A$ and $\bar{A}$) across two terms.
3. The remaining variables combine to form the **third term** ($BC$). _This third term is redundant and can be deleted._

---

**6. Part 1 Practice Problems**

- **Problem 1:** Simplify $F = A\bar{B}C + A\bar{B}\bar{C} + AB\bar{C} + ABC$.
    - _Solution:_ $A\bar{B}(C + \bar{C}) + AB(\bar{C} + C) = A\bar{B} + AB = A(\bar{B} + B) = \mathbf{A}$.
- **Problem 2:** Simplify $Y = \bar{W}X + WZ + XZ$.
    - _Solution:_ Variable $W$ appears complemented/uncomplemented in $\bar{W}X$ and $WZ$. $XZ$ is the consensus term. Drop $XZ \Rightarrow \mathbf{\bar{W}X + WZ}$.
- **Problem 3:** Given $F(A, B, C) = \Sigma m(0, 2, 4, 5, 7)$, find POS.
    - _Solution:_ Missing indices are $\{1, 3, 6\} \Rightarrow \Pi M(1, 3, 6) = \mathbf{(A + B + \bar{C})(A + \bar{B} + \bar{C})(\bar{A} + \bar{B} + C)}$.

---

**PART 2: Logic Gates, Properties & Circuit Simplification**

**1. Control Logic (Enable & Disable Circuits)**

- **OR Gate:** Control Input = $0 \rightarrow$ **Enable** ($A + 0 = A$); Control Input = $1 \rightarrow$ **Disable** ($A + 1 = 1$).
- **AND Gate:** Control Input = $1 \rightarrow$ **Enable** ($A \cdot 1 = A$); Control Input = $0 \rightarrow$ **Disable** ($A \cdot 0 = 0$).
- **XOR Gate:** Control = $0 \rightarrow$ Buffer ($A \oplus 0 = A$); Control = $1 \rightarrow$ Inverter ($A \oplus 1 = \bar{A}$).
- **XNOR Gate:** Control = $0 \rightarrow$ Inverter ($A \odot 0 = \bar{A}$); Control = $1 \rightarrow$ Buffer ($A \odot 1 = A$).

---

**2. Arithmetic Logic Gates (XOR & XNOR)**

**2.1 Exclusive-OR (XOR) Gate (**$\oplus$**)**

Acts as an **Odd Function Detector** (returns $1$ when an odd number of inputs are $1$).

- **Algebraic Expression:** $A \oplus B = \bar{A}B + A\bar{B}$
- **Canonical Forms:** SOP $= \Sigma m(1, 2)$; POS $= \Pi M(0, 3)$
- **Truth Table:** $0 \oplus 0 = 0, \quad 0 \oplus 1 = 1, \quad 1 \oplus 0 = 1, \quad 1 \oplus 1 = 0$

**2.2 Exclusive-NOR (XNOR) Gate (**$\odot$**)**

Acts as an **Equivalence Detector** or **Even Function Detector** (returns $1$ when inputs are equal).

- **Algebraic Expression:** $A \odot B = \bar{A}\bar{B} + AB$
- **Canonical Forms:** SOP $= \Sigma m(0, 3)$; POS $= \Pi M(1, 2)$
- **Truth Table:** $0 \odot 0 = 1, \quad 0 \odot 1 = 0, \quad 1 \odot 0 = 0, \quad 1 \odot 1 = 1$

---

**3. Fundamental Identities & Exchange Properties**

- **XOR Identities:** $A \oplus A = 0, \quad A \oplus \bar{A} = 1, \quad A \oplus 0 = A, \quad A \oplus 1 = \bar{A}$
- **XNOR Identities:** $A \odot A = 1, \quad A \odot \bar{A} = 0, \quad A \odot 0 = \bar{A}, \quad A \odot 1 = A$
- **Exchange Property:** $$A \oplus B = C \implies A \oplus C = B \quad \text{and} \quad B \oplus C = A$$
- **Complementation Rules:** $$\bar{A} \oplus B = A \oplus \bar{B} = A \odot B$$ $$\bar{A} \oplus \bar{B} = A \oplus B$$

---

**4. Advanced Theorems & Operator Behavior**

**4.1 Parity Rules & Matrix Relationship**

|Inputs|Relationship|Formula|
|---|---|---|
|**Odd Number of Inputs**|Functions are **Identical**|$A \oplus B \oplus C = A \odot B \odot C = \Sigma m(1, 2, 4, 7)$|
|**Even Number of Inputs**|Functions are **Complements**|$A \oplus B = \overline{A \odot B}$|

**4.2 Distributive Property & Cascades**

- AND is distributive over XOR: $A \cdot (B \oplus C) = AB \oplus AC$
- $N$-stage XOR cascade ($A \oplus A \dots$): Output is $0$ if $N$ is even; $A$ if $N$ is odd.
- $N$-stage XNOR cascade ($A \odot A \dots$): Output is $1$ if $N$ is even; $A$ if $N$ is odd.

---

**5. Part 2 Practice Problems**

- **Problem 1:** Simplify $E = AB \oplus BC$.
    - _Solution:_ $(\overline{AB})BC + AB(\overline{BC}) = (\bar{A} + \bar{B})BC + AB(\bar{B} + \bar{C}) = \bar{A}BC + AB\bar{C} = \mathbf{B(A \oplus C)}$.
- **Problem 2:** Simplify $P \oplus QP$.
    - _Solution:_ $\bar{P}(QP) + P(\overline{QP}) = 0 + P(\bar{Q} + \bar{P}) = \mathbf{P\bar{Q}}$.
- **Problem 3:** Two XOR gates: Gate 1 receives $\bar{A}, B$ (output $A \odot B$), Gate 2 receives $\bar{B}, A$ (output $A \odot B$). Gate 3 XORs them.
    - _Solution:_ $(A \odot B) \oplus (A \odot B) = \mathbf{0}$.

---

**PART 3: Universal Gates & NAND/NOR Implementations**

**1. Gate Universality**

A logic gate is **Universal** if any Boolean function can be implemented using **only that gate type**. **NAND** and **NOR** are the two universal gates.

---

**2. NAND Gate Fundamentals**

- **Expression:** $Y = \overline{A \cdot B}$
- **Dual Representation:** Equivalent to a **Bubbled OR gate** ($\bar{A} + \bar{B}$).
- **Operational Rules:** Any input $0 \rightarrow Y = 1$; all inputs $1 \rightarrow Y = 0$.
- **Properties:** Commutative ($\overline{AB} = \overline{BA}$), **Not Associative** ($\overline{\overline{AB}C} \neq \overline{A\overline{BC}}$).

---

**3. NOR Gate Fundamentals**

- **Expression:** $Y = \overline{A + B}$
- **Dual Representation:** Equivalent to a **Bubbled AND gate** ($\bar{A} \cdot \bar{B}$).
- **Operational Rules:** Any input $1 \rightarrow Y = 0$; all inputs $0 \rightarrow Y = 1$.
- **Properties:** Commutative, **Not Associative**.

---

**4. Master Gate Conversion & Minimum Gate Count Table**

|Target Gate|Minimum NAND Gates Required|Minimum NOR Gates Required|
|---|---|---|
|**NOT**|**1**|**1**|
|**AND**|**2**|**3**|
|**OR**|**3**|**2**|
|**XOR**|**4**|**5**|
|**XNOR**|**5**|**4**|
|**NAND**|**1**|**4**|
|**NOR**|**4**|**1**|

---

**5. Two-Level Logic Realizations**

- **SOP Expressions** $\rightarrow$ Implemented using a **2-Level NAND-NAND network**.
- **POS Expressions** $\rightarrow$ Implemented using a **2-Level NOR-NOR network**.

---

**6. Part 3 Practice Problems**

- **Problem 1:** Calculate minimum NAND gates for $F = AB + C$.
    - _Solution:_ $F = \overline{\overline{AB + C}} = \overline{\overline{AB} \cdot \bar{C}}$. Gate 1 calculates $\overline{AB}$, Gate 2 calculates $\bar{C}$, Gate 3 combines them. Total = **3 NAND gates**.
- **Problem 2:** Implement $Y = (A + B)(C + D)$ using NOR gates.
    - _Solution:_ $Y = \overline{\overline{(A+B)(C+D)}} = \overline{\overline{A+B} + \overline{C+D}}$. Gate 1: $\overline{A+B}$, Gate 2: $\overline{C+D}$, Gate 3 combines them. Total = **3 NOR gates**.

---

**PART 4: Karnaugh Map (K-Map) Optimization**

**1. Fundamentals & Cell Mapping**

**1.1 Gray Code Adjacency**

K-Maps order adjacent cells using Gray Code ($00 \rightarrow 01 \rightarrow 11 \rightarrow 10$) so that adjacent cells differ by exactly **1 binary bit**. Top/bottom rows and left/right columns exhibit **wrap-around adjacency**.

**1.2 Grid Layouts**

- **3-Variable Map (**$P \backslash QR$**):**

|$P \backslash QR$|$00$|$01$|$11$|$10$|
|---|---|---|---|---|
|**0**|Cell 0|Cell 1|Cell 3|Cell 2|
|**1**|Cell 4|Cell 5|Cell 7|Cell 6|

- **4-Variable Map (**$PQ \backslash RS$**):**

|$PQ \backslash RS$|$00$|$01$|$11$|$10$|
|---|---|---|---|---|
|**00**|Cell 0|Cell 1|Cell 3|Cell 2|
|**01**|Cell 4|Cell 5|Cell 7|Cell 6|
|**11**|Cell 12|Cell 13|Cell 15|Cell 14|
|**10**|Cell 8|Cell 9|Cell 11|Cell 10|

---

**2. POS K-Map Minimization Rules**

1. **Group** $0$**s** instead of $1$s.
2. Binary $0 \rightarrow$ Uncomplemented variable ($P$); Binary $1 \rightarrow$ Complemented variable ($\bar{P}$).
3. **Don't Cares (**$d$ **or** $X$**):** Treat as $0$ if they enlarge a group of $0$s; otherwise ignore (treat as $1$).
4. Group sizes must be powers of 2 ($1, 2, 4, 8, 16$).

---

**3. Implicant Classification**

- **Implicant:** Any individual minterm cell containing $1$ (SOP) or maxterm cell containing $0$ (POS).
- **Prime Implicant (PI):** The largest possible valid group of adjacent cells that cannot be covered by a larger group.
- **Essential Prime Implicant (EPI):** A PI containing at least one $1$ (or $0$) that is **not covered by any other PI**.
- **Redundant Prime Implicant (RPI):** A PI whose terms are all completely covered by EPIs.

---

**4. Part 4 Practice Problems**

- **Problem 1:** Minimize $f(P, Q, R) = \Pi M(0, 1, 3, 6, 7)$ using POS K-Map.
    - _Solution:_
        - Group 1 (Cells 0, 1): $(P + Q)$
        - Group 2 (Cells 1, 3): $(P + \bar{R})$
        - Group 3 (Cells 6, 7): $(\bar{P} + \bar{Q})$
        - Result: $\mathbf{F = (P + Q)(\bar{P} + \bar{Q})(P + \bar{R})}$.
- **Problem 2:** Given $F(P, Q, R) = \Sigma m(2, 3, 4, 5, 7)$, find Total Implicants, PIs, and EPIs.
    - _Solution:_
        - **Total Implicants:** 5 (count of minterms).
        - **PIs:** 4 groups ($\bar{P}Q$, $P\bar{Q}$, $PR$, $QR$).
        - **EPIs:** 2 groups ($\bar{P}Q$ covers cell 2 exclusively; $P\bar{Q}$ covers cell 4 exclusively).
        - Result: **Implicants = 5, PI = 4, EPI = 2**.