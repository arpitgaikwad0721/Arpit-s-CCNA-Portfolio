# Day 18 – VLAN Fundamentals (Revision of Day 16)

> **Course:** Jeremy's IT Lab – CCNA 200-301  
> **Day:** 18  
> **Topic:** VLANs (Virtual Local Area Networks) – Fundamentals  
> **Learning Focus:** Revisiting and learning the first half of Day 16 content

## Overview

On Day 18, I revisited the first part of the VLAN topic covered on Day 16 of Jeremy's IT Lab CCNA 200-301 course. The focus was on understanding LANs, broadcast domains, VLANs, why VLANs are used, and how VLANs differ from IP subnets. I also reviewed the basic purpose of access ports and the command used to check VLAN assignments on a Cisco switch.

This record covers the theory and fundamental concepts only. It does not document the full Day 16 Packet Tracer lab or claim that the complete VLAN configuration exercise was repeated.

## What I Learned

- A **LAN (Local Area Network)** can be understood as a single broadcast domain.
- A **broadcast domain** includes the devices that receive a broadcast frame sent by a member of that domain.
- A **VLAN (Virtual Local Area Network)** logically separates devices at Layer 2.
- VLANs are configured on a switch on a per-interface basis.
- Each VLAN forms a separate Layer 2 broadcast domain.
- A switch does not directly forward traffic between different VLANs.
- Communication between different VLANs requires a Layer 3 device, such as a router.
- VLANs can reduce unnecessary broadcast traffic by limiting the scope of broadcasts.
- VLANs can logically separate departments or groups of devices for administrative and security purposes.
- Different IP subnets alone do not automatically create separate Layer 2 broadcast domains; VLAN configuration provides that Layer 2 separation.
- An **access port** belongs to one VLAN and is commonly used to connect an end device such as a PC.
- The `show vlan brief` command displays VLANs and their assigned switch ports.

## Key Concepts

### 1. LAN and Broadcast Domain

A LAN is a group of devices in a local network. In the context of this lesson, a LAN can be described as a single broadcast domain.

A broadcast domain is the group of devices that receive a broadcast frame sent by a member of that domain. The Ethernet broadcast destination MAC address is:

```text
FFFF.FFFF.FFFF
```

Routers separate broadcast domains because they do not forward Layer 2 broadcast frames from one interface to another.

### 2. VLANs

A VLAN is a logical separation of devices at **Layer 2**. VLANs allow a switch-based network to be divided into separate broadcast domains.

For example:

| Department | VLAN |
|---|---:|
| Engineering | VLAN 10 |
| HR | VLAN 20 |
| Sales | VLAN 30 |

Devices in different VLANs are logically separated even when they are connected to the same physical switch.

### 3. VLANs and IP Subnets

IP subnets operate at Layer 3, while VLANs provide separation at Layer 2. Using different IP subnets does not, by itself, guarantee that devices belong to different Layer 2 broadcast domains.

For example, these are separate IPv4 subnets:

- Engineering: `192.168.1.0/26`
- HR: `192.168.1.64/26`
- Sales: `192.168.1.128/26`

VLANs must be configured appropriately to separate these groups into different Layer 2 broadcast domains.

### 4. Communication Between VLANs

A switch does not directly route traffic between VLANs. When devices in different VLANs need to communicate, traffic must pass through a Layer 3 device, such as a router.

```text
VLAN 10 → Router / Layer 3 Device → VLAN 20
```

The Layer 3 device can route traffic between the associated IP networks, provided the necessary addressing and routing configuration is in place.

### 5. Access Ports

An access port is assigned to a single VLAN and is commonly used to connect an end device.

Example configuration:

```text
switchport mode access
switchport access vlan 10
```

The first command configures the interface as an access port. The second assigns it to VLAN 10.

### 6. Purpose of VLANs

**Performance:** VLANs divide a network into separate broadcast domains, helping limit unnecessary broadcast traffic.

**Logical separation:** VLANs can separate groups such as Engineering, HR, and Sales. Traffic between VLANs must pass through a Layer 3 device, where suitable routing and security policies can be applied.

## Commands Reviewed

| Command | Purpose |
|---|---|
| `show vlan brief` | Displays VLANs, their status, and assigned switch ports |
| `switchport mode access` | Configures a switch interface as an access port |
| `switchport access vlan 10` | Assigns an access port to VLAN 10 |

## Key Takeaways

1. A broadcast domain contains devices that receive broadcast frames from members of that domain.
2. VLANs provide logical Layer 2 separation and create separate broadcast domains.
3. Different IP subnets do not automatically provide Layer 2 VLAN separation.
4. Access ports belong to a single VLAN and are commonly used for end devices.
5. A Layer 3 device is required for communication between different VLANs.
6. `show vlan brief` is useful for checking VLANs and their port assignments.

## Conclusion

Day 18 focused on revisiting the first half of the VLAN fundamentals from Day 16. I reviewed LANs, broadcast domains, the purpose of VLANs, the difference between VLANs and IP subnets, access ports, and basic VLAN verification. This revision strengthened my understanding of the concepts before moving on to more detailed VLAN configuration and practical exercises.
