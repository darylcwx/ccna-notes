## Table of Contents

```table-of-contents
title: 
style: nestedList
minLevel: 0 
maxLevel: 0 
exclude: 
includeLinks: true 
hideWhenEmpty: false 
debugInConsole: false 
```

## 1.0 Network Fundamentals

### 1.1 Components

- **Endpoints** (devices)
- **Intermediary/Network devices** (switches, routers)
- **Services** (FTP servers)
- **Media Types**
	- Wired: Ethernet, Serial
	- Wireless: [WLAN (802.11), Bluetooth (802.15), WiMAX (802.16)]

### 1.2 LAN, WAN, WLAN

#### 1.2.1 LAN

- Connects devices within an area
- Uses switches to forward data frames based on MAC
- Typically wired with ethernet

#### 1.2.2 VLAN (L2)

- [3.2 VLAN](Cisco%20Hands%20On.md#3.2%20VLAN)
- VLAN maps to a unique subnet (L3)
- Logical segmentation into separate broadcast domains
- Needs router to communicate across other VLANs
- Security, performance, management
- IEEE 802.1Q Tag Format (VLAN tagging protocol) - 4 bytes
	- [Preamble, SFD, Desti MAC, Source MAC, **VLAN Tag**, Type/Length, Payload, FCS]
	- **TPID**: 16 bits, `0x8100`
- **Priority Code Point (PCP)**: 3 bits, Class of Service (CoS) priority ^52f974
	- **Drop Eligible Indicator (DEI)**: 1 bit
	- **VLAN ID**: 12 bits, VLAN number (1- 4094)
- VLAN Range
	- Normal VLANs (1-1005)
	- Extended VLANs (1006-4094)
	- Older devices cannot use ext. range
- dot1q has native VLAN (VLAN 1)
	- native VLAN need to be same across switches, best to change to unused VLAN
	- native VLAN is untagged

#### 1.2.3 WAN

- Connects multiple LANs, Managed by ISP or SIs
- **Leased lines**
	- Hub and Spoke
		- Central control
	- Uses serial
- **Multi Protocol Label Switching (MPLS)**
	- Shared infrastructure, ISP in middle
	- CE (customer), PE (provider) , P
	- Uses **labels** for make forwarding decisions
	- Layer 3: Office A will learn Office B's routes via OSPF
	- Layer 2: A and B will OSPF peer directly
- **Digital Subscriber Line (DSL)**
	- Modem (phone lines)
- **Cable Internet (CATV)**
- Redundant Internet Connections
	- Single homed (1-to-1), Dual homed (2-to-1), Multihomed (1-to-2), Dual Multihomed (2-to-2)
	- Internet VPNs
	- Internet as a WAN = no security
	- Site-to-Site using **IPsec**
		- VPN tunnel (2 endpoints only)
		- Encrypt, +VPN header, +IP header, Decrypt
		- Limitations
			- IPsec no broadcast/multicast (no OSPF)
				- Generic Routing Encapsulation (GRE) over IPsec
					- GRE flexibility + IPsec security
					- Encap(Encrypt(Encap(packet w GRE header and new IP header)) w IPsec VPN header and new IP header)
					- Sort of like double hashing
			- Full mesh = hard
				- Dynamic Multipoint VPN (DMVPN)
					- IPsec tunnels to a Hub
					- Hub spreads routes
	- Remote Access using TLS
		- End devices to company resources via VPN tunnels

#### 1.2.4 WLAN

- **Access Points (APs)**
	- Connects wireless clients to wired LAN
	- Can be connected to switch, or another AP, or have SIM card
	- Can be cloud-based AP (NaaS)
- **WLAN controller**
- **BYOD**
	- [Cost savings, Reduced hardware expenses, Work from anywhere, Secure remote collaboration, Employee satisfaction]
- **VPN**
	- Virtual extension of LAN over public WAN
	- SSL & IPsec [L2, L3, L4, L3, L4]
	- L2-in-ip [L2, L3, L2, L3, L4]

#### 1.2.6 Virtualization

- Run multiple OS on 1 server
- Why?
	- Partitioning (run multiple OS, divide system resources)
	- Isolation (fault and security isolation at hardware level)
	- Encapsulation (saves entire state of VM)
	- Hardware Independence (migratebility)
- Type 1 hypervisor (VMM)
	- Manages and allocates CPU/RAM to each VM
	- Used in DCs, Runs on hardware, also called bare-metal/native hypervisor
- Type 2 hypervisor (hosted)
	- Runs on OS
- vSwitches connect to physical NIC of server

##### Containers

- Contain pre-packaged apps (bins/libs/deps)
- Run on Container Engine
- Orchestrator to deploy, manage containers
	- Kubernetes
	- Docker Swarm
- VMs vs Containers
	- VMs slow to boot, require more disk space, resources but portable and more isolated

#### 1.2.7 Cloud

- Types
	- On-Premise
	- Colocation
		- DCs rent space for client infrastructure
- **5 essential characteristics (OBRRM)**
	- [On-demand self-service, Broad network access, Resource pooling, Rapid elasticity, Measured service]
- **3 service models**
	- SaaS (M365, gSuite), PaaS (AWS Lambda), IaaS (EC2)
- **4 deployment models
	- Private (orgs), Community (group of orgs), Public (open to public), Hybrid (combi of any 3)
- Cost
	- Capital expenses (of buying hardware/software) reduced
	- Global scale (rapid)
	- Speed/agility (provided on demand within minutes)
	- Productivity (no more procure, rack, cable, install, update)
	- Reliability (backups, pull data, disaster recovery)

#### 1.2.8 Other

- **Data Center**
	- [Availability, Resiliency, Scalability]
	- Enterprise = [Connectivity, Security]
	- DC Switches (a lot faster, massive, low-latency)
	- Small Form-Factor Pluggable (1-800 gigabits/s)
- **Web Servers, DMZ, Extranet**
	- Web servers in DMZ, a controlled zone between Internet and internal network
	- Usually a subnet the hosts public services

### 1.3 Architectures

#### Terminologies

- Star (topology pattern)
- Full mesh (each connected to all)
- Partial mesh (some connected but not all)

#### 1.3.1 2 & 3 Tier LAN

![|250](assets/Fundamentals%20of%20Networking/img-20251121112447321.png)

- **Access Layer**
	- Many PoE-enabled ports to hosts
	- Runs: QoS, Port Security, DAI
- **Distribution Layerc**
	- Aggregates access layer via SVIs that hosts use as DGW
	- Runs: OSPF, STP
	- Leads to WAN, internet

![|450](assets/Fundamentals%20of%20Networking/img-20251121113640894.png)

- **Core Layer (if Distribution Layers > 3)**
	- Speed for large networks
	- Avoid CPU-intensive operations
	- Layer 3 only

#### 1.3.2 Spine-Leaf/Close (DC)

- Traditionally, 3-tier
- North-South = vertical
- East-West = lateral
- Virtual servers = more East-West = bottlenecks
- Each leaf is full mesh with each spine, and vice versa
- Path taken = random to load balance traffic

#### 1.3.3 SOHO (Small/Home Office)

- One device that does all (routing, switching, firewall, AP, modem)

### 1.4 Communication over LAN

#### 1.4.1 Physical Media Types

- **Twisted Pair (ethernet)**
	- Twisted because protects against electromagnetic interference
	- UTP (Unshielded Twisted Pair): 100m
	- STP (Shielded Twisted Pair)
		- Reduce interference
	- S/STP: Screened STP
		- More shields
	- Categories [CAT5, 5e, 6, 6a, 7, 8]
	- **Pins**:
		- FastEthernet: Tx - pins 1, 2, Rx - 3, 6
		- GigabitEthernet: all 4 pairs used (1-8)
	- **Cable types:**
		- Straight-through: host ↔ switch
		- Crossover: switch ↔ switch / host ↔ host / router ↔ router (if auto MDI-X disabled)
- **Optical Fiber**
	- [core, cladding, buffer]
	- [9, 125, 250] µm
	- Types
		- Multi-mode: [LED, slow BW, 2km, $]
		- Single-mode: [laser, high BW, 40+km, $$$]
	- Connected to Small Form-Factor Pluggables (SFP)
- **Coaxial Cable**
	- Copper core, Legacy use

#### 1.4.2 Ethernet Media Table

| IEEE Std | Media Type                 | Max Speed   | Cabling / Fiber   | Max Distance | Pairs Used | Notes                                          |
| -------- | -------------------------- | ----------- | ----------------- | ------------ | ---------- | ---------------------------------------------- |
| 802.3i   | 10BASE-T                   | 10 Mbps     | UTP Cat3/Cat5     | 100 m        | 2          | Original Ethernet (Baseband over twisted pair) |
| 802.3u   | 100BASE-TX                 | 100 Mbps    | UTP Cat5          | 100 m        | 2          | Fast Ethernet, 2 pairs                         |
| 802.3ab  | 1000BASE-T                 | 1 Gbps      | UTP Cat5e/6       | 100 m        | 4          | Gigabit Ethernet, all pairs used               |
| 802.3an  | 10GBASE-T                  | 10 Gbps     | UTP Cat6a/7       | 100 m        | 4          | 10-Gigabit over copper                         |
| 802.3z   | 1000BASE-SX                | 1 Gbps      | Multimode fiber   | 550 m        | N/A        | Short-range fiber (MMF)                        |
| 802.3z   | 1000BASE-LX                | 1 Gbps      | Single-mode fiber | 10 km        | N/A        | Long-range fiber (SMF)                         |
| 802.3ae  | 10GBASE-SR                 | 10 Gbps     | Multimode fiber   | 300 m        | N/A        | Short-range fiber                              |
| 802.3ae  | 10GBASE-LR                 | 10 Gbps     | Single-mode fiber | 10 km        | N/A        | Long-range fiber                               |
| 802.3ba  | 40GBASE-SR4 / 100GBASE-LR4 | 40/100 Gbps | MMF / SMF         | Varies       | N/A        | Data center / backbone links                   |

#### 1.4.3 Communication Types

- **Unicast**: one-to-one (flooding)
- **Broadcast**: one-to-all, FFFF:FFFF:FFFF (ARP)
- **Anycast**: one-to-nearest/best (CDNs)
- **Multicast**: one-to-selected (netflix)

##### Multicast

| **Purpose**            | **IPv6 Address** | **IPv4 Address** |
| ---------------------- | ---------------- | ---------------- |
| All nodes/hosts        | `FF02::1`        | `224.0.0.1`      |
| All routers            | `FF02::2`        | `224.0.0.2`      |
| All OSPF routers       | `FF02::5`        | `224.0.0.5`      |
| All OSPF DRs/BDRs      | `FF02::6`        | `224.0.0.6`      |
| All RIP routers        | `FF02::9`        | `224.0.0.9`      |
| All EIGRP routers      | `FF02::A`        | `224.0.0.10`     |
| HSRP, GLBP (multicast) |—| `224.0.0.102`    |
| VRRP (multicast)       |—| `224.0.0.18`     |

#### 1.4.4 Host-to-Host Packet Transfer Process (same LAN)

1. Host A wants to send data to Host B:
	- Host A has the IP address of Host B but needs Host B’s MAC address to send an Ethernet frame.
2. Host A checks its ARP cache:
	- Looks up if it already knows Host B's MAC address.
	- If MAC address is found, proceed to step 5.
	- If MAC address not found, proceed to step 3.
3. Host A sends an ARP Request:
	- ARP request is a broadcast frame (ff:ff:ff:ff:ff:ff) at Layer 2.
	- ARP request contains Host A’s IP & MAC, asking “Who has IP X.X.X.X? Tell me.”
4. ARP request arrives at the switch:
	- Switch receives the broadcast frame on the port connected to Host A.
	- Switch checks its MAC address table:
	- Adds/updates entry for Host A's MAC address and associated port.
	- Since broadcast frames must be sent to all devices, the switch floods the ARP request out on all other ports (except the incoming port).
5. Host B receives ARP request:
	- Checks if the IP in ARP request matches its own.
	- If yes, Host B sends an ARP reply directly to Host A’s MAC address.
	- ARP reply is a unicast frame containing Host B’s MAC address.
6. ARP reply arrives at the switch:
	- Switch receives ARP reply on port connected to Host B.
	- Switch updates its MAC address table with Host B’s MAC and port.
	- Switch forwards ARP reply unicast frame only to the port where Host A is connected.
7. Host A receives ARP reply:
	- Caches Host B's MAC address in its ARP table.
	- Now Host A can send the actual data packet encapsulated inside an Ethernet frame addressed to Host B's MAC.
8. Host A sends the Ethernet frame:
	- Frame contains Host B’s MAC as destination and Host A’s MAC as source.
	- Frame arrives at switch.
9. Switch processes the frame:
	- Checks MAC address table for destination MAC (Host B’s).
	- If destination MAC is in the table, forwards frame out the corresponding port.
	- If destination MAC is unknown, floods the frame out all ports except incoming port (rare here since learned via ARP).
10. Host B receives the Ethernet frame:
	- Processes the Ethernet frame.
	- Extracts the IP packet and processes it up the stack.
11. Higher layer protocols handle communication:
	- For TCP, acknowledgments and retransmissions handle reliability.
	- For UDP, best-effort delivery continues without guaranteed retransmission.

#### 1.4.5 Address Resolution Protocol (ARP) - 0x0806

![](assets/Fundamentals%20of%20Networking/img-20251022165531530.png)

1. A → B (knows IP only)
2. **Check ARP cache:** known ? send : send L2 broadcast frame (ff:ff:ff:ff:ff:ff)
3. **SW1:** receives broadcast, cache A's MAC → floods broadcast
4. **SW2:** receives broadcast, cache A's MAC → floods broadcast
5. **B:** receives ARP request → IP match? yes → send unicast ARP reply
6. **SW2:** receives ARP reply → cache B's MAC → have A's MAC, forward via port to SW1
7. **SW1**: receives ARP reply → cache B's MAC → forward to A's port
8. **A:** receives ARP reply → cache B's MAC → send actual Ethernet frame
9. **SW1**: forward frame based on MAC table
10. **SW2:** forward frame based on MAC table
11. **B**: receive frame → process packet

### 1.4 Addressing (L3/L2)

#### 1.4.1 MAC Address - 48 Bits (L2)

- 6 bytes: 3 bytes Organizationally Unique Identifier (OUI), 3 bytes device
- Globally unique Hexadecimal (0-9, A-F)
- Broadcast = `FFFF:FFFF:FFFF`
- Auto clears "aging" after 5 minutes

##### Ethernet Frame (64 bytes)

- **Header (14 bytes)**

| Desti   | Source  | Length/Type                                     |
| ------- | ------- | ----------------------------------------------- |
| 6 bytes | 6 bytes | 2 bytes                                         |
|         |         | LENGTH <= 1500 <= x <= 1536 <= TYPE (IPv4/IPv6) |

- **Payload (46 bytes)**
	- Padding bytes if needed
- **Trailer (4 bytes)**
		- Frame Check Sequence (FCS)
		- Detects corrupted data by running Cyclic Redundancy Check (CRC) algorithm

**Not actually part of Ethernet Frame**

| Preamble             | Start Frame Delimiter (SFD) |
| -------------------- | --------------------------- |
| 7 bytes              | 1 bytes (10101011)          |
| sync receiver clocks | end of preamble             |

#### 1.4.2 IPv4 Addressing - 32 Bits (L3)

- 4 bytes/octets, 8 bits per octet
- 8 bits = max value 255
- Need configure IPsec

##### Headers (20-60 bytes)

- Max Transmission Unit (MTU) = 1500 bytes (1480 bytes due to 4-byte increments)
	- Fragmented is over

| Field                                                                                                | Size (bits) | Description                                                 |
| ---------------------------------------------------------------------------------------------------- | ----------- | ----------------------------------------------------------- |
| Version                                                                                              | 4           | binary = 0100                                               |
| Internet Header Length (IHL)                                                                         | 4           | header length in **4 byte increments** (5-15 = 20-60 bytes) |
| Type of Service (ToS) = Differentiated Services Code Point (DSCP) + Explicit Congestion Notification | 6 + 2       | priority + congestion                                       |
| Total Length                                                                                         | 16          | total packet size in bytes (header + data, <=65535)         |
| =======================                                                                              | ======      | =====================================                       |
| Identification                                                                                       | 16          | fragmentation and reassembly                                |
| Flags                                                                                                | 3           | fragments (0, Don't Frag, More Frag)                        |
| Fragment Offset                                                                                      | 13          | position of fragment                                        |
| =======================                                                                              | ======      | =====================================                       |
| Time To Live (TTL)                                                                                   | 8           | max hops before packet dropped                              |
| Protocol                                                                                             | 8           | TCP - 6, UDP - 17, ICMP - 1, OSPF - 89)                     |
| Header Checksum                                                                                      | 16          | error-checking for header (hash)                            |
| =======================                                                                              | ======      | =====================================                       |
| Source Address                                                                                       | 32          | IPv4 address of sender                                      |
| =======================                                                                              | ======      | =====================================                       |
| Destination Address                                                                                  | 32          | IPv4 address of receiver                                    |
| =======================                                                                              | ======      | =====================================                       |
| Options (if IHL > 5)                                                                                 | 0-320       | extra options for control/debugging                         |
| Padding                                                                                              | Variable    | aligns header to 32-bit boundary                            |

^2cd22d

##### Subnet Classes

- `A B C, easy as 1 2 3` (Class A = 0XXX, Class B = 10XX, Class C = 110XX)
- Some are reserved (127.0.0.1)
- ==Subnet prefix must match class==

| Class | First Octet Range | Prefix Length | Leading Bits | Default Mask    | Purpose / Notes                   |
| ----- | ----------------- | ------------- | ------------ | --------------- | --------------------------------- |
| A     | 1–126             | /8            | 0            | `255.0.0.0`     | large networks (127 for loopback) |
| B     | 128–191           | /16           | 10           | `255.255.0.0`   | medium networks                   |
| C     | 192–223           | /24           | 110          | `255.255.255.0` | small networks                    |
| D     | 224–239           |               | 1110         | N/A             | multicast                         |
| E     | 240–255           |               | 1111         | N/A             | experimental / reserved           |

##### Binary Calculation

| Base    | 2^8 | 2^7 | 2^6 | 2^5 | 2^4 | 2^3 | 2^2 | 2^1 |
| ------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Sum     | 128 | 64  | 32  | 16  | 8   | 4   | 2   | 1   |
| Cum     | 128 | 192 | 224 | 240 | 248 | 252 | 254 | 255 |
| Binary  | 1   | 0   | 1   | 1   | 1   | 0   | 0   | 1   |
| Decimal | 128 | 0   | 32  | 16  | 8   | 0   | 0   | 1   |

| Example           | IP 1               | IP 2             | How                    |
| ----------------- | ------------------ | ---------------- | ---------------------- |
| IP address        | `192.168.24.19/24` | `10.0.1.15/8`    | N.A.                   |
| Subnet mask       | `255.255.255.0`    | `255.0.0.0`      | bits taken for network |
| Network address   | `192.168.24.0`     | `10.0.0.0`       | smallest, host all `0` |
| Broadcast address | `192.168.24.255`   | `10.255.255.255` | largest, host all `1`  |
| Host addresses    | `192.168.24.19`    | `10.0.1.15`      | same                   |
| Default gateway   | `192.168.24.1`     | `10.0.0.1`       | first usable           |
|                   |                    |                  |                        |

##### Subnetting (Variable Length Subnet Masking - VLSM)

- Network: 192.168.10.0/26
- Subnet Mask: 255.255.255.192
- Block size (by CIDR): `2^(32 - prefix)`
	- /25 = 128 = 128 remaining IP (126 usable)
	- /26 = 192 = 64 remaining IP (62)
	- /27 = 224 = 32 remaining IP (30)
	- /28 = 240 = 16 remaining IP (14)
	- /29 = 248 = 8 remaining IP (6)
	- /30 = 252 = 4 remaining IP (2)
	- /31 = 254 = 2 remaining IP (0), usable only for point-to-point connection
	- /32 - used for one specific host
- Sub-subnet based on requirements
	- e.g. 1, "support >= 6 IPs per block", use /29, since 8 IPs per block
- *"For each borrowed bit, the number of subnets doubles, 2, 4, 8, 16"*

| Subnet (/26) | Network          | Usable   | Broadcast |
| ------------ | ---------------- | -------- | --------- |
| 1            | `192.168.10.0`   | .1-62    | .63       |
| 2            | `192.168.10.64`  | .65-126  | .127      |
| 3            | `192.168.10.128` | .129-190 | .191      |
| 4            | `192.168.10.192` | .193-254 | .255      |

- Examples
	- 172.28.228.144/18
		- 172.28.192.1–172.28.255.254
		- 3 Oct, 6b, 7th = blocks of 64
		- 64 LCM to hit 144 = 192
	- 172.28.228.144/21
		- 172.28.224.1–172.28.231.254
	- 172.28.228.144/23
		- 172.28.228.1–172.28.229.254
	- 172.28.228.144/25
		- 172.28.228.129–172.28.228.254
		- 172.28.228.144/29172.28.228.145–172.28.228.150

#### 1.4.3 IPv6 Addressing - 128 Bits, 0x86DD (L3)

- Larger address space
- Simpler header
- Security (built-in encryption) and mobility
- Transition
- No broadcast addresses
- 64 bits **network**, 64 bits **interface**
- Each hexa = 4 bits
- Example
	- `FE80:0000:0000:0000:0002:0000:0080:FBE8`
	- `FE80::2:0:0:FBE8`

##### Headers (40 bytes)

| Field               | Size (bits) | Description                    |
| ------------------- | ----------- | ------------------------------ |
| Version             | 4           | binary = 0110                  |
| Traffic Class       | 8           | priority / QoS                 |
| Flow Label          | 20          | special traffic 'flow'         |
| =============       | ======      | ====================           |
| Payload Length      | 16          | in bytes                       |
| Next Header         | 8           | next protocol (TCP, UDP, etc.) |
| Hop Limit           | 8           | max hops before discard (TTL)  |
| =============       | ======      | ====================           |
| Source Address      | 128         | sender IPv6 address            |
| =============       | ======      | ====================           |
| Destination Address | 128         | receiver IPv6 address          |

##### Address Assignment

- Network address
	- `300D:F2:B34:2100::1200:1/56`
		- 16, 32, 48; `2` = 4 bits, `1` = 4 bits, total = 56 bits
	- `2001:DB8:8B00:1:FB89:17B:20:11/93`
		- 16, 32, 48, 64, 80; `0` = 4 bits, `1` = 4 bits, `7` = 4, `B` = 1|011 -> 1|000, total 93 bits, 1000 = `8`
		- ∴, Network prefix = `2001:DB8:8B00:1:FB89:178::` #practice
- **EUI-64**
	- Converting MAC (48) into host ID (64)
	- Insert `FFFE` (16 bits) in the middle of MAC
	- Flip the `7th bit`, convert back to hexadec
	- 7th bit
		- Universal/Local bit (U/L bit)
			- 0 = Universally Administered Address (UAA)
			- 1 = Locally Administered Address (LAA)
		- EUI-64 reverses this

##### Address Types

| Type           | Prefix         | Usage / Notes                                                                                                                                    |
| -------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| Global Unicast | n/a (2000::/3) | public routable addresses. 48 bits from ISP, 16 for subnets, 64 for hosts                                                                        |
| Unique Local   | `fd00::/8`     | private internal networks (global ID)                                                                                                            |
| Link-Local     | `fe80::/10`    | auto-generated on IPv6-enabled interfaces via EUI-64. used for single-link communication: OSPFv3, next-hops, NDP (IPv6 ARP), router comms, SLAAC |
| Multicast      | `ff00::/8`     | one-to-many (e.g., routing updates, services)                                                                                                    |
| Anycast        | n/a            | assigned to multiple interfaces, lowest-cost routing                                                                                             |
| Loopback       | ::1            | testing local stack                                                                                                                              |
| Unspecified    | ::             | placeholder, no address assigned                                                                                                                 |

##### Multicast Address Scopes

- Interface-local (`ff01`): within device only
- Link-local (`ff02`): local subnet only
- Site-local (`ff05`): LAN only
- Org-local (`ff08`): self exp.
- Global (`ff0e`): self exp.

##### Neighbor Discovery Protocol (NDP)

- NS <u>multicast</u> more efficient that ARP's <u>broadcast</u>
- Stored in IPv6 neighbor table
- **State-Less Address Auto-config (SLAAC)**
	- 2 <u>discovery</u> message types
		- **Router Solicitation (RS)** = ICMPv6 Type 133
			- `ff02::2` (all routers)
			- Request for identification when new connection
		- **Router Advertisement (RA)** = ICMPv6 Type 134
			- `ff02::1` (all nodes)
			- Router announces itself
			- Sent periodically, even without RS
	- 2 methods
		- manual: `R1(config)#ipv6 address prefix/XX eui-64`
		- auto: `R1(config)#ipv6 address autoconfig` (also via eui-64)
- 2 <u>resolution</u> message types
	- **Neighbor Solicitation (NS)** = ICMPv6 Type 135
		- Solicited-Node Multicast Address (targeted multicast)
			- `ff02::1:ff` + last 6 hex digits of unicast address
	- **Neighbor Advertisement (NA)** = ICMPv6 Type 136
- **Duplicate Address Detection (DAD)**: <u>NS</u> & <u>NA</u>
	- Host checks if other devices using same IPv6 address
	- Performed any time `#no shut` or IPv6 address configured on an interface (regardless method)

### 1.5 Layered Models

#### 1.5.1 OSI Model

| Layer        | Explanation                                        | Analogy                                       |
| ------------ | -------------------------------------------------- | --------------------------------------------- |
| Application  | app interface                                      | letter to post office                         |
| Presentation | data formatting, encryption                        | translate language                            |
| Session      | manages connections                                | post office opens a comms line to destination |
| Transport    | reliable delivery (TCP/UDP)                        | package is tracked and delivery guaranteed    |
| Network      | routing across networks (core & distribution), IPs | best route for package delivery               |
| Data Link    | local delivery and switching (access)              | package handed to local delivery trucks       |
| Physical     | transmission of raw bits                           | roads, trucks, wires                          |

#### 1.5.2 Encapsulation

| Layer       | Items                    |
| ----------- | ------------------------ |
| Application | User data, Data header   |
| Transport   | + Segment header (ports) |
| Internet    | + Packet header (IPs)    |
| Link        | + Frame header (MACs)    |

#### 1.5.3 Layer 4 (TCP Vs UDP) #ports

- Services
	- Reliable data transfer
	- Error recovery
	- Data sequencing
	- Flow control
- Layer 4 addressing (port numbers)
	- App layer protocol
	- IANA says port numbers:
		- Well known: 0-1023
		- Registered: 1024-49151
		- PrivateE 49152-65535
- **TCP/IP**
	- 3-way: [SYN, SYN/ACK, ACK]
		- Establish first before traffic flows
	- 4-way: [FIN, ACK, FIN, ACK]
	- Guaranteed delivery
	- Resend if error, interrupting streaming/music
	- Sequencing
		- Random initial sequence number
		- Forward acknowledgement
	- Flow control (window size)
	- For downloads
	- [FTP data (20), FTP control (21), SSH (22), Telnet (23), SMTP (25) DNS(53), HTTP (80), POP3 (110), HTTPS (443)]
- **UDP**
	- No guaranteed delivery
	- Fast but no error-checking
	- For voice, video
	- [DNS(53), DHCP server (67), DHCP client (68), TFTP (69), SNMP agent (161), SNMP manager (162), Syslog (514)]

### 1.6 Wireless (IEEE 802.11)

- Within range ? receives all frames
	- Privacy
	- Carrier Sense Multiple Access with Collision **Avoidance** (CSMA/CA) for half-duplex
- Regulated by various international bodies
- Consider signal coverage
	- Range
	- Absorption, reflection, refraction, diffraction, scattering
- Interference
- 802.11, b, a, g, n, ac, ax
	- Service sets (SS)
		- Independent
		- Infrastructure
		- Mesh
		- All devices share same SSID
		- Independent Basic Service Set (IBSS) - "ad hoc network"

			- > =2 devices connect directly, for file transfer (AirDrop)

		- Basic Service Set (BSS)
			- BSSID = MAC of AP's radio, used to identify AP
			- Usable area = Basic Service Area (BSA)
		- Extended Service Set (ESS)
			- Large wireless LANs
			- Same SSID but unique BSSID, with own channel to avoid interference
			- 10-15% overlap to not lose connection when roaming
		- Mesh BSS (MBSS)
			- 1 radio BSS, 1 radio bridge traffic from AP to AP
			- At least 1 AP connected to wired network, called Root Access Point (RAP)
			- The rest called Mesh APs
	- Distribution System (wired network)
		- WLAN map to separate VLAN and connected to wired network via trunk
- AP Operational Mode
	- Repeater mode
		- Retransmit what it receives
		- 1 radio ? must same channel = low throughput
		- 2 radios ? 1 receive 1 transmit
	- Workgroup bBidge (WGB)
		- Acts as a wireless client of another AP, to connect wired devices to WLAN
		- Universal WGB (UWGB) - allows one device
		- WGB (cisco) - allows multiple wired clients
	- Outdoor Bridge
		- Specialized antennas to connect networks over long distance
- AP Deployment
	- Autonomous (no need WLC)
		- Config individually
		- Viable in small networks only
		- Trunk links
	- Lightweight
		- 'Real-time' operations like tx/rx traffic, encrypt/decrypt, beacon/probe
		- Other functions by WLC = split MAC architecture
		- 2 tunnels between each AP an WLC
			- Control (UDP 5246) - enc
			- Data (UDP 5247) - all traffic goes to WLC first (can enc via DTLS)
		- Easier to scale
		- Modes
			- Local: standard
			- FlexConnect: same by can switch to wired if WLC down
			- Sniffer: just sniff 802.11 frames and send them
			- Monitor: just receives 802.11 frames, to detect rogue devices and send de-auth
			- Rogue detector: gets list of suspects from WLC. uses list, listens to ARP, detect rogue devices
			- Spectrum Expert Connect (SE-C): RF analysis on all channels
			- Bridge/Mesh: Outdoor bridge
			- Flex+Bridge: Adds FlexConnect to Bridge/Mesh mode
	- Cloud-based
		- Autonomous APs managed by cloud
		- Meraki
			- Config, monitor, generate reports, etc.
			- Data traffic not sent to cloud but directly to wired network
			- Only management/control sent to cloud
- WLC Deployment (split-MAC)
	- Unified (Separate but central)
	- Cloud-based (WLC is a VM)
	- Embedded (Integrated in a switch)
	- Mobility Express (Integrated in AP)

#### 1.6.1 Radio Frequency

- Amplitude (strength), frequency (Hz), period (4Hz = 0.25s)
- Wi-Fi
	- 2.4GHz (better in open space, better penetration, more devices = more interference)
		- Multiple channels, don't use overlapping channels so no interference
		- Use 1, 6, 11 in honeycomb
	- 5 GHz
	- Wi-Fi 6 = 6 GHz

<hr>

#### 1.6.2 Wireless

Architectures

- 802.11 frame
	- 2b: Frame control
	- 2b: Duration/ID
	- 6b: Desti
	- 6b: Source
	- 6b: Receiver
	- 2b: Sequence control
	- 6b: Transmitter
	- 2b: QoS
	- 4b: HT Control
	- Variable size: Packet
	- 4b: FCS
- 802.11 Association Process
	- States
		- Not auth, not assoc
		- Auth, not assoc
		- Auth and assoc
	- 2 methods
		- Active scanning (probe)
		- Passive scanning (beacon messages from AP)
	- Message Types
		- Management
			- Probe req > Probe res
			- Auth req > Auth res
			- Assoc req > Assoc res
		- Control
			- Req to send (RTS)
			- Clear to sen (CTS)
			- ACK
		- Data

#### 1.6.3 Wireless Security

- Authentication
	- Open (starbucks wifi)
	- WEP (RC4 algo)
		- Shared key
		- 40b or 104b (add IV 24b) = 64b or 128b
		- Easily cracked
		- Challenge > Reply > Ack
	- Extensible Authentication Protocol (EAP)
		- 802.1X: limit access til auth
		- Supplicant: device
		- Authenticator: AP/WLC
		- Auth Server: RADIUS
		- Open > EAP auth
	- LEAP
		- Mutual auth via user/pass
		- Not secure
	- EAP-FAST (Flex Auth, Secure Tunnel)
		- PAC gen, Tunnel, Auth
	- PEAP
		- DigSig, Tunnel, Auth
		- MS-CHAP
	- EAP-TLS
		- Both DigSig, Tunnel, Auth
- Encryption/Integrity
	- 2 keys, 1 for unicast, 1 group key for receiving broadcasts
	- Message Integrity Check (MIC) (like hashing)
	- Methods
		- WEP
		- TKIP
			- MIC (incl. sender MAC), Key mixing algo, IV (48b), Seq no.
		- CCMP

			- > TKIP, new hardware only, AES counter mode, CBC-MAC (MIC)

		- GCMP

			- > CCMP, better throughput,  AES counter mode, GMAC (MIC)

- WPA Certifications
	- Supports 2 auth modes
		- Personal (SOHO)
			- PSK
		- Enterprise (802.1X)
			- Uses RADIUS
			- All EAP supported
	- WPA: TKIP, 802.1X, PSK
	- WPA2: CCMP, 802.1X, PSK
	- WPA3: GCMP, 802.1X, PSK,
		- PMF (protects 802.11 from eavesdropping)
		- SAE (protects 4 way shake)
		- Forward secrecy (can't decrypt after transmitted)

#### 1.6.4 Wireless Config

- WLCs connects to Switch via static LAG (no PAgP/LACP)
- DHCP option 43 if WLC not reachable by AP
- WLC Ports
	- Service (management)
	- Distribution System (data)
	- Console (RJ45)
	- Redundancy (for HA)
- WLC Interfaces
	- Management (telnet, SSH, HTTP/S, RADIUS auth, NTP, Syslog, CAPWAP tunnels, etc.)
	- Redundancy Mgmt (for HA)
	- Virtual Int (for DHCP req)
	- Service Port Int (bound to service port)
	- Dynamic Int (Map WLAN to VLAN)
- WLC GUI Config
	- Plat (voice), Gold (video), Silver (best-effort), Bronze (background)

## 2.0 Network Access

### 2.0.1 Switches

- Uses MAC to forward frames
- Maintains MAC table (dynamic/static)
	- Determined from source MAC (incoming ARP request, ARP reply)
- Use **Flooding** when destination MAC unknown, destination found? record: ignore
- Use **Spanning Tree Protocol** to avoid loops
- Implements **ARP** to resolve IP to MAC
- **Auto-negotiation**: Interfaces 'advertise' their `speed` and `duplex` settings
- **Duplex**
	- Half (sync, one at a time, walkie-talkie)
	- Full (async, simultaneous, no collisions)
- Hubs
	- Half-duplex
	- Layer 1 repeaters
	- Carrier Sense Multiple Access with Collision Detection (CSMA/CD)
		- Detect and recover from collisions

### 2.1 VLANs

#### 2.1.1 Dynamic Trunking Protocol (DTP)

- Switchport in `dynamic desirable` will form a trunk with other Switches when
	- `switchport mode trunk`
	- `switchport mode dynamic desirable`
	- `switchport mode dynamic auto`
- `switchport dynamic auto` is passive and will not actively try nor form a trunk unless opposing switch is actively trying
- Should be disabled via `switchport nonegotiate`
- Trunking encapsulation
	- `negotiate`: `ISL` > `802.1q`

![](assets/Fundamentals%20of%20Networking/img-20251117111325296.png)

#### 2.1.2 VLAN Trunking Protocol (VTP)

- Configure VLANs on a central VTP server switch, other switches will sync their VLAN database to the server
- For large networks with many VLANs
- Rare and not recommended
	- Old switch with higher revision number will replace VLAN database, breaking the network
- Versions: 1, 2, 3
- Modes: server, client, transparent
- Cisco default = VTP server mode
- `// anything below here not so important //`
- VTP servers
	- Can CRUD VLANs
		- Increases revision number
	- Stored in NVRAM (persistent)
	- Advertises latest revision on trunk interfaces for sync
		- Latest = king
- VTP clients
	- Cannot CRUD
	- Not stored in NVRAM (unless VTPv3)
	- Forwards VTP ads to other clients
- CLI
	- `show vtp status`
	- `vtp domain cisco`
- Receives VTP ads with higher, will match and join same domain
- Transparent mode
	- Does not participate nor sync
	- Maintains own VLAN database in NVRAM
	- Can CRUD but not advertised
	- Will forward VTP ads if same domain
- Reset revision number to 0
	- Change domain to unused
	- Change to transparent mode

### 2.2 Inter-VLAN Connectivity

### 2.3 Spanning Tree Protocol (STP)

![](assets/Fundamentals%20of%20Networking/img-20251117111325315.png)

![](assets/Fundamentals%20of%20Networking/img-20251117111325378.png)

- Redundancy is essential (24/7/365)
	- Broadcast storms
	- MAC address flapping
- Prevent L2 loops by placing redundant ports in a blocking state, only entering a forwarding state if an active fails
- **Forwarding state** sends & receives
- **Blocking state** only sends or receives STP messages (called Bridge Protocol Data Units (BPDUs))
- Hello BPDU every 2 seconds out all interfaces
	- Only switches can send
- Bridge ID == Priority (4 bits) + VID (12) + MAC (48)
	- VID for PVST+
- Default bridge priority = 32768
	- Priority == priority ? lowest MAC address
	- Priority (multiples of 4096)+1
- Root Ports and Non-designated Ports expect to receive BPDUs
	- If not received by Timers (see below), ...
- Different root bridge for different VLANs to maximize interface bandwidth (load balancing)

```
// changing priority 
Sw(config)#spanning-tree vlan 1 cost 
Sw(config)#spanning-tree vlan 1 port-priority (+32)

Sw(config)#do sh spanning
```

#### 2.3.1 Original STP

##### 2.3.1.1 Steps

1. Lowest Bridge ID = Root Bridge
	- All ports forwarding
2. Choose 1 Root Port for each non-root switch
	- Ports across Root Port = forwarding
	1. Lowest Root cost
		- ![](assets/Fundamentals%20of%20Networking/img-20251107161808334.png)
	2. Lowest neighbor Bridge ID
	3. Lowest neighbor Port ID
		- Port priority (default 128) + port number
		- ![](assets/Fundamentals%20of%20Networking/img-20251106154116227.png)![](assets/Fundamentals%20of%20Networking/img-20251106154422338.png)

3. Remaining collision domains
	- 1 interface to forward
		1. Lowest Root cost
		2. Lowest Bridge ID
	- Other port blocking
4. Primary/Secondary Root Bridge

```
// reduces priority by 4096
Sw(config)#spanning-tree vlan 1 root primary
Sw(config)#spanning-tree vlan 1 root secondary
Sw(config)#do show spanning-tree
```

##### 2.3.1.2 States

```
Sw(config)#spanning-tree mode pvst/rapid-pvst
```

##### 2.3.1.3 STP

| **State**      | **BPDUs** | **Traffic** | **MAC Learning** | **Type**     | **Duration** |
| -------------- | --------- | ----------- | ---------------- | ------------ | ------------ |
| **Blocking**   | rx only   | no          | no               | stable       | N/A          |
| **Listening**  | tx/rx     | no          | no               | transitional | ~15 sec      |
| **Learning**   | tx/rx     | no          | yes              | transitional | ~15 sec      |
| **Forwarding** | tx/rx     | tx/rx       | yes              | stable       | N/A          |

##### 2.3.1.4 RSTP

| State      | BPDUs   | Traffic | MAC Learning | Type         |
| ---------- | ------- | ------- | ------------ | ------------ |
| Discarding | rx only | no      | no           | stable       |
| Learning   | tx/rx   | no      | yes          | transitional |
| Forwarding | tx/rx   | yes     | yes          | stable       |

##### 2.3.3 Timers

- Root bridges' timers decides the entire networks' timers

| **Timer**         | **Purpose**                                                                                                | **Duration**            |
| ----------------- | ---------------------------------------------------------------------------------------------------------- | ----------------------- |
| **Hello**         | RB send Hello BPDUs                                                                                        | **2 sec**               |
| **Forward Delay** | port stays in Listening and Learning (total 30s)                                                           | **15 sec**              |
| **Max Age**       | if Hello BPDUs not received by 20s, reevaluate, change to listening, learning, then forwarding (total 50s) | **20 sec** (≈10× Hello) |

##### 2.3.4 Standards

- 802.1d (OG, legacy, only 1 tree for entire network)
	- `0180.c200.0000`
- 802.1w RSTP
	- Speeds recalculation when topology changes (convergence)
	- Roles and States simplified
- 802.1s MSTP
	- Maps multiple VLANs into same STP
- PVST
	- Cisoc's Per-VLAN, only ISL
- PVST+ (802.1q)
	- Adds VID to the priority
	- `01:00:0c:cc:cc:cd`
- Rapid PVST+
	- Cisco fork of RSTP

##### 2.3.5 STP Toolkit

- **PortFast (Edge)**
	- Immediate forwarding state
	- Only for end hosts
	- Better UX
	- Default: enable on all access ports

```
Sw(config)#int <int>
Sw(config-if)#spanning-tree portfast
Sw(config)#spanning-tree portfast default 
```

- **BPDU Guard**
	- If enabled, receiving a BPDU from another switch errdisables the port
		- ErrDisable Recovery
			 - Default: disabled
				- `shut; no shut`
				- 5 mins
				- `Sw(config)#errdisable recovery interval <seconds>`
			- Disable for particular cause
				- `Sw(config)#errdisable recovery cause <cause>`
		- Default: enabled only on portfast ports

```
Sw(config-if)#spanning-tree bpduguard enable
Sw(config)#spanning-tree portfast bpduguard default
```

 - **BPDU Filter** (BPDU guard but no disable, just ignore)
	- Per port
			- `Sw(config-if)#spanning-tree bpdufilter enable`
			- BPDU ignored
			- BPDU.G not triggered
	- Global
			- `Sw(config)#spanning-tree portfast bpdufilter default`
			- BPDU.F disabled
			- BPDU.G triggered (errdisable)
- **Root Guard**
	- Prevents <u>Designated ports</u> from becoming <u>Root ports</u>
	1. If receive superior BPDU
	2. Rejects new root bridge
	3. Disables interface (BKN / ROOT_Inc)
	- Blocks link, both ends are designated ports

```
Sw(config-if)#spanning-tree guard root
```

- **Loop Guard**
	- Prevents <u>Non-Designated or Root ports</u> from becoming <u>Designated ports</u>
	1. If stop receiving BPDUs,
		- Unidirectional links usually caused by L1 hardware
		- Software bug preventing BPDU sending
	2. Becomes Designated port, causing L2 loop
	3. Broken state instead of Forwarding state
	- Still `up/up`

```
Sw(config-if)#spanning-tree guard loop
Sw(config)#spanning-tree loopguard default
Sw(config-if)#spanning-tree guard none
```

- Other
	- Select root bridge for optimal traffic flow (minimize latency, minimize congestion) and stability, reliability
		- By setting priority to 0
	- Loop and Root guard are mutually exclusive, and overwrites each other

#### 2.3.2 Rapid Spanning Tree Protocol

- <u>Alternate</u> port
	- Discarding port that receives a superior BPDU from another switch
- <u>Backup</u> port
	- Discarding port that receives a superior BPDU from <u>another interface on the same switch</u>
- Functions as Root port backup
- Built-in UplinkFast and BackboneFast (immediate Forwarding)
- Link types
	- Edge: port connected to end host, moves directly to Forwarding (PortFast)
		- `Sw(config-if)#spanning-tree portfast`
	- Point-to-point: direct connection between 2 switches
		- Automatically detected as point to point
		- `Sw(config-if)#spanning-tree link-type point-to-point`
	- Shared: connection to a hub, must be half duplex
		- Automatically detected as shared
		- `Sw(config-if)#spanning-tree link-type shared`

#### 2.3.3 EtherChannel (Port Channel / Link Aggregation Group)

- [3.3 EtherChannel](Cisco%20Hands%20On.md#3.3%20EtherChannel)
- End host bandwidth > distribution switch(es) bandwidth = oversubscription = congestion
- Groups interfaces together to act as a single interface
- Load balance through 'flows' via an algo
	- S. MAC, D. MAC, S.+D. MAC, S. IP, D. IP, S.+D. IP
- 3 methods
	- Port Aggregation Protocol (PAgP)
		- Cisco only
		- On/auto/desirable (PAgP)
	- Link Aggregation Control Protocol (LACP)
		- On/active/passive
	- Static EtherChannel
		- Avoid
- Up to 8 interfaces can form a single EtherChannel
- Interfaces in an EtherChannel must have the same
	- Duplex, speed, switchport mode, allowed VLANS/native VLAN

### 2.4 Discovery Protocols

- Periodic frames to neighbours
- Can be considered security risk

#### 2.4.1 CDP

- [3.4 CDP/LLDP](Cisco%20Hands%20On.md#3.4%20CDP/LLDP)
- CDPv2
- Multicast at `0100.0CCC.CCCC`
- Frame per 60s, to neighbour only
- 180s hold time
- S = Switch a

#### 2.4.2 LLDP (IEEE 802.1AB)

- Multicast at `0180.C200.000E`
- Frame per 30s
- 120s hold time
- B = Bridge (switch)

<hr>

## 3.0 IP Connectivity

### 3.1 Routing (L3)

- Packets treated independently
- No data-recovery
- Media-independent
- IPv4 (32 bits), IPv6 (128 bits), OSI
- [4.2 Routing Table](Cisco%20Hands%20On.md#4.2%20Routing%20Table)
- **Headers**
	- Version, IHL, **Service Type**, Total Length
	- ID, Flag, Fragment Offset
	- **TTL**, Protocol, **Header Checksum**
	- **Source addy**
	- **Destination addy**
	- Options, Padding
- **Routing**
	- **Codes**
		- **C**: Direct connection
		- **L**: Local Interface
		- **R**: RIP (Routing Information Protocol)
		- **D**: EIGRP (Enhanced Interior Gateway Routing Protocol)
		- **O**: OSPF (Open Shortest Path First)
		- **S**: Static (manually configured)
	- Connected `C` vs vs Local `L` routes
		- Connected route = forward
		- Local route = router receive for itself
	- Choose most specific route (longest prefix length)
	- Drop packet if no route found

#### 3.1.1 Inter-VLAN Routing (L3)

- [3.2 VLAN](Cisco%20Hands%20On.md#3.2%20VLAN)

| Item               | Explanation                               | Analogy                          |
| ------------------ | ----------------------------------------- | -------------------------------- |
| **Access Port**    | One VLAN, untagged.                       | One door → one room.             |
| **Trunk Port**     | Many VLANs, tagged.                       | Elevator for all floors.         |
| **Trunk → Access** | Trunk feeds into final access port.       | Elevator → correct door.         |
| **L3 Switch**      | Switch that routes.                       | Switch + traffic cop.            |
| **RoAS**           | Many VLANs via subinterfaces on one link. | One hallway with mini-doors.     |
| **EtherChannel**   | Many links → one logical link.            | Bundle hallway into one big one. |

##### Router on a Stick

- Route between multiple VLANs using a single interface on the router and switch
- Switch int = regular trunk
- Router int = Sub-interface config for each VLAN
- Send frames out of sub-interface with its configured VLAN tag
- $, Simple, easy congestion

##### Layer 3 Switch (Switch with Routing capabilities)

- Routing ok, Inter-VLAN ok, Config routes ok
- Switch Virtual Interfaces (SVI) are virtual interfaces
- Each SVI has an IP address that PC uses as default gateway
- $\$, Faster than RoaS, Scalable

##### Separate Dedicated Router

- Separate physical interfaces for each VLAN
- Best for complex/high-traffic networks
- \$\$\$, requires more physical interfaces

#### 3.1.2 Static Routing

##### Floating Static Route

- Not in routing table unless route learned via Dynamic is removed

```
// specify AD for static routes 
R1(config)#ip route 10.0.0.0 255.0.0.0 10.0.13.2 100
```

#### 3.1.3 Dynamic Routing Protocols

- Forms 'adjacencies'/'neighbor relationships' to advertise routes
- Chooses superior route via lower route <u>metric</u> for routing table
	- [110/3] = [admin distance/metric]
	- Same metric ? both added to routing table
		- Called Equal Cost Multi Path (ECMP)
			- Possible for static routes too = [1/0]
	- Different Routing Protocol ? admin distance
		- Lower = better

 **Admin Distance**

| Route Protocol / Type | AD  |     |
| --------------------- | --- | --- |
| Directly connected    | 0   |     |
| Static                | 1   |     |
| External BGP (eBGP)   | 20  |     |
| EIGRP (internal)      | 90  |     |
| IGRP                  | 100 |     |
| OSPF                  | 110 |     |
| IS-IS                 | 115 |     |
| RIP                   | 120 |     |
| EIGRP (external)      | 170 |     |
| Internal BGP (iBGP)   | 200 |     |
| Unusable route        | 255 |     |

##### 3.1.3.1 Routing Information Protocol (RIP)

- IGP, Distance vector, routing by rumor
- Metric: Hop count (15 max)
- Versions
	- **IPv4**
		- RIPv1 [only Class A, B, C, no support for VLSM, CIDR, subnet mask]
		- RIPv2 [VSLM, CIDR, subnet mask ok, multicast to 224.0.0.9]
	- **IPv6**: RIPng
- 2 messages types
	- **Request:** send routing table
	- **Response:** send local routing table to neighbors
	- Every 30 seconds
- Advertises router's interface

##### 3.1.3.2 Enhanced Interior Gateway Routing Protocol (EIGRP)

- IGP, Distance vector, routing by rumor
- Metric: f(bandwidth + delay) => BW of **slowest link** + delay of **all links**
- Uses wildcard subnet masking
	- `0` match, `1` don't have to match
- Terminology
	- **Feasible distance:** router's metric value to destination
	- **Reported/advertised distance:** neighbor's metric value to destination
		- ![](assets/Fundamentals%20of%20Networking/img-20251117111325449.png)
		- Red, yellow: feasible; Blue, purple: reported
	- **Successor**: route with lowest metric (best route)
	- **Feasible successor**: alternate route with reported distance < successor feasible distance
	- Unequal-cost load-balancing
		- Only on feasible successor routes
		- `R1(config-router)#variance 2`
			- Allows up to 2x successor route's FD

##### 3.1.3.3 Open Shortest Path First (OSPF)

- [OSPF](Cisco%20Hands%20On.md#OSPF)
- Link State, full network map via ads, best path
- more CPU, faster convergence
- Metric: Cost of each link by bandwidth
- Versions
	- v1: dead
	- v2: used for IPv4
	- v3: used for IPv6 (IPv4 also can)
- **Link State Advertisements (LSA)**
	- Organised in **Link State DB (LSDB)**
	- Flooded every 30 minutes (age timer)
	- Dijkstra
	- Types
		- Type 1 (router LSA)
			- router ID, lists networks attached to router's OSPF int
		- Type 2 (network LSA)
			- DR of each 'multi-access' network (broadcast type)
			- routers attached to multi-access network
		- Type 5 (AS external LSA)
			- ASBRs to describe external routes
- Large networks: longer time to calculate, more cpu, more memory, small change = re-flood
- Divide large networks into several smaller areas
- Rules
	- Each Area = unique LSDB
	- All areas must connect to Area 0 via Backbone Routers
	- Area Border Routers (ABR) sit in between Areas
		- ABRs maintain a separate LSB for each Area they're in (3 is too much)
	- Intra-area = route in same OSPF area
	- Inter-area = route in different OSPF area
	- Areas should be contiguous
	- Areas must have >=1 ABR connected to backbone
	- Interfaces in same subnet must be same area
	- Unique Process ID, IP address
	- Same netmask, area ID, timers
- OSPF Metric: Cost
	- Reference BW (default 100) / interface BW = cost (min value = 1)
	- Should change Ref BW
	- Loop back address cost = 1
	- Speed =/= BW
- Becoming OSPF neighbors
	- OSPF hello for potential OSPF neighbors (10 seconds) via `224.0.0.5` (multicast)
		- IP header with Protocol field `89`
	- Neighbor States (Demons In Texas Eat Eels Like Fries)
		- Down
		- Init [hello received, own self not recognized]
		- Two-Way [own self recognized, friendship established]
		- ExStart [higher RID = master, lower RID = slave]
		- Exchange [swapping DBDs (Database Description) ]
		- Loading [asking for missing LSAs (via LSR, LSU, LSAck)]
		- Full [fully synced, sends hello packet every 10s, resets 40s dead timer]
- Network Types
	- **Broadcast**
		- Each subnet one DR one BDR
			- Default on Ethernet, FDDI interface
			- None of those ? DROther
			- Elected via
				- OSPF interface priority (1)
				- OPSF router ID
			- Priority 0, cannot
		- Messages multicast `224.0.0.6`
	- **Point-to-point**
		- Default on HDLC, PPP (serial) interfaces
		- No DR/BDR
		- one side DCE, other side DTE
			- DCE need specify clock rate
		- Encap: cHDLC (default)
		- Encap: ppp `(config-if)#encap ppp`
		-
	- **Non-broadcast**
		- Frame relay, x.25
		- Manually configure neighbors
- Neighbor Requirements
	- Same Area number
	- Interfaces same subnet
	- OSPF process no shut
	- Unique router IDs
	- Hello and Dead timers must match
	- Auth settings must match
	- IP MTU settings must match
	- OSPF Network Type must match

##### 3.1.3.4 Intermediate System to Intermediate System (IS-IS)

- Metric: Cost of each link in the route (default 10)

##### 3.1.3.5 Border Gateway Protocol (BGP)

- EGP, Path vector

#### 3.1.4 First Hop Redundancy Protocol

![](assets/Fundamentals%20of%20Networking/img-20251112134600403.png)

- Virtual IP & MACS
- Active/standby
- Gratuitous ARP replies
	- Sent without being requested
	- Are broadcasts
- Non-preemptive
	- Won't give up role if real Active returns

##### Hot Standby Router Protocol (HSRP)

- Active and standby
- V1: `224.0.0.2`
- V2: IPv6, `224.0.0.102`
- vMACs
	- V1: `0000.0c07.acXX`
	- V2: `0000.0c9f.fXXX`
- If HSRP breaks, active-active, "split brain"

##### Virtual Router Redundancy Protocol (VRRP)

- Open standard
- Master and slave
- Multicast: `224.0.0.18`
- vMAC: `0000.5e00.01XX`

##### Gateway Load Balancing Protocol (GLBP)

- Cisco
- Load balances routers in a single subnet
- Active Virtual Gateway (AVG)
- Active Virtual Forwarders (AVF) = DGW
- Multicast (same as HSRP): `224.0.0.102`
- vMAC: 0007.b400.XXYY
	- XX = group number
	- YY = AVF number

#### 3.1.5 Virtual Routing and Forwarding (VRF)

- Similar to VLANs
- Cannot cross VRFs unless VRF Leaking is configured
- Usually used for MPLS, but without = VRF-lite
- Router can carry traffic from multiple customers (IPs can overlap)

#### 3.1.5 Infrastructure ACL (iACL)

- **Permits only authorized traffic to infra equipment, as well as permit transit traffic**
- Protects traffic destined to the network infra equipment to mitigate directed attacks
- Design depends on protocols used on the network infra equipment
- Deployed at network ingress points as a first line of defense
- Deployed elsewhere accordingly
- Disable unneeded services: [preserve resources, eliminates potential exploits]

```
Router#show control-plane host open-ports
```

<hr>

## 4.0 IP Services

### 4.1 Network Address Translation (NAT)

- [4.4 Network Address Translation (NAT)](Cisco%20Hands%20On.md#4.4%20Network%20Address%20Translation%20(NAT))
- Short term solutions for IPv4 (not enough addresses)
	- CIDR
	- Private IPv4
	- NAT
- Private IPs (cannot be used on internet)
	- A: `10.0.0.0/8` (`10.0.0.0 to 10.255.255.255`)
	- B: `172.16.0.0/12` (`172.16.0.0 to 172.31.255.255`)
	- C: `192.168.0.0/16` (`192.168.0.0 to 192.168.255.255`)

#### 4.1.1 Inside/Outside Local/Global

- Outside local and outside global same unless destination NAT is used
- Inside/outside = host location
- Local/global = perspective

| Source IPv4   | Source IPv4   | Desti IPv4    | Desti IPv4     |
| ------------- | ------------- | :------------ | -------------- |
| inside local  | inside global | outside local | outside global |
| 192.168.0.167 | 100.0.0.1     | 8.8.8.8       | 8.8.8.8        |

#### 4.1.2 Static NAT

- Doesn't preserve IPs because one-to one
- Public IPs, so they have to be owed or registered
- Permanent

#### 4.1.3 Dynamic NAT

- Static but automatic via ACLs
- Permitted = translated, Denied = <u>still go through, just not translated</u>
- NAT pool defines range of available `inside global` addresses
- Still one-to-one mapping
	- If run out = NAT Pool Exhaustion = <u>packet dropped</u>
- Will time out, then mapping can be reused

#### 4.1.4 Port Address Translation (PAT)

- NAT overload
- IP still translated
- Uses random port, and port already in use ? translate to another port
- Saves IPs because many inside hosts share one public IP, just diff ports

### 4.2 Network Time Protocol (NTP)

#### 4.2.1 Software/Hardware Clock

- Correct time within networks for tracking of events
- Clock sync is critical for correct chronological table of events, digital certs, auth protocols
- Port UDP 123
- * = not considered authoritative

#### 4.2.2 NTP

- [4.7 Time Sync (Clock, NTP)](Cisco%20Hands%20On.md#4.7%20Time%20Sync%20(Clock,%20NTP))
- Auto sync via server
- UDP port 123
- Stratum 1-15 (higher bad)
	- Accurate to 1ms if NTP server same LAN, ~50ms if over WAN
	- Reference clocks stratum 0
	- Stratum 1 == primary servers
	- Stratum >2 == secondary servers (both client and server mode)
- NTP clients request time from NTP servers
- Hosts can 'peer' with devices at same stratum
- 3 modes
	- Server
	- Client
	- Symmetric active
- NTP authentication (optional)

### 4.3 Domain Name Service (DNS)

- resolves domains to IP
- device asks DNS server for IP
- `nslookup youtube.com`
- 'A' record for IPv4
- quad 'A' for IPv6
- DNS message > 512 bytes ? TCP : UDP (regardless, port 53)
- DNS cache
- CNAME: Alias for 'A' record

```
ipconfig {/displaydns|/flushdns}

// cisco as a DNS server
R1(config)#ip dns server
R1(config)#ip host {name} {ip}
R1(config)#ip name-server 8.8.8.8 // backup DNS
R1(config)#ip domain lookup // default
R1(config)#do sh hosts

// cisco as a DNS client
R1(config)#ip name-server 8.8.8.8
R1(config)#ip domain name {domain} // giving R1 a domain
```

### 4.4 Dynamic Host Configuration Protocol (DHCP)

- Hosts to auto learn things like IP, subnet mask, DGW, DNS server etc.
- Used for 'clients', done by router
- Routers/servers are usually manually configured
- "Lease" IPs to clients
- DHCP servers UDP 67
- DHCP clients UDP 68

```
ipconfig /release // unicast
ipconfig /renew
```

- DORA
1. DHCP Discover
2. DHCP Offer (can request old IP, uni)
3. DHCP Request
4. DHCP Ack (uni)

#### 4.4.1 Configs

- Router as DHCP server

```
R1(config)#ip dhcp exclude {lower bound IP} {upper bound IP}
R1(config)#ip dhcp pool {name}
R1(dhcp-config)#network 192.168.1.0 /24 // subnet to give clients
R1(dhcp-config)#dns-server 8.8.8.8
R1(dhcp-config)#domain-name {domain}
R1(dhcp-config)#default-router DGW
R1(dhcp-config)#lease {D H mins}
R1#show ip dhcp binding
```

- Router as DHCP relay agent

```
R1(config-if)#ip helper-address {dhcp IP}
```

- Router as DHCP client

```
R2(config-if)#ip add dhcp
```

### 4.5 Simple Network Management Protocol (SNMP)

- mDev can notify NMS of events
- NMS can ask mDev for sitrep
- NMS can tell mDev to change configs
- NMS
	- SNMP Manager (UDP 162)
		- OS
	- SNMP Application
		- Interface
- mDev
	- SNMP Agent (UDP 161)
		- Notifs
	- Management Info Base (MIB)
		- Each variable has OID
			- OID contains:
			- ISO, identified org, dod, internet, mgmt, min-2, system, sysName
- Versions
	- SNMPv1
	- SNMPv2c (large msg = more efficient)
	- SNMPv3 (enc. and auth.)
- Messages
	- Read
		- get(), getNext(), getBulk()
	- Write
		- set()
	- Notifs
		- trap(), inform() - have Ack
	- Response
		- response()

### 4.6 Syslog

- [3.7 Syslog](Cisco%20Hands%20On.md#3.7%20Syslog)
- Event logging, for analysis, troubleshooting
- Messages from devices to server (SNMP opposite), can't pull info from devices or modify variables
- Format
	- `seq:time: %facility-severity-mnemonic: description`
	- Priority (8b)
	- Facility (5b)
	- Severity (3b)
		- 0: Emergency
		- 1: Alert
		- 2: Critical
		- 3: Error
		- 4: Warning
		- 5: Notification
		- 6: Informational
		- 7: Debugging
		- -: Every Awesome Cisco Engineer Will Need Icecream Daily (EACEWNID)
- Logging Locations
	- Console lines
	- Vty lines (telnet/SSH, default disabled)
	- Buffer (RAM)
	- External server (UDP 514)
- `#logging synchronous`

### 4.7 Power Over Ethernet & Quality of Service (QoS)

#### 4.7.1 IP Phones

- [switchport voice vlan](Cisco%20Hands%20On.md#^b9a180)
- Prioritize Voice over IP (VoIP) traffic
- Internal 3-port switch
	- 1 uplink to external switch
	- 1 downlink to PC
	- 1 to phone itself
- Separate VoIP and data traffic via VLANs

#### 4.7.2 PoE

- Allows Power Sourcing Equipment (PSE, usually a Switch) to provide power to Powered Devices (PD, usually IP phones, IP cameras, APs) over an Ethernet Cable
- PSE receives AC power, converts to DC, sends to PDs
- PSE sends low power signals, monitors, then determines how much power to send
- Power policing
	- `[sh] power inline police [int]`
	- `power inline police action err-disable` // requires reenabling
	- `power inline police action log`

| Name           | Standard #  | Watts | Powered Wire Pairs |
| -------------- | ----------- | ----- | ------------------ |
| Cisco ILP      | Proprietary | 7     | 2                  |
| PoE (Type 1)   | 802.3af     | 15    | 2                  |
| PoE+ (Type 2)  | 802.3at     | 30    | 2                  |
| UPoE (Type 3)  | 802.3bt     | 60    | 4                  |
| UPoE+ (Type 4) | 802.3bt     | 100   | 4                  |

#### 4.7.3 QoS

- Everything share IP network = cost savings + features = need more BW
- QoS treats different packets differently (acceptable for interactive audio)
	- Bandwidth
		- Capacity (G/M/Kbps)
		- Reserve BW for specific traffic
	- Delay (<=150ms)
		- TT from source to destination = 1-way delay
		- Return back = 2-way delay
	- Jitter (<=30ms)
		- Some packets 10ms, some 100ms = high jitter
		- Affects audio quality
		- 'Jitter buffer', to provide a fixed delay to audio packets
			- Jitter too high = overrun buffer = poor quality
	- Loss (<=1%)
		- Faulty cables/queues full and device discards packets
- Queueing
	- Receive faster than forward, enters FIFO queue
	- Queue full = packet discarded (Tail drop)
	- TCP global sync
		- Tail drop > Slow down > Under utilisation > Speed up > Congestion > Tail drop
	- Random Early Detection (RED)
		- Traffic reach threshold, drop random packets
		- Those flows will reduce rate
	- Weighted RED (WRED)
		- Drop packets based on class/priority
	- Multiple Queues (one per marking)
		- Ingress > Routing > Classify > Queue > Schedule > Transmit
		- Scheduling
			- Round-robin
			- Weighted
			- Class-Based Weighted Fair Queueing (CBWFQ)
			- Low-Latency Queue (strict priority queue)
				- PQ ok but other queues stuck
- Priority
	- Classification, via:
		- ACL
		- Network Based Application Recognition (NBAR)
			- Via DPI up to L7
		- L2/L3 headers
			- VLAN's dot1q's [PCP/CoS](#^52f974)
				- 0: best effort
				- 1: background
				- 2: excellent effort
				- 3: critical applications
				- 4: video
				- 5: voice
				- 6: internetwork control
				- 7: network control
			- IP phones
				- Init call = PCP3
				- Actual voice = PCP5
			- IP header's [DSCP](#^2cd22d)
				- DF: default forward (best effort) - 0
				- EF: expedited " (low D/J/L) - 46
				- AF: assured " (12 standard values)
					- ![](assets/Fundamentals%20of%20Networking/img-20251119154706165.png)
					- AF{class}{d.p.}
					- DSCP{binary sum}
					- `100110` (6 bits)
						- AF43 (AFXY)
						- DSCP38 (formula = `8X+2Y`)
				- CS: class selector (8 DSCP values, compatible IPP)
					- CS1-7, (for DSCP just * 8)
				- Basically
					- Voice traffic: `EF`
					- Interactive video: `AF4X`
					- Streaming video: `AF3X`
					- High priority data: `AF2X`
					- Best effort: `DF`
				- Trust Boundaries
					- Set at IP phone so markings are trusted
	- Shaping
		- Buffers traffic
	- Policing
		- Drops traffic
		- Allows bursts (go over rate), configurable
		- PC to internet

### 4.8 SSH

- Console port security

```
R1(config)#line console 0 // 1 conn only
R1(config-line)#password ccna 
R1(config-line)#login
R1(config-line)#login local // via local user account instead 
R1(config-line)#exec-timeout 3 
```

- Layer 2 Mgmt IP for SSH

```
// allow SSH via SVI 
SW1(config)#int vlan1
SW1(config-if)#ip add {ip}
SW1(config-if)#no shut
SW1(config-if)#exit
SW1(config)#ip def gate {dgw}

```

- Telnet (TCP 23)
	- Old, plaintext SSH

```
// standard security first
SW1(config)#line vty 0 15
SW1(config)#transport input {telnet|ssh} // only these allowed
```

- SSH
	- IOS image name 'K9' = SSH ok

```
SW1(config)#ip domain name {domain}
SW1(config)#crypto key generate rsa
SW1(config)# > 2048 bit keys
SW1(config)# // enable PW,
SW1(config)# // ACL, VTY, login local
SW1(config)# // exec timeout, transport input 
```

### 4.9 FTP & T (Trivial) FTP

- Used to upgrade the OS of a network device
	- Download new firmware and boot with it
- Both client-server model
- FTP (Control conn: TCP 21, Data conn: TCP 20 <= 2 active connections)
	- user/pw auth, but no enc.
	- FTPS (over SSL/TLS) - have enc.
	- SFTP (SSH FTP) - new and more secure
	- Active Mode
		- Server initiates TCP for the 2nd Data conn
	- Passive Mode
		- Client initiates TCP for the 2nd Data conn
		- Usually when FW in front of client
- TFTP (1st msg: UDP 69, thereafter: random port (TID))
	- no auth, no enc., lightweight
	- Receiver will Ack
	- No Ack by timer ? resend
	- Lock-step communication
		- Alternatively send messages and wait for reply
	- 3 phases
		- Connection
			- Both use a random port - Transfer Identifier (TID)
		- Data Transfer
		- Connection Termination
- IOS File Systems
	- disk = flash memory
	- opaque = internal functions
	- nvram = start conf
	- network = external file systems
- Using FTP/TFTP in IOS

```
// show
R1#show files systems
R1#show version
R1#show flash

// TFTP upgrading firmware
R1#copy tftp: flash:
// enter TFTP server IP {}
// source filename {}
// desti filename {}
R1#conf t
R1(config)#boot system flash:{new firmware}
R1(config)#exit
R1#wr
// reboot
R1#delete flash:{old firmware}
```

```
// FTP 
R1(config)#ip ftp username {user}
R1(config)#ip ftp passowrd {pass}

// same from copy onwards
```

<hr>

## 5.0 Security Fundamentals

### 5.1 Key Concepts

#### CIA

- Confidentiality
- Integrity
- Availability

#### Risk

- Vulnerability (potential weakness)
- Exploit (the vuln.)
- Threat (of E. the V.)
- Mitigation

### 5.2 Common Attacks

- DoS (A)
	- TCP SYN flood
	- DDoS = botnet to flood
- Spoofing (A)
	- DHCP exhaustion (DDoS but DHCP discover)
- Reflection/Amplification (A)
	- Attacker (spoof source IP) > Reflector (DNS server) > Target (gets reply)
	- Amp = reflector sends 2x of what it receives
- MITM (CI)
	- ARP poisoning (override ARP reply)
- Recon
	- OSINT for info gathering
- Malware (CIA)
	- Virus, worms, trojans
- Social Engineering
	- Phishing (spear phishing, whaling, vishing/smishing)
	- Watering hole (victim's frequent sites)
	- Tailgating
- Password-related
	- Dictionary
	- Brute-force

### 5.3 (MF)Authentication, Authorization, Accounting (AAA)

- Proof identity via something you know/have/are
- Digital cert (CSR via CA)
- Authentication = identity
- Authorization = privileges
- Accounting = logs
- AAA servers
	- RADIUS (UDP 1812, 1813)
	- TACACS+ (cisco's, TCP 49)

### 5.4 Security Programs

- User awareness (fake phishing emails to test idiots)
- User training
- Physical least privilege

### 5.5 Access Control Lists (ACL) (L3)

- [4.5 Access Control Lists (ACL)](Cisco%20Hands%20On.md#4.5%20Access%20Control%20Lists%20(ACL))
- Types
	- Number
		- Standard IP ACL: 1-99, 1300-1999
	- Name
- Apply to interface, either inbound or outbound
- Match ACE in ACL ? return
- **Wildcard Masking**
	- Inverse the subnet mask (`0` match, `1` any)
	- ACL permits `192.168.1.0`, wildcard `0.0.0.255`
		- First 3 octets must match, last octet can be anything
	- ACL permits `172.16.16.1`, wildcard `0.0.15.255`
		- Allows `172.16.16.X - 172.16.31.X`
		- Permit `host`/`any`

| Item   | Binary                                          | IP                            |
| ------ | ----------------------------------------------- | ----------------------------- |
| IP     | `1010 1100`.`0001 0000`.`0001 0000`.`0000 0001` | `172.16.16.1`                 |
| Mask   | `0000 0000`.`0000 0000`.`0000 1111`.`1111 1111` | `0.0.15.255`                  |
| Result | `1010 1100`.`0001 0000`.`0001 XXXX`.`XXXX XXXX` | `172.16.16.0 - 172.16.31.255` |

- **Standard IPv4 ACLs**
		- Earlier rules take precedence, implicit deny all at end
- **Extended IPv4 ACLs**
		- **Format**: `<sequence-number> <permit/deny> <tcp/icmp/...> <source IPv4 + port> <desti IPv4 + port>`
		- [Port numbers](https://github.com/darylcwx/ccna-notes/blob/7f3c6347bb33ec3fb95330d2810fc0bd84f42c5e/Fundamentals%20of%20Networking.md#1.5.3%20Layer%204%20\(TCP%20Vs%20UDP\))
- **Applying IPv4 ACLs**
		- **Standard**: near destination (affects only source IP, place later = ok)
		- **Extended**: near source (more fields, save BW early)

### 5.6 Port Security

- [3.5 Port Security](Cisco%20Hands%20On.md#3.5%20Port%20Security)
- Limits MACs from port
- Default: err-disabled, 1 MAC allowed (config or dynamic)
- MAC spoofing = easy, limiting number of MACs more useful
- Only usable if `switchport mode access/trunk`
- Aging [absolute/inactivity]
- Types [static/dynamic/sticky]
- 3 violation modes (unauthorized frame enters an int)
	- Shutdown (disabled, count = 1, syslog/SNMP notif)
	- Restrict (discards, count += 1, syslog/SNMP)
	- Protect (discards)
- Re-enabling
	- Unplug, shut, no shut
	- dynamic `errdisable recovery`
- Sticky MAC stores dynamically learned into run-conf

### 5.7 DHCP Snooping

- Rate-Limiting (prevent exhaustion)
- Default: all ports untrusted
	- Usually, uplink: trusted, downlink: untrusted
- Filters **only** DHCP messages on untrusted ports
	- DHCP server ? discard
	- DHCP client DISCOVER/REQUEST ? source MAC vs DHCP CHADDR match ? forward : discard
	- DHCP client RELEASE/DECLINE ? source IP vs receiving interface match in Snooping Binding Table ? forward : discard
- Attacks
	- DHCP starvation (flood discover)
	- DHCP poisoning (like ARP but DHCP)
- DHCP Servers: OFFER, ACK, NAK
- DHCP Clients: DISCOVER, REQUEST, RELEASE, DECLINE

#### DHCP Option 82 (information option)

 - Add. info like which agent receive, which interface, which VLAN
- Cisco switches adds by default, even if switch isn't a Relay Agent
- Cisco switches drop messages with Opt82 if received on untrusted port

### 5.8 Dynamic ARP Inspection (DAI)

- Rate-limiting
- Default: all ports untrusted
	- Usually, uplink: trusted, downlink: untrusted
- Filter **only** ARP messages on untrusted ports
	- Sender MAC and IP in DHCP Snoop Table ? forward : discard
- ARP Poisoning (MITM)
	- Attack send gARP msg using another IP
	- Traffic all goes to MITM first
- ARP ACLs (alternative if no DHCP)

### 5.8 Network Device Security

#### 5.8.1 Overview

- **Threats**: [remote, local access, physical, environmental, electrical, maintenance]
- **Causes**: [unauthorized, damage, theft, temperatures, humidity, voltage, improper handling, poor cabling]

#### 5.8.2 Passwords

- **Forced entry**: [brute force, guessing, dictionary attacks]
- **Policy**: [length, symbols; storage, protection, frequent change of password]
- **Alternatives**: [MFA, digital certs, biometrics]

#### 5.8.3 Authentication & Access Control

- **802.1X**: port-based access control
- **RADIUS**: central auth server for 802.1X
- **Flow**: device → switch → RADIUS → allow/deny port
- **Use case**: prevent rogue devices, secure wired/Wi-Fi ports

#### 5.8.4 VLAN Hopping

- **Attacks**: access traffic across VLANs without routing
- **Methods**: switch spoofing, double-tagging
- **Prevention** disable DTP, set access ports, native VLAN unused, prune trunks, port security

### 5.9 IPsec (Remote Access / Site-to-Site VPNs)

<hr>

## 6.0 Automation & Programmability

### 6.1 Network Automation

- Benefits
	- reduced human error, scalable, compliance, efficiency
	- Methods [SDN, ansible, python]
- Logical 'planes' of network functions
	- ![](assets/Fundamentals%20of%20Networking/img-20251126133857.png)
	- **Data/Forwarding plane ("do")**
		- Forwarding actual user data/traffic (router, switch, NAT, forward/discard packet)
	- **Control plane ("decide")**
		- Controls what Data plane does (e.g., by building router's routing table)
		- Overhead work (decides rules, but tells **DP** how - OSPF, STP, ARP)
	- **Management plane ("oversee")**
		- Configure, monitor, manage (protocols used for managing)
		- SSH/telnet, Syslog, SNMP, NTP
	- Control/Management traffic processed in CPU, Data traffic uses App-Specific Integrated Circuit (ASIC) is responsible for switching logic, MAC add table stored in Ternary Content-Addressable Memory (TCAM)

### 6.2 Software Defined Networking (SDN)

![](assets/Fundamentals%20of%20Networking/img-20251124095008106.png)

- Aka Software-Defined Architecture (SDA), Controller-Based Networking
- **Application Layer**
- **Control Layer**
	- Controller
		- Tells R1, R2 their routes
	- **Southbound Interface (SBI)**
		- Interact with network devices using APIs
		- [devices, topology, available interfaces, configs]
		- OpenFlow, OpFlex, onePK, NETCONF
	- **Northbound APIs**
		- Interact with controller with scripts/ap
- **Infrastructure Layer**
- **Cisco SD-Access**
	- Application-Centric Infrastructure (ACI) for DC
	- SD-WAN for WANs
	- Cisco Digital Network Architecture (DNA) Center is the Controller
	- **Fabric**
		- **Underlay (provides IP connectivity)**
			- L3 switches and their connections
			- Brownfield ? no underlay config : yes
			- **No STP/HSRP, L3 switches, IS-IS**
		- **Overlay (virtual network from underlay)**
			- SD-Access uses VXLAN (Extensible) to build tunnels for Data plane
			- Location ID Separation Protocol (LISP) - Control Plane
				- Maps (Endpoint Identifiers (EI) to Routing locators (RLOCs))
				- EID finds end hosts connected to Edge nodes, RLOCs identify the edge switch to reach end host
			- Cisco TrustSec (CTS) for policy control (QoS, Sec)
	- 3 roles of switches
		- Edge nodes (DGW of end hosts)
		- Border nodes (devices outside of SD-Access domain)
		- Control nodes (uses Location ID Separation Protocol (LISP) to perform Control Plane functions)
- DNAC vs Traditional
	- Traditional
		- Manually one-by-one via SSH/console
		- Config/policies managed per-device
		- New network deployments = long
		- Manual effort = errors/failures
	- DNAC
		- SDN Controller in SD-Access
		- Network manager in traditional network
		- DNAC GUI or REST to interact
		- Configs/policies/versions all centrally managed
		- SBI supports NETCONF, RESTCONF, telnet, SSH, SNMP
		- Intent-Based Networking (IBN)
			- "this group can't comms with that group"
			- "this group can access this server but not that"
			- DNA Center handles the rest
- Automation
	- Python with REGEX to parse `show` commands
	- Able without third-party apps/scripts
	- APIs = powerful

### 6.3 AI & ML

- ML is a subset of AI
	- Supervised Learning (labelled data)
		- Accurate, easy to do
		- Requires labelled data, output limited to labels
	- Unsupervised Learning (clustering)
		- Reveals hidden patterns
		- Less accurate, need to interpret
	- Reinforcement Learning (rewards/penalties)
		- Complex ok, adapts well
		- Resource intensive, learn wrong if reward system bugged
	- Deep Learning (NN)
		- Large, unstructured data ok, best performance for image recog, NLP, etc.
		- Resource intensive, black box model
- Predictive AI
	- ML to analyze historical and predict future outcomes
	- Decision-making through actionable insights, detect problems before it occurs
	- Requires high-quality historical data, accuracy == generalization
	- Apply: traffic forecasting, IDS, predictive maintenance
- Generative AI
	- ML to learn patterns to create new content
	- Automate creative tasks/content creation
	- Risk of misuse, generated content as good as training material, hallucinations
	- Apply: gen. docs for configs/policies, gen. configs based on docs, suggest optimized configs
- AI in Catalyst Center
	- AI Network Analytics
		- Finds network baseline, provides insights and recs, predicts and detects anomalies)
	- Machine Reasoning Engine (MRE)
		- Root cause analysis, suggest resolutions, takes automated corrective action, reduces downtime by identifying and resolving issues faster
	- AI Endpoint Analytics
		- Visibility on devices on network, classifies them
		- Detects unauthorized or unusual behavior
		- Simplifies onboarding by auto-profiling etc.
	- AI-enhanced Radio Resource Management (RRM)
		- Optimize network by adjusting radio settings
		- AI to balance load, reduce interference, and improve coverage across APs

### 6.4 APIs

- CRUD
- POST, GET, PUT, DELETE
- Client-server architecture
- Stateless
- Cacheable must be supported
- Cisco DevNet
	- Integrate APIs with Cisco ecosystem
- URI
	- Scheme: protocol
	- Authority: domain
	- Path: file path
- HTTP Response
	- 1XX: info (102: processing)
	- 2XX: success (200: ok, 201: created)
	- 3XX: redirection (301: moved)
	- 4XX: client error (403: unauth)
	- 5XX: server error (500: internal SE)
- Authentication
	- **Basic Auth**
		- User/pass in every req, encoded in **Base64** via **HTTPS**
		- Simple to implement
		- Credentials easily stolen, not secure
	- **Bearer Auth**
		- Token in HTTP header, given by Auth server
		- Tokens **expire** after some time, more secure than Basic
		- Haven't expire attacker still can access, tokens need to be refreshed
	- **API key Auth**
		- Unique **static key** in HTTP header
		- Easy, no need refresh, good for tracking customer API usage
		- Stolen = full access until revoked
	- **OAuth2.0**
		- Access tokens
			- Similar to Bearer
			- **Refreshes tokens**
			- **Limited access**
		1. Client > Auth req > RO (me)
		2. RO > Auth grant > Client
		3. Client > Auth grant > Auth server
		4. Auth server > Access token > Client
		5. Client > Access token > Resource server
		6. Resource server > Resource > Client
		- Parties
			- Resource Owner (me)
			- Client
			- Auth server
			- Resource server

### 6.5 JSON, XML, YAML

- Standardising data for communication between applications
- Open standard file format and data interchange format
- XML: `<key>value</key>`

### 6.6 Infrastructure as Code (IaC)

#### Config Management

- For existing systems ([Ansible](#Ansible), [Puppet](#Puppet), [Chef](#Chef))
- **Mutable**, changes == update
- "Config Drift" - over time configs deviate from standards
	- E.g., same syslog, SNMP, NTP
- Small changes/fixes that aren't documented lead to issues
- Version control yet unsure which is correct/match standards
- Templates/variables
	- Generate, edit, check, compare configs on large scale

```
// template
hostname {{hostname}}
!
interface g0/0
ip address {{address}} {{mask}}
ip ospf {{process}} area {{area}}

// variables for R1
hostname: R1
address: 192.168.1.1
mask: 255.255.255.0
process: 1
area: 0
```

#### Infrastructure Provisioning

- For initial setup, from scratch ([Terraform](#Terraform))
- Creates, modifies, and deletes infrastructure resources
- **Immutable**, changes == new VM == no config drift

#### Procedural Vs Declarative

- Procedural
	- Explicit steps to follow for outcome
	- Greater control
- Declarative
	- Defines end state
	- Terraform figures it out
	- Consistent

#### Ansible

- RedHat, in Python, agentless (no need software), **procedural**
- Push model via SSH with YAML
- Model
	- <u>Playbook</u> (blueprints, logic, actions)
	- Inventory
	- Templates
	- Variables

#### Puppet

- Ruby, agent-based, **declarative**
- Pull model with own language
	- Clients use TCP 8140 to pull from Puppet master
- Model
	- <u>Manifest</u> (desired state)
	- Templates

#### Chef

- Ruby, agent-based, **procedural**
- Pull model with Domain-Specific Language (DSL) on Ruby
	- Server use TCP 10002 to send to clients
- Model
	- Resources (configs)
	- Recipes (Logic and actions)
	- Cookbooks (x recipes)
	- <u>Run-list</u> (ordered list of recipes for desired state)

#### Terraform

- Go, Agentless, **declarative**, push model provisioning
- Integrations with AWS, Azure, K8s, Catalyst Center, ACI, IOS XE
- Config files > Terraform Core > Calls cloud providers' APIs > State file
- 3 steps
	- **Write**: define desired state in HashiCorp Config Lang (HCL (also a DSL))
	- **Plan**: verify changes
	- **Apply**: execute

## 7. Good To Know

### 7.1 Characteristics of a Network

- **Topology**: physical connectivity of devices and logical paths of data flows
	- Network diagram [Physical (layout), Logical (pathing), Colour legend]
- **Bitrate/Bandwidth**: rate in bits per second
- **Availability**: SLA for 99.99% uptime, Fault tolerance: 100%
- **Reliability**: operate without failures
- **Scalability**: scale without affecting performance
- **Redundancy**: more paths in case of failure
- **Security**: defense from threats
- **Quality of Service**: tools/mechanisms to use network resources
- **Cost**: expense + maintenance
- **Virtualization**: emulated (storage, compute, network, security)

### 7.2 KPIs

- Bandwidth
- Latency
- Jitter
- Packet Loss

### 7.3 Applications

- **Data**: [smooth, benign, drop & delay insensitive]
- **Voice**: [smooth, benign, drop & delay sensitive]
	- Latency <= 150ms, Loss <= 1%, Bandwidth (30-128Kbps)
	- Latency <= 150ms, Loss <= 0.1%-1%, Bandwidth (384 Kbps - 20+ Mbps)
- **Video**: [bursty, greedy, drop & delay sensitive]
- **RFC**: Request For Comments (universal SLAs)
	- Generally HTTP response <= 400ms
- **Classifications**: [batch, interactive, real-time]

## 8. Troubleshooting

- Hardware
- Software
- Config

### 8.1 Problems

- Asymmetric routing
	- Traceroute for A, B, C and C, B, A
	- No traceroute = hop by hop checks
- High Availability Between Firewalls
	- HA for session information
		- If link breaks, both FW think they are master, traffic fails
- ARP/MAC incomplete == IP conflict

#### 8.1.1 Steps

- Identify problem
- Gather information
- Analyze information
- Eliminate potential causes
- Propose hypothesis
- Test hypothesis
- Document solutions/workaround

#### 8.1.2 Actual Steps

- Verify the host IPv4 address and subnet mask
- Ping the loopback address 					(IPv4 stack)
- Ping the IPv4 address of the local machine
- Ping the default gateway
- Ping the remote server
- Errors and Definitions
	- **Input queue drop**: CPU cannot process packet in time
	- **Output queue drop**: Congestion on the interface
	- **Input errors**: Cabling problems, interface hardware problems
	- **Output errors**: Collisions during the transmission of a frame
	- **Excessive noise**: Cable exceeds max length
	- **Excessive collision**: Duplex mismatch

## 9. Yet to Categorize
