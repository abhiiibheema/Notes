Master Notes: MAC Sublayer Foundations, 48-Bit MAC Addressing & ALOHA Protocols (Batch 1)

---

Module 1: Data Link Layer Architecture & MAC Address Mechanics

1. Data Link Sublayer Division

The Data Link Layer (Layer 2 of the OSI model) is divided into two distinct sublayers:

1. **Logical Link Control (LLC) Sublayer**: Handles flow control, error control, and framing interfaces for higher network layer protocols.
2. **Media Access Control (MAC) Sublayer**: Manages access to shared physical communication channels when multiple nodes compete for the same medium.

```
+-----------------------------------------------------------------------------------+
|                              DATA LINK LAYER (LAYER 2)                             |
+-----------------------------------------------------------------------------------+
|  1. Logical Link Control (LLC) Sublayer  ---> Upper interface / Flow & Error Control |
|  2. Media Access Control (MAC) Sublayer  ---> Shared Medium Access & Hardware Addr|
+-----------------------------------------------------------------------------------+
```

2. MAC Address Fundamentals

- **Physical Hardware Address**: A MAC address is a permanent, unique physical address burned into the Network Interface Card (NIC) of a device by its manufacturer.
- **Address Size**: A MAC address is **48 bits (6 bytes)** long.
- **Scope**: Used for node-to-node frame delivery within a **Local Area Network (LAN)**.

```
                         48-BIT MAC ADDRESS STRUCTURE
                         
   Binary Form (48 bits):
   10101100 . 11010001 . 10111000 . 11010101 . 01011001 . 00001101
   \______/   \______/   \______/   \______/   \______/   \______/
    Byte 1     Byte 2     Byte 3     Byte 4     Byte 5     Byte 6
      |          |          |          |          |          |
   Hexadecimal Representation (6 Octets / 12 Hex Characters):
   "ac   :    d1   :    b8   :    d5   :    59   :    0d"
```

3. Special Broadcast MAC Address

- **Hexadecimal Form**: **ff:ff:ff:ff:ff:ff**
- **Binary Form**: All 48 bits set to `1` (`11111111...11111111`).
- **Function**: Instructs switches and network interfaces on a LAN to accept and deliver the frame to **all hosts** on the local network segment.

---

Module 2: Taxonomy of Multiple Access Protocols

When multiple nodes share a single communication channel, an access control protocol is required to coordinate transmissions and resolve interference.

```
                          MULTIPLE ACCESS PROTOCOL TAXONOMY
                                          |
         +--------------------------------+--------------------------------+
         |                                |                                |
   CHANNEL PARTITIONING             RANDOM ACCESS                  CONTROLLED ACCESS
   (Static Allocation)          (Contention-Based)                 (Taking Turns)
   - Fixed division             - No central control               - Dynamic allocation
   - No collisions              - Collisions possible              - No collisions
   - Efficient under            - Efficient under                  - Token / Polling /
     heavy load                   light load                         Reservation
   - TDMA, FDMA, CDMA           - ALOHA, CSMA, CSMA/CD             - Token Ring, Token Bus
```

```
+---------------------------------------------------------------------------------------------------+
|                                CATEGORIES OF MAC PROTOCOLS                                        |
+----------------------+---------------------------+------------------------+-----------------------+
| Category             | Allocation Mechanism      | Collision Risk         | Optimal Load          |
+----------------------+---------------------------+------------------------+-----------------------+
| Channel Partitioning | Static (Time/Freq/Code)   | No Collisions          | Heavy Network Traffic |
| Random Access        | No Allocation (Immediate) | Collisions Can Occur   | Light Network Traffic |
| Controlled Access    | Dynamic (Token/Polling)   | No Collisions          | Heavy Network Traffic |
+----------------------+---------------------------+------------------------+-----------------------+
```

---

Module 3: Pure ALOHA Protocol Mechanics & Derivations

Pure ALOHA is the simplest random access protocol, designed for uncoordinated nodes transmitting over a shared channel (such as satellite communications).

```
                            PURE ALOHA TRANSMISSION TIMELINE
                            
   Node A  | ---- Frame A ----> |
   Node B  |            | ---- Frame B ----> |  <=== COLLISION REGION!
   Node C  |                                 | ---- Frame C ----> |
   
   - Nodes transmit frames immediately whenever generated.
   - If two frames overlap even partially, both frames are completely corrupted.
```

1. Operational Rules

- **Immediate Transmission**: Whenever a node has a frame ready, it transmits immediately without checking whether the channel is busy.
- **Collision Behavior**: If two or more frames overlap by even a fraction of a bit, a **collision** occurs, and all overlapping frames are corrupted.
- **Acknowledgment & Back-Off**:
    - After transmitting, the sender waits for an acknowledgment (ACK) within a time-out duration equal to the **Round-Trip Propagation Delay (**$2 \cdot t_p$**)**.
    - If no ACK is received, the sender assumes a collision occurred, waits a **random back-off time**, and retransmits the frame.

2. Vulnerable Time Derivation

The **Vulnerable Time** is the time window during which no other node may start a frame transmission to avoid colliding with a target frame.

```
                      PURE ALOHA VULNERABLE TIME = 2 * tx
                      
           t - tx                    t                    t + tx
             |                       |                       |
             +-----------------------+-----------------------+
             |   Frame generated     | Target Frame          |
             |   here collides with  | Transmission          |
             |   the target frame    | (Duration = tx)       |
             +-----------------------+-----------------------+
             \_______________________________________________/
                         Vulnerable Period = 2 * tx
```

- Let $t_x$ be the frame transmission delay.
- If a target frame starts transmission at time $t$:
    - Any frame starting between $t - t_x$ and $t$ will overlap with the start of the target frame.
    - Any frame starting between $t$ and $t + t_x$ will overlap with the end of the target frame.
- **Pure ALOHA Vulnerable Time**: $$\text{Vulnerable Time}_{\text{Pure ALOHA}} = 2 \cdot t_x$$

3. Mathematical Throughput Model

- Let $G$ be the mean number of frame transmission attempts (including new frames and retransmissions) per frame time $t_x$.
- Under a **Poisson Distribution** model, the probability that $k$ frames are generated during a vulnerable period of length $2 t_x$ is given by: $$P(k) = \frac{(2G)^k \cdot e^{-2G}}{k!}$$
- The probability of a successful transmission (zero overlapping frames, $k = 0$) is: $$P(\text{Success}) = P(0) = e^{-2G}$$
- **Pure ALOHA Throughput Formula (**$S$**)**: $$S = G \cdot P(\text{Success}) = G \cdot e^{-2G}$$

4. Maximum Throughput Derivation

To find the value of $G$ that maximizes throughput $S$, set the first derivative of $S$ with respect to $G$ to zero:

$$\frac{dS}{dG} = \frac{d}{dG} \left( G \cdot e^{-2G} \right) = e^{-2G} + G \cdot (-2 e^{-2G}) = e^{-2G}(1 - 2G) = 0$$

$$1 - 2G = 0 \implies G = 0.5$$

Substituting $G = 0.5$ back into the throughput formula:

$$S_{\text{max}} = 0.5 \cdot e^{-2(0.5)} = \frac{1}{2e} \approx 0.1839 = \mathbf{18.39\%}$$

---

Module 4: Slotted ALOHA Protocol Mechanics & Derivations

Slotted ALOHA introduces time synchronization to cut the vulnerable time in half and double channel efficiency.

```
                           SLOTTED ALOHA TIME SLOTS
                           
   Slot Boundaries: |   Slot 1   |   Slot 2   |   Slot 3   |   Slot 4   |
   Transmissions:   | [Frame A]  | [Frame B]  | (Idle)     | [Frame D]  |
                    |            | [Frame C]  |            |            |
                    |            | COLLISION! |            | SUCCESS!   |
```

1. Operational Rules

- **Time Synchronization**: Time is divided into discrete slots, where each slot duration equals one frame transmission time $t_x$.
- **Slot-Boundary Transmissions**: A node can start transmitting a frame **only at the beginning of a time slot**.
- If a frame arrives mid-slot, the node must defer transmission until the start of the next slot.

2. Vulnerable Time Derivation

Because transmissions are restricted to slot boundaries, two frames can collide only if they are scheduled for the **exact same time slot**.

```
                   SLOTTED ALOHA VULNERABLE TIME = tx
                   
                        Slot t                  Slot t + tx
                          |                          |
                          +--------------------------+
                          | Target Frame             |
                          | (Duration = tx)          |
                          +--------------------------+
                          \__________________________/
                             Vulnerable Period = tx
```

- **Slotted ALOHA Vulnerable Time**: $$\text{Vulnerable Time}_{\text{Slotted ALOHA}} = t_x$$

3. Mathematical Throughput Model

- Using the Poisson distribution for a vulnerable period of length $t_x$: $$P(\text{Success}) = e^{-G}$$
- **Slotted ALOHA Throughput Formula (**$S$**)**: $$S = G \cdot e^{-G}$$

4. Maximum Throughput Derivation

Setting the derivative with respect to $G$ to zero:

$$\frac{dS}{dG} = \frac{d}{dG} \left( G \cdot e^{-G} \right) = e^{-G} - G \cdot e^{-G} = e^{-G}(1 - G) = 0 \implies G = 1$$

Substituting $G = 1$ back into the formula:

$$S_{\text{max}} = 1 \cdot e^{-1} = \frac{1}{e} \approx 0.3678 = \mathbf{36.78\%}$$

---

Module 5: Time-Slotted LAN Probability Framework

In a time-slotted LAN with a finite number of active nodes, success probabilities are analyzed using the **Binomial Distribution**.

```
+---------------------------------------------------------------------------------------------------+
|                             TIME-SLOTTED LAN PROBABILITY FORMULAS                                 |
+------------------------------------+--------------------------------------------------------------+
| Metric                             | Mathematical Formula                                         |
+------------------------------------+--------------------------------------------------------------+
| Success of a Particular Host       | $P_{\text{host}} = p \cdot (1 - p)^{N - 1}$                  |
| Channel Success (Any Host Succeeds)| $P_{\text{channel}} = N \cdot p \cdot (1 - p)^{N - 1}$        |
| Optimal Transmission Probability   | $p_{\text{opt}} = \frac{1}{N}$                              |
| Asymptotic Maximum Efficiency      | $\lim_{N \to \infty} P_{\text{channel}} = \frac{1}{e} \approx 36.78\%$ |
+------------------------------------+--------------------------------------------------------------+
```

Where:

- $N$: Total number of active stations/nodes in the network.
- $p$: Probability that a given station attempts transmission in a specific time slot.
- $1 - p$: Probability that a given station remains silent during a specific time slot.

---

Module 6: Step-by-Step Solved Exam Questions & GATE PYQs

Problem 1: Pure ALOHA Throughput & Success Rate Calculation

- **Question**: A pure ALOHA network has a channel transmission rate of 1 Mbps ($10^6$ bps) and sends frames of 1000 bits each. The aggregate transmission attempt rate across all nodes is 1000 frames per second. Calculate:
    1. The mean number of transmissions per frame time ($G$).
    2. The network throughput in frames per second.
- **Step-by-Step Solution**:
    1. Calculate frame transmission delay ($t_x$): $$t_x = \frac{1000 \text{ bits}}{10^6 \text{ bits/sec}} = 10^{-3} \text{ seconds} = 1 \text{ ms}$$
    2. Calculate $G$ (transmissions per frame time $t_x$): $$G = (1000 \text{ frames/sec}) \times (0.001 \text{ sec/frame}) = \mathbf{1}$$
    3. Calculate normalized throughput ($S$): $$S = G \cdot e^{-2G} = 1 \cdot e^{-2(1)} = e^{-2} = \frac{1}{e^2} \approx 0.13533$$
    4. Calculate successful frames delivered per second: $$\text{Throughput} = S \times (1000 \text{ attempts/sec}) = 0.13533 \times 1000 = \mathbf{135.33 \text{ frames/sec}}$$
- **Answer**: $G = 1$, Throughput = **135 frames/second**.

---

Problem 2: Slotted ALOHA Throughput Calculation

- **Question**: In a slotted ALOHA network, frame transmission delay is 10 ms. The aggregate transmission rate across all nodes is 100 frames per second. Calculate the throughput per frame time.
- **Step-by-Step Solution**:
    1. Calculate frame transmission delay ($t_x$): $$t_x = 10 \text{ ms} = 0.01 \text{ seconds}$$
    2. Calculate $G$ (transmissions per frame time $t_x$): $$G = (100 \text{ frames/sec}) \times (0.01 \text{ sec}) = \mathbf{1}$$
    3. Apply Slotted ALOHA throughput formula for $G = 1$: $$S = G \cdot e^{-G} = 1 \cdot e^{-1} = \frac{1}{e} \approx \mathbf{0.368}$$
- **Answer**: **0.368**.

---

Problem 3: Maximum Hosts Supported in Time-Slotted Protocol

- **Question**: A time-slotted MAC protocol operates with transmission probability $p = 0.2$ per slot for each host. What is the maximum number of hosts $N$ that this protocol can support if each host must be provided a minimum throughput of 0.16 frames per slot?
- **Step-by-Step Solution**:
    1. Formula for host throughput in a slotted system: $$P_{\text{host}} = p \cdot (1 - p)^{N - 1}$$
    2. Substitute given values $P_{\text{host}} = 0.16$ and $p = 0.2$: $$0.16 = 0.2 \cdot (1 - 0.2)^{N - 1}$$
    3. Simplify: $$\frac{0.16}{0.2} = (0.8)^{N - 1} \implies 0.8 = (0.8)^{N - 1}$$
    4. Equate exponents: $$N - 1 = 1 \implies N = \mathbf{2}$$
- **Answer**: **2 hosts**.

---

Problem 4: Collision-Free Transmission Probability in Slotted LAN

- **Question**: Consider a slotted LAN with four nodes $S_1, S_2, S_3, S_4$. The frame generation probabilities in a time slot are $p_1 = 0.1, p_2 = 0.2, p_3 = 0.3, p_4 = 0.4$ respectively. Calculate the probability of sending a frame in the first slot without any collision.
- **Step-by-Step Solution**:
    1. A collision-free transmission occurs if **exactly one node transmits** while the other three nodes remain silent.
    2. Calculate individual success probabilities for each node:
        - $S_1$ **succeeds**: $p_1(1-p_2)(1-p_3)(1-p_4) = 0.1 \times 0.8 \times 0.7 \times 0.6 = 0.0336$
        - $S_2$ **succeeds**: $(1-p_1)p_2(1-p_3)(1-p_4) = 0.9 \times 0.2 \times 0.7 \times 0.6 = 0.0756$
        - $S_3$ **succeeds**: $(1-p_1)(1-p_2)p_3(1-p_4) = 0.9 \times 0.8 \times 0.3 \times 0.6 = 0.1296$
        - $S_4$ **succeeds**: $(1-p_1)(1-p_2)(1-p_3)p_4 = 0.9 \times 0.8 \times 0.7 \times 0.4 = 0.2016$
    3. Sum the probabilities for total collision-free probability: $$P(\text{Collision-Free}) = 0.0336 + 0.0756 + 0.1296 + 0.2016 = \mathbf{0.44}$$
- **Answer**: **0.44**.