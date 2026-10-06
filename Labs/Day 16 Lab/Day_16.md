# Day 16 – VLANs (Virtual Local Area Networks) Part 1

## Course
**Jeremy's IT Lab — CCNA 200-301**

## Overview

Day 16 covered the fundamentals of **VLANs (Virtual Local Area Networks)**, including LANs, broadcast domains, the purpose of VLANs, and basic VLAN configuration on Cisco switches.

The Packet Tracer lab involved configuring three VLANs on a switch, assigning end-device interfaces to the appropriate VLANs, configuring three router interfaces as gateways for the VLANs, and verifying connectivity between PCs.

The three VLANs used in the lab were:

| VLAN | Department | Network |
|---|---|---|
| VLAN 10 | Engineering | `10.0.0.0/26` |
| VLAN 20 | HR | `10.0.0.64/26` |
| VLAN 30 | Sales | `10.0.0.128/26` |

## What I Learned

- A LAN can be defined as a single **broadcast domain**.
- A broadcast domain consists of devices that receive a broadcast frame sent by a member of that domain.
- A VLAN logically separates end hosts at **Layer 2**.
- VLANs are configured on switches on a **per-interface basis**.
- A switch does not directly forward traffic between different VLANs.
- Inter-VLAN communication requires traffic to pass through a Layer 3 device such as a router.
- VLANs can reduce unnecessary broadcast traffic.
- VLANs can also provide logical separation between groups of devices for security purposes.
- An **access port** belongs to a single VLAN and usually connects to an end host such as a PC.
- A **trunk port** carries multiple VLANs.
- VLANs `1`, `1002`, `1003`, `1004`, and `1005` exist by default and cannot be deleted.
- A VLAN can be automatically created when a switch interface is assigned to a VLAN that does not yet exist.
- The `show vlan brief` command can be used to verify VLANs and their assigned switch ports.

## Key Concepts

### LAN

A LAN is described in the course as a group of devices in a single location. A more specific definition is that a LAN is a **single broadcast domain**, including all devices within that broadcast domain.

### Broadcast Domain

A broadcast domain is the group of devices that will receive a broadcast frame sent by any one of its members.

The broadcast destination MAC address is:

```text
FFFF.FFFF.FFFF
```

Routers separate broadcast domains because broadcast traffic is not forwarded through a router interface.

### VLAN

A VLAN is a logical separation of devices at **Layer 2**.

VLANs:

- Are configured on switches on a per-interface basis.
- Logically separate end hosts at Layer 2.
- Prevent a switch from directly forwarding traffic between different VLANs.
- Separate broadcast domains on a switch.

### VLANs and Subnets

The theory material demonstrates that simply placing departments into different IP subnets does not automatically separate their Layer 2 broadcast domains.

For example:

- Engineering: `192.168.1.0/26`
- HR: `192.168.1.64/26`
- Sales: `192.168.1.128/26`

Even though these are different Layer 3 subnets, they can still exist within the same Layer 2 broadcast domain if VLAN separation is not configured.

### Inter-VLAN Communication

A switch does not perform inter-VLAN routing in the configuration covered in this lesson.

Traffic between different VLANs must therefore be sent through a router or another Layer 3 device.

```text
VLAN 10 → Router → VLAN 30
```

### Access Ports

An access port belongs to a single VLAN and normally connects to an end host such as a PC.

Example:

```text
switchport mode access
switchport access vlan 10
```

### Trunk Ports

A switchport that carries multiple VLANs is called a **trunk port**.

Trunking is introduced in the theory material as a topic for a later video.

### Purpose of VLANs

The theory material identifies two important purposes:

#### Performance

Large amounts of unnecessary broadcast traffic can reduce network performance.

VLANs can divide a network into separate broadcast domains and reduce the scope of broadcast traffic.

#### Security

VLANs can logically separate groups of users or devices.

For example:

```text
Engineering → VLAN 10
HR          → VLAN 20
Sales       → VLAN 30
```

Traffic between VLANs must pass through a Layer 3 device, where security policies can be applied.

## Packet Tracer Lab

### Objective

Configure a network containing three VLANs, assign PCs to the appropriate VLANs, configure router interfaces as gateways for each VLAN, verify the VLAN assignments, and test connectivity between PCs.

The lab also instructed checking broadcast behavior by sending a broadcast ping to a subnet broadcast address in Packet Tracer Simulation Mode.

### Question Description

The lab instructions required the following:

1. Configure the correct IP address and subnet mask on each PC.
2. Configure the default gateway as the last usable address of the corresponding subnet.
3. Make three connections between R1 and SW1.
4. Configure one R1 interface for each VLAN using the corresponding gateway address.
5. Configure SW1 interfaces in the appropriate VLANs.
6. Name the VLANs as Engineering, HR, and Sales.
7. Ping between PCs to check connectivity.
8. Send a broadcast ping from a PC to the subnet broadcast address and observe which devices receive the broadcast in Simulation Mode.

### Network Topology

The lab contains:

```text
                 R1
              /  |  \
             /   |   \
           VLAN10 VLAN20 VLAN30
             |     |     |
            SW1 ---+-----+
           / |      |     \
         PC1 PC2   PC3 PC4 PC5 PC6
```

The actual topology consists of:

- One Cisco 2911 router: `R1`
- One switch: `SW1`
- Six PCs: `PC1` through `PC6`
- Three VLANs:
  - VLAN 10 – Engineering
  - VLAN 20 – HR
  - VLAN 30 – Sales

### Addressing / Configuration Details

| VLAN | Department | Network | Subnet Mask | Gateway |
|---|---|---|---|---|
| VLAN 10 | Engineering | `10.0.0.0/26` | `255.255.255.192` | `10.0.0.62` |
| VLAN 20 | HR | `10.0.0.64/26` | `255.255.255.192` | `10.0.0.126` |
| VLAN 30 | Sales | `10.0.0.128/26` | `255.255.255.192` | `10.0.0.190` |

The PC addresses shown in the topology are:

| PC | VLAN | IP Address |
|---|---|---|
| PC1 | VLAN 10 | `10.0.0.1` |
| PC2 | VLAN 10 | `10.0.0.2` |
| PC3 | VLAN 20 | `10.0.0.65` |
| PC4 | VLAN 20 | `10.0.0.66` |
| PC5 | VLAN 30 | `10.0.0.129` |
| PC6 | VLAN 30 | `10.0.0.130` |

PC1 was configured with:

```text
IP Address:      10.0.0.1
Subnet Mask:     255.255.255.192
Default Gateway: 10.0.0.62
```

### Router Configuration

| R1 Interface | IP Address | Subnet Mask | VLAN |
|---|---|---|---|
| GigabitEthernet0/0 | `10.0.0.62` | `255.255.255.192` | VLAN 10 |
| GigabitEthernet0/1 | `10.0.0.126` | `255.255.255.192` | VLAN 20 |
| GigabitEthernet0/2 | `10.0.0.190` | `255.255.255.192` | VLAN 30 |

The interfaces were enabled using `no shutdown`.

### Switch VLAN Configuration

The final `show vlan brief` evidence shows:

| VLAN | Name Shown | Status | Assigned Ports |
|---|---|---|---|
| 10 | `VLAN0010` | active | `Gig0/1`, `Fa3/1`, `Fa4/1` |
| 20 | `VLAN0020` | active | `Gig1/1`, `Fa5/1`, `Fa6/1` |
| 30 | `VLAN0030` | active | `Gig2/1`, `Fa7/1`, `Fa8/1` |

The lab topology labels the VLANs as:

```text
VLAN10 → Engineering
VLAN20 → HR
VLAN30 → Sales
```

However, the final `show vlan brief` screenshot still displays the default VLAN names `VLAN0010`, `VLAN0020`, and `VLAN0030`. Therefore, renaming the VLANs to `Engineering`, `HR`, and `Sales` is **not confirmed as completed** in the provided lab evidence.

### Configuration Steps

#### 1. Configure PC IP Addressing

PC1 was configured with:

```text
IP Address:      10.0.0.1
Subnet Mask:     255.255.255.192
Default Gateway: 10.0.0.62
```

The other PC addresses are shown in the Packet Tracer topology.

#### 2. Configure R1 Interfaces

R1 was configured with one interface for each VLAN.

```text
enable
configure terminal

interface g0/0
ip address 10.0.0.62 255.255.255.192
no shutdown

interface g0/1
ip address 10.0.0.126 255.255.255.192
no shutdown

interface g0/2
ip address 10.0.0.190 255.255.255.192
no shutdown
```

The screenshots show the interfaces changing to an up state after `no shutdown`.

#### 3. Configure SW1 Access Ports

The Engineering PC ports were configured as access ports in VLAN 10:

```text
interface range F3/1, F4/1
switchport mode access
switchport access vlan 10
```

The HR PC ports were configured in VLAN 20:

```text
interface range F5/1, F6/1
switchport mode access
switchport access vlan 20
```

The Sales PC ports were configured in VLAN 30:

```text
interface range F7/1, F8/1
switchport mode access
switchport access vlan 30
```

#### 4. Verify VLAN Assignments

The switch configuration was checked using:

```text
do show vlan brief
```

The final output confirms that the relevant interfaces were assigned to VLANs 10, 20, and 30.

#### 5. Test Connectivity

PC1 was used to ping PC6:

```text
ping 10.0.0.130
```

The provided evidence shows that connectivity was eventually successful.

### Commands Practiced

| Command | Device | Purpose |
|---|---|---|
| `enable` | R1 | Enter privileged EXEC mode |
| `configure terminal` | R1 | Enter global configuration mode |
| `interface g0/0` | R1 | Enter GigabitEthernet0/0 configuration mode |
| `interface g0/1` | R1 | Enter GigabitEthernet0/1 configuration mode |
| `interface g0/2` | R1 | Enter GigabitEthernet0/2 configuration mode |
| `ip address 10.0.0.62 255.255.255.192` | R1 | Configure VLAN 10 gateway |
| `ip address 10.0.0.126 255.255.255.192` | R1 | Configure VLAN 20 gateway |
| `ip address 10.0.0.190 255.255.255.192` | R1 | Configure VLAN 30 gateway |
| `no shutdown` | R1 | Enable the interface |
| `interface range F3/1, F4/1` | SW1 | Select Engineering interfaces |
| `interface range F5/1, F6/1` | SW1 | Select HR interfaces |
| `interface range F7/1, F8/1` | SW1 | Select Sales interfaces |
| `switchport mode access` | SW1 | Configure access mode |
| `switchport access vlan 10` | SW1 | Assign interfaces to VLAN 10 |
| `switchport access vlan 20` | SW1 | Assign interfaces to VLAN 20 |
| `switchport access vlan 30` | SW1 | Assign interfaces to VLAN 30 |
| `do show vlan brief` | SW1 | Verify VLANs and assigned ports |
| `ping 10.0.0.130` | PC1 | Test connectivity to PC6 |

### Verification and Results

#### R1 Interface Verification

The R1 configuration screenshot shows the three configured interfaces:

```text
G0/0 → 10.0.0.62/26
G0/1 → 10.0.0.126/26
G0/2 → 10.0.0.190/26
```

The interfaces were enabled using `no shutdown`, and the screenshot shows the interfaces and line protocols changing to an up state.

#### VLAN Verification

The final `show vlan brief` output confirms:

```text
VLAN 10 → Gig0/1, Fa3/1, Fa4/1
VLAN 20 → Gig1/1, Fa5/1, Fa6/1
VLAN 30 → Gig2/1, Fa7/1, Fa8/1
```

All three VLANs are shown as active.

#### PC1 to PC6 Ping

PC1 was used to ping:

```text
10.0.0.130
```

The captured result was:

```text
Packets: Sent = 4, Received = 3, Lost = 1 (25% loss)
```

The output shows one initial request timing out, followed by three successful replies:

```text
Request timed out.

Reply from 10.0.0.130
Reply from 10.0.0.130
Reply from 10.0.0.130
```

Therefore, the captured test confirms successful replies after the initial timeout, with the test reporting **25% packet loss**.

### Troubleshooting

A configuration error is directly visible in the SW1 CLI evidence.

An attempt was made to enter:

```text
interface F3/1, F4/1
```

The switch returned:

```text
% Invalid input detected at '^' marker.
```

The command was corrected to:

```text
interface range F3/1, F4/1
```

The ports were then successfully configured using the interface range.

No other specific troubleshooting procedure is documented in the provided evidence.

### Lab Evidence and Folder Structure

| File | Purpose |
|---|---|
| `Day+16+Lab+Question+VLANs+(Part+1).pkt` | Original Packet Tracer lab |
| `Day+16+Lab+Arpit+Solution.pkt` | Packet Tracer solution file |
| `01_Network_In_Question.png` | Initial topology and lab instructions |
| `02_PC1_Default_Gateway.png` | PC1 default gateway configuration |
| `03_PC1_IP_Subnet_Configuration.png` | PC1 IP address and subnet mask |
| `04_R1_Interface_Configuration_No_Shutdown.png` | R1 interface configuration |
| `05_SW1_VLAN_Configuration_VLAN_Brief.png` | SW1 VLAN configuration and verification |
| `06_PC1_To_PC6_Ping_Works.png` | PC1 to PC6 connectivity test |
| `07_Broadcast_Ping_Works_Reply_1.png` | Broadcast ping evidence |
| `08_Broadcast_Ping_Works_Reply_2.png` | Broadcast ping evidence |
| `09_PC6_PC3_Ping.png` | PC6 to PC3 ping evidence |
| `10_Full_Network_Working_Setup.png` | Final network setup |

## Consolidated Commands Practiced

### Router

```text
enable
configure terminal

interface g0/0
ip address 10.0.0.62 255.255.255.192
no shutdown

interface g0/1
ip address 10.0.0.126 255.255.255.192
no shutdown

interface g0/2
ip address 10.0.0.190 255.255.255.192
no shutdown
```

### Switch

```text
interface range F3/1, F4/1
switchport mode access
switchport access vlan 10

interface range F5/1, F6/1
switchport mode access
switchport access vlan 20

interface range F7/1, F8/1
switchport mode access
switchport access vlan 30

do show vlan brief
```

### PC

```text
ping 10.0.0.130
```

### Theory

```text
show vlan brief

vlan 10
name ENGINEERING

vlan 20
name HR

vlan 30
name SALES

interface range g1/0 - 3
switchport mode access
switchport access vlan 10
```

## Key Takeaways

1. A LAN can be considered a single broadcast domain.
2. A broadcast domain contains devices that receive broadcast frames from members of that domain.
3. VLANs provide logical Layer 2 separation between groups of devices.
4. VLANs are configured on switch interfaces.
5. Access ports belong to a single VLAN and are commonly used for end devices.
6. Switches do not directly forward traffic between different VLANs.
7. Inter-VLAN communication requires a Layer 3 device such as a router.
8. VLANs can reduce unnecessary broadcast traffic.
9. VLANs can provide logical separation between departments.
10. `show vlan brief` is useful for verifying VLAN status and port assignments.
11. `interface range` allows multiple switch interfaces to be configured together.
12. The Packet Tracer lab provided practical experience with VLAN assignment, router interface configuration, access-port configuration, VLAN verification, and connectivity testing.

## Conclusion

Day 16 covered the fundamentals of **VLANs, LANs, and broadcast domains** and provided practical experience configuring VLANs on a Cisco switch.

The Packet Tracer lab used three VLANs:

- **VLAN 10 – Engineering**
- **VLAN 20 – HR**
- **VLAN 30 – Sales**

R1 was configured with gateway addresses for the three subnets, while SW1 interfaces were assigned to the corresponding VLANs.

The configuration was verified using `show vlan brief`, and connectivity was tested using ping. The lab also demonstrated the importance of using the correct `interface range` syntax when configuring multiple switch interfaces.
