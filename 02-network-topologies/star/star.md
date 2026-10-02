# Star Topology Implementation using Cisco Packet Tracer

## Aim:
1. To study and implement a Star Network Topology using Cisco Packet Tracer.
2. To understand device connectivity and communication using a central switch.

## Apparatus:
- Cisco Packet Tracer software
- 1 × Cisco 2960-24TT Switch (`Switch2`)
- 4 × Generic PCs (`PC0`, `PC1`, `PC2`, `PC3`)
- Copper Straight-Through Cables

---

## Procedure:

1. Open Cisco Packet Tracer and create a new workspace.
2. Add the following devices to the workspace:
   - **1 Switch:** Cisco 2960-24TT Switch (`Switch2`)
   - **4 PCs:** `PC0`, `PC1`, `PC2`, `PC3`
3. Place `Switch2` at the center and arrange the four PCs around it to form a star layout.
4. Connect each PC directly to `Switch2` using **Copper Straight-Through** cables:
   - `PC0` (FastEthernet0) → `Switch2` (FastEthernet0/1)
   - `PC1` (FastEthernet0) → `Switch2` (FastEthernet0/2)
   - `PC2` (FastEthernet0) → `Switch2` (FastEthernet0/3)
   - `PC3` (FastEthernet0) → `Switch2` (FastEthernet0/4)
5. Configure the IP addresses for all PCs according to the network plan:

| Device | IP Address | Subnet Mask | Default Gateway 
|---|---|---|---|
| PC0 | 192.168.1.10 | 255.255.255.0 |   
| PC1 | 192.168.1.20 | 255.255.255.0 |   
| PC2 | 192.168.1.30 | 255.255.255.0 |   
| PC3 | 192.168.1.40 | 255.255.255.0 |   

6. To configure each PC:
   - Click on the PC → navigate to **Desktop** → **IP Configuration**.
   - Select **Static**.
   - Enter the respective **IP Address** and **Subnet Mask**.
7. Wait until link lights turn from amber to green, indicating spanning tree and link convergence are complete.
8. Test connectivity from `PC0`:
   - Click on `PC0` → **Desktop** → **Command Prompt**.
   - Execute the following ping commands:
     ```text
     ping 192.168.1.20
     ping 192.168.1.30
     ping 192.168.1.40
     ```
9. Verify that `Reply from ...` messages are received with 0% packet loss.

---

## Failed Cases & Troubleshooting:

1. **Cable Disconnection:** If the cable between any PC (e.g., `PC1`) and `Switch2` is disconnected, only that specific host loses network access; the remaining PCs can still communicate uninterrupted.
2. **IP Misconfiguration:** If an IP address is duplicate or assigned outside the `192.168.1.0/24` subnet, `ping` requests will result in `Request timed out` or `Destination host unreachable`.
3. **Port Shutdown / Wrong Interface:** If an interface is administratively down or miswired, traffic will not flow to that host.
4. **Central Switch Failure:** If `Switch2` fails or is powered down, the entire network communication collapses (single point of failure).

---

## Output:
The resulting network forms a **Star Topology** where all end devices (`PC0`–`PC3`) are independently connected to a central switch (`Switch2`), enabling point-to-point physical links with centralized switching.