# Day 6 — Ethernet LAN Switching (Part 2)

## Overview

Day 6 of Jeremy's IT Lab CCNA 200-301 course focused on **Ethernet LAN Switching (Part 2)**. I learned how Ethernet frames are structured, how **ARP** resolves IP addresses to MAC addresses, how **ping** uses ICMP, and how switches learn and use MAC address tables.

I also practiced these concepts in **Cisco Packet Tracer** using a small LAN with two switches and four PCs. I observed ARP and ICMP traffic in Simulation Mode, checked ARP and MAC address tables, tested connectivity with `ping`, and cleared dynamically learned MAC addresses.

## What I Learned

- An Ethernet frame contains an Ethernet header, payload, and trailer. The Ethernet header and trailer together are **18 bytes**.
- The minimum Ethernet frame size is **64 bytes**, so the minimum payload size is **46 bytes**.
- If the payload is smaller than 46 bytes, **padding bytes** are added.
- **ARP (Address Resolution Protocol)** is used to discover the Layer 2 MAC address of a known Layer 3 IP address.
- ARP uses two messages:
  - **ARP Request** — broadcast to all hosts on the local network.
  - **ARP Reply** — unicast back to the host that sent the request.
- The broadcast MAC address is `FFFF.FFFF.FFFF`.
- The `arp -a` command can be used to view the ARP table.
- **Ping** is used to test network reachability and uses **ICMP Echo Request** and **ICMP Echo Reply** messages.
- A switch learns MAC addresses dynamically and stores them in its **MAC address table**.
- A known unicast frame can be forwarded directly when the destination MAC address is already known.
- Broadcast and unknown unicast frames are flooded because the switch does not have a specific destination entry for them.
- Dynamic MAC address entries can be cleared using Cisco IOS `clear mac address-table` commands.

## Key Concepts

### Ethernet Frame

The Ethernet frame consists of:

- Preamble
- SFD
- Destination MAC address
- Source MAC address
- Type
- Payload
- FCS

The Preamble and SFD are usually not considered part of the Ethernet header. The Ethernet header and trailer are **18 bytes**, and the minimum payload size is **46 bytes**.

### ARP

ARP maps a known IP address to its corresponding MAC address.

The basic process I observed was:

```text
PC1
 │
 │ ARP Request
 │ Destination MAC: FFFF.FFFF.FFFF
 ▼
Switches
 │
 │ ARP Request reaches PC3
 ▼
PC3
 │
 │ ARP Reply
 │ Destination MAC: PC1's MAC
 ▼
PC1
```

The ARP Request is broadcast, while the ARP Reply is unicast.

### ARP Table

The `arp -a` command displays:

- Internet Address — IP address
- Physical Address — MAC address
- Type — static or dynamic

Dynamic entries are learned through ARP.

### Ping and ICMP

Ping is a network utility used to test reachability and measure round-trip time.

It uses:

- ICMP Echo Request
- ICMP Echo Reply

For example:

```text
ping 192.168.1.3
```

### MAC Address Table

On a Cisco switch, the MAC address table contains:

- VLAN
- MAC Address
- Type
- Ports

The table can be viewed with:

```text
show mac address-table
```

The switch learns source MAC addresses dynamically as frames enter its interfaces.

## Packet Tracer Lab

For the Day 6 Packet Tracer lab, I worked with a LAN containing **four PCs and two Cisco 2960-24TT switches**.

The PCs were placed in the `192.168.1.0/24` network:

- PC1 — `192.168.1.1`
- PC2 — `192.168.1.2`
- PC3 — `192.168.1.3`
- PC4 — `192.168.1.4`

I started with empty ARP tables on the PCs and empty MAC address tables on the switches.

I first observed what happens when **PC1 pings PC3**. Using Packet Tracer Simulation Mode, I followed the ARP Request and ARP Reply before the ICMP communication. The ARP Request was broadcast, allowing the destination MAC address to be learned, followed by the ARP Reply and then ICMP Echo Request/Reply traffic.

I then generated additional traffic by pinging other PCs, checked the learned MAC addresses on both switches, and finally cleared the dynamic MAC address entries.

## Network Topology

```text
PC1 ── F0/1 ── SW1 ── G0/1 ── G0/1 ── SW2 ── F0/1 ── PC3
                 │                              │
               F0/2                           F0/2
                 │                              │
                PC2                            PC4

                    192.168.1.0/24
```

IP addressing shown in the lab:

```text
PC1 = 192.168.1.1
PC2 = 192.168.1.2
PC3 = 192.168.1.3
PC4 = 192.168.1.4
```

## Commands Practiced

### View the ARP table

```text
arp -a
```

Used on the PCs to view the current ARP entries.

### Test connectivity

```text
ping 192.168.1.3
```

Used to test connectivity from PC1 to PC3 and observe ICMP Echo Request/Reply traffic.

```text
ping 192.168.1.4
```

Used to test connectivity from PC1 to PC4.

### Enter privileged EXEC mode

```text
enable
```

Used on the switches before running privileged EXEC commands.

### View the MAC address table

```text
show mac address-table
```

Used on SW1 and SW2 to view dynamically learned MAC addresses and the interfaces associated with them.

### Clear all dynamic MAC addresses

```text
clear mac address-table dynamic
```

Used to remove the dynamically learned MAC addresses from the switch MAC address table.

### Clear dynamic MAC addresses for a specific interface

```text
clear mac address-table dynamic interface Gig0/0
```

The course material also demonstrated clearing dynamic MAC entries for a specific interface.

## Key Takeaways

- Ethernet frames have a minimum size of **64 bytes**, requiring a minimum payload of **46 bytes**.
- ARP resolves a known IP address to a MAC address.
- **ARP Request = broadcast**, while **ARP Reply = unicast**.
- The broadcast MAC address is `FFFF.FFFF.FFFF`.
- Ping uses **ICMP Echo Request** and **ICMP Echo Reply**.
- Switches dynamically learn MAC addresses and store them in the MAC address table.
- Known unicast frames can be forwarded directly, while broadcast and unknown unicast frames are flooded.
- `arp -a` and `show mac address-table` provide useful visibility into Layer 2 address learning.
- Dynamic MAC address entries can be cleared from a Cisco switch.

## Lab Evidence

The Day 6 folder contains the Packet Tracer lab files, course slides, topology screenshots, Simulation Mode captures, ping results, and MAC address table outputs.

### Packet Tracer Lab Files

I have included both the original **Packet Tracer lab question file** and my **completed solution file**:

- `Day+06+Lab+Question+Ethernet+LAN+Switching.pkt` — Original Packet Tracer lab question
- `Day+06+Lab+Arpit+Solution.pkt` — My completed Packet Tracer solution

### Course Material

- `Day+06+Slides+-+Ethernet+LAN+Switching+(Part+2).pdf` — Day 6 course slides covering Ethernet frames, ARP, ARP tables, ping/ICMP, MAC address tables, and clearing dynamic MAC addresses.

### Topology and ARP Evidence

- `01_Full_Network_Setup.png` — Shows the complete Packet Tracer network topology with PC1, PC2, PC3, PC4, SW1, SW2, and the `192.168.1.0/24` network.
- `02_PC1_Arp_Table_Empty.png` — Shows PC1 with an empty ARP table before generating traffic.
- `03_ICMP_ARP_PDU_Creation_As_Soon_As_Ping.png` — Shows the ARP and ICMP PDUs created when the ping was initiated.
- `04_ICMP_Layer2_Info_Arp_Creation.png` — Shows the Layer 2 information and ARP process for the ICMP communication.
- `05_Arp_PDU_Info_Broadcast_Destination_FFFF.png` — Shows the ARP Request using `FFFF.FFFF.FFFF` as the destination MAC address.
- `06_Complete_Arp_Request_Reply_Block.png` — Shows the completed ARP Request/Reply exchange in Simulation Mode.
- `07_ICMP_Request_Reply_Blocks_Working_Fine_4_For_Windows.png` — Shows the ICMP request and reply blocks in the simulation.
- `08_Ping_PC3_From_PC1.png` — Shows the successful ping from PC1 to PC3.
- `09_Ping_PC4_From_PC1.png` — Shows the successful ping from PC1 to PC4.

### MAC Address Table Evidence

- `10_Switch_2_SW2_Mac_Address_Table.png` — Shows the dynamically learned MAC addresses on SW2.
- `11_Switch_1_SW1_Mac_Address_Table.png` — Shows the dynamically learned MAC addresses on SW1.
- `12_Clear_Show_Mac_Address_Table_SW1_Switch_1.png` — Shows the MAC address table on SW1 after clearing the dynamic entries.
- `13_Clear_Show_ Mac_Address_Table_SW2_Switch_2.png` — Shows the MAC address table on SW2 after clearing the dynamic entries.

## Conclusion

Day 6 helped me connect the theory of ARP, ICMP, Ethernet frames, and MAC address learning with actual packet behavior in Packet Tracer. I practiced generating traffic, observing ARP and ICMP messages in Simulation Mode, checking ARP and MAC address tables, and clearing dynamically learned MAC addresses. This gave me a clearer understanding of how switches learn MAC addresses and how ARP and ICMP work together during communication on a LAN.
