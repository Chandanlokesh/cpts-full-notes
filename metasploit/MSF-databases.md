- `Msfconsole` has built-in support for the PostgreSQL database system.
- With it, we have direct, quick, and easy access to scan results with the added ability to import and export results in conjunction with third-party tools
- we can also config and save the payload

#### Setting up the Database

```shell-session
sudo service postgresql status
```
### start postgreSQL
```shell-session
sudo systemctl start postgresql
```

#### MSF - Initiate a Database

```shell-session
sudo msfdb init

sudo msfdb status
```

make sure to update or else we will get error

After the database has been initialized, we can start `msfconsole` and connect to the created database simultaneously.
```shell-session
sudo msfdb run
```
we already have the database configured and are not able to change the password to the MSF username, proceed with these commands:

```shell
CaraxesL@htb[/htb]$ msfdb reinit
CaraxesL@htb[/htb]$ cp /usr/share/metasploit-framework/config/database.yml ~/.msf4/
CaraxesL@htb[/htb]$ sudo service postgresql restart
CaraxesL@htb[/htb]$ msfconsole -q
```

if we need help with the db `msf > help database`

#### organising the workspace

- view the workspace `msf > workspace`
- creating the workspace `msf > workspace -a Target_1`
- select the workspace `msf > workspace Target_1`
- check the workspace we in that will be in `*`
- if we need any help we can use `workspace -h`

#### importing the scans
- we will import the nmap scan 
- `cat Target.nmap`
- `msf > db_import Target.xml`   xml files type is preferred for db_import
- then we can use this command to list stuffs
```shell-session

msf6 > hosts

Hosts
=====

address      mac  name  os_name  os_flavor  os_sp  purpose  info  comments
-------      ---  ----  -------  ---------  -----  -------  ----  --------
10.10.10.40             Unknown                    device         


msf6 > services

Services
========

host         port   proto  name          state  info
----         ----   -----  ----          -----  ----
10.10.10.40  135    tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  139    tcp    netbios-ssn   open   Microsoft Windows netbios-ssn
10.10.10.40  445    tcp    microsoft-ds  open   Microsoft Windows 7 - 10 microsoft-ds workgroup: WORKGROUP
10.10.10.40  49152  tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  49153  tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  49154  tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  49155  tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  49156  tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  49157  tcp    msrpc         open   Microsoft Windows RPC
```

#### using nmap inside MSFconsole

```shell-session
msf6 > db_nmap -sV -sS 10.10.10.8
msf6 > host
msf6 > services
```

#### data backup

```shell
#help
msf6 > db_export -h  

msf > db_export -f xml backup.xml
```

### Hosts and Services
The `hosts` command displays a database table automatically populated with the host addresses, hostnames, and other information we find about these during our scans and interactions
```shell-session
msf6 > hosts -h
msf6 > services -h
```

### Credentials
The `creds` command allows you to visualize the credentials gathered during your interactions with the target host. We can also add credentials manually, match existing credentials with port specifications, add descriptions, etc.

```shell-session
msf6 > creds -h
```

### Loot
The `loot` command works in conjunction with the command above to offer you an at-a-glance list of owned services and users. The loot, in this case, refers to hash dumps from different system types, namely hashes, passwd, shadow, and more

```shell-session
msf6 > loot -h
```
