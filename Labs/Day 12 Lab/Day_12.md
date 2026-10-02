# Day 12 -- Life of a Packet

## Course
**Jeremy's IT Lab — CCNA 200-301**

## Overview

Day 12 of Jeremy's IT Lab CCNA 200-301 course focused on the **life of a
packet** as it travels from a source host to a remote destination. The
lesson illustrated how ARP, routing decisions, and the forwarding of
Ethernet frames work together during end-to-end communication. It
covered the packet's path across multiple routers, including the use of
ARP to resolve the MAC address of the next-hop device.

The practical work consists of three Cisco Packet Tracer questions
involving packet transmission between PCs on different networks. The
questions focus on identifying source and destination MAC addresses at
specified points along the route. 

## What I Learned

### 1. Life of a Packet

When a host sends traffic to a remote destination, the packet is
forwarded through the network toward that destination. The route may
include multiple routers, and each router makes a forwarding decision
using its routing table.

The Day 12 example illustrates communication from **PC1
(192.168.1.1)** to **PC4 (192.168.4.1)** through routers R1, R2 and R4.
At each routed hop, the next-hop device and the outgoing interface
determine the Layer 2 frame used for the next segment.

### 2. ARP (Address Resolution Protocol)

ARP is used to discover the MAC address associated with an IPv4 address
on the local network. In the learning module, ARP is used whenever a device
needs the MAC address of the next-hop IP address before it can send an
Ethernet frame.

An ARP exchange has two messages:

-   **ARP Request:** The requesting device asks which device owns the
    target IPv4 address. The request is sent as a broadcast, using
    `ffff.ffff.ffff` as the destination MAC address in the examples
    shown.
-   **ARP Reply:** The device that owns the requested IPv4 address
    responds with its MAC address. The reply is sent as a unicast frame
    to the requester.

For example, PC1 (`192.168.1.1`) needs to reach its local gateway,
`192.168.1.254`. It broadcasts an ARP request asking for the gateway's
MAC address. R1 replies with its MAC address, `aaaa`, allowing PC1 to
address the Ethernet frame to R1.

The same process is repeated on later network segments when a router
needs to resolve the MAC address of its next hop.

### 3. Routing Table and Next-Hop Selection

A router checks its routing table to determine how to forward a packet
toward its destination network. The module shows these example
entries:

  Router   Destination network   Next hop / outgoing interface
  -------- --------------------- -------------------------------
  R1       `192.168.4.0/24`      Next hop `192.168.12.2`
  R2       `192.168.4.0/24`      Next hop `192.168.24.4`
  R4       `192.168.4.0/24`      Directly connected, `Gi0/2`

These entries illustrate how the packet is forwarded from one router to
another until it reaches the destination network. When the destination
network is directly connected, the router can forward the packet through
the corresponding interface rather than sending it to another router.

### 4. Layer 2 Addressing Across Routed Hops

The module tracks both IP addresses and MAC addresses as the
packet moves across the topology.

-   The **source and destination IP addresses** identify the original
    communicating hosts. In the PC1-to-PC4 example, they remain
    `192.168.1.1` and `192.168.4.1` as the packet passes through the
    routers.
-   The **source and destination MAC addresses** are associated with the
    current local Ethernet segment. They change as the packet is
    forwarded from one link to the next.
-   Before transmitting on a local Ethernet segment, a device uses ARP
    when it needs to learn the MAC address corresponding to the next-hop
    IPv4 address.

This distinction is important when examining a packet at different
interfaces: the IP header continues to identify the end hosts, while the
Ethernet frame addresses correspond to the current hop.

### 5. Encapsulation and De-encapsulation

The lesson introduces encapsulation and de-encapsulation as part of the
packet's journey. For transmission over an Ethernet segment, the
network-layer packet is carried inside a Layer 2 frame. At a router, the
incoming frame is processed and the packet is forwarded using a new
Layer 2 frame appropriate for the outgoing segment.

The examples in the module show this process through the changing MAC
addresses at each hop while the end-to-end IP addresses remain the same.

## Key Concepts

  -----------------------------------------------------------------------
  Concept                             Description
  ----------------------------------- -----------------------------------
  Life of a packet                    The sequence of forwarding steps
                                      taken as traffic travels from a
                                      source host to a remote
                                      destination.

  ARP                                 Address Resolution Protocol;
                                      resolves an IPv4 address to a MAC
                                      address on the local network.

  ARP request                         A broadcast message asking for the
                                      MAC address associated with a
                                      target IPv4 address.

  ARP reply                           A unicast response that provides
                                      the requested MAC address.

  Broadcast MAC address               `ffff.ffff.ffff`, shown as the
                                      destination MAC for ARP requests in
                                      the module.

  Routing table                       Information a router uses to select
                                      a route toward a destination
                                      network.

  Next hop                            The next router or device to which
                                      a packet is forwarded along a
                                      route.

  Encapsulation                       Carrying a network-layer packet
                                      inside a Layer 2 frame for
                                      transmission over a link.

  De-encapsulation                    Processing/removing the incoming
                                      Layer 2 framing so the packet can
                                      be handled and forwarded.

  Source/destination IP               End-to-end IP addressing shown in
                                      the packet as it travels through
                                      the routed path.

  Source/destination MAC              Link-local Ethernet addressing used
                                      for delivery across the current
                                      segment; updated at routed hops.

  Directly connected network          A network reachable through an
                                      interface on the router itself, as
                                      illustrated by R4's
                                      `192.168.4.0/24` route through
                                      `Gi0/2`.
                                      
  -----------------------------------------------------------------------

## Packet Tracer Labs

Three separate Packet Tracer questions were undertaken on Day 12. The
sections below record the requirements available at this stage.
Topology-specific analysis, configurations, commands, verification
results and evidence remain pending until the corresponding lab
screenshots and files are reviewed.

### Packet Tracer Lab 1 -- Day 12 Lab Question 1

#### Objective

Trace an ICMP packet sent from PC1 to PC4 through multiple routers and identify the source and destination MAC addresses at each specified point along the route.

#### Question Description

PC1 pings PC4. Using the CLI and Packet Tracer Simulation mode, inspect the packet at each of the following points and record its source and destination MAC addresses:

- **A.** PC1 → SW1
- **B.** SW1 → R1
- **C.** R1 → R2
- **D.** R2 → R3
- **E.** R3 → SW2
- **F.** SW2 → PC4

Before entering Simulation mode, ping PC4 from PC1 once to complete the ARP and MAC address learning process. Then use the CLI and Simulation mode to verify the MAC addresses at all six points.

#### Network Topology

The lab uses three routers (R1, R2 and R3) and two switches (SW1 and SW2) to connect PC1 to PC4. PC1 and PC4 belong to different IPv4 networks, and traffic passes through all three routers.

| Device / Link | Interface | IPv4 address / Network |
|---|---|---|
| PC1 | FastEthernet0 | 192.168.1.1/24 |
| SW1 | Fa0/1 (PC1), Gi0/1 (R1) | Layer 2 switch |
| R1 | Gi0/0 | 192.168.1.254/24 |
| R1–R2 | R1 Gi0/1 – R2 Gi0/0 | 192.168.12.0/24 |
| R2–R3 | R2 Gi0/1 – R3 Gi0/0 | 192.168.13.0/24 |
| R3 | Gi0/1 | 192.168.3.254/24 |
| SW2 | Gi0/1 (R3), Fa0/1 (PC4) | Layer 2 switch |
| PC4 | FastEthernet0 | 192.168.3.1/24 |

The topology also contains PC2 (192.168.1.2) and PC3 (192.168.1.3) connected to SW1, and PC5 (192.168.3.2) and PC6 (192.168.3.3) connected to SW2. The inter-router links use the 192.168.12.0/24 and 192.168.13.0/24 networks.

#### Configuration Steps

1. Used the existing Packet Tracer topology containing PC1, PC4, SW1, SW2 and routers R1, R2 and R3.
2. Used the configured IPv4 addressing and routing in the supplied solution topology.
3. Generated ICMP traffic by pinging PC4 (192.168.3.1) from PC1 (192.168.1.1) before entering Simulation mode, allowing ARP and MAC address learning to take place.
4. Switched to Simulation mode and inspected the ICMP packet as it travelled through the network.
5. Opened the CLI of the routers and switches to inspect their ARP and MAC address tables.

#### Commands Practiced

| Command | Device | Purpose |
|---|---|---|
| `ping 192.168.3.1` | PC1 | Test connectivity to PC4. |
| `ipconfig /all` | PC4 | Inspect IPv4 configuration and the physical MAC address. |
| `enable` | R1, R2, R3, SW1, SW2 | Enter privileged EXEC mode. |
| `show arp` | R1, R2, R3 | Inspect IPv4-to-MAC address mappings. |
| `show mac address-table` | SW1, SW2 | Inspect dynamically learned MAC addresses and associated switch ports. |

#### Verification and Results

The ping from PC1 (192.168.1.1) to PC4 (192.168.3.1) was successful. The PC1 command prompt showed four replies, with four packets received and zero packet loss (0%). The replies had a TTL of 125.

The ARP tables on R1, R2 and R3 and the MAC address tables on SW1 and SW2 were inspected to identify the MAC addresses used along the route. Packet Tracer Simulation mode was used to examine the ICMP packet at all six specified points.

**Source and destination MAC addresses**

| Point | Segment | Source MAC | Destination MAC |
|---|---|---|---|
| A | PC1 → SW1 | `0000.BA11.1111` | `0000.01AA.AAAA` |
| B | SW1 → R1 | `0000.BA11.1111` | `0000.01AA.AAAA` |
| C | R1 → R2 | `0000.01BB.BBBB` | `0000.01CC.CCCC` |
| D | R2 → R3 | `0000.01DD.DDDD` | `0000.01EE.EEEE` |
| E | R3 → SW2 | `0000.01FF.FFFF` | `0000.854.4444` |
| F | SW2 → PC4 | `0000.01FF.FFFF` | `0000.854.4444` |

The MAC addresses remain the same while a frame passes through a switch on the same Ethernet segment. They change when a router forwards the packet over a different segment. The source and destination IP addresses remain 192.168.1.1 and 192.168.3.1, respectively, throughout the routed path.

#### Lab Evidence and Subfolder Structure

```text
Day 12 Lab Question 1/
├── 01_Full_Network_Setup.png
├── 02_PC1_To_PC4_Ping_Analysis_At_PC1.png
├── 03_PC1_To_PC4_Ping_Analysis_At_SW1.png
├── 04_PC1_To_PC4_Ping_Analysis_At_R1.png
├── 05_PC1_To_PC4_Ping_Analysis_At_R2.png
├── 06_PC1_To_PC4_Ping_Analysis_At_R3.png
├── 07_PC1_To_PC4_Ping_Analysis_At_SW2.png
├── 08_PC1_To_PC4_Ping_Analysis_At_PC4.png
└── 09_Reached_PC4.png
```

The screenshots document the network topology, the packet's progression through the devices, the CLI address-table inspection and the successful arrival at PC4.

### Packet Tracer Lab 2 -- Day 12 Lab Question 2

#### Objective

Trace an ICMP packet sent from PC1 to PC3 within the same IPv4 network and identify the source and destination MAC addresses at the specified points along the route.

#### Question Description

PC1 pings PC3. Using the CLI and Packet Tracer Simulation mode, inspect the packet at each of the following points and record its source and destination MAC addresses:

- **A.** PC1 → SW1
- **B.** SW1 → PC3

Before entering Simulation mode, ping PC3 from PC1 once to complete the ARP and MAC address learning process. Then use the CLI and Simulation mode to verify the MAC addresses at both points.

#### Network Topology

The lab uses a Cisco 2960-24TT switch (SW1) to connect PC1, PC2 and PC3 within the same IPv4 network. PC1 and PC3 communicate through SW1 without requiring routing, as both devices belong to the 192.168.1.0/24 subnet.

| Device / Link | Interface | IPv4 address / Network |
|---|---|---|
| PC1 | FastEthernet0 | 192.168.1.1/24 |
| PC2 | FastEthernet0 | 192.168.1.2/24 |
| PC3 | FastEthernet0 | 192.168.1.3/24 |
| SW1 | Fa0/1 (PC1), Fa0/2 (PC2), Fa0/3 (PC3) | Layer 2 switch |
| SW1–R1 | SW1 Gi0/1 – R1 Gi0/0 | 192.168.1.0/24 LAN |
| R1 | Gi0/0 | 192.168.1.254/24 |

PC1 and PC3 are connected to SW1 through FastEthernet interfaces Fa0/1 and Fa0/3, respectively. SW1 also has an uplink to R1. However, traffic between PC1 and PC3 remains within the local LAN and does not pass through R1.

#### Configuration Steps

1. Used the existing Packet Tracer topology containing PC1, PC2, PC3, SW1 and R1.
2. Verified the IPv4 addressing and physical MAC addresses using `ipconfig /all` on the PCs.
3. From PC1, executed `ping 192.168.1.3` to generate ICMP traffic and complete the required ARP and MAC address learning process.
4. Used `arp -a` on PC1 to inspect the learned IP-to-MAC address mappings.
5. Opened the SW1 CLI and executed `show mac address-table` to inspect dynamically learned MAC addresses and their associated switch ports.
6. Entered Packet Tracer Simulation mode and inspected the ICMP packet as it travelled from PC1 through SW1 to PC3.


#### Commands Practiced

| Command | Device | Purpose |
|---|---|---|
| `ping 192.168.1.3` | PC1 | Test connectivity to PC3 and generate ICMP traffic. |
| `ipconfig /all` | PC1, PC3 | Inspect IPv4 configuration, subnet mask, default gateway and physical MAC address. |
| `arp -a` | PC1 | Display the ARP cache and inspect learned IP-to-MAC mappings. |
| `enable` | SW1 | Enter privileged EXEC mode. |
| `show mac address-table` | SW1 | Display dynamically learned MAC addresses and their associated switch ports. |

#### Verification and Results

The ping from PC1 (192.168.1.1) to PC3 (192.168.1.3) was successful. The captured output shows four replies, with four packets sent, four received and zero packet loss (0%).

The PC1 ARP table showed the mapping for PC3 (192.168.1.3) as `0010.1333.3333`. The default gateway (192.168.1.254) was mapped to `0000.01aa.aaaa`.

The SW1 MAC address table showed PC1's MAC address (`00D0.BA11.1111`) dynamically learned on Fa0/1 and the gateway MAC address (`0000.01aa.aaaa`) on Gi0/1.

**Source and destination MAC addresses**

| Point | Segment | Source MAC | Destination MAC |
|---|---|---|---|
| A | PC1 → SW1 | `00D0.BA11.1111` | `0010.1333.3333` |
| B | SW1 → PC3 | `00D0.BA11.1111` | `0010.1333.3333` |

At both points, the source MAC address is PC1's MAC address and the destination MAC address is PC3's MAC address. Since SW1 is forwarding the frame within the same LAN, it preserves both MAC addresses. The IP source and destination remain 192.168.1.1 and 192.168.1.3, respectively.

Packet Tracer Simulation mode showed the ICMP packet progressing from PC1 to SW1 and then to PC3. The packet reached PC3, confirming successful communication between the two hosts.

#### Lab Evidence and Subfolder Structure

```text
Day 12 Lab Question 2/
├── 01_Full_Network_Setup.png
├── 02_PC1_To_PC3_Ping_Analysis_At_PC1.png
├── 03_PC1_To_PC3_Ping_Analysis_At_SW1.png
├── 04_PC1_To_PC3_Ping_Analysis_At_PC3.png
└── 05_Reached_PC3.png
```

### Packet Tracer Lab 3 -- Day 12 Lab Question 3


#### Objective

Trace an ICMP packet sent from PC4 to PC1 across multiple routers and identify the source and destination MAC addresses at each specified point along the route.

#### Question Description

PC4 pings PC1. Using the CLI and Packet Tracer Simulation mode, inspect the packet at each of the following points and record its source and destination MAC addresses:

- **A.** PC4 → SW2
- **B.** SW2 → R3
- **C.** R3 → R2
- **D.** R2 → R1
- **E.** R1 → SW1
- **F.** SW1 → PC1

Before entering Simulation mode, ping PC1 from PC4 to complete the ARP and MAC address learning process. Then use the CLI and Simulation mode to verify the MAC addresses at each point along the route.

#### Network Topology

The lab uses two Cisco 2960-24TT switches (SW1 and SW2) and three routers (R1, R2 and R3) to connect PC1 and PC4 across multiple IPv4 networks. PC4 and PC1 belong to different subnets, so their communication requires routing through R3, R2 and R1.

| Device / Link | Interface | IPv4 address / Network |
|---|---|---|
| PC1 | FastEthernet0 | 192.168.1.1/24 |
| SW1 | Fa0/1 (PC1), Gi0/1 (R1) | Layer 2 switch |
| R1 | Gi0/0 | 192.168.1.254/24 |
| R1–R2 | R1 Gi0/1 – R2 Gi0/0 | 192.168.12.0/24 |
| R2 | Gi0/0 | 192.168.12.2/24 |
| R2–R3 | R2 Gi0/1 – R3 Gi0/0 | 192.168.23.0/24 |
| R3 | Gi0/0 | 192.168.23.3/24 |
| R3 | Gi0/1 | 192.168.3.254/24 |
| SW2 | Gi0/1 (R3), Fa0/1 (PC4) | Layer 2 switch |
| PC4 | FastEthernet0 | 192.168.3.1/24 |

PC4 is connected to SW2, which is connected to R3. R3 forwards traffic to R2, which forwards it to R1. R1 then forwards the packet through SW1 to PC1. Since PC1 and PC4 belong to different IPv4 subnets, the routers replace the Ethernet source and destination MAC addresses at each routed hop while retaining the original IP source and destination addresses.

#### Configuration Steps

1. Used the existing Packet Tracer topology containing PC1, PC4, SW1, SW2, R1, R2 and R3.
2. Verified the IPv4 addressing and physical MAC addresses using `ipconfig /all` on PC4.
3. From PC4, executed a ping command to generate ICMP traffic towards the 192.168.1.0/24 network.
4. Used `arp -a` on PC4 to inspect the learned IP-to-MAC address mappings.
5. Opened the SW2 CLI and executed `show mac address-table` to inspect dynamically learned MAC addresses and their associated switch ports.
6. Opened the CLI of R3, R2 and R1 and executed `show arp` to inspect the ARP entries associated with their interfaces and neighbouring devices.
7. Entered Packet Tracer Simulation mode and inspected the ICMP packet as it travelled from PC4 through SW2, R3, R2, R1 and SW1 to PC1.
8. Examined the Layer 2 Ethernet headers at each router and switch to identify the source and destination MAC addresses.


#### Commands Practiced

| Command | Device | Purpose |
|---|---|---|
| `ping 192.168.1.1` | PC4 | Test connectivity to PC1 and generate ICMP traffic. |
| `ipconfig /all` | PC4 | Inspect IPv4 configuration, subnet mask, default gateway and physical MAC address. |
| `arp -a` | PC4 | Display the ARP cache and inspect learned IP-to-MAC mappings. |
| `enable` | SW2, R1, R2, R3 | Enter privileged EXEC mode. |
| `show mac address-table` | SW2 | Display dynamically learned MAC addresses and their associated switch ports. |
| `show arp` | R1, R2, R3 | Display the ARP table and inspect IP-to-MAC mappings for directly connected devices. |

#### Verification and Results

The ping from PC4 to the 192.168.1.0 network was successful, with four replies received from 192.168.1.1, four packets received and zero packet loss (0%).

The PC4 configuration showed its IPv4 address as 192.168.3.1/24 and its physical MAC address as `000C.8544.4444`. The default gateway was 192.168.3.254.

The SW2 MAC address table showed PC4's MAC address (`000C.8544.4444`) dynamically learned on Fa0/1 and the router's MAC address on Gi0/1.

The ARP tables on R1, R2 and R3 showed the MAC addresses associated with their directly connected neighbours, allowing the Ethernet headers at the routed hops to be examined.

**Source and destination MAC addresses**

| Point | Segment | Source MAC | Destination MAC |
|---|---|---|---|
| A | PC4 → SW2 | `000C.8544.4444` | `0000.1FF.FFFF` |
| B | SW2 → R3 | `000C.8544.4444` | `0000.1FF.FFFF` |
| C | R3 → R2 | `0000.1EE.EEEE` | `0000.1DD.DDDD` |
| D | R2 → R1 | `0000.1CCC.CCCC` | `0000.1BBB.BBBB` |
| E | R1 → SW1 | `0000.1AAA.AAAA` | `00D0.BA11.1111` |
| F | SW1 → PC1 | `0000.1AAA.AAAA` | `00D0.BA11.1111` |

At the first routed hop, R3 replaces the incoming Ethernet header with a new one for forwarding towards R2. R2 performs the same operation when forwarding the packet to R1. R1 then creates a new Ethernet frame for delivery to PC1 through SW1.

The IP source and destination addresses remain unchanged during this process, whereas the Ethernet MAC addresses change at each routed hop. SW1 operates at Layer 2 and forwards the frame to PC1 without modifying its source or destination MAC addresses.

Packet Tracer Simulation mode showed the ICMP packet progressing from PC4 through SW2, R3, R2, R1 and SW1 towards PC1, illustrating the role of MAC addressing at each hop.

#### Lab Evidence and Subfolder Structure

```text
Day 12 Lab Question 3/
├── 01_Full_Network_Setup.png
├── 02_PC4_To_PC1_Ping_Analysis_At_PC4.png
├── 03_PC4_To_PC1_Ping_Analysis_At_SW2.png
├── 04_PC4_To_PC1_Ping_Analysis_At_R3.png
├── 05_PC4_To_PC1_Ping_Analysis_At_R2.png
├── 06_PC4_To_PC1_Ping_Analysis_At_R1.png
├── 07_PC4_To_PC1_Ping_Analysis_At_SW1.png
└── 08_PC4_To_PC1_Ping_Analysis_At_PC1.png
```


## Key Takeaways

-   ARP resolves a local next-hop IPv4 address to a MAC address.
-   ARP requests are broadcast, while ARP replies are unicast, as
    illustrated in the lesson.
-   Routers use their routing tables to select the next hop or a
    directly connected outgoing interface.
-   The source and destination IP addresses remain associated with the
    communicating hosts as the packet traverses the routed path shown in
    the lesson.
-   Ethernet source and destination MAC addresses are specific to the
    current link and are updated as the packet is forwarded between
    routed segments.
-   Packet Tracer Simulation mode can be used to inspect packet movement
    and address information at particular points in a topology.
-   The three practical questions apply these concepts to PC1-to-PC4,
    PC1-to-PC3 and PC4-to-PC1 traffic. 

## Lab Evidence and Folder Structure

The main Day 12 lab folder contains the Packet Tracer question and
solution files, a full-network setup image, and three separate question
folders. The folder structure shown in the supplied screenshot is
represented below; the contents of the three question folders remain to
be documented.

``` text
Day 12 Lab/
├── Day 12 Lab Arpit Solution.pkt
├── 01_Full_Network_Setup.png
├── Day 12 Lab Question Life of a Packet.pkt
├── Day 12 Lab Question 1/
│   └── 01_Full_Network_Setup.png
│   └── 02_......................
│   └── 03_......................
├── Day 12 Lab Question 2/
│   └── 01_Full_Network_Setup.png
│   └── 02_......................
│   └── 03_......................
└── Day 12 Lab Question 3/
│   └── 01_Full_Network_Setup.png
│   └── 02_......................
│   └── 03_......................

```

## Conclusion

Day 12 covered the life of a packet, with emphasis on ARP,
routing-table-based forwarding, encapsulation and de-encapsulation, and
the difference between end-to-end IP addressing and hop-by-hop MAC
addressing. The three Packet Tracer questions apply these concepts to
traffic between PCs on different networks. The detailed topology
observations, configuration steps, commands, verification results and
lab evidence is incorporated into the corresponding lab sections.
