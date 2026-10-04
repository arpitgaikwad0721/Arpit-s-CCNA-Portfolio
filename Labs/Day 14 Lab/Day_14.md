# Day 14 – Subnetting (Part 2)

## Course
**Jeremy's IT Lab — CCNA 200-301**

## Overview

Day 14 of Jeremy's IT Lab CCNA 200-301 course focused on **Subnetting (Part 2)**. The course material continued subnetting practice with Class C networks and then introduced subnetting Class B networks.

I worked through examples involving dividing networks into equal-size subnets, identifying subnet IDs and broadcast addresses, understanding the network and host portions of an IPv4 address, and using borrowed bits to determine the number of available subnets and hosts.

## What I Learned

### Subnetting Practice – Class C Networks

The course started with subnetting practice using Class C networks. One example used the `192.168.1.0/24` network and required dividing it into four subnets that could accommodate 45 hosts each.

Using `/26` provides four equal-size subnets:

- `192.168.1.0/26` → `192.168.1.0 – 192.168.1.63`
- `192.168.1.64/26` → `192.168.1.64 – 192.168.1.127`
- `192.168.1.128/26` → `192.168.1.128 – 192.168.1.191`
- `192.168.1.192/26` → `192.168.1.192 – 192.168.1.255`

The course also showed a useful way to identify the next subnet: find the broadcast address of the current subnet, then the next address becomes the network address of the following subnet.

### Subnetting Trick

The course introduced a subnetting trick based on looking at the binary value of the relevant octet and separating the **network portion** from the **host portion**.

For `/26`, the first two bits of the last octet are part of the network portion, while the remaining six bits are the host portion. The relevant place values are:

`128 64 32 16 8 4 2 1`

This makes it easier to recognize subnet boundaries such as:

- `192.168.1.0/26`
- `192.168.1.64/26`
- `192.168.1.128/26`
- `192.168.1.192/26`

The same approach was then applied to `/27`, where three bits are borrowed and the subnet boundary is based on the `32` place value.

### Creating Multiple Equal-Size Subnets

The course demonstrated that the number of borrowed bits determines how many subnets can be created.

The relationship shown in the course was:

- `2^x = number of subnets`
- `x = number of borrowed bits`
- `2^n – 2 = number of hosts`
- `n = number of host bits`

For example:

- Borrowing 1 bit → 2 subnets
- Borrowing 2 bits → 4 subnets
- Borrowing 3 bits → 8 subnets

For the `192.168.255.0/24` example, borrowing 3 bits produces `/27`, giving 8 possible equal-size subnets:

- `192.168.255.0/27`
- `192.168.255.32/27`
- `192.168.255.64/27`
- `192.168.255.96/27`
- `192.168.255.128/27`
- `192.168.255.160/27`
- `192.168.255.192/27`
- `192.168.255.224/27`

The course example only needed five of these subnets, but the `/27` prefix provides eight possible subnets.

### Identifying the Subnet of a Host

Another part of the course focused on determining which subnet a host belongs to.

For example, the course asked which subnet contains `192.168.5.57/27`. The subnet ID was identified as:

`192.168.5.32/27`

The same process was demonstrated with `192.168.29.219/29`, which belongs to:

`192.168.29.216/29`

These examples showed how the subnet boundary can be identified by examining the relevant bits of the address.

### Class C Subnets and Hosts

The course included a reference table for Class C networks:

| Prefix Length | Number of Subnets | Number of Hosts |
|---|---:|---:|
| /25 | 2 | 126 |
| /26 | 4 | 62 |
| /27 | 8 | 30 |
| /28 | 16 | 14 |
| /29 | 32 | 6 |
| /30 | 64 | 2 |
| /31 | 128 | 0 (2) |
| /32 | 256 | 0 (1) |

This helped connect the prefix length with the number of available subnets and hosts.

### Subnetting Class B Networks

The second major topic was subnetting Class B networks. The course emphasized that the process of subnetting Class A, Class B, and Class C networks is **exactly the same**. The main difference is the starting prefix length and the number of bits available for borrowing.

The course used `172.16.0.0/16` as the main Class B example and showed how increasing the prefix length increases the number of subnets.

Examples included:

- `/16` → borrowing 0 bits → cannot make any subnets
- `/17` → borrowing 1 bit → 2 subnets
- `/18` → borrowing 2 bits → 4 subnets
- `/19` → borrowing 3 bits → 8 subnets
- `/20` → borrowing 4 bits → 16 subnets
- `/21` → borrowing 5 bits → 32 subnets
- `/22` → borrowing 6 bits → 64 subnets
- `/23` → borrowing 7 bits → 128 subnets

The course also showed the corresponding subnet masks, including:

- `/17` → `255.255.128.0`
- `/18` → `255.255.192.0`
- `/19` → `255.255.224.0`
- `/20` → `255.255.240.0`
- `/21` → `255.255.248.0`
- `/22` → `255.255.252.0`
- `/23` → `255.255.254.0`

The examples continued into higher prefix lengths, showing that the same borrowing-bit method can be used throughout the Class B range.

### Choosing a Prefix Length

The course also presented questions where the required number of subnets or hosts had to be used to determine the appropriate prefix length.

For example, the course asked about creating 80 subnets from `172.16.0.0/16`. Since 64 subnets are not enough and 128 subnets are sufficient, the required prefix length is `/23`.

Another example asked for 250 subnets from `172.18.0.0/16`. The next available power of two is 256, which corresponds to borrowing 8 bits and using `/24`.

The course also included a requirement of 500 separate subnets from `172.22.0.0/16`, which requires `/25` because borrowing 9 bits provides 512 subnets.

### Class B Subnets and Hosts

The course provided the following Class B subnet and host reference table:

| Prefix Length | Number of Subnets | Number of Hosts |
|---|---:|---:|
| /17 | 2 | 32766 |
| /18 | 4 | 16382 |
| /19 | 8 | 8190 |
| /20 | 16 | 4094 |
| /21 | 32 | 2046 |
| /22 | 64 | 1022 |
| /23 | 128 | 510 |
| /24 | 256 | 254 |
| /25 | 512 | 126 |
| /26 | 1024 | 62 |
| /27 | 2048 | 30 |
| /28 | 4096 | 14 |
| /29 | 8192 | 6 |
| /30 | 16384 | 2 |
| /31 | 32768 | 0 (2) |
| /32 | 65536 | 0 (1) |

This table made it easier to select a prefix based on either the required number of subnets or the required number of hosts per subnet.

## Key Concepts

| Concept | Key Point |
|---|---|
| Borrowed bits | Borrowing bits from the host portion creates additional subnets. |
| Number of subnets | `2^x`, where `x` is the number of borrowed bits. |
| Number of hosts | `2^n – 2`, where `n` is the number of host bits. |
| Network portion | The bits covered by the prefix identify the network/subnet. |
| Host portion | The remaining bits identify hosts within the subnet. |
| Subnet ID | The network address of a particular subnet. |
| Broadcast address | The final address in a subnet's address range. |
| `/26` Class C | 4 subnets and 62 hosts per subnet. |
| `/27` Class C | 8 subnets and 30 hosts per subnet. |
| `/29` Class C | 32 subnets and 6 hosts per subnet. |
| `/20` Class B | 16 subnets and 4094 hosts per subnet. |
| `/23` Class B | 128 subnets and 510 hosts per subnet. |
| Class A, B and C subnetting | The course states that the subnetting process is exactly the same. |

## Key Takeaways

- I learned how to divide a Class C network into equal-size subnets and identify each subnet's network and broadcast range.
- I understood how the network and host portions change as bits are borrowed for subnetting.
- I learned to use the binary place values of an octet to quickly identify subnet boundaries.
- I practiced identifying the subnet ID of a host address such as `192.168.5.57/27` and `192.168.29.219/29` through the course examples.
- I understood the relationship between borrowed bits and the number of available subnets.
- I learned that subnetting Class A, Class B, and Class C networks follows the same basic process.
- I learned how to select an appropriate prefix length based on the required number of subnets or hosts.
- I became more comfortable working with Class B networks starting from a `/16` prefix.

## Learning Evidence

- **`Day+14+Slides+-+Subnetting+(Part+2).pdf`** — Main Day 14 course material covering Subnetting (Part 2), including Class C subnetting practice, subnetting tricks, subnet identification, subnet/host reference tables, Class B subnetting, worked examples, and quiz/review questions.

## Conclusion

Day 14 helped me build on the subnetting concepts from the previous course material by working through more subnetting examples and then applying the same process to Class B networks. The practice with subnet IDs, broadcast addresses, borrowed bits, prefix lengths, and subnet/host calculations made it easier for me to understand how different prefix lengths affect a network.
