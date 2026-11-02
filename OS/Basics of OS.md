# GATE CS Revision Report: Basics of Operating Systems (Topics 1-5)

## 1. OS Foundations: Objectives and Service Architecture

The Operating System (OS) is primarily architected as a **Resource Allocator**, managing the contention for hardware (CPU, RAM, I/O) among competing processes, and a **Control Program**, providing a set of utilities and APIs to simplify execution. For GATE, remember that OS design is a study in conflicting goals: the **Primary Goal** is User Convenience (Ease of Use), while the **Secondary Goal** is System Efficiency (Resource Utilization). In a throughput-oriented environment, we sacrifice convenience for performance; in a handheld device, the reverse is often true.

### Goal Hierarchy Analysis: The Trade-offs

- **Convenience (Primary):** Prioritizes the user interface and abstraction of hardware. Often leads to "bloat" and lower throughput.
- **Efficiency (Secondary):** Prioritizes "keeping the CPU busy." Measured via throughput and utilization. High efficiency often requires complex scheduling that adds overhead to the user experience.

### Service Classification: Functional Triggers

- **User Interface:** Triggered when a user provides input via GUI, Batch, or CLI.
- **Program Execution:** Triggered when a process is loaded into RAM and assigned CPU time.
- **I/O Operations:** Triggered when a running process requires access to a specific hardware peripheral.
- **File System Manipulation:** Triggered for the creation, deletion, or searching of data on non-volatile storage.
- **Communication:** Triggered for inter-process communication (IPC) via shared memory or message passing.
- **Error Detection:** Triggered by hardware faults or software exceptions to maintain system integrity.
- **Resource Allocation:** Triggered when multiple jobs compete for limited CPU cycles or memory.
- **Accounting:** Triggered for tracking resource usage for billing or system optimization.
- **Protection & Security:** Triggered to control access to system resources and protect user data.

These services form the functional interface that allows programs to transition from passive files on a disk to active execution models.

## 2. Evolutionary Taxonomy of Operating Systems

The history of Operating Systems is defined by the struggle against the **CPU-I/O bottleneck**. Because mechanical I/O is orders of magnitude slower than electronic CPU speeds, architects evolved systems to minimize CPU idleness.

### Comparative Analysis of Execution Models

- **Batch OS:** The first attempt to solve idleness via **Job Grouping**. Similar jobs were batched together to reduce setup time. However, the CPU still remained idle whenever a job performed I/O, resulting in poor utilization.
- **Multiprogramming:** The breakthrough "Overlap" model. By keeping multiple programs in RAM simultaneously, the OS switches the CPU to Program B the moment Program A initiates I/O. **Utilization is maximized** by ensuring the CPU rarely waits for slow peripherals.
- **Multitasking (Time-Sharing):** A logical extension of multiprogramming. Instead of waiting for I/O to switch, the OS uses **Time-Slicing** (Round Robin) to switch processes frequently. This prioritizes **Response Time** over simple throughput, creating the illusion of dedicated hardware for each user.

### Specialized Architectures

|   |   |   |
|---|---|---|
|Architecture|Coupling Model|Constraint / Goal|
|**Real-Time OS (RTOS)**|Highly Deterministic|**Hard Constraints:** Missing a deadline is total system failure. **Soft Constraints:** Performance degrades but system survives.|
|**Multiprocessing**|Tightly Coupled|Multiple CPUs sharing a single RAM/Bus. Goal: Increase throughput and reliability via parallel execution.|
|**Distributed Systems**|Loosely Coupled|Multiple autonomous processors communicating via a network. Goal: Resource sharing and load balancing across nodes.|

The management of these concurrent execution modes requires strict hardware enforcement to prevent user-level code from crashing the kernel.

## 3. Dual Mode Operations, System Calls, and Fork() Mechanics

To protect system integrity, modern hardware implements **Dual Mode Operation** using a physical **Mode Bit** (typically a hardware flip-flop). This bit determines which instructions are legal to execute at any given time.

### Mode Transition Matrix

|   |   |   |
|---|---|---|
|Feature|User Mode|Kernel Mode|
|**Mode Bit**|1|0|
|**Privilege Level**|Non-privileged; restricted.|Privileged; full hardware access.|
|**Instruction Access**|Standard logic, math, read-only.|I/O instructions, Halt, Context Switching.|
|**Transition Event**|System Call or Interrupt (Trap).|"Return to User" instruction.|

### The Trap Mechanism

A **Trap** is a software-generated interrupt. When a user process needs a privileged service (e.g., writing to a file), it triggers a Trap, which flips the mode bit from 1 to 0 and jumps to a specific location in the kernel. Once the kernel finishes, it flips the bit back to 1 and returns control.

**Critical "Common Trap" Alert:** Do not assume every memory request is a system call. `malloc()` is a **Library Function**. It only triggers a system call (like `sbrk`) when the heap is exhausted and the process needs more memory from the OS.

### Fork() Mathematical Foundation

Process creation via `fork()` is a frequent source of "traps" in GATE.

- **Total processes** created by n consecutive `fork()` calls = 2^n.
- **Total child processes** = 2^n - 1.

**Advanced Recursive Analysis (GATE-2024 Pattern):** Consider a recursive function f(n) that calls `fork()` and then calls itself twice. The recurrence relation for the number of times a "print" statement is executed is: T(n) = 2 + 2T(n-1) where T(0) = 0 (Base case: no calls, no prints).

**Closed-form Shortcut Derivation:** By expansion, we find: T(1) = 2 + 2(0) = 2 T(2) = 2 + 2(2) = 6 T(3) = 2 + 2(6) = 14 The closed-form formula is: T(n) = 2^{(n+1)} - 2 **Calculation for** **x=3****:** 2^{(3+1)} - 2 = 16 - 2 = \mathbf{14} **prints.**

## 4. Process Anatomy: Representation and Context

A "Program" is a passive entity (executable file on disk). A "Process" is an active entity in RAM.

### Memory Image Decomposition

1. **Stack:** Stores temporary data: local variables, function parameters, and return addresses. Grows downward.
2. **Heap:** Used for dynamic memory allocation (`malloc`/`new`). Grows upward toward the stack.
3. **Data:** Stores global and static variables.
4. **Code/Text:** The immutable sequence of instructions.

### The Process Control Block (PCB)

The PCB is a **Process Descriptor** stored in kernel memory, typically as a **doubly linked list** to allow O(1) insertion and deletion from scheduling queues.

- **Attributes:** PID (ID), PC (Next instruction), GPRs (Register state), Priority, State, List of Open Files.

### The Context Switch Logic

Handled by the **Dispatcher**, a context switch is the overhead-intensive procedure of swapping processes on the CPU:

1. **Save State:** Current register values and PC are saved into the old process’s PCB.
2. **Load State:** Values from the new process’s PCB are loaded into the CPU registers. _Note: Context switching time is pure overhead; the system does no "useful" work during this period._

## 5. Process Dynamics: State Transitions and Scheduling Theory

The OS manages the **Degree of Multiprogramming (DOM)**—the number of processes in RAM—by moving processes through the 5-State Model.

### State Transition Model Logic

- **Voluntary Transitions:** Running \rightarrow Terminated (Completion) or Running \rightarrow Blocked (Waiting for I/O). These are initiated by the process.
- **Involuntary Transitions:** Running \rightarrow Ready (Preemption). This is initiated by the OS (Scheduler/Timer).

### The Three-Tier Scheduler Comparison

|   |   |   |   |
|---|---|---|---|
|Scheduler|Role|Transition|Impact on DOM|
|**Long-Term (LTS)**|Job Selection|New \rightarrow Ready|**Increases** DOM.|
|**Short-Term (STS)**|CPU Selection|Ready \rightarrow Run|No change to DOM (Highest frequency).|
|**Medium-Term (MTS)**|Swapping|RAM \leftrightarrow Disk|**Increases or Decreases** DOM via swap-in/out.|

### Mathematical Bounds of Process States

Given n processes and m CPUs (n > m):

- **Ready State:** Max = n, Min = 0.
    - _Proof:_ Max is n if all processes are admitted but the CPU is busy with an interrupt or all CPUs are idle.
- **Running State:** Max = m, Min = 0.
    - _Proof:_ Max is m because you cannot execute more processes than physical hardware cores. Min is 0 if all processes are blocked on I/O.
- **Blocked State:** Max = n, Min = 0.
    - _Proof:_ All processes can theoretically be waiting for I/O simultaneously.

### Final Revision Summary: High-Frequency GATE Facts

- **PCB Location:** Stored in Kernel RAM. A user process **cannot** access its own PCB; this would violate protection.
- **DOM Control:** While LTS initially sets the DOM, the **Medium-Term Scheduler** is the architect's tool for reducing DOM to alleviate memory pressure (thrashing).
- **Dispatcher Nature:** It is the specific module that gives control of the CPU to the process selected by the Short-Term Scheduler.
- **Process Termination:** Upon exit, the OS deallocates the memory image **and** deletes the PCB from the kernel list.