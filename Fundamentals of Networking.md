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

## 1. Components

- **Endpoints** (devices)
- **Intermediary/Network devices** (switches, routers)
- **Services** (FTP servers)
- **Media Types**
	- Wired: Ethernet, Serial
	- Wireless: [WLAN (802.11), Bluetooth (802.15), WiMAX (802.16)]

## 2. LAN, WAN, WLAN Overview

### 2.1 LAN

- Connects devices within an area
- Uses switches to forward data frames based on MAC
- Typically wired with ethernet

#### 2.1.1 VLAN (L2)

- VLAN maps to a unique subnet (L3)
- Logical segmentation into separate broadcast domains
- Needs router to communicate across other VLANs
- Security, performance, management
- IEEE 802.1Q (VLAN tagging protocol)
  - [Desti MAC, Source MAC, **VLAN Tag**, Type/Length, Payload, FCS]
  - 4 byte VLAN Tag in Ethernet frame (L2)
	- **Type**: 16 bits, `0x8100`
	- **Priority**: 3 bits, QoS priority
	- **CFI**: 1 bit
	- **VLAN ID**: 12 bits, VLAN number (0-4095)

### 2.2 WAN

- Spans large geographic area
- Connects multiple LANs
- Managed by ISP or SIs
- WAN optimizer

### 2.3 WLAN

- **Access Points (APs)**
	- Connects wireless clients to wired LAN
	- Can be connected to switch, or another AP, or have SIM card
	- Can be cloud-based AP (NaaS)
- **WLAN controller**
	- 

- **BYOD**
	- [Cost savings, Reduced hardware expenses, Work from anywhere, Secure remote collaboration, Employee satisfaction]
- **VPN**
	- Virtual extension of LAN over public WAN
	- SSL & IPsec [L2, L3, L4, L3, L4]
	- L2-in-ip [L2, L3, L2, L3, L4]

### 2.4 Other

- **Data Center**
	- [Availability, Resiliency, Scalability]
	- Enterprise = [Connectivity, Security]
	- DC Switches (a lot faster, massive, low-latency)
	- Small Form-Factor Pluggable (1-800 gigabits/s)
- **Web Servers, DMZ, Extranet**
	- Web servers in DMZ, a controlled zone between Internet and internal network
	- Usually a subnet the hosts public services

## 3. Communication over LAN

### 3.1 Physical Media Types

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

#### 3.1.1 Ethernet Media Table

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

### 3.2 Communication Types

- **Unicast**: one-to-one (flooding)
- **Broadcast**: one-to-all, FFFF:FFFF:FFFF (ARP)
- **Anycast**: one-to-nearest/best (CDNs)
- **Multicast**: one-to-selected (netflix)

### 3.3 Host-to-Host Packet Transfer Process (same LAN)

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

### 3.4 Address Resolution Protocol (ARP) - 0x0806

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

## 4. Addressing (L3/L2)

### 4.1 MAC Address - 48 Bits (L2)

- 6 bytes: 3 bytes Organizationally Unique Identifier (OUI), 3 bytes device
- Globally unique Hexadecimal (0-9, A-F)
- Broadcast = `FFFF:FFFF:FFFF`
- Auto clears "aging" after 5 minutes

#### 4.1.1 Ethernet Frame (64 bytes)

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

### 4.2 IPv4 Addressing - 32 Bits (L3)

- 4 bytes/octets, 8 bits per octet
- 8 bits = max value 255
- Need configure IPsec

#### 4.2.1 Headers (20-60 bytes)

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

#### 4.2.2 Subnet Classes

- `A B C, easy as 1 2 3` (Class A = 0XXX, Class B = 10XX, Class C = 110XX)
- Some are reserved (127.0.0.1)
- ==Subnet prefix must match class==

| Class | First Octet Range | Prefix Length | Leading Bits | Default Mask    | Purpose / Notes                   |
| ----- | ----------------- | ------------- | ------------ | --------------- | --------------------------------- |
| A     | 1–126           | /8            | 0            | `255.0.0.0`     | large networks (127 for loopback) |
| B     | 128–191         | /16           | 10           | `255.255.0.0`   | medium networks                   |
| C     | 192–223         | /24           | 110          | `255.255.255.0` | small networks                    |
| D     | 224–239         |               | 1110         | N/A             | multicast                         |
| E     | 240–255         |               | 1111         | N/A             | experimental / reserved           |

#### 4.2.3 Binary Calculation

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

#### 4.2.4 Sub-sub Netting (Variable Length Subnet Masking - VLSM)

- Network: 192.168.10.0/26
- Subnet Mask: 255.255.255.192
- Block size (by CIDR): 1st bit = 128, 2nd bit = 64
	- /26 = 256-192 = 64 remaining IP, 256/64 = 4 blocks
	- /27 = 256-224 = 32 remaining IP, 256/32 = 8 blocks
	- /28 = 256-240 = 16 remaining IP, 256/16 = 16 blocks
	- /29 = 256-248 = 8 remaining IP, 256/8 = 32 blocks
- Sub-subnet based on requirements
	- e.g. 1, "support >= 6 IPs per block", use /29, since 8 IPs per block
	- e.g. 2,

| Subnet (/26) | Network          | Usable   | Broadcast |
| ------------ | ---------------- | -------- | --------- |
| 1            | `192.168.10.0`   | .1-62    | .63       |
| 2            | `192.168.10.64`  | .65-126  | .127      |
| 3            | `192.168.10.128` | .129-190 | .191      |
| 4            | `192.168.10.192` | .193-254 | .255      |

### 4.3 IPv6 Addressing - 128 Bits, 0x86DD (L3)

- Larger address space
- Simpler header
- Security (built-in encryption) and mobility
- Transition
- No broadcast addresses
- 64 bits **network**, 64 bits **interface**

#### 4.3.1 Headers (40 bytes)

| Field               | Size (bits) | Description                        |
| ------------------- | ----------- | ---------------------------------- |
| Version             | 4           | binary = 0110                      |
| Traffic Class       | 8           | packet priority / QoS              |
| Flow Label          | 20          | packet flow for special handling   |
| Payload Length      | 16          |                                    |
| Next Header         | 8           | next protocol (TCP, UDP, etc.)     |
| Hop Limit           | 8           | max hops before discard (like TTL) |
| Source Address      | 128         | IPv6 address of sender             |
| Destination Address | 128         | IPv6 address of receiver           |

#### 4.3.2 Link-Local Unicast Addresses

- Prefix: `FE80::/10`
- Local link only/same switch
- Used for Neighbor Discovery Protocol (NDP - similar to ARP), Router Comms/Ads, Auto-config (SLAAC)

#### 4.3.3 Unique Local Address

- Prefix: `FC00::/7`
- Private internal networks
- Used for within organization, not public

#### 4.3.4 Multicast

- Prefix: `FF00::/8`

#### 4.3.5 Anycast

- Lowest cost (CDN)

#### 4.3.6 Other

- Loopback: `::1` for testing purposes
- Unspecified `::` "no address" placeholder

#### 4.3.7 Address Allocation

- Static: manual interface ID
- Static: EUI-64 interface ID
  - Insert `FF:FE` (16 bits) in the middle of MAC
  - Flip the `7th bit`, convert back to hexadec
- Stateless Address Autoconfig (SLAAC)
  - `FF02::2`
  - `Router1(config)#ipv6 address autoconfig`
- Stateful DHCP (DHCPv6)
- Stateless DHCPv6

## 5. Layered Models

### 5.1 OSI Model

| Layer        | Explanation                                        | Analogy                                       |
| ------------ | -------------------------------------------------- | --------------------------------------------- |
| Application  | app interface                                      | letter to post office                         |
| Presentation | data formatting, encryption                        | translate language                            |
| Session      | manages connections                                | post office opens a comms line to destination |
| Transport    | reliable delivery (TCP/UDP)                        | package is tracked and delivery guaranteed    |
| Network      | routing across networks (core & distribution), IPs | best route for package delivery               |
| Data Link    | local delivery and switching (access)              | package handed to local delivery trucks       |
| Physical     | transmission of raw bits                           | roads, trucks, wires                          |

### 5.2 Encapsulation

| Layer       | Items                    |
| ----------- | ------------------------ |
| Application | User data, Data header   |
| Transport   | + Segment header (ports) |
| Internet    | + Packet header (IPs)    |
| Link        | + Frame header (MACs)    |

## 6. Protocols & Services

### 6.1 Transport Layer (L4)

- **TCP/IP**
	- 3-way: [SYN, SYN/ACK, ACK]
	- Guaranteed delivery (reliability, data integrity)
	- Resend if error, interrupting streaming/music
- **UDP**
	- No guaranteed delivery
	- Fast but no error-checking

### 6.2 Routing (L3)

- Packets treated independently
- No data-recovery
- Media-independent
- IPv4 (32 bits), IPv6 (128 bits), OSI
- [Routing table](./Cisco%20Hands%20On.md#3%20routing%20table)
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

#### 6.2.1 VLAN Routing (L3)

- [CLI](Cisco%20Hands%20On.md#2.4%20VLAN)
- Separate L2 broadcast domain
- Security, traffic control, performance
- Options
	1. Router on a stick
		- Single physical interfaces, Sub-interface config for each VLAN
			- IP address that acts as default gateway
			- `Fa0/1.10` for VLAN 10
			- `Fa0/1.20` for VLAN 20
	  	- $, Simple
	2. Layer 3 Switch (Switch with routing capabilities)
	  	- SVI (Switched Virtual Interfaces) configs
	  	- Each SVI has a IP address that acts as default gateway
	  	- \$\$, Faster than RoaS, Scalable
	3. Separate Dedicated Router
		- Separate physical interfaces for each VLAN
		- Best for complex/high-traffic networks
		- \$\$\$, requires more physical interfaces

#### 6.2.2 Static Routing

#### 6.2.3 Dynamic Routing Protocols

- Up-to-date, best available path (Dijkstra)
- Each router builds a topology map (LSDB - Link-State DB)
- **IGP** (Interior Gateway Protocol, used *within* a network)
  - **Link-State** (routers share topology for full map)
	- LSA (Link-State Advertisement) → Build full map, then Dijkstra
	- Faster convergence
	- [OSPF, IS-IS (intermediate to intermediate system)]
	  - **OSPF:**
		- Backbone, Areas
		- **Neighbor Adjacencies**
		  - HDLC (High-Level Data Link Control)
			- Physical p2p connection, no DR/BDR needed
		  - Point-to-Point Sub-interface
			- Virtual p2p connection (Frame Relay, ATM), no DR/BDR needed
		- **Neighbor States**
		  - Down [no hello packets, neighbor MIA]
		  - Init [hello received, but not recognized]
		  - 2-Way [mutual acknowledgement, friendship established]
		  - ExStart [electing who is master/slave]
		  - Exchange [swapping DBDs (Database Description Packets)]
		  - Loading [asking for missing LSAs (via LSR, LSU)]
		  - Full [database twins, neighbors fully synced]
  - **Distance-Vector**
	- Share full routes with neighbors
	- Partial view (only next hops)
	- Slower convergence
	- [RIP (routing info protocol), EIGRP (enhanced interior gateway routing protocol)]
- **EGP** (Exterior Gateway Protocol, used *between* networks/ISPs)
  - Path-Vector
  - [BGP (border gateway protocol)]

#### 6.2.4 Network Address Translation

- [CLI](Cisco%20Hands%20On.md#4.%20NAT%20(Network%20Address%20Translation))
- translate private → public IPs
- Terminology/Translation Mechanism

| Source IPv4   | Source IPv4   | Desti IPv4    | Desti IPv4     |
| ------------- | ------------- |:------------- | -------------- |
| inside local  | inside global | outside local | outside global |
| 192.168.10.10 | 209.165.200.5 | 209.165.201.1 | 209.165.201.1  |

- **Static NAT**: one-to-one
	- **Port Forwarding**: external port → specific internal port
  - **Dynamic NAT**: many-to-many (pool of public IPs)
  - **PAT**: many-to-one (distinguished by TCP/UDP ports)
  - **Advantages**: [flexibility of connections to public network, consistency for internal network addressing, network security]
  - **Disadvantages**: [end-to-end functionality and traceability lost, degraded performance]

#### 6.2.5 Infrastructure ACL (iACL)

- **Permits only authorized traffic to infra equipment, as well as permit transit traffic**
- Protects traffic destined to the network infra equipment to mitigate directed attacks
- Design depends on protocols used on the network infra equipment
- Deployed at network ingress points as a first line of defense
- Deployed elsewhere accordingly
- Disable unneeded services: [preserve resources, eliminates potential exploits]

```
Router#show control-plane host open-ports
```

### 6.3 Network Services

#### 6.3.1 Switches

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

#### 6.3.2 Spanning Tree Protocol (STP)

- **Standards**
  - IEEE 802.1D (CST, legacy, only 1 tree for entire network)
  - PVST+ (Cisco fork that provides 1 STP per VLAN)
  - 802.1s MSTP (Maps multiple VLANs into same STP)
  - 802.1w RSTP (improves convergence by revamping port roles and BPDU exchanges)
	- Speeds recalculation when topology changes (convergence)
	- Roles and States simplified
  - Rapid PVST+ (Cisco fork of RSTP using PVST+)
- **STP steps**
  1. Root Bridge (lowest ID = 'CEO')
  2. Root Port for each non-root switch
	 - Port with cheapest cost, fastest route to 'CEO'
  3. Designated Port for each segment
	 - Port on the Switch with fastest path to Root Bridge
	 - All traffic leaves that segment via the DP
	 - Repeat for each segment
	 - *DP = segment's VIP lane to 'CEO'*
  4. Root ports/Designated ports → Forwarding state, all else → Blocking state
- **Enhancements**
  - PortFast (immediate transition to forwarding state)
  - BPDU Guard (protects from loop, errdisables a port when BPDU received, used with PortFast)
  - BPDU Filter (BPDU guard but no disable, just ignore)
  - **STP Loop Guard** (stops port forwarding if BPDUs vanish)
  - **STP Root Guard** (prevent rouge switch from hijacking Root Bridge)
  - [EtherChannel](Cisco%20Hands%20On.md#2.5%20EtherChannel) (combines multiple physical links into 1 logical link → redundancy + BW)

#### 6.3.3 Dynamic Host Configuration Protocol (DHCP)

- assigns IPs clients

#### 6.3.4 Domain Name Service (DNS)

- resolves domain name IP

#### 6.3.5 Syslog

- [CLI](Cisco%20Hands%20On.md#2.6%20Syslog)
- Configure devices to send syslog messages on privilege mode, forward to:
  - Logging buffer
  - Console line
  - Terminal lines
  - Syslog server
- Format
  - Priority (8b)
	- Facility (5b)
	- Severity (3b)
	  - Seen from `%CDP-4-NATIVE_VLAN_MISMATCH:...`
	  - 0: Emergency
	  - 1: Alert
	  - 2: Critical
	  - 3: Error
	  - 4: Warning
	  - 5: Notification
	  - 6: Informational
	  - 7: Debugging
  - Header
	- Timestamp
	- Hostname
  - Message
	- Text

#### 6.3.6 Simple Network Management Protocol (SNMP)

- **SNMP Manager**: Polls agents on network
- **SNMP Agent**: Stores info and responds to manager requests, generates traps
- **MIB**: DB of objects
- **Overview**:
  - M → get-request → A
  - M → get-next-request → A
  - M → get-bulk-request → A
  - M → set-request → A
  - A → get-response → M
  - A → trap → M
  - A → inform → M

#### 6.3.7 Software Clock, Network Time Protocol (NTP)

- [CLI](Cisco%20Hands%20On.md#2.7%20Software%20Clock,%20NTP)
- Correct time within networks for tracking of events
- Clock sync is critical for correct chronological table of events, digital certs, auth protocols
- Port UDP 123
- Stratum (1-15)

## 7. Network Security

### 7.1 Firewall & IDS/IPS

- Operate on L3 and L4
- DPI (Deep Packet Inspection): content-based filtering

### 7.2 Access Control Lists (ACL) (L3)

- [CLI](Cisco%20Hands%20On.md#5.%20ACL%20(Access%20Control%20Lists))
- Permit/Deny
- **Wildcard Masking**
	- Inverse subnet mask (`0` match, `1` any)
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

- **Types**: [number, name]
- Config Standard IPv4
	- Earlier rules take precedence, implicit deny all at end
	- **Format**: `access-list <acl-number> <permit/deny> <source [wildcard]> | host <address>/<name> | any`
	- **Example**: `access-list 1 permit 172.16.0.0 0.0.255.255`
- Config Extended IPv4
	- **Format**: `<sequence-number> <permit/deny> <tcp/icmp/...> <source IPv4 + port> <desti IPv4 + port>`
- Applying IPv4 ACLs
	- **Standard**: near destination (affects only source IP, place later = ok)
	- **Extended**: near source (more fields, save BW early)

### 7.3 Network Device Security

- [Setting Switch Password](Cisco%20Hands%20On.md#2.2%20Switch%20Config)
- [Security Related](Cisco%20Hands%20On.md#2.9%20Security%20Related)

#### 7.3.1 Overview

- **Threats**: [remote, local access, physical, environmental, electrical, maintenance]
- **Causes**: [unauthorized, damage, theft, temperatures, humidity, voltage, improper handling, poor cabling]

#### 7.3.2 Passwords

- **Forced entry**: [brute force, guessing, dictionary attacks]
- **Policy**: [length, symbols; storage, protection, frequent change of password]
- **Alternatives**: [MFA, digital certs, biometrics]

#### 7.3.3 Authentication & Access Control

- **802.1X**: port-based access control
- **RADIUS**: central auth server for 802.1X
- **Flow**: device → switch → RADIUS → allow/deny port
- **Use case**: prevent rogue devices, secure wired/Wi-Fi ports

#### 7.3.4 Port Security

- [CLI](Cisco%20Hands%20On.md#2.10%20Port%20Security)
- Limit MAC per port
- Sticky MAC option to learn allowed devices
- Actions: [shutdown, restrict, protect]

#### 7.3.5 VLAN Hopping

- **Attacks**: access traffic across VLANs without routing
- **Methods**: switch spoofing, double-tagging
- **Prevention**: [disable DTP](Cisco%20Hands%20On.md#2.4%20VLAN), set access ports, native VLAN unused, prune trunks, port security

#### 7.3.6 ARP Spoofing Attack

- Attacker forges an ARP reply to map victim IP to his MAC
- Becomes MITM and intercepts traffic
- **Prevention**
  - DHCP snooping
  - [Dynamic ARP Inspection (DAI)](Cisco%20Hands%20On.md#2.11%20Dynamic%20ARP%20Inspection%20(DAI))
  - [Port Security](Fundamentals%20of%20Networking.md#7.3.4%20Port%20Security)

## 8. Network Overview and Performance

### 8.1 Characteristics of a Network

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

### 8.2 KPIs

- Bandwidth
- Latency
- Jitter
- Packet Loss

### 8.3 Applications

- **Data**: [smooth, benign, drop & delay insensitive]
- **Voice**: [smooth, benign, drop & delay sensitive]
	- Latency <= 150ms, Loss <= 1%, Bandwidth (30-128Kbps)
	- Latency <= 150ms, Loss <= 0.1%-1%, Bandwidth (384 Kbps - 20+ Mbps)
- **Video**: [bursty, greedy, drop & delay sensitive]
- **RFC**: Request For Comments (universal SLAs)
	- Generally HTTP response <= 400ms
- **Classifications**: [batch, interactive, real-time]

## 9. Troubleshooting

- Hardware
- Software
- Config

### 9.1 Recommended

#### 9.1.1 Steps

- Identify problem
- Gather information
- Analyze information
- Eliminate potential causes
- Propose hypothesis
- Test hypothesis
- Document solutions/workaround

#### 9.1.2 Actual Steps

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
