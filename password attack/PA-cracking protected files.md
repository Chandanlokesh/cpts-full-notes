- [file info](https://fileinfo.com/filetypes/encoded)

## Hunting for SSH keys
- will not have the file extinction 

#### finding the ssh key using grep
```shell
grep -rnE '^\-{5}BEGIN [A-Z0-9]+ PRIVATE KEY\-{5}$' /* 2>/dev/null
```
#### check if  the key is password protected
```shell
ssh-keygen -yf ~/.ssh/id_ed25519 # check ssh key is encrypted or not

# output is this means no password protected
ssh-ed25519 AAAAC3NzaC1...

# output is like this means password protected
Enter passphrase for "/home/jsmith/.ssh/id_rsa":

```

## Cracking encrypted SSH keys

```shell
ssh2john.py SSH.private > ssh.hash

john --wordlist=rockyou.txt ssh.hash

# we can view the resulting hash 
john ssh.hash --show
```

## cracking password protected documents
`office2john.py`, which can be used to extract password hashes from all common Office document formats.

```bash

# document file 
office2john.py Protected.docx > protected-docx.hash

john --wordlist=rockyou.txt protected-docx.hash

john protected-docx.hash --show

# pdf file
pdf2john.py PDF.pdf >pdf.pdf

john --wordlist=rockyou.txt pdf.hash

john pdf.hash --show
```