# Metropolitan Area Network (MAN) Implementation using Cisco Packet Tracer

## Aim:
To implement a Metropolitan Area Network (MAN) connecting two geographically separated LANs using two Cisco 1941 routers, switches, and PCs in Cisco Packet Tracer, and to verify inter-network communication using the `ping` command.

## Apparatus:
- Cisco Packet Tracer software
- 2 × Cisco 1941 Routers (`Router0`, `Router1`)            
- 2 × Cisco 2960-24TT Switches (`Switch0`, `Switch1`)            
- 4 × Generic PCs (`PC0`, `PC1`, `PC2`, `PC3`)            
- Copper Straight-Through Cables (LAN connections)            
- Copper Cross-Over Cable (Router-to-Router connection)            

---

## Procedure:

1. Open Cisco Packet Tracer and create a new workspace.
2. Add the following devices to the workspace:
   - **2 Routers:** Cisco 1941 Routers (`Router0`, `Router1`)            
   - **2 Switches:** Cisco 2960-24TT Switches (`Switch0`, `Switch1`)            
   - **4 PCs:** `PC0`, `PC1`, `PC2`, `PC3`            
3. Arrange the devices according to the topology diagram:
   - Place `Router0`, `Switch0`, `PC0`, and `PC1` on the left side (LAN 1)            .
   - Place `Router1`, `Switch1`, `PC2`, and `PC3` on the right side (LAN 2)            .
4. Connect the physical links:
   - Connect `Router0` to `Router1` using a **Copper Cross-Over Cable** (e.g., GigabitEthernet0/1 to GigabitEthernet0/1)            .
   - Connect `Router0` to `Switch0` using a **Copper Straight-Through Cable** (GigabitEthernet0/0 to FastEthernet0/1)            .
   - Connect `Router1` to `Switch1` using a **Copper Straight-Through Cable** (GigabitEthernet0/0 to FastEthernet0/1)            .
   - Connect `PC0` and `PC1` to `Switch0` using **Copper Straight-Through Cables**            .
   - Connect `PC2` and `PC3` to `Switch1` using **Copper Straight-Through Cables**            .
5. Configure the IP settings for all PCs:

| Device | Connected Switch | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| PC0 | Switch0 | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 |
| PC1 | Switch0 | 192.168.1.3 | 255.255.255.0 | 192.168.1.1 |
| PC2 | Switch1 | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 |
| PC3 | Switch1 | 192.168.2.3 | 255.255.255.0 | 192.168.2.1 |

6. To configure each PC:
   - Click on the PC -> navigate to **Desktop** -> **IP Configuration**.
   - Select **Static**.
   - Enter the corresponding **IP Address**, **Subnet Mask** (`255.255.255.0`), and **Default Gateway**.
7. Configure the router interfaces and enable them (`no shutdown`):

   **Router0 Configuration:**
   - LAN Interface (GigabitEthernet0/0):
     - IP Address: `192.168.1.1`
     - Subnet Mask: `255.255.255.0`
     - Port Status: **ON**
   - Inter-Router / WAN Link (GigabitEthernet0/1):
     - IP Address: `10.0.0.1`
     - Subnet Mask: `255.255.255.252`
     - Port Status: **ON**

   **Router1 Configuration:**
   - LAN Interface (GigabitEthernet0/0):
     - IP Address: `192.168.2.1`
     - Subnet Mask: `255.255.255.0`
     - Port Status: **ON**
   - Inter-Router / WAN Link (GigabitEthernet0/1):
     - IP Address: `10.0.0.2`
     - Subnet Mask: `255.255.255.252`
     - Port Status: **ON**

8. Configure static routing on both routers to allow inter-LAN communication:
   - On **Router0**: Add a static route for the remote network `192.168.2.0/24` pointing to next-hop `10.0.0.2`:
     ```text
     ip route 192.168.2.0 255.255.255.0 10.0.0.2
     ```
   - On **Router1**: Add a static route for the remote network `192.168.1.0/24` pointing to next-hop `10.0.0.1`:
     ```text
     ip route 192.168.1.0 255.255.255.0 10.0.0.1
     ```
9. Wait until all link triangles turn green, confirming physical links and interfaces are fully active            .
10. Test end-to-end communication from `PC0` across the MAN by opening:
    - `PC0` -> **Desktop** -> **Command Prompt**
11. Enter the following ping commands:
    ```text
    ping 192.168.2.2
    ping 192.168.2.3
    ```
12. If `Reply from...` messages are received (the first ping packet may time out during initial ARP resolution), inter-LAN routing across the MAN is successfully established.

---

## Failed Cases & Troubleshooting:

1. **Missing or Mismatched Default Gateway:** If a PC has an incorrect Default Gateway address, it will not know where to direct packets destined outside its local subnet, causing the ping to fail with `Request timed out`.
2. **Router Interface Administratively Down:** If the GigabitEthernet ports on either router are not turned **ON** (`no shutdown`), link lights remain red and packets cannot transit.
3. **Missing Static Route:** If the routers do not have routes configured for each other's LAN subnets, packets will be dropped by the router with a `Destination host unreachable` response.
4. **Subnet Mask Mismatch:** Assigning a conflicting mask on the inter-router link will prevent the point-to-point link from establishing Layer 3 reachability.

---

## Output:
The resulting network establishes a **Metropolitan Area Network (MAN)** connecting two remote branch LANs (`192.168.1.0/24` and `192.168.2.0/24`) via routed point-to-point router links, enabling full bidirectional communication verified by ICMP ping tests.