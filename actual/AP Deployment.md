## Staging (upgrade firmware)

- WLC ip: 192.168.1.11
	- `admin/P@ssw0rd`
	- Monitoring > Wireless > AP Statistics
- Console in
	- `Cisco/Cisco`
- AP IP as long as /24 ok

```
// replace IP last octet with running number
capwap ap ip 10.10.10. 255.255.255.0 10.10.10.1
```

## Configs

- Refer to Tech Refresh v1.3 for correct details
- In AP config spreadsheet
	- change **ip**, **DGW**, hostname
	- (usually just change last digit of IP)
	- copy paste next, repeat

- Console in
	- speed `115200`
	- `uobadmin/P@ssw0rd`
	- copy paste commands from excel

	```
	// checks
	x#sh capwap ip con
	x#sh run
	
	// ignore errors because WLC change to UOB side one already
	```

- Save config
	- Copy paste hostname in terminal
	- Right click title bar > Change Settings
	- Session > Logging > All session output
	- Browse
	- Filename = `{hostname}.txt`

```
ter len 0 
sh ver
sh inv
sh capwap ip config
sh run
ter len 25
```

- Change back
	- `Session > Logging > None`
