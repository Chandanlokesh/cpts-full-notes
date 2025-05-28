- [Metasploit](https://www.metasploit.com) is an automated attack framework developed by `Rapid7` that streamlines the process of exploiting vulnerabilities through the use of pre-built modules that contain easy-to-use options to exploit vulnerabilities and deliver payloads to gain a shell on a vulnerable system.
- [metasploit pro version](https://www.rapid7.com/products/metasploit/download/editions/)

### starting msf
```bash
sudo msfconsole
```
### searching in metasploit
```bash
search <service>
```

`56 exploit/windows/smb/psexec`

<span style="color:rgb(0, 176, 80)">56</span> -> number assigned for the search we can use that number to work withe payload
<span style="color:rgb(0, 176, 80)">exploit</span> -> its an exploit category
<span style="color:rgb(0, 176, 80)">windows</span> -> its for windows
<span style="color:rgb(0, 176, 80)">smb</span> -> service
<span style="color:rgb(0, 176, 80)">psexec </span>-> 56 exploit/windows/smb/psexec

### selecting the exploit

```bash
use <number>
```
### Examining an Exploit's Options
```bash
options

#example

set RHOST <IP>
set SHARE ADMIN$
...

```
### Exploit 

```bash
exploit
```
