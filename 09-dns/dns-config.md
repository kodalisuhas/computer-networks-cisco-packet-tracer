# Domain Name System (DNS) Implementation using Cisco Packet Tracer

## Aim:
To configure and verify the Domain Name System (DNS) alongside a dedicated Web Server in Cisco Packet Tracer, map a Fully Qualified Domain Name (FQDN) to an IPv4 address, and demonstrate domain name resolution via `nslookup` and web browser access from a client PC.

## Apparatus:
- Cisco Packet Tracer software
- 1 × Cisco Catalyst 2960-24TT Switch (`Switch0`)                
- 2 × Generic Servers (`Web-Server`, `DNS-Server`)                
- 1 × Generic PC (`PC0`)                
- Copper Straight-Through Cables                

---

## Network Architecture & Addressing Plan:

| Device | Interface | IP Address | Subnet Mask | Default Gateway | DNS Server IP | Service / Role |
|---|---|---|---|---|---|---|
| **PC0** | FastEthernet0 | 192.168.1.1 | 255.255.255.0 | *None* | 192.168.1.20 | DNS Client / Web Client |
| **Web-Server** | FastEthernet0 | 192.168.1.10 | 255.255.255.0 | *None* | *None* | HTTP / HTTPS Host |
| **DNS-Server** | FastEthernet0 | 192.168.1.20 | 255.255.255.0 | *None* | 192.168.1.20 | Domain Name Resolver |

---

## Procedure:

1. Open Cisco Packet Tracer and create a new workspace.
2. Add the following devices to the workspace:
   - **1 Switch:** Cisco Catalyst 2960-24TT (`Switch0`)                
   - **1 Client PC:** `PC0`                
   - **2 Servers:** Rename `Server0` to `Web-Server` and `Server1` to `DNS-Server`                
3. Connect the devices using **Copper Straight-Through** cables as shown in the topology diagram                :
   - `PC0` (FastEthernet0) → `Switch0` (FastEthernet0/1)                
   - `Web-Server` (FastEthernet0) → `Switch0` (FastEthernet0/2)                
   - `DNS-Server` (FastEthernet0) → `Switch0` (FastEthernet0/3)                

4. Configure the Web Server (`Web-Server`):
   - Navigate to **Desktop** → **IP Configuration** → select **Static**.
   - Enter IP Address: `192.168.1.10`, Subnet Mask: `255.255.255.0`.
   - Go to the **Services** tab → select **HTTP**.
   - Ensure both **HTTP** and **HTTPS** are toggled to **ON**.
   - Locate `index.html` in the file manager list, click **(edit)**, and update the content:
     ```html
     <!DOCTYPE html>
     <html>
     <head>
         <title>Campus Portal</title>
     </head>
     <body style="font-family:Arial; text-align:center; padding-top:40px;">
         <h1>Welcome to My DNS Web Page!</h1>
         <hr>
         <p>Domain Name Resolution Successful!</p>
         <p>Hosted on Web-Server IP: 192.168.1.10</p>
     </body>
     </html>
     ```
   - Click **Save** and confirm overwrite.

5. Configure the DNS Server (`DNS-Server`):
   - Navigate to **Desktop** → **IP Configuration** → select **Static**.
   - Enter IP Address: `192.168.1.20`, Subnet Mask: `255.255.255.0`.
   - Go to the **Services** tab → select **DNS**.
   - Toggle the **DNS Service** to **ON**.
   - Create a forward lookup Address Record (**A Record**):
     - **Name:** `www.example.com`
     - **Type:** `A Record`
     - **Address:** `192.168.1.10`
   - Click **Add** to commit the record into the DNS database.

6. Configure the Client PC (`PC0`):
   - Navigate to **Desktop** → **IP Configuration** → select **Static**.
   - Set IP Address: `192.168.1.1`
   - Set Subnet Mask: `255.255.255.0`
   - Set **DNS Server**: `192.168.1.20` *(Crucial: This directs PC0's name queries to the DNS server)*.

7. Verify connectivity and name resolution from `PC0`:
   - Open `PC0` → **Desktop** → **Command Prompt**.
   - **Step A: Test Layer 3 reachability:**
     ```text
     ping 192.168.1.20
     ping 192.168.1.10
     ```
     *(Confirm both servers reply with 0% packet loss).*
   - **Step B: Test DNS Resolution using `nslookup`:**
     ```text
     nslookup [www.example.com](https://www.example.com)
     ```
     *Expected Output:*
     ```text
     Server:  192.168.1.20
     Address: 192.168.1.20

     Name:    [www.example.com](https://www.example.com)
     Address: 192.168.1.10
     ```

8. Test web browsing via Domain Name:
   - Open `PC0` → **Desktop** → **Web Browser**.
   - In the URL bar, enter:
     ```text
     [http://www.example.com](http://www.example.com)
     ```
   - Click **Go**. The web page hosted on `Web-Server` (`192.168.1.10`) renders cleanly on `PC0`.

---

## Failed Cases & Troubleshooting:

1. **Missing DNS Server Field on PC0:**
   - If the `DNS Server` field under `PC0`'s IP Configuration is left blank (`0.0.0.0`), entering `http://www.example.com` in the browser returns `Host Name Unresolved` because the host has no resolver to query.
2. **DNS Service Disabled:**
   - If the DNS service toggle on `DNS-Server` remains **OFF**, `nslookup www.example.com` fails with `DNS request timed out`.
3. **Typo in DNS Record Mapping:**
   - If the A-Record address is entered incorrectly (e.g., pointing to `192.168.1.11` instead of `192.168.1.10`), the browser attempts to initiate a TCP handshake to a non-existent host, resulting in `Request timed out`.
4. **HTTP Service Disabled on Target Server:**
   - If `DNS-Server` successfully resolves the IP to `192.168.1.10`, but the **HTTP** service on `Web-Server` is turned **OFF**, the browser resolves the name but fails to render the page with `Connection refused / timed out`.

---

## Output:
The Domain Name System (DNS) was successfully configured on `DNS-Server` (`192.168.1.20`). When client `PC0` queried `www.example.com`, the DNS server translated the FQDN to IPv4 address `192.168.1.10`, allowing `PC0` to fetch and render the web page using human-readable domain names instead of numerical IP addresses.