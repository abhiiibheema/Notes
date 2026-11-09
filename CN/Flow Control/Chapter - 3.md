Master Notes: Selective Repeat (SR) ARQ, Piggybacking & Protocol Comparison (Batch 3)

---

Module 1: Selective Repeat (SR) ARQ Mechanics

While Go-Back-N ARQ improves sender throughput by pipelining, it wastes link bandwidth during error recovery by retransmitting all $N$ frames when a single frame is lost. **Selective Repeat (SR) ARQ** solves this inefficiency by retransmitting **only the specifically corrupted or lost frame**.

```
                         SELECTIVE REPEAT (SR) ARQ LOGIC
                         
   Sender Window (Wt = N)                                 Receiver Window (Wr = N)
  +-----------------------+                              +-----------------------+
  | Buffers up to N       |                              | Buffers up to N       |
  | unacknowledged frames |                              | out-of-order frames   |
  +-----------------------+                              +-----------------------+
              |                                                      |
              v                                                      v
   Selective Retransmission                              Independent ACK / NACK
   Resends ONLY the specific                             ACKs each valid frame
   missing or corrupt frame                              independently
```

```
                     SELECTIVE REPEAT FRAME LOSS & RECOVERY
                     
   Sender (Window = 4: F0, F1, F2, F3)                       Receiver (Window = 4: Expected 0,1,2,3)
     | --- Frame 0 ------------------------------------------> | Accepts F0; Sends ACK 0
     | --- Frame 1 (LOST IN TRANSIT!) X                       | 
     | --- Frame 2 ------------------------------------------> | Buffers F2; Sends ACK 2 / NACK 1
     | --- Frame 3 ------------------------------------------> | Buffers F3; Sends ACK 3
     |                                                         |
     | [Timer Expires / NACK 1 Received for Frame 1]           |
     | === Retransmits ONLY Frame 1 =========================> | Accepts F1; Delivers F0,F1,F2,F3
     |                                                         | Window slides forward!
```

Key Protocol Rules

1. **Receiver Buffering (**$W_r = N$**)**: The receiver allocates buffer space for $N$ frames. Out-of-order frames that fall within the current receive window are **accepted and buffered** rather than discarded.
2. **Selective / Independent Acknowledgments**: The receiver sends an individual ACK for every correctly received frame.
3. **Selective Retransmission**: The sender maintains an independent timer for each transmitted frame. When a timeout occurs or a NACK is received for a frame, **only that specific frame is retransmitted**.

---

Module 2: Sequence Number Space Constraints for Selective Repeat

To prevent overlap ambiguity between new frames and retransmitted old frames, Selective Repeat requires a larger sequence number space than Go-Back-N.

```
+-----------------------------------------------------------------------------------------------+
|                        SEQUENCE NUMBER FORMULAS FOR SELECTIVE REPEAT                          |
+---------------------------------------+-------------------------------------------------------+
| Property                              | Formula / Condition                                   |
+---------------------------------------+-------------------------------------------------------+
| Transmit & Receive Window Relation    | $W_t = W_r = N$ ($N > 1$)                             |
| Total Distinct Sequence Numbers Needed| $W_t + W_r = N + N = \mathbf{2N}$                     |
| Valid Sequence Number Range           | $0$ to $(2N - 1)$ (Total $2N$ distinct values)        |
| For $k$-bit Sequence Field            | $2^k \ge 2N \implies W_t \le 2^{k-1}$                 |
| Maximum Transmit Window Size ($W_t$)  | $W_t = 2^{k-1}$                                       |
| Minimum Bits for Sequence Field       | $k = \lceil \log_2(2N) \rceil$                        |
+---------------------------------------+-------------------------------------------------------+
```

```
  Example (3-bit Sequence Number Field, k = 3):
  Total Available Sequences = 2^3 = 8 (Sequence values: 0 to 7)
  Maximum Window Size W_t = W_r = 2^(3-1) = 2^2 = 4
  
  If W_t were set to 5 (violating the rule):
  Sender transmits F0, F1, F2, F3, F4. Receiver accepts all and slides window to 5,6,7,0,1.
  If ALL ACKs are lost, Sender retransmits F0, F1, F2, F3, F4.
  Receiver's window includes '0', so it would mistakenly accept old F0 as a brand-new frame!
```

---

Module 3: Piggybacking Concept & Header Optimization

**Piggybacking** is an optimization technique used in bidirectional (full-duplex) communication to reduce control overhead by combining data transmission with acknowledgments.

```
                             PIGGYBACKING FRAME STRUCTURE
                             
   +-----------------------+-----------------------+------------------------------------+
   | Data Sequence Number  | Acknowledgment Number | User Payload / Data                |
   | (k bits)              | (k bits)              | (Information Bytes)                |
   +-----------------------+-----------------------+------------------------------------+
```

Key Properties of Piggybacking

1. **Combined Control Field**: Instead of sending a separate control packet for an ACK, host A attaches its ACK for host B's received data directly onto outgoing data frames destined for host B.
2. **ACK Transmission Delay (**$t_{x\_ack}$**)**: Since the acknowledgment is embedded inside a full data frame, the ACK transmission delay equals the full data frame transmission delay: $$t_{x\_ack} = t_x$$
3. **Round-Trip Cycle Time with Piggybacking**: $$T_{\text{cycle}} = t_x + t_p + t_{x\_ack} + t_p = 2 t_x + 2 t_p$$

---

Module 4: Flow Control Protocols Head-to-Head Comparison

```
+----------------------------------------------------------------------------------------------------------+
|                             COMPREHENSIVE FLOW CONTROL COMPARISON MATRIX                                 |
+--------------------------+-----------------------+-----------------------+-------------------------------+
| Protocol Feature         | Stop-and-Wait ARQ     | Go-Back-N ARQ         | Selective Repeat ARQ          |
+--------------------------+-----------------------+-----------------------+-------------------------------+
| Transmit Window ($W_t$)  | $1$                   | $N$ ($N > 1$)         | $N$ ($N > 1$)                 |
| Receive Window ($W_r$)   | $1$                   | $1$                   | $N$ ($N > 1$)                 |
| Receiver Buffer Size     | 1 Frame               | 1 Frame               | $N$ Frames                    |
| Out-of-Order Handling    | Discarded             | Discarded             | Accepted & Buffered           |
| Acknowledgment Type      | Individual            | Cumulative            | Individual / Selective        |
| Retransmission Scope     | Single Frame          | All $N$ Frames        | Only the Lost Frame           |
| Total Sequences Needed   | $2$                   | $N + 1$               | $2N$                          |
| Max $W_t$ ($k$-bit field)| $1$                   | $2^k - 1$             | $2^{k-1}$                     |
| Efficiency ($\eta$)      | $\frac{1}{1 + 2a}$    | $\frac{N}{1 + 2a}$    | $\frac{N}{1 + 2a}$            |
| Channel Suitability      | Low Bandwidth-Delay   | High Bandwidth-Delay, | High Bandwidth-Delay,         |
|                          | Product links         | Low Error-Rate links  | High Error-Rate links         |
+--------------------------+-----------------------+-----------------------+-------------------------------+
```

---

Module 5: Step-by-Step Solved Problems & Exam Questions

Problem 1: Maximum Window Size for $n$-Bit Sequence Numbers

- **Question**: Find the maximum window size for data transmission using the Selective Repeat (Selective Reject) protocol with $n$-bit frame sequence numbers.
    - (A) $2^n$
    - (B) $2^{n-1}$
    - (C) $2^n - 1$
    - (D) $2^{n-2}$
- **Step-by-Step Solution**:
    1. An $n$-bit sequence field provides $2^n$ distinct sequence numbers.
    2. In Selective Repeat ARQ, $W_t = W_r = N$.
    3. Total distinct sequence numbers required: $W_t + W_r = N + N = 2N$.
    4. Set equal to total available sequence values: $2N = 2^n \implies N = \frac{2^n}{2} = \mathbf{2^{n-1}}$.
- **Answer**: **Option B (**$2^{n-1}$**)**.

---

Problem 2: Distinct Sequence Numbers for Arbitrary Window Sizes

- **Question**: In a sliding window ARQ scheme, the transmitter's window size is $N$ and the receiver's window size is $M$. What is the minimum number of distinct sequence numbers required to ensure correct operation?
    - (A) $\min(M, N)$
    - (B) $\max(M, N)$
    - (C) $M + N$
    - (D) $M \times N$
- **Step-by-Step Solution**:
    1. To avoid ambiguity caused by lost ACKs, the total distinct sequence numbers must cover the sum of transmitter and receiver window capacities.
    2. Minimum distinct sequence numbers $= W_t + W_r = \mathbf{N + M}$.
    3. Verification:
        - For Go-Back-N ($M = 1$): $N + 1$.
        - For Selective Repeat ($M = N$): $N + N = 2N$.
- **Answer**: **Option C (**$M + N$**)**.

---

Problem 3: Sequence Bits for Selective Repeat at 60% Target Utilization

- **Question**: Consider a Selective Repeat sliding window protocol that uses a frame size of $1\text{ KB}$ to send data on a $1.5\text{ Mbps}$ link with a one-way latency of $50\text{ ms}$. To achieve a link utilization of $60\%$, calculate the minimum number of bits required to represent the sequence number field.
- **Step-by-Step Solution**:
    1. Calculate frame transmission delay ($t_x$): $$\text{Frame Size} = 1\text{ KB} = 1000 \times 8 = 8000\text{ bits}$$ $$t_x = \frac{8000\text{ bits}}{1.5 \times 10^6\text{ bps}} = \frac{8}{1500}\text{ seconds} \approx 5.333\text{ ms}$$
    2. Calculate total round-trip cycle time ($T_{\text{cycle}}$): $$T_{\text{cycle}} = t_x + 2 t_p = 5.333\text{ ms} + 2(50\text{ ms}) = 105.333\text{ ms}$$
    3. Calculate window size $N$ for $\eta = 60\% = 0.6$: $$N = \left\lceil \eta \times \frac{T_{\text{cycle}}}{t_x} \right\rceil = \left\lceil 0.6 \times \frac{105.333\text{ ms}}{5.333\text{ ms}} \right\rceil = \lceil 0.6 \times 19.75 \rceil = \lceil 11.85 \rceil = \mathbf{12}$$
    4. In Selective Repeat, total distinct sequence numbers $= 2N = 2 \times 12 = 24$.
    5. Calculate minimum sequence bits field width ($k$): $$k = \lceil \log_2(24) \rceil \implies 2^4 = 16 < 24 \le 2^5 = 32 \implies k = \mathbf{5\text{ bits}}$$
- **Answer**: **5 bits**.

---

Problem 4: Sequence Bits for 100% Utilization in Selective Repeat

- **Question**: Consider a $128 \times 10^3\text{ bps}$ satellite link with a one-way propagation delay of $150\text{ ms}$. Selective Repeat protocol is used with a frame size of $1\text{ KB}$ ($1000\text{ bytes}$). Neglecting acknowledgment transmission time, find the minimum number of bits required for the sequence number field to achieve $100\%$ utilization.
- **Step-by-Step Solution**:
    1. Calculate frame transmission delay ($t_x$): $$\text{Frame Size} = 1000\text{ bytes} = 8000\text{ bits}$$ $$t_x = \frac{8000\text{ bits}}{128 \times 10^3\text{ bps}} = \frac{8}{128}\text{ seconds} = 0.0625\text{ seconds} = 62.5\text{ ms}$$
    2. Calculate total cycle time ($T_{\text{cycle}}$): $$T_{\text{cycle}} = t_x + 2 t_p = 62.5\text{ ms} + 2(150\text{ ms}) = 362.5\text{ ms}$$
    3. Calculate optimal window size ($N$) for $100\%$ utilization ($\eta = 1$): $$N = \left\lceil \frac{T_{\text{cycle}}}{t_x} \right\rceil = \left\lceil \frac{362.5\text{ ms}}{62.5\text{ ms}} \right\rceil = \lceil 5.8 \rceil = \mathbf{6}$$
    4. In Selective Repeat, total distinct sequence numbers $= 2N = 2 \times 6 = 12$.
    5. Minimum sequence bits field width ($k$): $$k = \lceil \log_2(12) \rceil \implies 2^3 = 8 < 12 \le 2^4 = 16 \implies k = \mathbf{4\text{ bits}}$$
- **Answer**: **4 bits**.

---

Problem 5: Sequence Bits and Wait Time with Piggybacking (Linked Question)

- **Part A Question**: Frames of $1000\text{ bits}$ are sent over a $10^6\text{ bps}$ duplex link between two hosts. One-way propagation time is $25\text{ ms}$. Frames are transmitted to maximally pack the link in transit. What is the minimum number of sequence bits ($l$) required?
    - (A) $l = 2$, (B) $l = 3$, (C) $l = 4$, (D) $l = 5$
- **Part A Solution**:
    1. $t_x = \frac{1000\text{ bits}}{10^6\text{ bps}} = 1\text{ ms}$.
    2. $t_p = 25\text{ ms}$.
    3. Transit link capacity in frames $= \frac{t_p}{t_x} = \frac{25\text{ ms}}{1\text{ ms}} = 25\text{ frames}$.
    4. To pack the link in transit, window size $N = 25$.
    5. Minimum sequence bits $l = \lceil \log_2(25) \rceil \implies 2^4 = 16 < 25 \le 2^5 = 32 \implies l = \mathbf{5\text{ bits}}$.
- **Part A Answer**: **Option D (**$l = 5$**)**.

---

- **Part B Question**: Suppose the sliding window protocol is used with a sender window size of $2^l = 32$ (where $l = 5$) and acknowledgments are always piggybacked. After sending $32$ frames, what is the minimum time the sender will have to wait before starting transmission of the next frame?
    - (A) $16\text{ ms}$
    - (B) $18\text{ ms}$
    - (C) $20\text{ ms}$
    - (D) $22\text{ ms}$
- **Part B Solution**:
    1. Time required to transmit $32$ frames continuously: $$t_{\text{send\_all}} = 32 \times t_x = 32 \times 1\text{ ms} = 32\text{ ms}$$
    2. Calculate total cycle time ($T_{\text{cycle}}$) with piggybacked ACKs ($t_{x\_ack} = t_x = 1\text{ ms}$): $$T_{\text{cycle}} = t_x + t_p + t_{x\_ack} + t_p = 1\text{ ms} + 25\text{ ms} + 1\text{ ms} + 25\text{ ms} = 52\text{ ms}$$
    3. Calculate waiting time after sending all 32 frames: $$\text{Wait Time} = T_{\text{cycle}} - t_{\text{send\_all}} = 52\text{ ms} - 32\text{ ms} = \mathbf{20\text{ ms}}$$
- **Part B Answer**: **Option C (**$20\text{ ms}$**)**.