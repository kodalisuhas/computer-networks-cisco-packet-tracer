
# Local Area Network (LAN) Implementation using Cisco Packet Tracer

## Aim:
To implement a Local Area Network (LAN) using a switch and three PCs in Cisco Packet Tracer and verify connectivity between the devices using the `ping` command.

## Apparatus:
- Cisco Packet Tracer software
- 1 × Cisco 2960-24TT Switch (`Switch0`)          
- 3 × Generic PCs (`PC0`, `PC1`, `PC2`)          
- Copper Straight-Through Cables          

---

## Procedure:

1. Open Cisco Packet Tracer and create a new workspace.
2. Add the following devices to the workspace:
   - **1 Switch:** Cisco 2960-24TT Switch (`Switch0`)          
   - **3 PCs:** `PC0`, `PC1`, `PC2`          
3. Place `Switch0` at the center/top and arrange the three PCs below it as shown in the topology diagram          .
4. Connect each PC directly to `Switch0` using **Copper Straight-Through** cables          :
   - `PC0` (FastEthernet0) → `Switch0`          
   - `PC1` (FastEthernet0) → `Switch0`          
   - `PC2` (FastEthernet0) → `Switch0`          
5. Configure the static IP addresses for each PC as follows:

| Device | Connected Switch | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| PC0 | Switch0 | 192.169.1.2 | 255.255.255.0 |            
| PC1 | Switch0 | 192.169.1.3 | 255.255.255.0 |            
| PC2 | Switch0 | 192.169.1.4 | 255.255.255.0 |            

6. To configure the IP address of each PC:
   - Click on the PC → navigate to **Desktop** → **IP Configuration**.
   - Select **Static**.
   - Enter the assigned **IP Address** and **Subnet Mask** (`255.255.255.0`).
7. Wait until all link status lights on `Switch0` and the PCs transition from amber to green, confirming link negotiation and spanning-tree convergence are complete          .
8. Test LAN connectivity from `PC0` by navigating to:
   - `PC0` → **Desktop** → **Command Prompt**
9. Execute the following ping commands:
   ```text
   ping 192.169.1.3
   ping 192.169.1.4

```

10. If `Reply from...` messages are received, end-to-end LAN connectivity between all endpoints is successfully established.

---

## Failed Cases & Troubleshooting:

1. **Physical Link Disconnection:** If the cable connecting a PC (e.g., `PC1`) to `Switch0` is detached or deleted, only that individual PC loses access; the remaining devices continue communicating normally.
2. **IP Address Misconfiguration:** If an incorrect subnet mask or an IP outside the `192.169.1.0/24` subnet is entered, the `ping` utility will return `Request timed out` or `Destination host unreachable`.
3. **Duplicate IP Assignment:** Assigning identical IP addresses to multiple PCs causes an ARP conflict, resulting in intermittent or failed connectivity.
4. **Switch Hardware/Power Outage:** Since all devices depend on `Switch0` for Layer 2 frame switching, a complete switch failure isolates all attached endpoints.

---

## Output:

The Local Area Network (LAN) is successfully implemented using one central switch (`Switch0`) and three host PCs (`PC0`–`PC2`), with data communication verified across the local subnet using ICMP ping requests.

```

```