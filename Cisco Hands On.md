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
## 1. Products
- NX-OS
	- Nexus Validation Training (NVT)
		- Comprehensive end-to-end systems test
- AI Canvas
	- Agents talk to relevant agents to solve problems
- Switches
	- Nexus 4000 - 9000
	- ACI (Application Centric Infrastructure)
## 2. Cisco CLI
- user exec mode (>)
- privileged exec mode (#)
### 2.1 Router Config
```
// start
Router>enable 
Router#configure terminal
Router(config)#hostname <new router host name>
Router(config)#
Router(config)#exit
Router#

// show
Router#show version
Router#show protocols
Router#show arp
Router#show cdp neighbors <detail (for IPv4)> (cisco)
Router#show lldp neighbors 		   			  (general)
Router#show running-config
Router#show startup-config
Router#show interfaces [interface-id]
Router#show ip protocols
Router#show ip interface brief
Router#show ip route
Router#show ipv6 interface brief
Router#show ipv6 route
Router#show ipv6 neighbors

// config interface
// in config-if mode, add "do" to execute "show" commands 
// e.g. Router(config-if)#do sh ipv6 interface brief

// hide from lldp neighbor discovery
Router(config-if)# [no] lldp run
Router(config-if)# [no] lldp transmit (hide from others in LLDP table)
Router(config-if)# [no] lldp receive  (don't see others in LLDP table)

Router(config)#interface gigabitEthernet 0/0

- Router(config-if)#ip address <ipv4> <subnet-mask>
OR
- Router(config-if)#ipv6 address <ipv6>/<prefix>
OR
- Router(config-if)#ipv6 address autoconfig

Router(config-if)#no shutdown
```
```
// generate RSA key
crypto key generate rsa
2048

// enable SSH
Router(config)#ip ssh version 2
Router(config)#exit
Router#show ip ssh

// set vty lines for SSH transport
Router#configure terminal
Router(config)#line vty 0 4
Router(config-line)#transport input ssh
Router(config-line)#login local
Router(config-line)#exit

// configure local account
Router(config)#username Joe privilege 15 secret CISCO
Router(config)#
```
### 2.2 Switch Config
```
// start
Switch#conf t
Switch(config)#hostname <new switch host name>
```
```
// setting password (1): plaintext, (2) secure by default (MD5 hashed)
Switch#conf t
Switch(config)#enable password <password>
Switch(config)#service password-encryption // type 7 encryption
Switch(config)#exit
Switch#show running-config | include enable	// filters for "enable" in running-config
// OR //
Switch(config)#enable secret <password> // type 5 encryption (MD5)

// setting username and password
Switch#conf t
Switch(config)#username <username> <password/secret> <password>
Switch(config)#line console 0
Switch(config-line)#login local
Switch(config-line)#end
Switch#show running-config | include username
username <username> <password/secret> <password>
Switch#show running-config partition line
...

// example login
Switch#disable
Switch>enable
Password: <password>
```
```
// show
Switch#show mac address-table
Switch#show spanning-tree
Switch#show interface status
Switch#show controllers
Switch#show running-config
Switch#show startup-config

// make configs persistent
Switch#copy running-config startup-config // `copy run start` works too

// wipe configs
Switch#erase startup-config
```
### 2.3 Test configs
```
ping <ipv4/ipv6> [source <interface>]	(packet loss)
telnet <ipv4/ipv6> [port]				(port check, clear text)
tracert <ipv4>							(hop path windows)
traceroute <ipv4>						(hop path mac/linux)
traceroute ipv6 <ipv6>					(IPv6 hop path)
ssh user@<ipv4/ipv6>					
arp -a
netstat									(active ports/connections)
```
### 2.4 VLAN
```
// show
Switch#show vlan brief
Switch#show interfaces trunk
Router#show vlans (trunking's subinterfaces)
Router#show running-config interface gig0/1.10 (subinterface config)
Router#show ip route

// config
Switch#conf t
Switch(config)#vlan <number> (number: standard 1-1005, extended 1006-4094)
Switch(config-vlan)#name <name>
Switch(config)#interface <interface>
```
```
// access 
// belongs to 1 VLAN, used for end devices
Switch(config-if)#switchport mode access
- Switch(config-if)#switchport access vlan <number> (data traffic: PCs, printers)
OR 
- Switch(config-if)#switchport voice vlan <number> (voice traffic: IP phone)
```
```
// trunk
// carries traffic for multiple VLANs
// connects (Sw to Sw) or (Sw to Routers)
// tags frames with VLAN ID using IEEE 802.1Q
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk encapsulation dot1q (specifies 802.1Q)
Switch(config-if)#switchport trunk native vlan 99 (untagged VLANs)
Switch(config-if)#switchport trunk allowed vlan 10,20,30,99
Switch(config-if)#
```
```
// verify
// "dynamic desirable" = auto mode, "switchport" = static access
Switch#show interfaces <interface> switchport
Switch#show interfaces trunk
```
### 2.5 EtherChannel
- Bundle multiple interfaces into 1 logical trunk (port-channel), configure VLAN/trunk on channel, bring links up, check summary
```
// show
Switch#show etherchannel summary

// config
// shutdown for safer config
Switch#interface range GigabitEthernet 0/1-4
Switch(config-if-range)#shutdown

// add port 0/1-4 to channel 1
// mode active = use LACP
Switch(config-if-range)#channel-group 1 mode active
Switch(config-if-range)# exit

// config port-channel
Switch(config)# interface port-channel 1
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk allowed vlan 1,2,20

// up ports
Switch(config-if)#interface range GigabitEthernet0/1-4
Switch(config-if-range)#no shutdown
```
### 2.6 Syslog
```
// send syslog to Loopback0
Router(config)#logging host <IPv4>
Router(config)#logging trap informational
Router(config)#logging source-interface Loopback0
```
### 2.7 Software Clock, NTP
```
Router#show clock
Router#show clock detail
Router#clock set 18:00:00 17 Oct 2025

// sync NTP
Central(config)#ntp master 2
Branch(config)#ntp server 209.165.201.1
SW1(config)#ntp server 10.1.1.1
```
### 2.8 Router Power-On Sequence
- On boot,
  
	| Component | Function | Description |
	| --- | --- | --- |
	| ROM | POST | Perform POST |
	| ROM | Bootstrap | Load Bootstrap |
	| Flash, TFTP server | Cisco IOS | Load OS |
	| NVRAM, TFTP server, Console | Config | Load config file or enter setup mode |
```
Router#show file systems

// this means still in "bios"
rommon>

// manual boot
Branch(config)#boot system flash:/c2900....bin // from flash
Branch(config)#boot system tftp://c2900....bin // from TFTP server
Branch(config)#boot system rom
```
- Validates IOS Images via MD5 checksum
- Store and manage IOS images and configs on a central TFTP server
- Secure Copy Protocol (SCP) for secure file transfer
```
// save config to a TFTP server
Branch#copy running-config tftp:
Address...? 172.16.1.100
Desti filename..? config.cfg
[copied in X secs]

// merge config file from TFTP
Branch#copy tftp: running-config
Address...? x
Source filename? config.cfg
...
```
### 2.9 Security Related
```
// logout on timeout
Switch#conf f
Switch(config)#line console 0 
Switch(config-line)#exec-timeout 5
```
```
// securing remote access - virtual terminal password config
Switch(config)#line vty 0 15
Switch(config-line)#login
Switch(config-line)#password <password>
```
```
// SSH config
Switch (config)#hostname Switch
Switch(config)#ip domain-name cisco.com
Switch(config)#username <username> secret <secret>
Switch(config)#crypto key generate rsa modulus 2048
...
*Dec 25 13:37:42.000 %SSH-5-ENABLED: SSH 1.99 has been enabled
...
Switch(config)#line vty 0 15
Switch(config-line)#login local
Switch(config-line)#transport input ssh
Switch(config-line)#exit
Switch(config)#ip ssh version 2
```
```
// login banner
Switch(config)#banner login "Please enter uName and pWord."
// a user trying to connect will see the above message
```
### 2.10 Port Security
- Change the switchport mode from auto to either
access or trunk
- Set the max number of secure MACs
- Specify the allowed MACs.
- Set the violation mode (optional): [protect, restrict, shutdown (default)]
- Set aging parameters (optional): [aging time, aging type {absolute (default) | inactivity}]
- Enable port security
```
Switch(config)# interface g0/8
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 2
Switch(config-if)# switchport port-security violation shutdown
Switch(config-if)# switchport port-security mac-address sticky
Switch(config-if)# switchport port-security aging time 120

// show
Switch#show port-security
Switch#show port-security inerface GigabitEthernet0/8
// port-status secure-down == violation

// 
Switch#no switchport port-security mac-address sticky
```
### 2.11 Dynamic ARP Inspection (DAI)
```
//enable DAI on VLAN
ip arp inspection <vlan-id>

// enable DAI on an interface
ip arp inspection trust
```
## 3. Routing Table
- **Example**

| Code  | Destination Network | [Admin Distance/Metric] | Next-hop IP        | Time Elapsed | Outgoing Interface |
| ----- | ------------------- | ----------------------- | ------------------ | ------------ | ------------------ |
| **C** | 192.168.1.0/24      | –                       | directly connected | –            | Gigabit| |Ethernet0/0 |
| **S** | 0.0.0.0/0           | [1/0]                   | via 192.168.1.1    | permanent    | GigabitEthernet0/0 |
| **D** | 10.10.10.0/24       | [90/30720]              | via 192.168.1.2    | 00:00:12     | GigabitEthernet0/1 |
| **O** | 172.16.0.0/16       | [110/20]                | via 10.1.1.2       | 00:00:30     | GigabitEthernet0/2 |
| **R** | 192.168.2.0/24      | [120/1]                 | via 10.0.0.1       | 00:02:15     | Serial0/0/0        |
- **Codes**
  	- **C**: Direct connection
  	- **L**: Local Interface
 	- **R**: RIP (Routing Information Protocol)
 	- **D**: EIGRP (Enhanced Interior Gateway Routing Protocol)
	- **O**: OSPF (Open Shortest Path First) 
	- **S**: Static (manually configured)
## 4. NAT (Network Address Translation)
```
// show
Router(config)#show ip nat translations

// config static
Router(config)#ip nat inside source static 172.16.1.10 209.165.200.230 8080
Router(config)#ip nat inside source static tcp 192.168.10.254 80 209.165.200.226 8080

// config dynamic
// acl required to specify which IPs to be translated
Router(config)#access-list 1 permit 10.1.1.0 0.0.0.255
Router(config)#ip nat pool NAT-POOL 209.165.200.230 209.165.200.235 netmask 255.255.255.254
Router(config)#ip nat inside source list 1 pool NAT-POOL

// config PAT
// same idea as dynamic, but need specify outside interface
Router(config)#access-list 1 permit 172.16.1.0 0.0.0.255
Router(config)#ip nat inside source list 1 interface GigabitEthernet 0/1 overload
```
## 5. ACL (Access Control Lists)
```
// by number
Router(config)#access-list 1 deny host 172.16.3.3
Router(config)#access-list 1 permit 172.16.0.0 0.0.255.255

// by name
Router(config)# ip access-list standard acl1
Router(config-std-nacl)# deny host 172.16.3.3
Router(config-std-nacl)# permit 172.16.0.0 0.0.255.255
```
```
// extended IPv4
Router(config)#ip-access-list extended 101
Router(config-ext-nacl)#permit tcp host 172.16.3.3 range 56000 60000 host 203.0.113.30 eq 80
```
```
// delete
Router(config)#no access-list <access-list-number>
Router(config)#no ip access-list <standard/extended> <access-list-name>
```
```
// modify
// delete then redo
```
```
// apply
Router(config-if)#ip access-group access-list-number ??????

Router#show access-lists
Router(config)#interface GigabitEthernet 0/1
Router(config-if)#ip access-group 15 out

```