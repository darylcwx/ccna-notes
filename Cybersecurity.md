## 1. Vulnerability Assessment & Pen Testing

- Proactively prioritize and remediate security vulns. before attackers exploit
- Regulatory compliance, trust, and cybersecurity posture

- Types
	- Network, system/OS, application, configuration, business logic, authentication
- Approaches
	- Black, white, grey box
- Tools
	- OpenVAS, Nmap, Metasploit
	- Nexus, Burp Suite
- Methodology
	1. Recon
	2. Vuln. Analysis
	3. Exploitation
	4. Post Exploitation
	5. Reporting

## 2. PCI DSS

- Payment Card Industry Data Security Standard
- Baseline of technical and operational requirements to protect account data
- Applicable to any entity (merchants, service providers, processors, acquirers, issuers)
- Data
	- Cardholder data: Primary account number (PAN), name, expiry
	- Sensitive: Full track data (mag stripe), CVV, PIN
- Transaction Flow
	- Consumer > merchant > acquiring bank > association > issuing banks
- Legitimate CC
	- Luhn's algorithm
		1. PAN
		2. From first number, double every other number
		3. If number has 2 digits, sum
		4. Sum all
	- Divisible by 10

| 6 Control Objectives                                      | 12 requirements                                                                                                                                                                                                | Remarks                                                                             |
| --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| Build and Maintain a<br>Secure Network and<br>Systems<br> | 1. Install and Maintain Network Security Controls<br>2. Apply Secure Configurations to All System Components                                                                                                   | Use firewalls, no default passwords/settings                                        |
| Protect Account Data                                      | 3. Protect Stored Account Data<br>4. Protect Cardholder Data with Strong Cryptography During<br>Transmission Over Open, Public Networks                                                                        | encryption, truncation, masking, hashing                                            |
| Maintain a Vulnerability<br>Management Program            | 5. Protect All Systems and Networks from Malicious Software<br>6. Develop and Maintain Secure Systems and Software                                                                                             | anti-malware solutions, update software/security patches                            |
| Implement Strong Access<br>Control Measures               | 7. Restrict Access to System Components and Cardholder Data<br>by Business Need to Know<br>8. Identify Users and Authenticate Access to System<br>Components<br>9. Restrict Physical Access to Cardholder Data | limit access, least privilege, RBAC, ZTA, unique ID, auth, restrict physical access |
| Regularly Monitor and Test<br>Networks                    | 10. Log and Monitor All Access to System Components and<br>Cardholder Data.<br>11. Test Security of Systems and Networks Regularly.                                                                            | track and monitor activities, pen test frequently                                   |
| Maintain an Information<br>Security Policy                | 12. Support Information Security with Organizational Policies and<br>Programs.                                                                                                                                 | info sec policies                                                                   |

## 3. GRC

- Cybersecurity Maturity Assessment

## 4. IT vs OT

- Hardware or software that detects change through direct monitoring or controlling physical devices (Sensors)
- OT asset types
	- IED: sensors/motors
	- RTU: remote terminal unit
	- PLC: programmable logic controller
	- HMI: human machine interface (software on windows)
	- Supervisory Workstation: data and charts
	- Data Historians: long term data
	- Other Assets: IoT, smart sensors
- Purdue Model
	- IT
		- Enterprise zone
	- OT
		- IDMZ
		- Control zone
		- Area zone
		- Cell zone