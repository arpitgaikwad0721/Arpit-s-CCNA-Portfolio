# CCNA 200-301 – Day 7: IPv4 Addressing

## Course
**Jeremy's IT Lab — CCNA 200-301**

## Course
**Jeremy's IT Lab — CCNA 200-301**

## Overview

Day 7 of Jeremy's IT Lab CCNA 200-301 course focused on **IPv4 Addressing (Part 1)**. I studied how IPv4 addresses are structured, how to convert between binary and decimal, and how network and host portions are identified.

The session also covered IPv4 address classes, prefix lengths, subnet masks, network addresses, and broadcast addresses. I focused on understanding how IPv4 addressing works and how it is used to identify devices and networks.

There was no Packet Tracer lab for this session. The day was dedicated to understanding the concepts through the course slides, examples, and review questions.

## What I Learned

### 1. Network Layer and Routing

I started by reviewing the **Network Layer (Layer 3) of the OSI model** and its role in communication between devices on different networks.

- It provides connectivity between end hosts on different networks.
- It uses logical addressing through IP addresses.
- It determines paths between source and destination networks.
- Routers operate at Layer 3 and are responsible for forwarding packets between networks.

I also reviewed how devices on the same network communicate through switches and how a router is needed when communication involves different networks.

### 2. IPv4 Address Structure

IPv4 addresses are **32 bits (4 bytes)** long and are represented in dotted-decimal notation.

An IPv4 address is divided into four 8-bit sections called **octets**. Each octet is represented by a decimal number ranging from 0 to 255.

For example:

- IPv4 address: `192.168.1.254`
- Binary representation: `11000000.10101000.00000001.11111110`

Each octet represents 8 bits, making a total of 32 bits.

I learned how dotted-decimal notation makes IPv4 addresses easier to read compared to their binary representation.

### 3. Decimal, Binary and Hexadecimal

A significant part of this session was dedicated to understanding number systems, particularly binary and decimal conversions.

**Binary (Base 2)** uses only two digits, `0` and `1`. Each bit represents a power of 2, with the positions in an octet having the following values:

`128, 64, 32, 16, 8, 4, 2, 1`

For example:

- `192` in binary is `11000000`.
- `168` in binary is `10101000`.
- `1` in binary is `00000001`.
- `254` in binary is `11111110`.

I also studied how to convert binary values back into decimal by adding the positional values wherever the corresponding bit is `1`.

For example:

`11000000 = 128 + 64 = 192`

The slides also introduced hexadecimal (Base 16), using the digits `0–9` and letters `A–F`. I reviewed how hexadecimal values are calculated using powers of 16.

These conversions are important for understanding IPv4 addresses and identifying their network and host portions.

### 4. Network and Host Portions

I learned that an IPv4 address can be divided into two parts:

- **Network portion:** Identifies the network to which the address belongs.
- **Host portion:** Identifies a particular device within that network.

The prefix length determines how many bits belong to the network portion, while the remaining bits belong to the host portion.

For example, in `192.168.1.254/24`, the first 24 bits represent the network portion, and the remaining 8 bits represent the host portion.

The slides illustrated how different prefix lengths divide the same 32-bit IPv4 address into network and host portions.

### 5. IPv4 Address Classes

I studied the traditional IPv4 address classes and how they are identified by their first octet.

| Class | First Octet Range | Default Prefix |
|---|---:|---:|
| A | 0–127 | `/8` |
| B | 128–191 | `/16` |
| C | 192–223 | `/24` |
| D | 224–239 | — |
| E | 240–255 | — |

The main points I learned were:

- **Class A, B and C** are associated with different default network and host portions.
- **Class D** is used for multicast addresses.
- **Class E** is reserved for experimental purposes.

I also reviewed how the leading bits of the first octet identify each class.

The examples in the slides included `12.128.251.23/8`, `154.78.111.32/16` and `192.168.1.254/24`, illustrating the different class-based divisions.

### 6. Prefix Lengths and Netmasks

I studied how prefix lengths represent the number of network bits in an IPv4 address and how they correspond to subnet masks.

The course covered the following default classful prefix lengths and their corresponding subnet masks:

| Class | Prefix Length | Subnet Mask |
|---|---|---|
| A | `/8` | `255.0.0.0` |
| B | `/16` | `255.255.0.0` |
| C | `/24` | `255.255.255.0` |

A subnet mask consists of consecutive `1` bits for the network portion, followed by `0` bits for the host portion.

For example, the `/24` prefix corresponds to the following subnet mask:

`255.255.255.0`

In binary:

`11111111.11111111.11111111.00000000`

Understanding the relationship between prefix lengths and subnet masks helped me understand how the network and host portions are separated.

### 7. Network Addresses

I learned that a **network address** identifies an entire network rather than an individual device.

The host portion of a network address consists entirely of `0` bits. A network address cannot be assigned to an individual host.

For example, in the network `192.168.1.0/24`, the address `192.168.1.0` is the network address.

The slides used network diagrams to illustrate how a network address represents the network shared by multiple devices.

### 8. Broadcast Addresses

A **broadcast address** is used to send traffic to all hosts on a particular network.

The host portion of a broadcast address consists entirely of `1` bits. Like a network address, a broadcast address cannot be assigned to an individual host.

For example, the broadcast address of `192.168.1.0/24` is `192.168.1.255`.

The course also illustrated the use of the broadcast destination IP address `192.168.1.255` alongside the Ethernet broadcast MAC address `FFFF.FFFF.FFFF`.

### 9. Loopback Addresses

I also studied IPv4 loopback addresses and their purpose.

- The loopback address range is `127.0.0.0` to `127.255.255.255`.
- Loopback addresses are used to test the network stack on the local device.
- They allow testing without requiring communication with another device.

The slides demonstrated the use of `127.0.0.1` and another address within the loopback range to illustrate local testing.

## Key Concepts

| Concept | What I Learned |
|---|---|
| Network Layer | Layer 3 of the OSI model, responsible for logical addressing and path selection. |
| IPv4 | A 32-bit logical addressing system. |
| Octet | An 8-bit section of an IPv4 address. |
| Dotted-decimal notation | The standard representation of IPv4 addresses using four decimal octets. |
| Binary | A base-2 number system used to represent individual IPv4 address bits. |
| Hexadecimal | A base-16 number system introduced as another way of representing values. |
| Network portion | The bits that identify the network. |
| Host portion | The bits that identify a host within the network. |
| IPv4 address classes | Traditional address categories A, B, C, D and E. |
| Prefix length | The number of bits allocated to the network portion. |
| Netmask | A 32-bit mask that separates the network and host portions. |
| Network address | An address with all host bits set to `0`. |
| Broadcast address | An address with all host bits set to `1`. |
| Loopback | An address range used to test the local network stack. |

## Key Takeaways

- IPv4 addresses consist of 32 bits divided into four 8-bit octets.
- Binary-to-decimal and decimal-to-binary conversions are essential for understanding IPv4 addressing.
- The prefix length determines the division between the network and host portions of an IPv4 address.
- Traditional IPv4 addressing uses five classes, with Classes A, B and C having default prefix lengths of `/8`, `/16` and `/24`, respectively.
- Subnet masks represent the network portion using `1` bits and the host portion using `0` bits.
- Network addresses have all host bits set to `0`, while broadcast addresses have all host bits set to `1`.
- Loopback addresses allow the local network stack to be tested without communicating with another device.

## Learning Evidence

I have uploaded the following material for Day 7:

- `Day_07.md` — My writtern notes of Jeremy's IT Lab course Day 7 covering the Network Layer, IPv4 address structure, binary and decimal conversions, address classes, prefix lengths, subnet masks, network addresses, broadcast addresses and loopback addresses.

## Conclusion

Day 7 helped me build a better understanding of IPv4 addressing, particularly how addresses are represented and divided into network and host portions. I also became more familiar with binary conversions, traditional IPv4 address classes, prefix lengths and subnet masks.

Since there was no Packet Tracer lab, I focused on understanding the theory and working through the examples provided in the course material. These concepts provide a foundation for understanding subnetting and more advanced networking topics in the upcoming sessions.
