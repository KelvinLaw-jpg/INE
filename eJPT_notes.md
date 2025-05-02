# eJPT Notes

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
