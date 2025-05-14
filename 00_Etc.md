## Passive info gathering

Tools: 

- DNS recon: Netcraft (what that site is running?), DNSRecon, dns dumpster, Fierce
- zone transfer: dnsenum, dig
- WAF detection: wafwoof <Target Domain/url>
- subdomain recon: sublist3r, dirb
- google dorking: google hacking database manage by offsec
- overall: Harvester
- credentials: haveibeenpwned

## Network scan
### Discovery
- nmap -sn (no port, aka ping sweep, or ping scan)
- netdiscover

### Port Scan
- nmap -sC (script scan to get more info)
- nmap -Pn (assume host is up)
- nmap -F (fast scan)
- nmap -sV (version scan of the app on port)
- nmap -O (OS scan, try identify OS)
- nmap -oN (output to normal format)
- nmap -oX (output to XML format, useful to import metasploit)

### Notes from first ctf exercise

- use nmap -sC and -sV to grab banner
- dirb is the fastest and least input needed
- rmb to use dirb -X <file extension> to bf hidden files such as `dirb http://target.ine.local -w /usr/share/dirb/wordlists/big.txt -X .bak,.tar.gz,.zip,.sql,.bak.zip`
- use httrack to download an offline version of the website, sometimes it shows more hidden info`httrack http://target.ine.local -O target.html`


Vulnerability Assessment
---

Tools: metasploit, Nessus

Common vulnerabilities of windows:
- Microsoft IIS port 80/443 - proprietary web server software
- WebDAV port 80/443 - http extension
- SMB/CIFS port 445
- RDP port 3389
- WinRM port 5986/443


Auditing Fundamentals
---

**Why Security Audits?** : mostly because compliance and regulations for companies

**What are the differences of audits, accessments and pentests?**: Security audits are evaluation and verification of the sercurity measures and controls in place within an organization to ensure 
they are effective, appropiate and comply with relevant standards, policy and regulations. It involves reviewing organization's info systems, networks, applications and operational procedures to 
indentify weaknesses, vulnerabilities, and area of improvements.



**Security Audit life cycle/process:**

1. Determine scope and objectives, relavent documents such as policies etc, establish auditing team
2. try to identify gaps of the company and the compliants, collect technical info
3. risk accessment, identify all assets, potential threats of assets, risk and risk levels
4. Audit Execution: perform technical testing, conduct technical assessment such as pentesting, vul scans, configuration reviews. Verify compliance and evaluate controls
5. Analysis findings, compare it to industry standards and prioritize issues such as CVSS scores
6. reporting and presenting recommendations to stakeholders
7. Remediation and continous updates

**Types of Security Audits**
Internal, External and Compliance audits, Technical audits, Network audits, and Application audits

Tools: lynis from cisofy

