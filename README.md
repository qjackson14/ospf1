<p align="center">




<h1>Video Demonstration</h1>

- ### [YouTube: Day 26 OSPF Pt. 1](https://www.youtube.com/watch?v=3XENb67temY)

<h2>Environments and Technologies Used</h2>

- Jeremy's IT Lab Youtube Channel
- Cisco Packet Tracer
  

  
<h2>Operating Systems Used </h2>

- Cisco IOS


<h2>Step-by-Step</h2>

- 🟢 <b>Step 1: Basic Configuration (Prerequisite)</b>

Set hostname:

hostname R1

Configure IP addresses on all interfaces

Enable interfaces:

no shutdown

- 🟢 <b>Step 2: Configure Loopback Interfaces</b>

On Each Router (R1–R4)

Example (R4)

enable

conf t

interface loopback 0

ip address 4.4.4.4 255.255.255.255

Repeat:

R1 → 1.1.1.1 /32

R2 → 2.2.2.2 /32

R3 → 3.3.3.3 /32

🔍 Verify

show ip interface brief

show interface loopback 0

✅ Loopback should be:

up/up

Mask = /32

- 🟢 <b>Step 3: Configure OSPF</b>

🔹 On R4 (Enable OSPF on ALL interfaces – Lab shortcut)

router ospf 4

network 0.0.0.0 255.255.255.255 area 0

passive-interface g0/0

passive-interface loopback0

🔹 On R3 (Specific interface configuration)

router ospf 3

network 10.0.13.2 0.0.0.0 area 0

network 10.0.34.1 0.0.0.0 area 0

network 3.3.3.3 0.0.0.0 area 0

passive-interface loopback0

🔹 On R2 (Wildcard range method)

router ospf 2

network 10.0.0.0 0.0.255.255 area 0

network 2.2.2.2 0.0.0.0 area 0

passive-interface loopback0

🔹 On R1 (Precise networks, excluding Internet link)

router ospf 1

network 10.0.12.0 0.0.0.3 area 0

network 10.0.13.0 0.0.0.3 area 0

network 1.1.1.1 0.0.0.0 area 0

passive-interface loopback0

- 🟢 <b>Step 4: Configure Default Route (R1 Only)</b>

1. Create static default route

ip route 0.0.0.0 0.0.0.0 203.0.113.2

2. Advertise it into OSPF

router ospf 1

default-information originate

- 🟢 <b>Step 5: Verify OSPF</b>

🔍 Check protocols

show ip protocols

🔍 Check neighbors

show ip ospf neighbor

🔍 Check LSDB

show ip ospf database

🔍 Check interfaces

show ip ospf interface

- 🟢 <b>Step 6: Verify Routing Tables</b>

On R2, R3, R4:

show ip route
