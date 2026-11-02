**Section 1: Control Logic (Enable & Disable Circuits)**

Control logic uses control signals to either allow data signals to pass through unchanged (**Enable**) or force the output to a fixed constant logic state (**Disable**).

**1.1 OR Gate Control Logic**

An OR gate uses **Control Input = 0** to enable and **Control Input = 1** to disable.

- **Enable Mode (Control = 0):** $$\text{Output} = A + B + 0 = A + B$$ _The input signal passes through completely unchanged._
- **Disable Mode (Control = 1):** $$\text{Output} = (A + B) + 1 = 1$$ _The output is forced permanently to logic HIGH (1)._

---

**1.2 AND Gate Control Logic**

An AND gate uses **Control Input = 1** to enable and **Control Input = 0** to disable.

- **Enable Mode (Control = 1):** $$\text{Output} = A \cdot B \cdot 1 = A \cdot B$$ _The input signal passes through completely unchanged._
- **Disable Mode (Control = 0):** $$\text{Output} = A \cdot B \cdot 0 = 0$$ _The output is forced permanently to logic LOW (0)._

---

**1.3 Buffer & Inverting Buffer Logic via XOR / XNOR**

**Using XOR Gate:**

- **Control = 0 (Buffer / Pass-Through):** $$A \oplus 0 = A$$
- **Control = 1 (Inverting Buffer / NOT Gate):** $$A \oplus 1 = \bar{A}$$

**Using XNOR Gate:**

- **Control = 0 (Inverting Buffer / NOT Gate):** $$A \odot 0 = \bar{A}$$
- **Control = 1 (Buffer / Pass-Through):** $$A \odot 1 = A$$

---

**Section 2: Arithmetic Logic Gates (XOR & XNOR)**

**2.1 Exclusive-OR (XOR) Gate (**$\oplus$**)**

The XOR gate acts as an **Odd Function Detector** (returns logic 1 when an odd number of inputs are 1).

- **Algebraic Expression:** $$A \oplus B = \bar{A}B + A\bar{B}$$
- **Canonical Forms:**
    - **SOP Form:** $\Sigma m(1, 2)$
    - **POS Form:** $\Pi M(0, 3)$
- **Truth Table:**

|$A$|$B$|$A \oplus B$|Behavior Note|
|---|---|---|---|
|0|0|**0**|Inputs same $\rightarrow 0$|
|0|1|**1**|Inputs different $\rightarrow 1$|
|1|0|**1**|Inputs different $\rightarrow 1$|
|1|1|**0**|Inputs same $\rightarrow 0$|

---

**2.2 Exclusive-NOR (XNOR) Gate (**$\odot$**)**

The XNOR gate acts as an **Equivalence Detector** or **Even Function Detector** (returns logic 1 when inputs are equal or when an even number of 1s are present).

- **Algebraic Expression:** $$A \odot B = \bar{A}\bar{B} + AB$$
- **Canonical Forms:**
    - **SOP Form:** $\Sigma m(0, 3)$
    - **POS Form:** $\Pi M(1, 2)$
- **Truth Table:**

|$A$|$B$|$A \odot B$|Behavior Note|
|---|---|---|---|
|0|0|**1**|Inputs same $\rightarrow 1$|
|0|1|**0**|Inputs different $\rightarrow 0$|
|1|0|**0**|Inputs different $\rightarrow 0$|
|1|1|**1**|Inputs same $\rightarrow 1$|

---

**Section 3: Fundamental Identities & Properties**

**3.1 Basic XOR Identities**

1. $A \oplus A = 0$
2. $\bar{A} \oplus \bar{A} = 0$
3. $A \oplus \bar{A} = 1$
4. $\bar{A} \oplus A = 1$
5. $A \oplus 0 = A$ _(Buffer property)_
6. $A \oplus 1 = \bar{A}$ _(Inverter property)_
7. $PQ \oplus 0 = PQ$
8. $PQ \oplus 1 = \overline{PQ} = \bar{P} + \bar{Q}$

---

**3.2 Basic XNOR Identities**

1. $A \odot A = 1$
2. $\bar{A} \odot \bar{A} = 1$
3. $A \odot \bar{A} = 0$
4. $\bar{A} \odot A = 0$
5. $A \odot 0 = \bar{A}$ _(Inverter property)_
6. $A \odot 1 = A$ _(Buffer property)_

---

**3.3 Exchange Property**

If a logic equation is expressed as $A \oplus B = C$, any two variables can be swapped with the output:

$$A \oplus B = C \implies A \oplus C = B \quad \text{and} \quad B \oplus C = A$$

- **Proof:** $$A \oplus C = A \oplus (A \oplus B) = (A \oplus A) \oplus B = 0 \oplus B = B$$

Similarly, for XNOR: $$A \odot B = C \implies A \odot C = B \quad \text{and} \quad B \odot C = A$$

---

**3.4 Complementation Rules**

Complementing **one** input converts XOR to XNOR (and vice versa). Complementing **both** inputs leaves the gate function unchanged.

1. $\bar{A} \oplus B = A \oplus \bar{B} = A \odot B = \bar{A}\bar{B} + AB$
2. $\bar{A} \oplus \bar{B} = A \oplus B$
3. $\bar{A} \odot B = A \odot \bar{B} = A \oplus B = \bar{A}B + A\bar{B}$
4. $\bar{A} \odot \bar{B} = A \odot B$

**Summary:** $$A \oplus B = \overline{A \odot B} = \bar{A} \odot B = A \odot \bar{B}$$

---

**Section 4: Advanced Theorems & Operator Behavior**

**4.1 Parity & Matrix Rules for N Inputs**

**XOR Multi-Input Rule:**

For any number of inputs, XOR evaluates to **1 if and only if the number of 1s in the input combination is ODD**.

**XNOR Multi-Input Rule:**

XNOR evaluates to **1** under two specific conditions:

1. **Even number of inputs** AND **Even number of 1s**.
2. **Odd number of inputs** AND **Odd number of 1s**.

**Generalized Relationship (Matrix Pattern):**

|Number of Inputs|XOR vs. XNOR Relationship|Mathematical Representation|
|---|---|---|
|**Odd Number of Inputs**|Functions are **Identical**|$A \oplus B \oplus C = A \odot B \odot C = \Sigma m(1, 2, 4, 7)$|
|**Even Number of Inputs**|Functions are **Complements**|$A \oplus B = \overline{A \odot B}$|

---

**4.2 Distributive Property & Operator Hierarchy**

- **XOR and XNOR are NOT distributive over AND or OR:** $$A \oplus (B + C) \neq (A \oplus B) + (A \oplus C)$$ $$A \oplus (B \cdot C) \neq (A \oplus B) \cdot (A \oplus C)$$
- **AND IS distributive over XOR:** $$A \cdot (B \oplus C) = AB \oplus AC$$

---

**4.3 Cascaded Repetition (N-Stage Chains)**

**N-Times Cascaded XOR:**

$$A \oplus A \oplus A \dots \text{ (N times)}$$

- If $N = \text{EVEN}$ $\rightarrow \mathbf{0}$
- If $N = \text{ODD}$ $\rightarrow \mathbf{A}$

**N-Times Cascaded XNOR:**

$$A \odot A \odot A \dots \text{ (N times)}$$

- If $N = \text{EVEN}$ $\rightarrow \mathbf{1}$
- If $N = \text{ODD}$ $\rightarrow \mathbf{A}$

---

**Section 5: Comprehensive Solved Problems**

**Problem 1: Simplifying Product-XOR Terms**

**Question:** Simplify the logic expression $E = AB \oplus BC$.

- **Step-by-Step Solution:**
    1. Apply the XOR expansion formula $X \oplus Y = \bar{X}Y + X\bar{Y}$: $$E = (\overline{AB}) \cdot (BC) + (AB) \cdot (\overline{BC})$$
    2. Use DeMorgan's Law on the complemented terms: $$E = (\bar{A} + \bar{B})BC + AB(\bar{B} + \bar{C})$$
    3. Expand the terms: $$E = \bar{A}BC + \bar{B}BC + AB\bar{B} + AB\bar{C}$$
    4. Apply the complement law ($B\bar{B} = 0$): $$E = \bar{A}BC + 0 + 0 + AB\bar{C} = \bar{A}BC + AB\bar{C}$$
    5. Factor out common term $B$: $$E = B(\bar{A}C + A\bar{C})$$
    6. Recognize the XOR expression $\bar{A}C + A\bar{C} = A \oplus C$: $$E = B \cdot (A \oplus C)$$

---

**Problem 2: Reduction with Variable and Sub-Term**

**Question:** Simplify $P \oplus QP$.

- **Step-by-Step Solution:**
    1. Expand using the XOR definition: $$P \oplus QP = \bar{P}(QP) + P(\overline{QP})$$
    2. Since $\bar{P} \cdot P = 0$, the first term disappears: $$\bar{P}(QP) = 0$$
    3. Apply DeMorgan's Law to the remaining term: $$P(\overline{QP}) = P(\bar{Q} + \bar{P}) = P\bar{Q} + P\bar{P}$$
    4. Since $P\bar{P} = 0$: $$P\bar{Q} + 0 = P\bar{Q}$$
- **Final Answer:** $P\bar{Q}$

---

**Problem 3: Two-Stage Circuit Analysis with Complemented Inputs**

**Question:** Determine the final output $y$ of a circuit where:

- Gate 1 (XOR) receives inputs $\bar{A}$ and $B$.
- Gate 2 (XOR) receives inputs $\bar{B}$ and $A$.
- Gate 3 (XOR) combines the outputs of Gate 1 and Gate 2.
- **Step-by-Step Solution:**
    1. Output of Gate 1: $$\bar{A} \oplus B = A \odot B$$
    2. Output of Gate 2: $$\bar{B} \oplus A = A \oplus \bar{B} = A \odot B$$
    3. Gate 3 performs XOR on identical inputs: $$y = (A \odot B) \oplus (A \odot B)$$
    4. Apply the identity $X \oplus X = 0$: $$y = 0$$
- **Final Answer:** Always **0**.

---

**Problem 4: Dual Circuit with Complement Outputs**

**Question:** Determine the output $y$ if:

- Gate 1 (XOR) has inputs $\bar{A}, \bar{B}$.
- Gate 2 (XOR) has inputs $A, \bar{B}$.
- Gate 3 (XOR) combines Gate 1 and Gate 2 outputs.
- **Step-by-Step Solution:**
    1. Output of Gate 1: $\bar{A} \oplus \bar{B} = A \oplus B$
    2. Output of Gate 2: $A \oplus \bar{B} = A \odot B = \overline{A \oplus B}$
    3. Gate 3 combines a term and its complement: Let $K = A \oplus B$. Then Gate 3 computes $K \oplus \bar{K}$.
    4. Apply identity $X \oplus \bar{X} = 1$: $$y = 1$$
- **Final Answer:** Always **1**.

---

**Problem 5: N-Stage Cascaded XOR Network Analysis**

**Question:** An $n$-stage cascaded network of XOR gates is constructed such that:

- Stage 1 receives inputs $1$ and $\bar{A}$.
- Every subsequent stage takes the previous stage's output and XORs it with $\bar{A}$. Determine the final output for an $n$-stage network.
- **Step-by-Step Solution:**
    1. **Stage 1 Output:** $$y_1 = 1 \oplus \bar{A} = \overline{\bar{A}} = A$$
    2. **Stage 2 Output:** $$y_2 = y_1 \oplus \bar{A} = A \oplus \bar{A} = 1$$
    3. **Stage 3 Output:** $$y_3 = y_2 \oplus \bar{A} = 1 \oplus \bar{A} = A$$
    4. **Pattern Observation:**
        - Odd stages ($n = 1, 3, 5, \dots$) produce output $A$.
        - Even stages ($n = 2, 4, 6, \dots$) produce output $1$.
- **Final Output:**
    - Output = $A$ if $n$ is **odd**.
    - Output = $1$ if $n$ is **even**.

---

**Problem 6: Complex Term Reduction with XOR**

**Question:** Simplify $F = \bar{P} \oplus (\overline{P + QR})$.

- **Step-by-Step Solution:**
    1. Apply DeMorgan's Law to the inner term: $$\overline{P + QR} = \bar{P} \cdot \overline{QR} = \bar{P}(\bar{Q} + \bar{R})$$
    2. Expression becomes: $$F = \bar{P} \oplus [\bar{P}(\bar{Q} + \bar{R})]$$
    3. Use the result from Problem 2 ($X \oplus YX = X\bar{Y}$), setting $X = \bar{P}$ and $Y = \bar{Q} + \bar{R}$: $$F = \bar{P} \cdot \overline{(\bar{Q} + \bar{R})}$$
    4. Apply DeMorgan's Law to the complemented term: $$\overline{\bar{Q} + \bar{R}} = \overline{\bar{Q}} \cdot \overline{\bar{R}} = QR$$
    5. Combine results: $$F = \bar{P}QR$$
- **Final Answer:** $\bar{P}QR$