cat- Laudanum is a repository of ready-made files that can be used to inject onto a victim and receive back access via a reverse shell, run commands on the victim host right from the browser, and more
- The repo includes injectable files for many different web application languages to include `asp, aspx, jsp, php,` and more
- found in the `/usr/share/laudanum` directory.
- can copy them as-is and place them where you need them on the victim to run
- must edit the file first to insert your `attacking` host IP address to ensure you can access the web shell or receive a callback in the instance that you use a reverse shell.

`note : i am using vertual host 
`nvim /etc/hosts`
`<ip> status.inlanefreight.local `


stay in the vpn after that move a copy for modification
```shell-session
cp /usr/share/laudanum/aspx/shell.aspx /home/tester/demo.aspx
```

Add your IP address to the `allowedIps` variable on line `59`. Make any other changes you wish
![[shell and payload/image-1 1.png]]

We are taking advantage of the upload function at the bottom of the status page(`Green Arrow`) for this to work. Select your shell file and hit upload. If successful, it should print out the path to where the file was saved (Yellow Arrow). Use the upload function. Success prints out where the file went, navigate to it.

![[image-1 2.png]]

`status.inlanefreight.local//files/demo.aspx`.
`systeminfo`

![[shell and payload/image 1.png]]