- [ExploitDB](https://www.exploit-db.com) is a great choice when searching for a custom exploit. We can use tags to search through the different exploitation scenarios for each available script. One of these tags is [Metasploit Framework (MSF)](https://www.exploit-db.com/?tag=3), which, if selected, will display only scripts that are also available in Metasploit module format.
- We can, however, find the exploit code [inside ExploitDB's entries](https://www.exploit-db.com/exploits/9861). Alternatively, if we do not want to use our web browser to search for a specific exploit within ExploitDB, we can use the CLI version, `searchsploit`.

```shell-session
searchsploit nagios3
```
- Note that not all `.rb` files are automatically converted to `msfconsole` modules. Some exploits are written in Ruby without having any Metasploit module-compatible code in them. We will look at one of these examples in the following sub-section.

```shell-session
searchsploit -t Nagios3 --exclude=".py"
```

We have to download the `.rb` file and place it in the correct directory. The default directory where all the modules, scripts, plugins, and `msfconsole` proprietary files are stored is `/usr/share/metasploit-framework`. The critical folders are also symlinked in our home and root folders in the hidden `~/.msf4/` location.

```shell-session
CaraxesL@htb[/htb]$ ls /usr/share/metasploit-framework/

app     db             Gemfile.lock                  modules     msfdb            msfrpcd    msf-ws.ru  ruby             script-recon  vendor
config  documentation  lib                           msfconsole  msf-json-rpc.ru  msfupdate  plugins    script-exploit   scripts
data    Gemfile        metasploit-framework.gemspec  msfd        msfrpc           msfvenom   Rakefile   script-password  tools
```
```shell-session
CaraxesL@htb[/htb]$ ls .msf4/

history  local  logos  logs  loot  modules  plugins  store
```
We copy it into the appropriate directory after downloading the [exploit](https://www.exploit-db.com/exploits/9861). Note that our home folder `.msf4` location might not have all the folder structure that the `/usr/share/metasploit-framework/` one might have. So, we will just need to `mkdir` the appropriate folders so that the structure is the same as the original folder so that `msfconsole` can find the new modules. After that, we will be proceeding with copying the `.rb` script directly into the primary location.


#### MSF - Loading Additional Modules at Runtime
```shell-session
CaraxesL@htb[/htb]$ cp ~/Downloads/9861.rb /usr/share/metasploit-framework/modules/exploits/unix/webapp/nagios3_command_injection.rb
CaraxesL@htb[/htb]$ msfconsole -m /usr/share/metasploit-framework/modules/
```
#### MSF - Loading Additional Modules
```shell-session
msf6> loadpath /usr/share/metasploit-framework/modules/
```
Alternatively, we can also launch `msfconsole` and run the `reload_all` command for the newly installed module to appear in the list.

```shell-session
msf6 > reload_all
msf6 > use exploit/unix/webapp/nagios3_command_injection 
msf6 exploit(unix/webapp/nagios3_command_injection) > show options
```

### Porting Over Scripts into Metasploit Modules
- need to learn ruby 
- we will go for [Bludit 3.9.2 - Authentication Bruteforce Mitigation Bypass](https://www.exploit-db.com/exploits/48746). We will need to download the script, `48746.rb` and proceed to copy it into the `/usr/share/metasploit-framework/modules/exploits/linux/http/`

```shell-session
ls /usr/share/metasploit-framework/modules/exploits/linux/http/ | grep bludit
```

```shell-session
 cp ~/Downloads/48746.rb /usr/share/metasploit-framework/modules/exploits/linux/http/bludit_auth_bruteforce_mitigation_bypass.rb
```
### Writing Our Module
- All necessary information about Metasploit Ruby coding can be found on the [Rubydoc.info Metasploit Framework](https://www.rubydoc.info/github/rapid7/metasploit-framework) related page
- [Metasploit: A Penetration Tester's Guide book from No Starch Press](https://nostarch.com/metasploit). Rapid7 has also created blog posts on this topic, which can be found [here](https://blog.rapid7.com/2012/07/05/part-1-metasploit-module-development-the-series/).

re read the module there is a sample code they have explaind
