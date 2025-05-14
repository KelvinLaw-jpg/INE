## Host Discovery

- Every machine connected to a network will have an IP address
- Goal is to determine all the machine and devices' IP and their open ports, OS, what services, service version
- Discover filtering and security measures: firewalls, IPS, IDS


### Nmap
- can use various protocols such as ICMP, ARP, TCP/UDP probes

- **When and why use each**
- ICMP is quick, but it gets block
- TCP SYN is stealthier, but some host might not response and it can be affect by firewalls

**Ping Sweep**
- fping is an improvement (still use ICMP) of ping eg: fping -a -g <10.10.23.0/24> 2> /dev/null
- Use nmap -Pn to get more accurate result

**Host Discovery (-sn)**
- Ping Sweeps: sending ICMP Echo Requests (sometimes doesn't work, and windows firewall by default block)
- ARP Scanning: Identify host on local network only, it will send an ACK package and if a RST packet if replied, then host is up
- PS and PA options by default use port 80 unless specified eg: PS1-1000
- TCP SYN Ping (Half-Open Scan): Stealth Scan
- UDP Ping: Send UDP packets instead of ICMP or TCP
- TCP ACK Ping: Send ACK packets to see if host is alive, this technique expect no response, so if there is a tcp rst response, it means host is alive
- SYN-ACK Ping: Sending TCP SYN-ACK packets to a specific port to host, if TCP RST is received, it means host is alive

Demo for Host Discovery (scans to use)
- SYN Ping(PS by default): nmap -sn <IP> //can only send ARP 
- namp -sn <IP> --send-ip //both ARP and ICMP send
- Usually you can put all the ip and ip reach in a txt file, note each ip and range has to be in their own line
- nmap -sn -iL <IP_file.txt>
- (TCP SYN ping): nmap -sn -PS(TCP SYN Ping) <IP>
- nmap -sn -PS80,3389,445 <IP> //send syn ping to specific ports
- ACK Ping(PA): nmap -sn -PA <IP>
- ICMP ping scan: nmap -sn -PE <IP> --send-ip //if at local, add --send-ip option
- HackerSploit option: nmap -sn -v -T4 <IP> //Find initial set of IP
- nmap -sn -PS21,22,25,80,445,3389,8080 -PU137,138 -T4 <IP> //Tailor the SYN ping ports to specific host, PU is for UDP 137,138 netbios

**Port Scanning (-Pn to skip host discovery)**
- SYN Scan vs TCP Scan by default (root vs non root): SYN does not complete handshake, TCP complete the whole handshake
- Non priv user need to specify in command: nmap -Ps -sS -F <IP>
- TCP scan are really loud and prone to detection, but it's very accurate since it will complete the connection
- UDP can tell if the scan is being filtered

Demo
- nmap <IP> //Default nmap port scan, if run with root account, it performs a syn port scan, by default nmap send most common 1000 ports, if running with non priv user, nmap use tcp connect port scan
- nmap -Pn <IP> //Ignore firewall blocking and scan them ports
- nmap -Pn -F <IP> //Fast scan: scan the most common 100 ports
- nmap -Pn -p <custom port(s)> <IP> //-p- specify all the TCP port range
- nmap -Pn -sU -p53,137,138,139 <IP>

**Service and OS version detection**
- Basic: nmap -Pn -sV -O -T4 <IP>
- Advance: nmap -Pn -sS -sV --version-intensity <8> -O --osscan-guess -p- <IP>
- Version-intensity varies from 0-9 and the higher the number the more accurate nmap will try to ascertain, but the longer it will be 

**Nmap Scripting Engine**
- location of all scripts: /usr/share/nmap/scripts/
- Scripts mainly(not exclusive) can be split in the following categories: Discovery, exploitation, brute forcing, and safe
- eg: the auth catergory scripts is used to automate authentication and credential automation related
- eg: broadcast scripts are used to facilitate broadcases or multicast communication for host discovery
- The default catergory is used to engage with the target in a safe and non dangerous way such as Exploitation
- To learn more about the specific script you can do: nmap --script-help=<script name, eg: mongodb-databases>
- To run specific script we can: nmap -sS -sV --script=mongodb-info -p- -T4 <IP>
- To combine multiple scripts or a whole category we can: nmap -sS -sV --script=ftp-* -p21 -T4 <IP>
- To specify a script cat we can: nmap -sS -sV --script=<category> -T4 <IP>

**TIP: To combine OS, service version detection and script scan we can just use: nmap -sS -A -p- -T4 <IP>

Demo
- nmap -sS -sV -sC -p- -T4 <IP> //Default script scan, rmb to ascertain that it is a port scan if not -sC wont work correctly

## Firewall Detection & IDS Evasion

