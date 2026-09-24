# Day 3 — TCP/IP Model

## Course
**Jeremy's IT Lab — CCNA 200-301**

## Overview

Day 3 of Jeremy's IT Lab CCNA 200-301 covered **protocols, standards, layered network models, the TCP/IP model, OSI model, encapsulation/decapsulation, and Protocol Data Units (PDUs)**. I also practiced these concepts in Cisco Packet Tracer using Simulation Mode and worked with `ipconfig`, `ipconfig /release`, and `ipconfig /renew`.

## What I Learned

- A **protocol** defines rules for communication, while a **standard** provides an agreed specification that allows different vendors' devices to communicate.
- **IEEE** develops technologies such as Ethernet and Wi-Fi, while the **IETF** defines Internet protocols and publishes RFCs.
- Layered models divide networking functions into separate layers, with each layer having its own role.
- The **TCP/IP 5-layer model** consists of:
  - Application
  - Transport
  - Internet
  - Local Network
  - Physical
- The **OSI model** has 7 layers and is commonly used as a reference model for understanding networking.
- I learned how data is **encapsulated** as it moves down the layers and **decapsulated** as it moves up the layers.
- I also learned the names of data at different layers: **segment/datagram → packet → frame**.

## Key Concepts

| Layer | Main Function | Examples |
|---|---|---|
| Application | Communication between applications | HTTP, FTP, DNS |
| Transport | Process-to-process communication using ports | TCP, UDP |
| Internet | Host-to-host communication using IP addresses | IPv4, IPv6, ICMP |
| Local Network | Hop-to-hop delivery using MAC addresses | Ethernet, Wi-Fi |
| Physical | Sends bits as signals | Copper, fiber, radio |

Other important concepts I practiced:

- **Encapsulation / Decapsulation**
- **Protocol Data Unit (PDU)**
- **Payload**
- **MAC address, IP address and port number**
- **Same-layer interaction**
- **Adjacent-layer interaction**
- **Separation of layers**

## Packet Tracer Lab

For the practical part, I used **Cisco Packet Tracer Simulation Mode** to observe network traffic and inspect PDUs at different devices and layers.

I also practiced:

- Using Simulation Mode and checking the **PDU Information** window.
- Observing **OSPF** traffic at R1.
- Observing **STP BPDU** traffic at SW2.
- Configuring DHCP on SRV1.
- Checking PC1's IP configuration using `ipconfig`.
- Releasing and renewing PC1's DHCP address using `ipconfig /release` and `ipconfig /renew`.

## Network Topology

The Packet Tracer topology consisted of:

```text
SRV1 ── SW1 ── R1 ── R2
          │        │
          │        └── 10.0.0.0/24
  PC1 ── SW2
       │ 
 192.168.1.0/24
```

The main networks shown were:

- **192.168.1.0/24** — LAN containing SRV1, SW1, SW2 and PC1
- **10.0.0.0/24** — connection between R1 and R2

Important addressing shown in the lab included:

- SRV1: `192.168.1.100`
- R1 LAN interface: `192.168.1.1`
- R1–R2 link: `10.0.0.1` / `10.0.0.2`
- PC1 received `192.168.1.11` after DHCP renewal.

## Commands Practiced

```text
ipconfig
```

Used to view the PC's current IP configuration, subnet mask and default gateway.

```text
ipconfig /release
```

Used to release the PC's current DHCP-assigned IPv4 configuration.

```text
ipconfig /renew
```

Used to request a new IPv4 configuration from the DHCP server.

## Key Takeaways

- I understood how the **TCP/IP layers work together** to deliver data.
- I learned the difference between **IP addresses, MAC addresses and port numbers**.
- I understood the flow of **encapsulation and decapsulation**.
- I learned how **segments/datagrams, packets and frames** relate to different layers.
- Using Packet Tracer Simulation Mode helped me connect the theoretical layers with actual network traffic.
- I practiced basic DHCP client operations using `ipconfig`.

## Lab Evidence

The following files document my Day 3 practical work:

- `Day+03+Arpit+Solution.pkt` — Completed Packet Tracer solution.
- `PDU_Info_R1_OSPF.png` — PDU information showing OSPF traffic at R1.
- `PDU_Info_SW2_STP.png` — PDU information showing STP traffic at SW2.
- `DHCP_Configuration_On_Server.png` — DHCP configuration on SRV1.
- `Ipconfig_options.png` — PC1 `ipconfig`, `ipconfig /release`, and `ipconfig /renew` practice.
- `Complete_Network_Setup.png` — Complete Packet Tracer network topology.

## Conclusion

Day 3 helped me understand how the **TCP/IP and OSI models explain the movement of data through a network**. I also connected these concepts to actual Packet Tracer traffic by using Simulation Mode and practicing DHCP address release and renewal on a PC.
