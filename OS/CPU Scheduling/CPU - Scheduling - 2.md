CPU SCHEDULING BATCH 2 MASTER REVISION NOTES

---

MODULE 1: PRIORITY-BASED SCHEDULING & STARVATION

1. RECALL-TRIGGER CONCEPTS

- **Scheduling Criteria:** Process priority assigned statically or dynamically.
- **Preemption Modes:**
    - **Non-Preemptive Priority:** Once assigned, the CPU cannot be taken away until process termination or I/O yield.
    - **Preemptive Priority:** If a newly arrived process has a higher priority than the currently running process, the running process is immediately preempted.
- **Priority Convention Rule:** Questions must explicitly define whether a **higher numerical value** represents a higher priority or a **lower numerical value** represents a higher priority.
- **Starvation:** Low-priority processes may wait indefinitely in the Ready Queue if higher-priority processes continuously arrive.
- **Solution (Aging):** Dynamically increase the priority of a process by a fixed increment as it waits in the Ready Queue over predefined time intervals.

---

2. SOLVED PRACTICE PROBLEMS

**Problem 1: Priority Non-Preemptive (Higher Number = Higher Priority)**

- **Process Dataset:**

|Process|Arrival Time ($AT$)|Burst Time ($BT$)|Priority|
|---|---|---|---|
|$P_1$|$0$|$4$|$4$|
|$P_2$|$1$|$2$|$5$|
|$P_3$|$2$|$3$|$6$|
|$P_4$|$3$|$1$|$10$ (Highest)|
|$P_5$|$4$|$2$|$9$|
|$P_6$|$5$|$6$|$7$|

- **Execution Trace & Gantt Chart:**
    1. At $t=0$, only $P_1$ is present. Non-preemptive $P_1$ executes continuously from $t=0$ to $t=4$ ($CT=4$).
    2. At $t=4$, ready processes are $P_2(5), P_3(6), P_4(10), P_5(9)$. Highest priority is $P_4(10) \implies P_4$ runs from $t=4$ to $t=5$ ($CT=5$).
    3. At $t=5$, $P_6(7)$ arrives. Ready queue contains $P_2(5), P_3(6), P_5(9), P_6(7)$. Highest priority is $P_5(9) \implies P_5$ runs from $t=5$ to $t=7$ ($CT=7$).
    4. At $t=7$, ready processes are $P_2(5), P_3(6), P_6(7)$. Highest priority is $P_6(7) \implies P_6$ runs from $t=7$ to $t=13$ ($CT=13$).
    5. At $t=13$, ready processes are $P_2(5), P_3(6)$. Highest priority is $P_3(6) \implies P_3$ runs from $t=13$ to $t=16$ ($CT=16$).
    6. At $t=16$, $P_2(5)$ runs from $t=16$ to $t=18$ ($CT=18$).

```
[ P1 ][ P4 ][ P5 ][   P6   ][ P3 ][ P2 ]
0     4    5    7          13    16    18
```

---

**Problem 2: Priority Preemptive (Higher Number = Higher Priority)**

- **Process Dataset:** Same dataset as Problem 1.
- **Execution Trace & Gantt Chart:**
    1. $t=0$: $P_1(4)$ starts.
    2. $t=1$: $P_2(\text{prio } 5)$ arrives. Since $5 > 4$, $P_1$ is preempted (remaining $BT=3$). $P_2$ runs.
    3. $t=2$: $P_3(\text{prio } 6)$ arrives. Since $6 > 5$, $P_2$ is preempted (remaining $BT=1$). $P_3$ runs.
    4. $t=3$: $P_4(\text{prio } 10)$ arrives. Since $10 > 6$, $P_3$ is preempted (remaining $BT=2$). $P_4$ runs to completion at $t=4$ ($CT=4$).
    5. $t=4$: $P_5(\text{prio } 9)$ arrives. Ready queue: $P_1(3, \text{prio } 4), P_2(1, \text{prio } 5), P_3(2, \text{prio } 6), P_5(2, \text{prio } 9)$. Highest priority is $P_5(9) \implies P_5$ runs from $t=4$ to $t=6$ ($CT=6$). ($P_6(7)$ arriving at $t=5$ does not preempt $P_5(9)$).
    6. $t=6$: Ready queue: $P_1(3, \text{prio } 4), P_2(1, \text{prio } 5), P_3(2, \text{prio } 6), P_6(6, \text{prio } 7)$. Highest priority is $P_6(7) \implies P_6$ runs from $t=6$ to $t=12$ ($CT=12$).
    7. $t=12$: Highest is $P_3(6) \implies P_3$ runs from $t=12$ to $t=14$ ($CT=14$).
    8. $t=14$: Highest is $P_2(5) \implies P_2$ runs from $t=14$ to $t=15$ ($CT=15$).
    9. $t=15$: $P_1(4)$ runs from $t=15$ to $t=18$ ($CT=18$).

```
[ P1 ][ P2 ][ P3 ][ P4 ][ P5 ][   P6   ][ P3 ][ P2 ][  P1  ]
0     1     2    3     4    6          12    14    15      18
```

---

**Problem 3: Priority Non-Preemptive (Lower Number = Higher Priority)**

- **Process Dataset:**

|Process|Arrival Time ($AT$)|Burst Time ($BT$)|Priority|
|---|---|---|---|
|$P_1$|$0$|$7$|$9$|
|$P_2$|$1$|$3$|$4$|
|$P_3$|$2$|$5$|$2$|
|$P_4$|$3$|$2$|$1$ (Highest)|
|$P_5$|$4$|$6$|$3$|
|$P_6$|$5$|$1$|$8$|

- **Execution Trace & Gantt Chart:**
    1. $t=0$: $P_1$ starts and executes continuously until $t=7$ ($CT=7$).
    2. $t=7$: All processes have arrived. Priority ordering (lower number = higher priority): $P_4(1) > P_3(2) > P_5(3) > P_2(4) > P_6(8)$.
    3. $P_4$ runs from $t=7$ to $t=9$ ($CT=9$).
    4. $P_3$ runs from $t=9$ to $t=14$ ($CT=14$).
    5. $P_5$ runs from $t=14$ to $t=20$ ($CT=20$).
    6. $P_2$ runs from $t=20$ to $t=23$ ($CT=23$).
    7. $P_6$ runs from $t=23$ to $t=24$ ($CT=24$).

```
[  P1 (7)  ][ P4 (2) ][ P3 (5) ][  P5 (6)  ][ P2 (3) ][P6(1)]
0          7         9         14          20        23   24
```

---

**Problem 4: Priority Preemptive (Lower Number = Higher Priority)**

- **Process Dataset:** Same dataset as Problem 3.
- **Execution Trace & Gantt Chart:**
    1. $t=0$: $P_1(7, \text{prio } 9)$ starts.
    2. $t=1$: $P_2(3, \text{prio } 4)$ arrives. Since $4 < 9$, $P_1$ is preempted (remaining $BT=6$). $P_2$ runs.
    3. $t=2$: $P_3(5, \text{prio } 2)$ arrives. Since $2 < 4$, $P_2$ is preempted (remaining $BT=2$). $P_3$ runs.
    4. $t=3$: $P_4(2, \text{prio } 1)$ arrives. Since $1 < 2$, $P_3$ is preempted (remaining $BT=4$). $P_4$ runs to completion at $t=5$ ($CT=5$). ($P_5(3)$ arriving at $t=4$ does not preempt $P_4(1)$).
    5. $t=5$: $P_6(1, \text{prio } 8)$ arrives. Ready queue: $P_1(6, \text{prio } 9), P_2(2, \text{prio } 4), P_3(4, \text{prio } 2), P_5(6, \text{prio } 3), P_6(1, \text{prio } 8)$. Highest priority is $P_3(2) \implies P_3$ runs from $t=5$ to $t=9$ ($CT=9$).
    6. $t=9$: Highest priority is $P_5(3) \implies P_5$ runs from $t=9$ to $t=15$ ($CT=15$).
    7. $t=15$: Highest priority is $P_2(4) \implies P_2$ runs from $t=15$ to $t=17$ ($CT=17$).
    8. $t=17$: Highest priority is $P_6(8) \implies P_6$ runs from $t=17$ to $t=18$ ($CT=18$).
    9. $t=18$: $P_1(6, \text{prio } 9)$ runs from $t=18$ to $t=24$ ($CT=24$).

```
[ P1 ][ P2 ][ P3 ][ P4 ][  P3  ][  P5  ][ P2 ][ P6 ][  P1  ]
0     1     2    3     5       9          15    17   18      24
```

---

MODULE 2: ROUND ROBIN (RR) SCHEDULING & CPU EFFICIENCY

1. RECALL-TRIGGER CONCEPTS & FORMULAS

- **Mechanics:** Preemptive FCFS operating on a fixed **Time Quantum (**$Q$**)**.
- **Ready Queue Insertion Rule (CRITICAL):** On a preemption tick at time $t$:
    1. First, append all **newly arrived processes** at time $t$ to the tail of the Ready Queue.
    2. Second, append the **preempted running process** to the tail of the Ready Queue behind new arrivals.
- **Quantum Selection Boundaries:**
    - If $Q$ is extremely small ($Q \to 0$), context switch overhead dominates, causing CPU efficiency to plunge to near zero ($\eta \approx 0$).
    - If $Q \ge \max(BT_i)$, Round Robin degenerates completely into **FCFS**.
- **CPU Efficiency Formula (**$\eta$**):** Where $Q$ is Time Quantum and $S$ is Context Switch Overhead: $$\eta = \frac{Q}{Q + S} \times 100\%$$
- **Total Context Switches Formula:** $$\text{Total Context Switches} = \left( \sum_{i=1}^n \left\lceil \frac{BT_{P_i}}{Q} \right\rceil \right) - 1$$

---

2. SOLVED PRACTICE PROBLEMS & GATE PYQS

**Problem 5: Round Robin Ready Queue State Trace (**$Q=2$**)**

- **Process Dataset:** $P_1(0, 5)$, $P_2(1, 4)$, $P_3(2, 1)$, $P_4(3, 3)$.
- **Step-by-Step Ready Queue Trace:**
    - $t=0$: $P_1$ runs. Ready Queue: `[P1]`.
    - $t=1$: $P_2$ arrives. Ready Queue: `[P2]`.
    - $t=2$: $P_3$ arrives. $P_1$ quantum expires (remaining $BT=3$). New arrival $P_3$ appended first, then $P_1$. Ready Queue: `[P2, P3, P1]`. $P_2$ runs.
    - $t=3$: $P_4$ arrives. Ready Queue: `[P3, P1, P4]`.
    - $t=4$: $P_2$ quantum expires (remaining $BT=2$). Preempted $P_2$ appended. Ready Queue: `[P3, P1, P4, P2]`. $P_3$ runs from $t=4$ to $t=5$ (completes, $CT=5$).
    - $t=5$: Ready Queue: `[P1, P4, P2]`. $P_1$ runs from $t=5$ to $t=7$ (preempted, remaining $BT=1$). Ready Queue: `[P4, P2, P1]`.
    - $t=7$: $P_4$ runs from $t=7$ to $t=9$ (preempted, remaining $BT=1$). Ready Queue: `[P2, P1, P4]`.
    - $t=9$: $P_2$ runs from $t=9$ to $t=11$ (completes, $CT=11$). Ready Queue: `[P1, P4]`.
    - $t=11$: $P_1$ runs from $t=11$ to $t=12$ (completes, $CT=12$). Ready Queue: `[P4]`.
    - $t=12$: $P_4$ runs from $t=12$ to $t=13$ (completes, $CT=13$).

```
[ P1 ][ P2 ][ P3 ][ P1 ][ P4 ][ P2 ][ P1 ][ P4 ]
0     2     4    5     7     9    11    12    13
```

---

**Problem 6: 7-Process Round Robin (**$Q=3$**) Context Switch Count**

- **Process Dataset:** $P_1(0, 12)$, $P_2(0, 5)$, $P_3(3, 9)$, $P_4(5, 6)$, $P_5(2, 8)$, $P_6(4, 2)$, $P_7(1, 7)$.
- **Mathematical Calculation of Total Context Switches:** $$\sum_{i=1}^7 \left\lceil \frac{BT_{P_i}}{3} \right\rceil = \left\lceil \frac{12}{3} \right\rceil + \left\lceil \frac{5}{3} \right\rceil + \left\lceil \frac{9}{3} \right\rceil + \left\lceil \frac{6}{3} \right\rceil + \left\lceil \frac{8}{3} \right\rceil + \left\lceil \frac{2}{3} \right\rceil + \left\lceil \frac{7}{3} \right\rceil$$ $$\text{Total Execution Slices} = 4 + 2 + 3 + 2 + 3 + 1 + 3 = 18\text{ slices}$$ $$\text{Total Context Switches} = 18 - 1 = \mathbf{17\text{ switches}}$$

---

**Problem 7: GATE-2013 MCQ (Priority Proportional to Waiting Time)**

- **Question:** A scheduling algorithm assigns priority proportional to waiting time. Every process starts with priority $0$. Priorities are re-evaluated every $T$ time units. Which algorithm is this equivalent to?
- **Proof:** At any interval $T$, all waiting processes have their priority increased proportionally to $T$, while the running process gains $0$ waiting time. Thus, the process that has waited longest becomes the highest priority every $T$ units. This creates strict round-robin rotation with time quantum $Q = T$.
- **Correct Answer:** **Option B (Round-Robin algorithm)**.

---

**Problem 8: GATE-2022 MCQ (Impossible Round Robin Sequence)**

- **Question:** Four processes $P, Q, R, S$ arrive at $t=0$ in order $P, Q, R, S$ with $Q=4$. The Gantt sequence is $P \to Q \to R \to S \to Q \to R \to Q$. Which burst time set is **NOT** possible?
    - (A) $P=4, Q=10, R=6, S=2$
    - (B) $P=2, Q=9, R=5, S=1$
    - (C) $P=4, Q=12, R=5, S=4$
    - (D) $P=3, Q=7, R=7, S=3$
- **Proof:**
    - $P$ executes once $\implies BT(P) \le 4$.
    - $S$ executes once $\implies BT(S) \le 4$.
    - $R$ executes twice $\implies 4 < BT(R) \le 8$.
    - $Q$ executes 3 times $\implies 8 < BT(Q) \le 12$.
    - In Option D, $Q = 7$, which is $\le 8$. A burst of $7$ with quantum $4$ requires at most $2$ execution slices ($4 + 3$). It can never execute 3 times!
- **Correct Answer:** **Option D**.

---

MODULE 3: MULTILEVEL QUEUE (MLQ) & MULTILEVEL FEEDBACK QUEUE (MLFQ)

1. RECALL-TRIGGER CONCEPTS

- **Multilevel Queue (MLQ):**
    - Divides the Ready Queue into separate queues based on process type (e.g., System Processes [High], Foreground/Interactive [Medium], Background/Batch [Low]).
    - Each queue has its own scheduling algorithm (e.g., $Q_1$: RR with $Q=2$, $Q_2$: FCFS).
    - **Preemption Rule:** Higher priority queues preempt lower priority queues. Low priority queues suffer from **severe starvation**. Processes cannot move between queues.
- **Multilevel Feedback Queue (MLFQ):**
    - Solves MLQ starvation by allowing processes to **move dynamically between queues**.
    - A process using too much CPU time is demoted to a lower priority queue.
    - A process waiting too long in a lower queue is promoted (aging) to a higher priority queue.

---

2. SOLVED PRACTICE PROBLEMS

**Problem 9: Multilevel Queue Trace (**$Q_1$**: RR** $Q=3$**,** $Q_2$**: FCFS)**

- **Process Dataset:**

|Process|Arrival Time|Burst Time|Queue|
|---|---|---|---|
|$P_1$|$0$|$3$|$Q_1$ (High)|
|$P_2$|$0$|$3$|$Q_1$ (High)|
|$P_3$|$2$|$8$|$Q_2$ (Low)|
|$P_4$|$10$|$4$|$Q_1$ (High)|
|$P_5$|$11$|$6$|$Q_2$ (Low)|
|$P_6$|$11$|$3$|$Q_1$ (High)|
|$P_7$|$19$|$2$|$Q_1$ (High)|
|$P_8$|$13$|$5$|$Q_2$ (Low)|

- **Execution Trace & Gantt Chart:**
    1. $t=0 \to 6$: $Q_1$ runs $P_1(0 \to 3, CT=3)$ and $P_2(3 \to 6, CT=6)$.
    2. $t=6$: $Q_1$ empty. $P_3(Q_2)$ starts running from $t=6$ to $t=10$ (remaining $BT=4$).
    3. $t=10$: $P_4(Q_1)$ arrives. $P_3$ preempted. $P_4$ runs in $Q_1$ from $t=10$ to $t=13$ (remaining $BT=1$).
    4. $t=11$: $P_6(Q_1)$ arrived and entered $Q_1$.
    5. $t=13$: $P_6(Q_1)$ runs from $t=13$ to $t=16$ ($CT=16$).
    6. $t=16$: $P_4(Q_1)$ resumes and runs $t=16$ to $t=17$ ($CT=17$).
    7. $t=17$: $Q_1$ empty. $P_3(Q_2)$ resumes and runs $t=17$ to $t=19$ (remaining $BT=2$).
    8. $t=19$: $P_7(Q_1)$ arrives. $P_3$ preempted. $P_7$ runs in $Q_1$ from $t=19$ to $t=21$ ($CT=21$).
    9. $t=21$: $Q_1$ empty. $P_3(Q_2)$ resumes and completes at $t=23$ ($CT=23$).
    10. $t=23$: $P_5(Q_2)$ runs from $t=23$ to $t=29$ ($CT=29$).
    11. $t=29$: $P_8(Q_2)$ runs from $t=29$ to $t=34$ ($CT=34$).

```
[ P1 ][ P2 ][ P3 ][ P4 ][ P6 ][ P4 ][ P3 ][ P7 ][ P3 ][  P5  ][  P8  ]
0     3    6     10   13    16   17    19   21    23   29    34
```

---

MODULE 4: COMPARATIVE ANALYSIS OF ALL SCHEDULING ALGORITHMS

|Algorithm|Preemptive / Non-Preemptive|Optimal Metric|Starvation Risk|Convoy Effect|Degeneration Case|
|---|---|---|---|---|---|
|**FCFS**|Non-Preemptive|Simple implementation|No|**Yes**|N/A|
|**SJF**|Non-Preemptive|Min Avg WT (Non-preemptive)|Yes (Long jobs)|No|FCFS if all BT equal|
|**SRTF**|Preemptive|**Min Avg WT & TAT (Absolute)**|Yes (Long jobs)|No|SJF if no new smaller BT arrives|
|**LJF**|Non-Preemptive|Favors CPU-bound|Yes (Short jobs)|**Yes**|FCFS if all BT equal|
|**LRTF**|Preemptive|Equalizes remaining BTs|Yes (Short jobs)|No|High-frequency 1-unit switching|
|**HRRN**|Non-Preemptive|Balances short & long jobs|**No**|No|SJF if all WT equal|
|**Priority**|Both|High priority response|Yes (Low priority)|No|Non-preemptive if no higher prio arrives|
|**Round Robin**|Preemptive|Interactive response|**No**|No|**FCFS if** $Q \ge \max(BT_i)$|

---

MODULE 5: ADVANCED CPU & I/O-BOUND OVERLAP SCHEDULING

1. RECALL-TRIGGER CONCEPTS & FORMULAS

- **Process Cycle:** Process alternates CPU bursts and I/O bursts ($\text{CPU}_1 \to \text{I/O} \to \text{CPU}_2$).
- **Multiprogramming CPU Utilization Formula:** Where $p$ is the fraction of time a process spends in I/O, and $n$ is the degree of multiprogramming: $$\text{CPU Utilization} = 1 - p^n$$

---

2. SOLVED PRACTICE PROBLEMS & GATE PYQS

**Problem 10: Non-Preemptive SJF with I/O Overlap**

- **Process Dataset (All arrive at** $t=0$**):**

|Process|$\text{CPU}_1$ Burst|I/O Burst|$\text{CPU}_2$ Burst|
|---|---|---|---|
|$P_1$|$2$|$5$|$5$|
|$P_2$|$4$|$5$|$2$|
|$P_3$|$3$|$2$|$4$|
|$P_4$|$6$|$4$|$1$|

- **Execution Trace:**
    1. $t=0 \to 2$: $P_1$ executes $\text{CPU}_1(2)$. $P_1$ I/O ends at $2+5=7$.
    2. $t=2 \to 5$: $P_3$ executes $\text{CPU}_1(3)$. $P_3$ I/O ends at $5+2=7$.
    3. $t=5 \to 9$: $P_2$ executes $\text{CPU}_1(4)$. $P_2$ I/O ends at $9+5=14$.
    4. At $t=9$: Ready queue: $P_1(\text{CPU}_2=5)$, $P_3(\text{CPU}_2=4)$, $P_4(\text{CPU}_1=6)$. Shortest is $P_3(4) \implies P_3$ executes $\text{CPU}_2$ from $t=9$ to $t=13$ ($CT=13$).
    5. At $t=13$: Ready queue: $P_1(\text{CPU}_2=5)$, $P_4(\text{CPU}_1=6)$. Shortest is $P_1(5) \implies P_1$ executes $\text{CPU}_2$ from $t=13$ to $t=18$ ($CT=18$).
    6. At $t=18$: Ready queue: $P_2(\text{CPU}_2=2$, I/O done at $14$), $P_4(\text{CPU}_1=6)$. Shortest is $P_2(2) \implies P_2$ executes $\text{CPU}_2$ from $t=18$ to $t=20$ ($CT=20$).
    7. At $t=20$: $P_4$ executes $\text{CPU}_1(6)$ from $t=20$ to $t=26$. $P_4$ I/O ends at $26+4=30$.
    8. $t=26 \to 30$: **CPU is IDLE for** $4$ **units**.
    9. $t=30 \to 31$: $P_4$ executes $\text{CPU}_2(1)$ ($CT=31$).

```
[ P1 ][ P3 ][  P2  ][ P3 ][  P1  ][ P2 ][   P4   ][Idle][P4]
0     2     5       9    13      18     20       26    30  31
```

- **KPI Calculations:**
    - Total Time = $31\text{ units}$, Total CPU Busy Time = $27\text{ units}$, CPU Idle Time = $4\text{ units}$.
    - $\%$ **CPU Utilization:** $(27 / 31) \times 100\% = \mathbf{87.10\%}$.
    - $\%$ **CPU Idle Time:** $(4 / 31) \times 100\% = \mathbf{12.90\%}$.
    - **Waiting Times in Ready Queue:**
        - $WT(P_1) = 18 - (2 + 5 + 5) = 6\text{ ms}$.
        - $WT(P_2) = 20 - (4 + 5 + 2) = 9\text{ ms}$.
        - $WT(P_3) = 13 - (3 + 2 + 4) = 4\text{ ms}$.
        - $WT(P_4) = 31 - (6 + 4 + 1) = 20\text{ ms}$.
        - **Average Waiting Time:** $(6 + 9 + 4 + 20) / 4 = \mathbf{9.75\text{ ms}}$.

---

**Problem 11: Preemptive SRTF with I/O Overlap**

- **Process Dataset:**

|Process|Arrival Time|$\text{CPU}_1$|I/O Burst|$\text{CPU}_2$|
|---|---|---|---|---|
|$P_1$|$0$|$6$|$7$|$1$|
|$P_2$|$1$|$4$|$2$|$9$|
|$P_3$|$2$|$1$|$6$|$5$|

- **Execution Trace & Preemption Proof:**
    1. $t=0$: $P_1(6)$ runs.
    2. $t=1$: $P_2(4)$ arrives. Remaining $P_1=5$. Since $4 < 5$, $P_1$ is preempted. $P_2$ runs.
    3. $t=2$: $P_3(1)$ arrives. Remaining $P_2=3$. Since $1 < 3$, $P_2$ is preempted. $P_3$ runs to completion of $\text{CPU}_1$ at $t=3$. $P_3$ I/O ends at $3+6=9$.
    4. $t=3 \to 6$: $P_2$ resumes and completes $\text{CPU}_1$ at $t=6$. $P_2$ I/O ends at $6+2=8$.
    5. $t=6 \to 11$: $P_1$ resumes and completes $\text{CPU}_1$ at $t=11$. $P_1$ I/O ends at $11+7=18$.
    6. $t=11 \to 16$: Ready: $P_2(\text{CPU}_2=9)$ and $P_3(\text{CPU}_2=5)$. Shortest is $P_3 \implies P_3$ executes $\text{CPU}_2$ to completion at $t=16$ ($CT=16$).
    7. $t=16 \to 18$: $P_2(\text{CPU}_2=9)$ executes.
    8. $t=18$: $P_1$ finishes I/O ready for $\text{CPU}_2(1)$. Remaining $P_2=7$. **Since** $1 < 7$**,** $P_2$ **is preempted!** $P_1$ executes $\text{CPU}_2$ from $t=18$ to $t=19$ ($CT=19$).
    9. $t=19 \to 26$: $P_2$ resumes and completes at $t=26$ ($CT=26$).

```
[ P1 ][ P2 ][ P3 ][ P2 ][   P1   ][  P3  ][ P2 ][ P1 ][  P2  ]
0     1     2    3     6        11      16     18   19     26
```

- **Average Waiting Time Calculation:**
    - $WT(P_1) = (6 - 1) + (18 - 18) = 5\text{ ms}$.
    - $WT(P_2) = (3 - 2) + (16 - 8) + (19 - 18) = 1 + 8 + 1 = 10\text{ ms}$.
    - $WT(P_3) = (2 - 2) + (11 - 9) = 2\text{ ms}$.
    - **Average Waiting Time:** $(5 + 10 + 2) / 3 = \mathbf{5.67\text{ ms}}$.

---

**Problem 12: GATE-2006 MCQ (Shortest Remaining Compute Time First)**

- **Question:** Processes $P_1(10), P_2(20), P_3(30)$ arrive at $t=0$. Each spends first $20\%$ in I/O, next $70\%$ in CPU computation, and last $10\%$ in I/O. Calculate $\%$ CPU Idle time under Shortest Remaining Compute Time First.
- **Burst Breakdown:**
    - $P_1(10): \text{I/O}_1=2, \text{CPU}=7, \text{I/O}_2=1$.
    - $P_2(20): \text{I/O}_1=4, \text{CPU}=14, \text{I/O}_2=2$.
    - $P_3(30): \text{I/O}_1=6, \text{CPU}=21, \text{I/O}_2=3$.
- **Execution Trace:**
    - $t=0 \to 2$: CPU idle ($2\text{ units}$) while all processes perform initial I/O.
    - $t=2 \to 9$: $P_1$ executes CPU burst ($7$). ($P_2$ ready at $t=4$, $P_3$ ready at $t=6$; $P_1$ remaining CPU is always smaller).
    - $t=9 \to 23$: $P_2$ executes CPU burst ($14$).
    - $t=23 \to 44$: $P_3$ executes CPU burst ($21$).
    - $t=44 \to 47$: $P_3$ performs final $\text{I/O}_2(3)$. **CPU idle for** $3\text{ units}$. Total execution time = $47\text{ units}$.
- $\%$ **CPU Idle Time Calculation:** $$\text{Total Idle Time} = 2 + 3 = 5\text{ units}$$ $$\% \text{ CPU Idle} = \frac{5}{47} \times 100\% = \mathbf{10.638\% \approx 10.6\%}$$
- **Correct Answer:** **Option B (**$10.6\%$**)**.

---

**Problem 13: GATE-2006 MCQ (Preemptive Priority with I/O Overlaps)**

- **Question:** Arrival, Priority (lower number = higher priority), CPU and I/O bursts:
    - $P_1(AT=0, \text{prio } 2): \text{CPU}_1=1, \text{I/O}=5, \text{CPU}_2=3$.
    - $P_2(AT=2, \text{prio } 3 \text{ [lowest]}): \text{CPU}_1=3, \text{I/O}=3, \text{CPU}_2=1$.
    - $P_3(AT=3, \text{prio } 1 \text{ [highest]}): \text{CPU}_1=2, \text{I/O}=3, \text{CPU}_2=1$. What are the finish times of $P_1, P_2, P_3$?
- **Execution Trace:**
    - $t=0 \to 1$: $P_1$ runs $\text{CPU}_1(1)$. $P_1$ I/O ends at $1+5=6$.
    - $t=1 \to 2$: CPU idle ($1\text{ unit}$).
    - $t=2 \to 3$: $P_2(\text{prio } 3)$ runs $\text{CPU}_1(1\text{ unit})$.
    - $t=3 \to 5$: $P_3(\text{prio } 1)$ arrives, preempts $P_2$ (remaining $\text{CPU}_1=2$), runs $\text{CPU}_1(2)$. $P_3$ I/O ends at $5+3=8$.
    - $t=5 \to 6$: $P_2(\text{prio } 3)$ resumes, runs $\text{CPU}_1(1\text{ unit})$.
    - $t=6 \to 8$: $P_1(\text{prio } 2)$ returns from I/O, preempts $P_2$ (remaining $\text{CPU}_1=1$), runs $\text{CPU}_2(2\text{ units})$.
    - $t=8 \to 9$: $P_3(\text{prio } 1)$ returns from I/O, preempts $P_1$ (remaining $\text{CPU}_2=1$), runs $\text{CPU}_2(1)$ to completion ($P_3$ **finishes at** $t=9$).
    - $t=9 \to 10$: $P_1(\text{prio } 2)$ resumes, completes $\text{CPU}_2(1)$ ($P_1$ **finishes at** $t=10$).
    - $t=10 \to 11$: $P_2(\text{prio } 3)$ completes $\text{CPU}_1(1)$. $P_2$ I/O ends at $11+3=14$.
    - $t=11 \to 14$: CPU idle ($3\text{ units}$).
    - $t=14 \to 15$: $P_2$ executes $\text{CPU}_2(1)$ ($P_2$ **finishes at** $t=15$).
- **Finish Times:** $P_1 = 10, P_2 = 15, P_3 = 9$.
- **Correct Answer:** **Option B (**$10, 15, 9$**)**.

---

**Problem 14: GATE-2003 MCQ (Round Robin vs FCFS CPU Utilization)**

- **Question:** Two processes alternate $10\text{ ms}$ CPU bursts with $90\text{ ms}$ I/O bursts. Which scheduling strategy yields the **least CPU utilization** over a long period?
- **Comparative Proof:**
    - **FCFS / SJF Execution Cycle:** $P_1$ runs $0 \to 10$, $P_2$ runs $10 \to 20$. CPU idle $20 \to 100$ ($80\text{ ms}$). Cycle repeats every $100\text{ ms}$ with $20\text{ ms}$ CPU work. $$\text{CPU Utilization} = \frac{20}{100} = \mathbf{20.00\%}$$
    - **Round Robin (**$Q=5\text{ ms}$**) Execution Cycle:** $P_1$ runs $0 \to 5$, $P_2$ runs $5 \to 10$, $P_1$ runs $10 \to 15$ ($P_1$ I/O ends at $15+90=105$), $P_2$ runs $15 \to 20$ ($P_2$ I/O ends at $20+90=110$). CPU idle $20 \to 105$ ($85\text{ ms}$). Cycle repeats every $105\text{ ms}$ with $20\text{ ms}$ CPU work. $$\text{CPU Utilization} = \frac{20}{105} = \mathbf{19.05\%}$$
- **Conclusion:** Round Robin with $Q=5\text{ ms}$ creates an extra $5\text{ ms}$ of idle delay per cycle, resulting in the **least CPU utilization**.
- **Correct Answer:** **Option D (Round robin scheduling with a time quantum of 5 ms)**.