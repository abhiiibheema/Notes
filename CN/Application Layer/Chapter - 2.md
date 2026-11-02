Comprehensive Master Notes: Hypertext Transfer Protocol (HTTP) & Connection Performance (Batch 2)

---

Module 1: HTTP Architecture & Protocol Fundamentals

The **Hypertext Transfer Protocol (HTTP)** is an application-layer protocol that powers data transfer on the World Wide Web using a stateless, client-server model.

```
                            WEB PAGE COMPOSITION & CLIENT-SERVER FLOW
                            
   Browser (HTTP Client)                                     Web Server (HTTP Server)
         |                                                              |
         | --- (1) TCP Connection Setup on Port 80 (3-Way Handshake) --> |
         |                                                              |
         | --- (2) HTTP GET Request Message (Human-readable ASCII) ---> |
         | <--- (3) HTTP Response Message + Requested Web Object ------ |
         |                                                              |
   Rendering Process
   Parses Base HTML File & Identifies Referenced URL Objects (Images, CSS, JS)
```

1. Core Properties of HTTP

- **Transport Protocol**: Uses **TCP on Port 80** (default well-known port) for reliable, ordered delivery.
- **Stateless Protocol**: The server retains no information or state history regarding past client requests. Every request is processed independently as a new transaction.
- **ASCII Text Format**: HTTP messages (requests and responses) are encoded in human-readable ASCII text.

2. Web Page Structure

- A web page consists of a **Base HTML File** together with multiple **Referenced Objects** (such as PNG/JPEG images, CSS stylesheets, and JavaScript files).
- Each referenced object is uniquely addressed by a **Uniform Resource Locator (URL)**.

3. HTTP Request & Response Messages

```
+-----------------------------------------------------------------------------------------------+
|                                    HTTP MESSAGE STRUCTURES                                    |
+-------------------+------------------------------------+--------------------------------------+
| Field             | HTTP Request Message (Client -> Server)| HTTP Response Message (Server -> Client)|
+-------------------+------------------------------------+--------------------------------------+
| Initial Line      | Request Line (`Method`, `URL`, `Ver`)| Status Line (`Ver`, `Status Code`, `Phrase`)|
| Header Lines      | Host, User-Agent, Accept, etc.     | Date, Server, Content-Length, etc.   |
| Entity Body       | Form data (POST/PUT payload)       | Requested Data Object (HTML/Image)   |
+-------------------+------------------------------------+--------------------------------------+
```

- **HTTP Methods**:
    - `GET`: Requests an object specified by the URL. Parameters are embedded directly within the URL string.
    - `POST`: Transmits user input (e.g., web form submission) inside the **Entity Body** of the request message.
    - `PUT`: Uploads a new object or completely replaces an existing file at the specified URL with the entity body content.
    - `HEAD`: Requests only the HTTP response headers (omits the object body) for debugging or metadata validation.
- **HTTP Status Code Classes**:
    - `1xx` **Informational**: Request received, continuing process.
    - `2xx` **Success**: Action successfully received and accepted (e.g., `200 OK`).
    - `3xx` **Redirection**: Further action required to complete request (e.g., `301 Moved Permanently`).
    - `4xx` **Client Error**: Request contains bad syntax or cannot be fulfilled (e.g., `400 Bad Request`, `404 Not Found`).
    - `5xx` **Server Error**: Server failed to fulfill an apparently valid request (e.g., `505 HTTP Version Not Supported`).

---

Module 2: Non-Persistent HTTP (HTTP/1.0) & RTT Calculations

In **Non-Persistent HTTP (HTTP/1.0)**, a separate TCP connection is opened to fetch **at most one object**, and is immediately closed once the object is transmitted.

```
                     NON-PERSISTENT HTTP TIME-LINE (PER OBJECT)
                     
   Client                                                      Server
     | --- SYN Segment ----------------------------------------> | \ 
     | <-- SYN-ACK Segment ------------------------------------ |  |-- 1st RTT (TCP Handshake)
     |                                                           | /
     | --- ACK + HTTP GET Request Message ---------------------> | \
     | <-- HTTP Response Message + Data Object ----------------- |  |-- 2nd RTT (HTTP Exchange)
     |     [Connection Closed / FIN]                             | /
     | <=======================================================> |
     |                     Object Transmission Time              |
```

1. Response Time Formula per Object

To fetch a single web object over non-persistent HTTP: $$\text{Response Time per Object} = 2 \times \text{RTT} + \text{Object Transmission Time}$$

Where:

- $\text{1st RTT}$: Establishing the TCP connection (3-way handshake: `SYN` $\to$ `SYN-ACK` $\to$ `ACK`).
- $\text{2nd RTT}$: Sending the HTTP `GET` request and receiving the first bytes of the HTTP response message.
- **Transmission Time**: For very small objects, object transmission time is practically negligible, making the response time approximately $2 \times \text{RTT}$.

2. Serial vs. Parallel Non-Persistent Transfers

If a webpage contains 1 Base HTML file and $N$ referenced objects:

- **Sequential (No Parallel Connections)**: $$\text{Total RTTs} = 2 \times \text{RTT (Base HTML)} + N \times (2 \times \text{RTT}) = 2(N + 1) \times \text{RTT}$$
- **With Parallel Connections (Max** $P$ **Parallel Channels)**: $$\text{Total RTTs} = 2 \times \text{RTT (Base HTML)} + \left\lceil \frac{N}{P} \right\rceil \times (2 \times \text{RTT})$$

---

Module 3: Persistent HTTP (HTTP/1.1) — Without vs. With Pipelining

In **Persistent HTTP (HTTP/1.1)**, the server leaves the TCP connection open after sending a response, allowing subsequent objects located on the **same server** to be transferred over the **same single TCP connection**.

```
                   PERSISTENT HTTP: WITHOUT vs. WITH PIPELINING
                   
   WITHOUT PIPELINING (Sequential / 1 RTT per Object)
   Client                                                      Server
     | === TCP Handshake (1 RTT) ==============================> |
     | --- HTTP GET (Base HTML) -------------------------------> |  (1 RTT for Base HTML)
     | <-- Base HTML Response --------------------------------- |
     | --- HTTP GET (Object 1) -------------------------------> |  (1 RTT for Object 1)
     | <-- Object 1 Response ---------------------------------- |
     | --- HTTP GET (Object 2) -------------------------------> |  (1 RTT for Object 2)
     | <-- Object 2 Response ---------------------------------- |

   WITH PIPELINING (Back-to-Back Requests / 1 RTT Total for All Objects)
   Client                                                      Server
     | === TCP Handshake (1 RTT) ==============================> |
     | --- HTTP GET (Base HTML) -------------------------------> |  (1 RTT for Base HTML)
     | <-- Base HTML Response --------------------------------- |
     | --- HTTP GET (Obj 1) + GET (Obj 2) + GET (Obj 3) ------> |  (1 RTT for ALL
     | <-- Obj 1 + Obj 2 + Obj 3 Responses -------------------- |   pipelined objects!)
```

1. Persistent HTTP WITHOUT Pipelining

- The client issues a new request only **after** receiving the response for the previous object.
- **Cost**: Requires **1 RTT for the initial TCP connection setup**, **1 RTT for the Base HTML file**, plus **1 RTT for each referenced object** sequentially: $$\text{Total RTTs} = 1 \text{ (TCP)} + 1 \text{ (Base HTML)} + N \times 1 = (N + 2) \times \text{RTT}$$

2. Persistent HTTP WITH Pipelining

- The client transmits requests for all referenced objects **back-to-back in a pipeline** as soon as it encounters their URLs, without waiting for intermediate responses.
- **Cost**: Requires **1 RTT for TCP setup**, **1 RTT for Base HTML**, and **1 RTT total** to receive all referenced small objects in parallel: $$\text{Total RTTs} = 1 \text{ (TCP)} + 1 \text{ (Base HTML)} + 1 \text{ (Pipelined Objects)} = \mathbf{3 \times \text{RTT}}$$

---

Module 4: Web Browsing Packet Traversal Chronology

When a user enters a domain name (e.g., `www.example.com`) in a fresh browser with empty caches:

```
  Step 1: Domain Resolution          Step 2: TCP Connection            Step 3: Object Request
 +-------------------------+        +------------------------+        +------------------------+
 | Outbound:               |        | Outbound:              |        | Outbound:              |
 | DNS Query Message       | ====>  | TCP SYN Segment        | ====>  | HTTP GET Request       |
 | Inbound:                |        | Inbound:               |        | Inbound:               |
 | DNS Response (Server IP)|        | TCP SYN-ACK Segment    |        | HTTP Response (HTML)   |
 +-------------------------+        +------------------------+        +------------------------+
```

1. **DNS Query**: Resolve hostname to IP address (Client $\to$ DNS Server).
2. **TCP Connection**: 3-way handshake setup with Web Server on Port 80 (`TCP SYN` $\to$ `TCP SYN-ACK` $\to$ `ACK`).
3. **HTTP GET Request**: Request the base index page (`HTTP GET`).
4. **Subsequent HTTP GET Requests**: Request embedded image/script objects.

---

Module 5: Step-by-Step Solved Problems & GATE PYQs

Problem 1: Non-Persistent HTTP Total TCP Connections [Class Example 1]

- **Question**: Consider a web browser at a client machine requesting a webpage using **non-persistent HTTP**. The webpage contains a base HTML file and **eight very small image objects**. Calculate the minimum number of TCP connections required to load the webpage completely.
- **Step-by-Step Solution**:
    1. In non-persistent HTTP, every single object requires its own dedicated TCP connection.
    2. Base HTML file requires **1 TCP connection**.
    3. 8 image objects require **8 separate TCP connections**.
    4. $\text{Total Connections} = 1 + 8 = \mathbf{9 \text{ TCP connections}}$.
- **Answer**: **9**.

---

Problem 2: Non-Persistent Connection Count [GATE-2020, 1-Mark]

- **Question**: Assume that you have made a request for a web page through your web browser to a web server. Initially, the browser cache is empty. Further, the browser is configured to send HTTP requests in **non-persistent mode**. The web page contains text (base HTML) and **five very small images**. What is the minimum number of TCP connections required to display the web page completely in your browser?
- **Step-by-Step Solution**:
    1. Base HTML text file = **1 TCP connection**.
    2. 5 image objects = **5 TCP connections**.
    3. $\text{Total Connections} = 1 + 5 = \mathbf{6 \text{ TCP connections}}$.
- **Answer**: **6**.

---

Problem 3: Sequential Non-Persistent HTTP Total RTTs [Class Example 2]

- **Question**: Consider a web browser requesting a webpage containing a base HTML file and **eight very small image objects** using **non-persistent HTTP**. Assume **no parallel TCP connections** are allowed. How many total RTTs does it take to load the webpage completely?
- **Step-by-Step Solution**:
    1. Loading Base HTML file: \(1 \text{ RTT (TCP setup)} + 1 \text{ RTT (HTTP GET/HTML response)} = \mathbf{2 \text{ RTTs}}\.
    2. For each of the 8 image objects fetched sequentially: $2 \text{ RTTs}$ per object.
    3. Total RTTs $= 2 \text{ RTT (HTML)} + 8 \times 2 \text{ RTT} = 2 + 16 = \mathbf{18 \text{ RTTs}}$.
- **Answer**: **18 RTTs**.

---

Problem 4: Non-Persistent HTTP with Limited Parallelism (5 Channels) [Class Example 3]

- **Question**: Consider a webpage with a base HTML file and **eight very small image objects** requested using **non-persistent HTTP**. Assume the browser can open at most **5 parallel TCP connections**. Calculate the total number of RTTs required to load the webpage completely.
- **Step-by-Step Solution**:
    1. Base HTML file fetch \(= \mathbf{2 \text{ RTTs}}\.
    2. Remaining objects to fetch = 8 images.
    3. **Batch 1 (First 5 images)**: Fetched simultaneously using 5 parallel TCP connections \(= \mathbf{2 \text{ RTTs}}\.
    4. **Batch 2 (Remaining 3 images)**: Fetched simultaneously using 3 parallel TCP connections \(= \mathbf{2 \text{ RTTs}}\.
    5. Total RTTs $= 2 + 2 + 2 = \mathbf{6 \text{ RTTs}}$.
- **Answer**: **6 RTTs**.

---

Problem 5: Non-Persistent HTTP with High Parallelism (10 Channels) [Class Example 4]

- **Question**: Consider a webpage with a base HTML file and **eight very small image objects** requested using **non-persistent HTTP**. Assume the browser can open at most **10 parallel TCP connections**. Calculate the total number of RTTs required to load the webpage completely.
- **Step-by-Step Solution**:
    1. Base HTML file fetch \(= \mathbf{2 \text{ RTTs}}\.
    2. All 8 image objects are fetched simultaneously in a single parallel batch using 8 out of the 10 available parallel channels \(= \mathbf{2 \text{ RTTs}}\.
    3. Total RTTs $= 2 + 2 = \mathbf{4 \text{ RTTs}}$.
- **Answer**: **4 RTTs**.

---

Problem 6: Persistent HTTP Connection Count [Class Example 5]

- **Question**: Consider a web browser requesting a webpage containing a base HTML file and **eight very small image objects** placed on the **same web server** using **persistent HTTP**. How many TCP connections are required to load the webpage completely?
- **Step-by-Step Solution**:
    1. Persistent HTTP (HTTP/1.1) leaves the TCP connection open after sending responses.
    2. Since the base HTML file and all 8 image objects reside on the same server, all objects are transmitted over a **single TCP connection**.
- **Answer**: **1 TCP connection**.

---

Problem 7: Persistent HTTP WITHOUT Pipelining Total RTTs [Class Example 6]

- **Question**: Consider a webpage containing a base HTML file and **eight very small image objects** on the same web server requested using **persistent HTTP without pipelining**. How many total RTTs does it take to load the webpage completely?
- **Step-by-Step Solution**:
    1. Initial TCP connection establishment $= \mathbf{1 \text{ RTT}}$.
    2. Fetching Base HTML file $= \mathbf{1 \text{ RTT}}$.
    3. Fetching 8 image objects sequentially over the open connection (1 RTT per object) $= 8 \times 1 = \mathbf{8 \text{ RTTs}}$.
    4. Total RTTs $= 1 + 1 + 8 = \mathbf{10 \text{ RTTs}}$.
- **Answer**: **10 RTTs**.

---

Problem 8: Persistent HTTP WITH Pipelining Total RTTs [Class Example 7]

- **Question**: Consider a webpage containing a base HTML file and **eight very small image objects** on the same web server requested using **persistent HTTP with pipelining**. How many total RTTs does it take to load the webpage completely?
- **Step-by-Step Solution**:
    1. Initial TCP connection establishment $= \mathbf{1 \text{ RTT}}$.
    2. Fetching Base HTML file $= \mathbf{1 \text{ RTT}}$.
    3. Requests for all 8 image objects are sent back-to-back in a pipeline, receiving all responses in parallel $= \mathbf{1 \text{ RTT}}$.
    4. Total RTTs $= 1 + 1 + 1 = \mathbf{3 \text{ RTTs}}$.
- **Answer**: **3 RTTs**.

---

Problem 9: Persistent HTTP Connection Feasibility [GATE-2014, Set-2, 2-Mark]

- **Question**: A graphical HTML browser resident at a network client machine Q accesses a static HTML webpage from an HTTP server S. The static HTML page has exactly one static embedded image which is also at S. Assuming no caching, which one of the following is correct about the HTML webpage loading (including the embedded image)?
    - (A) Q needs to send at least 2 HTTP requests to S, each necessarily in a separate TCP connection to server S.
    - (B) Q needs to send at least 2 HTTP requests to S, but a single TCP connection to server S is sufficient.
    - (C) A single HTTP request from Q to S is sufficient, and a single TCP connection between Q and#### Problem 9: Persistent Connection Efficiency
- **Scenario**: A client browser fetches a static webpage containing one embedded image. Both the HTML document and the image reside on the same web server. Assuming no local caching exists, how many HTTP requests and TCP connections are required?
- **Step-by-Step Solution**:
    1. Because two distinct web objects are requested (the base HTML page and the embedded image), the client must issue **2 separate HTTP GET requests**.
    2. Under Persistent HTTP (HTTP/1.1), a **single TCP connection** to the server remains open after the first response, allowing the second request to be transmitted over the same connection.
- **Conclusion**: **2 HTTP requests are required, but a single TCP connection is sufficient.**

---

Problem 10: Initial Web Browsing Packet Order

- **Scenario**: A host system is restarted (all caches cleared) and a user requests a webpage from a remote server. In what sequence are the network packets dispatched from the client?
- **Step-by-Step Solution**:
    1. **Step 1 (DNS Query)**: The client must first resolve the server's domain name into an IP address by sending a DNS query packet.
    2. **Step 2 (TCP Connection)**: Once the IP address is known, the client initiates the transport layer 3-way handshake by sending a `TCP SYN` segment to Port 80.
    3. **Step 3 (HTTP Request)**: After the TCP connection is established, the client transmits an `HTTP GET` request for the webpage.
- **Correct Sequence**: **DNS Query** $\longrightarrow$ **TCP SYN** $\longrightarrow$ **HTTP GET Request**.

---

Problem 11: Chronological Packet Order for Web Browsing

- **Scenario**: A user browses to a remote web page containing an index HTML file and embedded images over a single TCP connection. Assuming all DNS and browser caches are initially empty, arrange the following outbound packets in chronological order:
    - (i) `HTTP GET` request for the index page
    - (ii) `DNS` lookup request for the web server IP
    - (iii) `HTTP GET` request for an embedded image object
    - (iv) `TCP SYN` segment to initiate the server connection
- **Step-by-Step Solution**:
    1. **First**: Resolution of the server name via **(ii) DNS request**.
    2. **Second**: Connection establishment via **(iv) TCP SYN**.
    3. **Third**: Fetching the main page via **(i) HTTP GET for index page**.
    4. **Fourth**: Fetching referenced media via **(iii) HTTP GET for image object**.
- **Correct Order**: **(ii)** $\longrightarrow$ **(iv)** $\longrightarrow$ **(i)** $\longrightarrow$ **(iii)**.

---

Problem 12: HTTP 1.1 Protocol Features

- **Scenario**: Evaluate which of the following statements are valid regarding HTTP/1.1 client-server interaction:
    - **Statement A**: Multiple objects from different servers can be fetched over a single TCP connection. _(Invalid — a TCP connection is established between a client and a single specific server IP)._
    - **Statement B**: Multiple objects hosted on the same server can be fetched over a single TCP connection. _(Valid — Persistent HTTP feature)._
    - **Statement C**: Multiple webpages hosted on the same server can be downloaded over an open TCP connection. _(Valid — Connection reuse across requests)._
    - **Statement D**: A client can issue subsequent object requests before receiving responses for earlier requests. _(Valid — HTTP Pipelining)._
- **Valid Statements**: **Statements B, C, and D**.

---
