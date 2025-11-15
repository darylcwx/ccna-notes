

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


