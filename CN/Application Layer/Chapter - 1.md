Comprehensive Master Notes: Application Layer Architectures, Protocol State & DNS Mechanics (Batch 1)

---

Module 1: Application Layer Architectures & Process Communication

At the Application Layer, software processes running on distinct network hosts communicate with each other using two foundational architectural paradigms.

```
                           TWO COMMUNICATION PARADIGMS
                                        |
         +------------------------------+------------------------------+
         |                                                             |
   CLIENT-SERVER MODEL                                        PEER-TO-PEER (P2P) ARCHITECTURE
   - Always-on dedicated server host                          - No always-on dedicated server
   - Permanent, static IP address                             - Direct communication between arbitrary peers
   - Centralized data center deployment                       - Dynamic, frequently changing peer IP addresses
   - Clients do NOT communicate directly                      - Complex management & distributed lookups
```

1. Client-Server Model

- **Server**: An always-on host with a permanent, static IP address. Serves requests from multiple clients and is typically deployed in data centers for high availability.
- **Client**: Initiates communication with the server. May have a dynamic, temporary IP address assigned by DHCP. Clients **do not communicate directly** with each other.
- **Examples**: Web browsing (Client = Browser, Server = Web Server), Email (Client = User Agent, Server = Mail Server).

2. Peer-to-Peer (P2P) Architecture

- **Peers**: Arbitrary end-user systems communicate directly with each other without going through a central server.
- **IP Mobility**: Peers join and leave dynamically, continually changing IP addresses.
- **Examples**: P2P File Sharing (BitTorrent), Voice over IP (VoIP).

---

Module 2: Stateful vs. Stateless Protocols

Application layer protocols are classified based on whether the server maintains interaction memory across user requests.

```
+-----------------------------------------------------------------------------------------------+
|                                  STATEFUL VS. STATELESS PROTOCOLS                             |
+-------------------+------------------------------------+--------------------------------------+
| Property          | Stateless Protocols                | Stateful Protocols                   |
+-------------------+------------------------------------+--------------------------------------+
| Server Memory     | Maintains NO past client history   | Maintains active session state/history|
| Request Handling  | Every request treated as brand new | Requests interpreted using past state|
| Complexity        | Low server complexity & overhead   | Higher complexity; requires storage  |
| Fault Tolerance   | High (Server crash loses no state) | Low (Server crash breaks sessions)   |
| Examples          | DNS, HTTP, SNMP                    | DHCP, IMAP, FTP                      |
+-------------------+------------------------------------+--------------------------------------+
```

Special Protocol Classifications:

- **SMTP & POP3**: Default behavior is **stateful within a session**, but **stateless across multiple distinct sessions**.
- **Transport Layer Statefulness**:
    - **UDP**: Stateless transport protocol.
    - **TCP**: Stateful transport protocol (maintains sequence numbers, buffers, timers, and connection states).

---

Module 3: Domain Name System (DNS) Architecture & Hierarchy

The **Domain Name System (DNS)** is a stateless, client-server application layer protocol used to translate human-readable **Domain Names / Hostnames** (e.g., `www.amazon.com`) into 32-bit **IPv4 Addresses** (e.g., `192.0.2.1`), and vice versa.

```
                                  DNS SERVER HIERARCHY
                                           |
                                  +-----------------+
                                  | Root Name Server|
                                  +-----------------+
                                     /     |     \
                                    /      |      \
                        +--------------+ +--------------+ +--------------+
                        | .com TLD NS  | | .org TLD NS  | |  .in TLD NS  |
                        +--------------+ +--------------+ +--------------+
                               |                                |
                        +--------------+                 +--------------+
                        | [suspicious link removed]   |                 | gate.org.in  |
                        | Authoritative|                 | Authoritative|
                        +--------------+                 +--------------+
```

1. Three Tiers of the Official DNS Hierarchy

2. **Root Name Servers**:
    - The entry point of contact for any DNS lookup query entering the global hierarchy.
    - Managed globally by ICANN. There are **13 logical root name servers** worldwide (labeled A through M), each heavily replicated across hundreds of physical locations using Anycast routing.
3. **Top-Level Domain (TLD) Name Servers**:
    - Responsible for top-level domain extensions like `.com`, `.org`, `.net`, `.edu`, as well as country-code TLDs like `.in`, `.uk`, `.de`.
4. **Authoritative Name Servers**:
    - Maintained by domain owners or their hosting providers.
    - Holds the definitive, official mapping of hostnames to IP addresses for that organization's domains (e.g., `amazon.com`, `iitm.ac.in`).

5. Local DNS Name Servers (ISP Resolver)

- Every ISP or local network operates a **Local DNS Name Server** (Default Name Server).
- **Hierarchy Rule**: The Local DNS server **does NOT belong to the official DNS hierarchy**.
- Acts as a proxy intermediary: when a host issues a DNS query, it is forwarded directly to its Local DNS server. If the mapping is cached locally, it responds immediately; otherwise, it queries the DNS hierarchy on behalf of the client.

---

Module 4: Name Resolution Modes, Caching & Message Structure

1. Iterated (Iterative) vs. Recursive Queries

```
                      ITERATIVE QUERY vs. RECURSIVE QUERY
                      
    ITERATIVE QUERY: Contacted server returns "Address of NEXT server to ask"
    DNS Resolver  -------- (1) Query [suspicious link removed] --------> Root Name Server
                  <------- (2) Reply: Ask .com TLD --------
                  -------- (3) Query [suspicious link removed] --------> .com TLD Server
                  <------- (4) Reply: Ask [suspicious link removed] ------
                  -------- (5) Query [suspicious link removed] --------> [suspicious link removed] Authoritative
                  <------- (6) Reply: Web Server IP ---------

    RECURSIVE QUERY: Contacted server assumes full burden of chain-querying
    DNS Resolver ---> Root NS ---> .com TLD NS ---> Authoritative NS
                                                                  |
    DNS Resolver <--- Root NS <--- .com TLD NS <------------------+
```

- **Iterated Query**: The contacted server replies with the address of the next lower-level server to contact. The client's Local Resolver makes sequential calls step-by-step.
- **Recursive Query**: The contacted server assumes full responsibility for obtaining the final IP mapping by querying down the chain recursively.
- **DNS Caching & TTL**:
    - Once a DNS server learns a mapping, it stores it in its **DNS Cache** to serve future requests instantly.
    - Cached records automatically expire when their **Time-To-Live (TTL)** counter reaches zero.

2. DNS Resource Records (RR) Format

DNS database entries are formatted as a 4-tuple: **(Name, Value, Type, TTL)**.

```
+-----------+-------------------+---------------------------------------------------+
| RR Type   | Name Field        | Value Field                                       |
+-----------+-------------------+---------------------------------------------------+
| Type A    | Host Name         | IPv4 Address of the host                          |
| Type NS   | Domain Name       | Hostname/IP of Authoritative Name Server for domain|
| Type CNAME| Alias Host Name   | Canonical (real/official) Host Name               |
| Type MX   | Domain Name       | Hostname of associated SMTP Mail Server           |
+-----------+-------------------+---------------------------------------------------+
```

3. DNS Transport Layer Usage (UDP vs. TCP)

- **UDP Port 53 (Default)**: Used for standard DNS queries and replies whenever the message size is $\le 512 \text{ Bytes}$.
- **TCP Port 53 (Conditional)**: Used when DNS responses exceed **512 Bytes** or when performing **Zone Transfers** (replicating databases between primary and secondary name servers) where reliable transmission is required.

---

Module 5: Step-by-Step Solved Problems & GATE PYQs

Problem 1: Client-Server Application Categorization [GATE-2010, 1-Mark]

- **Question**: Which one of the following is NOT a client-server application?
    - (A) Internet chat
    - (B) Web browsing
    - (C) E-mail
    - (D) Ping
- **Step-by-Step Solution**:
    1. Web browsing uses the HTTP client-server model.
    2. E-mail uses SMTP/POP3/IMAP client-server models.
    3. Internet chat operates on a client-server model by default.
    4. **Ping** utilizes Network Layer ICMP Echo Request and Echo Reply messages directly between hosts without operating as an Application Layer client-server application.
- **Answer**: **Option D (Ping)**.

---

Problem 2: Maximum Application Layer Data Size [GATE-CS-2008, 1-Mark]

- **Question**: What is the maximum size of data that the application layer can pass on to the TCP layer below?
    - (A) Any Size
    - (B) $2^{16} \text{ bytes} - \text{size of TCP Header}$
    - (C) $2^{16} \text{ bytes}$
    - (D) 1500 bytes
- **Step-by-Step Solution**:
    1. The Application Layer is not constrained by fixed packet payload limits under the OSI / TCP-IP reference model; it can pass data of **any arbitrary size** down to the transport layer.
    2. The Transport Layer takes responsibility for slicing this data into appropriate segments (Segmentation) based on the MSS.
- **Answer**: **Option A (Any Size)**.

---

Problem 3: Stateful Protocol Identification [GATE-2016, Set-1, 1-Mark]

- **Question**: Which of the following is/are example(s) of stateful application layer protocols?
    - (i) HTTP
    - (ii) FTP
    - (iii) TCP
    - (iv) POP3
    - **Options**: (A) (i) and (ii) only, (B) (ii) and (iii) only, (C) (ii) and (iv) only, (D) (iv) only.
- **Step-by-Step Solution**:
    1. **HTTP**: Stateless application layer protocol.
    2. **FTP**: Stateful application layer protocol (maintains control connection state).
    3. **TCP**: Stateful protocol, BUT it operates at the **Transport Layer**, not the Application Layer.
    4. **POP3**: Stateful application layer protocol (maintains state within a session by default).
    5. Filtering for stateful _application layer_ protocols yields (ii) and (iv).
- **Answer**: **Option C ((ii) and (iv) only)**.

---

Problem 4: Iterative DNS Resolution RTT Calculation

- **Question**: Consider the resolution of domain name `www.amazon.com` by a DNS resolver using the iterative query mechanism. Assume no caching exists anywhere. Calculate:
    1. Total DNS query-response pairs involved.
    2. Total RTTs required.
- **Step-by-Step Solution**:
    1. **Query-Response Pair 1**: Resolver $\to$ Root Name Server $\to$ Returns `.com` TLD NS IP.
    2. **Query-Response Pair 2**: Resolver $\to$ `.com` TLD Name Server $\to$ Returns `amazon.com` Authoritative NS IP.
    3. **Query-Response Pair 3**: Resolver $\to$ `amazon.com` Authoritative Name Server $\to$ Returns IP of Web Server.
- **Answer**: **3 Query-Response Pairs, 3 RTTs**.

---

Problem 5: Four-Tier Iterative DNS Resolution [GATE-2022, 1-Mark]

- **Question**: Consider the resolution of the domain name `www.gate.org.in` by a DNS resolver. Assume that no resource records are cached anywhere across the DNS servers and that the iterative query mechanism is used in the resolution. The number of DNS query-response pairs involved in completely resolving the domain name is ________.
- **Step-by-Step Solution**:
    1. Trace the 4-tier domain hierarchy for `www.gate.org.in`:
        - **Pair 1**: Resolver queries **Root Name Server** $\to$ Receives IP of `.in` TLD Name Server.
        - **Pair 2**: Resolver queries **.in** **TLD Name Server** $\to$ Receives IP of `.org.in` 2nd-level TLD Name Server.
        - **Pair 3**: Resolver queries **.org.in** **2nd-level TLD Name Server** $\to$ Receives IP of `gate.org.in` Authoritative Name Server.
        - **Pair 4**: Resolver queries **gate.org.in** **Authoritative Name Server** $\to$ Receives IP address of the target host.
    2. Total query-response pairs $= 1 + 1 + 1 + 1 = \mathbf{4}$.
- **Answer**: **4**.

