# Day 15 -- Subnetting (Part 3)

## Course
**Jeremy's IT Lab — CCNA 200-301**

## Overview

Day 15 of Jeremy's IT Lab CCNA 200-301 course focused on **Subnetting
(Part 3)**, with emphasis on:

-   Subnetting Class A networks
-   Finding network, broadcast, first usable, and last usable addresses
-   Reviewing FLSM and VLSM
-   Understanding how VLSM allocates different subnet sizes according to
    host requirements
-   Applying VLSM to a practical Packet Tracer network
-   Configuring router interfaces and static routes
-   Verifying end-to-end connectivity between PCs

------------------------------------------------------------------------

## What I Learned

In this lesson, I continued subnetting practice by working with Class A
networks and VLSM.

The main points covered were:

-   Class A networks start with a default `/8` prefix.
-   Additional subnet bits can be borrowed from the host portion to
    create more subnets.
-   The number of usable hosts depends on the number of host bits
    remaining.
-   A subnet's network address and broadcast address cannot be assigned
    to hosts.
-   VLSM allows different subnets to use different prefix lengths.
-   When designing a VLSM network, the largest host requirement should
    be allocated first.
-   Static routes can be used to provide connectivity between different
    subnetworks.

------------------------------------------------------------------------

## Key Concepts

### Class A Subnetting

The default Class A network is `/8`.

Example:

``` text
Network: 10.0.0.0/8
Subnet Mask: 255.0.0.0
```

The number of subnet bits borrowed determines the new prefix length.

For example, borrowing 11 bits from a Class A `/8` network results in:

``` text
/8 + 11 = /19
```

This leaves 13 host bits.

Usable hosts:

``` text
2^13 - 2 = 8190
```

### Finding Network and Broadcast Addresses

For an IP address such as:

``` text
10.217.182.223/11
```

the relevant addresses are:

  Address Type        Address
  ------------------- ------------------
  Network Address     `10.192.0.0/11`
  First Usable        `10.192.0.1`
  Last Usable         `10.223.255.254`
  Broadcast Address   `10.223.255.255`

------------------------------------------------------------------------

## FLSM vs VLSM

### FLSM -- Fixed Length Subnet Mask

In FLSM, all subnets use the same subnet mask.

For example:

``` text
192.168.1.0/24
```

can be divided into subnets where every subnet has the same prefix
length.

### VLSM -- Variable Length Subnet Mask

VLSM allows different subnet sizes to be used within the same network.

This makes address allocation more efficient because each LAN can
receive a subnet based on its actual host requirement.

A typical VLSM allocation process is:

1.  Allocate the largest required subnet first.
2.  Allocate the second-largest subnet after it.
3.  Continue allocating the remaining subnets in descending order of
    host requirement.
4.  Use the remaining address space for smaller networks and
    point-to-point links.

------------------------------------------------------------------------

## VLSM Worked Example

The lesson demonstrated VLSM using:

``` text
192.168.1.0/24
```

The required subnets were allocated as follows:

  Network Requirement         Subnet               Prefix
  --------------------------- -------------------- --------
  Tokyo LAN A -- 110 hosts    `192.168.1.0/25`     `/25`
  Toronto LAN B -- 45 hosts   `192.168.1.128/26`   `/26`
  Toronto LAN A -- 29 hosts   `192.168.1.192/27`   `/27`
  Tokyo LAN B -- 8 hosts      `192.168.1.224/28`   `/28`
  Point-to-Point -- 2 hosts   `192.168.1.240/30`   `/30`

The important idea is to allocate the largest subnet first and
progressively use smaller subnet sizes for networks with fewer hosts.

------------------------------------------------------------------------

# Packet Tracer Lab

## Objective

Subnet the `192.168.5.0/24` network using VLSM to provide sufficient
addressing for each LAN and the point-to-point connection between R1 and
R2.

The lab also requires:

-   Assigning the first usable IP address to the PC in each LAN.
-   Assigning the last usable IP address to the router interface in each
    LAN.
-   Configuring router interfaces.
-   Configuring the R1-R2 point-to-point link.
-   Configuring static routes.
-   Verifying that all PCs can communicate with each other.

## Question Description

The given network is:

``` text
192.168.5.0/24
```

The network must be subnetted according to the host requirements of the
LANs and the R1-R2 point-to-point connection.

The VLSM addressing used in the lab was:

  -----------------------------------------------------------------------------------------
  Requirement    Network              First Usable      Last Usable       Broadcast
  -------------- -------------------- ----------------- ----------------- -----------------
  64 hosts       `192.168.5.0/25`     `192.168.5.1`     `192.168.5.126`   `192.168.5.127`

  45 hosts       `192.168.5.128/26`   `192.168.5.129`   `192.168.5.190`   `192.168.5.191`

  14 hosts       `192.168.5.192/28`   `192.168.5.193`   `192.168.5.206`   `192.168.5.207`

  9 hosts        `192.168.5.208/28`   `192.168.5.209`   `192.168.5.222`   `192.168.5.223`

  2 hosts        `192.168.5.224/30`   `192.168.5.225`   `192.168.5.226`   `192.168.5.227`
  
  -----------------------------------------------------------------------------------------

## Network Topology

The Packet Tracer topology consists of:

``` text
PC1 ---- SW1 ---- R1
                  |
PC2 ---- SW2 ----+
                  |
                  | Point-to-Point
                  |
                  +---- R2 ---- SW3 ---- PC3
                        |
                        +---- SW4 ---- PC4
```

The router interfaces are used as follows:

-   R1 G0/1 → PC2 LAN
-   R1 G0/0 → PC1 LAN
-   R1 G0/0/0 → R2
-   R2 G0/0 → PC3 LAN
-   R2 G0/1 → PC4 LAN
-   R2 G0/0/0 → R1

## Addressing Plan

  ------------------------------------------------------------------------------------
  Device         Interface      IP Address        Subnet Mask         Purpose
  -------------- -------------- ----------------- ------------------- ----------------
  PC1            NIC            `192.168.5.129`   `255.255.255.192`   First usable
                                                                      address

  R1             G0/0           `192.168.5.190`   `255.255.255.192`   Last usable
                                                                      address

  PC2            NIC            `192.168.5.1`     `255.255.255.128`   First usable
                                                                      address

  R1             G0/1           `192.168.5.126`   `255.255.255.128`   Last usable
                                                                      address

  R2             G0/0           `192.168.5.206`   `255.255.255.240`   Last usable
                                                                      address

  R2             G0/1           `192.168.5.222`   `255.255.255.240`   Last usable
                                                                      address

  R1             G0/0/0         `192.168.5.225`   `255.255.255.252`   Point-to-point
                                                                      link

  R2             G0/0/0         `192.168.5.226`   `255.255.255.252`   Point-to-point
                                                                      link
  ------------------------------------------------------------------------------------

For the LANs behind R2, the first usable PC addresses are:

``` text
PC3: 192.168.5.193/28
PC4: 192.168.5.209/28
```

------------------------------------------------------------------------

## Configuration Steps

### 1. Configure PC2

PC2 was configured with:

``` text
IPv4 Address: 192.168.5.1
Subnet Mask: 255.255.255.128
```

The first usable address of the `/25` subnet was assigned to the PC.

### 2. Configure R1 LAN Interfaces

R1 was configured with the last usable addresses of its two LAN subnets.

``` text
R1(config)# interface g0/1
R1(config-if)# ip address 192.168.5.126 255.255.255.128

R1(config)# interface g0/0
R1(config-if)# ip address 192.168.5.190 255.255.255.192
```

The interfaces were then enabled using:

``` text
R1(config)# interface g0/0
R1(config-if)# no shutdown
```

The interface status was checked using:

``` text
R1# show ip interface brief
```

### 3. Configure R2 LAN Interfaces

R2 was configured with the last usable addresses for the two LANs:

``` text
R2(config)# interface g0/0
R2(config-if)# ip address 192.168.5.206 255.255.255.240

R2(config)# interface g0/1
R2(config-if)# ip address 192.168.5.222 255.255.255.240
```

Both interfaces were enabled:

``` text
R2(config)# interface g0/0
R2(config-if)# no shutdown

R2(config)# interface g0/1
R2(config-if)# no shutdown
```

The interfaces were verified with:

``` text
R2# do show ip interface brief
```

The evidence showed both interfaces in an `up/up` state.

### 4. Configure the R1-R2 Point-to-Point Link

The point-to-point subnet is:

``` text
192.168.5.224/30
```

The usable addresses are:

``` text
R1: 192.168.5.225
R2: 192.168.5.226
```

This provides two usable addresses, one for each router.

### 5. Configure Static Routes on R2

R2 needs routes to the two LANs behind R1.

``` text
R2(config)# ip route 192.168.5.128 255.255.255.192 192.168.5.225
R2(config)# ip route 192.168.5.0 255.255.255.128 192.168.5.225
```

The routing table was checked using:

``` text
R2# do show ip route
```

The final routing table showed:

``` text
192.168.5.0/25 [1/0] via 192.168.5.225
192.168.5.128/26 [1/0] via 192.168.5.225
```

The directly connected R2 networks were also present.

### 6. Configure Static Routes on R1

R1 needs routes to the two LANs behind R2.

``` text
R1(config)# ip route 192.168.5.192 255.255.255.240 192.168.5.226
R1(config)# ip route 192.168.5.208 255.255.255.240 192.168.5.226
```

The routing table was verified using:

``` text
R1# do show ip route
```

The final table showed:

``` text
192.168.5.192/28 [1/0] via 192.168.5.226
192.168.5.208/28 [1/0] via 192.168.5.226
```

------------------------------------------------------------------------

## Commands Practiced

### PC Configuration

``` text
ipconfig
ipconfig /all
ping <destination-ip>
```

### Router Interface Configuration

``` text
enable
configure terminal
interface g0/0
interface g0/1
interface g0/0/0
ip address <ip-address> <subnet-mask>
no shutdown
```

### Verification Commands

``` text
show ip interface brief
show ip route
do show ip interface brief
do show ip route
ping <destination-ip>
```

### Static Routing

``` text
ip route <network-address> <subnet-mask> <next-hop-address>
```

------------------------------------------------------------------------

## Verification and Results

### Interface Verification

R1 was verified using:

``` text
show ip interface brief
```

R2 was verified using:

``` text
do show ip interface brief
```

The evidence showed the configured R2 LAN interfaces in an `up/up`
state.

### Routing Table Verification

R1 contained routes toward the R2 LANs:

``` text
192.168.5.192/28 via 192.168.5.226
192.168.5.208/28 via 192.168.5.226
```

R2 contained routes toward the R1 LANs:

``` text
192.168.5.0/25 via 192.168.5.225
192.168.5.128/26 via 192.168.5.225
```

### Ping Verification

The uploaded lab evidence includes successful connectivity tests:

-   `PC1_To_PC2_Ping_Works`
-   `PC3_To_PC2_Ping_Works`
-   `PC4_To_PC1_Ping_Works`
-   `Full_Network_Works_All_Pings_Work`

The final full-network screenshot indicates that the required PC-to-PC
connectivity was working.

------------------------------------------------------------------------

## Troubleshooting

### Incorrect Static Route Network Address

While configuring R2, an incorrect route was initially entered:

``` text
R2(config)# ip route 192.168.5.129 255.255.255.192 192.168.5.225
```

The router returned:

``` text
%Inconsistent address and mask
```

The issue was that `192.168.5.129` is a host address and cannot be used
as the network address with a `/26` mask.

The correct network address is:

``` text
192.168.5.128/26
```

Therefore, the correct command is:

``` text
R2(config)# ip route 192.168.5.128 255.255.255.192 192.168.5.225
```

This highlights the importance of identifying the correct **network
address** before configuring a static route.

------------------------------------------------------------------------

## Lab Evidence and Folder Structure

The Day 15 lab folder contains the following files:

``` text
Day 15 Lab/
│
├── Day+15+Lab+Question+VLSM.pkt
├── Day+15+Lab+Arpit+Solution.pkt
├── Day+15+Slides+-+Subnetting+(Part 3).pdf
├── subnet.txt
│
├── 01_PC2_IP_Mask_Configuration.png
├── 02_R1_Interface_IP_Configure_No_Shutdown.png
├── 03_R2_Interface_IP_Configure_No_Shutdown.png
├── 04_R2_Further_Configurations.png
├── 05_R2_IP_Route_Static_Configurations.png
├── 06_R2_Show_IP_Route.png
├── 07_R1_Show_IP_Route.png
├── 08_IP_Subnetting_And_Network_Working.png
├── 09_PC1_To_PC2_Ping_Works.png
├── 10_PC3_To_PC2_Ping_Works.png
├── 11_PC4_To_PC1_Ping_Works.png
└── 12_Full_Network_Works_All_Pings_Work.png
```

### Important Evidence

-   `01_PC2_IP_Mask_Configuration.png` -- PC2 IP and subnet mask
    configuration.
-   `02_R1_Interface_IP_Configure_No_Shutdown.png` -- R1 interface
    configuration and interface status.
-   `03_R2_Interface_IP_Configure_No_Shutdown.png` -- R2 interface
    configuration and interface status.
-   `06_R2_Show_IP_Route.png` -- R2 static routes and routing table.
-   `07_R1_Show_IP_Route.png` -- R1 static routes and routing table.
-   `08_IP_Subnetting_And_Network_Working.png` -- subnetting
    requirements and network task.
-   `09_PC1_To_PC2_Ping_Works.png` -- PC1 to PC2 connectivity
    verification.
-   `10_PC3_To_PC2_Ping_Works.png` -- PC3 to PC2 connectivity
    verification.
-   `11_PC4_To_PC1_Ping_Works.png` -- PC4 to PC1 connectivity
    verification.
-   `12_Full_Network_Works_All_Pings_Work.png` -- final full-network
    connectivity verification.

------------------------------------------------------------------------

## Consolidated Commands Practiced

  ----------------------------------------------------------------------------------------
  Command                                  Device                  Purpose
  ---------------------------------------- ----------------------- -----------------------
  `enable`                                 Router                  Enter privileged EXEC
                                                                   mode

  `configure terminal`                     Router                  Enter global
                                                                   configuration mode

  `interface g0/0`                         Router                  Enter GigabitEthernet
                                                                   interface configuration

  `interface g0/1`                         Router                  Enter GigabitEthernet
                                                                   interface configuration

  `interface g0/0/0`                       Router                  Enter point-to-point
                                                                   interface configuration

  `ip address <ip> <mask>`                 Router                  Assign an IP address
                                                                   and subnet mask

  `no shutdown`                            Router                  Enable an interface

  `show ip interface brief`                Router                  Quickly verify
                                                                   interface IPs and
                                                                   status

  `do show ip interface brief`             Router                  Run interface
                                                                   verification from
                                                                   configuration mode

  `ip route <network> <mask> <next-hop>`   Router                  Configure a static
                                                                   route

  `show ip route`                          Router                  Display the routing
                                                                   table

  `do show ip route`                       Router                  Display the routing
                                                                   table from
                                                                   configuration mode

  `ping <destination-ip>`                  PC/Router               Test IP connectivity

  `ipconfig`                               PC                      Display basic IP
                                                                   configuration

  `ipconfig /all`                          PC                      Display detailed IP
                                                                   configuration
  ----------------------------------------------------------------------------------------

------------------------------------------------------------------------

## Key Takeaways

-   Class A networks have a default `/8` prefix.
-   Borrowing additional bits from the host portion creates more
    subnets.
-   The number of usable hosts is calculated as `2^host-bits - 2`.
-   Network and broadcast addresses cannot be assigned to hosts.
-   FLSM uses the same subnet size for every subnet.
-   VLSM allows different subnet sizes to be used according to host
    requirements.
-   In VLSM, the largest subnet should generally be allocated first.
-   A `/30` subnet provides two usable host addresses, making it
    suitable for a point-to-point link.
-   Static routes must use the correct destination network address and
    subnet mask.
-   `show ip interface brief` is useful for quickly checking interface
    status.
-   `show ip route` is useful for verifying connected and static routes.
-   Successful ping tests confirm end-to-end connectivity.

------------------------------------------------------------------------

## Conclusion

Day 15 provided practical experience with **VLSM subnetting and static
routing**. I applied the subnetting concepts to divide `192.168.5.0/24`
into appropriately sized networks, configured router interfaces,
established the R1-R2 point-to-point connection, and added static
routes.

The lab also helped reinforce the importance of using the correct
network address when configuring routes. After correcting the invalid
static route and completing the required configurations, the final
Packet Tracer evidence showed successful connectivity across the
network.
