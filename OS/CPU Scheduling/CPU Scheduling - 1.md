CPU Scheduling Batch 1 Master Study Guide

1. Overview & Learning Objectives

CPU scheduling is a core function of an operating system kernel that determines which process in the ready queue is allocated the CPU when the processor becomes free. A solid understanding of process metrics, preemption rules, queue state tracking, and tie-breaking mechanisms is critical for analyzing system performance and solving competitive engineering problems.

By completing this study guide, you will be able to:

- Define and calculate all fundamental process timing metrics: Arrival Time ($AT$), Burst Time ($BT$), Completion Time ($CT$), Turnaround Time ($TAT$), Waiting Time ($WT$), and Response Time ($RT$).
- Contrast preemptive and non-preemptive scheduling paradigms and identify context-switching overheads.
- Construct Gantt charts and evaluate process execution metrics for First-Come, First-Served (FCFS), Shortest Job First (SJF), Shortest Remaining Time First (SRTF), Longest Job First (LJF), Longest Remaining Time First (LRTF), and Highest Response Ratio Next (HRRN).
- Apply exact tie-breaking chains for concurrent arrivals, equal remaining burst times, and equal priority states.
- Prove mathematical optimality properties, starvation conditions, and convoy effects across various scheduling policies.

---

2. Module 1: OS Scheduling Basics & Process Metrics

2.1 Process Lifecycle Timing Parameters

A process transitions between various states during its lifetime. Understanding the exact time bounds associated with CPU execution is the foundation of scheduling analysis.

- **Arrival Time (**$AT$**):** The absolute timestamp at which a process enters the Ready Queue and becomes eligible for CPU execution.
- **Burst Time (**$BT$ **/ Service Time):** The total CPU time required by a process to complete its execution (excluding I/O operations).
- **Completion Time (**$CT$**):** The absolute timestamp at which a process finishes its execution and exits the system.
- **Turnaround Time (**$TAT$**):** The total time spent by a process in the system from arrival to completion. $$TAT = CT - AT$$
- **Waiting Time (**$WT$**):** The total cumulative time a process spends waiting in the Ready Queue for CPU allocation. $$WT = TAT - BT$$
- **Response Time (**$RT$**):** The time elapsed between a process's arrival in the Ready Queue and its very first allocation of CPU execution time. $$RT = \text{Timestamp of First CPU Execution} - AT$$

**Key Rule for Non-Preemptive Scheduling:** In any non-preemptive scheduling policy, a process runs continuously from its start until completion once allocated the CPU. Consequently, for all non-preemptive algorithms, $RT = WT$.

2.2 Preemptive vs. Non-Preemptive Scheduling

Operating system schedulers operate under two primary execution modes:

1. **Non-Preemptive Scheduling:** Once a process is allocated the CPU, it retains control of the processor until it voluntarily terminates or requests an I/O operation. The kernel cannot forcefully revoke the CPU from a running process.
2. **Preemptive Scheduling:** The operating system kernel can forcefully interrupt a running process and return it to the Ready Queue. Preemption occurs when a higher-priority process arrives, a process with a shorter remaining burst enters the queue, or a time-quantum timer expires.

2.3 Optimization Objectives

System performance is evaluated across key quantitative metrics:

- **Maximize CPU Utilization:** Percentage of time the CPU is actively executing user or system processes.
- **Maximize Throughput:** Number of processes completed per unit of time.
- **Minimize Average Turnaround Time (**$\overline{TAT}$**):** Average residence time of processes in the system.
- **Minimize Average Waiting Time (**$\overline{WT}$**):** Average time processes wait in the Ready Queue.
- **Minimize Average Response Time (**$\overline{RT}$**):** Average time taken to start responding to user requests.

---

3. Module 2: First-Come, First-Served (FCFS) Scheduling

3.1 Mechanics & Properties

First-Come, First-Served (FCFS) is the simplest non-preemptive CPU scheduling algorithm.

- **Scheduling Criterion:** Process Arrival Time ($AT$).
- **Execution Mode:** Non-Preemptive.
- **Tie-Breaker Rule:** If multiple processes arrive at the exact same timestamp, the process with the **lowest Process ID (PID)** is scheduled first.
- **System Properties:**
    - Starvation-Free: Every process in the Ready Queue will eventually be served.
    - Low Scheduling Overhead: Requires a simple First-In, First-Out (FIFO) queue structure.
    - Susceptible to the Convoy Effect.

3.2 The Convoy Effect

The **Convoy Effect** occurs when a CPU-bound process with a very large burst time occupies the CPU, forcing several short, I/O-bound processes to wait in the Ready Queue. This leads to poor CPU and device utilization and severely inflates the average waiting time.

Mathematical Demonstration of Convoy Effect

Consider three processes arriving simultaneously at $t = 0$:

- $P_1$: $AT = 0$, $BT = 28\text{ ms}$
- $P_2$: $AT = 0$, $BT = 4\text{ ms}$
- $P_3$: $AT = 0$, $BT = 4\text{ ms}$

FCFS Execution Timeline (Gantt Chart)

|Execution Slice|Process|Start Time|End Time|
|---|---|---|---|
|Slice 1|$P_1$|$0\text{ ms}$|$28\text{ ms}$|
|Slice 2|$P_2$|$28\text{ ms}$|$32\text{ ms}$|
|Slice 3|$P_3$|$32\text{ ms}$|$36\text{ ms}$|

Performance Metrics Table

|Process|$AT$|$BT$|$CT$|$TAT = CT - AT$|$WT = TAT - BT$|
|---|---|---|---|---|---|
|$P_1$|$0\text{ ms}$|$28\text{ ms}$|$28\text{ ms}$|$28\text{ ms}$|$0\text{ ms}$|
|$P_2$|$0\text{ ms}$|$4\text{ ms}$|$32\text{ ms}$|$32\text{ ms}$|$28\text{ ms}$|
|$P_3$|$0\text{ ms}$|$4\text{ ms}$|$36\text{ ms}$|$36\text{ ms}$|$32\text{ ms}$|

- **Average Turnaround Time:** $$\overline{TAT} = \frac{28 + 32 + 36}{3} = \frac{96}{3} = 32.00\text{ ms}$$
- **Average Waiting Time:** $$\overline{WT} = \frac{0 + 28 + 32}{3} = \frac{60}{3} = 20.00\text{ ms}$$
- **Context Switches Between Processes:** $2$ switches (between $P_1 \to P_2$ and $P_2 \to P_3$). Initial start at $t=0$ and final termination at $t=36$ are ignored by standard convention.

3.3 FCFS with CPU Idle Gaps

When no processes are present in the Ready Queue at $t = 0$ or during intermediate execution intervals, the CPU remains idle.

Worked Problem: FCFS with Idle Intervals

Process dataset:

- $P_1$: $AT = 7$, $BT = 4$
- $P_2$: $AT = 8$, $BT = 2$
- $P_3$: $AT = 6$, $BT = 3$
- $P_4$: $AT = 4$, $BT = 1$
- $P_5$: $AT = 1$, $BT = 2$

Chronological Execution Trace

1. At $t = 0$: Ready Queue is empty. CPU is **idle** from $t = 0$ to $t = 1$.
2. At $t = 1$: $P_5$ arrives and executes from $1$ to $3$ ($CT = 3$).
3. At $t = 3$: Ready Queue is empty. CPU is **idle** from $t = 3$ to $t = 4$.
4. At $t = 4$: $P_4$ arrives and executes from $4$ to $5$ ($CT = 5$).
5. At $t = 5$: Ready Queue is empty. CPU is **idle** from $t = 5$ to $t = 6$.
6. At $t = 6$: $P_3$ arrives and executes from $6$ to $9$ ($CT = 9$).
7. At $t = 9$: $P_1$ (arrived at $7$) and $P_2$ (arrived at $8$) are both ready. $P_1$ arrived earlier, so $P_1$ executes from $9$ to $13$ ($CT = 13$).
8. At $t = 13$: $P_2$ executes from $13$ to $15$ ($CT = 15$).

FCFS Execution Timeline

|Interval|Active State|Duration|Notes|
|---|---|---|---|
|$0 \to 1$|CPU Idle|$1\text{ unit}$|Waiting for $P_5$ arrival|
|$1 \to 3$|$P_5$|$2\text{ units}$|Completes at $t=3$|
|$3 \to 4$|CPU Idle|$1\text{ unit}$|Waiting for $P_4$ arrival|
|$4 \to 5$|$P_4$|$1\text{ unit}$|Completes at $t=5$|
|$5 \to 6$|CPU Idle|$1\text{ unit}$|Waiting for $P_3$ arrival|
|$6 \to 9$|$P_3$|$3\text{ units}$|Completes at $t=9$|
|$9 \to 13$|$P_1$|$4\text{ units}$|Completes at $t=13$|
|$13 \to 15$|$P_2$|$2\text{ units}$|Completes at $t=15$|

Complete Results Table

|Process|$AT$|$BT$|$CT$|$TAT = CT - AT$|$WT = TAT - BT$|
|---|---|---|---|---|---|
|$P_1$|$7$|$4$|$13$|$6$|$2$|
|$P_2$|$8$|$2$|$15$|$7$|$5$|
|$P_3$|$6$|$3$|$9$|$3$|$0$|
|$P_4$|$4$|$1$|$5$|$1$|$0$|
|$P_5$|$1$|$2$|$3$|$2$|$0$|

- **Average Turnaround Time:** $$\overline{TAT} = \frac{6 + 7 + 3 + 1 + 2}{5} = \frac{19}{5} = 3.80\text{ units}$$
- **Average Waiting Time:** $$\overline{WT} = \frac{2 + 5 + 0 + 0 + 0}{5} = \frac{7}{5} = 1.40\text{ units}$$
- **CPU Utilization Calculations:**
    - Total Elapsed Time = $15\text{ units}$
    - Total Idle Time = $1 + 1 + 1 = 3\text{ units}$
    - Total Active CPU Time = $15 - 3 = 12\text{ units}$
    - $$\text{CPU Idle Percentage} = \frac{3}{15} \times 100\% = 20.00\%$$
    - $$\text{CPU Utilization Percentage} = \frac{12}{15} \times 100\% = 80.00\%$$

---

4. Module 3: Shortest Job First (SJF) Scheduling (Non-Preemptive)

4.1 Mechanics & Properties

Shortest Job First (SJF) is a non-preemptive algorithm that allocates the CPU to the process with the smallest Burst Time.

- **Scheduling Criterion:** Smallest Burst Time ($BT$).
- **Execution Mode:** Non-Preemptive.
- **Tie-Breaker Chain:** $$\text{Smallest } BT \longrightarrow \text{If } BT \text{ Equal: FCFS (Earliest } AT) \longrightarrow \text{If } AT \text{ Equal: Lowest PID}$$
- **Fundamental Theorem:** SJF is **provably optimal** for minimizing the average waiting time ($\overline{WT}$) and average turnaround time ($\overline{TAT}$) among all **non-preemptive** CPU scheduling algorithms.
- **Disadvantages:**
    - Cannot be implemented in general-purpose operating systems because future CPU burst times cannot be known in advance.
    - Can lead to **starvation** of long processes if short processes continuously enter the Ready Queue.

4.2 Comparative Proof of Optimality (SJF vs. FCFS)

Revisiting the Convoy Effect dataset under SJF non-preemptive scheduling:

- $P_1$: $AT = 0$, $BT = 28$
- $P_2$: $AT = 0$, $BT = 4$
- $P_3$: $AT = 0$, $BT = 4$

All three processes arrive at $t = 0$. Since $P_2$ and $P_3$ have smaller burst times ($4\text{ ms}$) than $P_1$ ($28\text{ ms}$), they run first. Tie-breaker between $P_2$ and $P_3$ is lowest PID ($P_2$).

SJF Execution Timeline

|Execution Slice|Process|Start Time|End Time|
|---|---|---|---|
|Slice 1|$P_2$|$0\text{ ms}$|$4\text{ ms}$|
|Slice 2|$P_3$|$4\text{ ms}$|$8\text{ ms}$|
|Slice 3|$P_1$|$8\text{ ms}$|$36\text{ ms}$|

Performance Comparison Table

|Process|$AT$|$BT$|FCFS $WT$|SJF $CT$|SJF $TAT$|SJF $WT$|
|---|---|---|---|---|---|---|
|$P_1$|$0$|$28$|$0\text{ ms}$|$36$|$36$|$8\text{ ms}$|
|$P_2$|$0$|$4$|$28\text{ ms}$|$4$|$4$|$0\text{ ms}$|
|$P_3$|$0$|$4$|$32\text{ ms}$|$8$|$8$|$4\text{ ms}$|

- **FCFS Results:** $\overline{TAT} = 32.00\text{ ms}$, $\overline{WT} = 20.00\text{ ms}$.
- **SJF Results:** $$\overline{TAT}_{\text{SJF}} = \frac{36 + 4 + 8}{3} = \frac{48}{3} = 16.00\text{ ms}$$ $$\overline{WT}_{\text{SJF}} = \frac{8 + 0 + 4}{3} = \frac{12}{3} = 4.00\text{ ms}$$
- **Conclusion:** SJF reduces the average waiting time from $20.00\text{ ms}$ down to $4.00\text{ ms}$ (a $80\%$ reduction).

4.3 Worked Problem: SJF with Distinct Arrival Times

Process dataset:

- $P_1$: $AT = 0$, $BT = 3$
- $P_2$: $AT = 1$, $BT = 4$
- $P_3$: $AT = 2$, $BT = 2$
- $P_4$: $AT = 4$, $BT = 1$

Step-by-Step Chronological Execution Trace

1. At $t = 0$: Only $P_1$ has arrived. $P_1$ starts execution and runs to completion at $t = 3$ ($CT = 3$).
2. At $t = 3$: Processes in Ready Queue are $P_2$ (arrived at $t=1$, $BT=4$) and $P_3$ (arrived at $t=2$, $BT=2$).
    - Comparing burst times: $BT(P_3) = 2 < BT(P_2) = 4$.
    - $P_3$ is selected and runs non-preemptively from $t = 3$ to $t = 5$ ($CT = 5$).
3. At $t = 5$: Processes in Ready Queue are $P_2$ (arrived at $t=1$, $BT=4$) and $P_4$ (arrived at $t=4$, $BT=1$).
    - Comparing burst times: $BT(P_4) = 1 < BT(P_2) = 4$.
    - $P_4$ is selected and runs from $t = 5$ to $t = 6$ ($CT = 6$).
4. At $t = 6$: Only $P_2$ remains in Ready Queue. $P_2$ runs from $t = 6$ to $t = 10$ ($CT = 10$).

Complete Metric Calculations

|Process|$AT$|$BT$|$CT$|$TAT = CT - AT$|$WT = TAT - BT$|
|---|---|---|---|---|---|
|$P_1$|$0$|$3$|$3$|$3$|$0$|
|$P_2$|$1$|$4$|$10$|$9$|$5$|
|$P_3$|$2$|$2$|$5$|$3$|$1$|
|$P_4$|$4$|$1$|$6$|$2$|$1$|

$$\overline{WT} = \frac{0 + 5 + 1 + 1}{4} = \frac{7}{4} = 1.75\text{ units}$$ $$\overline{TAT} = \frac{3 + 9 + 3 + 2}{4} = \frac{17}{4} = 4.25\text{ units}$$

---

5. Module 4: Shortest Remaining Time First (SRTF) Scheduling (Preemptive SJF)

5.1 Mechanics & Rules

Shortest Remaining Time First (SRTF) is the preemptive version of Shortest Job First.

- **Scheduling Criterion:** Shortest **Remaining** Burst Time.
- **Execution Mode:** Preemptive.
- **Absolute Optimality Theorem:** SRTF mathematically guarantees the **absolute minimum average waiting time (**$\overline{WT}$**) and average turnaround time (**$\overline{TAT}$**)** among ALL possible CPU scheduling algorithms (preemptive and non-preemptive).

The Preemption Laws & Tie-Breaker Rules

1. **Preemption Condition:** When a new process arrives, compare its total Burst Time against the **remaining** Burst Time of the currently executing process. $$\text{Preempt currently running process } P_{\text{curr}} \iff BT_{\text{new}} < \text{Remaining } BT(P_{\text{curr}})$$
2. **No-Preemption Tie-Breaker Law:** If the Burst Time of the newly arrived process is **exactly equal** to the remaining Burst Time of the running process: $$\text{If } BT_{\text{new}} = \text{Remaining } BT(P_{\text{curr}}) \implies \mathbf{\text{DO NOT PREEMPT!}}$$ The currently running process retains the CPU (favoring continuity and lower context-switching overhead).

5.2 Worked Problem: SRTF Tie-Breaker Verification

Process dataset:

- $P_1$: $AT = 0$, $BT = 4$
- $P_2$: $AT = 1$, $BT = 2$
- $P_3$: $AT = 2$, $BT = 1$

Step-by-Step Execution Trace

- $t = 0$**:** $P_1$ arrives and starts executing.
- $t = 1$**:** $P_2$ arrives with $BT = 2$. Remaining $BT(P_1) = 4 - 1 = 3$.
    - Since $BT(P_2) = 2 < 3$, $P_1$ is preempted (remaining $BT(P_1) = 3$).
    - $P_2$ is allocated the CPU and begins running.
- $t = 2$**:** $P_3$ arrives with $BT = 1$. Currently running $P_2$ has executed for $1\text{ unit}$, so remaining $BT(P_2) = 2 - 1 = 1$.
    - Comparing new arrival vs. running: $BT(P_3) = 1$ vs. Remaining $BT(P_2) = 1$.
    - Since $1$ is **not strictly less than** $1$, **NO PREEMPTION OCCURS!**
    - $P_2$ continues running and completes at $t = 3$ ($CT(P_2) = 3$).
- $t = 3$**:** Ready Queue contains $P_3$ (remaining $BT = 1$) and $P_1$ (remaining $BT = 3$).
    - $P_3$ is allocated the CPU and runs to completion at $t = 4$ ($CT(P_3) = 4$).
- $t = 4$**:** Only $P_1$ remains in Ready Queue. $P_1$ runs from $t = 4$ to $t = 7$ ($CT(P_1) = 7$).

SRTF Execution Timeline

|Interval|Active Process|Remaining BT at Start|State Changes / Events|
|---|---|---|---|
|$0 \to 1$|$P_1$|$4$|Preempted at $t=1$ (rem $BT = 3$)|
|$1 \to 3$|$P_2$|$2$|$P_3$ arrives at $t=2$ (equal $BT$, no preemption); completes at $t=3$|
|$3 \to 4$|$P_3$|$1$|Completes at $t=4$|
|$4 \to 7$|$P_1$|$3$|Resumes and completes at $t=7$|

Metric Results Table

|Process|$AT$|$BT$|$CT$|$TAT = CT - AT$|$WT = TAT - BT$|
|---|---|---|---|---|---|
|$P_1$|$0$|$4$|$7$|$7$|$3$|
|$P_2$|$1$|$2$|$3$|$2$|$0$|
|$P_3$|$2$|$1$|$4$|$2$|$1$|

- **Average Waiting Time:** $$\overline{WT} = \frac{3 + 0 + 1}{3} = \frac{4}{3} = 1.33\text{ ms}$$
- **Total Context Switches:** $3$ (at $t=1$, $t=3$, $t=4$).

5.3 Worked Problem: 6-Process Comprehensive Trace

Process dataset:

- $P_1$: $AT = 0$, $BT = 6$
- $P_2$: $AT = 1$, $BT = 5$
- $P_3$: $AT = 2$, $BT = 1$
- $P_4$: $AT = 3$, $BT = 2$
- $P_5$: $AT = 4$, $BT = 1$
- $P_6$: $AT = 5$, $BT = 4$

Detailed Chronological Clock-Tick Analysis

- $t = 0$**:** $P_1$ starts running.
- $t = 1$**:** $P_2(5)$ arrives. Running $P_1$ has remaining $BT = 5$. Since $5 = 5$, **no preemption**. $P_1$ continues.
- $t = 2$**:** $P_3(1)$ arrives. Running $P_1$ has remaining $BT = 4$. Since $1 < 4$, $P_1$ **is preempted** (rem $BT(P_1) = 4$). $P_3$ starts running.
- $t = 3$**:** $P_3$ completes ($CT = 3$). $P_4(2)$ arrives. Ready Queue: $P_1(4), P_2(5), P_4(2)$. Shortest is $P_4(2) \implies P_4$ starts running.
- $t = 4$**:** $P_5(1)$ arrives. Running $P_4$ has remaining $BT = 1$. Equal remaining times ($1 = 1$) $\implies$ **no preemption**. $P_4$ continues.
- $t = 5$**:** $P_4$ completes ($CT = 5$). $P_6(4)$ arrives. Ready Queue: $P_1(4), P_2(5), P_5(1), P_6(4)$. Shortest is $P_5(1) \implies P_5$ starts running.
- $t = 6$**:** $P_5$ completes ($CT = 6$). Ready Queue: $P_1(4), P_6(4), P_2(5)$. Shortest tied processes are $P_1(4)$ and $P_6(4)$. FCFS tie-breaker ($P_1$ arrived at $0$ vs $P_6$ at $5$) $\implies P_1$ runs from $t = 6$ to $t = 10$ ($CT = 10$).
- $t = 10$**:** Ready Queue: $P_6(4), P_2(5)$. Shortest is $P_6(4) \implies P_6$ runs from $t = 10$ to $t = 14$ ($CT = 14$).
- $t = 14$**:** $P_2(5)$ runs from $t = 14$ to $t = 19$ ($CT = 19$).

Complete Metrics Table

|Process|$AT$|$BT$|$CT$|$TAT = CT - AT$|$WT = TAT - BT$|
|---|---|---|---|---|---|
|$P_1$|$0$|$6$|$10$|$10$|$4$|
|$P_2$|$1$|$5$|$19$|$18$|$13$|
|$P_3$|$2$|$1$|$3$|$1$|$0$|
|$P_4$|$3$|$2$|$5$|$2$|$0$|
|$P_5$|$4$|$1$|$6$|$2$|$1$|
|$P_6$|$5$|$4$|$14$|$9$|$5$|

- **Average Waiting Time:** $$\overline{WT} = \frac{4 + 13 + 0 + 0 + 1 + 5}{6} = \frac{23}{6} = 3.83\text{ ms}$$
- **Total Context Switches:** $6$ (at $t=2, 3, 5, 6, 10, 14$).

5.4 Solved Exam PYQ: SRTF Parameter Deduction

Problem Statement

Four processes are scheduled using SRTF:

- $P_1$: $AT = 0$, $BT = 3$
- $P_2$: $AT = 1$, $BT = 1$
- $P_3$: $AT = 3$, $BT = 3$
- $P_4$: $AT = 4$, $BT = Z$

Given that the average waiting time ($\overline{WT}$) of all four processes is $1.00\text{ ms}$, determine the value of $Z$.

Mathematical Solution & Deduction

1. **Target Sum of Waiting Times:** $$\overline{WT} = \frac{\sum WT}{4} = 1 \implies \sum WT = 4\text{ ms}$$
2. **Trace Execution Chronologically:**
    - $t = 0 \to 1$: $P_1(3)$ runs. Remaining $BT(P_1) = 2$.
    - $t = 1$: $P_2(1)$ arrives. Since $1 < 2$, $P_2$ preempts $P_1$. $P_2$ runs from $t = 1$ to $t = 2$ ($CT = 2$).
        - $$WT(P_2) = CT - AT - BT = 2 - 1 - 1 = 0\text{ ms}$$
    - $t = 2 \to 3$: $P_1$ resumes (remaining $BT = 2$). At $t = 3$, remaining $BT(P_1) = 1$.
    - $t = 3$: $P_3(3)$ arrives. Remaining $BT(P_1) = 1$. Since $1 < 3$, $P_1$ continues and completes at $t = 4$ ($CT = 4$).
        - $$WT(P_1) = CT - AT - BT = 4 - 0 - 3 = 1\text{ ms}$$
    - $t = 4$: $P_4(Z)$ arrives. Ready Queue contains $P_3(BT = 3)$ and $P_4(BT = Z)$.
3. **Evaluate Remaining Required Waiting Time:** $$\sum WT = WT(P_1) + WT(P_2) + WT(P_3) + WT(P_4) = 4$$ $$1 + 0 + WT(P_3) + WT(P_4) = 4 \implies WT(P_3) + WT(P_4) = 3\text{ ms}$$
4. **Case Analysis on** $Z$**:**
    - **Case A (**$Z \ge 3$**):** $P_3(3)$ runs first from $t = 4$ to $t = 7$ ($CT = 7$).
        - $WT(P_3) = 7 - 3 - 3 = 1\text{ ms}$.
        - $P_4(Z)$ runs at $t = 7$. $WT(P_4) = 7 - 4 = 3\text{ ms}$.
        - $WT(P_3) + WT(P_4) = 1 + 3 = 4\text{ ms}$ (Total $\sum WT = 5\text{ ms}$, which exceeds the required $4\text{ ms}$). Invalid!
    - **Case B (**$Z < 3$**, let** $Z = 2$**):** $P_4(2)$ runs first from $t = 4$ to $t = 6$ ($CT = 6$).
        - $WT(P_4) = 6 - 4 - 2 = 0\text{ ms}$.
        - $P_3(3)$ runs from $t = 6$ to $t = 9$ ($CT = 9$).
        - $WT(P_3) = 9 - 3 - 3 = 3\text{ ms}$.
        - $WT(P_3) + WT(P_4) = 3 + 0 = 3\text{ ms}$ (Total $\sum WT = 1 + 0 + 3 + 0 = 4\text{ ms}$). Perfect match!
5. **Final Answer:** $Z = 2$.

---

6. Module 5: Longest Job First (LJF) & Longest Remaining Time First (LRTF)

6.1 Mechanics & Properties

- **LJF (Non-Preemptive):** Schedules the process with the largest Burst Time first.
- **LRTF (Preemptive):** Re-evaluates at every step and schedules the process with the largest remaining Burst Time.
- **System Characteristics:**
    - Favor CPU-bound processes over I/O-bound processes.
    - **Starvation-Free:** Because executing a process reduces its remaining burst time, a long process will eventually drop to the remaining time level of other processes, ensuring all processes get scheduled.
    - Suffer from severe context-switching overheads due to high-frequency preemption.

6.2 High-Frequency Context Switching in LRTF

A defining characteristic of LRTF is its behavior when multiple processes have equal remaining burst times. The scheduler alternates execution among all tied processes in **1-unit CPU time slices**, creating a high-frequency context-switching cascade.

6.3 Worked Problem: LRTF 4-Process Trace

Process dataset:

- $P_1$: $AT = 0$, $BT = 2$
- $P_2$: $AT = 1$, $BT = 3$
- $P_3$: $AT = 2$, $BT = 4$
- $P_4$: $AT = 3$, $BT = 2$

Chronological Execution Trace

- $t = 0 \to 1$**:** $P_1(2)$ runs. Remaining $BT(P_1) = 1$.
- $t = 1 \to 2$**:** $P_2(3)$ arrives. Since $3 > 1$, $P_2$ preempts $P_1$. $P_2$ runs. Remaining $BT(P_2) = 2$.
- $t = 2 \to 3$**:** $P_3(4)$ arrives. Since $4 > 2$, $P_3$ preempts $P_2$. $P_3$ runs. Remaining $BT(P_3) = 3$.
- $t = 3 \to 4$**:** $P_4(2)$ arrives. Queue remaining BTs: $P_1(1), P_2(2), P_3(3), P_4(2)$. Largest is $P_3(3) \implies P_3$ continues. Remaining $BT(P_3) = 2$.
- $t = 4$**:** Remaining BTs: $P_1(1), P_2(2), P_3(2), P_4(2)$.
    - Three-way tie among $P_2, P_3, P_4$ at remaining $BT = 2$.
    - Resolved by FCFS (earliest $AT$): $P_2(AT=1)$ runs from $4 \to 5$ (rem $BT(P_2) = 1$).
- $t = 5$**:** Remaining BTs: $P_1(1), P_2(1), P_3(2), P_4(2)$.
    - Tie between $P_3(2)$ and $P_4(2)$. Earliest $AT$ is $P_3(AT=2) \implies P_3$ runs from $5 \to 6$ (rem $BT(P_3) = 1$).
- $t = 6$**:** Remaining BTs: $P_1(1), P_2(1), P_3(1), P_4(2)$.
    - Largest is $P_4(2) \implies P_4$ runs from $6 \to 7$ (rem $BT(P_4) = 1$).
- $t = 7$**:** All processes now have an equal remaining Burst Time of $1\text{ unit}$!
    - Ready Queue: $P_1(1), P_2(1), P_3(1), P_4(1)$.
    - FCFS tie-breaker dictates order:
        - $P_1$ runs from $7 \to 8$ ($CT(P_1) = 8$).
        - $P_2$ runs from $8 \to 9$ ($CT(P_2) = 9$).
        - $P_3$ runs from $9 \to 10$ ($CT(P_3) = 10$).
        - $P_4$ runs from $10 \to 11$ ($CT(P_4) = 11$).

Complete Results Table

|Process|$AT$|$BT$|$CT$|$TAT = CT - AT$|$WT = TAT - BT$|
|---|---|---|---|---|---|
|$P_1$|$0$|$2$|$8$|$8$|$6$|
|$P_2$|$1$|$3$|$9$|$8$|$5$|
|$P_3$|$2$|$4$|$10$|$8$|$4$|
|$P_4$|$3$|$2$|$11$|$8$|$6$|

**Notable Observation:** Every single process in this execution achieves an identical Turnaround Time of $8\text{ units}$.

6.4 Solved Exam PYQ: LRTF Simultaneous Arrival Trace

Problem Statement

Three processes with Process IDs $0, 1, 2$ have compute burst times of $2, 4,$ and $8$ time units respectively. All processes arrive at time $t = 0$. The system uses LRTF scheduling with ties broken by giving priority to the process with the **lowest Process ID**. Calculate the average turnaround time ($\overline{TAT}$).

Step-by-Step Clock-Tick Execution Trace

- **Initial State (**$t = 0$**):** $P_0(2), P_1(4), P_2(8)$.
- $t = 0 \to 4$**:** $P_2(8)$ is the largest and runs for $4\text{ units}$.
    - At $t = 4$: Remaining $BT(P_2) = 8 - 4 = 4$. Remaining BTs: $P_0(2), P_1(4), P_2(4)$.
- $t = 4 \to 5$**:** Tied largest processes are $P_1(4)$ and $P_2(4)$. Lowest PID tie-breaker selects $P_1 \implies P_1$ runs.
    - At $t = 5$: Remaining $BT(P_1) = 3$. Remaining BTs: $P_0(2), P_1(3), P_2(4)$.
- $t = 5 \to 6$**:** Largest is $P_2(4) \implies P_2$ runs.
    - At $t = 6$: Remaining $BT(P_2) = 3$. Remaining BTs: $P_0(2), P_1(3), P_2(3)$.
- $t = 6 \to 7$**:** Tied largest are $P_1(3)$ and $P_2(3)$. Lowest PID selects $P_1 \implies P_1$ runs.
    - At $t = 7$: Remaining $BT(P_1) = 2$. Remaining BTs: $P_0(2), P_1(2), P_2(3)$.
- $t = 7 \to 8$**:** Largest is $P_2(3) \implies P_2$ runs.
    - At $t = 8$: Remaining $BT(P_2) = 2$. Remaining BTs: $P_0(2), P_1(2), P_2(2)$.
- $t = 8 \to 11$**:** All remaining burst times are equal to $2$. They execute in 1-unit slices by lowest PID ($P_0 \to P_1 \to P_2$):
    - $t = 8 \to 9$: $P_0$ runs (rem $1$).
    - $t = 9 \to 10$: $P_1$ runs (rem $1$).
    - $t = 10 \to 11$: $P_2$ runs (rem $1$).
- $t = 11 \to 14$**:** All remaining burst times are equal to $1$. They complete sequentially by lowest PID ($P_0 \to P_1 \to P_2$):
    - $t = 11 \to 12$: $P_0$ runs to completion ($CT(P_0) = 12$).
    - $t = 12 \to 13$: $P_1$ runs to completion ($CT(P_1) = 13$).
    - $t = 13 \to 14$: $P_2$ runs to completion ($CT(P_2) = 14$).

Results Table

|Process|$AT$|$BT$|$CT$|$TAT = CT - AT$|
|---|---|---|---|---|
|$P_0$|$0$|$2$|$12$|$12$|
|$P_1$|$0$|$4$|$13$|$13$|
|$P_2$|$0$|$8$|$14$|$14$|

$$\overline{TAT} = \frac{12 + 13 + 14}{3} = \frac{39}{3} = 13.00\text{ units}$$

---

7. Module 6: Highest Response Ratio Next (HRRN) Scheduling

7.1 Mechanics & Mathematical Formula

Highest Response Ratio Next (HRRN) is a non-preemptive algorithm designed to balance the advantages of Shortest Job First with starvation prevention.

- **Scheduling Criterion:** Highest Response Ratio ($RR$).
- **Execution Mode:** Non-Preemptive.
- **Response Ratio Formula:** $$RR = \frac{WT + BT}{BT} = \frac{WT}{BT} + 1$$ where $WT$ is the waiting time accumulated by the process so far, and $BT$ is its total CPU Burst Time.

7.2 Proof of Starvation-Free Property

In SJF, long processes can starve if short processes continually enter the system. HRRN prevents starvation through its mathematical structure:

- For **short jobs**, the Burst Time ($BT$) in the denominator is small, giving them an initially higher Response Ratio.
- For **long jobs**, as they wait in the Ready Queue, their Waiting Time ($WT$) in the numerator continuously increases.
- Eventually, $WT$ becomes large enough that the Response Ratio of the long process surpasses that of newly arrived short processes, guaranteeing execution.

7.3 Worked Problem: HRRN Step-by-Step Execution

Process dataset:

- $P_1$: $AT = 0$, $BT = 3$
- $P_2$: $AT = 2$, $BT = 6$
- $P_3$: $AT = 4$, $BT = 4$
- $P_4$: $AT = 6$, $BT = 5$
- $P_5$: $AT = 8$, $BT = 2$

Step-by-Step Chronological Calculations

1. **At** $t = 0$**:** Only $P_1$ has arrived. $P_1$ runs from $t = 0$ to $t = 3$ ($CT = 3$).
2. **At** $t = 3$**:** Processes present in Ready Queue: Only $P_2$ (arrived at $t = 2$).
    - $P_2$ executes non-preemptively from $t = 3$ to $t = 9$ ($CT = 9$).
3. **At** $t = 9$**:** Processes present in Ready Queue: $P_3$ (at $4$), $P_4$ (at $6$), $P_5$ (at $8$).
    - Calculate Response Ratio for all candidate processes at $t = 9$:
        - $$WT(P_3) = 9 - 4 = 5 \implies RR(P_3) = \frac{5 + 4}{4} = \frac{9}{4} = \mathbf{2.25}$$
        - $$WT(P_4) = 9 - 6 = 3 \implies RR(P_4) = \frac{3 + 5}{5} = \frac{8}{5} = 1.60$$
        - $$WT(P_5) = 9 - 8 = 1 \implies RR(P_5) = \frac{1 + 2}{2} = \frac{3}{2} = 1.50$$
    - Highest $RR$ is $P_3$ ($2.25$) $\implies P_3$ executes from $t = 9$ to $t = 13$ ($CT = 13$).
4. **At** $t = 13$**:** Processes present in Ready Queue: $P_4$ (at $6$), $P_5$ (at $8$).
    - Re-calculate Response Ratio for candidate processes at $t = 13$:
        - $$WT(P_4) = 13 - 6 = 7 \implies RR(P_4) = \frac{7 + 5}{5} = \frac{12}{5} = 2.40$$
        - $$WT(P_5) = 13 - 8 = 5 \implies RR(P_5) = \frac{5 + 2}{2} = \frac{7}{2} = \mathbf{3.50}$$
    - Highest $RR$ is $P_5$ ($3.50$) $\implies P_5$ executes from $t = 13$ to $t = 15$ ($CT = 15$).
5. **At** $t = 15$**:** Only $P_4$ remains in Ready Queue. $P_4$ executes from $t = 15$ to $t = 20$ ($CT = 20$).

HRRN Execution Timeline

|Interval|Active Process|Candidate RRs Evaluated|Selected Process & Reason|
|---|---|---|---|
|$0 \to 3$|$P_1$|Only $P_1$ present|Starts at $t=0$|
|$3 \to 9$|$P_2$|Only $P_2$ present|Arrived at $t=2$|
|$9 \to 13$|$P_3$|$RR(P_3)=2.25, RR(P_4)=1.60, RR(P_5)=1.50$|$P_3$ has highest $RR$|
|$13 \to 15$|$P_5$|$RR(P_4)=2.40, RR(P_5)=3.50$|$P_5$ has highest $RR$|
|$15 \to 20$|$P_4$|Only $P_4$ remaining|Completes execution|

Final Performance Metrics Table

|Process|$AT$|$BT$|$CT$|$TAT = CT - AT$|$WT = TAT - BT$|
|---|---|---|---|---|---|
|$P_1$|$0$|$3$|$3$|$3$|$0$|
|$P_2$|$2$|$6$|$9$|$7$|$1$|
|$P_3$|$4$|$4$|$13$|$9$|$5$|
|$P_4$|$6$|$5$|$20$|$14$|$9$|
|$P_5$|$8$|$2$|$15$|$7$|$5$|

- **Average Turnaround Time:** $$\overline{TAT} = \frac{3 + 7 + 9 + 14 + 7}{5} = \frac{40}{5} = 8.00\text{ units}$$
- **Average Waiting Time:** $$\overline{WT} = \frac{0 + 1 + 5 + 9 + 5}{5} = \frac{20}{5} = 4.00\text{ units}$$

---

8. Practice Short-Answer Quiz

Test your understanding of the concepts, tie-breakers, and mathematical proofs covered in Batch 1.

Questions

1. **Question 1:** Why is Response Time ($RT$) equal to Waiting Time ($WT$) for all non-preemptive scheduling algorithms?
2. **Question 2:** Explain the Convoy Effect. Which scheduling algorithm suffers from it, and why?
3. **Question 3:** What is the exact tie-breaking sequence when two processes in SJF have equal burst times and equal arrival times?
4. **Question 4:** In SRTF, if a newly arrived process $P_{\text{new}}$ has $BT = 3\text{ ms}$ and the currently running process $P_{\text{curr}}$ has a remaining burst time of $3\text{ ms}$, does preemption occur? State the exact rule.
5. **Question 5:** Prove mathematically why SJF is optimal for minimizing average waiting time among non-preemptive algorithms.
6. **Question 6:** Why is LRTF completely free from starvation despite favoring longer jobs?
7. **Question 7:** Describe the execution pattern of LRTF when multiple processes in the Ready Queue reach equal remaining burst times.
8. **Question 8:** State the Response Ratio formula used in HRRN and explain how the numerator and denominator prevent starvation.
9. **Question 9:** A process arrives at $t = 2$ with $BT = 5\text{ ms}$. At $t = 12$, calculate its Response Ratio under HRRN if it has not yet been scheduled.
10. **Question 10:** If four processes arrive at $t = 0$ with burst times $1, 2, 3, 4$ under FCFS, what is the total number of inter-process context switches?

---

Answer Key & Explanations

1. **Answer:** In non-preemptive scheduling, once a process is allocated the CPU, it runs without interruption until completion. Thus, its first allocation time is identical to its start time, making $RT = \text{Start} - AT = WT$.
2. **Answer:** The Convoy Effect occurs when a long CPU-bound process occupies the CPU, forcing short I/O-bound processes to wait in the Ready Queue. FCFS suffers from it because it allocates CPU strictly by arrival time without considering burst length.
3. **Answer:** Smallest $BT \to$ FCFS (Earliest $AT$) $\to$ Lowest Process ID (PID).
4. **Answer:** No preemption occurs. Under SRTF, preemption happens if and only if $BT_{\text{new}} < \text{Remaining } BT(P_{\text{curr}})$. When remaining times are equal ($3 = 3$), the running process retains the CPU.
5. **Answer:** Average waiting time is $\frac{1}{n} \sum WT_i$. Scheduling shorter burst times earlier reduces the cumulative waiting time imposed on all subsequent processes in the queue, minimizing the sum.
6. **Answer:** As LRTF executes the longest process, its remaining burst time decreases. Eventually, its remaining burst time equals that of other waiting processes, forcing the scheduler to distribute execution among all processes.
7. **Answer:** They execute in alternating $1\text{-unit}$ CPU time slices, cycling through all tied processes according to FCFS/PID tie-breakers.
8. **Answer:** $RR = \frac{WT + BT}{BT}$. The denominator $BT$ gives an initial advantage to short jobs, while the continuously accumulating $WT$ in the numerator guarantees that long-waiting jobs eventually attain the highest ratio.
9. **Answer:** Accumulated $WT = 12 - 2 = 10\text{ ms}$. $BT = 5\text{ ms}$. $$RR = \frac{10 + 5}{5} = \frac{15}{5} = 3.00$$
10. **Answer:** $3$ context switches (between $P_1 \to P_2$, $P_2 \to P_3$, and $P_3 \to P_4$).

---

9. Essay & Synthesis Questions

Use these open-ended synthesis prompts to test your deep conceptual mastery.

1. **Comparative Analysis:** Compare and contrast FCFS, SJF, SRTF, and HRRN across four dimensions: preemption mode, optimality, starvation risk, and implementation feasibility in a real-world OS.
2. **Preemption Dynamics:** Explain how equal remaining burst times are handled in SRTF versus equal remaining burst times in LRTF, highlighting the difference in context-switching overhead between the two.
3. **Queue State Tracing:** Given a complex arrival pattern, explain how to systematically maintain the Ready Queue state at each clock tick when transitioning between CPU-bound and I/O-bound execution states.
4. **Algorithmic Trade-offs:** Discuss why HRRN is rarely used in general-purpose operating systems despite being starvation-free and balancing short vs. long jobs effectively.
5. **Mathematical Limit Proof:** Prove that as the burst times of all processes in a system approach an equal value $K$, FCFS, SJF, SRTF, LJF, LRTF, and HRRN all produce identical execution orders.

---

10. Glossary of Key Terms

- **Arrival Time (**$AT$**):** The timestamp at which a process enters the Ready Queue.
- **Burst Time (**$BT$**):** The total CPU time required by a process for execution.
- **Completion Time (**$CT$**):** The timestamp at which a process finishes execution.
- **Context Switch:** The process of saving the state/context of a running process and loading the state of the next scheduled process.
- **Convoy Effect:** A phenomenon in FCFS where short processes wait behind a long CPU-bound process, degrading system performance.
- **First-Come, First-Served (FCFS):** A non-preemptive scheduling policy that executes processes in order of arrival.
- **Highest Response Ratio Next (HRRN):** A non-preemptive algorithm that selects processes based on Response Ratio ($RR = \frac{WT+BT}{BT}$).
- **Longest Job First (LJF):** A non-preemptive algorithm that prioritizes processes with the largest total burst time.
- **Longest Remaining Time First (LRTF):** A preemptive algorithm that prioritizes processes with the largest remaining burst time.
- **Non-Preemptive Scheduling:** A scheduling regime where processes voluntarily yield the CPU upon completion or I/O request.
- **Preemptive Scheduling:** A scheduling regime where the kernel can interrupt running processes to reallocate the CPU.
- **Response Ratio (**$RR$**):** The selection metric in HRRN defined as $\frac{WT + BT}{BT}$.
- **Response Time (**$RT$**):** The time from process arrival to its first CPU allocation.
- **Shortest Job First (SJF):** A non-preemptive algorithm that prioritizes processes with the smallest total burst time.
- **Shortest Remaining Time First (SRTF):** A preemptive algorithm that prioritizes processes with the smallest remaining burst time.
- **Starvation:** A condition where a process is indefinitely delayed from receiving CPU time due to higher-priority processes continuously arriving.
- **Turnaround Time (**$TAT$**):** Total time spent by a process in the system ($CT - AT$).
- **Waiting Time (**$WT$**):** Total cumulative time spent by a process waiting in the Ready Queue ($TAT - BT$).

thumb_upGood report thumb_downBad report