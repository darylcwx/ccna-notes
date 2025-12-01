Question 14

Refer to the exhibit.

```
MacOs$ ifconfig

en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=400<CHANNEL_IO>
	ether f0:18:98:64:60:32
	inet6 fe80::492:c09f:57cf:8c36%en0 prefixlen 64 secured scopeid 0x6
	inet 10.8.138.14 netmask 0xffffe000 broadcast 10.8.159.255
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect
	status: active
```

If the default gateway is the first usable IP address in the subnet, what is the default gateway?

- A. 10.8.138.1
- B. 10.8.144.1
- C. 10.8.132.1wrong
- D. 10.8.128.1correct

Explanation

From the netmask 0xffffe000 (hexadecimal), we deduce the subnet mask is 255.255.224.0 as f (hexa) = 1111 (binary) while e (hexa) = 1110 (binary). Therefore for the 3rd octet it is e0 = 1110 0000 = 224 -> Increment: 32

The PC IP address is 10.8.138.14 so we can find the 3rd octet of this subnet is: 138 / 32 = 4 (only get the integer). And 32 * 4 = 128. Therefore the subnet is 10.8.128.0/19 -> The first usable IP address is **10.8.128.1**.

<hr>
Question 43

A network engineer must configure two new subnets using the address block 10.70.128.0/19 to meet these requirements:

* The first subnet must support 24 hosts.
* The second subnet must support 472 hosts
* Both subnets must use the longest subnet mask possible from the address block

Which two configurations must be used to configure the new subnets and meet a requirement to use the first available address in each subnet for the router interfaces? (Choose two)

- A. interface vlan 1148
		ip address 10.70.148.1 255.255.254.0correct
- B. interface vlan 4722
		ip address 10.70.133.17 255.255.255.192
- C. interface vlan 3002
		ip address 10.70.147.17 255.255.255.224
- D. interface vlan 1234
		ip address 10.70.159.1 255.255.254.0
- E. interface vlan 155
		ip address 10.70.155.65 255.255.255.224correct

Question was not answered

Explanation

In order to support 24 (<25) hosts we need 5 bits 0 in the subnet mask so the last octet of the subnet mask must be 1110 0000 -> 255.255.255.224. In the answer above there are two IP address with subnet mask 255.255.255.224. They are:

+ 10.70.147.17 255.255.255.224: This IP address belongs to subnet 10.70.147.0/27 but 10.70.147.17 is not the first available address in this subnet (the first available address in this subnet is 10.70.147.1)
+ 10.70.155.65 255.255.255.224: This IP address belongs to subnet 10.70.155.64/27 and 10.70.155.65 is the first available address in this subnet -> Answer 'interface vlan 155
ip address 10.70.155.65 255.255.255.224' is correct.

In order to support 472 (<512 = 29) hosts we need 9 bits 0 in the subnet mask -> 255.255.254.0. In the answer above there are two IP address with subnet mask 255.255.254.0. They are:

+ 10.70.148.1 255.255.254.0: This IP address belongs to subnet 10.70.148.0/23 and it is the first available IP address in this subnet
+ 10.70.159.1 255.255.254.0: This IP address belongs to subnet 10.70.158.0/23. It is not the first available IP address in this subnet (the first available IP address is 10.70.158.1).

-> Answer 'interface vlan 1148
ip address 10.70.148.1 255.255.254.0' is correct.

<hr>

Question 50

Refer to the exhibit.

![OSPF_summary_address.jpg](https://www.9tut.com/images/ccna/OSPF/OSPF_summary_address.jpg)

Router R1 receives static routing updates from routers A, B, C. and D. The network engineer wants R1 to advertise static routes in OSPF area 1. Which summary address must be advertised in OSPF?

- A. 10.1.40.0/25
- B. 10.1.40.0/23correct
- C. 10.1.41.0/25
- D. 10.1.40.0/24

Explanation

Maybe there is a typo in this question for the subnet of routerC as 10.1.40.176/28 belongs to 10.1.40.128/25 subnet of routerB. Therefore we guess in fact the subnet of routerC is 10.1.**41**.176/28.

In four options only the subnet mask of /23 can cover all of these subnets so answer '10.1.40.0/23' is the best choice.
