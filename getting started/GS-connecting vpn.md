![[getting started/image.png]]- VPN works by routing our connecting device's internet connection through the target VPN's private server instead of our internet service provider (ISP). When connected to a VPN, data originates from the VPN server rather than our computer and will appear to originate from a public IP address other than our own

**client-based VPN and SSL VPN.**

SSL VPN uses the web browser as the VPN client. The connection is established between the browser and an SSL VPN gateway can be configured to only allow access to web-based applications such as email and intranet sites, or even the internal network but without the need for the end user to install or use any specialized software

Client-based VPN requires the use of client software to establish the VPN connection. Once connected, the user's host will work mostly as if it were connected directly to the company network and will be able to access any resources

### connecting to HTB VPN

```bash
sudo openvpn user.ovpn

>ifconfig

> netstat -rn  #show us the network accessible via the VPN
```
[Connection Troubleshooting](https://help.hackthebox.com/en/articles/5185536-t-connection-troubleshooting)
