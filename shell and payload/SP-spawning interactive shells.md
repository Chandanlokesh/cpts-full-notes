
- if we land on shell but have limited options and if python is also not installed we can try other options also listed bellow
- whenever we see `/bin/sh` or `/bin/bash`, this could also be replaced with the binary associated with the shell interpreter language present on that system. With most Linux systems, we will likely come across `bourne shell` (`/bin/sh`) and `bourne again shell` (`/bin/bash`) present on the system natively.

## /bin/sh -i
execute the shell interpreter specified in the path in interactive mode (`-i`)

#### Perl To Shell
```shell-session
perl —e 'exec "/bin/sh";'
perl: exec "/bin/sh";
```
#### Ruby
```shell-session
ruby: exec "/bin/sh"
```
#### Lua
```shell-session
lua: os.execute('/bin/sh')
```
#### AWK
[AWK](https://man7.org/linux/man-pages/man1/awk.1p.html) is a C-like pattern scanning and processing language present on most UNIX/Linux-based systems, widely used by developers and sysadmins to generate reports
```shell-session
awk 'BEGIN {system("/bin/sh")}'
```

## Find

[Find](https://man7.org/linux/man-pages/man1/find.1.html) is a command present on most Unix/Linux systems widely used to search for & through files and directories using various criteria.
```shell-session
find / -name nameoffile -exec /bin/awk 'BEGIN {system("/bin/sh")}' \;
```

#### Using Exec To Launch A Shell
```shell-session
find . -exec /bin/sh \; -quit
```

#### VIM
##### Vim To Shell

```shell-session
vim -c ':!/bin/sh'
```
##### Vim Escape
```shell-session
vim
:set shell=/bin/sh
:shell
```

#### permissions
We can always attempt to run this command to list the file properties and permissions our account has over any given file or binary:
```shell-session
ls -la <path/to/fileorbinary>

sudo -l
```
sudo -l above will need a stable interactive shell to run.