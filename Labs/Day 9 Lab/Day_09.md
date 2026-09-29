# Day 09 – Interface Configuration

## Course
**Jeremy's IT Lab — CCNA 200-301**

## Overview

Day 09 of Jeremy's IT Lab CCNA 200-301 course focused on **interface configuration** and the practical administration of Cisco network devices. The theory covered the purpose of configuring and managing device interfaces, while the Packet Tracer lab provided hands-on practice with hostnames, IPv4 addressing, interface descriptions, speed and duplex settings, and administratively disabling unused interfaces.

The practical activity used one router, two switches, and four PCs connected in a single IPv4 network. Configuration and verification were performed through the Cisco IOS CLI and the Packet Tracer end-device configuration interface.

## What I Learned

- How to configure device hostnames so that routers and switches can be identified easily in the CLI.
- How to assign an IPv4 address and subnet mask to a router interface and bring the interface up with `no shutdown`.
- How to configure static IPv4 addresses on PCs using Packet Tracer's **Config** interface.
- How interface descriptions help document the purpose and destination of a connection.
- How speed and duplex settings affect Ethernet links, and how to configure these properties on interfaces.
- How to administratively disable unused interfaces with `shutdown`.
- How to use `show interface status` on switches to inspect port names, link state, VLAN, duplex, speed, and interface type.
- How to inspect a router's saved configuration with `show startup-config` and verify interface configuration details.

## Key Concepts

### 1. Interface Configuration

A network interface is the point at which a device connects to another device or network. Cisco IOS provides interface configuration mode to configure individual router and switch ports.

A typical interface configuration begins by entering privileged EXEC mode, moving to global configuration mode, and selecting the interface:

```text
enable
configure terminal
interface GigabitEthernet0/0
```

The interface name must match the hardware interface available on the device. Interface configuration commands apply to the selected interface until the CLI exits that mode.

### 2. Hostname Configuration

A hostname gives a router or switch a recognizable name. It appears in the command prompt and makes it easier to identify the device being configured.

```text
configure terminal
hostname R1
```

The lab used the hostnames `R1`, `SW1`, and `SW2` for the router and the two switches, respectively.

### 3. IPv4 Address and Subnet Mask

An IPv4 address identifies a device's Layer 3 interface. The subnet mask determines which portion of the address represents the network and which portion represents the host.

The lab used the address space **172.16.0.0/16**, equivalent to the subnet mask `255.255.0.0`. The router's connected interface was configured with `172.16.255.254/16`, while the four PCs used host addresses `172.16.0.1` through `172.16.0.4`.

On a router, an IPv4 address is assigned in interface configuration mode:

```text
interface GigabitEthernet0/0
ip address 172.16.255.254 255.255.0.0
no shutdown
```

The `no shutdown` command removes the administrative shutdown state. A router interface that is enabled can still remain down if the physical link or the connected device is not operational.

The PCs were configured with static IPv4 addresses and the `255.255.0.0` subnet mask through their Packet Tracer configuration windows.

### 4. Interface Descriptions

An interface description is a text label that documents the purpose of a port or the device connected to it. Descriptions are useful when troubleshooting or reviewing a configuration, particularly when a device has multiple interfaces.

Example from the lab's configuration style:

```text
interface GigabitEthernet0/0
description ## To SW1 ##
```

The lab used descriptions to identify connected devices and marked unused interfaces as disabled in their descriptions. A description is administrative documentation; it does not change how packets are forwarded.

### 5. Ethernet Speed and Duplex

Ethernet interfaces have speed and duplex properties.

- **Speed** determines the link's data rate, such as 10 Mbps, 100 Mbps, or 1000 Mbps.
- **Half duplex** allows transmission in one direction at a time.
- **Full duplex** allows a device to transmit and receive simultaneously.
- **Auto-negotiation** allows connected Ethernet interfaces to agree on supported speed and duplex settings.

The lab instructions required speed and duplex to be configured manually on interfaces connected to other networking devices, rather than on interfaces connected to end hosts. The switch verification output displayed the negotiated link information for the connected ports, including full-duplex 100 Mbps FastEthernet links and full-duplex 1000 Mbps GigabitEthernet links.

### 6. Administrative Shutdown

The `shutdown` command administratively disables an interface. It is commonly used on unused ports to prevent them from being active unnecessarily. The `no shutdown` command re-enables an interface.

```text
interface GigabitEthernet0/2
shutdown
```

The lab required interfaces that were not connected to another device to be disabled. The switch status outputs showed unused FastEthernet ports and the unused GigabitEthernet port in the disabled state.

### 7. Interface Status Verification

The `show interface status` command provides a concise overview of switch ports. Its output includes the port, description/name, status, VLAN, duplex, speed, and interface type.

It can be used to check whether connected ports are operational and whether unused ports have been disabled. In the lab, the outputs from both switches were used to verify the configured port descriptions and the status of the connected and unused interfaces.

### 8. Running and Startup Configurations

Cisco IOS maintains configuration information in running and startup configurations:

- **Running configuration** is the active configuration currently held in RAM.
- **Startup configuration** is the saved configuration used when the device reloads.

The `show startup-config` command displays the saved configuration. The router screenshot showed the saved interface configuration, including the description and IPv4 address on `GigabitEthernet0/0`, along with the default speed and duplex settings shown in the output.

## Packet Tracer Lab

### Lab Objective

The Packet Tracer activity involved configuring a small switched network and verifying the interface settings. The tasks shown in the lab instructions were:

1. Configure the hostnames of `R1`, `SW1`, and `SW2`.
2. Configure the appropriate IP addresses on `R1`, `PC1`, `PC2`, `PC3`, and `PC4`.
3. Manually configure speed and duplex on interfaces connected to other networking devices (not end hosts).
4. Configure appropriate descriptions on each interface.
5. Disable interfaces that are not connected to other devices.

### Addressing Plan

The topology was labelled with the network `172.16.0.0/16`. The host addresses visible in the topology and PC configuration screenshots were:

| Device | Interface | IPv4 address | Subnet mask |
|---|---|---:|---:|
| R1 | GigabitEthernet0/0 | `172.16.255.254` | `255.255.0.0` |
| PC1 | FastEthernet0 | `172.16.0.1` | `255.255.0.0` |
| PC2 | FastEthernet0 | `172.16.0.2` | `255.255.0.0` |
| PC3 | FastEthernet0 | `172.16.0.3` | `255.255.0.0` |
| PC4 | FastEthernet0 | `172.16.0.4` | `255.255.0.0` |

The screenshots show static IPv4 configuration on the PCs. No default gateway values are included here because they are not clearly established by the supplied configuration evidence.

### Configuration Performed

#### Router R1

The router was renamed `R1`. Its `GigabitEthernet0/0` interface was assigned `172.16.255.254` with a `255.255.0.0` subnet mask and enabled using `no shutdown`. The saved configuration screenshot also shows a description identifying the connection to SW1.

```text
enable
configure terminal
hostname R1
interface GigabitEthernet0/0
description ## To SW1 ##
ip address 172.16.255.254 255.255.0.0
no shutdown
```

The startup configuration shown in the screenshot lists `duplex auto` and `speed auto` for this interface.

#### Switch SW1

The first switch was renamed `SW1`. Its connected ports were identified by descriptions: `FastEthernet0/1` to PC1, `FastEthernet0/2` to PC2, `GigabitEthernet0/1` to R1, and `GigabitEthernet0/2` to SW2.

The `show interface status` output showed the two PC-facing FastEthernet ports as connected at full duplex and 100 Mbps. Both GigabitEthernet uplinks were also shown as connected, full duplex, and 1000 Mbps. Unused FastEthernet ports were disabled.

#### Switch SW2

The second switch was renamed `SW2`. Its connected ports were identified as `FastEthernet0/1` to PC3, `FastEthernet0/2` to PC4, and `GigabitEthernet0/1` to SW1. The `show interface status` output showed the PC-facing FastEthernet ports connected at full duplex and 100 Mbps, and the GigabitEthernet uplink connected at full duplex and 1000 Mbps. Unused ports, including `GigabitEthernet0/2`, were disabled.

#### PCs

PC1, PC2, PC3, and PC4 were configured with static IPv4 addresses through the Packet Tracer **Config** tab. Each PC used the `255.255.0.0` subnet mask, matching the `/16` network shown in the topology.

### Network Topology

The following diagram represents the devices and interface connections shown in the supplied Packet Tracer screenshots.

```text
                         172.16.0.0/16

 R1                         SW1                              SW2
+---------+              +---------+                    +---------+
|  2911   | G0/0    G0/1 |2960-24TT| G0/2          G0/1 |2960-24TT|
|         |--------------|         |--------------------|         |
+---------+              +----+----+                    +----+----+
                              |                              |
                         F0/1 |     F0/2                F0/1 | F0/2
                              |      |                       |   |
                            +---+   +--+                 +---+  +---+
                           | PC1   | PC2 |              | PC3  | PC4 |
                           +-------+-----+              +------+-----+

R1 G0/0: 172.16.255.254/16
PC1:     172.16.0.1/16
PC2:     172.16.0.2/16
PC3:     172.16.0.3/16
PC4:     172.16.0.4/16
```

### Commands Practiced

| Command | Purpose |
|---|---|
| `enable` | Enters privileged EXEC mode. |
| `configure terminal` | Enters global configuration mode. |
| `hostname R1` | Sets the router hostname to `R1`; corresponding hostnames were set on the switches. |
| `interface GigabitEthernet0/0` | Enters configuration mode for the specified router interface. |
| `ip address 172.16.255.254 255.255.0.0` | Assigns the router interface its IPv4 address and subnet mask. |
| `description ## To SW1 ##` | Documents the purpose of the interface. |
| `no shutdown` | Administratively enables an interface. |
| `shutdown` | Administratively disables an interface. |
| `duplex auto` | Sets an interface to negotiate duplex automatically. |
| `speed auto` | Sets an interface to negotiate speed automatically. |
| `show ip interface brief` | Displays a concise summary of interface IP addresses and operational states. |
| `show interface status` | Displays switch-port status, description, VLAN, duplex, speed, and type. |
| `show startup-config` | Displays the saved configuration used at startup. |

## Lab Files

The following Packet Tracer files were included with the Day 09 lab materials:

Day_09/
│
├── Day_09.md
│
├── Day+09+Lab+Question+Interface+Configuration.pkt
├── Day+09+Lab+Arpit+Solution.pkt
│
├── 01_Full_Network_Setup.png
├── 02_Hostname_To_R1.png
├── 03_Hostname_To_SW1.png
├── 04_Hostname_To_SW2.png
├── 05_R1_IP_Address_Assigned_No_Shutdown.png
├── 06_PC1_IP_Configurations.png
├── 07_PC2_IP_Configurations.png
├── 08_PC3_IP_Configurations.png
├── 09_PC4_IP_Configurations.png
├── 10_SW1_Show_Interface_Status.png
├── 11_SW2_Show_Interface_Status.png
└── 12_R1_Startup_Config_Shows_Desc.png

The supplied screenshots document the network setup, hostname and IP configuration, switch interface status, and the router's startup configuration.

## Key Takeaways

- Hostnames and interface descriptions make device configurations easier to identify and maintain.
- Router interfaces require appropriate Layer 3 addressing and must be administratively enabled when they are intended to be active.
- End devices can be configured with static IPv4 addresses through Packet Tracer's configuration interface.
- Speed and duplex settings are important Ethernet interface properties, and their operational state can be checked through switch commands.
- Administratively disabling unused interfaces helps keep the device configuration intentional.
- Verification commands such as `show interface status` and `show startup-config` help confirm interface state and review saved configuration.

## Conclusion

Day 09 combined interface-configuration concepts with a practical Packet Tracer exercise. The lab provided experience configuring hostnames, IPv4 addressing, interface descriptions, speed and duplex settings, and unused-port shutdowns across a router and two switches. Reviewing switch interface status and the router's startup configuration also reinforced the importance of verifying device configuration after completing the setup.
