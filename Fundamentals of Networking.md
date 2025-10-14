```table-of-contents
```
# Components
- Endpoints: devices
- Intermediary/Network devices: switches etc
- Media: ethernet, serial, wireless links
	- 802.11: WLAN
	- 802.15: Bluetooth
	- 802.16: WiMAX
- Services: computer running FTP
# LAN, WAN
- Rack switches (layer 2, ethernet, connects devices within network)
	- Uses other routes if congested
	- ARP: IP to MAC table
	- Loop avoidance (Spanning Tree Protocol)
	- LAN Communication
		- Channels
			- Optical Fiber
				- [core, cladding, buffer]
				- [9, 125, 250] µm
				- Single-mode: [laser, higher BW & speed, longer distance, $$$]
				- Multi-mode: [LED, slower BW & speed, shorter distance, $]
			- Twisted Pair (ethernet LAN)
				- UTP: Unshielded Twisted Pair
				- STP: Shielded Twisted Pair
					- Reduce inteference
					- CAT 5, 5e, 6, 6a, 7, 8
				- S/STP: Screened STP
					- More shields
			- Coaxial
				- Copper core
				- Legacy use
				
		- Types
			- unicast (devices near you)
				- one to one
				- "flood" if unsure of MAC addy
			- broadcast
				- one to all (ARP)
			- anycast
				- one to nearest/best (CDNs)
			- multicast
				- one to selected few (netflix)
				- Protocol-Independent Multicast
		- Modes
			- Half Duplex: [sync, walkie-talkie, slower, old]
			- Full Duplex: [async, no collisions, faster, modern]
	- MAC table
		- Stores MAC addy found when receiving Frame 
		- Unknown? "flood"
		- Static: Perm
		- Dynamic: Stored in RAM

- Routers (connects LAN to WAN/internet)
- Access Points
- Connects LAN to storage area network (HDDs, SSDs)
- WLAN = wireless LAN
- WAN = large LAN, managed by SIs, has WAN optimizer
- VPN = virtual extension of LAN
	- SSL & IPsec [L2, L3, L4, L3, L4]
	- L2-in-ip [L2, L3, L2, L3, L4]
- Internet = large public WAN
- AP (can be connected to switch, or another AP, or have SIM card)
	- Cloud-based AP (NaaS)
- TCP/IP driver resides in the NIC of the device

# Wireless Access and BYOD
- Cost savings
- Reduced hardware expenses
- Work from anywhere within the office/hot-desking
- Secure remote collaboration
- Employee own device == welfare

# Enterprise
- [Connectivity, Security]

# Data Center
- [Availability, Resiliency, Scalability]
- DC Switches (a lot faster, massive, low-latency)
- Small Form-Factor Pluggable (1-800 gigabits/s)


# Web Servers, DMZ, Extranet
- Web servers in DMZ, a controlled zone between Internet and internal network
- Usually a subnet the hosts public services

# MAC Address (48 bits)
- 24 vendor, 24 device
- For delivery
- Hexadecimal (0-9 then A-F)
# IP Address
- 0.0.0.0 / 16
- 4 octets, 8 bits per octet = 32 bits
- 8 bits = max value 255
- Binary / 2

| Base    | 2^8 | 2^7 | 2^6 | 2^5 | 2^4 | 2^3 | 2^2 | 2^1 |
| ------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Sum     | 128 | 64  | 32  | 16  | 8   | 4   | 2   | 1   |
| Cum     | 128 | 192 | 224 | 240 | 248 | 252 | 254 | 255 |
| Binary  | 1   | 0   | 1   | 1   | 1   | 0   | 0   | 1   |
| Decimal | 128 | 0   | 32  | 16  | 8   | 0   | 0   | 1   |
## Classes
- A B C, easy as 1 2 3
- Some are reserved (127.0.0.1)
- ==Subnet prefix must match class==

| Class | Fixed Bit | Range                         | Subnet        |    
| ----- | --------- | ----------------------------- | ------------- | 
| A     | 1         | 10.0.0.0 - 10.255.255.255     | 255.0.0.0     |    
| B     | 2         | 172.16.0.0 - 172.16.255.255   | 255.255.0.0   |     
| C     | 3         | 192.168.0.0 - 192.168.255.255 | 255.255.255.0 |     
| D     | X         | 224 - 239                     | multi class   |     
| E     | X         | unsure                        | reserved      |     
## Example
| Example           | IP 1             | IP 2           | How                    |
| ----------------- | ---------------- | -------------- | ---------------------- |
| IP address        | 192.168.24.19/24 | 10.0.1.15/8    | N.A.                   |
| Subnet mask       | 255.255.255.0    | 255.0.0.0      | bits taken for network |
| Network address   | 192.168.24.0     | 10.0.0.0       | smallest               |
| Broadcast address | 192.168.24.255   | 10.255.255.255 | largest                |
| Host addresses    | 192.168.24.19    | 10.0.1.15      | same                   |
| Default gateway   | 192.168.24.1     | 10.0.0.1       | first usable           |
## Routing
- Internet layer
- Packets treated independently
- No data-recovery 
- Media-independent
- IPv4 (32 bits), IPv6 (128 bits), OSI
	- Version, IHL, Service Type, Total Length
	- ID, Flag, Fragment Offset
	- TTL, Protocol, Header Checksum
	- Source addy
	- Destination addy
	- Options, Padding
# OSI Model 
| Layer        | Explanation                                        | Analogy                                       |
| ------------ | -------------------------------------------------- | --------------------------------------------- |
| Application  | app interface                                      | letter to post office                         |
| Presentation | data formatting, encryption                        | translate language                            |
| Session      | manages connections                                | post office opens a comms line to destination |
| Transport    | reliable delivery (TCP/UDP)                        | package is tracked and delivery guaranteed    |
| Network      | routing across networks (core & distribution), IPs | best route for package delivery               |
| Data Link    | local delivery and switching (access)              | package handed to local delivery trucks       |
| Physical     | transmission of raw bits                           | roads, trucks, wires                          |
## Encapsulation 
| Layer       | Items                                                               |
| ----------- | ------------------------------------------------------------------- |
| Application | User data, Data header                                              |
| Transport   | User data, Data header, Segment header                              |
| Internet    | User data, Data header, Segment header, Packet header               |
| Link        | User data, Data header, Segment header, Packet header, Frame header |
## Transport
- Ports reside here
- TCP/IP
	- [ SYN, SYN/ACK, ACK ]
	- guaranteed delivery (reliability, data integrity)
	- resend if error, interrupting streaming/music
- UDP
	- [ send all ] 
	- not guaranteed
	- fast but no error-checking
### Firewall/IDS
- Operate on L3 and L4
- Deep Packet Inspection (DPI)
# DNS
- Dictionary, Domain to IP table
# DHCP
- 
# NAT
## Session
- SIP/H323
- Packet-Switch, Circuit-Switch
# Cisco
## Products
- NX-OS
	- Nexus Validation Training (NVT)
		- Comprehensive end-to-end systems test
- AI Canvas
	- Agents talk to relevant agents to solve problems
- Switches
	- Nexus 4000 - 9000
	- ACI (Application Centric Infrastructure)
# Characteristics of a Network
- Topology: physical connectivity of devices and logical paths of data flows
	- Network diagram
		- Physical (layout)
		- Logical (pathing)
		- Colour legend
- Bitrate/Bandwidth: rate in bits per second
- Availability: SLA for 99.99% uptime, Fault tolerance: 100%
- Reliability: operate without failures
- Scalability: scale without affecting performance
- Redundancy: more paths in case of failure
- Security: defense from threats
- Quality of Service: tools/mechanisms to use network resources
- Cost: expense + maintenance
- Virtualization: emulated (storage, compute, network, security)
# KPIs
- Bandwidth
- Latency
- Jitter
- Packet Loss
# Applications
- Data: smooth, benign, drop & delay insensitive
- Voice: smooth, benign, drop & delay sensitive
	- Latency <= 150ms, Loss <= 1%, Bandwidth (30-128Kbps)
	- Latency <= 150ms, Loss <= 0.1%-1%, Bandwidth (384 Kbps - 20+ Mbps)
- Video: bursty, greedy, drop & delay sensitive
- RFC: Request For Comments (universal SLAs)
	- Generally HTTP response <= 400ms
- Classifications
	- Batch
	- Interactive
	- Real-Time

