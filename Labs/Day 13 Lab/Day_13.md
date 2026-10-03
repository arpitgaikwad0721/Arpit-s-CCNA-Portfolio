# Day 13 – Subnetting (Part 1)

## Course
**Jeremy's IT Lab — CCNA 200-301**

## Overview

Day 13 of Jeremy's IT Lab CCNA 200-301 course focused on the basics of **CIDR (Classless Inter-Domain Routing)** and the **process of subnetting**. I studied why the older classful IPv4 addressing system could lead to wasted IP addresses, how CIDR removed the fixed Class A, B, and C prefix requirements, and how larger networks can be divided into smaller subnets for more efficient address usage. The course also introduced CIDR notation, calculating usable addresses from the number of host bits, and the basic process of dividing a `/24` network into smaller subnets.

There was **no Packet Tracer lab on Day 13**. This day's work was theory and course material only.

## What I Learned

### IPv4 Address Classes

The course first reviewed the traditional IPv4 address classes:

| Class | First Octet Range | Prefix Length |
|---|---:|---:|
| A | 0–127 | /8 |
| B | 128–191 | /16 |
| C | 192–223 | /24 |
| D | 224–239 | — |
| E | 240–255 | — |

The first-octet binary patterns were also shown for each class. Classes A, B, and C were associated with the traditional network prefix lengths of `/8`, `/16`, and `/24`.

The course explained that the IANA assigned IPv4 addresses/networks to companies based on their size. A very large company might receive a Class A or Class B network, while a smaller company might receive a Class C network. However, this classful system could result in a large amount of wasted address space.

One example showed a point-to-point network between R1 and R2 using `203.0.113.0/24`. Although the network contains 256 addresses, the network address, broadcast address, and the two router addresses leave 252 addresses unused. Another example showed a company needing IP addressing for 5000 end hosts. A Class C network would not be large enough, so a Class B network would have to be assigned, resulting in about 60000 wasted addresses.

### CIDR (Classless Inter-Domain Routing)

CIDR was introduced by the IETF in 1993 to replace the classful addressing system. The fixed requirements of Class A = `/8`, Class B = `/16`, and Class C = `/24` were removed.

This allowed networks to use different prefix lengths instead of being restricted to the traditional class-based sizes. Larger networks could therefore be split into smaller networks, improving address efficiency. These smaller networks are called **subnetworks** or **subnets**.

### CIDR Notation and Usable Addresses

The course demonstrated how a CIDR prefix determines the number of network and host bits. For example, `203.0.113.0/24` has 24 network bits and 8 host bits.

The formula used in the course for calculating usable addresses was:

```text
2^n - 2 = usable addresses
```

where `n` is the number of host bits. The two addresses removed from the usable host count are the network address and broadcast address.

For example, a `/24` network has 8 host bits:

```text
2^8 - 2 = 254 usable addresses
```

The course then worked through progressively smaller CIDR prefixes. The examples showed the corresponding subnet masks and usable address counts:

| CIDR | Dotted Decimal Mask | Host Bits | Usable Addresses |
|---|---|---:|---:|
| /25 | 255.255.255.128 | 7 | 126 |
| /26 | 255.255.255.192 | 6 | 62 |
| /27 | 255.255.255.224 | 5 | 30 |
| /28 | 255.255.255.240 | 4 | 14 |
| /29 | 255.255.255.248 | 3 | 6 |
| /30 | 255.255.255.252 | 2 | 2 |
| /31 | 255.255.255.254 | 1 | 0 using the `2^n - 2` calculation shown |
| /32 | 255.255.255.255 | 0 | The course shows `2^0 - 2 = -1` and marks this as not applicable |

The `/30` example was also used to show a practical point-to-point network. `203.0.113.0/30` contains the address range `203.0.113.0` through `203.0.113.3`, with `.1` and `.2` shown as the two router addresses. The remaining addresses in the original `203.0.113.0/24` block can then be used in other subnets.

The `/31` example showed `203.0.113.0/31` containing `.0` and `.1`. The slide also showed a router configuration warning when a `/31` mask was used on a non-point-to-point interface and specifically noted that `/31` should be used cautiously in that situation.

### CIDR Notation Reference

The course provided a direct mapping between several dotted-decimal subnet masks and their CIDR notation:

| Dotted Decimal | CIDR |
|---|---|
| 255.255.255.128 | /25 |
| 255.255.255.192 | /26 |
| 255.255.255.224 | /27 |
| 255.255.255.240 | /28 |
| 255.255.255.248 | /29 |
| 255.255.255.252 | /30 |
| 255.255.255.254 | /31 |
| 255.255.255.255 | /32 |

### Subnetting Basics

The second major part of the lesson introduced subnetting. The course used the example of dividing the `192.168.1.0/24` network into four subnets, with each subnet needing to accommodate **45 hosts**.

The course showed how different prefix lengths affect both the number of usable host addresses and the number of possible subnets. For example:

- `/30` provides 2 usable addresses and 4 total addresses.
- `/29` provides 6 usable addresses and 8 total addresses.
- `/28` provides 14 usable addresses and 16 total addresses.
- `/27` provides 30 usable addresses and 32 total addresses.
- `/26` provides 62 usable addresses and 64 total addresses.

For the requirement of four subnets with 45 hosts each, the `/26` example provides 62 usable addresses per subnet. The course diagram therefore uses `/26` as the relevant subnet size for the `192.168.1.0/24` network.

The course also explained the basic process of finding consecutive subnet network addresses in the quiz: find the broadcast address of the current subnet, then use the next address as the network address of the next subnet, repeating the process for the remaining subnets.

### Review and Quiz

The final quiz asked for the remaining subnets after giving **Subnet 1 as `192.168.1.0/26`** within the `192.168.1.0/24` network. The hint was to find the broadcast address of Subnet 1, use the next address as the network address of Subnet 2, and repeat the same process for Subnets 3 and 4.

The final review slide confirmed that Day 13 covered:

- **CIDR (Classless Inter-Domain Routing)**
- **The process of subnetting (basics)**

## Key Concepts

| Concept | What I Learned |
|---|---|
| IPv4 Address Classes | Traditional Class A, B, C, D, and E ranges and the first-octet patterns. |
| Classful Addressing | Class A, B, and C traditionally used `/8`, `/16`, and `/24` prefixes. |
| CIDR | Removed the fixed classful prefix requirements and allowed more flexible network sizes. |
| Subnet / Subnetwork | A smaller network created by dividing a larger network. |
| CIDR Notation | Prefix length such as `/24`, `/26`, `/30`, etc., identifies the network portion of an IPv4 address. |
| Host Bits | The bits remaining after the network prefix; they determine the number of addresses available in a network. |
| Usable Address Formula | `2^n - 2`, where `n` is the number of host bits, as used in the course examples. |
| Network Address | The address representing the subnet itself. |
| Broadcast Address | The final address in the subnet, shown in the course examples as unavailable for normal host assignment. |
| Subnetting | Dividing a larger network into smaller networks to use address space more efficiently. |
| /30 | 2 usable addresses; shown as a suitable size for the point-to-point example. |
| /26 | 62 usable addresses; used in the subnetting example requiring 45 hosts per subnet. |

## Key Takeaways

- The traditional classful IPv4 addressing system could cause significant IP address wastage.
- CIDR replaced the fixed Class A, B, and C prefix requirements with flexible prefix lengths.
- A subnet is a smaller network created by dividing a larger network.
- The number of usable IPv4 addresses can be calculated using `2^n - 2`, where `n` is the number of host bits in the examples covered.
- Increasing the CIDR prefix length reduces the number of host bits and therefore reduces the number of usable host addresses.
- The subnet mask and CIDR notation represent the same network boundary in different forms.
- A `/30` network provides 2 usable addresses and was demonstrated for a point-to-point connection.
- A `/26` network provides 62 usable addresses and was used for the example requiring four subnets with 45 hosts each.
- The basic subnetting process involves determining subnet ranges and using the next address after a subnet's broadcast address as the network address of the next subnet.

## Learning Evidence

- **`Day_13.md.pdf`** — My learnings form main day 13 course material from Jeremy's IT Lab. The topic covers IPv4 address classes, CIDR, CIDR notation, usable address calculations, subnetting basics, worked examples a subnetting and the final review.
- 

## Conclusion

Day 13 helped me understand why CIDR and subnetting are important for efficient IPv4 address usage. I learned how CIDR removes the limitations of classful addressing, how to calculate usable addresses from a prefix length, and how a larger network can be divided into smaller subnets. The subnetting examples and quiz also helped me start understanding how to determine subnet sizes and consecutive subnet ranges.
