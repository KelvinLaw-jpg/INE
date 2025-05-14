# Enumeration

### Port Scanning & Enum with Nmap & Metasploit

- Metasploit allows to integrate the result of Nmap scans into the ms database through msf console
- Export it as XML to import to metasploit (-oX)

msfconsole commands

- service postgresql start //start up postgresql, metasploit uses postgreSQL as db to store it's data
- msfconsole
- db_status //check db_status
- workspace -a Win2k12 //add workspace
- db_import <XML file to import eg:/root/windows_server_2012> (from prevous nmap scan)
- we can confirm the data is imported correctly by using hosts and services in msfconsole

- To initiate an nmap scan within msfconsole if you are lazy, you can: `db_nmap -Pn -sV -O <IP>`
- The above will import the result automatically
- To list vulnerabilities with the data just type `vulns` in msfconsole





