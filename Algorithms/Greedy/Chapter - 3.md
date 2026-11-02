JOB SEQUENCING WITH DEADLINES & OPTIMAL MERGE PATTERNS

---

1. JOB SEQUENCING WITH DEADLINES (JSD)

Problem Formulation & CPU Scheduling Model

The **Job Sequencing with Deadlines** problem models a single-processor CPU system operating under **non-preemptive scheduling**.

Given a set of $n$ jobs $J = \{J_1, J_2, \dots, J_n\}$, each job $i$ requires:

- **Execution / Burst Time:** Exactly **1 unit of CPU time** (default: $BT_i = 1$).
- **Arrival Time:** All jobs are available simultaneously at time $t = 0$ (default: $AT_i = 0$).
- **Deadline (**$D_i \ge 1$**):** The maximum time limit by which job $i$ must complete.
- **Profit (**$P_i > 0$**):** The financial or computational reward earned **only if** job $i$ completes on or before its deadline $D_i$ (i.e., completion time $C_i \le D_i$). If a job completes after $D_i$, no profit is earned.

```
                    Single CPU Non-Preemptive Timeline
┌──────────────┬──────────────┬──────────────┬───────┬──────────────────────────┐
│ Time Slot 1  │ Time Slot 2  │ Time Slot 3  │  ...  │ Time Slot Max(D)         │
│  [0 ──► 1]   │  [1 ──► 2]   │  [2 ──► 3]   │  ...  │ [Max(D)-1 ──► Max(D)]    │
└──────────────┴──────────────┴──────────────┴───────┴──────────────────────────┘
```

Goal

Select a feasible subset of jobs $J' \subseteq J$ and assign each job in $J'$ to a distinct time slot such that **all scheduled jobs finish within their deadlines** while **maximizing total profit**:

$$\text{Maximize } \sum_{i \in J'} P_i$$

---

Brute-Force vs. Greedy Strategy

- **Brute-Force Enumeration:** Evaluates all possible subsets of jobs. For $n$ jobs, there are $2^n$ candidate subsets. Testing each subset for scheduling feasibility takes $O(n!)$ or $O(n^n)$ time.
    - **Solution Space Size:** $O(2^n)$ — computationally impractical for large $n$.
- **Greedy Choice Property:** Prioritize jobs with the **highest profit** first. To maximize the chance of fitting other high-profit jobs into earlier slots, schedule each chosen job in the **latest possible free time slot** on or before its deadline $D_i$.

---

2. GREEDY SLOT ALLOCATION MECHANICS

The Late-Binding Scheduling Rule

For any job $i$ with deadline $D_i$, attempt to assign it to time slot $\min(D_i, D_{\max})$. If that slot is already occupied by a higher-profit job, search backward for the nearest available free slot $k \in [D_i - 1, D_i - 2, \dots, 1]$. If all slots up to $D_i$ are full, job $i$ cannot be scheduled and is rejected.

```
                      Greedy Job Allocation Pipeline
                                    │
           ┌────────────────────────┴────────────────────────┐
           ▼                                                 ▼
 Sort Jobs by Profit (Pᵢ)                           Slot Search Assignment
- Order: P₁ ≥ P₂ ≥ ... ≥ Pₙ                       - Target Slot: min(Dᵢ, D_max)
- Sorting Cost: O(n log n)                        - Scan leftward for free slot
```

---

Step-by-Step Algorithmic Procedure

```
Algo JobSequencingWithDeadlines(J, P, D, n) {
    // Step 1: Sort jobs in descending order of profits
    Sort_Descending_By_Profit(J, P, D, n);
    
    // Step 2: Find maximum deadline D_max
    D_max = Max(D[1 ... n]);
    
    // Step 3: Initialize Gantt chart time slots to FREE (-1)
    for (j = 1; j <= D_max; j++) {
        Slot[j] = -1;
    }
    
    total_profit = 0;
    count = 0;
    
    // Step 4: Greedily place jobs into latest available slots
    for (i = 1; i <= n; i++) {
        // Search for a free slot from min(D[i], D_max) down to 1
        for (k = Min(D[i], D_max); k >= 1; k--) {
            if (Slot[k] == -1) {
                Slot[k] = i;              // Assign job i to slot k
                total_profit += P[i];
                count++;
                break;                     // Move to next job
            }
        }
    }
    
    return total_profit;
}
```

---

Time Complexity Analysis

1. **Sorting Phase:** Sorting $n$ jobs by profit takes $O(n \log_2 n)$ time.
2. **Slot Search Phase:**
    - **Simple Array Scanning:** For each of the $n$ jobs, scanning up to $D_{\max}$ slots backwards takes $O(n \cdot D_{\max})$ or $O(n^2)$ time in the worst case (when $D_{\max} \approx n$).
    - **Disjoint-Set / Union-Find Optimization:** By using a Disjoint Set Union (DSU) data structure to maintain pointers to the nearest free slot, slot allocation runs in nearly linear time $O(n \cdot \alpha(n))$.
3. **Overall Time Complexity:**
    - **Standard Implementation:** $\mathbf{O(n^2)}$.
    - **DSU-Optimized Implementation:** $\mathbf{O(n \log_2 n)}$.
4. **Auxiliary Space Complexity:** $\mathbf{O(D_{\max})}$ to store the slot status array.

---

3. OPTIMAL MERGE PATTERNS & 2-WAY BINARY MERGE TREES

Problem Definition

Given $n$ pre-sorted files $F = \{F_1, F_2, \dots, F_n\}$ with respective record counts $Q = \{q_1, q_2, \dots, q_n\}$, merge all $n$ files into a single unified sorted file using 2-way merging.

Merging two files of sizes $x$ and $y$ requires $x + y$ **record movements** (copying elements to a new merged buffer). Because intermediate merged files must be merged again in subsequent steps, the order in which file pairs are merged significantly impacts the **Total Record Movements (TRM)**.

```
                         2-Way Binary Merge Tree
                                  (Root)
                                  /    \
                            (Merged)  [F₃]
                            /      \
                        [F₁]        [F₂]
```

---

Mathematical Model: Weighted External Path Length

Any 2-way merge sequence on $n$ files can be modeled as a **Strict Binary Tree** (where every non-leaf node has exactly 2 children):

- **Leaf Nodes (External Nodes):** Represent the initial input files $F_i$ of size $q_i$.
- **Internal Nodes:** Represent intermediate merged files. The value at an internal node equals the sum of its left and right children.

$$\text{Total Record Movements (TRM)} = \sum_{i=1}^n d_i \cdot q_i$$

Where:

- $q_i$: Number of records in file $F_i$.
- $d_i$: Distance (depth / level) of leaf node $F_i$ from the root node.

---

Greedy Choice Property (Min-Heap Strategy)

To minimize the weighted sum $\sum d_i \cdot q_i$, files with **larger record counts must be placed at smaller depths** (closer to the root), while files with **smaller record counts can be placed at greater depths**.

Greedy Merge Rule

At each step, select the **two files with the smallest record sizes**, merge them (cost = sum of their sizes), insert the newly merged intermediate file back into the pool, and repeat until only 1 file remains.

```
Algo OptimalMergePattern(Q, n) {
    // Step 1: Insert all n file sizes into a Min-Heap -> O(n)
    Build_Min_Heap(Q);
    
    total_record_movements = 0;
    
    // Step 2: Merge the two smallest files recursively -> (n - 1) passes
    for (i = 1; i <= n - 1; i++) {
        x = Extract_Min(Q); // Smallest file
        y = Extract_Min(Q); // Second smallest file
        
        merged_size = x + y;
        total_record_movements += merged_size;
        
        Insert_Min_Heap(Q, merged_size); // Insert merged file back
    }
    
    return total_record_movements;
}
```

---

Complexity Analysis of Optimal Merge Pattern

- **Min-Heap Implementation:**
    - Building Min-Heap takes $O(n)$ time.
    - There are $n - 1$ merge passes. Each pass performs two `Extract-Min` operations and one `Insert` operation, each taking $O(\log_2 n)$ time.
    - **Time Complexity:** $(n - 1) \times O(\log_2 n) = \mathbf{O(n \log_2 n)}$.
- **Unsorted List Implementation:** Searching for the two minimum elements in an array takes $O(n)$ per pass $\implies \mathbf{O(n^2)}$ time.
- **Auxiliary Space Complexity:** $\mathbf{O(n)}$ to store the Min-Heap / merge tree nodes.

---

4. COMPARATIVE SUMMARY MATRIX

|Feature / Metric|Job Sequencing with Deadlines|Optimal Merge Patterns|
|---|---|---|
|**Primary Goal**|Maximize total profit within deadlines|Minimize total record movements (TRM)|
|**Greedy Choice Metric**|Select max profit $P_i$ first|Select min file sizes $q_i, q_j$ first|
|**Primary Constraint**|Time slots $k \le D_i$|Binary merge structure (2 files at a time)|
|**Optimal Time Complexity**|$O(n \log_2 n)$ (using DSU)|$O(n \log_2 n)$ (using Min-Heap)|
|**Underlying Data Structure**|Slot array / Disjoint Set Union (DSU)|Min-Heap / Huffman Tree Structure|

---

5. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Job Sequencing Full Trace (NAT)

Given $n = 4$ jobs with profits and deadlines:

|Job|Profit ($P_i$)|Deadline ($D_i$)|
|---|---|---|
|**J1**|200|2|
|**J2**|30|1|
|**J3**|50|2|
|**J4**|80|1|

Find the maximum profit and the optimal job schedule.

Step-by-Step Solution:

1. **Sort Jobs in Descending Order of Profit:** Order: **J1** (200, $D=2$) $\to$ **J4** (80, $D=1$) $\to$ **J3** (50, $D=2$) $\to$ **J2** (30, $D=1$).
2. **Find Maximum Deadline:** $D_{\max} = \max(2, 1, 2, 1) = 2$. Available slots: Slot 1 1 and Slot 2 12.
3. **Slot Allocation Trace:**
    - **Process J1 (P=200, D=2):** Target slot $\min(2, 2) = 2$. Slot 2 is free. Assign **J1 to Slot 2**.
    - **Process J4 (P=80, D=1):** Target slot $\min(1, 2) = 1$. Slot 1 is free. Assign **J4 to Slot 1**.
    - **Process J3 (P=50, D=2):** Target slot 2 is occupied (J1). Check slot 1: occupied (J4). **Reject J3**.
    - **Process J2 (P=30, D=1):** Target slot 1 is occupied (J4). **Reject J2**.
4. **Final Schedule:** Slot 1 = **J4**, Slot 2 = **J1**.
5. **Total Maximum Profit:** $$P_{\text{total}} = 80 + 200 = \mathbf{280}$$

**Answer:** **Maximum Profit = 280**, Scheduled Jobs = **{J4, J1}**.

---

Problem 2: 9-Task Scheduling GATE PYQ (MCQ / NAT)

We are given 9 tasks $T_1, T_2, \dots, T_9$ with execution time = 1 unit each. Profits and deadlines are given below:

|Task|T1|T2|T3|T4|T5|T6|T7|T8|T9|
|---|---|---|---|---|---|---|---|---|---|
|**Profit**|15|20|30|18|18|10|23|16|25|
|**Deadline**|7|2|5|3|4|5|2|7|3|

1. Which tasks are left out in the optimal profit schedule?
2. What is the maximum profit earned?

Step-by-Step Solution:

1. **Sort Tasks in Descending Order of Profit:** Order: **T3** (30, D=5) $\to$ **T9** (25, D=3) $\to$ **T7** (23, D=2) $\to$ **T2** (20, D=2) $\to$ **T4** (18, D=3) $\to$ **T5** (18, D=4) $\to$ **T8** (16, D=7) $\to$ **T1** (15, D=7) $\to$ **T6** (10, D=5).
2. **Find** $D_{\max}$**:** $D_{\max} = 7$. Available slots: 1 to 7.
3. **Slot Allocation Trace:**
    - **T3 (P=30, D=5):** Assign to **Slot 5**.
    - **T9 (P=25, D=3):** Assign to **Slot 3**.
    - **T7 (P=23, D=2):** Assign to **Slot 2**.
    - **T2 (P=20, D=2):** Slot 2 full $\to$ Slot 1 free. Assign **T2 to Slot 1**.
    - **T4 (P=18, D=3):** Slots 3, 2, 1 are all full. **Reject T4**.
    - **T5 (P=18, D=4):** Slot 4 is free. Assign **T5 to Slot 4**.
    - **T8 (P=16, D=7):** Slot 7 is free. Assign **T8 to Slot 7**.
    - **T1 (P=15, D=7):** Slot 7 full $\to$ Slot 6 free. Assign **T1 to Slot 6**.
    - **T6 (P=10, D=5):** Slots 5, 4, 3, 2, 1 are all full. **Reject T6**.
4. **Summary of Scheduled Slots:** `Slot 1: T2 | Slot 2: T7 | Slot 3: T9 | Slot 4: T5 | Slot 5: T3 | Slot 6: T1 | Slot 7: T8`
5. **Left-out Tasks:** **T4 and T6**.
6. **Total Profit:** $$P_{\text{total}} = 20 + 23 + 25 + 18 + 30 + 15 + 16 = \mathbf{147}$$

**Answer:**

1. **T4 and T6 are left out**.
2. **Maximum Profit = 147**.

---

Problem 3: Optimal Merge Pattern Calculation (NAT)

Given 5 pre-sorted files with record sizes: $F_1 = 12, F_2 = 5, F_3 = 18, F_4 = 10, F_5 = 2$. Calculate the minimum Total Record Movements (TRM) required to merge them into a single sorted file.

Step-by-Step Solution:

1. **Initial File Sizes (Min-Heap):** $\{2, 5, 10, 12, 18\}$.
2. **Pass 1:** Pick two smallest $2$ and $5$. Merge cost $= 2 + 5 = 7$.
    - Active files: $\{7, 10, 12, 18\}$.
3. **Pass 2:** Pick two smallest $7$ and $10$. Merge cost $= 7 + 10 = 17$.
    - Active files: $\{12, 17, 18\}$.
4. **Pass 3:** Pick two smallest $12$ and $17$. Merge cost $= 12 + 17 = 29$.
    - Active files: $\{18, 29\}$.
5. **Pass 4:** Pick remaining two $18$ and $29$. Merge cost $= 18 + 29 = 47$.
    - Active files: $\{47\}$ (Complete).
6. **Total Record Movements (Sum of intermediate merge costs):** $$\text{TRM} = 7 + 17 + 29 + 47 = \mathbf{100}$$

Weighted External Path Length Verification:

- Tree Depths from Root:
    - $F_3 (18)$: depth 1 $\implies 18 \times 1 = 18$.
    - $F_1 (12)$: depth 2 $\implies 12 \times 2 = 24$.
    - $F_4 (10)$: depth 3 $\implies 10 \times 3 = 30$.
    - $F_5 (2)$: depth 4 $\implies 2 \times 4 = 8$.
    - $F_2 (5)$: depth 4 $\implies 5 \times 4 = 20$.
- Sum $= 18 + 24 + 30 + 8 + 20 = \mathbf{100}$.

**Answer:** **100 record movements**.

---

Problem 4: 3-File Merge Tree Optimization (MCQ)

Given 3 sorted files $A=10, B=15, C=5$. What is the minimum number of record movements to merge them?

- (A) 55
- (B) 50
- (C) 45
- (D) 30

Step-by-Step Solution:

1. Smallest two files are $C=5$ and $A=10$.
2. Merge $C$ and $A$: Cost $= 5 + 10 = 15$.
3. Merge intermediate file $15$ with $B=15$: Cost $= 15 + 15 = 30$.
4. Total Record Movements $= 15 + 30 = \mathbf{45}$.

**Answer:** **(C) 45**.