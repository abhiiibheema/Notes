Master Notes: Carrier Sense Multiple Access (CSMA) & CSMA/CD Collision Detection (Batch 2)

---

Module 1: Carrier Sense Multiple Access (CSMA) Principles

While ALOHA protocols allow nodes to transmit without checking channel status, **Carrier Sense Multiple Access (CSMA)** introduces a fundamental rule to minimize collisions: **"Sense before transmit"** (listen before speaking).

```
                            CSMA BASIC TRANSMISSION LOGIC
                            
   Node wants to transmit frame
                |
                v
   Sense the shared channel
                |
        +-------+-------+
        |               |
   Sensed IDLE     Sensed BUSY
        |               |
        v               v
  Transmit Frame   Defer Transmission
  (Immediate/p)    (Follow Persistence Rule)
```

1. How CSMA Reduces Collisions

- In ALOHA, a node transmits immediately, leading to a high rate of overlapping frames.
- In CSMA, if another node is currently transmitting, a sensing node detects the carrier signal (voltage level/rf energy) and **defers its transmission**, avoiding an obvious collision.

2. Why Collisions Can Still Occur in CSMA

Despite carrier sensing, collisions are **not completely eliminated** due to **Propagation Delay (**$t_p$**)**.

```
                   WHY COLLISIONS OCCUR IN CSMA
                   
   t = 0: Node A senses channel (IDLE) and starts transmitting Frame A.
   
          A | === Frame A Signal Propagation ===>               | B
            +---------------------------------------------------+
            
   t = t_p - ε: Frame A signal has NOT YET reached Node B.
                Node B senses channel (IDLE) and starts transmitting Frame B!
                
   t = t_p: Signals collide in transit!
```

- **Vulnerable Window**: If Node A transmits at time $t = 0$, the signal takes $t_p$ time to reach the furthest Node B.
- Any node sensing the channel between $t = 0$ and $t = t_p$ will perceive the channel as idle and transmit, resulting in a collision.

---

Module 2: CSMA Persistence Modes

When a node has a frame to send but senses that the channel is **busy**, its behavior is determined by its **Persistence Mode**.

```
+---------------------------------------------------------------------------------------------------+
|                                 CSMA PERSISTENCE MODES COMPARISON                                 |
+-------------------+------------------------------------+------------------------------------------+
| Persistence Mode  | Sensing Behavior When Busy         | Transmission When Idle                   |
+-------------------+------------------------------------+------------------------------------------+
| 1-Persistent      | Senses continuously until idle     | Transmits immediately with probability 1 |
| Non-Persistent    | Waits a random back-off time,      | Transmits immediately                    |
|                   | then senses channel again          |                                          |
| p-Persistent      | Senses continuously until idle     | Transmits with probability p,            |
| (Slotted)         |                                    | defers to next slot with probability 1-p |
+-------------------+------------------------------------+------------------------------------------+
```

```
                        SENSING BEHAVIOR OF PERSISTENCE MODES
                        
   1-Persistent:    Busy? ---> [Sense Continuously] ---> Becomes Idle ---> Transmit Immediately (p=1)
   
   Non-Persistent:  Busy? ---> [Wait Random Time]   ---> Sense Again   ---> If Idle, Transmit
   
   p-Persistent:    Busy? ---> [Sense Continuously] ---> Becomes Idle ---> Prob(p): Transmit
                                                                           ---> Prob(1-p): Wait 1 Slot
```

1. 1-Persistent CSMA

- **Rule**: If the channel is busy, the node continues sensing continuously until the channel becomes idle. The moment it becomes idle, the node transmits immediately with **probability 1**.
- **Advantage**: Maximizes channel utilization when traffic is light (no idle time wasted after channel clears).
- **Disadvantage**: High collision probability under heavy traffic. If two or more nodes are waiting while a channel is busy, all of them will sense it become idle simultaneously and transmit at once, guaranteeing a collision.

2. Non-Persistent CSMA

- **Rule**: If the channel is busy, the node **does not sense continuously**. It waits a **random back-off duration**, then senses the channel again.
- **Advantage**: Drastically reduces collisions under heavy traffic because waiting nodes pick different random delays and do not transmit at the same instant.
- **Disadvantage**: Increases channel idle time under light traffic, reducing efficiency.

3. p-Persistent CSMA

- **Rule**: Used in time-slotted channels. When the channel becomes idle, the node transmits with probability $p$, and defers to the next time slot with probability $1 - p$. If the next slot is also idle, it repeats the process.
- **Traffic Adaptation**:
    - Under heavy traffic, **decrease** $p$ to avoid collisions among multiple waiting nodes.
    - Under light traffic, **increase** $p$ toward 1 to minimize delay.

---

Module 3: CSMA with Collision Detection (CSMA/CD) Architecture

In standard CSMA, if a collision occurs, nodes continue transmitting their full frames blindly because they do not monitor the channel during transmission. **CSMA/CD** improves this by enforcing: **"Sense before transmit AND sense DURING transmission."**

```
                       CSMA/CD DUAL-SENSING MECHANISM
                       
   Sender Node
  +-----------------------------------------------------------------+
  | 1. Sense channel before transmitting (Carrier Sensing)          |
  | 2. If IDLE, start transmitting frame data                       |
  | 3. WHILE transmitting, continuously listen to channel voltage   |
  |    - If channel signal matches transmitted signal ===> Success  |
  |    - If channel signal is corrupted / double voltage ===> COLLISION! |
  +-----------------------------------------------------------------+
```

Key Properties of CSMA/CD

- **Used In**: Wired Local Area Networks (specifically **Ethernet / IEEE 802.3** bus topologies).
- **No Receiver ACKs**: CSMA/CD operates **without receiver acknowledgments**. Transmission success is inferred at the sender level if no collision is detected during frame transmission.
- **Immediate Abort**: As soon as a transmitting node detects a collision, it **stops transmitting frame data immediately**, saving bandwidth.

---

Module 4: Collision Detection Condition & Minimum Frame Size Derivation

For CSMA/CD to work correctly, a sender must be able to detect a collision **while it is still in the process of transmitting the frame**. If a frame is too short, the sender might finish transmitting before the collision signal returns, mistakenly believing the frame was delivered safely.

```
                      WORST-CASE COLLISION PROPAGATION TIMELINE
                      
   Node A (x = 0)                                                    Node B (x = L)
     | --- Frame A Signal Starts (t = 0) -----------------------------> |
     |                                                                 | (At t = tp - α)
     |                                                                 | Node B transmits Frame B!
     |                                                                 | COLLISION OCCURS NEAR B!
     | <--- Collision Wave Signal Travels Back ------------------------ |
     | 
   (At t = 2*tp - 2α)
   Collision signal arrives back at Node A!
   
   Worst-Case Boundary (α -> 0):
   Node A receives collision signal at time t = 2 * tp
```

1. Derivation of the Collision Detection Requirement

- Let $t_p$ be the one-way propagation delay between the two furthest nodes in the network (Node A and Node B).
- Node A begins transmitting at $t = 0$.
- In the worst-case scenario, Node B senses the channel just before Node A's signal arrives at $t = t_p - \epsilon$, finds it idle, and starts transmitting.
- A collision occurs near Node B at time $t \approx t_p$.
- The corrupted collision wave must travel all the way back across the link to Node A, arriving at time $t = 2 t_p$.
- **Fundamental CSMA/CD Condition**: Node A's frame transmission delay ($t_x$) must be at least as long as the Round-Trip Propagation Delay ($2 t_p$):

$$t_x \ge 2 \cdot t_p$$

2. Minimum Frame Size Formula

Since Frame Transmission Delay $t_x = \frac{\text{Frame Size}}{\text{Bandwidth}}$:

$$\frac{\text{Minimum Frame Size}}{\text{Bandwidth}} \ge 2 \cdot t_p$$

$$\text{Minimum Frame Size} = 2 \cdot t_p \cdot \text{Bandwidth}$$

Expressed in terms of distance and signal velocity:

$$\text{Minimum Frame Size} = 2 \cdot \left( \frac{\text{Distance}}{\text{Signal Speed}} \right) \cdot \text{Bandwidth}$$

3. Impact of Jamming Signal

When a node detects a collision, it immediately transmits a **48-bit Jamming Signal** ($t_{xj}$) to ensure all other nodes on the bus register the collision.

- Including the jamming signal transmission time:

$$t_x \ge 2 \cdot t_p + t_{xj}$$

$$\text{Minimum Frame Size} = (2 \cdot t_p + t_{xj}) \cdot \text{Bandwidth} = 2 \cdot t_p \cdot \text{Bandwidth} + \text{Jamming Signal Size}$$

---

Module 5: Step-by-Step Solved Exam Questions & GATE PYQs

Problem 1: Minimum Packet Size in Broadcast LAN

- **Question**: A 2 km long broadcast LAN has $10^7$ bps bandwidth and uses CSMA/CD. The signal travels along the wire at $2 \times 10^8$ m/s. What is the minimum packet size that can be used on this network?
    - (A) 50 bytes
    - (B) 100 bytes
    - (C) 200 bytes
    - (D) None of the above
- **Step-by-Step Solution**:
    1. Calculate one-way propagation delay ($t_p$): $$t_p = \frac{\text{Distance}}{\text{Signal Speed}} = \frac{2 \text{ km}}{2 \times 10^8 \text{ m/s}} = \frac{2000 \text{ m}}{2 \times 10^8 \text{ m/s}} = 10^{-5} \text{ seconds} = 10\ \mu\text{s}$$
    2. Calculate Round-Trip Propagation Delay ($2 t_p$): $$2 t_p = 2 \times 10^{-5} \text{ seconds}$$
    3. Calculate minimum frame size in bits: $$\text{Min Frame Size} = 2 t_p \times \text{Bandwidth} = (2 \times 10^{-5} \text{ sec}) \times (10^7 \text{ bps}) = 200 \text{ bits}$$
    4. Convert to bytes: $$\text{Min Frame Size in Bytes} = \frac{200 \text{ bits}}{8 \text{ bits/byte}} = \mathbf{25 \text{ bytes}}$$
    5. Check options: 50 bytes, 100 bytes, 200 bytes are incorrect.
- **Answer**: **Option D (None of the above; correct answer is 25 bytes)**.

---

Problem 2: Minimum Frame Size in Gigabit CSMA/CD LAN

- **Question**: A network with CSMA/CD protocol in the MAC layer is running at 1 Gbps over a 1 km cable with no repeaters. The signal speed in the cable is $2 \times 10^8$ m/sec. Calculate the minimum frame size required for this network.
- **Step-by-Step Solution**:
    1. Calculate one-way propagation delay ($t_p$): $$t_p = \frac{1000 \text{ m}}{2 \times 10^8 \text{ m/s}} = 5 \times 10^{-6} \text{ seconds} = 5\ \mu\text{s}$$
    2. Calculate Round-Trip Propagation Delay ($2 t_p$): $$2 t_p = 2 \times (5 \times 10^{-6} \text{ sec}) = 10^{-5} \text{ seconds}$$
    3. Calculate minimum frame size in bits ($\text{Bandwidth} = 1 \text{ Gbps} = 10^9 \text{ bps}$): $$\text{Min Frame Size} = (10^{-5} \text{ sec}) \times (10^9 \text{ bps}) = \mathbf{10,000 \text{ bits}}$$
    4. Convert to bytes: $$\text{Min Frame Size in Bytes} = \frac{10,000}{8} = \mathbf{1250 \text{ bytes}}$$
- **Answer**: **10,000 bits (or 1250 bytes)**.

---

Problem 3: Minimum Frame Size for 200m Cable

- **Question**: The minimum frame size required for a CSMA/CD based computer network running at 1 Gbps on a 200 m cable with a link speed of $2 \times 10^8$ m/s is:
    - (A) 125 bytes
    - (B) 250 bytes
    - (C) 500 bytes
    - (D) None of these
- **Step-by-Step Solution**:
    1. Calculate one-way propagation delay ($t_p$): $$t_p = \frac{200 \text{ m}}{2 \times 10^8 \text{ m/s}} = 10^{-6} \text{ seconds} = 1\ \mu\text{s}$$
    2. Calculate Round-Trip Propagation Delay ($2 t_p$): $$2 t_p = 2 \times 10^{-6} \text{ seconds}$$
    3. Calculate minimum frame size in bits: $$\text{Min Frame Size} = (2 \times 10^{-6} \text{ sec}) \times (10^9 \text{ bps}) = 2000 \text{ bits}$$
    4. Convert to bytes: $$\text{Min Frame Size in Bytes} = \frac{2000 \text{ bits}}{8} = \mathbf{250 \text{ bytes}}$$
- **Answer**: **Option B (250 bytes)**.

---

Problem 4: Minimum Size of Frame Given Maximum Propagation Delay

- **Question**: A network has a data transmission bandwidth of $20 \times 10^6$ bits per second. It uses CSMA/CD in the MAC layer. The maximum signal propagation time from one node to another node is 40 microseconds. Find the minimum size of a frame in the network in bytes.
- **Step-by-Step Solution**:
    1. Given: Bandwidth $= 20 \times 10^6$ bps, $t_p = 40\ \mu\text{s} = 40 \times 10^{-6}$ seconds.
    2. Calculate Round-Trip Propagation Delay ($2 t_p$): $$2 t_p = 2 \times (40 \times 10^{-6} \text{ sec}) = 80 \times 10^{-6} \text{ seconds}$$
    3. Calculate minimum frame size in bits: $$\text{Min Frame Size} = (80 \times 10^{-6} \text{ sec}) \times (20 \times 10^6 \text{ bps}) = 1600 \text{ bits}$$
    4. Convert to bytes: $$\text{Min Frame Size in Bytes} = \frac{1600}{8} = \mathbf{200 \text{ bytes}}$$
- **Answer**: **200 bytes**.

---

Problem 5: Ethernet Segment Minimum Frame Size

- **Question**: Consider an Ethernet segment with a transmission speed of $10^8$ bits/sec and a maximum segment length of 500 meters. If the speed of propagation of the signal in the medium is $2 \times 10^8$ meters/sec, then the minimum frame size (in bits) required for collision detection is ______.
- **Step-by-Step Solution**:
    1. Calculate one-way propagation delay ($t_p$): $$t_p = \frac{500 \text{ m}}{2 \times 10^8 \text{ m/s}} = 2.5 \times 10^{-6} \text{ seconds}$$
    2. Calculate Round-Trip Propagation Delay ($2 t_p$): $$2 t_p = 2 \times (2.5 \times 10^{-6} \text{ sec}) = 5 \times 10^{-6} \text{ seconds}$$
    3. Calculate minimum frame size in bits: $$\text{Min Frame Size} = (5 \times 10^{-6} \text{ sec}) \times (10^8 \text{ bps}) = \mathbf{500 \text{ bits}}$$
- **Answer**: **500 bits**.

---

Problem 6: Carrier-Sense Timing Analysis & Collision Avoidance

- **Question**: A communication system uses carrier sensing where a node must carrier-sense the medium for 5 units of time. If no other transmission is detected during this duration, it starts transmitting in the next time unit. If it detects another transmission, it waits until that transmission finishes and senses again for 5 time units. Transmissions last for 20 units of time. Signal propagation speed is 10 meters per unit time. Node P starts transmitting a packet at $t = 0$ after completing its sensing phase. Node Q has a packet to transmit at $t = 0$ and begins carrier-sensing. Find the maximum distance $d$ (in meters) between P and Q that allows Q to successfully avoid a collision.
- **Step-by-Step Solution**:
    1. Node P starts its transmission at time $t = 0$.
    2. Node Q starts carrier-sensing at $t = 0$ and will sense for 5 time units (from $t = 0$ to $t = 5$).
    3. If Node Q detects Node P's transmission signal at or before $t = 5$, Q will defer transmission and avoid a collision.
    4. The signal from Node P takes propagation delay $t_p = \frac{d}{\text{speed}} = \frac{d}{10}$ to reach Node Q.
    5. For Node Q to receive P's signal before its sensing phase ends at $t = 5$: $$t_p \le 5 \text{ time units} \implies \frac{d}{10} \le 5 \implies d \le 50 \text{ meters}$$
- **Answer**: **50 meters**.

---
