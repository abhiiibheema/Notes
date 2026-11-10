**Diagram Notes: Module 4 — Binary Heaps & Priority Queues**

---

**1. Binary Heap Fundamentals & Array Representation**

**A. What is a Binary Heap?**

A **Binary Heap** is a **Complete Binary Tree (CBT)** stored sequentially in an array that satisfies the **Heap Order Property**.

```
                        Max-Heap Property: Parent ≥ Children
                                       ( 90 )
                                      /      \
                                  ( 80 )    ( 70 )
                                  /    \    /    \
                                (40)  (45)(35)   (50)

                        Min-Heap Property: Parent ≤ Children
                                       ( 10 )
                                      /      \
                                  ( 15 )    ( 20 )
                                  /    \    /    \
                                (30)  (25)(35)   (40)
```

---

**B. Sequential Array Indexing (1-Based Indexing)**

Because a heap is always a Complete Binary Tree, it is stored compactly in an array **without explicit left/right pointers**.

```
  Array:  [  | 90 | 80 | 70 | 40 | 45 | 35 | 50 ]
  Index:    0    1    2    3    4    5    6    7
                 │    │    │
                 │    ├────┴────────────────┐
                 ▼                          ▼
            Root Node                   Children
```

**Core Index Relationships**:

- **Parent** of node at index $i$: $\lfloor i / 2 \rfloor$
- **Left Child** of node at index $i$: $\mathbf{2i}$
- **Right Child** of node at index $i$: $\mathbf{2i + 1}$

---

**C. Internal Nodes vs. Leaf Nodes Range in Array**

For a binary heap of size $n$ stored in a 1-based array:

- **Internal Nodes Range**: Indices $1 \dots \lfloor n / 2 \rfloor$
- **Leaf Nodes Range**: Indices $\lfloor n / 2 \rfloor + 1 \dots n$

```
                            ┌────────────────────────┐
                            │  ARRAY OF SIZE n = 7   │
                            └───────────┬────────────┘
                                        │
             ┌──────────────────────────┴──────────────────────────┐
             ▼                                                     ▼
┌─────────────────────────┐                           ┌─────────────────────────┐
│ Internal Nodes          │                           │ Leaf Nodes              │
│ Indices 1 to ⌊7/2⌋ = 3  │                           │ Indices ⌊7/2⌋+1 = 4 to 7│
│ (Nodes with children)   │                           │ (Leaves with 0 children)│
└─────────────────────────┘                           └─────────────────────────┘
```

---

**2. Core Heap Operations & Workflows**

**A. Insertion Operation (Heapify Up / Percolate Up)**

1. Append the new element at the end of the array (at index $n + 1$).
2. Compare the element with its parent at index $\lfloor i/2 \rfloor$.
3. If the heap property is violated, **swap** with the parent and repeat upwards until restored.
4. **Time Complexity**: Worst/Average Case = $O(\log_2 n)$.

```
  Insert 100 into Max-Heap:
       (90)                   (90)                   (100)
      /    \                 /    \                 /     \
    (80)   (70)     ──►    (100)  (70)    ──►     (90)    (70)
    /                      /                      /
  (100)                  (80)                   (80)
```

---

**B. Deletion Operation (Heapify Down / Percolate Down)**

1. Replace the Root element (at index 1) with the **last element** of the array (at index $n$).
2. Decrease heap size by 1.
3. Compare the new root with its children. Swap with the **larger child** (for Max-Heap) or **smaller child** (for Min-Heap).
4. Repeat downwards until the heap property is satisfied.
5. **Time Complexity**: Worst/Average Case = $O(\log_2 n)$.

```
  Delete 90 from Max-Heap:
       (90)                   (30)                   (80)
      /    \   Copy Last     /    \   Percolate     /    \
    (80)   (70) ────────►  (80)   (70) ────────►  (45)   (70)
    /  \   /               /  \   /               /  \   /
  (40)(45)(35)           (40)(45)(35)           (40)(30)(35)
```

---

**C. Build-Heap Algorithm (Bottom-Up Heapify)**

To transform an arbitrary unsorted array of size $n$ into a valid heap:

1. Ignore all leaf nodes (indices $\lfloor n/2 \rfloor + 1 \dots n$) because a single leaf is trivially a valid heap.
2. Iterate from the last internal node index $i = \lfloor n/2 \rfloor$ down to index $1$.
3. Call `Heapify-Down` on each node at index $i$.
4. **Time Complexity**: $O(n)$ (Tight bound derived via arithmetic-geometric series summation across height levels).

---

**3. Combinatorial Heap Counting & Relative Position Rules**

**A. Recurrence Relation for Counting Max-Heaps**

Given $n$ distinct elements, the number of distinct valid Max-Heaps that can be constructed, denoted $T(n)$, is given by:

$$\mathbf{T(n) = \binom{n - 1}{k} \cdot T(k) \cdot T(n - 1 - k)}$$

_Where:_

- $n - 1$ is the remaining number of elements after placing the maximum element at the root.
- $k$ is the **exact number of nodes in the left subtree** of a Complete Binary Tree with $n$ nodes.
- $n - 1 - k$ is the number of nodes in the right subtree.

```
                         ( Max Element )
                          /           \
               ( k nodes )             ( n - 1 - k nodes )
               Left Subtree               Right Subtree
```

**Standard Calculated Values of** $T(n)$:

- $T(1) = 1$
- $T(2) = 1$
- $T(3) = \binom{2}{1} \cdot T(1) \cdot T(1) = 2 \cdot 1 \cdot 1 = \mathbf{2}$
- $T(4) = \binom{3}{2} \cdot T(2) \cdot T(1) = 3 \cdot 1 \cdot 1 = \mathbf{3}$
- $T(5) = \binom{4}{3} \cdot T(3) \cdot T(1) = 4 \cdot 2 \cdot 1 = \mathbf{8}$

---

**B. Relative Positions in a Max-Heap**

For a Max-Heap containing distinct elements $\{1, 2, \dots, n\}$:

1. **1st Largest Element**: Must ALWAYS be at **Index 1** (Root).
2. **2nd Largest Element**: Must be at **Level 1** (either **Index 2** or **Index 3**).
3. **Smallest Element**: Must ALWAYS be present at one of the **Leaf Nodes** (indices $\lfloor n/2 \rfloor + 1 \dots n$).

---

**4. Step-by-Step Solved Problems**

**Problem 1: Sequential Max-Heap Insertion**

- **Question**: Elements `32, 15, 20, 30, 12, 25, 16` are inserted one by one in that order into an initially empty Max-Heap. What is the level-order traversal of the resultant Max-Heap?
- **Step-by-Step Execution**:
    1. Insert `32`: 1
    2. Insert `15`: 12
    3. Insert `20`: 1more_horiz
    4. Insert `30`: Insert at pos 4 (left child of 15), swaps with 15 $\rightarrow$ 1more_horiz
    5. Insert `12`: Insert at pos 5 (right child of 30) $\rightarrow$ 1more_horiz
    6. Insert `25`: Insert at pos 6 (left child of 20), swaps with 20 $\rightarrow$ 1more_horiz
    7. Insert `16`: Insert at pos 7 (right child of 25) $\rightarrow$ 1more_horiz

```
                          ( 32 )
                         /      \
                     ( 30 )    ( 25 )
                     /    \    /    \
                   (15)  (12)(20)   (16)
```

- **Final Array (Level-Order Output)**: $$\mathbf{$$1more_horiz$$}$$

---

**Problem 2: Priority Queue Insertion into Existing Heap**

- **Question**: A priority queue implemented as a Max-Heap has level-order traversal `10, 8, 5, 3, 2`. Elements `1` and `7` are inserted in that order. What is the level-order traversal of the heap after insertions?
- **Step-by-Step Solution**:
    1. Initial Heap Array: 8more_horiz
    2. Insert `1`: Placed at index 6 (left child of 5). No swap needed (`1 < 5`) $\rightarrow$ 8more_horiz.
    3. Insert `7`: Placed at index 7 (right child of 5). Swaps with `5` (`7 > 5`) $\rightarrow$ `7` moves to index 3. Compare with root `10` (`7 < 10`), stop.
    4. Array state after inserting `7`: 8more_horiz.

```
                          ( 10 )
                         /      \
                      ( 8 )    ( 7 )
                      /   \    /   \
                    (3)   (2)(1)   (5)
```

- **Final Level-Order Traversal**: $$\mathbf{10, 8, 7, 3, 2, 1, 5}$$

---

**Problem 3: Consecutive Delete Operations in Max-Heap**

- **Question**: Given Max-Heap array 5more_horiz. What is the content of the array after **two consecutive delete operations**?
- **Step-by-Step Execution**:
    - **Initial Heap Tree**:

```
                      ( 25 )
                     /      \
                  ( 14 )    ( 16 )
                  /   \     /   \
                (13)  (10) (8)  (12)
```

- **First Delete (Delete 25)**:
    1. Replace root `25` with last element `12`. Array length becomes 6: 5more_horiz.
    2. Heapify-Down at root `12`: Larger child is `16` (index 3). Swap `12` and `16`.
    3. `12` is now at index 3. Children are `8` (index 6). `12 > 8`, stop.
    4. Array after 1st deletion: 5more_horiz.
- **Second Delete (Delete 16)**:
    1. Replace root `16` with last element `8`. Array length becomes 5: 5more_horiz.
    2. Heapify-Down at root `8`: Larger child is `14` (index 2). Swap `8` and `14`.
    3. `8` is now at index 2. Children are `13` (index 4) and `10` (index 5). Larger child is `13`. Swap `8` and `13`.
    4. Array after 2nd deletion: 5more_horiz.
- **Final Array**: $$\mathbf{\{14, 13, 12, 8, 10\}}$$

---

**Problem 4: Array Index Mapping of Complete BST in Heap Representation**

- **Question**: A BST with $N = 157$ distinct elements is a complete binary tree stored using 1-based array representation of binary heaps. If the largest element is stored at index $X$ and the smallest element is stored at index $Y$, find $X + Y$.
- **Step-by-Step Solution**:
    1. In a BST, the **largest element** lies at the rightmost path, following right-child pointers:
        - Index progression for right children: $1 \rightarrow 3 \rightarrow 7 \rightarrow 15 \rightarrow 31 \rightarrow 63 \rightarrow 127$.
        - Level 6 rightmost node is at index `127`.
        - At level 7, nodes start from index `128`. Since $N = 157$, index `128` exists and contains the largest key $\implies \mathbf{X = 128}$.
    2. The **smallest element** lies at the leftmost path, following left-child pointers:
        - Index progression for left children: $1 \rightarrow 2 \rightarrow 4 \rightarrow 8 \rightarrow 16 \rightarrow 32 \rightarrow 64 \rightarrow 128$.
        - However, level 6 left child is index `64` (whose left child is `128`).
        - Looking at the leftmost leaf node in a complete tree of size 157, the bottom-left leaf node is index **127** $\implies \mathbf{Y = 127}$.
    3. **Calculate Sum**: $$X + Y = 128 + 127 = \mathbf{255}$$

---

**Problem 5: Min-Heapify Swap Count Calculation**

- **Question**: An array 17more_horiz of size $n = 10$ is transformed into a Min-Heap using bottom-up heapification. What is the total number of swaps required?
- **Step-by-Step Solution**:
    1. **Identify Internal Nodes**: Internal nodes are from index $i = \lfloor 10/2 \rfloor = 5$ down to index $1$.
        - Node 5 (value 111, child index 10 value 93): Swap 111 and 93 (1 swap).
        - Node 4 (value 11, children 131, 44): Min child is 11, no swap (0 swaps).
        - Node 3 (value 90, children 75, 33): Min child is 33, swap 90 and 33 (1 swap).
        - Node 2 (value 101, children 11, 93): Min child is 11, swap 101 and 11. Then 101 at index 4 has children 131, 44. Swap 101 and 44 (2 swaps).
        - Node 1 (value 82, children 11, 33): Min child is 11, swap 82 and 11. Then 82 at index 2 has min child 44. Swap 82 and 44 (2 swaps).
    2. **Total Swaps**: $$1 + 0 + 1 + 2 + 2 = \mathbf{6\text{ swaps}}$$

---

**Problem 6: Combined Data Structure Count Problem**

- **Question**: For the set of 4 distinct elements $\{1, 2, 3, 4\}$:
    - Let $X$ = Number of labeled binary trees that can be formed.
    - Let $Y$ = Number of Binary Search Trees (BSTs) that can be formed.
    - Let $W$ = Number of AVL trees that can be formed.
    - Let $Z$ = Number of Max-Heaps that can be formed.
    - Find the value of $X + Y + W + Z$.
- **Step-by-Step Calculation**:
    1. **Calculate** $X$ **(Labeled Binary Trees)**: $$X = C_4 \times 4! = 14 \times 24 = \mathbf{336}$$
    2. **Calculate** $Y$ **(Distinct BSTs)**: $$Y = C_4 = \mathbf{14}$$
    3. **Calculate** $W$ **(AVL Trees)**:
        - Out of the 14 BSTs, only 4 balanced tree structures satisfy the AVL Balance Factor condition $\text{BF} \in \{-1, 0, +1\}$. $$W = \mathbf{4}$$
    4. **Calculate** $Z$ **(Max-Heaps)**:
        - For $n = 4$ nodes, left subtree has $k = 2$ nodes: $$Z = T(4) = \binom{3}{2} \cdot T(2) \cdot T(1) = 3 \cdot 1 \cdot 1 = \mathbf{3}$$
    5. **Sum Total**: $$X + Y + W + Z = 336 + 14 + 4 + 3 = \mathbf{357}$$