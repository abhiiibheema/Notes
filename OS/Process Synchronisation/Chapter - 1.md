Process Synchronization Part 1 (Lectures 01-03) Revision Notes & Solved Questions

---

MODULE 1: COOPERATING PROCESSES, RACE CONDITIONS & CRITICAL SECTION PROBLEM (CSP)

1. Independent vs. Cooperating Processes

Operating systems categorize processes based on whether their execution is affected by or affects other running processes:

|Feature|Independent Processes|Cooperating / Coordinating / Communicating Processes|
|---|---|---|
|**Data Sharing**|Shares no data, variables, state, or memory with other processes.|Shares data, memory, buffers, files, or variables.|
|**Communication**|No communication needed with other processes.|Communicates via shared memory or message passing.|
|**Execution Speed**|Deterministic; unaffected by other processes.|Non-deterministic; depends on context switching and execution order.|
|**Synchronization**|Not required.|**Mandatory** to ensure expected outcomes.|

```
Ways of Communication:
 ├── 1. Shared Variables / Shared Resource Access
 └── 2. Message Passing
```

Impact of Missing Synchronization in Cooperating Processes:

1. **Inconsistency**: Shared data becomes corrupted due to interleaved writes.
2. **Loss of Data**: Overwriting values prior to reading leads to lost updates.
3. **Deadlock**: Concurrent processes wait infinitely for resources held by each other.

---

2. Race Condition

A **Race Condition** is an undesirable situation that occurs when multiple concurrent processes execute on shared data, and the final output depends strictly on the exact sequence or order in which instructions execute and context switches occur.

Assembly-Level Vulnerability Example

High-level statements (e.g., `count++` or `count--`) are not atomic. Modern architectures translate a single high-level statement into three machine-level instructions:

```
High-Level: count++                 High-Level: count--
-------------------                 -------------------
Instr 1: LOAD  R1, count            Instr 1: LOAD  R2, count
Instr 2: ADD   R1, #1               Instr 2: SUB   R2, #1
Instr 3: STORE count, R1            Instr 3: STORE count, R2
```

If a context switch happens between `LOAD` and `STORE`, the processes read stale values, leading to erroneous final state calculations.

---

3. Critical Section Problem (CSP)

Does an entire process require synchronization? **No.** Synchronization is required only for the specific instructions accessing shared data.

```
+-------------------------------------------------------+
|                   PROCESS STRUCTURE                   |
+-------------------------------------------------------+
|  Entry Section     --> Requests entry to access CS    |
|  Critical Section  --> Code segment accessing shared   |
|                        variables/resources            |
|  Exit Section       --> Announces completion of CS    |
|  Remainder Section --> Non-critical local operations  |
+-------------------------------------------------------+
```

- **Critical Section (CS)**: A code segment where shared variables or shared resources are accessed.
- **Critical Section Problem**: Providing synchronization mechanisms for Critical Section execution to guarantee system consistency.

---

4. The 3 Essential Criteria for CSP Solutions

Any solution proposed to resolve the Critical Section Problem must fulfill specific criteria:

```
Criteria Requirements:
 ├── Primary Rules (Compulsory)
 │    ├── 1. Mutual Exclusion
 │    └── 2. Progress
 └── Secondary Rule
      └── 3. Bounded Waiting
```

1. Mutual Exclusion (Mandatory Primary Rule)

If process $P_i$ is executing in its Critical Section, no other process is allowed to execute in its Critical Section simultaneously.

```
       Process P1                   Process P2
+----------------------+     +----------------------+
|    Entry Section     |     |    Entry Section     |
|  [ CRITICAL SECTION ]| <---| BLOCKED from entry   |
|    Exit Section      |     |                      |
+----------------------+     +----------------------+
```

2. Progress (Mandatory Primary Rule)

If no process is executing in its Critical Section and some processes wish to enter, only those processes **NOT** executing in their Remainder Section can participate in deciding which process enters next. This selection cannot be postponed indefinitely (prevents deadlocks and blocks uninterested processes from interfering).

3. Bounded Waiting (Secondary Rule)

There must be a limit or bound on the number of times other processes are allowed to enter their Critical Sections after a process has made a request to enter and before that request is granted (prevents starvation of waiting processes).

---

MODULE 2: SOFTWARE-BASED SYNCHRONIZATION SOLUTIONS

Core Rules for Solution Analysis

1. A process can be preempted at **any time** and after executing **any number of instructions**.
2. To disprove a condition (Mutual Exclusion, Progress, Bounded Waiting), it is sufficient to find **one valid counter-example** using an instruction interleaving trace.

---

Solution 1: Single Lock Variable (Software Lock)

```
Boolean lock = false; // Shared variable: false = free, true = locked

// Process P0 or P1
while (true) {
    while (lock);   // Entry Section: loop until lock becomes false
    lock = true;    // Set lock
    /* Critical Section */
    lock = false;   // Exit Section: release lock
    /* Remainder Section */
}
```

Evaluation

- **Mutual Exclusion**: **FAILS**.
    - _Counter-Example_: Assume `lock = false`. Process $P_0$ evaluates `while(lock)` as `false` and is preempted before setting `lock = true`. Process $P_1$ scheduled, evaluates `while(lock)` as `false`, sets `lock = true`, and enters CS. Process $P_0$ resumes, sets `lock = true`, and enters CS. Both processes are in CS concurrently.
- **Progress**: **SATISFIED**.
- **Bounded Waiting**: **FAILS**.

---

Solution 2: Strict Alternation / Turn Variable (2 Processes: $P_0$ and $P_1$)

```
int turn = 0; // Shared variable

// Process P0                          // Process P1
while (true) {                         while (true) {
    while (turn != 0);                     while (turn != 1);
    /* Critical Section */                  /* Critical Section */
    turn = 1;                              turn = 0;
    /* Remainder Section */                /* Remainder Section */
}                                      }
```

Evaluation

- **Mutual Exclusion**: **SATISFIED**. Since `turn` is a single variable, it can equal either `0` or `1`, allowing at most one process into CS.
- **Progress**: **FAILS**.
    - _Counter-Example_: Assume `turn = 0`. Process $P_0$ enters CS, exits, and sets `turn = 1`. Process $P_0$ now wants to re-enter CS immediately, but $P_1$ is executing in its Remainder Section and has no interest in entering CS. $P_0$ is trapped in `while(turn != 0)` because `turn` remains `1`. An uninterested process ($P_1$) prevents an interested process ($P_0$) from making progress.
- **Bounded Waiting**: **SATISFIED** (Bound = 1 turn).

---

Solution 3: Flag Array / Desired Array

```
boolean flag[2] = {false, false}; // Shared array

// Process Pi (where j = 1 - i)
while (true) {
    flag[i] = true;     // Express interest
    while (flag[j]);    // Wait if other process is interested
    /* Critical Section */
    flag[i] = false;    // Reset interest
    /* Remainder Section */
}
```

Evaluation

- **Mutual Exclusion**: **SATISFIED**.
- **Progress**: **FAILS** (Causes **Deadlock**).
    - _Counter-Example_: Both processes execute `flag[0] = true` and `flag[1] = true` almost simultaneously before evaluating their `while` loops. Now, $P_0$ loops forever on `while(flag[1])` and $P_1$ loops forever on `while(flag[0])`. Neither process enters CS.
- **Bounded Waiting**: **SATISFIED**.

---

Solution 4: Peterson's Algorithm (2-Process Solution)

Peterson's Algorithm combines the concepts of the **Turn Variable** and the **Flag Array** to provide a complete software solution for two processes.

```
// Shared Variables
boolean flag[2] = {false, false}; 
int turn;

// Structure for Process Pi (where j = 1 - i)
while (true) {
    flag[i] = true;                  // Announce interest
    turn = j;                        // Yield priority to other process
    while (flag[j] && turn == j);    // Busy wait condition
    
    /* Critical Section */
    
    flag[i] = false;                 // Reset interest
    
    /* Remainder Section */
}
```

Mathematical & Logical Proof

```
                  P0 Entry                       P1 Entry
            flag[0] = true                 flag[1] = true
              turn = 1                       turn = 0
                 │                              │
                 ▼                              ▼
    while (flag[1] && turn==1)     while (flag[0] && turn==0)
```

1. **Mutual Exclusion Proof**:
    - For both $P_0$ and $P_1$ to be in CS simultaneously, their respective `while` conditions must evaluate to `false`.
    - For $P_0$ in CS: $flag[1] == false \lor turn == 0$.
    - For $P_1$ in CS: $flag[0] == false \lor turn == 1$.
    - If both are interested, $flag[0] == true$ and $flag[1] == true$.
    - Thus, entry requires $turn == 0$ (for $P_0$) and $turn == 1$ (for $P_1$).
    - Since `turn` is a single atomic memory location, it can only equal `0` or `1` at any given instant. Thus, both conditions cannot be true at once. **Mutual Exclusion is Satisfied.**
2. **Progress Proof**:
    - If $P_1$ has no interest in CS, $flag[1] = false$. $P_0$ executes `turn = 1`, checks `while(flag[1] && turn == 1)`, which evaluates to `false` immediately due to $flag[1]$. $P_0$ enters CS without delay.
    - If both want to enter, `turn` holds the value written by whichever process executed `turn = j` _last_. The last writer yields entry to the other, resolving deadlocks. **Progress is Satisfied.**
3. **Bounded Waiting Proof**:
    - Upon exiting CS, $P_0$ sets $flag[0] = false$. This breaks $P_1$'s `while` loop condition immediately.
    - If $P_0$ tries to re-enter CS immediately, it sets $flag[0] = true$ and $turn = 1$, yielding to $P_1$. $P_1$ enters CS.
    - $P_1$ waits at most **1 entry**. **Bounded Waiting is Satisfied.**
4. **Modern Hardware Limitation**: Out-of-order execution engines and hardware instruction reordering in contemporary CPUs can execute memory operations out of sequence, breaking software-only synchronization algorithms like Peterson's without hardware memory barriers.

---

MODULE 3: HARDWARE-ASSISTED SYNCHRONIZATION

1. Atomic Instructions Concept

An **Atomic Instruction** is an uninterruptible instruction executed at the hardware level in a single CPU cycle. Context switches cannot occur during atomic instruction execution.

---

2. TestAndSet() Instruction

Hardware Definition

```
boolean TestAndSet(boolean *target) {
    boolean rv = *target; // Read original value
    *target = true;       // Set target to true
    return rv;            // Return original value
}
```

Application in CSP Entry Section

```
Boolean lock = false; // Shared variable

// Process Pi
while (true) {
    while (TestAndSet(&lock)); // Busy wait loop
    
    /* Critical Section */
    
    lock = false;              // Exit Section
    
    /* Remainder Section */
}
```

Trace & Evaluation

- **First Process Entry**: $P_0$ executes `TestAndSet(&lock)`. It reads `rv = false`, updates `lock = true`, and returns `false`. The loop `while(false)` terminates; $P_0$ enters CS.
- **Second Process Entry Attempt**: $P_1$ executes `TestAndSet(&lock)`. It reads `rv = true`, sets `lock = true`, and returns `true`. Loop `while(true)` spins continuously.

```
Evaluation Summary:
 ├── Mutual Exclusion: SATISFIED
 ├── Progress        : SATISFIED
 └── Bounded Waiting : FAILS (Unbounded spinlock leads to process starvation)
```

---

3. Swap() Instruction

Hardware Definition

```
void Swap(boolean *a, boolean *b) {
    boolean temp = *a;
    *a = *b;
    *b = temp;
}
```

Application in CSP Entry Section

```
Boolean lock = false; // Shared variable

// Process Pi
while (true) {
    Boolean key = true; // Local variable per process
    
    while (key == true) {
        Swap(&lock, &key);
    }
    
    /* Critical Section */
    
    lock = false;       // Exit Section
    
    /* Remainder Section */
}
```

Evaluation

- **First Process Entry**: $P_0$ sets local `key = true`. It executes `Swap(&lock, &key)`, resulting in `lock = true` and `key = false`. The loop `while(key == true)` terminates; $P_0$ enters CS.
- **Second Process Entry Attempt**: $P_1$ sets `key = true`. Executing `Swap` exchanges `lock (true)` and `key (true)`, keeping `key = true`. $P_1$ loops until $P_0$ writes `lock = false`.

```
Evaluation Summary:
 ├── Mutual Exclusion: SATISFIED
 ├── Progress        : SATISFIED
 └── Bounded Waiting : FAILS (Starvation possible without a waiting queue)
```

---

MODULE 4: SEMAPHORES (BINARY & COUNTING)

1. Definition & Operating Principles

A **Semaphore** $S$ is an integer variable that, apart from initialization, is accessed only through two standard, hardware-enforced atomic operations: `wait()` (or $P()$ / Degrade) and `signal()` (or $V()$ / Upgrade).

Atomic Definitions

```
void wait(Semaphore S) {
    while (S <= 0); // Busy wait (Spinlock implementation)
    S--;
}

void signal(Semaphore S) {
    S++;
}
```

---

2. Binary Semaphore (Mutex)

- **Value Range**: Strictly `0` or `1`.
- **Primary Use**: Mutual Exclusion for critical sections across arbitrary numbers of processes.

```
Semaphore mutex = 1; // Initialized to 1

// Process Pi
while (true) {
    wait(mutex);
    /* Critical Section */
    signal(mutex);
    /* Remainder Section */
}
```

---

3. Counting Semaphore

- **Value Range**: Unrestricted non-negative integers ($0, 1, 2, \dots, N$).
- **Primary Use**: Resource allocation with multiple instances and enforcing execution ordering constraints across processes.

Execution Order Enforcement Example ($P_1 \to P_2$)

To force statement $S_1$ in process $P_1$ to execute before statement $S_2$ in process $P_2$:

```
Semaphore synch = 0; // Initialize semaphore to 0

// Process P1                           // Process P2
S1;                                    wait(synch);
signal(synch);                         S2;
```

---

4. Implementation Types: Busy Waiting vs. Block/Wakeup

A. Busy Waiting (Spinlock)

Processes spin continuously in CPU instruction execution while waiting for entry. Wastes clock cycles.

B. Block / Wakeup (Non-Busy Waiting Implementation)

To eliminate CPU cycle waste, semaphores are redefined using an integer value and a process sleep queue:

```
typedef struct {
    int value;
    struct process *list; // Queue of waiting processes
} semaphore;

void wait(semaphore *S) {
    S->value--;
    if (S->value < 0) {
        // Add this process to S->list
        block(); // Suspends process, yields CPU
    }
}

void signal(semaphore *S) {
    S->value++;
    if (S->value <= 0) {
        // Remove a process P from S->list
        wakeup(P); // Moves process to Ready Queue
    }
}
```

- **Physical Meaning of Negative Semaphore Value**: If `S->value < 0`, its absolute magnitude $|S->value|$ represents the exact number of processes currently blocked and waiting in `S->list`.

---

MODULE 5: SOLVED LECTURE PRACTICE PROBLEMS & GATE PYQS

Problem 1: Counting Semaphore Value Derivation

Question

A counting semaphore $S$ is initialized to `10`. Subsequently, `6` $P$ (wait) operations and `4` $V$ (signal) operations are executed on $S$. What is the final value of $S$?

Derivation

$$S_{\text{final}} = S_{\text{initial}} - (\text{Number of } P \text{ operations}) + (\text{Number of } V \text{ operations})$$ $$S_{\text{final}} = 10 - 6 + 4 = 8$$

**Final Answer:** `8`

---

Problem 2: Resource Allocation via Counting Semaphore

Question

A counting semaphore $S$ is initialized to `7`. A total of `20` processes attempt to access resources governed by $S$. Each process performs a `wait(S)` before entry and a `signal(S)` upon completion.

1. How many processes can enter the Critical Section simultaneously?
2. If all `20` processes attempt entry simultaneously, how many processes will be in the blocked queue, and what is the value of $S$ in a block/wakeup implementation?

Derivation

1. The initial value $S = 7$ defines the available resource instances. Thus, a maximum of **7 processes** can enter the Critical Section concurrently.
2. If 20 processes execute `wait(S)`: $$\text{Blocked Processes} = \text{Total Requests} - \text{Available Resources} = 20 - 7 = 13$$ $$S_{\text{value}} = 7 - 20 = -13$$

**Final Answer:** Max Concurrent in CS = `7`; Blocked Processes = `13`; Semaphore Value = `-13`.

---

Problem 3: Enforcing Execution Order $S_1 \to S_2 \to S_3$

Question

Given 3 processes $P_1, P_2, P_3$ containing statements $S_1, S_2, S_3$ respectively. Construct code using binary semaphores `synch1` and `synch2` to force execution strictly in order $S_1 \to S_2 \to S_3$.

Solution & Derivation

Initialize semaphores: `synch1 = 0`, `synch2 = 0`.

```
// Process P1               // Process P2               // Process P3
S1;                         wait(synch1);               wait(synch2);
signal(synch1);             S2;                         S3;
                            signal(synch2);
```

- **Trace**:
    1. $P_2$ and $P_3$ are blocked immediately if scheduled first since `synch1 == 0` and `synch2 == 0`.
    2. $P_1$ executes $S_1$, then calls `signal(synch1)`, raising `synch1` to `1`.
    3. $P_2$ unblocks, executes $S_2$, then calls `signal(synch2)`, raising `synch2` to `1`.
    4. $P_3$ unblocks and executes $S_3$. Order guaranteed.

---

Problem 4: Race Condition Value Derivation for `count++` and `count--`

Question

Shared variable `count = 5`. $P_1$ executes `count++` and $P_2$ executes `count--` concurrently. Derive step-by-step assembly interleavings resulting in final values of `4`, `5`, and `6`.

Assembly Definitions

```
P1 (count++):                     P2 (count--):
L1: LOAD  R1, count               L2: LOAD  R2, count
I1: ADD   R1, #1                  D2: SUB   R2, #1
S1: STORE count, R1               S2: STORE count, R2
```

Derivation Traces

Case A: Final Value = 5 (Serial Execution)

1. $P_1$ executes $L_1, I_1, S1 \implies \text{count} = 6$.
2. $P_2$ executes $L_2, D_2, S2 \implies \text{count} = 5$.

Case B: Final Value = 4 ($P_2$ Writes Last)

1. $P_1$ executes $L_1$ ($R_1 = 5$) and $I_1$ ($R_1 = 6$). Context Switch to $P_2$.
2. $P_2$ executes $L_2$ ($R_2 = 5$), $D_2$ ($R_2 = 4$). Context Switch to $P_1$.
3. $P_1$ executes $S_1$ ($\text{count} = 6$). Context Switch to $P_2$.
4. $P_2$ executes $S_2$ ($\text{count} = R_2 = 4$). **Final Value = 4**.

Case C: Final Value = 6 ($P_1$ Writes Last)

1. $P_1$ executes $L_1$ ($R_1 = 5$), $I_1$ ($R_1 = 6$). Context Switch to $P_2$.
2. $P_2$ executes $L_2$ ($R_2 = 5$), $D_2$ ($R_2 = 4$), $S_2$ ($\text{count} = 4$). Context Switch to $P_1$.
3. $P_1$ executes $S_1$ ($\text{count} = R_1 = 6$). **Final Value = 6**.

---

Problem 5: Process Forking & Wait Loop Print Calculation (GATE 2024 NAT)

Question

Consider the following code snippet using `fork()` and `wait()` system calls:

```
int x = 3;
while (x > 0) {
    fork();
    printf("hello");
    wait(NULL);
    x--;
}
```

Find the total number of times the `printf` statement is executed.

Step-by-Step Mathematical Derivation

Let $T(x)$ be the total number of `printf` outputs generated by a process and all its descendants given counter value $x$.

When a process enters a loop iteration at value $x$:

1. It executes `fork()`, splitting into 2 processes (Parent and Child).
2. Both Parent and Child execute `printf("hello")`, yielding **2 prints** directly for this step.
3. The parent process calls `wait(NULL)`, blocking until its child process completes its execution tree.
4. The child completes the loop with value $x-1$ ($T(x-1)$ prints).
5. The parent resumes after the child terminates and completes its loop with value $x-1$ ($T(x-1)$ prints).

Recurrence relation: $$T(x) = 2 + 2 \cdot T(x-1) \quad \text{for } x > 0$$ $$T(0) = 0$$

Evaluation:

- $T(1) = 2 + 2 \cdot T(0) = 2 + 2(0) = 2$
- $T(2) = 2 + 2 \cdot T(1) = 2 + 2(2) = 6$
- $T(3) = 2 + 2 \cdot T(2) = 2 + 2(6) = 14$

**Final Answer:** `14`

---

Problem 6: Race Condition on Concurrent Division & Addition

Question

Shared variable $X = 10$. Process $P_1$: $X = X / 2$ Process $P_2$: $X = X + 4$ How many different values of $X$ are possible after both processes finish executing?

Derivation Traces

- **Case 1 (**$P_1$ **then** $P_2$**)**: $X = 10 / 2 = 5 \implies X = 5 + 4 = 9$.
- **Case 2 (**$P_2$ **then** $P_1$**)**: $X = 10 + 4 = 14 \implies X = 14 / 2 = 7$.
- **Case 3 (**$P_1, P_2$ **read** $X=10$ **concurrently,** $P_2$ **writes last)**:
    - $P_1$ reads $10$, computes $5$.
    - $P_2$ reads $10$, computes $14$.
    - $P_1$ writes $X = 5$.
    - $P_2$ writes $X = 14$. **Final** $X = 14$.
- **Case 4 (**$P_1, P_2$ **read** $X=10$ **concurrently,** $P_1$ **writes last)**:
    - $P_2$ reads $10$, computes $14$, writes $14$.
    - $P_1$ writes stored computation $X = 5$. **Final** $X = 5$.

Set of distinct values = $\{9, 7, 14, 5\}$.

**Final Answer:** `4` distinct values.

---

Problem 7: Shared Variable Computation (Process A & Process B)

Question

Shared variable $X = 4$.

- **Process A**: `int Y; Y = X * 2; X = Y;`
- **Process B**: `int Z; Z = X + 1; X = Z;` Find the total number of distinct possible values of $X$ after completion.

Derivation Traces

- **Case 1 (A fully, then B)**: $Y = 4 \times 2 = 8 \implies X = 8$. Process B: $Z = 8 + 1 = 9 \implies X = 9$.
- **Case 2 (B fully, then A)**: $Z = 4 + 1 = 5 \implies X = 5$. Process A: $Y = 5 \times 2 = 10 \implies X = 10$.
- **Case 3 (A and B read** $X=4$**; A writes last)**: B sets $Z = 5$, A sets $Y = 8$. B writes $X = 5$, then A writes $X = 8$.
- **Case 4 (A and B read** $X=4$**; B writes last)**: A sets $Y = 8$, B sets $Z = 5$. A writes $X = 8$, then B writes $X = 5$.

Set of distinct values = $\{9, 10, 8, 5\}$.

**Final Answer:** `4` distinct values.

---

Problem 8: Shared Variable $B$ Final Values (GATE PYQ)

Question

Shared variable $B = 2$.

- **Function P1()**: `C = B - 1; B = 2 * C;`
- **Function P2()**: `D = 2 * B; B = D - 1;` Find the number of distinct values $B$ can take after execution.

Derivation Traces

- **Case 1 (P1 then P2)**: $C = 2 - 1 = 1 \implies B = 2 \times 1 = 2$. P2: $D = 2 \times 2 = 4 \implies B = 4 - 1 = 3$.
- **Case 2 (P2 then P1)**: $D = 2 \times 2 = 4 \implies B = 4 - 1 = 3$. P1: $C = 3 - 1 = 2 \implies B = 2 \times 2 = 4$.
- **Case 3 (Both read** $B=2$**; P1 writes last)**: P2 computes $D = 4, B = 3$. P1 computes $C = 1$, writes $B = 2 \times 1 = 2$.
- **Case 4 (Both read** $B=2$**; P2 writes last)**: P1 computes $C = 1, B = 2$. P2 computes $D = 4$, writes $B = 4 - 1 = 3$.

Set of distinct values = $\{3, 4, 2\}$.

**Final Answer:** `3` distinct values.

---

Problem 9: Three Concurrent Processes on Uniprocessor (GATE PYQ)

Question

Three concurrent processes $P_1, P_2, P_3$ access shared variable $D$ initialized to `100`.

- $P_1$: $D = D + 20$
- $P_2$: $D = D - 50$
- $P_3$: $D = D + 10$ Processes run on a uniprocessor time-shared system. Let $X$ and $Y$ be the minimum and maximum possible final values of $D$.
- Find $Y - X$.
- Find the total number of distinct possible values of $D$.

Derivation

1. **Minimum Value (**$X$**)**:
    - $P_2$ reads $D = 100$.
    - $P_1$ and $P_3$ run sequentially: $D = 100 + 20 + 10 = 130$.
    - $P_2$ writes its stored calculation: $D = 100 - 50 = 50$.
    - Thus, $X = 50$.
2. **Maximum Value (**$Y$**)**:
    - $P_1$ reads $D = 100$.
    - $P_2$ runs fully: $D = 100 - 50 = 50$.
    - $P_3$ runs fully: $D = 50 + 10 = 60$.
    - $P_1$ writes its stored calculation: $D = 100 + 20 = 120$.
    - Alternatively, if $P_1$ and $P_2$ both read $100$, $P_2$ writes $50$, $P_1$ writes $120$, then $P_3$ executes on $120$: $D = 120 + 10 = 130$.
    - Thus, $Y = 130$.

$$Y - X = 130 - 50 = 80$$

1. **Distinct Values Analysis**:
    - **1 Process Impact Retained (Overwrites others)**:
        - $P_3$ last write over $100 \implies 110$
        - $P_1$ last write over $100 \implies 120$
        - $P_2$ last write over $100 \implies 50$
    - **2 Processes Impact Retained**:
        - $P_1$ overwritten, $P_2 \to P_3 \implies 130$
        - $P_2$ overwritten, $P_1 \to P_3 \implies 60$
        - $P_3$ overwritten, $P_1 \to P_2 \implies 70$
    - **3 Processes Impact Retained (All serial combinations)**:
        - $100 + 20 - 50 + 10 = 80$

Distinct values set = $\{50, 60, 70, 80, 110, 120, 130\}$.

**Final Answer:** $Y - X =$ `80`; Total Distinct Values = `7`.

---

Problem 10: Multi-Process Variable Bounds

Question

Shared variable $X = 20$. Total 7 processes execute: 4 processes increase $X$ by `2`, and 3 processes decrease $X$ by `3`. Find the minimum and maximum possible final values of $X$.

Derivation

- **Minimum Value**: All 4 increment processes complete their operations first ($X = 20 + 4 \times 2 = 28$). Then, all 3 decrement processes execute without lost updates: $$X_{\text{min}} = 20 - 3 - 3 - 3 = 11 \quad (\text{if all increment ops are lost/overwritten by decrements})$$
- **Maximum Value**: All 3 decrement processes execute first, and then all 4 increment processes execute: $$X_{\text{max}} = 20 + 2 + 2 + 2 + 2 = 28 \quad (\text{if all decrement ops are lost/overwritten by increments})$$

**Final Answer:** Min Value = `11`; Max Value = `28`.

---

Problem 11: Peterson's Logic Homework Verification Problems

Question 1 Verification (Lecture Notes Question 1)

```
int turn = 0; // Shared variable

// Process P0 or P1
while (true) {
    while (turn);
    turn = 1;
    /* Critical Section */
    turn = 0;
    /* Remainder Section */
}
```

- **Mutual Exclusion**: **FAILS**. If `turn = 0`, both $P_0$ and $P_1$ can evaluate `while(turn)` as `false` simultaneously, set `turn = 1`, and enter CS together.
- **Progress**: **SATISFIED**.

---

Question 2 Verification (Lecture Notes Question 2)

```
lock = False; // Shared variable

// Process P0                          // Process P1
while (true) {                         while (true) {
    while (lock != False);                 while (lock != True);
    /* Critical Section */                  /* Critical Section */
    lock = True;                           lock = False;
    /* Remainder Section */                /* Remainder Section */
}                                      }
```

- **Mutual Exclusion**: **SATISFIED**.
- **Bounded Waiting**: **SATISFIED**.
- **Progress**: **FAILS** (Forced strict alternation between `True` and `False`).

---

Question 3 Verification (Lecture Notes Question 3)

```
boolean flag[2] = {false, false};

// Process Pi (j = 1 - i)
while (true) {
    flag[i] = true;
    while (flag[j]);
    /* Critical Section */
    flag[i] = false;
    /* Remainder Section */
}
```

- **Mutual Exclusion**: **SATISFIED**.
- **Progress**: **FAILS** (Deadlock occurs if both set `flag[i] = true` simultaneously).
- **Bounded Waiting**: **SATISFIED**.