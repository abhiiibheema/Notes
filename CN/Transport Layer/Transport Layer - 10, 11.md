Module 1: The Five TCP Timers Architecture

Transmission Control Protocol (TCP) utilizes **five specialized timers** to manage lost packets, eliminate communication deadlocks, optimize network transmission efficiency, and manage socket handle lifecycles.

```
                             THE FIVE TCP TIMERS
                                      |
         +----------------------------+----------------------------+
         |                            |                            |
  1. Retransmission Timer      2. Persistent Timer           3. Keep-Alive Timer
     (Handles Lost Segments)      (Prevents Zero-Window         (Prevents Idle Deadlocks)
                                   Size Deadlock)
         |                            |
         +----------------------------+----------------------------+
         |                                                         |
  2. Time-Waited Timer                                      5. Delayed Timer
     (Connection Teardown; 2 * MSL)                            (Piggybacks ACKs; ~200ms)
```

1. **Retransmission Timer**: Manages timeouts for unacknowledged segments and triggers retransmissions.
2. **Persistent Timer**: Prevents "Zero-Window Size Deadlock" when a receiver buffer reopens.
3. **Keep-Alive Timer**: Detects crashed or disconnected peers to prevent long idle TCP connections.
4. **Time-Waited Timer**: Ensures reliable connection termination by remaining active for $2 \times \text{MSL}$ before closing the socket.
5. **Delayed Timer**: Optimizes network traffic by delaying ACKs for a brief window (e.g., 200 ms) so they can piggyback on outgoing data segments.

---

Module 2: Dynamic Retransmission Time-Out (RTO) Calculations

Because Internet paths vary dynamically in latency and jitter, TCP uses **dynamic retransmission timeout (RTO)** estimation algorithms rather than static timer values. For initial `SYN` segments during connection setup, RTO defaults to **1 or 3 seconds**.

```
                     RTO CALCULATION FLOW (JACOBSON'S ALGORITHM)
                     
   [ SampleRTT / MeasuredRTT ] = Time from segment transmission to ACK receipt
                                       |
                                       v
   [ SmoothedRTT ]  <--- Exponential Weighted Moving Average (alpha = 0.125)
                                       |
                                       v
   [ DeviatedRTT ]  <--- Mean Deviation Estimation (beta = 0.25)
                                       |
                                       v
   [ RTO Value ]    =  SmoothedRTT + 4 * DeviatedRTT   (Must be >= SmoothedRTT)
```

1. Measured RTT (SampleRTT)

- **Definition**: The measured time elapsed from transmitting a segment until receiving its corresponding ACK.

2. Smoothed RTT (EstimatedRTT)

Smooths out short-term network fluctuations using an **Exponential Weighted Moving Average (EWMA)**:

- **First Measurement**: $$\text{SmoothedRTT} = \text{MeasuredRTT}$$
- **Subsequent Measurements**: $$\text{SmoothedRTT} = (1 - \alpha) \cdot \text{SmoothedRTT} + \alpha \cdot \text{MeasuredRTT}$$ Where the standard smoothing weight is $\alpha = 0.125 = \frac{1}{8}$.

3. Deviated RTT (VariationRTT)

Estimates variance/jitter in Round-Trip Times:

- **First Measurement**: $$\text{DeviatedRTT} = \frac{\text{MeasuredRTT}}{2}$$
- **Subsequent Measurements**: $$\text{DeviatedRTT} = (1 - \beta) \cdot \text{DeviatedRTT} + \beta \cdot |\text{MeasuredRTT} - \text{SmoothedRTT}|$$ Where the standard deviation weight is $\beta = 0.25 = \frac{1}{4}$.

4. Final RTO Formula & Safety Bounds

$$\text{RTO Value} = \text{SmoothedRTT} + 4 \cdot \text{DeviatedRTT}$$

- **Safety Bound**: The computed RTO value must always satisfy: $$\text{RTO Value} \ge \text{SmoothedRTT}$$

5. Karn's Algorithm (Ambiguity Resolution & Exponential Backoff)

- **SampleRTT Ambiguity**: If a segment times out and is retransmitted, receiving an ACK creates ambiguity: does the ACK correspond to the original transmission or the retransmitted segment?
- **Karn's Rule 1**: **Do not update SampleRTT or SmoothedRTT using ACKs for retransmitted segments.**
- **Karn's Rule 2 (Exponential Backoff)**: Upon each consecutive retransmission timeout, double the current RTO value: $$\text{RTO}_{\text{new}} = 2 \times \text{RTO}_{\text{current}}$$

---

Module 3: Persistent & Keep-Alive Timers (Deadlock Prevention)

1. Zero-Window Size Deadlock & Persistent Timer

- **Deadlock Origin**:
    1. A TCP receiver advertises `Window Size = 0` ($\text{RWND} = 0$) when its receive buffer becomes full.
    2. The sender halts data transmission.
    3. Later, the receiver application reads data, freeing buffer space (e.g., 200 bytes), and sends an ACK with `Window Size = 200`.
    4. If this ACK packet is **lost in transit**, the sender continues waiting for a non-zero window update while the receiver waits for new data, creating an **indefinite deadlock**.

```
                 ZERO-WINDOW DEADLOCK RESOLUTION
                 
      TCP Sender                                  TCP Receiver
          |                                             |
          | <--- ACK (Window Size = 0) ---------------- |  (Buffer Full)
  (RWND = 0; Starts                                     |
   Persistent Timer)                                    |  (Buffer Frees 200B)
          | <--- ACK (Window Size = 200) [LOST!] --X--- |
          |                                             |
   (Timer Expires)                                      |
          | --- Probe Segment (Payload = 1 Byte) -----> |
          | <-- ACK (Window Size = 200) --------------- |  (Resends Window Size)
```

- **Resolution**: When `Window Size = 0` is received, the sender starts the **Persistent Timer**. When it expires, the sender transmits a **Probe Segment** containing **1 byte of dummy payload**. The receiver responds with an ACK containing its current available window size.

2. Keep-Alive Timer Mechanics

- **Purpose**: Prevents long idle TCP connections from consuming socket resources when a peer host crashes or becomes physically disconnected without executing a close handshake.
- **Mechanism**: Every time TCP receives a data or ACK segment from a peer, it resets the **Keep-Alive Timer** (typically set to 2 hours). If no communication occurs before the timer times out, TCP transmits a **Probe Segment** with **0 payload bytes**. If the peer fails to respond to consecutive probes, the socket is forcibly closed.

---

Module 4: Maximum Segment Size (MSS) & Framing Constraints

The **Maximum Segment Size (MSS)** specifies the maximum number of **payload bytes** a TCP segment can carry.

```
                      TCP SEGMENT & MSS STRUCTURE
                      
  +-------------------------------------------------------------------+
  |                       IP Packet (<= MTU)                          |
  |  +---------------------+---------------------------------------+  |
  |  |   IP Header Size    |           TCP Segment Size            |  |
  |  +---------------------+---------------------------------------+  |
  |                        |  TCP Header Size  |   MSS Payload     |  |
  |                        +-------------------+-------------------+  |
  +-------------------------------------------------------------------+
```

1. Mathematical Formulas

$$\text{Max TCP Segment Size} = \text{TCP Header Size} + \text{MSS}$$ $$\text{MSS} = \text{MTU} - \text{IP Header Size} - \text{TCP Header Size}$$

Where **MTU (Maximum Transmission Unit)** is the maximum packet size permitted by the physical link layer (e.g., 1500 bytes for standard Ethernet).

2. MSS Clamping Negotiation

During the 3-Way Handshake, both endpoints advertise their local MSS constraints in `SYN` segments using **TCP Options**. The final operational MSS for the connection is clamped to the minimum of the two advertised values: $$\text{Connection MSS} = \min(\text{MSS}_{\text{Client}}, \; \text{MSS}_{\text{Server}})$$

---

Module 5: Complete TCP Header Structure & TCP Options

```
 0                   16                  31 Bits
+-------------------+-------------------+
|  Source Port (16) | Destination Port(16)|
+-------------------+-------------------+
|          Sequence Number (32)         |
+---------------------------------------+
|        Acknowledgment Number (32)     |
+-------+-------+---+-------------------+
|HLEN(4)|Rsvd(6)|Flg|  Window Size (16)  |
+-------+-------+---+-------------------+
|   Checksum (16)   | Urgent Pointer(16)|
+-------------------+-------------------+
|  Options & Padding (0 to 40 Bytes)    |  <-- Max 10 Words (40 Bytes)
+---------------------------------------+
|                Payload                |
+---------------------------------------+
```

1. Header Field Specifications

- **Base Header Size**: **20 Bytes** (5 words of 32 bits).
- **Maximum Header Size**: **60 Bytes** (15 words of 32 bits).
- **HLEN / Data Offset (4 bits)**: Specifies total header size in 32-bit (4-byte) words.
    - $\text{Min HLEN} = 5 \quad (0101_2 \to 5 \times 4 = 20 \text{ Bytes})$
    - $\text{Max HLEN} = 15 \quad (1111_2 \to 15 \times 4 = 60 \text{ Bytes})$
- **Control Flags (6 bits)**: `URG`, `ACK`, `PSH`, `RST`, `SYN`, `FIN`.
- **Window Size (16 bits)**: Advertises available receive buffer space in bytes (max 65,535 bytes without scaling).

2. TCP Options Field (0 to 40 Bytes)

Each option component consists of: **Option Kind (1 byte)**, **Option Length (1 byte)**, and **Option Data**. If options do not end on a 32-bit boundary, **Padding (Kind 0)** is added.

|Option Kind|Option Length|Purpose / Function|
|---|---|---|
|**0**|—|**End of Option List (Padding)**|
|**1**|—|**No Operation (NOP)**|
|**2**|4 Bytes|**Maximum Segment Size (MSS)**|
|**3**|3 Bytes|**Window Scale Factor (Shift count** $S \le 14$**)**|
|**4**|2 Bytes|**Selective ACK Permitted (SACK Permitted)**|
|**5**|10 / 18 / 26 / 34 Bytes|**Selective ACKs (SACK Data Blocks)**|
|**8**|10 Bytes|**Timestamps (RTT Estimation & PAWS)**|

---

Module 6: User Datagram Protocol (UDP) Header & Mechanics

UDP is a connectionless, lightweight transport protocol providing process-to-process message transfer without guaranteed delivery, reordering, or flow/congestion control.

```
 0                   16                  31 Bits
+-------------------+-------------------+
|  Source Port (16) | Destination Port(16)|
+-------------------+-------------------+  <-- Fixed Header Size = 8 Bytes
|  Total Length (16)|   Checksum (16)   |
+-------------------+-------------------+
|                Payload                |
+---------------------------------------+
```

- **Fixed Header Size**: **8 Bytes** (4 fields of 16 bits).
- **Total Length (16 bits)**: Specifies total size of the UDP segment in bytes (Header + Payload).
- **Maximum UDP Segment Size**: $2^{16} - 1 = 65,535 \text{ Bytes}$.
- **Demultiplexing Scheme**: Identified using a **2-Tuple**: `(Destination IP Address, Destination Port Number)`.

---

Module 7: Step-by-Step Solved Problems & GATE/ISRO PYQs

Problem 1: Minimum Sequence Field Bits for 60s Non-Wrap [GATE 2026, Set-2]

- **Problem**: Design a link-layer protocol between two hosts directly connected over a link of length **3000 km**. Link bandwidth is $10^8 \text{ bits/sec}$ and propagation delay is **5 ns per meter**. Every byte is assigned a unique sequence number. Find the minimum sequence field bits $N$ such that sequence numbers do not wrap around before **60 seconds**.
- **Step-by-Step Solution**:
    1. **Maximum Bounded Duration**: $T = 60 \text{ seconds}$.
    2. **Bandwidth in Bytes/sec**: $$\text{Bandwidth} = \frac{10^8 \text{ bits/sec}}{8} = 1.25 \times 10^7 \text{ Bytes/sec}$$
    3. **Total Bytes Transmitted in 60 Seconds**: $$\text{Total Bytes} = 60 \times (1.25 \times 10^7) = 7.5 \times 10^8 \text{ Bytes}$$
    4. **Minimum Sequence Bits (**$N$**)**: $$N = \lceil \log_2(7.5 \times 10^8) \rceil = \lceil 29.48 \rceil = \mathbf{30 \text{ bits}}$$
- **Answer**: **30 bits**.

---

Problem 2: CWND Progression in Time t + 60 ms [GATE 2020]

- **Problem**: A TCP connection has $\text{RTT} = 6 \text{ ms}$, $\text{RWND} = 50 \text{ KB}$, $\text{SSTHRESH} = 32 \text{ KB}$, and $\text{MSS} = 2 \text{ KB}$. Connection establishes at $t=0$. Calculate $\text{CWND}$ (in KB) at $t + 60 \text{ ms}$.
- **Step-by-Step Solution**:
    1. Convert to MSS units:
        - $\text{SSTHRESH} = \frac{32 \text{ KB}}{2 \text{ KB}} = 16 \text{ MSS}$
        - $\text{RWND} = \frac{50 \text{ KB}}{2 \text{ KB}} = 25 \text{ MSS}$
    2. Number of RTT rounds in 60 ms: $\frac{60 \text{ ms}}{6 \text{ ms}} = 10 \text{ RTTs}$.
    3. Trace $\text{CWND}$ progression across RTT rounds:
        - **Slow Start Phase** (Doubles every RTT up to 16 MSS):
            - RTT 1: 2 MSS, RTT 2: 4 MSS, RTT 3: 8 MSS, RTT 4: 16 MSS (Reaches SSTHRESH).
        - **Congestion Avoidance Phase** (+1 MSS per RTT):
            - RTT 5: 17 MSS, RTT 6: 18 MSS, RTT 7: 19 MSS, RTT 8: 20 MSS, RTT 9: 21 MSS, RTT 10: **22 MSS**.
    4. Convert back to KB: $22 \text{ MSS} \times 2 \text{ KB} = \mathbf{44 \text{ KB}}$.
- **Answer**: **44 KB**.

---

Problem 3: Time Required to Recover CWND After Timeout [GATE 2014, Set-1]

- **Problem**: A TCP connection has $\text{CWND} = 32 \text{ KB}$ when a timeout occurs. Given $\text{RTT} = 100 \text{ ms}$ and $\text{MSS} = 2 \text{ KB}$, calculate the time taken (in ms) to get back to **32 KB** $\text{CWND}$.
- **Step-by-Step Solution**:
    1. Initial $\text{CWND} = \frac{32 \text{ KB}}{2 \text{ KB}} = 16 \text{ MSS}$.
    2. On timeout: $\text{SSTHRESH}_{\text{new}} = \frac{16}{2} = 8 \text{ MSS}$, and $\text{CWND}$ resets to **1 MSS**.
    3. Trace RTT rounds back to 16 MSS (32 KB):
        - RTT 1: 2 MSS, RTT 2: 4 MSS, RTT 3: 8 MSS (Slow Start complete).
        - RTT 4: 9 MSS, RTT 5: 10 MSS, RTT 6: 11 MSS, RTT 7: 12 MSS, RTT 8: 13 MSS, RTT 9: 14 MSS, RTT 10: 15 MSS, RTT 11: **16 MSS**.
    4. Total time = $11 \text{ RTTs} \times 100 \text{ ms} = \mathbf{1100 \text{ ms}}$.
- **Answer**: **1100 ms**.

---

Problem 4: Ideal TCP Window Size for BDP [ISRO 2025]

- **Problem**: What is the ideal TCP window size of a sender to fully utilize a link of bandwidth **8,192,000 bps** and latency **32 ms**?
- **Step-by-Step Solution**:
    1. $\text{Bandwidth} = 8,192,000 \text{ bps}$.
    2. $\text{BDP in bits} = (8,192,000 \text{ bps}) \times (0.032 \text{ sec}) = 262,144 \text{ bits}$.
    3. $\text{BDP in Bytes} = \frac{262,144}{8} = 32,768 \text{ Bytes} = \mathbf{2^{15} \text{ Bytes}}$.
- **Answer**: **32,768 Bytes** (Option D).

---

Problem 5: Window Scale Option Thresholds [GATE 2015, Set-2]

- **Problem**: For a TCP connection with bandwidth **1,048,560 bits/sec**, let $\alpha$ be the RTT in ms after which the TCP window scale option is needed, and $\beta$ be the maximum possible window size with scaling. Find $\alpha$ and $\beta$.
- **Step-by-Step Solution**:
    1. Max window without scaling = $2^{16} - 1 = 65,535 \text{ Bytes}$.
    2. $\alpha = \frac{(65,535 \times 8) \text{ bits}}{1,048,560 \text{ bits/sec}} = 0.5 \text{ seconds} = \mathbf{500 \text{ ms}}$.
    3. Max scale shift count in TCP is **14** $\implies \text{Scale Factor} = 2^{14}$.
    4. $\beta = 65,535 \times 2^{14} \text{ Bytes}$.
- **Answer**: **500 ms**, $65,535 \times 2^{14}$ (Option C).

---

Problem 6: TCP Timeout Statements Verification [GATE-IT-2007]

- **Problem**: Consider statements about TCP timeout values:
    - (i) Timeout is set to measured RTT during connection establishment for the entire connection duration.
    - (ii) Appropriate RTT estimation algorithm is used to set the timeout value.
    - (iii) Timeout value is set to twice the propagation delay.
- **Step-by-Step Solution**:
    - Statement (i) is False (RTO updates dynamically).
    - Statement (ii) is True (Uses Jacobson/Karn EWMA algorithm).
    - Statement (iii) is False.
- **Answer**: **(i) and (iii) are false, but (ii) is true** (Option B).