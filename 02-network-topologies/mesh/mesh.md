
# Mesh Topology Implementation using Cisco Packet Tracer

## Aim:
1. To study and implement a Full Mesh Network Topology using Cisco Packet Tracer.
2. To understand multi-path connectivity, link redundancy, and Spanning Tree Protocol (STP) behavior in a fully interconnected switch fabric.

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
3. Arrange the four switches in a square layout as shown in the topology diagram:
   - `Switch0` at the top-left     
   - `Switch1` at the top-right     
   - `Switch2` at the bottom-left     
   - `Switch3` at the bottom-right     
4. Connect the switches to form a full mesh topology where every switch is connected to every other switch:
   - `Switch0` ↔ `Switch1` (Top boundary)     
   - `Switch1` ↔ `Switch3` (Right boundary)     
   - `Switch3` ↔ `Switch2` (Bottom boundary)     
   - `Switch2` ↔ `Switch0` (Left boundary)     
   - `Switch0` ↔ `Switch3` (Diagonal link)     
   - `Switch1` ↔ `Switch2` (Diagonal link)     
5. Connect each PC directly to its nearest switch using Copper Straight-Through cables:
   - `PC0` (FastEthernet0) → `Switch0`     
   - `PC1` (FastEthernet0) → `Switch2`     
   - `PC2` (FastEthernet0) → `Switch1`     
   - `PC3` (FastEthernet0) → `Switch3`     
6. Configure the static IP addresses for each PC as follows:

| Device | Connected Switch | IP Address | Subnet Mask | 
|---|---|---|---|---|
| PC0 | Switch0 | 192.168.1.1 | 255.255.255.0 |       
| PC1 | Switch2 | 192.168.1.2 | 255.255.255.0 |       
| PC2 | Switch1 | 192.168.1.3 | 255.255.255.0 |       
| PC3 | Switch3 | 192.168.1.4 | 255.255.255.0 |       

7. To configure the IP address on each PC:
   - Click on the PC → navigate to **Desktop** → **IP Configuration**.
   - Select **Static**.
   - Enter the corresponding **IP Address** and **Subnet Mask**.
8. Wait for the link status indicators to converge:
   - All PC-to-switch links will turn green     .
   - Multiple redundant inter-switch links will display amber dots     ; this is normal Spanning Tree Protocol (STP) operation putting redundant loops into a blocking state to prevent broadcast storms.
9. Test the network connectivity from `PC0` by opening:
   - `PC0` → **Desktop** → **Command Prompt**
10. Enter the following ping commands:
   ```text
   ping 192.168.1.2
   ping 192.168.1.3
   ping 192.168.1.4

```

11. Confirm that `Reply from...` messages are received, demonstrating successful end-to-end communication across the mesh.

---

## Failed Cases & Observations:

1. **Inter-Switch Link Redundancy:** If an active cable between any two switches is deleted or fails, STP will dynamically unblock an alternate path (amber light transitions to green), allowing communication to persist with minimal packet interruption.
2. **Access Link Failure:** If the connection between an individual PC and its switch is removed, only that specific PC is isolated from the network.
3. **IP/Subnet Misconfiguration:** If an IP address is duplicate or assigned outside the `192.168.1.0/24` subnet, `ping` requests will fail with a `Request timed out` error.

---

## Output:

The resulting network establishes a **Mesh Topology**, where all switches are interconnected via dedicated point-to-point links. This layout provides high fault tolerance, alternative packet routing, and no single point of network-wide failure between switches.

```

```