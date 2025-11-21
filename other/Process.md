Client > CM > Pre-Sales >

## Automation

### General

- Screen scraping
- Some technologies already exist to do mass config (WLC to push firmware via templates)

### Staging (Upgrade Firmware)

- Pre-checks
	- Check console cable
	- Check UTP cable
- Check version
	- Ok = skip
	- Outdated
		- Config IP
		- Ping file server
		- TFTP firmware to device
		- Check MD5 (firmware integrity)
		- Install (will reboot)
		- Check version
		- Clean up IP
