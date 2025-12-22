```table-of-contents
title: 
style: nestedList # TOC style (nestedList|nestedOrderedList|inlineFirstLevel)
minLevel: 0 # Include headings from the specified level
maxLevel: 0 # Include headings up to the specified level
include: 
exclude: 
includeLinks: true # Make headings clickable
hideWhenEmpty: false # Hide TOC if no headings are found
debugInConsole: false # Print debug info in Obsidian console
```

## General

- OpenGear (remote console in to routers/switches/etc)
- Need Checkpoint VPN

- Upload via USB into SCP server
- DHCP server to assign IP
- Copy firmware over to each device via OpenGear

- Cisco Switch to port 1 of OpenGear

- Ubi Lab (dummy) is for testing

---

## Automation Functionalities

- All functionalities must have pre-check first

### Precheck

- OpenGear can reach, TFTP server can reach etc.
- Device really can cannot
- Create an environment file
	- Change ports/number of devices etc
	- `{sessionid}.env`
	- `py ./main.py {sessionid}.env precheck`
	- `{sessionid} == {email}.{time}`

### Upgrade

### Staging

- After staging need cleanup residue

### Generate Report

---

### Hardening (MVP @ 15 Jan)

- Precheck first then hardening
- Manually perform compliance checks first
	- Learn what Tenable is checking
	- See how compliance is verified
- Usually
	- `show run`
	- `show version`
	- `show ip ssh`
- Automation logic
	- Extract xml
		- Condition
		- Compliant ()
		- Remediate (`host(config)#aaa new-model`)
	- Commands as array
	- Send commands
	- Check show run
	- Remediate

---

## References

### TSG Staging Portal OneNote

https://nttlimited.sharepoint.com/:o:/r/teams/TSSigmaSquad/_layouts/15/Doc.aspx?sourcedoc=%7B3c7b71a7-e9c7-4c37-bdbf-a9ba882386c3%7D&action=edit&wd=target(Network%20STaging%20Portal%20-%20FAP.one%7C40c55be1-d5fa-43ed-baec-7a84ac27d047%2FStaging%20Portal%20-%20FAP%20%5C%7C%20Software%20Architecture%7C09df0aab-ae48-448e-8021-994415b21722%2F)&wdOrigin=TokenRefreshDialog&CID=1e13dc59-d035-4538-9c28-302c953ce9a2

### FAP Portal

https://github.com/NTT-AP-SG/fap-portal/

### Network Automation

https://github.com/NTT-AP-SG/network_automation
