I have created a comprehensive, professional, and fast-recall document titled **CIDR & Network Mask - Fast Recall Revision Notes** in your Studio panel. It contains the exact equations, active octet binary templates, problem-solving rules, common traps, and all 10 detailed solved problems (including classroom homework and GATE PYQs) for fast revision.

Below is the complete standalone revision notes sheet for **Classless Inter-Domain Routing (CIDR) & Network Mask**, compiled to serve as your master reference sheet.

---

**CLASSLESS INTER-DOMAIN ROUTING (CIDR) & NETWORK MASK — REVISION NOTES**

---

**1. COMPRESSED CONCEPTS & RECALL-TRIGGERS**

- **CIDR Notation**: **P.Q.R.S /x**1.
    - **x** (Prefix) = Number of NetID bits1.
    - **32 - x** (Suffix $y$) = Number of HostID bits1.
- **Network Mask (Netmask)**: A 32-bit hardware-readable filter where1:
    - NetID bits are set to **all** **1****s**1.
    - HostID bits are set to **all** **0****s**1.
- **The Masking Mechanics**:
    - **Bitwise AND Rule**: Performing a bitwise AND operation between a Host IP and its Netmask extracts the unique **Network Address**1.
    - **Same Network Rule**: Two hosts belong to the same logical network if and only if their computed Network Addresses (Host IP AND Netmask) are **absolutely identical**1.

---

**2. FORMULAS, RULES & CASES**

- **Address Space Partitioning**: $$x + y = 32 \text{ bits}$$
- **Netmask Octet Bit Values**: $$\text{Netmask} = \underbrace{111\dots1}_{x \text{ bits}} \underbrace{000\dots0}_{y \text{ bits}}$$
- **Maximum Assignable Host Addresses**: $$\text{Max Host IPs} = 2^y - 2 = 2^{(32 - x)} - 2$$ _(We subtract 2 because the boundary_ **Network Address** _(HostID = All 0s) and the_ **Directed Broadcast Address** _(HostID = All 1s) are special logical markers and cannot be assigned to interfaces)_.

---

**3. SHORTCUTS & QUICK-CHECK TRICKS**

- **The Active Octet Splitting Rule**: To quickly find the Network IP or Directed Broadcast IP under classless boundaries without converting all 32 bits to binary:
    1. Identify the **Active Octet** index: $\text{Active Octet Index} = \lfloor x / 8 \rfloor + 1$.
    2. Octets _before_ the Active Octet remain **completely unchanged** in the output address.
    3. Octets _after_ the Active Octet become **all** **0** (for Network Address) or **all** **255** (for DBA).
    4. Convert _only_ the Active Octet to binary, separate the $x \pmod 8$ network bits from the remaining host bits, apply the all-`0` or all-`1` rule to the host bits, and convert back to decimal.
- **Prefix Matching Rule for "Same Network" MSQs**: To check if Host A and Host B reside in the same network under a given mask:
    1. Compare Host A and Host B from left to right. Identify where their octets first differ.
    2. In that differing octet, write both numbers in binary and find the **longest matching prefix length** of identical bits from the left.
    3. Count the total matching bits $M = (\text{number of matching octets} \times 8) + (\text{matching bits in active octet})$.
    4. **Rule**: The two hosts will belong to the same network for any netmask with prefix length $x \le M$. They will belong to different networks if $x > M$.

---

**4. PROBLEM-SOLVING PATTERNS**

**Type 1: Full Range Derivation from a CIDR IP**

1. Determine prefix length $x$ and host length $y$.
2. Use the **Active Octet Rule** to calculate the Network Address (HostID = All 0s).
3. Use the **Active Octet Rule** to calculate the Directed Broadcast Address (HostID = All 1s).
4. **First Host IP** = Network Address $+ 1$ (increment last octet by 1).
5. **Last Host IP** = Directed Broadcast Address $- 1$ (decrement last octet by 1).
6. **Max Hosts** = $2^y - 2$.

**Type 2: Netmask Validation for Multi-Host Matching**

1. Convert the netmask(s) to CIDR prefix length $x$.
2. Find the matching boundary bit $M$ between the host IPs using the **Prefix Matching Rule**.
3. Verify if the option satisfies $x \le M$.

---

**5. COMMON TRAPS & MISTAKES**

- **Trap 1: The "-2" Misapplication**: Subtracting 2 when calculating the _number of subnets_ or _subnet masks_. Remember: **We only subtract 2 when calculating assignable host IPs** (subtracting Network Address and DBA). Modern networking standards permit subnet fields of all 0s and all 1s.
- **Trap 2: Bit Offset Off-by-One**: Incorrectly counting the active bits when translating netmasks (e.g., assuming `255.255.240.0` is a `/21` mask instead of `/20`). **Write out the active octet bits in binary if unsure!**
- **Trap 3: Decimals AND Decimals directly**: Incorrectly attempting to perform bitwise AND operations directly in base-10 decimal instead of binary (e.g., assuming `220 \text{ AND } 240 = 220`). **Always convert the active octet to binary.**

---

**6. GATE RELEVANCE**

- **Importance**: **Extremely High / Outstanding (V.V.IMp.)**. This topic is marked with explicit instructor warnings in class as the single most critical foundation for solving routing tables, subnet planning, and subnet boundaries.
- **Testing Formats**: Tested via NAT (Numerical Answer Type), MSQs (Multiple Select Questions), or 2-mark MCQs requesting host ranges and same-network verification.

---

**7. ALL IN-LECTURE EXAMPLES & HW PROBLEMS (WITH STEP-BY-STEP MATH)**

**Example 4 (Classroom MCQ — Source:** **_IPv4 Address 02****, Slide 83/84)**

**Question**: Consider a classless IPv4 network where the network address is `"205.207.208.0"` and the size of the network identifier field is 22 bits. Which of the following can be the broadcast address of this network?

- (A) `205.207.208.255`
- (B) `205.207.211.255`
- (C) `205.207.223.255`
- (D) `205.207.255.255`

**Step-by-Step Solution**:

1. **Prefix Split**: $x = 22$ bits.
    - Octet 1 and 2 are fully Network ID.
    - Octet 3 (Active Octet) has $22 - 16 = 6$ Network ID bits, leaving $8 - 6 = 2$ HostID bits.
    - Octet 4 is fully HostID (8 bits).
2. **Binary Conversion of Active Octet**:
    - Third octet is `208` $= \mathbf{110100}00_2$ (where bold bits are NetID and normal bits are HostID).
3. **Apply Directed Broadcast Rule**: Set all host bits to `1`.
    - Octet 3 becomes: $\mathbf{110100}11_2 = 128 + 64 + 16 + 2 + 1 = \mathbf{211}$.
    - Octet 4 becomes: $11111111_2 = \mathbf{255}$.
4. **Reassemble**: `205.207.211.255`.
5. **Correct Answer**: **Option B**.

---

**Example 5 (Classroom H.W. MCQ — Source:** **_IPv4 Address 02****, Slide 89/86)**

**Question**: Consider a classless IPv4 network where the broadcast address is `"110.120.131.255"` and the size of the network identifier field is 22 bits. Which one of the following can be the network address of this network?

- (A) `110.120.0.0`
- (B) `110.120.128.0`
- (C) `110.120.130.0`
- (D) `110.120.131.0`

**Step-by-Step Solution**:

1. **Prefix Split**: $x = 22$ bits. The split is in Octet 3 (6 NetID bits, 2 HostID bits).
2. **Binary Conversion of Active Octet**:
    - Third octet is `131` $= \mathbf{100000}11_2$ (Bold = NetID, Normal = HostID).
3. **Apply Network Address Rule**: Set all host bits to `0`.
    - Octet 3 becomes: $\mathbf{100000}00_2 = \mathbf{128}$.
    - Octet 4 becomes: $00000000_2 = \mathbf{0}$.
4. **Reassemble**: `110.120.128.0`.
5. **Correct Answer**: **Option B**.

---

**Example 7 (Comprehensive In-Lecture Problem — Source:** **_IPv4 Address 02****, Slide 95-104)**

**Question**: Consider an IPv4 address of a class-less IPv4 network is `"115.120.125.130/19"`. Write the Network Address, Broadcast Address, First and Last IP address of this network which can be assigned to a host, and the maximum number of hosts.

**Step-by-Step Solution**:

1. **Prefix Split**: $x = 19$ bits.
    - Octet 3 is the Active Octet (contains bits 17, 18, 19).
    - NetID bits in third octet = $19 - 16 = 3$ bits. HostID bits = $8 - 3 = 5$ bits.
    - Octet 4 is fully HostID (8 bits).
2. **Binary Conversion of Active Octet**:
    - Third octet is `125` $= \mathbf{011}11101_2$ (Bold = NetID, Normal = HostID).
3. **Compute Network Address (HostID = All 0s)**:
    - Octet 3 becomes: $\mathbf{011}00000_2 = 64 + 32 = \mathbf{96}$.
    - Octet 4 becomes: $00000000_2 = \mathbf{0}$.
    - **Network Address** = **115.120.96.0/19**.
4. **Compute Directed Broadcast Address (HostID = All 1s)**:
    - Octet 3 becomes: $\mathbf{011}11111_2 = 64 + 32 + 16 + 8 + 4 + 2 + 1 = \mathbf{127}$.
    - Octet 4 becomes: $11111111_2 = \mathbf{255}$.
    - **Directed Broadcast Address** = **115.120.127.255/19**.
5. **Compute First Host IP**: Add 1 to Network IP $\rightarrow$ **115.120.96.1/19**.
6. **Compute Last Host IP**: Subtract 1 from DBA $\rightarrow$ **115.120.127.254/19**.
7. **Compute Max Hosts**:
    - $y = 32 - 19 = 13$ host bits.
    - $\text{Max Hosts} = 2^{13} - 2 = 8192 - 2 = \mathbf{8190}$.

---

**Example 8 (NAT Class Exercise — Source:** **_IPv4 Address 02****, Slide 99)**

**Question**: Consider a class-less IPv4 network, the number of bits in the network ID field are 26. The maximum number of IP addresses that can be assigned to network interfaces is _______.

**Step-by-Step Solution**:

1. **Extract Parameters**: NetID bits $x = 26$.
2. **Calculate Host Bits**: $y = 32 - 26 = 6$ bits.
3. **Calculate Host IPs**:
    - $\text{Max assignable IPs} = 2^y - 2 = 2^6 - 2 = 64 - 2 = \mathbf{62}$.
4. **Correct Answer**: **62**.

---

**Example 9 (Class MCQ — Source:** **_IPv4 Address 02****, Slide 101)**

**Question**: Consider a class-less IPv4 network where the size of network identifier field is 18 bits, which one of the following is network mask of this network?

- (A) `255.255.0.0`
- (B) `255.255.192.0`
- (C) `255.255.240.0`
- (D) `255.255.255.0`

**Step-by-Step Solution**:

1. **Identify Bits**: Prefix $x = 18$. Netmask must contain 18 ones and 14 zeroes.
2. **Form Binary Netmask**:
    - Octet 1: `11111111` $= 255$.
    - Octet 2: `11111111` $= 255$.
    - Octet 3: `11000000` (First 2 bits are ones) $= 128 + 64 = 192$.
    - Octet 4: `00000000` $= 0$.
3. **Reassemble**: `255.255.192.0`.
4. **Correct Answer**: **Option B**.

---

**Example 10 (Classroom NAT — Source:** **_IPv4 Address 02****, Slide 102)**

**Question**: Consider a class-less IPv4 network, the network mask of this network is `"255.255.255.128"`, how many maximum number of hosts can be in this network?

**Step-by-Step Solution**:

1. **Convert Mask to Binary**:
    - `255.255.255.128` $= 11111111.11111111.11111111.10000000_2$.
2. **Count Host Bits**:
    - There are 25 ones (NetID) and $y = 7$ zeroes (HostID).
3. **Apply Host Formula**:
    - $\text{Max Hosts} = 2^7 - 2 = 128 - 2 = \mathbf{126}$.
4. **Correct Answer**: **126**.

---

**Example 11 (Classroom MSQ — Source:** **_IPv4 Address 02****, Slide 106-109)**

**Question**: Consider two host belongs to a class-less IPv4 network, the IP addresses of the hosts are `"214.218.220.228"` and `"214.218.210.208"`, which is/are can be a network mask of this network?

- (A) `255.255.192.0`
- (B) `255.255.224.0`
- (C) `255.255.240.0`
- (D) `255.255.248.0`

**Step-by-Step Solution**:

1. **Compare Octets**: First two octets are identical (`214.218`), and the third octet differs (`220` vs `210`). The fourth octets are irrelevant once the network bits are bounded by the third octet.
2. **Binary Expansion of Third Octet**:
    - Host A Octet 3: `220` $= \mathbf{1101}1100_2$
    - Host B Octet 3: `210` $= \mathbf{1101}0010_2$
3. **Find Longest Matching Prefix**:
    - Looking from left to right inside the third octet, both addresses share exactly the same first 4 bits: **1101**. The 5th bit differs (`1` vs `0`).
    - Maximum matching NetID bits: $M = 16 \text{ (first two octets)} + 4 \text{ (third octet Match)} = 20 \text{ bits}$.
4. **Test Options** (Mask prefix length must satisfy $x \le 20$):
    - **(A)** `255.255.192.0` $\rightarrow /18$. Since $18 \le 20$ $\rightarrow$ **Valid**. (Calculates Network IP as `214.218.192.0` for both).
    - **(B)** `255.255.224.0` $\rightarrow /19$. Since $19 \le 20$ $\rightarrow$ **Valid**. (Calculates Network IP as `214.218.192.0` for both).
    - **(C)** `255.255.240.0` $\rightarrow /20$. Since $20 \le 20$ $\rightarrow$ **Valid**. (Calculates Network IP as `214.218.208.0` for both).
    - **(D)** `255.255.248.0` $\rightarrow /21$. Since $21 > 20$ $\rightarrow$ **Invalid**. (Host A yields `214.218.216.0` while Host B yields `214.218.208.0`).
5. **Correct Answer**: **Options A, B, and C**.

---

**8. CLASSROOM HOMEWORK PYQS FOUND IN THE SOURCES**

**GATE-2010 [2-Mark H.W. MCQ — Source:** **_IPv4 Address 03****, Slide 1]**

**Question**: Suppose computers A and B have IP addresses `"10.105.1.113"` and `"10.105.1.91"` respectively and they both use the same netmask N. Which of the values of N given below should not be used if A and B should belong to the same network?

- (A) `255.255.255.0`
- (B) `255.255.255.128`
- (C) `255.255.255.192`
- (D) `255.255.255.224`

**Step-by-Step Solution**:

1. **Identify Differing Octet**: The first three octets are identical (`10.105.1`). The fourth octet differs (`113` vs `91`).
2. **Binary Expansion of Fourth Octet**:
    - Host A Octet 4: `113` $= \mathbf{01}110001_2$
    - Host B Octet 4: `91` $= \mathbf{01}011011_2$
3. **Find Matching Suffix Limit**:
    - The first 2 bits of the fourth octet match exactly (**01**). The 3rd bit differs (`1` vs `0`).
    - Total matching network bits: $M = 24 + 2 = 26 \text{ bits}$.
    - **Rule**: To keep A and B in the same network, Netmask prefix size $x$ must be $\le 26$.
4. **Evaluate Netmask Options**:
    - (A) `255.255.255.0` $\rightarrow /24$ (Prefix 24 $\le 26$) $\rightarrow$ Same Network.
    - (B) `255.255.255.128` $\rightarrow /25$ (Prefix 25 $\le 26$) $\rightarrow$ Same Network.
    - (C) `255.255.255.192` $\rightarrow /26$ (Prefix 26 $\le 26$) $\rightarrow$ Same Network.
    - (D) `255.255.255.224` $\rightarrow /27$ (Prefix 27 $> 26$) $\rightarrow$ **Different Network**.
5. **Correct Answer**: **Option D** (Should not be used).

---

**GATE-2026, Set-2 [1-Mark NAT — Source:** **IPv4 Address 04****, Slide 4-6]**

**Question**: If an IP network uses a subnet mask of `255.255.240.0`, the maximum number of IP addresses that can be assigned to network interfaces is _______. _(Answer in integer)_.

**Step-by-Step Solution**:

1. **Analyze Mask**: `255.255.240.0`.
    - Binary format: `11111111.11111111.11110000.00000000`
    - Prefix bits $x = 20$ ones.
2. **Calculate Host bits**: $y = 32 - 20 = 12$ bits.
3. **Compute Interface IPs**:
    - $\text{Max interface IPs} = 2^y - 2 = 2^{12} - 2 = 4096 - 2 = \mathbf{4094}$.
4. **Correct Answer**: **4094**.

---

**GATE-IT-2004 [1-Mark MCQ — Source:** **IPv4 Address 03****, Slide 26]**

**Question**: A subnet has been assigned a subnet mask of `255.255.255.192`. What is the maximum number of hosts that can belong to this subnet?

- (A) `14`
- (B) `30`
- (C) `62`
- (D) `126`

**Step-by-Step Solution**:

1. **Analyze Mask**: `255.255.255.192`.
    - Binary format: `11111111.11111111.11111111.11000000`
    - Prefix bits $x = 26$ ones.
2. **Calculate Host bits**: $y = 32 - 26 = 6$ zeroes.
3. **Compute Hosts**:
    - $\text{Max hosts} = 2^6 - 2 = 64 - 2 = \mathbf{62}$.
4. **Correct Answer**: **Option C**.

---

🎓 **Would you like me to generate a focused 5-question mock quiz based on classless boundary calculations to help verify your revision before we move to Subtopic 4?**