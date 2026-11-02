Deadlock Complete Notes & Diagrams (Lectures 01-03)

---

1. Module 1: Deadlock Fundamentals & Necessary Conditions

Deadlock Definition & Core Mechanics

In multi-programmed operating systems, a **deadlock** is an execution state wherein two or more processes are permanently blocked because each process holds a set of allocated system resources while waiting for additional resources currently held by other processes in the same waiting set. Because no process can proceed without acquiring its requested resource, and no process can voluntarily release its currently held resources until it completes execution, the system enters a static state of permanent blockage. Deadlocked processes consume zero CPU cycles, yet they indefinitely tie up critical hardware and system assets.

```
+-----------------------------------------------------------------------------------+
|                              DEADLOCK DEFINITION                                  |
|  "A set of processes is in a deadlock state when every process in the set         |
|   is waiting for an event that can only be caused by another process in the set."  |
+-----------------------------------------------------------------------------------+
```

OS Resource Allocation Example (3-Process Holding/Waiting Table)

To ground this theoretical framework in hardware execution, consider a system with three concurrently running processes ($P_1, P_2, P_3$) and three distinct system resources ($\text{Printer}, \text{Disk Drive}, \text{Digital Camera}$):

|Process|Resource Currently Held|Resource Requested (Waiting For)|
|---|---|---|
|$\mathbf{P_1}$|$\text{Printer}$|$\text{Disk Drive}$|
|$\mathbf{P_2}$|$\text{Disk Drive}$|$\text{Camera}$|
|$\mathbf{P_3}$|$\text{Camera}$|$\text{Printer}$|

_Mechanics of Failure_: $P_1$ cannot complete until it obtains the $\text{Disk Drive}$, which is locked by $P_2$. $P_2$ cannot release the $\text{Disk Drive}$ until it obtains the $\text{Camera}$, locked by $P_3$. $P_3$ cannot release the $\text{Camera}$ until it obtains the $\text{Printer}$, locked by $P_1$. All three processes are trapped in a non-terminating dependency loop.

---

Operations on Resources

A process must sequence its interaction with system resources through three standard system operations managed by the kernel:

1. **Request**: The process issues a system call to request the resource. If the resource cannot be granted immediately (e.g., it is allocated to another process), the requesting process must wait.
2. **Use**: The process operates on the resource (e.g., executing block I/O on a disk drive or streaming data to a serial port).
3. **Release**: The process relinquishes the resource via a system call, returning it to the unallocated resource pool managed by the OS scheduler.

The Process Wait State Transition

When a process requests an unavailable resource, the kernel transitions the process state from `Running` to `Blocked` (or `Waiting`).

```
                  Resource Request (Unavailable)
  +---------+ ------------------------------------> +---------+
  | Running |                                       | Blocked |
  +---------+                                       +---------+
       ^                                                 |
       |               Resource Released & Granted       |
       +------------------------------------------------ +---------+
                                                         |  Ready  |
                                                         +---------+
```

---

Real-World Analogy: 4-Way Traffic Gridlock

To visually conceptualize deadlock mechanics, consider a 4-way single-lane traffic intersection divided into four quadrants (`[Q1]`, `[Q2]`, `[Q3]`, `[Q4]`). Four vehicles approach simultaneously from four directions (North, South, East, West).

```
                     NORTH
                       | |
                       | |  Car North
                       | |    v
         --------------+ +--------------
         WEST    [Q1]  | |  [Q2]   EAST
         ---> Car West | |
         --------------+ +--------------
                       | | Car East <---
                 [Q4]  | |  [Q3]
         --------------+ +--------------
                       ^ |
                   Car South
                       | |
                       SOUTH
```

_Analogy Mapping_:

- **Resource Nodes**: Quadrants `[Q1]`, `[Q2]`, `[Q3]`, `[Q4]`.
- **Process Nodes**: Cars approaching from North, South, East, and West.
- **Current Allocation**: Car North occupies `[Q2]`; Car East occupies `[Q3]`; Car South occupies `[Q4]`; Car West occupies `[Q1]`.
- **Resource Request**: Car North requests `[Q3]`; Car East requests `[Q4]`; Car South requests `[Q1]`; Car West requests `[Q2]`.
- **Gridlock Condition**: No vehicle can move forward because its required quadrant is occupied by the vehicle to its right. Reverse movement is prohibited by traffic design rules (No Preemption). Traffic is permanently locked.

---

The 4 Necessary Conditions for Deadlock (Coffman Conditions)

Deadlock can occur **if and only if** all four of the following physical conditions are simultaneously satisfied within an operating system environment:

1. Mutual Exclusion

At least one resource must be held in a non-shareable mode. Only one process can access the resource at any given time. If another process requests that resource, the requesting process must be delayed until the resource is released.

```
[ Process P1 ] ---> ( Non-Shareable Resource R ) <--- [ Process P2 (Blocked) ]
```

2. Hold & Wait

A process must be currently holding at least one resource while simultaneously waiting to acquire additional resources that are currently being held by other processes.

```
[ Resource R1 ] ---> [ Process P1 ] ---Requests---> [ Resource R2 (Held by P2) ]
```

3. No Preemption

Resources cannot be forcibly revoked from a process holding them. A resource can only be released voluntarily by the process after it has completed its execution task.

```
[ Process P1 ] ---Holds---> [ Resource R ]  (OS Kernel cannot forcibly preempt R)
```

4. Circular Wait

A closed chain of processes $\{P_0, P_1, \dots, P_n\}$ exists such that $P_0$ is waiting for a resource held by $P_1$, $P_1$ is waiting for a resource held by $P_2$, and $P_n$ is waiting for a resource held by $P_0$.

```
+-----> ( P1 ) ---Holds R1, Waits R2---> ( P2 )
|                                           |
|                                     Holds R2, Waits R3
|                                           v
+------- Holds R3, Waits R1 ------------- ( P3 )
```

---

Deadlock vs. Livelock

While both conditions represent failure to make forward execution progress, their internal state dynamics are fundamentally distinct:

|Operational Feature|Deadlock|Livelock|
|---|---|---|
|**Process State**|Static in `Blocked` / `Waiting` state|Dynamic state changes (`Running` $\leftrightarrow$ `Ready`)|
|**CPU Utilization**|Zero CPU consumption by blocked processes|High CPU utilization (active CPU spinning/polling)|
|**Forward Progress**|Zero system progress|Zero system progress|
|**Behavioral Dynamics**|Processes are sleeping, waiting for non-occurring events|Processes actively change states to resolve contention, but repeatedly hit identical conflict conditions|

Livelock Pseudo-Code Example (Spinlock Lock Exchange Failure)

```
// Process P1 and Process P2 actively relinquish locks to avoid deadlock,
// but execute in lockstep, causing endless livelock loops.

void process_p1() {
    while (1) {
        acquire_lock(&lock_A);
        if (try_acquire_lock(&lock_B) == FAIL) {
            // Actively yield lock_A to allow P2 to proceed
            release_lock(&lock_A);
            delay_microseconds(10); // Active loop: P2 does the exact same, causing back-and-forth oscillation
            continue; 
        }
        // Critical Section
        release_lock(&lock_B);
        release_lock(&lock_A);
        break;
    }
}
```

Understanding these four static conditions allows computer scientists to construct structural graph models to trace process-resource interactions.

---

2. Module 2: Resource Allocation Graph (RAG) & Detection Rules

Resource Allocation Graph (RAG) Structure

A **Resource Allocation Graph (RAG)** is a directed graph $G = (V, E)$ used to visually depict and mathematically evaluate system state.

- **Set of Vertices (**$V$**)**: Partitioned into two node types:
    1. **Process Nodes (**$P = \{P_1, P_2, \dots, P_n\}$**)**: Rendered as **Circles**.
    2. **Resource Nodes (**$R = \{R_1, R_2, \dots, R_m\}$**)**: Rendered as **Rectangles**. Discrete dots inside the rectangle represent individual resource instances.
- **Set of Directed Edges (**$E$**)**:
    1. **Request Edge (**$P_i \rightarrow R_j$**)**: A directed edge pointing from process node $P_i$ to resource rectangle $R_j$. Indicates $P_i$ is currently blocked waiting for an instance of $R_j$.
    2. **Assignment (Allocation) Edge (**$R_j \rightarrow P_i$**)**: A directed edge pointing from a specific resource dot inside $R_j$ to process circle $P_i$. Indicates process $P_i$ currently holds that resource instance.

```
===================================================================================
                             RAG NOTATION VISUAL LEGEND
===================================================================================

  Process Node          :   ( P_i )
  
  Single-Instance Res   :   [ R_j |  •  ]
  
  Multi-Instance Res    :   [ R_j |  •  •  •  ]  (3 instances)
  
  Request Edge          :   ( P_i ) ----------------------------> [ R_j |  •  ]
  
  Assignment Edge       :   [ R_j | ( • ) ] --------------------> ( P_i )

===================================================================================
```

---

Core Graph Detection Rules

The presence of a directed cycle in a RAG yields definitive conclusions based on resource instance capacity:

1. **Single-Instance Resource Graphs**: If every resource node in the graph contains exactly **one instance**, then: $$\text{Cycle in RAG} \iff \text{Deadlock is GUARANTEED}$$ _Proof_: A cycle in a single-instance graph implies a closed circular wait chain with no alternate unallocated instances available to break the loop.
2. **Multi-Instance Resource Graphs**: If resource nodes contain **multiple instances**, then: $$\text{Cycle in RAG} \implies \text{Deadlock MAY exist (Necessary condition, but NOT sufficient)}$$ _Explanation_: A cycle indicates potential contention, but if a process outside the cycle holds an instance of a resource involved in the cycle, that process may terminate and return its instance, thereby breaking the cycle.

![[Pasted image 20261031103106.png|989]]

---

Visual Graph Diagrams & Reduction Mechanics

1. Single-Instance RAG with Deadlock Cycle

Every resource contains a single instance dot. The graph contains a closed cycle: $P_1 \rightarrow R_1 \rightarrow P_2 \rightarrow R_2 \rightarrow P_1$.

```
               +----------------------------------+
               |                                  |
               v                                  |
         [ R1 |  •  ]                            ( P2 )
               |                                  ^
               v                                  |
             ( P1 ) ---------------------> [ R2 |  •  ]
```

_Cycle Analysis_:

- $P_1$ holds $R_1$ and requests $R_2$.
- $P_2$ holds $R_2$ and requests $R_1$.
- Neither $P_1$ nor $P_2$ can make progress. **Deadlock is guaranteed.**

2. Multi-Instance RAG with Cycle (Deadlock Avoided)

Consider a multi-instance system structured as depicted in slide notes (`[SOURCE_IMAGE_9]`):

- $R_1$ has 3 instances: allocated to $P_1$ (2 instances) and $P_2$ (1 instance).
- $R_2$ has 2 instances: allocated to $P_2$ (1 instance) and $P_4$ (1 instance).
- $R_3$ has 3 instances: allocated to $P_3$ (1 instance) and $P_1$/$P_2$.
- $P_3$ issues a Request Edge to $R_2$. $P_2$ issues a Request Edge to $R_3$.

```
         [ R1 |  •  •  •  ]                     [ R2 |  •  •  ]
           /        |                             /        \
          v         v                            v          v
       ( P1 )     ( P2 ) <------------------- ( P3 )      ( P4 )
         |          |                            ^
         |          v                            |
         +-----> [ R3 |  •  •  •  ] -------------+
```

_Step-by-Step Graph Reduction Mechanics_:

1. **Identify Unblocked Processes**: Examine $P_4$. $P_4$ holds 1 instance of $R_2$, but its request set is empty ($\text{Request}_{P4} = \emptyset$). $P_4$ is completely unblocked.
2. **Execute & Release**: Process $P_4$ runs to completion and releases its instance of $R_2$.
3. **Graph Reduction**: Delete process node $P_4$ and erase assignment edge $R_2 \rightarrow P_4$. The unallocated instance count of $R_2$ increases to 1.
4. **Fulfill Pending Request**: The pending request edge $P_3 \rightarrow R_2$ can now be satisfied. Convert $P_3 \rightarrow R_2$ into an assignment edge $R_2 \rightarrow P_3$.
5. **Cascading Cycle Resolution**: $P_3$ is now unblocked. $P_3$ completes execution and releases all its allocated resources ($R_2$ and $R_3$).
6. **Final Resolution**: $P_2$ receives its requested instance of $R_3$, completes execution, and frees $R_1$. Finally, $P_1$ finishes.
7. **Conclusion**: Although a structural cycle initially existed ($P_2 \rightarrow R_3 \rightarrow P_3 \rightarrow R_2 \rightarrow P_2$), **the system was NOT deadlocked**.

Having established how graph dependencies visualize system states, we next examine strategies for managing deadlocks.

![[Pasted image 20261031103708.png|1018]]

---

3. Module 3: Deadlock Handling Strategies & Prevention

Overview of Deadlock Handling Strategies

Operating systems utilize four primary architectural strategies to address deadlocks:

|Strategy|Core Philosophy|System Overhead|Production Real-World Application|
|---|---|---|---|
|**Ignorance (Ostrich Algorithm)**|Ignore the problem entirely; assume deadlocks occur rare enough that system reboots are acceptable.|Zero runtime & design overhead|General-purpose OS (Windows, Linux, macOS, Android)|
|**Prevention**|Design system rules to statically eliminate at least 1 of the 4 Coffman conditions.|High design restrictions; low runtime utilization|Safety-critical real-time OS (aerospace, medical devices)|
|**Avoidance**|Dynamically monitor state safety before granting resource requests at runtime.|Moderate-to-High overhead; requires prior Max claim declarations|Specialized batch execution systems, DBMS|
|**Detection & Recovery**|Allow deadlocks to occur; run periodic cycle detection routines and recover via preemption/abortion.|High runtime CPU cost during detection & process rollback|High-availability database servers, mainframes|
![[Pasted image 20261031104013.png]]
---

Deep-Dive: Deadlock Prevention Rules

Deadlock Prevention operates by structuring system resource request protocols to guarantee that **at least one of the four Coffman necessary conditions can never hold**.

1. Invalidating Mutual Exclusion

- **Strategy**: Convert all system resources into shareable read-only access modes, or supply infinite hardware instances so no process ever waits.
- **Engineering Reality**: **Practically Impossible**. Dedicated physical hardware devices (such as printers, hardware encryption blocks, tape drives) are inherently non-shareable. Dual writes to a single printer stream corrupt physical output.

2. Invalidating Hold & Wait

- **Strategy (Hold-All-or-None Policy)**: Require a process to request and receive all required resources simultaneously prior to beginning execution. If even a single resource is unavailable, the kernel allocates zero resources, and the process waits.
- **Trade-offs**:
    1. **Severe Starvation**: A process needing multiple popular resources may wait indefinitely.
    2. **Low Resource Utilization**: A process requiring a printer for 1 second at the end of a 10-hour job must hold the printer for the entire 10 hours.

3. Invalidating No Preemption

- **Strategy (Resource Preemption Protocol)**: If a process holding resources requests an additional resource that cannot be immediately allocated, all resources currently held by that process are forcibly preempted (released) by the kernel. The process is rolled back and placed in the wait queue.
- **Trade-offs**:
    1. Applicable only to resources whose states can be easily saved and restored (e.g., CPU registers, memory pages).
    2. Cannot be applied to non-restorable stateful devices (printers, optical burners).
    3. Induces process **starvation** and can force processes into **unstable execution states**.

4. Invalidating Circular Wait

- **Strategy (Hierarchical Resource Ordering / Havender's Algorithm)**: Define a global linear ordering function $F: R \rightarrow \mathbb{N}$ mapping all resource types to unique integer indices ($R_1, R_2, \dots, R_m$). Enforce the strict rule that a process holding resource $R_i$ can only request resource $R_j$ if $i < j$.
- **Operational Case Study (****[SOURCE_IMAGE_15]****)**: Let system resources be ordered as $F(\text{Disk}) = 1$, $F(\text{Printer}) = 2$, $F(\text{Camera}) = 3$. If Process $P_3$ currently holds $R_3$ ($\text{Camera}$, index 3) and requires resource $R_1$ ($\text{Disk}$, index 1), it **cannot** directly request $R_1$ because $1 < 3$ violates the strict increasing index rule. To acquire $R_1$, $P_3$ **must release** $R_3$, acquire $R_1$, and then re-acquire $R_3$ in strictly increasing numerical order ($R_1 \rightarrow R_3$).

```
Hierarchical Request Rule Enforcement:
Resources: R1 (Index 1), R2 (Index 2), R3 (Index 3)

Valid Request Sequence   :  Acquire R1 -> Acquire R2 -> Acquire R3  (1 < 2 < 3)
Forbidden Request Event  :  Hold R3 -> Request R1  (1 < 3 Violation!)
Required Operational Fix :  Release R3 ===> Acquire R1 ===> Re-acquire R3
```

- **Trade-offs**: Restricts natural program execution logic, requires developers to manually structure allocation requests, and leads to process **starvation**.

While static prevention imposes rigid constraints on execution flow, dynamic avoidance offers operational flexibility by evaluating safety at runtime.

![[Pasted image 20261031104639.png]]

---

4. Module 4: Deadlock Avoidance & Banker's Algorithm

Deadlock Avoidance: State Definitions & Safety Concepts

Unlike prevention (which invalidates conditions statically), **Deadlock Avoidance** dynamically examines every resource request made to the OS kernel. A request is granted **if and only if** the resulting system state remains in a **Safe State**.

```
+-------------------------------------------------------+
|                    UNSAFE STATE                       |
|   +-----------------------------------------------+   |
|   |               DEADLOCK STATE                  |   |
|   |   (Processes trapped in permanent wait)       |   |
|   +-----------------------------------------------+   |
|   (Possibility of Deadlock exists in Unsafe State)    |
+-------------------------------------------------------+
|                     SAFE STATE                        |
|           (Guaranteed Zero Deadlock)                  |
+-------------------------------------------------------+
```

- **Safe State**: A state is safe if there exists at least one **Safe Sequence** $\langle P_1, P_2, \dots, P_n \rangle$ of process execution such that for each process $P_i$, the maximum resource claims of $P_i$ can be completely satisfied using current available resources plus resources currently held by all preceding processes $P_j$ ($j < i$).
- **Unsafe State**: A state where no safe sequence exists. An unsafe state is **NOT** a deadlock state; rather, an unsafe state **MAY** eventually lead to a deadlock if processes request their maximum declared resource limits simultaneously.
- **Deadlock State**: A strict subset of an unsafe state wherein processes are permanently trapped waiting for resources.

---

Formal Specifications of Banker's Algorithm (Dijkstra)

The Banker's Algorithm is a dynamic deadlock avoidance algorithm that tests system safety by simulating allocation against maximum declared process resource claims.

Algorithmic Data Structures

Let $n$ = number of processes, $m$ = number of resource types.

- $\text{Available}[m]$: Vector of length $m$. If $\text{Available}[j] = k$, there are $k$ instances of resource type $R_j$ currently unallocated.
- $\text{Max}[n \times m]$: Matrix defining maximum resource claims. If $\text{Max}[i, j] = k$, process $P_i$ may request at most $k$ instances of $R_j$.
- $\text{Allocation}[n \times m]$: Matrix defining current allocations. If $\text{Allocation}[i, j] = k$, process $P_i$ currently holds $k$ instances of $R_j$.
- $\text{Need}[n \times m]$: Matrix defining remaining resource requirements. $$\mathbf{\text{Need}[i, j] = \text{Max}[i, j] - \text{Allocation}[i, j]}$$

---

Formal Specification Box 1: The Safety Algorithm (`[SOURCE_IMAGE_32]`)

```
+-----------------------------------------------------------------------------------+
|                            BANKER'S SAFETY ALGORITHM                              |
+-----------------------------------------------------------------------------------+
| 1. Let Work be a vector of length m, and Finish be a boolean vector of length n.  |
|    Initialize:                                                                    |
|        Work = Available                                                           |
|        Finish[i] = false;  for i = 0, 1, 2, ..., n-1                              |
|                                                                                   |
| 2. Find an index i such that both:                                                |
|        (a) Finish[i] == false                                                     |
|        (b) Need_i <= Work                                                         |
|    If no such i exists, goto Step 4.                                              |
|                                                                                   |
| 3. Work = Work + Allocation_i                                                     |
|    Finish[i] = true                                                               |
|    goto Step 2.                                                                   |
|                                                                                   |
| 4. If Finish[i] == true for all i (0 <= i < n),                                   |
|    then the system is in a SAFE STATE.                                            |
|    Otherwise, the system is in an UNSAFE STATE.                                   |
+-----------------------------------------------------------------------------------+
```

---

Formal Specification Box 2: The Resource-Request Algorithm (`[SOURCE_IMAGE_38]`)

```
+-----------------------------------------------------------------------------------+
|                       RESOURCE-REQUEST ALGORITHM FOR PROCESS P_i                  |
+-----------------------------------------------------------------------------------+
| Let Request_i be the request vector for process P_i.                              |
|                                                                                   |
| 1. If Request_i <= Need_i, goto Step 2.                                           |
|    Otherwise, raise an Error condition (Process has exceeded maximum claim).      |
|                                                                                   |
| 2. If Request_i <= Available, goto Step 3.                                        |
|    Otherwise, process P_i MUST WAIT (Resources are not currently available).      |
|                                                                                   |
| 3. Have the system PRETEND to allocate requested resources to P_i by updating:    |
|        Available    = Available - Request_i                                       |
|        Allocation_i = Allocation_i + Request_i                                    |
|        Need_i       = Need_i - Request_i                                          |
|                                                                                   |
| 4. Execute the SAFETY ALGORITHM on the modified state vectors:                    |
|    - If Result == SAFE STATE   ===> Grant Request; Allocation Finalized.          |
|    - If Result == UNSAFE STATE ===> Reject Request; Roll back state modifications;|
|                                     Process P_i must wait.                        |
+-----------------------------------------------------------------------------------+
```
![[Pasted image 20261031105610.png]]
---

Step-by-Step Solved Lecture Numericals

Numerical 1: Single Resource Type Example (4 Processes $P_1-P_4$)

- **Given Constraints**: Total instances allocated across processes = 11. Initial Available = 1.
- **Derived Need Matrix Table**:

|Process|Allocation|Max|Need ($\mathbf{\text{Max} - \text{Allocation}}$)|
|---|---|---|---|
|**P1**|1|3|**2**|
|**P2**|5|8|**3**|
|**P3**|3|4|**1**|
|**P4**|2|7|**5**|

- **Step-by-Step Execution Trace**:
    1. **Initialization**: $\text{Work} = \text{Available} = 1$. $\text{Finish} = [\text{false}, \text{false}, \text{false}, \text{false}]$.
    2. **Evaluate** $P_3$: $\text{Need}_3 (1) \le \text{Work} (1) \implies \text{True}$.
        - $P_3$ executes, completes, and returns its allocation ($3$).
        - $\text{Work} = 1 + 3 = 4$. $\text{Finish}[3] = \text{true}$.
    3. **Evaluate** $P_1$: $\text{Need}_1 (2) \le \text{Work} (4) \implies \text{True}$.
        - $P_1$ executes, completes, and returns its allocation ($1$).
        - $\text{Work} = 4 + 1 = 5$. $\text{Finish}[1] = \text{true}$.
    4. **Evaluate** $P_2$: $\text{Need}_2 (3) \le \text{Work} (5) \implies \text{True}$.
        - $P_2$ executes, completes, and returns its allocation ($5$).
        - $\text{Work} = 5 + 5 = 10$. $\text{Finish}[2] = \text{true}$.
    5. **Evaluate** $P_4$: $\text{Need}_4 (5) \le \text{Work} (10) \implies \text{True}$.
        - $P_4$ executes, completes, and returns its allocation ($2$).
        - $\text{Work} = 10 + 2 = 12$. $\text{Finish}[4] = \text{true}$.
- **Conclusion**: All processes finish ($\text{Finish} = [\text{true}, \text{true}, \text{true}, \text{true}]$). System is in a **Safe State**.
- **List of All 4 Valid Safe Sequences (****[SOURCE_IMAGE_27]****)**:
    1. $\langle P_3, P_1, P_2, P_4 \rangle$
    2. $\langle P_3, P_2, P_1, P_4 \rangle$
    3. $\langle P_3, P_1, P_4, P_2 \rangle$
    4. $\langle P_3, P_2, P_4, P_1 \rangle$

---

Numerical 2: Multi-Resource Type Example (5 Processes $P_0-P_4$, Resources A, B, C)

- **Given**: Initial Available Vector $\text{Available} = (3, 3, 2)$.

Need Matrix Derivation Table:

|Process|Allocation (A B C)|Max (A B C)|Need Matrix ($\mathbf{\text{Need} = \text{Max} - \text{Allocation}}$)|
|---|---|---|---|
|**P0**|0 1 0|7 5 3|**7 4 3**|
|**P1**|2 0 0|3 2 2|**1 2 2**|
|**P2**|3 0 2|9 0 2|**6 0 0**|
|**P3**|2 1 1|2 2 2|**0 1 1**|
|**P4**|0 0 2|4 3 3|**4 3 1**|

Execution Safety Algorithm Trace:

- **Step 1**: Test $P_1$: $\text{Need}_1 (1, 2, 2) \le \text{Work} (3, 3, 2) \implies \text{True}$.
    - $P_1$ runs to completion. $\text{Work} = (3, 3, 2) + (2, 0, 0) = (5, 3, 2)$. $\text{Finish}[1] = \text{true}$.
- **Step 2**: Test $P_3$: $\text{Need}_3 (0, 1, 1) \le \text{Work} (5, 3, 2) \implies \text{True}$.
    - $P_3$ runs to completion. $\text{Work} = (5, 3, 2) + (2, 1, 1) = (7, 4, 3)$. $\text{Finish}[3] = \text{true}$.
- **Step 3**: Test $P_0$: $\text{Need}_0 (7, 4, 3) \le \text{Work} (7, 4, 3) \implies \text{True}$.
    - $P_0$ runs to completion. $\text{Work} = (7, 4, 3) + (0, 1, 0) = (7, 5, 3)$. $\text{Finish}[0] = \text{true}$.
- **Step 4**: Test $P_2$: $\text{Need}_2 (6, 0, 0) \le \text{Work} (7, 5, 3) \implies \text{True}$.
    - $P_2$ runs to completion. $\text{Work} = (7, 5, 3) + (3, 0, 2) = (10, 5, 5)$. $\text{Finish}[2] = \text{true}$.
- **Step 5**: Test $P_4$: $\text{Need}_4 (4, 3, 1) \le \text{Work} (10, 5, 5) \implies \text{True}$.
    - $P_4$ runs to completion. $\text{Work} = (10, 5, 5) + (0, 0, 2) = (10, 5, 7)$. $\text{Finish}[4] = \text{true}$.
- **Conclusion**: System is in a **Safe State**.
- **Established Safe Sequence**: $\langle P_1, P_3, P_0, P_2, P_4 \rangle$.
- **Final Available Vector**: $(10, 5, 7)$.

---

Numerical 3: Resource-Request Algorithm Solved Cases

Case A: Process $P_0$ requests $\text{Request}_{P0} = \langle 1, 0, 2 \rangle$

1. **Check 1**: $\text{Request}_{P0} \langle 1, 0, 2 \rangle \le \text{Need}_{P0} \langle 7, 4, 3 \rangle \implies \text{True}$.
2. **Check 2**: $\text{Request}_{P0} \langle 1, 0, 2 \rangle \le \text{Available} \langle 3, 3, 2 \rangle \implies \text{True}$.
3. **Pretend Allocation State Modification**:
    - $\text{Available} = \langle 3, 3, 2 \rangle - \langle 1, 0, 2 \rangle = \langle 2, 3, 0 \rangle$
    - $\text{Allocation}_{P0} = \langle 0, 1, 0 \rangle + \langle 1, 0, 2 \rangle = \langle 1, 1, 2 \rangle$
    - $\text{Need}_{P0} = \langle 7, 4, 3 \rangle - \langle 1, 0, 2 \rangle = \langle 6, 4, 1 \rangle$
4. **Run Safety Algorithm on Pretend State**:
    - Initial $\text{Work} = \langle 2, 3, 0 \rangle$.
    - Evaluate $\text{Need}_1 \langle 1, 2, 2 \rangle \le \langle 2, 3, 0 \rangle \implies \text{False}$ (fails on resource C).
    - Evaluate $\text{Need}_3 \langle 0, 1, 1 \rangle \le \langle 2, 3, 0 \rangle \implies \text{False}$ (fails on resource C).
    - Evaluate $\text{Need}_0, \text{Need}_2, \text{Need}_4 \implies \text{All False}$.
5. **Safety Result**: No process can execute. The system enters an **Unsafe State**.
6. **Final Decision**: Request is **REJECTED**. $P_0$ must wait, and state vectors are restored.

Case B: Process $P_3$ requests $\text{Request}_{P3} = \langle 0, 1, 0 \rangle$

1. **Check 1**: $\text{Request}_{P3} \langle 0, 1, 0 \rangle \le \text{Need}_{P3} \langle 0, 1, 1 \rangle \implies \text{True}$.
2. **Check 2**: $\text{Request}_{P3} \langle 0, 1, 0 \rangle \le \text{Available} \langle 3, 3, 2 \rangle \implies \text{True}$.
3. **Pretend Allocation State Modification**:
    - $\text{Available} = \langle 3, 3, 2 \rangle - \langle 0, 1, 0 \rangle = \langle 3, 2, 2 \rangle$
    - $\text{Allocation}_{P3} = \langle 2, 1, 1 \rangle + \langle 0, 1, 0 \rangle = \langle 2, 2, 1 \rangle$
    - $\text{Need}_{P3} = \langle 0, 1, 1 \rangle - \langle 0, 1, 0 \rangle = \langle 0, 0, 1 \rangle$
4. **Run Safety Algorithm on Pretend State**:
    - Test $P_1$: $\text{Need}_1 \langle 1, 2, 2 \rangle \le \text{Work} \langle 3, 2, 2 \rangle \implies \text{True}$.
    - Execution completes safely establishing safe sequence $\langle P_1, P_3, P_0, P_2, P_4 \rangle$.
5. **Final Decision**: System remains in a **Safe State**. Request is **GRANTED**.

---

5. Module 5: Deadlock Detection & Recovery

Core Theoretical Insight: Detection vs. Avoidance

As highlighted in lecture note `[SOURCE_IMAGE_51]`:

```
+-----------------------------------------------------------------------------------+
|                           CORE THEORETICAL TAKEAWAY                               |
|                                                                                   |
|         "Deadlock Detection is more powerful than Deadlock Avoidance"             |
|                                     ===>                                          |
|                  "Deadlock Avoidance is NOT practical"                            |
+-----------------------------------------------------------------------------------+
```

_Architectural Rationale_: Banker's algorithm avoidance requires processes to declare their maximum resource claim ($\text{Max}$ matrix) in advance. In general-purpose computing, processes cannot accurately predict their dynamic future resource needs. Consequently, production operating systems lean toward runtime **Deadlock Detection & Recovery** or **Ignorance** rather than dynamic avoidance.

---

Single-Instance Detection: Wait-For Graph (WFG) Mechanics

For systems where all resource types possess a **single instance**, deadlock detection is accomplished by reducing the Resource Allocation Graph into a **Wait-For Graph (WFG)**.

Transformation Algorithm (RAG $\rightarrow$ WFG):

1. Eliminate all Resource Nodes ($R_j$) from the RAG.
2. Construct a direct process-to-process edge $P_i \rightarrow P_j$ **if and only if** process $P_i$ is currently waiting for a resource instance that is held by process $P_j$.

```
Wait-For Edge Derivation:
   ( P_i ) ------------> [ R_k ] ------------> ( P_j )   ===>   ( P_i ) ------------> ( P_j )
        (Request Edge)         (Assignment Edge)                   (Direct Wait-For Edge)
```

$$\text{Cycle in Wait-For Graph} \iff \text{Deadlock Exists}$$

Structural 5-Process Graph Transformation (`[SOURCE_IMAGE_55]`)

Consider a system with 5 processes ($P_1, P_2, P_3, P_4, P_5$) and 5 single-instance resources ($R_1, R_2, R_3, R_4, R_5$).

```
   RESOURCE ALLOCATION GRAPH (RAG)                    WAIT-FOR GRAPH (WFG)

              ( P5 )                                         ( P5 )
                ^                                              ^
                |                                              |
             [ R3 ]                                            |
                ^                                              |
                |                                              |
   [ R1 ] <-- ( P2 ) --> [ R4 ]                   ( P1 ) ----> ( P2 ) ----> ( P3 )
     ^          |          |                        ^            |            |
     |          v          v                        |            v            v
   ( P1 )     [ R5 ] <-- ( P3 )                     +--------- ( P4 ) <-------+
     ^          ^
     |          |
   [ R2 ] <-- ( P4 )
```

_Detailed Process Dependency & Deadlock Analysis_:

- $P_1$ requests $R_1$ (held by $P_2$) $\implies$ Edge $P_1 \rightarrow P_2$.
- $P_2$ requests $R_3$ (held by $P_5$), $R_4$ (held by $P_3$), and $R_5$ (held by $P_4$) $\implies$ Edges $P_2 \rightarrow P_5$, $P_2 \rightarrow P_3$, $P_2 \rightarrow P_4$.
- $P_3$ requests $R_5$ (held by $P_4$) $\implies$ Edge $P_3 \rightarrow P_4$.
- $P_4$ requests $R_2$ (held by $P_1$) $\implies$ Edge $P_4 \rightarrow P_1$.
- **Cycle Analysis**:
    - Cycles present: $\langle P_1 \rightarrow P_2 \rightarrow P_4 \rightarrow P_1 \rangle$ and $\langle P_1 \rightarrow P_2 \rightarrow P_3 \rightarrow P_4 \rightarrow P_1 \rangle$.
    - **Deadlocked Process Set**: Processes $\{P_1, P_2, P_3, P_4\}$ are deadlocked in a circular chain.
    - **Exempt Process**: Process $P_5$ holds $R_3$ but makes zero requests ($\text{Out-degree} = 0$). $P_5$ runs to completion, releases $R_3$, and is **NOT deadlocked**.

![[Pasted image 20261031110522.png]]

---

Multi-Instance Resource Detection Algorithm

When system resources contain multiple instances, simple cycle detection is insufficient. The OS maintains state matrices ($\text{Available}$, $\text{Allocation}$, $\text{Request}$) and executes a matrix reduction algorithm.

Algorithm Steps & Data Structures

1. **Work & Finish Vectors**: Let $\text{Work}[m] = \text{Available}$. Initialize $\text{Finish}[i]$ for $i = 0, 1, \dots, n-1$:
    - If $\text{Allocation}_i \neq 0$, then $\text{Finish}[i] = \text{false}$.
    - Else, $\text{Finish}[i] = \text{true}$.
2. **Find Process**: Find an index $i$ such that:
    - $\text{Finish}[i] == \text{false}$
    - $\text{Request}_i \le \text{Work}$ If no such $i$ exists, go to Step 4.
3. **Update Work**:
    - $\text{Work} = \text{Work} + \text{Allocation}_i$
    - $\text{Finish}[i] = \text{true}$
    - Goto Step 2.
4. **Evaluate System Deadlock**: If $\text{Finish}[i] == \text{false}$ for any $i$ ($0 \le i < n$), then the system is in a **Deadlock State**, and process $P_i$ is deadlocked.

Step-by-Step Multi-Instance Detection Numerical Example

- **Given State**: 5 Processes ($P_0-P_4$), 3 Resources ($A, B, C$).
- $\text{Available} = (0, 0, 0)$.

|Process|Allocation (A B C)|Request Matrix (A B C)|Initial $\text{Finish}[i]$|
|---|---|---|---|
|**P0**|0 1 0|0 0 0|**false**|
|**P1**|2 0 0|2 0 2|**false**|
|**P2**|3 0 3|0 0 0|**false**|
|**P3**|2 1 1|1 0 0|**false**|
|**P4**|0 0 2|0 0 2|**false**|

- **Step-by-Step Matrix Trace**:
    1. **Initialization**: $\text{Work} = (0, 0, 0)$. $\text{Finish} = [\text{false}, \text{false}, \text{false}, \text{false}, \text{false}]$.
    2. **Evaluate** $P_0$: $\text{Request}_0 (0, 0, 0) \le \text{Work} (0, 0, 0) \implies \text{True}$.
        - $P_0$ executes to completion. $\text{Work} = (0, 0, 0) + (0, 1, 0) = (0, 1, 0)$. $\text{Finish}[0] = \text{true}$.
    3. **Evaluate** $P_2$: $\text{Request}_2 (0, 0, 0) \le \text{Work} (0, 1, 0) \implies \text{True}$.
        - $P_2$ executes to completion. $\text{Work} = (0, 1, 0) + (3, 0, 3) = (3, 1, 3)$. $\text{Finish}[2] = \text{true}$.
    4. **Evaluate** $P_1$: $\text{Request}_1 (2, 0, 2) \le \text{Work} (3, 1, 3) \implies \text{True}$.
        - $P_1$ executes to completion. $\text{Work} = (3, 1, 3) + (2, 0, 0) = (5, 1, 3)$. $\text{Finish}[1] = \text{true}$.
    5. **Evaluate** $P_3$: $\text{Request}_3 (1, 0, 0) \le \text{Work} (5, 1, 3) \implies \text{True}$.
        - $P_3$ executes to completion. $\text{Work} = (5, 1, 3) + (2, 1, 1) = (7, 2, 4)$. $\text{Finish}[3] = \text{true}$.
    6. **Evaluate** $P_4$: $\text{Request}_4 (0, 0, 2) \le \text{Work} (7, 2, 4) \implies \text{True}$.
        - $P_4$ executes to completion. $\text{Work} = (7, 2, 4) + (0, 0, 2) = (7, 2, 6)$. $\text{Finish}[4] = \text{true}$.
- **Detection Result**: All $\text{Finish}[i] == \text{true}$. System is **NOT in a Deadlock State**.

---

Detection Algorithm Invocation Frequency

Executing the detection algorithm incurs dynamic CPU overhead. Operating systems adopt two invocation strategies:

1. **Frequent Invocation (On Every Blocked Request)**:
    - _Advantage_: Pinpoints the exact process that triggered the deadlock.
    - _Disadvantage_: High CPU performance penalty ($O(n \cdot m^2)$ execution cost per request).
2. **Periodic Invocation**:
    - _Advantage_: Low average runtime overhead.
    - _Trigger Criteria_: Executed at fixed timer ticks (e.g., every 5 minutes) or when system CPU utilization drops below a specified threshold (e.g., CPU utilization $< 20\%$, indicating processes are blocked waiting for resources).

---

Deadlock Recovery Approaches

Once detection algorithms confirm a deadlock state, the OS resolves the cycle using three main strategies:

```
                           +------------------------+
                           |   DEADLOCK RECOVERY    |
                           +------------------------+
                                       |
        +------------------------------+------------------------------+
        |                              |                              |
        v                              v                              v
+------------------+        +---------------------+        +---------------------+
|      MANUAL      |        | PROCESS TERMINATION |        | RESOURCE PREEMPTION |
|   INTERVENTION   |        +---------------------+        +---------------------+
| (Operator Alerts)|        | 1. Abort All        |        | 1. Victim Selection |
+------------------+        | 2. Abort 1-by-1     |        | 2. Rollback         |
                            +---------------------+        | 3. Prevent Starvation|
                                                           +---------------------+
```

1. **Manual Intervention**: Notify the system administrator/operator to manually inspect and kill deadlocked tasks.
2. **Process Termination**:
    - _Abort All Deadlocked Processes_: Instantly breaks deadlock cycles, but discards computation done by long-running processes.
    - _Abort Processes One-by-One_: Terminate one deadlocked process at a time and re-run the detection algorithm until all deadlock cycles are eliminated.
3. **Resource Preemption**:
    - **Victim Selection**: Select the process with the minimum preemption cost (analyzing process priority, elapsed CPU time, and allocated resource count).
    - **Rollback**: Roll back the preempted victim process to a safe, previously saved **checkpoint** state, then restart execution.
    - **Prevent Starvation**: Increment a preemption counter for victim processes to ensure a single process is not repeatedly targeted.

Understanding these runtime mechanisms enables us to formulate exact mathematical bounds for deadlock-free operation.

---

6. Module 6: Minimum Resources & GATE Formulas (Mathematical Masterclass)

Core GATE Exam Mathematical Formulas

Let $n$ = total number of processes competing for a resource, $\text{Max}_i$ = maximum demand of process $i$, and $R$ = total available resource instances.

1. General Deadlock-Free Condition ($\text{Deadlock NEVER Occurs}$)

$$\sum_{i=1}^{n} (\text{Max}_i - 1) + 1 \le R$$

_Rigorous Mathematical Derivation_: To force a system into the worst-case non-deadlocked state of maximal contention, allocate each process $P_i$ exactly one instance less than its maximum requirement ($\text{Max}_i - 1$). The system holds maximum resources while no process can complete: $$\text{Max Allocated Resources without Deadlock} = \sum_{i=1}^{n} (\text{Max}_i - 1)$$ Adding exactly **1 additional resource instance** guarantees that at least one process receives its maximum requirement, completes, releases all its allocated resources, and prevents deadlock.

2. Equal Requirements Condition ($n$ processes, each needing max $k$ instances)

$$n \cdot (k - 1) + 1 \le R$$

3. Condition Where Deadlock MAY Occur

$$R \le \sum_{i=1}^{n} (\text{Max}_i - 1)$$

---

Solved Mathematical Problems & GATE PYQs

Problem 1 (Identical Requirements)

A system contains 3 processes ($A, B, C$), each requiring 4 instances of resource $R$.

1. Calculate minimum total resources $R_{\min}$ to guarantee **no deadlock**.
2. Calculate maximum resources $R_{\text{may}}$ where deadlock **MAY occur**.

**Solution**:

- Given: $n = 3$, $k = 4$.
- _Part 1 (No Deadlock Guarantee)_: $$R_{\min} = n \cdot (k - 1) + 1 = 3 \cdot (4 - 1) + 1 = 3(3) + 1 = \mathbf{10}$$
- _Part 2 (Deadlock MAY Occur)_: $$R_{\text{may}} = n \cdot (k - 1) = 3 \cdot (4 - 1) = 3(3) = \mathbf{9}$$

---

Problem 2 (Identical Requirements)

A system has 4 processes, each requiring 6 instances of a resource type. Find the maximum number of resources such that deadlock MAY occur.

**Solution**:

- Given: $n = 4$, $k = 6$.
- Apply formula: $$R_{\text{may}} = n \cdot (k - 1) = 4 \cdot (6 - 1) = 4(5) = \mathbf{20}$$

---

Problem 3 (GATE PYQ)

A system has 3 processes sharing $R$ instances of a resource. Each process requires a maximum of $K$ instances. If total available resources $R = 4$, calculate the maximum value of $K$ such that deadlock NEVER occurs.

**Solution**:

- Given: $n = 3$, $R = 4$.
- Apply deadlock-free formula: $$n \cdot (K - 1) + 1 \le R$$ $$3 \cdot (K - 1) + 1 \le 4$$ $$3 \cdot (K - 1) \le 3 \implies K - 1 \le 1 \implies K \le 2$$
- Maximum integer value $\mathbf{K_{\max} = 2}$.

---

Problem 4 (GATE-2026 NAT Practice)

A system contains 5 processes, each with a maximum requirement of 2 instances of resource type $R$. Find the minimum number of instances $k$ required to guarantee deadlock-free operation.

**Solution**:

- Given: $n = 5$, $\text{Max}_i = 2$.
- Apply formula: $$k_{\min} = n \cdot (\text{Max}_i - 1) + 1 = 5 \cdot (2 - 1) + 1 = 5(1) + 1 = \mathbf{6}$$

---

Problem 5 (Non-Identical Requirements)

Processes $A, B, C, D$ have maximum requirements $\text{Max} = (6, 5, 2, 3)$ and current allocations $\text{Allocation} = (5, 4, 1, 2)$. Calculate the minimum total resources needed to guarantee no deadlock occurs.

**Solution**:

- Given claims: $\text{Max}_A = 6, \text{Max}_B = 5, \text{Max}_C = 2, \text{Max}_D = 3$.
- Apply general formula: $$R_{\min} = \sum_{i=1}^{n} (\text{Max}_i - 1) + 1$$ $$R_{\min} = (6 - 1) + (5 - 1) + (2 - 1) + (3 - 1) + 1 = 5 + 4 + 1 + 2 + 1 = \mathbf{13}$$

---

Problem 6 (GATE MSQ Statement Verification - `[SOURCE_IMAGE_50]`)

**Question**: With respect to deadlocks in an operating system, which of the following statements is/are **FALSE**?

- **(a)** Banker's algorithm is used to prevent deadlocks.
- **(b)** Deadlock formation can be prevented by ensuring that the hold and wait condition is not allowed.
- **(c)** An assignment edge in a resource allocation graph is marked from a process to a resource.
- **(d)** A safe state guarantees that all processes can finish without formation of a deadlock.

**Detailed Verification Analysis**:

- _Statement (a)_: **FALSE**. Banker's algorithm is a dynamic Deadlock **Avoidance** algorithm, not a static prevention mechanism.
- _Statement (b)_: **TRUE**. Invalidating Hold & Wait (e.g., via hold-all-or-none policy) is a valid prevention method.
- _Statement (c)_: **FALSE**. An assignment edge is directed from a **resource node to a process node** ($R_j \rightarrow P_i$). An edge directed from a process node to a resource node is a **request edge** ($P_i \rightarrow R_j$).
- _Statement (d)_: **TRUE**. By definition, a safe state implies the existence of at least one safe sequence ensuring complete execution without deadlock.

**Answer**: Statements **(a) and (c)** are **FALSE**.

---

Mastering these core principles, graph models, and mathematical bounds provides the analytical framework needed to solve any operating system deadlock problem with precision.