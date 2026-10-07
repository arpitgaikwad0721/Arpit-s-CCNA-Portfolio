# Day 17 -- VLANs Part 2: Trunk Ports and 802.1Q

> **Relearning Note:** Day 17 originally included both theory and a
> Packet Tracer lab, but I did not understand the VLAN concepts well
> enough during the first attempt. This record is therefore written as a
> **relearning-focused note**.
>
> This file intentionally covers only the **first half of the Day 17
> material** from the uploaded slides, focusing on trunk ports, VLAN
> tagging, 802.1Q, native VLAN fundamentals, and the beginning of trunk
> configuration. Subnetting is not covered here because it will be
> revised separately.

## 📚 Theory

### 1. Trunk Ports

A **trunk port** is used to carry traffic belonging to **multiple VLANs
over a single physical interface**.

In a small network with only a few VLANs, it may be possible to use a
separate interface for each VLAN. However, as the number of VLANs
increases, this becomes inefficient because:

-   It wastes switch/router interfaces.
-   A router may not have enough physical interfaces for every VLAN.
-   Multiple VLANs can instead share one trunk link.

**Main idea:**

> **Access port → normally carries traffic for one VLAN**\
> **Trunk port → carries traffic for multiple VLANs**

The Day 17 topology shows this idea using multiple VLANs such as VLAN
10, VLAN 20, and VLAN 30 between switches and toward the router.

### 2. Why Trunk Ports Are Needed

Consider a network containing:

-   VLAN 10 -- Engineering
-   VLAN 20 -- HR
-   VLAN 30 -- Sales

If every VLAN required its own physical link between two switches, three
separate links would be needed.

A trunk allows these VLANs to share **one physical link**.

This makes the network more scalable and avoids wasting interfaces.

### 3. Tagged vs Untagged Traffic

When a switch sends frames over a trunk link, it **tags the frames** so
the receiving switch can determine which VLAN the frame belongs to.

  Port Type     VLAN Tagging
  ------------- --------------
  Access port   Untagged
  Trunk port    Tagged

A useful way to remember this:

> **Trunk = multiple VLANs = VLAN identification is required.**

The course example illustrate this with VLAN 10, VLAN 20, and VLAN 30
traffic travelling across the trunk between SW1 and SW2.

------------------------------------------------------------------------

## 4. VLAN Tagging Protocols

The slides introduce two main trunking protocols:

1.  **ISL (Inter-Switch Link)**
2.  **IEEE 802.1Q**

### ISL

ISL is:

-   An older Cisco proprietary trunking protocol.
-   Created before IEEE 802.1Q.
-   No longer important for modern CCNA practice according to the course
    material.

### IEEE 802.1Q

IEEE 802.1Q is:

-   An industry-standard VLAN tagging protocol.
-   Created by the IEEE.
-   The tagging method that should be focused on for CCNA.

The course specifically states that **802.1Q is the protocol to learn
for CCNA**.

------------------------------------------------------------------------

## 5. Ethernet Frame and 802.1Q Tag

A normal Ethernet frame contains an Ethernet header, the packet, and an
Ethernet trailer.

The Ethernet header includes fields such as:

-   Preamble
-   SFD (Start Frame Delimiter)
-   Destination
-   Source
-   Type/Length

When 802.1Q tagging is used, an additional **802.1Q tag** is inserted
into the Ethernet frame.

### Where is the 802.1Q tag inserted?

The 802.1Q tag is inserted:

``` text
Source | 802.1Q Tag | Type/Length
```

More specifically, it is placed **between the Source and Type/Length
fields**.

The 802.1Q tag is:

-   **4 bytes**
-   **32 bits**

It contains two main fields:

-   **TPID -- Tag Protocol Identifier**
-   **TCI -- Tag Control Information**

The TCI contains three sub-fields:

-   PCP
-   DEI
-   VID

------------------------------------------------------------------------

## 6. TPID -- Tag Protocol Identifier

**TPID** stands for **Tag Protocol Identifier**.

Important points:

-   Size: **16 bits (2 bytes)**\*
-   Value: **0x8100**
-   `0x` indicates hexadecimal.
-   The value `0x8100` indicates that the frame contains an **802.1Q
    tag**.

``` text
TPID = 0x8100
```

For CCNA revision, remember:

> **TPID identifies the frame as an 802.1Q-tagged frame.**

------------------------------------------------------------------------

## 7. PCP -- Priority Code Point

**PCP** stands for **Priority Code Point**.

Important points:

-   Size: **3 bits**
-   Used for **Class of Service (CoS)**.
-   It can be used to prioritize important traffic when the network is
    congested.

``` text
PCP → Traffic priority / Class of Service
```

------------------------------------------------------------------------

## 8. DEI -- Drop Eligible Indicator

**DEI** stands for **Drop Eligible Indicator**.

Important points:

-   Size: **1 bit**
-   Indicates frames that can be dropped if the network becomes
    congested.

``` text
DEI → Indicates whether a frame is eligible to be dropped during congestion
```

------------------------------------------------------------------------

## 9. VID -- VLAN ID

**VID** stands for **VLAN ID**.

Important points:

-   Size: **12 bits**
-   Identifies the VLAN to which the frame belongs.

Because the field is 12 bits:

``` text
2^12 = 4096
```

The theoretical range is:

``` text
0 – 4095
```

According to the slides:

-   VLAN 0 is reserved.
-   VLAN 4095 is reserved.
-   Therefore, the usable VLAN range is:

``` text
1 – 4094
```

### Easy way to remember

``` text
TPID → Is this an 802.1Q-tagged frame?
PCP  → Priority
DEI  → Can it be dropped during congestion?
VID  → Which VLAN?
```

------------------------------------------------------------------------

## 10. VLAN Ranges

The VLAN range **1--4094** is divided into two sections:

  VLAN Range   Type
  ------------ ----------------
  1--1005      Normal VLANs
  1006--4094   Extended VLANs

The course also note that some older devices cannot use the extended
VLAN range, while modern switches can generally be expected to support
it.

For CCNA revision, remember the two ranges:

``` text
Normal VLANs   → 1–1005
Extended VLANs → 1006–4094
```

------------------------------------------------------------------------

## 11. Native VLAN

802.1Q has a feature called the **native VLAN**.

The native VLAN is important because traffic belonging to the native
VLAN is handled differently from normally tagged trunk traffic.

### Important points

-   The default native VLAN is **VLAN 1** on trunk ports.
-   It can be manually changed on a trunk port.
-   Frames belonging to the native VLAN are sent **without an 802.1Q
    tag**.
-   When a switch receives an **untagged frame on a trunk**, it assumes
    the frame belongs to the native VLAN.
-   The native VLAN must **match on both ends of a trunk link**.

### Example

Suppose:

``` text
SW1 Native VLAN = 10
SW2 Native VLAN = 10
```

An untagged frame received on the trunk is interpreted as belonging to
VLAN 10.

The course demonstrate that when the native VLANs match, the frame can
be correctly understood by both switches.

### What happens if the native VLANs do not match?

For example:

``` text
SW1 Native VLAN = 30
SW2 Native VLAN = 10
```

An untagged frame sent by SW1 is interpreted by SW2 as belonging to VLAN
10.

This creates a mismatch because the two switches do not agree about
which VLAN untagged traffic belongs to.

The course also show the opposite problem: if a frame is tagged with
VLAN 30 when VLAN 30 is supposed to be the native VLAN, the receiving
switch can discard it because native VLAN traffic is expected to be
untagged.

### Key rule

> **The native VLAN must match on both sides of a trunk.**

------------------------------------------------------------------------

# 💻 Cisco IOS Commands

## 1. Enter the interface

``` text
enable
configure terminal
interface g0/0
```

The interface must be selected before configuring it as a trunk.

## 2. Configure the interface as a trunk

``` text
switchport mode trunk
```

This configures the switch interface as a trunk port.

On switches where the trunk encapsulation must be manually selected, the
course show configuring 802.1Q first:

``` text
switchport trunk encapsulation dot1q
switchport mode trunk
```

The course note that many modern switches support only 802.1Q, in which
case manually selecting the encapsulation is not necessary.

## 3. Verify trunk interfaces

``` text
show interfaces trunk
```

This command is used to verify trunk ports.

The output can show information such as:

-   Port
-   Mode
-   Encapsulation
-   Trunking status
-   Native VLAN
-   VLANs allowed on the trunk
-   VLANs allowed and active

## 4. Configure the VLANs allowed on a trunk

The slides show:

``` text
switchport trunk allowed vlan 10,30
```

This limits the trunk to the specified VLANs.

For example:

``` text
switchport trunk allowed vlan 10,30
```

means VLAN 10 and VLAN 30 are included in the allowed VLAN list.

### Adding a VLAN

The course also introduce the `add` option:

``` text
switchport trunk allowed vlan add 20
```

This adds VLAN 20 to the existing allowed VLAN list.

### Other options introduced in the slides

``` text
switchport trunk allowed vlan all
switchport trunk allowed vlan except <vlan-list>
switchport trunk allowed vlan none
switchport trunk allowed vlan remove <vlan-list>
```

  Option     Meaning
  ---------- -------------------------------------------
  `add`      Add VLANs to the current list
  `all`      Allow all VLANs
  `except`   Allow all VLANs except the specified ones
  `none`     Allow no VLANs
  `remove`   Remove VLANs from the current list

For this relearning stage, the most important commands to remember are:

``` text
switchport mode trunk
show interfaces trunk
switchport trunk allowed vlan 10,30
switchport trunk allowed vlan add 20
```

## 5. `show vlan brief` vs `show interfaces trunk`

A very important distinction from the slides:

``` text
show vlan brief
```

shows the **access ports assigned to VLANs**.

It does **not** show which trunk ports allow those VLANs.

For trunk verification, use:

``` text
show interfaces trunk
```

### Remember

``` text
show vlan brief
        ↓
Access-port VLAN assignments

show interfaces trunk
        ↓
Trunk ports and allowed VLANs
```

------------------------------------------------------------------------

# 🧪 Packet Tracer / Practical Application

The uploaded Day 17 material contains network topology and
trunk-configuration examples, but it does **not** contain separate
step-by-step Packet Tracer lab instructions or lab screenshots.

Therefore, this record does not invent a separate lab procedure.

The practical topology shown in the slides contains:

-   SW1
-   SW2
-   R1
-   VLAN 10 -- Engineering
-   VLAN 20 -- HR
-   VLAN 30 -- Sales

The important practical concept demonstrated is that a trunk link can
carry traffic for multiple VLANs between network devices.

The course also show that VLAN 20 does not need a separate SW1--SW2 link
when there are no VLAN 20 PCs connected to SW1; inter-VLAN communication
can instead involve R1.

------------------------------------------------------------------------

# 🧠 What I Need to Remember

-   A **trunk port carries multiple VLANs over one physical interface**.
-   Trunk links use **VLAN tagging** so the receiving switch knows which
    VLAN a frame belongs to.
-   **Access ports are untagged** and **trunk ports are tagged**.
-   For CCNA, focus on **IEEE 802.1Q (dot1q)** rather than ISL.
-   An 802.1Q tag is **4 bytes / 32 bits**.
-   The 802.1Q tag is inserted between the **Source** and
    **Type/Length** fields.
-   **TPID = 0x8100**.
-   **PCP = priority / Class of Service**.
-   **DEI = Drop Eligible Indicator**.
-   **VID identifies the VLAN**.
-   The VID is **12 bits**.
-   Usable VLAN IDs in the slide material are **1--4094**.
-   Normal VLANs: **1--1005**.
-   Extended VLANs: **1006--4094**.
-   The native VLAN is **VLAN 1 by default**.
-   Native VLAN frames are **untagged**.
-   An untagged frame received on a trunk is assumed to belong to the
    **native VLAN**.
-   The native VLAN must **match on both ends of the trunk**.
-   `show vlan brief` is for access-port VLAN assignments.
-   `show interfaces trunk` is for checking trunk ports.
-   `switchport mode trunk` configures a trunk.
-   `switchport trunk allowed vlan ...` controls which VLANs are allowed
    on the trunk.

------------------------------------------------------------------------

# ✅ Key Takeaways / Revision Checklist

-   [ ] I understand **why trunk ports are needed**.
-   [ ] I can explain the difference between an **access port and a
    trunk port**.
-   [ ] I understand why VLAN traffic needs to be identified across a
    trunk.
-   [ ] I know that **802.1Q is the trunking protocol to focus on for
    CCNA**.
-   [ ] I know where the **802.1Q tag** is inserted in an Ethernet
    frame.
-   [ ] I can identify **TPID, PCP, DEI, and VID**.
-   [ ] I remember that **TPID = 0x8100**.
-   [ ] I understand that **VID identifies the VLAN**.
-   [ ] I know the VLAN ranges **1--1005** and **1006--4094**.
-   [ ] I understand the basic purpose of the **native VLAN**.
-   [ ] I remember that **native VLAN traffic is untagged**.
-   [ ] I know that the **native VLAN must match on both sides of a
    trunk**.
-   [ ] I can use `show interfaces trunk` to verify trunk configuration.
-   [ ] I understand the basic purpose of
    `switchport trunk allowed vlan`.

------------------------------------------------------------------------

## 📌 Scope of This Day 17 Record

This record intentionally stops after covering the material through the
initial trunk configuration and allowed-VLAN commands. The later
**Router-on-a-Stick (ROAS)** material and remaining Day 17 topics are
intentionally left for the next revision stage.

**Subnetting/subnet revision is intentionally kept separate from this
record.**
