 - ls -lh /usr/share/wordlists/ 
	 - we can see all the wordlist 

### linux
files are stored in encrypted . this files are called shadow file located in `/etc/shadow` which will be in the form of `hashes`
this file is only read by root

```shell-session
cat /etc/shadow

...SNIP...
htb-student:$y$j9T$3QSBB6CbHEu...SNIP...f8Ms:18955:0:99999:7:::
```


| `htb-student:` | `$y$j$3QSBB6CbHEu...SNIP...f8Ms:` | `18955:`                | `0:`         | `99999:`     | `7:`                | `:`                    | `:`                  | `:`                |
| -------------- | --------------------------------- | ----------------------- | ------------ | ------------ | ------------------- | ---------------------- | -------------------- | ------------------ |
| `<username>`:  | `<encrypted password>`:           | `<day of last change>`: | `<min age>`: | `<max age>`: | `<warning period>`: | `<inactivity period>`: | `<expiration date>`: | `<reserved field>` |


| `$ <id>` | `$ <salt>` | `$ <hashed>`                  |
| -------- | ---------- | ----------------------------- |
| `$ y`    | `$ j9T`    | `$ 3QSBB6CbHEu...SNIP...f8Ms` |

|**ID**|**Cryptographic Hash Algorithm**|
|---|---|
|`$1$`|[MD5](https://en.wikipedia.org/wiki/MD5)|
|`$2a$`|[Blowfish](https://en.wikipedia.org/wiki/Blowfish_\(cipher\))|
|`$5$`|[SHA-256](https://en.wikipedia.org/wiki/SHA-2)|
|`$6$`|[SHA-512](https://en.wikipedia.org/wiki/SHA-2)|
|`$sha1$`|[SHA1crypt](https://en.wikipedia.org/wiki/SHA-1)|
|`$y$`|[Yescrypt](https://github.com/openwall/yescrypt)|
|`$gy$`|[Gost-yescrypt](https://www.openwall.com/lists/yescrypt/2019/06/30/1)|
|`$7$`|[Scrypt](https://en.wikipedia.org/wiki/Scrypt)|

### user level passwords
`/etc/passwd`
`/etc/group`

```shell-session
CaraxesL@htb[/htb]$ cat /etc/passwd

...SNIP...
htb-student:x:1000:1000:,,,:/home/htb-student:/bin/bash
```


| `htb-student:` | `x:`          | `1000:`  | `1000:`  | `,,,:`       | `/home/htb-student:` | `/bin/bash`                       |
| -------------- | ------------- | -------- | -------- | ------------ | -------------------- | --------------------------------- |
| `<username>:`  | `<password>:` | `<uid>:` | `<gid>:` | `<comment>:` | `<home directory>:`  | `<cmd executed after logging in>` |
[Linux User Auth](https://tldp.org/HOWTO/pdf/User-Authentication-HOWTO.pdf)

### windows authentication process
- Windows system, such as Kerberos authentication. The [Local Security Authority](https://learn.microsoft.com/en-us/windows-server/security/credentials-protection-and-management/configuring-additional-lsa-protection) (`LSA`) is a protected subsystem that authenticates users and logs them into the local computer. In addition, the LSA maintains information about all aspects of local security on a computer. It also provides various services for translating between names and security IDs (`SIDs`).
- security subsystem keeps track of the security policies and accounts that reside on a computer system. In the case of a Domain Controller, these policies and accounts apply to the domain where the Domain Controller is located.
- These policies and accounts are stored in Active Directory
![[password attack/image.png]]


|Component|What it Does|
|---|---|
|**Winlogon**|The main **login manager**. Handles login, logout, lock screen, etc.|
|**LogonUI**|The **login screen** you see. It asks for your username/password.|
|**Credential Providers**|The "plugin" that collects your credentials (like username & password).|
|**LSASS**|The **Local Security Authority**—checks your login details to see if they’re correct.|
|**Authentication Packages**|DLLs that actually do the checking of your password (like `msv1_0.dll`).|
|**SAM / Active Directory**|The **database** where your user info is stored—SAM for local users, AD for domain users.|

#### LSASS

[Local Security Authority Subsystem Service](https://en.wikipedia.org/wiki/Local_Security_Authority_Subsystem_Service) (`LSASS`) is a collection of many modules and has access to all authentication processes that can be found in `%SystemRoot%\System32\Lsass.exe`.

|**Authentication Packages**|**Description**|
|---|---|
|`Lsasrv.dll`|The LSA Server service both enforces security policies and acts as the security package manager for the LSA. The LSA contains the Negotiate function, which selects either the NTLM or Kerberos protocol after determining which protocol is to be successful.|
|`Msv1_0.dll`|Authentication package for local machine logons that don't require custom authentication.|
|`Samsrv.dll`|The Security Accounts Manager (SAM) stores local security accounts, enforces locally stored policies, and supports APIs.|
|`Kerberos.dll`|Security package loaded by the LSA for Kerberos-based authentication on a machine.|
|`Netlogon.dll`|Network-based logon service.|
|`Ntdsa.dll`|This library is used to create new records and folders in the Windows registry.|

- **SAM (Security Account Manager)** stores hashed passwords for local user accounts on Windows, located at:  
    `%SystemRoot%\system32\config\SAM` and mounted at `HKLM\SAM`.
    
    - Requires **SYSTEM-level access** to view.
        
- Passwords are stored as **LM or NTLM hashes**, using **cryptographic protection** to prevent unauthorized access.
    
- In a **workgroup**, the local SAM database handles user authentication.  
    In a **domain**, credentials are verified by the **Domain Controller (DC)** using the **Active Directory database** (`ntds.dit`).
    
- **SYSKEY**, introduced in Windows NT 4.0, **encrypts** the SAM database to add extra protection against offline password cracking.

![[password attack/image 1.png]]

`Credential Locker`
```powershell-session
PS C:\Users\[Username]\AppData\Local\Microsoft\[Vault/Credentials]\
```
### NTDS
- network env where many win systems
- sends to DC 
- ntds are the database file stored in AD its like a central db
- 
