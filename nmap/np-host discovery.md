- most effective host discovery method is using ICMP echo requests

#### scan network range
```shell
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5

10.129.2.4
10.129.2.10
10.129.2.11
10.129.2.18
```
#### scan IP list 

```shell
sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5

10.129.2.18
10.129.2.19
10.129.2.20
```

#### scanning multiple IPs
```shell
sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20| grep for | cut -d" " -f5


sudo nmap -sn -oA tnet 10.129.2.18-20| grep for | cut -d" " -f5

```
#### single IP scan
```shell
sudo nmap 10.129.2.18 -sn -oA host 
```
`--packet-trace`  this is used to see all the sent and received packets 

`-PE` is used to ping 
`--reason` tell why it got this result
`--disable-arp-ping` to disable ARP ping

[more info](https://nmap.org/book/host-discovery-strategies.html) great resource
