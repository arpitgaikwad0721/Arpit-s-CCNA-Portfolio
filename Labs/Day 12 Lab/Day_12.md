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
specified points along the route. Detailed lab configurations and
evidence are reserved for later updates after the individual lab files
and screenshots are analysed.

## What I Learned

### 1. Life of a Packet

When a host sends traffic to a remote destination, the packet is
forwarded through the network toward that destination. The route may
include multiple routers, and each router makes a forwarding decision
using its routing table.

The Day 12 presentation illustrates communication from **PC1
(192.168.1.1)** to **PC4 (192.168.4.1)** through routers R1, R2 and R4.
At each routed hop, the next-hop device and the outgoing interface
determine the Layer 2 frame used for the next segment.

### 2. ARP (Address Resolution Protocol)

ARP is used to discover the MAC address associated with an IPv4 address
on the local network. In the presentation, ARP is used whenever a device
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
toward its destination network. The presentation shows these example
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

The presentation tracks both IP addresses and MAC addresses as the
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

The examples in the slides show this process through the changing MAC
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
                                      the presentation.

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

Trace a packet sent from **PC1 to PC4** and identify the source and
destination MAC addresses at each specified point along the route.

#### Question Requirements

The question asks for the source and destination MAC addresses at the
following segments:

  Point   Segment specified in the question   Required information
  ------- ----------------------------------- --------------------------------------
  A       PC1 → SW1                           Source and destination MAC addresses
  B       SW1 → R1                            Source and destination MAC addresses
  C       R1 → R2                             Source and destination MAC addresses
  D       R2 → R3                             Source and destination MAC addresses
  E       R3 → SW2                            Source and destination MAC addresses
  F       SW2 → PC4                           Source and destination MAC addresses

The question instructs the learner to use the CLI and Packet Tracer
Simulation mode to verify the answers. It also asks that a ping be
performed before entering Simulation mode to complete the ARP/MAC
learning process.

#### Network Topology

**Pending detailed lab analysis.** The exact topology and relevant
interface-level details for this question will be documented after
reviewing the lab screenshots and Packet Tracer files.

#### Configuration Steps

**Pending detailed lab analysis.** No device configuration steps are
recorded at this stage.

#### Commands Practiced

**Pending detailed lab analysis.** Commands used during the completed
lab will be added after the solution is reviewed.

#### Verification and Results

**Pending verification.** Packet Tracer Simulation observations and the
verified MAC-address values will be added after analysing the lab
evidence.

#### Lab Evidence and Subfolder Structure

``` text
Day 12 Lab Question 1/
└── [Contents to be documented after detailed analysis]
```

The exact filenames, screenshots and solution contents will be added
after the corresponding subfolder is reviewed.

### Packet Tracer Lab 2 -- Day 12 Lab Question 2

#### Objective

Trace a packet sent from **PC1 to PC3** and identify the source and
destination MAC addresses at the two specified points in the route.

#### Question Requirements

The question asks for the source and destination MAC addresses at these
segments:

  Point   Segment specified in the question   Required information
  ------- ----------------------------------- --------------------------------------
  A       PC1 → SW1                           Source and destination MAC addresses
  B       SW1 → R1                            Source and destination MAC addresses

The question instructs the learner to use the CLI and Packet Tracer
Simulation mode to verify the answers. It also asks that a ping be
performed before entering Simulation mode to complete the ARP/MAC
learning process.

#### Network Topology

**Pending detailed lab analysis.** The exact topology and relevant
interface-level details for this question will be documented after
reviewing the lab screenshots and Packet Tracer files.

#### Configuration Steps

**Pending detailed lab analysis.** No device configuration steps are
recorded at this stage.

#### Commands Practiced

**Pending detailed lab analysis.** Commands used during the completed
lab will be added after the solution is reviewed.

#### Verification and Results

**Pending verification.** Packet Tracer Simulation observations and the
verified MAC-address values will be added after analysing the lab
evidence.

#### Lab Evidence and Subfolder Structure

``` text
Day 12 Lab Question 2/
└── [Contents to be documented after detailed analysis]
```

The exact filenames, screenshots and solution contents will be added
after the corresponding subfolder is reviewed.

### Packet Tracer Lab 3 -- Day 12 Lab Question 3

#### Objective

Trace a packet sent from **PC4 to PC1** and identify the source and
destination MAC addresses at each point specified in the question.

#### Question Requirements

The available question text asks for the source and destination MAC
addresses at the specified points along the route to PC1. The exact
point-by-point list is not fully available in the supplied question
view, so it is left for confirmation during detailed lab analysis rather
than inferred.

#### Network Topology

**Pending detailed lab analysis.** The exact topology and relevant
interface-level details for this question will be documented after
reviewing the lab screenshots and Packet Tracer files.

#### Configuration Steps

**Pending detailed lab analysis.** No device configuration steps are
recorded at this stage.

#### Commands Practiced

**Pending detailed lab analysis.** Commands used during the completed
lab will be added after the solution is reviewed.

#### Verification and Results

**Pending verification.** Packet Tracer Simulation observations and the
verified MAC-address values will be added after analysing the lab
evidence.

#### Lab Evidence and Subfolder Structure

``` text
Day 12 Lab Question 3/
└── [Contents to be documented after detailed analysis]
```

The exact filenames, screenshots and solution contents will be added
after the corresponding subfolder is reviewed.

## Commands Practiced

The supplied Day 12 presentation focuses on packet flow, ARP and routing
behaviour and does not provide IOS configuration command sequences. The
lab questions explicitly mention using `ping`, the CLI and Packet Tracer
Simulation mode.

  -------------------------------------------------------------------------
  Command / tool            Purpose                 Source
  ------------------------- ----------------------- -----------------------
  `ping <destination-IP>`   Generate connectivity   Lab questions
                            traffic to the          
                            specified destination;  
                            the lab questions ask   
                            for a ping before       
                            entering Simulation     
                            mode to complete        
                            ARP/MAC learning.       

  Packet Tracer Simulation  Observe packet movement Lab questions
  mode                      and inspect addressing  
                            information at the      
                            specified points.       

  CLI                       Use the device          Lab questions
                            command-line interface  
                            as part of verifying    
                            the answers.            
  -------------------------------------------------------------------------

Additional commands will be added here if they are identified in the
detailed analysis of the three labs.

## Key Takeaways

-   ARP resolves a local next-hop IPv4 address to a MAC address.
-   ARP requests are broadcast, while ARP replies are unicast, as
    illustrated in the presentation.
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
    PC1-to-PC3 and PC4-to-PC1 traffic. Their detailed configurations and
    verified observations will be added after the individual lab
    analyses.

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
│   └── [To be documented after detailed analysis]
├── Day 12 Lab Question 2/
│   └── [To be documented after detailed analysis]
└── Day 12 Lab Question 3/
    └── [To be documented after detailed analysis]
```

## Conclusion

Day 12 covered the life of a packet, with emphasis on ARP,
routing-table-based forwarding, encapsulation and de-encapsulation, and
the difference between end-to-end IP addressing and hop-by-hop MAC
addressing. The three Packet Tracer questions apply these concepts to
traffic between PCs on different networks. The detailed topology
observations, configuration steps, commands, verification results and
lab evidence will be incorporated into the corresponding lab sections
after each lab is analysed.
