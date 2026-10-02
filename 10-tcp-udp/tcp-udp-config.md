
# Implementation and Protocol Analysis of TCP and UDP in Cisco Packet Tracer

## Aim:
To configure, analyze, and compare the operational mechanics of the Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) in Cisco Packet Tracer using Simulation Mode to inspect Protocol Data Unit (PDU) headers, handshake sequences, port allocations, and control flags across HTTP and DNS services.

## Apparatus:
- Cisco Packet Tracer software
- 1 × Cisco Catalyst 2960-24TT Switch (`Switch0`)[cite: 13]
- 1 × Generic Server (`Server0` configured for HTTP & DNS services)[cite: 13]
- 1 × Generic Client PC (`PC0`)[cite: 13]
- Copper Straight-Through Cables[cite: 13]

---

## Network Architecture & Addressing Plan:

| Device | Interface | IP Address | Subnet Mask | Default Gateway | DNS Server IP | Assigned Roles |
|---|---|---|---|---|---|---|
| **PC0** | FastEthernet0 | 192.168.1.2 | 255.255.255.0 | *None* | 192.168.1.10 | DNS Client / HTTP Client[cite: 13] |
| **Server0** | FastEthernet0 | 192.168.1.10 | 255.255.255.0 | *None* | 192.168.1.10 | DNS Server (Port 53) / HTTP Web Server (Port 80)[cite: 13] |

---

## Procedure:

### Step 1: Physical Topology Setup
1. Open Cisco Packet Tracer and create a new workspace.
2. Add the following devices:
   - 1 Switch: Cisco Catalyst 2960-24TT (`Switch0`)[cite: 13]
   - 1 Host: Generic PC (`PC0`)[cite: 13]
   - 1 Server: Generic Server-PT (`Server0`)[cite: 13]
3. Connect the devices using **Copper Straight-Through** cables[cite: 13]:
   - `PC0` (FastEthernet0) → `Switch0` (FastEthernet0/1)[cite: 13]
   - `Server0` (FastEthernet0) → `Switch0` (FastEthernet0/2)[cite: 13]

### Step 2: IP Configuration
1. Configure `Server0`:
   - Open `Server0` → **Desktop** → **IP Configuration**.
   - Select **Static**.
   - Set **IP Address**: `192.168.1.10`
   - Set **Subnet Mask**: `255.255.255.0`
   - Set **DNS Server**: `192.168.1.10`
2. Configure `PC0`:
   - Open `PC0` → **Desktop** → **IP Configuration**.
   - Select **Static**.
   - Set **IP Address**: `192.168.1.2`
   - Set **Subnet Mask**: `255.255.255.0`
   - Set **DNS Server**: `192.168.1.10` *(Directs PC0 to Server0 for domain resolution)*

### Step 3: Service Configuration on Server0
1. **Enable Web Services (HTTP):**
   - Navigate to `Server0` → **Services** → **HTTP**.
   - Ensure **HTTP** is toggled to **ON** (and optionally **HTTPS** to **ON**).
   - Locate `index.html`, click **(edit)**, and confirm/insert custom text:
     ```html
     <html>
     <body>
     <h1>Welcome to TCP and UDP Protocol Lab!</h1>
     </body>
     </html>
     ```
   - Click **Save** and overwrite.
2. **Enable Domain Name Services (DNS):**
   - Navigate to `Server0` → **Services** → **DNS**.
   - Toggle **DNS Service** to **ON**.
   - Add a forward Address record (**A Record**):
     - **Name:** `www.test.com`
     - **Type:** `A Record`
     - **Address:** `192.168.1.10`
   - Click **Add**.

---

## Protocol Verification in Simulation Mode:

### Step 4: Observing UDP in Action (Via DNS Query)
1. Switch Packet Tracer from **Realtime Mode** to **Simulation Mode** (bottom-right panel or press `Shift + S`).
2. Click **Edit Filters / Show All/None**:
   - Filter and check **DNS** and **UDP** only (uncheck STP, ICMP, and ARP to isolate Layer 4 analysis).
3. On `PC0`, open **Desktop** → **Command Prompt** and execute:
   ```text
   nslookup [www.test.com](https://www.test.com)

```

4. Click **Capture / Forward** step-by-step:
* Observe the initial packet dispatched from `PC0` to `Server0` through `Switch0`.


5. Click on the animated packet envelope in the Event List to inspect **Inbound/Outbound PDU Details**:
* **Layer 4 Transport Protocol:** `UDP`
* **Source Port:** Dynamically allocated ephemeral client port (e.g., `1025` or `1026`)
* **Destination Port:** Fixed well-known service port `53`
* **Source IP:** `192.168.1.2` | **Destination IP:** `192.168.1.10`
* **Header Characteristics:** Observe the absence of sequence numbers, acknowledgment numbers, or connection control flags. UDP immediately transmits data without establishing a prior session.



---

### Step 5: Observing TCP in Action (Via HTTP Three-Way Handshake)

1. In Simulation Mode, click **Reset Simulation** or clear the event list.
2. Under **Edit Filters**, select **TCP** and **HTTP**.
3. On `PC0`, open **Desktop** → **Web Browser**, enter `http://www.test.com` into the URL field, and click **Go**.
4. Click **Capture / Forward** sequentially to trace the connection establishment and data transfer:
* **Packet 1 (TCP SYN):** `PC0` sends a TCP segment to `Server0` (Port 80) with the `SYN` control bit set (`SYN=1, ACK=0`) and an initial sequence number (ISN).
* **Packet 2 (TCP SYN-ACK):** `Server0` accepts the connection, replying with both `SYN` and `ACK` flags asserted (`SYN=1, ACK=1`), acknowledging the client ISN and stating its own sequence number.
* **Packet 3 (TCP ACK):** `PC0` returns an `ACK` segment (`SYN=0, ACK=1`), completing the **Three-Way Handshake**.


5. Continue clicking **Capture / Forward**:
* Once the handshake completes, inspect the subsequent packet: it transitions to Layer 7 **HTTP** (`GET / HTTP/1.1`) over the established TCP socket.
* `Server0` replies with `HTTP/1.1 200 OK` carrying the webpage payload, followed by mutual TCP sequence acknowledgments and eventual connection termination (`FIN/ACK`).


6. Inspect the TCP PDU Details:
* **Layer 4 Transport Protocol:** `TCP`
* **Destination Port:** `80` (HTTP)
* **Control Flags:** Inspected `SYN`, `ACK`, `FIN` fields
* **Tracking Fields:** 32-bit `Sequence Number`, `Acknowledgment Number`, and `Window Size`.



---

## Detailed Comparative Analysis:

| Architectural Metric | Transmission Control Protocol (TCP) | User Datagram Protocol (UDP) |
| --- | --- | --- |
| **OSI Layer** | Layer 4 (Transport Layer) | Layer 4 (Transport Layer) |
| **Operational Model** | Connection-Oriented | Connectionless |
| **Handshake Mechanism** | 3-Way Handshake (`SYN` → `SYN-ACK` → `ACK`) | None (Transmits datagrams immediately) |
| **Reliability & Delivery** | Guaranteed delivery (Retransmission via timers) | Best-effort (No retransmissions or acknowledgments) |
| **Flow & Congestion Control** | Sliding Window, Slow Start, Congestion Avoidance | None (Application must manage flow) |
| **Header Overhead** | 20 to 60 Bytes (Includes flags, options, seq numbers) | Fixed 8 Bytes (Source, Dest Port, Length, Checksum) |
| **Ordering** | Strict packet sequencing and reordering | Packets may arrive out of order or duplicated |
| **Observed Service** | HTTP (Port 80) / HTTPS (Port 443) | DNS (Port 53) |

---

## Failed Cases & Troubleshooting:

1. **Simulation Event Congestion:**
* If STP or ARP filters are left active, hundreds of background broadcast frames will populate the Event List, making it difficult to isolate the TCP handshake. Always clear event filters and select only target protocols (`TCP`, `HTTP`, `UDP`, `DNS`).


2. **DNS Server Misconfiguration on PC0:**
* If the `DNS Server` field on `PC0` is not pointing to `192.168.1.10`, running `nslookup` fails with a timeout and the browser cannot resolve `www.test.com` to initiate the TCP handshake.


3. **HTTP Service Disabled on Server0:**
* If HTTP service is toggled **OFF**, `PC0` will send a TCP SYN packet, but `Server0` will immediately reject it by returning a TCP `RST-ACK` (Reset) segment, aborting the connection.


4. **Transport Layer Port Conflicts:**
* In real-world enterprise deployments, querying a DNS response larger than the negotiated UDP buffer size forces DNS to truncate (`TC=1`) and re-establish over TCP port 53.



---

## Output:

Both transport protocols were successfully observed and verified in Cisco Packet Tracer:

* **UDP** demonstrated connectionless, low-overhead transmission during DNS query resolution (`nslookup www.test.com` on Port 53).
* **TCP** demonstrated connection-oriented, reliable delivery through the execution of a formal three-way handshake (`SYN`, `SYN-ACK`, `ACK`) prior to retrieving web content over HTTP (`Port 80`).

```

```