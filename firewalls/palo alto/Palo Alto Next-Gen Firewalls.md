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

## 5. NAT Policy

- Private (internal) users to access the public internet (outbound)
- Public (external) users to access private (internal) servers

### 5.1 Source NAT Config

![](../../assets/Palo%20Alto%20Next-Gen%20Firewalls/img-20251124091928418.png)

- **Static IP**: fixed 1-to-1, changes source IP, same source port, implicit bidirectional rule feature
- **Dynamic IP**: 1-to-1 of source IP (no port number), private source address translates to next available address in the range
- **Dynamic IP and Port (DIPP)**: multiple clients to use same public IP with different source port numbers
	- Oversubscription: IP and port pair used multiple times in concurrent sessions

### 5.2 Destination NAT Config

![](../../assets/Palo%20Alto%20Next-Gen%20Firewalls/img-20251124091928449.png)

- Static IP:
- Dynamic IP: translates original IP to destination host with a DHCP-assigned IP

## 6. App ID

### 6.1 Attack Surface

- UDP Traffic
	- not-applicable
		- traffic dropped per policy before application identified
	- incomplete
		- Three-way handshake did not complete or was followed by no data
	- insufficient-data
		- Not enough payload for identification
	- unknown-TCP
		- unidentifiable TCP traffic.
	- unknown-p2p
		- unidentifiable peer-to-peer traffic
- 

### 6.2 Concepts and Operation

- Application Shifts (network traffic from one application to another)
- Application Dependencies
- Implicit Applications

## 7. Security Profiles

- Content-ID: advanced threat prevention engine and policies to inspect and control content traversing the firewall
- Scans for: [software vuln. exploits, viruses, spyware, malicious URLs, restricted files and data]
- Additional security checks on allowed traffic (no point inspecting blocked traffic)
- Threat logs
- Vulnerability, Antivirus, Anti-Spyware, File Blocking, Data Filtering Profiles

## 8. WildFire (Threat Intelligence Cloud)

![](../../assets/Palo%20Alto%20Next-Gen%20Firewalls/img-20251124091928484.png)

- Continuously updates malware/phishing samples to cloud detected from unknown file/url uploads
- Verdicts: [benign, grayware, malicious, phishing]

## 9. Decryption

### SSL/TLS

- Encryption for data privacy, Hashing for data integrity, Certs for auth
- Asymmetric Digital Certificate (PUB_KEY, PRIV_KEY) to share session key (symmetric)
- Forward Proxy
	- Firewall that acts as MITM to intercept and decrypt packets
	- Not a must to decrypt traffic, but just to check certificate
	- Determine what to inspect
- Reasons not to: [prohibited by law, non-standard SSL implementation, new CA, specific server certs required, server requires client certificate]

## 10. URL Filtering

- Got URL Filtering license ? URL Filtering profile : Security Rule
- Precedence (predefined, EDL, bottom)

## 11. Logs and Report

- Dashboard: [data filter, URL filer, threats] of last hour
- Application Command Center (ACC):  widgets
- Logs
	- Firewall: timestamped list of events
	- Traffic: entry for each firewall session
	- Threat: when detected in allowed traffic
		- Critical, high: blocked, Medium: might need to be blocked, Low, informational: alerts
	- URL: for CI/CD of Security policy/URL filtering profiles
	- WildFire: who sent, who received, filename/URL link used to deliver
	- Data: when sensitive files seen by firewall
	- Unified: all-in-one
- App Scope Reports
	- Summary: top 5 of below
	- Change monitor: largest increase in the number of sessions over selected time period
	- Threat monitor: largest number of sessions over selected time period
	- Threat map: source/desti of threats by country
	- Network monitor: bandwidth consumption
	- Traffic map: source/desti of traffic by country
- Predefined and Custom Reports
	- Choose columns
	- Schedule or on-demand
	- Web interface or export
- Device Telemetry
	- Sharing of data with Cloud
	- Log availability, log analysis (PAN-OS software), log retention, alerts, automated responses

## 11. PAN OS Upgrade

## Agenda

 - Manage Firewall Configurations
 - Manage Firewall Administrator Accounts
 - Block Threats Using Security and NAT Policies
 - Block Packet and Protocol Based Attacks
 - Block Threats from Known Bad Sources
 - Block Threats by Identifying Applications
 - Maintain Application Based Policies
 - Block Unknown Threats
