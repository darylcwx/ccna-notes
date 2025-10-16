- [1. Products](#1-products)
- [2. Cisco CLI](#2-cisco-cli)
	- [2.1 Router Config](#21-router-config)
	- [2.2 Switch Config](#22-switch-config)
	- [2.3 Test configs](#23-test-configs)
	- [2.4 VLAN](#24-vlan)
	- [2.5 EtherChannel](#25-etherchannel)
- [3. Routing Table](#3-routing-table)
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
Router#show cdp neighbors <detail> (cisco products)
Router#show lldp neighbors 		   (general)
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

// setting password (1): plaintext, (2) secure by default (MD5 hashed)
- Switch(config)#enable password <password>
- Switch(config)#service password-encryption
- Switch# show running-config | include enable password
OR
- Switch(config)#enable secret <password>

// example login
Switch#disable
Switch>enable
Password: <password>

// show
Switch#show mac address-table
Switch#show spanning-tree
Switch#show interface status
Switch#show controllers
Switch#show running-config
Switch#show startup-config

// make configs persistent
Switch#copy running-config startup-config

// wipe configs
Switch#erase startup-config
```
### 2.3 Test configs
```
ping <ipv4/ipv6> [source <interface>]
telnet <ipv4/ipv6> [port]
tracert <ipv4>
traceroute <ipv4>
traceroute ipv6 <ipv6>
ssh user@<ipv4/ipv6>
arp -a
netstat
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
## 3. Routing Table
- **Example**
  - **Legend**: [Code, Destination Network, [Admin Distance (the lower the more trustworthy)/Metric (cost value)], Next Hop IP, Time Elapsed, Outgoing interface]
  - C 192.168.1.0/24 is directly connected, GigabitEthernet0/1
  - D 10.10.10.0/24 [90/30720] via 192.168.1.2, 00:00:12, GigabitEthernet0/1
- **Codes**
  	- C: Direct connection
  	- L: Local Interface
 	- R: RIP (Routing Information Protocol)
	- O: OSPF (Open Shortest Path First) 
	- D: EIGRP (Enhanced Interior Gateway Routing Protocol)
	- S: Static (manually configured)