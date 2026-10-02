# Virtual Local Area Network (VLAN) Implementation using Cisco Packet Tracer

## Aim:
To configure Virtual Local Area Networks (VLANs) on a Cisco Catalyst 2960 switch in Cisco Packet Tracer, assign specific switch ports to designated VLANs (`HR10` and `IT20`), verify communication between devices within the same VLAN, and demonstrate traffic isolation between different VLANs.

## Apparatus:
- Cisco Packet Tracer software
- 1 × Cisco Catalyst 2960-24TT Switch (`Switch0`)[cite: 8]
- 6 × Generic PCs (`PC0`, `PC1`, `PC2`, `PC3`, `PC4`, `PC5`)[cite: 8]
- Copper Straight-Through Cables[cite: 8]

---

## Network Architecture & VLAN Addressing Plan:

### 1. VLAN Definition:
- **VLAN 10 (`HR10`):** Subnet `192.168.10.0/24` (Assigned to `PC0`, `PC1`, `PC5`)[cite: 8]
- **VLAN 20 (`IT20`):** Subnet `192.169.20.0/24` (Assigned to `PC2`, `PC3`, `PC4`)[cite: 8]

### 2. Device Configuration Table:

| Device | Connected Port | VLAN ID | VLAN Name | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|---|---|
| PC0 | FastEthernet0/1 | VLAN 10 | HR10 | 192.168.10.1 | 255.255.255.0 | *None* |
| PC1 | FastEthernet0/2 | VLAN 10 | HR10 | 192.168.10.2 | 255.255.255.0 | *None* |
| PC2 | FastEthernet0/3 | VLAN 20 | IT20 | 192.169.20.2 | 255.255.255.0 | *None* |
| PC3 | FastEthernet0/4 | VLAN 20 | IT20 | 192.169.20.3 | 255.255.255.0 | *None* |
| PC4 | FastEthernet0/5 | VLAN 20 | IT20 | 192.169.20.4 | 255.255.255.0 | *None* |
| PC5 | FastEthernet0/6 | VLAN 10 | HR10 | 192.168.10.3 | 255.255.255.0 | *None* |

---

## Procedure:

1. Open Cisco Packet Tracer and create a new workspace.
2. Add the following devices to the workspace:
   - **1 Switch:** Cisco 2960-24TT Switch (`Switch0`)[cite: 8]
   - **6 PCs:** `PC0`, `PC1`, `PC2`, `PC3`, `PC4`, `PC5`[cite: 8]
3. Arrange the PCs around `Switch0` as shown in the topology diagram:
   - Place `PC0` and `PC1` on the left (HR department)[cite: 8].
   - Place `PC2`, `PC3`, and `PC4` at the bottom (IT department)[cite: 8].
   - Place `PC5` on the right (HR remote desk)[cite: 8].
4. Connect each PC to `Switch0` using **Copper Straight-Through** cables[cite: 8]:
   - `PC0` (FastEthernet0) → `Switch0` (FastEthernet0/1)[cite: 8]
   - `PC1` (FastEthernet0) → `Switch0` (FastEthernet0/2)[cite: 8]
   - `PC2` (FastEthernet0) → `Switch0` (FastEthernet0/3)[cite: 8]
   - `PC3` (FastEthernet0) → `Switch0` (FastEthernet0/4)[cite: 8]
   - `PC4` (FastEthernet0) → `Switch0` (FastEthernet0/5)[cite: 8]
   - `PC5` (FastEthernet0) → `Switch0` (FastEthernet0/6)[cite: 8]
5. Configure static IP addresses and Subnet Masks on each PC:
   - Click on each PC → navigate to **Desktop** → **IP Configuration** → select **Static**.
   - Enter the designated **IP Address** and **Subnet Mask** (`255.255.255.0`) as per the table.
6. Configure the VLANs and assign access ports on `Switch0`:
   - Click on `Switch0` → open the **CLI** tab.
   - Enter global configuration mode and execute:

   ```text
   Switch> enable
   Switch# configure terminal

   ! Create VLAN 10
   Switch(config)# vlan 10
   Switch(config-vlan)# name HR10
   Switch(config-vlan)# exit

   ! Create VLAN 20
   Switch(config)# vlan 20
   Switch(config-vlan)# name IT20
   Switch(config-vlan)# exit

   ! Assign Ports Fa0/1, Fa0/2, and Fa0/6 to VLAN 10
   Switch(config)# interface range fastEthernet 0/1 - 2
   Switch(config-if-range)# switchport mode access
   Switch(config-if-range)# switchport access vlan 10
   Switch(config-if-range)# exit

   Switch(config)# interface fastEthernet 0/6
   Switch(config-if)# switchport mode access
   Switch(config-if)# switchport access vlan 10
   Switch(config-if)# exit

   ! Assign Ports Fa0/3, Fa0/4, and Fa0/5 to VLAN 20
   Switch(config)# interface range fastEthernet 0/3 - 5
   Switch(config-if-range)# switchport mode access
   Switch(config-if-range)# switchport access vlan 20
   Switch(config-if-range)# end
   Switch# write memory

```

7. Verify the switchport assignments using the verification command:
```text
Switch# show vlan brief

```


*Confirm that `HR10` has active ports `Fa0/1, Fa0/2, Fa0/6` and `IT20` has active ports `Fa0/3, Fa0/4, Fa0/5`.*
8. Test intra-VLAN connectivity (successful ping cases):
* From `PC0` Command Prompt, ping other members of `VLAN 10`:
```text
ping 192.168.10.2   (PC1 - HR)
ping 192.168.10.3   (PC5 - HR)

```


* From `PC2` Command Prompt, ping other members of `VLAN 20`:
```text
ping 192.169.20.3   (PC3 - IT)
ping 192.169.20.4   (PC4 - IT)

```


* *Result:* `Reply from...` is received with 0% packet loss.


9. Test inter-VLAN isolation (failed ping cases):
* From `PC0` (VLAN 10) Command Prompt, ping a host in `VLAN 20`:
```text
ping 192.169.20.2   (PC2 - IT)

```


* *Result:* `Request timed out` (Packets are blocked by Layer 2 broadcast domain isolation).



---

## Failed Cases & Observations:

1. **Inter-VLAN Communication Attempt:**
* Attempting to ping from `PC0` (VLAN 10, HR) to `PC2` (VLAN 20, IT) will always fail with `Request timed out`. Because they reside in isolated broadcast domains, a Layer 2 switch cannot forward frames between them without a Layer 3 routing device (Router-on-a-Stick or Multi-Layer Switch).


2. **Port Assignment Mismatch:**
* If `PC5` is physically plugged into `Fa0/6`, but `Fa0/6` was left in the default `VLAN 1`, `PC5` will not be able to ping `PC0` or `PC1` despite having an IP in the `192.168.10.x` subnet.


3. **Subnet/VLAN Mismatch:**
* If a host in `VLAN 10` is mistakenly configured with an IP in the `192.169.20.x` subnet, ARP resolution fails because the broadcast domain boundary blocks cross-subnet address resolution.



---

## Output:

VLANs `HR10` and `IT20` were successfully configured on `Switch0`. Devices within the same VLAN (`PC0`, `PC1`, and `PC5` in `HR10`; `PC2`, `PC3`, and `PC4` in `IT20`) communicate seamlessly, while traffic between the two departments is completely segmented at Layer 2 without leakage.

```

```