# Wide Area Network (WAN) Implementation using Cisco Packet Tracer

## Aim:
To implement a Wide Area Network (WAN) connecting three geographically separated Local Area Networks (LANs) using three routers interconnected via Serial WAN links in Cisco Packet Tracer, configure static routing, and verify end-to-end communication across the autonomous segments using the `ping` command.

## Apparatus:
- Cisco Packet Tracer software
- 3 × Cisco Routers (`Router0`, `Router1`, `Router2` with Serial interfaces)
- 3 × Cisco 2960-24TT Switches (`Switch0`, `Switch1`, `Switch2`)
- 9 × Generic PCs (`PC0` through `PC8`)
- Copper Straight-Through Cables (LAN switch-to-host and switch-to-router connections)
- Serial DCE/DTE Cables (Router-to-router WAN connections)

---

## Network Architecture & Addressing Plan:

### 1. Host (PC) Configuration Table:

| Device | Connected LAN / Switch | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| PC0 | LAN 1 (Switch0) | 10.10.10.1 | 255.255.255.0 | 10.10.10.100 |
| PC1 | LAN 1 (Switch0) | 10.10.10.2 | 255.255.255.0 | 10.10.10.100 |
| PC2 | LAN 1 (Switch0) | 10.10.10.3 | 255.255.255.0 | 10.10.10.100 |
| PC3 | LAN 2 (Switch1) | 20.20.20.1 | 255.255.255.0 | 20.20.20.100 |
| PC4 | LAN 2 (Switch1) | 20.20.20.2 | 255.255.255.0 | 20.20.20.100 |
| PC5 | LAN 2 (Switch1) | 20.20.20.3 | 255.255.255.0 | 20.20.20.100 |
| PC6 | LAN 3 (Switch2) | 30.30.30.1 | 255.255.255.0 | 30.30.30.100 |
| PC7 | LAN 3 (Switch2) | 30.30.30.2 | 255.255.255.0 | 30.30.30.100 |
| PC8 | LAN 3 (Switch2) | 30.30.30.3 | 255.255.255.0 | 30.30.30.100 |

### 2. Router Interface Addressing:

| Router | Interface | Connected To / Link | IP Address | Subnet Mask | Port Status |
|---|---|---|---|---|---|
| **Router0** | FastEthernet0/0 | LAN 1 (Switch0) | 10.10.10.100 | 255.255.255.0 | ON (`no shutdown`) |
| | Serial2/0 | WAN Link 1 (to Router1) | 40.40.40.1 | 255.255.255.0 | ON (`no shutdown`) |
| **Router1** | FastEthernet0/0 | LAN 2 (Switch1) | 20.20.20.100 | 255.255.255.0 | ON (`no shutdown`) |
| | Serial2/0 | WAN Link 1 (to Router0) | 40.40.40.2 | 255.255.255.0 | ON (`no shutdown`) |
| | Serial3/0 | WAN Link 2 (to Router2) | 50.50.50.1 | 255.255.255.0 | ON (`no shutdown`) |
| **Router2** | FastEthernet0/0 | LAN 3 (Switch2) | 30.30.30.100 | 255.255.255.0 | ON (`no shutdown`) |
| | Serial2/0 | WAN Link 2 (to Router1) | 50.50.50.2 | 255.255.255.0 | ON (`no shutdown`) |

---

## Procedure:

1. Open Cisco Packet Tracer and create a new workspace.
2. Add the required devices to the workspace:
   - 3 Routers: `Router0`, `Router1`, `Router2`
   - 3 Switches: `Switch0`, `Switch1`, `Switch2`
   - 9 PCs: `PC0` through `PC8`
3. Arrange the devices into three distinct LAN regions connected to their respective routers:
   - **LAN 1 (Left):** `PC0`, `PC1`, `PC2` connected to `Switch0` and `Router0`
   - **LAN 2 (Center):** `PC3`, `PC4`, `PC5` connected to `Switch1` and `Router1`
   - **LAN 3 (Right):** `PC6`, `PC7`, `PC8` connected to `Switch2` and `Router2`
4. Connect each PC to its corresponding switch using **Copper Straight-Through cables**.
5. Connect each switch to its local router's `FastEthernet0/0` interface using **Copper Straight-Through cables**.
6. Interconnect the routers using **Serial DTE/DCE cables**:
   - `Router0` (`Serial2/0`) ↔ `Router1` (`Serial2/0`)
   - `Router1` (`Serial3/0`) ↔ `Router2` (`Serial2/0`)
7. Configure the static IP addresses, Subnet Masks, and Default Gateways for each PC according to the Host Configuration Table:
   - Navigate to **PC** → **Desktop** → **IP Configuration**.
8. Configure the FastEthernet LAN interfaces and Serial WAN interfaces on all three routers, ensuring **Port Status is set to ON** (`no shutdown`) on every interface.
9. Configure **Static Routing** on each router to enable cross-network forwarding:

   - **On Router0 (Left Router):**
     ```text
     ip route 20.20.20.0 255.255.255.0 40.40.40.2
     ip route 30.30.30.0 255.255.255.0 40.40.40.2
     ip route 50.50.50.0 255.255.255.0 40.40.40.2
     ```

   - **On Router1 (Central Router):**
     ```text
     ip route 10.10.10.0 255.255.255.0 40.40.40.1
     ip route 30.30.30.0 255.255.255.0 50.50.50.2
     ```

   - **On Router2 (Right Router):**
     ```text
     ip route 10.10.10.0 255.255.255.0 50.50.50.1
     ip route 20.20.20.0 255.255.255.0 50.50.50.1
     ip route 40.40.40.0 255.255.255.0 50.50.50.1
     ```

10. Wait until all interface link lights turn green.
11. Test LAN-to-LAN and WAN-to-WAN connectivity from `PC0` by navigating to:
    - `PC0` → **Desktop** → **Command Prompt**
12. Execute the following verification commands:
    ```text
    ping 10.10.10.100
    ping 20.20.20.1
    ping 30.30.30.1
    ```
13. If `Reply from...` messages are received (noting that the initial ICMP packet may experience a transient timeout due to ARP discovery across multiple hops), the end-to-end WAN routing path is established.

---

## Failed Cases & Troubleshooting:

1. **Destination Host Unreachable from Intermediate Hop:**
   - If pinging `30.30.30.1` outputs `Reply from 40.40.40.2: Destination host unreachable`, the packet successfully reaches `Router1`, but `Router1` lacks a valid static route to the target network or its outgoing serial interface (`Serial3/0`) is down.
2. **Missing Return Route (Asymmetric Route Failure):**
   - If `Router0` and `Router1` know the path forward to `30.30.30.0`, but `Router2` lacks a return route for `10.10.10.0`, the target receives the echo request but cannot return the echo reply, causing `Request timed out`.
3. **Invalid Default Gateway:**
   - Setting a Default Gateway ending in `.0` (the network identifier) or pointing to an unconfigured IP prevents endpoints from offloading foreign-bound packets to the local router.
4. **Interface Administratively Down:**
   - If any router interface remains in the `shutdown` state (indicated by red triangles), link negotiation fails and routing through that segment is impossible.

---

## Output:
The resulting architecture successfully implements a multi-hop **Wide Area Network (WAN)**. Three isolated local subnets (`10.10.10.0/24`, `20.20.20.0/24`, and `30.30.30.0/24`) achieve full bidirectional communication across two intermediate serial transit links via hop-by-hop static routing.