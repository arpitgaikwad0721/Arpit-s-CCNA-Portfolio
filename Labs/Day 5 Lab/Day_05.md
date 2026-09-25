# Day 05 – Ethernet LAN Switching

## Overview

Day 5 of Jeremy's IT Lab CCNA 200-301 course focused on **Ethernet LAN Switching** and the fundamentals of Layer 2 communication. I studied the Physical and Data Link layers of the OSI model, Protocol Data Units (PDUs), the structure of an Ethernet frame, MAC addresses, hexadecimal notation, and how switches learn and use MAC address tables.

A major part of the lesson focused on understanding how a switch decides whether to **forward** or **flood** an Ethernet frame based on the destination MAC address.

## What I Learned

### OSI Model – Physical and Data Link Layers

I reviewed the responsibilities of the lower two layers of the OSI model:

- The **Physical Layer (Layer 1)** defines the physical characteristics of the medium used to transfer data, including voltage levels, transmission distances, connectors, and cable specifications.
- Digital bits are converted into electrical signals for wired connections or radio signals for wireless connections.
- The **Data Link Layer (Layer 2)** provides node-to-node connectivity and data transfer, such as PC-to-switch, switch-to-router, and router-to-router communication.
- The Data Link Layer uses **Layer 2 addressing**, which is separate from Layer 3 addressing.
- Switches operate at **Layer 2**.

I also reviewed how data is represented as it moves through the OSI layers. The Data Link Layer uses **frames**, while the Network Layer uses packets and the Transport Layer uses segments.

### Ethernet Frames

I learned the main fields of an Ethernet frame and what each field is used for:

- **Preamble** – 7 bytes consisting of alternating 1s and 0s (`10101010 * 7`). It allows devices to synchronize their receiver clocks.
- **SFD (Start Frame Delimiter)** – 1 byte (`10101011`) that marks the end of the preamble and the beginning of the rest of the frame.
- **Destination MAC Address** – Identifies the device that should receive the frame.
- **Source MAC Address** – Identifies the device that sent the frame.
- **Type / Length** – A 2-byte field. A value of 1500 or less indicates the length of the encapsulated packet, while a value of 1536 or greater indicates the type of the encapsulated packet.
- **FCS (Frame Check Sequence)** – 4 bytes used to detect corrupted data using a CRC algorithm.

The Ethernet frame header and trailer shown in the course have a combined size of **26 bytes**.

### MAC Addresses

I learned that a **MAC address** is a 6-byte (48-bit) physical address assigned to a device. It is also referred to as a **Burned-In Address (BIA)**.

A MAC address:

- Is written using **12 hexadecimal characters**.
- Is globally unique.
- Has the first 3 bytes as the **OUI (Organizationally Unique Identifier)** assigned to the company that makes the device.
- Has the last 3 bytes unique to the individual device.

I also reviewed hexadecimal notation, which uses 16 possible values:

`0–9` and `A–F`

### Switch MAC Address Learning

A key part of Day 5 was understanding how switches learn MAC addresses.

When a switch receives an Ethernet frame, it uses the **Source MAC Address** to populate its MAC address table. The switch associates that MAC address with the interface on which the frame was received.

For example, if a frame arrives through `F0/1` with a particular source MAC address, the switch can record that MAC address as being reachable through `F0/1`.

This allows the switch to learn where devices are located and make forwarding decisions.

### Known and Unknown Unicast Frames

I learned the difference between known and unknown unicast frames:

- A **known unicast frame** is a frame where the destination MAC address is already present in the switch's MAC address table. The switch can forward the frame through the appropriate interface instead of flooding it.
- An **unknown unicast frame** is destined for a single host, but the switch does not yet know which interface leads to that destination. The switch floods the frame out of all interfaces except the interface on which it was received.

The course examples showed how MAC address tables are gradually populated as frames travel between devices and switches.

Dynamic MAC addresses are removed from the MAC address table after **5 minutes of inactivity**.

## Key Concepts

| Concept | What I Learned |
|---|---|
| Layer 1 – Physical | Deals with the physical medium, signals, cables, connectors, and related characteristics |
| Layer 2 – Data Link | Provides node-to-node connectivity and uses Layer 2 addressing |
| PDU | Data is called a segment, packet, or frame depending on the OSI layer |
| Ethernet Frame | Layer 2 PDU used for Ethernet communication |
| Preamble | 7 bytes used for receiver clock synchronization |
| SFD | Marks the end of the preamble |
| MAC Address | 6-byte / 48-bit physical address |
| OUI | First 3 bytes of a MAC address assigned to the manufacturer |
| Type / Length | Indicates the packet type or its length |
| FCS | Detects corrupted data using CRC |
| MAC Address Table | Maps MAC addresses to switch interfaces |
| Source MAC | Used by a switch to learn and populate its MAC address table |
| Known Unicast | Forwarded using the destination MAC address in the MAC table |
| Unknown Unicast | Flooded out of interfaces except the receiving interface |
| Dynamic MAC Address | Learned by the switch and removed after 5 minutes of inactivity |

## Key Takeaways

- I understood the main responsibilities of the **Physical and Data Link layers**.
- I learned how data is represented as **segments, packets, and frames** at different OSI layers.
- I can now identify the important fields of an **Ethernet frame** and their purposes.
- I understood that Ethernet communication at Layer 2 uses **MAC addresses**.
- I learned the structure of a **48-bit MAC address** and the role of the OUI.
- I understood how a switch learns **Source MAC addresses** and builds its MAC address table.
- I learned the difference between **known unicast forwarding** and **unknown unicast flooding**.
- I understood why hexadecimal notation is commonly used when working with MAC addresses.

## Learning Evidence

I have uploaded this file as evidence of my learning today i.e. on Day 05:

- `Day_05.md` — Jeremy's IT Lab CCNA 200-301 Day 5 course covering Ethernet LAN Switching, the Physical and Data Link layers, Ethernet frame structure, MAC addressing, hexadecimal notation, MAC address learning, forwarding, and flooding etc.

## Conclusion

Day 5 gave me a better understanding of how **Ethernet LAN switching works at Layer 2**. The main focus was understanding Ethernet frames, MAC addresses, and how switches learn device locations and make forwarding decisions. This helped connect the OSI model concepts from earlier lessons with how a switch actually handles Ethernet frames.
