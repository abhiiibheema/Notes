Module 1: Non-8-Byte MTU Payload Rounding Rules

When an IP datagram is fragmented at a host or router, the available payload capacity per fragment is calculated as:

$$\text{Max Payload Capacity} = \text{MTU} - \text{IP Header Size}$$

If $\text{Max Payload Capacity}$ is **not** a multiple of 8 bytes, the **8-byte alignment constraint** applies differently depending on whether the fragment is an intermediate fragment or the final fragment.

```
                    NON-ALIGNED MTU PAYLOAD SPLITTING
                    
    Incoming Payload = 600B | Link MTU = 300B | Header = 40B
    
    Calculated Max Payload = 300B - 40B = 260B (Not divisible by 8)
                                |
             +------------------+------------------+
             |                                     |
    Intermediate Fragments (MF = 1)        Last Fragment (MF = 0)
    -------------------------------        ----------------------
    Payload rounded DOWN to nearest        Carries remaining unsent
    multiple of 8 bytes:                   payload:
    floor(260 / 8) * 8 = 256 Bytes         600 - 256 = 344 Bytes
```

Rule Summary:

1. **Intermediate Fragments (****MF = 1****)**: The payload size **must be rounded down** to the nearest lower multiple of 8 bytes: $$\text{Intermediate Payload Size} = \left\lfloor \frac{\text{MTU} - \text{IP Header Size}}{8} \right\rfloor \times 8$$
2. **Last Fragment (****MF = 0****)**: Carries the remaining payload bytes without any 8-byte alignment restriction, provided the total fragment size does not exceed the MTU: $$\text{Last Fragment Payload} = \text{Remaining Unsent Payload} \le (\text{MTU} - \text{IP Header Size})$$
![[Pasted image 20261101131357.png]]

---

Module 2: The Don't Fragment (DF) Bit & Path MTU Discovery

The 3-bit Flags field in the IPv4 header controls whether intermediate routers are allowed to split datagrams.

```
                      DON'T FRAGMENT (DF = 1) ROUTER LOGIC
                      
   IP Datagram Arrives at Router (Total Length = 1500B, DF = 1)
                                 |
              Outgoing Link MTU = 1000B (1500B > 1000B)
                                 |
              +------------------+------------------+
              |                                     |
           DF == 0                               DF == 1
      (Fragmentation OK)                     (Do Not Fragment)
              |                                     |
      Fragment & Forward                     DISCARD DATAGRAM
                                                    |
                                            Send ICMP Error:
                                    "Fragmentation Needed & DF Set"
                                           (Type 3, Code 4)
```

Key Properties:

- **DF = 0**: Intermediate routers may fragment the datagram if it exceeds the outgoing link MTU.
- **DF = 1**: Intermediate routers are **forbidden** from fragmenting the datagram.
    - **Router Action on Over-Sized Packet**: If `Total Length > Link MTU` and `DF == 1`, the router **drops the packet** and returns an **ICMP Type 3, Code 4 Error Message** ("Fragmentation Needed and Don't Fragment Was Set") to the source host.
- **Primary Application**: Used in **Path MTU Discovery (PMTUD)** and **Remote Booting** to determine the smallest MTU along an end-to-end path without causing fragmentation in transit.

![[Pasted image 20261101131635.png|972]]

---

Module 3: Time-To-Live (TTL), Protocol Type & Header Checksum

```
 0                   8                   16                  31 Bits
+-------------------+-------------------+-------------------+
| Time-To-Live (8)  | Protocol Type (8) | Header Checksum (16)  |
+-------------------+-------------------+-------------------+
```

1. Time-To-Live (`TTL` — 8 bits)

- **Range**: Unsigned 8-bit integer (0 to 255).
- **Purpose**: Prevents IP datagrams from looping indefinitely in routing loops or circuits.
- **Router Decrement Rule**: Every intermediate router along the path **must decrement** **TTL** **by 1** (`TTL = TTL - 1`).
- **Expiration Action**: If a router decrements `TTL` to `0` (or `TTL <= 0`), the router **discards the datagram** and sends an **ICMP "Time Exceeded" Message (Type 11, Code 0)** back to the source host.

2. Protocol Type (8 bits)

- Identifies the higher-layer protocol encapsulated inside the IPv4 payload so the destination host can demultiplex the payload correctly:
    - `1` $\longrightarrow$ **ICMP**
    - `2` $\longrightarrow$ **IGMP**
    - `6` $\longrightarrow$ **TCP**
    - `17` $\longrightarrow$ **UDP**
    - `89` $\longrightarrow$ **OSPF**
- _Note_: Protocols like **RIP** run over UDP (`Protocol Type = 17`), whereas ICMP, IGMP, and OSPF run directly over IP.

3. Header Checksum (16 bits)

- **Coverage**: Calculated strictly over the **IPv4 Header ONLY** (base header + options). It does **NOT** cover payload data.
- **Algorithm**: 16-bit 1's complement sum of all 16-bit header words (with the Checksum field initialized to zero).
- **Mandatory Router Recomputation**: Because every intermediate router decrements `TTL` (and updates total length if fragmenting), **every router must recalculate and update the Header Checksum before forwarding the packet**.

![[Pasted image 20261101132653.png]]

---

Module 4: Mutable vs. Immutable IPv4 Header Fields

```
+-----------------------------------------------------------------------------------+
|                            ROUTER FIELD MODIFICATION SUMMARY                      |
+-------------------+----------------------------+----------------------------------+
| Field Category    | Header Fields              | Router Behavior                  |
+-------------------+----------------------------+----------------------------------+
| Always Modified   | Time-To-Live (TTL)         | Decremented by 1 at every hop    |
|                   | Header Checksum            | Recalculated at every hop        |
+-------------------+----------------------------+----------------------------------+
| Conditionally     | Total Length, Flags,       | Modified only if fragmentation   |
| Modified          | Offset                     | occurs at the router             |
|                   | Source IP, Destination IP  | Modified by NAT devices          |
+-------------------+----------------------------+----------------------------------+
| Constant /        | Version, Identification,   | Remains unchanged from sender    |
| Immutable         | Protocol Type              | to receiver                      |
+-------------------+----------------------------+----------------------------------+
```

---

Module 5: IPv4 Options Field

The **IPv4 Options** field extends the base 20-byte header up to 60 bytes (maximum 40 bytes for options).

```
 0                   8                   16                  31 Bits
+-------------------+-------------------+-------------------+-------------------+
| Option Type (8)   | Option Length (8) | Pointer (8)       | Route Data ...    |
+-------------------+-------------------+-------------------+-------------------+
```

Major Option Types:

1. **Record Route (Type 7)**:
    - Used to record the IP addresses of intermediate routers along the path.
    - **Maximum Router Address Limit**: Out of 40 option bytes, 3 bytes are used for overhead (`Type`, `Length`, `Pointer`), leaving 37 bytes for 4-byte IP addresses: $$\text{Max Recordable Router Addresses} = \left\lfloor \frac{37}{4} \right\rfloor = \mathbf{9 \text{ Routers}}$$
2. **Timestamp (Type 68)**:
    - Records both the 4-byte router IP address and a 32-bit timestamp (in milliseconds from UTC) at each hop (8 bytes per router).
3. **Strict Source Routing (Type 137)**:
    - The source host specifies the **exact mandatory path** of routers that the packet must visit. If a router cannot forward directly to the next listed router, the packet is dropped.
4. **Loose Source Routing (Type 131)**:
    - The source host specifies a set of required landmark routers, but the packet is allowed to traverse unlisted intermediate routers between them.

---

Module 6: Step-by-Step Solved Problems & PYQs

Problem 1: Non-Divisible MTU Payload Calculation

- **Question**: Consider a UDP segment of size 600 bytes passed to IPv4 for delivery. The MTU for the network is 300 bytes and the IPv4 header size is 40 bytes (`HLEN = 10`). Calculate the fragmentation offset value stored in the last fragment header.
- **Step-by-Step Solution**:
    1. **Maximum Payload Capacity**: $$\text{MTU} - \text{Header Size} = 300 - 40 = 260 \text{ Bytes}$$
    2. **Alignment Verification**: $260 / 8 = 32.5$ (Not a multiple of 8).
    3. **Intermediate Fragment Payload Size (****MF = 1****)**: $$\text{Payload}_{\text{intermediate}} = \lfloor 32.5 \rfloor \times 8 = 32 \times 8 = \mathbf{256 \text{ Bytes}}$$
    4. **Total Payload to Send**: 600 Bytes.
        - **Fragment 1**: Payload = 256B (`MF = 1`), Offset = $\frac{0}{8} = \mathbf{0}$
        - **Fragment 2**: Payload = 256B (`MF = 1`), Offset = $\frac{256}{8} = \mathbf{32}$
        - **Fragment 3 (Last)**: Payload = $600 - (256 + 256) = 88\text{B}$ (`MF = 0`), Offset = $\frac{512}{8} = \mathbf{64}$
- **Answer**: **64**.

---

Problem 2: Router Fragmentation Offset Calculation

- **Question**: An IP packet with a length of 4,500 bytes (including a 20-byte IPv4 header and a 40-byte TCP header) is forwarded to a router with link MTU = 600 bytes. All outgoing fragments use a 20-byte IP header. Calculate the fragmentation offset value stored in the third fragment.
- **Step-by-Step Solution**:
    1. **Total Payload Size**: $$4500 - 20 = 4480 \text{ Bytes}$$
    2. **Max Payload Capacity per Fragment**: $$\text{MTU} - \text{Header} = 600 - 20 = 580 \text{ Bytes}$$
    3. **Alignment Verification**: $580 / 8 = 72.5$ (Not a multiple of 8).
    4. **Intermediate Payload Size (****MF = 1****)**: $$\text{Payload}_{\text{intermediate}} = 72 \times 8 = \mathbf{576 \text{ Bytes}}$$
    5. **Offset of the Third Fragment**:
        - Fragment 1 payload = 576B (Bytes 0 to 575)
        - Fragment 2 payload = 576B (Bytes 576 to 1151)
        - Fragment 3 starts at Byte 1152: $$\text{Offset}_3 = \frac{1152}{8} = \mathbf{144}$$
- **Answer**: **144**.

---

Problem 3: Multi-Link Two-Router Multi-Hop Fragmentation

- **Question**: An IP datagram of size 1420 bytes (including 20 bytes IP header) is sent over a path of two links with a router in between. The first link MTU is 542 bytes, and the second link MTU is 360 bytes. Calculate the total number of fragments delivered at the receiver.
- **Step-by-Step Solution**:
    1. **Original Datagram**: Payload = $1420 - 20 = 1400 \text{ Bytes}$.
    2. **Fragmentation at Link 1 (MTU = 542B, Header = 20B)**:
        - Max payload capacity = $542 - 20 = 522\text{B}$.
        - Rounded intermediate payload = $\lfloor 522/8 \rfloor \times 8 = 65 \times 8 = 520 \text{ Bytes}$.
        - **Link 1 Output**:
            - Packet 1: Payload = 520B (`TL = 540B`)
            - Packet 2: Payload = 520B (`TL = 540B`)
            - Packet 3: Payload = 360B (`TL = 380B`)
    3. **Fragmentation at Link 2 Router (MTU = 360B, Header = 20B)**:
        - Max payload capacity = $360 - 20 = 340\text{B}$.
        - Rounded intermediate payload = $\lfloor 340/8 \rfloor \times 8 = 42 \times 8 = 336 \text{ Bytes}$.
        - **Processing Incoming Packets at Router 2**:
            - Packet 1 (520B payload > 340B) $\implies$ splits into **2 fragments** (336B + 184B).
            - Packet 2 (520B payload > 340B) $\implies$ splits into **2 fragments** (336B + 184B).
            - Packet 3 (360B payload > 340B) $\implies$ splits into **2 fragments** (336B + 24B).
    4. **Total Delivered Fragments**: $2 + 2 + 2 = \mathbf{6 \text{ Fragments}}$.
- **Answer**: **6**.

---

Problem 4: Fragment Loss & TCP Retransmission Rules

- **Question**: Consider two hosts P and Q connected through a router R. Link P-R MTU is 1500 bytes, and link R-Q MTU is 820 bytes. A TCP segment of 1400 bytes is sent from P to Q (IP header = 20 bytes, `DF = 0`). Which of the following statements is/are CORRECT?
    - **(A)** Two fragments are created at R and the IP datagram size carrying the second fragment is 620 bytes.
    - **(B)** If the second fragment is lost, R will resend the fragment with IP ID 0x1234.
    - **(C)** If the second fragment is lost, P is required to resend the whole TCP segment.
    - **(D)** TCP destination port can be determined by analyzing only the second fragment.
- **Step-by-Step Solution**:
    1. **Fragmentation Analysis at R**:
        - Payload = 1400B. Link R-Q MTU = 820B $\implies$ Max payload = $820 - 20 = 800\text{B}$.
        - Fragment 1 payload = 800B (`TL = 820B`).
        - Fragment 2 payload = 600B (`TL = 620B`).
        - **Statement A is TRUE**.
    2. **Retransmission Logic**:
        - Routers **never** retransmit lost fragments $\implies$ Statement B is FALSE.
        - Reassembly occurs only at destination Q. If any fragment is lost, TCP at destination cannot acknowledge the segment, prompting source P to retransmit the **entire TCP segment** $\implies$ **Statement C is TRUE**.
    3. **Header Inspection**:
        - TCP header (containing port numbers) resides exclusively in Fragment 1 (`Offset = 0`). Fragment 2 contains only raw payload bytes $\implies$ Statement D is FALSE.
- **Answer**: **Options A and C**.

---

Problem 5: Router Header Modification Verification

- **Question**: Which of the following fields of an IP header is/are ALWAYS modified by any router before it forwards the IP packet?
    - **(A)** Source IP Address
    - **(B)** Protocol Type
    - **(C)** Time to Live (TTL)
    - **(D)** Header Checksum
- **Step-by-Step Solution**:
    - Every intermediate router decrements `TTL` by 1 (`TTL--`).
    - Because `TTL` changes, the `Header Checksum` must always be recalculated.
    - Source IP and Protocol Type remain constant across standard routers.
- **Answer**: **Options C and D**.