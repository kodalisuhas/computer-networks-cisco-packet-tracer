# Experiment: Implementation and Verification of a Multi-Switch Local Area Network

## Aim
- To design, implement, and verify communication across a multi-switch local area network (LAN) using Cisco Packet Tracer.
- To understand packet transmission, switch interconnectivity, and end-device addressing in a linear/bus backbone topology.

---

## Apparatus Required
- **Simulation Software:** Cisco Packet Tracer
- **Network Devices:**
  - 3 × Cisco 2960-24TT Switches (`Switch0`, `Switch1`, `Switch2`)
  - 6 × End Devices (`PC0`, `PC1`, `PC2`, `PC3`, `PC4`, `PC5`)
  - Copper Straight-Through Cables (for PC-to-Switch and Switch-to-Switch via Auto-MDIX or Copper Cross-Over)

---

## Network Configuration

| Device | Interface | IP Address | Subnet Mask | Default Gateway 
|---|---|---|---|---|---|
| **PC0** | FastEthernet0 | `192.168.1.60` | `255.255.255.0` |  
| **PC1** | FastEthernet0 | `192.168.1.10` | `255.255.255.0` | 
| **PC2** | FastEthernet0 | `192.168.1.50` | `255.255.255.0` |
| **PC3** | FastEthernet0 | `192.168.1.20` | `255.255.255.0` |
| **PC4** | FastEthernet0 | `192.168.1.40` | `255.255.255.0` |
| **PC5** | FastEthernet0 | `192.168.1.30` | `255.255.255.0` |

---

## Procedure

1. **Workspace Setup:**
   - Launch Cisco Packet Tracer and open a blank workspace.

2. **Add Devices:**
   - Place 3 switches onto the workspace: `Switch0`, `Switch1`, and `Switch2`.
   - Place 6 PCs onto the workspace: `PC0`, `PC1`, `PC2`, `PC3`, `PC4`, and `PC5`.

3. **Topology Wiring:**
   - Interconnect the switches linearly (forming the backbone bus):
     - `Switch0` <------ `Switch1`
     - `Switch1` <------ `Switch2`
   - Connect the end devices to their designated switches using straight-through cables:
     - `PC0` ---> `Switch0`
     - `PC1` ---> `Switch0`
     - `PC2` ---> `Switch1`
     - `PC3` ---> `Switch1`
     - `PC4` ---> `Switch2`
     - `PC5` ---> `Switch2`

4. **IP Configuration:**
   - Click each PC ---> navigate to the **Desktop** tab ---> select **IP Configuration**.
   - Assign the static IP address and subnet mask according to the table above.

5. **Port Link Verification:**
   - Wait until all Spanning Tree Protocol (STP) transition states complete (orange link lights turn solid green).

6. **Connectivity Testing:**
   - Open **PC1** (`192.168.1.10`) ---> **Desktop** ---> **Command Prompt**.
   - Run the ping command to verify connectivity to all other endpoints:
     ```text
     ping 192.168.1.20
     ping 192.168.1.30
     ping 192.168.1.40
     ping 192.168.1.50
     ping 192.168.1.60
     ```

---

## Error Handling & Failure Scenarios

- **Link Failure:** If the trunk/interlink between `Switch0` and `Switch1` is severed, `PC0` and `PC1` will only be able to communicate with each other and will drop packets directed toward `Switch1` and `Switch2`.
- **IP / Subnet Mismatch:** If any PC is configured outside the `192.168.1.0/24` subnet without a router configured for inter-subnet routing, the ping request will fail (`Request timed out` or `Destination host unreachable`).
- **Duplicate IP Address:** Assigning the same IP to two PCs causes an ARP conflict, resulting in erratic communication or failed ping replies.

---

## Output and Conclusion
- All PCs successfully reply with `Reply from <IP>: bytes=32 time<1ms TTL=128`.
- The network architecture operates as a multi-switch line/bus backbone where frames forward through Layer 2 switching based on MAC learning. All devices within the `192.168.1.0/24` broadcast domain communicate seamlessly.