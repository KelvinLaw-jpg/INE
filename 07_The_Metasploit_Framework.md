# Metasploit Framework


### MSFDB
- sudo systemctl enable postgresql //enacle postgresql on startup
- sudo systemctl start postgresql //start the service
- sudo systemctl status postgresql //check postgresql status
- sudo msfdb //start msfdb
- sudo msfdb init //initialise db

### MSFConsole
- search -h to learn about search options
- Importing nmap scans: `db-import <path of nmap xml output>`
- To perfoem a scan on internal network through the initial victim, run `run autoroute -s <IP>` in meterpreter


### MySQL with MSF
- useful modules: version, schemadum, login, file_enum
- modules under /admin/ will need cred to use: mysql_enum, mysql_sql
- TIP: can use `loot`, and `creds` to see credentials etc

### Nessus with MSF
- Do the scan with nessus and export it in XML `export nessus`
- Load it in with `db_import <location of nessus scan>`
- use the `vulns` command to see possible vulnerabilies

### WMAP (web app) vuln scan with MSF
- 
