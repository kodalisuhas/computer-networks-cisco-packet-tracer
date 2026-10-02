# Comprehensive Guide to Virtual Local Area Networks (VLANs): Protocol Mechanics, Enterprise Engineering, and Industrial Security

---

## 1. Executive Summary & Foundational Concepts

A **Virtual Local Area Network (VLAN)** is a logical partition of a physical Layer 2 Ethernet switch into multiple, isolated broadcast domains. 

Before VLAN technology existed, segregating departments (such as HR, Finance, or Engineering) required purchasing separate physical switches, separate patch panels, and distinct cabling runs. VLANs virtualize this infrastructure, enabling diverse logical networks with independent security postures and addressing schemes to share the same physical hardware, backplanes, and cable pathways without data leakage.

### The Core Boundary Rule
- **Within the same VLAN**: Hosts communicate purely at **Layer 2 (Data Link Layer)** via direct MAC address lookups in the switch's Content Addressable Memory (CAM) table.
- **Between different VLANs**: Traffic cannot cross VLAN boundaries at Layer 2. To communicate between different VLANs, frames **must pass through a Layer 3 routing engine** (an inter-VLAN router or a Multi-Layer Switch). Without Layer 3 routing, isolation is absolute.

---

## 2. Low-Level Protocol Mechanics: The IEEE 802.1Q Standard

When frames traverse an internal switch backplane or pass over an aggregated inter-switch link, the switch must track which VLAN each Ethernet frame belongs to. This is standardized by **IEEE 802.1Q**.

```text
Standard Ethernet Frame:
[ Preamble ] [ Dest MAC ] [ Source MAC ] [ Type/Length ] [ Data / Payload ] [ FCS ]

802.1Q Tagged Frame (4 extra bytes inserted):
[ Preamble ] [ Dest MAC ] [ Source MAC ] [ 802.1Q Tag (4 Bytes) ] [ Type/Length ] [ Data ] [ FCS ]
                                          |--- TPID (16 bits) ---|
                                          |--- PCP  (3 bits)  ---|
                                          |--- DEI  (1 bit)   ---|
                                          |--- VID  (12 bits) ---|

```

### Breakdown of the 4-Byte (32-Bit) 802.1Q Tag:

1. **Tag Protocol Identifier (TPID - 16 bits)**:
* Always set to hex `0x8100`. Indicates that this frame has an 802.1Q tag appended.


2. **Priority Code Point (PCP - 3 bits)**:
* Implements **Layer 2 Quality of Service (QoS) / IEEE 802.1p**. Provides 8 distinct priority classes (0 to 7) to prioritize latency-critical voice (VoIP) and video over generic web traffic.


3. **Drop Eligible Indicator (DEI - 1 bit)**:
* Indicates whether the frame can be dropped during heavy switch buffer congestion.


4. **VLAN Identifier (VID - 12 bits)**:
* Specifies the exact VLAN the packet belongs to.
* Because VID is 12 bits long, the total number of assignable VLANs is 2^12 = 4,096:
* **VLAN 0 and 4095**: Reserved by the protocol.
* **VLAN 1**: Default native VLAN across enterprise switches (management/control traffic).
* **VLANs 2 – 1001**: Normal range VLANs (stored in `vlan.dat` flash memory).
* **VLANs 1006 – 4094**: Extended range VLANs (used in service provider networks and hyper-scale data centers).





---

## 3. Switch Port Modes: Access vs. Trunk

### Access Ports

* Configured on ports that connect directly to end-user devices (PCs, printers, individual servers).
* **Behavior**: An access port belongs to **only one** VLAN.
* **Untagged Ingress/Egress**:
* Standard Network Interface Cards (NICs) on PCs do not understand 802.1Q tags.
* When a PC transmits an untagged frame, the access switchport injects the internal VLAN ID into the frame header.
* When the switch sends a frame out to a PC, it **strips the 802.1Q tag**, handing a clean, raw Ethernet frame to the endpoint.



### Trunk Ports (IEEE 802.1Q)

* Configured on high-speed uplinks between switches, or between a switch and a router/firewall.
* **Behavior**: Multiplexes traffic from **dozens or hundreds of VLANs** over a single physical cable.
* **Tagged Transmission**: Every frame sent over a trunk carries its 4-byte 802.1Q tag so the receiving switch knows precisely which VLAN and CAM table to direct the traffic to.
* **Native VLAN**:
* One designated VLAN on each trunk (default is VLAN 1) carries untagged management frames (CDP, LLDP, STP BPDUs).
* *Industry Rule*: Both sides of a trunk must share the exact same Native VLAN; otherwise, a **Native VLAN Mismatch** occurs, cross-wiring traffic between two unrelated VLANs.



---

## 4. Inter-VLAN Routing Architectures

Since switches prevent cross-VLAN communication by design, real-world systems route between them using one of three primary methods:

### 1. Legacy Inter-VLAN Routing (Multiple Physical Cables)

* An external router connects one physical cable per VLAN to separate access ports on a switch.
* *Limitation*: Fails to scale; quickly exhausts expensive router interfaces.

### 2. Router-on-a-Stick (ROAS)

* A single high-speed physical interface (e.g., `GigabitEthernet0/0/0`) on a router connects to a switchport configured as an **802.1Q Trunk**.
* The router creates virtual **sub-interfaces** (e.g., `Gi0/0/0.10` and `Gi0/0/0.20`), each acting as the Default Gateway for its respective VLAN:
```text
Router(config)# interface gigabitEthernet 0/0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.100 255.255.255.0

```


* *Use Case*: Small branch offices, SMBs, and retail networks.

### 3. Layer 3 Multi-Layer Switching (Switched Virtual Interfaces - SVI)

* Standard design in modern enterprise campuses and data centers.
* The switch contains an internal hardware Layer 3 routing engine. Virtual router interfaces called **SVIs** (`interface vlan 10`, `interface vlan 20`) are configured directly inside the switch.
* **Hardware-Accelerated Routing**: Packets are routed at line rate (terabits per second) using **ASICs (Application-Specific Integrated Circuits)** via Cisco CEF (Cisco Express Forwarding), bypassing the CPU bottleneck of external routers.

---

## 5. Industrial & Real-World VLAN Deployments

### 1. Zero Trust Corporate Enterprise Networks

* **Departmental Isolation**: Financial databases, Human Resources, Research & Development, and Executive offices sit on independent VLANs.
* **Security Boundaries**: Inter-VLAN traffic passes through a Next-Generation Firewall (Palo Alto, Fortinet) where Layer 7 deep packet inspection and zero-trust policies block unauthorized lateral movement.

### 2. IP Telephony (Voice VLANs)

* Modern enterprise VoIP phones contain a built-in 3-port switch. One cable runs from the wall switch to the phone, and a secondary cable runs from the phone to the user's PC.
* **Dual-VLAN Ports**: The switchport is configured with a regular **Data VLAN** (untagged for the PC) and an auxiliary **Voice VLAN** (802.1Q tagged with PCP priority 5 for the phone).
* *Benefit*: Protects voice calls from buffer latency and prevents users from sniffing audio traffic via Wireshark on their workstations.

### 3. Industrial Automation & OT Networks (Purdue Model / ISA-95)

* In manufacturing plants and power generation facilities, Operational Technology (OT) is strictly partitioned from corporate IT:
* **VLAN 100**: Programmable Logic Controllers (PLCs) and field sensors.
* **VLAN 200**: Human-Machine Interface (HMI) workstations.
* **VLAN 300**: SCADA supervisory servers.


* *Benefit*: Ensures high-frequency multicast safety interlocks are never delayed or interrupted by general corporate internet traffic.

### 4. Cloud Hypervisors and Virtualization (ESXi, Proxmox, KVM)

* Enterprise bare-metal hypervisors run hundreds of virtual machines (VMs) on a single physical server blade.
* The physical NIC is configured as an 802.1Q trunk (Virtual Switch Tagging - VST). The hypervisor's virtual switch maps individual VM virtual NICs to designated VLAN tags, isolating tenant workloads securely on shared physical hardware.

---

## 6. Engineering Hazards, Attack Vectors, and Hardening Controls

### 1. VLAN Hopping via Dynamic Trunking Protocol (DTP)

* **Vulnerability**: Cisco switches historically defaulted switchports to `dynamic auto` or `dynamic desirable`. If an attacker connects a laptop running tools like Yersinia, they can spoof DTP negotiation packets, tricking the switch into turning the access port into a trunk, thereby gaining direct visibility into all VLANs.
* **Enterprise Hardening**:
```text
Switch(config-if)# switchport mode access
Switch(config-if)# switchport nonegotiate

```


Explicitly hardcode all end-user ports to access mode and disable automatic DTP negotiation globally.

### 2. Double-Tagging Attack (802.1Q Double Encapsulation)

* **Vulnerability**: An attacker on an access port sends a frame with **two** 802.1Q tags: an outer tag matching the trunk's **Native VLAN** and an inner tag matching a target victim VLAN (e.g., VLAN 20).
* The first switch strips the outer tag (because it matches the native VLAN) and forwards the frame over the trunk link without re-tagging.
* The second switch inspects the remaining inner tag (VLAN 20) and forwards the malicious frame into the victim VLAN, bypassing Layer 3 routing inspection.


* **Enterprise Hardening**:
* Never use `VLAN 1` as the native VLAN on trunks.
* Assign an unused "dummy" VLAN ID (e.g., `VLAN 999`) as the native VLAN across all trunks.
* Force explicit tagging of native VLAN frames:
```text
Switch(config)# vlan dot1q tag native

```





### 3. Spanning Tree Exhaustion per VLAN (PVST+ Limits)

* In large networks running Per-VLAN Spanning Tree Plus (PVST+), every single VLAN runs its own independent Spanning Tree instance.
* *Hazard*: If a core switch hosts 500 VLANs, it must compute 500 parallel STP topologies, causing control plane CPU exhaustion.
* **Enterprise Hardening**: Migrate from PVST+ to **Multiple Spanning Tree Protocol (MSTP - IEEE 802.1s)**, which maps dozens of VLANs into a single shared spanning tree instance.

### 4. Overcoming the 4,096 VLAN Limit (VXLAN)

* In multi-tenant cloud hyperscale environments (AWS, Azure, GCP), 4,096 VLAN IDs are insufficient to support millions of isolated customer instances.
* **Modern Evolution**: Modern data centers replace standard 802.1Q VLANs with **VXLAN (Virtual Extensible LAN - RFC 7348)**:
* Expands the 12-bit VLAN identifier into a **24-bit VNI (VXLAN Network Identifier)**, raising the scalability ceiling from 4,096 to over **16 million virtual segments**.
* Encapsulates Layer 2 Ethernet frames directly inside standard Layer 3 UDP packets (`Port 4789`), allowing complex virtual topologies to route transparently across underlying physical networks.



```

```