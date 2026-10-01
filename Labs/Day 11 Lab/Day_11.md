# Day 11 – CCNA 200-301

## Overview

Day 11 of Jeremy’s IT Lab CCNA 200-301 course focused on **static routing**, including a review of connected and local routes, how routers forward packets, static-route configuration, and default routes. The theory covered how routers use their routing tables to make forwarding decisions and how static routes provide reachability to remote networks.

The practical session included **two separate Cisco Packet Tracer labs**. Lab 1 involved configuring IP addressing, router hostnames, static routes, DNS-related settings, and PC connectivity. Lab 2 focused on identifying and correcting misconfigured static routes so that PC1 and PC2 could communicate. The notes below distinguish the lab requirements from the results visible in the supplied evidence.

## What I Learned

### 1. Connected and Local Routes

When an IP address is configured on an enabled router interface, IOS automatically adds routes for the connected network and the interface’s own IP address.

- **Connected route (`C`):** Represents the network directly attached to an interface. The route uses the subnet mask configured on that interface.
- **Local route (`L`):** Represents the exact IP address configured on the interface. It is installed with a `/32` mask because it identifies one IPv4 address.

For example, if `GigabitEthernet0/0` has `192.168.12.2/24`, the routing table contains a connected route for `192.168.12.0/24` and a local route for `192.168.12.2/32`.

These routes allow a router to reach its own interface addresses and directly connected networks. They do not, by themselves, provide reachability to remote networks.

### 2. Default Gateway and Packet Forwarding

An end device can send traffic directly to another host on its local subnet. When the destination is outside that subnet, the device sends the packet to its **default gateway**, which is normally the router interface on the local LAN.

For example, a host at `192.168.1.10/24` can use `192.168.1.1` as its default gateway. The destination IP remains the remote host’s IP, while the Ethernet frame on the local segment is addressed to the gateway’s MAC address. The host can use ARP to learn that MAC address.

A router receives the frame, removes the Layer 2 encapsulation, examines the destination IP address, and consults its routing table. It forwards the packet using the most-specific matching route. If no matching route exists, and no default route is available, the router drops the packet.

### 3. Static Routes

A **static route** is a route manually configured by an administrator. It tells the router which next hop or exit interface to use to reach a destination network.

The general command forms covered in the slides are:

```text
Router(config)# ip route <destination-network> <subnet-mask> <next-hop>
Router(config)# ip route <destination-network> <subnet-mask> <exit-interface>
Router(config)# ip route <destination-network> <subnet-mask> <exit-interface> <next-hop>
```

- **Next-hop route:** Specifies the IP address of the next router.
- **Exit-interface route:** Specifies the outgoing interface. The slides note that this form relies on Proxy ARP.
- **Fully specified static route:** Specifies both the exit interface and next-hop IP address.

The slides recommend that next-hop or fully specified routes can generally be used; the choice depends on the network and configuration requirements.

### 4. Static Routing and Return Traffic

For two hosts in different networks to communicate, the routers along the selected path need routes that support forwarding in both directions. A route in the forward direction alone does not guarantee that replies can return.

The slide example uses PC1 (`192.168.1.10`) and PC4 (`192.168.4.10`). One possible path is `PC1 → R1 → R3 → R4 → PC4`; another is through R2. In the illustrated configuration, static routes are placed on R1, R3, and R4 for the path through R3. A router does not need a route to every intermediate network if it only needs to forward traffic toward the destination using its next hop.

The example route entries are:

| Router | Destination | Next hop / route |
|---|---|---|
| R1 | `192.168.1.0/24` | Connected |
| R1 | `192.168.4.0/24` | `192.168.13.3` |
| R3 | `192.168.1.0/24` | `192.168.13.1` |
| R3 | `192.168.4.0/24` | `192.168.34.4` |
| R4 | `192.168.1.0/24` | `192.168.34.3` |
| R4 | `192.168.4.0/24` | Connected |

The route code `S` identifies a static route. The `[1/0]` notation shown in the examples represents administrative distance and metric.

### 5. Default Routes

A **default route** is written as `0.0.0.0/0`. It is the least-specific IPv4 route and matches destinations for which no more-specific route is available.

A default route is commonly used to send traffic toward an upstream router or the Internet, while more-specific routes handle known internal networks. The example in the slides configures R1 to use `203.0.113.2` as its next hop:

```text
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.2
```

After configuration, the routing table displays the route with `S*`, and the gateway-of-last-resort field identifies the next hop. The `*` indicates a candidate default route.

## Key Concepts

| Concept | Description |
|---|---|
| Connected route (`C`) | Automatically installed route to a network directly connected to an enabled, addressed interface. |
| Local route (`L`) | Automatically installed `/32` route to the IP address configured on a router interface. |
| Default gateway | The next-hop router an end device uses to reach destinations outside its local subnet. |
| Routing table | The router’s collection of known routes used to determine how packets should be forwarded. |
| Most-specific match | The matching route with the longest prefix is selected for forwarding. |
| Static route (`S`) | A manually configured route to a destination network. |
| Next hop | The IP address of the neighboring router to which a packet should be forwarded. |
| Exit interface | The local router interface through which traffic is sent. |
| Fully specified route | A static route that specifies both the exit interface and the next-hop address. |
| Default route (`S*`) | A route to `0.0.0.0/0`, used when no more-specific route matches. |
| Two-way reachability | The ability for traffic to travel to a destination and for the reply traffic to return. |
| DNS | A name-resolution service that maps hostnames to IP addresses; the lab includes DNS configuration, but the supplied screenshots do not expose its exact values. |
| Ping | An ICMP-based test used to check IP reachability between devices. |

## Packet Tracer Lab 1 – IP Addressing, Static Routing and DNS Configuration

### Lab Objective

The objective of Lab 1 was to build and configure the network shown in the Packet Tracer question, assign IP addressing to the PCs and router interfaces, configure router hostnames, set up static routes and DNS-related settings, and test connectivity between the end devices.

The folder contains a question `.pkt` file, a solution `.pkt` file, and numbered screenshots documenting setup and configuration. The screenshots’ filenames provide evidence of PC gateway/address settings, router hostname/interface configuration, enabled interfaces, static IP configuration, and a successful cross-PC ping. Exact DNS values are not visible in the supplied folder screenshot.

### Network Topology

The question screenshot shows a linear path of three routers between two LANs. PC1 is connected through SW1 to R1; R1 connects to R2, which connects to R3; R3 connects through SW2 to PC2.

```text
PC1 ── SW1 ── R1 ── R2 ── R3 ── SW2 ── PC2
 LAN 1        |      |      |          LAN 2
192.168.1.0/24  192.168.12.0/24  192.168.13.0/24  192.168.3.0/24
```

The labels visible in the question indicate the following addressing plan:

| Segment / device | Interface or setting | Address shown |
|---|---|---|
| PC1 LAN | Network | `192.168.1.0/24` |
| R1 | G0/1, toward SW1 / PC1 LAN | `192.168.1.254/24` |
| R1–R2 link | Network | `192.168.12.0/24` |
| R1 | G0/0, toward R2 | `192.168.12.1/24` |
| R2 | G0/0, toward R1 | `192.168.12.2/24` |
| R2–R3 link | Network | `192.168.13.0/24` |
| R2 | G0/1, toward R3 | `192.168.13.2/24` |
| R3 | G0/0, toward R2 | `192.168.13.3/24` |
| R3–PC2 LAN | Network | `192.168.3.0/24` |
| R3 | G0/1, toward SW2 / PC2 LAN | `192.168.3.254/24` |
| PC2 LAN | PC2 address shown | `192.168.3.1/24` |

The `/24` prefix corresponds to subnet mask `255.255.255.0`. The question screenshot labels the PC host addresses as `.1` on each LAN and the router LAN interfaces as `.254`.

### IP Addressing and PC Configuration

The lab required assigning IP addresses and subnet masks to the PCs and router interfaces according to the diagram. The visible solution filenames indicate that PC1 and PC2 address/subnet-mask settings and PC default gateways were captured.

| Device | IPv4 address | Subnet mask | Default gateway |
|---|---|---|---|
| PC1 | `192.168.1.1` | `255.255.255.0` | `192.168.1.254` |
| PC2 | `192.168.3.1` | `255.255.255.0` | `192.168.3.254` |

The default gateways are the router interfaces on the respective LANs. The supplied folder screenshot does not show the DNS server address configured on either PC, so that value is not reproduced here.

### Hostname and Interface Configuration

The lab question required configuring the routers according to the network diagram. The solution folder includes screenshots named for R1, R2, and R3 hostname changes and interface enablement. The filenames establish that these configuration stages were documented, but the folder view does not show the exact CLI transcripts.

The relevant Cisco IOS command pattern for assigning a hostname and enabling an addressed interface is:

```text
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip address <ip-address> <subnet-mask>
R1(config-if)# no shutdown
```

Repeat the hostname and interface configuration for the applicable router and interfaces, using the values from the topology. The commands above illustrate the configuration syntax; they are not a verbatim transcript of every command entered in the lab.

### Static Routing

Static routing was required to provide a path between the two LANs. The network uses three routers in sequence, so the routers need routes to the remote LAN and appropriate return routes. The specific route commands entered in this lab are not legible in the folder screenshot; therefore, the following are representative route patterns based on the displayed addressing plan, not a claim that these exact commands were captured.

```text
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.12.2
R2(config)# ip route 192.168.1.0 255.255.255.0 192.168.12.1
R2(config)# ip route 192.168.3.0 255.255.255.0 192.168.13.3
R3(config)# ip route 192.168.1.0 255.255.255.0 192.168.13.2
```

These routes illustrate how a router can forward traffic toward the next router on the path. The required routing configuration should be checked against the actual `.pkt` solution before treating the example commands as the exact submitted configuration.

### DNS Configuration

DNS translates a hostname into an IP address so that users and applications can address a host by name rather than remembering its numeric address. The Lab 1 question includes DNS configuration as part of the task.

The provided folder screenshot does not expose a DNS server address, hostname-to-address mapping, or DNS configuration screen. Consequently, the exact DNS settings and the result of any name-resolution test cannot be confirmed from the available evidence.

### Connectivity Testing

The solution folder includes `13_Ping_Works_Across_PCs.png`, which indicates that a cross-PC ping result was captured. This filename is evidence of a recorded working ping, although the screenshot itself is not available here to inspect its packet counts or exact command output.

A ping between PC1 and PC2 checks end-to-end IP reachability across the router path. A successful reply indicates that the forward path and the return path are functioning for that test. The command pattern is:

```text
ping 192.168.3.1
```

Run it from PC1 to test reachability to PC2. A reverse ping from PC2 to PC1 can additionally test the opposite direction.

### Commands Practiced

The lab’s documented tasks and screenshot names support the following command patterns. Exact DNS commands and all entered route commands are not visible in the folder screenshot.

```text
enable
configure terminal
hostname R1
interface gigabitEthernet 0/0
ip address <ip-address> <subnet-mask>
no shutdown
ip route <destination-network> <subnet-mask> <next-hop>
show ip interface brief
show ip route
ping <destination-ip>
```

## Packet Tracer Lab 2 – Router Troubleshooting

### Lab Objective

Lab 2 focused on troubleshooting a network in which PC1 and PC2 could not ping each other. The question states that there was **one misconfiguration on each router** and asks for the faults to be found and corrected. The completion condition specified in the question was successful ping connectivity between PC1 and PC2.

### Problem Description

The question screenshot shows three routers connecting the two end-device LANs:

```text
PC1 ── SW1 ── R1 ── R2 ── R3 ── SW2 ── PC2
192.168.1.0/24   192.168.12.0/24   192.168.13.0/24   192.168.3.0/24
```

The question explicitly states that PC1 and PC2 are unable to ping each other and that each router contains one misconfiguration. The screenshot does not identify the specific incorrect route values; those are documented in the solution screenshots’ filenames as misconfigured and corrected routes for R1, R2, and R3.

### Troubleshooting Approach

The lab can be approached systematically by checking connectivity and route information at each hop:

1. **Check the endpoint configuration.** Confirm that each PC has the intended IP address, subnet mask, and default gateway.
2. **Check interface status.** Verify that the relevant router interfaces are enabled and operational.
3. **Inspect routing tables.** Use `show ip route` to review connected, local, and static routes. Check that each router has a usable route toward the remote LAN.
4. **Inspect static route entries.** Compare each route’s destination network, subnet mask, next hop, and exit interface (if specified) with the topology.
5. **Correct the identified route entries.** The supplied evidence names corrected static-route screenshots for R1, R2, and R3, but the exact route statements are not visible in the folder listing.
6. **Retest end-to-end connectivity.** Repeat the PC1-to-PC2 ping after the route corrections.

This is a troubleshooting workflow based on the question and the evidence filenames. It does not assert a particular incorrect next hop or reproduce an unverified correction.

### Commands Practiced

The following commands are relevant to inspecting and correcting the route-related faults described in the question. The exact commands used in the solution are not visible in the folder listing.

```text
enable
show ip interface brief
show ip route
configure terminal
ip route <destination-network> <subnet-mask> <next-hop>
no ip route <destination-network> <subnet-mask> <next-hop>
ping <destination-ip>
```

`no ip route` is the IOS command form used to remove a matching static route; the route arguments must match the entry being removed. The corrected route should then be entered with the intended destination and next hop.

### Expected Verification

The question defines successful PC1-to-PC2 ping as the completion criterion. The Lab 2 folder includes both `02_PC1_To_PC2_Ping_Failed.png` and `09_Ping_PC1_To_PC2_Works.png`, which document a failed ping and a later working ping. These filenames indicate that both states were captured. The exact ping statistics are not available from the folder listing.

The folder also includes separate misconfigured-route and corrected-route screenshots for R1, R2, and R3. This provides evidence that route states were documented before and after correction, but the exact faulty values and commands cannot be established from filenames alone.

## Consolidated Commands Practiced

| Command | Purpose |
|---|---|
| `enable` | Enters privileged EXEC mode. |
| `configure terminal` | Enters global configuration mode. |
| `hostname <name>` | Sets the router’s hostname. |
| `interface gigabitEthernet <slot>/<port>` | Enters configuration mode for the specified Gigabit Ethernet interface. |
| `ip address <ip-address> <subnet-mask>` | Assigns an IPv4 address and subnet mask to an interface. |
| `no shutdown` | Administratively enables an interface. |
| `ip route <destination> <mask> <next-hop>` | Adds a static route using a next-hop address. |
| `no ip route <destination> <mask> <next-hop>` | Removes a matching static route. |
| `show ip interface brief` | Summarizes interface IP addresses and operational/status information. |
| `show ip route` | Displays the router’s IPv4 routing table. |
| `ping <destination-ip>` | Tests IP reachability to a destination using ICMP echo requests. |

## Key Takeaways

- Router interfaces generate **connected (`C`)** and **local (`L`)** routes automatically when they are addressed and enabled.
- A host uses its **default gateway** to send traffic beyond its local subnet.
- Routers rely on their routing tables and the most-specific matching route to decide where to forward packets.
- Static routes provide explicit paths to remote networks. Correct destination prefixes and next hops are essential.
- Successful end-to-end communication depends on both the forward and return paths.
- A default route (`0.0.0.0/0`) provides a route of last resort when no more-specific route matches.
- In practical troubleshooting, checking interface status, reviewing routing tables, examining static routes, and retesting with ping helps isolate routing problems.
- Lab 1 applied addressing, hostname, static routing, DNS-related configuration, and connectivity testing. Lab 2 focused on diagnosing router route misconfigurations and checking connectivity after correction.

## Lab Evidence

### Lab 1 – IP Addressing, Static Routing and DNS Configuration

```text
Day 11 Lab 1/
├── Day+11+Lab+Arpit+Solution+Part1.pkt
├── 13_Ping_Works_Across_PCs.png
├── 10_R1_Static_IP_Configured.png
├── 12_R3_Static_IP_Configured.png
├── 11_R2_Static_IP_Configured.png
├── 09_All_Network_Interfaces_Enabled.png
├── 08_PC2_IP_Address_Subnet_Mask.png
├── 07_PC2_Default_Gateway.png
├── 06_R3_Name_Change_Interface_Enabled.png
├── 05_R2_Name_Change_Interfaces_Enabled.png
├── 04_R1_Corrected_Misconfigured_Route.png
├── 03_PC1_IP_Address_Subnet_Mask.png
├── 02_PC1_Default_Gateway.png
├── 01_Network_Setup.png
└── Day+11+Lab+Question+Configuring+Static+Routes.pkt
```

### Lab 2 – Router Troubleshooting

```text
Day 11 Lab 2/
├── Day+11+Lab+Arpit+Solution+Part2.pkt
├── 09_Ping_PC1_To_PC2_Works.png
├── 08_R3_Corrected_Misconfigured_Route.png
├── 07_R3_Misconfigured_Route.png
├── 06_R2_Corrected_Misconfigured_Route.png
├── 05_R2_Misconfigured_Route.png
├── 04_R1_Corrected_Misconfigured_Route.png
├── 03_R1_Misconfigured_Route.png
├── 02_PC1_To_PC2_Ping_Failed.png
├── 01_Full_Network_Setup.png
└── Day+11+Lab+Question+Troubleshooting+Static+Routes.pkt
```

## Conclusion

Day 11 combined the theory of connected and local routes, static routing, packet forwarding, and default routes with two Packet Tracer exercises. Lab 1 brought together network addressing, router and PC configuration, static routing, DNS-related settings, and connectivity testing. Lab 2 applied a structured troubleshooting process to router route misconfigurations, with the supplied evidence documenting failed and working ping states. Together, the sessions reinforced how correct addressing and routing information enables end-to-end communication and how routing tables can be used to investigate connectivity issues.
