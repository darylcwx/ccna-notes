# Operating Cisco IOS Software
Implementing and Administering Cisco Solutions (CCNA) v2.2



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
## Cisco CLI
### CLI
```
// ping
ping 10.10.50.2 source ethernet 0/0

// test transport layer
telnet 10.10.50.2 80

// arp
show arp

// trace route

```

### 1. Router Config
- user exec mode (>)
- privileged exec mode (#)
```
Router> 
Router>enable 
Router#
Router#configure terminal
Router(config)#
Router(config)#hostname NYEDGE1
NYEDGE1(config)#
NYEDGE1(config)#exit
NYEDGE1#

// show router's interfaces
NYEDGE1#show ip interface brief

// show cable type of respective serial and port
NYEDGE1#show controllers serial 0/0/0

// show routing protocols
NYEDGE1#show ip protocols
=> NSF aware (Non Stop Forwarding enabled)

// show version
NYEDGE1#show version

// enter interface config mode
NYEDGE1#configure terminal
NYEDGE1(config)#
NYEDGE1(config)#interface gigabitEthernet 0/0
NYEDGE(config-if)#

// set params
NYEDGE1(config-if)#ip address 192.168.16.1 255.255.255.0
NYEDGE1(config-if)#no shutdown
NYEDGE1(config-if)#exit
NYEDGE1(config)#ip domain-name practice-labs.com
NYEDGE1(config)#

// generate RSA key
crypto key generate rsa
2048

// enable SSH
NYEDGE1(config)#ip ssh version 2
NYEDGE1(config)#exit
NYEDGE1#show ip ssh

// set vty lines for SSH transport
NYEDGE1#configure terminal
NYEDGE1(config)#line vty 0 4
NYEDGE1(config-line)#transport input ssh
NYEDGE1(config-line)#login local
NYEDGE1(config-line)#exit

// configure local account
NYEDGE1(config)#username Joe privilege 15 secret CISCO
NYEDGE1(config)#
```
### 2. Switch Config
```
Switch>enable
Switch#configure terminal
Switch(config)#hostname NYACCESS1
NYACCESS1(config)#enable password cisco1
NYACCESS1(config)#exit
NYACCESS1#disable
NYACCESS1>enable
Password: cisco1
NYACCESS1#show running-config
NYACCESS1#configure terminal
NYACCESS1(config)#enable secret cisco
NYACCESS1>enable
Password: cisco

// encrypt passwords
NYACCESS1#conf t
NYACCESS1(config)#service password-encryption
NYACESS1# show running-config | include enable password

// reuse config
NYACCESS1#show startup-config
NYACCESS1#copy running-config startup-config
NYACCESS1#erase startup-config

```

## 2. Routing Table
- **Example**

  - [Code, Destination Network, [Admin Distance (the lower the more trustworthy)/Metric (cost value)], Next Hop IP, Time Elapsed, Outgoing interface]
  - C 192.168.1.0/24 is directly connected, GigabitEthernet0/1
  - D 10.10.10.0/24 [90/30720] via 192.168.1.2, 00:00:12, GigabitEthernet0/1

- **Codes**
  	- C: Direct connection
  	- L: Local Interface
 	- R: RIP (Routing Information Protocol)
	- O: OSPF (Open Shortest Path First) 
	- D: EIGRP (Enhanced Interior Gateway Routing Protocol)
	- S: Static (manually configured)


- Longest Prefix Match
- LLDP
```
// enable 
Router#config terminal
Router(config)# [no] lldp run 			// global
Router(config-if)# [no] lldp transmit	// on interface
Router(config-if)# [no] lldp receive	// on interface

// show neighbors
// lldp - general, cdp - cisco products
Router#show lldp neighbors
Switch#show cdp neighbors
Switch#show cdp neighbors detail

```