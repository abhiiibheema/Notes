Comprehensive Master Notes: Transport Layer 01 & 02

---

Module 1: Transport Layer Architecture & Layer Boundaries

The **Transport Layer** (Layer 4 in OSI / Layer 3 in TCP/IP) provides **logical, end-to-end communication between application processes** running on distinct hosts.

```
+-----------------------------------------------------------------------------------+
|                                LAYER BOUNDARY SUMMARY                             |
+-------------------+----------------------------+-----------------+----------------+
| Layer             | Communication Scope        | Addressing      | PDU Name       |
+-------------------+----------------------------+-----------------+----------------+
| Application Layer | Process-to-Process Logic   | Service Name    | Message        |
| Transport Layer   | Process-to-Process Delivery| 16-Bit Port No. | Segment        |
| Network Layer     | Host-to-Host Routing       | 32-Bit IP Addr. | Datagram       |
| Data Link Layer   | Node-to-Node (Hop-by-Hop)  | 48-Bit MAC Addr.| Frame          |
| Physical Layer    | Physical Bit Transmission  | Physical Medium | Bits           |
+-------------------+----------------------------+-----------------+----------------+
```

Application Layer to Transport Layer Protocol Mapping

```
                         APPLICATION TO TRANSPORT MAPPING
                         
    +------------------------------------+------------------------------------+
    |                                    |                                    |
    v                                    v                                    v
TCP (Connection-Oriented)     UDP (Connectionless)          DUAL (TCP / UDP)
-------------------------     --------------------          ----------------
- HTTP / 1.1 & HTTP / 2       - HTTP / 3 (via QUIC)         - DNS (UDP default for
- FTP (File Transfer)         - DHCP (IP Allocation)          queries; TCP for zone
- SMTP (Email Send)           - SNMP (Net Management)         transfers / >512B)
- POP3 / IMAP (Email Read)    - RIP (Routing Protocol)
- BGP (Routing Protocol)      - Real-Time Streaming (RTP)
```

![[Pasted image 20261031162759.png]]
---

Module 2: Sliding Window Error & Flow Control Protocols

To handle noisy or unreliable physical channels, the transport layer employs **Sliding Window Automatic Repeat Request (ARQ)** protocols. Pipelining allows the sender to transmit multiple unacknowledged frames up to a specified window limit, maximizing channel bandwidth utilization.

```
                          SLIDING WINDOW PROTOCOLS
                                     |
           +-------------------------+-------------------------+
           |                                                   |
     GO-BACK-N ARQ                                   SELECTIVE REPEAT ARQ
  (Transmitter-Centric)                               (Receiver-Centric)
  - Receiver Window Size (Wr) = 1                     - Receiver Window Size (Wr) = Ws = N
  - Cumulative Acknowledgments                        - Selective Acknowledgments
  - Retransmits N frames on loss                      - Retransmits ONLY the lost frame
  - Minimum Sequence Numbers = N + 1                  - Minimum Sequence Numbers = 2N
```

---

1. Go-Back-N (GBN) ARQ Mechanics

- **Transmit Window Size (**$W_s$**)**: Set to $N$ (where $N > 1$).
- **Receive Window Size (**$W_r$**)**: Fixed strictly at $1$.
- **Receiver Logic**: Accepts frames **strictly in sequential order**. Any out-of-order frame is discarded immediately.
- **Acknowledgment Type**: Uses **Cumulative (Combined) Acknowledgments**. An ACK for sequence number $k$ confirms receipt of all frames up to and including $k$.
- **Retransmission Trigger**: On timeout or packet corruption, the sender retransmits **all** $N$ **unacknowledged frames** starting from the lost frame.

Sequence Number Formulas (GBN):

- **Minimum Distinct Sequence Numbers Needed**: $$S_{\min} = W_s + W_r = N + 1$$
- **Sequence Field Size (**$k$ **bits)**: $$k = \lceil \log_2(N + 1) \rceil \text{ bits}$$
- **Maximum Sender Window Size for a** $k$**-bit Header Field**: $$W_{s,\max} = 2^k - 1$$

---

2. Selective Repeat (SR) ARQ Mechanics

- **Transmit Window Size (**$W_s$**)**: Set to $N$.
- **Receive Window Size (**$W_r$**)**: Set equal to the transmit window size: $W_r = W_s = N$.
- **Receiver Logic**: Accepts and **buffers out-of-order frames** provided they fall within the active receiving window range $[i \text{ to } i + N - 1]$.
- **Acknowledgment Type**: Uses **Selective Acknowledgments (Selective ACK)**. Each correctly received frame is acknowledged individually.
- **Retransmission Trigger**: Upon timeout or receipt of a NACK (Selective Reject), the sender retransmits **only the specific lost frame**.

```
                     SELECTIVE REPEAT RECEIVER LOGIC
                     
                Frame Arrives with Sequence Number (seq)
                                   |
                  +----------------+----------------+
                  |                                 |
     In Expected Range [i to i+N-1]?            Outside Range?
                  |                                 |
         +--------+--------+               +--------+--------+
         |                 |               |                 |
    In Order?        Out of Order?     Duplicate Frame?   Invalid?
         |                 |               |                 |
     Deliver to        Buffer Frame    Discard Frame &   Discard Frame
   Upper Layer &       & Send Selective  Send Duplicate    Silently
    Send ACK             ACK              ACK
```

Sequence Number Formulas (SR):

- **Minimum Distinct Sequence Numbers Needed**: $$S_{\min} = W_s + W_r = N + N = 2N$$
- **Sequence Field Size (**$k$ **bits)**: $$k = \lceil \log_2(2N) \rceil \text{ bits}$$
- **Maximum Window Size for a** $k$**-bit Header Field**: $$W_{s,\max} = W_{r,\max} = 2^{k - 1}$$
![[Pasted image 20261031163443.png]]
---

Module 3: Protocol Efficiency & Channel Utilization

Channel utilization ($\eta$) measures the fraction of total cycle time during which useful payload data is being transmitted.

```
                           TIMING CYCLE ANALYSIS
                           
       Sender                                              Receiver
         | --- Transmission Delay (tx) ---> |                 |
         | ================================>|                 |
         |                                  | --- Prop (tp) ->|
         |                                                    | (Processes)
         | <---------------- ACK Prop (tp) ------------------ |
```

1. Fundamental Delay Formulas

- **Transmission Delay (**$t_x$**)**: $$t_x = \frac{\text{Frame Size (in bits)}}{\text{Bandwidth (in bits/sec)}}$$
- **Propagation Delay (**$t_p$**)**: $$t_p = \frac{\text{Distance}}{\text{Signal Propagation Velocity}}$$
- **Total Cycle Time (Round-Trip Time per Window)**: $$\text{Cycle Time} = t_x + 2 \cdot t_p$$
- **Delay Ratio (**$a$**)**: $$a = \frac{t_p}{t_x}$$

2. Efficiency ($\eta$) & Throughput Formulas

For a sliding window protocol with window size $N$: $$\eta = \frac{\text{Useful Transmission Time}}{\text{Total Cycle Time}} = \frac{N \cdot t_x}{t_x + 2 \cdot t_p} = \frac{N}{1 + 2a}$$

- **Condition for 100% Channel Utilization (**$\eta = 1$**)**: $$N_{\text{optimal}} = 1 + 2a = 1 + \frac{2 \cdot t_p}{t_x}$$
- **Throughput**: $$\text{Throughput} = \eta \times \text{Bandwidth}$$
![[Pasted image 20261031163648.png]]
---

3. Comprehensive Protocol Comparison Matrix

| Parameter                               | Go-Back-N (GBN) ARQ                | Selective Repeat (SR) ARQ    |
| --------------------------------------- | ---------------------------------- | ---------------------------- |
| **Sender Window Size (**$W_s$**)**      | $N$ ($N > 1$)                      | $N$ ($N > 1$)                |
| **Receiver Window Size (**$W_r$**)**    | Fixed at **1**                     | $W_r = W_s = N$              |
| **ACK Type**                            | Cumulative (Combined) ACK          | Selective ACK                |
| **Out-of-Order Handling**               | Discarded immediately              | Buffered in receiving window |
| **Retransmission Scope**                | All $N$ frames from the lost frame | Only the single lost frame   |
| **Distinct Sequence Numbers Needed**    | $N + 1$                            | $2N$                         |
| **Max Window Size (**$k$**-bit field)** | $2^k - 1$                          | $2^{k-1}$                    |
| **Preferred Environment**               | Low error-rate channels            | High error-rate channels     |

---

Module 4: TCP Connection Establishment — 3-Way Handshake Protocol

Transmission Control Protocol (TCP) is a connection-oriented, full-duplex, reliable byte-stream transport protocol. Before data transfer begins, the client and server initialize parameters using the **3-Way Handshake**.

```
     TCP CLIENT                                                      TCP SERVER
  [ CLOSED State ]                                                [ LISTEN State ]
         |                                                               |
         | ---- Packet 1: SYN (Seq = X, ACK = 0, No Payload) ----------> |
  [ SYN_SENT State ]                                            [ SYN_RCVD State ]
         |                                                               |
         | <--- Packet 2: SYN + ACK (Seq = Y, ACK = X + 1, No Payload) - |
  [ ESTABLISHED State ]                                                  |
         |                                                               |
         | ---- Packet 3: ACK (Seq = X + 1, ACK = Y + 1) -------------> |
  [ ESTABLISHED State ]                                           [ ESTABLISHED State ]
```

Detailed Packet Breakdown

1. **Packet 1 (****SYN** **Segment: Client** $\longrightarrow$ **Server)**:
    - **Control Flags**: `SYN = 1`, `ACK = 0`.
    - **Sequence Number**: `Seq = X` (Client’s randomly chosen Initial Sequence Number, $\text{Client\_ISN}$).
    - **Payload**: **0 bytes (No data)**.
    - **Purpose**: Client requests a new connection and advertises its ISN.
2. **Packet 2 (****SYN-ACK** **Segment: Server** $\longrightarrow$ **Client)**:
    - **Control Flags**: `SYN = 1`, `ACK = 1`.
    - **Sequence Number**: `Seq = Y` (Server’s randomly chosen Initial Sequence Number, $\text{Server\_ISN}$).
    - **Acknowledgment Number**: `ACK = X + 1` (Acknowledges receipt of client's `SYN`).
    - **Payload**: **0 bytes (No data)**.
    - **Purpose**: Server accepts the connection, advertises its own ISN, and acknowledges the client's ISN.
3. **Packet 3 (****ACK** **Segment: Client** $\longrightarrow$ **Server)**:
    - **Control Flags**: `SYN = 0`, `ACK = 1`.
    - **Sequence Number**: `Seq = X + 1`.
    - **Acknowledgment Number**: `ACK = Y + 1` (Acknowledges receipt of server's `SYN`).
    - **Payload**: **May carry data payload OR 0 bytes**.
    - **Purpose**: Client confirms server's ISN; connection becomes active on both endpoints.

---

Module 5: Core TCP Connection Rules & Loss Scenarios

1. Initial Sequence Number (ISN) Rules

- Both the client and server **randomly generate** their respective ISNs ($X$ and $Y$).
- **Security & Reliability**: Random ISN generation prevents sequence number prediction attacks and avoids collision with delayed duplicate packets from previous closed connections.

2. Sequence Space Consumption Rule

- The `SYN` flag **consumes exactly 1 sequence number space**, even though it carries zero payload bytes.
- Consequently, the first byte of actual payload data sent by the client has `Seq = X + 1`.
- Similarly, the first byte of actual payload data sent by the server has `Seq = Y + 1`.

3. TCP State Transitions during Connection Setup

- **Client State Path**: `CLOSED` $\longrightarrow$ `SYN_SENT` $\longrightarrow$ `ESTABLISHED`.
- **Server State Path**: `CLOSED` $\longrightarrow$ `LISTEN` $\longrightarrow$ `SYN_RCVD` $\longrightarrow$ `ESTABLISHED`.

4. Loss Scenarios & Timeout Handling

- **Packet 1 (****SYN****) Lost**: Client retransmission timer expires $\implies$ Client re-sends `SYN` with a new ISN.
- **Packet 2 (****SYN-ACK****) Lost**: Client times out in `SYN_SENT` state $\implies$ Client re-sends `SYN`. Server times out in `SYN_RCVD` state and drops the pending connection entry.
- **Packet 3 (****ACK****) Lost**: If the client transmits a subsequent data packet carrying payload, that data packet automatically functions as the cumulative ACK, completing connection setup at the server.
- **TCP Simultaneous Open**: Occurs when two peer hosts attempt to open a connection to each other simultaneously. Both send `SYN` segments concurrently, transition `SYN_SENT` $\to$ `SYN_RCVD`, and establish the connection via a **4-way cross-handshake**.

![[Pasted image 20261031174321.png|954]]

---

Module 6: Step-by-Step Solved Problems & GATE PYQs

Problem 1: Go-Back-N Retransmission Analysis [GATE-CS-2006]

- **Problem**: Station A sends a 9-packet message (P1 to P9) to Station B using Go-Back-3 ARQ ($W_s = 3$). Every **5th packet transmitted** gets lost (no ACKs are lost). Calculate the total number of frame transmissions required.
- **Step-by-Step Solution**:
    1. **Tx 1**: P1 (Received)
    2. **Tx 2**: P2 (Received)
    3. **Tx 3**: P3 (Received)
    4. **Tx 4**: P4 (Received)
    5. **Tx 5**: P5 **[LOST - 5th transmission]**
    6. GBN discards subsequent out-of-order packets (P6, P7). Sender times out on P5 and retransmits starting from P5:
        - **Tx 6**: P5 (Received)
        - **Tx 7**: P6 (Received)
        - **Tx 8**: P7 (Received)
        - **Tx 9**: P8 (Received)
        - **Tx 10**: P9 **[LOST - 10th transmission]**
    7. Sender times out on P9 and retransmits P9:
        - **Tx 11**: P9 (Received)
    8. Tracing through all 9 message packets yields a total of **16 frame transmissions**.
- **Answer**: **16 transmissions** (Option C).

---

Problem 2: Selective Repeat Sequence Field Bits [GATE-2014, Set-1]

- **Problem**: A Selective Repeat protocol uses frame size = **1 KB**, link bandwidth = **1.5 Mbps**, and one-way latency ($t_p$) = **50 ms**. Find the minimum bits required for the sequence number field to achieve **60% link utilization**.
- **Step-by-Step Solution**:
    1. **Frame Size**: $1 \text{ KB} = 8192 \text{ bits}$.
    2. **Transmission Delay (**$t_x$**)**: $$t_x = \frac{8192 \text{ bits}}{1.5 \times 10^6 \text{ bits/sec}} = 5.4613 \text{ ms}$$
    3. **Cycle Time**: $$\text{Cycle Time} = t_x + 2 \cdot t_p = 5.4613 + 100 = 105.4613 \text{ ms}$$
    4. **Window Size (**$N$**) for 60% Utilization (**$\eta = 0.6$**)**: $$\eta = \frac{N \cdot t_x}{\text{Cycle Time}} \implies 0.6 = \frac{N \cdot 5.4613}{105.4613} \implies N = 11.58 \implies N = 12$$
    5. **Total Distinct Sequence Numbers for Selective Repeat**: $$S_{\min} = 2N = 2 \times 12 = 24$$
    6. **Sequence Field Bits (**$k$**)**: $$k = \lceil \log_2(24) \rceil = \mathbf{5 \text{ bits}}$$
- **Answer**: **5 bits**.

---

Problem 3: Satellite Link Packet Size Calculation [GATE-IT-2008]

- **Problem**: A **1 Mbps** satellite link connects two ground stations. The altitude of the satellite is **36,504 km** and the speed of the signal is $3 \times 10^8\text{ m/s}$. What should be the packet size for a **channel utilization of 25%** using a **Go-Back-127** sliding window protocol?
- **Step-by-Step Solution**:
    1. **Ground-to-Ground Satellite Propagation Distance**: $$d = 2 \times 36,504\text{ km} = 73,008\text{ km} = 73,008,000\text{ m}$$
    2. **One-Way Propagation Delay (**$t_p$**)**: $$t_p = \frac{73,008,000}{3 \times 10^8} = 0.24336\text{ seconds} = 243.36\text{ ms}$$
    3. **Round-Trip Delay (**$2 \cdot t_p$**)**: $$2 \cdot t_p = 2 \times 243.36\text{ ms} = 486.72\text{ ms}$$
    4. **Window Size for Go-Back-127**: $$N = 127$$
    5. **Channel Utilization Formula (**$\eta = 0.25$**)**: $$\eta = \frac{N \cdot t_x}{t_x + 2 \cdot t_p} \implies 0.25 = \frac{127 \cdot t_x}{t_x + 486.72\text{ ms}}$$ $$0.25(t_x + 486.72) = 127 \cdot t_x \implies 121.68 = 126.75 \cdot t_x$$ $$t_x = \frac{121.68\text{ ms}}{126.75} = 0.96\text{ ms} = 0.00096\text{ seconds}$$
    6. **Packet Size in Bytes**: $$\text{Packet Size} = t_x \times \text{Bandwidth} = 0.00096\text{ s} \times 10^6\text{ bits/s} = 960\text{ bits}$$ $$\text{Packet Size in Bytes} = \frac{960}{8} = \mathbf{120\text{ Bytes}}$$
- **Answer**: **120 bytes** (Option C).

---

Problem 4: 3-Way Handshake Flag Verification [GATE-2025, Set-1]

- **Problem**: Consider the 3-way handshaking protocol for TCP connection establishment. Let the three packets exchanged be denoted as $P_1, P_2, P_3$ in order. Which of the following option(s) is/are TRUE regarding the flags set in these packets?
    - **(A)** $P_1: \text{SYN} = 1, \text{ACK} = 0$
    - **(B)** $P_2: \text{SYN} = 1, \text{ACK} = 1$
    - **(C)** $P_2: \text{SYN} = 0, \text{ACK} = 1$
    - **(D)** $P_3: \text{SYN} = 1, \text{ACK} = 1$
- **Step-by-Step Solution**:
    - Packet 1 (Client $\to$ Server): `SYN = 1, ACK = 0` $\implies$ **Option A is True**.
    - Packet 2 (Server $\to$ Client): `SYN = 1, ACK = 1` $\implies$ **Option B is True**; Option C is False.
    - Packet 3 (Client $\to$ Server): `SYN = 0, ACK = 1` $\implies$ Option D is False.
- **Answer**: **Options A and B**.

---

Problem 5: TCP ISN & Acknowledgment Rules [GATE-2024, Set-1]

- **Problem**: TCP client P establishes a connection to TCP server Q. Let $N_P$ denote the sequence number in the `SYN` sent from P to Q. Let $N_Q$ denote the acknowledgment number in the `SYN-ACK` from Q to P. Which of the following statements is/are CORRECT?
    - **(A)** The sequence number $N_P$ is always 0 for a new connection.
    - **(B)** The sequence number $N_P$ is chosen randomly by P.
    - **(C)** The acknowledgment number $N_Q$ is equal to $N_P$.
    - **(D)** The acknowledgment number $N_Q$ is equal to $N_P + 1$.
- **Step-by-Step Solution**:
    - $N_P$ is randomly generated by client P $\implies$ Option A is False, **Option B is True**.
    - Server Q acknowledges P's `SYN` by incrementing $N_P$ by 1: $N_Q = N_P + 1$ $\implies$ Option C is False, **Option D is True**.
- **Answer**: **Options B and D**.

---

Problem 6: Server States during 3-Way Handshake [GATE-IT-2008]

- **Problem**: Which of the following statements are TRUE for the 'three-way handshake' for TCP connection establishment?
    - **(S1)** Loss of `SYN + ACK` from the server will not establish a connection.
    - **(S2)** Loss of `ACK` from the client cannot establish the connection.
    - **(S3)** The server moves `LISTEN` $\to$ `SYN_RCVD` $\to$ `SYN_SENT` $\to$ `ESTABLISHED` in the state machine on no packet loss.
    - **(S4)** The server moves `LISTEN` $\to$ `SYN_RCVD` $\to$ `ESTABLISHED` in the state machine on no packet loss.
- **Step-by-Step Solution**:
    - S1 is True: If `SYN-ACK` is lost, client remains in `SYN_SENT` and no connection is formed.
    - S2 is False: Client enters `ESTABLISHED` on receiving `SYN-ACK`, and subsequent client data can complete connection setup on the server.
    - S3 is False & S4 is True: Server state flow is `LISTEN` $\longrightarrow$ `SYN_RCVD` $\longrightarrow$ `ESTABLISHED`.
- **Answer**: **Option B (S1 and S4 only)**.

---

Problem 7: Transport Protocol Functionality [GATE-CS-2003]

- **Problem**: Which of the following functionalities must be implemented by a transport protocol over and above the network protocol?
    - **(A)** Recovery from packet losses
    - **(B)** Detection of duplicate packets
    - **(C)** Packet delivery in the correct order
    - **(D)** End-to-end connectivity
- **Step-by-Step Solution**:
    - The primary fundamental boundary that the Transport Layer adds on top of Network Layer host-to-host delivery is **End-to-End (Process-to-Process) connectivity**.
- **Answer**: **Option D**.

---

Problem 8: Multi-Path Packet Routing [GATE-CS-2005]

- **Problem**: Packets of the same session may be routed through different paths in:
    - **(A)** TCP, but not UDP
    - **(B)** TCP and UDP
    - **(C)** UDP, but not TCP
    - **(D)** Neither TCP, nor UDP
- **Step-by-Step Solution**:
    - Both TCP and UDP rely on IP at the Network Layer. Since IP is a connectionless datagram network layer, individual IP datagrams carrying TCP segments or UDP datagrams can take different physical routes through intermediate routers.
- **Answer**: **Option B (TCP and UDP)**.