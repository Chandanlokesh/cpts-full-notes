#### Scan Network Range

lists all the ip in the network and disable the port scanning
```bash
sudo namp 10.129.20.0/24 -sn -oA output | grep for | cut -d" " -f5
```
for list of ip in the `.lst` file we `iL ip_list_file.lst`

- we can also list all the ip to scan and we can use `10.10.10.0-24` 
- `-PE` ping scan to the target
- `-Pn` no ping send TCP-SYN packet 
- `--reason` tells why i it given this output
- `--disable-arp-ping` by default nmap use arp to resolve the network
- `--packet-trace` shows all the packet sent and received 

## TTL

| Operating System         | Default TTL |
| ------------------------ | ----------- |
| **Linux** (Ubuntu, etc.) | 64          |
| **macOS**                | 64          |
| **Android**              | 64          |
| **Windows 10/11/Server** | 128         |
| **Windows XP/7/8**       | 128         |
| **Cisco Devices**        | 255         |
| **Solaris**              | 255         |
| **AIX**                  | 64          |
| **FreeBSD/OpenBSD**      | 64          |
| **Juniper Devices**      | 64 or 255   |
| **iOS (iPhone/iPad)**    | 64          |


| `state`            | `description`                                                                                                                                 |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
| open               | connection is established                                                                                                                     |
| closed             | getting RST flag                                                                                                                              |
| filtered           | no response is coming back                                                                                                                    |
| unfiltered         | the port is accessible but cant tell open or closed <br>and only getting TCP-ACK                                                              |
| open \| filtered   | firewall or packet filter is present                                                                                                          |
| closed \| filtered | occurs in the **IP ID idle** scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall. |
- `--top-ports=10` 
- `-n` disable the dns resolution

## connect scan
 - `-sT` full tcp handshake 
 - sends  SYN packet and responds with SYN-ACK then its open 
 - if the response is RST then closed
 - accurate but not stealthy 

`-sU` for udp scan, we only get response if the port is open
`-F` scan top 100 ports

### filtered ports
- reason 1 : drops the packet for no reason
- reason 2 : firewall will actively rejects the packet
- we and use `--max-retries 2` so our scan can be faster default is 10 

```
If you want to try bypassing:

You can use other techniques, like:

-f → Fragment packets
    
--source-port 53 → Spoof source port to look like DNS
    
--data-length → Add junk data to change packet signature
    
--ttl → Play with TTL to avoid detection
```

`-sV` scan the versions of the services

### outputs
- `-oN` normal format .`nmap`
- `-oG` Grepable output `.gnmap`
- `-oX` XML format `.xml`
- `-oA` all 3 format 
- style sheet
```bash
xsltproc target.xml -o target.html 
```

`-p-` all ports
`--stats-every=5s`  gives us the status every 5 seconds
`-v /-vv ` verbosity level

- to **grab the banner** (if we could't find the version the we use banner grabbing .text returned by service when a connection is made) 
	- using `sudo nmap 10.129.2.28 -p- -sV -Pn -n --disable-arp-ping --packet-trace`
	- using netcat `nc -nv 10.129.3.34 25
	- `sudo tcpdump -i eth0 host 10.10.14.2 and 10.129.2.28`

## Nmap Scripting Engine

- can create scripts in lua
- 14 categories of scripts
Categories and Their Descriptions

| `categories`            | `Descriptions`                                                                           |
| :---------------------- | :--------------------------------------------------------------------------------------- |
| auth                    | check authentication                                                                     |
| broadcast               | uses broadcast method to discover the host and services                                  |
| brute                   | brute-forcing attack against a service                                                   |
| default                 | `-sC`                                                                                    |
| discovery               | helps to evaluating which services are accessible on the network                         |
| dos (Denial of service) | check for potential of DoS attack                                                        |
| exploit                 | attempt to breach target by exploiting flaws which can gain access                       |
| external                | can use external DB or API to enhance the result                                         |
| fuzzer                  | sends random data to the target to detect vulnerabilities caused by unusual input        |
| intrusive               | use scripts that could negatively affect the target system . it is aggressive actions    |
| malware                 | check for any signs or signatures of malware infectons                                   |
| safe                    | used to do routine scans                                                                 |
| version                 | enhance `-sV` by giving detailed fingerprint of a service                                |
| vuln                    | vulnerabilities scripts aimed to identifying specific known vulnerabilities in a service |

```shell-session
sudo nmap <target> --script <script-name>,<script-name>,...
```

- `--traceroute` to see the network path (hops)
- `-A` aggressive scan
- [more docs](https://nmap.org/nsedoc/index.html)

## performance

- `-T <0-5>`  how fast need to scan
- `min-parallelism <num>` send multiple probes at once
- `--max-rtt-timeout <time>`  timeouts
- `--min-rate <num>` how many packets per seconds 
- `--max-retries <num>` how many number of retries need

#### RTT round trip time 

```bash
sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms
```

#### packet Rate

```bash
sudo nmap 10.129.2.0/24 -F --min-rate 300 -oN tnet.minrate300
```
use when internal pt, large network, good bandwidth, ctf

#### Time templets

|Template|Description|
|---|---|
|`-T0`|Paranoid (slowest, for stealth)|
|`-T1`|Sneaky|
|`-T2`|Polite (avoids overloading network)|
|`-T3`|Normal (default)|
|`-T4`|Aggressive (fast, may trigger alarms)|
|`-T5`|Insane (very fast, might miss data)|
[performance docs](https://nmap.org/book/man-performance.html)

## firewalls

`-sA` TCP ACK
`-sS` TCP SYN 
- make sure to check VPS (virtual private servers) having different address that may be attacked
- `-D RND:5` will generate random decoy IP's
- `-S `  specify the source IP
- `-O`  OS detection

```shell
sudo nmap 10.129.2.28 -n -Pn -p 445 -O -S 10.129.2.200 -e tun0
```

#### dns proxy 
- we can use dns queries to extract info because most of the firewalls wont filter this packets on UDP, TCP port 53
```bash
nmap -sS --source-port 53 <target>

namp -sS --dns-servers <dns1>,<dns2> <target ip>
namp --dns-servers 8.8.8.8,1.1.1.1 scanme.namp.org
```

namp is asking 8.8.8.8 or 1.1.1.1 for the ip of scanme.namp.org

#### Connect To The Filtered Port

```shell
sudo ncat -nv --source-port 53 10.129.2.28 50000
```
