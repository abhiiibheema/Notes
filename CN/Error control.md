Module 1: Fundamentals of Error Control & Error Types

1. Concept of Error

In data transmission, an **error** occurs when the transmitted binary sequence and the received binary sequence differ due to noise, attenuation, or interference in the communication channel.

- **Corrupted Bits**: Bits whose binary values flip during transmission ($0 \to 1$ or $1 \to 0$). The total number of bits in the frame remains constant.
- **Erased Bits**: Bits lost during transmission where the received bit count is strictly less than the transmitted bit count.

2. Classifications of Errors

3. **Single-Bit Error**: Exactly **one bit** in the received frame is corrupted.
4. **Burst Error (Multiple-Bit Error)**: **Two or more bits** in the received frame are corrupted.
    - **Burst Length (**$BL$**)**: The number of bits measured from the first corrupted bit to the last corrupted bit, inclusive.
    - **Fundamental Property**: In any burst error, the actual number of corrupted bits is always less than or equal to $BL$.

5. Error Detection vs. Error Correction

- **Error Detection**: The receiver identifies whether an error has occurred but cannot determine the exact position of the corrupted bits.
    - Recovery requires **retransmission** of the frame (e.g., Automatic Repeat reQuest - ARQ).
    - _Techniques_: Single-Bit Parity, 2D Parity, Checksum, Cyclic Redundancy Check (CRC).
- **Error Detection & Correction (Forward Error Correction - FEC)**: The receiver detects the presence of errors and pinpoints/fixes the corrupted bits without requesting retransmission.
    - Essential in high-latency links (e.g., satellite communications) where retransmission delays are prohibitive.
    - _Techniques_: Two-Dimensional (2D) Parity, Hamming Code.

---

Module 2: Block Coding Framework

In block coding, data is divided into fixed-size blocks and extended with redundant bits to form valid codewords.

1. Block Parameters

- **Data Block (**$m$ **bits)**: The original payload of length $m$ bits.
- **Redundant / Check Bits (**$r$ **bits)**: Extra bits added to enable detection or correction.
- **Codeword (**$N$ **bits)**: The transmitted block of size $N = m + r$: $$\text{Codeword} = [\text{Data bits } (m) \mid \text{Parity bits } (r)]$$ $$\text{Redundant bits } r = N - m$$

2. Space Statistics

- **Total Possible** $N$**-bit Combinations**: $2^N$
- **Valid Codewords**: $2^m$
- **Invalid Codewords**: $2^N - 2^m$

3. Decision Logic

- **Transmitter**: Maps every $m$-bit data word to a unique valid $N$-bit codeword.
- **Receiver**: Evaluates the received $N$-bit sequence against the set of valid codewords:
    - Sequence is a **Valid Codeword** $\implies$ **No error detected** (Accepted).
    - Sequence is an **Invalid Codeword** $\implies$ **Error detected** (Rejected/Discarded).

---

Module 3: Single-Bit Parity (1D Parity)

1. Mechanism

- Appends **1 parity bit** ($r = 1$) to $m$ data bits, resulting in a codeword length of $N = m + 1$.
- **Even Parity Scheme**:
    - _Transmitter_: Generates $P = d_1 \oplus d_2 \oplus \dots \oplus d_m$. $P$ is set such that the total count of $1$s in the $N$-bit block is even.
    - _Receiver_: Computes $d_1 \oplus d_2 \oplus \dots \oplus d_m \oplus P$. If the result is $0$ (even $1$s), no error is detected; if $1$ (odd $1$s), an error is detected.

2. Detection Performance

- **Detects 100% of single-bit errors**.
- **Detects 100% of burst errors with an ODD number of corrupted bits**.
- **Fails on EVEN numbers of corrupted bits**: An even number of bit flips preserves the parity balance, transforming one valid codeword into another valid codeword (undetected error).

---

Module 4: Two-Dimensional Parity (2D Parity)

1. Matrix Organization

Data bits are arranged in a grid of $(m-1)$ rows and $(n-1)$ columns. Parity bits are calculated across both dimensions to form an $m \times n$ matrix:

- **Data Bits**: $(m - 1) \times (n - 1)$
- **Parity Bits**: $m + n - 1$ (comprising $m-1$ row parities $R_i$, $n-1$ column parities $C_j$, and $1$ corner parity $P$)
- **Total Codeword Size**: $N = m \times n$ bits

2. Mathematical Calculations (Even Parity)

- **Row Parity (**$R_i$**)**: $R_i = d_{i,1} \oplus d_{i,2} \oplus \dots \oplus d_{i,n-1}$
- **Column Parity (**$C_j$**)**: $C_j = d_{1,j} \oplus d_{2,j} \oplus \dots \oplus d_{m-1,j}$
- **Overall Corner Parity (**$P$**)**: $P = R_1 \oplus R_2 \oplus \dots \oplus R_{m-1} = C_1 \oplus C_2 \oplus \dots \oplus C_{n-1}$

3. Error Detection & Correction Capabilities

- **1-Bit Error**: **Detected AND Corrected**. The intersection of the single failing row parity and single failing column parity isolates the exact bit coordinate to flip.
- **2-Bit Error**: **Detected, but CANNOT be corrected**.
- **3-Bit Error**: **Detected, but CANNOT be corrected**.
- **4-Bit Error**: **May go undetected**. If 4 corrupted bits form the 4 corners of a rectangle in the grid, all row and column parity counts remain even.
- **Guaranteed Detection Bound**: Always detects up to **3 arbitrary bit errors**.

4. Receiver Error Analysis Formula

To determine the minimum number of corrupted bits from a failing received matrix: $$n_{\min} = \max(\text{Unbalanced Rows}, \text{Unbalanced Columns})$$

---

Module 5: Checksum Schemes

Checksum divides data into $k$ blocks of $n$ bits each (e.g., $n = 16$ bits in IP/TCP/UDP headers).

1. Modulo-2 XOR Checksum Method

- **Sender**: Computes the bitwise XOR across all $k$ data blocks to generate the Checksum word.
- **Receiver**: Computes the bitwise XOR across all $k$ data blocks plus the received Checksum word.
    - Result $= 000\dots0 \implies$ **No error detected**.

2. One's Complement Sum Method (Standard Internet Checksum)

- **Sender Protocol**:
    1. Add all $k$ $n$-bit data blocks using **One's Complement Addition** (wrap around any carry bit beyond $n$ bits and add it to the LSB).
    2. Take the **1's complement (bitwise NOT)** of the final sum to produce the Checksum.
- **Receiver Protocol**:
    1. Add all $k$ data blocks plus the received Checksum using One's Complement Addition.
    2. The sum must equal **all 1s** ($111\dots1$).
    3. Taking the 1's complement of this result yields **all 0s** ($000\dots0$), confirming valid data.

---

Module 6: Cyclic Redundancy Check (CRC)

1. Mathematical Notation & Polynomial Mapping

- **Data Block**: $m$ bits, represented by polynomial $M(X)$.
- **Generator Polynomial**: $G(X)$ of degree $n$ (length $= n + 1$ bits).
- **CRC Checkbits**: $n$ bits, where $n = \text{degree}[G(X)]$.

2. Transmitter Algorithm

3. Shift the message polynomial by $n$ bits: $\text{Dividend} = M(X) \cdot X^n$ (appends $n$ zero bits to the payload).
4. Divide $M(X) \cdot X^n$ by $G(X)$ using **Modulo-2 Binary Division** (XOR operations without borrow/carry).
5. The resulting remainder polynomial $R(X)$ of degree $< n$ is the **CRC**.
6. **Transmitted Codeword**: $TD(X) = M(X) \cdot X^n + R(X)$.

7. Receiver Algorithm & Algebraic Error Analysis

- **Received Signal**: $RD(X) = TD(X) + E(X)$, where $E(X)$ is the **Error Polynomial**.
- **Receiver Division**: $$\frac{RD(X)}{G(X)} = \frac{TD(X)}{G(X)} + \frac{E(X)}{G(X)}$$
- Since $TD(X)$ is constructed to be perfectly divisible by $G(X)$, the remainder depends strictly on $E(X)$:
    - $\frac{E(X)}{G(X)} \neq 0 \implies$ **Error detected**.
    - $\frac{E(X)}{G(X)} = 0 \implies$ **Error goes undetected** (occurs when $E(X)$ is a multiple of $G(X)$).

4. Detection Power of Generator Polynomial $G(X)$

5. **Single-Bit Error** ($E(X) = X^i$): Detected if $G(X)$ contains two or more terms (i.e., $G(X) = X^n + \dots + 1$).
6. **Two-Bit Error** ($E(X) = X^i + X^j = X^j(1 + X^{i-j})$): Detected if $G(X)$ does not divide $(1 + X^k)$ for any $k \le \text{frame length}$.
7. **Odd Number of Error Bits**: Always detected if $(X + 1)$ is a factor of $G(X)$.
8. **Burst Errors**:
9. **100% detection** for any burst length $BL \le n$ (where $n = \text{degree}[G(X)]$).
10. For $BL = n + 1$, undetected error fraction $= (1/2)^{n-1}$.
11. For $BL > n + 1$, undetected error fraction $= (1/2)^n$.

---

Module 7: Hamming Code

Hamming Code is a single-bit error-correcting Forward Error Correction (FEC) scheme using parity check bits placed at specific bit positions.

1. Position Assignment & Parity Coverage

- **Parity Bit Coordinates**: Parity check bits $R_i$ are placed at position indexes corresponding to powers of 2 ($2^i \implies$ positions $1, 2, 4, 8, 16, \dots$).
- **Data Bit Coordinates**: Data bits $d_j$ fill all remaining non-power-of-2 bit positions.

2. Structure of 7-Bit Hamming Code ($m = 4$ Data Bits, $r = 3$ Parity Bits)

- **Bit Position Mapping (Position 1 to 7, LSb to MSb)**:
    - Pos 1 ($2^0$): $R_0$
    - Pos 2 ($2^1$): $R_1$
    - Pos 3: $d_0$
    - Pos 4 ($2^2$): $R_2$
    - Pos 5: $d_1$
    - Pos 6: $d_2$
    - Pos 7: $d_3$
- **Parity Bit Formulas (Even Parity)**: Each parity bit checks positions whose binary representation has a $1$ in its corresponding bit column:
    - $R_0$ checks positions $\{1, 3, 5, 7\} \implies R_0 = d_0 \oplus d_1 \oplus d_3$
    - $R_1$ checks positions $\{2, 3, 6, 7\} \implies R_1 = d_0 \oplus d_2 \oplus d_3$
    - $R_2$ checks positions $\{4, 5, 6, 7\} \implies R_2 = d_1 \oplus d_2 \oplus d_3$
- **Syndrome Word & Error Correction at Receiver**: The receiver recalculates the check bits to form the syndrome binary number $S = S_2 S_1 S_0$:
    - $S_0 = R_0 \oplus d_0 \oplus d_1 \oplus d_3$
    - $S_1 = R_1 \oplus d_0 \oplus d_2 \oplus d_3$
    - $S_2 = R_2 \oplus d_1 \oplus d_2 \oplus d_3$
    - If $S = 000 \implies$ No error.
    - If $S \neq 000 \implies$ The decimal value of $S$ identifies the **exact bit position** that failed.

---
1. Hamming Code Framework & Redundancy Formulas

In block coding, an original data payload of $m$ **bits** is extended by appending $r$ **redundant/check bits** to form an $N$**-bit codeword** ($N = m + r$)2.

- **Hamming Redundancy Inequality**: To enable single-bit error correction for $m$ data bits using $r$ check bits, the number of parity bits must satisfy34: $$2^r \ge m + r + 1$$ _(For example,_ $m = 4$ _data bits requires_ $r = 3$ _parity bits, since_ $2^3 = 8 \ge 4 + 3 + 1 = 8$_, yielding a 7-bit Hamming code_4_.)_
- **Parity Bit Placement Rule**: Parity check bits ($R_i$) are placed at position coordinates corresponding to powers of 2 ($2^i \implies$ positions $1, 2, 4, 8, \dots$)3. Data bits fill all remaining positions4.

7-Bit Hamming Code Bit Layout (4 Data Bits, 3 Parity Bits)

From position 1 (LSb) to position 7 (MSb)4:

- **Pos 1 (**$2^0$**)**: Parity Bit $R_0$4
- **Pos 2 (**$2^1$**)**: Parity Bit $R_1$4
- **Pos 3**: Data Bit $d_0$4
- **Pos 4 (**$2^2$**)**: Parity Bit $R_2$4
- **Pos 5**: Data Bit $d_1$4
- **Pos 6**: Data Bit $d_2$4
- **Pos 7**: Data Bit $d_3$4

Parity Calculation Formulas (Even Parity)

Each parity bit checks all bit positions whose binary index has a $1$ in the corresponding bit column5:

- $R_0$ checks positions $\{1, 3, 5, 7\} \implies R_0 = d_0 \oplus d_1 \oplus d_3$5
- $R_1$ checks positions $\{2, 3, 6, 7\} \implies R_1 = d_0 \oplus d_2 \oplus d_3$5
- $R_2$ checks positions $\{4, 5, 6, 7\} \implies R_2 = d_1 \oplus d_2 \oplus d_3$5

---

2. Hamming Distance ($d(x, y)$) & Minimum Distance ($d_{\min}$)

While your sources detail the construction of Hamming Code, the mathematical distance metrics that justify why these codes work are defined as follows:

Definition of Hamming Distance

The **Hamming Distance** $d(x, y)$ between two equal-length binary sequences $x$ and $y$ is the **number of bit positions in which the two sequences differ**.

- **Formula**: $$d(x, y) = \text{weight}(x \oplus y)$$ _(where_ $\oplus$ _represents the bitwise XOR operation, and_ $\text{weight}(\cdot)$ _counts the total number of_ $1$_s in the result)._
- **Calculation Example**: If $x = 1100001$ and $y = 1101001$: $$x \oplus y = 0001000 \implies \text{Count of 1s} = 1 \implies d(x, y) = 1$$

Minimum Hamming Distance ($d_{\min}$)

The **Minimum Hamming Distance** $d_{\min}$ of a block code is the smallest Hamming distance between any pair of distinct valid codewords in the code set: $$d_{\min} = \min \{ d(c_i, c_j) \mid c_i \neq c_j \}$$

---

3. Core Formulas for Error Detection and Correction

The value of $d_{\min}$ determines the capability of a block code to detect or correct transmission errors:

1. **To Detect up to** $t$ **bit errors**: $$d_{\min} \ge t + 1$$ _To detect single-bit errors (_$t = 1$_), a minimum distance of_ $d_{\min} \ge 2$ _is required (e.g., Single-Bit Parity Code)._
2. **To Correct up to** $t$ **bit errors**: $$d_{\min} \ge 2t + 1$$ _To correct single-bit errors (_$t = 1$_), a minimum distance of_ $d_{\min} \ge 3$ _is required (e.g., Hamming Code)._
3. **To Detect** $s$ **errors AND Correct** $t$ **errors simultaneously (**$s > t$**)**: $$d_{\min} \ge s + t + 1$$

---

🎯 Would you like to solve a numerical problem applying Hamming Code encoding and syndrome decoding step-by-step, or generate a **Practice Quiz** on Error Control formulas?

---

Module 9: Step-by-Step Practice Problems

Example 1: 2D Parity Minimum Corrupted Bits Calculation

- **Problem**: A received $4 \times 7$ 2D parity matrix reveals **1 unbalanced row** and **3 unbalanced columns**. Determine the minimum number of corrupted bits $n_{\min}$.
- **Solution**: $$n_{\min} = \max(\text{Unbalanced Rows}, \text{Unbalanced Columns}) = \max(1, 3) = 3$$
- **Result**: `3`

---

Example 2: CRC Codeword Generation

- **Problem**: Message $M = 11001001$, Generator polynomial $G(X) = X^3 + 1$ (Divisor `1001`, degree $n = 3$). Find the complete transmitted codeword.
- **Solution**:
    1. Append $3$ zero bits to $M$: $\text{Dividend} = 11001001000$.
    2. Perform Modulo-2 division $11001001000 \div 1001$:
        - $1100 \oplus 1001 = 101$
        - Bring down $1 \to 1011 \oplus 1001 = 010$
        - Bring down $0 \to 100 \oplus 000 = 100$
        - Bring down $0 \to 1000 \oplus 1001 = 001$
        - Bring down $0 \to 010 \oplus 000 = 010$
        - Bring down $0 \to 1000 \oplus 1001 = 011$
        - $\text{Remainder } R(X) = 011$.
    3. Append remainder to payload: Transmitted Codeword $= 11001001011$.
- **Result**: `11001001011`

---

Example 3: CRC Checkbits Sequence

- **Problem**: Message $M = 11000$, Generator $G(X) = X^3 + X + 1$ (Divisor `1011`, degree $n = 3$). Find checkbits $c_2 c_1 c_0$.
- **Solution**:
    1. Extended dividend $= 11000000$.
    2. Modulo-2 division $11000000 \div 1011$:
        - $1100 \oplus 1011 = 111$
        - Bring down $0 \to 1110 \oplus 1011 = 101$
        - Bring down $0 \to 1010 \oplus 1011 = 001$
        - Bring down $0 \to 0100 \oplus 0000 = 100$
        - $\text{Remainder } c_2 c_1 c_0 = 100$.
- **Result**: `100`

---

Example 4: Hamming Code Encoding

- **Problem**: Encode 4-bit data word `'1100'` ($d_3=1, d_2=1, d_1=0, d_0=0$) using 7-bit Even Parity Hamming Code.
- **Solution**:
    - $R_0 = d_0 \oplus d_1 \oplus d_3 = 0 \oplus 0 \oplus 1 = 1$
    - $R_1 = d_0 \oplus d_2 \oplus d_3 = 0 \oplus 1 \oplus 1 = 0$
    - $R_2 = d_1 \oplus d_2 \oplus d_3 = 0 \oplus 1 \oplus 1 = 0$
    - Transmitted Sequence (Pos 7 to Pos 1): $d_3 d_2 d_1 R_2 d_0 R_1 R_0 = 1 1 0 0 0 0 1$.
- **Result**: `1100001`

**Category 1: Routing & Network Functionality (GATE Questions)**

1. **[MCQ] [GATE-2011] [2-Mark]**1
    - **Question**: Consider a network with five nodes, N1 to N5, as shown1. The network uses a Distance Vector Routing protocol. Once the routes have stabilized, the distance vectors at different nodes are as following1:
        - $N_1: (0, 1, 7, 8, 4)$
        - $N_2: (1, 0, 6, 7, 3)$
        - $N_3: (7, 6, 0, 2, 6)$
        - $N_4: (8, 7, 2, 0, 4)$
        - $N_5: (4, 3, 6, 4, 0)$ Each distance vector is the distance of the best known path at that instance to nodes N1 to N5, where the distance to itself is 01. All links are symmetric and the cost is identical in both directions1. In each round, all nodes exchange their distance vectors with their respective neighbors, then all nodes update their distance vectors1.
2. **[MCQ] [GATE-2011] [2-Mark] (Part 2)**6
    - **Question**: Consider the same data as given in the previous question6. After the update in the previous question, the link N1-N2 goes down6. N2 will reflect this change immediately in its distance vector as cost infinite6. After the NEXT ROUND of update, what will be the cost to N1 in the distance vector of N3?6
        - A) $3$
        - B) $9$
        - C) $10$
        - D) $\infty$
    - **Answer**: **10 (Option C)**6
3. **[MSQ] [GATE-2023] [1-Mark]**7
    - **Question**: Which of the following statements is/are **INCORRECT** about the OSPF (Open Shortest Path First) routing protocol used in the Internet?7
        - A) OSPF implements Bellman-Ford algorithm to find shortest paths.
        - B) OSPF uses Dijkstra’s shortest path algorithm to implement least-cost path routing.
        - C) OSPF is used as an inter-domain routing protocol.
        - D) OSPF implements hierarchical routing.
    - **Answer**: **A and C**7
4. **[MCQ] [GATE-2020] [1-Mark]**8
    - **Question**: Consider the following statements about the functionality of an IP-based router:8
        - I. A router does not modify the IP packets during forwarding.
        - II. It is not necessary for a router to implement any routing protocol.
        - III. A router should reassemble IP fragments if the MTU of the outgoing link is larger than the size of the incoming IP packet. Which of the above statements is/are **TRUE**?8
        - A) I and II only
        - B) I only
        - C) II and III only
        - D) II only
    - **Answer**: **II only (Option D)**8

---
## Category 2: Two-Dimensional (2D) Parity

### 1. In-Class Example (NAT)

**Question:** A two-dimensional even-parity scheme is used for error detection.

- Block size: 4 × 6 (including parity bits)
- Data bits: 15 (a 3 × 5 data block)
- The receiver gets the matrix below and knows that _n_ bits are corrupted.

What is the minimum possible value of _n_?

$$  
\begin{array}{ccccc|c}  
1 & 0 & 1 & 1 & 0 & 1 \\  
0 & 1 & 1 & 1 & 1 & 1 \\  
0 & 0 & 1 & 0 & 1 & 1 \\  
\hline  
0 & 1 & 0 & 0 & 0 & 1  
\end{array}  
$$

**Solution:**

- Rows with odd parity (violations): row 2 (five 1s) and row 3 (three 1s). That is 2 rows.
- Columns with odd parity (violations): column 1 (one 1) and column 3 (three 1s). That is 2 columns.
- Each corrupted bit flips one row and one column, so 2 bad rows and 2 bad columns can be explained by 2 errors placed at the intersections, for example (row 2, column 1) and (row 3, column 3).

**Answer:** $\boxed{2}$

---

### 2. [MCQ] [GATE-IT-2008] [2 Marks]

**Question:** Data on a link uses the following 2D parity scheme.

- Each sequence of 28 bits is arranged in a 4 × 7 matrix.
- Rows are $r_0$ to $r_3$ and columns are $d_7$ to $d_1$.
- A parity column $d_0$ and a parity row $r_4$ are added using **even parity**.
- Each bit of $d_0$ is the parity of its row, and each bit of $r_4$ is the parity of its column.
- In total, 40 bits are transmitted.

The receiver gets the table below, which has _n_ corrupted bits. What is the minimum possible value of _n_?

$$  
\begin{array}{c|ccccccc|c}  
& d_7 & d_6 & d_5 & d_4 & d_3 & d_2 & d_1 & d_0 \\  
\hline  
r_0 & 0 & 1 & 0 & 1 & 0 & 0 & 1 & 1 \\  
r_1 & 1 & 1 & 0 & 0 & 1 & 1 & 1 & 0 \\  
r_2 & 0 & 0 & 0 & 1 & 0 & 1 & 0 & 0 \\  
r_3 & 0 & 1 & 1 & 0 & 1 & 0 & 1 & 0 \\  
\hline  
r_4 & 1 & 1 & 0 & 0 & 0 & 1 & 1 & 0  
\end{array}  
$$

**Options:**

- A) 1
- B) 2
- C) 3
- D) 4

**Solution:**

- Rows with odd parity: only $r_1$ (five 1s). That is 1 row.
- Columns with odd parity: $d_5$, $d_2$ and $d_0$. That is 3 columns.
- One error can fix only one column, so at least 3 errors are needed. Three errors, all in row $r_1$ at columns $d_5$, $d_2$ and $d_0$, flip the row's parity three times (odd), which matches the single bad row.

**Answer:** $\boxed{3}$, **Option C**
---

**Category 3: Cyclic Redundancy Check (CRC)**

1. **In-Class Example 1**210
    - **Question**: Given Generator polynomial $G(X) = X^3 + X^2 + 1$ and DATA = `10100110` (8 bits)2. Calculate the CRC check bits (3 bits)210.
    - **Answer**: **011**
2. **In-Class Example 2**11
    - **Question**: Given Generator polynomial $G(X) = X^3 + X + 1$ and DATA = `10100101`11. Calculate the 3-bit CRC check bits11.
    - **Answer**: **101**11
3. **In-Class Example 3**12
    - **Question**: Given Generator polynomial $G(X) = X^4 + X + 1$ and DATA = `10100101`12. Calculate the 4-bit CRC check bits12.
    - **Answer**: **1011**
4. **In-Class Example 5**
    - **Question**: Given Generator polynomial $G(X) = X + 1$ and Message (DATA) = `1011101`13. Find the Transmitted Data13.
    - **Answer**: **10111011** _(One-bit CRC with_ $G(X) = X + 1$ _is identical to single-bit even parity)_1314.
5. **In-Class Practice Conceptual Problems**15
    - **Problem A**: Find CRC when $G(X) = X^3 + 1$ and Data = `10011001`15.
    - **Problem B**: Find CRC when $G(X) = X^3 + X^2 + X + 1$ and Data = `11110001`15.
6. **In-Class Homework Problem (Burst Length Analysis)**1617
    - **Question**: Given $G(X) = X^3 + 1$, Transmitted data = `10100101011`, and Error Polynomial $E(X) = X^8 + X^5$16. Determine the Received Data and whether the receiver detects the error16.
    - **Answer**: Received Data = `10000001011`18. Since $BL = (8 - 5 + 1) = 4 > \text{degree}[G(X)] = 3$, $E(X) = X^5(X^3 + 1)$ is perfectly divisible by $G(X)$, so **No error detected**1719.
7. **[MCQ] [GATE-IT-2005] [2-Mark]**320
    - **Question**: Consider the message $M = 1010001101$20. The cyclic redundancy check (CRC) for this message using the divisor polynomial $X^5 + X^4 + X^2 + 1$ is:20
        - A) $01110$
        - B) $01011$
        - C) $10101$
        - D) $10110$
    - **Answer**: **01110 (Option A)**3
8. **[MCQ] [GATE-2026, Set-2] [2-Mark]**2122
    - **Question**: Consider the transmission of data bits `110001011` over a link that uses Cyclic Redundancy Check (CRC) code for error detection21. If the generator bit pattern is given to be `1001`, which one of the following options shows the remainder bit pattern appended to the data bits before transmission?21
        - A) $011$
        - B) $101$
        - C) $000$
        - D) $100$
    - **Answer**: **100 (Option D)**22
9. **[MCQ] [GATE-CS-2007] [2-Mark]**23
    - **Question**: The message `11001001` is to be transmitted using the CRC polynomial $X^3 + 1$ to protect it from errors23. The message that should be transmitted is:23
        - A) $11001001000$
        - B) $11001001011$
        - C) $11001010$
        - D) $110010010011$
    - **Answer**: **11001001011 (Option B)**23
10. **[MCQ] [GATE-2017, Set-1] [2-Mark]**2425
    - **Question**: A computer network uses polynomials over GF(2) for error checking with 8 bits as information bits and uses $X^3 + X + 1$ as the generator polynomial to generate the check bits24. In this network, the message `01011011` is transmitted as:24
        - A) $01011011010$
        - B) $01011011011$
        - C) $01011011101$
        - D) $01011011100$
    - **Answer**: **01011011101 (Option C)**25
11. **[MCQ] [GATE-2021, Set-2] [2-Mark]**2627
    - **Question**: Consider the cyclic redundancy check (CRC) based error detecting scheme having the generator polynomial $X^3 + X + 1$26. Suppose the message $m_4 m_3 m_2 m_1 m_0 = 11000$ is to be transmitted26. Check bits $c_2 c_1 c_0$ are appended at the end of the message by the transmitter using the above CRC scheme26. The transmitted bit string is denoted by $m_4 m_3 m_2 m_1 m_0 c_2 c_1 c_0$26. The value of the checkbit sequence $c_2 c_1 c_0$ is:26
        - A) $101$
        - B) $110$
        - C) $100$
        - D) $111$
    - **Answer**: **100 (Option C)**27
12. **[MCQ] [GATE-2009] [2-Mark]**28
    - **Question**: Let $G(X)$ be the generator polynomial used for CRC checking28. What is the condition that should be satisfied by $G(X)$ to detect odd number of bits in error?28
        - A) $G(X)$ contains more than two terms
        - B) $G(X)$ does not divide $1 + X^k$, for any $k$ not exceeding the frame length
        - C) $(1 + X)$ is a factor of $G(X)$
        - D) $G(X)$ has an odd number of terms
    - **Answer**: **(1 + X) is a factor of G(X) (Option C)**28

---

**Category 4: Checksum & Parity Word**

1. **In-Class Example 1 (Parity Word Checksum)**4
    - **Question**: Given Block Size = 5 bits, Number of blocks = 64. Sender data: `10101`, `01011`, `11010`, `10110`, `01011`, `00000`4. Calculate the Checksum (Parity Word using Modulo-2 XOR arithmetic)4.
    - **Answer**: Checksum = `11001`4. Transmitted Data = `10101 01011 11010 10110 01011 11001`29.
2. **In-Class Example 2 (One's Complement Checksum)**3031
    - **Question**: Given Block Size = 5 bits, Number of blocks = 630. Sender data: `10101`, `01011`, `11010`, `10110`, `01011`, `00000`30. Calculate One's Complement Sum and final Checksum30.
    - **Answer**: 1's Complement Sum = `11101`, Checksum = `00010`30.
3. **In-Class Example 3 (One's Complement Checksum with Carry)**3233
    - **Question**: Given Block Size = 5 bits, Number of blocks = 632. Sender data: `01101`, `11110`, `11011`, `10101`, `11011`, `00000`32. Calculate One's Complement Sum (wrapping carry) and final Checksum32.
    - **Answer**: Initial sum = `1110110` $\to$ Wrap 2-bit carry: `11100 + 11 = 11001`3234. Checksum = `00110`32.

---

**Category 5: Hamming Code**

1. **In-Class Example 1 (Hamming Code Encoding)**35
    - **Question**: Consider hamming code error control technique which uses 'even parity' and first parity bit placed at least significant bit position35. The four bits data `'1100'` (placed from most significant bit to least significant bit position) encoded as:35
    - **Answer**: **1100001**35
2. **In-Class Example 2 (Homework - Hamming Code Encoding)**36
    - **Question**: Consider hamming code error control technique which uses 'even parity' and first parity bit placed at least significant bit position36. The four bits data `'0111'` (placed from most significant bit to least significant bit position) encoded as:36
    - **Answer**:
        - Data bits: $d_3 = 0, d_2 = 1, d_1 = 1, d_0 = 1$3637.
        - $R_0 = d_0 \oplus d_1 \oplus d_3 = 1 \oplus 1 \oplus 0 = 0$37.
        - $R_1 = d_0 \oplus d_2 \oplus d_3 = 1 \oplus 1 \oplus 0 = 0$37.
        - $R_2 = d_1 \oplus d_2 \oplus d_3 = 1 \oplus 1 \oplus 0 = 0$37.
        - Transmitted Codeword ($d_3 d_2 d_1 R_2 d_0 R_1 R_0$) = **0110100**38