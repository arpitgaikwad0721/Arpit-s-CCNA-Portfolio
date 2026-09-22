# CCNA 200-301 — Day 2: Interfaces and Cables

## Overview

This document records my learning and hands-on work from **Day 2 of Jeremy's IT Lab — CCNA 200-301**.

Day 2 focused on **network interfaces, Ethernet, copper cabling, fiber-optic cabling, cable selection, interface types, and practical device-to-device connections**.

I also completed the associated **Packet Tracer Day 2 lab**, where I built the topology, selected appropriate cables based on device type and link distance, added the required fiber modules to routers, and brought the interfaces up using Cisco IOS commands.

---

## 1. What I Learned

### Network Interfaces

A network device can have different types of physical interfaces depending on the type of connection it supports.

The main interfaces I worked with in Packet Tracer included:

- **FastEthernet** — 100 Mbps-class Ethernet interface
- **GigabitEthernet** — 1 Gbps-class Ethernet interface
- **RJ-45 Ethernet ports** — commonly used with copper UTP cabling
- **Fiber interfaces / SFP-based connections** — used with fiber-optic cabling

A major practical lesson was that **having a router or switch is not enough**. The device must also have the appropriate physical interface/module for the cable and connection being used.

---

## 2. Ethernet Basics

Ethernet is a collection of network protocols and standards. In this lesson, the focus was mainly on **Ethernet cabling and the physical connections used by Ethernet networks**.

### IEEE 802.3

Ethernet standards are defined under the **IEEE 802.3 family of standards**.

Some important Ethernet standards covered were:

| Speed | Common Name | IEEE Standard | Informal Name | Maximum Length |
|---|---|---|---|---:|
| 10 Mbps | Ethernet | 802.3i | 10BASE-T | 100 m |
| 100 Mbps | Fast Ethernet | 802.3u | 100BASE-T | 100 m |
| 1 Gbps | Gigabit Ethernet | 802.3ab | 1000BASE-T | 100 m |
| 10 Gbps | 10 Gig Ethernet | 802.3an | 10GBASE-T | 100 m |

This helped me connect the **interface speed, Ethernet standard, cable type, and maximum supported distance**.

---

## 3. Bits, Bytes and Network Speed

I revised the basic relationship between bits and bytes:

- **8 bits = 1 byte**
- Network speed is normally expressed in **bits per second**, not bytes per second.
- Examples:
  - Kbps = kilobits per second
  - Mbps = megabits per second
  - Gbps = gigabits per second

The lesson also covered:

- 1 Kb = 1,000 bits
- 1 Mb = 1,000,000 bits
- 1 Gb = 1,000,000,000 bits
- 1 Tb = 1,000,000,000,000 bits

---

# 4. RJ-45 and UTP Cabling

## RJ-45

RJ-45 is the common connector associated with copper Ethernet connections.

In the lab, RJ-45 ports were used for Ethernet connections between devices such as:

- Routers
- Switches
- PCs
- Servers

## UTP

**UTP = Unshielded Twisted Pair**

UTP cables contain twisted wire pairs. The twisting helps reduce the effects of **electromagnetic interference (EMI)**.

The course covered the following general relationship:

- **10BASE-T / 100BASE-T** → 2 pairs / 4 wires
- **1000BASE-T / 10GBASE-T** → 4 pairs / 8 wires

---

# 5. Straight-Through vs Crossover Cables

One of the most important practical concepts from Day 2 was understanding **when the transmit and receive pins need to be crossed**.

## Straight-Through Cable

A straight-through cable keeps the corresponding pin numbers connected directly.

For the older 10/100 Mbps pin arrangement covered in the course:

- Transmit pins: **1 and 2**
- Receive pins: **3 and 6**

The device on one side transmits to the receive pins of the device on the other side.

Typical examples:

- PC → Switch
- Router → Switch
- Firewall → Switch

## Crossover Cable

A crossover cable swaps the transmit and receive pairs.

It is required when two devices of the same type have the same transmit/receive pin arrangement and do not support Auto MDI-X.

Examples from the course:

- Router → Router
- Switch → Switch
- PC → PC

For older devices:

**Transmit 1,2 → Receive 3,6**

and the connection crosses these pairs at the other end.

---

# 6. Auto MDI-X

**Auto MDI-X** allows a device to automatically detect how the neighboring device is using its transmit and receive pairs and adjust accordingly.

This means that modern Auto MDI-X-capable devices can often work correctly even when the cable type would traditionally have required a crossover cable.

However, for this Packet Tracer lab, the instructions specifically asked me to **assume Auto MDI-X is disabled or unsupported**.

Therefore, I had to deliberately select the appropriate cable type rather than relying on automatic detection.

This made the lab useful for understanding the actual reason behind straight-through and crossover cabling.

---

# 7. Fiber-Optic Cabling

Fiber-optic cables transmit data using **light** rather than electrical signals.

I learned that fiber is useful when:

- The link distance is greater than typical UTP limits.
- EMI resistance is important.
- Higher-distance backbone connections are required.
- A network needs connections between buildings or across longer distances.

## SFP Transceiver

**SFP = Small Form-Factor Pluggable**

SFP transceivers allow compatible network devices to use fiber-optic connections.

In the Packet Tracer lab, I worked with router modules that provide fiber interfaces.

---

# 8. Single-Mode vs Multimode Fiber

### Multimode Fiber

Multimode fiber has a **wider core** and allows multiple modes/angles of light to travel through the fiber.

Key points from the course:

- Wider core than single-mode fiber
- Supports longer distances than UTP
- Shorter maximum distance than single-mode fiber
- Generally cheaper than single-mode fiber
- Uses less expensive LED-based transmitters in the course material

### Single-Mode Fiber

Single-mode fiber has a **narrower core** and allows light to travel in a single mode.

Key points from the course:

- Narrower core than multimode fiber
- Uses a laser-based transmitter
- Supports longer distances than UTP and multimode fiber
- More expensive than multimode fiber because of the more expensive laser-based transmitters

---

# 9. Fiber Standards and Distance

The course covered these examples:

| Standard | IEEE | Speed | Fiber Type | Maximum Length |
|---|---|---:|---|---:|
| 1000BASE-LX | 802.3z | 1 Gbps | Multimode / Single-mode | 550 m (MM) / 5 km (SM) |
| 10GBASE-SR | 802.3ae | 10 Gbps | Multimode | 400 m |
| 10GBASE-LR | 802.3ae | 10 Gbps | Single-mode | 10 km |
| 10GBASE-ER | 802.3ae | 10 Gbps | Single-mode | 30 km |

This gave me a practical way to choose fiber based on **distance and cost**.

---

# 10. UTP vs Fiber

| Feature | UTP | Fiber-Optic |
|---|---|---|
| Cost | Lower | Higher |
| Maximum distance | Shorter, around 100 m for the Ethernet examples covered | Much longer |
| EMI | Can be affected by EMI | Not vulnerable to EMI |
| Connector/interface cost | RJ-45 is cheaper | SFP interfaces are more expensive |
| Long-distance links | Limited | Well suited |
| Security characteristic covered in course | Can emit a faint signal outside the cable | Does not emit the same type of signal outside the cable |

The main practical takeaway was:

> **Do not select a cable only because it is cheaper. The cable must support the required distance and physical environment.**

---

# 11. Hands-On Packet Tracer Lab

## Lab Objective

The Day 2 lab required me to connect the network devices according to the provided labels and use the **appropriate type of cable**.

The topology contained:

- **4 routers:** R1, R2, R3, R4
- **8 switches:** SW1 through SW8
- **3 PCs:** PC1, PC2, PC3
- **1 server:** SRV1

The lab also included different link distances so that cable selection had to be considered rather than simply using one cable type everywhere.

---

## 12. Topology Built

The completed Packet Tracer topology contained two main sections connected through the router backbone.

### Left-side network

```text
                 R1
                /  \
             R2     R1-R3
            /  \
          SW1--SW2
          |      |
         SW3    SW4
          |      |
         PC1    PC2
```

The actual Packet Tracer topology included:

- R1 ↔ R2
- R2 ↔ SW1
- R2 ↔ SW2
- SW1 ↔ SW2
- SW1 ↔ SW3
- SW3 ↔ PC1
- SW2 ↔ SW4
- SW4 ↔ PC2

### Right-side network

```text
                 R3
                  |
                  R4
                /   \
              SW5---SW6
              |       |
             SW7     SW8
              |       |
             PC3     SRV1
```

The actual topology included:

- R3 ↔ R4
- R4 ↔ SW5
- R4 ↔ SW6
- SW5 ↔ SW6
- SW5 ↔ SW7
- SW7 ↔ PC3
- SW6 ↔ SW8
- SW8 ↔ SRV1

---

# 13. Important Link Distances in the Lab

The topology explicitly showed different distances on the router-to-router connections:

| Link | Distance |
|---|---:|
| R1 ↔ R2 | 50 meters |
| R1 ↔ R3 | 3 kilometers |
| R3 ↔ R4 | 250 meters |

These distances were important because the appropriate physical medium depends on the maximum supported cable distance.

For example:

- A 50 m connection can fall within the UTP distance range covered in the lesson.
- A 250 m connection exceeds the 100 m UTP limit, so fiber is appropriate.
- A 3 km connection requires a fiber solution capable of supporting that distance; the course material identifies single-mode fiber as suitable for multi-kilometer links.

---

# 14. Router Module Installation

A practical part of the lab was adding the required modules to the routers before making the fiber connections.

I used the **Physical** view in Packet Tracer and worked with the available router modules, including modules for:

- Single-mode fiber
- Multimode/fiber connectivity
- Gigabit Ethernet connectivity

The module-selection screen helped me understand that network interfaces are not always fixed. Depending on the router model, additional interfaces can be provided through hardware modules.

### Practical workflow

1. Open the router.
2. Go to the **Physical** tab.
3. Identify the available module slots.
4. Select the required interface module.
5. Install the appropriate module.
6. Return to the logical topology.
7. Use the newly available interface for the required connection.

The module installation was an important part of the lab because the required fiber connection could not simply be made without the appropriate physical interface.

---

# 15. Cisco IOS Interface Configuration

The lab also gave me hands-on practice with basic Cisco IOS interface commands.

## Enter privileged EXEC mode

```text
enable
```

This changes the router from user EXEC mode to privileged EXEC mode.

## Enter global configuration mode

```text
configure terminal
```

or:

```text
conf t
```

This allows configuration changes to be made.

## Check IP interface information

```text
show ip interface
```

This can be used to inspect the IP-related status and configuration of router interfaces.

---

# 16. Selecting an Interface

The interface command depends on the actual interface installed on the router.

### FastEthernet

```text
interface FastEthernetx/x
```

Example:

```text
interface FastEthernet0/0
```

### GigabitEthernet

```text
interface GigabitEthernetx/x
```

Example:

```text
interface GigabitEthernet0/0
```

`x/x` is a placeholder. The actual interface number must be taken from the router in Packet Tracer.

---

# 17. Bringing an Interface Up

A newly configured or administratively disabled interface can be enabled using:

```text
no shutdown
```

A typical configuration flow is:

```text
enable
configure terminal
interface GigabitEthernet0/0
no shutdown
```

or for FastEthernet:

```text
enable
configure terminal
interface FastEthernet0/0
no shutdown
```

The key lesson here was that **physically connecting a cable does not automatically mean the router interface is administratively enabled**.

---

# 18. Cable Selection Logic Used in the Lab

Because the lab asked me to assume Auto MDI-X was disabled or unsupported, I had to think about the device types.

### Typical connections

| Connection | Traditional cable choice |
|---|---|
| PC → Switch | Straight-through |
| Router → Switch | Straight-through |
| Router → Router | Crossover |
| Switch → Switch | Crossover |
| PC → PC | Crossover |
| Router/Switch → Fiber device | Appropriate fiber cable/module |

For the long-distance router links, fiber was selected based on the distance requirements.

The important point was not memorizing a cable name in isolation, but understanding **why the cable is required for that particular pair of devices and link distance**.

---

# 19. What the Packet Tracer Lab Demonstrated

The completed lab gave me practical experience with the physical side of networking rather than only studying the theory.

I practiced:

- Identifying different network interfaces
- Understanding FastEthernet and GigabitEthernet interfaces
- Identifying RJ-45 Ethernet connections
- Understanding UTP cabling
- Choosing straight-through vs crossover cables
- Understanding why Auto MDI-X changes cable requirements
- Understanding fiber-optic connections
- Distinguishing single-mode and multimode fiber
- Considering cable distance before selecting a medium
- Adding router interface modules
- Connecting routers, switches, PCs and a server
- Checking interfaces in Packet Tracer
- Entering Cisco IOS configuration modes
- Selecting specific FastEthernet/GigabitEthernet interfaces
- Using `no shutdown` to bring interfaces up

---

# 20. Key Lessons I Took Away

### 1. Cable selection depends on the devices

The correct cable is determined by the type of devices being connected, especially when Auto MDI-X is unavailable.

### 2. Distance matters

UTP Ethernet connections covered in this lesson have a maximum length of about **100 meters**. Longer links require an appropriate fiber solution.

### 3. Fiber is not one single category

There is a practical difference between:

- Multimode fiber — shorter range and generally lower cost
- Single-mode fiber — longer range and generally higher cost

### 4. Interfaces matter as much as cables

A router must have the correct physical interface/module to support the selected connection.

### 5. A connected interface may still be down

Cisco IOS configuration is required when an interface is administratively shut down.

```text
no shutdown
```

### 6. Packet Tracer helps connect theory with physical networking

The lab made the concepts of interfaces, modules, cable types and distances much easier to understand because I had to actually select and connect the components.

---

# 21. Day 2 Quick Revision Sheet

```text
RJ-45
→ Common connector for copper Ethernet

UTP
→ Unshielded Twisted Pair

10BASE-T / 100BASE-T
→ 2 pairs / 4 wires

1000BASE-T / 10GBASE-T
→ 4 pairs / 8 wires

Straight-through
→ Traditionally used between different device types
→ Example: PC ↔ Switch

Crossover
→ Traditionally used between similar device types
→ Example: Switch ↔ Switch

Auto MDI-X
→ Automatically detects and adjusts transmit/receive pairs

Multimode Fiber
→ Wider core
→ Multiple light modes
→ Shorter range than single-mode
→ Lower cost

Single-mode Fiber
→ Narrower core
→ Single mode
→ Longer range
→ Higher cost

SFP
→ Small Form-Factor Pluggable

UTP
→ Around 100 m for the Ethernet standards covered

Cisco IOS
→ enable
→ configure terminal
→ interface FastEthernetx/x
→ interface GigabitEthernetx/x
→ no shutdown
→ show ip interface
```

---

# 22. Evidence of Hands-On Work

The Day 2 work was completed in Cisco Packet Tracer.

The recorded work includes:

- **Full topology view** showing the completed network.
- **Zoomed upper section** showing the router backbone and switch connections.
- **Zoomed lower section** showing the access switches, PCs and server.
- **Router module installation** showing the addition/selection of fiber-related modules.

Suggested filenames for storing the evidence in the GitHub repository:

```text
01-full-topology.png
02-upper-topology.png
03-lower-topology.png
04-router-modules.png
```

---

# 23. Conclusion

Day 2 gave me a practical foundation in the **physical side of computer networking**.

Instead of only learning that networks use cables, I learned how to decide **which cable and interface should be used, why the choice matters, how distance affects the medium, and how different physical interfaces are added and enabled on network devices**.

The Packet Tracer assignment reinforced these concepts by requiring me to build the topology myself, install the required router modules, select appropriate copper and fiber connections, and work with Cisco IOS interface commands.

This lab was therefore a useful step from basic networking theory toward actually understanding how network devices are physically connected and prepared for communication.
