
# Tree Topology Implementation using Cisco Packet Tracer

## Aim:
1. To study and implement a Tree (Hierarchical) Network Topology using Cisco Packet Tracer.
2. To understand device connectivity, hierarchical branching, and data transmission between root, intermediate switches, and leaf end-devices.

## Apparatus:
- Cisco Packet Tracer software
- 3 × Cisco 2960-24TT Switches (`Switch0`, `Switch1`, `Switch2`)        
- 4 × Generic PCs (`PC0`, `PC1`, `PC2`, `PC3`)        
- Copper Straight-Through Cables

---

## Procedure:

1. Open Cisco Packet Tracer and create a new workspace.
2. Add the following devices:
   - **3 Switches:** Cisco 2960-24TT Switches (`Switch0`, `Switch1`, `Switch2`)        
   - **4 PCs:** `PC0`, `PC1`, `PC2`, `PC3`        
3. Arrange the devices in a hierarchical tree structure as shown in the topology diagram:
   - Place `Switch0` at the top level as the root switch        .
   - Place `Switch1` (left branch) and `Switch2` (right branch) at the intermediate level        .
   - Place `PC0` and `PC1` beneath `Switch1`        .
   - Place `PC2` and `PC3` beneath `Switch2`        .
4. Connect the switches hierarchically using Copper Straight-Through cables:
   - `Switch0` ↔ `Switch1`        
   - `Switch0` ↔ `Switch2`        
5. Connect the PCs to their respective intermediate switches using Copper Straight-Through cables:
   - `PC0` (FastEthernet0) → `Switch1`        
   - `PC1` (FastEthernet0) → `Switch1`        
   - `PC2` (FastEthernet0) → `Switch2`        
   - `PC3` (FastEthernet0) → `Switch2`        
6. Configure the static IP addresses for each PC as follows:

| Device | Connected Switch | IP Address | Subnet Mask |
|---|---|---|---|---|
| PC0 | Switch1 | 192.168.5.1 | 255.255.255.0 |          
| PC1 | Switch1 | 192.168.5.2 | 255.255.255.0 |          
| PC2 | Switch2 | 192.168.5.3 | 255.255.255.0 |          
| PC3 | Switch2 | 192.168.5.4 | 255.255.255.0 |          

7. To configure the IP address on each PC:
   - Click on the PC → navigate to **Desktop** → **IP Configuration**.
   - Select **Static**.
   - Enter the respective **IP Address** and **Subnet Mask**.
8. Wait until all link indicators turn green, confirming that all physical connections and spanning tree states are active        .
9. Test network connectivity from `PC0` by opening:
   - `PC0` → **Desktop** → **Command Prompt**
10. Enter the following ping commands:
   ```text
   ping 192.168.5.2
   ping 192.168.5.3
   ping 192.168.5.4

```

11. If `Reply from...` messages are received, connectivity across local branches and through the root switch has been established successfully.

---

## Failed Cases & Observations:

1. **Root Switch Failure:** If `Switch0` goes down or both uplinks are disconnected, inter-branch communication (e.g., `PC0`/`PC1` to `PC2`/`PC3`) will fail, although intra-branch traffic (e.g., `PC0` to `PC1`) will continue working.
2. **Branch Uplink Failure:** If the link between `Switch0` and `Switch1` is disconnected, all devices under `Switch1` (`PC0` and `PC1`) are isolated from the rest of the network.
3. **End-Device Disconnection:** Removing a cable between a PC (e.g., `PC2`) and its parent switch isolates only that specific host without impacting any other branch.
4. **IP Misconfiguration:** If an incorrect subnet mask or an out-of-subnet IP is assigned, ping requests will result in `Request timed out`.

---

## Output:

The resulting network forms a **Tree Topology**, where `Switch0` functions as the central root switch distributing connectivity to child switches (`Switch1` and `Switch2`), which in turn provide network access to the end-host PCs.

```

```