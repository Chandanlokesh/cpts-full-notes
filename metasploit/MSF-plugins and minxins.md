### Plugins
- Plugins are readily available software that has already been released by third parties and have given approval to the creators of Metasploit to integrate their software inside the framework.
- Plugins directly with the API and can be used to manipulate the entire framework. They can be useful for automating repetitive tasks, adding new commands to the `msfconsole`, and extending the already powerful framework.

### Using Plugins
- need to ensure it is installed in the correct directory on our machine.
- Navigating to `/usr/share/metasploit-framework/plugins`

```shell-session
ls /usr/share/metasploit-framework/plugins
```

If the plugin is found here, we can fire it up inside `msfconsole`

```shell
msf > load nessus

msf > nessus_help

#if we get any error we can use this command

msf > load Plugin_That_Does_Not_Exist

```

### installing new plugins
- we can take the .rb file provided on the maker's page and place it in the folder at `/usr/share/metasploit-framework/plugins` with the proper permissions.

```shell
git clone https://github.com/darkoperator/Metasploit-Plugins
ls Metasploit-Plugins
.....
.....

#we can copy all those plugins to the folder 


sudo cp ./Metasploit-Plugins/pentest.rb /usr/share/metasploit-framework/plugins/pentest.rb

```

check the plugin's installation by running the `load` command.
```shell-session
msf6 > load pentest
```


list of popular plugins below:

| [nMap (pre-installed)](https://nmap.org)                                                                            | [NexPose (pre-installed)](https://sectools.org/tool/nexpose/)                                                                       | [Nessus (pre-installed)](https://www.tenable.com/products/nessus)                                               |
| ------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| [Mimikatz (pre-installed V.1)](http://blog.gentilkiwi.com/mimikatz)                                                 | [Stdapi (pre-installed)](https://www.rubydoc.info/github/rapid7/metasploit-framework/Rex/Post/Meterpreter/Extensions/Stdapi/Stdapi) | [Railgun](https://github.com/rapid7/metasploit-framework/wiki/How-to-use-Railgun-for-Windows-post-exploitation) |
| [Priv](https://github.com/rapid7/metasploit-framework/blob/master/lib/rex/post/meterpreter/extensions/priv/priv.rb) | [Incognito (pre-installed)](https://www.offensive-security.com/metasploit-unleashed/fun-incognito/)                                 | [Darkoperator's](https://github.com/darkoperator/Metasploit-Plugins)                                            |

### Mixins
**Mixins** are **modules** (Ruby classes that can't be instantiated) that provide **methods** to other classes **without requiring inheritance**. Instead of creating a hierarchy of parent and child classes, Ruby allows you to "mix in" reusable code via the `include` keyword. This is why we say mixins are a form of **inclusion**, not inheritance.

- A module might include a **network protocol** mixin like `Tcp`, `Udp`, or `Http`.
    
- Exploits can include common payload handlers or session management features.

Using mixins means you can add extra functionality to payloads (or any other module), even if those features weren’t originally part of the base class.