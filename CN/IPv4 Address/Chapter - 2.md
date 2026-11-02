Comprehensive Master Notes: Subnetting (FLSM & VLSM) & Network Layer Traversal (Batch 2)

---

Module 1: Subnetting Concepts & Fixed-Length Subnet Masking (FLSM)

1. Subnetting Definition & Objectives

Subnetting is the technique of logically dividing a single large physical or logical network block into smaller, distinct sub-networks (subnets).

- **Primary Benefits**: Reduces broadcast traffic domains, improves security isolation, and simplifies administrative network management.

2. Three-Tier Address Hierarchy

Subnetting expands the standard 2-tier address structure into a 3-tier hierarchy by borrowing $y_1$ bits from the HostID field:

```
  0                           x x+1               x+y1 x+y1+1                 31 Bits
  +----------------------------+----------------------+----------------------------+
  |    Network ID (x bits)     | Subnet ID (y1 bits)  |     Host ID (y2 bits)      |
  +----------------------------+----------------------+----------------------------+
  \__________________________________________________/
                 Extended Prefix (x + y1 bits)
```

$$\text{Total Address Bits} = x + y_1 + y_2 = 32 \text{ bits}$$

3. Subnet Mask Expansion

When a network is subnetted using $y_1$ bits, the prefix increases from $x$ bits to $(x + y_1)$ bits. The new **Subnet Mask** sets all $(x + y_1)$ prefix bits to 1s and the remaining $y_2$ HostID bits to 0s.

4. Key Formulas for FLSM

5. **Number of Subnets Created**: $$\text{Number of Subnets} = 2^{y_1}$$
6. **Total Addresses per Subnet (Block Size)**: $$\text{Block Size} = 2^{y_2}$$
7. **Maximum Usable Hosts per Subnet**: $$\text{Usable Hosts per Subnet} = 2^{y_2} - 2$$ _(Subtracting 2 for the Subnet Address and the Subnet Broadcast Address)._
8. **Subnet Address (Subnet ID)**: NetID = Assigned, SubnetID = Specific Pattern, **HostID = All 0s**.
9. **Subnet Broadcast Address**: NetID = Assigned, SubnetID = Specific Pattern, **HostID = All 1s**.

10. Inefficiencies of FLSM

In **Fixed-Length Subnet Masking (FLSM)**, every subnet receives an equal number of IP addresses ($2^{y_2}$) regardless of actual departmental host requirements. This leads to significant IP address waste if departments have widely varying host counts.

---

Module 2: Variable-Length Subnet Masking (VLSM) & Subnet Design

1. VLSM Definition & Dynamic Netmasks

**Variable-Length Subnet Masking (VLSM)** allows different subnets within the same parent network to use different prefix lengths (and different subnet masks).

```
                             PARENT BLOCK (/24)
                             [256 Total Addresses]
                                       |
                   +-------------------+-------------------+
                   |                                       |
            SUBNET 1 (/25)                           RESERVED (/25)
          [128 Addresses]                         [128 Addresses]
                                                           |
                                           +---------------+---------------+
                                           |                               |
                                    SUBNET 2 (/26)                  SUBNET 3 (/26)
                                    [64 Addresses]                  [64 Addresses]
```

2. The Mandatory Allocation Rule (Largest First)

When partitioning an IP block using VLSM:

- **Rule**: Always allocate subnets starting with the **largest host requirement first**, then proceed in decreasing order of requirement size.
- **Reason**: Allocating smaller subnets first causes block boundary misalignment and fragments the address space, preventing clean larger allocations later.

3. Block Alignment Constraint

Every allocated block of size $S = 2^k$ must start at an IP address whose decimal value in the active octet is an **exact integer multiple of** $S$.

---

Module 3: Network Layer Traversal & Device Visit Rules

1. Layer-Wise Device Capabilities

- **Router (Layer 3 Device)**: Inspects destination IP addresses, updates TTL/checksum, and forwards packets across subnet boundaries.
- **Switch (Layer 2 Device)**: Inspects destination MAC addresses to forward frames within a single subnet.
- **Hub / Repeater (Layer 1 Device)**: Operates strictly at the physical layer, replicating raw bits.

2. Data Link Layer Visit Count Rule

A packet visits the **Data Link Layer** whenever it enters or leaves an active interface operating at Layer 2 or higher.

```
  Source Host (A) =====> Switch (S) =====> Router (R) =====> Destination Host (B)
     [Visit 1]            [Visit 2]        [Visit 3, 4]           [Visit 5]
```

$$\text{Total Data Link Visits} = (2 \times \text{Routers}) + \text{Switches} + 2$$

---

Module 4: Step-by-Step Solved Problems

Problem 1: FLSM with 3-Bit Subnetting

- **Question**: Consider a classless IPv4 network `50.60.70.0/24`. The network is divided using 3-bit subnetting. Write the subnet addresses for all subnets.
- **Step-by-Step Solution**:
    1. Base Prefix $x = 24$, Subnet bits $y_1 = 3$, Host bits $y_2 = 32 - (24 + 3) = 5$.
    2. New Subnet Prefix $= 24 + 3 = \mathbf{/27}$.
    3. Number of Subnets $= 2^3 = \mathbf{8 \text{ Subnets}}$.
    4. Block Size $= 2^5 = \mathbf{32 \text{ Addresses per subnet}}$.
    5. Subnet Addresses progression in active 4th octet (step size 32):
        - **Subnet 1**: `50.60.70.0/27`
        - **Subnet 2**: `50.60.70.32/27`
        - **Subnet 3**: `50.60.70.64/27`
        - **Subnet 4**: `50.60.70.96/27`
        - **Subnet 5**: `50.60.70.128/27`
        - **Subnet 6**: `50.60.70.160/27`
        - **Subnet 7**: `50.60.70.192/27`
        - **Subnet 8**: `50.60.70.224/27`
- **Answer**: **8 subnets starting from** **50.60.70.0/27** **to** **50.60.70.224/27**.

---

Problem 2: FLSM with 4-Bit Subnetting on `/20` Network

- **Question**: Consider a network `60.70.80.0/20`. The network is divided using 4-bit subnetting. Write all subnet addresses.
- **Step-by-Step Solution**:
    1. Base Prefix $x = 20$, Subnet bits $y_1 = 4$, Host bits $y_2 = 32 - (20 + 4) = 8$.
    2. New Subnet Prefix $= 20 + 4 = \mathbf{/24}$.
    3. Number of Subnets $= 2^4 = \mathbf{16 \text{ Subnets}}$.
    4. Expand active 3rd octet ($80_{10} = 01010000_2$):
        - NetID (20 bits): `60.70.0101` | SubnetID (4 bits): `0000` to `1111` | HostID (8 bits).
    5. Progression of 3rd octet values ($80$ to $95$):
        - **1st Subnet**: `60.70.80.0/24`
        - **2nd Subnet**: `60.70.81.0/24`
        - **3rd Subnet**: `60.70.82.0/24`
        - ...
        - **16th Subnet**: `60.70.95.0/24`
- **Answer**: **16 subnets ranging from** **60.70.80.0/24** **to** **60.70.95.0/24**.

---

Problem 3: FLSM with 5-Bit Subnetting on `/20` Network

- **Question**: Consider network `60.70.80.0/20` divided using 5-bit subnetting. Write the first and last subnet details.
- **Step-by-Step Solution**:
    1. Base Prefix $x = 20$, Subnet bits $y_1 = 5$, Host bits $y_2 = 7$.
    2. New Subnet Prefix $= 20 + 5 = \mathbf{/25}$.
    3. Number of Subnets $= 2^5 = \mathbf{32 \text{ Subnets}}$.
    4. **First Subnet**: `60.70.80.0/25`
    5. **32nd (Last) Subnet**:
        - 3rd octet $= 95$, 4th octet SubnetID bit $= 1$ ($128$).
        - **Subnet Address**: `60.70.95.128/25`
        - **Subnet Broadcast**: `60.70.95.255/25`
- **Answer**: **32 subnets from** **60.70.80.0/25** **to** **60.70.95.128/25**.

---

Problem 4: Subnet & Broadcast Pair Verification

- **Question**: Network `183.185.176.0/20` is divided using 6-bit subnetting. Which options contain a correct pair of subnet address and subnet broadcast address for the same subnet?
    - (A) `183.185.176.0` and `183.185.176.31`
    - (B) `183.185.180.128` and `183.185.180.191`
    - (C) `183.185.184.0` and `183.185.188.63`
    - (D) `183.185.190.192` and `183.185.190.255`
- **Step-by-Step Solution**:
    1. Prefix $= 20 + 6 = \mathbf{/26}$.
    2. Host bits $y_2 = 32 - 26 = 6\text{ bits} \implies \text{Block Size} = 2^6 = \mathbf{64 \text{ Addresses}}$.
    3. A valid subnet of size 64 in the 4th octet must start at a multiple of 64 ($0, 64, 128, 192$) and end at $(\text{Start} + 63)$.
    4. **Check Option A**: Start $= 0$, Broadcast should be $63$ (Option says $31$) $\implies$ **Incorrect**.
    5. **Check Option B**: Start $= 128$, Broadcast $= 128 + 63 = 191 \implies$ **Correct**.
    6. **Check Option C**: 3rd octet mismatch ($184$ vs $188$) $\implies$ **Incorrect**.
    7. **Check Option D**: Start $= 192$, Broadcast $= 192 + 63 = 255 \implies$ **Correct**.
- **Answer**: **Options B and D**.

---

Problem 5: VLSM Allocation for 50%, 25%, 25% Split

- **Question**: Network `50.60.70.0/24` is divided into three subnets: Subnet 1 has approx 50% hosts, Subnets 2 and 3 each have approx 25% hosts. Write the subnet addresses.
- **Step-by-Step Solution**:
    1. Total Block Size $= 256 \text{ Addresses}$ (`/24`).
    2. **Subnet 1 (50% = 128 addresses)**: Needs $2^7$ size $\implies$ Prefix $= 32 - 7 = \mathbf{/25}$.
    3. **Subnets 2 & 3 (25% = 64 addresses each)**: Needs $2^6$ size $\implies$ Prefix $= 32 - 6 = \mathbf{/26}$.
    4. **Allocation Scheme**:
        - **Subnet 1**: `50.60.70.0/25` (Range `.0` to `.127`)
        - **Subnet 2**: `50.60.70.128/26` (Range `.128` to `.191`)
        - **Subnet 3**: `50.60.70.192/26` (Range `.192` to `.255`)
- **Answer**: **Subnet 1:** **50.60.70.0/25****, Subnet 2:** **50.60.70.128/26****, Subnet 3:** **50.60.70.192/26**.

---

Problem 6: Subnetting for Department Requirements

- **Question**: An organization has a 16-bit network identifier and wishes to form subnets for 64 departments. Find the required subnet mask.
    - (A) `255.255.0.0`
    - (B) `255.255.64.0`
    - (C) `255.255.128.0`
    - (D) `255.255.252.0`
- **Step-by-Step Solution**:
    1. Base NetID $x = 16$ bits.
    2. Subnets needed $= 64 \implies y_1 = \lceil \log_2 64 \rceil = \mathbf{6 \text{ Subnet bits}}$.
    3. Extended Prefix $= 16 + 6 = \mathbf{22 \text{ bits}}$.
    4. Subnet Mask (22 ones followed by 10 zeroes): `11111111 . 11111111 . 11111100 . 00000000` $= \mathbf{255.255.252.0}$.
- **Answer**: **Option D (****255.255.252.0****)**.

---

Problem 7: Subnet Membership Test

- **Question**: Machines M, N, and P have IPs `100.10.5.2`, `100.10.5.5`, and `100.10.5.6` respectively with subnet mask `255.255.255.252`. Which statement is true?
    - (A) M, N, and P all belong to the same subnet
    - (B) Only N and P belong to the same subnet
    - (C) Only M and N belong to the same subnet
    - (D) M, N, and P belong to three different subnets
- **Step-by-Step Solution**:
    1. Mask `255.255.255.252` $= \mathbf{/30}$ (Block Size $= 2^2 = \mathbf{4 \text{ Addresses}}$).
    2. Subnet boundaries in the 4th octet go in steps of 4:
        - **Block 1 (****.0 - .3****)**: Contains `100.10.5.2` (**Machine M**).
        - **Block 2 (****.4 - .7****)**: Contains `100.10.5.5` (**Machine N**) and `100.10.5.6` (**Machine P**).
    3. Therefore, only N and P share the same subnet.
- **Answer**: **Option B (Only N and P belong to the same subnet)**.

---

Problem 8: Feasible Subnet Allocation for Variable Sizes

- **Question**: A company has network address `204.204.204.0/24`. It wishes to have three subnets: one with 100 hosts and two with 50 hosts each. Which option represents a feasible set of subnet addresses?
- **Step-by-Step Solution**:
    1. **Subnet 1 (100 hosts)**: Needs $100 + 2 = 102$ addresses $\implies$ Size $128 = 2^7 \implies \mathbf{/25}$.
    2. **Subnets 2 & 3 (50 hosts each)**: Needs $50 + 2 = 52$ addresses $\implies$ Size $64 = 2^6 \implies \mathbf{/26}$.
    3. **Feasible Allocation Scheme**:
        - Allocate `/25` block at `204.204.204.0/25` or `204.204.204.128/25`.
        - Allocate remaining half into two `/26` blocks.
    4. The set containing `204.204.204.128/25`, `204.204.204.64/26`, and `204.204.204.0/26` cleanly covers the space without overlap.
- **Answer**: **204.204.204.128/25****,** **204.204.204.64/26****,** **204.204.204.0/26**.

---

Problem 9: CIDR Representation for Address Range

- **Question**: Which CIDR prefix exactly represents the range of IP addresses `10.12.2.0` to `10.12.3.255`?
- **Step-by-Step Solution**:
    1. Total addresses in range $= 2 \times 256 = \mathbf{512 \text{ Addresses}}$.
    2. Host bits $y = \log_2(512) = 9 \text{ bits}$.
    3. CIDR Prefix $= 32 - 9 = \mathbf{23 \text{ bits}} \implies \mathbf{10.12.2.0/23}$.
- **Answer**: **10.12.2.0/23**.

---

Problem 10: Last Assignable Host IP in Subnet

- **Question**: In the network `200.10.11.144/27`, find the fourth octet (in decimal) of the last IP address which can be assigned to a host.
- **Step-by-Step Solution**:
    1. Prefix $= 27 \implies$ Host bits $= 32 - 27 = 5 \text{ bits}$.
    2. Subnet Block Size $= 2^5 = 32 \text{ Addresses}$.
    3. Subnet Range $= .144$ to $.175$.
        - **Subnet Address**: `200.10.11.144`
        - **Subnet Broadcast**: `200.10.11.175`
        - **Last Usable Host IP**: $175 - 1 = \mathbf{158}$.
- **Answer**: **158**.