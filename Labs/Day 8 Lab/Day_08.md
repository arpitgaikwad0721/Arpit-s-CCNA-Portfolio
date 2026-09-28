# CCNA 200-301 – Day 8: IPv4 Addressing (Part 2)

## Course
**Jeremy's IT Lab — CCNA 200-301**

## Overview

Day 8 of Jeremy's IT Lab CCNA 200-301 course focused on **IPv4 Addressing (Part 2)**. I continued learning how IPv4 addresses are divided into network and host portions, how to calculate the number of usable hosts, and how to identify the network address, broadcast address, and usable host range.

Along with the theory, I completed a **Cisco Packet Tracer lab** in which I configured three interfaces on a router, manually assigned IPv4 addresses to three PCs, verified the configurations, saved the router configuration, and tested connectivity using `ping`.

## What I Learned

### 1. IPv4 Address Classes

I reviewed the traditional IPv4 address classes and how they are identified using the first octet.

| Class | First-octet range | Leading bits | Default prefix |
|---|---:|---|---|
| A | 0–127 | `0` | `/8` |
| B | 128–191 | `10` | `/16` |
| C | 192–223 | `110` | `/24` |
| D | 224–239 | `1110` | — |
| E | 240–255 | `1111` | — |

Classes A, B, and C were traditionally used for unicast addressing. Class D is used for multicast, while Class E is reserved for experimental purposes.

The default prefixes of Classes A, B, and C are part of the historical classful addressing system. Modern IPv4 networks generally use **Classless Inter-Domain Routing (CIDR)**, where the prefix length specifies the network portion.

### 2. Maximum Number of Hosts

I learned how to calculate the maximum number of usable host addresses in a subnet using the number of host bits.

```text
Maximum usable hosts = 2^h - 2
```

Here, `h` represents the number of host bits. For an ordinary IPv4 subnet, two addresses are excluded:

- **Network address:** The address with all host bits set to `0`.
- **Broadcast address:** The address with all host bits set to `1`.

Examples covered in the course:

| Network | Prefix | Host bits | Total addresses | Usable hosts |
|---|---:|---:|---:|---:|
| `192.168.1.0` | `/24` | 8 | 256 | 254 |
| `172.16.0.0` | `/16` | 16 | 65,536 | 65,534 |
| `10.0.0.0` | `/8` | 24 | 16,777,216 | 16,777,214 |

This helped me understand how a prefix length determines the number of addresses available within a network.

### 3. Network, Broadcast and Usable Addresses

I studied how to find the important addresses within an IPv4 network.

- **Network address:** Identifies the network. The host portion contains all `0` bits.
- **Broadcast address:** Used to reach all hosts on the subnet. The host portion contains all `1` bits.
- **First usable address:** The address immediately after the network address.
- **Last usable address:** The address immediately before the broadcast address.

The examples from the slides included:

| Network | Network address | First usable | Last usable | Broadcast address |
|---|---|---|---|---|
| `192.168.1.0/24` | `192.168.1.0` | `192.168.1.1` | `192.168.1.254` | `192.168.1.255` |
| `172.16.0.0/16` | `172.16.0.0` | `172.16.0.1` | `172.16.255.254` | `172.16.255.255` |
| `10.0.0.0/8` | `10.0.0.0` | `10.0.0.1` | `10.255.255.254` | `10.255.255.255` |

To calculate these values, I need to identify the network and host bits using the prefix length. The network and broadcast addresses are reserved for their respective purposes and are not assigned to ordinary hosts.

### 4. Configuring IPv4 Addresses on Cisco Devices

The lesson also covered how to configure IPv4 addresses on Cisco router interfaces using Cisco IOS commands.

The general configuration process is:

1. Enter privileged EXEC mode using `enable`.
2. Enter global configuration mode using `configure terminal`.
3. Select the required interface.
4. Assign an IPv4 address and subnet mask using `ip address`.
5. Add an interface description, if required.
6. Enable the interface using `no shutdown`.
7. Verify the interface status and configuration using show commands.

Router interfaces are administratively down by default in the lab environment, so I used `no shutdown` to enable them. I also learned to use `show ip interface brief` for a quick interface-status check and `show running-config` to review the active configuration.

## Key Concepts

| Concept | What I Learned |
|---|---|
| IPv4 address class | A traditional way of grouping IPv4 addresses based on the first octet. |
| Host bits | The bits available for identifying devices within a subnet. |
| Maximum usable hosts | Calculated as `2^h - 2` for ordinary IPv4 subnets, where `h` is the number of host bits. |
| Network address | The address with all host bits set to `0`; it identifies the subnet. |
| Broadcast address | The address with all host bits set to `1`; it is used to reach all hosts on the subnet. |
| First usable address | The address immediately after the network address. |
| Last usable address | The address immediately before the broadcast address. |
| Interface description | A text label that helps identify the purpose of a router interface. |
| `no shutdown` | Administratively enables a router interface. |
| Interface status | The Layer 1 and Layer 2 state displayed by Cisco verification commands. |

## Packet Tracer Lab

### Lab Objective

The objective of the lab was to configure the IPv4 addresses of the router interfaces and end devices according to the supplied topology, enable the interfaces, verify the settings, save the configuration, and test connectivity between the PCs.

### Network Topology

The lab contained **R1**, three switches (**SW1, SW2, and SW3**) and three PCs (**PC1, PC2, and PC3**). Each PC was connected to a separate LAN through a switch, with R1 providing the Layer 3 interface for each network.

```text
PC1 ───── SW1 ───── R1 G0/0
                      │
PC2 ───── SW2 ───── R1 G0/1
                      │
PC3 ───── SW3 ───── R1 G0/2
```

The topology uses three separate IPv4 networks. R1 has a dedicated Gigabit Ethernet interface for each LAN.

### IP Addressing

| Network | Device/interface | IPv4 address | Subnet mask |
|---|---|---|---|
| `15.0.0.0/8` | PC1 | `15.0.0.1` | `255.0.0.0` |
| `15.0.0.0/8` | R1 `GigabitEthernet0/0` | `15.255.255.254` | `255.0.0.0` |
| `182.98.0.0/16` | PC2 | `182.98.0.1` | `255.255.0.0` |
| `182.98.0.0/16` | R1 `GigabitEthernet0/1` | `182.98.25.254` | `255.255.0.0` |
| `201.191.20.0/24` | PC3 | `201.191.20.1` | `255.255.255.0` |
| `201.191.20.0/24` | R1 `GigabitEthernet0/2` | `201.191.20.254` | `255.255.255.0` |

### Router Configuration

I first checked the router interfaces and changed the hostname to `R1`. I then configured the three Gigabit Ethernet interfaces with their respective IPv4 addresses and subnet masks. I added descriptions and enabled each interface with `no shutdown`.

The main configuration commands were:

```text
enable
configure terminal
hostname R1

interface GigabitEthernet0/0
 ip address 15.255.255.254 255.0.0.0
 description ## 15.255.255.254 Up ##
 no shutdown
 exit

interface GigabitEthernet0/1
 ip address 182.98.25.254 255.255.0.0
 description ## 182.98.25.254 Up ##
 no shutdown
 exit

interface GigabitEthernet0/2
 ip address 201.191.20.254 255.255.255.0
 description ## 201.191.20.254 Up ##
 no shutdown
 exit
```

I used `show ip interface brief` to check the interface IP addresses and operational status. I also reviewed the running configuration and saved it to startup configuration.

```text
show ip interface brief
show running-config
copy running-config startup-config
```

### PC Configuration

I manually assigned the IPv4 address and subnet mask to each PC using the **IP Configuration** window in Packet Tracer.

| PC | IPv4 address | Subnet mask |
|---|---|---|
| PC1 | `15.0.0.1` | `255.0.0.0` |
| PC2 | `182.98.0.1` | `255.255.0.0` |
| PC3 | `201.191.20.1` | `255.255.255.0` |

After assigning the addresses, I used `ipconfig` on each PC to verify the IPv4 settings.

### Connectivity Testing

After configuring R1 and the PCs, I tested connectivity from PC1 to PC2 and PC3 using `ping`.

```text
ping 182.98.0.1
ping 201.191.20.1
```

The ping screenshot documents the connectivity tests from PC1 to both destination PCs. This provided a practical check that the addressing and router interface configuration were in place for communication between the three networks.

### Commands Practiced

| Command | Purpose |
|---|---|
| `enable` | Enters privileged EXEC mode. |
| `configure terminal` | Enters global configuration mode. |
| `hostname R1` | Changes the router hostname to `R1`. |
| `show ip interface brief` | Displays interface IP addresses and their status. |
| `interface GigabitEthernet0/0` | Selects R1's first Gigabit Ethernet interface. |
| `interface GigabitEthernet0/1` | Selects R1's second Gigabit Ethernet interface. |
| `interface GigabitEthernet0/2` | Selects R1's third Gigabit Ethernet interface. |
| `ip address <address> <subnet-mask>` | Assigns an IPv4 address and subnet mask to an interface. |
| `description <text>` | Adds a description to an interface. |
| `no shutdown` | Administratively enables the selected interface. |
| `exit` | Leaves the current configuration context. |
| `show running-config` | Displays the active configuration. |
| `copy running-config startup-config` | Saves the running configuration to startup configuration. |
| `ipconfig` | Displays the PC's IP configuration in Packet Tracer. |
| `ping <destination>` | Tests IP connectivity to a destination address. |

## Key Takeaways

- The number of host bits determines the number of usable IPv4 addresses in a subnet.
- For ordinary IPv4 subnets, the maximum usable host count is `2^h - 2`.
- The network and broadcast addresses have special purposes and are not assigned to ordinary hosts.
- The subnet mask and prefix length determine which part of an IPv4 address represents the network.
- Router interfaces need appropriate IP addresses and subnet masks for their connected networks.
- Cisco router interfaces must be enabled with `no shutdown` when they are administratively down.
- `show ip interface brief` is useful for quickly checking interface addresses and status.
- `show running-config` and `copy running-config startup-config` help review and save the router configuration.
- Correct IP addressing on the PCs and router interfaces can be checked with `ipconfig` and `ping`.

## Lab Evidence

The following files document the Day 8 theory and Packet Tracer work. The filenames are retained as provided.

| File | What it demonstrates |
|---|---|
| `Day+08+Slides+-+IPv4+Addressing+(Part+2).pdf` | Course slides covering IPv4 address classes, host-count calculations, network and broadcast addresses, usable host ranges, and Cisco IP address configuration. |
| `Day+08+Lab+Question+IPv4+Addresses.pkt` | The original Packet Tracer lab question file. |
| `Day+08+Lab+Arpit+Solution.pkt` | My completed Packet Tracer solution. |
| `Day+08+Lab+-+IPv4+Addresses.pkt` | Additional Packet Tracer lab file included with the Day 8 materials. |
| `01_Full_Network_Setup.png` | The overall Packet Tracer topology and lab setup. |
| `02_Hostname_Changed_To_R1.png` | The router hostname changed to `R1`. |
| `03_Show_Ip_Interface_Brief.png` | The initial interface summary. |
| `04_Interface_Gig00_Configuration_Show_IP_Interface_Brief.png` | Configuration and verification of `GigabitEthernet0/0`. |
| `05_Interface_Gig01_Configuration_Show_IP_Interface_Brief.png` | Configuration and verification of `GigabitEthernet0/1`. |
| `06_Interface_Gig02_Configuration_Show_IP_Interface_Brief.png` | Configuration and verification of `GigabitEthernet0/2`. |
| `07_Show_Running_Config_Hostname.png` | Verification of the router hostname in the running configuration. |
| `08_Show_Running_Config_Interface_Configurations.png` | Review of the configured router interfaces. |
| `09_Running_Config_To_Startup_Config_Save.png` | Saving the running configuration to startup configuration. |
| `10_PC1_IP_Manual_Assignment.png` | Manual IPv4 address and subnet mask assignment for PC1. |
| `11_PC1_Ipconfig.png` | Verification of PC1's IPv4 settings using `ipconfig`. |
| `12_PC2_IP_Manual_Assignment.png` | Manual IPv4 address and subnet mask assignment for PC2. |
| `13_PC2_Ipconfig.png` | Verification of PC2's IPv4 settings using `ipconfig`. |
| `14_PC3_IP_Manual_Assignment.png` | Manual IPv4 address and subnet mask assignment for PC3. |
| `15_PC3_Ipconfig.png` | Verification of PC3's IPv4 settings using `ipconfig`. |
| `16_PC1_To_PC2_And_PC3_Ping.png` | Ping tests from PC1 to PC2 and PC3. |

## Conclusion

Day 8 helped me move from understanding IPv4 address ranges and host calculations to applying IP addressing in a working network. In Packet Tracer, I configured R1's three Gigabit Ethernet interfaces, manually assigned IPv4 addresses to the PCs, checked the interface status, reviewed and saved the configuration, and tested connectivity from PC1 to PC2 and PC3. The lab reinforced how IPv4 addresses, subnet masks, and enabled router interfaces work together to support communication between networks.
