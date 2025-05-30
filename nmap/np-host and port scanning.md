### scanned results
| **State**          | **Description**                                                                                                                                                                                         |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `open`             | This indicates that the connection to the scanned port has been established. These connections can be **TCP connections**, **UDP datagrams** as well as **SCTP associations**.                          |
| `closed`           | When the port is shown as closed, the TCP protocol indicates that the packet we received back contains an `RST` flag. This scanning method can also be used to determine if our target is alive or not. |
| `filtered`         | Nmap cannot correctly identify whether the scanned port is open or closed because either no response is returned from the target for the port or we get an error code from the target.                  |
| `unfiltered`       | This state of a port only occurs during the **TCP-ACK** scan and means that the port is accessible, but it cannot be determined whether it is open or closed.                                           |
| `open\|filtered`   | If we do not get a response for a specific port, `Nmap` will set it to that state. This indicates that a firewall or packet filter may protect the port.                                                |
| `closed\|filtered` | This state only occurs in the **IP ID idle** scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall.                                           |
#### Scanning Top 10 TCP Ports

```shell-session
sudo nmap 10.129.2.28 --top-ports=10 
```

#### Nmap - Trace the Packets

```shell-session
sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping
```

#### Connect Scan

The Nmap [TCP Connect Scan](https://nmap.org/book/scan-methods-connect-scan.html) (`-sT`) uses the TCP three-way handshake to determine if a specific port on a target host is open or closed. The scan sends an `SYN` packet to the target port and waits for a response. It is considered open if the target port responds with an `SYN-ACK` packet and closed if it responds with an `RST` packet.

```shell-session
sudo nmap 10.129.2.28 -p 443 --packet-trace --disable-arp-ping -Pn -n --reason -sT 
```
#### Filtered Ports

In most cases, firewalls have certain rules set to handle specific connections. The packets can either be `dropped`, or `rejected`. When a packet gets dropped, `Nmap` receives no response from our target, and by default, the retry rate (`--max-retries`) is set to `10`. This means `Nmap` will resend the request to the target port to determine if the previous packet was accidentally mishandled or not.
As a response, we receive an `ICMP` reply with `type 3` and `error code 3`, which indicates that the desired port is unreachable.

### Discovering Open UDP Ports
Since `UDP` is a `stateless protocol` and does not require a three-way handshake like TCP. We do not receive any acknowledgment. Consequently, the timeout is much longer, making the whole `UDP scan` (`-sU`) much slower than the `TCP scan` (`-sS`).

```shell-session
sudo nmap 10.129.2.28 -F -sU
```
only if the port is open then only we will get response

#### Version Scan

```shell-session
sudo nmap 10.129.2.28 -Pn -n --disable-arp-ping --packet-trace -p 445 --reason  -sV
```
[port scanning techniques](https://nmap.org/book/man-port-scanning-techniques.html)


