```shell-session
sudo nmap 10.129.2.28 -p- -sV --stats-every=5s
```
We can also increase the `verbosity level` (`-v` / `-vv`), which will show us the open ports directly when `Nmap` detects them.

```shell-session
sudo nmap 10.129.2.28 -p- -sV -v 
```
If we `manually` connect to the SMTP server using `nc`, grab the banner, and intercept the network traffic using `tcpdump`, we can see what `Nmap` did not show us.

#### Tcpdump

```shell-session
sudo tcpdump -i eth0 host 10.10.14.2 and 10.129.2.28
```
#### Nc
```shell-session
nc -nv 10.129.2.28 25
```
After that, the target SMTP server sends us a TCP packet with the `PSH` and `ACK` flags, where `PSH` states that the target server is sending data to us and with `ACK` simultaneously informs us that all required data has been sent.