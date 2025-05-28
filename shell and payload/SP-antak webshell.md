[ipsec](https://ippsec.rocks/?#) the god of learning

- aspx ; `Active Server Page Extended` (`ASPX`) is a file type/extension written for [Microsoft's ASP.NET Framework](https://docs.microsoft.com/en-us/aspnet/overview).
- on a web server running the ASP.NET framework, web form pages can be generated for users to input data. On the server side, the information will be converted into HTML.
- Nishang is an Offensive PowerShell toolset that can provide options for any portion of your pentest
- Antak is a web shell built in ASP.Net included within the [Nishang project](https://github.com/samratashok/nishang).
- antak uses powershell to communicate with windows server

#### working with antak
- file will be in `/usr/share/nishang/Antak-WebShell`
- exe each command as a new process and can be executed in memory
- need vhost

```shell-session
 cp /usr/share/nishang/Antak-WebShell/antak.aspx /home/administrator/Upload.aspx
```
![[shell and payload/image 2.png]]upload the file 
move to the uploaded directory then we need to execute that payload 


![[shell and payload/image 3.png]]![[shell and payload/image 4.png]]
