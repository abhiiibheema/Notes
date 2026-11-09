Master Notes: Pipelining Principles & Go-Back-N (GBN) ARQ (Batch 2)

---

Module 1: Sliding Window Concept & Pipelining Principles

In Stop-and-Wait protocols, the sender remains idle for a significant portion of the round-trip cycle time while waiting for an acknowledgment. **Pipelining / Sliding Window Protocols** solve this inefficiency by allowing the sender to transmit **multiple data frames continuously** before pausing for acknowledgments.

```
                           STOP-AND-WAIT vs. SLIDING WINDOW
                           
   STOP-AND-WAIT (1 Frame per Cycle):
   Sender: [ Tx Frame 1 ] -------- (Wait 2*tp) --------> [ ACK Received ] ---> [ Tx Frame 2 ]
   Link Utilization: LOW
   
   SLIDING WINDOW (N Frames per Cycle):
   Sender: [ Tx F1 ][ Tx F2 ][ Tx F3 ]...[ Tx FN ] ----> [ ACK 1 ] ---> [ Slide Window & Tx FN+1 ]
   Link Utilization: HIGH (Up to 100%)
```

1. Transmit and Receive Windows

- **Transmit Window Size (**$W_t$ **or** $N$**)**: The maximum number of unacknowledged data frames the sender can transmit without waiting for an ACK.
- **Receive Window Size (**$W_r$**)**: The maximum number of frames the receiver can accept and buffer at one time.
- **Window Sliding Action**:
    - As the sender transmits frames, the trailing edge of the transmit window shrinks available capacity.
    - When a valid ACK arrives, the window slides forward, allowing new frames to be transmitted.

---

Module 2: Efficiency & Optimal Window Sizing Formulas

The performance of a sliding window protocol depends on how many frames $N$ can fit into a single round-trip cycle time.

```
                      ROUND-TRIP CYCLE TIME & WINDOW FILLING
                      
   Sender                                                      Receiver
     | === Tx Frame 1 (tx) ==================================> | \
     | === Tx Frame 2 (tx) ==================================> |  |
     | === Tx Frame 3 (tx) ==================================> |  |-- Total Cycle Time
     | ...                                                     |  |   (tx + 2*tp)
     | === Tx Frame N (tx) ==================================> |  |
     | <--- ACK 1 Propagation (tp) --------------------------- | /
     |     [ACK 1 Received - Window Slides]
```

1. Channel Efficiency / Link Utilization Formula

Efficiency ($\eta$) is the ratio of useful data transmission time to total cycle time:

$$\eta = \frac{\text{Frames Transmitted} \times \text{Frame Transmission Delay}}{\text{Total Round-Trip Cycle Time}}$$

$$\eta = \frac{N \cdot t_x}{T_{\text{cycle}}} = \frac{N \cdot t_x}{t_x + 2t_p} = \frac{N}{1 + 2a}$$

Where:

- $N$: Transmit Window Size ($W_t$).
- $t_x$: Single Frame Transmission Delay ($\text{Frame Size} / \text{Bandwidth}$).
- $t_p$: One-Way Propagation Delay ($\text{Distance} / \text{Propagation Velocity}$).
- $a$: Normalized Propagation Delay ($t_p / t_x$).
- $T_{\text{cycle}}$: Round-Trip Cycle Time ($t_x + 2t_p$).

2. Optimal Window Size ($N_{\text{opt}}$) for 100% Link Utilization

To achieve **100% channel utilization (**$\eta = 1$**)**, the sender must continuously transmit frames during the entire cycle time without experiencing idle periods:

$$N_{\text{opt}} = \left\lceil \frac{T_{\text{cycle}}}{t_x} \right\rceil = \left\lceil \frac{t_x + 2t_p}{t_x} \right\rceil = \lceil 1 + 2a \rceil$$

3. Minimum Window Size for Target Efficiency ($\eta_{\text{target}}$)

To achieve a specific target efficiency $\eta_{\text{target}}$ (e.g., 50% or 60%):

$$N = \left\lceil \eta_{\text{target}} \cdot \frac{T_{\text{cycle}}}{t_x} \right\rceil = \left\lceil \eta_{\text{target}} \cdot (1 + 2a) \right\rceil$$

4. Effective Throughput

$$\text{Throughput} = \eta \times \text{Bandwidth} = \frac{N \times \text{Frame Size}}{T_{\text{cycle}}}$$

---

Module 3: Go-Back-N (GBN) ARQ Mechanics & Error Recovery

Go-Back-N ARQ is a pipelined error control protocol that allows the sender to transmit up to $N$ frames before receiving an acknowledgment, while keeping receiver complexity minimal.

```
+-----------------------------------------------------------------------------------------------+
|                                  GO-BACK-N ARQ PARAMETERS                                     |
+-------------------+------------------------------------+--------------------------------------+
| Feature           | Parameter Value                    | Behavioral Constraint                |
+-------------------+------------------------------------+--------------------------------------+
| Transmit Window   | $W_t = N$ ($N > 1$)                | Sender buffers up to $N$ frames       |
| Receive Window    | $W_r = 1$                          | Accepts ONLY strictly in-order frames|
| Receiver Buffering| 0 Frames (No buffer for out-of-order)| Discards all out-of-order arrivals   |
| Acknowledgments   | Cumulative ACKs                    | ACK $k$ confirms all frames up to $k$|
| Retransmission    | Go-Back-$N$ Retransmission         | Resends ALL $N$ unacknowledged frames|
+-------------------+------------------------------------+--------------------------------------+
```

```
                     GO-BACK-N FRAME LOSS & RETRANSMISSION
                     
   Sender (Window = 4: F0, F1, F2, F3)                       Receiver (Expected: 0)
     | --- Frame 0 ------------------------------------------> | Accepts F0; Expected -> 1
     | --- Frame 1 (LOST IN TRANSIT!) X                       | 
     | --- Frame 2 ------------------------------------------> | Discards F2 (Out of order!); Resends ACK 0
     | --- Frame 3 ------------------------------------------> | Discards F3 (Out of order!); Resends ACK 0
     |                                                         |
     | [Timer Expires for Frame 1]                             |
     | === Retransmits ALL N frames starting at F1 ===========> |
     | --- Frame 1, Frame 2, Frame 3, Frame 4                | Accepts in order starting at F1
```

1. In-Order Arrival & Cumulative ACKs

- **Strict In-Order Requirement**: The receiver expects frames in exact sequence. If frame $k$ is expected, any arriving frame with a sequence number other than $k$ is **discarded immediately**.
- **Cumulative ACK**: An ACK with sequence number $k$ acknowledges that all frames up to $k$ have been received correctly. If an intermediate ACK is lost but a subsequent ACK arrives, all preceding frames are implicitly acknowledged.

2. Retransmission Strategy

When a frame loss occurs or a timeout expires for the oldest unacknowledged frame, the sender **goes back** $N$ **steps** and retransmits **all unacknowledged frames currently residing in the transmit window**.

---

Module 4: Sequence Numbering Constraints in Go-Back-N ARQ

To prevent duplicate frame ambiguity when all ACKs in a window are lost, the total number of distinct sequence numbers must exceed the sum of sender and receiver window sizes.

```
+-----------------------------------------------------------------------------------------------+
|                             SEQUENCE NUMBER FORMULAS FOR GBN ARQ                              |
+---------------------------------------+-------------------------------------------------------+
| Property                              | Formula                                               |
+---------------------------------------+-------------------------------------------------------+
| Total Distinct Sequence Numbers Needed| $W_t + W_r = N + 1$                                   |
| Valid Sequence Number Range           | $0$ to $N$ (Total $N + 1$ distinct values)            |
| For $k$-bit Sequence Field            | $2^k \ge N + 1 \implies W_t \le 2^k - 1$              |
| Minimum Bits for Sequence Field       | $k = \lceil \log_2(N + 1) \rceil$                     |
+---------------------------------------+-------------------------------------------------------+
```

```
  Example (2-bit Sequence Number Field, k = 2):
  Total Available Sequences = 2^2 = 4 (Sequence values: 0, 1, 2, 3)
  Maximum Transmit Window Size W_t = 2^k - 1 = 4 - 1 = 3
  
  If W_t were set to 4 (violating the rule):
  Sender transmits F0, F1, F2, F3. Receiver accepts all and expects F0.
  If ALL ACKs are lost, Sender timeouts and retransmits F0, F1, F2, F3.
  Receiver CANNOT distinguish new F0 from duplicate F0!
```

---

Module 5: Step-by-Step Solved Problems & Exam Questions

Problem 1: Optimal Window Size Calculation

- **Question**: Station A uses 32-byte packets to transmit messages to Station B using a sliding window protocol. The round-trip delay between A and B is 80 milliseconds and the bottleneck bandwidth on the path is 128 kbps. What is the optimal window size that A should use?
    - (A) 20, (B) 40, (C) 160, (D) 320
- **Step-by-Step Solution**:
    1. Calculate frame transmission delay ($t_x$): $$t_x = \frac{32 \text{ bytes} \times 8 \text{ bits/byte}}{128 \times 10^3 \text{ bits/sec}} = \frac{256 \text{ bits}}{128,000 \text{ bps}} = 0.002 \text{ seconds} = 2 \text{ ms}$$
    2. Round-trip cycle time ($T_{\text{cycle}}$) is given directly as $80 \text{ ms}$.
    3. Calculate optimal window size ($N_{\text{opt}}$): $$N_{\text{opt}} = \left\lceil \frac{T_{\text{cycle}}}{t_x} \right\rceil = \left\lceil \frac{80 \text{ ms}}{2 \text{ ms}} \right\rceil = \mathbf{40}$$
- **Answer**: **Option B (40)**.

---

Problem 2: Maximum Achievable Throughput

- **Question**: Host A is sending data to Host B over a full-duplex link using the sliding window protocol. Send and receive window sizes are 5 packets each. Data packets are 1000 bytes long with a transmission time of 50 microseconds. ACK packets are very small with negligible transmission time. The one-way propagation delay is 200 microseconds. Calculate the maximum achievable throughput.
    - (A) $7.69 \times 10^6 \text{ B/s}$, (B) $11.11 \times 10^6 \text{ B/s}$, (C) $12.33 \times 10^6 \text{ B/s}$, (D) $15.00 \times 10^6 \text{ B/s}$
- **Step-by-Step Solution**:
    1. Given: $W_t = 5$, $t_x = 50\ \mu\text{s}$, $t_p = 200\ \mu\text{s}$.
    2. Calculate total cycle time ($T_{\text{cycle}}$): $$T_{\text{cycle}} = t_x + 2 t_p = 50\ \mu\text{s} + 2(200\ \mu\text{s}) = 450\ \mu\text{s}$$
    3. Calculate maximum achievable throughput: $$\text{Throughput} = \frac{W_t \times \text{Packet Size}}{T_{\text{cycle}}} = \frac{5 \times 1000 \text{ bytes}}{450 \times 10^{-6} \text{ seconds}} = \frac{5000}{0.00045} \approx \mathbf{11.11 \times 10^6 \text{ bytes/sec}}$$
- **Answer**: **Option B (**$11.11 \times 10^6 \text{ bytes/sec}$**)**.

---

Problem 3: Minimum Window Size for 100% Link Utilization

- **Question**: Consider a sliding window protocol over a lossless link with a window size of $W$ frames. Each frame is 1000 bits. Link bandwidth is 100 kbps and one-way propagation delay is 100 ms. Processing and ACK transmission times are zero. Find the minimum window size $W$ required to achieve 100% link utilization.
    - (A) 10, (B) 11, (C) 20, (D) 21
- **Step-by-Step Solution**:
    1. Calculate frame transmission delay ($t_x$): $$t_x = \frac{1000 \text{ bits}}{100 \times 10^3 \text{ bps}} = 0.01 \text{ seconds} = 10 \text{ ms}$$
    2. Calculate total cycle time ($T_{\text{cycle}}$): $$T_{\text{cycle}} = t_x + 2 t_p = 10 \text{ ms} + 2(100 \text{ ms}) = 210 \text{ ms}$$
    3. Calculate minimum window size $W$ for 100% utilization: $$W = \left\lceil \frac{T_{\text{cycle}}}{t_x} \right\rceil = \left\lceil \frac{210 \text{ ms}}{10 \text{ ms}} \right\rceil = \mathbf{21}$$
- **Answer**: **Option D (21)**.

---

Problem 4: Minimum Window Size for 50% Link Utilization

- **Question**: A sliding window flow-control protocol operates over a full-duplex error-free link. Frame size is 2000 bits, ACK size is 10 bits. Link data rate is 1 Mbps in each direction. One-way propagation delay is 100 ms. Calculate the minimum sender window size required to achieve a link utilization of 50%.
- **Step-by-Step Solution**:
    1. Calculate data frame transmission delay ($t_x$): $$t_x = \frac{2000 \text{ bits}}{10^6 \text{ bps}} = 0.002 \text{ seconds} = 2 \text{ ms}$$
    2. ACK transmission delay ($t_{\text{ack}} = 10 \text{ bits} / 10^6 \text{ bps} = 0.01 \text{ ms}$) is negligible.
    3. Calculate total cycle time ($T_{\text{cycle}}$): $$T_{\text{cycle}} = t_x + 2 t_p = 2 \text{ ms} + 2(100 \text{ ms}) = 202 \text{ ms}$$
    4. Apply target efficiency formula for $\eta = 50\% = 0.5$: $$N = \left\lceil \eta \times \frac{T_{\text{cycle}}}{t_x} \right\rceil = \left\lceil 0.5 \times \frac{202 \text{ ms}}{2 \text{ ms}} \right\rceil = \lceil 0.5 \times 101 \rceil = \lceil 50.5 \rceil = \mathbf{51}$$
- **Answer**: **51**.

---

Problem 5: Sequence Number Bits Formula Derivation

- **Question**: Stations M and N are separated by distance $L$ km. All frames are $K$ bits long. Propagation delay per kilometer is $t$ seconds. Channel capacity is $R$ bits/second. Processing delay is negligible. Find the minimum number of bits for the sequence number field for maximum utilization in a sliding window protocol.
    - (A) $\lceil \log_2 \frac{2LtR + 2K}{K} \rceil$
    - (B) $\lceil \log_2 \frac{2LtR}{K} \rceil$
    - (C) $\lceil \log_2 \frac{2LtR + K}{K} \rceil$
    - (D) $\lceil \log_2 \frac{2LtR + K}{2K} \rceil$
- **Step-by-Step Solution**:
    1. One-way propagation delay $t_p = L \cdot t$ seconds.
    2. Frame transmission delay $t_x = \frac{K}{R}$ seconds.
    3. Total cycle time $T_{\text{cycle}} = t_x + 2 t_p = \frac{K}{R} + 2Lt = \frac{2LtR + K}{R}$.
    4. Optimal window size $N = \frac{T_{\text{cycle}}}{t_x} = \frac{(2LtR + K)/R}{K/R} = \frac{2LtR + K}{K}$.
    5. Minimum sequence bits field width $k = \left\lceil \log_2(N) \right\rceil = \left\lceil \log_2 \left( \frac{2LtR + K}{K} \right) \right\rceil$.
- **Answer**: **Option C (**$\lceil \log_2 \frac{2LtR + K}{K} \rceil$**)**.

---

Problem 6: Sequence Bits for Full Channel Capacity in GBN ARQ

- **Question**: Two systems 8000 km apart are connected by a link with bandwidth $500 \times 10^6$ bps and propagation speed $4 \times 10^6$ m/s. Average packet size is $10^7$ bits. Design a Go-Back-N sliding window protocol to use the link at full capacity. Calculate the minimum size in bits of the sequence number field.
- **Step-by-Step Solution**:
    1. Calculate one-way propagation delay ($t_p$): $$\text{Distance} = 8000 \text{ km} = 8 \times 10^6 \text{ meters}$$ $$t_p = \frac{8 \times 10^6 \text{ m}}{4 \times 10^6 \text{ m/s}} = 2 \text{ seconds}$$
    2. Calculate frame transmission delay ($t_x$): $$t_x = \frac{10^7 \text{ bits}}{500 \times 10^6 \text{ bps}} = \frac{1}{50} \text{ seconds} = 0.02 \text{ seconds}$$
    3. Calculate total cycle time ($T_{\text{cycle}}$): $$T_{\text{cycle}} = t_x + 2 t_p = 0.02 + 2(2) = 4.02 \text{ seconds}$$
    4. Calculate optimal window size ($N$): $$N = \left\lceil \frac{T_{\text{cycle}}}{t_x} \right\rceil = \left\lceil \frac{4.02}{0.02} \right\rceil = 201$$
    5. In Go-Back-N ARQ, total distinct sequence numbers required = $N + 1 = 201 + 1 = 202$.
    6. Minimum sequence bits field width $k$: $$k = \lceil \log_2(202) \rceil \implies 2^7 = 128 < 202 \le 2^8 = 256 \implies k = \mathbf{8 \text{ bits}}$$
- **Answer**: **8 bits**.

---

Problem 7: Total Transmitted Packets Under Loss Pattern

- **Question**: Hosts A and B use Go-Back-3 ARQ ($N = 3$). Host A sends a file divided into 7 packets. Every 4th packet that A transmits gets lost (no ACKs ever get lost). What is the total number of packets that A will transmit (including retransmissions) to send the file to B?
- **Step-by-Step Solution**:
    1. **Attempt 1**: Transmits Packet 1 (1st tx - OK).
    2. **Attempt 2**: Transmits Packet 2 (2nd tx - OK).
    3. **Attempt 3**: Transmits Packet 3 (3rd tx - OK).
    4. **Attempt 4**: Transmits Packet 4 (**4th tx - LOST!**).
    5. Sender has sent 3 packets in window (4, 5, 6). While 4 is lost, 5 and 6 arrive out-of-order and are discarded by receiver.
    6. **Attempt 5**: Transmits Packet 5 (5th tx - Discarded by Rx).
    7. **Attempt 6**: Transmits Packet 6 (6th tx - Discarded by Rx).
    8. Timeout occurs for Packet 4. Sender goes back 3 and retransmits starting at Packet 4:
    9. **Attempt 7**: Retransmits Packet 4 (7th tx - OK).
    10. **Attempt 8**: Retransmits Packet 5 (**8th tx - LOST!**).
    11. **Attempt 9**: Retransmits Packet 6 (9th tx - Discarded by Rx).
    12. **Attempt 10**: Transmits Packet 7 (10th tx - Discarded by Rx).
    13. Timeout occurs for Packet 5. Sender goes back 3 and retransmits starting at Packet 5:
    14. **Attempt 11**: Retransmits Packet 5 (11th tx - OK).
    15. **Attempt 12**: Retransmits Packet 6 (**12th tx - LOST!**).
    16. **Attempt 13**: Retransmits Packet 7 (13th tx - Discarded by Rx).
    17. Timeout occurs for Packet 6. Sender goes back 3 and retransmits starting at Packet 6:
    18. **Attempt 14**: Retransmits Packet 6 (14th tx - OK).
    19. **Attempt 15**: Retransmits Packet 7 (15th tx - OK).
    20. Total transmissions executed by sender = **15 packets**.
- **Answer**: **15**.