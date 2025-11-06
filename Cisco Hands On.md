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
	- ACPI (Application Centric Infrastructure)

## 2. General CLI

- user exec mode (>)
- privileged exec mode (#)

### 2.1 SSH

```
Switch#conf t
Switch(config)#ip domain-name <ip>
Switch(config)#crypto key generate rsa
Switch(config)#username <> privilege <> secret <>
Switch(config)#ip ssh version 2
Switch(config)#line vty 0 4
Switch(config)#transport input ssh
Switch(config)#login local
Switch(config)#exec-timeout 10 0
Switch(config)#end
Switch(config)#write mem
```

## 3. Switch Config

### 3.1 Basic

- By default
	- **NO** `shutdown` command applied
	- in `up/up` state if connected : `down/down` state if not connected

```
// start
Switch#conf t
Switch(config)#hostname <new switch host name>
```

```

// setting password (1): plaintext, (2) secure by default (MD5 hashed)

Switch#conf t
Switch(config)#enable password <password>
Switch(config)#service password-encryption       // type 7 encryption
Switch(config)#exit
Switch#show running-config | include enable	     // filters run-conf for "enable"
// == OR ==//
Switch(config)#enable secret <password>          // type 5 encryption (MD5)

// setting username and password
Switch#conf t
Switch(config)#username <username> <password/secret> <password>
Switch(config)#line console 0
Switch(config-line)#login local                  // use local login
Switch(config-line)#end
Switch#show running-config | include username    // filters run-conf for "username"
username <username> <password/secret> <password>
Switch#show running-config partition line

// example login
Switch#disable
Switch>enable
Password: <password>

// reset if no password 
Switch#write erase
reload

```

```
// disabling multiple interfaces
Switch(config)#int range f0/5 - 6, f0/9 - 12
Switch(config-if-range)#description ## not in use ##
Switch(config-if-range)#shutdown
```

```
conf t
interface GigabitEthernet0/0/1
 switchport mode access
 switchport access vlan 112
 spanning-tree portfast
exit

interface GigabitEthernet0/0/2
 switchport mode access
 switchport access vlan 112
 spanning-tree portfast
exit
```

```
// show
Switch#show mac address-table            // MAC table entries
Switch#show spanning-tree               // STP state, root bridge info
Switch#show interface status            // port status, VLAN, duplex, speed, type
Switch#show controllers                 // hardware stats (PHY, errors)
Switch#show running-config              // active config
Switch#show startup-config              // saved config

// make configs persistent
Switch#copy running-config startup-config      // `copy run start` works too
Switch#write memory

// wipe configs
Switch#erase startup-config
```

### 3.2 VLAN

```
// show
Switch#show vlan brief
Switch#show interfaces trunk

Router#show vlans (trunking's subinterfaces)
Router#show running-config interface gig0/1.10 (subinterface config)
Router#show ip route
Router#ip route 0.0.0.0 0.0.0.0 8.8.8.8

// config
Switch#conf t
Switch(config)#vlan <number> 
Switch(config-vlan)#name <name>
```

```
// access
// belongs to 1 VLAN, used for end devices
Switch(config-if)#switchport mode access

Switch(config-if)#switchport access vlan <number> (data traffic: PCs, printers)
// OR
Switch(config-if)#switchport voice vlan <number> (voice traffic: IP phone)
```

```
// trunk
// carries traffic for multiple VLANs
Switch#show interfaces trunk

Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk encapsulation dot1q 

// tag VLANs
Switch(config-if)#switchport trunk allowed vlan 10,20,30,99

// diff switch but same native VLAN 
Switch(config-if)#switchport trunk native vlan 99

Switch#show interfaces trunk
```

```
// router on a stick
R1(config)#int g0/0
R1(config-if)#no shut

R1(config-if)#int g0/0.10
R1(config-subif)#encap dot1q 10 <native>
R1(config-subif)#ip addr <ip> // last usable

R1(config-subif)#int g0/0.20
R1(config-subif)#encap dot1q 20 <native>

R1#sh ip int br

// setting native VLAN
R1(config-subif)#encap dot1q <vid> native
// OR
R1(config-if)#ip add <ip>
```

```
<<<<<<< HEAD
// layer 3 switch
// allow layer 3 routing
Switch(config)#ip routing

// create vlan first
Switch(config)#int vlan10

// config int
Switch(config)#int g0/1
Switch(config-if)#no switchport 
Switch(config-if)#no shut 
```

=======
// Switch Virtual Interface (SVI)
Switch(config)#ip routing
Switch(config)#int 'vlan10'
Switch(config-if)#no switchport
Switch(config-if)#no shut 

```
>>>>>>> origin/main
### 3.3 EtherChannel

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

### 3.4 Port Security

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

Switch#show port-security interface GigabitEthernet0/8

// port-status secure-down == violation

Switch#no switchport port-security mac-address sticky

```

### 3.5 Dynamic ARP Inspection (DAI)

- Layer 2 security feature to prevent ARP spoofing by validating ARP packets against DHCP snooping table

```

Switch(config)#ip arp inspection <vlan-id>     // enable DAI on VLAN

Switch(config-if)#ip arp inspection trust      // enable DAI on an interface

```

### 3.6 Syslog

```

// send syslog to Loopback0

Router(config)#logging host <IPv4>

Router(config)#logging trap informational

Router(config)#logging source-interface Loopback0

```

## 4. Router Config

### 4.1 Basic

- By default
	- `shutdown` command applied
	- in `administratively down/down` state

```
// start
Router>enable
Router#configure terminal
Router(config)#hostname <new router host name>
Router(config)#
Router(config)#exit
Router#

// show
Router#show version                 // OS version, uptime, model, serial, etc
Router#show protocols               // Routing protocols & active interfaces
Router#show arp                     // ARP table: IP ↔ MAC mappings
Router#show cdp neighbors detail    // Cisco Discovery info (device ID, IP, intf)
Router#show lldp neighbors          // LLDP neighbor info (vendor-neutral)
Router#show running-config          // Current active config in RAM
Router#show startup-config          // Saved config in NVRAM
Router#show interfaces [intf-id]    // Detailed stats: bandwidth, errors, duplex
Router#show interfaces description  // Interface names + brief status
Router#show ip protocols            // Shows routing protocol parameters
Router#show ip interface brief      // Quick IP + interface status summary
Router#show ip route                // IPv4 routing table
Router#show ipv6 interface brief    // IPv6 interface summary
Router#show ipv6 route              // IPv6 routing table
Router#show ipv6 neighbors          // IPv6 neighbor (ND) table

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

### 4.2 Routing Table

`show ip route`

| Code  | Destination Network | [Admin Distance/Metric] | Next-hop IP        | Time Elapsed | Outgoing Interface |
| ----- | ------------------- | ----------------------- | ------------------ | ------------ | ------------------ |
| **C** | 192.168.1.0/24      |–| directly connected |–| GigabitEthernet0/0 |
| **S** | 0.0.0.0/0           | [1/0]                   | via 192.168.1.1    | permanent    | GigabitEthernet0/0 |
| **D** | 10.10.10.0/24       | [90/30720]              | via 192.168.1.2    | 00:00:12     | GigabitEthernet0/1 |
| **O** | 172.16.0.0/16       | [110/20]                | via 10.1.1.2       | 00:00:30     | GigabitEthernet0/2 |
| **R** | 192.168.2.0/24      | [120/1]                 | via 10.0.0.1       | 00:02:15     | Serial0/0/0        |

### 4.3 Network Address Translation (NAT)

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

### 4.4 Access Control Lists (ACL)

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

// delete config then redo

```

```

// apply

Router(config-if)#ip access-group access-list-number ??????

Router#show access-lists

Router(config)#interface GigabitEthernet 0/1

Router(config-if)#ip access-group 15 out

```

### 4.5 Security (SSH, local)

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

### 4.6 Time Sync (Clock, NTP)

```

Router#show clock

Router#show clock detail

Router#clock set 18:00:00 17 Oct 2025

// sync NTP

Central(config)#ntp master 2

Branch(config)#ntp server 209.165.201.1

SW1(config)#ntp server 10.1.1.1

```

## 5. Test Configs

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

## 6. Router Power-On Sequence

- On boot,

| Component              | Function  | Description                             |
| ---------------------- | --------- | --------------------------------------- |
| ROM                    | POST      | Perform Power-On Self-Test (POST)       |
| ROM                    | Bootstrap | Load bootstrap program                  |
| Flash / TFTP Server    | Cisco IOS | Load operating system (Cisco IOS)       |
| NVRAM / TFTP / Console | Config    | Load startup config or enter setup mode |

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
```

## 7. Week 4 Hands On

### 7.1 Save Local Config

PuTTY > Session > Logging > 

1. "All session output"
2. Browse > save location
PuTTY > Session > start connection:

```
RTR01# terminal length 0     // ignores --more--  
RTR01# show running-config
```

### 7.2 Factory Reset

- FW - Fortinet
	- CLI>exec factoryreset
- FW - palo alto
	- Restore to 1st version
- Router/Switch
	- Switch(config)#write erase
	- Switch(config)#del vlan.dat
	- Switch(config)#reload

### 7.3 Common Troubleshooting Problems

- Gateway
	- Router(config)#ip route 0.0.0.0 0.0.0.0
	- Going to internet ok, but coming back specific
- Off internet/wifi
- Firewall
	- Source/Destination/Port/Service (must have)
- Port Channel
	- Ensure both switches use the same (LACP, PAgP, EtherChannel)
