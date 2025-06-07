- A service is an application running on a computer that performs some useful function for other users or computers
- What we're interested in are services that have either been misconfigured or have a vulnerability
- port numbers range from 1 to 65,535, with the range of well-known ports 1 to 1,023 being
- Port 0 is a reserved port in TCP/IP networking and is not used in TCP or UDP messages. it is called as wild card port

### Nmap

```shell
nmap -sV -sC -p- 10.129.42.253
```

#### nmap scripts

```shell-session
locate scripts/citrix
```
The syntax for running an Nmap script is `nmap --script <script name> -p<port> <host>`.


#### Banner Grabbing

`nmap -sV --script=banner <target>`

```shell-session
nc -nv 10.129.42.253 21
```

#### FTP
- after doing nmap scan we got to know anonymous login is allowed

```bash
ftp -p 10.129.42.153

anonymous
```

#### SMB

- SMB (Server Message Block) is a prevalent protocol on Windows machines that provides many vectors for vertical and lateral movement.
- smb versions are vulnerable to RCE exploit like eternalblue
- nmap script `nmap --script smb-os-discovery.nse -p445 10.10.10.40`
#### Shares
SMB allows users and administrators to share folders and make them accessible remotely by other users.

A tool that can enumerate and interact with SMB shares is [smbclient](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html).

-N suppressing the password prompt
-L specifies that we want to retrieve a list of available shares on the remote host

```shell
smbclient -N -L \\\\10.129.42.253

smbclient -N -L \\\\10.129.42.253\\users  #non default user

smbclient -U bob \\\\10.129.42.253\\users
```
#### SNMP
- SNMP Community strings provide information and statistics about a router or device, helping us gain access to it

```shell-session
snmpwalk -v 2c -c public 10.129.42.253 1.3.6.1.2.1.1.5.0

snmpwalk -v 2c -c private 10.129.42.253
```

A tool such as [onesixtyone](https://github.com/trailofbits/onesixtyone) can be used to brute force the community string names using a dictionary file of common community strings such as the `dict.txt` file included in the GitHub repo for the tool.

```shell-session
onesixtyone -c dict.txt 10.129.42.254
```