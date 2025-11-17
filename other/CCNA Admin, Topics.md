```table-of-contents
title: 
style: nestedList
minLevel: 0 
maxLevel: 0 
include: 
exclude: 
includeLinks: true
hideWhenEmpty: false 
debugInConsole: false 
```

## 1. Exam Format

850/1000 to pass, ~2 minutes per question

### 1.1 Question Style

1. Configure
2. Monitor
3. Troubleshoot
4. Combination/More MCQ

### 1.2 Example Lab Questions

- Full question
	1. hostname
	2. intf description
	3. intf ip
	4. access/trunk
	5. VLAN
	6. password
	7. vty for SSH
- NAT
- inter-VLAN routing
- Troubleshooting
	- E.g., ping something, ping fail, find source of error

## 2. Online Test

- Proctors are very strict
- Physical exam suggested
- Huge hassle
	- Camera on
	- Eyes must be downwards
	- Day 1 Cisco trainer's test was terminated because he stretched and yawned lol

## 3. Self Study

- Exploring Layer 3 Redundancy
- Introducing WAN technologies
- Introducing QoS
- Explaining Wireless Fundamentals
- Introducing Architectures and Virtualization
- Explaining Software-DEfined Networking
- Introducing Network Programmability
- Examining the Security Threat Landscape
- Implementing Threat Defense Technologies

## 4. Bring For Test

- NRIC
- 

## 5. Topics

### ✅ **1.0 Network Fundamentals**

**(Basics, addressing, models, media, wireless concepts)**
- Components
- LAN, WAN, WLAN Overview
- Physical Media Types
- Ethernet Media Table
- Communication Types
- Host-to-Host process (same LAN)
- ARP
- IPv4 Addressing
	- Headers
	- Subnetting / VLSM
	- Binary calc
	- Private IPv4
- IPv6 Addressing
	- Headers
	- Unicast types
	- Multicast, Anycast
	- EUI-64
- Verify IP params (client OS)
- Wireless Principles
	- Channels
	- SSID
	- RF
	- Encryption
- Virtualization Fundamentals (VRF, containers)
- Switching Concepts
	- MAC learning
	- Frame switching/flooding
	- MAC table

---

### ✅ **2.0 Network Access**

**(Switching, VLANs, trunks, STP, EtherChannel, WLAN infra)**
- VLANs
	- Access ports
	- Trunk ports
	- 802.1Q
	- Native VLAN
	- Inter-VLAN connectivity (concept, not routing yet)
- Layer 2 discovery (CDP/LLDP)
- EtherChannel (LACP)
- Spanning Tree Protocol
	- STP / RSTP
	- Root port, root bridge
	- Port states
	- PortFast, BPDU guard/filter, root guard, loop guard
	- STP Toolkit
- Wireless architecture
	- AP modes
	- WLC interactions
	- AP/WLC cables, access/trunk/LAG

---

### ✅ **3.0 IP Connectivity**

**(Routing brain: routing table, static, OSPF, inter-VLAN routing)**
- Routing concepts
	- Routing table components
	- Routing codes, prefix, mask
	- Next hop
	- Admin distance
	- Metric
	- Gateway of last resort
- Router forwarding decision
	- Longest prefix match
	- AD
	- Metric
- **Inter-VLAN Routing**
	- Router on a Stick
	- L3 Switch
	- Dedicated router
- Static routing
	- Default / network / host routes
	- Floating static
- Dynamic Routing Protocols
	- RIP
	- EIGRP
	- OSPF (single area)
		- Neighbors
		- P2P
		- Broadcast (DR/BDR)
		- Router ID
	- IS-IS
	- BGP
- First Hop Redundancy Protocols
	- HSRP overview

---

### ✅ **4.0 IP Services**

**(NAT, DHCP, DNS, NTP, SNMP, Syslog, QoS, remote access, FTP/TFTP)**
- NAT (inside source, static + pools)
- DHCP (client/relay)
- DNS
- SNMP
- Syslog (facilities + levels)
- NTP (client/server)
- QoS (PHB, classification/marking/queuing/policing/shaping)
- Remote access
	- SSH
- FTP / TFTP

---

### ✅ **5.0 Security Fundamentals**

**(core security, device security, ACL, L2 security, wireless security)**
- Key security concepts (threats, vulns, exploits, mitigation)
- Security program elements
- Device access control
	- Local passwords
	- Password policies
	- MFA / certs / biometrics
- Layer 2 Security
	- DHCP snooping
	- DAI
	- Port security
	- VLAN hopping
	- ARP spoofing
- ACL (L3)
- Wireless security
	- WPA/WPA2/WPA3
	- Config WLAN (WPA2 PSK)
- IPsec VPN basics (remote + site-to-site)

---

### ✅ **6.0 Automation & Programmability**

**(SDN, controllers, APIs, JSON, config mgmt tools)**

- Automation impact
- Traditional vs controller-based networking
- SDN concepts
	- Control vs data plane
	- Overlay/underlay/fabric
	- Northbound/Southbound APIs
- AI/ML in network ops
- REST API basics
	- Auth types
	- CRUD
	- HTTP verbs
	- Encoding
- JSON components
- Config management
	- Ansible
	- Terraform
