R2(config)#access-list 1 deny 192.168.1.0 0.0.0.255

R2(config)#access-list 1 permit any

R2(config)#int g0/2

R2(config)#ip access-list 1 out

R2(config)#ip access-list standard

R2(config)#access-list 2 deny 192.168.2.1

R2(config)#access-list 2 permit 192.168.2.0 0.0.0.255

R2(config)#access-list 2 permit host 192.168.1.1

R2(config)#access-list 2 deny 192.168.1.0 0.0.0.255

R2(config)#access list 2 permit any

R2(config)#int g0/1

R2(config)#ip access-list 2 out

R1(config-ext-nacl)#permit ip 0.0.0.0 255.255.255.255 0.0.0.0 255.255.255.255

R1(config-ext-nacl)#deny udp 10.0.0.0 0.0.255.255 192.168.1.1 0.0.0.0

R1(config-ext-nacl)#deny icmp 172.16.1.1 0.0.0.0 192.168.0.0 0.0.0.255

R1(config-ext-nacl)#permit tcp 10.0.0.0 0.0.255.255 2.2.2.2 0.0.0.0 eq 443

R1(config-ext-nacl)#deny udp any range 20000-30000 3.3.3.3 0.0.0.0

R1(config-ext-nacl)#permit tcp 172.16.1.0 0.0.0.255 gt 9999 4.4.4.4 0.0.0.0 neq 23

R1(config)#ip access-list extended http_svr1
R1(config-ext-nacl)#deny tcp 192.168.1.0 0.0.0.255 host 10.0.1.100 eq 443
R1(config-ext-nacl)#permit any any
R1(config-ext-nacl)#int g0/1
R1(config-if)#ip access-group http_svr1 in

R1(config-if)#ip access-list ext 192.168.2.0_no_10.0.2.0
R1(config-ext-nacl)#deny 192.168.2.0 0.0.0.255 10.0.2.0 0.0.0.255
R1(config-ext-nacl)#permit ip any any
R1(config-ext-nacl)#int g0/2
R1(config-if)#ip access-group 192.168.2.0_no_10.0.2.0 in

R1(config-if)#ip access-list extended acl3
R1(config-ext-nacl)#deny icmp 192.168.1.0 0.0.0.255 10.0.1.0 0.0.0.255
R1(config-ext-nacl)#deny icmp 192.168.1.0 0.0.0.255 10.0.2.0 0.0.0.255
R1(config-ext-nacl)#deny icmp 192.168.2.0 0.0.0.255 10.0.1.0 0.0.0.255
R1(config-ext-nacl)#deny icmp 192.168.2.0 0.0.0.255 10.0.2.0 0.0.0.255
R1(config-ext-nacl)#permit any any
R1(config-ext-nacl)#int g0/0
R1(config-if)#ip access-group acl3 out