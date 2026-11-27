1. Foundational Addressing, Binary Conversions & Special IP Identification

An IPv4 address is a 32-bit logical identifier structured hierarchically into a Network Identifier ($\text{NetID}$) and a Host Identifier ($\text{HostID}$):

$$x + y = 32$$

where $x$ denotes the prefix length ($\text{NetID}$ bits) and $y$ denotes the host field width ($\text{HostID}$ bits). Special addresses include the Network Address (all host bits set to 0), the Directed Broadcast Address (all host bits set to 1), the Limited Broadcast Address (`255.255.255.255`), and the This Host address (`0.0.0.0`).

Problem 1.1: 32-Bit Binary to Dotted-Decimal Conversion

**Problem Statement**: Given the IPv4 address in binary representation, determine its equivalent dotted-decimal representation:

$$\texttt{11011101 . 10011010 . 01100111 . 10000110}$$

- **Options**:
    - (A) `221.154.104.134`
    - (B) `220.153.104.133`
    - (C) `221.154.103.134`
    - (D) `221.153.103.133`

**Step-by-Step Solution**: Decompose the 32-bit sequence into four independent 8-bit octets and evaluate the base-10 integer value of each octet:

- **Octet 1**: $$11011101_2 = 2^7 + 2^6 + 2^4 + 2^3 + 2^2 + 2^0 = 128 + 64 + 16 + 8 + 4 + 1 = 221$$
- **Octet 2**: $$10011010_2 = 2^7 + 2^4 + 2^3 + 2^1 = 128 + 16 + 8 + 2 = 154$$
- **Octet 3**: $$01100111_2 = 2^6 + 2^5 + 2^2 + 2^1 + 2^0 = 64 + 32 + 4 + 2 + 1 = 103$$
- **Octet 4**: $$10000110_2 = 2^7 + 2^2 + 2^1 = 128 + 4 + 2 = 134$$

Concatenating the decimal values yields `221.154.103.134`.

- **Correct Answer**: **Option C (****221.154.103.134****)**

---

Problem 1.2: Network Address Derivation from Arbitrary NetID Bits

**Problem Statement**: A classless IPv4 network has a 12-bit Network Identifier assigned as `0110 0110 0101`. Identify the correct dotted-decimal representation of the Network Address.

- **Options**:
    - (A) `100.80.0.0`
    - (B) `100.96.0.0`
    - (C) `102.80.0.0`
    - (D) `102.96.0.0`

**Step-by-Step Solution**:

1. By architectural definition, a Network Address has all Host Identifier ($\text{HostID}$) bits set to zero.
2. The total address width is 32 bits. With $x = 12$ bits of $\text{NetID}$, the $\text{HostID}$ width is: $$y = 32 - 12 = 20 \text{ bits}$$
3. Align the bits across four 8-bit octets:
    - **Octet 1 (Bits 1–8)**: First 8 bits of $\text{NetID} = \texttt{01100110}_2 = 64 + 32 + 4 + 2 = 102$
    - **Octet 2 (Bits 9–16)**: Remaining 4 bits of $\text{NetID}$ followed by 4 leading zero bits of $\text{HostID} = \texttt{01010000}_2 = 64 + 16 = 80$
    - **Octet 3 (Bits 17–24)**: 8 bits of $\text{HostID} = \texttt{00000000}_2 = 0$
    - **Octet 4 (Bits 25–32)**: 8 bits of $\text{HostID} = \texttt{00000000}_2 = 0$

Combining the octets produces `102.80.0.0`.

- **Correct Answer**: **Option C (****102.80.0.0****)**

---

Problem 1.3: Directed Broadcast Address Derivation from Assigned NetID Bits

**Problem Statement**: A classless IPv4 network has a 12-bit Network Identifier assigned as `1001 1010 1100`. Identify the correct dotted-decimal representation of the Directed Broadcast Address.

- **Options**:
    - (A) `154.192.0.0`
    - (B) `154.192.255.255`
    - (C) `154.207.255.255`
    - (D) `154.208.255.255`

**Step-by-Step Solution**:

1. A Directed Broadcast Address retains the assigned $\text{NetID}$ bits while all 20 $\text{HostID}$ bits are set to binary ones.
2. Align the bits across four 8-bit octets:
    - **Octet 1 (Bits 1–8)**: First 8 bits of $\text{NetID} = \texttt{10011010}_2 = 128 + 16 + 8 + 2 = 154$
    - **Octet 2 (Bits 9–16)**: Remaining 4 bits of $\text{NetID}$ (`1100`) followed by 4 ones of $\text{HostID} = \texttt{11001111}_2 = 128 + 64 + 8 + 4 + 2 + 1 = 207$
    - **Octet 3 (Bits 17–24)**: All ones $= \texttt{11111111}_2 = 255$
    - **Octet 4 (Bits 25–32)**: All ones $= \texttt{11111111}_2 = 255$

Combining the evaluated octets yields `154.207.255.255`.

- **Correct Answer**: **Option C (****154.207.255.255****)**

---

2. Classless Inter-Domain Routing (CIDR) & Network Mask Analysis

Under CIDR notation ($P.Q.R.S/x$), the netmask contains $x$ contiguous binary ones followed by $32 - x$ contiguous zeroes. The bitwise logical AND operation between any host IP and the netmask produces the unique Network Address:

$$\text{Network Address} = \text{Host IP} \land \text{Netmask}$$

The maximum assignable host interfaces on any CIDR block with $y$ host bits is:

$$N_{\text{hosts}} = 2^y - 2$$

Problem 2.1: Directed Broadcast Address for `/22` Block

**Problem Statement**: Consider a classless IPv4 network with Network Address `205.207.208.0` and a prefix length of 22 bits. Identify the Directed Broadcast Address of this network.

- **Options**:
    - (A) `205.207.208.255`
    - (B) `205.207.211.255`
    - (C) `205.207.223.255`
    - (D) `205.207.255.255`

**Step-by-Step Solution**:

1. With prefix length $x = 22$, the host field width is $y = 32 - 22 = 10\text{ bits}$.
2. Inspect the third octet where boundary splitting occurs:
    - First 16 bits cover Octets 1 and 2 (`205.207`).
    - Octet 3 contains $22 - 16 = 6\text{ NetID bits}$ and 2 host bits.
    - Decimal 208 in binary: $$208 = 11010000_2$$
    - The first 6 bits are `110100`, and the 2 host bits are `00`.
3. Set all 10 host bits to 1 to obtain the Directed Broadcast Address:
    - Third Octet becomes $\texttt{11010011}_2 = 208 + 3 = 211$.
    - Fourth Octet becomes $\texttt{11111111}_2 = 255$.
4. Resulting Broadcast Address: `205.207.211.255`.
5. **Correct Answer**: **Option B (****205.207.211.255****)**

---

Problem 2.2: Reverse Derivation of Network Address from Broadcast Address

**Problem Statement**: A classless IPv4 network has a Directed Broadcast Address of `110.120.131.255` and a 22-bit Network Identifier. Find the Network Address.

- **Options**:
    - (A) `110.120.0.0`
    - (B) `110.120.128.0`
    - (C) `110.120.130.0`
    - (D) `110.120.131.0`

**Step-by-Step Solution**:

1. Prefix length $x = 22 \implies$ Host bits $y = 10$.
2. The active octet is Octet 3 ($22 - 16 = 6\text{ NetID bits}$, 2 host bits).
3. Decimal 131 in binary: $$131 = 10000011_2$$
4. In a broadcast address, all host bits are 1. The trailing 2 bits of Octet 3 are indeed `11`.
5. To obtain the Network Address, set these 2 host bits to `00`: $$\text{Third Octet} = 10000000_2 = 128$$
6. All 8 bits of Octet 4 are set to 0.
7. Resulting Network Address: `110.120.128.0/22`.
8. **Correct Answer**: **Option B (****110.120.128.0****)**

---

Problem 2.3: Comprehensive Block Metrics for `115.120.125.130/19`

**Problem Statement**: For the host IP address `115.120.125.130/19`, determine:

1. Network Address
2. Directed Broadcast Address
3. First Assignable Host IP
4. Last Assignable Host IP
5. Maximum Number of Assignable Host Interfaces

**Step-by-Step Solution**:

1. **Bit Allocation**:
    - Prefix length $x = 19$. Host field width $y = 32 - 19 = 13\text{ bits}$.
    - Octets 1 and 2 (`115.120`) account for 16 prefix bits.
    - Octet 3 contains $19 - 16 = 3\text{ NetID bits}$ and 5 host bits.
    - Octet 4 contains 8 host bits.
2. **Third Octet Binary Analysis**: $$125_{10} = \mathbf{011}11101_2$$
    - $\text{NetID bits} = \mathbf{011}_2$.
    - Host bits in Octet 3 $= 11101_2$.
3. **Calculate Network Address**:
    - Set all 13 host bits to 0.
    - $\text{Octet 3} = \mathbf{011}00000_2 = 64 + 32 = 96$.
    - $\text{Octet 4} = 00000000_2 = 0$.
    - $\text{Network Address} = \mathbf{115.120.96.0/19}$.
4. **Calculate Directed Broadcast Address**:
    - Set all 13 host bits to 1.
    - $\text{Octet 3} = \mathbf{011}11111_2 = 96 + 31 = 127$.
    - $\text{Octet 4} = 11111111_2 = 255$.
    - $\text{Broadcast Address} = \mathbf{115.120.127.255/19}$.
5. **Host Boundaries**:
    - $\text{First Usable IP} = \text{Network Address} + 1 = \mathbf{115.120.96.1/19}$.
    - $\text{Last Usable IP} = \text{Broadcast Address} - 1 = \mathbf{115.120.127.254/19}$.
6. **Maximum Assignable Hosts**: $$N_{\text{hosts}} = 2^{13} - 2 = 8192 - 2 = \mathbf{8190}$$

---

Problem 2.4: Maximum Host Interface Allocations

The following reference cases demonstrate the calculation of assignable host interfaces across varying prefix and netmask configurations:

- **Case A (26-bit NetID)**:
    - Host bits $y = 32 - 26 = 6$.
    - Maximum assignable IP addresses: $$2^6 - 2 = 64 - 2 = \mathbf{62}$$
- **Case B (Netmask** **255.255.255.128****)**:
    - Octet 4 is $128 = 10000000_2$, meaning 1 prefix bit and 7 host bits.
    - Total host bits $y = 7$.
    - Maximum hosts: $$2^7 - 2 = 128 - 2 = \mathbf{126}$$
- **Case C (GATE-2026 Set-2: Netmask** **255.255.240.0****)**:
    - Third octet $240 = 11110000_2 \implies 4\text{ host bits}$.
    - Fourth octet $0 = 00000000_2 \implies 8\text{ host bits}$.
    - Total host bits $y = 4 + 8 = 12$.
    - Maximum assignable IP addresses: $$2^{12} - 2 = 4096 - 2 = \mathbf{4094}$$
- **Case D (GATE-IT-2004: Netmask** **255.255.255.192****)**:
    - Fourth octet $192 = 11000000_2 \implies 6\text{ host bits}$.
    - Maximum hosts: $$2^6 - 2 = 64 - 2 = \mathbf{62}$$

---

Problem 2.5: Multi-Candidate Netmask Validation for Same-Network Membership

**Problem Statement**: Two hosts have IP addresses `214.218.220.228` and `214.218.210.208`. Identify which of the following netmasks allow both hosts to belong to the same network:

- (A) `255.255.192.0` (/18)
- (B) `255.255.224.0` (/19)
- (C) `255.255.240.0` (/20)
- (D) `255.255.248.0` (/21)

**Step-by-Step Solution**: Two hosts reside on the same network if and only if their respective bitwise AND products with the netmask yield identical network addresses. Octets 1 and 2 are identical (`214.218`). Compare the active third octets in binary:

- $\text{Host}_1 \text{ Third Octet}: 220_{10} = \mathbf{1101}1100_2$
- $\text{Host}_2 \text{ Third Octet}: 210_{10} = \mathbf{1101}0010_2$

Notice that both numbers share the exact same first 4 bits (`1101`), but differ at bit 5:

- Bit 5 of 220 is `1`.
- Bit 5 of 210 is `0`.

Evaluate each netmask candidate:

1. **Option A (****255.255.192.0** **/18)**:
    - Uses 2 bits in Octet 3 (`11000000`).
    - Both share `11` $\implies$ Result is `214.218.192.0` for both. (Valid)
2. **Option B (****255.255.224.0** **/19)**:
    - Uses 3 bits in Octet 3 (`11100000`).
    - Both share `110` $\implies$ Result is `214.218.192.0` for both. (Valid)
3. **Option C (****255.255.240.0** **/20)**:
    - Uses 4 bits in Octet 3 (`11110000`).
    - Both share `1101` $\implies$ Result is `214.218.208.0` for both. (Valid)
4. **Option D (****255.255.248.0** **/21)**:
    - Uses 5 bits in Octet 3 (`11111000`).
    - Bit 5 differs (`1` vs `0`):
        - $\text{Host}_1 \land \text{Mask} \implies \texttt{11011000}_2 = 216$
        - $\text{Host}_2 \land \text{Mask} \implies \texttt{11010000}_2 = 208$
    - Yields different network addresses (`214.218.216.0` vs `214.218.208.0`). (Invalid)
5. **Correct Answer**: **Options A, B, and C**

---

Problem 2.6: Computer Network Configuration Invalidation [GATE-2010]

**Problem Statement**: Computers A and B have IP addresses `10.105.1.113` and `10.105.1.91` respectively. They are configured with the same netmask $N$. Which of the following values of $N$ will **not** allow them to belong to the same network?

- (A) `255.255.255.0`
- (B) `255.255.255.128`
- (C) `255.255.255.192`
- (D) `255.255.255.224`

**Step-by-Step Solution**: Octets 1, 2, and 3 are identical (`10.105.1`). Compare the fourth octets in binary:

- $\text{Host A}: 113_{10} = \mathbf{01}110001_2$
- $\text{Host B}: 91_{10} = \mathbf{01}011011_2$

Observe the bit matches from the left:

- Bit 1: Both are `0`.
- Bit 2: Both are `1`.
- Bit 3: Host A is `1`, Host B is `0`. They diverge at the 3rd bit of the fourth octet.

Evaluate the masks:

1. `255.255.255.0` (/24): Considers 0 bits of Octet 4 $\implies$ Identical.
2. `255.255.255.128` (/25): Checks 1st bit (`0` for both) $\implies$ Identical.
3. `255.255.255.192` (/26): Checks first 2 bits (`01` for both) $\implies$ Identical (`10.105.1.64`).
4. `255.255.255.224` (/27): Checks first 3 bits (`11100000`). Since bit 3 differs:
    - $\text{Host A} \implies \texttt{01100000}_2 = 96$
    - $\text{Host B} \implies \texttt{01000000}_2 = 64$
    - Places the machines in different subnets.
5. **Correct Answer**: **Option D (****255.255.255.224****)**

---

3. Fixed-Length Subnet Masking (FLSM) Design & Iterations

In FLSM, an initial network block ($/x$) is divided into $2^{y_1}$ equal-sized subnets by borrowing $y_1$ bits from the host field. The resulting subnet mask has a prefix length of $x + y_1$.

Problem 3.1: 3-Bit Subnet Partitioning of `50.60.70.0/24`

**Problem Statement**: A classless network `50.60.70.0/24` is divided using 3-bit subnetting. List the complete parameters of all 8 subnets.

**Step-by-Step Solution**: The base prefix is $x = 24$. Borrowing $y_1 = 3$ subnet bits creates an effective prefix of $/27$ with $y_2 = 5$ host bits per subnet, yielding a block size of $2^5 = 32$ addresses.

The following structured table maps the complete progression across all 8 subnets:

|Subnet Number|Subnet Identifier|Subnet Address|First Assignable Host|Last Assignable Host|Subnet Directed Broadcast|
|---|---|---|---|---|---|
|Subnet 1|`000`|`50.60.70.0/27`|`50.60.70.1`|`50.60.70.30`|`50.60.70.31`|
|Subnet 2|`001`|`50.60.70.32/27`|`50.60.70.33`|`50.60.70.62`|`50.60.70.63`|
|Subnet 3|`010`|`50.60.70.64/27`|`50.60.70.65`|`50.60.70.94`|`50.60.70.95`|
|Subnet 4|`011`|`50.60.70.96/27`|`50.60.70.97`|`50.60.70.126`|`50.60.70.127`|
|Subnet 5|`100`|`50.60.70.128/27`|`50.60.70.129`|`50.60.70.158`|`50.60.70.159`|
|Subnet 6|`101`|`50.60.70.160/27`|`50.60.70.161`|`50.60.70.190`|`50.60.70.191`|
|Subnet 7|`110`|`50.60.70.192/27`|`50.60.70.193`|`50.60.70.222`|`50.60.70.223`|
|Subnet 8|`111`|`50.60.70.224/27`|`50.60.70.225`|`50.60.70.254`|`50.60.70.255`|

---

Problem 3.2: 4-Bit Subnet Partitioning of `60.70.80.0/20`

**Problem Statement**: A network `60.70.80.0/20` is partitioned using 4-bit subnetting. Determine the total subnets and their address boundaries.

**Step-by-Step Solution**:

1. Base prefix $x = 20$. Subnet bits $y_1 = 4$.
2. New subnet prefix $= 20 + 4 = /24$.
3. Total subnets created $= 2^4 = 16$.
4. Host bits remaining $y_2 = 32 - 24 = 8$ (allocating 256 addresses per subnet).
5. Third octet base value: $$80_{10} = \mathbf{0101}0000_2$$ The first 4 bits (`0101`) are fixed by the `/20` prefix. The borrowed 4 bits vary from `0000` to `1111` ($0$ to $15$).
6. Subnet addresses step by 1 in the third octet:
    - **Subnet 1**: `60.70.80.0/24` (Broadcast: `60.70.80.255`)
    - **Subnet 2**: `60.70.81.0/24` (Broadcast: `60.70.81.255`)
    - **Subnet 3**: `60.70.82.0/24` (Broadcast: `60.70.82.255`)
    - ...
    - **Subnet 15**: `60.70.94.0/24` (Broadcast: `60.70.94.255`)
    - **Subnet 16**: `60.70.95.0/24` (Broadcast: `60.70.95.255`)

---

Problem 3.3: 5-Bit and 3-Bit Variations on `/20`

**5-Bit Subnetting on** **60.70.80.0/20**:

- Subnet prefix $= 20 + 5 = /25$. Subnet count $= 2^5 = 32$.
- Block size per subnet $= 2^7 = 128$ addresses.
- Subnets advance in increments of 128 in the fourth octet:
    - Subnet 1: `60.70.80.0/25`
    - Subnet 2: `60.70.80.128/25`
    - Subnet 3: `60.70.81.0/25`
    - Subnet 4: `60.70.81.128/25`
    - ...
    - Subnet 31: `60.70.95.0/25`
    - Subnet 32: `60.70.95.128/25`

**3-Bit Subnetting on** **60.70.80.0/20**:

- Subnet prefix $= 20 + 3 = /23$. Subnet count $= 2^3 = 8$.
- Block size per subnet $= 2^9 = 512$ addresses (steps of 2 in the third octet).
- Subnets: `60.70.80.0/23`, `60.70.82.0/23`, `60.70.84.0/23`, `60.70.86.0/23`, `60.70.88.0/23`, `60.70.90.0/23`, `60.70.92.0/23`, `60.70.94.0/23`.

---

Problem 3.4: 6-Bit Subnetting Boundary Pairs [MSQ H.W.]

**Problem Statement**: Consider a classless network `183.185.176.0/20`. The network is divided with 6-bit subnetting. Which of the following options contain correct pairs of Subnet Address and Subnet Broadcast Address?

- (A) Subnet Address: `183.185.176.0`, Broadcast Address: `183.185.176.63`
- (B) Subnet Address: `183.185.180.128`, Broadcast Address: `183.185.180.191`
- (C) Subnet Address: `183.185.184.0`, Broadcast Address: `183.185.188.63`
- (D) Subnet Address: `183.185.190.192`, Broadcast Address: `183.185.190.255`

**Step-by-Step Solution**:

1. Base prefix $x = 20$. Borrowed bits $y_1 = 6$.
2. New subnet prefix $= 20 + 6 = /26$.
3. Remaining host bits $y_2 = 32 - 26 = 6\text{ bits}$.
4. Block size per subnet $= 2^6 = 64$ addresses.
5. In every valid subnet, the fourth octet must start at an integer multiple of 64 ($0, 64, 128, 192$) and end at $\text{Base} + 63$ ($63, 127, 191, 255$).
6. Evaluate the options:
    - Option A: `183.185.176.0` to `183.185.176.63` (Span = 64 addresses, multiple of 64). (Valid)
    - Option B: `183.185.180.128` to `183.185.180.191` (Span = 64 addresses, multiple of 64). (Valid)
    - Option C: `183.185.184.0` to `183.185.188.63`. Octet 3 jumps from 184 to 188 ($4 \times 256 + 64 = 1088$ addresses). Far exceeds a `/26` block. (Invalid)
    - Option D: `183.185.190.192` to `183.185.190.255` (Span = 64 addresses, multiple of 64, inside the `/20` range which ends at `.191.255`). (Valid)
7. **Correct Answer**: **Options A, B, and D**

---

Problem 3.5: Subnet Membership of Three Machines [GATE-2019]

**Problem Statement**: Consider three machines M, N, and P with IP addresses `100.10.5.2`, `100.10.5.5`, and `100.10.5.6` respectively. The subnet mask is configured as `255.255.255.252` for all three machines. Which one of the following statements is true?

- (A) M, N, and P all belong to the same subnet
- (B) Only N and P belong to the same subnet
- (C) Only M and N belong to the same subnet
- (D) M, N, and P belong to three different subnets

**Step-by-Step Solution**:

1. Netmask `255.255.255.252` represents a `/30` prefix ($30$ ones and $2$ zeroes).
2. Subnet block size $= 2^2 = 4$ IP addresses.
3. Subnets in the fourth octet advance in multiples of 4:
    - Block 0: `100.10.5.0` to `100.10.5.3` (Usable: `.1` and `.2`)
    - Block 1: `100.10.5.4` to `100.10.5.7` (Usable: `.5` and `.6`)
    - Block 2: `100.10.5.8` to `100.10.5.11`
4. Map the machines:
    - **Machine M (****100.10.5.2****)** falls into Block 0 (`100.10.5.0/30`).
    - **Machine N (****100.10.5.5****)** falls into Block 1 (`100.10.5.4/30`).
    - **Machine P (****100.10.5.6****)** falls into Block 1 (`100.10.5.4/30`).
5. Machines N and P share subnet `100.10.5.4/30`, whereas Machine M is on a distinct subnet.
6. **Correct Answer**: **Option B (Only N and P belong to the same subnet)**

---

Problem 3.6: Broadcast Address Feasibility Analysis [GATE-IT-2006]

**Problem Statement**: A subnetted network has broadcast address `144.16.95.255`. Its subnet mask:

- (A) is necessarily `255.255.224.0`
- (B) is necessarily `255.255.240.0`
- (C) is necessarily `255.255.248.0`
- (D) could be any one of `255.255.224.0`, `255.255.240.0`, `255.255.248.0`

**Step-by-Step Solution**:

1. Address `144.16.95.255` has Octet 4 as all ones (`255`).
2. Examine Octet 3 ($95$ in binary): $$95_{10} = 01011111_2$$ Observe that the trailing 5 bits are all ones (`11111`).
3. For an address to be a valid directed broadcast, **all host bits must be 1**.
    - If host bits $= 13$ (Mask `/19` = `255.255.224.0`), Octet 3 must end in $13 - 8 = 5$ ones. Binary $010\mathbf{11111}$ has 5 trailing ones $\implies$ Valid.
    - If host bits $= 12$ (Mask `/20` = `255.255.240.0`), Octet 3 must end in $12 - 8 = 4$ ones. Binary $0101\mathbf{1111}$ has 4 trailing ones $\implies$ Valid.
    - If host bits $= 11$ (Mask `/21` = `255.255.248.0`), Octet 3 must end in $11 - 8 = 3$ ones. Binary $01011\mathbf{111}$ has 3 trailing ones $\implies$ Valid.
4. Because any prefix from $/19$ to $/21$ satisfies the all-ones condition, the mask is not uniquely constrained to one option.
5. **Correct Answer**: **Option D (could be any one of** **255.255.224.0****,** **255.255.240.0****,** **255.255.248.0****)**

---

Problem 3.7: Subnet Sizing for Corporate Departments [GATE-CS-2005]

**Problem Statement**: An organization has a 16-bit network identifier and wishes to form subnets for 64 departments. The subnet mask would be:

- (A) `255.255.0.0`
- (B) `255.255.64.0`
- (C) `255.255.128.0`
- (D) `255.255.252.0`

**Step-by-Step Solution**:

1. Base prefix $x = 16$.
2. To support $N = 64$ departments, the borrowed subnet bits must satisfy: $$2^{y_1} \ge 64 \implies y_1 = \lceil \log_2 64 \rceil = 6\text{ bits}$$
3. New prefix length $= 16 + 6 = 22\text{ bits}$.
4. Construct the 22-bit netmask:
    - Octets 1 and 2: `255.255` (16 bits)
    - Octet 3: Six ones followed by two zeroes $= \texttt{11111100}_2 = 128 + 64 + 32 + 16 + 8 + 4 = 252$
    - Octet 4: `0`
5. Netmask: `255.255.252.0`.
6. **Correct Answer**: **Option D (****255.255.252.0****)**

---

Problem 3.8: Maximum Hosts per Subnet under `/21` Mask [GATE-CS-2008]

**Problem Statement**: If a network on the Internet has a subnet mask of `255.255.248.0`, what is the maximum number of hosts per subnet?

- (A) 1022
- (B) 1023
- (C) 2046
- (D) 2047

**Step-by-Step Solution**:

1. Evaluate the mask in binary:
    - Octet 1: $255 = 11111111_2$ (8 bits)
    - Octet 2: $255 = 11111111_2$ (8 bits)
    - Octet 3: $248 = 11111000_2$ (5 ones, 3 zeroes)
    - Octet 4: $0 = 00000000_2$ (8 zeroes)
2. Prefix length $x = 8 + 8 + 5 = 21\text{ bits}$.
3. Host field width $y = 32 - 21 = 11\text{ bits}$.
4. Maximum assignable hosts per subnet: $$N_{\text{hosts}} = 2^{11} - 2 = 2048 - 2 = \mathbf{2046}$$
5. **Correct Answer**: **Option C (2046)**

---

4. Variable-Length Subnet Masking (VLSM) & Hierarchical Block Allocation

VLSM allows subnets of unequal sizes to be carved out of an address block. The foundational design rule requires allocating blocks in descending order of size (largest host requirement first) to prevent address fragmentation and overlapping boundaries.

Problem 4.1: Asymmetric Partitioning of `50.60.70.0/24`

**Problem Statement**: Divide `50.60.70.0/24` into three subnets such that Subnet 1 satisfies 50% of the hosts, and Subnets 2 and 3 each satisfy 25% of the hosts. Identify all valid allocation configurations.

**Step-by-Step Solution**: The base block contains $2^8 = 256$ addresses. Subnet 1 requires a 128-address block ($/25$), while Subnets 2 and 3 require 64 addresses each ($/26$).

The address space can be split into four distinct non-overlapping configurations:

|Partition Scheme|Subnet 1 (/25, Size 128)|Subnet 2 (/26, Size 64)|Subnet 3 (/26, Size 64)|
|---|---|---|---|
|**Scheme 1**|`50.60.70.0/25` (`.0 - .127`)|`50.60.70.128/26` (`.128 - .191`)|`50.60.70.192/26` (`.192 - .255`)|
|**Scheme 2**|`50.60.70.0/25` (`.0 - .127`)|`50.60.70.192/26` (`.192 - .255`)|`50.60.70.128/26` (`.128 - .191`)|
|**Scheme 3**|`50.60.70.128/25` (`.128 - .255`)|`50.60.70.0/26` (`.0 - .63`)|`50.60.70.64/26` (`.64 - .127`)|
|**Scheme 4**|`50.60.70.128/25` (`.128 - .255`)|`50.60.70.64/26` (`.64 - .127`)|`50.60.70.0/26` (`.0 - .63`)|

All four schemes fully partition the 256-address space without collision.

---

Problem 4.2: Enterprise Subnet Allocation [GATE-IT-2005]

**Problem Statement**: A company has a network address `204.204.204.0/24`. It wishes to have three subnets: one with 100 hosts and two with 50 hosts each. Which one of the following options represents a feasible set of subnet addresses?

- (A) `204.204.204.128/26`, `204.204.204.0/25`, `204.204.204.64/25`
- (B) `204.204.204.0/26`, `204.204.204.192/25`, `204.204.204.64/25`
- (C) `204.204.204.128/25`, `204.204.204.192/26`, `204.204.204.224/26`
- (D) `204.204.204.128/25`, `204.204.204.64/26`, `204.204.204.0/26`

**Step-by-Step Solution**:

1. Sizing:
    - Subnet for 100 hosts: Requires $2^y - 2 \ge 100 \implies y = 7$ bits $\implies$ Block size $128$ ($/25$).
    - Two subnets for 50 hosts: Requires $2^y - 2 \ge 50 \implies y = 6$ bits $\implies$ Block size $64$ each ($/26$).
2. Analyze the options:
    - Option A: Lists two `/25` blocks and one `/26` block ($128 + 128 + 64 = 320 > 256$). Exceeds address space. (Invalid)
    - Option B: Also lists two `/25` blocks. Exceeds capacity. (Invalid)
    - Option C: Uses `/25` (`.128 - .255`), then `/26` at `.192` and `.224`. Both `.192` and `.224` fall _inside_ the `.128 - .255` block! Severe overlap. (Invalid)
    - Option D:
        - Subnet 1: `204.204.204.128/25` (Range: `.128` to `.255`, size 128)
        - Subnet 2: `204.204.204.64/26` (Range: `.64` to `.127`, size 64)
        - Subnet 3: `204.204.204.0/26` (Range: `.0` to `.63`, size 64)
        - Total allocated: $128 + 64 + 64 = 256$. Zero overlap. (Feasible)
3. **Correct Answer**: **Option D**

---

Problem 4.3: Minimal Prefix Representation of Continuous Range [GATE-2024 Set-1]

**Problem Statement**: Find the single CIDR prefix that exactly represents the range of IP addresses from `10.12.2.0` to `10.12.3.255`.

**Step-by-Step Solution**:

1. Count the total number of addresses in the range:
    - From `10.12.2.0` to `10.12.2.255` = $256$ addresses.
    - From `10.12.3.0` to `10.12.3.255` = $256$ addresses.
    - Total addresses $= 256 + 256 = 512 = 2^9$.
2. For an exact block of $2^9$ addresses, the host field width is $y = 9$ bits.
3. Prefix length: $$x = 32 - 9 = 23\text{ bits}$$
4. Verify boundary alignment:
    - Third octet starts at 2 ($00000010_2$).
    - With $/23$, the first 7 bits of Octet 3 are fixed ($0000001_2$) and the 8th bit is host bit 0.
    - Range covers $10.12.0000001\mathbf{0}.0$ to $10.12.0000001\mathbf{1}.255$, which is exactly `10.12.2.0` through `10.12.3.255`.
5. CIDR representation: `10.12.2.0/23`.
6. **Correct Answer**: **10.12.2.0/23**

---

Problem 4.4: Boundary Host IP Calculation [GATE-2015 Set-3]

**Problem Statement**: In the network `200.10.11.144/27`, find the fourth octet (in decimal) of the last IP address that can be assigned to a host.

**Step-by-Step Solution**:

1. In standard CIDR, prefix $/27$ establishes a block size of $2^5 = 32$.
2. On standard subnet boundaries with block size 32, subnets advance as `.0`, `.32`, `.64`, `.96`, `.128`, `.160`.
3. The block encompassing host range around 144 is base `200.10.11.128/27`:
    - Subnet Address: `200.10.11.128`
    - First Host IP: `200.10.11.129`
    - Last Host IP: `200.10.11.158`
    - Directed Broadcast: `200.10.11.159`
4. The fourth octet of the last assignable host IP is **158**.
5. **Correct Answer**: **158**

---

Problem 4.5: Subnet Block Sizing for 50 Hosts [Example 19]

**Problem Statement**: An ISP has an address chunk `136.122.165.0/24`. An organization requests an IP address range for 50 hosts. Write all possible CIDR prefixes for this allocation.

**Step-by-Step Solution**:

1. Host Requirement: 50 hosts. $$2^y - 2 \ge 50 \implies 2^y \ge 52 \implies y = 6\text{ bits}$$
2. Prefix length: $$x = 32 - 6 = 26\text{ bits}$$
3. Subnet block size $= 2^6 = 64$ addresses.
4. Carving `/26` blocks out of `136.122.165.0/24` creates 4 candidate prefixes:
    - Block 1: `136.122.165.0/26` (Range: `.0` to `.63`)
    - Block 2: `136.122.165.64/26` (Range: `.64` to `.127`)
    - Block 3: `136.122.165.128/26` (Range: `.128` to `.191`)
    - Block 4: `136.122.165.192/26` (Range: `.192` to `.255`)

Any of these four `/26` blocks is a valid allocation for the organization.

---

Problem 4.6: Hierarchical Address Allocation by ISP [GATE-2012]

**Problem Statement**: An ISP has the block `245.248.128.0/20`. The ISP wants to give half of this chunk to Organization A and a quarter to Organization B, while retaining the rest. Which of the following is a valid allocation?

- (A) `245.248.136.0/21` and `245.248.128.0/22`
- (B) `245.248.128.0/21` and `245.248.128.0/22`
- (C) `245.248.132.0/22` and `245.248.132.0/21`
- (D) `245.248.136.0/24` and `245.248.132.0/21`

**Step-by-Step Solution**:

1. ISP Block: `245.248.128.0/20`. Total addresses $= 2^{12} = 4096$.
    - Octet 3 binary: $128_{10} = \mathbf{1000}0000_2$.
    - Range: `245.248.128.0` to `245.248.143.255`.
2. Half of the block:
    - Size $= 2048$ addresses $\implies /21$ prefix.
    - Split into two halves at Octet 3:
        - Lower half: `245.248.128.0/21` (Octet 3: $128$ to $135$)
        - Upper half: `245.248.136.0/21` (Octet 3: $136$ to $143$)
3. Quarter of the block:
    - Size $= 1024$ addresses $\implies /22$ prefix.
    - Sub-dividing the lower half ($128$ to $135$) gives:
        - First quarter: `245.248.128.0/22` (Octet 3: $128$ to $131$)
        - Second quarter: `245.248.132.0/22` (Octet 3: $132$ to $135$)
4. Check Option A:
    - Organization A: `245.248.136.0/21` (covers $136$ to $143$)
    - Organization B: `245.248.128.0/22` (covers $128$ to $131$)
    - Ranges are completely disjoint. (Valid)
5. Check Option B:
    - Organization A: `245.248.128.0/21` (covers $128$ to $135$)
    - Organization B: `245.248.128.0/22` (covers $128$ to $131$)
    - Overlap! Organization B is completely inside Organization A. (Invalid)
6. **Correct Answer**: **Option A**

---

Problem 4.7: Route-Aggregation-Constrained Block Allocation [GATE-2020]

**Problem Statement**: An organization requires IP addresses for 1500 computers. An ISP uses CIDR and allocates from `202.61.0.0/17`. The ISP wants to minimize the number of routing entries in its router using route aggregation. Which of the following address spaces are potential candidates?

- I. `202.61.84.0/21`
- II. `202.61.104.0/21`
- III. `202.61.64.0/21`
- IV. `202.61.144.0/21`
- **Options**:
    - (A) I and II only
    - (B) II and III only
    - (C) III and IV only
    - (D) I and IV only

**Step-by-Step Solution**:

1. Capacity requirement: 1500 computers $\implies 2^y - 2 \ge 1500 \implies y = 11$ host bits ($2^{11} = 2048$).
    - Prefix length $x = 32 - 11 = 21$.
2. Alignment Constraint:
    - For a `/21` block, Octet 3 must be an integer multiple of $2^{24 - 21} = 2^3 = 8$.
    - Valid Octet 3 values: $0, 8, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 104, 112, 120$.
3. ISP Space Range:
    - Base: `202.61.0.0/17`.
    - Octet 3 binary for `/17`: bit 1 is fixed as `0`.
    - Range of Octet 3: $0$ to $127$.
4. Test the candidates:
    - Candidate I (`202.61.84.0/21`): Octet 3 is 84. Is $84$ divisible by 8? $84 / 8 = 10.5$ (No! Invalid block boundary).
    - Candidate II (`202.61.104.0/21`): Octet 3 is 104. $104 / 8 = 13$ (Divisible) and $104 \le 127$ (Inside ISP space). Valid!
    - Candidate III (`202.61.64.0/21`): Octet 3 is 64. $64 / 8 = 8$ (Divisible) and $64 \le 127$ (Inside ISP space). Valid!
    - Candidate IV (`202.61.144.0/21`): Octet 3 is 144. While $144 / 8 = 18$, $144 > 127$ (Falls outside the ISP's `/17` block!). Invalid!
5. **Correct Answer**: **Option B (II and III only)**

---

5. CIDR Supernetting & Address Block Aggregation

Supernetting combines multiple smaller, contiguous network blocks into a single summary route with a shorter prefix. For $2^k$ blocks to be aggregateable:

1. All blocks must be contiguous.
2. All blocks must be equal in size.
3. The starting block address must be aligned to a multiple of the combined supernet block size.

Problem 5.1: 4-Block Binary Supernet Synthesis

**Problem Statement**: Given four contiguous network addresses with 6-bit $\text{NetID}$ and 2-bit $\text{HostID}$:

- $\text{Net}_1 = 10100000/6$
- $\text{Net}_2 = 10100100/6$
- $\text{Net}_3 = 10101000/6$
- $\text{Net}_4 = 10101100/6$

Determine the aggregate supernet address.

**Step-by-Step Solution**:

1. Number of networks $= 4 = 2^2 \implies$ Compress the prefix by 2 bits ($6 - 2 = /4$).
2. Compare the 6 prefix bits across all 4 networks:
    - $\text{Net}_1: \mathbf{1010}00$
    - $\text{Net}_2: \mathbf{1010}01$
    - $\text{Net}_3: \mathbf{1010}10$
    - $\text{Net}_4: \mathbf{1010}11$
3. All 4 networks share the common 4-bit prefix `1010`. The remaining 4 bits vary across all combinations from `0000` to `1111`.
4. Supernet Address: `10100000/4`.

---

Problem 5.2: Aggregation of Four `/21` Classless Blocks

**Problem Statement**: Synthesize the following four network addresses into a single supernet:

- `210.205.192.0/21`
- `210.205.200.0/21`
- `210.205.208.0/21`
- `210.205.216.0/21`

**Step-by-Step Solution**:

1. Aggregating 4 networks reduces the prefix by $\log_2 4 = 2$ bits: $$\text{Supernet Prefix} = 21 - 2 = 19$$
2. Octets 1 and 2 are identical (`210.205`). Inspect Octet 3 in binary:
    - $192_{10} = \mathbf{11000}000_2$
    - $200_{10} = \mathbf{11001}000_2$
    - $208_{10} = \mathbf{11010}000_2$
    - $216_{10} = \mathbf{11011}000_2$
3. For prefix `/19`, examine the first 3 bits of Octet 3 ($19 - 16 = 3$ bits):
    - All four addresses share the leading 3 bits: `110`.
    - Setting the remaining bits of Octet 3 to zero gives $\texttt{11000000}_2 = 192$.
4. Supernet Address: `210.205.192.0/19`.

---

Problem 5.3: Client Allocation from Large ISP Block [GATE-2026 Set-1 MSQ]

**Problem Statement**: An ISP holding the address block `202.16.0.0/15` assigns a block of 6000 IP addresses to a client using the CIDR supernetting approach. Which of the following address blocks can be assigned by the ISP?

- (A) `202.16.0.0/19`
- (B) `202.16.32.0/19`
- (C) `202.17.64.0/19`
- (D) `202.17.24.0/19`

**Step-by-Step Solution**:

1. Sizing Requirement:
    - The client requires 6000 IPs.
    - Smallest power of 2 accommodating 6000 is $2^{13} = 8192$.
    - Host bits $y = 13 \implies$ Prefix length $x = 32 - 13 = 19$.
2. ISP Space Boundaries:
    - ISP block: `202.16.0.0/15`.
    - Octet 2 binary for 16 is $\mathbf{0001000}0_2$.
    - A `/15` prefix fixes the first 7 bits of Octet 2 (`0001000`). The 8th bit can be `0` or `1`, allowing Octet 2 to be either **16** or **17**.
    - Octets 3 and 4 can range from `0.0` to `255.255`.
3. Boundary Alignment for `/19`:
    - Octet 3 contains $19 - 16 = 3$ prefix bits.
    - Octet 3 must be a multiple of $2^{24 - 19} = 2^5 = 32$.
    - Valid Octet 3 values: $0, 32, 64, 96, 128, 160, 192, 224$.
4. Evaluate candidates:
    - Option A (`202.16.0.0/19`): Octet 2 is 16 (in ISP range), Octet 3 is 0 (multiple of 32). (Valid)
    - Option B (`202.16.32.0/19`): Octet 2 is 16 (in ISP range), Octet 3 is 32 (multiple of 32). (Valid)
    - Option C (`202.17.64.0/19`): Octet 2 is 17 (in ISP range), Octet 3 is 64 (multiple of 32). (Valid)
    - Option D (`202.17.24.0/19`): Octet 3 is 24. 24 is NOT a multiple of 32! (Invalid)
5. **Correct Answer**: **Options A, B, and C**

---

Problem 5.4: Routing Table Aggregation Validation [GATE-2022 MSQ]

**Problem Statement**: Consider the routing table of an organization's router shown below:

|Subnet Number|Subnet Mask|Next Hop|
|---|---|---|
|`12.20.164.0`|`255.255.252.0` (/22)|R1|
|`12.20.170.0`|`255.255.254.0` (/23)|R2|
|`12.20.168.0`|`255.255.254.0` (/23)|Interface 0|
|`12.20.166.0`|`255.255.254.0` (/23)|Interface 1|
|default||R3|

Which of the following prefixes in CIDR notation can be collectively used to correctly aggregate all of the subnets in the routing table?

- (A) `12.20.164.0/20`
- (B) `12.20.164.0/22`
- (C) `12.20.164.0/21`
- (D) `12.20.168.0/22`

**Step-by-Step Solution**:

1. Analyze the individual subnets in the third octet:
    - Entry 1: `12.20.164.0/22` covers Octet 3 values: $164, 165, 166, 167$ (Size 4, `/22`).
    - Entry 2: `12.20.170.0/23` covers Octet 3 values: $170, 171$ (Size 2, `/23`).
    - Entry 3: `12.20.168.0/23` covers Octet 3 values: $168, 169$ (Size 2, `/23`).
    - Entry 4: `12.20.166.0/23` covers Octet 3 values: $166, 167$ (Size 2, `/23`).
2. Total span of addresses in Octet 3:
    - Span covers $164$ through $171$, totaling $8$ contiguous octet values ($171 - 164 + 1 = 8$).
    - A block of 8 in Octet 3 corresponds to $2^3 \times 256 = 2048 = 2^{11}$ addresses $\implies /21$ prefix ($32 - 11 = 21$).
3. Check alignment of $164$ with `/21`:
    - For `/21`, Octet 3 must be a multiple of $2^{24 - 21} = 8$.
    - Is 164 divisible by 8? $164 / 8 = 20.5$ (No! $164$ cannot be the start of a `/21` block).
4. Since a single `/21` block cannot start at 164, decompose the span into aligned blocks:
    - Range $164$ to $167$ forms an aligned `/22` block: `12.20.164.0/22`.
    - Range $168$ to $171$ forms an aligned `/22` block: `12.20.168.0/22`.
5. Combining `12.20.164.0/22` and `12.20.168.0/22` exactly aggregates all subnets without covering outside addresses.
6. **Correct Answer**: **Options B and D**

---

6. Router Routing Tables & Longest Prefix Matching (LPM)

Routers forward packets using the **Longest Prefix Match (LPM)** algorithm. When a packet arrives, the router computes the logical bitwise AND between the destination IP address and the subnet mask of every entry in its forwarding table. If multiple entries match, the packet is forwarded through the interface associated with the longest prefix length (largest number of mask ones).

Problem 6.1: Dual Destination Forwarding Lookup [GATE-CS-2004]

**Problem Statement**: The routing table of a router is shown below:

|Destination|Subnet Mask|Interface|
|---|---|---|
|`128.75.43.0`|`255.255.255.0` (/24)|Eth0|
|`128.75.43.0`|`255.255.255.128` (/25)|Eth1|
|`192.12.17.5`|`255.255.255.255` (/32)|Eth3|
|Default||Eth2|

On which interfaces will the router forward packets addressed to destinations `128.75.43.16` and `192.12.17.10` respectively?

- (A) Eth1 and Eth2
- (B) Eth0 and Eth2
- (C) Eth0 and Eth3
- (D) Eth1 and Eth3

**Step-by-Step Solution**:

**Packet 1:** **128.75.43.16**

1. Test `/25` (`255.255.255.128`):
    - Fourth octet: $16 \land 128 = 0$.
    - Result: `128.75.43.0` $\implies$ Matches the entry for Eth1.
2. Test `/24` (`255.255.255.0`):
    - Fourth octet: $16 \land 0 = 0$.
    - Result: `128.75.43.0` $\implies$ Matches the entry for Eth0.
3. Apply Longest Prefix Match:
    - Prefix `/25` is longer than `/24`.
    - **Selected Interface: Eth1**.

**Packet 2:** **192.12.17.10**

1. Test Eth0 & Eth1: Octets do not match `128.75.43.0`.
2. Test Eth3 (`/32`): Requires exact match with `192.12.17.5`. Since $10 \neq 5$, it does not match.
3. Fall through to **Default route**:
    - **Selected Interface: Eth2**.
4. **Correct Answer**: **Option A (Eth1 and Eth2)**

---

Problem 6.2: Multi-Prefix Forwarding Table Lookup [GATE-IT-2006]

**Problem Statement**: A router uses the following routing table:

|Destination|Mask|Interface|
|---|---|---|
|`144.16.0.0`|`255.255.0.0` (/16)|Eth0|
|`144.16.64.0`|`255.255.224.0` (/19)|Eth1|
|`144.16.68.0`|`255.255.255.0` (/24)|Eth2|
|`144.16.68.64`|`255.255.255.224` (/27)|Eth3|

A packet bearing destination address `144.16.68.117` arrives at the router. On which interface will it be forwarded?

**Step-by-Step Solution**: Destination IP: `144.16.68.117`. Evaluate entries from longest to shortest prefix:

1. **Test** **/27** **(****255.255.255.224** **on Eth3)**:
    - Active fourth octet: $117_{10} = \mathbf{011}10101_2$.
    - Netmask 224: $224_{10} = \mathbf{111}00000_2$.
    - Bitwise AND: $$117 \land 224 = \texttt{01100000}_2 = 64 + 32 = 96$$
    - Result: `144.16.68.96`.
    - Stored table destination is `144.16.68.64` ($96 \neq 64$). **No Match!**
2. **Test** **/24** **(****255.255.255.0** **on Eth2)**:
    - Mask zeroes out Octet 4.
    - Result: `144.16.68.0`.
    - Matches stored destination `144.16.68.0`! **Match!** (Prefix length = 24).
3. **Test** **/19** **and** **/16**:
    - While they also match, their prefix lengths (19 and 16) are shorter than 24.
4. By Longest Prefix Match, `/24` wins.
5. **Correct Answer**: **Option C (Eth2)**

---

Problem 6.3: Non-Matching CIDR Forwarding Lookup [GATE-2014]

**Problem Statement**: An IP router implementing CIDR receives a packet with destination address `131.23.151.76`. The router's table has entries:

|Prefix|Output Interface|
|---|---|
|`131.16.0.0/12`|3|
|`131.28.0.0/14`|5|
|`131.19.0.0/16`|2|
|`131.22.0.0/15`|1|

Identify the output interface.

**Step-by-Step Solution**: Inspect Octet 2 of destination: $23_{10} = \mathbf{000101}11_2$.

1. **Test** **/16** **(****131.19.0.0/16****)**:
    - Requires Octet 2 to be exactly 19 ($00010011_2$). Since $23 \neq 19$, **No Match**.
2. **Test** **/15** **(****131.22.0.0/15** **on Interface 1)**:
    - Masks first 7 bits of Octet 2: $$23 \land \texttt{11111110}_2 = 23 \land 254 = 22$$
    - Result: `131.22.0.0` matches table entry `131.22.0.0`! **Match!** (Prefix = 15).
3. **Test** **/14** **(****131.28.0.0/14****)**:
    - Mask first 6 bits of Octet 2: $$23 \land \texttt{11111100}_2 = 23 \land 252 = 20 \neq 28 \implies \text{No Match}$$
4. **Test** **/12** **(****131.16.0.0/12****)**:
    - Mask first 4 bits of Octet 2: $$23 \land 240 = 16 \implies \text{Matches, but prefix length 12 is shorter than 15.}$$
5. Longest matching prefix is `/15`.
6. **Correct Answer**: **Interface 1**

---

Problem 6.4: Active Octet Subnet Forwarding [GATE-2025]

**Problem Statement**: A packet with destination IP address `145.36.109.70` arrives at a router with the following table:

|Subnet|Subnet Mask (CIDR)|Interface|
|---|---|---|
|`145.36.0.0`|`/16`|E1|
|`145.36.128.0`|`/17`|E2|
|`145.36.64.0`|`/18`|E3|
|`145.36.225.0`|`/24`|E4|
|Default|--|E5|

Which interface will the packet be forwarded to?

**Step-by-Step Solution**: Inspect Octet 3: $109_{10} = \mathbf{01}101101_2$.

1. **Test** **/24** **(E4)**:
    - Requires Octet 3 to be 225. $109 \neq 225 \implies$ No match.
2. **Test** **/18** **(E3)**:
    - Mask uses 2 bits in Octet 3 ($192_{10} = \mathbf{11}000000_2$): $$109 \land 192 = \texttt{01000000}_2 = 64$$
    - Result: `145.36.64.0` matches stored entry `145.36.64.0`! **Match!** (Prefix = 18).
3. **Test** **/17** **(E2)**:
    - Mask uses 1 bit in Octet 3 ($128_{10} = \mathbf{1}0000000_2$): $$109 \land 128 = 0 \neq 128 \implies \text{No match.}$$
4. **Test** **/16** **(E1)**:
    - Matches, but prefix length 16 is shorter than 18.
5. Longest matching prefix is `/18`.
6. **Correct Answer**: **Interface E3 (Option A)**

---

Problem 6.5: Aggregate Packet Counting per Next-Hop [GATE-2024 Set-1]

**Problem Statement**: The forwarding table of an IP router is given below:

|Prefix|Next Hop Router|
|---|---|
|`10.1.1.0/24`|R1|
|`10.1.1.128/25`|R2|
|`10.1.1.64/26`|R3|
|`10.1.1.192/26`|R4|

The router forwards 20 packets each to 5 hosts: `10.1.1.16`, `10.1.1.72`, `10.1.1.132`, `10.1.1.191`, and `10.1.1.205`. Find the number of packets forwarded via next hop router R2.

**Step-by-Step Solution**: Map each destination address using Longest Prefix Match:

1. **Host** **10.1.1.16**:
    - Binary of 16: $\texttt{00010000}_2$.
    - Does not match `/25` (bit 1 is 0), does not match `/26` at 64 (bit 2 is 0).
    - Matches `/24` $\implies$ **Next Hop: R1**.
2. **Host** **10.1.1.72**:
    - Binary of 72: $\texttt{01001000}_2$.
    - Matches `/26` at 64 (leading bits `01`) $\implies$ **Next Hop: R3**.
3. **Host** **10.1.1.132**:
    - Binary of 132: $\texttt{10000100}_2$.
    - Leading bits are `10`. Matches `/25` (`10.1.1.128/25`). Does not match `/26` at 192 (needs `11`).
    - Longest match is `/25` $\implies$ **Next Hop: R2**.
4. **Host** **10.1.1.191**:
    - Binary of 191: $\texttt{10111111}_2$.
    - Leading bits are `10`. Matches `/25` (`.128` to `.255`). Does not match `/26` at 192 (`.192` to `.255`).
    - Longest match is `/25` $\implies$ **Next Hop: R2**.
5. **Host** **10.1.1.205**:
    - Binary of 205: $\texttt{11001101}_2$.
    - Leading bits are `11`. Matches `/26` at 192 (`10.1.1.192/26`).
    - Longest match is `/26` $\implies$ **Next Hop: R4**.

**Summary**:

- Hosts forwarded via R2: `10.1.1.132` and `10.1.1.191` (2 hosts).
- Total packets forwarded via R2: $$2 \times 20 \text{ packets} = \mathbf{40}$$
- **Correct Answer**: **40**

---

Problem 6.6: Nested Prefix Selection [Example 20]

**Problem Statement**: A CIDR-based IPv4 router has the following entries in its routing table:

|Prefix|Interface|
|---|---|
|`120.0.0.0/8`|Eth1|
|`120.120.0.0/16`|Eth2|
|`120.120.120.0/24`|Eth3|

A packet bearing destination IP address `123.123.123.123` arrives at the router. Identify the forwarding interface.

**Step-by-Step Solution**:

1. Check `/24` (`120.120.120.0/24`):
    - First 3 octets of destination are `123.123.123`.
    - $123 \neq 120 \implies$ No match.
2. Check `/16` (`120.120.0.0/16`):
    - First 2 octets are `123.123`.
    - $123 \neq 120 \implies$ No match.
3. Check `/8` (`120.0.0.0/8`):
    - First octet is 123. Stored entry is 120.
    - $123 \neq 120 \implies$ No match!
4. Since no entry matches, the packet is forwarded to the default gateway or dropped with an ICMP Destination Host/Network Unreachable message.

---

Problem 6.7: Asymmetric Subnet Assumption Between Two Computers [GATE-CS-2006]

**Problem Statement**: Two computers C1 and C2 are configured as follows:

- C1 IP: `203.197.2.53`, Netmask: `255.255.128.0` (/17)
- C2 IP: `203.197.75.201`, Netmask: `255.255.192.0` (/18)

Which of the following statements is true?

- (A) C1 and C2 both assume they are on the same network
- (B) C2 assumes C1 is on same network, but C1 assumes C2 is on a different network
- (C) C1 assumes C2 is on same network, but C2 assumes C1 is on a different network
- (D) C1 and C2 both assume they are on different networks

**Step-by-Step Solution**: A host determines if a remote IP is on its local subnet by applying **its own netmask** to both its own IP and the target IP.

**Perspective of Computer C1 (Mask** **/17****,** **255.255.128.0****)**:

- C1 computes its own network: $$2 \land 128 = 0 \implies \mathbf{203.197.0.0/17}$$
- C1 evaluates target C2 (`203.197.75.201`): $$75 \land 128 = 0 \implies \mathbf{203.197.0.0/17}$$
- Both yield `203.197.0.0/17`.
- **C1 assumes C2 is on the SAME network**.

**Perspective of Computer C2 (Mask** **/18****,** **255.255.192.0****)**:

- C2 computes its own network: $$75_{10} = \texttt{01001011}_2 \land \texttt{11000000}_2 (192) = \texttt{01000000}_2 = 64 \implies \mathbf{203.197.64.0/18}$$
- C2 evaluates target C1 (`203.197.2.53`): $$2_{10} = \texttt{00000010}_2 \land 192 = 0 \implies \mathbf{203.197.0.0/18}$$
- Since `203.197.64.0` $\neq$ `203.197.0.0`, C2 sees C1 on an external subnet and will attempt to route via its gateway.
- **C2 assumes C1 is on a DIFFERENT network**.
- **Correct Answer**: **Option C (C1 assumes C2 is on same network, but C2 assumes C1 is on a different network)**

---

Problem 6.8: Default Gateway Assignment in Multi-Subnet Architecture [GATE-IT-2008]

**Problem Statement**: Host X has IP address `192.168.1.97` and is connected through two routers R1 and R2 to another host Y with IP address `192.168.1.80`. Router R1 has IP addresses `192.168.1.135` and `192.168.1.110`. Router R2 has IP addresses `192.168.1.67` and `192.168.1.155`. The netmask used across the network is `255.255.255.224`. Which IP address should Host X configure as its default gateway?

- (A) `192.168.1.67`
- (B) `192.168.1.110`
- (C) `192.168.1.135`
- (D) `192.168.1.155`

**Step-by-Step Solution**:

1. Netmask `255.255.255.224` represents a `/27` prefix.
2. Block size $= 2^{32 - 27} = 2^5 = 32$ addresses.
3. Subnets in Octet 4 advance in multiples of 32:
    - Subnet 0: `.0` to `.31`
    - Subnet 1: `.32` to `.63`
    - Subnet 2: `.64` to `.95`
    - Subnet 3: `.96` to `.127`
    - Subnet 4: `.128` to `.159`
4. Locate Host X (`192.168.1.97`):
    - Falls into Subnet 3 (`.96` to `.127`).
5. A host can only communicate directly with a default gateway interface that resides on the **exact same physical subnet**.
6. Check the candidate router interfaces:
    - `192.168.1.67` $\implies$ In Subnet 2 (`.64 - .95`).
    - `192.168.1.110` $\implies$ In Subnet 3 (`.96 - .127`). (Same subnet as Host X!)
    - `192.168.1.135` $\implies$ In Subnet 4 (`.128 - .159`).
    - `192.168.1.155` $\implies$ In Subnet 4 (`.128 - .159`).
7. Therefore, Host X must configure `192.168.1.110` as its default gateway.
8. **Correct Answer**: **Option B (****192.168.1.110****)**

---

Problem 6.9: Address Resolution Protocol Functionality [GATE-CS-2005]

**Problem Statement**: The Address Resolution Protocol (ARP) is used for:

- (A) Finding the IP address from the DNS
- (B) Finding the IP address of the default gateway
- (C) Finding the IP address that corresponds to a MAC address
- (D) Finding the MAC address that corresponds to an IP address

**Step-by-Step Solution**:

1. Dynamic host delivery requires binding a 32-bit logical network layer address (IPv4) to a 48-bit physical data link layer address (MAC).
2. **ARP (Address Resolution Protocol)** takes a known destination IP address and broadcasts an ARP Request frame to resolve the corresponding physical **MAC address**.
3. **RARP (Reverse ARP)** performs the opposite function (resolving an IP address from a known MAC address).
4. **Correct Answer**: **Option D (Finding the MAC address that corresponds to an IP address)**

---

Problem 6.10: Data Link Layer Traversal Frequency

The encapsulation and decapsulation cycle of network packets through intermediate infrastructure devices governs the number of visits to Layer 2:

- **Scenario 1: Two hosts connected via one intermediate router**:
    - Topology: $\text{Host S} \longrightarrow \text{Router R} \longrightarrow \text{Host D}$.
    - Traversal steps:
        1. Host S encapsulates the packet into a frame at its Data Link layer (Visit 1).
        2. Router R receives the frame and decapsulates it at its Data Link layer (Visit 2).
        3. Router R re-encapsulates the packet into a new frame at its outgoing Data Link layer (Visit 3).
        4. Host D receives and decapsulates the frame at its Data Link layer (Visit 4).
    - Total Data Link layer visits = **4**.
- **Scenario 2: Two hosts connected via one Ethernet switch and one router**:
    - Topology: $\text{Host A} \longrightarrow \text{Switch S} \longrightarrow \text{Router R} \longrightarrow \text{Host B}$.
    - Traversal steps:
        1. Host A encapsulates the packet into an Ethernet frame (Visit 1).
        2. Switch S is a Layer-2 device; it inspects the MAC header at the Data Link layer to forward the frame without decapsulation/re-encapsulation (Visit 2).
        3. Router R receives and decapsulates the frame at its input Data Link interface (Visit 3).
        4. Router R re-encapsulates the packet at its output Data Link interface (Visit 4).
        5. Host B receives and processes the frame at its Data Link layer (Visit 5).
    - Total Data Link layer visits = **5**.