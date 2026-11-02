4: HUFFMAN ENCODING & DATA COMPRESSION

---

1. DATA COMPRESSION PARADIGMS & ENCODING TYPES

**Huffman Encoding** is a widely used greedy algorithm for **lossless data compression**. It reduces the total storage size or transmission bandwidth required for text/files without losing any original information.

```
                           Data Encoding Schemes
                                     │
         ┌───────────────────────────┴───────────────────────────┐
         ▼                                                       ▼
  Uniform Encoding (Fixed-Length)               Non-Uniform Encoding (Variable-Length)
 - Every character gets equal bit length        - High frequency  ──► Shorter codes (fewer bits)
 - Formula: ⌈log₂ Nrceil bits/character            - Low frequency   ──► Longer codes (more bits)
 - Compression Ratio = 0% (No Compression)      - Achieves Optimal Data Compression
```

---

A. Uniform Encoding (Fixed-Length)

In uniform encoding, every distinct character in an alphabet set of size $N$ is assigned a binary code of identical bit length.

Bit Formula for Uniform Encoding

To uniquely represent $N$ distinct characters:

$$\text{Bits per character} = \lceil \log_2 N \rceil$$

- **4 characters:** $\lceil \log_2 4 \rceil = 2$ bits/char (e.g., $W=00, X=01, Y=10, Z=11$).
- **5 to 8 characters:** $\lceil \log_2 5 \rceil = 3$ bits/char (can uniquely encode up to $2^3 = 8$ elements).
- **10 characters:** $\lceil \log_2 10 \rceil = 4$ bits/char (can uniquely encode up to $2^4 = 16$ elements).
- **100 characters:** $\lceil \log_2 100 \rceil = 7$ bits/char (can uniquely encode up to $2^7 = 128$ elements).

🚩 **FUNDAMENTAL LIMITATION OF UNIFORM ENCODING:** Uniform encoding treats all characters equally regardless of their frequency of occurrence in the text. Because every character consumes the exact same number of bits, **no data compression is possible using uniform encoding**.

---

B. Non-Uniform Encoding (Variable-Length)

Non-uniform encoding assigns variable bit lengths to characters based on their **frequency** or **probability of occurrence** $P(c)$.

Inverse Proportionality Rule

$$\text{Bit Length } (d_i) \propto \frac{1}{\text{Frequency } (f_i)}$$

1. **Frequently occurring elements:** Assigned **shorter binary codes** (fewer bits).
2. **Rarely occurring elements:** Assigned **longer binary codes** (more bits).
3. **Prefix Code Property:** No assigned binary code is a prefix of another character's binary code. This guarantees unambiguous decoding without needing separator bits.

---

2. HUFFMAN ALGORITHM & TREE CONSTRUCTION

Huffman Encoding builds a **Binary Trie / Extended Binary Tree** bottom-up by applying the **Optimal Merge Pattern** strategy (Min-Heap priority queue).

```
                      Huffman Tree Construction
                                  │
         ┌────────────────────────┴────────────────────────┐
         ▼                                                 ▼
   Min-Heap Priority Queue                      Bottom-Up Merging
- Insert all N characters by frequency         - Extract 2 smallest nodes
- Build Heap: O(N)                             - Parent weight = Sum of children
- Extract-Min: O(log N)                         - Re-insert parent into Min-Heap
```

---

Step-by-Step Algorithmic Procedure

```
Algo HuffmanEncoding(C, F, N) {
    // Step 1: Initialize a leaf node for each character and insert into Min-Heap Q
    for (i = 1; i <= N; i++) {
        node = NewNode(C[i], F[i]);
        Insert_Min_Heap(Q, node);
    }
    
    // Step 2: Merge the two lowest-frequency nodes iteratively (N - 1 passes)
    for (i = 1; i <= N - 1; i++) {
        left  = Extract_Min(Q); // Smallest frequency node
        right = Extract_Min(Q); // Second smallest frequency node
        
        // Create new internal node with combined weight
        parent = NewNode(NULL, left.weight + right.weight);
        parent.left  = left;
        parent.right = right;
        
        Insert_Min_Heap(Q, parent);
    }
    
    // Step 3: Root of Huffman Tree remains in Q
    root = Extract_Min(Q);
    
    // Step 4: Traverse tree to assign binary prefix codes (Left = 0, Right = 1)
    AssignCodes(root, "");
}
```

---

3. MATHEMATICAL FORMULAS & PERFORMANCE METRICS

4. Weighted External Path Length (Average Bits per Character)

When character frequencies are given as probabilities $P_i$ (where $\sum P_i = 1.0$):

$$\text{Average Bits / Character} = \sum_{i=1}^N d_i \cdot P_i$$

Where:

- $d_i$: Depth of character $i$ in the Huffman Tree (length of its binary code).
- $P_i$: Probability of occurrence of character $i$.

---

2. Total Encoded Bit Length for a Text

When character counts/frequencies $f_i$ are given for a text of length $L = \sum f_i$:

$$\text{Total Bits (Huffman)} = \sum_{i=1}^N d_i \cdot f_i$$

$$\text{Total Bits (Uniform)} = L \times \lceil \log_2 N \rceil$$

$$\text{Bit Savings} = \text{Total Bits (Uniform)} - \text{Total Bits (Huffman)}$$

---

Time & Space Complexity

- **Building Huffman Tree (Min-Heap):** $N - 1$ merges $\times O(\log_2 N)$ heap ops $\implies \mathbf{O(N \log_2 N)}$.
- **Pre-sorted Frequencies Optimization:** If frequencies are provided pre-sorted, using two queues yields $O(N)$ time.
- **Auxiliary Space Complexity:** $\mathbf{O(N)}$ to store tree nodes and priority queue.

---

4. COMPARATIVE SUMMARY MATRIX

|Metric / Parameter|Uniform Encoding (Fixed-Length)|Huffman Encoding (Variable-Length)|
|---|---|---|
|**Code Assignment Basis**|Arbitrary index assignment|**Character Frequency / Probability**|
|**Bits Per Character**|Constant ($\lceil \log_2 N \rceil$)|Variable (Short for frequent, Long for rare)|
|**Prefix Property**|Trivially satisfied (equal lengths)|**Strictly enforced by Tree leaves**|
|**Data Compression**|**0% Compression (No Savings)**|**Optimal Lossless Data Compression**|
|**Algorithmic Time**|$O(N)$|$\mathbf{O(N \log_2 N)}$|

---

5. COMPLETE SOLVED PROBLEM SET & EXAM QUESTIONS

---

Problem 1: Full Probability Trace & Average Bits Calculation

A language consists of 5 characters $\{A, B, C, D, E\}$ with probabilities: $A = 0.23, B = 0.05, C = 0.48, D = 0.15, E = 0.09$.

1. Calculate the bits/character required under **Uniform Encoding**.
2. Construct the **Huffman Tree** and find the bit code length for each character.
3. Calculate the **Average Bits per Character** using Huffman Encoding.

Step-by-Step Solution:

1. **Uniform Encoding:** For $N = 5$ characters: $$\text{Uniform Bits} = \lceil \log_2 5 \rceil = \mathbf{3 \text{ bits/character}}$$
2. **Huffman Tree Construction (Min-Heap Merges):**
    - **Initial Pool:** $\{B(0.05), E(0.09), D(0.15), A(0.23), C(0.48)\}$.
    - **Merge 1:** Pick smallest two $B(0.05)$ and $E(0.09)$ $\to$ Node $N_1(0.14)$.
        - Active: $\{D(0.15), N_1(0.14), A(0.23), C(0.48)\}$.
    - **Merge 2:** Pick smallest two $N_1(0.14)$ and $D(0.15)$ $\to$ Node $N_2(0.29)$.
        - Active: $\{A(0.23), N_2(0.29), C(0.48)\}$.
    - **Merge 3:** Pick smallest two $A(0.23)$ and $N_2(0.29)$ $\to$ Node $N_3(0.52)$.
        - Active: $\{C(0.48), N_3(0.52)\}$.
    - **Merge 4:** Pick remaining two $C(0.48)$ and $N_3(0.52)$ $\to$ Root $N_4(1.00)$.
3. **Code Depth Analysis:**
    - **C:** depth 1 $\implies 1$ bit.
    - **A:** depth 2 $\implies 2$ bits.
    - **D:** depth 3 $\implies 3$ bits.
    - **B:** depth 4 $\implies 4$ bits.
    - **E:** depth 4 $\implies 4$ bits.
4. **Average Bits per Character Calculation:** $$\text{Avg Bits} = \sum P_i \cdot d_i = (0.48 \times 1) + (0.23 \times 2) + (0.15 \times 3) + (0.05 \times 4) + (0.09 \times 4)$$ $$\text{Avg Bits} = 0.48 + 0.46 + 0.45 + 0.20 + 0.36 = \mathbf{1.95 \text{ bits/character}}$$

**Answer:**

1. Uniform = **3 bits/char**.
2. Depths = **C:1, A:2, D:3, B:4, E:4**.
3. Average Bits = **1.95 bits/character**.

---

Problem 2: Encoded String Compression (GATE PYQ)

Consider the string `"abbccddeee"`. Each letter must be assigned a binary code satisfying the prefix property. Calculate the **minimum length of the encoded string in bits**.

Step-by-Step Solution:

1. **Compute Character Frequencies:**
    - Total length $L = 10$.
    - $a: 1$, $b: 2$, $c: 2$, $d: 2$, $e: 3$.
2. **Build Huffman Tree:**
    - **Merge 1:** Pick smallest two $a(1)$ and $b(2)$ $\to N_1(3)$.
        - Active: $c(2), d(2), e(3), N_1(3)$.
    - **Merge 2:** Pick smallest two $c(2)$ and $d(2)$ $\to N_2(4)$.
        - Active: $e(3), N_1(3), N_2(4)$.
    - **Merge 3:** Pick smallest two $e(3)$ and $N_1(3)$ $\to N_3(6)$.
        - Active: $N_2(4), N_3(6)$.
    - **Merge 4:** Pick remaining two $N_2(4)$ and $N_3(6)$ $\to \text{Root}(10)$.
3. **Determine Code Lengths (Depths):**
    - $e$: depth 2 $\implies 2$ bits.
    - $c, d$: depth 2 $\implies 2$ bits each.
    - $a, b$: depth 3 $\implies 3$ bits each.
4. **Calculate Total Encoded Bits:** $$\text{Total Bits} = (f_a \times 3) + (f_b \times 3) + (f_c \times 2) + (f_d \times 2) + (f_e \times 2)$$ $$\text{Total Bits} = (1 \times 3) + (2 \times 3) + (2 \times 2) + (2 \times 2) + (3 \times 2) = 3 + 6 + 4 + 4 + 6 = \mathbf{23 \text{ bits}}$$

**Answer:** **23 bits**.

---

Problem 3: 100-Character Message Expected Length (GATE PYQ)

A message is made up entirely of characters from the set $X = \{P, Q, R, S, T\}$ with probabilities: $P = 0.22, Q = 0.34, R = 0.17, S = 0.19, T = 0.08$. If a message of **100 characters** is encoded using Huffman coding, calculate the expected length of the encoded message in bits.

Step-by-Step Solution:

1. **Merge Step-by-Step:**
    - Min-Heap Pool: $T(0.08), R(0.17), S(0.19), P(0.22), Q(0.34)$.
    - Merge $T(0.08) + R(0.17) \to N_1(0.25)$.
    - Pool: $S(0.19), P(0.22), N_1(0.25), Q(0.34)$.
    - Merge $S(0.19) + P(0.22) \to N_2(0.41)$.
    - Pool: $N_1(0.25), Q(0.34), N_2(0.41)$.
    - Merge $N_1(0.25) + Q(0.34) \to N_3(0.59)$.
    - Merge $N_2(0.41) + N_3(0.59) \to \text{Root}(1.00)$.
2. **Determine Code Lengths:**
    - $P$: 2 bits, $S$: 2 bits, $Q$: 2 bits.
    - $R$: 3 bits, $T$: 3 bits.
3. **Calculate Average Bits/Character:** $$\text{Avg Bits} = (0.34 \times 2) + (0.22 \times 2) + (0.19 \times 2) + (0.17 \times 3) + (0.08 \times 3)$$ $$\text{Avg Bits} = 0.68 + 0.44 + 0.38 + 0.51 + 0.24 = \mathbf{2.25 \text{ bits/character}}$$
4. **Calculate Total Bits for 100 Characters:** $$\text{Total Bits} = 100 \times 2.25 = \mathbf{225 \text{ bits}}$$

**Answer:** **225 bits**.

---

Problem 4: Decoding Fibonacci-Based Huffman Code (GATE PYQ)

Characters $A$ through $H$ have frequencies based on the first 8 Fibonacci numbers: $A=1, B=1, C=2, D=3, E=5, F=8, G=13, H=21$. A Huffman code is assigned to represent these characters. What sequence of characters corresponds to the encoded binary string: `110111100111010`?

Step-by-Step Solution:

1. **Fibonacci Skewed Huffman Tree Property:** When frequencies follow Fibonacci numbers, merging sequentially creates a completely **left-skewed / right-skewed binary tree**.
2. **Prefix Code Assignment (Left=1, Right=0):**
    - $H(21) \implies 0$
    - $G(13) \implies 10$
    - $F(8) \implies 110$
    - $E(5) \implies 1110$
    - $D(3) \implies 11110$
    - $C(2) \implies 111110$
    - $B(1) \implies 1111101$
    - $A(1) \implies 1111100$
3. **Parse Bit Sequence** **110111100111010** **Left-to-Right:**
    - Match `110` $\to$ **F**
    - Match `11110` $\to$ **D**
    - Match `0` $\to$ **H**
    - Match `1110` $\to$ **E**
    - Match `10` $\to$ **G**

**Answer:** Decoded Sequence = **FDHEG**.
