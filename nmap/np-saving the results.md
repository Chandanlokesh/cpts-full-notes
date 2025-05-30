- Normal output (`-oN`) with the `.nmap` file extension
- Grepable output (`-oG`) with the `.gnmap` file extension
- XML output (`-oX`) with the `.xml` file extension
- We can also specify the option (`-oA`) to save the results in all formats.

```shell-session
sudo nmap 10.129.2.28 -p- -oA target
```
With the XML output, we can easily create HTML reports that are easy to read, even for non-technical people.

o convert the stored results from XML format to HTML, we can use the tool `xsltproc`.

```shell-session
xsltproc target.xml -o target.html
```
