# Day 01 — Network Devices & Packet Tracer Lab

## Course
**Jeremy's IT Lab — CCNA 200-301**

## What I learned

Day 1 introduced the basic devices that make up a network:

- **Client** — a device that accesses a service provided by a server.
- **Server** — a device that provides functions or services to clients.
- **Switch** — connects multiple end hosts within the same LAN.
- **Router** — connects different networks and forwards traffic between them.
- **Firewall** — monitors and controls network traffic based on configured rules.

The lesson also introduced the idea that the same device can act as a client in one situation and a server in another.

## Packet Tracer Lab

I built the network shown in the Day 1 lab and brought the connections up in Packet Tracer.

### Network layout

```text
New York Branch

PC1 ──┐
       ├── Switch1 ── Router3 ── FW1 ── The Internet ── Router2 ── FW2 ── Switch2 ──┬── Server0
PC2 ──┘                                                                               └── Server1
                                                                                         Tokyo Branch

                                      Attacker
                                         │
                                         └── The Internet
```

The final topology contains two branch networks connected through the Internet, with a firewall protecting each branch and an attacker device connected on the Internet side.

## Router Configuration / Troubleshooting

While bringing up the network, some router interfaces were **administratively down**. I learned the basic process for checking and enabling a router interface.

### Basic process used

1. Open the router CLI.
2. When asked for the initial configuration dialog, enter `no`.
3. Press **Enter** when prompted with `Press RETURN to get started!`
4. Enter privileged mode:

```text
enable
```

5. Check the interfaces:

```text
show ip interface brief
```

6. Enter configuration mode:

```text
configure terminal
```

7. Select the required interface:

```text
interface gigabitEthernet 0/x
```

8. Enable the interface:

```text
no shutdown
```

9. Exit configuration mode:

```text
exit
```

or:

```text
end
```

10. Run `show ip interface brief` again and check the interface status.

This was my first practical troubleshooting experience with Cisco IOS in Packet Tracer.

## Final Result

The final Packet Tracer topology was brought up successfully, with the connections showing green in the completed network.

## Files

For this day, the lab folder contains:

- `Day-01-Lab-Question.pkt` — original lab/question file
- `Day-01-Lab-Solution.pkt` — completed Packet Tracer lab
- `Screenshot-Final-Network-Setup.png` — screenshot of the completed topology
- `Day_1.md` — notes/working file

## Takeaway

Day 1 gave me a basic understanding of the main network devices and their roles, followed by my first hands-on Cisco Packet Tracer setup. The most useful practical part was learning how to check router interfaces and bring an administratively down interface back up using `no shutdown`.

## Course Progress

**Day 01 completed — Network Devices**
