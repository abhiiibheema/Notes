Process Synchronization Part 2 (Lectures 04-05) Revision Notes & Solved Questions

---

MODULE 1: BOUNDED BUFFER (PRODUCER-CONSUMER) PROBLEM

Problem Definition

The Bounded Buffer (or Producer-Consumer) problem describes a scenario where two classes of processes—**Producers** and **Consumers**—share a fixed-size buffer consisting of $N$ memory slots.

```
+---+---+---+---+---+ ... +-----+
| A | B | C | D |   |     |     |  <-- Shared Circular Buffer (Capacity N)
+---+---+---+---+---+ ... +-----+
  0   1   2   3   4        N-1
  ^                         ^
  |                         |
Producers                 Consumers
(Insert items)           (Remove items)
```

- **Producer:** Generates data items and attempts to insert them into available buffer slots.
- **Consumer:** Removes items from occupied buffer slots and processes them.
- **Synchronization Rules:**
    1. **Producer Blocking:** A producer must block if the buffer is full ($empty = 0$).
    2. **Consumer Blocking:** A consumer must block if the buffer is empty ($full = 0$).
    3. **Mutual Exclusion:** Only one process (producer or consumer) can modify the buffer pointers and content at any given time to avoid race conditions.

---

Shared Variables & Semaphores

|Variable|Type|Initial Value|Description / Purpose|
|---|---|---|---|
|`mutex`|Binary Semaphore|`1`|Enforces mutual exclusion during buffer modification.|
|`full`|Counting Semaphore|`0`|Tracks the number of occupied/filled buffer slots.|
|`empty`|Counting Semaphore|$N$|Tracks the number of available/empty buffer slots.|

---

Correct Solution Code

```
// Shared Variables Initialisation
semaphore mutex = 1;
semaphore full  = 0;
semaphore empty = N;

// Producer Process
void Producer() {
    while(true) {
        /* Produce an item */
        
        wait(empty);    // Decrement empty slots count; block if empty == 0
        wait(mutex);    // Acquire lock for buffer modification
        
        /* Add produced item to buffer */
        
        signal(mutex);  // Release lock
        signal(full);   // Increment full slots count; wake up blocked consumer
    }
}

// Consumer Process
void Consumer() {
    while(true) {
        wait(full);     // Decrement full slots count; block if full == 0
        wait(mutex);    // Acquire lock for buffer modification
        
        /* Remove item from buffer */
        
        signal(mutex);  // Release lock
        signal(empty);  // Increment empty slots count; wake up blocked producer
        
        /* Consume the item */
    }
}
```

---

Critical Deadlock Traps (Statement Swapping Proofs)

1. Producer Swapping Trap

If the first two statements of the Producer process are swapped:

```
// Swapped Producer
wait(mutex);   // Line 1
wait(empty);   // Line 2
/* Add item */
signal(mutex);
signal(full);
```

- **Proof of Deadlock (When Buffer is FULL):**
    1. Assume the buffer is **FULL** ($empty = 0$, $full = N$, $mutex = 1$).
    2. The Producer executes Line 1: `wait(mutex)`. Since $mutex = 1$, it succeeds. $mutex$ becomes $0$.
    3. The Producer executes Line 2: `wait(empty)`. Since $empty = 0$, the Producer **blocks** inside `wait(empty)`.
    4. A Consumer now attempts to remove an item to free space. It executes `wait(full)`, which succeeds ($full = N-1$).
    5. The Consumer then attempts to execute `wait(mutex)`. Since $mutex = 0$ (held by the blocked Producer), the Consumer **blocks** on $mutex$.
    6. **Conclusion:** The Producer holds $mutex$ while waiting for $empty > 0$. The Consumer holds an item space while waiting for $mutex = 1$. Neither can proceed. **DEADLOCK!**

2. Consumer Swapping Trap

If the first two statements of the Consumer process are swapped:

```
// Swapped Consumer
wait(mutex);   // Line 1
wait(full);    // Line 2
/* Remove item */
signal(mutex);
signal(empty);
```

- **Proof of Deadlock (When Buffer is EMPTY):**
    1. Assume the buffer is **EMPTY** ($empty = N$, $full = 0$, $mutex = 1$).
    2. The Consumer executes Line 1: `wait(mutex)`. Since $mutex = 1$, it succeeds. $mutex$ becomes $0$.
    3. The Consumer executes Line 2: `wait(full)`. Since $full = 0$, the Consumer **blocks** inside `wait(full)`.
    4. A Producer now attempts to produce an item. It executes `wait(empty)`, which succeeds ($empty = N-1$).
    5. The Producer then attempts to execute `wait(mutex)`. Since $mutex = 0$ (held by the blocked Consumer), the Producer **blocks** on $mutex$.
    6. **Conclusion:** The Consumer holds $mutex$ while waiting for $full > 0$. The Producer holds an empty slot while waiting for $mutex = 1$. Neither can proceed. **DEADLOCK!**

---

MODULE 2: READERS-WRITERS PROBLEM

Problem Definition

The Readers-Writers problem models access to a shared resource (such as a file or database) accessed concurrently by two types of processes:

- **Readers:** Perform read-only operations.
- **Writers:** Perform write/update operations.

Access Compatibility Rules

|Active Process|Incoming Reader|Incoming Writer|
|---|---|---|
|**Reader**|Allowed ($\checkmark$)|Blocked ($\times$)|
|**Writer**|Blocked ($\times$)|Blocked ($\times$)|

- **Concurrent Readers:** Multiple readers may access the resource simultaneously.
- **Exclusive Writer:** A writer requires absolute exclusive access (no other readers or writers allowed).

---

Shared Variables & Semaphores

|Variable|Type|Initial Value|Description / Purpose|
|---|---|---|---|
|`mutex`|Binary Semaphore|`1`|Protects updates to the integer variable `readcount`.|
|`wrt`|Binary Semaphore|`1`|Enforces exclusive writing access / locks out writers when readers are present.|
|`readcount`|Integer|`0`|Maintains the count of active Reader processes inside the Critical Section.|

---

Code Structure

```
// Shared Variables Initialisation
semaphore mutex = 1;
semaphore wrt   = 1;
int readcount   = 0;

// Writer Process
void Writer() {
    while(true) {
        wait(wrt);          // Request exclusive access
        
        /* Perform Writing */
        
        signal(wrt);         // Release exclusive access
    }
}

// Reader Process
void Reader() {
    while(true) {
        // Entry Section for Readers
        wait(mutex);
        readcount++;
        if (readcount == 1) {
            wait(wrt);      // First reader locks 'wrt' to block writers
        }
        signal(mutex);

        /* Perform Reading */

        // Exit Section for Readers
        wait(mutex);
        readcount--;
        if (readcount == 0) {
            signal(wrt);     // Last reader releases 'wrt' to allow writers
        }
        signal(mutex);
    }
}
```

---

Execution Analysis & Properties

1. **First Reader Lock:** When the first reader process executes, `readcount` becomes `1`. It executes `wait(wrt)`, acquiring the `wrt` semaphore and locking out all writers.
2. **Cascading Reader Entry:** Subsequent readers increment `readcount` under protection of `mutex`. Because `readcount > 1`, they skip `wait(wrt)` and enter the reading section directly without blocking.
3. **Last Reader Release:** As readers leave, `readcount` is decremented. When the active reader count hits `0` (`readcount == 0`), the leaving reader executes `signal(wrt)`, unlocking the resource for any waiting writer.
4. **System Trade-off (Writer Starvation):** If new readers continuously arrive before the existing readers complete execution, `readcount` never reaches `0`. Consequently, waiting writers will suffer from **indefinite starvation**.

---

MODULE 3: DINING PHILOSOPHERS PROBLEM

Problem Definition

$K$ philosophers are seated around a circular dining table with $K$ chopsticks placed between them. Each philosopher alternates between thinking and eating.

```
         Philosopher P0
          /          \
   Chopstick ch[0]  Chopstick ch[1]
        /              \
Philosopher P4       Philosopher P1
       |                |
   Chopstick ch[4]  Chopstick ch[2]
        \              /
Philosopher P3 ----- Philosopher P2
          Chopstick ch[3]
```

- **Chopsticks Semaphores:** Binary semaphore array `ch[K]` initialized to `{1, 1, ..., 1}`.
- **Resource Requirements:** To eat, Philosopher $P_i$ must acquire two chopsticks:
    1. Left chopstick: `ch[i]`
    2. Right chopstick: `ch[(i + 1) % K]`

---

Naive Solution Pseudocode

```
// Code for Philosopher P_i
void Philosopher(int i) {
    while(true) {
        wait(ch[i]);                 // Pick up left chopstick
        wait(ch[(i + 1) % K]);       // Pick up right chopstick
        
        /* Eat Spaghetti */
        
        signal(ch[i]);               // Put down left chopstick
        signal(ch[(i + 1) % K]);     // Put down right chopstick
        
        /* Think */
    }
}
```

---

Deadlock Proof

- **Scenario:** All $K$ philosophers become hungry simultaneously.
- **Trace:**
    1. Every philosopher $P_i$ ($0 \le i \le K-1$) concurrently executes `wait(ch[i])`.
    2. All chopsticks $ch[0 \dots K-1]$ drop from $1$ to $0$. Every philosopher successfully holds their left chopstick.
    3. Next, every $P_i$ attempts to execute `wait(ch[(i + 1) % K])` to acquire their right chopstick.
    4. Since $ch[(i + 1) % K] = 0$ for all $i$, every philosopher **blocks**.
- **Outcome:** Circular dependency is formed where every process holds one resource and waits forever for another. **DEADLOCK!**

---

3 Deadlock Avoidance Methods

1. **Limit Table Capacity:** Allow at most $K - 1$ philosophers to sit at the table simultaneously (using a counting semaphore initialized to $K - 1$).
    - _Mechanism:_ With $K-1$ philosophers and $K$ chopsticks, the Pigeonhole Principle guarantees at least one philosopher can obtain both chopsticks, eat, and release them.
2. **Asymmetric Chopstick Pick-up:**
    - Odd-numbered philosophers pick up the left chopstick first, then the right chopstick.
    - Even-numbered philosophers pick up the right chopstick first, then the left chopstick.
    - _Mechanism:_ Breaks the symmetric circular wait condition.
3. **Simultaneous / Atomic Pickup:**
    - A philosopher picks up chopsticks only if **both** left and right chopsticks are available at the exact same instant.
    - _Mechanism:_ Implemented by enclosing the chopstick acquisition logic inside a critical section protected by a binary semaphore.

---

MODULE 4: CHARACTERISTICS & ADVANCED SEMAPHORE TRAPS

Characteristics & Advantages vs. Disadvantages

Advantages

- **Machine Independence:** Abstract hardware-independent synchronization primitives.
- **Flexibility:** Easily adaptable to both mutual exclusion and generalized multi-instance resource management.
- **Order Enforcement:** Effectively coordinates operational sequences across asynchronous processes.

Disadvantages & Pitfalls

- **Deadlock Risks:** Incorrect ordering of `wait()` and `signal()` operations causes catastrophic system lockups.
- **Starvation:** Unfair queuing algorithms or high process contention can lock processes out indefinitely.
- **Priority Inversion:** A lower-priority process holding a semaphore blocks a higher-priority process, while an intermediate process preempts the lower-priority one.
- **Busy Waiting (Spinlock Burden):** Naive implementations waste CPU cycles evaluating while loops ($while(S \le 0)$).

---

Busy Waiting vs. Non-Busy Waiting Semaphores

Busy Waiting (Spinlock)

```
wait(S) {
    while (S <= 0); // Busy wait consumes CPU time
    S--;
}
```

Non-Busy Waiting (Block / Wakeup Implementation)

```
typedef struct {
    int value;
    struct process *queue;
} Semaphore;

void wait(Semaphore S) {
    S.value--;
    if (S.value < 0) {
        // Add this process to S.queue
        block(); // Transitions state from Running -> Blocked
    }
}

void signal(Semaphore S) {
    S.value++;
    if (S.value <= 0) {
        // Remove a process P from S.queue
        wakeup(P); // Transitions state from Blocked -> Ready
    }
}
```

---

Advanced Code Manipulation Traps

1. Process P10 Signal Modification Question

Consider 10 processes $P_1, P_2, \dots, P_{10}$ sharing binary semaphore $S = 1$. Processes $P_1 \dots P_9$ have code: `wait(S); CS; signal(S);` Process $P_{10}$ has modified entry code: `signal(S); CS; signal(S);`

Case A: Single Execution (Processes execute only once)

- **Trace:**
    1. $P_1$ executes `wait(S)` $\rightarrow S$ becomes `0`, $P_1$ enters CS. (Count in CS = 1)
    2. $P_{10}$ executes entry `signal(S)` $\rightarrow S$ becomes `1`, $P_{10}$ enters CS. (Count in CS = 2)
    3. $P_2$ executes `wait(S)` $\rightarrow S$ becomes `0`, $P_2$ enters CS. (Count in CS = 3)
    4. Remaining processes $P_3 \dots P_9$ call `wait(S)` and block because $S = 0$.
- **Maximum processes in CS together = 3** ($P_1, P_{10}, P_2$).

Case B: Infinite Loop (`while(true)`)

```
// P10 code inside while(true)
while(true) {
    signal(S);
    CS;
    signal(S);
}
```

- **Trace:** Because $P_{10}$ runs in an infinite loop, it continually issues `signal(S)` operations at the start of its iterations. Every `signal(S)` increases $S$ back to 1, enabling another blocked process ($P_3, P_4, \dots, P_9$) to execute `wait(S)` and enter CS.
- **Maximum processes in CS together = 10** (All processes can enter CS simultaneously).

---

2. Enforcing Sequence: $P_0 \rightarrow P_1 \rightarrow P_0 \rightarrow P_1 \dots$

Requires **2 Binary Semaphores**: Initialized as $S_0 = 1, S_1 = 0$.

```
// Process P0                     // Process P1
while(true) {                     while(true) {
    wait(S0);                         wait(S1);
    /* Code P0 */                     /* Code P1 */
    signal(S1);                       signal(S0);
}                                 }
```

---

3. Enforcing Sequence: $P_1 \rightarrow P_0 \rightarrow P_2 \rightarrow P_1 \rightarrow P_0 \rightarrow P_2 \dots$

Requires **3 Binary Semaphores**: Initialized as $S_0 = 0, S_1 = 1, S_2 = 0$.

```
// Process P0                     // Process P1                     // Process P2
while(true) {                     while(true) {                     while(true) {
    wait(S0);                         wait(S1);                         wait(S2);
    /* Code P0 */                     /* Code P1 */                     /* Code P2 */
    signal(S2);                       signal(S0);                       signal(S1);
}                                 }                                 }
```

---

4. GATE-2022 MCQ: Sequence `BCABCABCA...`

Three threads $T_1, T_2, T_3$ synchronized using binary semaphores $S_1, S_2, S_3$.

```
// T1                             // T2                             // T3
while(true) {                     while(true) {                     while(true) {
    wait(S3);                         wait(S1);                         wait(S2);
    print("C");                       print("B");                       print("A");
    signal(S2);                       signal(S3);                       signal(S1);
}                                 }                                 }
```

- **Derivation:**
    1. Desired sequence: $B \rightarrow C \rightarrow A \rightarrow B \rightarrow C \rightarrow A \dots$
    2. First character printed is `'B'`, which is executed by thread $T_2$.
    3. For $T_2$ to run first, `wait(S1)` must succeed immediately $\implies \mathbf{S_1 = 1}$.
    4. Threads $T_1$ and $T_3$ must be blocked initially:
        - $T_1$ blocked on `wait(S3)` $\implies \mathbf{S_3 = 0}$.
        - $T_3$ blocked on `wait(S2)` $\implies \mathbf{S_2 = 0}$.
    5. $T_2$ prints `'B'`, then issues `signal(S3)` (unblocks $T_1$). $T_1$ prints `'C'`, issues `signal(S2)` (unblocks $T_3$). $T_3$ prints `'A'`, issues `signal(S1)` (unblocks $T_2$).
- **Correct Initialization:** $\mathbf{S_1 = 1, S_2 = 0, S_3 = 0}$ **(Option C)**.

---

5. Precedence Graph Execution Control

To enforce precedence constraints ($P_0$ and $P_1$ run first concurrently, then $P_2$ runs after both complete):

```
     P0       P1
      \      /
       v    v
         P2
```

- **Semaphores:** $S_0 = 1, S_1 = 1, S_{21} = 0, S_{22} = 0$.

```
// Process P0                     // Process P1                     // Process P2
wait(S0);                         wait(S1);                         wait(S21);
/* Execute P0 */                  /* Execute P1 */                  wait(S22);
signal(S21);                      signal(S22);                      /* Execute P2 */
                                                                    signal(S0);
                                                                    signal(S1);
```

---

MODULE 5: SOLVED GATE PYQS & MATHEMATICAL DERIVATIONS

Solved Question 1: NAT Semaphore Blocking Calculation

Problem Statement

Consider a non-negative counting semaphore $S$. The operation $P(S)$ decrements $S$, and $V(S)$ increments $S$. During an execution, $43$ $P(S)$ operations and $27$ $V(S)$ operations are issued in some order. Find the largest initial value of $S$ for which at least $5$ $P(S)$ operations remain blocked.

Mathematical Derivation

1. **Given Data:**
    - Total $P(S)$ operations issued = $43$.
    - Minimum blocked $P(S)$ operations = $5$.
    - Total $V(S)$ operations issued = $27$.
2. **Successful** $P(S)$ **Operations:** $$\text{Successful } P(S) = \text{Total } P(S) - \text{Blocked } P(S) = 43 - 5 = 38$$
3. **Blocking Condition Equation:** A $P(S)$ operation blocks if and only if $S$ reduces to $0$. To maximize the initial value $x$ while guaranteeing at least $5$ blocked $P(S)$ operations, $S$ must reach exactly $0$ after consuming all successful $P(S)$ operations and all $V(S)$ operations: $$\text{Initial Value } (x) - \text{Successful } P(S) + \text{Total } V(S) = 0$$ $$x - 38 + 27 = 0$$ $$x - 11 = 0 \implies x = 11$$

Final Answer

$$\mathbf{11}$$

---

Solved Question 2: GATE-2016 NAT Semaphore Blocking

Problem Statement

Consider a non-negative counting semaphore $S$. During an execution, $20$ $P(S)$ operations and $12$ $V(S)$ operations are issued in some order. What is the largest initial value of $S$ for which at least one $P(S)$ operation will remain blocked?

Mathematical Derivation

1. **Given Data:**
    - Total $P(S)$ operations issued = $20$.
    - Minimum blocked $P(S)$ operations = $1$.
    - Total $V(S)$ operations issued = $12$.
2. **Successful** $P(S)$ **Operations:** $$\text{Successful } P(S) = 20 - 1 = 19$$
3. **Blocking Condition Equation:** $$\text{Initial Value } (x) - \text{Successful } P(S) + \text{Total } V(S) = 0$$ $$x - 19 + 12 = 0$$ $$x - 7 = 0 \implies x = 7$$

Final Answer

$$\mathbf{7}$$

---

Solved Question 3: GATE-2013 NAT - Race Condition Maximum Value

Problem Statement

A shared variable $x$, initialized to $0$, is operated on by four concurrent processes $W, X, Y, Z$.

- Processes $W$ and $X$: Read $x$, increment by $1$, store to memory, and terminate.
- Processes $Y$ and $Z$: Read $x$, decrement by $2$, store to memory, and terminate.

Each process executes $P(S)$ before reading $x$ and $V(S)$ after storing $x$. Semaphore $S$ is initialized to $2$. What is the maximum possible value of $x$ after all processes complete execution?

Mathematical Derivation

1. **Semaphore Analysis:** Since $S = 2$, two processes can enter their critical sections concurrently. Mutual exclusion **fails**, creating race conditions!
2. **Operations breakdown:**
    - $W$: $x = x + 1$
    - $X$: $x = x + 1$
    - $Y$: $x = x - 2$
    - $Z$: $x = x - 2$
3. **Execution Trace for Maximum Value:** To maximize $x$, the decrement operations of $Y$ and $Z$ must be completely overwritten/invalidated by the final write-backs of $W$ and $X$.
    - **Step 1:** Processes $Y$ and $Z$ invoke `wait(S)`. Since $S=2$, both enter. Both read $x = 0$ into local registers.
    - **Step 2:** $Y$ and $Z$ calculate local results ($0 - 2 = -2$).
    - **Step 3:** Context switch occurs before $Y$ or $Z$ can write back to memory.
    - **Step 4:** $Y$ and $Z$ write back $-2$ to memory.
    - **Step 5:** Process $W$ executes: reads $x = 0$ (if it read $x=0$ at the start before $Y/Z$ wrote $-2$), increments to $1$, and writes $x = 1$.
    - **Step 6:** Process $X$ executes: reads $x = 1$, increments to $2$, and writes $x = 2$ last.
    - **Final Memory State:** The write-back $x = 2$ overwrites all earlier decrements.

Final Answer

$$\mathbf{2}$$

---

Solved Question 4: Distinct Values Race Condition NAT

Problem Statement

A shared variable $x$, initialized to $0$, is operated on by four concurrent processes $W, X, Y, Z$.

- Processes $W$ and $X$: Increment $x$ by $2$.
- Processes $Y$ and $Z$: Decrement $x$ by $3$.

Counting semaphore $S$ is initialized to $2$. What is the total number of distinct possible final values of $x$?

Step-by-Step Derivation

1. **Standard Serial Execution (No Overwriting):** $$x = 0 + 2 + 2 - 3 - 3 = -2$$
2. **Overwriting Scenarios (Race Conditions):** Because $S = 2$, any set of process updates can read an initial state $0$ concurrently and overwrite other updates upon exit.

The set of distinct values generated by all valid race-interleavings is enumerated as follows:

1. **Only** $W$ **survives:** $0 + 2 = 2$
2. **Only** $Y$ **survives:** $0 - 3 = -3$
3. $W$ **and** $X$ **survive:** $0 + 2 + 2 = 4$
4. $Y$ **and** $Z$ **survive:** $0 - 3 - 3 = -6$
5. $W$ **and** $Y$ **survive:** $0 + 2 - 3 = -1$
6. $W, X,$ **and** $Y$ **survive:** $0 + 2 + 2 - 3 = 1$
7. $W, Y,$ **and** $Z$ **survive:** $0 + 2 - 3 - 3 = -4$
8. **All operations survive:** $0 + 2 + 2 - 3 - 3 = -2$
9. **Distinct Values Set:** $$\text{Values} = \{2, -3, 4, -6, -1, 1, -4, -2\}$$ Count of distinct values = $8$.

Final Answer

$$\mathbf{8}$$

---

Solved Question 5: GATE-2023 MCQ - Minimum Values of X

Problem Statement

Consider two functions `incr` and `decr`:

```
void incr() {               void decr() {
    wait(s);                    wait(s);
    X = X + 1;                  X = X - 1;
    signal(s);                  signal(s);
}                           }
```

There are $5$ threads invoking `incr()` once and $3$ threads invoking `decr()` once on shared variable $X$ (initial value $X = 10$).

- **Implementation I1:** $s$ is a binary semaphore initialized to $1$.
- **Implementation I2:** $s$ is a counting semaphore initialized to $2$.

Let $V1, V2$ be the minimum possible values of $X$ at the end of execution for $I1$ and $I2$ respectively. Which choice corresponds to $(V1, V2)$?

Step-by-Step Derivation

1. Evaluation of Implementation I1 ($s$ is Binary Semaphore, Init = 1)

- **Property:** Initial value $s = 1$ guarantees strict **Mutual Exclusion**.
- All $5$ increments and $3$ decrements execute atomically without interference.
- The order of thread execution does not affect the arithmetic outcome: $$V1 = 10 + (5 \times 1) - (3 \times 1) = 10 + 5 - 3 = 12$$
- Since mutual exclusion holds strictly, $12$ is the **only** possible outcome. Thus, minimum $V1 = 12$.

2. Evaluation of Implementation I2 ($s$ is Counting Semaphore, Init = 2)

- **Property:** Initial value $s = 2$ allows two threads into the critical section simultaneously, breaking mutual exclusion and introducing race conditions.
- **Goal:** Find the **minimum possible final value** ($V2$).
- To minimize $X$, we must maximize the effect of `decr()` operations and overwrite/loss of `incr()` operations.
- **Execution Trace for Minimum Value:**
    1. All $3$ `decr()` threads execute sequentially without being overwritten: $$X = 10 - 1 - 1 - 1 = 7$$
    2. All $5$ `incr()` threads read $X = 10$ concurrently before any writes occur, calculate local values $11$, and complete their write-backs before the decrements finalize, OR the decrements read $X = 10$, compute $7$, and commit their write-back LAST.
    3. The write-back of $7$ overwrites all $5$ increment operations.
    4. Minimum final value $V2 = 7$.

3. Conclusion

$$(V1, V2) = (12, 7)$$

Correct Option

**Option C: (12, 7)**