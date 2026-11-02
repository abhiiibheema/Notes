2: FRACTIONAL KNAPSACK PROBLEM

---

1. PROBLEM FORMULATION & CLASSIFICATION

The **Knapsack Problem** is a classic resource-allocation optimization problem. A thief or decision-maker carries a knapsack (bag) with a maximum weight capacity $M$. There are $n$ available objects, where each object $i$ (for $1 \le i \le n$) has an associated:

- **Weight (**$W_i$**):** The physical space or weight consumed by the object.
- **Profit (**$P_i$**):** The value or benefit gained by selecting the object.
- **Decision Variable (**$x_i$**):** The fraction of object $i$ placed into the knapsack.

```
                           Knapsack Problem
                                  │
         ┌────────────────────────┴────────────────────────┐
         ▼                                                 ▼
 Fractional Knapsack                               0/1 (Binary) Knapsack
- Division allowed: 0 ≤ xᵢ ≤ 1                    - Division forbidden: xᵢ ∈ {0, 1}
- Example: Liquid, Sugar, Grains (Aachar/Pickle)  - Example: Laptops, Mobile Phones, Car
- Paradigm: GREEDY METHOD                         - Paradigm: DYNAMIC PROGRAMMING
- Solved in O(n log n) time                       - Solved in O(n · M) pseudo-polynomial
```

---

Mathematical Model

The goal is to select fractions $x_i$ of the $n$ objects to **maximize total profit** while satisfying the **knapsack capacity constraint**:

$$\text{Maximize } f(X) = \sum_{i=1}^n x_i \cdot P_i$$

$$\text{Subject to the constraint: } \sum_{i=1}^n x_i \cdot W_i \le M$$

$$\text{Explicit Constraint: } 0 \le x_i \le 1 \quad \forall i \in \{1, 2, \dots, n\}$$

---

2. HEURISTIC STRATEGIES COMPARISON

To determine the greedy choice criteria, we evaluate three potential prioritization strategies on a common problem instance.

Benchmark Instance

Suppose we have $n = 3$ objects and a knapsack capacity $M = 20$:

- **Weights:** $W =$1more_horiz
- **Profits:** $P =$2more_horiz

```
Object 1: W₁ = 18, P₁ = 25  ──► P₁/W₁ = 25/18 ≈ 1.38
Object 2: W₂ = 15, P₂ = 24  ──► P₂/W₂ = 24/15 = 1.60
Object 3: W₃ = 10, P₃ = 15  ──► P₃/W₃ = 15/10 = 1.50
```

---

Strategy 1: Greedy about Maximum Profit ($P_i$)

Prioritize objects with the highest raw profit first, regardless of weight.

- **Selection Order:** Object 1 ($P=25$) $\to$ Object 2 ($P=24$) $\to$ Object 3 ($P=15$).
- **Pick Object 1:** Take full object $x_1 = 1$. Consumes weight $18$. Remaining capacity $= 20 - 18 = 2$.
- **Pick Object 2:** Only capacity $2$ remains out of $15$. Take fraction $x_2 = \frac{2}{15}$. Remaining capacity $= 0$.
- **Pick Object 3:** No capacity left. $x_3 = 0$.

$$\text{Total Profit} = (1 \times 25) + \left(\frac{2}{15} \times 24\right) + (0 \times 15) = 25 + 3.2 = \mathbf{28.2}$$

---

Strategy 2: Greedy about Minimum Weight ($W_i$)

Prioritize objects with the smallest weight first, attempting to fit as many items as possible.

- **Selection Order:** Object 3 ($W=10$) $\to$ Object 2 ($W=15$) $\to$ Object 1 ($W=18$).
- **Pick Object 3:** Take full object $x_3 = 1$. Consumes weight $10$. Remaining capacity $= 20 - 10 = 10$.
- **Pick Object 2:** Only capacity $10$ remains out of $15$. Take fraction $x_2 = \frac{10}{15} = \frac{2}{3}$. Remaining capacity $= 0$.
- **Pick Object 1:** No capacity left. $x_1 = 0$.

$$\text{Total Profit} = (1 \times 15) + \left(\frac{10}{15} \times 24\right) + (0 \times 25) = 15 + 16 = \mathbf{31.0}$$

---

Strategy 3: Greedy about Profit-per-Unit Weight ($P_i / W_i$) [OPTIMAL]

Combine both profit and weight by computing the **density / profit ratio** $r_i = \frac{P_i}{W_i}$ for each object, then sort in non-increasing order of $r_i$.

- **Density Ratios:** $r_1 = 1.38$, $r_2 = 1.60$, $r_3 = 1.50$.
- **Selection Order:** Object 2 ($r=1.60$) $\to$ Object 3 ($r=1.50$) $\to$ Object 1 ($r=1.38$).
- **Pick Object 2:** Take full object $x_2 = 1$. Consumes weight $15$. Remaining capacity $= 20 - 15 = 5$.
- **Pick Object 3:** Remaining capacity is $5$ out of $10$. Take fraction $x_3 = \frac{5}{10} = \frac{1}{2}$. Remaining capacity $= 0$.
- **Pick Object 1:** No capacity left. $x_1 = 0$.

$$\text{Total Profit} = (1 \times 24) + \left(\frac{1}{2} \times 15\right) + (0 \times 25) = 24 + 7.5 = \mathbf{31.5}$$

---

Strategy Comparison Summary Matrix

|Strategy|Sorting Metric|Total Profit Obtained|Is Optimal?|
|---|---|---|---|
|**Strategy 1**|Maximize Profit ($P_i$)|$28.2$|No|
|**Strategy 2**|Minimize Weight ($W_i$)|$31.0$|No|
|**Strategy 3**|Maximize Ratio ($P_i / W_i$)|$31.5$|**YES (Always Optimal)**|

---

3. ALGORITHM & COMPLEXITY ANALYSIS

Algorithmic Implementation

```
Algo FractionalKnapsack(P, W, M, n) {
    // Step 1: Calculate profit-to-weight ratio for all objects
    for (i = 1; i <= n; i++) {
        R[i] = P[i] / W[i];
        X[i] = 0.0; // Initialize solution vector
    }
    
    // Step 2: Sort objects in descending order of ratio R[i]
    Sort_Descending(R, P, W, n);
    
    cu = M; // cu = remaining knapsack capacity
    
    // Step 3: Greedily pack objects
    for (i = 1; i <= n; i++) {
        if (W[i] <= cu) {
            X[i] = 1.0;        // Take full object
            cu = cu - W[i];    // Reduce capacity
        } else {
            X[i] = cu / W[i];  // Take fractional part
            cu = 0;            // Knapsack is full
            break;             // Terminate loop
        }
    }
    
    // Step 4: Compute total profit
    total_profit = 0;
    for (i = 1; i <= n; i++) {
        total_profit += X[i] * P[i];
    }
    
    return total_profit;
}
```

---

Detailed Time & Space Complexity

```
                       Fractional Knapsack Execution Work
                                       │
         ┌─────────────────────────────┼─────────────────────────────┐
         ▼                             ▼                             ▼
 Compute Ratios (Pᵢ/Wᵢ)         Sort Objects by Ratio        Pack Knapsack Loop
     O(n) operations               O(n log n) time             O(n) single pass
```

1. **Calculating Ratios:** Computing $r_i = P_i / W_i$ for $n$ elements takes $O(n)$ time.
2. **Sorting Phase:** Sorting $n$ objects in non-increasing order of ratios using Merge Sort or Quick Sort takes $O(n \log_2 n)$ time.
3. **Greedy Packing Phase:** Single loop iteration across at most $n$ objects takes $O(n)$ time.

$$\text{Total Time Complexity} = O(n) + O(n \log_2 n) + O(n) = \mathbf{O(n \log_2 n)}$$

- **Presorted Special Case:** If the objects are already provided in sorted order of $P_i / W_i$, the sorting step is bypassed, reducing time complexity to $O(n)$.
- **Auxiliary Space Complexity:** $O(n)$ to store the ratios $R$ and solution vector $X$.

---

4. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Full Execution Trace (NAT)

A knapsack has capacity $M = 15$. There are $n = 7$ objects with given weights and profits:

|Object|Weight ($W_i$)|Profit ($P_i$)|
|---|---|---|
|**O1**|4|18|
|**O2**|1|3|
|**O3**|3|5|
|**O4**|7|7|
|**O5**|1|6|
|**O6**|5|15|
|**O7**|2|10|

Find the maximum profit using the Optimal Greedy Strategy.

Step-by-Step Solution:

1. **Compute Ratios (**$P_i / W_i$**):**
    - **O1:** $18 / 4 = 4.5$
    - **O2:** $3 / 1 = 3.0$
    - **O3:** $5 / 3 \approx 1.67$
    - **O4:** $7 / 7 = 1.0$
    - **O5:** $6 / 1 = 6.0$
    - **O6:** $15 / 5 = 3.0$
    - **O7:** $10 / 2 = 5.0$
2. **Sort Objects in Descending Order of Ratios:** Order: **O5** (6.0) $\to$ **O7** (5.0) $\to$ **O1** (4.5) $\to$ **O2** (3.0) $\to$ **O6** (3.0) $\to$ **O3** (1.67) $\to$ **O4** (1.0).
3. **Pack Objects into Knapsack (Capacity** $M = 15$**):**
    - **Take O5:** $W_5 = 1$. Capacity left $= 15 - 1 = 14$. Profit $= 6$. $x_5 = 1$.
    - **Take O7:** $W_7 = 2$. Capacity left $= 14 - 2 = 12$. Profit $= 6 + 10 = 16$. $x_7 = 1$.
    - **Take O1:** $W_1 = 4$. Capacity left $= 12 - 4 = 8$. Profit $= 16 + 18 = 34$. $x_1 = 1$.
    - **Take O2:** $W_2 = 1$. Capacity left $= 8 - 1 = 7$. Profit $= 34 + 3 = 37$. $x_2 = 1$.
    - **Take O6:** $W_6 = 5$. Capacity left $= 7 - 5 = 2$. Profit $= 37 + 15 = 52$. $x_6 = 1$.
    - **Take Fractional O3:** $W_3 = 3$, remaining capacity $= 2$. Take fraction $x_3 = \frac{2}{3}$. $$\text{Fractional Profit} = \frac{2}{3} \times 5 = \frac{10}{3} \approx 3.33$$
    - **Take O4:** Capacity left $= 0$. $x_4 = 0$.
4. **Calculate Total Profit:** $$\text{Total Profit} = 6 + 10 + 18 + 3 + 15 + 3.33 = \mathbf{55.33}$$

**Answer:** **55.33** (or $166/3$).

---

Problem 2: 0/1 Knapsack vs. Greedy Ratio Gap (GATE PYQ)

Consider items with weights and values:

|Item|Weight (kg)|Value (Rupees)|
|---|---|---|
|**1**|10|60|
|**2**|7|28|
|**3**|4|20|
|**4**|2|24|

Knapsack capacity $M = 11$ kg. No item can be split (0/1 constraint). Let $V_{\text{opt}}$ be the optimal value and $V_{\text{greedy}}$ be the value obtained by packing items in descending order of value-to-weight ratio. Calculate $V_{\text{opt}} - V_{\text{greedy}}$.

Step-by-Step Solution:

1. **Compute Ratios (**$V_i / W_i$**):**
    - Item 1: $60/10 = 6$
    - Item 2: $28/7 = 4$
    - Item 3: $20/4 = 5$
    - Item 4: $24/2 = 12$
    - Priority Order: **Item 4** (12) $\to$ **Item 1** (6) $\to$ **Item 3** (5) $\to$ **Item 2** (4).
2. **Compute** $V_{\text{greedy}}$ **(Greedy Selection without splitting):**
    - Pick Item 4 (Weight = 2, Value = 24). Remaining capacity $= 11 - 2 = 9$.
    - Next ratio is Item 1 (Weight = 10). Since $10 > 9$ and splitting is forbidden, **Item 1 cannot be packed**.
    - Next ratio is Item 3 (Weight = 4). Remaining capacity $= 9 - 4 = 5$. Value $= 24 + 20 = 44$.
    - Next ratio is Item 2 (Weight = 7). $7 > 5$, cannot fit.
    - Greedy Items Packed: $\{3, 4\}$ $\implies V_{\text{greedy}} = 20 + 24 = \mathbf{44}$.
3. **Compute** $V_{\text{opt}}$ **(Brute Force / Optimal 0/1 Selection):**
    - Test feasible subset combinations within weight capacity $M \le 11$:
        - Subset $\{1\}$: Weight $= 10 \le 11$, Value $= 60$.
        - Subset $\{2, 3\}$: Weight $= 7 + 4 = 11 \le 11$, Value $= 28 + 20 = 48$.
        - Subset $\{2, 4\}$: Weight $= 7 + 2 = 9 \le 11$, Value $= 28 + 24 = 52$.
        - Subset $\{3, 4\}$: Weight $= 4 + 2 = 6 \le 11$, Value $= 20 + 24 = 44$.
    - Maximum value is achieved by subset $\{1\}$ $\implies V_{\text{opt}} = \mathbf{60}$.
4. **Calculate Difference:** $$V_{\text{opt}} - V_{\text{greedy}} = 60 - 44 = \mathbf{16}$$

**Answer:** **16**.