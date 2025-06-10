- file transfer protocol (L7)
- port 20,21 and use TCP
- `active FTP` client establishes the connection 
- when server is willing to connect if client has firewall then we cant establish the connection so `passive FTP` will helps the server will announce what port to connect with the client
- FTP [commands](https://web.archive.org/web/20230326204635/https://www.smartfile.com/blog/the-ultimate-ftp-commands-list/)
- FTP [status code](https://en.wikipedia.org/wiki/List_of_FTP_server_return_codes)
- FTP is a `clear-text` protocol and  allow `anonymous FTP` login

## TFTP
- trivial file transfer protocol simpler then ftp and no authentication needed and use UTP for comm

| `**Commands**` | `**Description**`                                                                                                                      |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `connect`      | Sets the remote host, and optionally the port, for file transfers.                                                                     |
| `get`          | Transfers a file or set of files from the remote host to the local host.                                                               |
| `put`          | Transfers a file or set of files from the local host onto the remote host.                                                             |
| `quit`         | Exits tftp.                                                                                                                            |
| `status`       | Shows the current status of tftp, including the current transfer mode (ascii or binary), connection status, time-out value, and so on. |
| `verbose`      | Turns verbose mode, which displays additional information during file transfer, on or off.                                             |

### Default config
- common FTP in linux [vsFTPd](https://security.appspot.com/vsftpd.html) [man page](http://vsftpd.beasts.org/vsftpd_conf.html) 
- `/etc/vsftpd.conf`
- `sudo apt install vsftpd`
- `/etc/ftpusers`
- `anonymous` access

```bash
ftp <IP>

# enable this options to show more options
>debug 
>trace


>ls
>status
>ls -R
>get file-name
>exit
```

#### download all the files 

```bash
wget -m --no-passive ftp://anonymous:anonymous@10.129.14.136
```

#### uploading a file to ftp

```bash
ftp> put testupload.txt
```

### footprinting the service

```shell-session
find / -type f -name ftp* 2>/dev/null | grep scripts
```

nmap
```shell-session
sudo nmap -sV -p21 -sC -A 10.129.14.136
```
[ftp-anon](https://nmap.org/nsedoc/scripts/ftp-anon.html) NSE script checks whether the FTP server allows anonymous login, ftp-syst runs stat command

#### server interaction
- if FTP server runs with TLS/SSL encryption
```bash
openssl s_clint -connect 10.129.14.136:21 -starttls ftp
```

- just grab a banner or to see we can connect
```bash
nc -nv 10.129.14.136 21

# or

telnet 10.129.14.136 21
```

	we can try `--script-trace` with `-sC`