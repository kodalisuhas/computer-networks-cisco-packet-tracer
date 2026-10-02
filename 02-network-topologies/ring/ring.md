# Ring Topology Implementation using Cisco Packet Tracer

## Aim:
1. To study and implement a Ring Network Topology using Cisco Packet Tracer.
2. To understand device communication, closed-loop connectivity, and redundant link behavior between switches.

## Apparatus:
- Cisco Packet Tracer software
- 4 × Cisco 2960-24TT Switches (`Switch0`, `Switch1`, `Switch2`, `Switch3`)
- 4 × Generic PCs (`PC0`, `PC1`, `PC2`, `PC3`)
- Copper Straight-Through / Crossover Cables

---

## Procedure:

1. Open Cisco Packet Tracer and create a new workspace.
2. Add the following devices:
   - **4 Switches:** Cisco 2960-24TT Switches (`Switch0`, `Switch1`, `Switch2`, `Switch3`)
   - **4 PCs:** `PC0`, `PC1`, `PC2`, `PC3`
3. Arrange the four switches in a closed-loop ring arrangement as shown in the topology diagram:
   - `Switch0` at the bottom-left
   - `Switch1` at the bottom-right
   - `Switch2` at the top-right
   - `Switch3` at the top-left
4. Connect the switches sequentially to form a closed ring:
   - `Switch0` ↔ `Switch1`
   - `Switch1` ↔ `Switch2`
   - `Switch2` ↔ `Switch3`
   - `Switch3` ↔ `Switch0`
5. Connect each PC directly to its corresponding switch using Copper Straight-Through cables:
   - `PC0` (FastEthernet0) → `Switch0`
   - `PC1` (FastEthernet0) → `Switch1`
   - `PC2` (FastEthernet0) → `Switch2`
   - `PC3` (FastEthernet0) → `Switch3`
6. Configure the static IP addresses for each PC as follows:

| Device | Connected Switch | IP Address | Subnet Mask |
|---|---|---|---|---|
| PC0 | Switch0 | 192.168.3.1 | 255.255.255.0 |     
| PC1 | Switch1 | 192.168.3.2 | 255.255.255.0 |    
| PC2 | Switch2 | 192.168.3.3 | 255.255.255.0 |    
| PC3 | Switch3 | 192.168.3.4 | 255.255.255.0 |    

7. To configure the IP address on each PC, navigate to:
   - Click on the PC → **Desktop** → **IP Configuration** → select **Static**.
   - Enter the respective IP Address and Subnet Mask (`255.255.255.0`).
8. Wait for the link indicators to initialize:
   - PC-to-switch links will show green triangles.
   - One inter-switch link may show an amber dot; this is expected behavior due to Spanning Tree Protocol (STP) preventing Layer 2 loops.
9. Test the network connectivity from `PC0` by opening:
   - `PC0` → **Desktop** → **Command Prompt**
10. Enter the following ping commands:
   ```text
   ping 192.168.3.2
   ping 192.168.3.3
   ping 192.168.3.4

```

11. Verify that `Reply from...` messages are received successfully with 0% packet loss.

---

## Failed Cases & Observations:

1. **Host-to-Switch Link Disconnection:** If the cable between a PC (e.g., `PC1`) and its switch is removed, only that individual PC loses communication with the network.
2. **Inter-Switch Link Failure & STP Convergence:** If an active cable between any two switches is disconnected, the Spanning Tree Protocol (STP) automatically transitions the blocked port (amber) to a forwarding state (green), rerouting packets along the alternate path of the ring.
3. **IP Configuration Error:** Assigning duplicate IPs or mismatched subnet masks will cause the `ping` utility to output `Request timed out` or `Destination host unreachable`.

---

## Output:

The resulting network establishes a **Ring Topology**, where the four switches form a closed loop and each PC is linked directly to an access switch, providing alternate routing paths and full communication across the subnet.

```

```