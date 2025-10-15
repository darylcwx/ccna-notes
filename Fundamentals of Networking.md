- [1. Components](#1-components)
- [2. LAN, WAN, WLAN Overview](#2-lan-wan-wlan-overview)
	- [2.1 LAN](#21-lan)
	- [2.2 WAN](#22-wan)
	- [2.3 WLAN](#23-wlan)
	- [2.4 Other](#24-other)
- [3. Communication over LAN](#3-communication-over-lan)
	- [3.1 Physical Media Types](#31-physical-media-types)
	- [3.2 Communication Types](#32-communication-types)
	- [3.3 Host-to-Host Packet Transfer Process (same LAN)](#33-host-to-host-packet-transfer-process-same-lan)
- [4. Addressing](#4-addressing)
	- [4.1 MAC Address (L2)](#41-mac-address-l2)
	- [4.2 IP Addressing (L3)](#42-ip-addressing-l3)
		- [4.2.1 Subnet Classes](#421-subnet-classes)
		- [4.2.2 Binary Calculation](#422-binary-calculation)
		- [Example](#example)
		- [4.2.3 Sub-sub Netting (Variable Length Subnet Masking - VLSM)](#423-sub-sub-netting-variable-length-subnet-masking---vlsm)
- [5. Layered Models](#5-layered-models)
	- [5.1 OSI Model](#51-osi-model)
	- [5.2 Encapsulation](#52-encapsulation)
- [6. Protocols \& Services](#6-protocols--services)
	- [6.1 Transport Layer (L4)](#61-transport-layer-l4)
	- [6.2 Routing (L3)](#62-routing-l3)
	- [6.3 Network Services (L2)](#63-network-services-l2)
- [7. Network Security](#7-network-security)
	- [7.1 Firewall \& IDS/IPS](#71-firewall--idsips)
- [8. Network Overview and Performance](#8-network-overview-and-performance)
	- [8.1 Characteristics of a Network](#81-characteristics-of-a-network)
	- [8.2 KPIs](#82-kpis)
	- [8.3 Applications](#83-applications)
- [9. Troubleshooting](#9-troubleshooting)
	- [9.1 Recommended](#91-recommended)
		- [Steps](#steps)
		- [Actual Steps](#actual-steps)

## 1. Components
- **Endpoints** (devices)
- **Intermediary/Network devices** (switches, routers)
- **Services** (FTP servers)
- **Media Types**
	- Wired: Ethernet, Serial
	- Wireless: [WLAN (802.11), Bluetooth (802.15), WiMAX (802.16)]
##  2. LAN, WAN, WLAN Overview
### 2.1 LAN
- Connects devices within an area
- Uses switches to forward data frames based on MAC
- Typically wired with ethernet
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
	- Configures AP automatically/centrally
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
- **Optical Fiber**
	- [core, cladding, buffer]
	- [9, 125, 250] µm
	- Types
		- Single-mode: [laser, high BW, longer distance, $$$]
		- Multi-mode: [LED, slow BW, shorter distance, $]
- **Twisted Pair (ethernet)**
	- UTP (Unshielded Twisted Pair)
	- STP (Shielded Twisted Pair)
		- Reduce inteference
	- S/STP: Screened STP
		- More shields
	- Categories [CAT5, 5e, 6, 6a, 7, 8]
- **Coaxial Cable**
	- Copper core, Legacy use
### 3.2 Communication Types
- **Unicast**: one-to-one (flooding)
- **Broadcast**: one-to-all (ARP)
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
## 4. Addressing
### 4.1 MAC Address (L2)
- 24 bits vendor ID, 24 bits device
- Hexadecimal (0-9, A-F)
### 4.2 IP Addressing (L3)
- IPv4
	- 32 bits/4 octets, 8 bits per octet
	- 8 bits = max value 255
#### 4.2.1 Subnet Classes
- A B C, easy as 1 2 3
- Some are reserved (127.0.0.1)
- ==Subnet prefix must match class==

| Class | Fixed Bit | Range                         | Subnet        |    
| ----- | --------- | ----------------------------- | ------------- | 
| A     | 1         | 10.0.0.0 - 10.255.255.255     | 255.0.0.0     |    
| B     | 2         | 172.16.0.0 - 172.16.255.255   | 255.255.0.0   |     
| C     | 3         | 192.168.0.0 - 192.168.255.255 | 255.255.255.0 |     
| D     | 4         | 224.0.0.0 - 239.255.255.255   | Multicast     |     
| E     | 5         | 240.0.0.0 - 255.255.255.255   | Reserved      |  
#### 4.2.2 Binary Calculation

| Base    | 2^8 | 2^7 | 2^6 | 2^5 | 2^4 | 2^3 | 2^2 | 2^1 |
| ------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Sum     | 128 | 64  | 32  | 16  | 8   | 4   | 2   | 1   |
| Cum     | 128 | 192 | 224 | 240 | 248 | 252 | 254 | 255 |
| Binary  | 1   | 0   | 1   | 1   | 1   | 0   | 0   | 1   |
| Decimal | 128 | 0   | 32  | 16  | 8   | 0   | 0   | 1   |
#### Example
| Example           | IP 1             | IP 2           | How                    |
| ----------------- | ---------------- | -------------- | ---------------------- |
| IP address        | 192.168.24.19/24 | 10.0.1.15/8    | N.A.                   |
| Subnet mask       | 255.255.255.0    | 255.0.0.0      | bits taken for network |
| Network address   | 192.168.24.0     | 10.0.0.0       | smallest               |
| Broadcast address | 192.168.24.255   | 10.255.255.255 | largest                |
| Host addresses    | 192.168.24.19    | 10.0.1.15      | same                   |
| Default gateway   | 192.168.24.1     | 10.0.0.1       | first usable           |
#### 4.2.3 Sub-sub Netting (Variable Length Subnet Masking - VLSM)   
- Network: 192.168.10.0/26
- Subnet Mask: 255.255.255.192
- Block size (by CIDR): 3rd bit = 64
	- /26 = 64 IP, 4 blocks
	- /27 = 32 IP, 8 blocks
	- /28 = 16 IP, 16 blocks
- Sub-subnet based on requirements
	- e.g., "support >= 6 IPs", use /29

	| Subnet (/26)       | Network   | Usable   | Broadcast |
	| ------------------ | --------- | ----- | ------ |
	| 1 | 192.168.10.0   | .1-62    | .63  |
	| 2 | 192.168.10.64  | .65-126  | .127 |
	| 3 | 192.168.10.128 | .129-190 | .191 |
	| 4 | 192.168.10.192 | .193-254 | .255 |
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
	- 3-way: [ SYN, SYN/ACK, ACK ]
	- Guaranteed delivery (reliability, data integrity)
	- Resend if error, interrupting streaming/music
- **UDP**
	- [ send all ] 
	- Not guaranteed
	- Fast but no error-checking
### 6.2 Routing (L3)
- Packets treated independently
- No data-recovery 
- Media-independent
- IPv4 (32 bits), IPv6 (128 bits), OSI
- **Headers**
	- Version, IHL, **Service Type**, Total Length
	- ID, Flag, Fragment Offset
	- **TTL**, Protocol, **Header Checksum**
	- **Source addy**
	- **Destination addy**
	- Options, Padding
- **Router**
	- Path determination
	- Packet forwarding
### 6.3 Network Services (L2)
- **DNS**: resolves domain name → IP
- **DHCP**: assigns IPs →  clients
- **NAT**: translate private → public IPs
- **Switches**
	- Uses MAC to forward frames
	- Maintains MAC table (dynamic/static)
      	- Determined from source MAC (incoming ARP request, ARP reply)
	- Use **Flooding** when destination MAC unknown
	- Use **Spanning Tree Protocol** to avoid loops
	- Implements **ARP** to resolve IP to MAC
    	- 
	- **Duplex**
		- Half (sync, one at a time, walkie-talkie)
		- Full (async, simultaneous, no collisions)
## 7. Network Security
### 7.1 Firewall & IDS/IPS
- Operate on L3 and L4
- DPI (Deep Packet Inspection): content-based filtering
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
#### Steps
- Identify problem
- Gather information
- Analyze information
- Eliminate potential causes
- Propose hypothesis
- Test hypothesis
- Document solutions/workaround
#### Actual Steps
- Verify the host IPv4 address and subnet mask
- Ping the loopback address
- Ping the IPv4 address of the local machine
- Ping the default gateway
- Ping the remote server
  
``` 
// check duplex/speed for RJ45
show interfaces
```
- **Input queue drop**: CPU cannot process packet in time
- **Output queue drop**: Congestion on the interface
- **Input errors**: Cabling problems, interface hardware problems
- **Output errors**: Collisions during the transmission of a frame