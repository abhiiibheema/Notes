#### GATE 2027 CS TOPIC WISE TEST - 13

### Q.1

Consider the following resources allocation graph with four processes _P_1, _P_2, _P_3 and _P_4 and three resources _R_1, _R_2 and _R_3.  
System has two instant of _R_1, _R_2 and _R_3 respectively.

![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip11-1776781005509.png)

Which of the following safe sequence are correct?

- 1.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip12-1776781027346.png)
    
- 2.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip13-1776781033785.png)
    
- 3.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip14-1776781042996.png)
    
- 4.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip15-1776781051890.png)
    

##### Question Type: **MCQ**; Marks for correct answer: **2**; Negative Marks: **0**

### Q.2

In a computer system employing a segmentation-based memory management scheme, which of the following task(s) is/are performed by the Memory Management Unit (MMU) hardware during a memory access cycle?

- 1.
    
    Allocation of a new segment table in the main memory for a newly created process.
    
- 2.
    
    Computation of the physical address by adding the segment base address to the logical offset.
    
- 3.
    
    Generating a hardware trap if the logical offset exceeds the segment limit specified in the segment table.
    
- 4.
    
    Determining which segments of a process should be swapped out to the disk when physical memory is full.
    

##### Question Type: **MCQ**; Marks for correct answer: **2**; Negative Marks: **0**

### Q.3

Consider the following track request in the disk queue:  
95, 180, 34, 119, 11, 123, 62, 64  
Shortest Seek Time First (SSTF) disk scheduling algorithm is used and the R/W head initially positioned at location 50. Track are numbered from 0 to 199. If the seek time is 4 msec per track movement, the total seek time needed to service all the request is ________ msec.

- 1.
    
    844
    
- 2.
    
    236
    
- 3.
    
    944
    
- 4.
    
    278
    

##### Question Type: **SCQ**; Marks for correct answer: **2**; Negative Marks: **0.66**

### Q.4

A computer system uses paging for memory management.  
If the page size is halved while keeping the total physical memory size unchanged, which of the following statements are generally true?

- 1.
    
    The size of page table per process increases.
    
- 2.
    
    TLB hit ratio increases.
    
- 3.
    
    Internal fragmentation decreases.
    
- 4.
    
    Number of pages per process increases.
    

##### Question Type: **MCQ**; Marks for correct answer: **2**; Negative Marks: **0**

### Q.5

Consider a system with a physical address size of 44 bits and a virtual address space of 128 GB.  
The system uses paging with page size of 8 KB. Assume that each page table fits entirely with in a single page.  
The minimum number of levels required in the page table is ________.

Backspace7894561230.-

←→

Clear All

##### Question Type: **NAT**; Marks for correct answer: **2**; Negative Marks: **0**

### Q.6

Which of the following statements is/are correct?

- 1.
    
    In an inverted page table, each entry stores the virtual page number and process identifier.
    
- 2.
    
    Inverted page tables reduce memory overhead compared to conventional page tables.
    
- 3.
    
    Reference bit is used to track whether a page has been accessed recently.
    
- 4.
    
    Dirty bits used to indicate an illegal access of page.
    

##### Question Type: **MCQ**; Marks for correct answer: **2**; Negative Marks: **0**

### Q.7

A demand-paged memory system has R page frames. A process repeatedly accesses a fixed ordered list of _S_ different pages, one after another, and once the last page is accessed, the same order starts again.  
Assume:  
• All _S_ pages are distinct.  
• Initially, memory contains no pages.  
• _S_ > _R_  
Which of the following statements is/are correct?

- 1.
    
    If the number of frames is at least equal to the number of distinct pages, then after one complete pass through the list, no further page faults will occur.
    
- 2.
    
    For this cyclic reference string of _S_ distinct pages, FIFO and LRU will generate identical pagefault counts for any number of frames less than S.
    
- 3.
    
    The most-recently-loaded replacement policy always performs as well as the optimal replacement policy for this reference pattern.
    
- 4.
    
    Suppose _S_ = 2_R_. If FIFO is used and the ordered list is executed four consecutive times, then every page reference results in a page fault.
    

##### Question Type: **MCQ**; Marks for correct answer: **2**; Negative Marks: **0**

### Q.8

Which of the following is correct?

- 1.
    
    Demand paging brings a page into the memory only then it is needed.
    
- 2.
    
    Demand paging requires more main memory than normal paging.
    
- 3.
    
    Demand paging can bring entire process into memory at load time (for very small processes).
    
- 4.
    
    In demand paging, a given instruction can lead to page fault.
    

##### Question Type: **MCQ**; Marks for correct answer: **2**; Negative Marks: **0**

### Q.9

Consider the following statements: (with respect to disk space allocation methods)  
**_S_1 :** Linked allocation method suffer from external fragmentation.  
**_S_2 :** Index allocation method suffer from internal fragmentation.  
**_S_3 :** Linked allocation suffers from pointer overhead per block.  
Which of the above statements is correct?

- 1.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip5-1776779127488.png)
    
- 2.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip6-1776779133708.png)
    
- 3.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip7-1776779139378.png)
    
- 4.
    
    ![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip8-1776779145369.png)
    

##### Question Type: **SCQ**; Marks for correct answer: **1**; Negative Marks: **0.33**

### Q.10

Which of the following statements about deadlocking is/are correct?

- 1.
    
    If a system is in an unsafe state at time T, it cannot be deadlock-free at time T.
    
- 2.
    
    If a system is in a safe state at time T, deadlock cannot occur unless additional requests are made.
    
- 3.
    
    If circular wait exists, then deadlock must occur.
    
- 4.
    
    If Banker’s algorithm rejects a request at time T, granting it may or may not cause deadlock.
    

##### Question Type: **MCQ**; Marks for correct answer: **1**; Negative Marks: **0**

### Q.11

Consider a swapping system in which memory consists of hole sizes in following order:  
10 KB, 4 KB, 17 KB, 18 KB, 7 KB, 9 KB, 12 KB, 15 KB, 20 KB  
Which hole is taken for segment request of 12 KB  
(i) By using first fit  
(ii) By using best fit  
(iii) By using worst fit

- 1.
    
    17 KB, 17 KB, 20 KB
    
- 2.
    
    17 KB, 12 KB, 17 KB
    
- 3.
    
    17 KB, 12 KB, 20 KB
    
- 4.
    
    17 KB, 18 KB, 12 KB
    

##### Question Type: **SCQ**; Marks for correct answer: **1**; Negative Marks: **0.33**

### Q.12

In a paging system, it takes 30 ns to search TLB and 90 ns to access the main memory. If TLB misses 25% of time, the effective memory access time is ________ (in ns).

Backspace7894561230.-

←→

Clear All

##### Question Type: **NAT**; Marks for correct answer: **1**; Negative Marks: **0**

### Q.13

Match **List-I** with **List-II** and select the correct answer using the codes given below the lists:

![](https://cdnstatic.madeeasyprime.com/resources/image/mceclip3-1776778830688.png)

- 1.
    
    (a)
    
- 2.
    
    (b)
    
- 3.
    
    (c)
    
- 4.
    
    (d)
    

##### Question Type: **SCQ**; Marks for correct answer: **1**; Negative Marks: **0.33**

### Q.14

In the context of operating systems, which of the following components resolves external references among multiple object modules to generate a single executable file?

- 1.
    
    Compiler
    
- 2.
    
    Linker
    
- 3.
    
    Loader
    
- 4.
    
    Memory Management Unit (MMU)
    

##### Question Type: **SCQ**; Marks for correct answer: **1**; Negative Marks: **0.33**

### Q.15

A bit map can be used to keep track of which blocks are free in a file system’s partition on disk.  
Assuming 1 KB block size and disk size of 64 GB, number of disks blocks for bit map ________ in kilo?

Backspace7894561230.-

←→

Clear All

##### Question Type: **NAT**; Marks for correct answer: **1**; Negative Marks: **0**

### Q.16

Consider a main memory with 3 frames for the following page reference string:  
6, 4, 5, 2, 7, 6, 3, 5, 3, 5, 7, 6  
Assuming that execution of the process is initiated after loading page 6 in memory, the number of page fault using LRU are

- 1.
    
    7
    
- 2.
    
    10
    
- 3.
    
    8
    
- 4.
    
    9
    

##### Question Type: **SCQ**; Marks for correct answer: **1**; Negative Marks: **0.33**

### Q.17

Consider a computer system with 35 bits virtual address and 44 bits physical address. The page size is 4 KB. Each page table entry contains 2 valid bits, 2 reference bits, 1 dirty bit and 3 protection bits. The approximate size of the single level page table is ________ (MB).  
[**Note:** The system is byte addressable]

- 1.
    
    42
    
- 2.
    
    52
    
- 3.
    
    44
    
- 4.
    
    40
    

##### Question Type: **SCQ**; Marks for correct answer: **1**; Negative Marks: **0.33**