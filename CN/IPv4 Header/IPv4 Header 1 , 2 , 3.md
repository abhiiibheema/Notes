Module 1: IPv4 Datagram Architecture & Header Field Fundamentals

The **IPv4 Packet Header** ranges from a **minimum base size of 20 bytes (160 bits)** up to a **maximum size of 60 bytes (480 bits)** depending on the presence of optional header fields.

```
 0                   4                   8                   16                  31 Bits
+-------------------+-------------------+-------------------+-------------------+
|  VER (4 bits)     |   HLEN (4 bits)   |  Type of Service  | Total Length (16 bits)|
+-------------------+-------------------+-------------------+-------------------+
|    Identification Number (16 bits)    |0|D|M|   Fragmentation Offset (13 bits)   |
|                                       | |F|F|                                 |
+-------------------+-------------------+-----+-------------+-------------------+
| Time-To-Live (8)  | Protocol Type (8) |     Header Checksum (16 bits)         |
+---------------------------------------+---------------------------------------+
|                       Source IPv4 Address (32 bits)                           |
+-------------------------------------------------------------------------------+
|                     Destination IPv4 Address (32 bits)                        |
+-------------------------------------------------------------------------------+
|                   Optional Header / Options (0 to 40 Bytes)                   |
+-------------------------------------------------------------------------------+
|                                PAYLOAD DATA                                   |
+-------------------------------------------------------------------------------+
```

1. Core Header Fields & Scaling Rules

2. **Version (****VER** **— 4 bits)**:
    - Specifies the IP protocol version being used (fixed at `4` or `0100` in binary for IPv4).
3. **Header Length (****HLEN** **— 4 bits)**:
    - Defines the total size of the IPv4 header measured in **32-bit (4-byte) words**.
    - **Scaling Formula**: $$\text{Header Size in Bytes} = \text{HLEN} \times 4 \text{ Bytes}$$
    - **Valid Bound**: Since `HLEN` is a 4-bit field, its value ranges from $5_{10}$ (`0101`) to $15_{10}$ (`1111`): $$5 \le \text{HLEN} \le 15$$
        - **Minimum Header Size (Base Header)**: $5 \times 4 = \mathbf{20 \text{ Bytes}}$
        - **Maximum Header Size**: $15 \times 4 = \mathbf{60 \text{ Bytes}}$
4. **Options Field Size Calculation**:
    - Any header size beyond the 20-byte base header is allocated to the **IPv4 Options** field: $$\text{Options Size (in Bytes)} = (\text{HLEN} - 5) \times 4 \text{ Bytes}$$ $$\text{Max Options Size} = (15 - 5) \times 4 = \mathbf{40 \text{ Bytes}}$$
5. **Total Length (****TL** **— 16 bits)**:
    - Defines the complete size of the IPv4 datagram in **bytes**, including both the header and the payload: $$20 \le \text{Total Length} \le (2^{16} - 1) = \mathbf{65,535 \text{ Bytes}}$$
6. **Payload Size Formula**: $$\text{Payload Size} = \text{Total Length} - (\text{HLEN} \times 4) \text{ Bytes}$$
![[Pasted image 20261101123630.png]]
---

Module 2: Fragmentation Mechanics — Identification, Flags & Offset

When an IP packet exceeds the **Maximum Transmission Unit (MTU)** of a physical link, it must be broken into smaller pieces called **fragments**.

```
                         IPv4 FRAGMENT HEADER CONTROL
                         
   Original Datagram (TL = 2508B) ---> [ Identification No. = Y ]
                                              |
                   +--------------------------+--------------------------+
                   |                          |                          |
             Fragment 1                 Fragment 2                 Fragment 3
           +------------+             +------------+             +------------+
           | Id No. = Y |             | Id No. = Y |             | Id No. = Y |
           | MF = 1     |             | MF = 1     |             | MF = 0     |
           | Offset = 0 |             | Offset=110 |             | Offset=220 |
           +------------+             +------------+             +------------+
```

1. Identification Number (16 bits)

- Assigned by the sending host to uniquely identify all fragments originating from the same original IP datagram.
- **Rule**: All fragments created from the same original packet **retain the exact same Identification Number**.

2. Control Flags Field (3 bits)

- **Bit 0**: Reserved (must be 0).
- **Bit 1 — Don't Fragment (****DF****)**:
    - `DF = 1`: Instructs routers **never to fragment** the datagram. If the packet exceeds a link's MTU, it is dropped and an ICMP error is returned.
    - `DF = 0`: Fragmentation is permitted.
- **Bit 2 — More Fragments (****MF****)**:
    - `MF = 1`: Indicates that **more fragments follow** the current fragment.
    - `MF = 0`: Indicates that this is the **last fragment** in the sequence (or an unfragmented packet).

3. Fragmentation Offset Field (13 bits) & The 8-Byte Alignment Rule

- **Purpose**: Defines the relative starting position of a fragment's payload data relative to the original unfragmented payload.
- **Scaling Factor = 8**: To fit a 16-bit byte address space into a 13-bit header field, the offset measures payload position in **8-byte words** (13 bits + 3 bits from scaling = 16 bits): $$\text{Fragmentation Offset Value} = \frac{\text{Starting Byte Offset}}{8}$$
- **The Mandatory 8-Byte Alignment Rule**:
    - For all intermediate fragments (`MF = 1`), the payload size **MUST be a multiple of 8 bytes** so that the subsequent fragment's offset yields an integer value.
    - For the last fragment (`MF = 0`), the payload does **not** need to be a multiple of 8 bytes.

![[Pasted image 20261101124126.png]]

---

Module 3: Host vs. Router Fragmentation & Reassembly Rules

```
                      FRAGMENTATION & REASSEMBLY PATH
                      
  Source Host                    Intermediate Routers                    Destination Host
+--------------+                +---------------------+                +------------------+
| Creates IP   | -- IP Pkt -->  | Checks MTU.         | -- Fragments ->| ONLY Destination |
| Datagram as  |                | Fragments if packet |                | Reassembles All  |
| per Source   |                | size > link MTU.    |                | Fragments back   |
| MTU          |                | (No Reassembly!)    |                | to Payload       |
+--------------+                +---------------------+                +------------------+
```

Key Rules:

1. **Source Host Fragmentation**: Occurs at the sending host if the application message size exceeds the local network MTU.
2. **Intermediate Router Fragmentation**: Occurs at any router along the path where an incoming datagram size exceeds the next outgoing link MTU ($\text{Datagram Size} > \text{MTU}_{\text{OUT}}$).
3. **Destination-Only Reassembly**: **Reassembly is performed ONLY at the final destination host**. Intermediate routers **never** reassemble fragments because fragments may take different physical routes to the destination.

![[Pasted image 20261101124340.png]]

---

Module 4: Step-by-Step Solved Problems

Problem 1: HLEN & Options Field Calculation

- **Question**: The starting eight bits of an IPv4 packet are `0100 1000`. Calculate the size of the IPv4 packet options field in bytes.
- **Step-by-Step Solution**:
    1. First 4 bits (`0100`) = `VER = 4` (IPv4).
    2. Next 4 bits (`1000`) = `HLEN` $= 8_{10}$ words.
    3. Total Header Size $= 8 \times 4 = 32 \text{ Bytes}$.
    4. Options Field Size $= (\text{HLEN} - 5) \times 4 = (8 - 5) \times 4 = \mathbf{12 \text{ Bytes}}$.
- **Answer**: **12 Bytes**.

---

Problem 2: Payload Extraction from HLEN & Total Length

- **Question**: An IPv4 packet has a `Total Length` field value of 600 and an `HLEN` value of 12. Calculate the payload size in bytes.
- **Step-by-Step Solution**:
    1. `HLEN = 12 words` $\implies \text{Header Size} = 12 \times 4 = 48 \text{ Bytes}$.
    2. `Total Length = 600 Bytes`.
    3. $\text{Payload Size} = \text{Total Length} - \text{Header Size} = 600 - 48 = \mathbf{552 \text{ Bytes}}$.
- **Answer**: **552 Bytes**.

---

Problem 3: Complete UDP Segment Fragmentation Tracking

- **Question**: A UDP segment carries 2500 bytes of user data. MTU for the source network is 900 bytes and the IPv4 header size is 20 bytes (`HLEN = 5`). Calculate the total number of fragments, header values, and offsets for all fragments.
- **Step-by-Step Solution**:
    1. **Total UDP Segment Size**: $$\text{Payload Data} + \text{UDP Header (8B)} = 2500 + 8 = \mathbf{2508 \text{ Bytes}}$$
    2. **Maximum Payload per Fragment**: $$\text{Max Payload} = \text{MTU} - \text{Header Size} = 900 - 20 = 880 \text{ Bytes}$$ _(Check alignment: 880 is divisible by 8_ $\implies \frac{880}{8} = 110$ _words)_.
    3. **Total Fragments (**$N$**)**: $$N = \left\lceil \frac{2508}{880} \right\rceil = \lceil 2.85 \rceil = \mathbf{3 \text{ Fragments}}$$
    4. **Fragment Header Tracking Table**:

| Fragment       | Payload Size | Header Size | Total Length (`TL`) | `MF` Flag | Byte Range   | Offset Value                    |
| -------------- | ------------ | ----------- | ------------------- | --------- | ------------ | ------------------------------- |
| **Fragment 1** | 880 Bytes    | 20 Bytes    | **900 Bytes**       | **1**     | 0 to 879     | $\frac{0}{8} = \mathbf{0}$      |
| **Fragment 2** | 880 Bytes    | 20 Bytes    | **900 Bytes**       | **1**     | 880 to 1759  | $\frac{880}{8} = \mathbf{110}$  |
| **Fragment 3** | 748 Bytes    | 20 Bytes    | **768 Bytes**       | **0**     | 1760 to 2507 | $\frac{1760}{8} = \mathbf{220}$ |

---

Problem 4: Fragment Identification & Sequence Position

- **Question**: In an IPv4 datagram, the `M` bit is 0, `HLEN = 10`, `Total Length = 400`, and `Fragment Offset = 300`. Determine the fragment position and the sequence numbers of the first and last payload bytes.
- **Step-by-Step Solution**:
    1. **Fragment Position**: Since `M bit (MF) = 0`, this is the **Last Fragment**.
    2. **Header Size**: $\text{HLEN} \times 4 = 10 \times 4 = 40 \text{ Bytes}$.
    3. **Payload Size**: $\text{Total Length} - \text{Header Size} = 400 - 40 = 360 \text{ Bytes}$.
    4. **First Byte Sequence Position**: $$\text{First Byte} = \text{Offset} \times 8 = 300 \times 8 = \mathbf{2400}$$
    5. **Last Byte Sequence Position**: $$\text{Last Byte} = \text{First Byte} + \text{Payload Size} - 1 = 2400 + 360 - 1 = \mathbf{2759}$$
- **Answer**: **Last fragment, 2400 and 2759**.

---

Problem 5: Ethernet LAN UDP Fragmentation

- **Question**: Host A sends a UDP datagram containing 8880 bytes of user data to Host B over an Ethernet LAN (`MTU = 1500 bytes`). UDP header size is 8 bytes, IP header size is 20 bytes. Calculate the total number of IP fragments and the offset value in the last fragment.
- **Step-by-Step Solution**:
    1. **Total UDP Segment Size**: $$8880 + 8 = \mathbf{8888 \text{ Bytes}}$$
    2. **Max Payload Size per Fragment**: $$\text{MTU} - \text{IP Header} = 1500 - 20 = 1480 \text{ Bytes}$$ _(Check alignment:_ $1480 / 8 = 185$ _words_ $\implies$ _valid multiple of 8)_.
    3. **Total Number of Fragments (**$N$**)**: $$N = \left\lceil \frac{8888}{1480} \right\rceil = \lceil 6.005 \rceil = \mathbf{7 \text{ Fragments}}$$
    4. **Offset Value of the Last (7th) Fragment**: $$\text{Offset} = \frac{(N - 1) \times \text{Max Payload}}{8} = \frac{6 \times 1480}{8} = \frac{8880}{8} = \mathbf{1110}$$
- **Answer**: **7 fragments, Offset = 1110**.

---

Problem 6: Ethernet Frame UDP Payload Fragmentation

- **Question**: A UDP segment carrying a 7488-byte payload is transmitted over Ethernet (`MTU = 1500 bytes`, IP header = 20 bytes, no options). Find the total number of fragments transmitted and the size of the last fragment including the IPv4 header.
- **Step-by-Step Solution**:
    1. **Total UDP Segment Size**: $$7488 + \text{UDP Header (8B)} = \mathbf{7496 \text{ Bytes}}$$
    2. **Max Payload Size per Fragment**: $$1500 - 20 = 1480 \text{ Bytes}$$
    3. **Total Number of Fragments (**$N$**)**: $$N = \left\lceil \frac{7496}{1480} \right\rceil = \lceil 5.06 \rceil = \mathbf{6 \text{ Fragments}}$$
    4. **Payload in First 5 Fragments**: $$5 \times 1480 = 7400 \text{ Bytes}$$
    5. **Payload in Last (6th) Fragment**: $$7496 - 7400 = 96 \text{ Bytes}$$
    6. **Total Size of Last Fragment (including IP Header)**: $$96 + 20 = \mathbf{116 \text{ Bytes}}$$
- **Answer**: **6 fragments, 116 bytes total size for the last fragment**.