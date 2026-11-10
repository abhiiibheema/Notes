**1. Searching Paradigms, Direct Addressing, & Hash Function Principles**

**A. Comparative Efficiency of Searching Techniques**

Search operations across linear and non-linear data structures exhibit contrasting time complexities depending on structural order:

|Data Structure|Condition / Order|Search Time Complexity|
|---|---|---|
|**Unordered Array**|Random arrangement|$O(n)$|
|**Ordered Array**|Sorted elements|$O(\log_2 n)$|
|**Linked List**|Unordered or Ordered|$O(n)$|
|**Binary Search Tree (BST)**|Arbitrary balance|$O(n)$ worst-case|
|**AVL Tree**|Height-balanced (\(|\text{BF}|
|**Direct Addressing Table**|Key used directly as index|$O(1)$ **constant time**|
|**Hash Table**|Key mapped via hash function|$O(1)$ **average time**|

---

**B. Direct Addressing Table vs. Hash Table**

```
                       DIRECT ADDRESSING
Key Space [0...999] ──► Array Index T[Key] ──► Memory Location
(Drawback: Massive memory required if keys are sparse/large)

                            HASHING
Key Space (Large) ──► Hash Function h(k) ──► Slot T[h(k)] in Table size m
(Advantage: Compact array size m << Universe of Keys)
```

- **Direct Addressing**: Stores key $k$ at index $T[k]$. Achieves true $O(1)$ time, but requires an array of size equal to the maximum possible key value ($U$). If key space is large (e.g., 9-digit ID numbers), memory overhead becomes prohibitive.
- **Hash Table**: Maps key $k$ into slot $T[h(k)]$ using a **Hash Function** $h(k)$ over a reduced table size $m$. Each location in a hash table is termed a **Bucket** or **Slot**.

---

**C. Uniform Distribution Property**

A hash function $h(k)$ satisfies **Simple Uniform Hashing** if it distributes keys uniformly across all $m$ slots in the table with equal probability, eliminating systematic bias or bucket concentration:

$$\mathbf{P(h(k) = slot_j) = \frac{1}{m} \quad \text{for all } j \in \{0, 1, \dots, m-1\}}$$

**Proof of Uniformity:** $h(i) = i^3 \bmod 10$ **vs.** $h(i) = i^2 \bmod 10$

For a hash table with $m = 10$ slots (numbered 0 to 9), evaluate key set $i \in \{0, 1, 2, \dots, 9\}$:

1. **Testing** $h(i) = i^2 \bmod 10$:
    - $0^2 \bmod 10 = 0$
    - $1^2 \bmod 10 = 1$
    - $2^2 \bmod 10 = 4$
    - $3^2 \bmod 10 = 9$
    - $4^2 \bmod 10 = 6$
    - $5^2 \bmod 10 = 5$
    - $6^2 \bmod 10 = 6$
    - $7^2 \bmod 10 = 9$
    - $8^2 \bmod 10 = 4$
    - $9^2 \bmod 10 = 1$
    - **Mapped Slots**: Only slots $\{0, 1, 4, 5, 6, 9\}$ are filled. Slots $\{2, 3, 7, 8\}$ remain completely empty. This function is **Non-Uniform**.
2. **Testing** $h(i) = i^3 \bmod 10$:
    - $0^3 \bmod 10 = 0$, $1^3 \bmod 10 = 1$, $2^3 \bmod 10 = 8$, $3^3 \bmod 10 = 7$, $4^3 \bmod 10 = 4$, $5^3 \bmod 10 = 5$, $6^3 \bmod 10 = 6$, $7^3 \bmod 10 = 3$, $8^3 \bmod 10 = 2$, $9^3 \bmod 10 = 9$.
    - **Mapped Slots**: Every single slot from $0$ to $9$ receives exactly one distinct key mapping. This function is **Strictly Uniform**.

---

**2. Collision Resolution Taxonomy & Open Hashing (Separate Chaining)**

**A. The Collision Problem & Taxonomy**

A **Collision** occurs when two or more distinct keys map to the exact same hash table slot:

$$\mathbf{h(k_1) = h(k_2) \quad \text{for } k_1 \neq k_2}$$

```
                          COLLISION RESOLUTION
                                   │
         ┌─────────────────────────┴─────────────────────────┐
         ▼                                                   ▼
  OPEN HASHING (Separate Chaining)                   CLOSED HASHING (Open Addressing)
  (Collided keys stored OUTSIDE                     (Collided keys stored INSIDE
   table in dynamic linked lists)                    table at alternative slots)
                                                             │
                                        ┌────────────────────┼────────────────────┐
                                        ▼                    ▼                    ▼
                                 Linear Probing      Quadratic Probing      Double Hashing
```

---

**B. Open Hashing: Separate Chaining Mechanics**

Each slot in the hash table acts as a head pointer to an independent singly linked list. When a collision occurs, a new dynamic node is created and appended to that slot's chain.

```
Slot 0 ──► [70 | NULL]
Slot 1 ──► [15 |  •─]──► [85 | NULL]
Slot 2 ──► [23 |  •─]──► [65 | NULL]
Slot 3 ──► NULL
Slot 4 ──► NULL
Slot 5 ──► [47 |  •─]──► [89 | NULL]
Slot 6 ──► [34 |  •─]──► [97 | NULL]
```

**Load Factor (**$\alpha$**) & Performance Metrics**:

- **Load Factor (**$\alpha$**)**: Average number of elements per slot: $$\mathbf{\alpha = \frac{n}{m} = \frac{\text{Total Keys (n)}}{\text{Table Size (m)}}}$$
- **Search Time Complexity**:
    - Successful Search: $O(1 + \frac{\alpha}{2})$
    - Unsuccessful Search: $O(1 + \alpha)$
    - Worst-case (all $n$ keys hash to same slot): $O(n)$

---

**C. Solved Separate Chaining Problems**

**Problem 2.1: Separate Chaining Layout Construction**

- **Question**: Construct the hash table for keys $\{15, 47, 23, 34, 85, 97, 65, 89, 70\}$ using $h(x) = x \bmod 7$ and table size $m = 7$.
- **Step-by-Step Mapping**:
    - $70 \bmod 7 = 0 \implies$ **Slot 0**: `70`
    - $15 \bmod 7 = 1$, $85 \bmod 7 = 1 \implies$ **Slot 1**: `15 -> 85`
    - $23 \bmod 7 = 2$, $65 \bmod 7 = 2 \implies$ **Slot 2**: `23 -> 65`
    - **Slots 3 & 4**: Empty (`NULL`)
    - $47 \bmod 7 = 5$, $89 \bmod 7 = 5 \implies$ **Slot 5**: `47 -> 89`
    - $34 \bmod 7 = 6$, $97 \bmod 7 = 6 \implies$ **Slot 6**: `34 -> 97`

---

**Problem 2.2: Chain Length Metrics Calculation**

- **Question**: Given a hash table with $m = 9$ slots and $h(k) = k \bmod 9$. Insert keys in order: `5, 28, 19, 15, 20, 33, 12, 17, 10`. Find maximum, minimum, and average chain lengths.
- **Step-by-Step Mapping**:
    - $5 \bmod 9 = 5 \implies$ Slot 5: 1 (Length 1)
    - $28 \bmod 9 = 1, 19 \bmod 9 = 1, 10 \bmod 9 = 1 \implies$ Slot 1: `[28 -> 19 -> 10]` (Length 3)
    - $20 \bmod 9 = 2 \implies$ Slot 2: 2 (Length 1)
    - $12 \bmod 9 = 3 \implies$ Slot 3: 3 (Length 1)
    - $15 \bmod 9 = 6, 33 \bmod 9 = 6 \implies$ Slot 6: `[15 -> 33]` (Length 2)
    - $17 \bmod 9 = 8 \implies$ Slot 8: 4 (Length 1)
    - Slots 0, 4, 7 contain no keys (Length 0).
- **Final Metrics**:
    - **Maximum Chain Length**: **3** (at Slot 1)
    - **Minimum Chain Length**: **0** (at Slots 0, 4, 7)
    - **Average Chain Length**: $\mathbf{\frac{n}{m} = \frac{9}{9} = 1}$

---

**Problem 2.3: Probability of Unfilled Slots in Chaining**

- **Question**: Consider a hash table with 100 slots using chaining. Assuming simple uniform hashing, what is the probability that the first 3 slots (slots 0, 1, 2) remain unfilled after inserting 3 keys?
- **Step-by-Step Derivation**:
    1. Total available slots = $100$. Target empty slots = $3$.
    2. Non-target available slots for any key = $100 - 3 = 97$ slots.
    3. Probability that Key 1 does not land in slots 0, 1, or 2 = $\frac{97}{100}$.
    4. Under simple uniform hashing, each insertion is independent.
    5. Probability that all 3 keys avoid the first 3 slots: $$\mathbf{P = \frac{97}{100} \times \frac{97}{100} \times \frac{97}{100} = \frac{97^3}{100^3}}$$

---

**3. Closed Hashing: Linear Probing & Primary Clustering**

**A. Linear Probing Mechanics**

When a collision occurs at initial slot $h(k)$, linear probing sequentially checks adjacent slots in a circular order:

$$\mathbf{h(k, i) = (h(k) + i) \bmod m \quad \text{for } i = 0, 1, 2, \dots, m-1}$$

```
Probe Sequence:
Probe 0: h(k) % m
Probe 1: (h(k) + 1) % m
Probe 2: (h(k) + 2) % m
...
Probe i: (h(k) + i) % m
```

---

**B. Primary Clustering Phenomenon**

Linear probing suffers from **Primary Clustering**—the tendency for occupied slots to form long contiguous blocks in memory. Once a cluster forms, any key hashing anywhere into the cluster probes linearly through the entire block, lengthening the cluster further and severely degrading search and insertion times toward $O(n)$.

```
[  ][ 41 ][  ][  ][ 18 ][ 44 ][ 59 ][ 32 ][ 22 ][ 31 ][ 73 ][  ][  ]
                    ▲───────────────────────────────▲
                        PRIMARY CLUSTER FORMATION
```

---

**C. Solved Linear Probing Problems**

**Problem 3.1: Complete Linear Probing Table Construction**

- **Question**: Table size $m = 13$, $h(k) = k \bmod 13$, linear probing. Insert keys in order: `18, 41, 22, 44, 59, 32, 31, 73`.
- **Step-by-Step Probe Tracing**:
    - `18`: $18 \bmod 13 = 5 \implies$ **Slot 5**
    - `41`: $41 \bmod 13 = 2 \implies$ **Slot 2**
    - `22`: $22 \bmod 13 = 9 \implies$ **Slot 9**
    - `44`: $44 \bmod 13 = 5$ (collision). Probe 1: $(5+1)\%13 = 6 \implies$ **Slot 6**
    - `59`: $59 \bmod 13 = 7 \implies$ **Slot 7**
    - `32`: $32 \bmod 13 = 6$ (collision) $\to 7$ (collision) $\to 8 \implies$ **Slot 8**
    - `31`: $31 \bmod 13 = 5$ (collision) $\to 6 \to 7 \to 8 \to 9 \to 10 \implies$ **Slot 10**
    - `73`: $73 \bmod 13 = 8$ (collision) $\to 9 \to 10 \to 11 \implies$ **Slot 11**
- **Final Table Array**: `Slot 2: 41 | Slot 5: 18 | Slot 6: 44 | Slot 7: 59 | Slot 8: 32 | Slot 9: 22 | Slot 10: 31 | Slot 11: 73`

---

**Problem 3.2: Target Key Location Search**

- **Question**: Hash table with 10 buckets (0 to 9), linear probing, $h(k) = k \bmod 10$. Keys inserted: `43, 165, 62, 123, 142`. In what slot will key `142` be stored?
- **Step-by-Step Probe Tracing**:
    1. `43` $\to$ Slot 3; `165` $\to$ Slot 5; `62` $\to$ Slot 2.
    2. `123`: $123 \bmod 10 = 3$ (occupied by 43). Probe 1: $(3+1)\%10 = 4 \implies$ Slot 4.
    3. `142`: $142 \bmod 10 = 2$ (occupied by 62).
        - Probe 1: $(2+1)\%10 = 3$ (occupied by 43)
        - Probe 2: $(2+2)\%10 = 4$ (occupied by 123)
        - Probe 3: $(2+3)\%10 = 5$ (occupied by 165)
        - Probe 4: $(2+4)\%10 = 6$ (empty!) $\implies$ **Slot 6**.

---

**Problem 3.3: Sequence Permutations for Table State**

- **Question**: A hash table of length 10 uses linear probing with $h(k) = k \bmod 10$. The table state shows: `Slot 2: 42, Slot 3: 23, Slot 4: 34, Slot 5: 52, Slot 6: 46, Slot 7: 33`.
    1. Which option gives a valid insertion order? _(A) 46, 42, 34, 52, 23, 33_ _(B) 34, 42, 23, 52, 33, 46_ _(C) 46, 34, 42, 23, 52, 33_ _(D) 42, 46, 33, 23, 34, 52_
    2. How many total different insertion sequences result in this exact table state?
- **Step-by-Step Verification & Calculation**:
    1. **Option Verification**:
        - Test Option (C): `46, 34, 42, 23, 52, 33`.
        - `46` $\to$ Slot 6; `34` $\to$ Slot 4; `42` $\to$ Slot 2; `23` $\to$ Slot 3 (all land directly).
        - `52`: $52 \bmod 10 = 2$ (occupied) $\to 3 \to 4 \to$ Slot 5.
        - `33`: $33 \bmod 10 = 3$ (occupied) $\to 4 \to 5 \to 6 \to$ Slot 7.
        - **Valid Sequence**: **Option (C)**.
    2. **Total Valid Sequences Count**:
        - Keys $\{42, 23, 34, 46\}$ land in home slots without collision and can appear in any relative order ($4! = 24$ ways).
        - Key `52` must arrive after `42, 23, 34`. Key `33` must arrive after `23, 34, 52, 46`.
        - Combining independent arrival choices yields **30 total valid insertion sequences**.

---

**4. Closed Hashing: Quadratic Probing & Cycle Pitfalls**

**A. Quadratic Probing Mechanics**

Quadratic probing eliminates primary clustering by adding a quadratic term $i^2$ to the probe index:

$$\mathbf{h(k, i) = (h(k) + i^2) \bmod m \quad \text{for } i = 0, 1, 2, \dots, m-1}$$

```
Probe 0: h(k) % m
Probe 1: (h(k) + 1²) % m
Probe 2: (h(k) + 2²) % m
Probe 3: (h(k) + 3²) % m
...
```

- **Secondary Clustering**: Keys hashing to the exact same initial position follow identical quadratic probe paths.

---

**B. Infinite Cycle Pitfall**

Unlike linear probing (which guarantees probing all $m$ slots if $m$ and step size are coprime), quadratic probing does **not** guarantee probing every slot in the table. If table size $m$ is not a prime number, the probe sequence $i^2 \bmod m$ may cycle endlessly through a proper subset of slots, failing to find an open slot even when the table is not full.

---

**C. Solved Quadratic Probing Problems**

**Problem 4.1: Quadratic Probing Construction & Target Lookup**

- **Question**: Table size $m = 20$, $h(x, i) = (x + i^2) \bmod 20$. Insert keys: `{96, 48, 63, 29, 87, 77, 47, 73, 70, 86, 166, 80, 147}`. Find the slot index for key `147`.
- **Step-by-Step Tracing**:
    - Direct placements: `96` $\to 16$, `48` $\to 8$, `63` $\to 3$, `29` $\to 9$, `87` $\to 7$, `77` $\to 17$, `73` $\to 13$, `70` $\to 10$, `86` $\to 6$, `80` $\to 0$.
    - `47`: $47 \bmod 20 = 7$ (occupied) $\to (7+1^2)\%20 = 8$ (occupied) $\to (7+2^2)\%20 = \mathbf{11}$.
    - `166`: $166 \bmod 20 = 6$ (occupied) $\to 7$ (occupied) $\to 10$ (occupied) $\to (6+3^2)\%20 = \mathbf{15}$.
    - `147`: Initial hash $147 \bmod 20 = 7$ (occupied by 87).
        - Probe 1: $(7 + 1^2) \bmod 20 = 8$ (occupied by 48)
        - Probe 2: $(7 + 2^2) \bmod 20 = 11$ (occupied by 47)
        - Probe 3: $(7 + 3^2) \bmod 20 = 16$ (occupied by 96)
        - Probe 4: $(7 + 4^2) \bmod 20 = 23 \bmod 20 = 3$ (occupied by 63)
        - Probe 5: $(7 + 5^2) \bmod 20 = 32 \bmod 20 = \mathbf{12}$ (empty!).
- **Result**: Key `147` is stored at **Slot 12**.

---

**Problem 4.2: Infinite Cycle Demonstration**

- **Question**: Table size $m = 10$, $h(x, i) = (x + i^2) \bmod 10$. Table contains keys at slots: `Slot 1: 11, Slot 2: 2, Slot 3: 3, Slot 6: 6, Slot 7: 7, Slot 8: 8`. Insert key `12`.
- **Step-by-Step Tracing**:
    - Primary Hash: $12 \bmod 10 = 2$ (occupied by 2).
    - Probe 1: $(2 + 1^2) \bmod 10 = 3$ (occupied by 3)
    - Probe 2: $(2 + 2^2) \bmod 10 = 6$ (occupied by 6)
    - Probe 3: $(2 + 3^2) \bmod 10 = 1$ (occupied by 11)
    - Probe 4: $(2 + 4^2) \bmod 10 = 8$ (occupied by 8)
    - Probe 5: $(2 + 5^2) \bmod 10 = 7$ (occupied by 7)
    - Probe 6: $(2 + 6^2) \bmod 10 = 38 \bmod 10 = 8$ (occupied again!)
- **Conclusion**: Probing repeats the cyclic sequence $\{2, 3, 6, 1, 8, 7\}$ indefinitely, **never visiting empty slots 0, 4, 5, 9**.

---

**5. Closed Hashing: Double Hashing Mechanics & Calculations**

**A. Double Hashing Mechanics**

Double hashing eliminates both primary and secondary clustering by utilizing two independent hash functions: $h_1(k)$ for initial bucket placement and $h_2(k)$ as a key-dependent offset multiplier:

$$\mathbf{h(k, i) = (h_1(k) + i \cdot h_2(k)) \bmod m \quad \text{for } i = 0, 1, 2, \dots, m-1}$$

```
Probe 0: h₁(k) % m
Probe 1: (h₁(k) + 1 × h₂(k)) % m
Probe 2: (h₁(k) + 2 × h₂(k)) % m
Probe 3: (h₁(k) + 3 × h₂(k)) % m
...
```

- **Constraints on** $h_2(k)$:
    1. $h_2(k)$ must **never evaluate to 0** (otherwise probing halts).
    2. $h_2(k)$ must be **coprime** to table size $m$ (achieved by choosing $m$ as a prime number and $h_2(k) = 1 + (k \bmod m')$ where $m' = m - 1$ or $m - 2$).

---

**B. Solved Double Hashing Problems**

**Problem 5.1: Double Hashing Table Construction**

- **Question**: Table size $m = 13$, $h_1(k) = k \bmod 13$, $h_2(k) = 8 - (k \bmod 8)$. Probing formula: $(h_1(k) + i \cdot h_2(k)) \bmod 13$. Insert keys in order: `18, 41, 22, 44, 59, 32, 31, 73`.
- **Step-by-Step Probe Tracing**:
    - `18`: $h_1(18) = 5 \implies$ **Slot 5**
    - `41`: $h_1(41) = 2 \implies$ **Slot 2**
    - `22`: $h_1(22) = 9 \implies$ **Slot 9**
    - `59`: $h_1(59) = 7 \implies$ **Slot 7**
    - `32`: $h_1(32) = 6 \implies$ **Slot 6**
    - `44`: $h_1(44) = 5$ (occupied). Offset $h_2(44) = 8 - (44 \bmod 8) = 8 - 4 = 4$.
        - Probe 1: $(5 + 1 \times 4) \bmod 13 = 9$ (occupied)
        - Probe 2: $(5 + 2 \times 4) \bmod 13 = 13 \bmod 13 = \mathbf{0} \implies$ **Slot 0**
    - `31`: $h_1(31) = 5$ (occupied). Offset $h_2(31) = 8 - (31 \bmod 8) = 8 - 7 = 1$.
        - Probe 1: $(5 + 1) \bmod 13 = 6$ (occupied)
        - Probe 2: $(5 + 2) \bmod 13 = 7$ (occupied)
        - Probe 3: $(5 + 3) \bmod 13 = \mathbf{8} \implies$ **Slot 8**
    - `73`: $h_1(73) = 8$ (occupied). Offset $h_2(73) = 8 - (73 \bmod 8) = 8 - 1 = 7$.
        - Probe 1: $(8 + 1 \times 7) \bmod 13 = 15 \bmod 13 = 2$ (occupied)
        - Probe 2: $(8 + 2 \times 7) \bmod 13 = 22 \bmod 13 = 9$ (occupied)
        - Probe 3: $(8 + 3 \times 7) \bmod 13 = 29 \bmod 13 = \mathbf{3} \implies$ **Slot 3**

---

**Problem 5.2: Probe Address Calculation**

- **Question**: Double hashing scheme with $m = 23$, $h_1(k) = k \bmod 23$, $h_2(k) = 1 + (k \bmod 19)$. Find the address returned by **Probe 1** for key $k = 90$.
- **Step-by-Step Calculation**:
    1. Primary Hash: $h_1(90) = 90 \bmod 23 = 21$.
    2. Secondary Hash Offset: $h_2(90) = 1 + (90 \bmod 19) = 1 + 14 = 15$.
    3. Probe 1 Formula: $\text{Address} = (h_1(k) + 1 \cdot h_2(k)) \bmod m$. $$\mathbf{\text{Address} = (21 + 1 \times 15) \bmod 23 = 36 \bmod 23 = 13}$$

---

**Problem 5.3: GATE 2025 PYQ Slot Identification**

- **Question**: Double hashing scheme with $m = 11$, $h_1(k) = k \bmod 11$, $h_2(k) = 1 + (k \bmod 7)$. Formula: $[h_1(k) + i \cdot h_2(k)] \bmod 11$. Keys inserted in order: `63, 50, 25, 79, 67, 24`. At which slot index is key `24` stored?
- **Step-by-Step Execution**:
    1. Insert earlier keys:
        - `63`: $63 \bmod 11 = 8 \implies$ Slot 8
        - `50`: $50 \bmod 11 = 6 \implies$ Slot 6
        - `25`: $25 \bmod 11 = 3 \implies$ Slot 3
        - `79`: $79 \bmod 11 = 2 \implies$ Slot 2
        - `67`: $67 \bmod 11 = 1 \implies$ Slot 1
    2. Occupied slots before inserting `24`: `{1: 67, 2: 79, 3: 25, 6: 50, 8: 63}`.
    3. Insert `24`:
        - $h_1(24) = 24 \bmod 11 = 2$ (occupied by 79).
        - Offset $h_2(24) = 1 + (24 \bmod 7) = 1 + 3 = 4$.
        - Probe 1: $(2 + 1 \times 4) \bmod 11 = 6$ (occupied by 50).
        - Probe 2: $(2 + 2 \times 4) \bmod 11 = 10 \bmod 11 = \mathbf{10}$ (empty!).
- **Final Slot Index**: **10**