# eJPT Notes

## Passive info gathering

Tools: 

- DNS recon: Netcraft (what that site is running?), DNSRecon, dns dumpster, Fierce
- zone transfer: dnsenum, dig
- WAF detection: wafwoof <Target Domain/url>
- subdomain recon: sublist3r
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
