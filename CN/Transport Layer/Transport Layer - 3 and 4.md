Comprehensive Master Notes: Transport Layer 03 & 04 (Sockets, Multiplexing, Buffers & Data Transfer)

---

Module 1: Network Sockets & Process Communication Architecture

A **Network Socket** serves as the software interface, "door," or endpoint of a two-way communication link between an application process and the network protocol stack.

```
                      NETWORK SOCKET ARCHITECTURE
                      
    Host A Application                                Host B Application
     [ Process P1 ]                                    [ Process P2 ]
           |                                                 |
  +-----------------+                               +-----------------+
  | Network Socket  |  <====== Logical Pair =====>  | Network Socket  |
  +-----------------+          of Sockets           +-----------------+
           |                                                 |
   Transport Layer                                   Transport Layer
   (TCP / UDP)                                       (TCP / UDP)
```

1. Core Properties of Sockets

- **Interface**: Acts as an abstraction layer between user-space application processes and kernel-space transport layer implementations.
- **Socket Pair Concept**: Communication between two network processes occurs via a **pair of sockets** (one at the sender host, one at the receiver host).
- **Data Structure**: Maintained internally by the OS kernel to track state variables, buffer memory addresses, IP addresses, and port numbers.

2. Two Primary Transport Layer Socket Types

```
                           SOCKET TYPES
                                 |
         +-----------------------+-----------------------+
         |                                               |
  DATAGRAM SOCKET                                STREAM SOCKET
  (SOCK_DGRAM)                                  (SOCK_STREAM)
  - Bound to UDP protocol                       - Bound to TCP protocol
  - Connectionless                              - Connection-Oriented
  - Unreliable, datagram-based                  - Reliable, byte-stream based
```

- **Datagram Socket (****SOCK_DGRAM****)**: Used by **UDP**. Transfers independent messages (datagrams) without establishing a prior connection.
- **Stream Socket (****SOCK_STREAM****)**: Used by **TCP**. Establishes a reliable, full-duplex, point-to-point byte-stream connection.

![[Pasted image 20261031181918.png]]
---

Module 2: Multiplexing & Demultiplexing Mechanics

Multiplexing at the sender and demultiplexing at the receiver allow a single physical network interface and IP layer to serve multiple concurrent application processes.

```
                           SENDER MULTIPLEXING
                           
    [ Process P1 ]     [ Process P2 ]     [ Process P3 ]
    (Socket 1)          (Socket 2)          (Socket 3)
         \                  |                  /
          +-----------------+-----------------+
                            |
                   Transport Layer (TCP/UDP)
               (Adds Header: Ports & Controls)
                            |
                     Network Layer (IP)
```

1. Sender Multiplexing

- **Mechanism**: Collects message chunks from multiple active application sockets, encapsulates each chunk with a **Transport Layer Protocol Header** containing 16-bit **Source Port** and **Destination Port** fields, and passes the resulting segments down to the Network Layer (IP).

2. Receiver Demultiplexing

- **Mechanism**: Receives transport layer segments from the IP layer, inspects header fields, and delivers the payload to the exact application socket meant to receive it.

```
                        DEMULTIPLEXING SCHEMES
                                   |
         +-------------------------+-------------------------+
         |                                                   |
  CONNECTIONLESS DEMUX                                CONNECTION-ORIENTED DEMUX
  (UDP Protocol)                                     (TCP Protocol)
  - Identifies socket using 2-Tuple:                 - Identifies socket using 4-Tuple:
    1. Destination IP Address                          1. Source IP Address
    2. Destination Port Number                         2. Source Port Number
                                                       3. Destination IP Address
                                                       4. Destination Port Number
```

![[Pasted image 20261031182446.png]]

---

Module 3: Concurrent TCP Server Architecture & Socket Lifecycle

A concurrent TCP server can handle multiple simultaneous client connections over a single advertised port number by spawning dedicated child processes and connection sockets.

```
                  CONCURRENT TCP SERVER WORKFLOW
                  
TCP Client (P1)                                 TCP Server (P2)
Port: A                                         Port: B (Welcome Socket)
  |                                                    |
  | -------- 3-Way Handshake Connection Request -----> |
  |                                                    |
  |                                          [ Calls fork() ]
  |                                                    | Creates Child P21
  | <======= Logical Data Transfer Channel ==========> | Connection Socket 1
  |            (Full Duplex / Point-to-Point)          | (Port B)
```

1. Welcome Socket vs. Connection Socket

- **Welcome Socket (Listening Socket)**:
    - Created by the server process (e.g., $P_2$ on Port $B$) using `SOCK_STREAM`.
    - Responsible **strictly** for listening to incoming connection requests and conducting the 3-Way Handshake.
- **Connection Socket (Data Transfer Socket)**:
    - When a 3-way handshake succeeds, the server executes a system call (such as `fork()`) to create a child process ($P_{21}$).
    - A new **Connection Socket** is instantiated for $P_{21}$ on the **same port (**$B$**)**.
    - All subsequent data transfer for Client $P_1$ occurs through this dedicated Connection Socket, leaving the Welcome Socket free to accept new clients.

2. Multiple Concurrent Clients Example

If a second client ($P_3$ on Port $C$) connects to the server Port $B$:

1. $P_3$ completes the 3-way handshake with $P_2$'s Welcome Socket.
2. Server spawns a second child process ($P_{23}$) with Connection Socket 2 on Port $B$.
3. Incoming TCP segments arriving at Port $B$ are demultiplexed to $P_{21}$ or $P_{23}$ using the full **4-Tuple**:
    - Segment for $P_{21}$: `(Client_IP_1, Port A, Server_IP, Port B)`
    - Segment for $P_{23}$: `(Client_IP_2, Port C, Server_IP, Port B)`

![[Pasted image 20261031183144.png]]

---

Module 4: TCP Send & Receive Buffer State Management

Each TCP Stream Socket maintains two distinct circular memory buffers in kernel space: a **Send Buffer** (`Send Buffer`) and a **Receive Buffer** (`Recv Buffer`).

```
                    SOCKET BUFFER ARCHITECTURE
                    
+-------------------------------------------------------------------+
|                        TCP STREAM SOCKET                          |
|                                                                   |
|   +-----------------------------------------------------------+   |
|   | Send Buffer (Holds outgoing application bytes)            |   |
|   +-----------------------------------------------------------+   |
|                                                                   |
|   +-----------------------------------------------------------+   |
|   | Receive Buffer (Holds incoming segments & out-of-order)   |   |
|   +-----------------------------------------------------------+   |
+-------------------------------------------------------------------+
```

1. Sender State Variables (Client / Transmitter)

Let $X$ be the Client's Initial Sequence Number ($\text{Client\_ISN}$).

- **LastByteWritten**: The sequence number offset of the last byte written into the Send Buffer by the application process.
- **LastByteSent**: The sequence number offset of the highest byte transmitted out over the network by TCP.
- **LastByteAcked**: The sequence number offset of the highest byte acknowledged by the receiver.

```
                          SEND BUFFER STRUCTURE
                          
   [ Bytes Acked & Freed ] [ Bytes Sent, Unacked ] [ Bytes Written, Unsent ] [ Available ]
  +-----------------------+-----------------------+-------------------------+-------------+
  |                       |                       |                         |             |
  +-----------------------+-----------------------+-------------------------+-------------+
  ^                       ^                       ^                         ^
  Begin                   LastByteAcked           LastByteSent              LastByteWritten
```

Initial State (After Handshake, Before Data):

$$\text{LastByteWritten} = \text{LastByteSent} = \text{LastByteAcked} = \text{Client\_ISN} = X$$

Send Buffer Capacity Constraint:

The total unacknowledged bytes in the Send Buffer cannot exceed `MaxSendBuffer`: $$(\text{LastByteWritten} - \text{LastByteAcked}) \le \text{MaxSendBuffer}$$

Application Write Condition:

If process wants to write $N$ bytes:

- **If** $[(\text{LastByteWritten} - \text{LastByteAcked}) + N] > \text{MaxSendBuffer}$: Application is blocked/must wait.
- **Else**: Write allowed; update $\text{LastByteWritten} = \text{LastByteWritten} + N$.

---

2. Receiver State Variables (Server / Target)

Let $Y$ be the Server's Initial Sequence Number ($\text{Server\_ISN}$) and $X$ be the Client's ISN.

- **LastByteRead**: Sequence number offset of the last byte read from `Recv Buffer` by the receiving application process.
- **NextByteExpected**: The sequence number of the next contiguous byte TCP expects to receive.
- **LastByteRcvd**: Sequence number offset of the highest byte received and held in `Recv Buffer`.

```
                        RECEIVE BUFFER STRUCTURE
                        
   [ Read & Removed ] [ Contiguous Bytes Ready ] [ Out-of-Order ] [ Buffer Gap ]
  +------------------+--------------------------+----------------+--------------+
  |                  |                          |                |              |
  +------------------+--------------------------+----------------+--------------+
  ^                  ^                          ^                ^
  Begin              LastByteRead               LastByteRcvd     NextByteExpected
```

Initial State (After Handshake, Before Data):

$$\text{LastByteRead} = X, \quad \text{NextByteExpected} = X + 1, \quad \text{LastByteRcvd} = X$$

![[Pasted image 20261031183633.png]]
---

Module 5: Byte-Stream Sequence Numbering & Payload Rules

TCP is a **byte-stream protocol** (stream-oriented protocol). It treats data as an unstructured, ordered stream of bytes.

1. Core Sequence Numbering Rules

- Every individual byte of application data is assigned a unique **32-bit sequence number**.
- **Segment Header Rule**: The **Sequence Number field in a TCP Segment Header** equals the **sequence number of the FIRST byte** carried in that segment's payload.

```
                         BYTE-STREAM NUMBERING
                         
   Payload Bytes:   [ Byte 501 ] [ Byte 502 ] ... [ Byte 600 ]
                    ^
   TCP Segment Header: Sequence Number = 501
   Payload Length = 100 Bytes
```

2. Maximum Segment Size (MSS)

- **Definition**: The **Maximum Segment Size (MSS)** specifies the maximum number of **data payload bytes** that a TCP segment can carry.
- **Important Constraint**: MSS **does NOT include the TCP Header size**.

$$\text{Max Total TCP Segment Size} = \text{TCP Header Size} + \text{MSS}$$

3. Data Transmission Equations

When TCP transmits a segment from the `Send Buffer`:

1. Fills up to $\text{MSS}$ bytes into the payload: from $(\text{LastByteSent} + 1)$ to $(\text{LastByteSent} + \text{MSS})$.
2. Header fields populated: $$\text{Sequence Number} = \text{LastByteSent} + 1$$ $$\text{ACK Number} = \text{NextByteExpected}$$
3. Update state variable: $$\text{LastByteSent} = \text{LastByteSent} + \text{MSS}$$

![[Pasted image 20261031184220.png]]

---

Module 6: Receiver Buffer Processing Cases

When a TCP receiver receives a segment containing $K$ bytes of payload, it processes the segment under one of five distinct cases depending on sequence alignment:

```
                     RECEIVER SEGMENT PROCESSING
                                  |
    +-----------------------------+-----------------------------+
    |                             |                             |
In-Order Arrival             Out-of-Order                  Duplicate / Old
(Cases I, III, IV)           (Case II)                     (Case V)
```

Case I: In-Order Segment Arrival (No Gaps)

- **Condition**: $(\text{Sequence Number} == \text{NextByteExpected})$ AND $(\text{LastByteRcvd} + 1 == \text{NextByteExpected})$.
- **Action**: Payload bytes stored contiguously in `Recv Buffer`.
- **State Updates**: $$\text{NextByteExpected} = \text{NextByteExpected} + K$$ $$\text{LastByteRcvd} = \text{LastByteRcvd} + K$$
- **ACK Sent**: $\text{ACK Number} = \text{NextByteExpected}$.

---

Case II: Out-of-Order Segment Arrival (Gap Created)

- **Condition**: $\text{Sequence Number} > \text{NextByteExpected}$.
- **Action**: Stores payload bytes in `Recv Buffer` leaving a gap of $(\text{Sequence Number} - \text{NextByteExpected})$ empty bytes.
- **State Updates**: $$\text{LastByteRcvd} = \text{Sequence Number} + K - 1$$ $$\text{NextByteExpected} \text{ remains UNCHANGED}$$
- **ACK Sent**: Sends a **Duplicate ACK** with $\text{ACK Number} = \text{NextByteExpected}$.

---

Case III: Missing Segment Arrives (Partial Gap Fill)

- **Condition**: $(\text{Sequence Number} == \text{NextByteExpected})$ AND $(\text{LastByteRcvd} > \text{NextByteExpected})$ AND $(\text{Missing Bytes} > K)$.
- **Action**: Fills the initial part of the gap.
- **State Updates**: $$\text{NextByteExpected} = \text{NextByteExpected} + K$$ $$\text{LastByteRcvd} \text{ remains UNCHANGED}$$
- **ACK Sent**: $\text{ACK Number} = \text{NextByteExpected}$.

---

Case IV: Missing Segment Arrives (Complete Gap Fill)

- **Condition**: $(\text{Sequence Number} == \text{NextByteExpected})$ AND $(\text{LastByteRcvd} > \text{NextByteExpected})$ AND $(\text{Missing Bytes} == K)$.
- **Action**: Fills the remaining gap completely, making all buffered bytes contiguous up to $\text{LastByteRcvd}$.
- **State Updates**: $$\text{NextByteExpected} = \text{LastByteRcvd} + 1$$
- **ACK Sent**: $\text{ACK Number} = \text{NextByteExpected} = \text{LastByteRcvd} + 1$.

---

Case V: Duplicate Segment Arrival

- **Condition**: $\text{Sequence Number} < \text{NextByteExpected}$.
- **Action**: Ignores/discards duplicate payload.
- **ACK Sent**: Re-sends ACK with $\text{ACK Number} = \text{NextByteExpected}$.
![[Pasted image 20261031184722.png]]

---

Module 7: Sender ACK Processing & Acknowledgment Field Rules

1. Sender Processing of ACKs

When the sender receives an ACK segment with $\text{ACK Number} = A$:

1. Updates ACK state variable: $$\text{LastByteAcked} = A - 1$$
2. **Buffer Cleanup**: Bytes from the start of `Send Buffer` up to $\text{LastByteAcked}$ are freed from memory.

3. Acknowledgment Number Header Field Rules

- **ACK Flag Rule**: The Acknowledgment Number field in the TCP header is valid **only if the ACK flag is set to 1**.
- **SYN** **Segment Exception**: For a pure `SYN` segment (`SYN = 1, ACK = 0`), the Acknowledgment Number field contains a **garbage value**.
- **Default ACK Mechanism**: TCP uses **Cumulative Acknowledgments** by default in the base header ($\text{ACK Number} = \text{NextByteExpected}$).
- **Selective Acknowledgment (SACK)**: TCP supports selective acknowledgments via the **Options field** in the TCP header.

---

Module 8: Step-by-Step Solved Problems & GATE PYQs

Problem 1: Byte-Stream Sequence Allocation [GATE-IT-2004]

- **Problem**: In TCP, a unique sequence number is assigned to each:
    - (A) byte
    - (B) word
    - (C) segment
    - (D) message
- **Step-by-Step Solution**:
    - TCP is a byte-stream protocol. Each individual byte of application data is assigned a unique 32-bit sequence number.
- **Answer**: **Option A (byte)**.

---

Problem 2: Sequence Number Calculation for Sequential Segments

- **Problem**: Consider three TCP segments transmitted one after another. Each TCP segment carries **1000 bytes** of user data in its payload field. The sequence number field value in the first segment header is **13000**. What is the sequence number field value in the third segment header?
- **Step-by-Step Solution**:
    1. **First Segment**:
        - $\text{Sequence Number} = 13000$
        - Payload bytes: $13000 \text{ to } 13999$ (1000 bytes)
    2. **Second Segment**:
        - $\text{Sequence Number} = 13000 + 1000 = 14000$
        - Payload bytes: $14000 \text{ to } 14999$ (1000 bytes)
    3. **Third Segment**:
        - $\text{Sequence Number} = 14000 + 1000 = \mathbf{15000}$
- **Answer**: **15000**.

---

Problem 3: Payload Size Extraction from Sequence Numbers

- **Problem**: Consider a TCP connection where the sender is sending two segments back to back. The sequence numbers of the first and second segments are **5000** and **9000**, respectively. What is the size of data (in bytes) carried by the first segment in its payload field?
- **Step-by-Step Solution**:
    1. First segment starts at sequence number $5000$.
    2. Second segment starts at sequence number $9000$.
    3. The first segment carries bytes from sequence number $5000$ up to $8999$.
    4. Payload size of first segment $= 9000 - 5000 = \mathbf{4000\text{ bytes}}$.
- **Answer**: **4000 bytes**.

---

Problem 4: Server State during Handshake Timeout

- **Problem**: Consider a TCP client and a TCP server running on two different machines. During TCP connection establishment, the TCP client calls `connect` to initiate the connection and a `SYN` segment is sent to the TCP server. Server-side TCP responds by sending a `SYN-ACK` segment. As per the TCP connection state diagram, in which state does the server-side TCP wait for the `ACK` segment from the client-side TCP?
    - (A) `SYN_SENT`
    - (B) `SYN_RECEIVED`
    - (C) `LISTEN`
    - (D) `ESTABLISHED`
- **Step-by-Step Solution**:
    - On receiving `SYN`, the server transitions from `LISTEN` to `SYN_RECEIVED` (or `SYN_RCVD`) and sends `SYN-ACK`. It remains in `SYN_RECEIVED` while waiting for the final `ACK` from the client.
- **Answer**: **Option B (****SYN_RECEIVED****)**.