# Essential Cisco IOS and Host Command Reference Guide

---

## Lab Procedure: Accessing the Cisco IOS Command Line Interface (CLI)

- **Step 1:** Open Cisco Packet Tracer and create a clean workspace.
- **Step 2:** Drag a Cisco Router (e.g., Cisco 2911 or 1941) or a Cisco Switch (2960) into the workspace.
- **Step 3:** Click on the device and navigate to the **CLI** tab.
- **Step 4:** If prompted with `Continue with configuration dialog? [yes/no]:`, type `no` and press **Enter** to access the initial prompt.

---

## 1. Cisco IOS Modes, Navigation, and Core Utility Commands

Cisco IOS uses a structured hierarchy of command modes. Commands available at one level are restricted until you navigate into the proper mode.

| Command | Prompt / Context | Purpose & Technical Explanation |
|---|---|---|
| `enable` | `Router>` | Enters **Privileged EXEC Mode** (`Router#`). Unlocks advanced diagnostics, hardware verification, and access to configuration mode. |
| `disable` | `Router#` | Exits Privileged EXEC mode and drops the user back to restricted **User EXEC Mode** (`Router>`). |
| `configure terminal` (or `conf t`) | `Router#` | Enters **Global Configuration Mode** (`Router(config)#`), allowing device-wide configuration changes (interfaces, routing, hostnames). |
| `exit` | Any sub-mode | Steps back **one level** in the mode hierarchy (e.g., from `config-if` back to `config`, or from `config` back to privileged mode). |
| `end` | Any sub-mode | Immediately jumps back to **Privileged EXEC Mode** (`Router#`), regardless of how deeply nested the configuration prompt is. |
| `Ctrl + Z` | Any sub-mode | Keyboard shortcut equivalent to the `end` command; returns instantly to Privileged EXEC mode. |
| `?` (Context Help) | Any mode | Displays all available commands valid within the current execution mode. |
| `show ?` | Any mode | Displays every parameter, keyword, and subsystem that can follow the `show` command. |
| `<partial_cmd><Tab>` | Any mode | **Tab Completion**: Automatically completes a partially typed keyword once enough characters are provided to identify it uniquely. |
| `hostname <name>` | `Router(config)#` | Renames the device identifier displayed at the CLI prompt (e.g., `hostname R1` changes the prompt to `R1(config)#`). |

---

## 2. Interface Configuration & Port State Commands

Commands used to assign Layer 3 addresses, configure Layer 2 parameters, and bring physical links into service.

| Command | Execution Mode | Purpose & Technical Explanation |
|---|---|---|
| `interface <type> <num>` | `Router(config)#` | Enters **Interface Configuration Mode** (e.g., `interface gigabitEthernet 0/0` changes prompt to `Router(config-if)#`). |
| `interface range <type> <id-id>` | `Switch(config)#` | Selects multiple consecutive ports simultaneously for bulk configuration (e.g., `interface range fastEthernet 0/1 - 10`). |
| `ip address <ip> <mask>` | `Router(config-if)#` | Assigns an IPv4 address and subnet mask to the chosen interface (e.g., `ip address 192.168.1.1 255.255.255.0`). |
| `no shutdown` | `Router(config-if)#` | **Brings the interface UP**. Cisco router ports default to administratively down (`shutdown`). This command activates the interface and starts link negotiation. |
| `shutdown` | `Router(config-if)#` | Administratively turns off an interface, halting all Layer 1/Layer 2 transmissions. |
| `clock rate <rate>` | `Router(config-if)#` | Sets the hardware serialization clock rate (e.g., `clock rate 64000`) on serial interfaces acting as Data Communications Equipment (**DCE**). |
| `description <text>` | `Router(config-if)#` | Attaches a human-readable label to an interface to document connected circuits or endpoints (e.g., `description Uplink to Core Router`). |

---

## 3. Switching & VLAN Management Commands

Commands executed on Cisco Catalyst switches to configure Layer 2 boundaries, trunk lines, and port security.

| Command | Execution Mode | Purpose & Technical Explanation |
|---|---|---|
| `vlan <vlan-id>` | `Switch(config)#` | Creates a new VLAN or enters its configuration sub-mode (e.g., `vlan 10`). |
| `name <vlan-name>` | `Switch(config-vlan)#` | Assigns a descriptive text identifier to the active VLAN (e.g., `name HR_Department`). |
| `switchport mode access` | `Switch(config-if)#` | Explicitly sets a port as an untagged access port, dedicated to connecting a single host workstation or printer. |
| `switchport access vlan <id>` | `Switch(config-if)#` | Assigns an access port to a specific VLAN (e.g., `switchport access vlan 10`). Traffic arriving from that host is isolated to that broadcast domain. |
| `switchport mode trunk` | `Switch(config-if)#` | Configures an inter-switch or router uplink as an **802.1Q trunk**, allowing frames from multiple VLANs to traverse a single physical link with VLAN tags. |
| `switchport trunk allowed vlan <list>` | `Switch(config-if)#` | Restricts trunk traffic to explicitly permitted VLAN IDs (e.g., `switchport trunk allowed vlan 10,20,30`). |
| `switchport nonegotiate` | `Switch(config-if)#` | Disables Dynamic Trunking Protocol (DTP) frame generation on the port to prevent unauthorized trunk negotiation attacks. |

---

## 4. Routing & Forwarding Commands

Commands used to configure static routes, routing protocols, and inter-VLAN sub-interfaces.

| Command | Execution Mode | Purpose & Technical Explanation |
|---|---|---|
| `ip route <dest-net> <mask> <next-hop>` | `Router(config)#` | Creates a **Static Route** in the Routing Information Base (RIB). Instructs the router to send packets for `<dest-net>` to the adjacent router interface IP (`<next-hop>`). |
| `ip route 0.0.0.0 0.0.0.0 <next-hop>` | `Router(config)#` | Creates a **Default Route (Gateway of Last Resort)**. Forwards any packet whose destination IP does not match a more specific route. |
| `encapsulation dot1Q <vlan-id>` | `Router(config-subif)#` | Used in **Router-on-a-Stick (ROAS)** architectures. Enables 802.1Q tagging on a virtual sub-interface (e.g., `Gi0/0.10`) to terminate an access VLAN at Layer 3. |
| `ip routing` | `Switch(config)#` | Activates the Layer 3 routing engine on a Multi-Layer Switch (e.g., Cisco 3560/3650), enabling inter-VLAN routing across Switched Virtual Interfaces (SVIs). |

---

## 5. System State, Diagnostics, and Verification Commands

Read-only inspection commands executed from Privileged EXEC mode to troubleshoot connectivity, hardware states, and tables.

| Command | Execution Mode | Purpose & Technical Explanation |
|---|---|---|
| `show ip interface brief` | `Router#` | Displays a concise table of all physical/virtual interfaces, their assigned IP addresses, Layer 1 status (`Status`), and Layer 2 status (`Protocol`). |
| `show ip route` | `Router#` | Displays the active IPv4 routing table, showing directly connected networks (`C`), local interfaces (`L`), and static routes (`S`). |
| `show vlan brief` | `Switch#` | Displays all configured VLANs along with their current status and list of assigned access ports. |
| `show interfaces trunk` | `Switch#` | Verifies active trunk ports, native VLANs, encapsulation methods, and allowed VLAN lists across uplinks. |
| `show mac address-table` | `Switch#` | Dumps the switch's Content Addressable Memory (CAM) table, showing MAC addresses learned on specific physical ports. |
| `show running-config` (or `sh run`) | `Router#` / `Switch#` | Dumps the current active operational configuration stored in volatile **RAM**. |
| `show startup-config` (or `sh start`)| `Router#` / `Switch#` | Dumps the persistent configuration stored in non-volatile **NVRAM**, which loads when the device powers on. |
| `show arp` | `Router#` | Displays the router's ARP cache, mapping resolved IPv4 addresses to physical MAC addresses and interfaces. |

---

## 6. Device Security & Non-Volatile Memory (NVRAM) Persistence

Commands used to safeguard administrative access and ensure configuration persistence across reboots.

| Command | Execution Mode | Purpose & Technical Explanation |
|---|---|---|
| `enable secret <password>` | `Router(config)#` | Sets an encrypted (MD5 or SHA-256) password required to transition from User EXEC (`>`) to Privileged EXEC (`#`). |
| `service password-encryption` | `Router(config)#` | Encrypts all plaintext passwords stored in the configuration file using a reversible Type 7 cipher, obscuring them from casual observation. |
| `write memory` (or `wr`) | `Router#` / `Switch#` | Fast method to save the active running configuration from RAM into NVRAM. |
| `copy running-config startup-config` | `Router#` / `Switch#` | The formal Cisco IOS command to commit the active memory state into NVRAM. If omitted, all configurations are lost upon power loss or reboot. |
| `reload` | `Router#` / `Switch#` | Performs a controlled software reboot of the device operating system. |
| `erase startup-config` | `Router#` / `Switch#` | Wipes the non-volatile memory (NVRAM). After running this command followed by `reload`, the device reboots to factory default settings. |

---

## 7. Client Host & Application Testing Commands

Commands executed in the client workstation's **Command Prompt** or interactive application shells (`FTP`, `Telnet`) to test protocol reachability.

| Command / Keyword | Environment | Purpose & Technical Explanation |
|---|---|---|
| `ipconfig` | Host Command Prompt | Displays the host's current IPv4 address, Subnet Mask, and Default Gateway. |
| `ipconfig /all` | Host Command Prompt | Displays complete network settings, including physical MAC address, assigned DNS servers, and DHCP lease data. |
| `ping <target-ip>` | Host Command Prompt | Sends ICMP Echo Request packets to verify end-to-end Layer 3 reachability and measures Round-Trip Time (RTT). |
| `tracert <target-ip>` | Host Command Prompt | Traces the hop-by-hop path taken by packets to reach a destination by incrementing the TTL counter and listening for ICMP Time Exceeded responses. |
| `nslookup <domain-name>` | Host Command Prompt | Sends a DNS query to the configured DNS server to resolve an FQDN (e.g., `www.example.com`) to an IP address. |
| `ftp <server-ip>` | Host Command Prompt | Initiates a TCP control connection to an FTP server on port 21 to start an authenticated file transfer session. |
| `dir` | FTP Shell (`ftp>`) | Lists files and subdirectories hosted in the current working directory of the remote FTP server. |
| `put <filename>` | FTP Shell (`ftp>`) | **Uploads** a local file from the client host's storage up to the remote FTP server. |
| `get <filename>` | FTP Shell (`ftp>`) | **Downloads** a file from the remote FTP server down to the client host's local storage. |
| `quit` | FTP Shell (`ftp>`) | Closes data/control connections cleanly and exits the interactive FTP session back to the command prompt. |