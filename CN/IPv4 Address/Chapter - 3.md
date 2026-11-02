Comprehensive Master Notes: Routing Tables, Supernetting, Classful Addressing & NAT (Batch 3)

---

Module 1: Routing Tables & Packet Forwarding (Longest Prefix Matching)

1. Routing & Forwarding Table Architecture

A router inspects the **Destination IP Address** of every arriving packet and uses its **Forwarding Table** to determine the next-hop router or output interface.

```
                           ROUTER FORWARDING DECISION
                           
   Arriving Packet ---> [ Destination IP Address ]
                               |
            For each entry in Forwarding Table:
            Perform Bitwise AND: (Dest IP) AND (Subnet Mask)
                               |
            +------------------+------------------+
            |                                     |
       Match Found                           No Match
            |                                     |
  Select match with LARGEST                Forward via
  Prefix Length (/x)                       DEFAULT ROUTE
  (Longest Prefix Match)                   (0.0.0.0/0)
```

2. Forwarding Table Entry Structure

Each row in a forwarding table consists of:

1. **Destination Subnet / Prefix**: The network address (`P.Q.R.S / x`).
2. **Subnet Mask**: Network mask corresponding to the prefix length.
3. **Next-Hop / Output Interface ID**: The local exit interface (e.g., `Eth0`, `Interface 1`) or next-hop router IP.

4. Longest Prefix Matching (LPM) Algorithm

When a destination IP address matches **multiple** entries in a routing table, the router MUST select the entry with the **longest prefix length (largest** $x$ **value)**.

- **Reason**: A longer prefix represents a more specific, smaller network block, providing a more accurate forwarding path.

4. The Default Route (`0.0.0.0/0`)

- **Prefix Length**: `/0` (Mask = `0.0.0.0`).
- **Behavior**: Bitwise AND with `0.0.0.0` always produces `0.0.0.0`, matching **every** destination IP address.
- **Role**: Acts as a wildcard catch-all route when a packet does not match any specific subnet entry in the routing table.

---

Module 2: Supernetting & Route Aggregation (Prefix Aggregation)

1. Supernetting Definition & Objectives

Supernetting (or Route Aggregation / CIDR Aggregation) is the process of logically combining multiple contiguous smaller network blocks into a single larger summary network route.

- **Primary Objective**: Minimizes the size of routing tables across internet core routers, reducing memory footprint and lookup latency.

```
                            ROUTE AGGREGATION
                            
   Subnet 1: 50.60.70.0/26   [64 IPs]  --+
   Subnet 2: 50.60.70.64/26  [64 IPs]  --+---> Combined Summary Route:
   Subnet 3: 50.60.70.128/26 [64 IPs]  --+     50.60.70.0/24 [256 IPs]
   Subnet 4: 50.60.70.192/26 [64 IPs]  --+
```

2. Four Mandatory Conditions for Supernetting

To combine $N$ individual CIDR blocks into a single valid supernet:

1. **Contiguity**: All blocks must be strictly contiguous in address space with no gaps.
2. **Equal Size**: All individual blocks must have the exact same prefix length (same number of addresses).
3. **Power of 2**: The number of blocks $N$ being aggregated must be a power of 2 ($N = 2^k$, where $k$ is the number of bits saved in the supernet prefix).
4. **Block Boundary Alignment**: The starting IP address of the first block must be an exact integer multiple of the total aggregated supernet block size ($2^{32 - x_{\text{super}}}$).

5. Supernet Prefix Calculation Formula

If combining $N = 2^k$ identical blocks of prefix length $x_{\text{sub}}$: $$x_{\text{super}} = x_{\text{sub}} - k$$

---

Module 3: Legacy Classful IPv4 Addressing Architecture

Prior to CIDR, IPv4 addresses were divided into 5 fixed categories called **Classes A, B, C, D, and E** based on leading bit patterns in the first octet.

```
+---------------------------------------------------------------------------------------------------------+
|                                    CLASSFUL IPv4 ADDRESSING SUMMARY                                     |
+-------+--------------+------------------+--------------+---------------+--------------------------------+
| Class | Leading Bits | First Octet Range| NetID / HostID| Default Mask  | Total Networks / Max Hosts     |
+-------+--------------+------------------+--------------+---------------+--------------------------------+
| A     | `0`          | 1 to 126         | 8 / 24 bits  | 255.0.0.0     | 126 Nets / 16,777,214 Hosts    |
| B     | `10`         | 128 to 191       | 16 / 16 bits | 255.255.0.0   | 16,384 Nets / 65,534 Hosts     |
| C     | `110`        | 192 to 223       | 24 / 8 bits  | 255.255.255.0 | 2,097,152 Nets / 254 Hosts     |
| D     | `1110`       | 224 to 239       | Multicast    | None          | Group IDs (268,435,456 Groups) |
| E     | `1111`       | 240 to 255       | Reserved     | None          | Experimental / Research Use    |
+-------+--------------+------------------+--------------+---------------+--------------------------------+
```

- **Class A Reserved Blocks**:
    - `0.0.0.0/8`: "This Host" network.
    - `127.0.0.0/8`: **Loopback Testing Address Space** (`127.0.0.1` refers to local host).

---

Module 4: Network Address Translation (NAT / PAT)

1. Purpose & Private IP Ranges (RFC 1918)

To mitigate global IPv4 address exhaustion, RFC 1918 designated specific non-routable **Private IP Address Ranges** for internal local area networks:

- **Class A Private Range**: `10.0.0.0` to `10.255.255.255` (`10.0.0.0/8`)
- **Class B Private Range**: `172.16.0.0` to `172.31.255.255` (`172.16.0.0/12`)
- **Class C Private Range**: `192.168.0.0` to `192.168.255.255` (`192.168.0.0/16`)

```
   Internal Private Network                         Global Public Internet
   [ Host A: 192.168.1.10:5000 ] --+
                                   |---> [ NAT Router ] ===> [ Public IP: 203.0.113.1:10000 ]
   [ Host B: 192.168.1.20:5000 ] --+
```

2. Port Address Translation (NAPT) Mechanism

A NAT router maps multiple private IP hosts onto a single public IP address using transport-layer **Port Numbers**:

- **Outbound Translation**: Replaces private `(Source IP, Source Port)` with `(Public IP, Assigned NAT Port)`.
- **Inbound Translation**: Replaces public `(Destination IP, NAT Port)` with the original private `(Private IP, Private Port)` using its internal **NAT Translation Table**.

---

Module 5: Step-by-Step Solved Problems & PYQs

Problem 1: ISP Address Allocation with Variable Sizes

- **Question**: An ISP has the block `245.248.128.0/20`. It wants to allocate half of this block to Organization A, a quarter to Organization B, and retain the rest. Find a valid allocation scheme.
- **Step-by-Step Solution**:
    1. **Total Block Size**: `/20` $\implies 2^{32 - 20} = 2^{12} = \mathbf{4096 \text{ Addresses}}$.
    2. **Organization A (Half = 2048 Addresses)**:
        - Requires size $2048 = 2^{11} \implies$ Prefix $= 32 - 11 = \mathbf{/21}$.
        - **Allocation A**: **245.248.128.0/21** (Range: `.128.0` to `.135.255`).
    3. **Organization B (Quarter = 1024 Addresses)**:
        - Requires size $1024 = 2^{10} \implies$ Prefix $= 32 - 10 = \mathbf{/22}$.
        - Start at next aligned boundary: **245.248.136.0/22** (Range: `.136.0` to `.139.255`).
    4. **ISP Remaining Block (1024 Addresses)**:
        - **245.248.140.0/22** (Range: `.140.0` to `.143.255`).
- **Answer**: **A:** **245.248.128.0/21****, B:** **245.248.136.0/22****, Remaining:** **245.248.140.0/22**.

---

Problem 2: ISP Route Aggregation Alignment [GATE-2020]

- **Question**: An organization requires IP addresses for 1500 computers. An ISP serves requests from space `202.61.0.0/17`. The ISP wants to allocate a block to minimize routing entries using route aggregation. Which of the following candidate spaces can be allotted?
    - I. `202.61.84.0/21`
    - II. `202.61.104.0/21`
    - III. `202.61.64.0/21`
    - IV. `202.61.144.0/21`
- **Step-by-Step Solution**:
    1. Host requirement = $1500 + 2 = 1502 \implies \text{Block Size} = 2048 = 2^{11} \implies \mathbf{/21}$.
    2. **Alignment Constraint**: For a `/21` block, the 3rd octet start value must be an integer multiple of $\frac{2048}{256} = \mathbf{8}$.
    3. **Evaluate Candidates**:
        - I. `84 / 8 = 10.5` $\implies$ Not divisible by 8 (**Invalid**).
        - II. `104 / 8 = 13` $\implies$ Integer multiple (**Valid**).
        - III. `64 / 8 = 8` $\implies$ Integer multiple (**Valid**).
        - IV. `144 / 8 = 18` $\implies$ Integer multiple (**Valid**, but check ISP parent boundary `.0/17` which covers `.0.0` to `.127.255`). Candidate IV (`144`) lies outside the ISP's `/17` range!
    4. Therefore, candidates II and III lie within the ISP space and satisfy alignment.
- **Answer**: **II and III only (Option B)**.

---

Problem 3: CIDR Supernetting Block Allocation [GATE-2026, Set-1, MSQ]

- **Question**: An ISP having address block `202.16.0.0/15` assigns a block of 6000 IP addresses to a client using CIDR supernetting. Which of the following address blocks can be assigned?
    - (A) `202.16.0.0/19`
    - (B) `202.16.32.0/19`
    - (C) `202.17.64.0/19`
    - (D) `202.17.24.0/19`
- **Step-by-Step Solution**:
    1. Address requirement $= 6000 \implies \text{Block Size} = 8192 = 2^{13} \implies \mathbf{/19}$.
    2. **Alignment Constraint**: In a `/19` block, the 3rd octet start value must be a multiple of $\frac{8192}{256} = \mathbf{32}$.
    3. **Evaluate Candidates**:
        - (A) `202.16.0.0/19`: Octet 3 = `0` (multiple of 32) $\implies$ **Valid**.
        - (B) `202.16.32.0/19`: Octet 3 = `32` (multiple of 32) $\implies$ **Valid**.
        - (C) `202.17.64.0/19`: Octet 2 = `17` (within `/15`), Octet 3 = `64` (multiple of 32) $\implies$ **Valid**.
        - (D) `202.17.24.0/19`: Octet 3 = `24` (Not a multiple of 32) $\implies$ **Invalid**.
- **Answer**: **Options A, B, and C**.

---

Problem 4: Longest Prefix Matching Forwarding [GATE-2014, 2-Mark]

- **Question**: An IP router receives a packet with address `131.23.151.76`. The routing table has the following entries:
    - `131.16.0.0/12` $\longrightarrow$ Interface 3
    - `131.28.0.0/14` $\longrightarrow$ Interface 5
    - `131.19.0.0/16` $\longrightarrow$ Interface 2
    - `131.22.0.0/15` $\longrightarrow$ Interface 1 Find the output interface ID.
- **Step-by-Step Solution**:
    1. Test `131.23.151.76` against each prefix:
        - **/12** **Mask (****255.240.0.0****)**: $131.23 \;\text{AND}\; 255.240 = 131.16.0.0 \implies$ **Matches Interface 3** (Length 12).
        - **/14** **Mask (****255.252.0.0****)**: $131.23 \;\text{AND}\; 255.252 = 131.20.0.0 \neq 131.28.0.0 \implies$ **No Match**.
        - **/16** **Mask (****255.255.0.0****)**: $131.23 \neq 131.19 \implies$ **No Match**.
        - **/15** **Mask (****255.254.0.0****)**: $131.23 \;\text{AND}\; 255.254 = 131.22.0.0 \implies$ **Matches Interface 1** (Length 15).
    2. Apply Longest Prefix Matching: Match length 15 > Match length 12.
- **Answer**: **Interface 1**.

---

Problem 5: Routing Table Lookup [GATE-2025, 2-Mark]

- **Question**: A packet with destination IP address `145.36.109.70` arrives at a router. Routing table:
    - `145.36.0.0/16` $\longrightarrow$ E1
    - `145.36.128.0/17` $\longrightarrow$ E2
    - `145.36.64.0/18` $\longrightarrow$ E3
    - `145.36.225.0/24` $\longrightarrow$ E4
    - `Default` $\longrightarrow$ E5 Identify the output interface.
- **Step-by-Step Solution**:
    1. Expand 3rd octet of destination (`109` $= 01101101_2$).
    2. **Test** **/16**: Matches `145.36.0.0` (Length 16).
    3. **Test** **/17**: Bit 17 is `0` $\implies$ Result `.0.0` $\neq .128.0$ (**No Match**).
    4. **Test** **/18**: Top 2 bits of 3rd octet are `01` $= 64_{10} \implies$ Result `145.36.64.0` (**Matches E3**, Length 18).
    5. **Test** **/24**: `109` $\neq 225$ (**No Match**).
    6. Longest matching prefix = `/18` (Interface E3).
- **Answer**: **E3 (Option A)**.

---

Problem 6: Default Gateway Selection Across Subnets [GATE-IT-2008]

- **Question**: Host X (`192.168.1.97`) is connected through routers R1 and R2 to Host Y (`192.168.1.80`). R1 has IPs `192.168.1.135` and `192.168.1.110`. R2 has IPs `192.168.1.67` and `192.168.1.155`. The netmask is `255.255.255.224` (/27). Which IP should Host X configure as its default gateway?
- **Step-by-Step Solution**:
    1. Mask `255.255.255.224` $= \mathbf{/27}$ (Block Size = 32 addresses).
    2. Identify Host X's subnet range in 4th octet:
        - Host X IP = `97`. Subnet boundaries: `.0, .32, .64, .96, .128`.
        - Host X belongs to subnet **192.168.1.96/27** (Range: `.96` to `.127`).
    3. Locate router interface IP residing in the **exact same subnet** (`.96` to `.127`):
        - R1 interface `192.168.1.110` lies between `.96` and `.127`.
- **Answer**: **192.168.1.110** **(Option B)**.

---

Problem 7: Supernet Address Derivation for Four Subnets

- **Question**: Find the supernet address for four network blocks: `50.60.70.0/26`, `50.60.70.64/26`, `50.60.70.128/26`, and `50.60.70.192/26`.
- **Step-by-Step Solution**:
    1. Number of blocks $N = 4 = 2^2 \implies$ Save 2 bits from prefix.
    2. Original prefix $= 26 \implies$ Supernet prefix $= 26 - 2 = \mathbf{24}$.
    3. Combine start addresses: All four blocks span `.0` to `.255` in the 4th octet.
- **Answer**: **50.60.70.0/24**.