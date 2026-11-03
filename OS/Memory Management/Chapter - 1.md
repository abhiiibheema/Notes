MODULE 1: CONTIGUOUS MEMORY ALLOCATION & PARTITIONING

Definition & Types

Memory Management is a fundamental module of an Operating System responsible for managing the primary memory (Main Memory).

- **Core Functions of Memory Management**:
    1. **Memory Allocation**: Allocates memory space to new arriving processes.
    2. **Memory Deallocation**: Reclaims memory space when processes terminate.
    3. **Memory Protection**: Ensures that an executing process can only access its own allocated memory space.
- **Goals of Memory Management**:
    1. Maximum utilization of main memory space.
    2. Ability to run larger programs in limited physical space.
- **Taxonomy of Memory Allocation Schemes**:
    - **Contiguous Memory Management**: The entire process must be stored in consecutive (contiguous) memory locations.
        - _Fixed Partition Contiguous Allocation_
        - _Variable Partition Contiguous Allocation_
    - **Non-Contiguous Memory Management**: Parts of a process can be stored in non-consecutive memory locations.
        - _Paging_
        - _Segmentation_

---

Fixed Partition Contiguous Allocation

- **Mechanics**: Main memory is divided into a fixed number of partitions at the time of system initialization. Each partition can hold at most **one process** at a time. The sizes of individual partitions can be equal or unequal.
- **Constraint**: The **Degree of Multiprogramming** (maximum number of processes in main memory) is strictly bounded by the total number of partitions.
- **Limitation**: Suffers from **Internal Fragmentation**. Internal fragmentation occurs when allocated partition space exceeds the memory required by a process, leaving unused space inside the partition that cannot be utilized by other processes.

Partition Allocation Policies

1. **First Fit**: Scans partitions from the start of memory and allocates the _first available partition_ that is large enough to hold the process.
2. **Best Fit**: Scans all available partitions and allocates the _smallest partition_ that is large enough to hold the process.
3. **Worst Fit**: Scans all available partitions and allocates the _largest partition_.
4. **Next Fit**: Similar to First Fit, but starts searching for an available partition from the location of the _last allocated partition_.

---

Solved Table Example: Fixed Partitioning Allocation

Consider 4 partitions of sizes: **100KB, 120KB, 150KB, and 80KB**. Processes arrive in order: **P1 (110KB)** and **P2 (70KB)**.

Step-by-Step Allocation Tracing:

- **First Fit**:
    - $P_1$ (110KB): Starts from top. 100KB is too small. 120KB partition fits. $P_1 \to 120\text{KB}$. Internal Frag = $120 - 110 = 10\text{KB}$.
    - $P_2$ (70KB): Starts from top. 100KB partition fits. $P_2 \to 100\text{KB}$. Internal Frag = $100 - 70 = 30\text{KB}$.
    - Total Internal Fragmentation = $10\text{KB} + 30\text{KB} = 40\text{KB}$.
- **Best Fit**:
    - $P_1$ (110KB): Available suitable partitions are 120KB and 150KB. Smallest suitable is 120KB. $P_1 \to 120\text{KB}$. Internal Frag = $10\text{KB}$.
    - $P_2$ (70KB): Available suitable partitions are 100KB, 150KB, 80KB. Smallest suitable is 80KB. $P_2 \to 80\text{KB}$. Internal Frag = $80 - 70 = 10\text{KB}$.
    - Total Internal Fragmentation = $10\text{KB} + 10\text{KB} = 20\text{KB}$.
- **Worst Fit**:
    - $P_1$ (110KB): Largest available partition is 150KB. $P_1 \to 150\text{KB}$. Internal Frag = $150 - 110 = 40\text{KB}$.
    - $P_2$ (70KB): Remaining available partitions are 100KB, 120KB, 80KB. Largest suitable is 120KB. $P_2 \to 120\text{KB}$. Internal Frag = $120 - 70 = 50\text{KB}$.
    - Total Internal Fragmentation = $40\text{KB} + 50\text{KB} = 90\text{KB}$.
- **Next Fit**:
    - $P_1$ (110KB): Starts from top, allocated to 120KB partition. Internal Frag = $10\text{KB}$. Last position pointer is set after 120KB block.
    - $P_2$ (70KB): Search starts after 120KB partition. Next available suitable block is 150KB. $P_2 \to 150\text{KB}$. Internal Frag = $150 - 70 = 80\text{KB}$.
    - Total Internal Fragmentation = $10\text{KB} + 80\text{KB} = 90\text{KB}$.

|Allocation Policy|$P_1$ Allocation (110KB)|$P_2$ Allocation (70KB)|$P_1$ Internal Frag|$P_2$ Internal Frag|Total Internal Frag|
|---|---|---|---|---|---|
|**First Fit**|120KB|100KB|10KB|30KB|**40KB**|
|**Best Fit**|120KB|80KB|10KB|10KB|**20KB**|
|**Worst Fit**|150KB|120KB|40KB|50KB|**90KB**|
|**Next Fit**|120KB|150KB|10KB|80KB|**90KB**|

---

Variable Partition Contiguous Allocation

- **Mechanics**: Partitions are created dynamically when a process arrives, with a partition size exactly equal to the requesting process size.
- **Advantages**:
    1. **Zero Internal Fragmentation**: Memory allocated matches exact requested size.
    2. **Unbounded Multiprogramming**: Degree of multiprogramming is not constrained by a fixed count of predefined partitions.
- **Limitation**: Suffers from **External Fragmentation**. This occurs when total free memory across scattered holes is sufficient to satisfy a request, but the memory is non-contiguous, preventing allocation.
- **Solution**: **Compaction** — Relocating all allocated processes to one end of main memory so that all scattered free holes combine into a single contiguous block.
- **Allocation Policy Rules of Thumb**:
    - **Best Fit** performs better in **Fixed Partitioning** (minimizes internal fragmentation per static block).
    - **Worst Fit** performs better in **Variable Partitioning** (leaves larger remaining leftover holes that can fit future arriving processes).

---

Step-by-Step Solved Numericals

Numerical 1: Comparing First Fit and Best Fit

**Question**: Consider process requests in order: **300K, 25K, 125K, 50K**. Available contiguous free blocks are **150K** followed by **350K**. Evaluate First Fit and Best Fit.

- **First Fit Analysis**:
    1. Request **300K**: Block 150K is too small. Allocated in 350K block. Leftover in second block = $350K - 300K = 50K$.
    2. Request **25K**: First available block from top is 150K. Allocated in 150K block. Leftover in first block = $150K - 25K = 125K$.
    3. Request **125K**: First available hole is the remaining 125K in first block. Allocated. Remaining in first block = $0K$.
    4. Request **50K**: Fits in the remaining 50K hole of second block. Allocated.
    5. _Result_: **First Fit satisfies all requests.**
- **Best Fit Analysis**:
    1. Request **300K**: Smallest fitting block is 350K. Allocated. Remaining in second block = $50K$.
    2. Request **25K**: Free holes are 150K and 50K. Smallest fitting hole is 50K. Allocated in second block. Remaining in second block = $50K - 25K = 25K$.
    3. Request **125K**: Free holes are 150K and 25K. Smallest fitting hole is 150K. Allocated in first block. Remaining in first block = $150K - 125K = 25K$.
    4. Request **50K**: Available holes are 25K and 25K. Neither hole is $\ge 50K$. **Request fails!**
    5. _Result_: **Best Fit fails.**

**Conclusion**: First fit but not best fit (Option B).

---

Numerical 2 (GATE-2026 MCQ)

**Question**: Physical memory has 8 non-adjacent holes of sizes **20KB, 4KB, 25KB, 18KB, 7KB, 9KB, 15KB, 12KB**. Processes $P_1$ (16KB) and $P_2$ (9KB) arrive in order and are allocated using **Best-Fit**. Find the number of holes of size less than 8KB after allocation.

- **Allocation Execution**:
    1. Allocation of $P_1$ (16KB):
        - Suitable holes ($\ge 16\text{KB}$): $\{20\text{KB}, 25\text{KB}, 18\text{KB}\}$.
        - Best-Fit choice (smallest): **18KB hole**.
        - Remaining hole created = $18\text{KB} - 16\text{KB} = \mathbf{2\text{KB}}$.
    2. Allocation of $P_2$ (9KB):
        - Suitable holes ($\ge 9\text{KB}$): $\{20\text{KB}, 25\text{KB}, 9\text{KB}, 15\text{KB}, 12\text{KB}\}$.
        - Best-Fit choice (smallest): **9KB hole**.
        - Remaining hole created = $9\text{KB} - 9\text{KB} = \mathbf{0\text{KB}}$ (completely filled).
- **Updated Hole List**: $$\text{Holes} = [20\text{KB}, 4\text{KB}, 25\text{KB}, 2\text{KB}, 7\text{KB}, 0\text{KB}, 15\text{KB}, 12\text{KB}]$$
- **Counting Holes** $< 8\text{KB}$:
    - Hole 2: $4\text{KB} < 8\text{KB}$ ($\checkmark$)
    - Hole 4: $2\text{KB} < 8\text{KB}$ ($\checkmark$)
    - Hole 5: $7\text{KB} < 8\text{KB}$ ($\checkmark$)
    - (Note: $0\text{KB}$ denotes a fully occupied memory region, not a free hole).

**Correct Answer**: **3**

---

MODULE 2: PAGING BASICS & ADDRESS TRANSLATION

Mechanics of Paging

Paging is a non-contiguous memory management technique that avoids external fragmentation.

- **Logical View**: A process is divided into equal-sized blocks called **Pages**.
- **Physical View**: Main memory is divided into equal-sized blocks called **Frames**.
- **Fundamental Requirement**: $$\text{Page Size} = \text{Frame Size}$$
- Pages of a process are loaded into any available physical frames in main memory.

---

Page Table Data Structure

The OS maintains a dedicated **Page Table** for each process.

- **Function**: Maps logical Page Numbers ($p$) to physical Frame Numbers ($f$).
- **Entries**: Number of entries in a Page Table equals the total number of pages in the process.

---

Address Translation Architecture

```
           +---------------------------------+
           |       CPU Logical Address       |
           +----------------+----------------+
                            |
                     +------+------+
                     |  p   |  d   |
                     +--+---+--+---+
                        |      |
           +------------+      |
           |                   |
           v                   |
   +---------------+           |
   |  Page Table   |           |
   +---------------+           |
   | Page | Frame  |           |
   +------+--------+           |
   | ...  |  ...   |           |
   |  p   |   f  --+-------+   |
   | ...  |  ...   |       |   |
   +---------------+       |   |
                           v   v
                     +-------+---+
                     |   f   | d |
                     +-------+---+
                     | Physical Address |
                     +---------+--------+
                               |
                               v
                     +------------------+
                     |   Main Memory    |
                     +------------------+
```

1. CPU generates a **Logical Address (LA)** divided into:
    - $p$: Page Number (Index into Page Table)
    - $d$: Page Offset (Byte offset within the page)
2. Memory Management Unit (MMU) uses $p$ to look up the corresponding frame $f$ in the Page Table.
3. The **Physical Address (PA)** is formed by concatenating $[f \mid d]$.
4. **Key Rule**: The offset $d$ passes through completely unchanged during translation because Page Size equals Frame Size.

---

Key Bits & Dimensional Formulas

- **Logical Address Space (LAS) & Physical Address Space (PAS)**: $$\text{Process Size} = 2^{\text{LA}}\text{ Bytes}$$ $$\text{Main Memory Size} = 2^{\text{PA}}\text{ Bytes}$$
- **Page Offset Bits (**$d$**)**: $$d = \log_2(\text{Page Size})$$ $$\text{Page Size} = 2^d\text{ Bytes}$$
- **Page Number Bits (**$p$**) & Frame Bits (**$f$**)**: $$p = \text{LA} - d = \log_2(\text{Number of Pages})$$ $$f = \text{PA} - d = \log_2(\text{Number of Frames})$$ $$\text{Number of Pages in Process} = \frac{\text{Process Size}}{\text{Page Size}} = 2^p$$ $$\text{Number of Frames in Main Memory} = \frac{\text{Main Memory Size}}{\text{Frame Size}} = 2^f$$
- **Page Table Sizing**: $$\text{PTE Size} = \text{Frame bits } (f) + \text{Extra bits (Valid/Invalid, Dirty, Protection bits)}$$ $$\text{Page Table Size} = (\text{Number of Entries}) \times (\text{PTE Size}) = 2^p \times \text{PTE Size}$$

---

Step-by-Step Solved Examples & Homework

Example 1

**Given**: Process Size = $128\text{B}$, Main Memory = $2\text{KB}$, Page Size = $8\text{B}$.

- $\text{LA} = \log_2(128) = 7\text{ bits}$
- $\text{PA} = \log_2(2\text{KB}) = \log_2(2^{11}) = 11\text{ bits}$
- $d = \log_2(8) = 3\text{ bits}$
- $p = \text{LA} - d = 7 - 3 = 4\text{ bits}$ ($\text{Pages} = 2^4 = 16$)
- $f = \text{PA} - d = 11 - 3 = 8\text{ bits}$ ($\text{Frames} = 2^8 = 256$)
- Address Formats: $$\text{LA} = [p=4\text{ bits} \mid d=3\text{ bits}]$$ $$\text{PA} = [f=8\text{ bits} \mid d=3\text{ bits}]$$

---

Example 2

**Given**: Process Size = $4\text{KB}$, Main Memory = $16\text{KB}$, Page Size = $16\text{B}$.

- $\text{LA} = \log_2(4\text{KB}) = 12\text{ bits}$
- $\text{PA} = \log_2(16\text{KB}) = 14\text{ bits}$
- $d = \log_2(16) = 4\text{ bits}$
- $p = 12 - 4 = 8\text{ bits}$ ($\text{Pages} = 2^8 = 256$)
- $f = 14 - 4 = 10\text{ bits}$ ($\text{Frames} = 2^{10} = 1024$)
- $\text{PTE Size} = f + 0 = 10\text{ bits}$
- $\text{Page Table Size} = 256 \times 10\text{ bits} = 2560\text{ bits}$

---

Homework Question 1

**Given**: Process Size = $16\text{MB}$, MM Size = $4\text{GB}$, Page Size = $2\text{KB}$.

1. **Pages in process**: $\frac{16\text{MB}}{2\text{KB}} = \frac{2^{24}}{2^{11}} = 2^{13} = \mathbf{8192}$
2. **Frames in MM**: $\frac{4\text{GB}}{2\text{KB}} = \frac{2^{32}}{2^{11}} = 2^{21} = \mathbf{2,097,152}$
3. **Bits for page number (**$p$**)**: $\mathbf{13\text{ bits}}$
4. **Bits for frame number (**$f$**)**: $\mathbf{21\text{ bits}}$
5. **Entries in Page Table**: $2^{13} = \mathbf{8192}$
6. **Page Table Size**: $2^{13} \times 21\text{ bits} = \mathbf{172,032\text{ bits}}$

---

Homework Question 2

**Given**: Process Size = $128\text{MB}$, MM Size = $2\text{GB}$, Page Size = $1\text{KB}$.

1. **Pages in process**: $\frac{128\text{MB}}{1\text{KB}} = \frac{2^{27}}{2^{10}} = 2^{17} = \mathbf{131,072}$
2. **Frames in MM**: $\frac{2\text{GB}}{1\text{KB}} = \frac{2^{31}}{2^{10}} = 2^{21} = \mathbf{2,097,152}$
3. **Bits for page number (**$p$**)**: $\mathbf{17\text{ bits}}$
4. **Bits for frame number (**$f$**)**: $\mathbf{21\text{ bits}}$
5. **Entries in Page Table**: $2^{17} = \mathbf{131,072}$
6. **Page Table Size**: $2^{17} \times 21\text{ bits} = \mathbf{2,752,512\text{ bits}}$

---

Homework Question 3

**Given**: $\text{LA} = 25\text{ bits}$, $\text{PA} = 33\text{ bits}$, Page Size = $4\text{KB}$ ($d=12\text{ bits}$).

1. **Pages in process**: $2^{\text{LA} - d} = 2^{25 - 12} = 2^{13} = \mathbf{8192}$
2. **Frames in MM**: $2^{\text{PA} - d} = 2^{33 - 12} = 2^{21} = \mathbf{2,097,152}$
3. **Bits for page number (**$p$**)**: $25 - 12 = \mathbf{13\text{ bits}}$
4. **Bits for frame number (**$f$**)**: $33 - 12 = \mathbf{21\text{ bits}}$
5. **Entries in Page Table**: $2^{13} = \mathbf{8192}$
6. **Page Table Size**: $2^{13} \times 21\text{ bits} = \mathbf{172,032\text{ bits}}$

---

GATE-2015 Numerical Problem

**Question**: Page Size = $8\text{KB}$ ($d=13\text{ bits}$), $\text{PA} = 32\text{ bits}$. $\text{PTE} = \text{Valid}(1) + \text{Dirty}(1) + \text{Permission}(3) + \text{Frame bits}(f)$. Maximum Page Table Size = $24\text{MB}$. Find Logical Address length.

- **Step 1**: Find frame bits $f$: $$f = \text{PA} - d = 32 - 13 = 19\text{ bits}$$
- **Step 2**: Calculate total PTE size: $$\text{PTE Size} = 1 + 1 + 3 + 19 = 24\text{ bits} = 3\text{ Bytes}$$
- **Step 3**: Find total number of entries in Page Table: $$\text{Number of Entries} = \frac{\text{Max PT Size}}{\text{PTE Size}} = \frac{24\text{MB}}{3\text{B}} = 8\text{M entries} = 2^{23}\text{ entries}$$
- **Step 4**: Determine page bits $p$ and total Logical Address size: $$p = 23\text{ bits}$$ $$\text{LA Length} = p + d = 23 + 13 = \mathbf{36\text{ bits}}$$

---

Paging Fragmentation Profile

- **External Fragmentation**: **0 Bytes** (completely eliminated).
- **Internal Fragmentation**: Occurs only in the last allocated page of a process.
    - Maximum Internal Fragmentation = $\text{Page Size} - 1\text{ Byte}$
    - Average Internal Fragmentation = $\frac{\text{Page Size}}{2}$

---

MODULE 3: PAGE TABLE STORAGE & EMAT WITHOUT TLB

Page Table Base Register (PTBR)

- Page Tables are stored directly in **Main Memory**.
- The CPU contains a dedicated hardware register named **Page Table Base Register (PTBR)**, which holds the starting physical memory address of the current active process's Page Table.
- Upon a context switch, the OS updates the PTBR with the new process's Page Table base address.

---

Effective Memory Access Time (EMAT) without TLB

- **Access Mechanism**: Every logical memory reference requires **2 physical main memory accesses**:
    1. Access 1: Read the Page Table Entry from main memory (located via $\text{PTBR} + p \times \text{PTE Size}$) to obtain frame number $f$.
    2. Access 2: Read the actual word from main memory at physical address $[f \mid d]$.

$$\text{EMAT} = 2 \times t_{mm}$$

where $t_{mm}$ represents Main Memory Access Time.

- **Special Exception**: If an entire Page Table is small enough to be stored directly inside fast CPU registers, Page Table lookup time becomes negligible ($0\text{ns}$ memory access): $$\text{EMAT} = t_{mm}$$

---

MODULE 4: TRANSLATION LOOKASIDE BUFFER (TLB) & TLB MAPPING SCHEMES

TLB Overview

A **Translation Lookaside Buffer (TLB)** is an associative, high-speed hardware cache inside the Memory Management Unit (MMU) designed to store recent Page-to-Frame mappings $(p \to f)$.

---

EMAT Formula with TLB

Let:

- $H =$ TLB Hit Ratio
- $t_{\text{TLB}} =$ TLB Lookup Time
- $t_{mm} =$ Main Memory Access Time

$$\text{EMAT} = H \times (t_{\text{TLB}} + t_{mm}) + (1 - H) \times (t_{\text{TLB}} + 2 \times t_{mm})$$ $$\text{EMAT} = t_{\text{TLB}} + t_{mm} + (1 - H)t_{mm}$$

$$\text{Speedup} = \frac{\text{EMAT without TLB}}{\text{EMAT with TLB}} = \frac{2 \times t_{mm}}{\text{EMAT with TLB}}$$

Numerical Example:

Given $t_{mm} = 500\text{ns}$, $t_{\text{TLB}} = 20\text{ns}$, $H = 80\% = 0.8$: $$\text{EMAT} = 20 + 500 + (1 - 0.8) \times 500 = 520 + 100 = \mathbf{620\text{ ns}}$$ $$\text{Speedup} = \frac{2 \times 500}{620} = \frac{1000}{620} \approx \mathbf{1.613}$$

---

TLB Hardware Mapping Schemes & Bit-Splitting Formulas

1. Fully Associative Mapping

- Mappings can be placed in any TLB slot. Lookups search all entries simultaneously in parallel.
- Logical Address partition: $[p \mid d]$. No index bits required.

2. Direct Mapping

- Logical Address partition: $[\text{Tag} \mid \text{TLB Entry No.} \mid d]$
- Formulas: $$\text{TLB Entry No. bits} = \log_2(\text{Total TLB Entries})$$ $$\text{Tag bits} = p - \text{TLB Entry No. bits}$$
- **Problem 1**: $\text{LA} = 10\text{ bits}$, Page Size = $4\text{B}$ ($d=2 \implies p=8$), TLB Entries = $8$ ($2^3 \implies 3\text{ bits}$).
    - $\text{TLB Entry No. bits} = 3\text{ bits}$
    - $\text{Tag bits} = 8 - 3 = \mathbf{5\text{ bits}}$
- **Problem 2**: $\text{LA} = 31\text{ bits}$, Page Size = $8\text{KB}$ ($d=13 \implies p=18$), 256-entry TLB ($2^8 \implies 8\text{ bits}$).
    - $\text{TLB Entry No. bits} = 8\text{ bits}$
    - $\text{Tag bits} = 18 - 8 = \mathbf{10\text{ bits}}$
- **Problem 3**: Virtual Address = $44\text{ bits}$, Page Size = $1\text{KB}$ ($d=10 \implies p=34$), 16KB TLB with 4B PTE.
    - $\text{Total TLB Entries} = \frac{16\text{KB}}{4\text{B}} = 4\text{K} = 2^{12} \implies 12\text{ bits}$
    - $\text{Tag bits} = 34 - 12 = \mathbf{22\text{ bits}}$

3. Set-Associative Mapping

- Logical Address partition: $[\text{Tag} \mid \text{TLB Set No.} \mid d]$
- Formulas: $$\text{Number of Sets} = \frac{\text{Total TLB Entries}}{\text{Associativity } (N)}$$ $$\text{Set No. bits} = \log_2(\text{Number of Sets})$$ $$\text{Tag bits} = p - \text{Set No. bits}$$
- **Problem 1**: $\text{LA} = 20\text{ bits}$, Page Size = $256\text{B}$ ($d=8 \implies p=12$), 128-entry 4-way set-associative TLB.
    - $\text{Number of Sets} = \frac{128}{4} = 32 = 2^5 \implies 5\text{ bits}$
    - $\text{Tag bits} = 12 - 5 = \mathbf{7\text{ bits}}$
- **Problem 2**: Virtual Address = $36\text{ bits}$, Page Size = $16\text{KB}$ ($d=14 \implies p=22$), 256-entry 4-way set-associative TLB.
    - $\text{Number of Sets} = \frac{256}{4} = 64 = 2^6 \implies 6\text{ bits}$
    - $\text{Tag bits} = 22 - 6 = \mathbf{16\text{ bits}}$

---

MODULE 5: SEGMENTATION

Mechanics

Segmentation is a user-centric non-contiguous allocation scheme. A process is divided into variable-sized logical modules called **Segments** (e.g., Code/Main Program, Subroutines, Stack, Symbol Table).

---

Segment Table Structure

Each process has a **Segment Table** containing:

1. **Base Address**: The starting physical address of the segment in main memory.
2. **Limit**: The maximum permissible size (length) of the segment in Bytes.

---

Address Translation Architecture & Trap Condition

```
           +---------------------------------+
           |       CPU Logical Address       |
           +----------------+----------------+
                            |
                     +------+------+
                     |  s   |  d   |
                     +--+---+--+---+
                        |      |
           +------------+      |
           |                   |
           v                   |
   +---------------+           |
   | Segment Table |           |
   +---------------+           |
   | Limit | Base  |           |
   +-------+-------+           |
   |  ...  |  ...  |           |
   | Limit | Base -+-------+   |
   +---+---+-------+       |   |
       |                   |   |
       v                   v   v
    Check:               +---+---+
   (d < Limit)?          | Base  | + d
   /          \          +---+---+
 Yes           No            |
  |             |            v
Perform     Trap to OS: Physical Address
Addition    Segmentation Fault
```

1. CPU generates Logical Address $[s \mid d]$ where $s = \text{Segment Number}$ and $d = \text{Offset}$.
2. MMU fetches entry $s$ from the Segment Table.
3. **Hardware Bounds Check**: $$\text{If } d < \text{Limit} \implies \text{Physical Address} = \text{Base} + d$$ $$\text{If } d \ge \text{Limit} \implies \mathbf{Trap\text{ }to\text{ }OS:\text{ }Segmentation\text{ }Fault}$$

---

Lecture Table Example

**Segment Table Data**:

- Segment 0: Base = 2000, Limit = 1500
- Segment 1: Base = 9000, Limit = 400
- Segment 2: Base = 500, Limit = 1000
- Segment 3: Base = 5000, Limit = 2000

Translations:

1. **Address (Seg 1, Offset 234)**:
    - Check: $234 < 400$ (Valid)
    - $\text{PA} = 9000 + 234 = \mathbf{9234}$
2. **Address (Seg 2, Offset 927)**:
    - Check: $927 < 1000$ (Valid)
    - $\text{PA} = 500 + 927 = \mathbf{1427}$
3. **Address (Seg 0, Offset 1646)**:
    - Check: $1646 \ge 1500$ (Invalid)
    - **Result**: $\mathbf{Segmentation\text{ }Fault}$
4. **Address (Seg 3, Offset 1885)**:
    - Check: $1885 < 2000$ (Valid)
    - $\text{PA} = 5000 + 1885 = \mathbf{6885}$

---

Key Difference: Paging vs Segmentation

|Feature|Paging|Segmentation|
|---|---|---|
|**Partition Size**|Fixed equal size (Page Size)|Variable size (Logical modules)|
|**Offset** $d$ **Source**|$\log_2(\text{Page Size})$|$\log_2(\text{Maximum Segment Size})$|
|**Internal Fragmentation**|Yes (in last page)|**No**|
|**External Fragmentation**|**No**|Yes|

---

NAT Example

**Given**: Maximum Segment Size = $16\text{KB}$ ($d = \log_2(16\text{K}) = 14\text{ bits}$), Total Segments = $2^{10}$ ($s = 10\text{ bits}$). $$\text{Logical Address Length} = s + d = 10 + 14 = \mathbf{24\text{ bits}}$$

---

MODULE 6: VIRTUAL MEMORY & DEMAND PAGING

Concept & Demand Paging

- **Virtual Memory**: An OS mechanism that allows execution of processes that are not completely loaded in physical main memory by using secondary disk storage as an extension of main memory.
- **Demand Paging**: Pages of a process are loaded into main memory only when explicitly requested (demanded) by the CPU during execution.

---

Page Table Control Bits

- **Valid / Invalid Bit**:
    - `1`: Page is present in physical main memory.
    - `0`: Page is absent from main memory (located on disk).
- **Dirty / Modified Bit**:
    - `0`: Page is clean (unmodified). Can be overwritten directly on replacement.
    - `1`: Page is dirty (modified). Must be written back to disk before replacement.

---

Page Fault Service Workflow

A **Page Fault** occurs when the CPU references a page with Valid/Invalid bit $= 0$.

```
 [CPU Access] ---> [Page Table: Bit=0] ---> [Trap to OS (Page Fault)]
                                                     |
 [Restart Instruction] <--- [Update Page Table] <--- [Read Page from Disk to Free Frame]
```

1. Hardware traps to the OS upon detecting an invalid page reference.
2. OS saves active process execution context (registers, Program Counter).
3. OS locates missing page location on secondary disk.
4. OS locates a free physical frame (executing page replacement if no frame is free).
5. OS transfers page from disk to frame.
6. OS updates Page Table (sets Frame Number and Valid bit $= 1$).
7. OS restores context and restarts the faulted instruction.

---

Demand Paging EMAT Calculations & GATE PYQs

Basic Demand Paging EMAT Formula

$$\text{EMAT} = (1 - p)(2 \cdot t_{mm}) + p \cdot (t_{mm} + \text{Page Fault Service Time})$$ where $p = \text{Page Fault Rate}$.

---

GATE-2018 Formula Derivation

**Problem**: Let memory access time without fault = $M$, with fault = $D$, average access time = $X$. Derive page fault rate $p$.

$$\text{Derivation:}$$ $$X = (1 - p)M + p D$$ $$X = M - pM + pD$$ $$X - M = p(D - M)$$ $$p = \frac{X - M}{D - M}$$

---

Comprehensive EMAT Formula (TLB + Paging + Page Faults)

$$\text{EMAT} = t_{\text{TLB}} + H \cdot t_{mm} + (1 - H) \left[ t_{mm} + (1 - p)t_{mm} + p \cdot \text{PFST} \right]$$

---

Complete Solved Practice Question

**Given Data**:

- TLB Hit Ratio ($H$) = $90\% = 0.9$
- TLB Access Time ($t_{\text{TLB}}$) = $10\text{ns}$
- Main Memory Access Time ($t_{mm}$) = $200\text{ns}$
- Page Fault Rate ($p$) = $1\% = 0.01$
- Page Fault Service Time ($\text{PFST}$) = $100\mu\text{s} = 100,000\text{ns}$

$$\text{Step-by-Step Evaluation:}$$ $$\text{EMAT} = 10 + (0.9 \times 200) + (1 - 0.9) \left[ 200 + (1 - 0.01) \times 200 + 0.01 \times 100,000 \right]$$ $$\text{EMAT} = 10 + 180 + 0.1 \left[ 200 + (0.99 \times 200) + 1000 \right]$$ $$\text{EMAT} = 190 + 0.1 \left[ 200 + 198 + 1000 \right]$$ $$\text{EMAT} = 190 + 0.1 \left[ 1398 \right]$$ $$\text{EMAT} = 190 + 139.8 = \mathbf{329.8\text{ ns}}$$
