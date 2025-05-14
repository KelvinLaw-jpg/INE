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

**FTP Enum with msf modules**
Tips for metasploit search: `search type:<type: auxiliary> name:<name: ftp>

Common modules 
- auxiliary/scanner/ftp/ftp_version //banner grabbing
- auxiliary/scanner/ftp/ftp_login //bf the ftp with list
- Tips for login module: `set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt` & `set PASS_FILE /usr/share/metasploit-framework/data/wordlists/common_passwords.txt`

**SMB Enum with msf modules**

Tip: use setg(set global) when you first ran msfconsole to set the RHOSTS, so you dont need to keep setting it with in all the modules 

Common modules
- smb_enumusers (get username)
- smb_enumshares
- smb_login (BF the username)

**web/http Enum with msf modules**

Common modules
- http_version
- http_header
- robots_txt
- dir_scanner (for directory brute force)
- files_dir (for file brute force in a directory)
- http_login (an http login util
- apache_user_enum (user enum)

**MySQL Enum**

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

**SSH Enum**

Common modules
- ssh_login
- ssh_enumusers

**SMTP Enum**

Great thing with SMTP is for enumerating username and password and test it out at ssh

