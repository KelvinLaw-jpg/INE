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

### Auxiliary Modules
- Aux modultes are used for scanning, discovery, and fuzzing
- The goal is to enumerate info from targets
- Why do it again with msf modules? Reason is to learn more about the internal network and mahchines. These modules really shines in the post exploitation phrases. When other systems within the network that's not open to the public network, nmap will not work.

### FTP Enum 
Tips for metasploit search: `search type:<type: auxiliary> name:<name: ftp>

msf modules 
- auxiliary/scanner/ftp/ftp_version //banner grabbing
- auxiliary/scanner/ftp/ftp_login //bf the ftp with list
- Tips for login module: `set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt` & `set PASS_FILE /usr/share/metasploit-framework/data/wordlists/common_passwords.txt`

**nmap scripts and their use**

| **Category**            | **Script**                      | **Description** |
|-------------------------|----------------------------------|-----------------|
| **Anonymous Access**    | `ftp-anon`                      | Attempts to log in to FTP with anonymous credentials and list files. |
| **Brute Forcing**       | `ftp-brute`                     | Performs brute-force password auditing against FTP. |
| **Vulnerability Check** | `ftp-bounce`                    | Checks for FTP bounce attack vulnerability. |
|                         | `ftp-proftpd-backdoor`          | Checks for ProFTPD backdoor (CVE-2010-4221). |
|                         | `ftp-vsftpd-backdoor`           | Detects vsftpd 2.3.4 backdoor (smiley face vulnerability). |
|                         | `ftp-vuln-cve2010-4221`         | Checks for ProFTPD 1.3.2b RCE vulnerability. |
| **Information Gathering**| `ftp-syst`                     | Retrieves FTP server system information using the SYST command. |
|                         | `ftp-libopie`                   | Attempts to bypass One-Time Passwords (OTP) in OPIE. |
| **TFTP Specific**       | `tftp-enum`                     | Attempts to enumerate files on a TFTP server. |
|                         | `tftp-version`                  | Attempts to determine the TFTP server version. |


### SMB Enum

Tip: use setg(set global) when you first ran msfconsole to set the RHOSTS, so you dont need to keep setting it with in all the modules 

Another useful tool: enum4linux -a target.ine.local

**msf modules**
- smb_enumusers (get username)
- smb_enumshares
- smb_login (BF the username)

**nmap scripts and their use**
Useful command: `nmap -p139,445 --script "smb-*" --script-args=smbuser=administrator,smbpass=smbserver_771  demo.ine.local -Pn -n -vv  `

| **Category**            | **Script**                        | **Description** |
|-------------------------|----------------------------------|-----------------|
| **Enumeration & Info**  | `smb-enum-domains`               | List Windows domains the host belongs to. |
|                         | `smb-enum-groups`                | List Windows groups. |
|                         | `smb-enum-processes`             | List running processes via SMB. |
|                         | `smb-enum-services`              | List running services on the system. |
|                         | `smb-enum-sessions`              | Enumerate SMB sessions (who’s connected). |
|                         | `smb-enum-shares`                | List SMB shared folders. |
|                         | `smb-enum-users`                 | List local users on the SMB server. |
|                         | `smb-os-discovery`               | Identify the operating system via SMB. |
|                         | `smb-system-info`                | Gather basic SMB system information. |
|                         | `smb-server-stats`               | Show SMB server stats (traffic, open connections). |
| **File & Directory**    | `smb-ls`                         | List files and directories on shares. |
|                         | `smb-mbenum`                     | Enumerate systems via NetBIOS. |
|                         | `smb-print-text`                 | Send text file to a printer share. |
|                         | `smb-protocols`                  | List supported SMB protocols (1/2/3). |
|                         | `smb2-time`                      | Get system time via SMBv2. |
|                         | `smb2-capabilities`              | Show SMBv2 server capabilities. |
|                         | `smb2-security-mode`             | Display SMBv2 signing/encryption settings. |
|                         | `smb-security-mode`              | Show SMBv1 signing/encryption requirements. |
| **Authentication**      | `smb-brute`                      | Brute-force SMB login credentials. |
|                         | `smb-psexec`                     | Remote command execution via SMB (PsExec-like). |
| **Vulnerability Check** | `smb-double-pulsar-backdoor`     | Detect DoublePulsar backdoor. |
|                         | `smb-vuln-conficker`             | Check for Conficker worm vulnerability. |
|                         | `smb-vuln-cve-2017-7494`         | Check Samba RCE vulnerability (2017-7494). |
|                         | `smb-vuln-cve2009-3103`          | Check privilege escalation via SMB. |
|                         | `smb-vuln-ms06-025`              | Detect vuln in Routing and Remote Access. |
|                         | `smb-vuln-ms07-029`              | Check RPC Interface Mapping vuln. |
|                         | `smb-vuln-ms08-067`              | Check for classic RCE vuln (used by EternalBlue). |
|                         | `smb-vuln-ms10-054`              | Check RCE vuln on XP/2003. |
|                         | `smb-vuln-ms10-061`              | Printer service vulnerability detection. |
|                         | `smb-vuln-ms17-010`              | Check for EternalBlue (MS17-010). |
|                         | `smb-vuln-regsvc-dos`            | DoS check for Windows Registry Service. |
|                         | `smb-vuln-webexec`               | WebEx RCE vulnerability check. |
| **Exploitation/Noise**  | `smb-webexec-exploit`            | Try exploiting WebExec RCE. |
|                         | `smb-flood`                      | DoS by flooding SMB with connections. |
|                         | `smb2-vuln-uptime`               | Checks uptime and vuln status via SMBv2. |



### web/http Enum with msf modules

Common modules
- http_version
- http_header
- robots_txt
- dir_scanner (for directory brute force)
- files_dir (for file brute force in a directory)
- http_login (an http login util
- apache_user_enum (user enum)

### MySQL Enum

Common modules
- mysql_version
- mysql_login (TIP: since a lot of mysql server has a root username for priv ac, we can just bf the password)
- mysql_enum (need username and password)

```
use auxiliary/scanner/mysql/mysql_file_enum
set USERNAME root
set PASSWORD twinkle
set RHOSTS demo.ine.local
set FILE_LIST /usr/share/metasploit-framework/data/wordlists/directory.txt
set VERBOSE true
run
```

```
use auxiliary/scanner/mysql/mysql_writable_dirs
set RHOSTS demo.ine.local
set USERNAME root
set PASSWORD twinkle
set DIR_LIST /usr/share/metasploit-framework/data/wordlists/directory.txt
run
```

### SSH Enum

Common modules
- ssh_login
- ssh_enumusers

### SMTP Enum

Great thing with SMTP is for enumerating username and password and test it out at ssh

- another neat little tool outside of msf is smtp-user-enum, see if it is installed, if not just install with apt

