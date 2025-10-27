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

## Palo Alto Networks Portfolio

## Connect to the Management Network

### 1.1 Service Routes

- By default, MGT port is used to access external services

### 1.2 Activate Firewall

- PAN-OS Software Updates
	- Check now, download, install
- Dynamic Updates

## 2. Configuration Management

### 2.1 Firewall Types

- Running Configuration
- Candidate Configuration
- Actions: [revert, save, load, export, import]
- Full Commit, Per-admin Commit
- [Preview, Summary, Validate]
- [Commit lock, Config lock]

### 2.2 Firewall Logs

- View, filter, download in CSV

## 3. Zone and Interfaces

### 3.1 Security Zones Overview

- Network Segmentation (Security zones)
	- Reduces attack surface by grouping functional departments
- ZTA
	- Inspect both outbound and inbound
- Network Interfaces
	- 

### 3.2 Network Interfaces

- Firewall data plane controls in-band interfaces (each band 1 zone)
	- `ethernet 1/1`
- Split into logical interfaces (VLAN tagging)
	- `ethernet 1/1.1` + `ethernet 1/1.2`
- Usually L3 zone [L3, VLAN, loopback, tunnel]
- L2 zone, V-wire zone, Tunnel zone, Tap zone
- MGT and HA not assigned to zones

### 3.3 Interface Types

- **Tap**
	- Passive monitoring of switch traffic from a port
	- Cannot control or shape traffic
	- For evaluation and auditing
- **Virtual Wire (V-wire)**
	- L1+ (no MAC address)
- **Layer 3**
	- V-wire + L3 services (logical routers, VPN, routing protocols)
	- Enable IPv4 and IPv6 support (enable IPv6 on firewall)

### 3.4 Logical Routers and Layer 3 Interfaces

- Logical Router
	- Support [BGPv4, OSPFv2, OSPFv3, RIPv2]
	- Multicast routing [PIM_SM, PIM-SSM]
	- Assign [multiple] static default routes
		- Route with lowest metric
		- Path monitoring to determine if routes are usable
		- Firewall switches default route during path failure

## 4. Security Policy

### 4.1 Security Policy Fundamentals

- Flow Logic (Session Setup)
	1. Source Zone
	2. Zone and/or DoS Protection
	3. Forwarding Lookup (PBF)
	4. Destination Zone
	5. Security Policy Check
	6. Assign Session ID

- Inspect and Control Network Traffic
- Sessions and Flows
- Display Security Policy Rules (Policies > Security)
- Manage Ruleset [add, delete, clone, override, revert, enable, disable, move]
- Security Policy Rule Types [intrazone, interzone, universal]
- Custom and Predefined Rules
- Security Policy Rule Match (top to bottom, once match, return)
- Policy Rule Hit Count (hit rate)
- Rule Shadowing (old unused rules)

### 4.2 Security Policy Administration

- General Tab/Rule Changes Archive (audit)
- Source Tab/Destination Tab
- Application Tab (dependencies)
	- Unresolved = reported
- Service/URL Category Tab
- New Service Definition (assigned ports that applications can use, by default only: service-http, service-https)
- Actions Settings (scheduling)
	- Usage Tab (audit)
- Enable Intrazone and Interzone Logging
- Find unused Security Policy Rules (remove to up efficiency/simplification)

## Agenda

 - Manage Firewall Configurations
 - Manage Firewall Administrator Accounts
 - Block Threats Using Security and NAT Policies
 - Block Packet and Protocol Based Attacks
 - Block Threats from Known Bad Sources
 - Block Threats by Identifying Applications
 - Maintain Application Based Policies
 - Block Unknown Threats
