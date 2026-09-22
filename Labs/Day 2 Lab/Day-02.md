# CCNA 200-301 — Day 2: Interfaces and Cables

## Course
**Jeremy's IT Lab — CCNA 200-301**

## Overview

Day 2 of **Jeremy's IT Lab CCNA 200-301** focused on network interfaces, Ethernet cabling, UTP, fiber optics, cable selection, and basic Cisco interface configuration.

I also completed the Day 2 Packet Tracer lab, where I connected routers, switches, PCs and a server using the appropriate cables and added the required router modules for fiber connections.

## What I Learned

### Ethernet & Interfaces

- Ethernet is based on the **IEEE 802.3** standards.
- **FastEthernet** supports up to 100 Mbps.
- **GigabitEthernet** supports up to 1 Gbps.
- RJ-45 is commonly used for copper Ethernet connections.
- Network speed is measured in bits per second (Mbps, Gbps, etc.).

### UTP Cabling

**UTP (Unshielded Twisted Pair)** uses twisted wire pairs and is commonly used for Ethernet connections.

I learned the difference between:

- **Straight-through cable** — traditionally used between different device types, such as PC → Switch.
- **Crossover cable** — traditionally used between similar device types, such as Switch → Switch or Router → Router.
- **Auto MDI-X** — allows devices to automatically detect and adjust the transmit/receive pairs.

For this lab, Auto MDI-X was assumed to be disabled, so I had to select the correct cable type manually.

### Fiber Optics

I learned the basic differences between **multimode and single-mode fiber**:

- **Multimode:** wider core, shorter range, generally cheaper.
- **Single-mode:** narrower core, much longer range, generally more expensive.
- **SFP (Small Form-Factor Pluggable)** modules are used for fiber connections.

The lab also made the importance of distance clear. The topology included links of **50 m, 250 m and 3 km**, so the cable medium had to be selected accordingly.

## Network Topology

The Packet Tracer lab was built around two interconnected sections:

```text
                         R1
                       /    \
                    R2       R3
                  /   \       |
                SW1---SW2     R4
                |       |    /  \
               SW3     SW4 SW5---SW6
                |       |   |     |
               PC1     PC2 SW7   SW8
                          |       |
                         PC3     SRV1
```

### Main Connections

**Router backbone**
- R1 ↔ R2 — 50 m
- R1 ↔ R3 — 3 km
- R3 ↔ R4 — 250 m

**Left-side LAN**
- R2 ↔ SW1
- R2 ↔ SW2
- SW1 ↔ SW2
- SW1 ↔ SW3 ↔ PC1
- SW2 ↔ SW4 ↔ PC2

**Right-side LAN**
- R4 ↔ SW5
- R4 ↔ SW6
- SW5 ↔ SW6
- SW5 ↔ SW7 ↔ PC3
- SW6 ↔ SW8 ↔ SRV1

The different link distances were part of the exercise and helped demonstrate why the choice between copper, multimode fiber and single-mode fiber matters.

## Packet Tracer Lab

The completed topology included:

- 4 routers — R1 to R4
- 8 switches — SW1 to SW8
- 3 PCs
- 1 server

I practiced:

- Connecting routers, switches and end devices
- Selecting appropriate copper and fiber cables
- Adding fiber-related modules to routers
- Working with FastEthernet and GigabitEthernet interfaces
- Bringing interfaces up using Cisco IOS

### Commands Practiced

```text
enable
configure terminal
show ip interface
interface FastEthernetx/x
interface GigabitEthernetx/x
no shutdown
```

The `no shutdown` command was particularly important because an interface can be physically connected but still administratively disabled.

## Key Takeaways

The main thing I learned from Day 2 was that **network cabling is not just about connecting two devices**. The device type, interface, cable type and distance all matter.

The Packet Tracer lab helped me connect the theory with an actual network topology and gave me practical experience with Cisco interfaces and physical connections.

### Cable Selection

While completing the cabling, I considered both **distance and cost** when choosing the appropriate medium. For the **3 km R1–R3 link**, I used **single-mode fiber** because the distance is beyond the practical range of UTP and the multimode fiber standards covered in the course, while single-mode is designed for much longer distances. For the **250 m R3–R4 link**, I used **multimode fiber** because it supports this distance while being more cost-effective than single-mode; although single-mode could also support the distance, it would have been unnecessarily expensive for this link. For the remaining shorter connections, I used **UTP copper cabling**, since the distances were within the 100 m range covered in the course and UTP is generally cheaper and suitable for these connections. This made the cable selection a balance between **required distance, suitability, and cost** rather than simply using fiber everywhere.

## Lab Evidence

Screenshots included with the lab show:

- Full completed topology
- Upper and lower sections of the topology
- Router module installation for fiber connectivity

filenames of screenshots:

```text
Final_Solution_Full_View.png
Final_Solution_Upper_Part_Zoomed.png
Final_Solution_Lower_Part_Zoomed.png
Adding_Modules_SingleMode_Multimode_On_Router.png
```

### Lab Files

I have included both the original **Packet Tracer lab question file** and my **completed solution file** for reference:

- `Day+02+Lab+Question+Connecting+Devices.pkt` — Original lab question
- `Day+02+Lab+Arpit+Solution.pkt` — My completed solution

## Conclusion

Day 2 gave me a practical introduction to the **physical side of networking**. I now have a better understanding of Ethernet interfaces, copper and fiber cabling, cable selection, and basic Cisco interface commands.
