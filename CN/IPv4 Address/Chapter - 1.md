Comprehensive Master Notes: IPv4 Addressing Foundations, Special IPs & CIDR (Batch 1)

---

Module 1: IPv4 Addressing Foundations & Layer-Wise Architecture

1. Layer-Wise Network Addressing Structure

An IPv4 communication path relies on three distinct layers of addressing to deliver data packets from a source process to a destination process:

```
+------------------+------------------------+---------------------------------+
| OSI Layer        | Address Type           | Data Unit & Scope               |
+------------------+------------------------+---------------------------------+
| Transport Layer  | 16-Bit Port Number     | Process-to-Process (Segment)    |
| Network Layer    | 32-Bit IPv4 Address    | Host-to-Host (Datagram)         |
| Data Link Layer  | 48-Bit MAC Address     | Node-to-Node (Frame)            |
+------------------+------------------------+---------------------------------+
```

2. Binary vs. Dotted-Decimal Representation

- Machines process IPv4 addresses as **32-bit continuous binary strings**.
- Humans read IPv4 addresses in **Dotted-Decimal Notation**, where the 32 bits are divided into **four 8-bit octets (bytes)** separated by dots:

```
  32-bit Binary:   11011101 . 10011010 . 01100111 . 10000110
                   \______/   \______/   \______/   \______/
                     Byte 1     Byte 2     Byte 3     Byte 4
                       |          |          |          |
  Dotted Decimal:     221   .    154   .    103   .    134
```

3. IPv4 Address Division: NetID and HostID

Every IPv4 address is hierarchically partitioned into two structural parts:

- **Network Identifier (NetID —** $x$ **bits)**: Identifies the specific physical or logical network to which the device belongs.
- **Host Identifier (HostID —** $y$ **bits)**: Uniquely identifies a specific host interface within that network.

$$x + y = 32 \text{ bits}$$

```
  0                                        x x+1                           31 Bits
  +-----------------------------------------+-----------------------------------+
  |          Network ID (x bits)            |        Host ID (y bits)           |
  +-----------------------------------------+-----------------------------------+
```

---

Module 2: Special IPv4 Addresses & Transmission Modes

IPv4 defines three transmission modes (**Unicast**, **Broadcast**, and **Multicast**) and reserves specific address bit patterns for special network control functions.

```
                          SPECIAL IPv4 ADDRESSES
                                    |
         +--------------------------+--------------------------+
         |                          |                          |
   Network Address            Broadcast Address         Special Control
   (HostID = All 0s)                |                   - Limited Broadcast (255.255.255.255)
                                    |                   - "This Host" (0.0.0.0)
                        +-----------+-----------+       - Multicast Group IPs
                        |                       |
                 Directed Broadcast      Limited Broadcast
                 (HostID = All 1s)       (All 32 Bits = 1)
```

1. Network Address (Network ID)

- **Bit Pattern**: NetID = Assigned ($x$ bits), **HostID = All Zero Bits** ($y$ bits = $00\dots0$).
- **Function**: Represents the entire network block as a single entity in router forwarding tables.
- **Assignability**: **Non-assignable** to individual host network interfaces.

2. Directed Broadcast Address (DBA)

- **Bit Pattern**: NetID = Assigned ($x$ bits), **HostID = All One Bits** ($y$ bits = $11\dots1$).
- **Function**: Used to broadcast a packet to **all hosts on a targeted destination network** from a remote network across routers.
- **Assignability**: **Non-assignable** to host interfaces.

3. Limited Broadcast Address (LBA)

- **Bit Pattern**: **255.255.255.255** (All 32 bits set to `1`).
- **Function**: Used to broadcast a packet to **all hosts within the local network segment** to which the transmitting host belongs.
- **Router Constraint**: Routers **never forward** packets with destination IP `255.255.255.255` outside the local link boundary.
- **Primary Use Case**: Used during **DHCP Discovery** by an unconfigured booting client to locate a local DHCP server.

4. "This Host" Address

- **Bit Pattern**: **0.0.0.0** (All 32 bits set to `0`).
- **Function**: A temporary source address used by a booting host before it acquires an IP address via DHCP.

5. Multicast Group IP Addresses

- **Function**: Special IP addresses used as a **Group Identifier** (one-to-group delivery).
- **Header Rule**: In multicast packets, the **Source IP** is always a unicast host IP address, while the **Destination IP** is a multicast group address.

---

Module 3: Classless Inter-Domain Routing (CIDR) & Network Mask

Classless Inter-Domain Routing (CIDR) replaces rigid classful boundaries (Class A, B, C) with flexible, variable-length network prefixes.

1. CIDR Notation & Netmask Structure

- **CIDR Format**: Represented as **P.Q.R.S / x**, where $x$ denotes the prefix length (number of NetID bits).
- **HostID Bits (**$y$**)**: $$y = 32 - x$$
- **Network Mask (Netmask)**: A 32-bit value where the **first** $x$ **bits (NetID) are all set to 1**, and the **remaining** $y$ **bits (HostID) are all set to 0**.

```
  Netmask Example (/19):
  11111111 . 11111111 . 11100000 . 00000000
  \__________________/   \___/   \_______/
     16 One Bits       3 Ones    13 Zeroes   ===>  255.255.224.0
```

2. Bitwise AND Rule & Network Address Extraction

To find the Network Address of any host IP, perform a bitwise AND operation between the host IP address and the Netmask:

$$\text{Network Address} = \text{Host IP Address} \;\textbf{AND}\; \text{Netmask}$$

```
   Host IP:    115 . 120 . 01111101 . 10000010  (115.120.125.130)
   Netmask:    255 . 255 . 11100000 . 00000000  (255.255.224.0 /19)
   ---------------------------------------------------------------
   Net Addr:   115 . 120 . 01100000 . 00000000  (115.120.96.0)
```

3. The "Same Network" Rule

Two host IP addresses belong to the **same logical network** if and only if applying the network mask yields the **exact same Network Address** for both hosts.

4. Block Capacity Formulas

Given a network with prefix length $x$ and HostID bits $y = 32 - x$:

1. **Total IP Addresses in Block**: $$\text{Total Addresses} = 2^y = 2^{32 - x}$$
2. **Maximum Assignable Host IPs**: $$\text{Max Usable Host IPs} = 2^y - 2 = 2^{32 - x} - 2$$ _(Subtracting 2 for the Network Address and the Directed Broadcast Address)._
3. **First Assignable Host IP**: $$\text{First Host IP} = \text{Network Address} + 1$$
4. **Last Assignable Host IP**: $$\text{Last Host IP} = \text{Directed Broadcast Address} - 1$$

---

Module 4: Step-by-Step Solved Problems & Teacher Examples

Example 1: Binary to Dotted-Decimal Conversion

- **Problem**: Convert the 32-bit binary address `11011101 10011010 01100111 10000110` to dotted-decimal notation.
- **Step-by-Step Solution**:
    1. Octet 1: $11011101_2 = 128 + 64 + 0 + 16 + 8 + 4 + 0 + 1 = \mathbf{221}$
    2. Octet 2: $10011010_2 = 128 + 0 + 0 + 16 + 8 + 0 + 2 + 0 = \mathbf{154}$
    3. Octet 3: $01100111_2 = 0 + 64 + 32 + 0 + 0 + 4 + 2 + 1 = \mathbf{103}$
    4. Octet 4: $10000110_2 = 128 + 0 + 0 + 0 + 0 + 4 + 2 + 0 = \mathbf{134}$
- **Answer**: **221.154.103.134**.

---

Example 2: Network Address Calculation from NetID Bits

- **Problem**: A classless network has a 12-bit NetID assigned as `0110 0110 0101`. Identify the decimal network address.
- **Step-by-Step Solution**:
    1. NetID size = 12 bits, HostID size = $32 - 12 = 20\text{ bits}$.
    2. For the Network Address, set all 20 HostID bits to zero:

```
Binary: 01100110 01010000 00000000 00000000
        \______/ \______/ \______/ \______/
         Octet 1  Octet 2  Octet 3  Octet 4
```

- Octet 1: $01100110_2 = 102$
- Octet 2: $01010000_2 = 80$
- Octet 3: $00000000_2 = 0$
- Octet 4: $00000000_2 = 0$
- **Answer**: **102.80.0.0**.

---

Example 3: Directed Broadcast Address Calculation from NetID Bits

- **Problem**: A classless network has a 12-bit NetID assigned as `1001 1010 1100`. Identify the decimal broadcast address.
- **Step-by-Step Solution**:
    1. NetID size = 12 bits, HostID size = 20 bits.
    2. For the Directed Broadcast Address, set all 20 HostID bits to one:

```
Binary: 10011010 11001111 11111111 11111111
        \______/ \______/ \______/ \______/
         Octet 1  Octet 2  Octet 3  Octet 4
```

- Octet 1: $10011010_2 = 154$
- Octet 2: $11001111_2 = 207$
- Octet 3: $11111111_2 = 255$
- Octet 4: $11111111_2 = 255$
- **Answer**: **154.207.255.255**.

---

Example 4: Broadcast Address for Network `205.207.208.0/22`

- **Problem**: For a network with Network Address `205.207.208.0` and a 22-bit NetID, find the broadcast address.
- **Step-by-Step Solution**:
    1. NetID = 22 bits, HostID = 10 bits.
    2. Write `205.207.208.0` in binary focused on the active 3rd octet:
        - $208_{10} = 11010000_2$
        - Prefix (22 bits): `205.207. 110100` | HostID (10 bits): `00 00000000`
    3. Set all 10 HostID bits to 1:
        - Prefix + HostID: `205.207. 110100` | `11 11111111`
    4. Convert active 3rd octet back to decimal:
        - $11010011_2 = 211$
    5. Octet 4 = $11111111_2 = 255$.
- **Answer**: **205.207.211.255**.

---

Example 5: Network Address from Broadcast Address `110.120.131.255`

- **Problem**: For a network with broadcast address `110.120.131.255` and a 22-bit NetID, find the network address.
- **Step-by-Step Solution**:
    1. NetID = 22 bits, HostID = 10 bits.
    2. Expand active 3rd octet ($131_{10} = 10000011_2$):
        - Broadcast: `110.120. 100000` | `11 11111111`
    3. For Network Address, set all 10 HostID bits to 0:
        - Net Address: `110.120. 100000` | `00 00000000`
    4. Convert active 3rd octet to decimal:
        - $10000000_2 = 128$
- **Answer**: **110.120.128.0**.

---

Example 6: Host Range for NetID `1100 1100 1010` (12 bits)

- **Problem**: Given a 12-bit NetID `1100 1100 1010`, find the first and last assignable host IP addresses.
- **Step-by-Step Solution**:
    1. Base Network Address:
        - `11001100 10100000 00000000 00000000` = `204.160.0.0`
    2. Broadcast Address:
        - `11001100 10101111 11111111 11111111` = `204.175.255.255`
    3. First Assignable Host IP: `204.160.0.0 + 1` = **204.160.0.1**
    4. Last Assignable Host IP: `204.175.255.255 - 1` = **204.175.255.254**
- **Answer**: First IP = **204.160.0.1**, Last IP = **204.175.255.254**.

---

Example 7: Full Block Statistics for `115.120.125.130/19`

- **Problem**: For host IP `115.120.125.130/19`, calculate: Network Address, Broadcast Address, First Host IP, Last Host IP, and Max assignable hosts.
- **Step-by-Step Solution**:
    1. NetID = 19 bits, HostID = 13 bits.
    2. Expand 3rd octet ($125_{10} = 01111101_2$):
        - Host IP: `115.120. 011` | `11101 . 10000010`
    3. **Network Address**:
        - Set HostID bits to 0s: `115.120. 01100000 . 00000000`
        - $01100000_2 = 96$ $\implies$ **115.120.96.0/19**
    4. **Broadcast Address**:
        - Set HostID bits to 1s: `115.120. 01111111 . 11111111`
        - $01111111_2 = 127$ $\implies$ **115.120.127.255/19**
    5. **First Host IP**: **115.120.96.1/19**
    6. **Last Host IP**: **115.120.127.254/19**
    7. **Max Assignable Hosts**:
        - $2^{13} - 2 = 8192 - 2 = \mathbf{8190 \text{ hosts}}$.

---

Example 8: Max Hosts for 26-bit NetID

- **Problem**: Calculate the maximum number of IP addresses assigned to host interfaces for a 26-bit NetID network.
- **Step-by-Step Solution**:
    1. HostID bits ($y$) = $32 - 26 = 6\text{ bits}$.
    2. Max hosts = $2^6 - 2 = 64 - 2 = \mathbf{62}$.
- **Answer**: **62**.

---

Example 9: Netmask for 18-bit NetID

- **Problem**: Find the network mask for an 18-bit NetID.
- **Step-by-Step Solution**:
    1. Write 18 ones followed by 14 zeroes:
        - `11111111 . 11111111 . 11000000 . 00000000`
    2. Decimal conversion:
        - Octet 1 = 255
        - Octet 2 = 255
        - Octet 3 = $11000000_2 = 192$
        - Octet 4 = 0
- **Answer**: **255.255.192.0**.

---

Example 10: Max Hosts for Netmask `255.255.255.128`

- **Problem**: How many maximum hosts can be assigned in a network with mask `255.255.255.128`?
- **Step-by-Step Solution**:
    1. Octet 4 = $128_{10} = 10000000_2$ (1 NetID bit, 7 HostID bits).
    2. Total NetID bits = $8 + 8 + 8 + 1 = 25\text{ bits}$ (`/25`).
    3. HostID bits ($y$) = 7 bits.
    4. Max hosts = $2^7 - 2 = 128 - 2 = \mathbf{126}$.
- **Answer**: **126**.

---

Example 11 (MSQ): Same Network Test for Two Hosts

- **Problem**: Given two hosts `214.218.220.228` and `214.218.210.208`, determine which of the following netmasks allow them to belong to the same network:
    - (A) `255.255.192.0` (/18)
    - (B) `255.255.224.0` (/19)
    - (C) `255.255.240.0` (/20)
    - (D) `255.255.248.0` (/21)
- **Step-by-Step Solution**:
    1. Expand 3rd octets in binary:
        - Host 1 (`220`): $11011100_2$
        - Host 2 (`210`): $11010010_2$
    2. **Option A (****/18** **— mask 3rd octet** **11000000****)**:
        - Host 1: $11011100 \;\text{AND}\; 11000000 = 11000000 (192)$
        - Host 2: $11010010 \;\text{AND}\; 11000000 = 11000000 (192)$
        - Both yield `214.218.192.0` $\implies$ **VALID**
    3. **Option B (****/19** **— mask 3rd octet** **11100000****)**:
        - Host 1: $11011100 \;\text{AND}\; 11100000 = 11000000 (192)$
        - Host 2: $11010010 \;\text{AND}\; 11100000 = 11000000 (192)$
        - Both yield `214.218.192.0` $\implies$ **VALID**
    4. **Option C (****/20** **— mask 3rd octet** **11110000****)**:
        - Host 1: $11011100 \;\text{AND}\; 11110000 = 11010000 (208)$
        - Host 2: $11010010 \;\text{AND}\; 11110000 = 11010000 (208)$
        - Both yield `214.218.208.0` $\implies$ **VALID**
    5. **Option D (****/21** **— mask 3rd octet** **11111000****)**:
        - Host 1: $11011100 \;\text{AND}\; 11111000 = 11011000 (216)$
        - Host 2: $11010010 \;\text{AND}\; 11111000 = 11010000 (208)$
        - Mismatch (`214.218.216.0` vs `214.218.208.0`) $\implies$ **INVALID**
- **Answer**: **Options A, B, and C**.

---

GATE-2026 (Set-2) [1-Mark]

- **Problem**: An IP network uses a subnet mask of `255.255.240.0`. Calculate the maximum number of IP addresses that can be assigned to network interfaces.
- **Step-by-Step Solution**:
    1. Mask in binary: `255.255.11110000.0`
    2. Number of zero bits (HostID $y$) = $4 + 8 = 12\text{ bits}$.
    3. Max assignable IPs = $2^{12} - 2 = 4096 - 2 = \mathbf{4094}$.
- **Answer**: **4094**.

---

GATE-IT-2004 [1-Mark]

- **Problem**: A subnet has been assigned a subnet mask of `255.255.255.192`. What is the maximum number of hosts that can belong to this subnet?
- **Step-by-Step Solution**:
    1. Octet 4 = $192_{10} = 11000000_2$ (6 zero bits).
    2. HostID bits ($y$) = 6 bits.
    3. Max hosts = $2^6 - 2 = 64 - 2 = \mathbf{62}$.
- **Answer**: **62**.