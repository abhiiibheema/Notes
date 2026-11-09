Master Notes: Flow Control Fundamentals & Stop-and-Wait Protocols (Batch 1)

---

Module 1: Flow Control Principles & Delay Metrics

Flow Control is a data link layer and transport layer mechanism designed to match the transmission speed of a fast sender with the processing speed of a slow receiver1. Its primary objective is to **prevent receiver buffer overrun** and avoid frame loss.

```
                         SENDER-RECEIVER FLOW CONTROL
                         
   Sender (Fast Tx)                                          Receiver (Slow Rx)
  +------------------+                                      +------------------+
  | Send Buffer      | ---- Data Frame (tx + tp) ---------> | Receive Buffer   |
  | (Max 1 Frame)    |                                      | (Max 1 Frame)    |
  |                  | <--- ACK Frame (tx_ack + tp) ------- |                  |
  +------------------+                                      +------------------+
```

1. Key Delay Components

- **Transmission Delay (**$t_x$**)**: The time required to push all bits of a frame onto the physical medium: $$t_x = \frac{\text{Frame Size (bits)}}{\text{Bandwidth / Data Rate (bits/sec)}}$$
- **Propagation Delay (**$t_p$**)**: The time taken by a single bit to travel across the physical medium from sender to receiver: $$t_p = \frac{\text{Distance}}{\text{Propagation Velocity}}$$
- **Processing Delay (**$t_{\text{proc}}$**)**: The time spent by receiver or sender nodes to process frame headers and verify checksums.
- **Queuing Delay (**$t_{\text{queue}}$**)**: The time spent by a frame waiting in transmitter or receiver buffers prior to processing or transmission.

2. Round-Trip Cycle Time ($T_{\text{cycle}}$)

The total time elapsed from the start of transmitting a data frame until the complete arrival of its corresponding acknowledgment (ACK):

$$T_{\text{cycle}} = t_x + t_p + t_{\text{proc\_recv}} + t_{x\_ack} + t_p + t_{\text{proc\_send}} + t_{\text{queue}}$$

- **Standard Default Formula**: When queuing/processing delays are negligible and ACK frame transmission time ($t_{x\_ack}$) is very small compared to $t_x$ ($t_{x\_ack} \ll t_x$): $$T_{\text{cycle}} \approx t_x + 2t_p$$

3. Normalized Propagation Delay ($a$)

The ratio of one-way propagation delay to frame transmission delay: $$a = \frac{t_p}{t_x}$$

---

Module 2: Stop-and-Wait Protocol (Noiseless Channel)

In a **Stop-and-Wait Protocol**, the sender transmits exactly **one data frame** and pauses transmission until it receives an ACK from the receiver.

```
                     STOP-AND-WAIT TIMELINE (PER CYCLE)
                     
   Sender                                                      Receiver
     | --- Data Frame Transmission (tx) -----------------------> | \
     |     Propagation Delay (tp)                                |  |
     |                                                           |  |-- Total Cycle Time
     | <--- ACK Propagation Delay (tp) ------------------------- |  |   (tx + 2*tp)
     |      [ACK Received - Next Frame Can Be Transmitted]       | /
     | <=======================================================> |
     |                     Useful Time = tx                      |
```

1. Channel Efficiency / Link Utilization ($\eta$)

Efficiency represents the fraction of time the transmission channel is actively carrying useful data bits:

$$\eta = \frac{\text{Useful Transmission Time}}{\text{Total Cycle Time}} = \frac{t_x}{T_{\text{cycle}}}$$

- **Normalized Efficiency Formula**: Substituting $T_{\text{cycle}} = t_x + 2t_p$: $$\eta = \frac{t_x}{t_x + 2t_p} = \frac{1}{1 + 2a}$$

2. Efficiency Benchmarks & Conditions

- **100% Link Utilization (**$\eta = 1$**)**: Achieved **only when propagation delay is zero** ($t_p = 0 \implies T_{\text{cycle}} = t_x$).
- **50% Link Utilization (**$\eta = 0.5$**)**: Achieved when $T_{\text{cycle}} = 2 t_x \implies t_x = 2 t_p \implies a = 0.5$.
- **25% Link Utilization (**$\eta = 0.25$**)**: Achieved when $T_{\text{cycle}} = 4 t_x \implies 2 t_p = 3 t_x \implies a = 1.5$.

3. Throughput / Effective Data Rate

The actual rate at which useful data is transmitted across the link:

$$\text{Throughput} = \eta \times \text{Bandwidth} = \frac{\text{Frame Size}}{T_{\text{cycle}}}$$

4. Utilization Trends

Link utilization in Stop-and-Wait is **lowest** under high propagation delays (longer physical link length) combined with high bandwidths (higher transmission rates causing small $t_x$ and large $a$).

---

Module 3: Stop-and-Wait ARQ (Noisy Channel)

When transmitting over unreliable or noisy channels, frames or ACKs can be corrupted or lost. Stop-and-Wait Automatic Repeat Request (ARQ) adds error recovery mechanisms to guarantee reliable delivery.

```
+-----------------------------------------------------------------------------------------------+
|                                STOP-AND-WAIT ARQ ERROR HANDLING                               |
+-------------------+------------------------------------+--------------------------------------+
| Anomaly           | Problem                            | ARQ Solution Mechanism               |
+-------------------+------------------------------------+--------------------------------------+
| Lost Data Frame   | Receiver never gets frame; sender  | Time-out Timer expires at sender     |
|                   | waits indefinitely for ACK         | $\implies$ Retransmits frame         |
| Lost ACK Frame    | Receiver gets frame, but ACK is    | Time-out Timer expires $\implies$ Sender |
|                   | lost; sender retransmits           | resends duplicate frame              |
| Delayed ACK Frame | ACK arrives after timer timeout    | 1-Bit Sequence Numbering allows Rx   |
|                   | causing duplicate frames           | to identify/discard duplicate        |
+-------------------+------------------------------------+--------------------------------------+
```

```
                     STOP-AND-WAIT ARQ SEQUENCE NUMBERING
                     
   Transmitter (Seq: 0 or 1)                                Receiver (Expected: 0 or 1)
  +-------------------------+                              +---------------------------+
  | Frame F0 (Seq = 0)      | ---- Frame 0 --------------> | Accepts F0; Expected -> 1 |
  |                         | <--- ACK 0 ----------------- | Sends ACK 0               |
  | Frame F1 (Seq = 1)      | ---- Frame 1 --------------> | Accepts F1; Expected -> 0 |
  |                         | <--- ACK 1 ----------------- | Sends ACK 1               |
  +-------------------------+                              +---------------------------+
```

1. Core Operating Parameters

- **Transmit Window Size (**$W_t$**)**: **1** (Sender buffers 1 frame until acknowledged).
- **Receive Window Size (**$W_r$**)**: **1** (Receiver accepts only 1 expected frame at a time).
- **Sender/Receiver Buffer Size**: **1 frame** each.
- **Sequence Number Field Width**: **1 bit** (Sequence values alternate between `0` and `1`): $$\text{Sequence Number} = (\text{Frame Number}) \bmod 2$$
- **Total Distinct Sequence Numbers Required**: $W_t + W_r = 1 + 1 = \mathbf{2}$.

2. Protocol Operating Rules

- **New Frame Rule**: For a new frame transmission, the sender toggles/alternates the sequence number bit (`0` $\to$ `1` $\to$ `0`).
- **Retransmission Rule**: On retransmission due to time-out or NACK, the sender keeps the **same sequence number bit**.
- **Receiver Duplicate Handling**: If a frame arrives with a sequence number matching the previously accepted frame, the receiver **discards/ignores the duplicate frame** but **re-sends the ACK**.

---

Module 4: Step-by-Step Solved Problems & Exam Questions

Problem 1: Channel Utilization Calculation

- **Question**: Frames of size 3000 bits are transmitted between two nodes using the Stop-and-Wait protocol The channel transmission rate is 2000 bps and the one-way propagation delay is 100 ms. Assuming negligible processing time and ACK frame size, calculate the channel utilization percentage.
- **Step-by-Step Solution**:
    1. Calculate frame transmission delay ($t_x$): $$t_x = \frac{3000 \text{ bits}}{2000 \text{ bps}} = 1.5 \text{ seconds} = 1500 \text{ ms}$$
    2. Calculate total cycle time ($T_{\text{cycle}}$): $$T_{\text{cycle}} = t_x + 2 t_p = 1500 \text{ ms} + 2(100 \text{ ms}) = 1700 \text{ ms} = 1.7 \text{ seconds}$$
    3. Calculate efficiency / channel utilization ($\eta$): $$\eta = \frac{t_x}{T_{\text{cycle}}} = \frac{1.5}{1.7} \approx 0.88235 = \mathbf{88.23\%}$$
- **Answer**: **88.23%**.

---

Problem 2: Transmission Efficiency with Processing & ACK Delays

- **Question**: Stop-and-Wait ARQ operates with a channel bit rate of 1 Mbps, propagation delay of 0.75 ms, frame processing time of 0.25 ms, information frame size of 1980 bytes, overhead of 20 bytes, and ACK frame size of 20 bytes3839. Calculate the transmission efficiency percentage.
- **Step-by-Step Solution**:
    1. Calculate data frame transmission delay ($t_x$): $$\text{Total Frame Size} = 1980 + 20 = 2000 \text{ bytes} = 16,000 \text{ bits}$$ $$t_x = \frac{16,000 \text{ bits}}{10^6 \text{ bps}} = 0.016 \text{ seconds} = 16 \text{ ms}$$
    2. Calculate ACK frame transmission delay ($t_{x\_ack}$): $$\text{ACK Size} = 20 \text{ bytes} = 160 \text{ bits}$$ $$t_{x\_ack} = \frac{160 \text{ bits}}{10^6 \text{ bps}} = 0.00016 \text{ seconds} = 0.16 \text{ ms}$$
    3. Calculate total round-trip cycle time ($T_{\text{cycle}}$): $$T_{\text{cycle}} = t_x + t_p + t_{\text{proc}} + t_{x\_ack} + t_p = 16 + 0.75 + 0.25 + 0.16 + 0.59 = 17.75 \text{ ms}$$
    4. Calculate transmission efficiency ($\eta$): $$\eta = \frac{t_x}{T_{\text{cycle}}} \times 100\% = \frac{15.84 \text{ ms}}{17.75 \text{ ms}} \times 100\% = \mathbf{89.23\%}$$
- **Answer**: **89.23%**.

---

Problem 3: Lowest Utilization Scenario Identification

- **Question**: Two hosts are connected by a point-to-point link using the Stop-and-Wait protocol. In which scenario is link utilization the lowest?
- **Step-by-Step Solution**:
    1. Efficiency formula: $\eta = \frac{1}{1 + 2a}$, where $a = \frac{t_p}{t_x} = \frac{t_p \cdot B}{L}$.
    2. Utilization ($\eta$) is minimized when $a$ is maximized.
    3. $a$ increases when propagation delay $t_p$ increases (longer link length) and bandwidth $B$ / transmission rate increases (higher transmission rate causing smaller $t_x$).
- **Answer**: **Longer link length and higher transmission rate**.

---

Problem 4: Minimum Frame Size for Target Efficiency ($\ge 50\%$)

- **Question**: A channel has a bit rate of 4 kbps and a one-way propagation delay of 20 ms. The transmission time of the acknowledgment frame is negligible1741. To achieve a channel efficiency of at least 50%, find the minimum required frame size.
- **Step-by-Step Solution**:
    1. For $\eta \ge 50\% = 0.5$, we require $T_{\text{cycle}} \le 2 t_x$.
    2. Substitute $T_{\text{cycle}} = t_x + 2 t_p$: $$t_x + 2 t_p \le 2 t_x \implies t_x \ge 2 t_p = 2 \times 20 \text{ ms} = 40 \text{ ms}$$
    3. Calculate minimum frame size ($L_{\text{min}}$): $$L_{\text{min}} = t_x \times B = 40 \text{ ms} \times 4 \text{ kbps} = (40 \times 10^{-3} \text{ s}) \times (4000 \text{ bits/s}) = \mathbf{160 \text{ bits}} \text{ (or } 20 \text{ bytes)}$$
- **Answer**: **160 bits (or 20 bytes)**41.

---

Problem 5: Propagation Delay Calculation from Efficiency

- **Question**: A link has a transmission speed of $10^6$ bits/sec and transmits data packets of size 1000 bytes each. Acknowledgment transmission delay and processing delays are negligible. If the efficiency of the Stop-and-Wait protocol is exactly 25%, find the one-way propagation delay in milliseconds.
- **Step-by-Step Solution**:
    1. Calculate frame transmission delay ($t_x$)$$t_x = \frac{1000 \text{ bytes} \times 8 \text{ bits/byte}}{10^6 \text{ bits/sec}} = \frac{8000}{10^6} = 8 \text{ ms}$$
    2. Apply efficiency equation for $\eta = 25\% = \frac{1}{4}$: $$\eta = \frac{t_x}{t_x + 2 t_p} = \frac{1}{4} \implies t_x + 2 t_p = 4 t_x \implies 2 t_p = 3 t_x$$
    3. Solve for $t_p$: $$t_p = \frac{3 \times 8 \text{ ms}}{2} = \mathbf{12 \text{ ms}}$$
- **Answer**: **12 ms**2043.

---

Problem 6: Sender Throughput under Asymmetric Data Rates

- **Question**: A sender uses Stop-and-Wait ARQ on a link. Data frames are 1000 bytes with a sender transmission rate of 80 Kbps. Acknowledgments are 100 bytes with a receiver transmission rate of 8 Kbps. The one-way propagation delay is 100 ms. Assuming no frame loss, calculate the sender throughput in bytes.
- **Step-by-Step Solution**:
    1. Calculate data frame transmission delay ($t_x$): $$t_x = \frac{1000 \text{ bytes} \times 8 \text{ bits/byte}}{80 \times 10^3 \text{ bps}} = \frac{8000}{80,000} = 0.1 \text{ seconds} = 100 \text{ ms}$$
    2. Calculate ACK frame transmission delay ($t_{x\_ack}$): $$t_{x\_ack} = \frac{100 \text{ bytes} \times 8 \text{ bits/byte}}{8 \times 10^3 \text{ bps}} = \frac{800}{8000} = 0.1 \text{ seconds} = 100 \text{ ms}$$
    3. Calculate total round-trip cycle time ($T_{\text{cycle}}$): $$T_{\text{cycle}} = t_x + t_p + t_{x\_ack} + t_p = 100 + 100 + 100 + 100 = 400 \text{ ms} = 0.4 \text{ seconds}$$
    4. Calculate sender throughput: $$\text{Throughput} = \frac{\text{Frame Size}}{T_{\text{cycle}}} = \frac{1000 \text{ bytes}}{0.4 \text{ seconds}} = \mathbf{2500 \text{ bytes/sec}}$$
- **Answer**: **2500 bytes/second**.