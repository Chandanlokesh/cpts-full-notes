[Hashcat](https://hashcat.net/) is a well-known password cracking tool for Linux, Windows, and macOS

syntax
```shell
hashcat -a 0 -m 0 <hashes> [wordlist, rule, mask, ...]
```
`-a` attack mode
`-m` hash type
`<hash>` hash string or a file containing one or more pass hashes 

#### hash type
`hashcat --help` each hash type has some ID 
[hash list](https://hashcat.net/wiki/doku.php?id=example_hashes)
```shell
hashid -m '$1$FNr44XZC$wQxY6HHLrgrGX0e1195k.1'
```

### attack modes
##### Dictionary attack
[Dictionary attack](https://hashcat.net/wiki/doku.php?id=dictionary_attack) (`-a 0`)
he user provides password hashes and a wordlist as input, and Hashcat tests each word in the list as a potential password until the correct one is found or the list is exhausted.
```shell
hashcat -a 0 -m 0 e3e3ec5831ad5e7288241960e5d4fdb8 /usr/share/wordlists/rockyou.txt
```

The rule files that come with hashcat are typically found under `/usr/share/hashcat/rules`
we would append the `-r <ruleset>` option to the command,
```shell
hashcat -a 0 -m 0 1b0556a75770563578569ae21392630c /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule
```

#### Mask attack

[Mask attack](https://hashcat.net/wiki/doku.php?id=mask_attack) (`-a 3`) is a type of brute-force attack in which the keyspace is explicitly defined by the user. For example, if we know that a password is eight characters long, rather than attempting every possible combination, we might define a mask that tests combinations of six letters followed by two numbers.

|Symbol|Charset|
|---|---|
|?l|abcdefghijklmnopqrstuvwxyz|
|?u|ABCDEFGHIJKLMNOPQRSTUVWXYZ|
|?d|0123456789|
|?h|0123456789abcdef|
|?H|0123456789ABCDEF|
|?s|«space»!"#$%&'()*+,-./:;<=>?@[]^_`{|
|?a|?l?u?d?s|
|?b|0x00 - 0xff|
Custom charsets can be defined with the `-1`, `-2`, `-3`, and `-4` arguments, then referred to with `?1`, `?2`, `?3`, and `?4`.

Let's say that we specifically want to try passwords which start with an uppercase letter, continue with four lowercase letters, a digit, and then a symbol. The resulting hashcat mask would be `?u?l?l?l?l?d?s`.

```shell
hashcat -a 3 -m 0 1e293d6912d074c0fd15844d803400dd '?u?l?l?l?l?d?s'
```