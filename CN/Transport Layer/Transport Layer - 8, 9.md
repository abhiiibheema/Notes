TComprehensive Master Notes: Transport Layer 08 & 09 (TCP Flow Control, Congestion Control & Bandwidth-Delay Product)

---

Module 1: Flow Control vs. Congestion Control Architecture

The Transport Layer manages data delivery speed using two distinct mechanisms: **Flow Control** (preventing the sender from overwhelming the receiver) and **Congestion Control** (preventing the sender from overwhelming intermediate network links).

```
                             SENDER TRANSMIT WINDOW (W)
                                         |
                       W = min( CWND , RWND )
                                  |
         +------------------------+------------------------+
         |                                                 |
  RECEIVER WINDOW (RWND)                          CONGESTION WINDOW (CWND)
  - Set by Receiver Buffer                         - Set dynamically by Sender
  - Advertised in TCP Header                        - Inferred via Network Feedback (ACKs/Loss)
  - Prevents Receiver Buffer Overflow              - Prevents Network Link Congestion
```

The Dynamic Transmit Window Equation

At any instant, the maximum quantity of unacknowledged data payload bytes that TCP is permitted to transmit into the network is bounded by:

$$W = \min(\text{CWND}, \; \text{RWND})$$

Where:

- $\text{RWND}$ **(Receiver Window)**: The dynamic buffer space advertised by the receiver in the 16-bit **Window Size field** of the TCP header.
- $\text{CWND}$ **(Congestion Window)**: The maximum estimated capacity of the network path maintained locally by the sender.

---

Module 2: The Four Phases of TCP Congestion Control

TCP dynamically adjusts its $\text{CWND}$ by transitioning through defined algorithmic phases based on feedback from incoming ACKs and loss events.

```
                  TCP CONGESTION CONTROL STATE TRANSITIONS
                  
                       +-----------------------+
                       |      SLOW START       |
                       |  (Exponential Growth) |
                       +-----------------------+
                                   |
                         CWND >= SSTHRESH
                                   |
                                   v
                       +-----------------------+
                       | CONGESTION AVOIDANCE  |
                       |    (Linear Growth)    |
                       +-----------------------+
                           /               \
         3 Duplicate ACKs /                 \ Timeout Event
                         v                   v
         +-----------------------+   +-----------------------+
         |     FAST RECOVERY     |   |      SLOW START       |
         |  (CWND = SSTHRESH)    |   |    (CWND = 1 MSS)     |
         +-----------------------+   +-----------------------+
```

---

1. Slow Start Phase (Exponential Growth)

- **Initial State**: Upon establishing a new TCP connection (or after a timeout), $\text{CWND}$ is initialized to **1 MSS (Maximum Segment Size)**.
- **Slow Start Threshold (**$\text{SSTHRESH}$**)**: Initialized to a large default value (e.g., 64 KB or 32 MSS).
- **Update Rule (per ACK)**: For every valid ACK received: $$\text{CWND} = \text{CWND} + 1 \text{ MSS}$$
- **Effective Growth per RTT**: If an entire window of $N$ segments is successfully acknowledged within one Round-Trip Time (RTT), $\text{CWND}$ doubles: $$\text{CWND}_{\text{next RTT}} = 2 \times \text{CWND}_{\text{current RTT}}$$
- **Transition Trigger**: Slow Start terminates and hands control over to **Congestion Avoidance** as soon as: $$\text{CWND} \ge \text{SSTHRESH}$$

---

2. Congestion Avoidance Phase (Linear Growth / Additive Increase)

- **Goal**: Probe for extra network capacity cautiously without causing sudden packet loss.
- **Update Rule (per ACK)**: For every valid ACK received: $$\text{CWND} = \text{CWND} + \frac{1}{\text{CWND}} \text{ MSS}$$
- **Effective Growth per RTT**: Increases $\text{CWND}$ linearly by **1 MSS per RTT**: $$\text{CWND}_{\text{next RTT}} = \text{CWND}_{\text{current RTT}} + 1 \text{ MSS}$$

---

Module 3: Loss Detection & Multiplicative Decrease

TCP detects network congestion through two distinct signals: **Timeout Expiration** and **3 Duplicate ACKs**.

```
+-----------------------------------------------------------------------------------------+
|                              LOSS DETECTION MECHANISMS                                  |
+------------------------------------+----------------------------------------------------+
| Parameter / Trigger                | Timeout Expiration                                 |
+------------------------------------+----------------------------------------------------+
| Congestion Severity                | High / Severe (Whole pipe drained)                 |
| New Threshold (SSTHRESH)           | max( floor(CWND / 2) , 2 MSS )                      |
| Reset Value of CWND                | Reset to 1 MSS                                     |
| Next Operational Phase             | Restarts in SLOW START                             |
+------------------------------------+----------------------------------------------------+
| Parameter / Trigger                | 3 Duplicate ACKs (Fast Retransmit)                 |
+------------------------------------+----------------------------------------------------+
| Congestion Severity                | Moderate / Light (Out-of-order packets arriving)   |
| New Threshold (SSTHRESH)           | floor(CWND / 2)                                    |
| Reset Value of CWND (TCP Tahoe)    | Reset to 1 MSS                                     |
| Reset Value of CWND (TCP Reno)     | Set to SSTHRESH (Fast Recovery)                    |
| Next Operational Phase             | CONGESTION AVOIDANCE (Skips Slow Start in Reno)   |
+------------------------------------+----------------------------------------------------+
```

---

1. Fast Retransmit Algorithm

When a single segment is lost in a sequence, subsequent arriving segments generate **Duplicate ACKs** specifying the expected missing sequence number.

- Upon receiving **3 Duplicate ACKs** (4 identical ACKs total), the sender infers that a segment was lost rather than delayed.
- **Action**: Immediately retransmits the missing segment **without waiting for the Retransmission Timeout (RTO) timer to expire**.

2. Fast Recovery Algorithm (TCP Reno)

- After performing Fast Retransmit upon 3 Duplicate ACKs, TCP Reno skips the Slow Start phase entirely.
- Sets $\text{SSTHRESH} = \frac{\text{CWND}}{2}$ and $\text{CWND} = \text{SSTHRESH}$.
- Immediately enters **Congestion Avoidance**, growing $\text{CWND}$ linearly from $\text{SSTHRESH}$.

---

Module 4: Bandwidth-Delay Product (BDP) & Dynamic Window Scaling

The **Bandwidth-Delay Product (BDP)** defines the maximum quantity of data that can be "in flight" (filling the physical link transmission pipe) at any given moment.

```
                           BANDWIDTH-DELAY PRODUCT PIPE
                           
   Sender ========================================================> Receiver
   (Rate = Bandwidth B)          Pipe Volume = B * RTT              (Latency = RTT)
   ========================================================================
```

1. BDP Fundamental Equations

- **BDP Formula**: $$\text{BDP (in bits)} = \text{Bandwidth (in bits/sec)} \times \text{RTT (in seconds)}$$ $$\text{BDP (in Bytes)} = \frac{\text{Bandwidth (bits/sec)} \times \text{RTT (sec)}}{8}$$
- **Optimal Window Size for 100% Channel Utilization**: To continuously saturate a link without idling, the sender's window size must equal or exceed the BDP: $$W_{\text{optimal}} = \text{BDP (in Bytes)}$$

2. TCP Window Scale Option (RFC 1323)

- **Base TCP Header Limit**: The default TCP header field for `Window Size` is **16 bits**, restricting the maximum advertised window to $2^{16} - 1 = 65,535 \text{ Bytes} \approx 65.5 \text{ KB}$.
- **High-Bandwidth Delay Networks**: On high-speed long-haul networks (e.g., 1 Gbps with 100 ms RTT, $\text{BDP} \approx 12.5 \text{ MB}$), 65.5 KB limits utilization to under 1%.
- **Window Scale Factor (WSF)**: Configured during the 3-Way Handshake via TCP Options. Adds a shift count $S \le 14$, expanding the effective window limit to: $$\text{Effective Window} = \text{Advertised Window} \times 2^S \le 2^{30} \text{ Bytes (1 GB)}$$

---

Module 5: Step-by-Step Solved Problems & GATE PYQs

Problem 1: CWND Progression Analysis (Slow Start & Congestion Avoidance) [GATE-CS-2008]

- **Problem**: Consider a TCP connection operating in Slow Start and Congestion Avoidance phases. The Maximum Segment Size (MSS) is **2 KB**. The current Congestion Window ($\text{CWND}$) is **8 KB**, and the Slow Start Threshold ($\text{SSTHRESH}$) is **16 KB**. If the next **3 RTTs** experience no packet loss, what will be the value of $\text{CWND}$ after 3 RTTs?
- **Step-by-Step Solution**:
    1. **Initial Parameters**: $\text{CWND} = 8 \text{ KB} = 4 \text{ MSS}$, $\text{SSTHRESH} = 16 \text{ KB} = 8 \text{ MSS}$.
    2. Since $\text{CWND} < \text{SSTHRESH}$, TCP is in **Slow Start Phase** (Doubles every RTT):
        - **After RTT 1**: $\text{CWND} = 4 \times 2 = 8 \text{ MSS} = \mathbf{16 \text{ KB}}$
    3. Now $\text{CWND} == \text{SSTHRESH} = 16 \text{ KB}$. TCP transitions to **Congestion Avoidance Phase** (Increases by 1 MSS per RTT):
        - **After RTT 2**: $\text{CWND} = 8 \text{ MSS} + 1 \text{ MSS} = 9 \text{ MSS} = \mathbf{18 \text{ KB}}$
        - **After RTT 3**: $\text{CWND} = 9 \text{ MSS} + 1 \text{ MSS} = 10 \text{ MSS} = \mathbf{20 \text{ KB}}$
- **Answer**: **20 KB**.

---

Problem 2: Timeout Event Window Recalculation [GATE-2014, Set-3]

- **Problem**: Consider a TCP connection with $\text{MSS} = 1 \text{ KB}$. The current congestion window size is **32 KB**. A **timeout** occurs during transmission. Calculate the new values of $\text{SSTHRESH}$ and $\text{CWND}$ immediately after the timeout.
- **Step-by-Step Solution**:
    1. **Initial** $\text{CWND}$: $32 \text{ KB} = 32 \text{ MSS}$.
    2. **Timeout Rule Application**: $$\text{SSTHRESH}_{\text{new}} = \left\lfloor \frac{\text{CWND}}{2} \right\rfloor = \frac{32}{2} = \mathbf{16 \text{ KB}}$$ $$\text{CWND}_{\text{new}} = \mathbf{1 \text{ MSS} = 1 \text{ KB}}$$
- **Answer**: $\text{SSTHRESH} = 16 \text{ KB}$, $\text{CWND} = 1 \text{ KB}$.

---

Problem 3: TCP Tahoe vs. TCP Reno Dup ACK Handling [GATE-2017, Set-2]

- **Problem**: A TCP connection experiences congestion when its $\text{CWND}$ has reached **24 MSS**. The loss is detected by **3 Duplicate ACKs**. What will be the new $\text{CWND}$ values under **TCP Tahoe** and **TCP Reno**, respectively?
- **Step-by-Step Solution**:
    1. **Threshold Calculation (Both Protocols)**: $$\text{SSTHRESH}_{\text{new}} = \frac{24}{2} = 12 \text{ MSS}$$
    2. **TCP Tahoe Response**: On 3 Dup ACKs, Tahoe resets $\text{CWND}$ to **1 MSS**.
    3. **TCP Reno Response**: On 3 Dup ACKs, Reno sets $\text{CWND} = \text{SSTHRESH} = \mathbf{12 \text{ MSS}}$ (Fast Recovery).
- **Answer**: **Tahoe = 1 MSS, Reno = 12 MSS**.

---

Problem 4: Bandwidth-Delay Product & Optimal Window Sizing [GATE-2016, Set-1]

- **Problem**: A satellite link has a bandwidth of **100 Mbps** and a one-way propagation delay of **125 ms**. What is the minimum window size required in TCP to achieve **100% channel utilization**?
- **Step-by-Step Solution**:
    1. **One-Way Delay (**$t_p$**)**: $125 \text{ ms}$.
    2. **Round-Trip Time (RTT)**: $$\text{RTT} = 2 \times t_p = 2 \times 125 \text{ ms} = 250 \text{ ms} = 0.25 \text{ seconds}$$
    3. **Bandwidth-Delay Product (BDP)**: $$\text{BDP} = \text{Bandwidth} \times \text{RTT} = (100 \times 10^6 \text{ bits/sec}) \times 0.25 \text{ sec} = 25,000,000 \text{ bits}$$
    4. **Optimal Window Size in Bytes**: $$W_{\text{optimal}} = \frac{25,000,000}{8} = \mathbf{3,125,000 \text{ Bytes} \approx 3.125 \text{ MB}}$$
- **Answer**: **3,125,000 Bytes**.

---

Problem 5: Multi-Round CWND Progression with Timeout [GATE-2021, Set-1]

- **Problem**: A TCP sender starts with $\text{CWND} = 1 \text{ MSS}$ and $\text{SSTHRESH} = 16 \text{ MSS}$. In the **10th RTT**, a **timeout** occurs. Determine the value of $\text{CWND}$ at the **14th RTT**.
- **Step-by-Step Solution**:
    1. **Trace RTT 1 to RTT 9 (Slow Start up to SSTHRESH=16, then Congestion Avoidance)**:
        - RTT 1: 1 MSS
        - RTT 2: 2 MSS
        - RTT 3: 4 MSS
        - RTT 4: 8 MSS
        - RTT 5: 16 MSS (Reaches SSTHRESH)
        - RTT 6: 17 MSS (Congestion Avoidance starts)
        - RTT 7: 18 MSS
        - RTT 8: 19 MSS
        - RTT 9: 20 MSS
    2. **RTT 10 Timeout Event**: $\text{CWND}$ was 20 MSS. $$\text{SSTHRESH}_{\text{new}} = \left\lfloor \frac{20}{2} \right\rfloor = 10 \text{ MSS}$$ $$\text{CWND}_{\text{after RTT 10}} = 1 \text{ MSS}$$
    3. **Trace RTT 11 to RTT 14 (Restart in Slow Start)**:
        - RTT 11: 2 MSS
        - RTT 12: 4 MSS
        - RTT 13: 8 MSS
        - RTT 14: $\text{CWND}$ reaches $\text{SSTHRESH} = 10 \text{ MSS}$ (since doubling from 8 would exceed 10, it caps at $\text{SSTHRESH} = \mathbf{10 \text{ MSS}}$).
- **Answer**: **10 MSS**.