- we discover a network configuration management tool called [rConfig](https://www.rconfig.com). This application is used by network & system administrators to automate the process of configuring network appliances.
- we can see the rConfig version number (`3.9.6`). We should use this information to start looking for any `CVEs`, `publicly available exploits`, and `proof of concepts` (`PoCs`).
- exploit modules in their [repos on github](https://github.com/rapid7/metasploit-framework/tree/master/modules/exploits).
- if the exploit is not visible we add that using this command
```shell
locate exploits
```
that is `/usr/share/metasploit-framework/modules/exploits`

```shell-session
meterpreter > shell
```
We can drop into a system shell (`shell`) to gain access to the target system as if we were logged in and open a terminal.

#### Spawning a TTY Shell with Python

`non-tty shell`. These shells have limited functionality and can often prevent our use of essential commands like `su` (`switch user`) and `sudo` (`super user do`), which we will likely need if we seek to escalate privileges.

```shell-session
python -c 'import pty; pty.spawn("/bin/sh")' 
```
then we can use normal shell