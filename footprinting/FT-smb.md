- server message block
- uses TCP 137,138,139 and 445 exclusively
- provide arbitrary parts of its local file system as shares.
- samba uses Common Internet File System (`CIFS`) network protocol. [CIFS](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/934c2faa-54af-4526-ac74-6a24d126724e)
- The SMB server daemon (`smbd`) belonging to Samba provides the first two functionalities, while the NetBIOS (an api that provides blueprint of the application) message block daemon (`nmbd`) implements the last two functionalities.
- when a machine goes online, it needs a name, which is done through the so-called `name registration` procedure. Either each host reserves its hostname on the network, or the [NetBIOS Name Server](https://networkencyclopedia.com/netbios-name-server-nbns/) (`NBNS`) is used for this purpose. It also has been enhanced to [Windows Internet Name Service](https://networkencyclopedia.com/windows-internet-name-service-wins/) (`WINS`).
- smb [settings](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html)
```bash
cat /etc/samba/smb.conf |grep -v "#\|\;"
```
### restarting the samba and listing the connection to share
```bash
sudo systemctl restart smbd

smbclint -N -L //10.129.14.128

# seeing the shares there is a share called notes

smbcleint //10.129.14.128/notes

>help #list all the commands
```

- smb allows to execute local sys command using `!<cmd>`
- `get file-name`
`smbstatus` check these connection 
- The domain controllers keep track of users and passwords in their own `NTDS.dit` and `Security Authentication Module` (`SAM`) and authenticate each user when they log in for the first time and wish to access another machine's share.

### Footprinting the service

```bash
sudo nmap <IP> -sV -sC -p139,445
```

#### manually 

```shell
rpcclient -U "" 10.129.14.128

rpcclient>
```

|**Query**|**Description**|
|---|---|
|`srvinfo`|Server information.|
|`enumdomains`|Enumerate all domains that are deployed in the network.|
|`querydominfo`|Provides domain, server, and user information of deployed domains.|
|`netshareenumall`|Enumerates all available shares.|
|`netsharegetinfo <share>`|Provides information about a specific share.|
|`enumdomusers`|Enumerates all domain users.|
|`queryuser <RID>`|Provides information about a specific user.|
#### brute forcing user RIDs

```shell
for i in $(seq 500 1100);do rpcclient -N -U "" 10.129.14.128 -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo "";done
```

alternative to this would be a Python script from [Impacket](https://github.com/SecureAuthCorp/impacket) called [samrdump.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/samrdump.py).

```shell
samrdump.py 10.129.14.128
```

the [SMBMap](https://github.com/ShawnDEvans/smbmap) 
```shell-session
smbmap -H 10.129.14.128
```

and [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec) tools are also widely used and helpful for the enumeration of SMB services.

```shell-session
crackmapexec smb 10.129.14.128 --shares -u '' -p ''
```
worth mentioning is the so-called [enum4linux-ng](https://github.com/cddmp/enum4linux-ng) tool automate many queries and return large amount of info

##### Enum4Linux-ng - Installation
```bash
git clone https://github.com/cddmp/enum4linux-ng.git

cd enum4linux-ng
pip3 install -r requirements.txt

./enum4linux-ng.py <IP> -A
```