- Basic OSINT (Open Source Intelligence) techniques can be highly effective in uncovering such personal information and may assist in password guessing. More information about OSINT can be found in the [OSINT: Corporate Recon module](https://academy.hackthebox.com/course/preview/osint-corporate-recon).

#### example in hashcat 
- we have a password file that contain a word so we are  using custom rules to generate a custom password-lists

```shell
hashcat --force password.list -r custom.rule --stdout | sort -u > mut_password.list
```
[doc for writhing the rules ](https://hashcat.net/wiki/doku.php?id=rule_based_attack)

## Generating wordlists using CeWL

```shell
cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist
```

`-d` depth to spider
`-m` minimum length of the word
`--lowercase` storage of the found words in lowercase
`-w` file where we want to store the wordlists
