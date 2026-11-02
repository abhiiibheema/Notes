Comprehensive Master Notes: Transport Layer 05, 06 & 07 (TCP Connection Termination, State Diagrams, Socket API & Timing Constraints)

---

Module 1: TCP Connection Termination Mechanics (The 4-Way Handshake)

Transmission Control Protocol (TCP) supports full-duplex connection teardown. Either peer (client or server) can initiate the connection close sequence independently using the **FIN (Finish)** control flag.

```
     TCP PEER A (Initiator)                                          TCP PEER B (Acceptor)
  [ ESTABLISHED State ]                                          [ ESTABLISHED State ]
         |                                                               |
         | ---- Message 1: FIN + ACK (Seq = X, ACK = Y, With/No Payload) -> |
  [ FIN_WAIT_1 State ]                                            [ CLOSE_WAIT State ]
         |                                                               |
         | <--- Message 2: ACK (Seq = Y, ACK = X + 1, With/No Payload) -- |
  [ FIN_WAIT_2 State ]                                                  |
         |                                                 (Application Data Cleanup)
         |                                                               |
         | <--- Message 3: FIN + ACK (Seq = Z, ACK = X + 1, With/No Payload)
  [ TIME_WAIT State ]                                             [ LAST_ACK State ]
         |                                                               |
         | ---- Message 4: ACK (Seq = X + 1, ACK = Z + 1, No Payload) -> |
  [ Wait 2 * MSL ]                                               [ CLOSED State ]
         |                                                               
  [ CLOSED State ]                                                       
```

1. Detailed 4-Way Handshake Packet Sequence

2. **Message 1 (****FIN** **Segment from Peer A)**:
    - **Control Flags**: `FIN = 1`, `ACK = 1`.
    - **Sequence Number**: `Seq = X` (where $X$ is the current sequence position).
    - **Payload**: May carry final payload bytes or 0 payload bytes.
    - **Sequence Space Consumption**: The `FIN` flag **consumes exactly 1 sequence number space** in the stream.
3. **Message 2 (****ACK** **Segment from Peer B)**:
    - **Control Flags**: `FIN = 0`, `ACK = 1`.
    - **Acknowledgment Number**: `ACK = X + 1` (Acknowledges Peer A's `FIN`).
    - **Meaning**: Peer B acknowledges Peer A's request to close. Peer A cannot send any more application data, but Peer B can still transmit pending data to Peer A (Half-Closed state).
4. **Message 3 (****FIN** **Segment from Peer B)**:
    - **Control Flags**: `FIN = 1`, `ACK = 1`.
    - **Sequence Number**: `Seq = Z` (Server's sequence position after finishing data flush).
    - **Acknowledgment Number**: `ACK = X + 1`.
    - **Meaning**: Peer B completes its remaining transfers and notifies Peer A that its sending direction is also closed.
5. **Message 4 (****ACK** **Segment from Peer A)**:
    - **Control Flags**: `FIN = 0`, `ACK = 1`.
    - **Sequence Number**: `Seq = X + 1`.
    - **Acknowledgment Number**: `ACK = Z + 1` (Acknowledges Peer B's `FIN`).
    - **Payload**: Strictly **0 payload bytes**.

---

2. Connection Close Special Variants

- **3-Way Termination Optimization**: If Peer B has no pending data to send when Peer A's `FIN` arrives, Peer B can combine Message 2 and Message 3 into a single `FIN + ACK` segment (`FIN = 1, ACK = 1`), reducing the termination procedure to 3 steps.
- **Abrupt Termination via Reset (****RST** **Flag)**:
    - A host can forcefully terminate a TCP connection immediately by sending a segment with the **RST = 1** flag.
    - **No Handshake**: On receiving an `RST` segment, the receiver immediately deletes the connection socket and frees resources without performing any multi-step handshake.

---

Module 2: Complete TCP Finite State Machine (State Transition Diagram)

```
                               +-------------------+
                               |      CLOSED       |
                               +-------------------+
                                 /               \
                 Passive Open   /                 \  Active Open
                 (listen())    /                   \ (connect())
                              v                     v
                      +--------------+        +--------------+
                      |    LISTEN    |        |   SYN_SENT   |
                      +--------------+        +--------------+
                             |                       |
                  Recv SYN   |                       | Recv SYN + ACK
                  Send SYN+ACK                      | Send ACK
                             v                       v
                      +--------------+        +--------------+
                      |   SYN_RCVD   |        | ESTABLISHED  |<---+
                      +--------------+        +--------------+    |
                             |                       |            | Data
                  Recv ACK   |            Active Close| (close())  | Transfer
                             v                       v            |
                      +--------------+        +--------------+----+
                      | ESTABLISHED  |        |  FIN_WAIT_1  |
                      +--------------+        +--------------+
                                              /       |       \
                                Recv ACK     /        |        \ Recv FIN+ACK
                                            v         |         v Send ACK
                                     +------------+   |   +------------+
                                     | FIN_WAIT_2 |   |   |   CLOSING  |
                                     +------------+   |   +------------+
                                            |         |         |
                                   Recv FIN |         | Recv FIN| Recv ACK
                                   Send ACK |         | Send ACK|
                                            v         v         v
                                     +---------------------------------+
                                     |            TIME_WAIT            |
                                     +---------------------------------+
                                                     |
                                                     | 2 * MSL Timeout
                                                     v
                                             +---------------+
                                             |    CLOSED     |
                                             +---------------+
```

1. Detailed State Definitions

- **CLOSED**: Synthetic starting and ending point; no active or pending connection handles exist.
- **LISTEN**: Server-side state waiting for incoming connection requests (`SYN`).
- **SYN_SENT**: Client state after sending `SYN`, waiting for matching `SYN-ACK`.
- **SYN_RCVD**: Server state after receiving `SYN` and sending `SYN-ACK`, waiting for final connection `ACK`.
- **ESTABLISHED**: Active full-duplex data transfer phase.
- **FIN_WAIT_1**: Initiator state after sending initial `FIN`, waiting for peer's `ACK` or `FIN`.
- **CLOSE_WAIT**: Acceptor state after receiving peer's `FIN` and sending `ACK`, waiting for local application close.
- **FIN_WAIT_2**: Initiator state after receiving `ACK` for its `FIN`, waiting for peer's incoming `FIN`.
- **LAST_ACK**: Acceptor state after sending its own `FIN`, waiting for the final confirming `ACK`.
- **CLOSING**: Transmit state when both endpoints initiate `close()` simultaneously (Simultaneous Close).
- **TIME_WAIT**: Active close initiator state waiting for $2 \times \text{MSL}$ before returning to `CLOSED`.

---

2. The Purpose of the `TIME_WAIT` State (2 * MSL Timer)

The endpoint that initiates the active close **MUST** remain in the `TIME_WAIT` state for a period equal to $2 \times \text{MSL}$ (where MSL = Maximum Segment Lifetime) due to two crucial reasons:

1. **Reliable Final ACK Delivery**: If the final `ACK` (Message 4) is lost in transit, Peer B will time out and retransmit its `FIN` (Message 3). The host in `TIME_WAIT` must remain active to resend the `ACK` and allow Peer B to close gracefully.
2. **Old Segment Drainage**: Ensures that any delayed duplicate segments from the current connection expire and disappear from intermediate routers before a new socket reuses the exact same IP/Port numbers.

![[Pasted image 20261031193601.png]]

---

Module 3: Unix Socket API System Calls & Call Ordering

The Socket Application Programming Interface (API) provides library system calls for process communication over TCP (`SOCK_STREAM`) and UDP (`SOCK_DGRAM`).

```
                    SOCKET API CALL SEQUENCE & HANDSHAKE ALIGNMENT
                    
      TCP CLIENT PROCESS                                    TCP SERVER PROCESS
  +------------------------+                            +------------------------+
  |  client_fd = socket()  |                            |  server_fd = socket()  |
  +------------------------+                            +------------------------+
              |                                                     |
  (Optional bind())                                     +------------------------+
              |                                         |        bind()          |
              |                                         +------------------------+
              |                                                     |
              |                                         +------------------------+
              |                                         |        listen()        |
              |                                         +------------------------+
              |                                                     |
              | ---------- 1. SYN (connect()) --------------------> | (Handshake handled by OS)
              | <--------- 2. SYN + ACK --------------------------- | (Placed in Accept Queue)
  +------------------------+                                        |
  |  connect() returns 0   |                            +------------------------+
  +------------------------+                            | new_fd = accept()      |
              |                                         +------------------------+
              | <--------- 3. ACK --------------------------------- |
  [ ESTABLISHED State ]                                 [ ESTABLISHED State ]
              |                                                     |
  +------------------------+                            +------------------------+
  |  write() / send()      | ======= Data Bytes ======> |  read() / recv()       |
  +------------------------+                            +------------------------+
              |                                                     |
  +------------------------+                            +------------------------+
  |  close()               | ---------- 4. FIN -------> |  read() returns 0      |
  +------------------------+                            +------------------------+
                                                        |  close()               |
                                                        +------------------------+
```

---

1. Server-Side System Call Sequence

2. **socket()**: $$\text{int sockfd} = \text{socket}(\text{AF\_INET}, \text{SOCK\_STREAM}, 0)$$
    - Allocates an active socket file descriptor in the OS kernel.
3. **bind()**: $$\text{int bind}(\text{int sockfd}, \text{const struct sockaddr } *\text{addr}, \text{socklen\_t addrlen})$$
    - Binds the server's local IP address (`INADDR_ANY`) and port number (`sin_port`) to `sockfd`.
4. **listen()**: $$\text{int listen}(\text{int sockfd}, \text{int backlog})$$
    - **Converts an unconnected active socket into a passive listening socket** ("Welcome Socket").
    - `backlog`: Specifies the maximum capacity of the queue for pending completed connections (`Accept Queue`).
5. **accept()**: $$\text{int new\_sockfd} = \text{accept}(\text{int sockfd}, \text{struct sockaddr } *\text{addr}, \text{socklen\_t } *\text{addrlen})$$
    - Blocks until a connection is available in the `Accept Queue`.
    - **Returns a NEW file descriptor (****new_sockfd****)** corresponding to the connected socket for data transfer, keeping `sockfd` open for new incoming connection requests.
6. **read()** **/** **recv()** **&** **write()** **/** **send()**: Transfers payload data through `new_sockfd`.
7. **close()**: Initiates connection termination and releases socket resources.
8. **Mandatory Server Invocation Order**: $$\textbf{socket()} \longrightarrow \textbf{bind()} \longrightarrow \textbf{listen()} \longrightarrow \textbf{accept()} \longrightarrow \textbf{recv() / read()}$$

---

2. Client-Side System Call Sequence

3. **socket()**: Creates client stream socket descriptor (`client_sockfd`).
4. **connect()**: $$\text{int result} = \text{connect}(\text{int sockfd}, \text{const struct sockaddr } *\text{serv\_addr}, \text{socklen\_t addrlen})$$
    - Triggers the kernel to send the **SYN** **packet** and initiate the TCP 3-Way Handshake.
    - **Returns 0 on success** (when `SYN-ACK` is received); returns **-1 on error/timeout**.
    - _Note_: `bind()` is optional on the client side; if omitted, the OS automatically assigns an ephemeral port number during `connect()`.
5. **write()** **/** **send()** **&** **read()** **/** **recv()**: Data exchange phase.
6. **close()**: Sends `FIN` segment to terminate the connection.

![[Pasted image 20261031194922.png]]

---

Module 4: Maximum Segment Lifetime (MSL) & Sequence Wrap-Around Time

1. Maximum Segment Lifetime (MSL)

- **Definition**: The maximum bounded duration that an IP datagram / TCP segment can survive within an internetwork before being discarded by routers.
- **Standard MSL Values**: Historically set to **120 seconds**, **60 seconds**, or **30 seconds** depending on implementation.

2. Sequence Number Wrap-Around Time

Because TCP uses a finite **32-bit sequence number field**, after transmitting $2^{32}$ bytes, sequence numbers overflow and wrap around to zero.

```
                         SEQUENCE WRAP-AROUND
                         
  Sequence 0 ------> Sequence 2^32 - 1 ----(Wrap-Around)----> Sequence 0
  |                                                          |
  +------------------- Total Capacity = 2^32 Bytes ----------+
```

- **Wrap-Around Time Formula**: The minimum time required to consume the full sequence space at a given transmission bandwidth: $$\text{Wrap-Around Time} = \frac{2^k \text{ Bytes}}{\text{Bandwidth (in Bytes/sec)}}$$ For standard TCP with a 32-bit sequence field ($k = 32$): $$\text{Wrap-Around Time} = \frac{2^{32} \text{ Bytes}}{\text{Bandwidth (in Bytes/sec)}}$$

---

3. Safety Condition Against Sequence Collision (Aliasing)

To prevent active segments from being confused with old, delayed segments in the network, the sequence wrap-around time must not be shorter than the maximum segment lifetime:

$$\text{MSL} \le \text{Minimum Wrap-Around Time}$$

Minimum Sequence Number Field Bits Formula:

To safely operate at a given bandwidth and MSL without collision, the minimum sequence field size $k$ bits is: $$2^k = \text{MSL} \times \text{Bandwidth (in Bytes/sec)}$$ $$k = \lceil \log_2(\text{MSL} \times \text{Bandwidth in Bytes/sec}) \rceil \text{ bits}$$
![[Pasted image 20261031195610.png]]
---

Module 5: Step-by-Step Solved Problems & GATE PYQs

Problem 1: Client State during Connection Close [GATE-2017, Set-1]

- **Problem**: Consider a TCP client and a TCP server running on two different machines. After completing data transfer, the TCP client calls `close` to terminate the connection and a `FIN` segment is sent to the TCP server. Server-side TCP responds by sending an `ACK` which is received by the client-side TCP. As per the TCP connection state diagram (RFC 793), in which state does the client-side TCP connection wait for the `FIN` from the server-side TCP?
    - (A) `LAST-ACK`
    - (B) `TIME-WAIT`
    - (C) `FIN-WAIT-1`
    - (D) `FIN-WAIT-2`
- **Step-by-Step Solution**:
    1. Client sends `FIN` $\implies$ Transitions from `ESTABLISHED` to `FIN_WAIT_1`.
    2. Client receives `ACK` for its `FIN` $\implies$ Transitions from `FIN_WAIT_1` to `FIN_WAIT_2`.
    3. In `FIN_WAIT_2`, the client waits for the incoming `FIN` from the server.
- **Answer**: **Option D (****FIN-WAIT-2****)**.

---

Problem 2: Simultaneous Connection Close [GATE-2026, Set-1]

- **Problem**: With respect to a TCP connection between a client and a server, which one of the following statements is true?
    - (A) The client and server use a two-way handshake mechanism before the start of data transmission.
    - (B) The TCP connection is half-duplex.
    - (C) The server cannot initiate closing of the connection before the client initiates closing of the connection.
    - (D) The client and server can initiate closing of the connection at the same time.
- **Step-by-Step Solution**:
    - Statement A is False (uses 3-way handshake).
    - Statement B is False (TCP is full-duplex).
    - Statement C is False (either peer can close first).
    - Statement D is True: TCP supports **Simultaneous Connection Close**, where both peers send `FIN` segments concurrently and transition through `CLOSING` $\to$ `TIME_WAIT`.
- **Answer**: **Option D**.

---

Problem 3: System Call Triggering SYN Packet [GATE-CS-2008]

- **Problem**: Which of the following system calls results in the sending of `SYN` packets?
    - (A) `Socket`
    - (B) `Bind`
    - (C) `Listen`
    - (D) `Connect`
- **Step-by-Step Solution**:
    - The `connect()` system call executed by the client initiates the 3-Way Handshake by transmitting a `SYN` segment to the server.
- **Answer**: **Option D (****Connect****)**.

---

Problem 4: Passive Socket Conversion System Call [GATE-2014, Set-2]

- **Problem**: Which one of the following socket API functions converts an unconnected active TCP socket into a passive socket?
    - (A) `Connect`
    - (B) `Bind`
    - (C) `Listen`
    - (D) `Accept`
- **Step-by-Step Solution**:
    - The `listen()` system call converts an active stream socket into a passive listening socket ("Welcome Socket") that accepts incoming connection requests.
- **Answer**: **Option C (****Listen****)**.

---

Problem 5: UNIX Socket Server Order [GATE-2015, Set-2]

- **Problem**: Identify the correct order in which a server process must invoke the function calls `accept`, `bind`, `listen`, and `recv` according to UNIX socket API.
    - (A) `listen, accept, bind, recv`
    - (B) `bind, listen, accept, recv`
    - (C) `bind, accept, listen, recv`
    - (D) `accept, listen, bind, recv`
- **Step-by-Step Solution**:
    - The mandatory server sequence is: `bind()` (assign IP/Port) $\to$ `listen()` (enable welcome queue) $\to$ `accept()` (obtain connected socket) $\to$ `recv()` (read incoming data).
- **Answer**: **Option B (****bind, listen, accept, recv****)**.

---

Problem 6: Server Preemption & Unexecuted Accept Call [GATE-CS-2008]

- **Problem**: A client process P needs to make a TCP connection to a server process S. Consider the following situation: the server process S executes a `socket()`, a `bind()`, and a `listen()` system call in that order, following which it is preempted. Subsequently, the client process P executes a `socket()` system call followed by `connect()` system call to connect to the server process S. The server process has not executed any `accept()` system call. Which one of the following events could take place?
    - (A) `connect()` system call returns successfully
    - (B) `connect()` system call blocks
    - (C) `connect()` system call returns an error
    - (D) `connect()` system call results in a core dump
- **Step-by-Step Solution**:
    1. The TCP 3-Way Handshake is handled entirely by the operating system kernel once the socket enters the `LISTEN` state via `listen()`.
    2. When client P calls `connect()`, the kernel sends `SYN`, receives `SYN-ACK` from the server kernel, and sends the final `ACK`.
    3. The completed connection is placed into the server's `Accept Queue`.
    4. Client `connect()` returns successfully (0) as soon as `SYN-ACK` is acknowledged, regardless of whether the server application has called `accept()` yet.
- **Answer**: **Option A (****connect() system call returns successfully****)**.

---

Problem 7: Sequence Wrap-Around Time Calculation [GATE Numerical]

- **Problem**: Consider a TCP connection where an end-to-end estimated bandwidth is **4 Gbps**. What is the minimum time (in seconds, rounded to two decimal places) when the sequence numbers wrap around?
- **Step-by-Step Solution**:
    1. **Sequence Space Capacity**: $$2^{32} \text{ Bytes} = 4,294,967,296 \text{ Bytes}$$
    2. **Bandwidth in Bytes per Second**: $$\text{Bandwidth} = 4 \text{ Gbps} = \frac{4 \times 10^9 \text{ bits/sec}}{8} = 0.5 \times 10^9 \text{ Bytes/sec} = 500,000,000 \text{ Bytes/sec}$$
    3. **Wrap-Around Time**: $$\text{Wrap-Around Time} = \frac{2^{32} \text{ Bytes}}{\text{Bandwidth in Bytes/sec}} = \frac{4,294,967,296}{500,000,000} \approx \mathbf{8.59 \text{ seconds}}$$
- **Answer**: **8.59 seconds** (Acceptable Range: **8 to 9 seconds**).