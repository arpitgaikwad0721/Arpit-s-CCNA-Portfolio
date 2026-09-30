# CCNA 200-301 – Day 10

## IPv4 Header and Routing Fundamentals

## Course
**Jeremy's IT Lab — CCNA 200-301**

## Overview

On Day 10, I studied the structure of an IPv4 packet and the purpose of the different fields in its header. I also examined packet captures in Wireshark to understand how IPv4 header information appears in actual traffic, including fragmentation and the Don’t Fragment (DF) flag.

Along with the IPv4 Header topic, I covered the **Routing Fundamentals** section from the beginning of Day 11. This introduced routing tables, connected and local routes, and how routers select a route based on a packet’s destination IP address. There was no Packet Tracer lab on this day; the learning was based on the course material and its examples.

## What I Learned

### 1. IPv4 Packet Structure

I reviewed how data is encapsulated as it moves through the OSI model. The protocol data units (PDUs) covered in the slides were:

- **Data** – the original data.
- **Segment** – data with a Layer 4 header.
- **Packet** – a Layer 3 header is added to the segment.
- **Frame** – a Layer 2 header and trailer are added to the packet.

The IPv4 header contains information that helps routers and receiving devices handle and deliver packets.

### 2. Fields of the IPv4 Header

I studied the purpose and size of the main IPv4 header fields:

- **Version (4 bits):** Identifies the IP version. IPv4 uses the value `4` (`0100`), while IPv6 uses `6` (`0110`).
- **Internet Header Length (IHL) (4 bits):** Indicates the IPv4 header length in 4-byte increments. The minimum value is 5 (20 bytes) and the maximum is 15 (60 bytes). The field is needed because the Options field can vary in length.
- **DSCP (6 bits):** Differentiated Services Code Point, used for Quality of Service (QoS), including prioritizing delay-sensitive traffic such as voice and video.
- **ECN (2 bits):** Explicit Congestion Notification, which can provide end-to-end congestion notification without dropping packets. It requires support from both endpoints and the network infrastructure.
- **Total Length (16 bits):** Specifies the total length of the IPv4 packet, including the Layer 3 header and encapsulated Layer 4 segment. It is measured in bytes and can range from 20 to 65,535.
- **Identification (16 bits):** Helps identify which original packet a fragment belongs to. Fragments from the same packet carry the same identification value.
- **Flags (3 bits):** Used to control and identify fragmentation. The reserved bit is always 0; the DF bit indicates that a packet should not be fragmented; and the MF bit indicates whether more fragments follow. The MF bit is 0 on the last fragment and on unfragmented packets.
- **Fragment Offset (13 bits):** Indicates a fragment’s position in the original packet and allows fragments to be reassembled even if they arrive out of order.
- **Time To Live (TTL) (8 bits):** Helps prevent routing loops. Each router decreases the TTL by 1, and a packet with a TTL of 0 is dropped. The slides give 64 as the recommended default TTL.
- **Protocol (8 bits):** Identifies the encapsulated Layer 4 protocol or other payload protocol. The examples covered were TCP (`6`), UDP (`17`), ICMP (`1`) and OSPF (`89`).
- **Header Checksum (16 bits):** Checks for errors in the IPv4 header. It does not check the encapsulated data; protocols such as TCP and UDP have their own checksum fields for that purpose.
- **Source and Destination IP Address (32 bits each):** Identify the sender and intended receiver of the packet.
- **Options (0–320 bits):** A rarely used, variable-length field. An IHL value greater than 5 indicates that Options are present.

### 3. Fragmentation and Wireshark

The course examples helped me connect the IPv4 fragmentation fields with packet captures. The slides explain that packets may be fragmented when they are larger than the MTU, which is usually 1500 bytes. The receiving host reassembles the fragments.

In Wireshark, I reviewed the IPv4 header details in a packet capture, including the source and destination addresses, total length, identification, flags, fragment offset, TTL, protocol and header checksum. The examples showed how fragments belonging to the same packet can be identified and how the MF bit and fragment offset provide information about fragmentation.

The course also showed ping examples using a larger packet size and the DF option. The `size 1000` example produced fragmented packets in the capture, while the DF-bit example showed that packets could not be sent successfully when fragmentation was disallowed.

### 4. Routing Fundamentals

I also studied the introductory routing material from Day 11. Routing is the process routers use to determine the path IP packets should take to reach their destinations. Routers keep known destinations in a routing table and consult it when forwarding packets.

The course introduced two methods for learning routes:

- **Dynamic routing:** Routers use routing protocols, such as OSPF, to exchange routing information automatically.
- **Static routing:** A network engineer or administrator manually configures routes.

A route provides instructions for reaching a destination: forward the packet to a next hop, send it directly if the destination is on a connected network, or receive it locally if the destination is the router’s own IP address.

### 5. Connected and Local Routes

When an IP address is configured on an interface and the interface is enabled, the router automatically adds two routes to its routing table:

- **Connected route (`C`):** A route to the network directly connected to the interface. For example, an interface configured with `192.168.1.1/24` has a connected route to `192.168.1.0/24`.
- **Local route (`L`):** A route to the exact IP address configured on the interface. In the example above, the local route is `192.168.1.1/32`. The `/32` prefix matches only that specific address.

A connected route allows the router to forward packets for destinations within the connected network out of the relevant interface. A local route tells the router that a packet addressed to its own interface IP is for itself, rather than something to forward.

### 6. Route Selection

I learned that a route matches a packet when the destination IP address falls within the network specified by that route. If multiple routes match, the router chooses the **most specific matching route**, meaning the route with the longest prefix length.

For example, a packet destined for `192.168.1.1` matches both `192.168.1.0/24` and `192.168.1.1/32`. The `/32` local route is more specific, so the router receives the packet for itself.

The routing examples also showed that a packet destined for a host on a directly connected network is sent out of the corresponding interface. If no route matches the destination, the router drops the packet. This differs from a switch, which floods frames when it does not have a matching destination MAC address entry.

## Key Concepts

| Concept | What I learned |
|---|---|
| IPv4 header | Carries information used to handle and deliver an IPv4 packet. |
| IHL | Specifies header length in 4-byte increments; values range from 5 to 15. |
| Total Length | Specifies the complete packet length in bytes. |
| Fragmentation | Splits a packet into fragments when required by the MTU; the receiving host reassembles them. |
| DF and MF | DF indicates that a packet should not be fragmented; MF indicates that more fragments follow. |
| TTL | Decreases by 1 at each router and prevents packets from circulating indefinitely. |
| Header Checksum | Checks the IPv4 header, not the encapsulated data. |
| Routing table | Stores routes to destinations known by a router. |
| Connected (`C`) route | Represents the network directly connected to an enabled interface. |
| Local (`L`) route | Represents the exact IP address configured on an interface, using a `/32` prefix. |
| Longest prefix match | Selects the most specific matching route when more than one route matches. |
| No matching route | The router drops the packet if it has no route matching the destination. |

The course slides also included Cisco CLI examples such as `show ip route`, `show ip int br`, interface IP configuration commands, and ping commands used in the demonstrations. These are recorded here as **commands shown in the course material**, not as commands I personally practiced.

## Key Takeaways

- The IPv4 header contains fields for addressing, packet length, fragmentation, packet lifetime and protocol identification.
- IHL measures header length in 4-byte increments, while Total Length measures the entire packet in bytes.
- The Identification, Flags and Fragment Offset fields help with packet fragmentation and reassembly.
- TTL is reduced at each router, and packets with a TTL of 0 are dropped.
- The IPv4 Header Checksum checks only the header; TCP and UDP have separate checksums for their encapsulated data.
- Routers use routing tables to decide how to handle packets.
- Connected and local routes are automatically added when an IP address is configured on an enabled interface.
- When multiple routes match, the router uses the longest prefix match. If no route matches, it drops the packet.

## Learning Evidence

- `Day+10+Slides+-+IPv4+Header.pdf` – Course slides covering IPv4 packet structure, header fields, fragmentation, Wireshark packet captures and the review quiz.
- `Day+11+(part+1)+Slides+-+Routing+Fundamentals.pdf` – The first part of the Day 11 course slides, which I covered on Day 10. It includes routing basics, routing-table entries, connected and local routes, route selection examples and review questions.

## Conclusion

Day 10 helped me understand the information carried in an IPv4 header and how it is used in packet delivery, especially when fragmentation is involved. Reviewing the packet captures in Wireshark made the header fields easier to relate to actual packets. Covering the introductory routing material also helped me understand how routers use connected and local routes and select a route for a packet’s destination. These topics provide a foundation for the upcoming study of static routing.
