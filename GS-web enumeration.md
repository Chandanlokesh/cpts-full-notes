### Gobuster

After discovering a web application, it is always worth checking to see if we can uncover any hidden files or directories on the webserver that are not intended for public access. We can use a tool such as [ffuf](https://github.com/ffuf/ffuf) or [GoBuster](https://github.com/OJ/gobuster) to perform this directory enumeration.

```shell-session
gobuster dir -u http://10.10.10.121/ -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

[http status code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)

#### DNS Subdomain Enumeration

There also may be essential resources hosted on subdomains, such as admin panels or applications with additional functionality that could be exploited.

#### Install SecLists

SecLists GitHub [repo](https://github.com/danielmiessler/SecLists)

```shell-session
git clone https://github.com/danielmiessler/SecLists

sudo apt install seclist -y
```

```shell-session
gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt
```
### Web Enumeration Tips

#### Banner Grabbing / Web Server Headers

```shell-session
curl -IL https://www.inlanefreight.com
```
handy tool is [EyeWitness](https://github.com/FortyNorthSecurity/EyeWitness), which can be used to take screenshots of target web applications, fingerprint them, and identify possible default credentials.

#### Whatweb

We can extract the version of web servers, supporting frameworks, and applications using the command-line tool `whatweb`

```shell-session
whatweb --no-errors 10.10.10.0/24
```

#### Certificates

SSL/TLS certificates are another potentially valuable source of information if HTTPS is in use

#### Robots.txt

It is common for websites to contain a `robots.txt` file, whose purpose is to instruct search engine web crawlers such as Googlebot which resources can and cannot be accessed for indexing. The `robots.txt` file can provide valuable information such as the location of private files and admin pages.

`http://10.10.10.121/private` in a browser reveals a HTB admin login page.

#### Source Code

It is also worth checking the source code for any web pages we come across. We can hit `[CTRL + U]` to bring up the source code window in a browser.
