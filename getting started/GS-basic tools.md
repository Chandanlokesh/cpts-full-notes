### ssh
- [Secure Shell (SSH)](https://en.wikipedia.org/wiki/SSH_\(Secure_Shell\)) is a network protocol that runs on port `22` by default and provides users such as system administrators a secure way to access a computer remotely
- SSH uses a client-server model, connecting a user running an SSH client application such as `OpenSSH` to an SSH server.
```shell-session
ssh Bob@10.10.10.10

Bob@remotehost's password: *********
```
### netcat
[Netcat](https://linux.die.net/man/1/nc), `ncat`, or `nc`, is an excellent network utility for interacting with TCP/UDP ports. It can be used for many things during a pentest. Its primary usage is for connecting to shells,

`netcat` can be used to connect to any listening port and interact with the service running on that port

```bash
netcat 10.10.10.10. 22

#we are listning 
# this is called banner grabbing
```

Another similar network utility is [socat](https://linux.die.net/man/1/socat), which has a few features that `netcat` does not support, like forwarding ports and connecting to serial devices. `Socat` can also be used to [upgrade a shell to a fully interactive TTY](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/#method-2-using-socat). We will see a few examples of this in a later section. `Socat` is a very handy utility that should be a part of every penetration tester's toolkit. A [standalone binary](https://github.com/andrew-d/static-binaries) of `Socat` can be transferred to a system after obtaining remote code execution to get a more stable reverse shell connection.

### tmux

```shell-session
sudo apt install tmux -y
```
[[Tmux]]

### vim
